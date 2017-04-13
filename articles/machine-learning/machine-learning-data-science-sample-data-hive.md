<properties
    pageTitle="Példa a Azure HDInsight-struktúra táblában található adatok |} Microsoft Azure"
    description="Azure hdinsight szolgáltatáshoz (Hadopop) struktúra táblában található adatok mintavételi lefelé"
    services="machine-learning,hdinsight"
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

# <a name="sample-data-in-azure-hdinsight-hive-tables"></a>Minta Táblázatadatok megkeresésére Azure HDInsight-struktúra

Ez a cikk azt ismertetik, hogyan lehet lefelé kétmintás struktúra lekérdezésekkel Azure HDInsight-struktúra táblákban tárolt adatok. Háromféleképpen popularly használt mintavételnél foglalkozunk:

* Egységes véletlen mintavételnél
* Véletlen mintavételnél csoportok szerint
* Rétegzett mintavételnél

**Miért példa az adatok?**
Az adatkészlet szeretné elemezni túl nagy méretű, esetén általában célszerű az lefelé kétmintás az adatokat egy kisebb, de képviselő és könnyebben kezelhető méretének csökkentése érdekében. Ez lehetővé teszi az adatok ismertetése, feltárására és szolgáltatás műszaki. A szerepkör a adatok tudományos folyamatban ahhoz, hogy az adatkezelési és gépi tanulási modellek gyors prototípusának elkészítéséhez.

A **menü** alatti, amelyek az adatokat a különböző tároló környezetekben témakörökre mutató hivatkozásokat.

[AZURE.INCLUDE [cap-sample-data-selector](../../includes/cap-sample-data-selector.md)]

A feladat végrehajtásához mintavételnél Ez a lépés a [Csapat adatok tudományos folyamat (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).


## <a name="how-to-submit-hive-queries"></a>Módját struktúra lekérdezések
A központi csomópontra a Hadoop fürt Hadoop parancssori konzolról struktúra lekérdezések küldhető el. Ehhez jelentkezzen be a Hadoop fürt a központi csomópontot, nyissa meg a parancssort Hadoop konzolt, és küldje el a struktúra lekérdezések onnan. A Hadoop parancssori konzolban struktúra lekérdezések elküldése című témakö [struktúra lekérdezések elküldése](machine-learning-data-science-move-hive-tables.md#submit).

## <a name="uniform"></a>Egységes véletlen mintavételnél
Egységes véletlen mintavételnél azt jelenti, hogy az adathalmaz minden egyes sorához éppen mintát egyenlő lehetőségét. Ez hozzáadásával egy további mező rand() az adatkészlet a belső "select" lekérdezést, és a külső "select" lekérdezés adott feltétel véletlen mezőhöz kell végrehajtani.

Íme egy példa a lekérdezésre:

    SET sampleRate=<sample rate, 0-1>;
    select
        field1, field2, …, fieldN
    from
        (
        select
            field1, field2, …, fieldN, rand() as samplekey
        from <hive table name>
        )a
    where samplekey<='${hiveconf:sampleRate}'

Ebben az esetben `<sample rate, 0-1>` adja meg a rekordok, amelyek a felhasználók mintát szeretne arányát.

## <a name="group"></a>Véletlen mintavételnél csoportok szerint

Amikor mintavételnél kockák adatokat, érdemes vagy részösszegekbe minden egyes kockák változó adott érték előfordulásait. Ez a Mit jelent "csoport mintavételnél".
Például ha "Állapot" kockák változó, amelynek értékek NY, MA, hitelesítésszolgáltató, Jersey, PA stb, mindig kell foglalni, hogy azok is mintát vagy nem azonos állam rekordok kívánt.

Íme egy példa a lekérdezésre, hogy a minták csoport:

    SET sampleRate=<sample rate, 0-1>;
    select
        b.field1, b.field2, …, b.catfield, …, b.fieldN
    from
        (
        select
            field1, field2, …, catfield, …, fieldN
        from <table name>
        )b
    join
        (
        select
            catfield
        from
            (
            select
                catfield, rand() as samplekey
            from <table name>
            group by catfield
            )a
        where samplekey<='${hiveconf:sampleRate}'
        )c
    on b.catfield=c.catfield

## <a name="stratified"></a>Rétegzett mintavételnél

Véletlen mintavételnél van műanyaggal rétegezett részletez kockák változó kapott minták esetén értékeit, hogy a kockák, amelyek az ahogy a szülő népesség, ahonnan a minták nyerték arányt. Az előző példát, a fenti tegyük fel, hogy az adatok helytelen alszint populáció államok, NJ 100 észrevételek, NY 60 észrevételek rendelkezik, pedig Postafiók 300 észrevételek mondja ki. Ha ad meg, hogy mennyi díjat kell rétegzett példákat talál arra, hogy 0,5, majd a kapott minta kell rendelkeznie körülbelül 50 30 és 150 észrevételek NJ, NY és Postafiók rendre.

Íme egy példa a lekérdezésre:

    SET sampleRate=<sample rate, 0-1>;
    select
        field1, field2, field3, ..., fieldN, state
    from
        (
        select
            field1, field2, field3, ..., fieldN, state,
            count(*) over (partition by state) as state_cnt,
            rank() over (partition by state order by rand()) as state_rank
        from <table name>
        ) a
    where state_rank <= state_cnt*'${hiveconf:sampleRate}'


Speciális olyan mintavételnél módszerekről struktúra rendelkezésre álló további tudnivalókért lásd: [LanguageManual mintavételnél](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+Sampling).
