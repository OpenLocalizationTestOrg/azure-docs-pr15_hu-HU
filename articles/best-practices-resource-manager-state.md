<properties
    pageTitle="Az erőforrás-kezelő sablonok kezelési állapot |} Microsoft Azure"
    description="Összetett objektumok rendszerállapot-adatok megosztása Azure erőforrás-kezelő sablonok és csatolt sablon használatához az alábbi módszerek ajánlott megjelenítése"
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
    ms.date="10/26/2016"
    ms.author="tomfitz"/>

# <a name="sharing-state-in-azure-resource-manager-templates"></a>Az erőforrás-kezelő Azure sablonok állam megosztása

Ez a témakör bemutatja a gyakorlati tanácsok a kezelése és megosztása sablonok állapotot. A paraméterek és a változók látható ez a témakör ábrázolnak definiálhatók objektumok típusú kényelmesen rendszerezheti az üzembe igényeknek megfelelően alakíthatja. Ezek a példák a saját környezetben érvényes tulajdonság értékeket tartalmazó objektumok hajtja végre.

Ez a témakör a nagyobb szeretne része. Olvassa el a teljes papíron, töltse le a [világ osztály erőforrás Manager sablonok szempontot, és az igazolt eljárások](http://download.microsoft.com/download/8/E/1/8E1DBEFA-CECE-4DC9-A813-93520A5D7CFE/World Class ARM Templates - Considerations and Proven Practices.pdf).


## <a name="provide-standard-configuration-settings"></a>Szabványos beállításainak megadása

Hanem egy sablont, amely a teljes rugalmasság és számtalan változatok kínál, a közös mintát is adja meg a kijelölt ismert konfigurációt. Gyakorlatilag azt mondja a felhasználók választhatnak szabványos póló méreteket, például védőfalas, kicsi, közepes és nagyméretű. A póló méretű más példák termék szeretne rendelni, például közösségi edition vagy enterprise edition. Egyéb esetben lehet, hogy terhelést nyelvspecifikus beállítások egy technológia – például a térkép csökkentése vagy nincs sql.

Összetett objektumokkal változók, amelyeknél az adatokat, más néven "tulajdonságcsomaghoz" webhelycsoportok létrehozása, és az erőforrás deklarálási meghajtóra a sablon használatára. Ezt a megközelítést ügyfelek nyújt a helyes, ismert konfigurációk különböző méretű, hogy be vannak állítva. Nélkül ismert konfigurációk esetén a sablon felhasználóinak kell önállóan méretező fürt határozza meg, a platform erőforrás korlátozó tényező, és végezze el a matematikai azonosítása, az eredményül kapott szétválasztás tároló fiókok és más erőforrások (miatt fürt méretét és az erőforrás megkötésekkel). Mellett a jobb használhatóság végez a vevő, néhány ismert konfigurációk könnyebben támogatja, és előadása sűrűség magasabb szintű segíthetnek.

A következő példa bemutatja, hogyan adatok gyűjteménye, amely az összetett objektumokat tartalmazó változók. A webhelycsoportok értékek, amelyek segítségével virtuális gép méretét, hálózati beállítások, az operációs rendszer beállításai és elérhetőség beállításainak megadása

    "variables": {
      "tshirtSize": "[variables(concat('tshirtSize', parameters('tshirtSize')))]",
      "tshirtSizeSmall": {
        "vmSize": "Standard_A1",
        "diskSize": 1023,
        "vmTemplate": "[concat(variables('templateBaseUrl'), 'database-2disk-resources.json')]",
        "vmCount": 2,
        "storage": {
          "name": "[parameters('storageAccountNamePrefix')]",
          "count": 1,
          "pool": "db",
          "map": [0,0],
          "jumpbox": 0
        }
      },
      "tshirtSizeMedium": {
        "vmSize": "Standard_A3",
        "diskSize": 1023,
        "vmTemplate": "[concat(variables('templateBaseUrl'), 'database-8disk-resources.json')]",
        "vmCount": 2,
        "storage": {
          "name": "[parameters('storageAccountNamePrefix')]",
          "count": 2,
          "pool": "db",
          "map": [0,1],
          "jumpbox": 0
        }
      },
      "tshirtSizeLarge": {
        "vmSize": "Standard_A4",
        "diskSize": 1023,
        "vmTemplate": "[concat(variables('templateBaseUrl'), 'database-16disk-resources.json')]",
        "vmCount": 3,
        "slaveCount": 2,
        "storage": {
          "name": "[parameters('storageAccountNamePrefix')]",
          "count": 2,
          "pool": "db",
          "map": [0,1,1],
          "jumpbox": 0
        }
      },
      "osSettings": {
        "scripts": [
          "[concat(variables('templateBaseUrl'), 'install_postgresql.sh')]",
          "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/shared_scripts/ubuntu/vm-disk-utils-0.1.sh"
        ],
        "imageReference": {
          "publisher": "Canonical",
          "offer": "UbuntuServer",
          "sku": "14.04.2-LTS",
          "version": "latest"
        }
      },
      "networkSettings": {
        "vnetName": "[parameters('virtualNetworkName')]",
        "addressPrefix": "10.0.0.0/16",
        "subnets": {
          "dmz": {
            "name": "dmz",
            "prefix": "10.0.0.0/24",
            "vnet": "[parameters('virtualNetworkName')]"
          },
          "data": {
            "name": "data",
            "prefix": "10.0.1.0/24",
            "vnet": "[parameters('virtualNetworkName')]"
          }
        }
      },
      "availabilitySetSettings": {
        "name": "pgsqlAvailabilitySet",
        "fdCount": 3,
        "udCount": 5
      }
    }

Figyelje meg, hogy a **tshirtSize** változó keresztül (**kicsi**, **Közepes**, **nagy**) paraméter megadott póló méretének összefűzi a szöveg **tshirtSize**szeretne. Ez a változó segítségével beolvashatja a társított összetett objektumváltozó az adott póló méret.

A sablon a következő változók majd hivatkozhat. A névvel ellátott változók és tulajdonságaik mutató hivatkozás egyszerűbbé teszi a sablon képletszintaxisát és megkönnyíti a környezet ismertetése. A következő példa egy erőforrást, telepítse az értékek beállítására korábban látható objektumok segítségével határozza meg. Például a virtuális méretének beállítása úgy, hogy az érték beolvasása `variables('tshirtSize').vmSize` során az érték, a lemez méretét keresi ki a `variables('tshirtSize').diskSize`. Ezeken kívül az URI csatolt sablon értékének beállítása `variables('tshirtSize').vmTemplate`.

    "name": "master-node",
    "type": "Microsoft.Resources/deployments",
    "apiVersion": "2015-01-01",
    "dependsOn": [
        "[concat('Microsoft.Resources/deployments/', 'shared')]"
    ],
    "properties": {
        "mode": "Incremental",
        "templateLink": {
          "uri": "[variables('tshirtSize').vmTemplate]",
          "contentVersion": "1.0.0.0"
        },
        "parameters": {
          "adminPassword": {
            "value": "[parameters('adminPassword')]"
          },
          "replicatorPassword": {
            "value": "[parameters('replicatorPassword')]"
          },
          "osSettings": {
            "value": "[variables('osSettings')]"
          },
          "subnet": {
            "value": "[variables('networkSettings').subnets.data]"
          },
          "commonSettings": {
            "value": {
              "region": "[parameters('region')]",
              "adminUsername": "[parameters('adminUsername')]",
              "namespace": "ms"
            }
          },
          "storageSettings": {
            "value":"[variables('tshirtSize').storage]"
          },
          "machineSettings": {
            "value": {
              "vmSize": "[variables('tshirtSize').vmSize]",
              "diskSize": "[variables('tshirtSize').diskSize]",
              "vmCount": 1,
              "availabilitySet": "[variables('availabilitySetSettings').name]"
            }
          },
          "masterIpAddress": {
            "value": "0"
          },
          "dbType": {
            "value": "MASTER"
          }
        }
      }
    }

