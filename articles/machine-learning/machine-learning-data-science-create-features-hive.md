<properties
    pageTitle="Az adatok funkciók létrehozása struktúra lekérdezésekkel Hadoop fürt |} Microsoft Azure"
    description="Példák a szolgáltatások létrehozása az Azure hdinsight szolgáltatáshoz Hadoop fürt tárolt adatok struktúra lekérdezéseket."
    services="machine-learning"
    documentationCenter=""
    authors="bradsev"
    manager="jhubbard"
    editor="cgronlun"  />

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/19/2016"
    ms.author="hangzh;bradsev" />


#<a name="create-features-for-data-in-an-hadoop-cluster-using-hive-queries"></a>Az adatok funkciók létrehozása egy Hadoop fürt struktúra lekérdezésekkel

A dokumentum létrehozása az Azure hdinsight szolgáltatáshoz Hadoop fürt struktúra lekérdezésekkel tárolt adatok funkcióinak mutatja. Ezeket a struktúra lekérdezéseket használja a beágyazott struktúra felhasználói függvényeket (UDF), a parancsfájlok, amelynek találhatók.

A szolgáltatások használatához szükséges műveletek intenzív memória lehet. Struktúra lekérdezéseit válik a több kritikus ebben az esetben, és javítani egyes paraméterek beállítása. A következő paraméterek beállítása a végleges szakasz Budai tárgyalja.

