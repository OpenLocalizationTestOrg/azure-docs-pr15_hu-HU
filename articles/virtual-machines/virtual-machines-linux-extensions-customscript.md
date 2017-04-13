<properties
   pageTitle="Egyéni parancsfájlok Linux VMs |} Microsoft Azure"
   description="Az egyéni parancsfájl bővítmény használatával automatizálhatja a Linux virtuális beállítási feladatok"
   services="virtual-machines-linux"
   documentationCenter=""
   authors="neilpeterson"
   manager="timlt"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure-services"
   ms.date="09/22/2016"
   ms.author="nepeters"/>

# <a name="using-the-azure-custom-script-extension-with-linux-virtual-machines"></a>Linux virtuális gépeken futó az Azure egyéni parancsfájl bővítmény használata

Az egyéni parancsfájl bővítmény letölti és végrehajtja a parancsfájlok Azure virtuális gépeken. A bővítmény akkor hasznos, ha a bejegyzés konfigurációs, szoftver telepítése vagy bármely más konfiguráció / felügyeleti feladatot. Parancsfájlok letöltött Azure tároló vagy más elérhető internetes helyre, vagy a bővítmény futási idő adni. Az egyéni parancsfájl bővítmény Azure erőforrás-kezelő sablonok integrálódik, és is futtathatók az Azure CLI, PowerShell, Azure portál vagy az Azure virtuális gép REST API-t.

A dokumentum részletezi az egyéni parancsfájl kiterjesztésre az Azure CLI, és egy erőforrás-kezelő Azure-sablon használatához, és is részletek Linux rendszerben hibaelhárítási lépéseket.

## <a name="extension-configuration"></a>Bővítmény konfigurálása

Az egyéni parancsfájl bővítmény konfiguráció adja meg a összetevőjét, például a parancsprogram-helyet és egy futtatandó parancsot. Ebben a konfigurációban tárolható konfigurációs fájlokat a parancssorban vagy egy erőforrás-kezelő Azure sablonban megadott. Bizalmas adatokat a védett konfiguráció, amely a titkosított és a virtuális gép belül csak visszafejteni tárolható. A védett konfiguráció akkor hasznos, ha a végrehajtás parancsot tartalmazó titkos kulcsok, például a jelszó.

### <a name="public-configuration"></a>Nyilvános konfigurálása

Séma:

- **commandToExecute**: (kötelező, karakterlánc) végrehajtásához, a bejegyzés pont parancsfájl
- **fileUris**: (nem kötelező, karakterlánc tömb) az URL-címeit, a letöltendő fájlokat.
- **időbélyeg** (nem kötelező, egész) a mező használatával csak egy ismétlése a parancsprogram elindítani az aktuális mező értékének módosításával.

```none
{
  "fileUris": ["<url>"],
  "commandToExecute": "<command-to-execute>"
}
```

### <a name="protected-configuration"></a>Védett konfigurálása

Séma:

- **commandToExecute**: (nem kötelező, karakterlánc) a bejegyzés pont parancsfájl végrehajtani. Helyett használja ezt a mezőt, ha a parancs tartalmaz, például jelszavak titkos kulcsok.
- **storageAccountName**: (nem kötelező, karakterlánc) tárterület-fióknak a nevét. Ha a tároló hitelesítő adatok megadásához összes fileUris az URL-ek a Azure BLOB kell lennie.
- **storageAccountKey**: (nem kötelező, karakterlánc) a hívóbetű tárterület-fiók.


```json
{
  "commandToExecute": "<command-to-execute>",
  "storageAccountName": "<storage-account-name>",
  "storageAccountKey": "<storage-account-key>"
}
```

## <a name="azure-cli"></a>Azure CLI

Amikor az Azure CLI az egyéni parancsfájl bővítmény futtatása, hozzon létre konfigurációs fájl vagy fájlok legalább, amelyben a fájl uri és a parancs végrehajtása.

```none
azure vm extension set <resource-group> <vm-name> CustomScript Microsoft.Azure.Extensions 2.0 --auto-upgrade-minor-version --public-config-path /scirpt-config.json
```

Másik lehetőségként a parancs futtatható a `--public-config` és `--private-config` beállítás, amely lehetővé teszi, hogy a konfiguráció kell megadni a végrehajtás során, és egy külön konfigurációs fájl nélkül.

```none
azure vm extension set <resource-group> <vm-name> CustomScript Microsoft.Azure.Extensions 2.0 --auto-upgrade-minor-version --public-config '{"fileUris": ["https://gist.github.com/ahmetalpbalkan/b5d4a856fe15464015ae87d5587a4439/raw/466f5c30507c990a4d5a2f5c79f901fa89a80841/hello.sh"],"commandToExecute": "./hello.sh"}'
```

### <a name="azure-cli-examples"></a>Azure CLI példák

**Példa 1** - parancsprogram nyilvános konfigurálása.

```json
{
  "fileUris": ["https://gist.github.com/ahmetalpbalkan/b5d4a856fe15464015ae87d5587a4439/raw/466f5c30507c990a4d5a2f5c79f901fa89a80841/hello.sh"],
  "commandToExecute": "./hello.sh"
}
```

Azure CLI parancsot:

