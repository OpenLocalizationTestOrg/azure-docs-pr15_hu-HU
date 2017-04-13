<properties
    pageTitle="Forgatókönyvek azonosítása és megtervezése a speciális adatkezelési analytics |} Microsoft Azure"
    description="Fejlett analitikai megtervezése kérdések sorozata figyelembe vételével."
    services="machine-learning"
    documentationCenter=""
    authors="bradsev"
    manager="jhubbard"
    editor="cgronlun" />

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/19/2016"
    ms.author="bradsev" />


# <a name="how-to-identify-scenarios-and-plan-for-advanced-analytics-data-processing"></a>Forgatókönyvek és fejlett analitikai adatkezelési terv azonosítása

Milyen erőforrásokat kell használni kíván jeleníteni, amikor a környezet beállítása a tegye meg egy adatkészlet feldolgozása fejlett analitikai? Ez a cikk javasol kérdések, amelynek segítségével azonosítani a tevékenységek és a kapcsolódó források igényektől sorozata. Magas szintű lépéseket sorrendjének prediktív elemzéséhez keret határolja [a csapat adatok tudományos folyamat (TDSP) tartalma?](data-science-process-overview.md). Ezeket a lépéseket mindegyik igényel, adott erőforrás adott igényektől fontos tevékenységek Az igényektől azonosítása kérdések adatok logisztika, jellemzők, az adatkészleteket, és az eszközök, hajtsa végre az elemzés inkább nyelvek minőségének vonatkoznak.

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="logistic-questions-data-locations-and-movement"></a>Logisztikai kérdések: adatok helye és mozgás
A logisztikai kérdések a helyet, a **adatforrást**, a **cél Megcélozhat** Azure-ban és az adatokat, többek között az ütemezés, a mennyiség és az erőforrások érintett áthelyezése vonatkozó követelmények vonatkoznak. Többször helyezhető át a analytics során szükség lehet az adatokat. A leggyakoribb helyzet, ha a helyi adatok áthelyezése néhány tárolási forma Azure és gépi tanulási Studio be.

1. **Mi az az adatforrást?** Helyi vagy a felhőben, az azt? Példa:
    - HTTP-cím nyilvánosan elérhető adatai.
    - Az adatokat helyi vagy hálózati fájl helyen tárolja.
    - Az adatok olyan SQL Server-adatbázishoz.
    - Az adatok tárolása az Azure tároló tárolóban

2. **Mi az Azure a cél?** Ha nem meg kell lennie feldolgozása vagy modellezése? Példa:
    - Azure Blob-tárolóhoz
    - Az SQL Azure-adatbázisból
    - SQL Server Azure virtuális
    - HDInsight (Azure a Hadoop) vagy a struktúra táblák
    - Azure gépi tanulási
    - Csatlakoztatható Azure virtuális merevlemez.

3. **Hogyan lesz a helyezze át az adatokat?** A folyamatok és erőforrások ingest, illetve adat betöltése számos különböző tárhely és a környezetek feldolgozása az alábbi témakörökben mutatja.

    -  [Adatok betöltése tárterület-verziót tartalmazó környezetek elemzéséhez](machine-learning-data-science-ingest-data.md)
    -  [Az adatimportálás képzés az Azure gépi tanulási Studio különböző adatforrásokból származó](machine-learning-data-science-import-data.md).

4. **Az adatok-nek rendszeres időközönként áthelyezését vagy módosított áttelepítés során?** Fontolja meg inkább Azure adatok gyári (ADF) amikor adatokat kell folyamatosan áttelepített, különösen akkor, ha egy hibrid forgatókönyv mindkét helyszíni segítségével érheti el és felhőalapú erőforrások van szó, vagy ha az adatok feldolgozva van, vagy módosítani kell, vagy az áttelepítendő során hozzáadott logikájának nem kell. További tudnivalókért lásd: a [egy helyszíni SQL server Azure Data Factory az SQL Azure adatainak áthelyezése](machine-learning-data-science-move-sql-azure-adf.md)

5. **Az adatok mekkora Azure kell áthelyezni?** Bizonyos környezetekben kapacitásának, amelyek a nagyon nagy adathalmazok haladhatja meg. Példát tanulmányozza a fájlméretet érintő korlátozásokról gépi tanulási Studio a következő szakaszban. Ebben az esetben egy mintája szerepel az adatok használhatók az elemzés során. További információ arról, hogy hogyan lefelé kétmintás különböző Azure környezetben adatkészletet, akkor olvassa el [az adatok tudományos folyamatban mintaadatokat](machine-learning-data-science-sample-data.md).


## <a name="data-characteristics-questions-type-format-and-size"></a>Adatok jellemzők kérdések: típusa, formázása és mérete
E kérdésekre kulcsfontosságúak a tárhely megtervezése és a környezetben, amelyek megfelelnek a különböző típusú adatokat, és, amelyek van, bizonyos korlátozások feldolgozása.

