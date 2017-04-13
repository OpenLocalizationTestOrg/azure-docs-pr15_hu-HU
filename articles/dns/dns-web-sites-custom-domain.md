<properties
   pageTitle="Egy webalkalmazás egyéni DNS-rekordok létrehozása |} Microsoft Azure  "
   description="Bemutatja, hogyan hozhat létre egyéni tartomány DNS-rekordok webalkalmazás Azure DNS használatával."
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

# <a name="create-dns-records-for-a-web-app-in-a-custom-domain"></a>Egy webalkalmazás DNS-rekordok létrehozása az egyéni tartomány

Egyéni tartomány tárolni a web Apps alkalmazások Azure DNS is használhatja. Például az Azure-webappokban hoz létre, és azt szeretné, hogy a felhasználók által vagy eléréséhez a contoso.com vagy www.contoso.com tartománynév használják.

Ehhez van két rekord létrehozásához:

- Mutasson a contoso.com legfelső szintű "A" rekord létrehozása
- A www nevét, amely az A rekordot mutat "CNAME" rekordot

Felhívjuk a figyelmét arra, hogy az Azure-webalkalmazást A rekordot hoz létre, ha az A rekordot kell manuálisan frissíthető, ha az alapul szolgáló IP-cím web app megváltozott.

## <a name="before-you-begin"></a>Első lépések

Megkezdése előtt kell először hozzon létre egy DNS-zóna Azure DNS-ben, és a zóna delegálása az Azure DNS-tartományregisztrálója.

1. A DNS-zóna létrehozásához kövesse a [DNS-zóna](dns-getstarted-create-dnszone.md)létrehozása.
2. A DNS-Azure DNS-delegálása, kövesse a [DNS-tartomány delegálás](dns-domain-delegation.md).

Miután létrehozta a zónán, és Azure DNS-meghatalmazó azt, majd készíthet rekordok az egyéni tartomány.


## <a name="1-create-an-a-record-for-your-custom-domain"></a>1. az egyéni tartomány egy A-rekord létrehozása

Az A rekord nevét megfeleltetése IP-címének szolgál. A következő példa azt hozzárendel @ olyan IPv4-címet A rekorddal:

### <a name="step-1"></a>Lépés: 1

Hozzon létre egy A rekordot, és egy változó $rs hozzárendelése

    $rs= New-AzureRMDnsRecordSet -Name "@" -RecordType "A" -ZoneName "contoso.com" -ResourceGroupName "MyAzureResourceGroup" -Ttl 600

### <a name="step-2"></a>Lépés: 2

Adja hozzá a IPv4-értéket a korábban létrehozott rekordkészlet "@" a hozzárendelt $rs változó használatával. A hozzárendelt IPv4-értéket a webalkalmazás az IP-címe lesz.

