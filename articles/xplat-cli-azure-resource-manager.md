
<properties
    pageTitle="Az Azure CLI rendelkező erőforrások kezelése |} Microsoft Azure"
    description="Az Azure parancssori felület (CLI) segítségével Azure erőforrások és csoportok kezelése"
    editor=""
    manager="timlt"
    documentationCenter=""
    authors="dlepow"
    services="azure-resource-manager"/>

<tags
    ms.service="azure-resource-manager"
    ms.workload="multiple"
    ms.tgt_pltfrm="vm-multiple"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/22/2016"
    ms.author="danlep"/>

# <a name="use-the-azure-cli-to-manage-azure-resources-and-resource-groups"></a>Az Azure CLI segítségével Azure erőforrások és erőforrás-csoportok kezelése


> [AZURE.SELECTOR]
- [Portál](azure-portal/resource-group-portal.md) 
- [Azure CLI](xplat-cli-azure-resource-manager.md)
- [Azure PowerShell](powershell-azure-resource-manager.md)
- [REST API-VAL](resource-manager-rest-api.md)


Az Azure parancssori kezelőfelületről Azure egyike számos eszközt üzembe helyezéséhez és kezeléséhez, az erőforrás-kezelő erőforrások is használhatja. Ez a cikk gyakori módokon Azure erőforrások és erőforrás-csoportok kezelése a Azure CLI segítségével erőforrás-kezelő üzemmódban vezet be. Szeretne tudni a CLI az erőforrások bevezetését tervezi témakörökben [Deploy az erőforrás-kezelő sablonok és Azure CLI](resource-group-template-deploy-cli.md). Azure erőforrások és erőforrás-kezelő háttér olvassa el az [Azure erőforrás szolgáltatásának áttekintése](azure-resource-manager/resource-group-overview.md).

>[AZURE.NOTE] Az Azure CLI az Azure erőforrások kezelésére, [telepítenie](xplat-cli-install.md)kell az Azure CLI, és [Jelentkezzen be az Azure](xplat-cli-connect.md) használatával a `azure login` parancsot. Ellenőrizze, hogy az erőforrás-kezelő üzemmódban van a CLI (futtatása `azure config mode arm`). Ha már végzett az alábbi tényezőket, készen lépjen.



## <a name="get-resource-groups-and-resources"></a>Erőforrás-csoportok és a források

### <a name="resource-groups"></a>Erőforrás-csoportok

Úgy juthat az összes erőforrás csoport előfizetése, és a helyekre listáját, a művelet végrehajtása

    azure group list
    

### <a name="resources"></a>Erőforrások
 A lista összes erőforrásában található csoport, például egy név *testRG*, használja a következő parancsot.

    azure resource list testRG

Egy adott erőforrás a csoporton belül, például egy virtuális nevű *MyUbuntuVM*megtekintéséhez egy paranccsal az alábbihoz hasonló.

    azure resource show testRG MyUbuntuVM Microsoft.Compute/virtualMachines -o "2015-06-15"
    
Figyelje meg a **Microsoft.Compute/virtualMachines** paramétert. Ez a paraméter típusáról az erőforrás meg a kért információkat.
    
>[AZURE.NOTE]Az **azure erőforrás** parancsok kívül a **lista** parancs használatakor, meg kell adnia az API-verzió, az erőforrás **-o** paraméter. Ha nem tudja, hogy az API-verziójával kapcsolatban, olvassa el a sablonfájlt, és keresse meg a apiVersion mező az erőforrás. További API-verziókról az erőforrás-kezelő című témakörben talál [erőforrás-kezelő szolgáltatók régiók, API-verziók sémák és](resource-manager-supported-services.md).

Erőforrások részleteinek megtekintésekor gyakran célszerű használni a `--json` paraméter. Ezt a paramétert a kimenet olvashatóbbá teszi, mert bizonyos értékek beágyazott struktúrákat, illetve a webhelycsoportok. A következő példa bemutatja, hogyan ad az eredményt, a JSON-dokumentumként **megjelenítése** parancsot.

    azure resource show testRG MyUbuntuVM Microsoft.Compute/virtualMachines -o "2015-06-15" --json

>[AZURE.NOTE] Ha azt szeretné, mentse a JSON adatokat fájl használatával a &gt; irányítsa át a kimeneti fájl karaktert. Példa:
>
> `azure resource show testRG MyUbuntuVM Microsoft.Compute/virtualMachines -o "2015-06-15" --json > myfile.json`

### <a name="tags"></a>Címkék

[AZURE.INCLUDE [resource-manager-tag-resources-cli](../includes/resource-manager-tag-resources-cli.md)]

## <a name="manage-resources"></a>Erőforrások kezelése


Erőforráscsoport egy erőforrás, például a tárterület-fiók hozzáadásához parancsot a hasonló:

    azure resource create testRG MyStorageAccount "Microsoft.Storage/storageAccounts" "westus" -o "2015-06-15" -p "{\"accountType\": \"Standard_LRS\"}"
    
Az API-verzió, az erőforrás **-o** paraméter megadása, kívül minden szükséges vagy további tulajdonságok JSON-formátumú karakterláncként átadni **-p** paraméter használatával.
    
    
Ha törölni szeretne egy meglévő erőforrás, például egy virtuális gép erőforrást, a paranccsal az alábbihoz hasonló.

    azure resource delete testRG MyUbuntuVM Microsoft.Compute/virtualMachines -o "2015-06-15"

Meglévő erőforrások erőforráscsoport vagy-előfizetésre egy másik áthelyezéséhez az **azure erőforrás áthelyezés** parancs használata A következő példa bemutatja, hogy miként helyezheti át a vgx.dll gyorsítótár új erőforráscsoport. A **-i** paraméter nyújtanak áthelyezése meg az erőforrás-azonosító vesszővel tagolt listáját.


    azure resource move -i "/subscriptions/{guid}/resourceGroups/OldRG/providers/Microsoft.Cache/Redis/examplecache" -d "NewRG"

