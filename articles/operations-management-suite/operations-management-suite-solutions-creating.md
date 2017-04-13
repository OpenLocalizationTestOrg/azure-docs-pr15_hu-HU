<properties
   pageTitle="Megoldások kezelése műveletek Management csomagja (MOBILE) |} Microsoft Azure"
   description="Megoldások kezelése bővítése műveletek Management csomagja (MOBILE) megadásával ügyfelek vehet fel a MOBILE munkaterület csomagolt management forgatókönyvet.  Ez a cikk részletesen hogyan használható a saját környezetben management megoldások hozhat létre, vagy ügyfelei számára hozzáférhetővé."
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
   ms.date="10/27/2016"
   ms.author="bwren" />

# <a name="creating-management-solutions-in-operations-management-suite-oms-preview"></a>Megoldások kezelése műveletek Management csomagja (MOBILE) (előzetes verzió)

>[AZURE.NOTE]Az előzetes dokumentáció management megoldások létrehozásához a MOBILE, amely jelenleg előzetes verzióban. Bármely leírása alább séma van változhatnak.  

Megoldások kezelése bővítése műveletek Management csomagja (MOBILE) megadásával ügyfelek vehet fel a MOBILE munkaterület csomagolt management forgatókönyvet.  Ez a cikk részletesen megoldások saját kezelése, hogy a saját környezetben használja, vagy a Közösség – ügyfelek számára elérhetővé.

## <a name="planning-your-management-solution"></a>A kezelési megoldás tervezése
Egy adott eset támogató több erőforrások belefoglalása MOBILE megoldások kezelése  A megoldás tervezésekor kell koncentrálhat az alkalmazási példát, amely szeretné elérése és támogatja ezt az összes szükséges források.  Minden megoldást kell önkiszolgáló szereplő és igényel, hogy az egyes erőforrások határozza meg, még akkor is, ha egy vagy több erőforrások is határozzák meg más megoldást.  Kezelési megoldás telepítése után az egyes erőforrások jön létre, kivéve, ha már létezik, és megadhatja, hogy az erőforrásokhoz mi történik, ha a megoldás törlődik.  

Kezelési megoldás például egy [Azure automatizálási runbook](../automation/automation-intro.md) által gyűjtött adatokat a napló Analytics tárházba az [Ütemterv](../automation/automation-schedules.md) és a [Nézet](../log-analytics/log-analytics-view-designer.md) , amely az egyes megjelenítések gyűjtött adatok tartalmazhatnak olyan.  Az azonos ütemtervet egy másik megoldás tapasztalhatott.  A kezelési megoldás szerzőjeként volna meghatározhatja az összes három erőforrásokat, de adja meg, hogy a runbook és a Megjelenítés kell automatikusan törlődjenek a megoldást eltávolításakor.    Volna definiálása az ütemtervet, de megadhatja, hogy meg kell a helyén maradjon, ha a megoldás eltávolította abban az esetben, ha még használatban volt a másik megoldást.

## <a name="management-solution-files"></a>Kezelési megoldás fájlok
[Erőforrás-kezelés](../resource-manager-template-walkthrough.md)sablonként Management megoldások végrehajtását.  Útmutató a tartalom-előállítás megoldások kezelése a fő feladat van tanulási hogyan [sablon](../resource-group-authoring-templates.md)szerkesztése.  Ebben a cikkben a megoldások és a szokásos megoldások erőforrások definiálásáról használható sablonok egyedi részleteit.