Az IP-cím webalkalmazást megkereséséhez hajtsa végre az [Azure App szolgáltatásban a saját tartománynév beállítása](../web-sites-custom-domain-name.md#Find-the-virtual-IP-address)című témakör lépéseit.

    Add-AzureRMDnsRecordConfig -RecordSet $rs -Ipv4Address <your web app IP address>

### <a name="step-3"></a>3 lépés

A módosítások a rekordkészlet. Használat `Set-AzureRMDnsRecordSet` az Azure DNS-rekordot a módosítások feltöltése:

    Set-AzureRMDnsRecordSet -RecordSet $rs

## <a name="2-create-a-cname-record-for-your-custom-domain"></a>2. az egyéni tartomány CNAME rekordjának létrehozása

Ha már kezeli tartománya Azure DNS (lásd: [DNS-tartomány delegálás](dns-domain-delegation.md), használhatja a következő a példa contoso.azurewebsites.net CNAME rekord létrehozásához.

### <a name="step-1"></a>Lépés: 1

Nyissa meg a PowerShell, és hozzon létre egy új CNAME rekord beállítása, és hozzárendelése egy változó $rs. Ez a példa hoz létre egy rekordkészletben típus CNAME "élő idővel" 600 másodperc DNS-zóna neve "contoso.com".

    $rs = New-AzureRMDnsRecordSet -ZoneName contoso.com -ResourceGroupName myresourcegroup -Name "www" -RecordType "CNAME" -Ttl 600

    Name              : www
    ZoneName          : contoso.com
    ResourceGroupName : myresourcegroup
    Ttl               : 600
    Etag              : 8baceeb9-4c2c-4608-a22c-229923ee1856
    RecordType        : CNAME
    Records           : {}
    Tags              : {}


### <a name="step-2"></a>Lépés: 2

Miután létrehozta a CNAME rekord beállítása, hozzon létre egy alias érték, amely a web app fog mutatni szeretne.

A korábban már kiosztott változó "$rs" használatával segítségével az alábbi PowerShell-parancsot a web app contoso.azurewebsites.net aliasa létrehozása.

    Add-AzureRMDnsRecordConfig -RecordSet $rs -Cname "contoso.azurewebsites.net"

    Name              : www
    ZoneName          : contoso.com
    ResourceGroupName : myresourcegroup
    Ttl               : 600
    Etag              : 8baceeb9-4c2c-4608-a22c-229923ee185
    RecordType        : CNAME
    Records           : {contoso.azurewebsites.net}
    Tags              : {}

### <a name="step-3"></a>3 lépés

Hajtsa végre a módosításokat a `Set-AzureRMDnsRecordSet` parancsmagot:

    Set-AzureRMDnsRecordSet -RecordSet $rs

Ellenőrizheti, hogy a rekord hozta létre megfelelően, az nslookup használatával "www.contoso.com" lekérdezése az alább látható módon:

    PS C:\> nslookup
    Default Server:  Default
    Address:  192.168.0.1

    > www.contoso.com
    Server:  default server
    Address:  192.168.0.1

    Non-authoritative answer:
    Name:    <instance of web app service>.cloudapp.net
    Address:  <ip of web app service>
    Aliases:  www.contoso.com
    contoso.azurewebsites.net
    <instance of web app service>.vip.azurewebsites.windows.net

## <a name="create-an-awverify-record-for-web-apps"></a>Hozzon létre egy "awverify" rekordot web Apps alkalmazások


Ha úgy dönt, hogy egy A rekordot használják a webalkalmazás, haladjon végig a tartományigazolási folyamatot annak érdekében, hogy az egyéni tartomány tulajdonjogának. Ebben a lépésben ellenőrzése: hozzon létre egy speciális CNAME rekord "awverify" nevű történik. Ez a szakasz csak A rekordok vonatkozik.


### <a name="step-1"></a>Lépés: 1

A "awverify" rekord létrehozására. Az alábbi példában azt az egyéni tartomány tulajdonjogának ellenőrzéséhez a contoso.com a "aweverify" rekordot hoz létre.

    $rs = New-AzureRMDnsRecordSet -ZoneName contoso.com -ResourceGroupName myresourcegroup -Name "awverify" -RecordType "CNAME" -Ttl 600

    Name              : awverify
    ZoneName          : contoso.com
    ResourceGroupName : myresourcegroup
    Ttl               : 600
    Etag              : 8baceeb9-4c2c-4608-a22c-229923ee1856
    RecordType        : CNAME
    Records           : {}
    Tags              : {}


### <a name="step-2"></a>Lépés: 2

"Awverify" rekordkészletben létrehozását követően rendelje hozzá a CNAME rekord beállítása aliast. Az alábbi példában a CNAMe rekordot, más néven értékűre awverify.contoso.azurewebsites.net hozzárendel azt.

    Add-AzureRMDnsRecordConfig -RecordSet $rs -Cname "awverify.contoso.azurewebsites.net"

    Name              : awverify
    ZoneName          : contoso.com
    ResourceGroupName : myresourcegroup
    Ttl               : 600
    Etag              : 8baceeb9-4c2c-4608-a22c-229923ee185
    RecordType        : CNAME
    Records           : {awverify.contoso.azurewebsites.net}
    Tags              : {}

### <a name="step-3"></a>3 lépés

Hajtsa végre a módosításokat a `Set-AzureRMDnsRecordSet cmdlet`, ahogy az alábbi parancsot.

    Set-AzureRMDnsRecordSet -RecordSet $rs



## <a name="next-steps"></a>Következő lépések

Kövesse a [alkalmazás szolgáltatásbeli saját tartománynév beállítása](../app-service-web/web-sites-custom-domain-name.md) az egyéni tartomány használata a webalkalmazás konfigurálása.








