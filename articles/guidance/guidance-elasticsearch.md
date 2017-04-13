
<properties
   pageTitle="Azure útmutatásának Elasticsearch |} Microsoft Azure"
   description="Azure útmutatásának Elasticsearch."
   services=""
   documentationCenter="na"
   authors="dragon119"
   manager="bennage"
   editor=""
   tags=""/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/22/2016"
   ms.author="masashin"/>

# <a name="elasticsearch-on-azure-guidance"></a>Azure útmutatásának Elasticsearch 

[AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

Elasticsearch pedig egy erősen méretezhető Megnyitás-forrás keresőmotor adatbázis. Célszerű gyors elemzése és nagy adatkészletek tárolt információk felfedezése igénylő helyzeteket. Ez az útmutató vizsgálja meg néhány főbb tényezők egy Elasticsearch fürthöz módszert, amellyel optimalizálása, és tesztelje a konfigurációt a kiemelve tervezésekor figyelembe.

> [AZURE.NOTE]Ez az útmutató azt feltételezi, hogy néhány egyszerű ismerős Elasticsearch. Részletesebb információkért keresse fel a [Elasticsearch webhely](https://www.elastic.co/products/elasticsearch). 

- **[Futó Elasticsearch Azure a][]** egy rövid bevezető Elasticsearch általános szerkezetének, és leírja, hogy miként végrehajtása egy Elasticsearch fürthöz Azure használatával. 
- **[Hangolása adatok bevitel teljesítményt Elasticsearch Azure a][]** ismerteti a példányban Elasticsearch fürt és a különféle beállítási lehetőségeket is célszerű figyelembe venni, amikor adatokat bevitel nagy mértékű várt mélyreható elemzés.
- **[Hangolása adatok összesítése és Elasticsearch Azure a lekérdezési teljesítményt][]** biztosít mélyreható elemzést figyelembe beállítások optimalizálása a rendszer a lekérdezést, és keressen teljesítményt kiválasztásához.
- Néhány fontos funkciókat, amelyek az adatok elvesztését és a bővített adatok helyreállítás időpontok esélye minimálisra Elasticsearch fürt **[ellenállóképességére és helyreállítási Elasticsearch Azure kattintson a konfigurálás][]** ismerteti.
- **[A teljesítmény Elasticsearch Azure a tesztelés környezet létrehozása][]** ismerteti, hogyan állíthatja be a teljesítmény adatok bevitel és a lekérdezés munkaterhelésekből teszteléshez Elasticsearch fürt-környezet. 
- Próba-csomagok JMeter együtt mint például a Elasticsearch fürt való adatfeltöltés feladatokat végző JUnit próba beépített Java-kód használatával megvalósított futó teljesítmény tesztek **[végrehajtása egy JMeter tesztelése Elasticsearch megtervezése][]** foglalja össze.
- **[Üzembe helyezése a JMeter JUnit bemutató Elasticsearch teljesítmény teszteléshez][]** ismerteti, hogyan hozhat létre és használhat egy JUnit bemutató, amely készíthet, és egy Elasticsearch fürthöz feltölteni az adatokat. Ezzel a megoldással a rugalmas megközelítés betöltése tesztelése, hogy a nagy mennyiségű tesztadatokat nélkül attól függően, hogy a külső adatok fájlokat hozhat létre. 
- **[Az automatikus Elasticsearch tűrőképessége vizsgálatok futtatása][]** összefoglalja, hogy miként futtassa a tűrőképessége teszteket a sorozatban felhasznált. 
- **[Az automatikus Elasticsearch teljesítmény tesztek futó][]** összefoglalja, hogy miként futtassa a teljesítmény teszteket a sorozat használt.


[Azure Elasticsearch fut]: guidance-elasticsearch-running-on-azure.md
[Adatok bevitel Elasticsearch Azure a teljesítmény javítása]: guidance-elasticsearch-tuning-data-ingestion-performance.md
[A környezet vizsgálata Elasticsearch Azure a teljesítmény létrehozása]: guidance-elasticsearch-creating-performance-testing-environment.md
[Elasticsearch JMeter Teszttervre végrehajtása]: guidance-elasticsearch-implementing-jmeter-test-plan.md
[Üzembe helyezése a JMeter JUnit bemutató Elasticsearch teljesítmény tesztelése]: guidance-elasticsearch-deploying-jmeter-junit-sampler.md
[Adatok összesítése és Elasticsearch Azure a lekérdezési teljesítmény javítása]: guidance-elasticsearch-tuning-data-aggregation-and-query-performance.md
[A Azure Elasticsearch ellenállóképességére és helyreállítási konfigurálása]: guidance-elasticsearch-configuring-resilience-and-recovery.md
[Az automatikus Elasticsearch Tűrőképessége vizsgálatok futtatása]: guidance-elasticsearch-running-automated-resilience-tests.md
[Az automatizált Elasticsearch teljesítmény vizsgálatok futtatása]: guidance-elasticsearch-running-automated-performance-tests.md