## <a name="control-access-to-resources"></a>Erőforrások való hozzáférés szabályozása

Az Azure CLI hozhat létre és kezelhet házirendek való hozzáférés korlátozása Azure erőforrások is használhatja. Házirend-definíciók és az erőforrások hozzárendelése házirendek hátteréről [erőforrások kezelésére és a hozzáférés-házirend használata](resource-manager-policy.md).

Például a következő szabályzat letiltja az összes kérelmet adott helyre nem nyugati USA-beli Észak központi US, és mentse a házirend-definíciós fájl policy.json:

    {
    "if" : {
        "not" : {
        "field" : "location",
        "in" : ["westus" ,  "northcentralus"]
        }
    },
    "then" : {
        "effect" : "deny"
    }
    }

Futtassa a **házirend-definícióhoz létrehozása** parancsot:

    azure policy definition create MyPolicy -p c:\temp\policy.json
    
Ez a parancs megjeleníti az alábbihoz hasonló eredményt ad.

    + Házirend-definícióhoz MyPolicy adatok létrehozása: házirendnév: MyPolicy adatok: PolicyDefinitionId: /subscriptions/###-###-###-###-###/providers/Microsoft.Authorization/policyDefinitions/MyPolicy

    adatok: PolicyType: egyéni adatok: DisplayName: adatok meghatározatlan: leírása: adatok meghatározatlan: PolicyRule: mező = helyét, a [westus, northcentralus], effektus = elutasítása

 A egy keresési tartományt, amelyet a házirend hozzárendelése, **PolicyDefinitionId** által eredményül adott az előző parancs használata A következő példában a hatókör az előfizetést, de is korlátozhatja az erőforrás csoportok, illetve egyes erőforrásokhoz:

    Azure házirendek létrehozása MyPolicyAssignment -p /subscriptions/###-###-###-###-###/providers/Microsoft.Authorization/policyDefinitions/MyPolicy -s /subscriptions/###-###-###-###-### /

Első, módosítása és eltávolítása a házirend-definíciók a **házirend-definícióhoz megjelenítése**, a **házirend-definícióhoz beállítása**és a **házirend-definícióhoz törlése** parancs használatával.

Hasonlóképpen kérjen, módosítása, vagy házirend-hozzárendelések eltávolítása a **házirend hozzárendelése megjelenítése**, **házirendek beállítása**és a **házirend hozzárendelése törlése** parancs használatával.


## <a name="export-a-resource-group-as-a-template"></a>Erőforráscsoport exportálása sablonként

Egy meglévő erőforrás-csoport megtekintheti az erőforráscsoport az erőforrás-kezelő sablonját. A sablon exportálása kínál a két előnyökkel jár:

1. Egyszerűen automatizálhatja a jövőbeli telepítéseknél a megoldást, mivel minden infrastruktúra határozza meg a sablont.

2. Akkor is megismerje sablon szintaxis megjeleníti a JSON, amely a megoldás.

Az Azure CLI használ, egy sablon, amely az aktuális állapotát az erőforráscsoport exportálása, vagy töltse le a sablont, amelyet egy adott telepítéshez használtak.

* **A sablon erőforráscsoport exportálása** – Ez akkor hasznos, ha végzett módosítások erőforráscsoport, és annak aktuális állapotában JSON ábrázolása beolvasásához szükség. A létrehozott sablonra azonban csak minimális számú paramétereket és nincsenek változók tartalmaz. A sablonban szereplő értékek táblázatparancsok nagy része csomagolásukkor. A létrehozott sablont mielőtt kívánt előfordulhat, hogy paramétereket konvertálása több értékét, testre szabhatja a különböző környezetekben a telepítést.

    A sablon erőforráscsoport exportálása egy helyi könyvtár, futtassa a `azure group export` parancs az alábbi példában látható módon. (Cserélje le a megfelelő operációs rendszer környezetben helyi könyvtár.)

        azure group export testRG ~/azure/templates/

* **Töltse le a sablont, egy adott telepítéshez** – Ez akkor hasznos, ha a sablon megjelenítése a tényleges erőforrások üzembe használt kell. A sablon összes paramétereket és az eredeti telepítéshez meghatározott változók tartalmaz. Jó helyen jár Ha a szervezet módosította az erőforráscsoport kívül a sablonban meghatározása, ezzel a sablonnal nem jelenítik meg erőforráscsoport aktuális állapotát.

    Töltse le a sablont, egy helyi könyvtár egy adott telepítéshez használni, futtassa a `azure group deployment template download` parancsot. Példa:

        azure group deployment template download TestRG testRGDeploy ~/azure/templates/downloads/
 
>[AZURE.NOTE] Sablon exportálása előzetes verzióban, és nem az összes erőforrástípus ismer exportálása egy sablont. Ha megpróbál exportálása egy sablont,, amely arról tájékoztat, hogy egyes erőforrások nem exportált hiba jelenhet meg. Ha szükséges, a kézi megadása után letölti a sablon ezek az erőforrások.



## <a name="next-steps"></a>Következő lépések

* Részleteket az üzembe helyezési műveleteket, és az Azure CLI telepítési hibáinak elhárítása, lásd: [az Azure CLI telepítési műveletek megtekintése](resource-manager-troubleshoot-deployments-cli.md).
* A CLI használatával állítsa be az adott alkalmazás vagy parancsfájl forrásokat, című témakörben olvashat [Azure-CLI használata egy egyszerű erőforrások eléréséhez szolgáltatás hozzon létre](resource-group-authenticate-service-principal-cli.md).


