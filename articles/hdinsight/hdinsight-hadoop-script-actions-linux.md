<properties
    pageTitle="Parancsfájl-Linux-alapú HDInsight a művelet fejlesztési |} Microsoft Azure"
    description="Hogyan lehet Linux-alapú HDInsight fürt parancsfájl művelettel testreszabása. Parancsfájl-műveletek is Azure hdinsight szolgáltatáshoz fürt testreszabása fürt konfigurációs beállítások megadásával, és további szolgáltatásról, eszközök és más szoftverek telepítése a fürt. "
    services="hdinsight"
    documentationCenter=""
    authors="Blackmist"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/05/2016"
    ms.author="larryfr"/>

# <a name="script-action-development-with-hdinsight"></a>Parancsfájl művelet fejlesztési a hdinsight szolgáltatáshoz

Parancsfájl-műveletek is Azure hdinsight szolgáltatáshoz fürt testreszabása fürt konfigurációs beállítások megadásával, és további szolgáltatásról, eszközök és más szoftverek telepítése a fürt. Parancsfájl-műveletek csoport létrehozása során, vagy a futó fürthöz használható.

> [AZURE.NOTE] A dokumentum adatai Linux-alapú HDInsight fürt alkalmazásra. Windows-alapú fürt való használatáról a parancsfájl-műveletek további tudnivalókért lásd [parancsfájl művelet fejlesztési a HDInsight (Windows)](hdinsight-hadoop-script-actions.md).

## <a name="what-are-script-actions"></a>Mik azok a parancsfájl-műveletek

Parancsfájl-műveletek Bash parancsfájlok futó Azure fürt csomópontok konfigurációs módosíthassa vagy telepítse a szoftvert. A parancsprogram művelet legfelső szintű megy végbe, és a teljes hozzáférés jogosultsággal, és a fürt csomópontok.

Parancsfájl-műveletek alkalmazhatók keresztül az alábbi módszereket:

| Ezzel a paranccsal parancsfájl alkalmazása... | Során fürt létrehozási... | A futó fürthöz... |
| ----- |:-----:|:-----:|
| Azure portál | ✓ | ✓ |
| Azure PowerShell | ✓ | ✓ |
| Azure CLI | &nbsp; | ✓ |
| .NET-SDK hdinsight szolgáltatáshoz | ✓ | ✓ |
| Erőforrás-kezelő Azure-sablon | ✓ | &nbsp; |

Használatáról bővebben a módszerekről az alábbi parancsfájl-műveletek alkalmazásához olvassa el a [testreszabása HDInsight fürt parancsfájl-műveletek használata](hdinsight-hadoop-customize-cluster-linux.md)című témakört.

## <a name="bestPracticeScripting"></a>Gyakorlati tanácsok a parancsprogram fejlesztési

Egyéni parancsfájl-HDInsight fürt fejlesztésekor létezik néhány gyakorlati tanácsok, amelyre érdemes figyelni:

