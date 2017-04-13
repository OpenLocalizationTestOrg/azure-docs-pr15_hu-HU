<properties
    pageTitle="A HDInsight művelet fejlesztési parancsfájl |} Microsoft Azure"
    description="Megtudhatja, hogy miként Hadoop fürt parancsfájl művelettel testreszabása. Parancsfájl művelet használható a Hadoop fürthöz külön szoftver telepítése vagy fürt a telepített alkalmazások beállításainak módosításához."
    services="hdinsight"
    documentationCenter=""
    tags="azure-portal"
    authors="mumian"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/19/2016"
    ms.author="jgao"/>

# <a name="develop-script-action-scripts-for-hdinsight-windows-based-clusters"></a>Parancsfájl műveletet parancsfájlok HDInsight Windows-alapú fürtre vonatkozóan kidolgozása

Megtudhatja, hogy miként parancsfájl műveletet parancsfájlokat hdinsight szolgáltatásból lehetőségre. Parancsfájl műveletet parancsfájlok használatáról további tudnivalókért lásd [testreszabása HDInsight fürt parancsfájl művelettel](hdinsight-hadoop-customize-cluster.md). Az ugyanazon a cikkben Linux-alapú HDInsight fürt írták [HDInsight kidolgozása parancsfájl művelet parancsfájlok](hdinsight-hadoop-script-actions-linux.md)talál.

> [AZURE.NOTE] A dokumentum adatai a Windows-alapú HDInsight fürt jellemző. Windows-alapú fürt való használatáról a parancsfájl-műveletek további tudnivalókért lásd [parancsfájl művelet fejlesztési a HDInsight (Linux)](hdinsight-hadoop-script-actions-linux.md).


Parancsfájl művelet használható a Hadoop fürthöz külön szoftver telepítése vagy fürt a telepített alkalmazások beállításainak módosításához. Parancsfájl-műveletek parancsfájlok, a fürt csomóponton Ha HDInsight fürt van telepítve, és a fürt csomópontjai HDInsight konfigurálás befejezése után végrehajtása. A parancsprogram művelet végrehajtása a rendszer rendszergazdai jogokkal, és teljes hozzáférési jogosultsággal, és a fürt csomópontok biztosít. Minden fürt biztosítható, hogy a sorrendben, amelyben megadott futtatható parancsfájl műveletek listával.

