<properties
   pageTitle="Az Azure portálon kezelheti DNS-rekord beállítása és a rekordok |} Microsoft Azure"
   description="DNS-rekord beállítása és Azure DNS-rekordok kezelése, ha a tartomány Azure DNS-szolgáltatója. Az összes PowerShell-parancsait a rekord beállítása és a rekordok műveletekhez"
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

# <a name="manage-dns-records-and-record-sets-by-using-powershell"></a>DNS-rekordok kezelése, és a rekord állítja be a PowerShell használatával



> [AZURE.SELECTOR]
- [Azure portál](dns-operations-recordsets-portal.md)
- [Azure CLI](dns-operations-recordsets-cli.md)
- [A PowerShell](dns-operations-recordsets.md)



Ez a cikk bemutatja, hogyan kezelheti a rekord beállítása és a DNS-zóna rekordjainak a Windows PowerShell használatával.

Fontos, hogy a DNS-rekord beállítása és az egyes DNS-rekordok közötti különbség. Egy rekordkészletben gyűjteménye, amely lehet ugyanaz a neve, azonos típusú zóna rekordjainak. További tudnivalókért lásd: [Hozzon létre a DNS-rekord beállítása és a rekordok az Azure portál használatával](dns-getstarted-create-recordset-portal.md).

A rekord beállítása és a rekordok kezelésének, szüksége van az Azure erőforrás-kezelő PowerShell-parancsmagok legújabb verzióját. További információért megtudhatja, [hogy miként telepítheti, állíthatja Azure PowerShell](../powershell-install-configure.md). További információt a PowerShell használata [Azure PowerShell használata az Azure erőforrás-kezelő](../powershell-azure-resource-manager.md)témakörben találhat.



## <a name="create-a-new-record-set-and-a-record"></a>Hozzon létre egy új rekordkészletben és a rekord létrehozása

Hozzon létre egy rekordot, a PowerShell használatával, lásd: [Hozzon létre a DNS-rekord beállítása és a rekordok PowerShell használatával](dns-getstarted-create-recordset.md).

## <a name="get-a-record-set"></a>Első egy rekordkészletben

Egy meglévő rekordkészletben lekérdezni használata `Get-AzureRmDnsRecordSet`. Adja meg a rekord beállítása relatív név, a record type és az időzónát.

    $rs = Get-AzureRmDnsRecordSet –Name www –RecordType A -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup

Hasonlóan `New-AzureRmDnsRecordSet`, a rekord neve relatív nevét, ami azt jelenti, kell zárnia a zóna nevét kell lennie.

A zone a zóna neve és az erőforrás csoport neve, illetve a zóna objektum használatával adhatja meg:

    $zone = Get-AzureRmDnsZone -Name contoso.com -ResouceGroupName MyAzureResourceGroup
    $rs = Get-AzureRmDnsRecordSet -Name www –RecordType A -Zone $zone

`Get-AzureRmDnsRecordSet`a helyi objektumot, amely a rekord beállítása a Azure DNS-ben létrehozott adja vissza.

## <a name="list-record-sets"></a>Lista-rekord beállítása

Is használhatja`Get-AzureRmDnsRecordSet` lista rekord csoportján, ha nincs megadva a *– név* és/vagy *– RecordType* paraméterek.

### <a name="to-list-all-record-sets"></a>A lista összes rekord beállítása

Ebben a példában a nevét vagy rekordtípus függetlenül minden rekord készletben adja eredményül:

    $list = Get-AzureRmDnsRecordSet -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup

### <a name="to-list-record-sets-of-a-given-record-type"></a>Egy adott rekordtípust rekord csoportjaihoz lista

Ez a példa megfelelő az adott rekordtípus minden rekord készletben adja eredményül. Ebben az esetben a rekord, amely beállítása "A" rekordok visszaadott érték:

    $list = Get-AzureRmDnsRecordSet –RecordType A -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup

A zone vagy a *Zóna_neve –* és *– ResourceGroupName* paramétereket (lásd az), vagy a zóna objektum megadásával adható meg:

    $zone = Get-AzureRmDnsZone -Name contoso.com -ResouceGroupName MyAzureResourceGroup
    $list = Get-AzureRmDnsRecordSet -Zone $zone

