<properties
   pageTitle="Erőforrás-kezelő sablon erőforrások csatolható |} Microsoft Azure"
   description="Az erőforrás-kezelő séma üzembe helyezése a kapcsolódó források keresztül sablon közötti csatolások látható."
   services="azure-resource-manager"
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

# <a name="resource-links-template-schema"></a>Erőforrás-hivatkozások sablon séma

Kapcsolatot hoz létre két erőforrások között. A hivatkozás egy erőforrás az adatforrás erőforrás neve érvényes. A hivatkozást a második erőforrás van a cél az erőforrás neve.

## <a name="schema-format"></a>Séma formázása

Hozzon létre hivatkozást, a sablon a források szakaszában a következő séma hozzá.
    
    {
        "type": enum,
        "apiVersion": "2015-01-01",
        "name": string,
        "dependsOn": [ array values ],
        "properties":
        {
            "targetId": string,
            "notes": string
        }
    }



## <a name="values"></a>Értékek

Az alábbi táblázat ismerteti a értékeket kell beállítania a sémában.

| név | Érték |
| ---- | ---- |
| típus | Felsorolás<br />Szükséges<br />**{névtér} / {} típus/szolgáltatók/hivatkozások**<br /><br />Az erőforrás típusa hozhat létre. {Névtér} {típusú} értékeket a forrás erőforrás szolgáltató névtér és az erőforrás típusa hivatkozni rájuk. |
| apiVersion | Felsorolás<br />Szükséges<br />**2015-01-01**<br /><br />Az erőforrás létrehozásához használandó API verziószáma. |  
| név | Karakterlánc<br />Szükséges<br />**{resouce}/Microsoft.Resources/{linkname}**<br /> legfeljebb 64 karaktert, és nem tartalmazhat <>, %-ot, és,?, vagy bármely vezérlőkarakterek.<br /><br />Egy adott legfölül forrás erőforrás nevét és egy nevet a hivatkozás céljául érték. |
| dependsOn | Tömb<br />Nem kötelező.<br />Erőforrás vesszővel tagolt listáját a nevét vagy az erőforrás egyedi azonosítóját.<br /><br />A webhelycsoport erőforrások függ, hogy ezt a hivatkozást. Ha az erőforrások csatol telepítik az ugyanazt a sablont, bele ezek az erőforrások nevei: Ez az elem annak érdekében, hogy először rendszerbe. | 
| Tulajdonságok | Objektum<br />Szükséges<br />[objektum tulajdonságai](#properties)<br /><br />Egy objektum, amely azonosítja az erőforrás hivatkozni szeretne, és a megjegyzéseket fűzhet a hivatkozást. |  

<a id="properties" />
### <a name="properties-object"></a>objektum tulajdonságai

| név | Érték |
| ------- | ---- |
| targetId | Karakterlánc<br />Szükséges<br />**{erőforrás-azonosító}**<br /><br />Az azonosítója, a cél erőforrás hivatkozni szeretne. |
| jegyzetek | Karakterlánc<br />Nem kötelező.<br />legfeljebb 512 karakternél<br /><br />A zárolás leírását. |


## <a name="how-to-use-the-link-resource"></a>A hivatkozás erőforrás használata

Alkalmazhat egy hivatkozást az erőforrások esetén a telepítés után is függőség két erőforrások között. Az alkalmazás például egy adatbázist, egy másik erőforráscsoport lehet csatlakozni. Megadhatja a függőséget: hozzon létre hivatkozást a alkalmazásból az adatbázishoz. Hivatkozások lehetővé teszi a dokumentum két erőforrások közötti kapcsolatra. Ön vagy valaki más a szervezet később kérdezheti le egy erőforrás megtudhatja, hogy az erőforrás működése más erőforrások: a hivatkozásokat.

Az összes csatolt erőforrás ugyanabban az előfizetésben kell tartoznia. Az egyes erőforrások más erőforrások: 50 csatolható. A csatolt erőforrásokat törölt vagy áthelyezni, ha a hivatkozás tulajdonosa kell üríteni a hátralévő hivatkozásra.

Hivatkozások többi dolgozni, a [Csatolt forrásokban](https://msdn.microsoft.com/library/azure/mt238499.aspx)talál.

A következő Azure PowerShell paranccsal látható a teljes az előfizetés hivatkozásait. Az eredmények korlátozásához további paramétereket lehet nyújtani.

    Get-AzureRmResource -ResourceType Microsoft.Resources/links -isCollection -ResourceGroupName <YourResourceGroupName>

## <a name="examples"></a>Példák

Az alábbi példa csak olvasható lakat vonatkozik webalkalmazást.

    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "hostingPlanName": {
                "type": "string"
            }
        },
        "variables": {
            "siteName": "[concat('site',uniqueString(resourceGroup().id))]"
        },
        "resources": [
            {
                "apiVersion": "2015-08-01",
                "type": "Microsoft.Web/serverfarms",
                "name": "[parameters('hostingPlanName')]",
                "location": "[resourceGroup().location]",
                "sku": {
                    "tier": "Free",
                    "name": "f1",
                    "capacity": 0
                },
                "properties": {
                    "numberOfWorkers": 1
                }
            },
            {
                "apiVersion": "2015-08-01",
                "name": "[variables('siteName')]",
                "type": "Microsoft.Web/sites",
                "location": "[resourceGroup().location]",
                "dependsOn": [ "[parameters('hostingPlanName')]" ],
                "properties": {
                    "serverFarmId": "[parameters('hostingPlanName')]"
                }
            },
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
        ],
        "outputs": {}
    }

## <a name="quickstart-templates"></a>Quickstart útmutató sablonok

Az alábbi quickstart útmutató sablonok üzembe erőforrások hivatkozással.

- [A felhasználó sorba logika alkalmazással](https://azure.microsoft.com/documentation/templates/201-alert-to-queue-with-logic-app)
- [Értesítés tartalékidő logika alkalmazással](https://azure.microsoft.com/documentation/templates/201-alert-to-slack-with-logic-app)
- [Egy meglévő átjáró API alkalmazás kiépítése](https://azure.microsoft.com/documentation/templates/201-api-app-gateway-existing)
- [Az új átjáró API alkalmazás kiépítése](https://azure.microsoft.com/documentation/templates/201-api-app-gateway-new)
- [Sablon használatával logika alkalmazást és az API alkalmazás létrehozása](https://azure.microsoft.com/documentation/templates/201-logic-app-api-app-create)
- [Egy szöveges üzenetet küld, értesítés akkor indul el, ha logika alkalmazással](https://azure.microsoft.com/documentation/templates/201-alert-to-text-message-with-logic-app)


## <a name="next-steps"></a>Következő lépések

- A sablon szerkezet kapcsolatos tudnivalókért lásd: a [szerzői Azure erőforrás-kezelő sablonok](resource-group-authoring-templates.md).
