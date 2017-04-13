<properties 
    pageTitle="Erőforrások áthelyezése új erőforráscsoport |} Microsoft Azure" 
    description="Erőforrás-kezelő Azure segítségével erőforrások áthelyezése új erőforráscsoport vagy-előfizetésre." 
    services="azure-resource-manager" 
    documentationCenter="" 
    authors="tfitzmac" 
    manager="timlt" 
    editor="tysonn"/>

<tags 
    ms.service="azure-resource-manager" 
    ms.workload="multiple" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/21/2016" 
    ms.author="tomfitz"/>

# <a name="move-resources-to-new-resource-group-or-subscription"></a>Új erőforráscsoport vagy-előfizetésre erőforrások áthelyezése

Ez a témakör bemutatja, hogyan erőforrások áthelyezése vagy új előfizetést, vagy új erőforráscsoport ugyanabban az előfizetésben. A portál, PowerShell, Azure CLI vagy a REST API segítségével erőforrás áthelyezése. Ez a témakör az áthelyezés műveletek találhatók bármely segítség nélkül a Azure támogatási szolgálattól.

Általában erőforrások áthelyezése, amikor úgy dönt, hogy:

- Számlázási céljából az erőforrás kell egy másik előfizetést élő.
- Erőforrás már nem osztja meg a azonos életciklusáról a erőforrásként ez volt korábban csoportosítva. Új erőforráscsoport helyezze át úgy, hogy az erőforrás kezelheti külön-külön más erőforrások: a kívánt.

Erőforrások mozgatásakor a cél csoport és a forrás csoportja a művelet során zárolva van. Írja be, és törölje a műveletek az áthelyezés befejeződéséig a csoportok a blokkolt.

Hol található az erőforrás nem módosítható. Erőforrás áthelyezése csak áthelyezi egy új erőforráscsoport. Előfordulhat, hogy az új erőforráscsoport egy másik helyre, de nem változtatja meg, hogy az erőforrás helyét.

> [AZURE.NOTE] Ez a cikk ismerteti, hogyan áthelyezése erőforrások egy meglévő Azure belüli partner ajánlja fel a szolgáltatást. Ha el szeretné módosítani az Azure-fiók (például a frissítés előtti fizetés kirovó), miközben továbbra is dolgozik, amikor a meglévő kínáló, a [Váltás másik ajánlat Azure előfizetése](billing-how-to-switch-azure-offer.md)témakörben talál. 

## <a name="checklist-before-moving-resources"></a>Mielőtt erőforrások ellenőrzőlista

Létezik néhány fontos lépést meg, hogy egy erőforrás áthelyezése előtt végezze el. Ezek a feltételek ellenőrzésével hibák elkerülése érdekében.

