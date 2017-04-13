<properties
   pageTitle="DNS-rekord beállítása és Azure CLI használatával Azure DNS-rekordok kezelése |} Microsoft Azure"
   description="DNS-rekord beállítása és Azure DNS-rekordok kezelése, ha a tartomány Azure DNS-szolgáltatója. Minden CLI parancs a rekord beállítása és a rekordok műveletekhez."
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
   ms.date="09/22/2016"
   ms.author="jtuliani"/>

# <a name="manage-dns-records-and-record-sets-by-using-cli"></a>DNS-rekordokat és a rekord beállítása CLI kezelése


> [AZURE.SELECTOR]
- [Azure portál](dns-operations-recordsets-portal.md)
- [Azure CLI](dns-operations-recordsets-cli.md)
- [A PowerShell](dns-operations-recordsets.md)


Ez a cikk bemutatja, hogyan rekord beállítása és a DNS-zóna rekordjainak kezelése a platformok Azure parancssori kezelőfelületről használatával.

Fontos, hogy a DNS-rekord beállítása és az egyes DNS-rekordok közötti különbség. Egy rekordkészletben gyűjteménye, amely lehet ugyanaz a neve, azonos típusú rekordok zónában. További tudnivalókért olvassa el a [ismertetése rekord beállítása és a rekordok](dns-getstarted-create-recordset-cli.md)című témakört.


## <a name="configure-the-cross-platform-azure-cli"></a>Az Azure-platformok CLI konfigurálása

Azure DNS egy Azure csak erőforrás-kezelő szolgáltatást. Nincsenek-Azure szolgáltatás felügyeleti API-val. Győződjön meg arról, hogy az Azure CLI van konfigurálva, az erőforrás-kezelő üzemmódban használatával a `azure config mode arm` parancsot.

Ha látható **Hiba: "dns" nem egy azure parancs**, annak oka az legvalószínűbb Azure CLI Azure Szolgáltatáskezelés módban, nem erőforrás-kezelő módban használja.

## <a name="create-a-new-record-set-and-record"></a>Hozzon létre egy új rekordkészletben és rögzítése

Hozzon létre egy új rekord beállítása az Azure-portálon, lásd: [a rekord beállítása és a bejegyzéseket hozhat létre](dns-getstarted-create-recordset-cli.md).


## <a name="retrieve-a-record-set"></a>Beolvashatja egy rekordkészletben

Egy meglévő rekordkészletben lekérdezni használata `azure network dns record-set show`. Adja meg a zone nevét, az erőforráscsoport rekord beállítása relatív nevét, és rekordtípust. Az alábbi példában az értékek lecserélése saját használja.

    azure network dns record-set show myresourcegroup contoso.com www A


## <a name="list-record-sets"></a>Lista-rekord beállítása

Használatával megtekintheti az összes DNS-zóna rekordjainak listáját a `azure network dns record-set list` parancsot. Adja meg az erőforrás-csoport nevét és zóna szüksége.

### <a name="to-list-all-record-sets"></a>A lista összes rekord beállítása

Ebben a példában a nevét vagy rekordtípus függetlenül minden rekord készletben adja eredményül:

    azure network dns record-set list myresourcegroup contoso.com

### <a name="to-list-record-sets-of-a-given-type"></a>Egy adott típusú rekord csoportjaihoz lista

Ez a példa minden rekord készletben, amelyek megfelelnek a megadott rekordtípusra (ebben az esetben az "Egy" rekordokat) adja eredményül:

    azure network dns record-set list myresourcegroup contoso.com A


## <a name="add-a-record-to-a-record-set"></a>Rekord hozzáadása egy rekordkészletben

Vesz fel rekordokat rekord beállítása használatával a `azure network dns record-set add-record`parancsot. Rekordok hozzáadása egy rekordkészletben paraméterek beállítása alatt álló rekordot típusától függően változnak. Például "A" típusú rekord bizonyos használatakor csak megadhatja rekordok a paraméter `-a <IPv4 address>`.

Hozzon létre egy rekordkészletben, használja a `azure network dns record-set create`parancsot. Adja meg a zone nevét, az erőforráscsoport rekord relatív neve, rekordtípust, és az idő beállítása Live (TTL). Ha a `--ttl` paraméter nincs megadva, négy alapértelmezett az érték (másodpercben).

    azure network dns record-set create myresourcegroup  contoso.com "test-a"  A --ttl 300


Miután létrehozta a "Egy" rekordkészletben, kiegészítése IPv4-címe a `azure network dns record-set add-record`parancsot.

    azure network dns record-set add-record myresourcegroup contoso.com "test-a" A -a 192.168.1.1


Az alábbi példák bemutatják, hogyan minden bejegyzéstípus rekord létrehozásához. Minden rekord beállítása egyetlen olyan rekordot tartalmazza.

[AZURE.INCLUDE [dns-add-record-cli-include](../../includes/dns-add-record-cli-include.md)]


## <a name="update-a-record-in-a-record-set"></a>Frissítse a rekordot egy rekordkészletben

