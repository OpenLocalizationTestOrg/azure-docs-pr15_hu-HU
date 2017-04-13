<properties
   pageTitle="Erőforrás-kezelő sablon tárolására |} Microsoft Azure"
   description="Az erőforrás-kezelő séma üzembe helyezése a sablon tárterület-fiókokat látható."
   services="azure-resource-manager,storage"
   documentationCenter="na"
   authors="tfitzmac"
   manager="timlt"
   editor=""/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="04/05/2016"
   ms.author="tomfitz"/>

# <a name="storage-account-template-schema"></a>Tárterület-fiók sablon séma

Létrehoz egy tárterület-fiókot.

## <a name="schema-format"></a>Séma formázása

Tárterület-fiók létrehozása, a sablon a források szakaszában a következő séma hozzá.

    {
        "type": "Microsoft.Storage/storageAccounts",
        "apiVersion": "2015-06-15",
        "name": string,
        "location": string,
        "properties": 
        {
            "accountType": string
        }
    }

## <a name="values"></a>Értékek

Az alábbi táblázat ismerteti a értékeket kell beállítania a sémában.

| név | Érték |
| ---- | ---- |
| típus | Felsorolás<br />Szükséges<br />**Microsoft.Storage/storageAccounts**<br /><br />Az erőforrás típusa hozhat létre. |
| apiVersion | Felsorolás<br />Szükséges<br />**2015-06-15** vagy **a Skype 2015-05-01 – előzetes verzió**<br /><br />Az erőforrás létrehozásához használandó API verziószáma. | 
| név | Karakterlánc<br />Szükséges<br />3 – 24 karakter, csak számokat és betűket kisbetűk.<br /><br />Tárterület-fiók létrehozása neve. A név összes Azure egyedinek kell lennie. Fontolja meg inkább a [uniqueString](resource-group-template-functions.md#uniquestring) függvény és az elnevezésük is egységes az alábbi példában látható módon. |
| hely | Karakterlánc<br />Szükséges<br />Egy tartomány, amely támogatja a tárterület-fiókok. Annak megállapításához, érvényes régiók, olvassa el a [támogatott régiók](resource-manager-supported-services.md#supported-regions)című témakört.<br /><br />A terület tárolni a tárterület-fiókot. |
| Tulajdonságok | Objektum<br />Szükséges<br />[objektum tulajdonságai](#properties)<br /><br />Adja meg a tárterület-fiók létrehozása objektum. |

<a id="properties" />
### <a name="properties-object"></a>objektum tulajdonságai

| név | Érték |
| ---- | ---- | 
| accountType | Karakterlánc<br />Szükséges<br />**Standard_LRS**, **Standard_ZRS**, **Standard_GRS**, **Standard_RAGRS**vagy **Premium_LRS**<br /><br />A tároló fiók típusát. A megengedett értékek Standard helyileg felesleges, normál időzónát felesleges, szabványos Geo-redundáns, szabványos olvasásra Geo redundáns és prémium helyileg felesleges felelnek meg. Az alábbi fióktípusok információkért lásd: [Azure tároló replikáció](./storage/storage-redundancy.md ). |

    
## <a name="examples"></a>Példák

Az alábbi példában egy egyedi nevet a csoport erőforrás-azonosító alapján való üzembe helyezése egy szabványos helyileg felesleges tárterület-fiókkal.

    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {},
        "variables": {},
        "resources": [
            {
                "type": "Microsoft.Storage/storageAccounts",
                "apiVersion": "2015-06-15",
                "name": "[concat('storage', uniqueString(resourceGroup().id))]",
                "location": "[resourceGroup().location]",
                "properties": 
            {
                    "accountType": "Standard_LRS"
            }
            }
        ],
        "outputs": {}
    }

## <a name="quickstart-templates"></a>Quickstart útmutató sablonok

Vannak olyan számos quickstart útmutató sablont, amely tartalmazza a tárterület-fiók. Az alábbi sablonok néhány gyakori alkalmazási területek szemlélteti:

- [Szabványos tárterület-fiók létrehozása](https://azure.microsoft.com/documentation/templates/101-storage-account-create)
- [Egy Windows virtuális egyszerű telepítése](https://azure.microsoft.com/documentation/templates/101-vm-simple-windows)
- [Egy Linux virtuális egyszerű telepítése](https://azure.microsoft.com/documentation/templates/101-vm-simple-linux)
- [Hozzon létre egy CDN-profilt, egy CDN-végpontot, mint origin tároló fiókkal](https://azure.microsoft.com/documentation/templates/201-cdn-with-storage-account)
- [A Powershell DSC kiterjesztés használatával 9 VMs magas Availabilty SharePoint-Farm létrehozása](https://azure.microsoft.com/documentation/templates/sharepoint-server-farm-ha)
- [Egy 5 egyszerű telepítési csomópont biztonságos szolgáltatás háló fürtre engedélyezni ÜVEGVATTA](https://azure.microsoft.com/documentation/templates/service-fabric-secure-cluster-5-node-1-nodetype-wad)
- [A 4 üres adatok lemezt Windows képen lévő virtuális gép létrehozása](https://azure.microsoft.com/documentation/templates/101-vm-multiple-data-disk)


## <a name="next-steps"></a>Következő lépések

- Tárhely, általános információt című témakörben [a Microsoft Azure-tárterületet](./storage/storage-introduction.md).
- Új tárterület-fiók használata egy virtuális gép sablonok például [egy egyszerű Linux virtuális Deploy](https://azure.microsoft.com/documentation/templates/101-simple-linux-vm/) vagy [Deploy egy egyszerű Windows virtuális](https://azure.microsoft.com/documentation/templates/101-simple-windows-vm/)témakörben talál.
