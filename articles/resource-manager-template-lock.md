<properties
   pageTitle="Erőforrás-kezelő sablon erőforrás zárolások |} Microsoft Azure"
   description="Az erőforrás-kezelő séma találnak az erőforrás zárolások keresztül sablon látható."
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
   ms.date="10/03/2016"
   ms.author="tomfitz"/>

# <a name="resource-locks-template-schema"></a>Erőforrás zárolások sablon séma

Hoz létre lakat erőforrás és az annak alárendelt erőforrásokat.

## <a name="schema-format"></a>Séma formázása

A sablon a források szakaszában a következő séma hozzáadása lakat létrehozásához.
    
    {
        "type": enum,
        "apiVersion": "2015-01-01",
        "name": string,
        "dependsOn": [ array values ],
        "properties":
        {
            "level": enum,
            "notes": string
        }
    }



## <a name="values"></a>Értékek

Az alábbi táblázat ismerteti a értékeket kell beállítania a sémában.

| név | Szükséges | Leírás |
| ---- | -------- | ----------- |
| típus | igen | Az erőforrás típusa hozhat létre.<br /><br />Az erőforrások:<br />**{névtér} / {} típus/szolgáltatók/zárolás:**<br /><br/>Az erőforrás-csoportok:<br />**Microsoft.Authorization/locks** |
| apiVersion | igen | Az erőforrás létrehozásához használandó API verziószáma.<br /><br />Használata:<br />**2015-01-01**<br /><br /> |
| név | igen | Egy érték, amely meghatározza az erőforrás zárolásához és a zárolás nevét. Legfeljebb 64 karakter is lehet, és nem tartalmazhat <>, %-ot, és,?, vagy bármely vezérlőkarakterek.<br /><br />Az erőforrások:<br />**{resource}/Microsoft.Authorization/{lockname}**<br /><br />Az erőforrás-csoportok:<br />**{lockname}** |
| dependsOn | nem | Erőforrás vesszővel tagolt listáját a nevét vagy az erőforrás egyedi azonosítóját.<br /><br />Az erőforrások függ, hogy a zárolás gyűjteménye. Az erőforrás van zárolása telepítve van az ugyanazt a sablont, terjed ki, hogy az erőforrás neve biztosítja, hogy az erőforrás először telepíti az elemben. | 
| Tulajdonságok | igen | Egy objektum, amely azonosítja a zárolt és a megjegyzéseket fűzhet a zárolás.<br /><br />[Tulajdonságok objektum](#properties-object)látható. |  

### <a name="properties-object"></a>objektum tulajdonságai

| név | Szükséges | Leírás |
| ---- | -------- | ----------- |
| szint   | igen | A hatókör alkalmazása zárolása típusát.<br /><br />**CannotDelete** – a felhasználók, módosíthatja az erőforrás, de nem törli.<br />**Csak olvasható** - felhasználók olvashatják az erőforrás, de azt nem törölheti vagy semmilyen műveletet végre. |
| jegyzetek   | nem | A zárolás leírását. Legfeljebb 512 karakterből állhat. |


## <a name="how-to-use-the-lock-resource"></a>A rögzített erőforrás használata

Ez az erőforrás vesz fel, hogy az erőforrás adott műveleteket a sablont. A zárolás összes felhasználók és csoportok vonatkozik.

Hozhat létre, és zárolásainak kezelése törölhet, rendelkeznie kell **Microsoft.Authorization/** * vagy * *Microsoft.Authorization/locks/* ** műveletek. A beépített szerepkörök, csak **tulajdonos** és a **felhasználói hozzáférés rendszergazda ** adják meg ezeket a műveleteket. Szerepköralapú hozzáférés-vezérlés információkért olvassa el az [Azure szerepköralapú hozzáférés-vezérlés](./active-directory/role-based-access-control-configure.md)című témakört.

A zárolása az adott erőforrás és a gyermek erőforrások érvényes.

Lakat **Eltávolítása-AzureRmResourceLock** PowerShell-parancsot, vagy a [törlési művelet](https://msdn.microsoft.com/library/azure/mt204562.aspx) a REST API eltávolíthatja.

## <a name="examples"></a>Példák

Az alábbi példa a megfelelő web App alkalmazásban nem törlés lakat vonatkozik.

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
                "name": "[variables('siteName')]",
                "type": "Microsoft.Web/sites",
                "location": "[resourceGroup().location]",
                "properties": {
                    "serverFarmId": "[parameters('hostingPlanName')]"
                },
            },
            {
                "type": "Microsoft.Web/sites/providers/locks",
                "apiVersion": "2015-01-01",
                "name": "[concat(variables('siteName'),'/Microsoft.Authorization/MySiteLock')]",
                "dependsOn": [ "[variables('siteName')]" ],
                "properties":
                {
                    "level": "CannotDelete",
                    "notes": "my notes"
                }
             }
        ],
        "outputs": {}
    }

A következő példában az erőforráscsoport nem törlés lakat érinti.

    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {},
        "variables": {},
        "resources": [
            {
                "type": "Microsoft.Authorization/locks",
                "apiVersion": "2015-01-01",
                "name": "MyGroupLock",
                "properties":
                {
                    "level": "CannotDelete",
                    "notes": "my notes"
                }
            }
        ],
        "outputs": {}
    }

## <a name="next-steps"></a>Következő lépések

- A sablon szerkezet kapcsolatos tudnivalókért lásd: a [szerzői Azure erőforrás-kezelő sablonok](resource-group-authoring-templates.md).
- További információt a zárolás témakörökben [zárolása az Azure erőforrás-kezelő](resource-group-lock-resources.md).
