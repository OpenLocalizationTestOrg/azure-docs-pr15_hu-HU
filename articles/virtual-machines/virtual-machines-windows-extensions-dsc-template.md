<properties
   pageTitle="Állapot konfigurációs erőforrás-kezelő sablon kívánt |} Microsoft Azure"
   description="Erőforrás-kezelő sablon definícióját állam konfiguráció szükséges az Azure példákkal és hibaelhárítási"
   services="virtual-machines-windows"
   documentationCenter=""
   authors="zjalexander"
   manager="timlt"
   editor=""
   tags="azure-service-management,azure-resource-manager"
   keywords=""/>

<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="na"
   ms.date="09/15/2016"
   ms.author="zachal"/>

# <a name="windows-vmss-and-desired-state-configuration-with-azure-resource-manager-templates"></a>A Windows VMSS és állapot konfiguráció szükséges Azure erőforrás-kezelő sablonokkal
Ez a cikk ismerteti az erőforrás-kezelő sablon, az [állapot konfigurációs kívánt bővítmény kezelő](virtual-machines-windows-extensions-dsc-overview.md). 

## <a name="template-example-for-a-windows-vm"></a>A Windows virtuális sablon példa

A következő kódtöredékének mutat, az erőforrás szakaszba sablon.

```json
            "name": "Microsoft.Powershell.DSC",
            "type": "extensions",
             "location": "[resourceGroup().location]",
             "apiVersion": "2015-06-15",
             "dependsOn": [
                  "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
              ],
              "properties": {
                  "publisher": "Microsoft.Powershell",
                  "type": "DSC",
                  "typeHandlerVersion": "2.20",
                  "autoUpgradeMinorVersion": true,
                  "forceUpdateTag": "[parameters('dscExtensionUpdateTagVersion')]",
                  "settings": {
                      "configuration": {
                          "url": "[concat(parameters('_artifactsLocation'), '/', variables('dscExtensionArchiveFolder'), '/', variables('dscExtensionArchiveFileName'))]",
                          "script": "dscExtension.ps1",
                          "function": "Main"
                      },
                      "configurationArguments": {
                          "nodeName": "[variables('vmName')]"
                      }
                  },
                  "protectedSettings": {
                      "configurationUrlSasToken": "[parameters('_artifactsLocationSasToken')]"
                  }

```

## <a name="template-example-for-windows-vmss"></a>A Windows VMSS sablon példa

Egy VMSS csomópont "Tulajdonságok" szakasz a "VirtualMachineProfile", "extensionProfile" attribútumot tartalmazó tartalmaz. DSC megjelenik a "bővítmények". 

```json
"extensionProfile": {
            "extensions": [
                {
                    "name": "Microsoft.Powershell.DSC",
                    "properties": {
                        "publisher": "Microsoft.Powershell",
                        "type": "DSC",
                        "typeHandlerVersion": "2.20",
                        "autoUpgradeMinorVersion": true,
                        "forceUpdateTag": "[parameters('DscExtensionUpdateTagVersion')]",
                        "settings": {
                            "configuration": {
                                "url": "[concat(parameters('_artifactsLocation'), '/', variables('DscExtensionArchiveFolder'), '/', variables('DscExtensionArchiveFileName'))]",
                                "script": "DscExtension.ps1",
                                "function": "Main"
                            },
                            "configurationArguments": {
                                "nodeName": "localhost"
                            }
                        },
                        "protectedSettings": {
                            "configurationUrlSasToken": "[parameters('_artifactsLocationSasToken')]"
                        }
                    }
                }
            ]
```

## <a name="detailed-settings-information"></a>A beállítások részletes információkat

A következő séma nem egy erőforrás-kezelő Azure-sablon Azure DSC bővítmény beállítások része.

```json

"settings": {
  "wmfVersion": "latest",
  "configuration": {
    "url": "http://validURLToConfigLocation",
    "script": "ConfigurationScript.ps1",
    "function": "ConfigurationFunction"
  },
  "configurationArguments": {
    "argument1": "Value1",
    "argument2": "Value2"
  },
  "configurationData": {
    "url": "https://foo.psd1"
  },
  "privacy": {
    "dataCollection": "enable"
  },
  "advancedOptions": {
    "downloadMappings": {
      "customWmfLocation": "http://myWMFlocation"
    }
  } 
},
"protectedSettings": {
  "configurationArguments": {
    "parameterOfTypePSCredential1": {
      "userName": "UsernameValue1",
      "password": "PasswordValue1"
    },
    "parameterOfTypePSCredential2": {
      "userName": "UsernameValue2",
      "password": "PasswordValue2"
    }
  },
  "configurationUrlSasToken": "?g!bber1sht0k3n",
  "configurationDataUrlSasToken": "?dataAcC355T0k3N"
}

```

