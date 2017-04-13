<properties 
    pageTitle="Erőforrás-kezelő források zárolása |} Microsoft Azure" 
    description="Felhasználók számára letiltott művelet frissítése vagy bizonyos erőforrások törlése a korlátozás alkalmazza az összes felhasználók és szerepkörök." 
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
    ms.date="08/15/2016" 
    ms.author="tomfitz"/>

# <a name="lock-resources-with-azure-resource-manager"></a>Az Azure erőforrás-kezelő források zárolása

Rendszergazdaként szüksége lehet zárolni egy előfizetés, erőforráscsoport vagy erőforrás meg, hogy más felhasználók a szervezet véletlen törlését vagy kritikus erőforrások módosítása. Beállíthatja, hogy a zárolás szint **CanNotDelete** vagy **csak olvasható**. 

- **CanNotDelete** azt jelenti, hogy jogosult felhasználók továbbra is olvashatja és módosítani az erőforrást, de azok nem törölheti azt. 
- **Csak olvasható** azt jelenti, hogy jogosult felhasználók olvashatják az erőforrás, de azt nem törölheti vagy semmilyen műveletet végre. Az erőforrás a engedéllyel az **olvasó** szerepkör korlátozódik. 

**Csak olvasható** alkalmazása vezethet nem várt eredmények, mert bizonyos műveletek, amelyek úgy tűnik, például a további műveletek ténylegesen megkövetelése a további műveletek. Például úgy, hogy a **csak olvasható** lakat tárterület-fiókjában megakadályozza, hogy minden felhasználó bejegyzésére a billentyűket. A lista billentyűk művelet POST kérelem keresztül kezelni, mert a visszaadott billentyűk érhetők el a írási műveletek. Egy másik például úgy, hogy a **csak olvasható** lakat-szolgáltatási alkalmazás erőforrás megakadályozza, hogy a Visual Studio Server Explorer az erőforrás fájl jelenik meg, mert adott kapcsolati csak írási hozzáféréssel.

Szerepköralapú hozzáférés-vezérlés eltérően használatával zárolásainak kezelése a korlátozás alkalmazása az összes felhasználók és szerepkörök. Engedélyek beállítása a felhasználók és szerepkörök kapcsolatos további tudnivalókért olvassa el az [Azure szerepköralapú hozzáférés-vezérlés](./active-directory/role-based-access-control-configure.md)című témakört.

A szülő hatókör lakat alkalmazásakor az összes alárendelt erőforrás ugyanazt a zárolás örökli. Páros erőforrásokat, amelyeket később ad hozzá a zárolás örökölhet a szülő. Öröklődés a legszigorúbb zárolás elsőbbséget.

## <a name="who-can-create-or-delete-locks-in-your-organization"></a>Ki is létrehozása vagy törlése a szervezet zárolások

Hozhat létre, és zárolásainak kezelése törölhet, rendelkeznie kell a hozzáférést **Microsoft.Authorization/\* ** vagy **Microsoft.Authorization/locks/\* ** műveletek. A beépített szerepkörök csak a **tulajdonos** és a **Felhasználói hozzáférés rendszergazda** megkapja ezeket a műveleteket.

## <a name="creating-a-lock-through-the-portal"></a>A portálon keresztül lakat létrehozása

[AZURE.INCLUDE [resource-manager-lock-resources](../includes/resource-manager-lock-resources.md)]

## <a name="creating-a-lock-in-a-template"></a>Lakat létrehozása sablonból

A következő példa bemutatja egy sablont, amely lakat hoz létre a tárterület-fiókjában. A tárhely a fiókot, amelyen alkalmazni a zárolás paraméterként megadva. A zárolás nevét a zárolás Ez esetben **myLock**a **/Microsoft.Authorization/** az erőforrás nevét és a összefűzésével jön létre.

A megadott típus az erőforrás típusa. Tárhely állítsa be a "Microsoft.Storage/storageaccounts/providers/locks".

    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "lockedResource": {
          "type": "string"
        }
      },
      "resources": [
        {
          "name": "[concat(parameters('lockedResource'), '/Microsoft.Authorization/myLock')]",
          "type": "Microsoft.Storage/storageAccounts/providers/locks",
          "apiVersion": "2015-01-01",
          "properties": {
            "level": "CannotDelete"
          }
        }
      ]
    }

## <a name="creating-a-lock-with-rest-api"></a>Lakat létrehozása REST API-val

A [Zárolásainak kezelése a REST API](https://msdn.microsoft.com/library/azure/mt204563.aspx)-val telepített források zárolása A REST API segítségével létrehozása és törlése a zárolások és meglévő zárolások adatainak beolvasásához.

Futtassa a zárolás létrehozásához:

    PUT https://management.azure.com/{scope}/providers/Microsoft.Authorization/locks/{lock-name}?api-version={api-version}

A hatókör lehet egy előfizetés, erőforráscsoport vagy erőforrás. A zárolás-neve kivágni kívánt hívja fel a zárolás. Api-verziót használja **a Skype 2015-01-01**.

A kérelmet írja be a JSON-objektum, amely meghatározza az a tulajdonságokat a zárolás.

    {
      "properties": {
        "level": "CanNotDelete",
        "notes": "Optional text notes."
      }
    } 

Példák olvassa el a [REST API-zárolásainak kezelése](https://msdn.microsoft.com/library/azure/mt204563.aspx)című témakört.

## <a name="creating-a-lock-with-azure-powershell"></a>Lakat létrehozása az Azure PowerShell

Azure PowerShell telepített erőforrások zárolja a **New-AzureRmResourceLock** használ, az alábbi példában látható módon.

    New-AzureRmResourceLock -LockLevel CanNotDelete -LockName LockSite -ResourceName examplesite -ResourceType Microsoft.Web/sites -ResourceGroupName exampleresourcegroup

Azure PowerShell más parancsok biztosít zárolások, például a **Set-AzureRmResourceLock** lakat frissítése és **Törlése – AzureRmResourceLock** lakat törlése működik.

## <a name="next-steps"></a>Következő lépések

- [További információt az erőforrás zárolások használata zárolása lefelé az Azure forrásokban](http://blogs.msdn.com/b/cloud_solution_architect/archive/2015/06/18/lock-down-your-azure-resources.aspx) talál
- Az erőforrások logikailag rendszerezése kapcsolatos további tudnivalókért lásd: a [címkék használata a rendszerezheti az erőforrások](resource-group-using-tags.md)
- Melyik található erőforrás erőforráscsoport módosításához lásd: a [áthelyezése új erőforráscsoport források](resource-group-move-resources.md)
- Szabályok és korlátozásokat át az előfizetés a testre szabott házirendek alkalmazhat. További tudnivalókért lásd: az [Erőforrások kezelésére és a hozzáférés vezérléséhez feltételei](resource-manager-policy.md).
