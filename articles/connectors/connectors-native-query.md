<properties
    pageTitle="A lekérdezés művelet hozzáadása az összefüggés-alkalmazások |} Microsoft Azure"
    description="A lekérdezés műveletet, például a szűrő tömb műveletek elvégzésére vonatkozó áttekintése."
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
   ms.date="07/20/2016"
   ms.author="jehollan"/>

# <a name="get-started-with-the-query-action"></a>A lekérdezés művelet – első lépések

A lekérdezés művelet használatával dolgozhat kötegekben és a tömbök munkafolyamatok elvégezheti:

- Feladatot hozhat létre minden fontosként rekordhoz adatbázisból.
- Az összes PDF-fájl melléklet mentése az e-maileket az Azure blob.

Első lépésiről a lekérdezés művelet összefüggés-alkalmazásban, olvassa el a [egy összefüggés-alkalmazás létrehozása](../app-service-logic/app-service-logic-create-a-logic-app.md)című témakört.

## <a name="use-the-query-action"></a>A lekérdezés művelettel

Művelet egy műveletet, amely a munkafolyamat egy logikai alkalmazásban definiált végzi el. [További tudnivalók a műveletek](connectors-overview.md).  

A lekérdezés jelenleg a műveletnek egy műveletet, a szűrő tömb a-szerkesztőben megjelenő neve. Lehetővé teszi, hogy a lekérdezés tömb, és a szűrt eredményben visszaadására.

Az alábbiakban hogyan hozzáadhatja egy logikai alkalmazásban:

1. Az **Új lépést** gombra.
2. Válassza az **Add művelet**.
3. A művelet Keresés mezőbe írja be a **szűrő tömb** művelet lista **szűrő** .

    ![Jelölje ki a lekérdezés műveletet](./media/connectors-native-query/using-action-1.png)

4. Jelölje ki a tömbképletet szűréséhez. (Az alábbi képernyőképen látható a tömb Twitter keresési eredmények).
5. Hozzon létre egy feltétel, a minden elem ki szeretné számítani. (Az alábbi képernyőképen szűri a felhasználók, akik rendelkeznek 100-nál több követői twitterre.)

    ![A lekérdezés művelet végrehajtása](./media/connectors-native-query/using-action-2.png)

    A művelet csak, amely megfelel az szűrő eredményeket tartalmazó egy új tömböt fog kimeneti.
6. Kattintson az eszköztár mentés a bal felső sarkában, és az összefüggés-alkalmazás mentése és közzététele (aktiválása).

## <a name="query-action"></a>Lekérdezés művelet

Az alábbiakban a műveletet, amely támogatja az Ez az összekötő részleteit. Az összekötő tartalmaz egy lehetséges műveletet.

|Művelet|Leírás|
|---|---|
|Tömb szűrése|Kiértékeli a tömb összes elemének feltételt, és az eredményt ad|

## <a name="action-details"></a>Művelet részletei

A lekérdezés művelet megtalálható egy lehetséges műveletet. Az alábbi táblázat ismerteti a műveletet, és a megfelelő kimeneti részletek társított a művelettel a kötelező és választható beviteli mezők.

### <a name="filter-array"></a>Tömb szűrése
Az alábbiakban a műveletet, amely lehetővé teszi a kimenő HTTP felkérés beviteli mezők.
A * azt jelenti, hogy kötelező megadni.

|Megjelenítendő név|Tulajdonság neve|Leírás|
|---|---|---|
|A *|a|A tömb szűrése|
|A feltétel *|Ha|A feltétel mindegyik elemhez ki szeretné számítani|
<br>

### <a name="output-details"></a>Kimeneti részletei

Az alábbiakban a HTTP-válasz kimeneti részleteit.

|Tulajdonság neve|Adattípus|Leírás|
|---|---|---|
|Szűrt tömb|tömb|Minden egyes szűrt eredménye az objektumot tartalmazó tömb|

## <a name="next-steps"></a>Következő lépések

Most próbálja ki az platform- és [összefüggés-alkalmazás létrehozása](../app-service-logic/app-service-logic-create-a-logic-app.md). Megismerheti az egyéb elérhető összekötők logika alkalmazások [API-khoz lista](apis-list.md)megjeleníti.
