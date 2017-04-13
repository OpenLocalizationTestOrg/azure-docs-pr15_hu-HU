<properties
   pageTitle="Erőforrás-kezelő REST API-khoz |} Microsoft Azure"
   description="Az erőforrás-kezelő REST API-khoz hitelesítési és használati példák áttekintése"
   services="azure-resource-manager"
   documentationCenter="na"
   authors="navalev"
   manager="timlt"
   editor=""/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="06/23/2016"
   ms.author="navale;tomfitz;"/>
   
# <a name="resource-manager-rest-apis"></a>Erőforrás-kezelő REST API-hoz

> [AZURE.SELECTOR]
- [Azure PowerShell](powershell-azure-resource-manager.md)
- [Azure CLI](xplat-cli-azure-resource-manager.md)
- [Portál](./azure-portal/resource-group-portal.md) 
- [REST API-VAL](resource-manager-rest-api.md)

Minden hívás az Azure erőforrás-kezelő, minden telepített sablon mögött mögött minden konfigurált tároló fiók mögött van egy vagy több hívásokat az Azure erőforrás-kezelő RESTful API-hoz. Ez a témakör munkaidejének e API-k és az hogyan felhívhatja őket bármely SDK egyáltalán használata nélkül. Lehet, hogy ez nagyon hasznos, ha azt szeretné, hogy a teljes hozzáférés az összes kérelmet Azure, vagy ha a használni kívánt nyelvet a SDK nem érhető el, vagy nem támogatja a műveleteket szeretne végezni.

