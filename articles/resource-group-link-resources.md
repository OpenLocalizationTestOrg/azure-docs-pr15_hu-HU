<properties 
    pageTitle="Források az Azure erőforrás-kezelő csatolása |} Microsoft Azure" 
    description="Kapcsolódó források az Azure erőforrás-kezelő más erőforrás-csoportok közötti mutató hivatkozás létrehozása" 
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
    ms.date="08/01/2016" 
    ms.author="tomfitz"/>

# <a name="linking-resources-in-azure-resource-manager"></a>Források az Azure erőforrás-kezelő összekapcsolása

A telepítés során jelölheti ki egy erőforrás egy másik erőforrás függ, de adott életciklus telepítési végződik. A telepítés után nincs azonosított kapcsolat függő erőforrások között. Erőforrás-kezelővel egy erőforrás erőforrások között állandó kapcsolatok létrehozására csatolása nevű funkció.

Az erőforrás csatolása, az erőforrás csoportok átterjedő kapcsolatok dokumentálhatja. Például érdemes közös, hogy az adatbázis saját életciklus egy erőforrás csoportban találhatók, és egy másik életciklus az alkalmazás egy másik erőforráscsoport tárolnak. Az alkalmazás csatlakozik az adatbázishoz úgy szeretné megjelölni az alkalmazás és az adatbázis közötti kapcsolat. 

Az összes csatolt erőforrás ugyanabban az előfizetésben kell tartoznia. Az egyes erőforrások más erőforrások: 50 csatolható. A csak a lekérdezés kapcsolódó források módja az REST API-k. A csatolt erőforrásokat törölt vagy áthelyezni, ha a hivatkozás tulajdonosa kell üríteni a hátralévő hivatkozásra. Ön **nem** figyelmeztet, ha egy erőforráshoz csatolt más erőforrások törlése.

## <a name="linking-in-templates"></a>A sablonok összekapcsolása

Hivatkozás meg egy sablont, az erőforrás-szolgáltató névtér kombináló erőforrástípust és az adatforrás erőforrást **/providers/links**típusú tartalmazza. A név tartalmaznia kell a forrás erőforrás nevét. Az erőforrás-azonosító a cél erőforrás megadnia. Az alábbi példában egy olyan webhelyet, és a tárterület-fiók közötti kapcsolat hoz létre.

    {
      "type": "Microsoft.Web/sites/providers/links",
      "apiVersion": "2015-01-01",
      "name": "[concat(variables('siteName'),'/Microsoft.Resources/SiteToStorage')]",
      "dependsOn": [ "[variables('siteName')]" ],
      "properties": {
        "targetId": "[resourceId('Microsoft.Storage/storageAccounts','storagecontoso')]",
        "notes": "This web site uses the storage account to store user information."
      }
    }


Teljes körű leírását a sablon formátumot, az [erőforrás-hivatkozások - sablon séma](resource-manager-template-links.md)láthatók.

## <a name="linking-with-rest-api"></a>Csatolás REST API-val

A telepített erőforrások közötti kapcsolat megadásához futtatása:

    PUT https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/{provider-namespace}/{resource-type}/{resource-name}/providers/Microsoft.Resources/links/{link-name}?api-version={api-version}

{Előfizetés azonosítója} cserélje ki az előfizetés azonosítója. Cseréje {-erőforráscsoport}, {szolgáltató-névtér, {erőforrás-típus}, {erőforrás-név} értékekkel, a hivatkozásban szereplő első erőforrás azonosítására szolgáló. {Hivatkozás neve} cserélje le a hivatkozás létrehozása nevére. Használat 2015-01-01-verzióhoz tartozó api.

A kérelem többek között, amely meghatározza a második erőforrás a hivatkozást objektumként:

    {
        "name": "{new-link-name}",
        "properties": {
            "targetId": "subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/{provider-namespace}/{resource-type}/{resource-name}/",
            "notes": "{link-description}",
        }
    }

A Tulajdonságok elemet a második erőforrás azonosítóját tartalmazza.

Az előfizetés lekérdezheti a hivatkozások:

    https://management.azure.com/subscriptions/{subscription-id}/providers/Microsoft.Resources/links?api-version={api-version}

Ha további példákat, például beolvasni a hivatkozásokat, hogy miként [Csatolt forrásokban](https://msdn.microsoft.com/library/azure/mt238499.aspx)talál.

## <a name="next-steps"></a>Következő lépések

- Az erőforrások címkékkel is rendezheti. Címkézési erőforrások kapcsolatos további tudnivalókért lásd: [az erőforrások rendszerezéséhez használata címkék](resource-group-using-tags.md).
- Sablonok létrehozásával, és az erőforrások telepítendő definiálása leírását olvassa el a [sablonok létrehozása](resource-group-authoring-templates.md)című témakört.