## <a name="add-a-record-to-a-record-set"></a>Rekord hozzáadása egy rekordkészletben

Vesz fel rekordokat rekord beállítása használatával a `Add-AzureRmDnsRecordConfig` parancsmag. Ez a kapcsolat nélküli műveletet. Csak a helyi objektumot, amely a rekord beállítása módosul.

Rekordok hozzáadása egy rekordkészletben paramétereinek a rekordkészlet típusától függően változnak. Például "A" típusú rekord bizonyos használatakor csak megadhatja rekordok a paraméter *-IPv4-cím*.

További rekordok manuálisan is hozzáadhatók az egyes rekordok által további hívások beállítása `Add-AzureRmDnsRecordConfig`. Bármely rekordkészletben legfeljebb 20 rekordok is adhat. Azonban "CNAME" típusú rekord beállítása tartalmazhat legfeljebb egy rekordot, és egy rekordkészletben két elemekről nem tartalmazhat. Üres rekord beállítása (a nulla rekordok) hozhatók létre, de nem jelennek meg az Azure DNS-névkiszolgálókat.

A rekordkészletben tartalmazza a kívánt gyűjtemény a rekordok, szükség használatával végrehajtani a `Set-AzureRmDnsRecordSet` parancsmag. Miután követtek egy rekordkészletben, lecseréli a meglévő rekord beállítása a Azure DNS-ben.

### <a name="to-create-an-a-record-set-with-a-single-record"></a>Az egyetlen olyan rekordot be A rekord létrehozása

    $rs = New-AzureRmDnsRecordSet -Name "test-a" -RecordType A -Ttl 60 -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    Add-AzureRmDnsRecordConfig -RecordSet $rs -Ipv4Address "1.2.3.4"
    Set-AzureRmDnsRecordSet -RecordSet $rs

Létrehozhat egy bejegyzést a műveletsort is *adatcsatornán*, ami azt jelenti, a rekordkészletben objektum beengedné paraméterként helyett használja a függőleges vonás útján adják át. Példa:

    New-AzureRmDnsRecordSet -Name "test-a" -RecordType A -Ttl 60 -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup | Add-AzureRmDnsRecordConfig -Ipv4Address "1.2.3.4" | Set-AzureRmDnsRecordSet

### <a name="additional-record-type-examples"></a>További rekordtípus példák

[AZURE.INCLUDE [dns-add-record-ps-include](../../includes/dns-add-record-ps-include.md)]

## <a name="modify-existing-record-sets"></a>Módosítsa a meglévő rekord beállítása

Egy meglévő rekordkészletben módosítása lépései hasonlóak megtett lépések rögzítéséhez rekordok létrehozásakor. A műveletek sorrendjét a következőképpen történik:

1.  Használatával a meglévő rekord beolvasása `Get-AzureRmDnsRecordSet`.

2.  Módosíthatja a rekordok hozzáadása, eltávolítása a rekordok, a rekord paramétereinek módosítása a rekordot, vagy a rekord módosítása adja a time to live (TTL). Ez a kapcsolat nélküli műveletet. Csak a helyi objektumot, amely a rekord beállítása módosul.

3.  A módosítások véglegesítése használatával a `Set-AzureRmDnsRecordSet` parancsmag. Ez az eljárás lecseréli a meglévő rekord beállítása a Azure DNS-ben.


### <a name="to-update-a-record-in-an-existing-record-set"></a>Egy meglévő rekord beállítása egy rekord módosítása

Ebben a példában azt egy meglévő "Egy" rekord IP-címének módosítása:

    $rs = Get-AzureRmDnsRecordSet -name "test-a" -RecordType A -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    $rs.Records[0].Ipv4Address = "134.170.185.46"
    Set-AzureRmDnsRecordSet -RecordSet $rs

