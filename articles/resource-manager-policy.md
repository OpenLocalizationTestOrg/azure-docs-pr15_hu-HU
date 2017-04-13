<properties
    pageTitle="Azure erőforrás-kezelő házirend |} Microsoft Azure"
    description="Azure erőforrás-kezelő házirend használata, hogy a különböző tartományok, például előfizetést, az erőforrás csoportok vagy az egyes erőforrások megsértése ismerteti."
    services="azure-resource-manager"
    documentationCenter="na"
    authors="ravbhatnagar"
    manager="timlt"
    editor="tysonn"/>

<tags
    ms.service="azure-resource-manager"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="10/25/2016"
    ms.author="gauravbh;tomfitz"/>

# <a name="use-policy-to-manage-resources-and-control-access"></a>Házirend segítségével kezelheti az erőforrásokat, és a hozzáférés szabályozása

Azure erőforrás-kezelő most lehetővé teszi az egyéni házirendek keresztül, a hozzáférés vezérléséhez. A házirend megakadályozhatja, hogy felhasználók a szervezet a megszakítása szabályokat, hogy a szervezet erőforrások kezelésére van szükség. 

A műveletek vagy információforrások, amelyek kifejezetten van megtagadja leíró házirend-definíciók létrehozása Ezeket a kívánt hatókörét, például az előfizetést, erőforráscsoport vagy egy adott erőforrás a házirend-definíciók rendelhet hozzá. 

Ez a cikk a házirend adatdefiníciós nyelvet használó házirendek létrehozása egyszerű felépítésének azt ismertetik. Ezután azt ismertetik, hogyan alkalmazhat ezek a házirendek különböző tartományok elemre.

## <a name="how-is-it-different-from-rbac"></a>Miben különbözik ez RBAC?

Házirend és szerepköralapú hozzáférés-vezérlés között néhány főbb különbségek vannak, de a legfontosabb dolog megértéséhez, hogy házirendek RBAC működik együtt. Házirendek használatához kell hitelesített RBAC keresztül. RBAC, eltérően házirend egy alapértelmezett engedélyezése és explicit megtagadja a rendszer. 

RBAC a különböző hatókör a **felhasználó** által végezhető műveletek koncentrál. Például egy adott felhasználói bekerül a kívánt hatókörét, az erőforráscsoport vonatkozó munkatársi szerepkörök, a felhasználó erőforrás csoporton együtt végezhet módosításokat. 

Házirend **erőforrás** műveletek a különböző hatókörök koncentrál. Például házirendek, megadhatja, hogy a információforrások, amelyek kiépítése vagy korlátozása a helyek, ahol az erőforrásokat kell építenie típusú.

## <a name="common-scenarios"></a>Gyakori alkalmazási területek

Egy leggyakoribb helyzet szükség részlegszintű címkékre visszaterhelési célra. Egy szervezet előfordulhat, hogy szeretné engedélyezni a műveleteket csak esetén a megfelelő költséghely; egyéb esetben azok utasítania. Ezzel a házirenddel segíti őket a megfelelő költséghely a végrehajtott műveletek díjat.

Egy másik leggyakoribb helyzet, hogy a szervezet előfordulhat, hogy szabályozni szeretné a helyek, ahol erőforrások jönnek létre. Vagy az erőforrások való hozzáférés szabályozása azáltal, hogy csak bizonyos típusú erőforrásokat kell építenie szeretnének.

Hasonlóképpen egy szervezet szabályozhatja a szolgáltatás katalógus vagy be a kívánt elnevezési szabályai az erőforrásokat.

Szabályok használata esetén forgatókönyvekben könnyen érhető el.

## <a name="policy-definition-structure"></a>Házirend-definíciós szerkezete

