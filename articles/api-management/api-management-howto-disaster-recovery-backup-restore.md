<properties 
    pageTitle="Megvalósítása vészhelyreállítás szolgáltatás biztonsági mentéssel és Azure API-kezelés visszaállítása |} Microsoft Azure" 
    description="Megtudhatja, hogy miként biztonsági mentése és visszaállítása Azure API-kezelés vészhelyreállítás végrehajtásához." 
    services="api-management" 
    documentationCenter="" 
    authors="steved0x" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="api-management" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/25/2016" 
    ms.author="sdanie"/>

# <a name="how-to-implement-disaster-recovery-using-service-backup-and-restore-in-azure-api-management"></a>Megvalósítása vészhelyreállítás szolgáltatás biztonsági mentéssel és Azure API-kezelés visszaállítása

Közzététele és Azure API-kezelés telik a sok hibatűrést, illetve egyéb kell illesztenie tervezése infrastruktúra-funkciókkal keresztül az API-k kezelése kiválasztásával megvalósításához és kezelése. Az Azure platform megszünteti a nagy törtrészre a költség tört a lehetséges hibák.

Elérhetőség helyreállítása a régió, ahol a szolgáltatás API-kezelés üzemeltetik, érintő problémák készen áll a szolgáltatás egy másik régióbeli pótlására bármikor kell lennie. Az elérhetőség célok és helyreállítási idő cél függően érdemes lehet lefoglalása egy vagy több régióban biztonsági szolgáltatás, és próbálja meg a beállítási és a tartalom szinkronizálja az aktív szolgáltatással megőrzéséhez. A szolgáltatás biztonsági mentés és visszaállítás szolgáltatás szükséges építőelem biztosít a katasztrófa helyreállítási stratégia végrehajtása.

Ez az útmutató jeleníti meg, hogyan kívánja hitelesíteni a Azure erőforrás-kezelő kérelmeket, és hogyan biztonsági mentési és visszaállítási az API Management szolgáltatás példányainak.

>[AZURE.NOTE] A folyamat mentésével és visszaállításával vészhelyreállítás szolgáltatás API-kezelés példány a API Management szolgáltatás példányainak felhasználási területei, például a fejlesztői kiegészítését is használható.
>
>Figyelje meg, hogy minden egyes biztonsági másolat 7 nap múlva lejár. Biztonsági mentés visszaállítása a 7 nappal érvényességi időszakának lejárta után próbál, ha a visszaállítás nem sikerül egy `Cannot restore: backup expired` üzenetet.

## <a name="authenticating-azure-resource-manager-requests"></a>Azure hitelesítő erőforrás-kezelő kéréseket