## <a name="pass-state-to-a-template"></a>Állapot átadni sablon

Állapot osztani egy sablont, közvetlenül a telepítés során nyújtó paraméterek keresztül.

Az alábbi táblázat a gyakran használt paraméterek sablonokat.

név | Érték | Leírás
---- | ----- | -----------
hely    | Karakterlánc-Azure régiók korlátozott listájából   | A hely, ahol az erőforrások van telepítve.
storageAccountNamePrefix    | Karakterlánc    | A tároló fiók hol kerülnek, a virtuális lemezt egyedi DNS neve
Tartománynév  | Karakterlánc    | A nyilvánosan hozzáférhető jumpbox virtuális formátumban tartománynevét: {tartománynév} **. { hely}.cloudapp.com** például: **mydomainname.westus.cloudapp.azure.com**
adminUsername   | Karakterlánc    | A VMs felhasználóneve
adminPassword   | Karakterlánc    | A VMs jelszavának
tshirtSize  | A korlátozott listájából karakterlánc felajánlott póló méretek   | Az elnevezett skála egység kiépítése mérete. Ha például a "– kicsi", "közepes", "Nagy"
virtualNetworkName  | Karakterlánc    | A virtuális hálózat, amely használni szeretné a fogyasztói nevét.
enableJumpbox   | Karakterlánc (engedélyezett/letiltott) korlátozott listájából | Paraméter, amely azonosítja a környezet egy jumpbox engedélyezi-e. Értékek: "engedélyezett", "Letiltva"

