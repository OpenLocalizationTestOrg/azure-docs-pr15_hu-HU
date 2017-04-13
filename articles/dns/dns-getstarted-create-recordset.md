<properties
   pageTitle="Hozzon létre egy rekordkészletben és a PowerShell használatá DNS-zóna rekordjainak |} Microsoft Azure"
   description="A host rekordok létrehozása az Azure DNS módja. Rekord beállítása állítja be, és a rekordok a PowerShell használatával"
   services="dns"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor=""/>

<tags
   ms.service="dns"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/16/2016"
   ms.author="sewhee"/>



# <a name="create-dns-record-sets-and-records-by-using-powershell"></a>DNS-rekord beállítása és a rekordok létrehozása a PowerShell használatával


> [AZURE.SELECTOR]
- [Azure portál](dns-getstarted-create-recordset-portal.md)
- [A PowerShell](dns-getstarted-create-recordset.md)
- [Azure CLI](dns-getstarted-create-recordset-cli.md)

Ebben a cikkben megismerkedhet azon a folyamaton, létrehozása a rekordok és a rekordok beállítása a Windows PowerShell használatával. Miután létrehozta a DNS-zóna, a DNS-rekordokat a tartomány felvétele. Ehhez először megtudhatja, hogy DNS-rekordokat és a rekord beállítása.

[AZURE.INCLUDE [dns-about-records-include](../../includes/dns-about-records-include.md)]

## <a name="verify-that-you-have-the-latest-version-of-powershell"></a>Ellenőrizze, hogy rendelkezik-e a legújabb PowerShell

Győződjön meg arról, hogy telepítette az Azure erőforrás-kezelő PowerShell-parancsmagok legújabb verzióját. Megtudhatja, [hogy miként telepítheti, állíthatja Azure PowerShell](../powershell-install-configure.md) PowerShell-parancsmagok telepítésével kapcsolatos további információt.

## <a name="create-a-record-set-and-record"></a>Hozzon létre egy rekordot, és a rekord

Ez a szakasz ismerteti, hogyan hozhat létre egy rekordot, és a rekord.


### <a name="1-connect-to-your-subscription"></a>1. csatlakoztatása az előfizetés

Nyissa meg a PowerShell konzolt, és csatlakozni a fiókjához. Használja az alábbi példa csatlakozni:

    Login-AzureRmAccount

Jelölje be az előfizetések a fiókhoz.

    Get-AzureRmSubscription

Adja meg az előfizetést, szeretné használni.

    Select-AzureRmSubscription -SubscriptionName "Replace_with_your_subscription_name"

A PowerShell használatával kapcsolatos további tudnivalókért lásd: [A Windows PowerShell használatá az erőforrás-kezelő](../powershell-azure-resource-manager.md).


### <a name="2-create-a-record-set"></a>2. Hozzon létre egy rekordkészletben

Rekord beállítása használatával hozza létre a `New-AzureRmDnsRecordSet` parancsmag. Egy rekordkészletben létrehozásakor meg kell adnia a a rekord beállítása a nevét, a zóna, a time to live (TTL) és a rekordtípusra.

Létrehozhat egy bejegyzést a csúcs a zóna beállítása (ebben az esetben "contoso.com"), használja a rekord nevét "@", idézőjelekkel együtt. Az egy közös DNS is egységes.

Az alábbi példa létrehoz egy rekordot a DNS-zóna "contoso.com" a "www" relatív nevű beállítása. A teljesen minősített a rekordkészlet neve "www.contoso.com". A record type "A";, és a TTL (élettartam) 60 másodpercre. Miután elkészült a ezt a lépést, egy üres "www" rekord beállítása a változó *$rs*rendelt lesz.

    $rs = New-AzureRmDnsRecordSet -Name "www" -RecordType "A" -ZoneName "contoso.com" -ResourceGroupName "MyAzureResourceGroup" -Ttl 60

#### <a name="if-a-record-set-already-exists"></a>Ha egy rekord beállítása a már létezik

