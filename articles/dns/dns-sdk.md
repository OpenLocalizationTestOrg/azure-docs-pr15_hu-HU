<properties 
   pageTitle="Hozzon létre a DNS-zónák, és rögzítse a .NET SDK használatával Azure DNS beállítása |} Microsoft Azure" 
   description="Hogyan hozhat létre a DNS-zónák és rekord Azure DNS a .NET SDK használatával állítja be." 
   services="dns" 
   documentationCenter="na" 
   authors="jtuliani" 
   manager="carmonm" 
   editor=""/>

<tags
   ms.service="dns"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services" 
   ms.date="09/19/2016"
   ms.author="jtuliani"/>


# <a name="create-dns-zones-and-record-sets-using-the-net-sdk"></a>Hozzon létre a DNS-zónák és a .NET SDK használatával rekord beállítása

Műveletek létrehozása, törlése vagy frissítése a DNS-zónák, a rekord beállítása és a rekordok DNS SDK .NET DNS-kezelési tárral használatával automatizálhatja. Visual Studio teljes projekt érhető el [itt.](https://www.microsoft.com/en-us/download/details.aspx?id=47268&WT.mc_id=DX_MVP4025064&e6b34bbe-475b-1abd-2c51-b5034bcdd6d2=True)

## <a name="create-a-service-principal-account"></a>Hozzon létre egy egyszerű szolgáltatásfiók

Általában programozott Azure forrá hozzáférés saját felhasználói hitelesítő adatokat, hanem a saját fiók keresztül. Ezek a dedikált fiókok úgynevezett "szolgáltatás fő" fiókok. Az Azure DNS SDK minta projekt használatához először hozzon létre egy egyszerű szolgáltatásfiók, és rendelje hozzá a megfelelő engedélyekkel.

1. Kövesse [ezeket az utasításokat](../resource-group-authenticate-service-principal.md) a fiók hozzárendelése szolgáltatáshoz fő (az Azure DNS SDK minta projekt feltételezi, hogy jelszó-alapú hitelesítés.)

2. Hozzon létre egy erőforrás csoportot ([az alábbiakban hogyan](../azure-portal/resource-group-portal.md)).

3. A fő szolgáltatásfiók "DNS Zone közreműködői" hozzáférési engedélyek megadása a erőforráscsoport Azure RBAC használatával ([az alábbiakban hogyan](../active-directory/role-based-access-control-configure.md).)

4. Az Azure DNS SDK minta projekt használ, szerkesztheti a "program.cs" fájlt az alábbi képlettel történik:
    * Helyezze be a tenantId, a clientId (más néven Fiókazonosító), a titkos (szolgáltatás fő fiók jelszava) és a használt lépés: 1 subscriptionId vonatkozó helyes értékek.
    * Adja meg az erőforrás-csoport nevét, a 2.
    * Írja be a DNS-zóna név lehetőség.

## <a name="nuget-packages-and-namespace-declarations"></a>NuGet-csomagok és névtér nyilatkozatokat

Az Azure DNS .NET SDK használatához kell telepítse az **Azure DNS könyvtár** NuGet csomag és más szükséges Azure-csomagokat.
 
1. A **Visual Studióban**nyissa meg a projekt vagy új projektet. 

2. Kattintson az **eszközök** **>** **NuGet csomag Manager** **>** **megoldás... NuGet csomagjai kezelése**. 

3. Kattintson a **Tallózás gombra**, a **előzetes olyan** jelölőnégyzet engedélyezése és **Microsoft.Azure.Management.Dns** írja be a keresőmezőbe.

4. Jelölje ki a csomagot, majd kattintson a **telepítse** a Visual Studio projekthez hozzáadni.
 
5. Ismételje meg a fenti is telepíteni az alábbi csomagok folyamatot: **Microsoft.Rest.ClientRuntime.Azure.Authentication** és **Microsoft.Azure.Management.ResourceManager**.

## <a name="add-namespace-declarations"></a>Névtér deklarációs hozzáadása

Adja hozzá a következő névtér nyilatkozatokat

    using Microsoft.Rest.Azure.Authentication;
    using Microsoft.Azure.Management.Dns;
    using Microsoft.Azure.Management.Dns.Models;

## <a name="initialize-the-dns-management-client"></a>A DNS-kezelési ügyfél inicializálni

A *DnsManagementClient* a módszerek és a DNS-zónák és rekordkészlet kezeléséhez szükséges tulajdonságokat tartalmazza.  A következő kódot jelentkezik be a fő szolgáltatásfiók, és létrehoz egy DnsManagementClient objektumot.

    // Build the service credentials and DNS management client
    var serviceCreds = await ApplicationTokenProvider.LoginSilentAsync(tenantId, clientId, secret);
    var dnsClient = new DnsManagementClient(serviceCreds);
    dnsClient.SubscriptionId = subscriptionId;

## <a name="create-or-update-a-dns-zone"></a>Létrehozása és frissítése a DNS-zóna

A DNS-zóna létrehozásához először egy "Zóna" objektum jön létre a DNS-zóna paramétereket tartalmaz. Egy adott tartomány DNS-zónák nem kapcsolódnak, mert a hely beállítása "global". Ebben a példában az [Azure erőforrás-kezelő "címke"](https://azure.microsoft.com/updates/organize-your-azure-resources-with-tags/) is megjelenik a zónába.

A zone-objektumra, amely a zóna paraméterek ténylegesen, létrehozásához és frissítése az Azure DNS-zóna *DnsManagementClient.Zones.CreateOrUpdateAsyc* módszer átadott.

>[AZURE.NOTE] DnsManagementClient három üzemmóddal támogatja: szinkron ("CreateOrUpdate"), aszinkron ("CreateOrUpdateAsync"), vagy aszinkron hozzáférést a HTTP-válasz ("CreateOrUpdateWithHttpMessagesAsync").  Bármely, a következő üzemmódok, megadhatja a alkalmazás igényeitől függően.

Azure DNS optimista feldolgozási [Etags](dns-getstarted-create-dnszone.md)nevű támogatja. Ebben a példában megadása "*" a "If-nincs – egyezés" Élőfej Ez az Azure DNS hozhat létre a DNS-zóna, ha egy nem létezik.  A hívás sikertelen lesz, ha a megadott névvel rendelkező zónán már létezik az adott erőforrás csoportban.

    // Create zone parameters
    var dnsZoneParams = new Zone("global"); // All DNS zones must have location = "global"
    
    // Create a Azure Resource Manager 'tag'.  This is optional.  You can add multiple tags
    dnsZoneParams.Tags = new Dictionary<string, string>();
    dnsZoneParams.Tags.Add("dept", "finance");
    
    // Create the actual zone.
    // Note: Uses 'If-None-Match *' ETAG check, so will fail if the zone exists already.
    // Note: For non-async usage, call dnsClient.Zones.CreateOrUpdate(resourceGroupName, zoneName, dnsZoneParams, null, "*")
    // Note: For getting the http response, call dnsClient.Zones.CreateOrUpdateWithHttpMessagesAsync(resourceGroupName, zoneName, dnsZoneParams, null, "*")
    var dnsZone = await dnsClient.Zones.CreateOrUpdateAsync(resourceGroupName, zoneName, dnsZoneParams, null, "*");

## <a name="create-dns-record-sets-and-records"></a>DNS-rekord beállítása és a rekordok létrehozása

DNS-rekordjait, egy rekordkészletben kezelhetők. Egy rekordkészletben egy rekordhalmaz, ugyanazt a nevet és rekordtípus zónán belül.  Rekordkészletben név viszonyított a zóna neve, nem a teljesen minősített tartománynév van.

Létrehozása vagy módosítása a rekord beállítása, "RecordSet" paraméterek objektum létrejön, és *DnsManagementClient.RecordSets.CreateOrUpdateAsync*átadott. A DNS-zónák, vannak három üzemmóddal: szinkron ("CreateOrUpdate"), aszinkron ("CreateOrUpdateAsync"), vagy aszinkron hozzáférést a HTTP-válasz ("CreateOrUpdateWithHttpMessagesAsync").

DNS-zónák, a rekord beállítása a műveletek tartalmazzák a optimista feldolgozási.  Ebben a példában a "Ha, hol.van" sem "If-nincs – egyezés" megadva, mivel a rekordkészlet mindig létrejön.  A hívás felülírja bármely ugyanazt a nevet és a record type be a DNS-zóna a meglévő rekordba.

    // Create record set parameters
    var recordSetParams = new RecordSet();
    recordSetParams.TTL = 3600;

    // Add records to the record set parameter object.  In this case, we'll add a record of type 'A'
    recordSetParams.ARecords = new List<ARecord>();
    recordSetParams.ARecords.Add(new ARecord("1.2.3.4"));

    // Add metadata to the record set.  Similar to Azure Resource Manager tags, this is optional and you can add multiple metadata name/value pairs
    recordSetParams.Metadata = new Dictionary<string, string>();
    recordSetParams.Metadata.Add("user", "Mary");

    // Create the actual record set in Azure DNS
    // Note: no ETAG checks specified, will overwrite existing record set if one exists
    var recordSet = await dnsClient.RecordSets.CreateOrUpdateAsync(resourceGroupName, zoneName, recordSetName, RecordType.A, recordSetParams);

## <a name="get-zones-and-record-sets"></a>Zónák és rekord beállítása

A *DnsManagementClient.Zones.Get* és *DnsManagementClient.RecordSets.Get* módszerek rendre beolvasásához egyes zónák és a rekord készleteket. Rekordkészlet típusukra, neve és az időzóna és erőforrás csoport léteznek a azonosítja. Zónák azonosítja a nevére, és az erőforráscsoport léteznek a.

    var recordSet = dnsClient.RecordSets.Get(resourceGroupName, zoneName, recordSetName, RecordType.A);
    
## <a name="update-an-existing-record-set"></a>Egy meglévő rekordkészletben frissítése

Frissítése egy meglévő DNS-rekord beállítása, először a rekordkészlet beolvasásához, azután rekordkészletben tartalmának frissítése, majd a módosítás elküldése.  Ebben a példában azt adja meg a "Etag" a "Ha, hol.van" paramétert a beolvasott rekord csoportjából. A hívás sikertelen lesz, ha egy egyidejű művelet módosította időközben a rekordot.

    var recordSet = dnsClient.RecordSets.Get(resourceGroupName, zoneName, recordSetName, RecordType.A);

    // Add a new record to the local object.  Note that records in a record set must be unique/distinct
    recordSet.ARecords.Add(new ARecord("5.6.7.8"));

    // Update the record set in Azure DNS
    // Note: ETAG check specified, update will be rejected if the record set has changed in the meantime
    recordSet = await dnsClient.RecordSets.CreateOrUpdateAsync(resourceGroupName, zoneName, recordSetName, RecordType.A, recordSet, recordSet.Etag);

## <a name="list-zones-and-record-sets"></a>Lista zónák és a rekord beállítása

Zónák listában, végezze el a *DnsManagementClient.Zones.List...* műveleteket, amelyek támogatják az adott erőforrás csoport az összes zóna, vagy az adott Azure előfizetés minden zónáinak listázása (át erőforráscsoport.) Rekordok beállítása a lista, módszerekkel *DnsManagementClient.RecordSets.List...* , amely támogatja a vagy listaelem minden rekord beállítása a megadott zónán vagy csak egy bizonyos típusú adott rekord készleteket.

Megjegyzés: Ha zónák bejegyzésére, és is lehet paginated, amelynek eredménye rekord beállítása.  A következő példa bemutatja, hogyan Végigléptetés az eredmények oldalát. ("2"-mesterségesen kis oldalméret használható lapozási kényszerítése; a gyakorlatban a ezt a paramétert elhagyják kell, és használja az alapértelmezett oldalméret.)

    // Note: in this demo, we'll use a very small page size (2 record sets) to demonstrate paging
    // In practice, to improve performance you would use a large page size or just use the system default
    int recordSets = 0;
    var page = await dnsClient.RecordSets.ListAllInResourceGroupAsync(resourceGroupName, zoneName, "2");
    recordSets += page.Count();

    while (page.NextPageLink != null)
    {
        page = await dnsClient.RecordSets.ListAllInResourceGroupNextAsync(page.NextPageLink);
        recordSets += page.Count();
    }

## <a name="next-steps"></a>Következő lépések

Töltse le az [Azure DNS .NET SDK minta projekt](https://www.microsoft.com/en-us/download/details.aspx?id=47268&WT.mc_id=DX_MVP4025064&e6b34bbe-475b-1abd-2c51-b5034bcdd6d2=True), további példákat, hogyan használhatja az Azure DNS .NET SDK, beleértve a más DNS-bejegyzés típusainak példákat is tartalmaz.
