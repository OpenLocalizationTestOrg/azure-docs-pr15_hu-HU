<properties
   pageTitle="Erőforrás-áttekintés szolgáltató hálózati |} Microsoft Azure"
   description="További tudnivalók az új hálózati erőforrás szolgáltató az Azure erőforrás-kezelő:"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn" />
<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="03/15/2016"
   ms.author="jdial" />

# <a name="network-resource-provider"></a>Hálózati erőforrás-szolgáltató
Egy megerősítő segítségre van szüksége az aktuális business success, az azt jelenti, hogy létrehozása és kezelése a nagyméretű hálózati tartsa szem előtt alkalmazások Agilis, rugalmas, biztonságos és megismételhető módon. Azure erőforrás-kezelő (ARM) az ilyen alkalmazások erőforrások erőforráscsoport egyetlen gyűjteményének létrehozását teszi lehetővé. Ilyen erőforrások kezelhetők a ARM különböző erőforrás-szolgáltatók között.

Azure erőforrás-kezelő támaszkodik más erőforrás-szolgáltatók az erőforrások hozzáférést biztosít. Vannak olyan három fő erőforrás-szolgáltatók: hálózat, a tárhely és a számítási. A dokumentum azt ismerteti, hogy a tulajdonságokat és a hálózati erőforrás szolgáltató előnyei többek között:

- **Metaadat-alapú** – információkat vehet erőforrások-címkék használata. A címkéket erőforráscsoport és előfizetések erőforrás-kihasználtság követésére használható.
- **A hálózat jobban kézben** - erőforrások módszerektől összekapcsolt hálózati, és szabályozhatja őket finomabb módon. Ez azt jelenti, hogy nagyobb rugalmasság a hálózati erőforrások kezelésére van.
- **Gyorsabb konfigurációs** - hálózati erőforrások módszerektől összekapcsolt, mert hozhat létre és téve ezzel párhuzamosan hálózati erőforrások. Ezt a konfigurációs idő csökkentett jelentősen rendelkezik.
- **Szerepkör alapú hozzáférés-vezérlés** - RBAC alapértelmezett szerepkörök adott biztonsági hatókörrel mellett a létrehozására szolgáló egyéni szerepkörök biztonságos kezelésére nyújt.
- **Egyszerűbb kezelése és telepítése** – könnyebb üzembe helyezéséhez és kezeléséhez alkalmazások, mivel is létrehozhat egy teljes alkalmazás Papírhalom erőforrások erőforráscsoport egyetlen gyűjteményei. És üzembe helyezéséhez óta telepítheti a sablon JSON hasznos egyszerűen megadásával gyorsabban.
- A **Gyors testreszabás** - telepítések megismételhető és gyors testreszabásának engedélyezése deklaráció stílusú sablonokat is használhatja.
- **Megismételhető testreszabási** - telepítések megismételhető és gyors testreszabásának engedélyezése deklaráció stílusú sablonokat is használhatja.
- **Kapcsolatok kezelése** – is használhatja az alábbi felületeken közül az erőforrások kezelése:
    - REST API-val alapján
    - A PowerShell
    - .NET SDK
    - Node.JS SDK
    - Java SDK
    - Azure CLI
    - Előzetes portál
    - ARM-sablon nyelve

## <a name="network-resources"></a>Hálózati erőforrások
Hálózati erőforrások független, most kezelése bízza meg őket kezelhetők a egyetlen számítási erőforrás (virtuális gépen). Ezzel biztosíthatja, hogy nagyobb fokú rugalmasság és a szerkesztési egy összetett és nagy skála infrastruktúra erőforráscsoport agility.

A többszörös többszintű alkalmazások érintő minta telepítésre sematikus alább látható. Az egyes erőforrások jelenik meg, például hálózati kártyák, nyilvános IP-címek és VMs, egymástól függetlenül is kezelhetők.

![Hálózati erőforrás modell](./media/resource-groups-networking/Figure2.png)

Minden erőforrás egy ugyanazokat a tulajdonságokat tartalmazza, és saját egyéni tulajdonsága. Az általános tulajdonságok a következők:

|A tulajdonság|Leírás|Minta értékek|
|---|---|---|
|**név**|Egyedi erőforrás nevét. Minden erőforrástípus saját elnevezési korlátozásainak tartalmaz.|PIP01, VM01, NIC01|
|**hely**|Azure régiót, amelyben az erőforrás található|westus, eastus|
|**azonosító**|Egyedi alapú URI azonosítók|/Subscriptions/<subGUID>/resourceGroups/TestRG/providers/Microsoft.Network/publicIPAddresses/TestPIP|

Ellenőrizheti, hogy az egyedi tulajdonságai az erőforrásokat, az alábbi szakaszok.

[AZURE.INCLUDE [virtual-networks-nrp-pip-include](../../includes/virtual-networks-nrp-pip-include.md)]

