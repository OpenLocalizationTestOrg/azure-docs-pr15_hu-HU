<properties
   pageTitle="Az Azure fürt és az erőforrások törlése |} Microsoft Azure"
   description="Megtudhatja, hogy milyen teljesen törölni szeretne egy szolgáltatás háló fürt vagy törlése az erőforráscsoport a fürt tartalmazó vagy erőforrásokat szelektív törlésével."
   services="service-fabric"
   documentationCenter=".net"
   authors="ChackDan"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/09/2016"
   ms.author="chackdan"/>

# <a name="delete-a-service-fabric-cluster-on-azure-and-the-resources-it-uses"></a>Azure és az erőforrások használ szolgáltatási háló fürt törlése

A szolgáltatás háló fürtre sok Azure erőforrás mellett az fürt erőforrás magát tevődik össze. Úgy, hogy teljesen törölni a szolgáltatás háló fürtre történik, az összes erőforrás törölnie kell.
Két lehetőség közül választhat: vagy törlése az erőforrás csoportra a fürt (amely a fürt erőforrás és törlése más erőforrások erőforráscsoport) vagy a fürt erőforrás kifejezetten törlése és a hozzá társított erőforrások (de nem az erőforráscsoport más erőforrások).

>[AZURE.NOTE] A fürt erőforrás **nem** törlése az összes az egyéb információforrások, amelyek a szolgáltatás háló fürt áll.

## <a name="delete-the-entire-resource-group-rg-that-the-service-fabric-cluster-is-in"></a>A teljes erőforráscsoport (RG), amely a szolgáltatás háló fürt törlése

Győződjön meg arról, hogy törli a fürthöz, beleértve az erőforráscsoport rendelt összes erőforrás legegyszerűbb módja: az. A PowerShell használatá erőforráscsoport törölheti vagy az Azure portálon keresztül. Ha az erőforráscsoport információforrások, amelyek nem kapcsolódnak szolgáltatás háló fürt, ezután törölheti adott erőforrások.

### <a name="delete-the-resource-group-using-azure-powershell"></a>Az erőforráscsoport Azure PowerShell használatával törlése

Az erőforráscsoport a következő Azure PowerShell-parancsmagok futtatásával is törölheti. Győződjön meg arról, hogy Azure PowerShell 1.0, vagy nagyobb telepítve van a számítógépen. Ha nem ezt előtt, hajtsa végre az című cikk lépéseit [telepítése és beállítása Azure PowerShell.](../powershell-install-configure.md)

Nyisson meg egy PowerShell-ablakot, és futtassa a következő PS parancsmagok:

```powershell
Login-AzureRmAccount

Remove-AzureRmResourceGroup -Name <name of ResouceGroup> -Force
```

A törlés megerősítéséhez, ha nem használta a kérdés jelenik meg a *-hatályba* lehetőséget. A megerősítést kérő a RG és a benne összes erőforrás törlődnek.

### <a name="delete-a-resource-group-in-the-azure-portal"></a>Az Azure-portálon erőforrás csoport törlése  

1. Jelentkezzen be az [Azure-portálon](https://portal.azure.com).
2. Nyissa meg a törölni kívánt szolgáltatás háló fürt.
3. Kattintson a fürt alapvető tudnivalók lapon az erőforrás csoport nevére.
4. Ekkor megjelenik az **Erőforrás csoport Essentials** lapon.
5. Kattintson a **Törlés**gombra.
6. Az erőforráscsoport a törlés végrehajtásához lapon kövesse az utasításokat.

![Erőforrás-csoport törlése][ResourceGroupDelete]


## <a name="delete-the-cluster-resource-and-the-resources-it-uses-but-not-other-resources-in-the-resource-group"></a>A fürt erőforrás és az erőforrásokat, akkor használja, de nem az erőforráscsoport más erőforrások törlése

Ha az erőforráscsoport csak a törölni kívánt szolgáltatás háló fürt kapcsolódó erőforrásokat, majd célszerűbb az erőforrás teljes csoport törlése. Ha szelektív törölni szeretné az erőforráscsoport az erőforrások egyesével –, majd kövesse az alábbi lépéseket.

Ha telepítette az fürt a portálon vagy a sablonok használata az erőforrás-kezelő szolgáltatás háló sablonok közül, a fürt használó összes erőforrás címkézve van a következő két címkéket. Döntse el, milyen erőforrások törölni kívánt használhatja őket.

***#1 címke:*** Kulcs clusterName, érték = = "a fürt neve"

***#2 címke:*** Kulcs erőforrásnév, érték = = ServiceFabric

### <a name="delete-specific-resources-in-the-azure-portal"></a>Az Azure-portálon adott erőforrások törlése

1. Jelentkezzen be az [Azure-portálon](https://portal.azure.com).
2. Nyissa meg a törölni kívánt szolgáltatás háló fürt.
3. Nyissa meg az alapvető tudnivalók lap **összes beállítást** .
4. A **címkék** az **Erőforrás-kezelés** csoportban kattintson a beállítások lap.
5. Kattintson a lista beszerzése az adott címkével megjelölt összes erőforrás a Címkék lap a **címkék** közül.

    ![Erőforrás-címkék][ResourceTags]

6. Ha befejezte a címkézett erőforrások listája, az erőforrások parancsára, és törölje őket.

    ![Címkézett erőforrások][TaggedResources]

### <a name="delete-the-resources-using-azure-powershell"></a>Az Azure PowerShell használatával erőforrások törlése

Az erőforrások egy az egyhez szerint törölheti a következő Azure PowerShell-parancsmagok futtatásával. Győződjön meg arról, hogy Azure PowerShell 1.0, vagy nagyobb telepítve van a számítógépen. Ha nem ezt előtt, hajtsa végre az című cikk lépéseit [telepítése és beállítása Azure PowerShell.](../powershell-install-configure.md)

Nyisson meg egy PowerShell-ablakot, és futtassa a következő PS parancsmagok:

```powershell
Login-AzureRmAccount
```
Az egyes erőforrások szeretne törölni, futtassa a következő:

```powershell
Remove-AzureRmResource -ResourceName "<name of the Resource>" -ResourceType "<Resource Type>" -ResourceGroupName "<name of the resource group>" -Force
```

A fürt erőforrás törlése, futtassa a következő:

```powershell
Remove-AzureRmResource -ResourceName "<name of the Resource>" -ResourceType "Microsoft.ServiceFabric/clusters" -ResourceGroupName "<name of the resource group>" -Force
```

## <a name="next-steps"></a>Következő lépések
Olvassa el az alábbi módon is megtudhatja, milyen fürt frissítése és szolgáltatások szétválasztás:

- [Tudnivalók a fürt frissítések](service-fabric-cluster-upgrade.md)
- [További tudnivalók a maximális méret állapot-nyilvántartó szolgáltatások szétválasztás:](service-fabric-concepts-partitioning.md)


<!--Image references-->
[ResourceGroupDelete]: ./media/service-fabric-cluster-delete/ResourceGroupDelete.PNG

[ResourceTags]: ./media/service-fabric-cluster-delete/ResourceTags.png

[TaggedResources]: ./media/service-fabric-cluster-delete/TaggedResources.PNG