Kezelési megoldás fájl alapszerkezetét megegyezik egy [Erőforrás-kezelő sablont](resource-group-authoring-templates.md#template-format) , amely az alábbiak szerint.  Az alábbi szakaszok ismerteti, hogy a legfelső szintű elemeket és és a megoldás a tartalmukat.  

    {
       "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
       "contentVersion": "",
       "parameters": {  },
       "variables": {  },
       "resources": [  ],
       "outputs": {  }
    }

## <a name="parameters"></a>Paraméterek

[Paraméterek](../resource-group-authoring-templates.md#parameters) értékeket van szüksége a felhasználótól való telepítésekor a projektmenedzsment megoldás.  Szabványos paraméterek összes megoldások lesz, és felvehet további paramétereket szükség szerint a adott megoldás.  Hogyan felhasználók fog szolgálni paraméterértékeket való telepítésekor a megoldás az adott paraméter és a megoldás telepítési módja függ.


Amikor a felhasználó a kezelési megoldás az [Azure Marketplace szolgáltatásból](operations-management-suite-solutions.md#finding-and-installing-solutions) vagy az [Azure quickstart útmutató sablonok](operations-management-suite-solutions.md#finding-and-installing-solutions) keresztül telepíti őket egy [MOBILE munkaterület és automatizálási fiók](operations-management-suite-solutions-creating.md#oms-workspace-and-automation-account)megadását kéri.  Ezeket az értékeket az egyes a szabványos paraméterek kitöltéséhez használt.  A felhasználó nem kéri a közvetlenül a szükséges értékeket a szabványos paraméterek, de értékek nyújt további paramétereket kérni.

Amikor a felhasználó a megoldás [egy másik módszert](operations-management-suite-solutions.md#finding-and-installing-solutions)telepíti, az összes normál és minden további paraméterei kell adnia egy értéket.

Az alábbiakban látható minta paraméter.

    "Daily Start Time": {
        "type": "string",
        "metadata": {
            "description": "Enter time for starting VMs by resource group.",
            "control": "datetime",
            "category": "Schedule"
        }

Az alábbi táblázat ismerteti a paraméter tulajdonságainak.

| Attribútum | Leírás |
|:--|:--|
| típus        | A paraméterek adattípusát. A felhasználó számára megjelenik a beviteli vezérlő adatok típusától függ.<br><br>logikai – legördülő listából<br>karakterlánc - szövegdoboz<br>int - szövegdoboz<br>SecureString - jelszó mező<br> |
| kategória    | A paraméter választható kategóriát.  Paraméterek ugyanabba a kategóriába sorolhatók. |
| vezérlő     | Karakterlánc paraméterekkel további funkciókat.<br><br>datetime - Datetime vezérlőelem jelenik meg.<br>globálisan egyedi azonosítója - globálisan egyedi azonosítója érték automatikusan létrejön, és nem jelenik meg a paramétert. |
| Leírás | A paraméter esetleg adjon egy leírást.  Az információk buborék mellett a paraméter jelenik meg. |


### <a name="standard-parameters"></a>Szabványos paraméterek
Az alábbi táblázat a szabványos paramétereket összes kezelési megoldás.  Ezeket az értékeket a program kéri a szükséges őket, ha a megoldás telepítve van a Microsoft Azure piactéren vagy quickstart útmutató sablonok helyett a felhasználó kitölti.  A felhasználó értékeket kell adnia őket, ha a megoldás telepítve van egy másik módszerrel.

>[AZURE.NOTE]A felhasználói felület a Microsoft Azure piactéren és quickstart útmutató sablonok vár a paraméterek neve a táblázatban.  Ha másik paraméterek neve a felhasználó fogja kérni fogja őket, és ezek nem automatikusan tölti fel.


| Paraméter | Típus | Leírás |
|:--|:--|:--|
| Fióknév       | karakterlánc |  Azure automatizálási fiók nevére. |
| pricingTier       | karakterlánc | Log Analytics-munkaterület és a fiók Azure automatizálási árak réteg. |
| regionId          | karakterlánc | Régió az Azure automatizálási fiók. |
| megoldás neve      | karakterlánc | A megoldás neve. |
| workspaceName     | karakterlánc | Jelentkezzen be az analitikai munkaterület neve. |
| workspaceRegionId | karakterlánc | A napló Analytics-munkaterület régió. |





### <a name="sample"></a>Minta
Az alábbiakban a minta paraméter entitás megoldás.  Ide tartoznak a összes a szokásos és a két további paraméterei azonos kategóriájában.

    "parameters": {
        "workspaceName": {
            "type": "string",
            "metadata": {
                "description": "A valid Log Analytics workspace name"
            }
        },
        "accountName": {
            "type": "string",
            "metadata": {
                "description": "A valid Azure Automation account name"
            }
        },
        "workspaceRegionId": {
            "type": "string",
            "metadata": {
                "description": "Region of the Log Analytics workspace"
            }
        },
        "regionId": {
            "type": "string",
            "metadata": {
                "description": "Region of the Azure Automation account"
            }
        },
        "pricingTier": {
            "type": "string",
            "metadata": {
                "description": "Pricing tier of both Log Analytics workspace and Azure Automation account"
            }
        },
        "jobIdGuid": {
        "type": "string",
            "metadata": {
                "description": "GUID for a runbook job",
                "control": "guid",
                "category": "Schedule"
            }
        },
        "startTime": {
            "type": "string",
            "metadata": {
                "description": "Time for starting the runbook.",
                "control": "datetime",
                "category": "Schedule"
            }
        }


Hivatkozott paraméterértékeket az egyéb elemek a megoldás a szintaxis **paraméterek (paraméter neve)**.  Például a munkaterület neve eléréséhez használja **parameters('workspaceName')** 

## <a name="variables"></a>Változók

A **változók** elem tartalmazza a használni kívánt értékeket a többi a projektmenedzsment megoldás.  Ezeket az értékeket nem érhetők el a felhasználó telepíti a megoldást.  A szerző biztosítson egy központi helyen, ahol azok kezelése, előfordulhat, hogy felhasználható többször a megoldás értékei szolgálnak. Kell helyezi el az értékeket adott meg a változók és nem a hardcoding őket a **források** elem a megoldást.  Ez a kód olvashatóbbá teszi, és lehetővé teszi, hogy könnyen módosíthatja ezeket az értékeket újabb verzióiban.

Következő képen megoldások használt tipikus paraméterekkel **változók** elem.

    "variables": { 
        "SolutionVersion": "1.1", 
        "SolutionPublisher": "Contoso", 
        "SolutionName": "My Solution",
        "LogAnalyticsApiVersion": "2015-11-01-preview",
        "AutomationApiVersion": "2015-10-31"
    },

Hivatkozott változóértékkel keresztül az alábbi **változók ("változó neve")**a megoldást.  Ha például a megoldás neve változó eléréséhez használja **variables('solutionName')** 


## <a name="resources"></a>Erőforrások

A **források** elem határozza meg, hogy a különböző erőforrások, a kezelés megoldásban.  Ez a sablon a legnagyobb és a bonyolult része lesz.  Az alábbi szerkezet az erőforrások határozzák meg.  

    "resources": [
        {
            "name": "<name-of-the-resource>",           
            "apiVersion": "<api-version-of-resource>",
            "type": "<resource-provider-namespace/resource-type-name>",     
            "location": "<location-of-resource>",
            "tags": "<name-value-pairs-for-resource-tagging>",
            "comments": "<your-reference-notes>",
            "dependsOn": [
                "<array-of-related-resource-names>"
            ],
            "properties": "<unique-settings-for-the-resource>",
            "resources": [
                "<array-of-child-resources>"
            ]
        }
    ]

### <a name="dependencies"></a>Függőségek
A **dependsOn** elemek meghatározza, hogy egy másik erőforrás [függőség](../resource-group-define-dependencies.md) .  Ha a megoldás telepítve van, egy erőforrás nem jön létre, mindaddig, amíg az összes függőségét létrehozott.  Ha például a megoldás Előfordulhat, hogy [egy runbook indítása](operations-management-suite-solutions-resources-automation.md#runbooks) telepítéskor [feldolgozás erőforrás](operations-management-suite-solutions-resources-automation.md#automation-jobs)használatával.  A feladat erőforrás runbook erőforrást győződjön meg arról, hogy a runbook jön létre, mielőtt a feladat jön létre függő lenne.

### <a name="oms-workspace-and-automation-account"></a>MOBILE munkaterület és automatizálási fiók
Egy [MOBILE munkaterület](../log-analytics/log-analytics-manage-access.md) tartalmazzák, nézetek és [automatizálási fiókot](../automation/automation-security-overview.md#automation-account-overview) , amely tartalmazhat runbooks és a kapcsolódó források kezelése megoldáshoz.  Ezek érhető el kell, mielőtt a megoldást az erőforrások jönnek létre, és nem kell meghatározni a megoldást magát.  A felhasználó automatikusan, [Adja meg a munkaterületet és a fiók](operations-management-suite-solutions.md#oms-workspace-and-automation-account) a megoldást üzembe helyez, őket, de a szerzőjeként, vegye figyelembe az alábbiakat.


## <a name="solution-resource"></a>Megoldás erőforrás
Minden megoldást szükséges a **források** elem, amely meghatározza a megoldást magát egy erőforrás bejegyzést.  Ez egy **Microsoft.OperationsManagement/solutions**típusú lesz.  Következő képen az erőforrás-megoldás.  A különböző elemek az alábbi szakaszok ismertetik.

    "name": "[concat(variables('SolutionName'), '[ ' ,parameters('workspacename'), ' ]')]",
    "location": "[parameters('workspaceRegionId')]",
    "tags": { },
    "type": "Microsoft.OperationsManagement/solutions",
    "apiVersion": "[variables('LogAnalyticsApiVersion')]",
    "dependsOn": [
        "[concat('Microsoft.Automation/automationAccounts/', parameters('accountName'), '/runbooks/', variables('RunbookName'))]",
        "[concat('Microsoft.Automation/automationAccounts/', parameters('accountName'), '/schedules/', variables('ScheduleName'))]",
        "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'), '/views/', variables('ViewName'))]"
    ]
    "properties": {
        "workspaceResourceId": "[concat(resourceGroup().id, '/providers/Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'))]",
        "referencedResources": [
            "[concat('Microsoft.Automation/automationAccounts/', parameters('accountName'), '/schedules/', variables('ScheduleName'))]"
        ],
        "containedResources": [
            "[concat('Microsoft.Automation/automationAccounts/', parameters('accountName'), '/runbooks/', variables('RunbookName'))]",
            "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'), '/views/', variables('ViewName'))]"
        ]
    },
    "plan": {
        "name": "[concat(variables('SolutionName'), '[' ,parameters('workspacename'), ']')]",
        "Version": "[variables('SolutionVersion')]",
        "product": "AzureSQLAnalyticSolution",
        "publisher": "[variables('SolutionPublisher')]",
        "promotionCode": ""
    }

### <a name="solution-name"></a>Megoldás neve
A megoldás nevét az Azure előfizetés egyedinek kell lennie. A javasolt használandó értéke az alábbi.  Ez az a változó **megoldás neve** az alap nevét és a **workspaceName** paraméter neve kattintva győződjön meg róla, hogy a név egyedi használ.

    [concat(variables('SolutionName'), ' [' ,parameters('workspaceName'), ']')]

Ez oldja meg a következőhöz hasonló névre.

    My Solution Name [MyWorkspace]
 

### <a name="dependencies"></a>Függőségek
A megoldás erőforrás kell rendelkeznie [függőség](../resource-group-define-dependencies.md) minden más erőforrás a megoldás létezik, mielőtt a megoldás létrehozható szükségük óta.  Ehhez az egyes erőforrásokhoz bejegyzés hozzáadása a az **dependsOn** elemet.


### <a name="properties"></a>Tulajdonságok
A megoldás erőforrás az alábbi táblázatban az tulajdonságokat tartalmaz.  Ide tartoznak a források hivatkozott és a megoldást, amely meghatározza, hogy miként kezeli az erőforrás a megoldás telepítése után tartalmazza.  A megoldást az egyes erőforrásokhoz szerepelnie kell a **referencedResources** vagy a **containedResources** tulajdonságot.

| A tulajdonság | Leírás |
|:--|:--|
| workspaceResourceId | A képernyőn a MOBILE munkaterület azonosító * <Resource Group ID>/providers/Microsoft.OperationalInsights/workspaces/\<munkaterület neve\>*. |
| referencedResources   | A megoldást, amely nem lehet eltávolítani, ha a megoldás törlődik az erőforrások listája. |
| containedResources   | A megoldás, hogy meg kell szüntetni a megoldás eltávolításakor erőforrások listája. |

A fenti példa egy runbook, ütemezés és nézet megoldást szolgál.  Az ütemezés és runbook is *hivatkozott* elem **tulajdonságainak** , így azok nem törlődnek a megoldást törlődik.  A nézet nem *tartalmaz* , a megoldást eltávolításakor eltűnnek.


### <a name="plan"></a>Terv
A **terv** entitás a megoldás erőforrás az alábbi táblázatban az tulajdonságokat tartalmaz. 

| A tulajdonság | Leírás |
|:--|:--|
| név          | A megoldás neve. |
| verzió       | A megoldás a szerző által meghatározott verziója. |
| termék       | Egyedi karakterlánc azonosítja a megoldást. |
| a Publisher     | A Publisher a megoldást. |


## <a name="other-resources"></a>Egyéb források
A részletek és információforrások, amelyek megegyeznek a következő cikkekben management megoldások mintát is elérheti.

- [Nézetek és irányítópultok](operations-management-suite-solutions-resources-views.md)
- [Automatizálási erőforrások](operations-management-suite-solutions-resources-automation.md)

## <a name="testing-a-management-solution"></a>Kezelési megoldás tesztelése
Előtt, hogy a kezelés megoldást üzembe helyezné, célszerű tesztelése [Próba-AzureRmResourceGroupDeployment](../resource-group-template-deploy.md#deploy-with-powershell)használatával.  Ez az érvényesítés, a megoldásfájlt és a Súgó azonosította esetleges problémákat, mielőtt megnyitná azokat.



## <a name="next-steps"></a>Következő lépések

- További tudnivalók a [szerzői Azure erőforrás-kezelő sablonok](../resource-group-authoring-templates.md)részleteit.
- Keresés [Azure quickstart útmutató sablonok](https://azure.microsoft.com/documentation/templates) minták Megnyitás más erőforrás-kezelő sablonokat.
- A [nézetek kezelése megoldássá hozzáadása](operations-management-suite-solutions-resources-views.md)részleteinek megtekintése.
- [Automatizálási erőforrások kezelése megoldássá](operations-management-suite-solutions-resources-automation.md)hozzáadásának részleteinek megtekintése.
 