[AZURE.INCLUDE [virtual-networks-nrp-nic-include](../../includes/virtual-networks-nrp-nic-include.md)]

[AZURE.INCLUDE [virtual-networks-nrp-nsg-include](../../includes/virtual-networks-nrp-nsg-include.md)]

[AZURE.INCLUDE [virtual-networks-nrp-udr-include](../../includes/virtual-networks-nrp-udr-include.md)]

[AZURE.INCLUDE [virtual-networks-nrp-vnet-include](../../includes/virtual-networks-nrp-vnet-include.md)]

[AZURE.INCLUDE [virtual-networks-nrp-dns-include](../../includes/virtual-networks-nrp-dns-include.md)]

[AZURE.INCLUDE [virtual-networks-nrp-lb-include](../../includes/virtual-networks-nrp-lb-include.md)]

[AZURE.INCLUDE [virtual-networks-nrp-appgw-include](../../includes/virtual-networks-nrp-appgw-include.md)]

[AZURE.INCLUDE [virtual-networks-nrp-vpn-include](../../includes/virtual-networks-nrp-vpn-include.md)]

[AZURE.INCLUDE [virtual-networks-nrp-tm-include](../../includes/virtual-networks-nrp-tm-include.md)]

## <a name="management-interfaces"></a>Kapcsolatok kezelése
Az Azure hálózati erőforrások különböző felületek használatával kezelheti. A dokumentum azt összpontosít e felületek fonókábelt: REST API-val és a sablonok.

### <a name="rest-api"></a>REST API-VAL
A korábban említett hálózati erőforrások kezelhető felületek, beleértve a REST API-t, .NET SDK, Node.JS SDK, Java SDK, PowerShell, CLI, Azure portál és sablonok számos keresztül.

A Rest API a HTTP 1.1 protokollt specifikációja felel meg. Az API általános URI szerkezetének alább látható:

    https://management.azure.com/subscriptions/{subscription-id}/providers/{resource-provider-namespace}/locations/{region-location}/register?api-version={api-version}

És a paraméterek kapcsos zárójelek jelenítik meg az alábbi elemeket:

- **előfizetés-azonosító** – az Azure előfizetés azonosítója.
- **erőforrás-szolgáltató-névtér** - használt szolgáltató névtere. A hálózati erőforrás szolgáltató értéke *Microsoft.Network*.
- **régió neve** - az Azure terület neve

Az alábbi HTTP módszerek támogatottak, amikor híváskezdeményezési a REST API-hoz:

- **HELYEZI el** – egy adott típusú erőforrás létrehozásához használt módosítsa az erőforrás-tulajdonság, vagy módosítsa a társítás erőforrások között.
- **GET** - kiépített erőforrás adatok beolvasásához használt.
- **Törlés** - törléséhez a meglévő erőforrás.