1. **Mik azok az adattípusok?** Példa:
    - Numerikus
    - A kockák
    - Karakterláncok
    - Bináris

2. **Az adatok formázását?** Példa:
    - Vesszővel tagolt (CSV) vagy tabulátorral tagolt strukturálatlan (TSV)-fájl
    - Próbálta, tömörítve vagy tömörítés nélkül
    - Azure BLOB
    - Táblázatok Hadoop-struktúra
    - SQL Server-táblák

2. **Mekkora az adatok?**
    - Kicsi: 2GB-nál kisebb
    - Közepes: Nagyobb, mint 2 GB-os és 10GB-nál kisebb
    - Nagy: 10GB-nál nagyobb

Az Azure gépi tanulási Studio környezetben például vennie:

- Az adatok formátumok és Azure gépi tanulási Studio, lásd: [támogatott formátumok és adattípusok](machine-learning-data-science-import-data.md#data-formats-and-data-types-supported) szakasz által támogatott fájltípusok listáját.
- Adatok korlátozása az Azure gépi tanulási Studio a további tudnivalókért lásd a **milyen nagy az adatkészlet lehet a modulok?** [importálása és gépi tanulási adatainak exportálása](machine-learning-faq.md#machine-learning-studio-questions) szakasza

Egyéb Azure szolgáltatás a analytics folyamatban használt korlátozások a további tudnivalókért lásd [Azure-előfizetést és a szolgáltatás korlátozások, a kvótákat, és a kényszerek](../azure-subscription-service-limits.md).

## <a name="data-quality-questions-exploration-and-pre-processing"></a>Adatok minőség kérdések: feltárására és előtti feldolgozása

1. **Mi lehet tudni az adatokkal kapcsolatban?** Adatok feltárása, amikor egy ismertetése egyszerű jellemzőit eléréséhez szükséges. Mi minták vagy trendeket, tárgyi bizonyítékokat, Mit nevezünk kiugró van, vagy hány értékek hiányoznak. Ebben a lépésben fontos annak megállapítása, szükség esetén, amelyek alapján a legmegfelelőbb szolgáltatások vagy írjon be egy / elemzés feltételezéseket kidolgozásában és a további adatgyűjtés tervei kidolgozásában előtti feldolgozás mértékét. Leíró statisztika kiszámításához és megrajzolásához a képi megjelenítések adatok vizsgálati hasznos technikákat. Arról, hogy hogyan különböző Azure környezetben adatkészletet feltárása részletekért olvassa el [az adatok tudományos folyamatban adatok feltárása](machine-learning-data-science-explore-data.md).

2. **Az adatok előfeldolgozása, illetve tisztítására kell?**
Előfeldolgozása és adatainak törlése a fontos feladatok, amely általában kell elvégezni, mielőtt adatkészlet használható hatékony gépi tanulási. Nyers adatokból gyakran zajos és mellékletei nem megbízható, és értékek hiányozhat. Ezek az adatok használatának modellezési is eredményt félrevezető. Leírása olvassa el a [továbbfejlesztett gépi adatainak előkészítése feladatok tanulási](machine-learning-data-science-prepare-data.md)elemre.

## <a name="tools-and-languages-questions"></a>Eszközök és nyelvek kapcsolatos kérdések
Vannak olyan sok attól függően, hogy milyen nyelvek és fejlesztői környezet és eszközök vagy szükséges leginkább conformable használja az alábbi lehetőségek közül.

1. **Milyen nyelveken szeretne használni az elemzéshez?**  
    - R
    - Python
    - SQL

2. **Milyen eszközök, használjon adatelemzés?**
    - [Microsoft Azure Powershell](powershell-install-configure.md) - parancsprogram nyelven az Azure erőforrások felügyeletéhez parancsfájl nyelve.
    - [Azure gépi tanulási Studio](machine-learning-what-is-ml-studio/)
    - [Forradalom Analytics](http://www.revolutionanalytics.com/revolution-r-open)
    - [RStudio](http://www.rstudio.com)
    - [Python Tools for Visual Studio](http://microsoft.github.io/PTVS/)
    - [Anaconda](https://www.continuum.io/why-anaconda)
    - [Jupyter jegyzetfüzetek](http://jupyter.org/)
    - [Microsoft Power BI](http://powerbi.microsoft.com)


## <a name="identify-your-advanced-analytics-scenario"></a>Fejlett analitikai igényektől azonosítása
Miután az előző szakaszban a kérdéseket fogadta, készen áll megállapíthatja, hogy melyik forgatókönyv legjobb illeszkedik az eset. A minta esetek a [fejlett analitikai az Azure gépi tanulási forgatókönyvek](machine-learning-data-science-plan-sample-scenarios.md)mutatja.
