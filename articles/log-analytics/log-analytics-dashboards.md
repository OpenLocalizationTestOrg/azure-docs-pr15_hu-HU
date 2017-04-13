<properties
    pageTitle="Egyéni irányítópult létrehozása a napló Analytics |} Microsoft Azure"
    description="Ez az útmutató segítségével megtudhatja, hogyan napló Analytics irányítópultok is ábrázolása összes a mentett naplót keresések, biztosítva egy egyetlen lens megtekintése a környezetben."
    services="log-analytics"
    documentationCenter=""
    authors="bandersmsft"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="banders"/>

# <a name="create-a-custom-dashboard-in-log-analytics"></a>A napló Analytics egyéni irányítópult létrehozása

Ez az útmutató segítségével megtudhatja, hogyan napló Analytics irányítópultok is ábrázolása összes a mentett naplót keresések, biztosítva egy egyetlen lens megtekintése a környezetben.

![Minta irányítópulton](./media/log-analytics-dashboards/oms-dashboards-example-dash.png)

Az egyéni irányítópultok a MOBILE portálon létrehozott is elérhetők a MOBILE Mobile alkalmazásban. Lásd: további információt az alkalmazásokat az alábbi lapokon.

- [A Microsoft Store MOBILE mobilalkalmazásban](http://www.windowsphone.com/store/app/operational-insights/4823b935-83ce-466c-82bb-bd0a3f58d865)
- [Az Apple iTunes MOBILE mobilalkalmazásban](https://itunes.apple.com/app/microsoft-operations-management/id1042424859?mt=8)

![mobil irányítópult](./media/log-analytics-dashboards/oms-search-mobile.png)

## <a name="how-do-i-create-my-dashboard"></a>Hogyan hozhatok létre saját irányítópult?

A kezdéshez a MOBILE áttekintése lap megnyitásához. A bal oldalon megjelenik a **Saját irányítópult** csempére. Kattintson arra az irányítópult végezhet részletezést.

![– Áttekintés](./media/log-analytics-dashboards/oms-dashboards-overview.png)


## <a name="adding-a-tile"></a>Egy mozaikon hozzáadása

Az irányítópultok csempék vannak a mentett naplót keresések hajtott. Számos előre elvégzett mentett naplót keresésekhez, azonnal máris elkezdheti MOBILE megtalálható. Az alábbi lépéseket, hogy hogyan kezdje el a szerkezet használja.

A saját irányítópult nézetben, egyszerűen kattintson a **Testreszabás** beírása üzemmód testreszabása.

![Képi](./media/log-analytics-dashboards/oms-dashboards-pictorial01.png)

 A panel, amely megnyitja a lap jobb oldalán látható az összes a munkaterület mentett naplót keresések. Egy mentett naplót keresést, a mozaik ábrázolásához mentett keresés mutasson, és kattintson a **plusz** szimbólum gombra.

![1 Csempék felvétele](./media/log-analytics-dashboards/oms-dashboards-pictorial02.png)

Ha a **plusz** szimbólum gombra kattint, új csempe a saját irányítópult nézetben jelenik meg.

![2 Csempék felvétele](./media/log-analytics-dashboards/oms-dashboards-pictorial03.png)


## <a name="edit-a-tile"></a>Csempe szerkesztése

A saját irányítópult nézetben, egyszerűen kattintson a **Testreszabás** beírása üzemmód testreszabása. Kattintson a szerkeszteni kívánt csempére. A jobb oldali ablaktábla módosításokat szerkesztéséhez, és a kijelölt beállítások:

![Csempe szerkesztése](./media/log-analytics-dashboards/oms-dashboards-pictorial04.png)

![Csempe szerkesztése](./media/log-analytics-dashboards/oms-dashboards-pictorial05.png)

### <a name="tile-visualizations"></a>Megjelenítések csempe#
Vannak háromféle csempe megjelenítések közül választhat:

|diagramtípus|rendeltetés|
|---|---|
|![Sávdiagram](./media/log-analytics-dashboards/oms-dashboards-bar-chart.png)|Jeleníti meg a mentett naplót találatok ütemterv sávdiagram, vagy az eredmények függően a mező listában, ha a napló keresési eredmények mező szerint összesíti, vagy sem.
|![metrikus](./media/log-analytics-dashboards/oms-dashboards-metric.png)|A teljes napló keresési eredmény találatok egy csempére számként jeleníti meg. Metrikus csempék lehetővé teszi, a mozaik kiemeli a küszöb elérésekor küszöb beállítása.|
|![sor](./media/log-analytics-dashboards/oms-dashboards-line.png)|Vonaldiagram jeleníti meg a mentett naplót a keresési eredmény értékű találatok ütemterv.|

### <a name="threshold"></a>Küszöbérték
Egy mozaikon a metrikus megjelenítést használ a küszöbértéket hozhat létre. Adott küszöbértéknél a csempe létrehozásához válassza. Válassza ki, hogy ha az érték vagy csoportban a megadott küszöbértékét keresztül, akkor az alábbi küszöbértéket beállítása, jelölje ki a csempére.

## <a name="organizing-the-dashboard"></a>Az irányítópult rendszerezése
Rendszerezheti az irányítópult, nyissa meg a saját irányítópult nézetet, és kattintson a **Testreszabás** beírása üzemmód testreszabása. Kattintással és húzással a szeretne áthelyezni, és helyezze át, amelyre a csempe kell csempére.

![Az irányítópult rendszerezése](./media/log-analytics-dashboards/oms-dashboards-organize.png)

## <a name="remove-a-tile"></a>Egy mozaikon eltávolítása
Eltávolítása egy csempét, nyissa meg a saját irányítópult nézetet, és kattintson a **Testreszabás** adja meg üzemmód testreszabása. Jelölje ki az eltávolítani kívánt csempére, majd kattintson a jobb oldali panelen válassza a **Csempe eltávolítása**.

![Egy Mozaikon eltávolítása](./media/log-analytics-dashboards/oms-dashboards-remove-tile.png)

## <a name="next-steps"></a>Következő lépések

- [Értesítések](log-analytics-alerts.md) létrehozása napló Analytics értesítések létrehozása és ismételt problémákat.