A lekérdezések, amelyek bemutatják példák a [Következőt: Taxi utazás adatok](http://chriswhong.com/open-data/foil_nyc_taxi/) adott esetek is rendelkezésre állnak [Github](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/DataScienceScripts)tárban tárolnak. Ezek a lekérdezések már rendelkezik megadott adatszerkezet és futtatásához benyújtandó készen állnak. A végleges csoportban a paramétereket, amelyek a felhasználók is finomhangolása, így javítható teljesítményének struktúra lekérdezések is tárgyalja.

[AZURE.INCLUDE [cap-create-features-data-selector](../../includes/cap-create-features-selector.md)]Ez a **menüben** , amelyek bemutatják, hogyan hozhat létre az adatok szolgáltatásai különböző környezetekben témakörökre mutató hivatkozásokat tartalmaz. Ezt a műveletet a [Csapat adatok tudományos folyamat (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/)a lépés nem.


## <a name="prerequisites"></a>Előfeltételek
Ez a cikk tartalma feltételezi, hogy:

* Az Azure tároló fiókot hozta létre. Ha utasításokat van szüksége, olvassa el az [Azure tároló fiók létrehozása](../storage/storage-create-storage-account.md#create-a-storage-account)
* A HDInsight szolgáltatáshoz a testre szabott Hadoop fürtre kiépítve.  Ha a utasításokat van szüksége, olvassa el a [Testreszabása Azure hdinsight szolgáltatáshoz Hadoop fürt speciális ábrázolja](machine-learning-data-science-customize-hadoop-cluster.md).
* Az adatok struktúra táblák Azure hdinsight szolgáltatáshoz Hadoop fürt feltöltötte. Ha nem rendelkezik, kövesse a [struktúra táblázatok létrehozása és betöltés adatokat](machine-learning-data-science-move-hive-tables.md) először struktúra táblák tölthet fel adatokat.
* Engedélyezve van a fürt távoli eléréséhez. Ha a utasításokat van szüksége, olvassa el a [hozzáférést a címsor csomópontot, Hadoop fürt](machine-learning-data-science-customize-hadoop-cluster.md#headnode).


##<a name="hive-featureengineering"></a>Szolgáltatás-generálás

Ebben a részben néhány példákat a, amelyben szolgáltatások is lehet generálni struktúra lekérdezésekkel ismertetjük. További funkciókkal hozott létre, miután felveheti oszlopként a meglévő táblához, vagy hozzon létre egy új táblázatot a Továbbiak és elsődleges kulcsot, majd az eredeti táblázatot összekapcsolhatók. Az alábbiakban a példák bemutatják:

1. [Gyakoriság alapú szolgáltatás előállítása](#hive-frequencyfeature)
2. [Bináris osztályozás kockák változót kockázatok](#hive-riskfeature)
3. [Szolgáltatások kinyerése a Datetime mezőben](#hive-datefeatures)
4. [Szolgáltatások kinyerése a szövegmezőben](#hive-textfeatures)
5. [GPS koordináták közötti távolság kiszámítása](#hive-gpsdistance)

###<a name="hive-frequencyfeature"></a>Gyakoriság alapú szolgáltatás előállítása

Érdemes gyakran kockák változó mértékének a gyakoriságok, illetve az egyes szintek a több kockák változók kombinációinak gyakoriságok számítja ki. Felhasználók a következő parancsfájl használatával kiszámítása az alábbi gyakoriságok:

        select
            a.<column_name1>, a.<column_name2>, a.sub_count/sum(a.sub_count) over () as frequency
        from
        (
            select
                <column_name1>,<column_name2>, count(*) as sub_count
            from <databasename>.<tablename> group by <column_name1>, <column_name2>
        )a
        order by frequency desc;


###<a name="hive-riskfeature"></a>Bináris osztályozás kockák változót kockázatok

Bináris osztályozás nem numerikus kockák változók konvertálása numerikus funkciók, amikor a modellek csak használt numerikus szolgáltatások szükség. Ez történik, nem numerikus szintenként numerikus kockázat képre cserélésével. Ebben a részben akkor jelenik meg néhány általános struktúra lekérdezés, amely egy kockák változó (log valószínűleg) kockázat értékeket számíthat ki.


        set smooth_param1=1;
        set smooth_param2=20;
        select
            <column_name1>,<column_name2>,
            ln((sum_target+${hiveconf:smooth_param1})/(record_count-sum_target+${hiveconf:smooth_param2}-${hiveconf:smooth_param1})) as risk
        from
            (
            select
                <column_nam1>, <column_name2>, sum(binary_target) as sum_target, sum(1) as record_count
            from
                (
                select
                    <column_name1>, <column_name2>, if(target_column>0,1,0) as binary_target
                from <databasename>.<tablename>
                )a
            group by <column_name1>, <column_name2>
            )b

Ebben a példában változók `smooth_param1` és `smooth_param2` sima ki az adatokat a kockázat értékek vannak beállítva. Kockázatok -információ és információ közötti tartományt kell. A kockázatok > 0 azt jelzi, hogy a valószínűsége, hogy a célhely egyenlő 1 0,5-nél nagyobb.

A kockázat után táblázat kiszámítása, felhasználók rendelhet a kockázat értékek táblázat a kockázat táblázatot összefűzésével. A struktúra csatlakozó lekérdezés adta meg az előző szakaszban.

###<a name="hive-datefeatures"></a>Szolgáltatások kinyerése DateTime típusú mezők

Egy sor olyan függvényekben DateTime típusú mezők feldolgozásra megtalálható struktúra. Struktúra, alapértelmezett dátum-idő formátuma "yyyy-MM-dd 00:00:00" ("1970-01-01 12:21:32" például). Ebben a részben bemutatjuk példák, amelyek a hónap, a datetime mezőben a hónap napja kibontása és további példa eltérő datetime karakterlánc az alapértelmezett formátum az alapértelmezett formázása, fordított sorrendben karakterlánc datetime formátumban.

        select day(<datetime field>), month(<datetime field>)
        from <databasename>.<tablename>;

Ez a struktúra lekérdezés feltételezi, hogy a *& #60; a dátum és idő mező >* az alapértelmezett datetime formátumban.

Ha a dátum és idő mező nem az alapértelmezett formátumot, kell először a datetime mezőben konvertálása Unix időbélyeg, és alakítsa át a Unix időbélyeg datetime karakterlánc, amely az alapértelmezett formátum. A dátum és idő esetén az alapértelmezett formátum felhasználók alkalmazhat a beágyazott datetime UDF szolgáltatások kibontásához.

        select from_unixtime(unix_timestamp(<datetime field>,'<pattern of the datetime field>'))
        from <databasename>.<tablename>;

Ebben a lekérdezésben Ha a *& #60; a dátum és idő mező >* van, például a mintázat *03/26 és a Skype 2015 12:04:39*, a *"& #60; mintát a dátum és idő mező >"* kell `'MM/dd/yyyy HH:mm:ss'`. Ha tesztelni szeretné azt, felhasználók futtatását is lehetővé teszi.

        select from_unixtime(unix_timestamp('05/15/2015 09:32:10','MM/dd/yyyy HH:mm:ss'))
        from hivesampletable limit 1;

Ebben a lekérdezésben a *hivesampletable* előtelepítve a minden Azure hdinsight szolgáltatáshoz Hadoop fürt alapértelmezés szerint, amikor a fürt kiépített környezetet.


###<a name="hive-textfeatures"></a>Szolgáltatások kinyerése szövegmezői

Amikor a struktúratáblával van egy szövegmező, ahová a szavakat, amelyek szóközöket határolja karakterláncot tartalmazó, az alábbi lekérdezés kibontása a karakterlánc és a szavak száma a karakterlánc hosszát.

        select length(<text field>) as str_len, size(split(<text field>,' ')) as word_num
        from <databasename>.<tablename>;

###<a name="hive-gpsdistance"></a>GPS koordináták adathalmazok közötti távolságokat kiszámítása

A lekérdezés szakaszban említett közvetlenül alkalmazható a következőt: Taxi utazás adatokat. A lekérdezés célja bemutatják, hogyan alkalmazhat egy beágyazott matematikai függvények struktúrában szolgáltatások létrehozásához.

A mezők az alábbi lekérdezés használt, a felvétel és dropoff helyek nevű GPS koordinátáit *Felvétel\_hosszúság*, *Felvétel\_szélesség*, *dropoff\_hosszúság*, és *dropoff\_szélesség*. A lekérdezések, amely kiszámítja a felvétel és dropoff koordináták közvetlen távolságát a következők:

        set R=3959;
        set pi=radians(180);
        select pickup_longitude, pickup_latitude, dropoff_longitude, dropoff_latitude,
            ${hiveconf:R}*2*2*atan((1-sqrt(1-pow(sin((dropoff_latitude-pickup_latitude)
            *${hiveconf:pi}/180/2),2)-cos(pickup_latitude*${hiveconf:pi}/180)
            *cos(dropoff_latitude*${hiveconf:pi}/180)*pow(sin((dropoff_longitude-pickup_longitude)*${hiveconf:pi}/180/2),2)))
            /sqrt(pow(sin((dropoff_latitude-pickup_latitude)*${hiveconf:pi}/180/2),2)
            +cos(pickup_latitude*${hiveconf:pi}/180)*cos(dropoff_latitude*${hiveconf:pi}/180)*
            pow(sin((dropoff_longitude-pickup_longitude)*${hiveconf:pi}/180/2),2))) as direct_distance
        from nyctaxi.trip
        where pickup_longitude between -90 and 0
        and pickup_latitude between 30 and 90
        and dropoff_longitude between -90 and 0
        and dropoff_latitude between 30 and 90
        limit 10;

Péter Lapisu szerzője, amely kiszámítja a két GPS koordináták közötti távolságot a matematikai egyenletek találhatók a <a href="http://www.movable-type.co.uk/scripts/latlong.html" target="_blank">Movable Type parancsfájlok</a> webhelyen. A Javascript, a függvény a `toRad()` van csak *lat_or_lon*pi/180*, amely Fokot radiánná alakít át. Ebben az esetben *lat_or_lon * a szélesség és hosszúság. Mivel a struktúra, a függvény nem nyújtunk hozzá `atan2`, a függvényt tartalmaz, de `atan`, a `atan2` függvény valósítható `atan` függvény használatával a megadott <a href="http://en.wikipedia.org/wiki/Atan2" target="_blank">Wikipedia</a>meghatározása a fenti struktúra lekérdezésben.

![Munkaterület létrehozása](./media/machine-learning-data-science-create-features-hive/atan2new.png)

Egy teljes listát az beágyazott UDF találhatók a **Beépített függvények** szakaszában a <a href="https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF#LanguageManualUDF-MathematicalFunctions" target="_blank">Wikilapok Apache struktúra</a>struktúra).  

## <a name="tuning"></a>Speciális témakörök: finomításához struktúra paraméterek lekérdezés sebességének javítása

Előfordulhat, hogy az alapértelmezett paraméter beállítások struktúra fürt nem alkalmas a struktúra lekérdezések és az adatok, amelyek a lekérdezések feldolgozás alatt. Ebben a részben néhány paramétert, hogy a felhasználók is finomhangolása struktúra lekérdezések teljesítményének javítása ölelik fel. Felhasználók kell adja hozzá a paramétert a lekérdezések adatainak feldolgozása előtt a lekérdezések beállítása.

1. **Java halommemória terület**: csatlakozás nagy adathalmazok vagy hosszú bejegyzések feldolgozása érintő lekérdezések esetén **nincs halommemória hely** az egyik azok a gyakori hibák. A paraméterek *mapreduce.map.java.opts* és *mapreduce.task.io.sort.mb* kívánt értékek megadásával kell állítani. Lássunk egy példát:

        set mapreduce.map.java.opts=-Xmx4096m;
        set mapreduce.task.io.sort.mb=-Xmx1024m;


    A paraméter lefoglalja 4GB memóriát Java halommemória területre, és is teszi a rendezést hatékonyabb által több memóriát oszt ki azt. Érdemes a megoldást lejátszani, ha minden feladatot halommemória terület hiba hibáinak.

2. **Elosztott blokkolása méret** : ezt a paramétert a fájlrendszer tárolja az adatokat a legkisebb mértékegységet állítja be. Szerepel példaként Ha a Elosztott blokk mérete 128MB, majd méretű adatokat kisebb, mint és legfeljebb 128MB vannak tárolva egyetlen blokkokból álló nagyobb, mint 128MB jutó további blokkolja-adatok közben. Válasszon nagyon kicsi blokk méretet okoz a Hadoop nagy költségek, mivel a név csomópontot a fájlhoz tartozó megfelelő Tiltás kereséséhez számos további kérések feldolgozása. A javasolt beállításokat, amikor foglalkozó gigabájt (vagy nagyobb) adatok:

        set dfs.block.size=128m;

3. **Optimalizálása join művelet struktúrában** : közben a térkép/kicsinyítés keretrendszer join művelet általában helyébe a csökkentése fázisban, előfordulhat, hatalmas nyereség érhető el a térkép fázisban (más néven "mapjoins") illesztések ütemezésével. Irányítsa át a struktúra ehhez, amikor csak lehetséges, hogy állíthatók be:

        set hive.auto.convert.join=true;

4. **A struktúra mappers számának megadása** : közben Hadoop lehetővé teszi, hogy a felhasználó adja meg a szűkítő számát, a mappers egy általában nem állítható be a felhasználó által. A trükk, amely lehetővé teszi, hogy bizonyos fokig ennyi vezérlőelemének azt javasoljuk, hogy adja meg a Hadoop változók, *mapred.min.split.size* és *mapred.max.split.size* egyes térkép tevékenységek méretének határozza meg:

        num_maps = max(mapred.min.split.size, min(mapred.max.split.size, dfs.block.size))

    Az alapértelmezett érték *mapred.min.split.size* általában 0, amely *mapred.max.split.size* **Long.MAX** pedig, *dfs.block.size* 64 MB. Ahogy a képen látható, az adatok mérete a megadott "a"beállítás a paraméterek beállítása őket lehetővé teszi, hogy nekünk finomhangolása használt mappers számát.

5. Néhány egyéb további **Speciális beállítások** optimalizálása struktúra teljesítmény megemlítik alatt. Ezek lehetővé teszik a memória megfeleltetése és csökkentheti a tevékenységekhez rendelt, és a teljesítmény színcsúszkákkal hasznos lehet. Kérjük, ne feledje, hogy a *mapreduce.reduce.memory.mb* nem lehet nagyobb, mint a Hadoop fürt minden dolgozó csomópontjának fizikai memória méretét.

        set mapreduce.map.memory.mb = 2048;
        set mapreduce.reduce.memory.mb=6144;
        set mapreduce.reduce.java.opts=-Xmx8192m;
        set mapred.reduce.tasks=128;
        set mapred.tasktracker.reduce.tasks.maximum=128;
