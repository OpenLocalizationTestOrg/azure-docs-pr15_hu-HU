<properties
    pageTitle="Erőforrás-kezelő Azure-sablon exportálása |} Microsoft Azure"
    description="Azure erőforrás kezelése segítségével sablon exportálása egy meglévő erőforrás-csoportot."
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
    ms.topic="get-started-article"
    ms.date="10/20/2016"
    ms.author="tomfitz"/>

# <a name="export-an-azure-resource-manager-template-from-existing-resources"></a>Az erőforrás-kezelő Azure-sablon exportálása a meglévő erőforrások

Erőforrás-kezelő lehetővé teszi, hogy az erőforrás-kezelő sablon exportálása az előfizetésben, meglévő forrásokból. Létrehozott sablonon is használhatja, ha többet szeretne tudni a sablon szintaxis vagy automatizálhatja a újratelepítés a megoldás, szükség szerint.

Fontos, hogy látható, hogy sablon exportálása két különböző módon:

- A tényleges sablont, akkor a telepítéshez használt exportálhatja. A exportált sablonban szerepel a paramétereket és a változók, pontosan úgy, ahogyan az eredeti sablonban néztek. Ezt a módszert akkor hasznos, ha erőforrásokat a portálon keresztül telepítette. Most érdemes megtudhatja, hogy miként ezek az erőforrások létrehozása a sablon létrehozása.
- Egy sablon, amely az aktuális állapotát az erőforráscsoport exportálhatja. Az exportált sablon nem egy tetszőleges sablont, akkor a telepítéshez használt alapul. Ehelyett létrehoz egy sablont, amely az erőforráscsoport pillanatfelvételek. Az exportált sablonjának sok csomagolásukkor értéket, és valószínűleg nem annyi paramétert, mint a szokásos módon definiálása. Ez a módszer akkor hasznos, ha a módosította az erőforráscsoport parancsfájlok vagy a portálon keresztül. Most kell rögzítése az erőforráscsoport sablonként.

Ez a témakör bemutatja mindkét megközelítésnek.

Ebben az oktatóanyagban jelentkezzen be az Azure-portálra, tárterület-fiók létrehozása, és a fiókhoz tartozó tárterület sablon exportálása. Hozzáadhat egy virtuális hálózati erőforráscsoport módosítása. Végül egy új sablont, amely az aktuális állapotáról exportálhatja. Bár ez a cikk olyan egyszerűsített infrastruktúra koncentrál, a fenti lépéseket kihasználva bonyolultabb megoldás sablon exportálása.

## <a name="create-a-storage-account"></a>Tárterület-fiók létrehozása