Az előző szakaszában használt **tshirtSize** paraméter határozható meg:

    "parameters": {
      "tshirtSize": {
        "type": "string",
        "defaultValue": "Small",
        "allowedValues": [
          "Small",
          "Medium",
          "Large"
        ],
        "metadata": {
          "Description": "T-shirt size of the MongoDB deployment"
        }
      }
    }


## <a name="pass-state-to-linked-templates"></a>Állapot átadni csatolt sablonok

Csatolt sablonok való csatlakozáskor, gyakran kombinálja a statikus, és hozza létre a változók.

### <a name="static-variables"></a>Statikus változók

Statikus változók gyakran használatával adja meg a viszonyítási értékeket, például az URL-címek, hogy sablon mindenütt használatban vannak.

A következő sablon kivonat az `templateBaseUrl` adja meg a sablon gyökerének GitHub. A következő sorba hoz létre egy új változó `sharedTemplateUrl` , amely a megosztott erőforrásokat sablon ismert nevű alap URL-címe fűzi össze. Hogy a sor alatt egy összetett objektumváltozó póló méretét, ha az alap URL-cím ismert konfigurációs sablon helyét a tömb összefűzéséből és tárolt tárolására használja a `vmTemplate` tulajdonság.

Ezt a megközelítést az az előnye, hogy, ha megváltozik a sablon helyét, csak módosítania a statikus változó átadja a csatolt sablonok egész egy helyen.

    "variables": {
      "templateBaseUrl": "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/postgresql-on-ubuntu/",
      "sharedTemplateUrl": "[concat(variables('templateBaseUrl'), 'shared-resources.json')]",
      "tshirtSizeSmall": {
        "vmSize": "Standard_A1",
        "diskSize": 1023,
        "vmTemplate": "[concat(variables('templateBaseUrl'), 'database-2disk-resources.json')]",
        "vmCount": 2,
        "slaveCount": 1,
        "storage": {
          "name": "[parameters('storageAccountNamePrefix')]",
          "count": 1,
          "pool": "db",
          "map": [0,0],
          "jumpbox": 0
        }
      }
    }

### <a name="generated-variables"></a>A létrehozott változók

Statikus változót kívül többváltozós jönnek létre dinamikusan. Ebben a részben néhány létrehozott változók elterjedt típusú azonosítja.

#### <a name="tshirtsize"></a>tshirtSize

Jártas a létrehozott változó a fenti példákban megismert.

#### <a name="networksettings"></a>networkSettings

A beosztását, lehetőséget vagy hatókörű megoldás végpont sablon a csatolt sablonok általában erőforrások létrehozása megtalálható a hálózaton. Egy egyszerű megközelítés, mentheti a hálózati beállításokat, és adja meg azokat a csatolt sablonokra egy összetett objektum használatára.

Példa hálózati beállítások kommunikáció alatt látható legyen.

    "networkSettings": {
      "vnetName": "[parameters('virtualNetworkName')]",
      "addressPrefix": "10.0.0.0/16",
      "subnets": {
        "dmz": {
          "name": "dmz",
          "prefix": "10.0.0.0/24",
          "vnet": "[parameters('virtualNetworkName')]"
        },
        "data": {
          "name": "data",
          "prefix": "10.0.1.0/24",
          "vnet": "[parameters('virtualNetworkName')]"
        }
      }
    }