Ha egy rekord beállítása a már létezik, a parancs sikertelen, kivéve, ha a *-felülírása* kapcsolót. A *-felülírása* parancs elindítja a megerősítést kérő, amelyek segítségével is megjelenik a *-hatályba* válthat.


    $rs = New-AzureRmDnsRecordSet -Name www -RecordType A -Ttl 300 -ZoneName contoso.com -ResouceGroupName MyAzureResouceGroup [-Tag $tags] [-Overwrite] [-Force]


Ebben a példában megadása a zóna segítségével a zóna és erőforrás csoport nevét. Azt is megteheti, adhatja meg a zone objektumra, mint által visszaadott `Get-AzureRmDnsZone` vagy `New-AzureRmDnsZone`.

    $zone = Get-AzureRmDnsZone -Name contoso.com –ResourceGroupName MyAzureResourceGroup
    $rs = New-AzureRmDnsRecordSet -Name www -RecordType A -Ttl 300 –Zone $zone [-Tag $tags] [-Overwrite] [-Force]

`New-AzureRmDnsRecordSet`a helyi objektumot, amely a rekord beállítása a Azure DNS-ben létrehozott adja vissza.

### <a name="3-add-a-record"></a>3. a rekord hozzáadása

A "www" újonnan létrehozott rekordkészlet használatához kell rekordok hozzáadása. IPv4 is hozzáadhat a "www" rekordhoz *A* rekordok beállítása a következő példa használatával. Ebben a példában az előző lépésben megadott változó *$rs* támaszkodik.

Rekordok felvétele a rekord beállítása használatával `Add-AzureRmDnsRecordConfig` offline művelet. Csak a helyi változó *$rs* frissül.


    Add-AzureRmDnsRecordConfig -RecordSet $rs -Ipv4Address 134.170.185.46
    Add-AzureRmDnsRecordConfig -RecordSet $rs -Ipv4Address 134.170.188.221

### <a name="4-commit-the-changes"></a>4. a módosítások véglegesítése

A módosítások a rekordkészlet. Használat `Set-AzureRmDnsRecordSet` az Azure DNS-rekordot a módosítások feltöltése.

    Set-AzureRmDnsRecordSet -RecordSet $rs

### <a name="5-retrieve-the-record-set"></a>5. beolvasni a rekord beállítása

A rekord beállítása az Azure DNS használatával meghallgathatja `Get-AzureRmDnsRecordSet` az alábbi példában látható módon.


    Get-AzureRmDnsRecordSet –Name www –RecordType A -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup


    Name              : www
    ZoneName          : contoso.com
    ResourceGroupName : MyAzureResourceGroup
    Ttl               : 3600
    Etag              : 68e78da2-4d74-413e-8c3d-331ca48246d9
    RecordType        : A
    Records           : {134.170.185.46, 134.170.188.221}
    Tags              : {}


Az új rekord megadása lekérdezni az nslookup eszköz és más DNS-eszközök is használhatja.

Ha még nem delegált az Azure DNS névkiszolgálóinak a tartomány, meg kell explicit módon adja meg a nevét, a kiszolgáló és a cím, a zóna.


    nslookup www.contoso.com ns1-01.azure-dns.com

    Server: ns1-01.azure-dns.com
    Address:  208.76.47.1

    Name:    www.contoso.com
    Addresses:  134.170.185.46
                134.170.188.221

## <a name="create-a-record-set-of-each-type-with-a-single-record"></a>Egyetlen rekordot tartalmazó minden típusú rekord készlet létrehozása


Az alábbi példák bemutatják, hogyan minden bejegyzéstípus rekord létrehozásához. Minden rekord beállítása egyetlen olyan rekordot tartalmazza.

[AZURE.INCLUDE [dns-add-record-ps-include](../../includes/dns-add-record-ps-include.md)]


## <a name="next-steps"></a>Következő lépések

[Hogyan kell kezelni a DNS-zónák a PowerShell használatával](dns-operations-dnszones.md)

[DNS-rekordok kezelése, és a rekord állítja be a PowerShell használatával](dns-operations-recordsets.md)

[A .NET SDK Azure műveletek automatizálása](dns-sdk.md)
