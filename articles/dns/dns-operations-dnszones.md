<properties
   pageTitle="Kezelése a PowerShell használatá DNS-zónák |} Microsoft Azure"
   description="DNS-zónák Azure Powershell használatával kezelheti. Hogyan frissítheti, törölheti, vagy hozzon létre a DNS-zónák Azure DNS"
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

# <a name="how-to-manage-dns-zones-using-powershell"></a>Hogyan kell kezelni a DNS-zónák a PowerShell használatával

> [AZURE.SELECTOR]
- [Azure CLI](dns-operations-dnszones-cli.md)
- [A PowerShell](dns-operations-dnszones.md)



Ez a cikk bemutatja, hogyan kezelheti a DNS-zóna PowerShell használatával. Használja ezeket a lépéseket, hogy szüksége lesz az Azure erőforrás-kezelő PowerShell-parancsmagok (1.0 vagy újabb) legújabb verziójának telepítéséhez. Megtudhatja, [hogy miként telepítheti, állíthatja Azure PowerShell](../powershell-install-configure.md) PowerShell-parancsmagok telepítésével kapcsolatos további információt.


## <a name="create-a-new-dns-zone"></a>Hozzon létre egy új DNS-zóna

A DNS-zóna létrehozásához lásd: a [Create a DNS-zóna PowerShell használatával](dns-getstarted-create-dnszone.md).

## <a name="get-a-dns-zone"></a>A DNS-zóna beszerzése

A DNS-zóna beolvasásához, használja a `Get-AzureRmDnsZone` parancsmag. Ez a művelet a DNS zone objektum egy meglévő zóna Azure DNS-ben való megfelelő adja eredményül. Az objektum (például az a rekord beállítása) a zóna kapcsolatos adatokat tartalmaz, de nem tartalmaz a rekord beállítása magukat.

    $zone = Get-AzureRmDnsZone -Name contoso.com –ResourceGroupName MyAzureResourceGroup

## <a name="list-dns-zones"></a>Lista DNS-zónák

A zóna nevét a név elhagyása által `Get-AzureRmDnsZone`, akkor is számba veheti a webhely összes zónák egy erőforrás csoportban. Ez a művelet a zóna objektumok tömbjét adja eredményül.

    $zoneList = Get-AzureRmDnsZone -ResourceGroupName MyAzureResourceGroup

## <a name="update-a-dns-zone"></a>Frissítse a DNS-zóna

A DNS-zóna erőforrás is lehet módosítani a `Set-AzureRmDnsZone`. Ez nem frissíti a DNS-rekord készletek a zónán belül (megtudhatja, [hogy miként kezelheti a DNS-rekordok](dns-operations-recordsets.md)). Csak a zóna erőforrás magát tulajdonság frissítése szolgál. Ez a jelenleg csak a az Azure erőforrás-kezelő "címkék" a zóna erőforrás. További információt talál [Etags és a címkék](dns-getstarted-create-dnszone.md#Etags-and-tags) .

Update DNS-zóna kétféle módon használja az alábbiak egyikét:

### <a name="specify-the-zone-using-the-zone-name-and-resource-group"></a>Adja meg a zónában, a zóna nevét és az erőforrás csoport használata

    Set-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup [-Tag $tags]

### <a name="specify-the-zone-using-a-zone-object"></a>Adja meg a zone egy $zone objektuma segítségével

Adja meg a zóna használata egy $zone objektumot `Get-AzureRmDnsZone`. Használatakor `Set-AzureRmDnsZone` $zone objektummal Etag ellenőrzések használandó egyidejű változtatások nem írják felül biztosítása. A választható használható *-felülírja a* váltás ellenőrzések mellőzése. További információt talál [Etags és a címkék](dns-getstarted-create-dnszone.md#Etags-and-tags) .


    $zone = Get-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup
    <..modify $zone.Tags here...>
    Set-AzureRmDnsZone -Zone $zone [-Overwrite]


## <a name="delete-a-dns-zone"></a>A DNS-zóna törlése

DNS-zónák a eltávolítása-AzureRmDnsZone parancsmaggal is törli.

Mielőtt törölné a DNS-zóna Azure DNS-ben, szüksége lesz rekordok minden készletben, kivéve a SOA és a Névkiszolgálói rekordok a legfelső szintű a zóna a zóna létrehozásakor automatikusan létrehozott törlése.

Ha el szeretné távolítani a DNS-zóna az alábbi két módszer egyikével:

### <a name="specify-the-zone-using-the-zone-name-and-resource-group-name"></a>Adja meg a zónában, a zóna neve és a csoport erőforrásnév

Ez a művelet tartalmaz egy nem kötelező *-hatályba* kapcsoló, amelyben el szeretné távolítani a DNS-zóna megerősítését kérő letiltja.

    Remove-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup [-Force]

### <a name="specify-the-zone-using-a-zone-object"></a>Adja meg a zone egy $zone objektuma segítségével

Adja meg a zóna használata egy $zone objektumot `Get-AzureRmDnsZone`. Ez a művelet tartalmaz egy nem kötelező *-hatályba* kapcsoló, amelyben el szeretné távolítani a DNS-zóna megerősítését kérő letiltja. Hasonlóan `Set-AzureRmDnsZone`, adja meg a zone egy $zone objektuma segítségével lehetővé teszi, hogy a Etag ellenőrzi, hogy győződjön meg róla, egyidejű változtatások nem törlődnek. <BR>
A választható *-felülírása* jelző ellenőrzések letiltja. További információt talál [Etags és a címkék](dns-getstarted-create-dnszone.md#Etags-and-tags) .

    $zone = Get-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup
    Remove-AzureRmDnsZone -Zone $zone [-Force] [-Overwrite]



A zone objektum is átirányítható paraméterként átadott helyett:

    Get-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup | Remove-AzureRmDnsZone [-Force] [-Overwrite]

## <a name="next-steps"></a>Következő lépések

Miután létrehozta a DNS-zóna, létrehozása [rekord beállítása és a rekordok](dns-getstarted-create-recordset.md) névfeloldás tartománya internetes indításához.