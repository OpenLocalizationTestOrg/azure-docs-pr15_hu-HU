<properties
    pageTitle="Real-egyszer-stat az Azure CDN |} Microsoft Azure"
    description="Valós idejű statisztika Azure CDN teljesítményéről valós idejű adatok szolgál, az ügyfelek tartalom során."
    services="cdn"
    documentationCenter=""
    authors="camsoper"
    manager="erikre"
    editor=""/>

<tags
    ms.service="cdn"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/28/2016"
    ms.author="casoper"/>

# <a name="real-time-stats-in-microsoft-azure-cdn"></a>A Microsoft Azure CDN valós idejű statisztika

[AZURE.INCLUDE [cdn-premium-feature](../../includes/cdn-premium-feature.md)]

## <a name="overview"></a>– Áttekintés

Ez a dokumentum a Microsoft Azure CDN valós idejű stat ismerteti.  Ezt a funkciót tartalmaz a valós idejű adatokat, például a sávszélesség, gyorsítótár állapotok és egyidejű kapcsolatok CDN-profilját, tartalmat az ügyfelek során. Folyamatos nyomon követését, a szolgáltatás állapotát jelző bármikor, többek között az események éles szolgáltatás lehetővé teszi.

A következő grafikonok állnak rendelkezésre:

* [Sávszélességre](#bandwidth)
* [Állapot kódok](#status-codes)
* [Gyorsítótár állapotok](#cache-statuses)
* [Kapcsolatok](#connections)


## <a name="accessing-real-time-stats"></a>Valós idejű stat elérése

1. Az [Azure-portálon](https://portal.azure.com)nyissa meg a CDN-profil.

    ![CDN-profil lap](./media/cdn-real-time-stats/cdn-profile-blade.png)

2. Az a CDN a profil lap a **kezelés** gombra.

    ![CDN-profil lap kezelése gomb](./media/cdn-real-time-stats/cdn-manage-btn.png)

    Ekkor megnyílik a CDN kezelőportálja segítségével.

3. Mutasson rá az **analitikai** fülre, majd mutasson a **Valós idejű stat** előugró.  Kattintson a **nagy HTTP-objektumot**.

    ![CDN-adatkezelési portál](./media/cdn-real-time-stats/cdn-premium-portal.png)

    A valós idejű stat grafikonok jelennek meg.
    
A kijelölt időszak, kezdve az oldal betöltődésekor valós idejű statisztika egyes a diagramok megjelenítése.  A diagramok frissítése automatikusan, minden néhány másodpercet.  A **Diagram frissítése** gombra, ha jelen van, törli a diagram, amely után csak jeleníti meg a kijelölt adatok.

## <a name="bandwidth"></a>A sávszélesség

![Sávszélesség-diagram](./media/cdn-real-time-stats/cdn-bandwidth.png)

A **sávszélesség** -diagramon a kijelölt időtartomány fölé a jelenlegi platform használt sávszélességet láthatók. A diagram árnyékolt részét azt jelzi, hogy a sávszélesség-használat. A használatban lévő sávszélesség pontos összegét közvetlenül a vonaldiagram alatt jelenik meg.

## <a name="status-codes"></a>Állapot kódok

![Állapotdiagram kódot.](./media/cdn-real-time-stats/cdn-status-codes.png)

Az **Állapot kódok** graph azt jelzi, hogy milyen gyakran bizonyos HTTP válasz kódok fölé kijelölt időtartomány fordul elő.

> [AZURE.TIP]  Egyes HTTP állapot kód beállítások leírását olvassa el a [Azure CDN HTTP állapot kódok](https://msdn.microsoft.com/library/mt759238.aspx)című témakört.

HTTP állapot kódok listáját közvetlenül a diagram felett jelenik meg. Ebben a listában azt jelzi, hogy minden állapotkód, amely a vonaldiagram és az aktuális adott állapotkód másodpercenként előfordulások kell foglalni. Alapértelmezés szerint a következő állapot kódokat a diagramon, minden egyes vonal jelenik meg. Jó helyen jár csak Lync-állapot kód, amely a felhasználó számára különleges jelentőséggel az a CDN-konfigurációs választhat. Ehhez jelölje be a megfelelő állapotot kódok törölje az összes többi beállítást, majd kattintson a **Diagram frissítése**. 

Az egy adott állapotkód naplózott adatok átmenetileg elrejtheti.  A jelmagyarázat közvetlenül a diagram alatt kattintson az elrejteni kívánt állapotkód. A állapotkód azonnal rejtve marad le. Kattintson újra a állapotkód okoz ismét megjeleníteni ezt a beállítást.

## <a name="cache-statuses"></a>Gyorsítótár állapotok

![Gyorsítótár állapotok graph](./media/cdn-real-time-stats/cdn-cache-status.png)

A **Gyorsítótár állapotok** grafikon azt jelzi, hogy milyen gyakran gyorsítótár állapotok bizonyos típusú fölé kijelölt időtartomány fordul elő. 

> [AZURE.TIP]  Egyes gyorsítótár állapot kód beállítások leírását olvassa el a [Azure CDN-gyorsítótár állapot kódok](https://msdn.microsoft.com/library/mt759237.aspx)című témakört.

Gyorsítótár állapot kódok listáját közvetlenül a diagram felett jelenik meg. Ebben a listában azt jelzi, hogy minden állapotkód, amely a vonaldiagram és az aktuális adott állapotkód másodpercenként előfordulások kell foglalni. Alapértelmezés szerint minden egyes e állapot kódok a diagramban vonal jelenik meg. Jó helyen jár csak Lync-állapot kód, amely a felhasználó számára különleges jelentőséggel az a CDN-konfigurációs választhat. Ehhez jelölje be a megfelelő állapotot kódok törölje az összes többi beállítást, majd kattintson a **Diagram frissítése**. 

Az egy adott állapotkód naplózott adatok átmenetileg elrejtheti.  A jelmagyarázat közvetlenül a diagram alatt kattintson az elrejteni kívánt állapotkód. A állapotkód azonnal rejtve marad le. Kattintson újra a állapotkód okoz ismét megjeleníteni ezt a beállítást.

## <a name="connections"></a>Kapcsolatok

![Kapcsolatok a diagram](./media/cdn-real-time-stats/cdn-connections.png)

Ez a diagram azt jelzi, hogy hány kapcsolatok bevezetett peremhálózat-kiszolgálói. Minden egyes tárgyi eszköz, amely a kapcsolat CDN eredményez áthaladt kérelem.

## <a name="next-steps"></a>Következő lépések

- Értesítés az [Azure CDN valós idejű riasztásai](cdn-real-time-alerts.md)
- Alaposabban az [speciális HTTP-jelentések](cdn-advanced-http-reports.md)
- [Szokásai](cdn-analyze-usage-patterns.md) elemzése

