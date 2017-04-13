<properties
    pageTitle="Előre kérést, és a visszajelzések műveleteket |} Microsoft Azure"
    description="A kérelem és válasz eseményindító és a művelet egy Azure logikájának alkalmazásban – áttekintés"
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
   ms.date="07/18/2016"
   ms.author="jehollan"/>

# <a name="get-started-with-the-request-and-response-components"></a>A kérelem és válasz összetevők – első lépések

Egy logikai alkalmazásban a kérelem és válasz elemekkel reagálni tud valós idejű eseményeket.

Ha például közül választhat:

- Az adatok HTTP kérelem megválaszolása keresztül egy logikai alkalmazás helyszíni adatbázisból.
- Egy külső webhook esemény logika alkalmazás elindítása.
- Hívja fel a összefüggés-at, az egy másik összefüggés-alkalmazásokban a kérelem és válasz műveletet.

Első lépésiről a kérelem és válasz műveletek összefüggés-alkalmazásban, olvassa el a [egy összefüggés-alkalmazás létrehozása](../app-service-logic/app-service-logic-create-a-logic-app.md)című témakört.

## <a name="use-the-http-request-trigger"></a>A HTTP-kérés eseményindító használata

Az eseményindító az eseményre kattintva elindíthatja a munkafolyamatot egy logikai alkalmazásban definiált használható. [További tudnivalók a eseményindítók](connectors-overview.md).

Íme egy példa sorozat, hogy miként állíthatja be a logika alkalmazás tervezőben HTTP kérelmet.

1. Adja hozzá az eseményindító **kérelem – Ha a HTTP-kérelem érkezik** a logika alkalmazásban. Tetszés szerint megadhatja a JSON-séma (például [JSONSchema.net](http://jsonschema.net)eszköz használatával) összehívás törzsében. Ezzel a tervező a HTTP-kérés tulajdonságok tokenek létrehozásához.
2. Egyéb művelet végrehajtása, hogy mentheti a logika alkalmazás hozzáadása
3. Miután mentette a logika alkalmazás, a HTTP kérelem URL-címe a kérelem kártyáról elérheti.
4. Az URL-címre HTTP POST (használhatja például [Postman](https://www.getpostman.com/)eszköz) elindítja a logikájának alkalmazást.

>[AZURE.NOTE] Ha egy válasz művelet nem definiálja egy `202 ACCEPTED` választ a rendszer azonnal adja vissza kíván a hívó féllel. A válasz művelettel testreszabása választ.

![Válasz az eseményindító](./media/connectors-native-reqres/using-trigger.png)

## <a name="use-the-http-response-action"></a>A HTTP-válasz művelettel

A HTTP-válasz művelet gomb csak érvényes, ha a HTTP-kérelem által elindított munkafolyamat felhasználná azt. Ha egy válasz művelet nem definiálja egy `202 ACCEPTED` válasz a rendszer azonnal adja vissza kíván a hívó féllel.  Tetszőleges lépés a munkafolyamaton belül a egy válasz műveletet is hozzáadhat. A logika alkalmazás csak továbbra is a beérkező kérelem nyissa meg a válaszra egy percig.  A munkafolyamat nem válaszol küldött (és a definíció szerepel egy válasz művelet), ha egy perc után egy `504 GATEWAY TIMEOUT` a rendszer adja vissza kíván a hívó féllel.

Az alábbi módszerrel HTTP-válasz művelet hozzáadása:

1. Az **Új lépést** gombra.
2. Válassza az **Add művelet**.
3. Művelet Keresés mezőbe írja be a **választ** , listáját, a válasz művelet.

    ![Jelölje ki a válasz műveletet](./media/connectors-native-reqres/using-action-1.png)

4. Vegye fel a HTTP válaszüzenetet szükséges paramétereket.

    ![A válasz művelet végrehajtása](./media/connectors-native-reqres/using-action-2.png)

5. Kattintson az eszköztár mentés a bal felső sarkában, és az logikájának-alkalmazás mentése és közzététele (aktiválása).

## <a name="request-trigger"></a>Eseményindító kérése

Az alábbiakban az eseményindító, amely támogatja az Ez az összekötő részleteit. Egy egyetlen kérelem eseményindító van.

|Eseményindító|Leírás|
|---|---|
|Kérés|Fordul elő, ha a HTTP-kérelem érkezik|

## <a name="response-action"></a>Válasz művelet

Az alábbiakban a műveletet, amely támogatja az Ez az összekötő részleteit. Van egy egyetlen válasz műveletet, amely csak akkor használható, amikor egy kérés eseményindító együtt.

|Művelet|Leírás|
|---|---|
|Válasz|Válasz arra a korrelációs HTTP függvény|

### <a name="trigger-and-action-details"></a>Eseményindító és a művelet részletei

Az alábbi táblázat ismerteti a beviteli mezők a kiváltó ok mező és a művelet, és a megfelelő kimeneti részleteket.

#### <a name="request-trigger"></a>Eseményindító kérése
Az eseményindító bejövő HTTP-összehívásokban egy beviteli mező a következő:

|Megjelenítendő név|Tulajdonság neve|Leírás|
|---|---|---|
|JSON séma|séma|A JSON séma a HTTP-kérés szervezet|
<br>

**Kimeneti részletei**

Az alábbiakban a kérés részleteinek kimeneti.

|Tulajdonság neve|Adattípus|Leírás|
|---|---|---|
|Élőfejek|objektum|Fejlécek kérése|
|Szervezet|objektum|Request objektum|

#### <a name="response-action"></a>Válasz művelet

Az alábbiakban a HTTP-válasz művelet beviteli mezők. A * azt jelenti, hogy kötelező megadni.

|Megjelenítendő név|Tulajdonság neve|Leírás|
|---|---|---|
|Állapot kód *|statusCode|A HTTP-állapotkódot|
|Élőfejek|Élőfejek|Bármely válasz fejlécek, amelyet fel szeretne venni egy JSON-objektum|
|Szervezet|szervezet|A válasz szervezet|

## <a name="next-steps"></a>Következő lépések

Most próbálja ki az platform- és [összefüggés-alkalmazás létrehozása](../app-service-logic/app-service-logic-create-a-logic-app.md). Megismerheti az egyéb elérhető összekötők logika alkalmazások [API-khoz lista](apis-list.md)megjeleníti.