### <a name="to-add-another-ip-address-1234-to-an-existing-a-record-set-www"></a>Egy másik IP-cím (1.2.3.4) felvenni egy meglévő "Egy" rekord megadása ("www"):

    azure network dns record-set add-record  myresourcegroup contoso.com  A
    -a 1.2.3.4
    info:    Executing command network dns record-set add-record
    Record set name: www
    + Looking up the dns zone "contoso.com"
    + Looking up the DNS record set "www"
    + Updating DNS record set "www"
    data:    Id                              : /subscriptions/################################/resourceGroups/myresourcegroup/providers/Microsoft.Network/dnszones/contoso.com/a/www
    data:    Name                            : www
    data:    Type                            : Microsoft.Network/dnszones/a
    data:    Location                        : global
    data:    TTL                             : 4
    data:    A records:
    data:        IPv4 address                : 192.168.1.1
    data:        IPv4 address                : 1.2.3.4
    data:
    info:    network dns record-set add-record command OK

### <a name="to-remove-an-existing-value-from-a-record-set"></a>Ha el szeretne távolítani egy meglévő érték egy rekordkészletben
Használat `azure network dns record-set delete-record`.

    azure network dns record-set delete-record myresourcegroup contoso.com www A -a 1.2.3.4
    info:    Executing command network dns record-set delete-record
    + Looking up the DNS record set "www"
    Delete DNS record? [y/n] y
    + Updating DNS record set "www"
    data:    Id                              : /subscriptions/################################/resourceGroups/myresourcegroup/providers/Microsoft.Network/dnszones/contoso.com/A/www
    data:    Name                            : www
    data:    Type                            : Microsoft.Network/dnszones/A
    data:    Location                        : global
    data:    TTL                             : 4
    data:    A records:
    data:    IPv4 address                    : 192.168.1.1
    data:
    info:    network dns record-set delete-record command OK



## <a name="remove-a-record-from-a-record-set"></a>Rekord eltávolítása egy rekordkészletben

Rekordok beállítása a rekord eltávolítható `azure network dns record-set delete-record`. A rekord, amely az eltávolítandó kell a meglévő rekordba pontosan egyező át az összes paramétert.

Az utolsó rekord eltávolítása egy rekordkészletben nem törli a rekordkészlet. További tudnivalókért olvassa el a [rekord törlése](#delete)című cikkben szakaszát.

    azure network dns record-set delete-record myresourcegroup contoso.com www A -a 192.168.1.1

    azure network dns record-set delete myresourcegroup contoso.com www A

### <a name="remove-an-aaaa-record-from-a-record-set"></a>Egy AAAA rekord eltávolítása egy rekordkészletben

    azure network dns record-set delete-record myresourcegroup contoso.com test-aaaa  AAAA -b "2607:f8b0:4009:1803::1005"

### <a name="remove-a-cname-record-from-a-record-set"></a>A CNAME rekord eltávolítása egy rekordkészletben

    azure network dns record-set delete-record myresourcegroup contoso.com test-cname CNAME -c www.contoso.com


### <a name="remove-an-mx-record-from-a-record-set"></a>Az MX rekord eltávolítása egy rekordkészletben

    azure network dns record-set delete-record myresourcegroup contoso.com "@" MX -e "mail.contoso.com" -f 5

### <a name="remove-an-ns-record-from-record-set"></a>Az NS rekord eltávolítása rekordkészletben

    azure network dns record-set delete-record myresourcegroup contoso.com  "test-ns" NS -d "ns1.contoso.com"

### <a name="remove-a-ptr-record-from-a-record-set"></a>PTR-rekordja eltávolítása egy rekordkészletben
Ebben az esetben "saját-arpa-zónában (Zone.com)" a ARPA zónában, amely az IP-tartomány jelöli.  IP-címet a IP-tartományon belül minden beállítása a ebbe a zónába PTR-rekordja felel meg.

    azure network dns record-set delete-record myresourcegroup my-arpa-zone.com "10" PTR -P "myservice.contoso.com"

### <a name="remove-an-srv-record-from-a-record-set"></a>A rekord eltávolítása egy rekordkészletben

    azure network dns record-set delete-record myresourcegroup contoso.com  "_sip._tls" SRV -p 0 -w 5 -o 8080 -u "sip.contoso.com"

### <a name="remove-a-txt-record-from-a-record-set"></a>TXT rekord eltávolítása egy rekordkészletben

    azure network dns record-set delete-record myresourcegroup contoso.com  "test-TXT" TXT -x "this is a TXT record"

## <a name="delete"></a>Rekord törlése

Rekord beállítása a segítségével törölheti a `Remove-AzureRmDnsRecordSet` parancsmag. Nem lehet törölni a SOA és NS rekord állítja be, a zóna csúcs (neve = "@") létrehozott automatikusan a zóna létrehozásakor. Azok automatikusan törlődnek, ha a zóna törlődik.

A következő példában az "Egy" rekord beállítása "próba-a" a "contoso.com" DNS-zóna kikerül:

    azure network dns record-set delete myresourcegroup contoso.com  "test-a" A

A választható *kérdések –* Váltás a megerősítést kérő mellőzése használható.


## <a name="next-steps"></a>Következő lépések

Azure DNS szolgáltatással kapcsolatos további tudnivalókért olvassa el a [Azure DNS – áttekintés](dns-overview.md)című témakört. Tudni DNS automatizálása olvassa el a [létrehozásához a DNS-zónák és rekord beállítása a .NET SDK használatával](dns-sdk.md)című témakört.

Ha fordított DNS-rekordok kezelése, megtudhatja, [hogy miként kezelje DNS-rekordjait a szolgáltatások használata az Azure CLI fordított](dns-reverse-dns-record-operations-cli.md).