1. Az [Azure portálon](https://portal.azure.com)válassza az **Új** > **tároló** > **tárterület-fiókot**.

      ![tárhely létrehozása](./media/resource-manager-export-template/create-storage.png)

2. Tárterület-fiók létrehozása **tároló**nevét, a Monogram és a dátumot. A tároló fióknév Azure mindenütt egyedinek kell lennie. A név már használatban van, ha megjelenik egy hiba jelenik meg, miszerint a név használatban van. Próbálja meg a módosítást. Az erőforráscsoport hozzon létre egy új erőforrás csoportot, és **ExportGroup**nevet. A Tulajdonságok parancsot használhatja az alapértelmezett értékeket is. Jelölje ki a **létrehozása**.

      ![a szükséges értékeket tároló](./media/resource-manager-export-template/provide-storage-values.png)

A telepítési eltarthat egy kis ideig. A telepítés befejezése után-előfizetése tartalmazza a tárhely fiókot.

## <a name="view-a-template-from-deployment-history"></a>Egy sablont, a telepítési előzmények megtekintése

1. Nyissa meg az erőforrás csoport lap az új erőforrás csoport. Figyelje meg, hogy a lap az utolsó telepítési eredményének megjelenítése. Jelölje be ezt a hivatkozást.

      ![erőforrás-csoport lap](./media/resource-manager-export-template/resource-group-blade.png)

2. A csoport-alapú telepítés előzményeinek megtekintése A lehetőséget választja a lap valószínűleg csak egy telepítési sorolja fel. Jelölje ki a telepítéssel.

     ![utolsó telepítési](./media/resource-manager-export-template/last-deployment.png)

3. A lap jeleníti meg a telepítési összefoglalását. Az összefoglaló állapotát a telepítési és a műveletek és paraméterekkel megadott értékeket tartalmazza. Ha látni szeretné a sablont, akkor a telepítéshez használt, jelölje ki a **Nézet sablont**.

     ![telepítési összegzésének megtekintése](./media/resource-manager-export-template/deployment-summary.png)

4. Erőforrás-kezelő meg olvassa be az alábbi hat fájlokat:

   1. **Sablon** – a sablont, amely definiálja az infrastruktúra a megoldás. A tárterület-fiókot a portálon keresztül létrehozásakor az erőforrás-kezelő azokat a sablon használatával, és mentése későbbi használatra sablonon.
   2. **Paraméterek** - paraméter fájl átadni a telepítés során, az értékek használható. Az első a telepítés során megadott értékeket tartalmazza, de módosíthatja bármely ezeket az értékeket, amikor meg újratelepítése a sablont.
   3. **CLI** - az Azure parancssor line felület (CLI) parancsprogram bevezetését tervezi a sablon használható.
   4. A **PowerShell** - az Azure PowerShell parancsfájl, amelyet a sablon üzembe is használhatja.
   5. **.NET** - telepítéshez használni a sablon használható A .NET osztály.
   6. **Fonetikus** – A fonetikus osztályjegyzetfüzet telepítéséhez a sablon használható.

     A fájlok keresztül elérhetők a kapcsolaton keresztül a lap. A lap alapértelmezés szerint jeleníti meg a sablont.

       ![sablonok nézet](./media/resource-manager-export-template/view-template.png)

     Vegyük figyelmet bizonyos a sablonba. A sablon hasonlóan kell kinéznie:

        {"$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#", "contentVersion": "1.0.0.0", "paraméterek": {"név": {"típus": "Karakterlánc"}, "accountType": {"típus": "Karakterlánc"}, "hely": {"típus": "Karakterlánc"}, "encryptionEnabled": {"defaultValue": értéke igaz, "típus": "Logikai"}}, "erőforrások": [{"típus": "Microsoft.Storage/storageAccounts", "termékváltozat": {"név": "[parameters('accountType')]"} "jellegű",: "Tároló", "név": "[parameters('name')]", "apiVersion": "2016-01-01", "hely": "[parameters('location')]", "Tulajdonságok": {"titkosítási": {"szolgáltatások": {"blob": {"engedélyezett": "[parameters('encryptionEnabled')]"}}, "keySource": "Microsoft.Storage"}}}]}
 
Ezzel a sablonnal az a tárterület-fiók létrehozásához használt tényleges sablon. Figyelje meg, amelyek segítségével üzembe tároló fiókok különböző típusú paramétert tartalmaz. Sablon felépítésének kapcsolatos további információért lásd: [Azure erőforrás-kezelő létrehozáshoz használható sablonok](resource-group-authoring-templates.md). Lásd: [Azure erőforrás-kezelő sablon függvények](resource-group-template-functions.md)a függvény a sablon is használhatja a teljes listáját.


## <a name="add-a-virtual-network"></a>A virtuális hálózat hozzáadása

A sablont, amelyet az előző szakaszban letöltötte az adott eredeti telepítéshez infrastruktúra jelöli. Azonban hogy nem figyelembe veszi a telepítés után bármilyen módosítást.
Bemutatják a probléma, nézzük módosítsa az erőforráscsoport egy virtuális hálózaton keresztül a portálon hozzáadásával.

1. Az erőforrás a csoport lap válassza a **Hozzáadás**elemre.

      ![erőforrás hozzáadása](./media/resource-manager-export-template/add-resource.png)

2. Jelölje ki **a virtuális hálózati** a rendelkezésre álló erőforrásokat.

      ![Jelölje ki a virtuális hálózati](./media/resource-manager-export-template/select-vnet.png)

2. A virtuális hálózat **VNET**nevet, és az alapértelmezett értékek használata az egyéb tulajdonságait. Jelölje ki a **létrehozása**.

      ![Értesítés beállítása](./media/resource-manager-export-template/create-vnet.png)

3. Után a virtuális hálózat sikeresen telepítve van az erőforráscsoport, tekintse meg ismét a telepítés előzményeket. Ekkor megjelenik a két telepítések. Ha nem látható a második telepítési, előfordulhat, zárja be az erőforrás csoport lap, és nyissa meg újra. Jelölje ki a legutóbbi több példányban.

      ![telepítési előzmények](./media/resource-manager-export-template/deployment-history.png)

4. A sablon adott telepítéshez megtekintése. Figyelje meg, hogy megadja-e a virtuális hálózat. Nem tartalmazza a tárterület-fiók korábbi telepítette. Ha már nem egy sablont, amely az erőforráscsoport az összes erőforrás.

## <a name="export-the-template-from-resource-group"></a>A sablon exportálása erőforráscsoport

Úgy juthat az erőforráscsoport aktuális állapota, exportálása egy sablont, amely mutatja az erőforráscsoport pillanatképét.  

> [AZURE.NOTE] Nem lehet exportálni, amelynek legfeljebb 200 erőforrások erőforráscsoport sablon.

1. A sablon egy erőforrás csoport megtekintéséhez jelölje be az **automatizálási parancsfájl**.

      ![Erőforráscsoport exportálása](./media/resource-manager-export-template/export-resource-group.png)

     Nem minden erőforrástípus támogatja az Exportálás sablon függvény. Az erőforráscsoport csak tartalmaz, a tárhely és a jelen cikkben felsorolt virtuális hálózati, ha nem látható hiba történt. Ha más erőforrástípus hozott létre, megjelenhet hibaüzenet arról, hogy az Exportálás probléma van. Megismerheti azokat a [problémák kijavítása exportálás](#fix-export-issues) szakasz problémák kezelése.

      

2. A hat fájlokat helyezéséhez használható újra megjelenik, de ez esetben a sablon kissé eltérő. Ezzel a sablonnal csak két paraméterrel rendelkezik: egyet a tárterület-fiók nevét, és egy virtuális hálózat nevét.

        "parameters": {
          "virtualNetworks_VNET_name": {
            "defaultValue": "VNET",
            "type": "String"
          },
          "storageAccounts_storagetf05092016_name": {
            "defaultValue": "storagetf05092016",
            "type": "String"
          }
        },

     Erőforrás-kezelő nem fejeződött beolvasni a telepítés során használt sablonokat. Ehelyett, hozza létre a források aktuális beállításaitól alapuló új sablon. Például a sablon állítja a tárolási helye és replikációs számlaérték:

        "location": "northeurope",
        "tags": {},
        "properties": {
            "accountType": "Standard_RAGRS"
        },

3. Ha továbbra is dolgozik, ezzel a sablonnal lehetőségeit, néhány. Töltse le a sablont, és rajta a munkát helyileg JSON-szerkesztő. Vagy a tár a sablon mentéséhez és a rajta a munkát a portálon keresztül.

     Ha például [VIEWBEN kódot](resource-manager-vs-code.md) vagy [Visual Studio](vs-azure-tools-resource-groups-deployment-projects-create-deploy.md)JSON szerkesztőprogrammal kényelmes, előfordulhat, hogy inkább a sablon helyileg letöltéséről és használatáról a, hogy szerkesztő. Ha nincs beállítva vannak JSON-szerkesztő, előfordulhat, hogy inkább szerkeszti a sablont a portálon keresztül. Ez a témakör a többi feltételezi, hogy a sablon a tár a portálon mentett. Azonban a azonos szintaxis módosítások elvégzése a sablonba e helyileg dolgozik, a JSON-szerkesztő, illetve a portálon keresztül.

     Helyi meghajtóra dolgozik, jelölje be a **Letöltés választógombot**.

      ![sablon letöltése](./media/resource-manager-export-template/download-template.png)

     Haladjon végig a portálon, jelölje be a **tárba a Hozzáadás gombra**.

      ![tár hozzáadása](./media/resource-manager-export-template/add-to-library.png)

     Ha a sablon elhelyezése a tárakban, írja be a sablon nevét és leírását. Válassza a **Mentés**.

     ![sablon értékeivel](./media/resource-manager-export-template/set-template-values.png)

4. Nyissa meg a tár mentése sablonként, azt **További szolgáltatások**, írja be a **sablonok** szűrést, és jelölje ki a **sablonok**.

      ![sablonok keresése](./media/resource-manager-export-template/find-templates.png)

5. Jelölje be a sablon mentett nevű.

      ![sablon kiválasztása](./media/resource-manager-export-template/select-library-template.png)

## <a name="customize-the-template"></a>A sablon testreszabása

Az exportált sablon jól működik, ha szeretne létrehozni, a tárhely ugyanazzal a fiókkal és a virtuális hálózat minden telepítéshez. Erőforrás-kezelő azonban lehetőséget nyújt annak, hogy a sokkal nagyobb rugalmasság érdekében telepítheti a sablonok. Ha például a telepítés során érdemes lehet adja meg a tárterület-fiók létrehozása vagy a virtuális hálózati cím előtag és előtagot használni kívánt értékeket.

Ebben a részben vesz fel paraméterek az exportált sablon, hogy újra felhasználhatja a sablont, amikor rendszerbe állítják ezek az erőforrások más környezetben. Ha annak valószínűségét, hogy ha a hiba, amikor rendszerbe állítják a sablon csökkenteni szeretné a sablon is hozzáadhat funkciókkal. Már nem kell egy egyedi nevet a tárolási fiók találat. Ehelyett a sablon létrehoz egy egyedi nevet. Korlátozza a a tárhely fióktípus csak érvényes beállítások megadott értékeket.

1. Jelölje ki, ha testre szeretné szabni a sablon **szerkesztése** lehetőséget.

     ![sablon megjelenítése](./media/resource-manager-export-template/show-template.png)

1. Jelölje ki a sablont.

     ![sablon szerkesztése](./media/resource-manager-export-template/edit-template.png)

1. Engedélyezni, érdemes lehet adja meg a telepítés során értékeket adják át, a **Paraméterek** rész cserélje új paraméter definíciók. Figyelje meg, **allowedValues** **storageAccount_accountType**az értékeket. Ha véletlenül érvénytelen értéket ad, a telepítés megkezdése előtt ezt a hibát ismerhető fel. Is látható, hogy csak egy előtagot meg van adva a tárhely figyelembe jelölőnégyzetét, az előtag korlátozódik, 11 karaktert. Az előtag 11 karakter korlátozásához biztosíthatja, hogy a teljes neve nem haladja meg a tárterület-fiók karakterek maximális száma. Az előtag lehetővé teszi a tárhely fiókra alkalmazhatja a elnevezésük is egységes. Láthatja, hogy miként hozhat létre egy egyedi nevet a következő lépésben.

        "parameters": {
          "storageAccount_prefix": {
            "type": "string",
            "maxLength": 11
          },
          "storageAccount_accountType": {
            "defaultValue": "Standard_RAGRS",
            "type": "string",
            "allowedValues": [
              "Standard_LRS",
              "Standard_ZRS",
              "Standard_GRS",
              "Standard_RAGRS",
              "Premium_LRS"
            ]
          },
          "virtualNetwork_name": {
            "type": "string"
          },
          "addressPrefix": {
            "defaultValue": "10.0.0.0/16",
            "type": "string"
          },
          "subnetName": {
            "defaultValue": "subnet-1",
            "type": "string"
          },
          "subnetAddressPrefix": {
            "defaultValue": "10.0.0.0/24",
            "type": "string"
          }
        },

2. A sablon **változói** szakasza jelenleg ürül. **Változók** szakaszban hozzon létre, amelyek megkönnyítik – a többség a sablon az alábbi értékeket. Ez a szakasz cserélje le változó új definíciót. A **storageAccount_name** változó az előtag és a paraméter egyedi karakterlánc létrehozott erőforráscsoport azonosítója alapján fűzi össze. Már nem kell egy egyedi nevet találat, ha a paraméter értéke nyújtó.

        "variables": {
          "storageAccount_name": "[concat(parameters('storageAccount_prefix'), uniqueString(resourceGroup().id))]"
        },

3. A paraméterek és változó használni az erőforrás-definíciókat, új erőforrás-definíciók a **források** részben cserélje. Figyelje meg, hogy az erőforrás-definíciókat, nem az az érték, az erőforrás-tulajdonság rendelt kissé változott. A Tulajdonságok ugyanazok, mint az exportált sablonból tulajdonságait. Egyszerűen rendelni kívánt tulajdonságok paraméterértékeket csomagolásukkor értékek helyett. Hol található az erőforrások ugyanazon a helyen használni az erőforráscsoport **resourceGroup () .location** kifejezés keresztül van beállítva. A tárhely fióknév létrehozott változó hivatkozott és a **változók** kifejezés.

        "resources": [
          {
            "type": "Microsoft.Network/virtualNetworks",
            "name": "[parameters('virtualNetwork_name')]",
            "apiVersion": "2015-06-15",
            "location": "[resourceGroup().location]",
            "properties": {
              "addressSpace": {
                "addressPrefixes": [
                  "[parameters('addressPrefix')]"
                ]
              },
              "subnets": [
                {
                  "name": "[parameters('subnetName')]",
                  "properties": {
                    "addressPrefix": "[parameters('subnetAddressPrefix')]"
                  }
                }
              ]
            },
            "dependsOn": []
          },
          {
            "type": "Microsoft.Storage/storageAccounts",
            "name": "[variables('storageAccount_name')]",
            "apiVersion": "2015-06-15",
            "location": "[resourceGroup().location]",
            "tags": {},
            "properties": {
                "accountType": "[parameters('storageAccount_accountType')]"
            },
            "dependsOn": []
          }
        ]

4. Ha végzett, kattintson az **OK gombra** a sablon szerkesztése.

5. Jelölje ki, hogy **Mentse** a sablont a módosítások mentéséhez.

     ![sablon mentése](./media/resource-manager-export-template/save-template.png)

6. A frissített sablon üzembe helyezéséhez jelölje be a **központi telepítés**.

     ![sablon terjesztése](./media/resource-manager-export-template/deploy-template.png)

7. Adja meg a paraméterek értékeket, és válassza az új erőforráscsoport telepítéshez használni kívánt erőforrások.

## <a name="update-the-downloaded-parameters-file"></a>A paraméterek letöltött fájl frissítése

Ha hangklippel dolgozik a letöltött fájlok (helyett a portál tárat), frissítenie kell a paraméter letöltött fájlt. A paraméterek a sablon már nem egyezik. Nem rendelkezik paraméter fájl használatára, de egyszerűsíti a folyamat során meg újratelepítése környezetben. Az alapértelmezett értékeket definiált a sablonban számos a paramétereket, hogy a paraméter fájl csak a két érték kell használnia.

Cserélje le a parameters.json fájl tartalma:

```
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "storageAccount_prefix": {
      "value": "storage"
    },
    "virtualNetwork_name": {
      "value": "VNET"
    }
  }
}
```

A frissített paraméter fájl biztosít értékek csak paramétereket, amelyek nem rendelkezik egy alapértelmezett értéket. Biztosíthat értékeket a többi paraméterekkel, ha azt szeretné, hogy egy érték, amely eltér az alapértelmezett értéket.

## <a name="fix-export-issues"></a>Exportálás problémák megoldása

Nem minden erőforrástípus támogatja az Exportálás sablon függvény. Erőforrás-kezelő kifejezetten nem exportálja néhány erőforrástípus, hogy a bizalmas adatokat ki. Például a webhely config Ha egy kapcsolati karakterláncot, valószínűleg nem szeretné, hogy kifejezetten megjelenik egy exportált sablonban. Vissza a sablonba a hiányzó erőforrások manuális hozzáadásával elérheti a probléma megoldásához.

> [AZURE.NOTE] Exportálás problémák csak nem a telepítési előzmények közül, hanem egy erőforrás csoport exportáláskor találkozik. A legutóbbi üzembe legpontosabban megfelel az erőforrás csoport aktuális állapotát, ha a telepítő előzmények közül, hanem az erőforráscsoport exportálhatja kell a sablont. Ha módosította az erőforráscsoport nem egyetlen sablonba definiált csak exportálása egy erőforrás csoport.

Ha például egy sablont, amely tartalmazza a web app, az SQL-adatbázis és a kapcsolati karakterláncot, a webhely config erőforráscsoport exportálásához jelenik meg a következő üzenet:

![hibaüzenet látható](./media/resource-manager-export-template/show-error.png)

Jelölje ki az üzenetet jeleníti meg pontosan milyen erőforrástípus nem exportált. 
     
![hibaüzenet látható](./media/resource-manager-export-template/show-error-details.png)

Ez a témakör bemutatja a leggyakoribb megoldásai.

### <a name="connection-string"></a>Kapcsolati karakterlánc

A webhelyek erőforrás megadása a kapcsolati karakterláncot az adatbázishoz:

```
{
  "type": "Microsoft.Web/sites",
  ...
  "resources": [
    {
      "apiVersion": "2015-08-01",
      "type": "config",
      "name": "connectionstrings",
      "dependsOn": [
          "[concat('Microsoft.Web/Sites/', parameters('<site-name>'))]"
      ],
      "properties": {
          "DefaultConnection": {
            "value": "[concat('Data Source=tcp:', reference(concat('Microsoft.Sql/servers/', parameters('<database-server-name>'))).fullyQualifiedDomainName, ',1433;Initial Catalog=', parameters('<database-name>'), ';User Id=', parameters('<admin-login>'), '@', parameters('<database-server-name>'), ';Password=', parameters('<admin-password>'), ';')]",
              "type": "SQLServer"
          }
      }
    }
  ]
}
```    

### <a name="web-site-extension"></a>Webhely-bővítmény

A webhely erőforrás megadása a kód telepítése:

```
{
  "type": "Microsoft.Web/sites",
  ...
  "resources": [
    {
      "name": "MSDeploy",
      "type": "extensions",
      "location": "[resourceGroup().location]",
      "apiVersion": "2015-08-01",
      "dependsOn": [
        "[concat('Microsoft.Web/sites/', parameters('<site-name>'))]"
      ],
      "properties": {
        "packageUri": "[concat(parameters('<artifacts-location>'), '/', parameters('<package-folder>'), '/', parameters('<package-file-name>'), parameters('<sas-token>'))]",
        "dbType": "None",
        "connectionString": "",
        "setParameters": {
          "IIS Web Application Name": "[parameters('<site-name>')]"
        }
      }
    }
  ]
}
```

### <a name="virtual-machine-extension"></a>Virtuális gép bővítmény

Példák a virtuális gép bővítmények című témakörben talál [Azure Windows virtuális bővítmény konfigurációs minták](./virtual-machines/virtual-machines-windows-extensions-configuration-samples.md).

### <a name="virtual-network-gateway"></a>Virtuális hálózati átjáró

Hozzáadhat egy virtuális hálózati átjáró erőforrás.

```
{
  "type": "Microsoft.Network/virtualNetworkGateways",
  "name": "[parameters('<gateway-name>')]",
  "apiVersion": "2015-06-15",
  "location": "[resourceGroup().location]",
  "properties": {
    "gatewayType": "[parameters('<gateway-type>')]",
    "ipConfigurations": [
      {
        "name": "default",
        "properties": {
          "privateIPAllocationMethod": "Dynamic",
          "subnet": {
            "id": "[resourceId('Microsoft.Network/virtualNetworks/subnets', parameters('<vnet-name>'), parameters('<new-subnet-name>'))]"
          },
          "publicIpAddress": {
            "id": "[resourceId('Microsoft.Network/publicIPAddresses', parameters('<new-public-ip-address-Name>'))]"
          }
        }
      }
    ],
    "enableBgp": false,
    "vpnType": "[parameters('<vpn-type>')]"
  },
  "dependsOn": [
    "Microsoft.Network/virtualNetworks/codegroup4/subnets/GatewaySubnet",
    "[concat('Microsoft.Network/publicIPAddresses/', parameters('<new-public-ip-address-Name>'))]"
  ]
},
```

### <a name="local-network-gateway"></a>Helyi hálózati átjáró

Adja hozzá a helyi hálózaton átjáró erőforrás típusú.

```
{
    "type": "Microsoft.Network/localNetworkGateways",
    "name": "[parameters('<local-network-gateway-name>')]",
    "apiVersion": "2015-06-15",
    "location": "[resourceGroup().location]",
    "properties": {
      "localNetworkAddressSpace": {
        "addressPrefixes": "[parameters('<address-prefixes>')]"
      }
    }
}
```

### <a name="connection"></a>Kapcsolat

Adja hozzá a kapcsolat az erőforrás típusa.

```
{
    "apiVersion": "2015-06-15",
    "name": "[parameters('<connection-name>')]",
    "type": "Microsoft.Network/connections",
    "location": "[resourceGroup().location]",
    "properties": {
        "virtualNetworkGateway1": {
        "id": "[resourceId('Microsoft.Network/virtualNetworkGateways', parameters('<gateway-name>'))]"
      },
      "localNetworkGateway2": {
        "id": "[resourceId('Microsoft.Network/localNetworkGateways', parameters('<local-gateway-name>'))]"
      },
      "connectionType": "IPsec",
      "routingWeight": 10,
      "sharedKey": "[parameters('<shared-key>')]"
    }
},
```


## <a name="next-steps"></a>Következő lépések

Gratulálok! Sablon exportálása a portálon létrehozott forrásaiból megtanulta azt.

- Sablon keresztül [PowerShell](resource-group-template-deploy.md), [Azure CLI](resource-group-template-deploy-cli.md)vagy [REST API -t](resource-group-template-deploy-rest.md)telepítheti.
- Sablon Powershellen keresztül exportálása című témakör tartalmazza [Az Azure erőforrás-kezelő Azure a PowerShell használatával](powershell-azure-resource-manager.md).
- Azure CLI keresztül sablon exportálása című témakör tartalmazza [az Azure CLI for Mac, Linux, és a Windows Azure erőforrás-kezelővel használatát](xplat-cli-azure-resource-manager.md).