#### <a name="availabilitysettings"></a>availabilitySettings

Erőforrások csatolt sablonok létrehozott egy elérhetőségének beállítása gyakran kerülnek. A következő példában az elérhetőség beállítása mappanevet, és is a hibafa és frissítés tartományban megszámolása használni.

    "availabilitySetSettings": {
      "name": "pgsqlAvailabilitySet",
      "fdCount": 3,
      "udCount": 5
    }

Több elérhetőségének beállítása (például egy fő csomópontok) és az adatok csomópontokat, egy másik használata a név előtaggal van szüksége, ha több elérhetőségének beállítása adja meg, vagy hajtsa végre a modell hozhat létre egy adott póló méretű változó korábbi látható.

#### <a name="storagesettings"></a>storageSettings

Csatolt sablonok gyakran megosztott tároló részleteket. Az alábbi példában egy *storageSettings* objektum részletesen leírja a tárterület-fiók és a tároló nevét.

    "storageSettings": {
        "vhdStorageAccountName": "[parameters('storageAccountName')]",
        "vhdContainerName": "[variables('vmStorageAccountContainerName')]",
        "destinationVhdsContainer": "[concat('https://', parameters('storageAccountName'), variables('vmStorageAccountDomain'), '/', variables('vmStorageAccountContainerName'), '/')]"
    }

#### <a name="ossettings"></a>osSettings

Csatolt sablonokkal szüksége lehet átadni az operációs rendszer-beállítási különféle csomópontok különböző ismert konfigurációs típusok között. Egy összetett objektum egyszerűen tárolása és megosztása az operációs rendszer-információkat és is megkönnyíti a több operációs rendszer lehetőségek támogatási telepítéshez.

A következő példa bemutatja a *osSettings*objektum:

    "osSettings": {
      "imageReference": {
        "publisher": "Canonical",
        "offer": "UbuntuServer",
        "sku": "14.04.2-LTS",
        "version": "latest"
      }
    }

#### <a name="machinesettings"></a>machineSettings

A létrehozott, *machineSettings* változó egy összetett objektumra, amely kombinálja core változót egy virtuális hozhat létre. A változók közé tartoznak a rendszergazda felhasználónevet és jelszót, a virtuális nevét, és az operációs rendszer kép összefoglaló előtaggal.

    "machineSettings": {
        "adminUsername": "[parameters('adminUsername')]",
        "adminPassword": "[parameters('adminPassword')]",
        "machineNamePrefix": "mongodb-",
        "osImageReference": {
            "publisher": "[variables('osFamilySpec').imagePublisher]",
            "offer": "[variables('osFamilySpec').imageOffer]",
            "sku": "[variables('osFamilySpec').imageSKU]",
            "version": "latest"
        }
    },

Figyelje meg, hogy *osImageReference* értékeket beolvassa a fő sablonban definiált *osSettings* változó. Ez azt jelenti, akkor könnyen módosíthatja az operációs rendszer egy virtuális – teljes egészében vagy egy sablon fogyasztói a igényei alapján.

#### <a name="vmscripts"></a>vmScripts

A *vmScripts* objektum töltheti le, és egy virtuális-példányt, beleértve a kívüli és belüli hivatkozás hajtsa végre a parancsfájlok adatait tartalmazza. Kívüli beleértendők a infrastruktúra. Belső beleértendők a telepített szoftverek telepítése és beállítása.

A *scriptsToDownload* tulajdonsággal a parancsfájlok kattintva töltse le a virtuális listája. Az objektum különféle műveletek parancssori argumentumok hivatkozásokat is tartalmaz. Az alábbi műveletek például végrehajtása az alapértelmezett telepítés minden egyes csomópont-telepítés után csomópontjait telepítik futó és, lehet, hogy egy adott sablon jellemző további parancsfájlok.

Ebben a példában az üzembe MongoDB egy nagy elérhetősége előadásához soron igénylő használt sablonból. A *arbiterNodeInstallCommand* *vmScripts* telepítése a soron lett hozzáadva.