> [AZURE.NOTE] Ha az alábbi hibaüzenet tapasztalhatja:
>
>     System.Management.Automation.CommandNotFoundException; ExceptionMessage : The term 'Save-HDIFile' is not recognized as the name of a cmdlet, function, script file, or operable program. Check the spelling of the name, or if a path was included, verify that the path is correct and try again.
> Ennek az oka, nem tartalmazzák a segítő módszereket.  [Egyéni parancsfájlok segítő módszerei](hdinsight-hadoop-script-actions.md#helper-methods-for-custom-scripts)látható.

## <a name="sample-scripts"></a>Minta parancsfájlok

Windows operációs rendszer HDInsight fürt készítéséhez parancsfájl művelet Azure PowerShell-parancsprogramot. A következő mintát parancsfájl, állítsa be a webhely konfigurációs fájlokat:

[AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

    param (
        [parameter(Mandatory)][string] $ConfigFileName,
        [parameter(Mandatory)][string] $Name,
        [parameter(Mandatory)][string] $Value,
        [parameter()][string] $Description
    )

    if (!$Description) {
        $Description = ""
    }

    $hdiConfigFiles = @{
        "hive-site.xml" = "$env:HIVE_HOME\conf\hive-site.xml";
        "core-site.xml" = "$env:HADOOP_HOME\etc\hadoop\core-site.xml";
        "hdfs-site.xml" = "$env:HADOOP_HOME\etc\hadoop\hdfs-site.xml";
        "mapred-site.xml" = "$env:HADOOP_HOME\etc\hadoop\mapred-site.xml";
        "yarn-site.xml" = "$env:HADOOP_HOME\etc\hadoop\yarn-site.xml"
    }

    if (!($hdiConfigFiles[$ConfigFileName])) {
        Write-HDILog "Unable to configure $ConfigFileName because it is not part of the HDI configuration files."
        return
    }

    [xml]$configFile = Get-Content $hdiConfigFiles[$ConfigFileName]

    $existingproperty = $configFile.configuration.property | where {$_.Name -eq $Name}

    if ($existingproperty) {
        $existingproperty.Value = $Value
        $existingproperty.Description = $Description
    } else {
        $newproperty = @($configFile.configuration.property)[0].Clone()
        $newproperty.Name = $Name
        $newproperty.Value = $Value
        $newproperty.Description = $Description
        $configFile.configuration.AppendChild($newproperty)
    }

    $configFile.Save($hdiConfigFiles[$ConfigFileName])

    Write-HDILog "$configFileName has been configured."

A parancsprogram megnyitja a négy paraméter, a konfigurációs fájl nevét, a tulajdonság meg szeretné változtatni, az értéket be szeretné állítani, és egy leírást. Példa:

    hive-site.xml hive.metastore.client.socket.timeout 90

Ezek a paraméterek fog értéke hive.metastore.client.socket.timeout 90 a struktúra-site.xml fájlban.  Az alapértelmezett értéke 60 másodperc.

A mintaparancsfájl [https://hditutorialdata.blob.core.windows.net/customizecluster/editSiteConfig.ps1](https://hditutorialdata.blob.core.windows.net/customizecluster/editSiteConfig.ps1)is találhatók.

HDInsight nyújt további összetevőit telepítése HDInsight fürt több parancsfájlok:

név | Parancsfájl
----- | -----
**A külső telepítése** | https://hdiconfigactions.BLOB.Core.Windows.NET/sparkconfigactionv03/Spark-Installer-v03.ps1. Lásd: a [telepítési és használati dokumentuma a HDInsight fürt][hdinsight-install-spark].
**R telepítése** | https://hdiconfigactions.BLOB.Core.Windows.NET/rconfigactionv02/r-Installer-v02.ps1. Lásd: a [telepítés és a HDInsight fürt R használata][hdinsight-r-scripts].
**Solr telepítése** | https://hdiconfigactions.BLOB.Core.Windows.NET/solrconfigactionv01/solr-Installer-v01.ps1. Lásd: [telepítése és használata a HDInsight Solr fürtök](hdinsight-hadoop-solr-install.md).
- **Giraph telepítése** | https://hdiconfigactions.BLOB.Core.Windows.NET/giraphconfigactionv01/giraph-Installer-v01.ps1. Lásd: [telepítése és használata a HDInsight Giraph fürtök](hdinsight-hadoop-giraph-install.md).

Parancsfájl műveletet telepíthető az Azure portál Azure PowerShell vagy a HDInsight .NET SDK használatával.  További tudnivalókért lásd: a [parancsprogram művelettel testreszabása HDInsight fürt][hdinsight-cluster-customize].

> [AZURE.NOTE] A minta parancsfájlok csak 3.1-es HDInsight fürt vagy annál újabb működik. HDInsight fürt verzióján további tudnivalókért olvassa el a [HDInsight fürt verzióival](hdinsight-component-versioning.md)foglalkozó.





## <a name="helper-methods-for-custom-scripts"></a>Egyéni parancsfájlok segítő módszerei

Parancsfájl művelet segítő módszereket, amelyekkel egyéni parancsfájlok írása közben segédprogramot. Ezek [https://hdiconfigactions.blob.core.windows.net/configactionmodulev05/HDInsightUtilities-v05.psm1](https://hdiconfigactions.blob.core.windows.net/configactionmodulev05/HDInsightUtilities-v05.psm1)definiálva, és beépíthetők a parancsfájlok, használja az alábbiakat:

    # Download config action module from a well-known directory.
    $CONFIGACTIONURI = "https://hdiconfigactions.blob.core.windows.net/configactionmodulev05/HDInsightUtilities-v05.psm1";
    $CONFIGACTIONMODULE = "C:\apps\dist\HDInsightUtilities.psm1";
    $webclient = New-Object System.Net.WebClient;
    $webclient.DownloadFile($CONFIGACTIONURI, $CONFIGACTIONMODULE);

    # (TIP) Import config action helper method module to make writing config action easy.
    if (Test-Path ($CONFIGACTIONMODULE))
    {
        Import-Module $CONFIGACTIONMODULE;
    }
    else
    {
        Write-Output "Failed to load HDInsightUtilities module, exiting ...";
        exit;
    }

Az alábbiakban a segítő módszerek a parancsfájl által biztosított:

Súgó mód | Leírás
-------------- | -----------
**Mentés-HDIFile** | Töltse le egy fájlt a a megadott egységes erőforrás azonosító (URI) van társítva az Azure virtuális csomópontot a fürt rendelt helyi lemezen helyre.
**Bontsa ki a HDIZippedFile** | Bontsa ki az egy tömörített fájlt.
**Meghívása HDICmdScript** | Parancsfájl lebonyolítása cmd.exe.
**Az írás-HDILog** | Írja be az egyéni parancsfájl egy parancsprogramot művelet használható a kimeneti.
**Get-szolgáltatások** | Ismerkedés a gépen futó, ahol a parancsfájl végrehajtja a szolgáltatások listáját.
**Get-szolgáltatás** | Az adott szolgáltatás nevű bemeneteként, részletes információ egy adott szolgáltatás (szolgáltatás neve, folyamat azonosító, állapot, stb.) a számítógépen, ahová a parancsfájl végrehajtja a.
**Get-HDIServices** | Ismerkedés a számítógépen futó, ahol a parancsfájl végrehajtja a HDInsight-szolgáltatások listáját.
**Get-HDIService** | Az adott HDInsight szolgáltatás nevét a bemeneteként, a részletes információkat egy adott szolgáltatás elérhető (szolgáltatás neve, folyamat azonosító, állapot, stb.) a számítógépen, ahová a parancsfájl végrehajtja a.
**Get-ServicesRunning** | Ismerkedés a rendszert futtató számítógépen hol a parancsfájl végrehajtja a szolgáltatások listáját.
**Get-ServiceRunning** | Ellenőrizze, hogy egy adott szolgáltatás (név) szerint fut a számítógépen hol a parancsfájl hajt végre.
**Get-HDIServicesRunning** | Ismerkedés a számítógépen futó, ahol a parancsfájl végrehajtja a HDInsight-szolgáltatások listáját.
**Get-HDIServiceRunning** | Ellenőrizze, hogy egy adott HDInsight szolgáltatáshoz (név) szerint fut a számítógépen hol végrehajtja a parancsfájl.
**Get-HDIHadoopVersion** | Telepítve van a számítógépen, ahová a parancsfájl végrehajtja a Hadoop verziója kap.
**Próba-IsHDIHeadNode** | Ellenőrizze, hogy a számítógépen, ahová a parancsfájl végrehajtja a egy központi csomópontot.
**Próba-IsActiveHDIHeadNode** | Ellenőrizze, hogy a számítógépen, ahová a parancsfájl végrehajtja a egy aktív központi csomópontra.
**Próba-IsHDIDataNode** | Ellenőrizze, hogy a számítógépen, ahová a parancsfájl végrehajtja a adatok csomópontot.
**Szerkesztés-HDIConfigFile** | Konfigurációs fájl struktúra-site.xml, core-site.xml, fájlrendszerhez-site.xml, mapred-site.xml vagy fonal-site.xml szerkesztése.


## <a name="best-practices-for-script-development"></a>Gyakorlati tanácsok a parancsprogram fejlesztési

Egyéni parancsfájl-HDInsight fürt fejlesztésekor létezik néhány gyakorlati tanácsok, amelyre érdemes figyelni:

- A Hadoop-verziója a ellenőrzése

    Csak a HDInsight (Hadoop 2.4) 3.1-es verzióját és támogatási parancsfájl művelettel egyéni összetevők telepítése fürt felett. Az egyéni parancsfájl Hadoop verziószámának más feladatok elvégzéséhez a parancsfájl a továbblépés előtt kell használnia a **Get-HDIHadoopVersion** segítő módszerrel.


- Adja meg az állandó forrásokra mutató hivatkozásokat parancsfájl

    Felhasználók ellenőriznie kell, hogy minden a parancsfájlok és más eltérések a testre szabott fürt használt továbbra is elérhető a fürt időtartama és, ezeket a fájlokat a verziói ne módosítsa az időtartam. Ezek az erőforrások szükség, ha a fürt csomópontjai újra imaging szükség. Célszerű, töltse le és a felhasználó a vezérlő tárterület-fiókjában mindent archivál. Ez az alapértelmezett tárterület-fiókot, és meg a testre szabott fürtre telepítési egyszerre, további tárterület-fiókok egyikével is lehet.
    A külső és R testreszabott fürt minták feltéve dokumentációjában, például azt saját egy helyi példányt az erőforrásokat a tárhely fiókba: https://hdiconfigactions.blob.core.windows.net/.


- Győződjön meg arról, hogy a fürt testreszabási parancsfájl idempotent

    Várt kell, hogy a csomópontok HDInsight fürt lesz újra leképezett fürt élettartama során. Amikor egy fürt újra leképezett futtatása a fürt testreszabási parancsfájl Ez a parancsfájl kell, hogy frissítéskor újra imaging, a parancsfájl biztosítania kell, hogy a fürt visszakerül azonos értelemben idempotent államot, amelyben a után a fürt kezdetben létrehozásának első alkalommal adódott a parancsfájlt, testre kell megtervezni. Például ha egyéni parancsfájl D:\AppLocation-alkalmazás telepítve első futtatása, majd minden későbbi futtatás, után újra imaging, a parancsfájl jelölje kell hogy az alkalmazás létezik a D:\AppLocation helyen előtt más eljárás lépéseit a parancsfájl.


- Az optimális helyen egyéni összetevők telepítése

    Ha újra leképezett fürt csomópontjait, a C:\ erőforrás meghajtó és D:\ rendszermeghajtóra újra formázott, eredményül kapott adatokat és alkalmazásokat, hogy ezek a meghajtókon lévő telepítve funkcióvesztés is lehet. Is ennek az Azure virtuális gép (virtuális) csomópontot a fürt részét képező megszakad, és helyettesíti be egy új csomópontot. A D:\ meghajtón, vagy a fürt C:\apps helyén összetevők telepítheti. A C:\ meghajtón más helyekre vannak fenntartva. Adja meg, hogy hol alkalmazások, illetve a tárak vannak telepítve legyen a fürt testreszabási parancsfájl helyét.


- Magas rendelkezésre állásának a fürt-architektúra

    A HDInsight rendelkezik egy aktív-passzív architektúra magas elérhetőség egy központi csomópont van aktív mód (ahol a HDInsight futnak szolgáltatások) és a többi központi csomópont (mely HDInsight a szolgáltatások nem futnak) készenléti módban van. A csomópontok lépjen aktív és passzív mód, ha a HDInsight szolgáltatások futása megszakad. Ha egy parancsprogramot művelet services telepítése nagyon elérhetőség mindkét központi csomóponton, vegye figyelembe, hogy a HDInsight feladatátvevő mechanizmusa nem tudnak a felhasználó által telepített szolgáltatások automatikusan átadni. Így a felhasználó telepített szolgáltatások csomóponton HDInsight központi, amely várhatóan könnyen hozzáférhető kell saját feladatátvevő mechanizmusa Ha aktív-passzív módban van, vagy aktív-aktív üzemmódban.

    Mindkét központi csomóponton lefut egy HDInsight parancsfájl művelet parancs, amikor a címsor-csomópont szerepkör a *ClusterRoleCollection* paraméter érték van megadva. Így egyéni parancsfájl tervezésekor győződjön meg róla, hogy a parancsprogram érdemes szem előtt tartania: Ezzel a beállítással. Problémákat tapasztal, ahol ugyanazok a szolgáltatások telepített és mindkét központi csomópontok lépések és azok, amilyennek a többi gyorskorcsolyázásban nem kell futtatnia. Is érdemes szem előtt, hogy adatokat elvesznek újra imaging, így parancsfájl műveletet keresztül telepített szoftver működnie kell az ilyen eseményekre rugalmassá. Alkalmazások a sok csomópontok van elosztva könnyen hozzáférhető adatokkal végzett munkához úgy kell megtervezni. Figyelje meg, hogy akár 1/5 fürt csomópontok is kell újból telepíteni egy időben.


- Állítsa be az egyéni összetevők Azure Blob-tároló használatára

    Az egyéni összetevők, a fürt csomóponton telepített esetleg a alapértelmezett konfiguráció használata tároló Hadoop elosztott fájl rendszer (hdfs) lehetőségre. Módosítania kell a beállításait, használja helyette az Azure Blob-tárolóhoz. Egy fürt újra kép a Fájlrendszerhez fájlrendszer formázott kap, és ott tárolt adatok elvesztése lenne. Azure Blob-tárolóhoz használata helyett biztosítja, hogy az adatok megmaradnak.

## <a name="common-usage-patterns"></a>Gyakori szokásai

Ebben a részben néhány saját egyéni parancsfájl írása közben fellépő esetleges előfordulhat, hogy a közös használat mintákat megvalósításáról ismerteti.

### <a name="configure-environment-variables"></a>Változók környezet beállítása

A parancsprogram művelet fejlesztése, gyakran változók környezet beállításához szükséges a érzi lesz. Egy legvalószínűbb például akkor, ha egy külső webhelyről töltheti le a bináris, telepítse a fürt, és vegye fel a helyet, ahol telepítve van az "ÚTVONAL" környezeti változóba. A következő kódtöredékének bemutatja, hogyan környezeti változók beállíthatja az egyéni parancsfájl.

    Write-HDILog "Starting environment variable setting at: $(Get-Date)";
    [Environment]::SetEnvironmentVariable('MDS_RUNNER_CUSTOM_CLUSTER', 'true', 'Machine');

Ez az utasítás állítja be a környezetet változó **MDS_RUNNER_CUSTOM_CLUSTER** "igaz" értéket, és is beállítja az a változó legyen a számítógép egészére körét. Időnként fontos, hogy a megfelelő hatókör – a számítógép vagy felhasználó környezeti változók vannak beállítva. Olvassa el [az alábbi] [ 1] környezeti változók beállításával kapcsolatos további információt.

### <a name="access-to-locations-where-the-custom-scripts-are-stored"></a>Az Access az egyéni parancsfájlok tároló helyekre

Parancsfájlok fürt testreszabásához használható van szüksége bármelyik kell a fürt alapértelmezett tároló fiók vagy egy nyilvános csak olvasható tároló további tárterület-fiók. Ha a parancsprogram fér hozzá az erőforrások máshol található ezek kell nyilvánosan hozzáférhető (legalább nyilvános csak olvasható). Például érdemes lehet megnyitni egy fájlt, és mentse a SaveFile-HDI paranccsal.

    Save-HDIFile -SrcUri 'https://somestorageaccount.blob.core.windows.net/somecontainer/some-file.jar' -DestFile 'C:\apps\dist\hadoop-2.4.0.2.1.9.0-2196\share\hadoop\mapreduce\some-file.jar'

Ebben a példában gondoskodnia kell arról, hogy nyilvánosan hozzáférhető-e a tároló "somecontainer", "somestorageaccount" tárterület-fiókjában. Ellenkező esetben a parancsfájl a "Nem található" kivétel throw és sikertelen lesz.

### <a name="pass-parameters-to-the-add-azurermhdinsightscriptaction-cmdlet"></a>A paramétereket át a Hozzáadás-AzureRmHDInsightScriptAction parancsmag

A Hozzáadás-AzureRmHDInsightScriptAction parancsmag át több paramétereket, kell a karakterlánc érték, amely az összes paramétert a parancsfájl tartalmazhat. Példa:

    "-CertifcateUri wasbs:///abc.pfx -CertificatePassword 123456 -InstallFolderName MyFolder"

vagy

    $parameters = '-Parameters "{0};{1};{2}"' -f $CertificateName,$certUriWithSasToken,$CertificatePassword


### <a name="throw-exception-for-failed-cluster-deployment"></a>Kivétel sikertelen fürt telepítéshez throw

Ha szeretne értesítést kaphat pontosan a arra, hogy testreszabási fürt nem sikerült megfelelően működjön, fontos, hogy kivételhibát és a fürt létrehozása sikertelen lesz. Például érdemes lehet feldolgozni egy fájlt, ha létezik és kezelheti a hiba eset, ahol a fájl nem létezik. Ez biztosítja, hogy a parancsprogram biztonságosan kijelentkezik, és a fürt állapotának megfelelően ismert. A következő kódtöredékének biztosít a cél példa:

    If(Test-Path($SomePath)) {
        #Process file in some way
    } else {
        # File does not exist; handle error case
        # Print error message
    exit
    }

Az ebben a kódtöredék a fájl nem létezik, ha vezet, ahol az a parancsfájl ténylegesen kijelentkezik biztonságosan után hibaüzenet nyomtatása és a fürt eléri akkor "" sikeresen fürtre testreszabási folyamat feltételezve fut állam. Ha azt szeretné, hogy pontosan Ha értesítést szeretne kapni arról, hogy testreszabási lényegében fürt meg a várt módon hiányzó fájl miatt nem sikerült, érdemes kivételhibát, és nem sikerül a fürt testreszabási lépés megfelelőbb. Cél kell használnia a következő példa kódrészletet helyette.

    If(Test-Path($SomePath)) {
        #Process file in some way
    } else {
        # File does not exist; handle error case
        # Print error message
    throw
    }


## <a name="checklist-for-deploying-a-script-action"></a>Üzembe helyezése a parancsprogram művelet ellenőrzőlistája
A azt tartott bevezetését tervezi a parancsfájlok elkészítésekor lépések a következők:

1. Helyezze a fájlokat, amelyek tartalmazzák az egyéni parancsfájlok, amely a fürt csomópontok elérhető a telepítés során helyen. Ez az alapértelmezett vagy további tárterület fiókok megadott fürt telepítési, vagy bármely más nyilvánosan hozzáférhető tároló tároló időben lehet.
2. Győződjön meg arról, hogy azok idempotently, hajtsa végre, hogy a parancsprogram futtatható többször ugyanazon a csomóponton parancsfájlok csekkek hozzáadása.
3. Az **Írás-kimeneti** Azure PowerShell-parancsmag használatával STDOUT, valamint a STDERR nyomtatni. **A Host írási**ne használjon.
4. Ideiglenes fájl mappát, például $env: TEMP megtartása a letöltött fájl a parancsfájlok által használt, és ezután eltávolítással parancsfájlok van végrehajtása után.
5. Csak a D:\ vagy C:\apps egyéni szoftver telepítése. A c meghajtó más pontjaira nem fogja használni, mint fenntartott. Figyelje meg, hogy a c meghajtó kívül a C:\apps mappában lévő fájlok telepítése vonhat telepítési hiba során újra a csomópont képek.
6. Abban az esetben, ha az operációs rendszer szintű beállításai vagy a Hadoop szolgáltatás konfigurációs fájl megváltoztak, akkor előfordulhat, hogy szeretné indítani a HDInsight-szolgáltatások, hogy az illető felveszi is meg OS szintű beállítást, például a környezeti változók a parancsfájlok beállítása.

## <a name="debug-custom-scripts"></a>Egyéni parancsfájlok hibakeresése

A parancsprogram hibanaplóit tárolja, az alapértelmezett tárterület-fiók a létrehozása a fürthöz megadott egyéb kimeneti együtt. A naplók tárolódnak nevű táblázat *u < \cluster-name-fragment >< \time-stamp > setuplog*. Ezek a rekordok minden, amelyen a parancsfájl fut a fürt csomópontok (fő csomópont és dolgozó csomópontok) rendelkező összesített naplók.
Ugyanígy jelölje be a naplókat környezetbe HDInsight Tools for Visual Studio. [Visual Studio Hadoop-eszközök HDInsight az első lépések](hdinsight-hadoop-visual-studio-tools-get-started.md#install-hdinsight-tools-for-visual-studio) című eszközök telepítése

**A Visual Studio segítségével napló ellenőrzése**

1. Nyissa meg a Visual Studio.
2. Kattintson a **Nézet**fülre, és kattintson a **Kiszolgáló Intéző**.
3. Kattintson a jobb gombbal a "Azure", kattintson a csatlakozás **Microsoft Azure előfizetések**elemre, és írja be a hitelesítő adatait.
4. Bontsa ki a **tárhely**, bontsa ki az alapértelmezett fájlrendszer az Azure tárterület-fiókkal, **táblázatok**kibontásához, és kattintson duplán a tábla neve.


A fürt csomópontok megjelenítéséhez a mindkettő be is távoli teheti STDOUT és egyéni parancsfájlok STDERR. A naplók minden csomóponton adott csak az adott csomópontot, és **C:\HDInsightLogs\DeploymentAgent.log**be van jelentkezve. A naplófájlok minden kimeneti értékeket az egyéni parancsfájlokat a bejegyzéshez. Egy példa napló kódtöredékének egy külső parancsfájl beavatkozásra néz ki:

    Microsoft.Hadoop.Deployment.Engine.CustomPowershellScriptCommand; Details : BEGIN: Invoking powershell script https://configactions.blob.core.windows.net/sparkconfigactions/spark-installer.ps1.;
    Version : 2.1.0.0;
    ActivityId : 739e61f5-aa22-4254-aafc-9faf56fc2692;
    AzureVMName : HEADNODE0;
    IsException : False;
    ExceptionType : ;
    ExceptionMessage : ;
    InnerExceptionType : ;
    InnerExceptionMessage : ;
    Exception : ;
    ...

    Starting Spark installation at: 09/04/2014 21:46:02 Done with Spark installation at: 09/04/2014 21:46:38;

    Version : 2.1.0.0;
    ActivityId : 739e61f5-aa22-4254-aafc-9faf56fc2692;
    AzureVMName : HEADNODE0;
    IsException : False;
    ExceptionType : ;
    ExceptionMessage : ;
    InnerExceptionType : ;
    InnerExceptionMessage : ;
    Exception : ;
    ...

    Microsoft.Hadoop.Deployment.Engine.CustomPowershellScriptCommand;
    Details : END: Invoking powershell script https://configactions.blob.core.windows.net/sparkconfigactions/spark-installer.ps1.;
    Version : 2.1.0.0;
    ActivityId : 739e61f5-aa22-4254-aafc-9faf56fc2692;
    AzureVMName : HEADNODE0;
    IsException : False;
    ExceptionType : ;
    ExceptionMessage : ;
    InnerExceptionType : ;
    InnerExceptionMessage : ;
    Exception : ;


Ebben a naplóban nincs bejelölve, hogy a külső parancsfájl művelet végrehajtása a a virtuális HEADNODE0 nevű és, hogy a kivételek fordultak elő a végrehajtás során.

Abban az esetben, ha-végrehajtási hiba lép fel, azt mutatja be, hogy a kimeneti is ez naplófájlban fogja tartalmazni. A naplókban megadott adatokkal is segíthetnek hibakeresése során felmerülő problémák parancsfájl.


## <a name="see-also"></a>Lásd még:

- [Parancsfájl művelettel HDInsight fürt testreszabása][hdinsight-cluster-customize]
- [Telepítése és a külső HDInsight fürt használata][hdinsight-install-spark]
- [Telepítése és R HDInsight fürt használata][hdinsight-r-scripts]
- [Telepítési és használati Solr a HDInsight fürtök](hdinsight-hadoop-solr-install.md).
- [Telepítési és használati Giraph a HDInsight fürtök](hdinsight-hadoop-giraph-install.md).

[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-cluster-customize]: hdinsight-hadoop-customize-cluster.md
[hdinsight-install-spark]: hdinsight-hadoop-spark-install.md
[hdinsight-r-scripts]: hdinsight-hadoop-r-scripts.md
[powershell-install-configure]: install-configure-powershell.md

<!--Reference links in article-->
[1]: https://msdn.microsoft.com/library/96xafkes(v=vs.110).aspx