A kérelem és a válasz megfelelnek a JSON tartalom formátumot. További információra kíváncsi olvassa el a [Azure erőforrás-kezelés API](https://msdn.microsoft.com/library/azure/dn948464.aspx)című témakört.

### <a name="arm-template-language"></a>ARM-sablon nyelve
Kívül imperatively (keresztül API-khoz vagy SDK) erőforrások kezelésére, akkor is használhatja deklaráció programozási stílus létrehozása és kezelése a hálózati erőforrások nyelvének ARM sablon használatával.

A minta megadott sablon lenti –

    {
      "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json",
      "contentVersion": "<version-number-of-template>",
      "parameters": { <parameter-definitions-of-template> },
      "variables": { <variable-definitions-of-template> },
      "resources": [ { <definition-of-resource-to-deploy> } ],
      "outputs": { <output-of-template> }    
    }

A sablon (elsősorban JSON leírását az erőforrások és a paraméterek keresztül beékelt példány értékeket). Az alábbi példában egy virtuális hálózati 2 alhálózat létrehozásához használható.

    {
        "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/VNET.json",
        "contentVersion": "1.0.0.0",
        "parameters" : {
          "location": {
            "type": "String",
            "allowedValues": ["East US", "West US", "West Europe", "East Asia", "South East Asia"],
            "metadata" : {
              "Description" : "Deployment location"
            }
          },
          "virtualNetworkName":{
            "type" : "string",
            "defaultValue":"myVNET",
            "metadata" : {
              "Description" : "VNET name"
            }
          },
          "addressPrefix":{
            "type" : "string",
            "defaultValue" : "10.0.0.0/16",
            "metadata" : {
              "Description" : "Address prefix"
            }

          },
          "subnet1Name": {
            "type" : "string",
            "defaultValue" : "Subnet-1",
            "metadata" : {
              "Description" : "Subnet 1 Name"
            }
          },
          "subnet2Name": {
            "type" : "string",
            "defaultValue" : "Subnet-2",
            "metadata" : {
              "Description" : "Subnet 2 name"
            }
          },
          "subnet1Prefix" : {
            "type" : "string",
            "defaultValue" : "10.0.0.0/24",
            "metadata" : {
              "Description" : "Subnet 1 Prefix"
            }
          },
          "subnet2Prefix" : {
            "type" : "string",
            "defaultValue" : "10.0.1.0/24",
            "metadata" : {
              "Description" : "Subnet 2 Prefix"
            }
          }
        },
        "resources": [
        {
          "apiVersion": "2015-05-01-preview",
          "type": "Microsoft.Network/virtualNetworks",
          "name": "[parameters('virtualNetworkName')]",
          "location": "[parameters('location')]",
          "properties": {
            "addressSpace": {
              "addressPrefixes": [
                "[parameters('addressPrefix')]"
              ]
            },
            "subnets": [
              {
                "name": "[parameters('subnet1Name')]",
                "properties" : {
                  "addressPrefix": "[parameters('subnet1Prefix')]"
                }
              },
              {
                "name": "[parameters('subnet2Name')]",
                "properties" : {
                  "addressPrefix": "[parameters('subnet2Prefix')]"
                }
              }
            ]
          }
        }
        ]
    }

Van lehetőség a paraméterértékeket manuálisan sablon használata esetén, vagy használhatja a paraméter fájlt. Az alábbi példában a sablonnal használandó paraméterértékeket lehetséges halmazának jeleníti meg:

    {
      "location": {
          "value": "East US"
      },
      "virtualNetworkName": {
          "value": "VNET1"
      },
      "subnet1Name": {
          "value": "Subnet1"
      },
      "subnet2Name": {
          "value": "Subnet2"
      },
      "addressPrefix": {
          "value": "192.168.0.0/16"
      },
      "subnet1Prefix": {
          "value": "192.168.1.0/24"
      },
      "subnet2Prefix": {
          "value": "192.168.2.0/24"
      }
    }


Sablonok használata a legfontosabb előnye a következők:

- Készíthet egy összetett infrastruktúra deklaráció stílussal egy erőforrás csoportban. ARM létrehozása az erőforrások függőség kezelése, beleértve az üzletifolyamat-tervező kezeli.
- Az infrastruktúrára hozhat létre megismételhető útján különböző régiók keresztül, valamint egy régión belüli egyszerűen a paraméterek módosítása.
- A deklaráció stílust a sablonok létrehozása és az infrastruktúra helyezése rövidebb az átvezetési idő vezet.

Mintasablonok tanulmányozza az [Azure quickstart útmutató sablonok](https://github.com/Azure/azure-quickstart-templates)című témakört.

ARM sablon nyelvi további tudnivalókért lásd: [Azure erőforrás-kezelő sablon nyelve](../resource-group-authoring-templates.md).

A fenti minta sablon virtuális hálózati és alhálózat erőforrások használja. Vannak más használhat az alább felsorolt hálózati erőforrások:

### <a name="using-a-template"></a>Sablon használatával

Telepítheti szolgáltatások Azure sablonból PowerShell, AzureCLI, vagy úgy, hogy egy kattintással GitHub az üzembe elvégzéséhez. Egy sablonból a GitHub szolgáltatásainak üzembe helyezéséhez hajtsa végre az alábbi lépéseket:

1. Nyissa meg a template3 fájlt a GitHub. Példaként nyissa meg a [két alhálózat virtuális-hálózaton](https://github.com/Azure/azure-quickstart-templates/tree/master/101-virtual-network).
2. Kattintson a **Deploy az Azure**, és jelentkezzen be az Azure-portálra a hitelesítő adatokkal.
3. Ellenőrizze a sablont, és kattintson a **Mentés**gombra.
4. Kattintson a **szerkesztése paramétereinek** , és válasszon egy helyet, például *Nyugati Amerikai Egyesült Államok*, a vnet és alhálózat.
5. Ha szükséges, a **ADDRESSPREFIX** és **SUBNETPREFIX** paramétereinek módosítása, és kattintson **az OK**gombra.
6. Kattintson a **Jelöljön ki egy erőforrás csoportot** , és kattintson az erőforráscsoport fel szeretné venni a vnet és az alhálózathoz gombjára. Új erőforráscsoport azt is megteheti, **vagy hozzon létre új**kattintva hozhat létre.
3. Kattintson a **létrehozása**gombra. Figyelje meg a **sablon kiépítési telepítésének**megjelenítése csempére. Miután a telepítés befejeződött, látni fogja a hasonló képernyőn az alábbi.

![Példa a sablon telepítésének](./media/resource-groups-networking/Figure6.png)


## <a name="next-steps"></a>Következő lépések

[Azure erőforrás-kezelő sablon nyelve](../resource-group-authoring-templates.md)

[Azure hálózati – gyakran használt sablonok](https://github.com/Azure/azure-quickstart-templates)

[Kiszámítása az erőforrás-szolgáltató](../virtual-machines/virtual-machines-windows-compare-deployment-models.md)

[Azure erőforrás szolgáltatásának áttekintése](../azure-resource-manager/resource-group-overview.md)
