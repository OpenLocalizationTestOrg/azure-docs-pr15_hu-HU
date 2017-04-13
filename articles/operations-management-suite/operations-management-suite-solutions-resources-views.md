<properties
   pageTitle="Tevékenységek kezelése csomagja (MOBILE) management megoldások nézetek |} Microsoft Azure"
   description="Adatkezelési megoldások műveletek Management csomagja (MOBILE) fog rendszerint adatok ábrázolásához egy vagy több nézetét.  Ez a cikk ismerteti, hogyan hozta létre a nézet Tervező nézet exportálása és belőlük egy projektmenedzsment megoldás. "
   services="operations-management-suite"
   documentationCenter=""
   authors="bwren"
   manager="jwhit"
   editor="tysonn" />
<tags
   ms.service="operations-management-suite"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/17/2016"
   ms.author="bwren" />

# <a name="views-in-operations-management-suite-oms-management-solutions-preview"></a>Nézetek a tevékenységek kezelése csomagja (MOBILE) management megoldások (előzetes verzió)

>[AZURE.NOTE]Az előzetes dokumentáció management megoldások létrehozásához a MOBILE, amely jelenleg előzetes verzióban. Bármely következőkben séma van változhatnak.    

[Adatkezelési megoldások műveletek Management csomagja (MOBILE)](operations-management-suite-solutions.md) fog rendszerint adatok ábrázolásához egy vagy több nézetét.  Ez a cikk ismerteti a [Tervező nézet](../log-analytics/log-analytics-view-designer.md) által létrehozott nézetek exportálhatók a és a belőlük egy projektmenedzsment megoldás.  

>[AZURE.NOTE]Ez a cikk a minták paramétereket és nem szükséges, vagy közös management megoldások és [létrehozása management megoldások műveletek Management csomagja (MOBILE)](operations-management-suite-solutions-creating.md) ismertetett változók használata 


## <a name="prerequisites"></a>Előfeltételek
Ez a cikk tartalma feltételezi, hogy Ön már ismeri [kezelési megoldás létrehozása](operations-management-suite-solutions-creating.md) és a megoldás fájl felépítésének módját.


## <a name="overview"></a>– Áttekintés

Kezelési megoldás nézet felvételéhez hozza létre **az erőforrás** azt a [megoldásfájlt](operations-management-suite-solutions-creating.md).  A nézet részletes konfigurációs ismerteti, hogy a JSON, általában összetett bár és valami nem, hogy egy tipikus megoldás Szerző létrehozhatnak manuálisan kell.  A leggyakoribb módszer, hogy a [Tervező nézet](../log-analytics/log-analytics-view-designer.md)használata a nézet létrehozása, exportálhatja, annak részletes konfigurációs hozzáadása a megoldáshoz. 

A nézet hozzáadása a megoldást az alapvető lépések a következők:  Az egyes lépések leírása az alábbi szakaszok részletesen.

1. A nézet exportálása fájlba.
2. A nézet erőforrás létrehozása a megoldást.
3. Adja hozzá a részletek.

## <a name="export-the-view-to-a-file"></a>A nézet exportálása fájlba
Kövesse az utasításokat a [Napló Analitikájának megtekintése Tervező](../log-analytics/log-analytics-view-designer.md) nézet exportálása fájlba.  Az exportált fájl JSON formátumban, ugyanazokat az [elemeket a megoldást fájlként](operations-management-suite-solutions-creating.md#management-solution-files)lesz.  

Az **erőforrások** elemet a nézet fájl egy erőforrás egy **Microsoft.OperationalInsights/workspaces** , amely a MOBILE munkaterület típusú lesz.  Ez az elem egy aleleme **nézetek** a nézet jelöl és részletes konfigurációját tartalmaz egy típusú lesz.  Másolja a vágólapra: Ez az elem részleteit, és másolja a megoldás.


## <a name="create-the-view-resource-in-the-solution"></a>A nézet erőforrás a megoldás létrehozása
A következő nézet erőforrás hozzáadása a megoldást fájl az **erőforrások** elemet.  Ez az alábbiakban ismertetjük, hogy akkor is hozzá kell adnia változók használ.  Ne feledje, hogy a **Irányítópult** és **OverviewTile** tulajdonságot, amely felülírja az exportált nézetet fájlból megfelelő tulajdonságok helyőrzők.
 
    {
        "apiVersion": "[variables('LogAnalyticsApiVersion')]",
        "name": "[concat(parameters('workspaceName'), '/', variables('ViewName'))]",
        "type": "Microsoft.OperationalInsights/workspaces/views",
        "location": "[parameters('workspaceregionId')]",
        "id": "[Concat('/subscriptions/', subscription().subscriptionId, '/resourceGroups/', resourceGroup().name, '/providers/Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'),'/views/', variables('ViewName'))]",
        dependson": [
            ],
        "properties": {
            "Id": "[variables('ViewName')]",
            "Name": "[variables('ViewName')]",
            "DisplayName": "[variables('ViewName')]",
            "Description": "",
            "Author": "[variables('ViewAuthor')]",
            "Source": "Local",
            "Dashboard": ,
            "OverviewTile": 
        }
    }