## <a name="details"></a>Részletek
| Tulajdonság neve | Típus | Leírás |
| --- | --- | --- |
| settings.wmfVersion | karakterlánc | Adja meg a Windows Management Framework, amely a virtuális telepíthető verzióját. Az érték legújabb"telepítések WMF legfrissebb verziója. Ez a tulajdonság csak az aktuális lehetséges értékek **"4.0-s", "5.0-s", "5.0PP' és"legkésőbb"**. A lehetséges értékei frissítések fizetnie. Az alapértelmezett érték legújabb".|
| Settings.Configuration.URL | karakterlánc | Itt adhatja meg az URL-címét, ahonnan a DSC konfigurációs zip-fájl letöltése. Ha megadott URL-cím megköveteli a biztonsági jogkivonat eléréséhez, meg kell protectedSettings.configurationUrlSasToken tulajdonságot állítsa az érték a biztonsági jogkivonat. Ez a tulajdonság szükség, ha settings.configuration.script és/vagy settings.configuration.function vannak definiálva. |
| Settings.Configuration.Script | karakterlánc | Adja meg a fájl nevét a parancsfájl, amely tartalmazza a DSC konfigurációs definícióját. Ez a parancsfájl URL-CÍMÉT a configuration.url tulajdonságban meghatározott letöltött zip-fájl gyökérmappájában kell lennie. Ez a tulajdonság szükség, ha settings.configuration.url és/vagy settings.configuration.script vannak definiálva. |
| Settings.Configuration.Function | karakterlánc | A DSC konfigurációs nevét adja meg. A konfiguráció nevű tartalmaznia kell a configuration.script által meghatározott parancsfájl. Ez a tulajdonság szükség, ha settings.configuration.url és/vagy settings.configuration.function vannak definiálva. |
| settings.configurationArguments | Webhelycsoport | Határozza meg, hogy meg szeretné átadni a DSC konfigurációs paramétereket. Ez a tulajdonság nincs titkosítva. |
| settings.configurationData.url | karakterlánc | Itt adhatja meg az URL-címe, amelyhez a konfigurációs adatok (.pds1) fájl letöltése a DSC konfigurációs használja. Ha megadott URL-cím megköveteli a biztonsági jogkivonat eléréséhez, meg kell protectedSettings.configurationDataUrlSasToken tulajdonságot állítsa az érték a biztonsági jogkivonat.|
| settings.privacy.dataEnabled | karakterlánc | Engedélyezi vagy tiltja telemetriai webhelycsoport. Ezt a tulajdonságot csak lehetséges értékei **"Engedélyezése", "Letiltva", ", vagy $null**. A tulajdonság értékét hagyja, üres vagy null lehetővé teszi, hogy telemetriai. Az alapértelmezett érték ". [További információ](https://blogs.msdn.microsoft.com/powershell/2016/02/02/azure-dsc-extension-data-collection-2/) |
| settings.advancedOptions.downloadMappings | Webhelycsoport | Határozza meg, amelyből töltse le a WMF más helyekre. [További információ](http://blogs.msdn.com/b/powershell/archive/2015/10/21/azure-dsc-extension-2-2-amp-how-to-map-downloads-of-the-extension-dependencies-to-your-own-location.aspx) |
| protectedSettings.configurationArguments | Webhelycsoport | Határozza meg, hogy meg szeretné átadni a DSC konfigurációs paramétereket. Ez a tulajdonság titkosítva van. |
| protectedSettings.configurationUrlSasToken | karakterlánc | Adja meg a Társítások jogkivonat az URL-cím configuration.url által meghatározott eléréséhez. Ez a tulajdonság titkosítva van. |
| protectedSettings.configurationDataUrlSasToken | karakterlánc | Adja meg a Társítások jogkivonat az URL-cím configurationData.url által meghatározott eléréséhez. Ez a tulajdonság titkosítva van. |

## <a name="settings-vs-protectedsettings"></a>Beállítások és összehasonlítása ProtectedSettings
Minden beállítás a beállítások szövegfájlba a virtuális menti.
A "beállítások" olyan nyilvános tulajdonságok, mert nincsenek a beállítások szövegfájl titkosítva.
"ProtectedSettings" csoportban a tulajdonságok egy tanúsítvánnyal titkosított, és nem jelennek meg a fájlt a virtuális az egyszerű szöveg.

Ha a konfigurációs hitelesítő adatait, protectedSettings is szerepeltetni:

```json
"protectedSettings": {
    "configurationArguments": {
        "parameterOfTypePSCredential1": {
            "userName": "UsernameValue1",
            "password": "PasswordValue1"
        }
    }
}
```

## <a name="example"></a>Példa

Az alábbi példa a [DSC bővítmény kezelő áttekintése lapon](virtual-machines-windows-extensions-dsc-overview.md)az "Első lépések" szakaszában származik.
Ebben a példában a sablonok erőforrás-kezelő parancsmagok helyett a bővítmény telepítése. Mentse a "IisInstall.ps1" konfigurációt, helyezze el a egy. ZIP-fájlt, és töltse fel a fájlt az elérhető URL-címre. Ez a példa Azure blob-tárolóhoz, de lehet letölteni. Tetszőleges bárhonnan ZIP-fájlok.

Az erőforrás-kezelő Azure sablonba az alábbi kódot utasítja, hogy a virtuális, töltse le a megfelelő fájlt, és futtassa a megfelelő PowerShell függvény:

```json
"settings": {
    "configuration": {
        "url": "https://demo.blob.core.windows.net/",
        "script": "IisInstall.ps1",
        "function": "IISInstall"
    }
    } 
},
"protectedSettings": {
    "configurationUrlSasToken": "odLPL/U1p9lvcnp..."
}
```

## <a name="updating-from-the-previous-format"></a>Az előző formátumának módosítása
A korábbi formátumban (ModulesUrl, ConfigurationFunction, SasToken vagy tulajdonságok nyilvános tulajdonságait tartalmazó) automatikusan beállításaitól alkalmazkodás az aktuális formátumra, és csak ugyanúgy működjenek előtt.

A következő séma milyen az előző beállítások séma hasonlított:

```json
"settings": {
    "WMFVersion": "latest",
    "ModulesUrl": "https://UrlToZipContainingConfigurationScript.ps1.zip",
    "SasToken": "SAS Token if ModulesUrl points to private Azure Blob Storage",
    "ConfigurationFunction": "ConfigurationScript.ps1\\ConfigurationFunction",
    "Properties":  {
        "ParameterToConfigurationFunction1":  "Value1",
        "ParameterToConfigurationFunction2":  "Value2",
        "ParameterOfTypePSCredential1": {
            "UserName": "UsernameValue1",
            "Password": "PrivateSettingsRef:Key1" 
        },
        "ParameterOfTypePSCredential2": {
            "UserName": "UsernameValue2",
            "Password": "PrivateSettingsRef:Key2"
        }
    }
},
"protectedSettings": { 
    "Items": {
        "Key1": "PasswordValue1",
        "Key2": "PasswordValue2"
    },
    "DataBlobUri": "https://UrlToConfigurationDataWithOptionalSasToken.psd1"
}
```

Az alábbiakban hogyan az előző formátum alkalmazkodik az aktuális formátumra:

| Tulajdonság neve | Megfelelője az előző séma |
| --- | --- |
| settings.wmfVersion | beállítások. WMFVersion |
| Settings.Configuration.URL | beállítások. ModulesUrl |
| Settings.Configuration.Script | Beállítások első része. ConfigurationFunction (előtt "\\\\") |
| Settings.Configuration.Function | Második rész beállítást. ConfigurationFunction (miután "\\\\") |
| settings.configurationArguments | beállítások. Tulajdonságok |
| settings.configurationData.url | protectedSettings.DataBlobUri (nélkül Társítások jogkivonat) |
| settings.privacy.dataEnabled | beállítások. Privacy.DataEnabled |
| settings.advancedOptions.downloadMappings | beállítások. AdvancedOptions.DownloadMappings |
| protectedSettings.configurationArguments | protectedSettings.Properties |
| protectedSettings.configurationUrlSasToken | beállítások. SasToken |
| protectedSettings.configurationDataUrlSasToken | Biztonsági jogkivonat protectedSettings.DataBlobUri |


## <a name="troubleshooting---error-code-1100"></a>Hibaelhárítás – 1 100 kódszámú hiba jelenik meg
1 100 kódszámú hiba jelenik meg, az azt jelzi, hogy a felhasználó által megadott információkat a DSC bővítmény probléma van.
A hibák szövegét változó és meg is változhat.
Íme néhány a hibák, előfordulhat, hogy problémába, és hogyan háríthatja őket.

### <a name="invalid-values"></a>Érvénytelen értékű
"Privacy.dataCollection is"{0}". A csak lehetséges értékek ","engedélyezése és letiltása"" "WmfVersion is"{0}". Csak lehetséges értékei... és "legújabb" "

Probléma: Nem alkalmazható, egy megadott értéket.

Megoldás: Módosítsa az érvénytelen érték érvényes érték. Lásd: a táblázat adatai csoportban.

### <a name="invalid-url"></a>Érvénytelen URL-címe
"ConfigurationData.url is"{0}". Ez nem egy érvényes URL-cím""DataBlobUri is "{0}". Ez nem egy érvényes URL-cím""Configuration.url is "{0}". Ez nem egy érvényes URL-cím"

Probléma: A megadott URL-je nem érvényes.

Megoldás: Jelölje be az összes a megadott URL-címet. Győződjön meg arról, hogy az összes URL-címét úgy, hogy érvényes helyekre, hogy a bővítmény hozzáférhet-e a távoli számítógépen.

### <a name="invalid-configurationargument-type"></a>Érvénytelen ConfigurationArgument típusa
"Érvénytelen configurationArguments írja be a következőt: {0}"

Probléma: A ConfigurationArguments tulajdonság nem lehet úgy, hogy egy Hashtable objektum. 

Megoldás: Ellenőrizze a ConfigurationArguments tulajdonság egy Hashtable. Kövesse a portáladatbázis előző példában megadott formátumban. Ügyelni kell a ajánlatok, vesszővel és kapcsos zárójeleket.

### <a name="duplicate-configurationarguments"></a>Ismétlődő ConfigurationArguments
"A nyilvános és a védett configurationArguments ismétlődő argumentumokat"{0}"található"

Probléma: A ConfigurationArguments nyilvános beállításai és a védett beállításai ConfigurationArguments tartalmazzák az azonos nevű tulajdonságok.

Megoldás: Távolítsa el az ismétlődő tulajdonságok közül.

### <a name="missing-properties"></a>Hiányzó tulajdonságai
"Configuration.function igényel, hogy configuration.url vagy configuration.module meg van adva"

"Configuration.url igényel, hogy configuration.script van megadva."

"Configuration.script igényel, hogy configuration.url van megadva."

"Configuration.url igényel, hogy configuration.function van megadva."

"ConfigurationUrlSasToken igényel, hogy configuration.url van megadva."

"ConfigurationDataUrlSasToken igényel, hogy configurationData.url van megadva."

Probléma: Egy definiált tulajdonság szüksége van egy másik tulajdonságot, amely nem található.

Megoldás: 
- Adja meg a hiányzó tulajdonságot.
- Távolítsa el a szükséges a hiányzó tulajdonság tulajdonságot.


## <a name="next-steps"></a>Következő lépések
DSC Ismerkedjen meg és virtuális gép skála [Használatáról virtuális gép skála állítja be az Azure DSC kiterjesztéssel](../virtual-machine-scale-sets/virtual-machine-scale-sets-dsc.md) beállítása

A [cég DSC biztonságos hitelesítő adatok kezelése](virtual-machines-windows-extensions-dsc-credentials.md)részletesebb tájékoztatás található. 

További tájékoztatást a az Azure DSC bővítmény kezelő című témakörben [az Azure kívánt állam konfigurációs bővítmény kezelője](virtual-machines-windows-extensions-dsc-overview.md). 

További információt a PowerShell DSC, [Keresse fel a PowerShell dokumentáció központját](https://msdn.microsoft.com/powershell/dsc/overview). 
