<properties
   pageTitle="A mértékek az Azure Service háló töredezettségmentesítését |} Microsoft Azure"
   description="Töredezettségmentesítési használ, vagy másként szolgáltatás háló a mértékek stratégia csomagolóanyag áttekintése"
   services="service-fabric"
   documentationCenter=".net"
   authors="masnider"
   manager="timlt"
   editor=""/>

<tags
   ms.service="Service-Fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/19/2016"
   ms.author="masnider"/>

# <a name="defragmentation-of-metrics-and-load-in-service-fabric"></a>A mértékek és a szolgáltatás háló betöltés töredezettségmentesítését
A szolgáltatás háló fürt erőforrás-kezelő főként foglalkozik értelmez a betöltés terjesztése – gondoskodhat arról, hogy az összes a fürt csomópontok egyaránt kihasználtság terheléselosztás. Ez általában az a legbiztonságosabb és legintelligensebb elrendezés értelmez hibák életben maradt, mivel lehetővé teszi, hogy, hogy az adott meghibásodása meg egy adott terhelést néhány nagy része nem lép. A szolgáltatás háló fürt erőforrás-kezelő támogatja, valamint egy másik stratégia melyiket érdemes töredezettségmentesítési. Töredezettségmentesítési általában azt jelenti, hogy kísérel meg egy mérőszám hasznosítása elosztása a fürt, hanem azt ténylegesen próbálja meg összesítése azt. Ez a normál stratégia – optimalizálás, a kis méretre az egy adott mérőszám metrikus betöltése átlagos szórása alapján fürt helyett egy fortunate inverzió, azt indítsa el az eltérés nő optimalizálás. De miért lehet szükség a stratégia?

Jól Ha Ön már egyenletesen a Betöltés a fürt csomópontjai közötti majd, már fogyasztásra felfelé a forrásokhoz, amelyek a csomópontok kínálatát. Ez általában a nem probléma, de előfordul, hogy bizonyos feladatok létrehozása a szolgáltatásokat, amelyek rendkívül nagy, és felhasználja a csomópont többsége – mondjuk 75 %-os és 95 %, a csomópont erőforrások tenné, amilyennek egyetlen szolgáltatás példányának vagy replika dedikált. Ez a probléma nem, a fürt erőforrás-kezelő észleli azt kell átrendezése a fürt felszabadítása a nagy terhelés és amiatt, hogy akkor történhet meg, hogy a szolgáltatás létrehozáskor, de időközben, hogy a terhelést várnia a fürt ütemezve van.

Tekintve, hogy az új feladatok ütemezés általában legalább egy kis késés bizalmas, ha másképp azt nem tesz semmit azt is előfordul, hogy bAlacsony jobbra által adott SLA szolgáltatások és állapot való navigálás, különösen akkor, ha a fürt munkaterhelésekből mérete általában nagy (és ezért a fürt Navigálás tovább tart,) sok esetén. Sor kerülhet szimulációk valós fürt adatokon alapuló létrehozását időpontok mért azt, amikor bekerül, ha elég nagy szolgáltatások és a fürt meglehetősen lett kihasználtság, hogy szeretné-e nagy szolgáltatások kibocsátása lassú, és, hogy azt javíthatja a töredezettségmentesítési mértékek szabályzatának bevezetésével.

Hasonlóan a fájl létrehozása vagy access sikerült első lassú, ha a számítógép merevlemezére tördelt volt, és így volt nagyobb összefüggő blokkolja a meghajtó töredezettségmentesítésével sikerült kell felgyorsul, beállíthatja, hogy az erőforrás felülete próbál meg ezzel kapcsolatban beérkező kevesebb csomópontok be a betöltés szolgáltatások tömörítheti, úgy, hogy mindig (szinte) a még nagyobb szolgáltatások számára töredezettségmentesítési mérőszámok , amelyek lehetővé teszik gyorsan lehet létrehozni. A legtöbben nem szükséges, mert szolgáltatások általában kis kell lennie, és így nem keresse meg azokat a számára nehezen, de ha nagyméretű szolgáltatásokkal, és szükség szerint csoportokat gyorsan létre (és hajlandó elfogadni a kompromisszumok például megnövelt impactfulness hibák és az egyes erőforrások, miközben ütemezhető munkaterhelésekből várnak megalkotása unutilized), majd a töredezettségmentesítési stratégia szól.

Az alábbi ábrán a két különböző fürt, amely töredezettségmentességét és egyet, amely nem vizuálisan ábrázolhatók adja vissza. A kiegyensúlyozott lehetőséget választja vegye figyelembe a mozgását szükségessé válhat közül a legnagyobb szolgáltatás objektumok helyére, mintha egy új létrehozott, a töredezettségmentesített fürt, ahol azt is azonnal fektetni csomópontok 4-es és 5 és összehasonlítása.

![Megfelel, és fürt töredezettségmentesíteni összehasonlítása][Image1]

## <a name="defragmentation-pros-and-cons"></a>Töredezettségmentesítési előnyei és hátrányai
Úgy Mik ezek más elvi kompromisszumok? Azt javasoljuk, hogy a feladatok alapos mérése töredezettségmentesítési mértékek bekapcsolása előtt. Egy rövid tartalomjegyzék dolog, amit érdemes megfontolni a következő:

| Töredezettségmentesítési szakemberek  | Töredezettségmentesítési Negatívumokat |
|----------------------|----------------------|
|Lehetővé teszi, hogy a nagyméretű szolgáltatások gyorsabb létrehozása | Koncentrátumok feltölteni kevesebb csomópontok kérelem növelése
|Lehetővé teszi, hogy adatokat mozgását alsó létrehozása során.    | Hibák is hatással lehet további szolgáltatások és további tejeskanna okozó
|Lehetővé teszi, hogy a követelmények rich leírása és a térközt visszanyerése | Összetettebb általános erőforrás-kezelés konfigurálása

Akkor is keverjen ki a azonos fürt töredezettségmentesített és a normál mértékek, és az erőforrás-kezelő hajt végre, a legjobb annak érdekében, hogy egy elrendezést a összesíti a töredezettségmentesítési mérési módja miatt annyi szerint is során, próbálja ki a többi széttáró kap. A pontos eredményeket kapja meg a mértékek töredezettségmentesítési mértékek és súlyok, jelenlegi terhelések, stb számú képest terheléselosztás számát függ.

## <a name="configuring-defragmentation-metrics"></a>Töredezettségmentesítési mértékek konfigurálása
Töredezettségmentesítési mértékek konfigurálása a fürt globális döntés, és egyéni mértékek kiválasztott töredezettségmentesítéshez:

ClusterManifest.xml:

```xml
<Section Name="DefragmentationMetrics">
    <Parameter Name="Disk" Value="true" />
    <Parameter Name="CPU" Value="false" />
</Section>
```

## <a name="next-steps"></a>Következő lépések
- Az erőforrás-kezelő fürt túl sok a fürt leíró lehetőséget. Megtudhatja, hogy több róluk nézze meg a [szolgáltatás háló fürt ismertető](service-fabric-cluster-resource-manager-cluster-description.md) cikkben
- Mértékek, hogyan kezeli az szolgáltatás háló fürt erőforrás-kezelő felhasználás és a fürt kapacitása. Ha tudni szeretné őket, és hogyan konfigurálhatók többet tanulmányozza [Ez a cikk](service-fabric-cluster-resource-manager-metrics.md)

[Image1]:./media/service-fabric-cluster-resource-manager-defragmentation-metrics/balancing-defrag-compared.png