A `Set-AzureRmDnsRecordSet` parancsmag etag ellenőrzések használja annak érdekében, hogy egyidejű változtatások nem írja felül. Használja a *-felülírása* Üzenetjelölő ellenőrzések mellőzése. További információ című témakörben [olvashat etags és a címkék](dns-getstarted-create-dnszone.md#tagetag).

### <a name="to-modify-an-soa-record"></a>Egy SOA rekord módosítása

Nem lehet hozzáadni vagy rekordok eltávolítása a zóna csúcs értéken automatikusan létrehozott SOA rekord (neve = "@"). A paraméterek (kivéve az "Host") a SOA rekordon belül bármelyikét módosíthatja, és a rekord beállítása a TTL (élettartam).

A következő példa bemutatja, hogyan módosíthatja a SOA rekord az *e-mailek* tulajdonsága:

    $rs = Get-AzureRmDnsRecordSet -Name "@" -RecordType SOA -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    $rs.Records[0].Email = "admin.contoso.com"
    Set-AzureRmDnsRecordSet -RecordSet $rs

### <a name="to-modify-ns-records-at-the-zone-apex"></a>A zone csúcs a Névkiszolgálói rekordok módosítása

Nem hozzáadása, eltávolítása, és nem módosíthatók a rekordok automatikusan létrehozott NS rekord megadása elemre a zóna csúcs (neve = "@"). Csak módosítása, hogy engedélyezve van TTL (élettartam) rekordkészletben módosításához szükséges engedélyekkel.

A következő példa bemutatja, hogyan módosíthatja a TTL (élettartam) tulajdonsága NS rekord megadása:

    $rs = Get-AzureRmDnsRecordSet -Name "@" -RecordType NS -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    $rs.Ttl = 300
    Set-AzureRmDnsRecordSet -RecordSet $rs

### <a name="to-add-records-to-an-existing-record-set"></a>Rekordok felvétele egy meglévő rekord beállítása

Ebben a példában a meglévő rekordkészlet két további MX-rekordok hozzáadása azt:

    $rs = Get-AzureRmDnsRecordSet -name "test-mx" -RecordType MX -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    Add-AzureRmDnsRecordConfig -RecordSet $rs -Exchange "mail2.contoso.com" -Preference 10
    Add-AzureRmDnsRecordConfig -RecordSet $rs -Exchange "mail3.contoso.com" -Preference 20
    Set-AzureRmDnsRecordSet -RecordSet $rs

## <a name="remove-a-record-from-an-existing-record-set"></a>Rekord eltávolítása egy meglévő rekord beállítása

Rekordok beállítása a rekord eltávolítható `Remove-AzureRmDnsRecordConfig`. A rekord, amely az eltávolítandó kell a meglévő rekordba pontosan egyező át az összes paramétert. Módosítások kell lekötött, használatával `Set-AzureRmDnsRecordSet`.

Az utolsó rekord eltávolítása egy rekordkészletben nem törli a rekordkészlet. [Rekord törlése](#delete-a-record-set) alatti talál további információt.


    $rs = Get-AzureRmDnsRecordSet -Name "test-a" -RecordType A -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    Remove-AzureRmDnsRecordConfig -RecordSet $rs -Ipv4Address "1.2.3.4"
    Set-AzureRmDnsRecordSet -RecordSet $rs

Ha el szeretne távolítani egy bejegyzést egy rekordkészletben a műveletsort is átirányítható, ami azt jelenti, a rekordkészletben objektum beengedné paraméterként helyett használja a függőleges vonás útján adják át. Példa:

    Get-AzureRmDnsRecordSet -Name "test-a" -RecordType A -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup | Remove-AzureRmDnsRecordConfig -Ipv4Address "1.2.3.4" | Set-AzureRmDnsRecordSet

### <a name="remove-an-aaaa-record-from-a-record-set"></a>Egy AAAA rekord eltávolítása egy rekordkészletben

    $rs = Get-AzureRmDnsRecordSet -Name "test-aaaa" -RecordType AAAA -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    Remove-AzureRmDnsRecordConfig -RecordSet $rs -Ipv6Address "2607:f8b0:4009:1803::1005"
    Set-AzureRmDnsRecordSet -RecordSet $rs

### <a name="remove-a-cname-record-from-a-record-set"></a>A CNAME rekord eltávolítása egy rekordkészletben

Mivel a CNAME rekord beállítása legfeljebb egy rekordot is tartalmazhatnak, egy üres rekordkészletben eltávolítása, a rekordot elhagyja.

    $rs =  Get-AzureRmDnsRecordSet -name "test-cname" -RecordType CNAME -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    Remove-AzureRmDnsRecordConfig -RecordSet $rs -Cname "www.contoso.com"
    Set-AzureRmDnsRecordSet -RecordSet $rs

### <a name="remove-an-mx-record-from-a-record-set"></a>Az MX rekord eltávolítása egy rekordkészletben

    $rs = Get-AzureRmDnsRecordSet -name "test-mx" -RecordType MX -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    Remove-AzureRmDnsRecordConfig -RecordSet $rs -Exchange "mail.contoso.com" -Preference 5
    Set-AzureRmDnsRecordSet -RecordSet $rs

### <a name="remove-an-ns-record-from-record-set"></a>Az NS rekord eltávolítása rekordkészletben

    $rs = Get-AzureRmDnsRecordSet -Name "test-ns" -RecordType NS -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    Remove-AzureRmDnsRecordConfig -RecordSet $rs -Nsdname "ns1.contoso.com"
    Set-AzureRmDnsRecordSet -RecordSet $rs

### <a name="remove-an-srv-record-from-a-record-set"></a>A rekord eltávolítása egy rekordkészletben

    $rs = Get-AzureRmDnsRecordSet -Name "_sip._tls" -RecordType SRV -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    Remove-AzureRmDnsRecordConfig -RecordSet $rs –Priority 0 –Weight 5 –Port 8080 –Target "sip.contoso.com"
    Set-AzureRmDnsRecordSet -RecordSet $rs

### <a name="remove-a-txt-record-from-a-record-set"></a>TXT rekord eltávolítása egy rekordkészletben

    $rs = Get-AzureRmDnsRecordSet -Name "test-txt" -RecordType TXT -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    Remove-AzureRmDnsRecordConfig -RecordSet $rs -Value "This is a TXT record"
    Set-AzureRmDnsRecordSet -RecordSet $rs

## <a name="delete-a-record-set"></a>Rekord törlése

Rekord beállítása a segítségével törölheti a `Remove-AzureRmDnsRecordSet` parancsmag. Nem lehet törölni a SOA és NS rekord állítja be, a zóna csúcs (neve = "@") létrehozott automatikusan a zóna létrehozásakor. Azok automatikusan törlődnek, ha a zóna törlődik.

Ha el szeretne távolítani egy rekordkészletben használja az alábbi módszerek egyikét:

### <a name="specify-all-the-parameters-by-name"></a>Adja meg a paraméterek neve

A választható *-hatályba* kapcsoló akkor használható mellőzése a megerősítést kérő párbeszédpanelen.

    Remove-AzureRmDnsRecordSet -Name "test-a" -RecordType A -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup [-Force]


### <a name="specify-the-record-set-by-name-and-type-and-specify-the-zone-by-object"></a>Adja meg a név és típus szerint rekordkészlet, és adja meg a zone objektum

    $zone = Get-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup
    Remove-AzureRmDnsRecordSet -Name "test-a" -RecordType A -Zone $zone [-Force]

### <a name="specify-the-record-set-by-object"></a>Adja meg a bejegyzés objektum beállítása

    $rs = Get-AzureRmDnsRecordSet -Name "test-a" -RecordType A -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    Remove-AzureRmDnsRecordSet –RecordSet $rs [-Overwrite] [-Force]

Amikor a rekord beállítása objektuma segítségével adja meg, lehetővé teszi annak érdekében, hogy a program nem törli egyidejű változtatások etag ellenőrzések. A választható *-felülírása* jelző ellenőrzések letiltja. További információt talál [Etags és a címkék](dns-getstarted-create-dnszone.md#tagetag) .

A rekordkészletben objektum is átirányítható paraméterként átadott helyett:

    Get-AzureRmDnsRecordSet -Name "test-a" -RecordType A -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup | Remove-AzureRmDnsRecordSet [-Overwrite] [-Force]

## <a name="next-steps"></a>Következő lépések

Azure DNS szolgáltatással kapcsolatos további tudnivalókért olvassa el a [Azure DNS – áttekintés](dns-overview.md)című témakört. Tudni DNS automatizálása olvassa el a [létrehozásához a DNS-zónák és rekord beállítása a .NET SDK használatával](dns-sdk.md)című témakört.

További információt a fordított DNS-rekordok [kezelése a PowerShell használatá szolgáltatások fordított DNS-rekordok](dns-reverse-dns-record-operations-ps.md)megtekintése
