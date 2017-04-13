<properties
    pageTitle="A HTTP-művelet hozzáadása az összefüggés-alkalmazások |} Microsoft Azure"
    description="A HTTP-művelet tulajdonságok áttekintése"
    services=""
    documentationCenter=""
    authors="jeffhollan"
    manager="erikre"
    editor=""
    tags="connectors"/>

<tags
   ms.service="logic-apps"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="07/15/2016"
   ms.author="jehollan"/>

# <a name="get-started-with-the-http-action"></a>Első lépések a HTTP-művelet

A HTTP-művelet munkafolyamatok kiterjesztése a szervezet számára, és a HTTP bármely végpontra kommunikáció.

képes vagy:

- Logika aktiválása a (eseményindító), ha olyan webhely esetében, kezelheti megszakad alkalmazás munkafolyamatok létrehozása.
- Kommunikáció bármely végpontra HTTP ki szeretné terjeszteni a munkafolyamatok más szolgáltatásokat.

Első lépésiről a HTTP-művelet összefüggés-alkalmazásban, olvassa el a [egy összefüggés-alkalmazás létrehozása](../app-service-logic/app-service-logic-create-a-logic-app.md)című témakört.

## <a name="use-the-http-trigger"></a>A HTTP eseményindító használata

Az eseményindító az eseményre kattintva elindíthatja a munkafolyamatot egy logikai alkalmazásban definiált használható. [További tudnivalók a eseményindítók](connectors-overview.md).

Íme egy példa sorozat, hogy miként állíthatja be a HTTP eseményindító a logika alkalmazás-szerkesztőben.

1. Adja hozzá a HTTP eseményindító a logika alkalmazásban.
2. Adja meg a paraméterek a lekérdezni kívánt HTTP végpontot.
3. Módosítsa a milyen gyakran kell lekérdezik az ismétlődési gyakoriságát.
4. A logikai alkalmazás most akkor indul el, a minden olyan tartalom, a függvény által visszaadott minden-ellenőrzése során.

![HTTP eseményindító](./media/connectors-native-http/using-trigger.png)

### <a name="how-the-http-trigger-works"></a>A HTTP eseményindító működése

A HTTP eseményindító HTTP zárólap felhívja ismétlődő intervallummal. Alapértelmezés szerint minden HTTP-válasz kód kisebb, mint 300 eredmények futtatása összefüggés-alkalmazásban. A megadott feltétel után a HTTP-hívás határozza meg, ha a logikai alkalmazást kell fire kiértékelő kód nézetben is hozzáadhat. Íme egy példa akkor indul el, amikor a visszaadott állapotkód nagyobb vagy egyenlő HTTP eseménykód `400`.

```javascript
"Http":
{
    "conditions": [
        {
            "expression": "@greaterOrEquals(triggerOutputs()['statusCode'], 400)"
        }
    ],
    "inputs": {
        "method": "GET",
        "uri": "https://blogs.msdn.microsoft.com/logicapps/",
        "headers": {
            "accept-language": "en"
        }
    },
    "recurrence": {
        "frequency": "Second",
        "interval": 15
    },
    "type": "Http"
}
```