```none
azure vm extension set <resource-group> <vm-name> CustomScript Microsoft.Azure.Extensions 2.0 --auto-upgrade-minor-version --public-config-path /public.json
```

**Példa 2** – nem parancsprogram nyilvános konfigurálása.

```json
{
  "commandToExecute": "apt-get -y update && apt-get install -y apache2"
}
```

Azure CLI parancsot:

```none
azure vm extension set <resource-group> <vm-name> CustomScript Microsoft.Azure.Extensions 2.0 --auto-upgrade-minor-version --public-config-path /public.json
```

**Példa 3** – egy nyilvános konfigurációs fájl használatával adja meg a parancsfájlt URI, és egy védett konfigurációs fájl használatával adja meg a végrehajtandó parancsot.

Nyilvános konfigurációs fájl:

```json
{
  "fileUris": ["https://gist.github.com/ahmetalpbalkan/b5d4a856fe15464015ae87d5587a4439/raw/466f5c30507c990a4d5a2f5c79f901fa89a80841/hello.sh"],
}
```

Védett konfigurációs fájl:  

```json
{
  "commandToExecute": "./hello.sh <password>"
}
```

Azure CLI parancsot:

```none
azure vm extension set <resource-group> <vm-name> CustomScript Microsoft.Azure.Extensions 2.0 --auto-upgrade-minor-version --public-config-path ./public.json --private-config-path ./protected.json
```

## <a name="resource-manager-template"></a>Erőforrás-kezelő sablon

Az Azure egyéni parancsfájl bővítmény erőforrás-kezelő sablon használatával virtuális gép telepítési egyszerre futtatható. Ehhez a telepítési sablon megfelelően formázott JSON hozzá.

### <a name="resource-manager-examples"></a>Erőforrás-kezelő példák

**Példa 1** – nyilvános konfigurációs.

```json
{
    "name": "scriptextensiondemo",
    "type": "extensions",
    "location": "[resourceGroup().location]",
    "apiVersion": "2015-06-15",
    "dependsOn": [
        "[concat('Microsoft.Compute/virtualMachines/', parameters('scriptextensiondemoName'))]"
    ],
    "tags": {
        "displayName": "scriptextensiondemo"
    },
    "properties": {
        "publisher": "Microsoft.Azure.Extensions",
        "type": "CustomScript",
        "typeHandlerVersion": "2.0",
        "autoUpgradeMinorVersion": true,
      "settings": {
        "fileUris": [
          "https://gist.github.com/ahmetalpbalkan/b5d4a856fe15464015ae87d5587a4439/raw/466f5c30507c990a4d5a2f5c79f901fa89a80841/hello.sh"
        ],
        "commandToExecute": "sh hello.sh"
      }
    }
}
```

**Példa 2** – védett konfigurációs parancs végrehajtása.

```json
{
  "name": "config-app",
  "type": "extensions",
  "location": "[resourceGroup().location]",
  "apiVersion": "2015-06-15",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', concat(variables('vmName'),copyindex()))]"
  ],
  "tags": {
    "displayName": "config-app"
  },
  "properties": {
    "publisher": "Microsoft.Azure.Extensions",
    "type": "CustomScript",
    "typeHandlerVersion": "2.0",
    "autoUpgradeMinorVersion": true,
    "settings": {
      "fileUris": [
        "https://gist.github.com/ahmetalpbalkan/b5d4a856fe15464015ae87d5587a4439/raw/466f5c30507c990a4d5a2f5c79f901fa89a80841/hello.sh
      ]              
    },
    "protectedSettings": {
      "commandToExecute": "sh hello.sh <password>"
    }
  }
}
```

Lásd: a .net Core zenét áruházból bemutató egy kész példa - [Zenét áruházból bemutatása](https://github.com/neilpeterson/nepeters-azure-templates/tree/master/dotnet-core-music-linux-vm-sql-db).

## <a name="troubleshooting"></a>Hibaelhárítás

Az egyéni parancsfájl bővítmény futtatásakor a parancsfájl a létrehozott vagy letöltött könyvtárában a következőhöz hasonló be. A parancs is menti be ezt a címtárat a `stdout` és `stderr` fájlt.

```none
/var/lib/azure/custom-script/download/0/
```

Az Azure parancsfájl bővítmény itt naplót hoz létre.

```none
/var/log/azure/customscript/handler.log
```

Az egyéni parancsfájl bővítmény végrehajtási állapota is tartalmazó az Azure CLI tudja visszaszerezni.

```none
azure vm extension get <resource-group> <vm-name>
```

A kimenet néz ki a következő szöveggel:

```none
info:    Executing command vm extension get
+ Looking up the VM "scripttst001"
data:    Publisher                   Name                                      Version  State
data:    --------------------------  ----------------------------------------  -------  ---------
data:    Microsoft.Azure.Extensions  CustomScript                              2.0      Succeeded
data:    Microsoft.OSTCExtensions    Microsoft.Insights.VMDiagnosticsSettings  2.3      Succeeded
info:    vm extension get command OK
```

## <a name="next-steps"></a>Következő lépések

Más virtuális parancsfájl bővítmények olvashat a [Linux az Azure parancsfájl bővítmény áttekintése](./virtual-machines-linux-extensions-features.md)című témakörben találhat.