A következő változók hozzáadása a megoldásfájlt [változók](operations-management-suite-solutions-creating.md#variables) eleme, és cserélje ki a azoknak a megoldás.

    "LogAnalyticsApiVersion": "2015-11-01-preview",
    "ViewAuthor": "Your name."
    "ViewDescription": "Optional description of the view."
    "ViewName": "Provide a name for the view here."


Figyelje meg, hogy az erőforrás teljes megtekintése átmásolhatja a exportált nézetet fájlból, de a következő módosításokat az, hogy a megoldás az működjön kimutatásadatokat.  

- A nézet erőforrás **típusa** kell **nézetekből** **Microsoft.OperationalInsights/workspaces**módosítható.
- A **name** tulajdonság az nézet erőforrás kell lehet módosítani a munkaterület nevét.
- A munkaterület függőség kell távolítható el, mivel a munkaterület erőforrás nem definiálja a megoldás.
- **DisplayName** tulajdonság megjelenjen a nézetet az igényeknek megfelelően.  Az **azonosító** **neve**és **DisplayName** összes egyeznie kell.
- Paraméterek neve módosítania kell a megfelelő paramétereket szükséges csoportja.
- Változók kell a megoldást definiálva, és használja a megfelelő beállításokat.

## <a name="add-the-view-details"></a>A részletek hozzáadása
A nézet erőforrás az exportált nézetet fájlban tartalmazni fogja az **Irányítópult** és **OverviewTile** nevű **Tulajdonságok** elem két olyan elemeket, amelyek a részletes konfigurációs a nézet tartalmazza.  A két elem és a tartalmukat másolja az erőforrás a megoldást fájl megtekintése a **Tulajdonságok** eleme. 

## <a name="example"></a>Példa
Ha például a következő példában egy egyszerű megoldásfájlt megtekintésre.  Három pontra (…) szóközt okokból **Irányítópult** és **OverviewTile** tartalmát jelennek meg.


    {
        "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "workspaceName": {
                "type": "string"
            },
            "accountName": {
                "type": "string"
            },
            "workspaceRegionId": {
                "type": "string"
            },
            "regionId": {
                "type": "string"
            },
            "pricingTier": {
                "type": "string"
            }
        },
        "variables": {
            "SolutionVersion": "1.1",
            "SolutionPublisher": "Contoso",
            "SolutionName": "Contoso Solution",
            "LogAnalyticsApiVersion": "2015-11-01-preview",
            "ViewAuthor":  "user@contoso.com",
            "ViewDescription":  "This is a sample view.",
            "ViewName":  "Contoso View"
        },
        "resources": [
            {
                "name": "[concat(variables('SolutionName'), '(' ,parameters('workspacename'), ')')]",
                "location": "[parameters('workspaceRegionId')]",
                "tags": { },
                "type": "Microsoft.OperationsManagement/solutions",
                "apiVersion": "[variables('LogAnalyticsApiVersion')]",
                "dependsOn": [
                    "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspacename'), '/views/', variables('ViewName'))]"
                ],
                "properties": {
                    "workspaceResourceId": "[concat(resourceGroup().id, '/providers/Microsoft.OperationalInsights/workspaces/', parameters('workspacename'))]",
                    "referencedResources": [
                    ],
                    "containedResources": [
                        "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'), '/views/', variables('ViewName'))]"
                    ]
                },
                "plan": {
                    "name": "[concat(variables('SolutionName'), '(' ,parameters('workspaceName'), ')')]",
                    "Version": "[variables('SolutionVersion')]",
                    "product": "ContosoSolution",
                    "publisher": "[variables('SolutionPublisher')]",
                    "promotionCode": ""
                }
            },
            {
                "apiVersion": "[variables('LogAnalyticsApiVersion')]",
                "name": "[concat(parameters('workspaceName'), '/', variables('ViewName'))]",
                "type": "Microsoft.OperationalInsights/workspaces/views",
                "location": "[parameters('workspaceregionId')]",
                "id": "[Concat('/subscriptions/', subscription().subscriptionId, '/resourceGroups/', resourceGroup().name, '/providers/Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'),'/views/', variables('ViewName'))]",
                "dependson": [
                ],
                "properties": {
                    "Id": "[variables('ViewName')]",
                    "Name": "[variables('ViewName')]",
                    "DisplayName": "[variables('ViewName')]",
                    "Description": "[variables('ViewDescription')]",
                    "Author": "[variables('ViewAuthor')]",
                    "Source": "Local",
                    "Dashboard": ...,
                    "OverviewTile": ...
                }
            }
        ]
    }




## <a name="next-steps"></a>Következő lépések

- Ismerje meg, hogy minden részlete [management megoldások](operations-management-suite-solutions-creating.md)létrehozása.
- [A kezelési megoldás az automatizálási runbooks](operations-management-suite-solutions-resources-automation.md)tartalmazzák.