A HTTP eseményindító paraméterek tájékozódhat [MSDN](https://msdn.microsoft.com/library/azure/mt643939.aspx#HTTP-trigger)érhetők el.

## <a name="use-the-http-action"></a>A HTTP-művelet használata

Művelet egy műveletet, amely a munkafolyamatot, amely egy logikai alkalmazás könyvjelzőnév végzi el. [További tudnivalók a műveletek](connectors-overview.md).

1. Az **Új lépést** gombra.
2. Válassza az **Add művelet**.
3. A művelet Keresés mezőbe írja be a **http** listáját a HTTP-művelet.

    ![Jelölje ki a HTTP-művelet](./media/connectors-native-http/using-action-1.png)

4. Vegye fel a HTTP-híváshoz szükséges paramétereket.

    ![A HTTP-művelet végrehajtása](./media/connectors-native-http/using-action-2.png)

5. Kattintson az eszköztár mentés a bal felső sarokban. Az összefüggés-alkalmazás mentése és közzététele (aktiválása).

## <a name="http-trigger"></a>HTTP eseményindító

Az alábbiakban az eseményindító, amely támogatja az Ez az összekötő részleteit. A HTTP-összekötő egy eseményindító tartalmaz.

|Eseményindító|Leírás|
|---|---|
|HTTP|A HTTP-hívást kezdeményez, és a válasz tartalmát adja vissza.|

## <a name="http-action"></a>HTTP-művelet

Az alábbiakban a műveletet, amely támogatja az Ez az összekötő részleteit. A HTTP-összekötő tartalmaz egy lehetséges műveletet.

|Művelet|Leírás|
|---|---|
|HTTP|A HTTP-hívást kezdeményez, és a válasz tartalmát adja vissza.|

## <a name="http-details"></a>HTTP részletei

Az alábbi táblázat ismerteti a műveletet, és a megfelelő kimeneti részletek társított a művelettel a kötelező és választható beviteli mezők.


#### <a name="http-request"></a>HTTP-kérés
Az alábbiakban a műveletet, amely lehetővé teszi a kimenő HTTP felkérés beviteli mezők.
A * azt jelenti, hogy kötelező megadni.

|Megjelenítendő név|Tulajdonság neve|Leírás|
|---|---|---|
|A módszer *|a módszer|HTTP-művelet használata|
|URI *|URI|A URI a HTTP-kérés|
|Élőfejek|Élőfejek|A HTTP-fejlécek, amelyet fel szeretne venni egy JSON-objektum|
|Szervezet|szervezet|A HTTP összehívás törzsében|
|Hitelesítés|hitelesítés|A [hitelesítési](#authentication) szakasz részletei|
<br>

#### <a name="output-details"></a>Kimeneti részletei

Az alábbiakban a HTTP-válasz kimeneti részleteit.

|Tulajdonság neve|Adattípus|Leírás|
|---|---|---|
|Élőfejek|objektum|Válasz fejlécek|
|Szervezet|objektum|Válasz objektum|
|Állapotkód|int|HTTP-állapotkód|

## <a name="authentication"></a>Hitelesítés

A logika alkalmazások funkció Azure alkalmazás szolgáltatás lehetővé teszi, hogy HTTP végpontok hitelesítését eltérő típusú. Ez a hitelesítés **HTTP**, **[HTTP + Swagger](./connectors-native-http-swagger.md)**és a **[HTTP Webhook](./connectors-native-webhook.md)** összekötőkkel is használhatja. A következő típusú hitelesítési konfigurálható:

* [Alapszintű hitelesítés](#basic-authentication)
* [Ügyfél-tanúsítvány hitelesítés](#client-certificate-authentication)
* [Azure Active Directory (Azure Active Directory) OAuth hitelesítés](#azure-active-directory-oauth-authentication)

#### <a name="basic-authentication"></a>Alapszintű hitelesítés

A következő hitelesítési objektum alapszintű hitelesítés van szükség.
A * azt jelenti, hogy kötelező megadni.

|Tulajdonság neve|Adattípus|Leírás|
|---|---|---|
|Típus *|típus|Hitelesítés típusa (kell lennie `Basic` alapszintű hitelesítés)|
|Username *|felhasználónév|Felhasználónév hitelesítést végezni|
|Jelszó *|jelszó|Jelszó-hitelesítést végezni|

>[AZURE.TIP] Ha azt szeretné, hogy nem lehet beolvasni a definíció, használata jelszóval egy `securestring` paraméter és a `@parameters()` [munkafolyamat definition függvény](http://aka.ms/logicappdocs).

A hitelesítés mezőben úgy szeretné hozzon létre egy objektumot jelennek meg:

```javascript
{
    "type": "Basic",
    "username": "user",
    "password": "test"
}
```

#### <a name="client-certificate-authentication"></a>Ügyfél-tanúsítvány hitelesítés

A következő hitelesítési objektum ügyfél tanúsítvány-hitelesítés van szükség. A * azt jelenti, hogy kötelező megadni.

|Tulajdonság neve|Adattípus|Leírás|
|---|---|---|
|Típus *|típus|Milyen típusú hitelesítéssel (kell lennie `ClientCertificate` az ügyfél SSL-tanúsítványok)|
|PFX *|PFX|A személyes információkat Exchange (PFX) fájl Base64 kódolású tartalmát|
|Jelszó *|jelszó|A jelszó a PFX fájl elérésére|

>[AZURE.TIP] Használhatja a `securestring` paraméter és a `@parameters()` , amely nem lesz a összefüggés-alkalmazás mentése után a definíció olvashatók paraméter [munkafolyamat definition függvény](http://aka.ms/logicappdocs) .

Példa:

```javascript
{
    "type": "ClientCertificate",
    "pfx": "aGVsbG8g...d29ybGQ=",
    "password": "@parameters('myPassword')"
}
```

#### <a name="azure-ad-oauth-authentication"></a>Azure Active Directory OAuth hitelesítés

A következő hitelesítési objektum Azure Active Directory OAuth hitelesítés van szükség. A * azt jelenti, hogy kötelező megadni.

|Tulajdonság neve|Adattípus|Leírás|
|---|---|---|
|Típus *|típus|Milyen típusú hitelesítéssel (kell lennie `ActiveDirectoryOAuth` az Azure Active Directory OAuth)|
|Bérlői *|Bérlői webhelyen|Az Azure AD-bérlő bérlői azonosítója|
|A célközönség *|a célközönség|Beállítása`https://management.core.windows.net/`|
|Ügyfél azonosítója *|clientId|Az ügyfél-azonosító az Azure Active Directory-alkalmazáshoz|
|Titkos *|titkos kulcs|A titkos az ügyfél, a token igénylő|

>[AZURE.TIP] Használhatja a `securestring` paraméter és a `@parameters()` , amely nem olvasható, a mentés után definition paraméter [munkafolyamat definition függvény](http://aka.ms/logicappdocs) .

Példa:

```javascript
{
    "type": "ActiveDirectoryOAuth",
    "tenant": "72f988bf-86f1-41af-91ab-2d7cd011db47",
    "audience": "https://management.core.windows.net/",
    "clientId": "34750e0b-72d1-4e4f-bbbe-664f6d04d411",
    "secret": "hcqgkYc9ebgNLA5c+GDg7xl9ZJMD88TmTJiJBgZ8dFo="
}
```

## <a name="next-steps"></a>Következő lépések

Most próbálja ki az platform- és [összefüggés-alkalmazás létrehozása](../app-service-logic/app-service-logic-create-a-logic-app.md). Megismerheti az egyéb elérhető összekötők logika alkalmazások [API-khoz lista](apis-list.md)megjeleníti.
