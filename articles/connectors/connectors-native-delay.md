<properties
    pageTitle="Késleltetést hozzáadása a logikájának alkalmazásokban |} Microsoft Azure"
    description="A késleltetés és késleltetés áttekintése – addig, amíg a műveletek, és hogyan használhatók az Azure összefüggés-alkalmazásokban."
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

# <a name="get-started-with-the-delay-and-delay-until-actions"></a>Első lépések a késleltetés és késleltetés a-ig műveletek

Késleltetés használatával és "késleltetés-mindaddig, amíg" műveleteket is elvégezheti az munkafolyamatokban.

Ha például közül választhat:

- Várja meg, amíg egy hét.napja állapotfrissítés küldése e-mailek fölé.
- A munkafolyamat tovább tarthat, amíg a HTTP-hívás folytatása, és az eredmény beolvasása előtti befejezési idő.

Első lépésiről a késleltetés művelet összefüggés-alkalmazásban, olvassa el a [egy összefüggés-alkalmazás létrehozása](../app-service-logic/app-service-logic-create-a-logic-app.md)című témakört.

## <a name="use-the-delay-actions"></a>A késleltetés műveletek használata

Művelet egy műveletet, amely a munkafolyamat egy logikai alkalmazásban definiált végzi el. [További tudnivalók a műveletek](connectors-overview.md).

Íme egy példa sorozat, hogyan használhatja a késleltetés lépés egy logikai alkalmazásban:

1. Miután megadta az eseményindító, kattintson az **Új lépést** hozzáadni a műveletet.
2. Keresse meg a **késleltetés** jelenítse meg a késleltetés műveleteket. Ebben a példában a **késleltetés**kijelöli azt.

    ![Késleltetés műveletek](./media/connectors-native-delay/using-action-1.png)

3. A művelet tulajdonságait, a késleltetés konfigurálása végrehajtása

    ![Késleltetés config](./media/connectors-native-delay/using-action-2.png)

4. Kattintson a **Mentés** és aktiválhatja a logikájának alkalmazást.


## <a name="action-details"></a>Művelet részletei

Az ismétlődés eseményindító a következő tulajdonságokat beállítható úgy, hogy rendelkezik.

### <a name="delay-action"></a>Késleltetés művelet

Ez a művelet a Futtatás meghatározott időközönként késlelteti.
A * azt jelenti, hogy kötelező megadni.

|Megjelenítendő név|Tulajdonság neve|Leírás|
|---|---|---|
|Darab *|Darabszám|Időegységek késleltetés száma|
|Egység *|egység|A időegység: `Second`, `Minute`, `Hour`, vagy`Day`|
<br>

### <a name="delay-until-action"></a>Késleltetés-ig művelet

Ez a művelet csak a megadott dátum/idő a Futtatás késlelteti.
A * azt jelenti, hogy kötelező megadni.

|Megjelenítendő név|Tulajdonság neve|Leírás|
|---|---|---|
|Év *|időbélyeg|Az év (GMT)-ig késleltetése|
|Hónap *|időbélyeg|A hónap addig, amíg a (GMT) késleltetése|
|Napi *|időbélyeg|Késleltetés (GMT) addig, amíg a nap|
<br>


## <a name="next-steps"></a>Következő lépések

Most próbálja ki az platform- és [összefüggés-alkalmazás létrehozása](../app-service-logic/app-service-logic-create-a-logic-app.md). Megismerheti az egyéb elérhető összekötők logika alkalmazások [API-khoz lista](apis-list.md)megjeleníti.