Házirend-definícióhoz JSON alapján hoz létre. Egy vagy több feltételek logikai operátorok, a műveletek meghatározó áll, és effektus tájékoztat arról, mi történik a feltételek teljesülése esetén. A séma [http://schema.management.azure.com/schemas/2015-10-01-preview/policyDefinition.json](http://schema.management.azure.com/schemas/2015-10-01-preview/policyDefinition.json)van közzétéve. 

A házirend alapvetően, a következő elemeket tartalmazza:

**Feltétel/logikai operátorok:** egy sor olyan feltételeket, amelyeket logikai operátorok keresztül kezelhetők.

**Effektus:** mi történik, ha a feltétel teljesül – tiltása vagy a naplózási. Naplózási effektus figyelmeztetés szolgáltatás eseménynaplójának bocsát ki. Például a rendszergazda hozhat létre egy szabályt, amely okoz a naplózási esemény, ha bárki létrehoz egy nagy virtuális. A rendszergazda később áttekintheti a naplókat.

    {
      "if" : {
          <condition> | <logical operator>
      },
      "then" : {
          "effect" : "deny | audit | append"
      }
    }
    
## <a name="policy-evaluation"></a>Házirend-értékelést

Házirendek erőforrások létrehozásakor értékeli ki. Abban az esetben, ha a sablon telepítésének házirendek értékeli ki az egyes erőforrásokhoz a sablonban kibocsátása során. 

> [AZURE.NOTE] Házirend jelenleg nem értékeli erőforrástípus, amelyek nem támogatják a címkék, milyen és helyet, például a Microsoft.Resources/deployments erőforrástípus. Ez a támogatás belekerül később bármikor. Kompatibilitás a korábbi verziókkal kapcsolatos problémák elkerülése érdekében kifejezetten adjon meg típus házirendek létrehozás esetén. Egy címke házirendet, amely nem meghatározása: például minden érvényes. Ebben az esetben a sablon telepítésének meghiúsulhat, ha egy beágyazott erőforrás, amely nem támogatja a címkék, és a telepítési erőforrástípus házirend-értékelést hozzá lett adva. 

## <a name="logical-operators"></a>Logikai operátorok

A támogatott logikai operátorokat, az alábbi együtt a következők:

| Kezelő neve     | Szintaxis         |
| :------------- | :------------- |
| Nem            | "nem": {&lt;feltétel vagy operátorral &gt;}             |
| És           | "allOf": [{&lt;feltétel vagy operátorral &gt;}, {&lt;feltétel vagy operátorral &gt;}] |
| Vagy                         | "anyOf": [{&lt;feltétel vagy operátorral &gt;}, {&lt;feltétel vagy operátorral &gt;}] |

Erőforrás-kezelő lehetővé teszi, hogy adja meg a komplex logikai a házirend beágyazott operátorok keresztül. Ha például megtagadhatja erőforrás létrehozása egy megadott erőforrástípus az adott helyen. Példa beágyazott operátorokat alatt jelenik meg.

## <a name="conditions"></a>Feltételek

A feltétel értéke, hogy egy **mező** , vagy a **forrás** megfelelnek bizonyos feltételeknek. A támogatott feltétel nevek és szintaxis a következők:

| Feltétel neve | Szintaxis                |
| :------------- | :------------- |
| Egyenlő             | "eredménye": "&lt;érték&gt;"               |
| Például                  | "like": "&lt;érték&gt;"                   |
| Tartalmaz          | "tartalmaz": "&lt;érték&gt;"|
| A                        | "a": ["&lt;érték1&gt;","&lt;érték2&gt;"]|
| ContainsKey    | "containsKey": "&lt;keyName&gt;" |
| Létezik     | "van": "&lt;logikai&gt;" |

### <a name="fields"></a>Mezők

Feltételek alkotta mezők és a források használatával. Egy mező az erőforrás-tartalom, az erőforrás szerinti leírására szolgál, hogy a tulajdonságait jelöli. Forrás a kérést, magát jellemzői jelöli. 

Az alábbi mezők és adatforrások támogatottak:

Mezők: **nevét**, **milyen**, **típusa**, **hely**, **címkék**, **címkék.** *, and * *a tulajdonság alias**. 

### <a name="property-aliases"></a>Aliasok tulajdonság 
A tulajdonság alias házirend-definícióban az erőforrás típusa adott tulajdonságait, például beállítások és termékváltozatok eléréséhez használható nevét. A tulajdonságot csak tartalmazó összes API verziója között működik. Aliasok tudja visszaszerezni használatával a REST API-t (Powershell támogatása a jövőben hozzáadódik) alább látható módon:

    GET /subscriptions/{id}/providers?$expand=resourceTypes/aliases&api-version=2015-11-01
    
Az alias meghatározását alább látható. Amint látható, alias határozza meg, elérési utak API különböző verzióiban, akkor is, ha a tulajdonság neve módosítása. 

    "aliases": [
        {
          "name": "Microsoft.Storage/storageAccounts/sku.name",
          "paths": [
            {
              "path": "properties.accountType",
              "apiVersions": [
                "2015-06-15",
                "2015-05-01-preview"
              ]
            },
            {
              "path": "sku.name",
              "apiVersions": [
                "2016-01-01"
              ]
            }
          ]
        }
    ]

A támogatott aliasok jelenleg meg:

| Alias neve | Leírás |
| ---------- | ----------- |
| {resourceType}/sku.name | Támogatott az erőforrás típusa: Microsoft.Compute/virtualMachines,<br />Microsoft.Storage/storageAccounts,<br />Microsoft.Web/serverFarms,<br /> Microsoft.Scheduler/jobcollections,<br />Microsoft.DocumentDB/databaseAccounts,<br />Microsoft.Cache/Redis,<br />Microsoft.CDN/profiles |
| {resourceType}/sku.family | Támogatott erőforrástípus Microsoft.Cache/Redis |
| {resourceType}/sku.capacity | Támogatott erőforrástípus Microsoft.Cache/Redis |
| Microsoft.Compute/virtualMachines/imagePublisher |  |
| Microsoft.Compute/virtualMachines/imageOffer  |  |
| Microsoft.Compute/virtualMachines/imageSku  |  |
| Microsoft.Compute/virtualMachines/imageVersion  |  |
| Microsoft.Cache/Redis/enableNonSslPort |  |
| Microsoft.Cache/Redis/shardCount |  |
| Microsoft.SQL/servers/version |  |
| Microsoft.SQL/servers/databases/requestedServiceObjectiveId |  |
| Microsoft.SQL/servers/databases/requestedServiceObjectiveName |  |
| Microsoft.SQL/servers/databases/edition |  |
| Microsoft.SQL/servers/databases/elasticPoolName |  |
| Microsoft.SQL/servers/elasticPools/dtu |  |
| Microsoft.SQL/servers/elasticPools/edition |  |

Jelenleg házirend csak akkor működik a helyezése kérések. 

## <a name="effect"></a>Hatás
Házirend háromféle effektus – **megtagadja** **naplózási**, és a **Hozzáfűzés**támogatja. 

- Megtagadja létrehoz egy eseményt a naplóban, és nem sikerül a kérelmet
- Naplózási esemény napló készít, de nem sikerül a kérelmet
- Hozzáfűző mezők meghatározott hozzáadja a kérelmet 

A **hozzáfűző**meg kell adnia az alábbiakat:

    ....
    "effect": "append",
    "details": [
      {
        "field": "field name",
        "value": "value of the field"
      }
    ]

Az érték lehet egy karakterlánc vagy objektum egy JSON formázása. 

## <a name="policy-definition-examples"></a>Házirend-definíciós példák

Most Vegyük szemügyre hogyan azt szabályzat a eléréséhez az előző jelenik meg.

### <a name="chargeback-require-departmental-tags"></a>Visszaterhelési: Csak a részlegszintű címkék

Az alábbi házirend megtagadja nincs telepítve "costCenter" kulcsot tartalmazó címke.

    {
      "if": {
        "not" : {
          "field" : "tags",
          "containsKey" : "costCenter"
        }
      },
      "then" : {
        "effect" : "deny"
      }
    }

A következő házirendet hozzáfűzi costCenter címke előre megadott érték, ha nincs címkék nem bemutató. 

    {
      "if": {
        "field": "tags",
        "exists": "false"
      },
      "then": {
        "effect": "append",
        "details": [
          {
            "field": "tags",
            "value": {"costCenter":"myDepartment" }
          }
        ]
      }
    }
    
A következő házirendet hozzáfűzi costCenter címke előre definiált értéket tartalmazó, nem szerepel a costCenter címke, de más címkéket érhetők el. 

    {
      "if": {
        "allOf": [
          {
            "field": "tags",
            "exists": "true"
          },
          {
            "field": "tags.costCenter",
            "exists": "false"
          }
        ]
    
      },
      "then": {
        "effect": "append",
        "details": [
          {
            "field": "tags.costCenter",
            "value": "myDepartment"
          }
        ]
      }
    }


### <a name="geo-compliance-ensure-resource-locations"></a>GEO megfelelőségi: Győződjön meg arról helyek erőforrás

A következő példa bemutatja egy házirendet, amely megtagadja, ahol hely nincs Észak-Európában vagy nyugati Europe.

    {
      "if" : {
        "not" : {
          "field" : "location",
          "in" : ["northeurope" , "westeurope"]
        }
      },
      "then" : {
        "effect" : "deny"
      }
    }

### <a name="service-curation-select-the-service-catalog"></a>Szolgáltatás Curation: Jelölje be a szolgáltatás katalógus

A következő példa bemutatja egy házirendet, amely lehetővé teszi a műveleteket csak a szolgáltatások típusú Microsoft.Resources/ a\*, Microsoft.Compute/\*, Microsoft.Storage/\*, Microsoft.Network/\* engedélyezettek. Semmi másra van megtagadja.

    {
      "if" : {
        "not" : {
          "anyOf" : [
            {
              "field" : "type",
              "like" : "Microsoft.Resources/*"
            },
            {
              "field" : "type",
              "like" : "Microsoft.Compute/*"
            },
            {
              "field" : "type",
              "like" : "Microsoft.Storage/*"
            },
            {
              "field" : "type",
              "like" : "Microsoft.Network/*"
            }
          ]
        }
      },
      "then" : {
        "effect" : "deny"
      }
    }

### <a name="use-approved-skus"></a>Jóváhagyott termékváltozatok használata

A következő példa bemutatja a tulajdonság alias termékváltozatok korlátozhatja a használatát. A példában szereplő csak Standard_LRS és Standard_GRS jóváhagyott tároló fiókok.

    {
      "if": {
        "allOf": [
          {
            "field": "type",
            "equals": "Microsoft.Storage/storageAccounts"
          },
          {
            "not": {
              "allof": [
                {
                  "field": "Microsoft.Storage/storageAccounts/sku.name",
                  "in": ["Standard_LRS", "Standard_GRS"]
                }
              ]
            }
          }
        ]
      },
      "then": {
        "effect": "deny"
      }
    }
    

### <a name="naming-convention"></a>Elnevezési konvenció

A következő példa bemutatja a helyettesítő karakter, amelyet a feltétel "például" támogat a használatát. A feltétel tájékoztatja, hogy ha a név nem egyezik meg az említett mintát (namePrefix\*nameSuffix) majd utasítania.

    {
      "if" : {
        "not" : {
          "field" : "name",
          "like" : "namePrefix*nameSuffix"
        }
      },
      "then" : {
        "effect" : "deny"
      }
    }
    
### <a name="tag-requirement-just-for-storage-resources"></a>Címke kötelező csak tároló-erőforrások

Az alábbi példa bemutatja, hogyan egymásba csak tároló erőforrás-alkalmazás címke megkövetelésére logikai operátorokat.

    {
        "if": {
            "allOf": [
              {
                "not": {
                  "field": "tags",
                  "containsKey": "application"
                }
              },
              {
                "field": "type",
                "equals": "Microsoft.Storage/storageAccounts"
              }
            ]
        },
        "then": {
            "effect": "audit"
        }
    }

## <a name="policy-assignment"></a>Házirend-hozzárendelés

Különböző tartományok, például előfizetés, erőforrás-csoportok és az egyes erőforrások házirendek alkalmazzák. Az összes alárendelt erőforrás házirendek örökli. Igen a házirend erőforráscsoport vonatkozik, érdemes alkalmazandó erőforráscsoport az összes erőforrás.

## <a name="creating-a-policy"></a>Házirend létrehozása

Ez a témakör részletes meg, hogyan házirend hozhatók létre REST API-t.

### <a name="create-policy-definition-with-rest-api"></a>Házirend-definícióhoz létrehozása REST API-val

Létrehozhat egy házirendet a [Házirend-definíciók REST API](https://msdn.microsoft.com/library/azure/mt588471.aspx)-val. A REST API segítségével létrehozása és törlése a házirend-definíciók és meglévő-definíciók adatainak.

Hozzon létre egy házirendet, futtatása:

    PUT https://management.azure.com/subscriptions/{subscription-id}/providers/Microsoft.authorization/policydefinitions/{policyDefinitionName}?api-version={api-version}

Ez a példa hasonló kérelem szervezethez:

    {
      "properties":{
        "policyType":"Custom",
        "description":"Test Policy",
        "policyRule":{
          "if" : {
            "not" : {
              "field" : "tags",
              "containsKey" : "costCenter"
            }
          },
          "then" : {
            "effect" : "deny"
          }
        }
      },
      "name":"testdefinition"
    }


Api-verzió *2016-04-01*használja. Példák és további részleteket című témakörben talál [A házirend-definíciók REST API -t](https://msdn.microsoft.com/library/azure/mt588471.aspx).

### <a name="create-policy-definition-using-powershell"></a>Hozzon létre a házirend-definícióhoz PowerShell használatával

A New-AzureRmPolicyDefinition parancsmaggal házirend-definícióhoz hozhat létre. Az alábbi példa létrehoz lehetővé tevő erőforrásainak csak Észak-Európa és a nyugati Europe házirend.

    $policy = New-AzureRmPolicyDefinition -Name regionPolicyDefinition -Description "Policy to allow resource creation only in certain regions" -Policy '{  
      "if" : {
        "not" : {
          "field" : "location",
          "in" : ["northeurope" , "westeurope"]
        }
      },
      "then" : {
        "effect" : "deny"
      }
    }'          

A kimenet végrehajtás $policy objektumot tárolja, és később házirendek során használható. A házirend paraméter a fájl elérési útját .json házirendet tartalmazó is biztosítható, hogy a házirend beágyazott megadása helyett.

    New-AzureRmPolicyDefinition -Name regionPolicyDefinition -Description "Policy to allow resource creation only in certain    regions" -Policy "path-to-policy-json-on-disk"

### <a name="create-policy-definition-using-azure-cli"></a>Azure CLI használatával házirend-definícióhoz létrehozása

A házirend-definíciós parancs alább látható módon az azure CLI használata házirend-definícióhoz hozhat létre. Az alábbi példa létrehoz lehetővé tevő erőforrásainak csak Észak-Európa és a nyugati Europe házirend.

    azure policy definition create --name regionPolicyDefinition --description "Policy to allow resource creation only in certain regions" --policy-string '{   
      "if" : {
        "not" : {
          "field" : "location",
          "in" : ["northeurope" , "westeurope"]
        }
      },
      "then" : {
        "effect" : "deny"
      }
    }'    
    

Ajánlatos adja meg a fájl elérési útját .json tartalmazó helyett a házirend beágyazott alább látható módon adja meg a házirend.

    azure policy definition create --name regionPolicyDefinition --description "Policy to allow resource creation only in certain regions" --policy "path-to-policy-json-on-disk"


## <a name="applying-a-policy"></a>A házirend alkalmazása

### <a name="policy-assignment-with-rest-api"></a>Házirend-hozzárendelés REST API-val

A kívánt hatókör – a [Házirend-hozzárendelésekhez REST API -t](https://msdn.microsoft.com/library/azure/mt588466.aspx)a házirend-definícióhoz alkalmazhat. A REST API segítségével létrehozása és törlése a házirend-hozzárendelések és meglévő hozzárendelésekkel kapcsolatos információk.

Futtassa a házirend-hozzárendelés létrehozásához:

    PUT https://management.azure.com /subscriptions/{subscription-id}/providers/Microsoft.authorization/policyassignments/{policyAssignmentName}?api-version={api-version}

A {házirend-hozzárendelés}-e a házirend-hozzárendelés nevét. Api-verzió *2016-04-01*használja. 

Ez a példa hasonló kérelem szervezethez:

    {
      "properties":{
        "displayName":"VM_Policy_Assignment",
        "policyDefinitionId":"/subscriptions/########/providers/Microsoft.Authorization/policyDefinitions/testdefinition",
        "scope":"/subscriptions/########-####-####-####-############"
      },
      "name":"VMPolicyAssignment"
    }

Példák és további részleteket című témakörben talál [Házirend-hozzárendelésekhez REST API -t](https://msdn.microsoft.com/library/azure/mt588466.aspx).

### <a name="policy-assignment-using-powershell"></a>Házirend-hozzárendelés PowerShell használatával

Az előbb létrehozott Powershellen keresztül a kívánt hatókör a New-AzureRmPolicyAssignment parancsmaggal házirend alkalmazhatja:

    New-AzureRmPolicyAssignment -Name regionPolicyAssignment -PolicyDefinition $policy -Scope    /subscriptions/########-####-####-####-############/resourceGroups/<resource-group-name>
        
$Policy az alábbiakban a csoportházirend-objektum visszaadott eredményeként végrehajtása a New-AzureRmPolicyDefinition parancsmag felett látható módon. Az alábbi hatóköre adja meg, hogy az erőforrás-csoport nevét.

Ha el szeretné távolítani a fenti házirend-hozzárendelés, is elvégezheti az alábbi képlettel történik:

    Remove-AzureRmPolicyAssignment -Name regionPolicyAssignment -Scope /subscriptions/########-####-####-####-############/resourceGroups/<resource-group-name>

Első, módosítása vagy eltávolítása a kurzor a házirend-definíciók Get-AzureRmPolicyDefinition, Set-AzureRmPolicyDefinition és eltávolítás-AzureRmPolicyDefinition parancsmagok között.

Hasonlóképpen kérjen, módosítása, vagy távolítsa el a kurzor át a Get-AzureRmPolicyAssignment, a Set-AzureRmPolicyAssignment, és a eltávolítása-AzureRmPolicyAssignment parancsmagok házirend-hozzárendelések.

### <a name="policy-assignment-using-azure-cli"></a>Házirend-hozzárendelés Azure CLI használatával

Az előbb létrehozott Azure CLI keresztül a kívánt hatókör a házirend hozzárendelése parancs használatával házirend alkalmazhatja:

    azure policy assignment create --name regionPolicyAssignment --policy-definition-id /subscriptions/########-####-####-####-############/providers/Microsoft.Authorization/policyDefinitions/<policy-name> --scope    /subscriptions/########-####-####-####-############/resourceGroups/<resource-group-name>
        
Az alábbi hatóköre adja meg, hogy az erőforrás-csoport nevét. Ha a házirend-definíciós-id paraméter értéke ismeretlen, akkor lehet szerezze be az Azure CLI keresztül alább látható módon: 

    azure policy definition show <policy-name>

Ha el szeretné távolítani a fenti házirend-hozzárendelés, is elvégezheti az alábbi képlettel történik:

    azure policy assignment delete --name regionPolicyAssignment --scope /subscriptions/########-####-####-####-############/resourceGroups/<resource-group-name>

Első, módosítsa, vagy törölje a házirend-definíciók keresztül házirend-definícióhoz megjelenítése beállításához, és parancsok rendre törlése.

Hasonlóképpen első, módosítása vagy eltávolítása a házirend-hozzárendelések a házirend hozzárendelése keresztül megjelenítése és törlése a kurzor a parancsok.

##<a name="policy-audit-events"></a>Naplózási házirend-események

Miután telepítette a házirendet, lásd: a házirend-események elkezdi. Bármelyik Ugrás is portálon használja PowerShell vagy az Azure CLI az adatok eléréséhez. 

### <a name="policy-audit-events-using-powershell"></a>Házirend események PowerShell használatával

Az összes effektus megtagadja kapcsolódó eseményeket megtekintéséhez automatizálhatja az alábbi PowerShell-parancsot:

    Get-AzureRmLog | where {$_.OperationName -eq "Microsoft.Authorization/policies/deny/action"} 

Naplózási effektus kapcsolódó összes eseményének megtekintéséhez automatizálhatja a következő parancsot:

    Get-AzureRmLog | where {$_.OperationName -eq "Microsoft.Authorization/policies/audit/action"} 

### <a name="policy-audit-events-using-azure-cli"></a>Házirend események Azure CLI használatával

Az összes megtekintheti erőforrás eseményeinek effektus megtagadja kapcsolódó csoportjában a következő CLI parancsot használhatja:

    azure group log show ExampleGroup --json | jq ".[] | select(.operationName.value == \"Microsoft.Authorization/policies/deny/action\")"

Naplózási effektus kapcsolódó összes eseményének megtekintéséhez paranccsal a következő CLI:

    azure group log show ExampleGroup --json | jq ".[] | select(.operationName.value == \"Microsoft.Authorization/policies/audit/action\")"
