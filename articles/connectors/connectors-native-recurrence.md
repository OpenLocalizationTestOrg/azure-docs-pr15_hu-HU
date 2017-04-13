<properties
    pageTitle="Az ismétlődés eseményindító hozzáadása a logikájának alkalmazásokban |} Microsoft Azure"
    description="Az ismétlődés eseményindító, és hogyan használhatja azt az Azure logika alkalmazás áttekintése."
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

# <a name="get-started-with-the-recurrence-trigger"></a>Az ismétlődés eseményindító – első lépések

Az ismétlődés eseményindító segítségével létrehozhat hatékony munkafolyamatok a felhőben.

Ha például közül választhat:

- A munkafolyamat minden nap SQL tárolt eljárás futtatása ütemezése.
- A múlt héten kapcsolatos bizonyos keresztes e-mailben minden twitterre összefoglalását.

Első lépésiről az ismétlődés eseményindító összefüggés-alkalmazásban, olvassa el a [egy összefüggés-alkalmazás létrehozása](../app-service-logic/app-service-logic-create-a-logic-app.md)című témakört.

## <a name="use-a-recurrence-trigger"></a>Ismétlődés eseményindító használata

Az eseményindító az eseményre kattintva elindíthatja a munkafolyamatot egy logikai alkalmazásban definiált használható. [További tudnivalók a indítók](connectors-overview.md).

Íme egy példa sorozat, hogy miként állíthat be egy logikai alkalmazásban az ismétlődés eseményindító:

1. Az **Ismétlődés** eseményindító hozzáadása egy logikai alkalmazásban első lépéseként.
2. A paraméterek adja meg az ismétlődési gyakoriságát.

A logika alkalmazás letöltése után minden időtartományt a Futtatás indítja el.

![HTTP eseményindító](./media/connectors-native-recurrence/using-trigger.png)

## <a name="trigger-details"></a>Eseményindító részletei

Az ismétlődés eseményindító a következő tulajdonságokat konfigurálható tulajdonsággal tartalmaz.

A akkor megadott időközönként után indul el egy logikájának alkalmazást.
A * azt jelenti, hogy kötelező megadni.

|Megjelenítendő név|Tulajdonság neve|Leírás|
|---|---|---|
|Gyakoriság *|gyakoriság|The unit of time: `Second`, `Minute`, `Hour`, `Day`, or `Year`.|
|Intervallum *|intervallum|Az adott gyakoriság az ismétlődés időközének.|
|Időzóna|Időzóna|A kezdés idejének a világidőtől való eltérését nélkül megadva, ez az időzóna lesz.|
|Elindítása|Kezdés időpontja|A kezdő időpont [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601#Combined_date_and_time_representations)formátumban.|
<br>


## <a name="next-steps"></a>Következő lépések

Most próbálja ki az platform- és [összefüggés-alkalmazás létrehozása](../app-service-logic/app-service-logic-create-a-logic-app.md). Megismerheti az egyéb elérhető összekötők logika alkalmazások [API-khoz lista](apis-list.md)megjeleníti.