Ez a cikk nem megy minden API-val, amely az Azure-ban van téve keresztül, de inkább használata jóváhagyást, és hogyan a csatlakozhat hozzájuk példaként néhány. Ha alapvető megismeri jóváhagyást és olvassa el a [Azure erőforrás-kezelő REST API-hivatkozás](https://msdn.microsoft.com/library/azure/dn790568.aspx) a részletes információkat a többi az API-k használata.

## <a name="authentication"></a>Hitelesítés
ARM-hitelesítés által Azure Active Directory (AD) kezeli. Hogy bármilyen először kell hitelesítést végezni az Azure Active Directory-hitelesítési jogkivonat fogadásához API csatlakoztatni, amikor továbbíthatja minden kérelmet. Ahogy azt is azt mutatja be, közvetlenül a REST API-k tiszta hívás, azt is feltételezi, hogy nem szeretné az egy normál felhasználónév-jelszó, ahol a pop-Close-Up képernyős előfordulhat, hogy kéri felhasználónevét és jelszavát, és talán még további hitelesítési mechanizmusok két tényező hitelesítési esetek használt hitelesítő. Emiatt azt hoz létre az Azure Active Directory-alkalmazások és a használandó egyszerű úgynevezett való bejelentkezéshez. De ne feledje, hogy több hitelesítési eljárás támogatja az Azure Active Directory és a mindegyikük felhasználhatók, hogy hitelesítés szükséges az ezt követő API-kérelmek jogkivonat beolvasásához.
Hajtsa végre az [létrehozása az Azure Active Directory-alkalmazás és a szolgáltatás elve](./resource-group-create-service-principal-portal.md) lépésenkénti útmutatást.

### <a name="generating-an-access-token"></a>Egy hozzáférési jogkivonat létrehozása 
Azure Active Directory hitelesítését végzi Azure Active Directory, hívását login.microsoftonline.com helyen található. Annak érdekében, hogy Ön hitelesítő kell lennie az alábbi adatokat:

* Azure Active Directory bérlői ID (adott Azure AD-bejelentkezési, gyakran ugyanaz, mint a vállalat, de nem szükséges esetén a neve)
* Azonosítója (Azure AD-alkalmazás létrehozása lépés során tett)
* Jelszót (az Azure Active Directory-alkalmazás létrehozása során választotta)

Az a HTTP-kérés alatti ügyeljen arra, hogy "Azure AD-bérlő Azonosítót", "Azonosítója" és "Jelszó" cserélje ki a megfelelő értékeket.

**Általános HTTP-kérés:**

```HTTP
POST /<Azure AD Tenant ID>/oauth2/token?api-version=1.0 HTTP/1.1 HTTP/1.1
Host: login.microsoftonline.com
Cache-Control: no-cache
Content-Type: application/x-www-form-urlencoded

grant_type=client_credentials&resource=https%3A%2F%2Fmanagement.core.windows.net%2F&client_id=<Application ID>&client_secret=<Password>
```

… (Ha sikerül hitelesítés) fog eredmény az alábbihoz hasonló választ:

```json
{
  "token_type": "Bearer",
  "expires_in": "3600",
  "expires_on": "1448199959",
  "not_before": "1448196059",
  "resource": "https://management.core.windows.net/",
  "access_token": "eyJ0eXAiOiJKV1QiLCJhb...86U3JI_0InPUk_lZqWvKiEWsayA"
}
```
(A fenti válasz a access_token van már csonkolva olvashatóság)

**Hozzáférési jogkivonat Bash használatával létrehozása:**

```console
curl -X POST -H "Content-Type: application/x-www-form-urlencoded" -d "grant_type=client_credentials&resource=https://management.core.windows.net&client_id=<application id>&client_secret=<password you selected for authentication>" https://login.microsoftonline.com/<Azure AD Tenant ID>/oauth2/token?api-version=1.0
```

**A PowerShell használatával hozzáférési jogkivonat létrehozása:**

```powershell
Invoke-RestMethod -Uri https://login.microsoftonline.com/<Azure AD Tenant ID>/oauth2/token?api-version=1.0 -Method Post
 -Body @{"grant_type" = "client_credentials"; "resource" = "https://management.core.windows.net/"; "client_id" = "<application id>"; "client_secret" = "<password you selected for authentication>" }
```

A válasz egy hozzáférési jogkivonat, információ arról, hogy mennyi ideig, hogy a token érvényes, és milyen erőforrás is használhatja az adott jogkivonat információkat tartalmaz.
A hozzáférési jogkivonat, az előző a HTTP-hívás kapott kell átadni az összes kérelme a ARM API-hoz fejléceként "Engedély" nevű "Bearer YOUR_ACCESS_TOKEN" értékű. Figyelje meg a "Bearer" és a hozzáférési jogkivonat közötti távolságot.

Amint a fenti HTTP eredményből látható, a token érvényes idő, ameddig kell gyorsítótár és újra felhasználhat az adott azonos jogkivonat egy adott időszakra. Akkor is, ha minden API-híváshoz Azure Active Directory hitelesítő lehetőség, nagyon hatékony lenne.

## <a name="calling-arm-rest-apis"></a>Hívófél ARM REST API-hoz

[Azure erőforrás-kezelő REST API -khoz is dokumentált itt](https://msdn.microsoft.com/library/azure/dn790568.aspx) , de a Kijelentkezés ebből az oktatóanyagból a dokumentumhoz a használatát, az egyes és minden hatókörét. Ez a dokumentáció csak néhány API-hoz való használandó az egyszerű használatát ismertetik az API-t, majd ezt követően hivatkozunk, a hivatalos dokumentációt.

### <a name="list-all-subscriptions"></a>A lista minden előfizetés

A legegyszerűbb műveleteket végezheti el egyik elérésére jogosult a rendelkezésre álló előfizetések listáját. Kattintson a kérelem megtekintheti, hogyan hozzáférési jogkivonat átadott a fejléceként alatt.

(Cserélje YOUR_ACCESS_TOKEN a tényleges hozzáférési jogkivonat.)

```HTTP
GET /subscriptions?api-version=2015-01-01 HTTP/1.1
Host: management.azure.com
Authorization: Bearer YOUR_ACCESS_TOKEN
Content-Type: application/json
```

.. eredményeként kap, a választható előfizetések listáját a szolgáltatás egyszerű hozzáférhetnek

(Az alábbi előfizetési azonosítók van már rövidíteni olvashatóság érdekében)

```json
{
  "value": [
    {
      "id": "/subscriptions/3a8555...555995",
      "subscriptionId": "3a8555...555995",
      "displayName": "Azure Subscription",
      "state": "Enabled",
      "subscriptionPolicies": {
        "locationPlacementId": "Internal_2014-09-01",
        "quotaId": "Internal_2014-09-01"
      }
    }
  ]
}
```

### <a name="list-all-resource-groups-in-a-specific-subscription"></a>A lista összes erőforráscsoport az adott előfizetés

A ARM API-hoz elérhető összes erőforrások erőforráscsoport vannak ágyazva. Fogjuk lekérdezés ARM meglévő erőforráscsoport az előfizetés használatának a HTTP GET kérés alatt. Figyelje meg, hogyan az előfizetés azonosítója átadott az URL-címe részeként ezúttal.

(Cserélje YOUR_ACCESS_TOKEN és ELŐFIZETÉS_AZONOSÍTÓJA a tényleges hozzáférési jogkivonat és előfizetés-azonosító)

```HTTP
GET /subscriptions/SUBSCRIPTION_ID/resourcegroups?api-version=2015-01-01 HTTP/1.1
Host: management.azure.com
Authorization: Bearer YOUR_ACCESS_TOKEN
Content-Type: application/json
```

A kérdésre adott választ kaphat függ, hogy rendelkezik-e definiált erőforrás csoportokat, és ha igen, hogy hány.

(Az alábbi előfizetési azonosítók van már rövidíteni olvashatóság érdekében)

```json
{
    "value": [
        {
            "id": "/subscriptions/3a8555...555995/resourceGroups/myfirstresourcegroup",
            "name": "myfirstresourcegroup",
            "location": "eastus",
            "properties": {
                "provisioningState": "Succeeded"
            }
        },
        {
            "id": "/subscriptions/3a8555...555995/resourceGroups/mysecondresourcegroup",
            "name": "mysecondresourcegroup",
            "location": "northeurope",
            "tags": {
                "tagname1": "My first tag"
            },
            "properties": {
                "provisioningState": "Succeeded"
            }
        }
    ]
}
```

### <a name="create-a-resource-group"></a>Erőforráscsoport létrehozása

Az eddigi azt már csak lett lekérdezése információt ARM API-khoz, helyette létrehozunk források és az első lépésként el őket az összes erőforráscsoport legegyszerűbb ideje. A következő HTTP-kérés hoz létre egy új erőforráscsoport a terület és a helyre, és egy vagy több címkék hozzáadása (alább valójában csak a minta hozzáadása egy címke).

(Cserélje YOUR_ACCESS_TOKEN, ELŐFIZETÉS_AZONOSÍTÓJA, RESOURCE_GROUP_NAME a tényleges hozzáférési jogkivonat, előfizetés Azonosítóját és nevét az erőforráscsoport létre szeretne hozni)

```HTTP
PUT /subscriptions/SUBSCRIPTION_ID/resourcegroups/RESOURCE_GROUP_NAME?api-version=2015-01-01 HTTP/1.1
Host: management.azure.com
Authorization: Bearer YOUR_ACCESS_TOKEN
Content-Type: application/json

{
  "location": "northeurope",
  "tags": {
    "tagname1": "test-tag"
  }
}
```

Ha sikeres, akkor ehhez hasonló választ kap

```json
{
  "id": "/subscriptions/3a8555...555995/resourceGroups/RESOURCE_GROUP_NAME",
  "name": "RESOURCE_GROUP_NAME",
  "location": "northeurope",
  "tags": {
    "tagname1": "test-tag"
  },
  "properties": {
    "provisioningState": "Succeeded"
  }
}
```

Erőforráscsoport sikeresen létrehozott Azure-ban. Gratulálok!

### <a name="deploy-resources-to-a-resource-group-using-an-arm-template"></a>ARM-sablon segítségével erőforráscsoport erőforrások telepítése

ARM telepítheti az erőforrások ARM sablonok használatával. Egy ARM sablont a több erőforrások és a függőségek határozza meg. Ebben a szakaszban fog csak feltételezzük már jól ismert ARM sablonokat, és csak bemutatjuk, hogyan készíthet az API-hívás indítása a telepítés az egyik. ARM-sablonok egy részletes leírást található.

ARM-sablon telepítési sokkal eltérő nem hogyan hívja fel az egyéb API-khoz. Egy fontos méretarány, hogy a sablon telepítésének igazán hosszú időt vehet, attól függően, hogy mi az a sablonban szereplő és az API-hívás csak ad vissza miatt Önnek, a telepítés állapotának Query Fejlesztőeszközök annak érdekében, hogy megtudhatja, hogy a telepítés befejeződött.

Ebben a példában a [GitHub](https://github.com/Azure/azure-quickstart-templates)elérhető nyilvánosan elérhető ARM sablon használjuk. A sablon fogjuk használni fog egy Linux virtuális üzembe az USA-beli nyugati régió. Akkor is, ha ez a sablon lesz elérhető a sablon egy nyilvános tárban tárolnak, például GitHub, emellett bejelölheti a teljes sablon átadni a kérelem részeként. Figyelje meg, hogy elvégezheti a szükséges paraméterértékeket a kérelmet, a használt sablont belül használt részeként.

(Cseréje ELŐFIZETÉS_AZONOSÍTÓJA, RESOURCE_GROUP_NAME, DEPLOYMENT_NAME, YOUR_ACCESS_TOKEN, GLOBALY_UNIQUE_STORAGE_ACCOUNT_NAME, ADMIN_USER_NAME, ADMIN_PASSWORD és DNS_NAME_FOR_PUBLIC_IP a kérelmet megfelelő értékek)

```HTTP
PUT /subscriptions/SUBSCRIPTION_ID/resourcegroups/RESOURCE_GROUP_NAME/providers/microsoft.resources/deployments/DEPLOYMENT_NAME?api-version=2015-01-01 HTTP/1.1
Host: management.azure.com
Authorization: Bearer YOUR_ACCESS_TOKEN
Content-Type: application/json

{
  "properties": {
    "templateLink": {
      "uri": "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-simple-linux-vm/azuredeploy.json",
      "contentVersion": "1.0.0.0",
    },
    "mode": "Incremental",
    "parameters": {
        "newStorageAccountName": {
          "value": "GLOBALY_UNIQUE_STORAGE_ACCOUNT_NAME"
        },
        "adminUsername": {
          "value": "ADMIN_USER_NAME"
        },
        "adminPassword": {
          "value": "ADMIN_PASSWORD"
        },
        "dnsNameForPublicIP": {
          "value": "DNS_NAME_FOR_PUBLIC_IP"
        },
        "ubuntuOSVersion": {
          "value": "15.04"
        }
    }
  }
}
```

A kérés a meglehetősen hosszú JSON válasz van már hiányzik, annak érdekében, hogy jobban olvashatóvá teheti a dokumentáció a. A válasz is tartalmaz, amely az imént létrehozott sablon üzembe helyezésével kapcsolatos információk.