- [Cél: a Hadoop-verziója](#bPS1)
- [Cél: az operációs rendszer verziója](#bps10)
- [Adja meg az állandó forrásokra mutató hivatkozásokat parancsfájl](#bPS2)
- [Előre lefordított források](#bPS4)
- [Győződjön meg arról, hogy a fürt testreszabási parancsfájl idempotent](#bPS3)
- [Magas rendelkezésre állásának a fürt-architektúra](#bPS5)
- [Állítsa be az egyéni összetevők Azure Blob-tároló használatára](#bPS6)
- [Információ STDOUT és STDERR írása](#bPS7)
- [Fájlok mentése ASCII LF sorvégződések együtt](#bps8)
- [Az ideiglenes (tranziens) hibák visszaállításához használandó újraküldés logikája](#bps9)

> [AZURE.IMPORTANT] Parancsfájl-műveletek végre kell hajtania 60 percen belül, illetve azok fog időtúllépés. Csomópont-kiépítése során a parancsfájl párhuzamosan más beállítás és konfiguráció folyamatok van futtatta. Versenyhelyzetből erőforrások, például a Processzor-és időértékek hálózati sávszélesség jelenhet meg a parancsfájlt, mint a fejlesztői környezet fejeződik.

### <a name="bPS1"></a>Cél: a Hadoop-verziója

Különböző verziói HDInsight rendelkezik Hadoop-szolgáltatások és -összetevők telepítése különböző verziói. A parancsfájlok egy adott verziójához szolgáltatás vagy összetevő vár, ha csak kell használni a parancsfájlt, amely tartalmazza a szükséges összetevők HDInsight verziójával. HDInsight részét képező összetevő verzióján információt találhat, használja a [HDInsight-összetevő verziószámozás](hdinsight-component-versioning.md) dokumentumot.

###<a name="bps10"></a>Célalkalmazás az operációs rendszer verziója

Linux-alapú HDInsight a Ubuntu Linux eloszlás alapján. Különböző verziói HDInsight Ubuntu, amely hatással lehetnek a parancsprogram viselkedését különböző verziói támaszkodhat. HDInsight 3.4 és korábbi verziók például Ubuntu verzióit használó Upstart alapulnak. 3.5-ös verzió Ubuntu 16.04, amelyet használ Systemd alapul. Systemd és Upstart támaszkodik különböző parancsokat, hogy a parancsprogram is dolgozhat kell írni.

Egy másik fontos HDInsight 3.4 és 3.5-ös közötti különbség, amely `JAVA_HOME` Java 8 most mutat.

Az operációs rendszer verziója ellenőrizheti a `lsb_release`. A szín telepítés forgatókönyv a következő kódtöredék bemutatja, hogyan állapítható meg, ha a parancsfájl Ubuntu 14 vagy 16 fut:

    OS_VERSION=$(lsb_release -sr)
    if [[ $OS_VERSION == 14* ]]; then
        echo "OS verion is $OS_VERSION. Using hue-binaries-14-04."
        HUE_TARFILE=hue-binaries-14-04.tgz
    elif [[ $OS_VERSION == 16* ]]; then
        echo "OS verion is $OS_VERSION. Using hue-binaries-16-04."
        HUE_TARFILE=hue-binaries-16-04.tgz
    fi
    ...
    if [[ $OS_VERSION == 16* ]]; then
        echo "Using systemd configuration"
        systemctl daemon-reload
        systemctl stop webwasb.service    
        systemctl start webwasb.service
    else
        echo "Using upstart configuration"
        initctl reload-configuration
        stop webwasb
        start webwasb
    fi
    ...
    if [[ $OS_VERSION == 14* ]]; then
        export JAVA_HOME=/usr/lib/jvm/java-7-openjdk-amd64
    elif [[ $OS_VERSION == 16* ]]; then
        export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64
    fi

A teljes parancsfájl, amely tartalmazza ezeket a https://hdiconfigactions.blob.core.windows.net/linuxhueconfigactionv02/install-hue-uber-v02.sh kódrészletek is megkeresheti.

Ubuntu HDInsight által használt verzióját a [HDInsight-összetevő verziójú](hdinsight-component-versioning.md) dokumentum című cikkben találhat.

Systemd és Upstart közötti különbségek, című cikkben talál részletes [Systemd Upstart felhasználók számára](https://wiki.ubuntu.com/SystemdForUpstartUsers).

### <a name="bPS2"></a>Adja meg az állandó forrásokra mutató hivatkozásokat parancsfájl

Győződjön meg arról, hogy a parancsprogramokat és a parancsfájl által használt erőforrások továbbra is elérhető a fürt időtartama, és, ezeket a fájlokat a verziói ne módosítsa az időtartam. Ezek az erőforrások szükség, ha új csomópontok bekerülnek a fürthöz méretezés műveletek során.

Célszerű, töltse le és az Azure tároló fiókkal az előfizetés a mindent archivál.

> [AZURE.IMPORTANT] A tárterület-fiókkal az alapértelmezett tároló fiókja a fürt vagy egy nyilvános, csak olvasható tároló további tárterület-fiók kell lennie.

A Microsoft által nyújtott minták például egy nyilvános, csak olvasható tároló a HDInsight csoportja által fenntartott [https://hdiconfigactions.blob.core.windows.net/](https://hdiconfigactions.blob.core.windows.net/) tároló fiók tárolja.

### <a name="bPS4"></a>Előre lefordított források

Futtassa a szükséges idő csökkentése érdekében, ne műveletek összeállítása forráskód erőforrásokat. Ehelyett előre állíthat össze az erőforrásokat, és tárolhatja a bináris verzió Azure Blob-tárolóhoz, hogy gyorsan letölthető a fürthöz a forgatókönyv.

### <a name="bPS3"></a>Győződjön meg arról, hogy a fürt testreszabási parancsfájl idempotent

Parancsfájlok kell megtervezni, lehet, hogy a parancsprogram Ha futtatta többször értelemben idempotent, biztosítsa, hogy a fürt visszakerül ugyanabban az állapotban minden alkalommal, amikor azt van futtatta.

Például ha egyéni parancsfájl-/usr/local/bin alkalmazás telepíti a első futtatása, majd a minden későbbi futtatás a parancsfájl ellenőriznie kell hogy az alkalmazás már létezik az /usr/local/bin helyen előtt más eljárás lépéseit a parancsfájl.

### <a name="bPS5"></a>Magas rendelkezésre állásának a fürt-architektúra

HDInsight fürt Linux-alapú biztosít, amelyek a fürt belül aktív két központi csomópontot, és műveletek vannak parancsfájl futtatta mindkét csomópontok. Az alkatrészek telepíti csak egy központi csomópont számíthat, ha egy parancsfájlt, amely a program csak a telepítésekor a fürt két központi csomópontját egyik kell tervezésekor.

> [AZURE.IMPORTANT] Alapértelmezett szolgáltatások HDInsight részeként telepítette átadni a két központi csomópontok között, ha szükséges, azonban ez a funkció nem bővített egyéni parancsfájl ctions keresztül telepítette-összetevők szolgálnak. Ha az összetevők telepítése nagyon elérhetővé szeretné tenni a parancsprogram művelet keresztül, be kell állítani a saját feladatátvevő mechanizmusa, amely a két rendelkezésre álló központi csomópontok használja.

### <a name="bPS6"></a>Állítsa be az egyéni összetevők Azure Blob-tároló használatára

A fürt telepített összetevők esetleg alapértelmezett beállításokkal által használt tárterület Hadoop elosztott fájl rendszer (hdfs) lehetőségre. HDInsight használja az alapértelmezett tároló Azure Blob-tároló (WASB). Ez egy Fájlrendszerhez kompatibilis fájlrendszert biztosít, amelyek az adatok továbbra is fennáll, akkor is, ha a fürt törlődik. Konfigurálja a összetevők WASB Fájlrendszerhez helyett használandó telepítését.

Ha például az alábbi a giraph-examples.jar fájl másolása a helyi fájlrendszerből WASB:

    hadoop fs -copyFromLocal /usr/hdp/current/giraph/giraph-examples.jar /example/jars/

### <a name="bPS7"></a>Információ STDOUT és STDERR írása

Parancsfájl végrehajtás során STDOUT és STDERR írt adatok be van jelentkezve, és a Ambari webes felhasználói felület használatával megtekintheti.

> [AZURE.NOTE] Ambari csak akkor érhető el, ha a fürt sikeresen jön létre. Ha egy parancsprogramot művelettel fürt létrehozása során, és nem sikerül létrehozni, című hibaelhárítási [testreszabása HDInsight fürtök parancsfájl művelettel](hdinsight-hadoop-customize-cluster-linux.md#troubleshooting) szolgáltatást mellőző formáit férnek hozzá a naplózott adatok.

A legtöbb telepítési csomagok és segédprogramok már ír információk STDOUT és STDERR, azonban előfordulhat, hogy szeretne további naplózás. Szöveg küldeni STDOUT használata `echo`. Példa:

        echo "Getting ready to install Foo"

Alapértelmezés szerint `echo` STDOUT karakterláncot küldi. Irányítsa át, hogy a ezzel hozzáadása `>&2` előtt `echo`. Példa:

        >&2 echo "An error occurred installing Foo"

Ez átirányítja STDOUT (1, így itt nem szereplő alapértelmezés) küldött adatok, ezzel (2). IO átirányítással kapcsolatos további tudnivalókért olvassa el a [http://www.tldp.org/LDP/abs/html/io-redirection.html](http://www.tldp.org/LDP/abs/html/io-redirection.html)című témakört.

Parancsfájl-műveletek során adatainak megjelenítése a további tudnivalókért olvassa el a [parancsfájl művelettel testreszabása HDInsight fürt](hdinsight-hadoop-customize-cluster-linux.md#troubleshooting) című témakört.

###<a name="bps8"></a>Fájlok mentése ASCII LF sorvégződések együtt

Bash parancsfájlok LF megszakítja vonallal ASCII-formátumban kell tárolni. Ha a fájlok vannak tárolva lehetnek egy bájt sorrendben be van jelölve a fájl, vagy sorvégződések CRLF, amely a Windows-szerkesztők közös, az elején UTF-8 majd a parancsfájl meghiúsul hibákkal, az alábbihoz hasonló:

    $'\r': command not found
    line 1: #!/usr/bin/env: No such file or directory

###<a name="bps9"></a>Az ideiglenes (tranziens) hibák visszaállításához használandó újraküldés logikája

Csomagok apt kérése vagy egyéb műveleteket, amelyek az adatok továbbítja az interneten keresztül, telepítése, fájlok letöltése a művelet tranziens hálózati hibák miatt sikertelen lehet. A távoli erőforrás kommunikál, például lehet hibás keresztül biztonsági a csomópont a webhelyet.

Hogy a parancsprogram rugalmassá tranziens hibákat, az újraküldés logikája alkalmazhat. Az alábbi képen egy függvény, amely bármely parancsot átadott rá (Ha a parancs nem sikerül,), próbálja meg akár háromszor. Várakozik, hogy a két másodperc minden újrapróbálkozási között.

    #retry
    MAXATTEMPTS=3

    retry() {
        local -r CMD="$@"
        local -i ATTMEPTNUM=1
        local -i RETRYINTERVAL=2

        until $CMD
        do
            if (( ATTMEPTNUM == MAXATTEMPTS ))
            then
                    echo "Attempt $ATTMEPTNUM failed. no more attempts left."
                    return 1
            else
                    echo "Attempt $ATTMEPTNUM failed! Retrying in $RETRYINTERVAL seconds..."
                    sleep $(( RETRYINTERVAL ))
                    ATTMEPTNUM=$ATTMEPTNUM+1
            fi
        done
    }

Az alábbi példák ezzel a függvénnyel.

    retry ls -ltr foo

    retry wget -O ./tmpfile.sh https://hdiconfigactions.blob.core.windows.net/linuxhueconfigactionv02/install-hue-uber-v02.sh

## <a name="helpermethods"></a>Egyéni parancsfájlok segítő módszerei

parancsfájl művelet segítő módszereket, amelyekkel egyéni parancsfájlok írása közben segédprogramot. Ezek [https://hdiconfigactions.blob.core.windows.net/linuxconfigactionmodulev01/HDInsightUtilities-v01.sh](https://hdiconfigactions.blob.core.windows.net/linuxconfigactionmodulev01/HDInsightUtilities-v01.sh)definiálva, és beépíthetők a parancsfájlok, használja az alábbiakat:

    # Import the helper method module.
    wget -O /tmp/HDInsightUtilities-v01.sh -q https://hdiconfigactions.blob.core.windows.net/linuxconfigactionmodulev01/HDInsightUtilities-v01.sh && source /tmp/HDInsightUtilities-v01.sh && rm -f /tmp/HDInsightUtilities-v01.sh

Ez a következő segítőket elérhetővé teszi használatra a parancsprogram:

| Segítő használatát | Leírás |
| ------------ | ----------- |
| `download_file SOURCEURL DESTFILEPATH [OVERWRITE]` | A forrás URL-címe a letölti egy fájlt a megadott fájl elérési útját. Alapértelmezés szerint ne írja felül a meglévő fájl. |
| `untar_file TARFILE DESTDIR` | Az első karaktertől tar fájl (használatával `-xf`,) a cél címtárhoz. |
| `test_is_headnode` | Ha futtatta csomóponton egy központi, eredménye 1; minden egyéb esetben pedig 0. |
| `test_is_datanode` | Ha az aktuális csomópont adatok (dolgozó) csomópontot, eredménye 1; minden egyéb esetben pedig 0. |
| `test_is_first_datanode` | Ha az aktuális csomópont (más néven workernode0) első adatok (dolgozó) csomópontot, eredménye 1; minden egyéb esetben pedig 0. |
| `get_headnodes` | A fürt a tartománynevét, a headnodes adja eredményül. Nevek vesszővel tagolt. A függvény üres karakterláncot hiba ad vissza. |
| `get_primary_headnode` | A tartománynevét, az elsődleges headnode kap. A függvény üres karakterláncot hiba ad vissza. |
| `get_secondary_headnode` | A tartománynevét, a másodlagos headnode kap. A függvény üres karakterláncot hiba ad vissza. |
| `get_primary_headnode_number` | A numerikus utótag, az elsődleges headnode kap. A függvény üres karakterláncot hiba ad vissza. |
| `get_secondary_headnode_number` | A numerikus utótag, a másodlagos headnode kap. A függvény üres karakterláncot hiba ad vissza. |

## <a name="commonusage"></a>Gyakori szokásai

Ebben a részben néhány saját egyéni parancsfájl írása közben fellépő esetleges előfordulhat, hogy a közös használat mintákat megvalósításáról ismerteti.

### <a name="passing-parameters-to-a-script"></a>Paraméterek átadása parancsfájl

Bizonyos esetekben a parancsprogram szükség lehet a paraméterek. Előfordulhat például, a rendszergazdai jelszavát a fürt annak érdekében, hogy az adatok kinyerése a Ambari REST API-t.

A parancsprogram átadott _pozíciójelző paraméterek_neve és vannak rendelve `$1` az első paraméterként `$2` a második, és így-on. `$0`a parancsprogram magát a nevét tartalmazza.

Aposztrófok (') kell szögletes értékek paraméterként a parancsfájl, így az átadott érték van kezeli az adott szövegkonstans, és a speciális kezelés megadott nem található karakterek, például "!".

### <a name="setting-environment-variables"></a>Változók környezet beállítása

Egy környezeti változóba beállítása hajtja végre a következőket:

    VARIABLENAME=value

Hol VARIABLENAME a változó nevét. Ezt követően a változó elérésére `$VARIABLENAME`. Ha például JELSZÓT nevű környezeti változó pozíciójelző paraméter által megadott értéket rendeli, használja a következő:

    PASSWORD=$1

Tudta majd használja a későbbi hozzáférés az adatokhoz való `$PASSWORD`.

A parancsfájl belül környezeti változók csak létezik a parancsfájl hatálya alá. Egyes esetekben előfordulhat, hogy a parancsprogram befejeződése után áll fenn a rendszer széles környezet változói hozzáadása. Általában az, hogy a fürt SSH keresztül csatlakozó felhasználók is használhassák a parancsfájl által telepített összetevőket. Ez a környezeti változó hozzáadásával elvégezhető `/etc/environment`. Ha például a következő hozzáadja __HADOOP\_CONF\_DIR__:

    echo "HADOOP_CONF_DIR=/etc/hadoop/conf" | sudo tee -a /etc/environment

### <a name="access-to-locations-where-the-custom-scripts-are-stored"></a>Az Access az egyéni parancsfájlok tároló helyekre

Fürt testreszabásához használt parancsfájlokat van szüksége bármelyik akkor alapértelmezett tároló fiókja a fürt vagy, ha a tárhely másik fiókba, nyilvános csak olvasható tárolóban. Ha a parancsprogram fér hozzá az erőforrások máshol található ezek is kell lennie a nyilvánosan hozzáférhető (legalább nyilvános csak olvasható). Például érdemes lehet letölteni egy fájlt a fürt használatával `download_file`.

A kisegítő lehetőségekkel kiegészített Azure tároló fiók fájlt tárolja a fürt (például az alapértelmezett tároló fiók), gyors hozzáférést biztosít, mint a tárhely az Azure hálózaton belül.

### <a name="checking-the-operating-system-version"></a>Az operációs rendszer verziójának ellenőrzése

Különböző verziói HDInsight Ubuntu különböző verzióiban támaszkodhat. Lehet, hogy a parancsprogram ellenőriznie OS verziók közötti különbségek. Előfordulhat például, Ubuntu verziója van kötve bináris telepítéséhez.

Jelölje be az operációs rendszer verziója, használja a `lsb_release`. Ha például az alábbi bemutatja, hogyan lehet attól függően, hogy az operációs rendszer verziója különböző tar fájl hivatkozni szeretne:

    OS_VERSION=$(lsb_release -sr)
    if [[ $OS_VERSION == 14* ]]; then
        echo "OS verion is $OS_VERSION. Using hue-binaries-14-04."
        HUE_TARFILE=hue-binaries-14-04.tgz
    elif [[ $OS_VERSION == 16* ]]; then
        echo "OS verion is $OS_VERSION. Using hue-binaries-16-04."
        HUE_TARFILE=hue-binaries-16-04.tgz
    fi

## <a name="deployScript"></a>Üzembe helyezése a parancsprogram művelet ellenőrzőlistája

A azt tartott bevezetését tervezi a parancsfájlok elkészítésekor lépések a következők:

- Helyezze a fájlokat, amelyek tartalmazzák az egyéni parancsfájlok, amely a fürt csomópontok elérhető a telepítés során helyen. Ez az alapértelmezett vagy további tárterület fiókok megadott fürt telepítési, vagy bármely más nyilvánosan hozzáférhető tároló tároló időben lehet.

- Adja hozzá a parancsfájlok, hogy biztosan impotently, végrehajtás, hogy a parancsprogram futtatható többször ugyanazon a csomóponton ellenőrzést.

- Egy ideiglenes fájl címtár /tmp használatával nyomon a letöltött fájlokat a parancsfájlok által használt, majd eltávolítással parancsfájlok van végrehajtása után.

- Abban az esetben, ha az operációs rendszer szintű beállításai vagy a Hadoop szolgáltatás konfigurációs fájl megváltoztak, akkor előfordulhat, hogy szeretné indítani a HDInsight-szolgáltatások, hogy az illető felveszi is meg OS szintű beállítást, például a környezeti változók a parancsfájlok beállítása.

## <a name="runScriptAction"></a>Hogyan tehető függővé egy parancsprogramot művelet

Parancsfájl-műveletek is használhatja az Azure portálon Azure PowerShell, Azure erőforrás Manager (ARM) sablonok vagy a HDInsight .NET SDK HDInsight fürt testreszabásához. Útmutatásért lásd: [parancsfájl művelet használatáról](hdinsight-hadoop-customize-cluster-linux.md).

## <a name="sampleScripts"></a>Egyéni parancsfájl minták

A Microsoft parancsfájl-összetevők telepítése egy HDInsight fürthöz mintát biztosít. A minta parancsfájlok és használatuk útmutatók érhetők el az alábbi hivatkozásokat:

- [Telepítése és HDInsight fürt szín használata](hdinsight-hadoop-hue-linux.md)
- [Telepítése és R HDInsight Hadoop fürt használata](hdinsight-hadoop-r-scripts-linux.md)
- [Telepítése és HDInsight fürt Solr használata](hdinsight-hadoop-solr-install-linux.md)
- [Telepítése és HDInsight fürt Giraph használata](hdinsight-hadoop-giraph-install-linux.md)  

> [AZURE.NOTE] A dokumentumok feletti csatolt Linux-alapú HDInsight fürt vonatkoznak. Parancsprogramok használata a Windows-alapú HDInsight lásd: a [parancsprogram művelet fejlesztési HDInsight (Windows),](hdinsight-hadoop-script-actions.md) vagy használja a rendelkezésre álló hivatkozások minden cikk tetején.

##<a name="troubleshooting"></a>Hibaelhárítás

Parancsfájlok kialakítása használata során felmerülő hibákat a következők:

__Error__: `$'\r': command not found`. Előfordul, hogy követi `syntax error: unexpected end of file`.

_OK_: ezt a hibát, ha a sorokat egy parancsprogramot végződjön CRLF okozza. UNIX rendszerek csak LF számíthat, mint a sor-végződés.

Ez a probléma leggyakrabban fordul elő, ha a Windows-környezet a hozta létre a parancsfájl CRLF van egy közös vonal befejezése a Windows sok szöveget szerkesztők számára.

_Megoldás_: Ha egy lehetőséget a szövegszerkesztőben, Unix formátum vagy kiválasztása LF a sor-végződés. Akkor is használhatja az alábbi parancsok UNIX operációs rendszeren egy LF a CRLF módosítása:

> [AZURE.NOTE] Az alábbi parancsok nagyjából megfelelői, annak, hogy azok módosítsa a CRLF sorvégződések LF. Jelölje ki azt a rendszer a segédprogramok alapján.

| Parancs | Jegyzetek |
| ------- | ----- |
| `unix2dos -b INFILE` | Az eredeti fájlt a másolat készül a. BAK bővítmény |
| `tr -d '\r' < INFILE > OUTFILE` | KIMENŐ_FÁJL tartalmazó csak LF végződések javítása változat fog tartalmazni. |
| `perl -pi -e 's/\r\n/\n/g' INFILE` | Módosítja Ez a fájl közvetlenül az új fájl létrehozása nélkül |
| ```sed 's/$'"/`echo \\\r`/" INFILE > OUTFILE``` | KIMENŐ_FÁJL tartalmazó csak LF végződések javítása változat fog tartalmazni.

__Error__: `line 1: #!/usr/bin/env: No such file or directory`.

_OK_: Ez a hiba történik, amikor a parancsfájl mentette a bájt sorrend jel (AJ) UTF-8.

_Megoldás_: mentse a fájlt, ASCII, vagy UTF-8 AJ nélkül. Előfordulhat, hogy is használatával a következő parancsot a Linux vagy a Unix rendszerű hozzon létre egy új fájlt a AJ nélkül:

    awk 'NR==1{sub(/^\xef\xbb\xbf/,"")}{print}' INFILE > OUTFILE

A fenti parancs __BEMENŐ_FÁJL__ cserélje a AJ tartalmazó fájlt. __KIMENŐ_FÁJL__ kell egy új fájlnevet, amely nélkül az AJ a parancsfájl fog tartalmazni.

## <a name="seeAlso"></a>Következő lépések

* Megtudhatja, hogyan [testreszabása HDInsight fürt parancsfájl művelettel](hdinsight-hadoop-customize-cluster-linux.md)

* A [HDInsight .NET SDK hivatkozás](https://msdn.microsoft.com/library/mt271028.aspx) használatával több .NET-alkalmazások kezelése HDInsight létrehozásával kapcsolatos tudnivalók

* A [HDInsight REST API](https://msdn.microsoft.com/library/azure/mt622197.aspx) segítségével miként használhatja a többi HDInsight fürt felügyeleti műveleteket.