1. A szolgáltatás engedélyeznie kell az azt jelenti, hogy erőforrások áthelyezése. Lásd: mely [szolgáltatások engedélyezése mozgó erőforrások](#services-that-enable-move)információt az alábbi listából.
1. A forrás- és céltáblák előfizetések léteznie kell, [az Active Directory-bérlői](./active-directory/active-directory-howto-tenant.md)ugyanahhoz a belül. Új bérlő áthelyezéséhez az ügyfélszolgálat.
2. Az erőforrás-szolgáltató az erőforrás helyezi át, a cél előfizetés regisztrálnia kell. Ha nem, hibaüzenet arról, hogy az **előfizetés nincs regisztrált egy erőforrás típusa**. Ez a probléma során felmerülő erőforrás áthelyezése egy új előfizetést, de, hogy az előfizetés soha nem használta az adott erőforrás típusa. Megtudhatja, hogy miként a regisztrációs állapotának ellenőrzése és erőforrás-szolgáltatók regisztrálni, akkor olvassa el az [erőforrás szolgáltatók és -típusok](../resource-manager-supported-services.md#resource-providers-and-types).
4. Ha az áthelyezni kívánt alkalmazás szolgáltatási alkalmazást, [alkalmazás szolgáltatáselérhetőség](#app-service-limitations)ellenőrzését.
4. Ha helyreállítási szolgáltatásokhoz tartozó erőforrások áthelyezni, ellenőrzését [helyreállítási szolgáltatások korlátai](#recovery-services-limitations)
5. Ha forrásokkal Klasszikus modell között áthelyezni, [Klasszikus telepítési korlátozások](#classic-deployment-limitations)ellenőrzését.

## <a name="when-to-call-support"></a>Ha a támogatási szolgálat hívása

Ez a témakör látható önkiszolgáló műveletek révén a legtöbb erőforrás léphet. Használja az önkiszolgáló műveletek:

- Erőforrás-kezelő erőforrások áthelyezése.
- Helyezze át a klasszikus erőforrások [Klasszikus telepítési korlátozások](#classic-deployment-limitations)megfelelően. 

Ha kell, hívja a támogatási szolgálatot:

- Az erőforrások áthelyezése egy új Azure-fiók (és az Active Directory-bérlői webhelyen).
- Klasszikus erőforrások áthelyezése, de gondjai vannak a korlátozásokat.

## <a name="services-that-enable-move"></a>Szolgáltatások, amelyek lehetővé teszik az áthelyezés

Most a szolgáltatásokat, amelyek lehetővé teszik a áthelyezése egy új erőforráscsoport és az előfizetés a következők:

- API-kezelés
- Alkalmazás szolgáltatás alkalmazások (web Apps alkalmazások) – lásd: [alkalmazás szolgáltatáselérhetőség](#app-service-limitations)
- Automatizálási
- Köteg
- CDN
- Cloud Services – lásd: a [Klasszikus telepítési korlátai](#classic-deployment-limitations)
- Adatok gyári
- DNS
- DocumentDB
- HDInsight fürt
- IoT hubok
- Fő tárolóból elemre 
- Media Services
- Mobil tetszés szerint elmélyedhet
- Értesítés hubok
- Műveleti Hírcsatornájában
- Gyorsítótár vgx.dll
- A Feladatütemező
- Keresés
- Szolgáltatás Bus
- Tárhely
- Tárolás (klasszikus) – lásd: a [Klasszikus telepítési korlátai](#classic-deployment-limitations)
- SQL-adatbázis kiszolgálója – az adatbázis és kiszolgáló erőforrás azonos csoportban lévő kell lennie. Az SQL server áthelyezésekor az összes az adatbázisok is áthelyeződnek.
- Virtuális gépeken futó - azonban hatással van nem támogatja a új előfizetést lépés, ha a tanúsítványok tárolja a kulcs tárolóból elemre.
- Virtuális gépeken futó (klasszikus) – lásd: a [Klasszikus telepítési korlátai](#classic-deployment-limitations)
- Virtuális hálózatok

## <a name="services-that-do-not-enable-move"></a>Ne engedélyezze az áthelyezés szolgáltatások

A szolgáltatások, amely jelenleg nem engedélyezi a áthelyezése egy erőforrás a következők:

- Alkalmazás átjáró
- Alkalmazás Hírcsatornájában
- Express-továbbítására
- Helyreállítási szolgáltatások tárolóra - is nem áthelyezése a számítási, hálózati és tárolását rendelt erőforrások a helyreállítási szolgáltatások tárolóra című [helyreállítási szolgáltatások korlátozások vonatkoznak](#recovery-services-limitations).
- Virtuális gépeken futó kulcs tárolóra tárolt tanúsítvány
- Virtuális gépeken futó skála beállítása
- Virtuális hálózatok (klasszikus) – lásd: a [Klasszikus telepítési korlátai](#classic-deployment-limitations)
- Virtuális Magánhálózati átjáró

## <a name="app-service-limitations"></a>Alkalmazás szolgáltatáselérhetőség

-Szolgáltatási alkalmazás alkalmazások használatakor csak egy alkalmazás szolgáltatáscsomagja nem aktiválhatók. Alkalmazás szolgáltatás alkalmazások áthelyezéséhez a következők lehetnek:

- Ugrás az alkalmazás szolgáltatáscsomagja és az összes alkalmazás szolgáltatás erőforrás az erőforráscsoport, amely még nem találhatók alkalmazás szolgáltatás erőforrások új erőforráscsoport. Ez a követelmény azt jelenti, hogy még az alkalmazás szolgáltatás információforrások, amelyek nem társulnak alkalmazás szolgáltatáscsomagja kell áthelyezni. 
- Az alkalmazások áthelyezése egy másik erőforráscsoport, de ne az összes alkalmazás milyen szolgáltatáscsomagok az eredeti erőforráscsoport.

Az eredeti erőforráscsoport-alkalmazás háttérismeretek erőforrás is, ha nem tudja áthelyezni anyagerőforráshoz, mivel alkalmazás háttérismeretek jelenleg nem engedélyezi az áthelyezés. Ha az alkalmazás az összefüggéseket erőforrás alkalmazás szolgáltatás alkalmazások áthelyezésekor, a teljes áthelyezés művelet sikertelen lesz. Jó helyen jár az alkalmazás az összefüggéseket és az alkalmazás szolgáltatáscsomagja nem kell az azonos erőforráscsoport, az alkalmazás az alkalmazás megfelelő működéséhez tárolnak.

Ha például az erőforráscsoport tartalmazza:

- **webes a** amely társított **terv egy** és **alkalmazás-háttérismeretek-a**
- **terv-b** és az **alkalmazás-háttérismeretek-b** társított **web-b**

A beállítások a következők:

- Áthelyezése **a webes**, **terv a**, **webhely-b**és a **terv-b**
- Ugrás **a webes** és **web-b**
- Ugrás **egy webhelyen**
- **Webhely-b** áthelyezése

Minden más kombináció magában foglalja a áthelyezése egy erőforrás típusa, amely nem tudja áthelyezni (alkalmazás háttérismeretek), vagy hagyja, hogy nem kell hagyni mögött alkalmazás szolgáltatáscsomagja (bármely alkalmazás szolgáltatás erőforrás típusa) áthelyezésekor erőforrástípust mögött.

Ha egy másik erőforráscsoport az alkalmazás szolgáltatás csomagnál található a web App alkalmazásban, de mindkettőt új erőforráscsoport szeretne áthelyezni kívánt, az áthelyezés a két lépést kell végrehajtania. Példa:

- **webes a** **webes** csoportjában található
- **terv a** **terv** csoportjában található
- Kívánt **webes egy** és **csomagot a** megfelelő pontján **kombinált** csoport

Ez a lépés elvégzéséhez műveleteket két külön áthelyezése a következő sorrendben:

1. Ugrás a **webes a** **terv -** csoportba
2. Áthelyezése **a webes** és **terv egy** **kombinált**csoportba.

Áthelyezheti az alkalmazás szolgáltatás tanúsítványt új erőforráscsoport vagy-előfizetésre nélkül kapcsolatos problémák megoldásához. Azonban a webalkalmazás az SSL-tanúsítvány külső felekkel megvásárolt és töltenek fel az alkalmazást tartalmaz, törölnie kell a tanúsítvány mielőtt a web App alkalmazásban. Ha például végezheti el az alábbi lépéseket:

1. A feltöltött tanúsítvány törlése a web App alkalmazásban
2. A web app áthelyezése
3. Töltse fel a tanúsítvány a web App alkalmazásban

## <a name="recovery-services-limitations"></a>Helyreállítási szolgáltatások korlátai

Áthelyezése a tárhely, a hálózaton, nincs engedélyezve, és állíthatja be az Azure-webhely helyreállításhoz vészhelyreállítás használt erőforrások számítja ki. 

Tegyük fel például, a helyszíni gépek tárterület-fiókhoz (Storage1) replikációs be van állítva, és a lyncen feladatátvétel után az Azure virtuális gép (VM1) csatolni virtuális hálózatokat (Network1) szerint védett gépen szeretné. Ezek az Azure erőforrások - Storage1 VM1 és Network1 - nem tudja áthelyezni, az azonos előfizetésben erőforráscsoport vízszintesen vagy a előfizetésekben.

## <a name="classic-deployment-limitations"></a>Klasszikus telepítési korlátai

A beállítások között a Klasszikus modell forrásokkal áthelyezése e áthelyezi az erőforrások előfizetés belül, illetve új előfizetést függően eltérőek lehetnek. 

### <a name="same-subscription"></a>Azonos előfizetés

Erőforrások egy erőforrás csoportból áthelyezése egy másik erőforráscsoport ugyanabban az előfizetésben belül, amikor a következő megkötések vonatkoznak:

- Nem lehet áthelyezni a virtuális hálózatok (klasszikus).
- Virtuális gépeken futó (klasszikus) kell helyezhető át a felhőalapú szolgáltatással. 
- Felhőalapú szolgáltatás csak ha az áthelyezés a virtuális gépeken futó tartalmazó helyeződnek át.
- Egyszerre csak egy felhőalapú szolgáltatásba áthelyezhető.
- Egyszerre csak egy tároló fiók (klasszikus) áthelyezhető.
- Tárterület-fiókot (klasszikus) nem lehet áthelyezni egy műveletben egy virtuális gép vagy egy felhőalapú szolgáltatásba.

Klasszikus erőforrások áthelyezése egy új erőforráscsoport belül ugyanabban az előfizetésben, használja a szabványos áthelyezés műveletek keresztül a [portálon](#use-portal), [Azure PowerShell](#use-powershell), [Azure CLI](#use-azure-cli)vagy [REST API -t](#use-rest-api). Az erőforrás-kezelő erőforrások áthelyezése használata ugyanazokat a műveleteket használhat.

### <a name="new-subscription"></a>Új előfizetéshez

Amikor új előfizetést erőforrások helyez át, a következő megkötések vonatkoznak:

- Az előfizetés összes klasszikus erőforrás azonos művelet kell áthelyezni.
- A target előfizetés klasszikus anyagainak nem tartalmazhat.
- Az áthelyezés csak egy külön REST API-t a klasszikus Ugrás keresztül kell kérni. A szokásos erőforrás-kezelő Áthelyezés parancs nem működnek a klasszikus erőforrások áthelyezése új előfizetést.

Új előfizetéshez klasszikus erőforrások áthelyezéséhez a többi műveletek klasszikus erőforrások jellemző kell használnia. A következő lépésekkel klasszikus erőforrások áthelyezése új előfizetést.

1. Jelölje be, ha az adatforrás-előfizetés részt vehet egy keresztté-előfizetés áthelyezése. Használja az alábbi műveletet:

         POST https://management.azure.com/subscriptions/{sourceSubscriptionId}/providers/Microsoft.ClassicCompute/validateSubscriptionMoveAvailability?api-version=2016-04-01
    
     Az összehívás törzsébe a következők:

         {
           "role": "source"
         }

     A válasz, a érvényességi művelet a következő formátumban van:

         {
           "status": "{status}",
           "reasons": [
             "reason1",
             "reason2"
           ]
         }

2. Ellenőrizze, hogy ha a célként megadott előfizetés részt vehet egy keresztté-előfizetés áthelyezése. Használja az alábbi műveletet:

         POST https://management.azure.com/subscriptions/{destinationSubscriptionId}/providers/Microsoft.ClassicCompute/validateSubscriptionMoveAvailability?api-version=2016-04-01

     Az összehívás törzsébe a következők:

         {
           "role": "target"
         }

     A válasz a forrás előfizetés érvényességi megegyező formátumban van.

3. Ha mindkét előfizetések érvényesíteni, összes klasszikus erőforrás egy előfizetésről áttérni egy másik előfizetést, az alábbi műveletet:

         POST https://management.azure.com/subscriptions/{subscription-id}/providers/Microsoft.ClassicCompute/moveSubscriptionResources?api-version=2016-04-01

    Az összehívás törzsébe a következők:

         {
           "target": "/subscriptions/{target-subscription-id}"
         }

Néhány percig, amíg a művelet futtathatja. 

## <a name="use-portal"></a>Portál használata

Új erőforráscsoport **azonos előfizetés**erőforrások áthelyezéséhez jelölje ki a tartalmazó ezek az erőforrások erőforráscsoport, és kattintson az **Ugrás** gombra.

![erőforrások áthelyezése](./media/resource-group-move-resources/edit-rg-icon.png)

Vagy **új előfizetést**erőforrások áthelyezéséhez jelölje ki a tartalmazó ezek az erőforrások erőforráscsoport, és válassza a Szerkesztés előfizetés ikonra.

![erőforrások áthelyezése](./media/resource-group-move-resources/change-subscription.png)

Jelölje ki az erőforrások áthelyezése és a cél erőforráscsoport. Igazolja, hogy szeretne-e ezek az erőforrások parancsfájlok frissítése, és kattintson **az OK gombra**. A Szerkesztés előfizetés ikonra az előző lépésben kiválasztott, ha a cél előfizetés is választania kell.

![Cél kijelölése](./media/resource-group-move-resources/select-destination.png)

**Értesítések**láthatja, hogy fut-e az áthelyezés.

![áthelyez állapotadatok megjelenítése](./media/resource-group-move-resources/show-status.png)

Amikor befejezte, az eredmény értesítést kap.

![Áthelyezés eredmény megjelenítése](./media/resource-group-move-resources/show-result.png)

## <a name="use-powershell"></a>A PowerShell használata

Szeretné áthelyezni meglévő erőforrások erőforráscsoport vagy -előfizetésre egy másik, **Áthelyezés-AzureRmResource** paranccsal.

Az első példa egy erőforrást áthelyezése egy új erőforráscsoport mutatja.

    $resource = Get-AzureRmResource -ResourceName ExampleApp -ResourceGroupName OldRG
    Move-AzureRmResource -DestinationResourceGroupName NewRG -ResourceId $resource.ResourceId

A második példában több erőforrások áthelyezése egy új erőforráscsoport mutatja.

    $webapp = Get-AzureRmResource -ResourceGroupName OldRG -ResourceName ExampleSite
    $plan = Get-AzureRmResource -ResourceGroupName OldRG -ResourceName ExamplePlan
    Move-AzureRmResource -DestinationResourceGroupName NewRG -ResourceId $webapp.ResourceId, $plan.ResourceId

Új előfizetéshez áthelyezéséhez a **DestinationSubscriptionId** paraméter egy értéket tartalmazza.

Győződjön meg arról, hogy a megadott erőforrások áthelyezni kívánt kérni.

    Confirm
    Are you sure you want to move these resources to the resource group
    '/subscriptions/{guid}/resourceGroups/newRG' the resources:

    /subscriptions/{guid}/resourceGroups/destinationgroup/providers/Microsoft.Web/serverFarms/exampleplan
    /subscriptions/{guid}/resourceGroups/destinationgroup/providers/Microsoft.Web/sites/examplesite
    [Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"): y

## <a name="use-azure-cli"></a>Azure CLI használata

Meglévő erőforrások erőforráscsoport vagy-előfizetésre egy másik áthelyezéséhez az **azure erőforrás áthelyezés** parancs használata Meg kell adnia az erőforrások áthelyezése erőforrás-azonosítók. Erőforrás-azonosítók a következő parancsot a szerezheti be:

    azure resource list -g sourceGroup --json

Amely adja eredményül a következő formátumban:

    [
      {
        "id": "/subscriptions/{guid}/resourceGroups/sourceGroup/providers/Microsoft.Storage/storageAccounts/storagedemo",
        "name": "storagedemo",
        "type": "Microsoft.Storage/storageAccounts",
        "location": "southcentralus",
        "tags": {},
        "kind": "Storage",
        "sku": {
          "name": "Standard_RAGRS",
          "tier": "Standard"
        }
      }
    ]

A következő példa bemutatja, hogy miként helyezheti át a tárterület-fiók új erőforráscsoport. A **-i** paraméter nyújtanak áthelyezése meg az erőforrás-azonosító vesszővel tagolt listáját.

    azure resource move -i "/subscriptions/{guid}/resourceGroups/sourceGroup/providers/Microsoft.Storage/storageAccounts/storagedemo" -d "destinationGroup"
    
Győződjön meg arról, hogy az adott erőforrás áthelyezni kívánt kérni.

## <a name="use-rest-api"></a>REST API-t használja.

Meglévő erőforrások erőforráscsoport vagy-előfizetésre egy másik áthelyezéséhez futtassa:

    POST https://management.azure.com/subscriptions/{source-subscription-id}/resourcegroups/{source-resource-group-name}/moveResources?api-version={api-version} 

Az összehívás törzsébe adja meg a cél erőforráscsoport, valamint az erőforrásokat. Az áthelyezés többi kapcsolatos további tudnivalókért olvassa el az [erőforrások áthelyezése](https://msdn.microsoft.com/library/azure/mt218710.aspx)című témakört.


## <a name="next-steps"></a>Következő lépések
- Az előfizetés kezeléséhez PowerShell-parancsmagok kapcsolatos további tudnivalókért lásd: [Az erőforrás-kezelő Azure-PowerShell használatá](powershell-azure-resource-manager.md).
- Az előfizetés kezeléséhez Azure CLI parancsokról című témakörben talál [az Azure CLI az erőforrás-kezelő használatával](xplat-cli-azure-resource-manager.md).
- Az előfizetés kezeléséhez portál szolgáltatásairól című témakörben talál [az erőforrások kezelésére Azure portálon](./azure-portal/resource-group-portal.md).
- Logikus alkalmazása az erőforrások kapcsolatos további tudnivalókért olvassa el a [címkék használata a rendszerezheti az erőforrások](resource-group-using-tags.md)című témakört.