>[AZURE.IMPORTANT] Biztonsági mentési és visszaállítási a REST API-t használ Azure erőforrás-kezelő és egy másik hitelesítési módszer, mint a REST API-k az API-kezelés szervezetek kezelésére szolgáló. Ez a szakasz lépései bemutatják, hogyan Azure erőforrás-kezelő kérések hitelesítést végezni. További tudnivalókért lásd: [Azure erőforrás-kezelő hitelesítése kérések](http://msdn.microsoft.com/library/azure/dn790557.aspx).

Minden erőforrásokat az Azure erőforrás-kezelő használja a feladatot az Azure Active Directoryval, az alábbi lépésekkel hitelesíteni kell.

-   Az Azure Active Directory-bérlői alkalmazás hozzáadása.
-   A felvett alkalmazások engedélyeinek beállítása.
-   Szerezze be a token kérések az Azure erőforrás-kezelő hitelesítése.

Az első lépésként hozzon létre egy Azure Active Directory-alkalmazást. Jelentkezzen be az [Azure klasszikus portál](http://manage.windowsazure.com/) az előfizetést, az API-kezelés service példányát tartalmazza, és keresse meg az **alkalmazások** fülre, az alapértelmezett Azure Active Directory számára.

>[AZURE.NOTE] Ha az Azure Active Directory alapértelmezett könyvtár nem látható a fiókjához, lépjen kapcsolatba a rendszergazda az Azure előfizetés a fiókjába a szükséges engedélyeket szeretne adni. Az alapértelmezett könyvtár megkeresése a további tudnivalókért lásd [Keresse meg az alapértelmezett könyvtár](../virtual-machines/virtual-machines-windows-create-aad-work-id.md#locate-your-default-directory-in-the-azure-portal).

![Azure Active Directory-alkalmazás létrehozása][api-management-add-aad-application]

Kattintson a **Hozzáadás**, **szervezeten fejlesztésével az alkalmazás hozzáadása**gombra, és válassza a **natív ügyfélalkalmazás**. Adjon egy jól értelmezhető nevet, és kattintson a következő nyílra. Például: írja be az URL-cím helyőrző `http://resources` az **Átirányítás URI**, ahogy kötelező, de az érték nem használatos később. A jelölőnégyzet bejelölésével az alkalmazás mentése gombra.

Amikor az alkalmazás menti, kattintson a **Konfigurálás**gombra, görgessen le a **engedélyek egyéb alkalmazások** csoportban, és kattintson az **alkalmazás hozzáadása**elemre.

![Engedélyek hozzáadása][api-management-aad-permissions-add]

Jelölje ki a **Windows** **Azure szolgáltatás felügyeleti API** , és jelölje be a jelölőnégyzetet, az alkalmazás hozzáadása.

![Engedélyek hozzáadása][api-management-aad-permissions]

Kattintson a **Meghatalmazott engedélyeit** az újonnan hozzáadott **a Windows** **Azure szolgáltatás felügyeleti API** -alkalmazás mellett, jelölje be a **Hozzáférés Azure Service-kezelés (előzetes verzió)**jelölőnégyzetet, és kattintson a **Mentés**gombra.

![Engedélyek hozzáadása][api-management-aad-delegated-permissions]

Előtt, hogy a biztonsági másolat készítése, és visszaállítja az API-khoz meghívását, jogkivonat első szükség. Az alábbi példa a [Microsoft.IdentityModel.Clients.ActiveDirectory](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory) nuget csomagot használ, a token beolvasásához.

    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    using System;

    namespace GetTokenResourceManagerRequests
    {
        class Program
        {
            static void Main(string[] args)
            {
                var authenticationContext = new AuthenticationContext("https://login.windows.net/{tenant id}");
                var result = authenticationContext.AcquireToken("https://management.azure.com/", {application id}, new Uri({redirect uri});

                if (result == null) {
                    throw new InvalidOperationException("Failed to obtain the JWT token");
                }

                Console.WriteLine(result.AccessToken);

                Console.ReadLine();
            }
        }
    }

Csere `{tentand id}`, `{application id}`, és `{redirect uri}` az alábbi utasításokat.

Csere `{tenant id}` az imént létrehozott Azure Active Directory-alkalmazás, a bérlői azonosítóval. Az azonosító **Nézet végpontok**kattintva érheti el.

![Végpontok][api-management-aad-default-directory]

![Végpontok][api-management-endpoint]

Csere `{application id}` és `{redirect uri}` az **Ügyfél-azonosító** , és az URL-cím használata a **Átirányítás URL-címe** szakaszából az Azure Active Directory-alkalmazás **beállítása** lapon. 

![Erőforrások][api-management-aad-resources]

Miután értékeket ad meg, a példa térjen vissza jogkivonat a következőhöz hasonló.

![Jogkivonat][api-management-arm-token]

Hívja fel a biztonsági mentés és visszaállítás műveletek, az alábbi szakaszokban ismertetett, mielőtt a többi hívás engedélyezési kérelem fejlécét beállítása.

    request.Headers.Add(HttpRequestHeader.Authorization, "Bearer " + token);

## <a name="step1"> </a>Biztonsági mentése az API alkalmazáskezelési szolgáltatás
Biztonsági másolatot készíteni az API-kezelés szolgáltatáskérés probléma a következő HTTP:

`POST https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.ApiManagement/service/{serviceName}/backup?api-version={api-version}`

Ha:

* `subscriptionId`-azonosítóját az előfizetést próbál API Management szolgáltatást tartalmazó biztonsági másolatot készít
* `resourceGroupName`– olyan karakterlánc, a "API - alapértelmezett-{szolgáltatás-régió}" hol `service-region` azonosítja az Azure terület, ahol az API Management szolgáltatás, próbálja biztonsági másolat üzemelteti, például`North-Central-US`
* `serviceName`-az API alkalmazáskezelési szolgáltatás neve készít biztonsági másolatot meg a saját egyszerre
* `api-version`-cseréje`2014-02-14`

A kérés törzsében adja meg a cél Azure tároló fióknév, hívóbetű, blob-tárolóhoz nevét és biztonsági másolat nevét:

    '{  
        storageAccount : {storage account name for the backup},  
        accessKey : {access key for the account},  
        containerName : {backup container name},  
        backupName : {backup blob name}  
    }'

Értékének beállítása a `Content-Type` kérelem fejléc `application/json`.

Biztonsági mentés egy időigényes művelet elvégzéséhez több percig tart.  Ha sikeres volt-e a kérést, és a biztonsági mentés kezdeményeztek jelenik meg, a `202 Accepted` állapotkód-válasz a egy `Location` fejléc.  Ellenőrizze, hogy az URL-cím kérések "GET" a `Location` megtudhatja, hogy a művelet állapotának fejléc. Miközben folyamatban van a biztonsági mentés, akkor is megkapja a "202 elfogadott" állapotkódot. A válasz kódot, amely `200 OK` megjelöli a biztonsági mentés sikeres befejezését.

**Megjegyzés**:

- A **tároló** a összehívás törzsébe **léteznie kell,**a megadott.
* Biztonsági másolat folyamat során, **nem kell megpróbálja szolgáltatás adatkezelési műveletek** például Termékváltozat frissítése vagy vissza léptetheti tartomány neve módosítása stb. 
* Helyreállítása **csak 7 napig biztosítva van a biztonsági mentés** óta a létrehozása időpontjában. 
* **Látogatottsági adatok** analytics létrehozására használható jelentések, az **alkalmazás nem része** a biztonságimásolat. [Azure API Management REST API][] segítségével rendszeres beolvasásához analytics-jelentések az adatvesztés megelőzése érdekében.
* A gyakoriság, amellyel szolgáltatás biztonsági mentés végrehajtása a helyreállítási pont cél hatással vannak. Minimalizálásához meg véleményét rendszeres biztonsági mentés végrehajtása, valamint a igény szerinti biztonsági mentés végrehajtása az API-kezelési szolgáltatáshoz fontos a módosítások végrehajtása után.
* A szolgáltatás beállításai (pl. API-hoz, házirendek, developer portal megjelenés) közben biztonsági mentést végzett **módosítások** , folyamat, **Előfordulhat, hogy nem fognak szerepelni a biztonsági mentés és ezért elvesznek**.

## <a name="step2"> </a>Egy API alkalmazáskezelési szolgáltatás visszaállítása
Visszaállítása az API-kezelés szolgáltatás egy korábban létrehozott biztonsági másolatból ellenőrizze a következő HTTP-kérés:

`POST https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.ApiManagement/service/{serviceName}/restore?api-version={api-version}`

Ha:

* `subscriptionId`-az előfizetés meg visszaállítani a biztonsági másolatot az API Management szolgáltatást tartalmazó azonosító
* `resourceGroupName`– olyan karakterlánc, a "API - alapértelmezett-{szolgáltatás-régió}" hol `service-region` az Azure régió, a API Management szolgáltatásra, a biztonsági másolatot a visszaállítandó hol vannak tárolva, például azonosító`North-Central-US`
* `serviceName`-a nevét a szolgáltatás be visszaállítják meg a saját egyszerre API-kezelés
* `api-version`-cseréje`2014-02-14`

A kérés törzsében adja meg a biztonságimásolat-fájlt hely, azaz Azure tároló fióknév, hívóbetű, blob-tárolóhoz nevét és biztonsági másolat nevét:

    '{  
        storageAccount : {storage account name for the backup},  
        accessKey : {access key for the account},  
        containerName : {backup container name},  
        backupName : {backup blob name}  
    }'

Értékének beállítása a `Content-Type` kérelem fejléc `application/json`.

Visszaállítás egy időigényes művelet befejezéséhez és 30 vagy több perc foglalhat.  Ha sikeres volt-e a kérést, és a visszaállításhoz kezdeményeztek jelenik meg, a `202 Accepted` állapotkód-válasz a egy `Location` fejléc.  Ellenőrizze, hogy az URL-cím kérések "GET" a `Location` megtudhatja, hogy a művelet állapotának fejléc. A visszaállítás alatt, akkor is megkapja a "202 elfogadott" állapotkód. A válasz kódot, amely `200 OK` megjelöli a visszaállítási művelet sikeres befejezését.

>[AZURE.IMPORTANT] **A Termékváltozat** a szolgáltatást, hogy a biztonsági másolat szolgáltatás visszaállítják SKU azokat **meg kell egyeznie** vissza.
>
>A szolgáltatás beállításai (pl. API-hoz, házirendek, developer portal megjelenés) visszaállítási művelet során végzett **módosítások** van folyamatban **felülíródnak**.

## <a name="next-steps"></a>Következő lépések
Nézze meg a két különböző forgatókönyvek a biztonsági mentés és visszaállítás folyamat az alábbi Microsoft blogok.

-   [Azure API kimutatásokat bizonyos](https://www.returngis.net/en/2015/06/replicate-azure-api-management-accounts/) 
    -   Köszönjük, hogy a Gisela saját hozzájárulása ebben a cikkben.
-   [Azure API kezelése: Biztonsági mentésével és visszaállításával konfigurációs](http://blogs.msdn.com/b/stuartleeks/archive/2015/04/29/azure-api-management-backing-up-and-restoring-configuration.aspx)
    -   A részletes által Stuart megközelítés nem egyezik meg a hivatalos útmutatást, de nagyon érdekes.

[Backup an API Management service]: #step1
[Restore an API Management service]: #step2


[Azure API-kezelés REST API-val]: http://msdn.microsoft.com/library/azure/dn781421.aspx

[api-management-add-aad-application]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-add-aad-application.png

[api-management-aad-permissions]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-aad-permissions.png
[api-management-aad-permissions-add]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-aad-permissions-add.png
[api-management-aad-delegated-permissions]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-aad-delegated-permissions.png
[api-management-aad-default-directory]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-aad-default-directory.png
[api-management-aad-resources]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-aad-resources.png
[api-management-arm-token]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-arm-token.png
[api-management-endpoint]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-endpoint.png
 