A változók szakasz, ahol elérhetők, amelyek meghatározzák az adott szöveget, hajtsa végre a megfelelő értékekkel, parancsfájlt változó figyelembevételével.

    "vmScripts": {
        "scriptsToDownload": [
            "[concat(variables('scriptUrl'), 'mongodb-', variables('osFamilySpec').osName, '-install.sh')]",
            "[concat(variables('sharedScriptUrl'), 'vm-disk-utils-0.1.sh')]"
        ],
        "regularNodeInstallCommand": "[variables('installCommand')]",
        "lastNodeInstallCommand": "[concat(variables('installCommand'), ' -l')]",
        "arbiterNodeInstallCommand": "[concat(variables('installCommand'), ' -a')]"
    },


## <a name="return-state-from-a-template"></a>Lépjen vissza az állapot sablonból

Nem, csak akkor továbbíthatja adatok sablonba, meg is oszthatja azt a hívó sablon vissza az adatokat. A **Kimenet** csoportban a csatolt sablon adhat meg, amely a forrás sablon által igénybe vehető kulcs/érték párokká.

A következő példa bemutatja, hogyan adják át a létrehozott csatolt sablonba magánjellegű IP-címét.

    "outputs": {
        "masterip": {
            "value": "[reference(concat(variables('nicName'),0)).ipConfigurations[0].properties.privateIPAddress]",
            "type": "string"
         }
    }

A fő sablon belül használhatja az adatokat a következő szintaxissal:

    "[reference('master-node').outputs.masterip.value]"

Ez a kifejezés a kimeneti értékeket szakasz vagy a források részben fő sablon is használhatja. A kifejezés nem használhatja változók szakaszában, mivel a futási idejű állapotát támaszkodik. Ez az érték a fő sablonból kiszámításához használja:

    "outputs": { 
      "masterIpAddress": {
        "value": "[reference('master-node').outputs.masterip.value]",
        "type": "string"
      }
     
Példa a kimeneti értékeket szakasza csatolt sablon használatával állítsa vissza adatok lemezt virtuális géphez lásd: a [több adatok lemezre virtuális gép létrehozása](resource-group-create-multiple.md#creating-multiple-data-disks-for-a-virtual-machine)elemre.

## <a name="define-authentication-settings-for-virtual-machine"></a>Virtuális gép hitelesítési beállítások megadása

Az azonos mintának beállításokkal jelennek meg a korábban segítségével adja meg a hitelesítési beállítások virtuális géphez. Milyen típusú hitelesítéssel hoz létre egy átadása paramétert.

    "parameters": {
      "authenticationType": {
        "allowedValues": [
          "password",
          "sshPublicKey"
        ],
        "defaultValue": "password",
        "metadata": {
          "description": "Authentication type"
        },
        "type": "string"
      }
    }

A különböző hitelesítéstípusok változói felvette, és a változó tárolásához, melyik típust használja a telepítéshez, ha a paraméter értéke alapján.

    "variables": {
      "osProfile": "[variables(concat('osProfile', parameters('authenticationType')))]",
      "osProfilepassword": {
        "adminPassword": "[parameters('adminPassword')]",
        "adminUsername": "notused",
        "computerName": "[parameters('vmName')]",
        "customData": "[base64(variables('customData'))]"
      },
      "osProfilesshPublicKey": {
        "adminUsername": "notused",
        "computerName": "[parameters('vmName')]",
        "customData": "[base64(variables('customData'))]",
        "linuxConfiguration": {
          "disablePasswordAuthentication": "true",
          "ssh": {
            "publicKeys": [
              {
                "keyData": "[parameters('sshPublicKey')]",
                "path": "/home/notused/.ssh/authorized_keys"
              }
            ]
          }
        }
      }
    }

A virtuális gép megadásakor a **osProfile** beállíthatja a létrehozott változó.

    {
      "type": "Microsoft.Compute/virtualMachines",
      ...
      "osProfile": "[variables('osProfile')]"
    }


## <a name="next-steps"></a>Következő lépések
- A sablon a szakaszok kapcsolatos további tudnivalókért lásd: [Azure erőforrás-kezelő sablonok létrehozása](resource-group-authoring-templates.md)
- A sablon belül használható függvények megtekintéséhez kattintson a [Azure erőforrás-kezelő sablon függvények](resource-group-template-functions.md)

