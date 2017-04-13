<properties
    pageTitle="Tárolók megoldás napló Analytics |} Microsoft Azure"
    description="A tárolók megoldást napló Analytics segít megtekintése és kezelése a Docker tároló hosts egyetlen helyen."
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



# <a name="containers-preview-solution-log-analytics"></a>Tárolók (előzetes verzió) megoldás napló Analytics

Ez a cikk ismerteti, hogyan állíthatja be és használja a tárolók megoldást napló Analytics, amely segít a megtekintése és kezelése a Docker tároló hosts egyetlen helyen. Docker rendszer szoftver virtualizációs szoftvertelepítést az informatikai infrastruktúrát az automatizáló tárolók létrehozásához használt.

A megoldást mely tárolók futtatja a tároló állomáson, és mi képek futtatja a tárolók megtekintheti. Megjelenítő jegyzettárolót használt parancsok részletes naplózás információkat tekinthet meg. És elháríthatja a tárolók megtekintésével és a keresés központi naplók távolról a Docker hosts megtekintése nélkül. Tárolók, amelyek lehetnek a állomás zajos és igénybe fölösleges erőforrások is megkeresheti. És központi Processzor, a memóriahasználat, a tárhely és a hálózati használatát és a teljesítmény információk tárolók tekinthet meg.

## <a name="installing-and-configuring-the-solution"></a>Való telepítéséről és konfigurálásáról a megoldás

Az alábbi információk segítségével telepítse és állítsa be a megoldást.

A tárolók megoldás hozzáadása a MOBILE munkaterület [hozzáadása napló Analytics-megoldások a megoldástárból](log-analytics-add-solutions.md)leírt eljárással.

Két módon szeretné telepíteni, és a MOBILE Docker használata:

- A Linux támogatott operációs rendszerek telepítése és futtatása Docker és majd telepíteni és beállítani MOBILE ügynök Linux
- CoreOS, a telepítése és futtatása Docker és beállította a OMSAgent belül tároló futtatása

A támogatott Docker és Linux operációs rendszereken olvassa el a [GitHub](https://github.com/Microsoft/OMS-docker)tároló állomáson.

>[AZURE.IMPORTANT] Docker futtatása **előtt** a [MOBILE Agent Linux](log-analytics-linux-agents.md) telepítése a tároló hosts kell lennie. Ha már telepítette az ügynök Docker telepítése előtt, kell telepítse újra a MOBILE Agent Linux rendszerhez. Docker kapcsolatos további tudnivalókért lásd: a [Docker webhelyet](https://www.docker.com).

Az alábbi beállításokat, mielőtt tárolók figyelheti a tároló hosts konfigurált van szüksége.

## <a name="configure-settings-for-the-linux-container-host"></a>A Linux tároló host beállításainak konfigurálása

Docker telepítése után az alábbi beállítások a tároló szolgáltatója használatával állítsa be a használatra agent Docker. CoreOS nem támogatja ezt a konfigurációs eljárást.

### <a name="to-configure-settings-for-the-container-host---systemd-suse-opensuse-centos-7x-rhel-7x-and-ubuntu-15x-and-higher"></a>A tároló host - systemd beállításainak konfigurálása (SUSE, openSUSE, CentOS 7.x, RHEL 7.x és Ubuntu 15.x vagy újabb)

1. Szerkesztése docker.service kattintva adja meg a következőket:

    ```
    [Service]
    ...
    Environment="DOCKER_OPTS=--log-driver=fluentd --log-opt fluentd-address=localhost:25225"
    ...
    ```

2. Adja hozzá a $DOCKER\_ENGEDÉLYEZHETI &quot;ExecStart = / usr/bin/docker démon&quot; a docker.service fájl. A következő példa használatával.

    ```
    [Service]
    Environment="DOCKER_OPTS=--log-driver=fluentd --log-opt fluentd-address=localhost:25225"
    ExecStart=/usr/bin/docker daemon -H fd:// $DOCKER_OPTS
    ```

3. Indítsa újra a Docker szolgáltatást. Példa:

    ```
    sudo systemctl restart docker.service
    ```

### <a name="to-configure-settings-for-the-container-host---upstart-ubuntu-14x"></a>A tároló host - beállításainak konfigurálása Upstart (Ubuntu 14.x)

1. /Etc/default/docker szerkesztése, és adja meg a következőket:

    ```
    DOCKER_OPTS="--log-driver=fluentd --log-opt fluentd-address=localhost:25225"
    ```

2. Mentse a fájlt, és indítsa újra a Docker és a MOBILE szolgáltatásokat.

    ```
    sudo service docker restart
    ```

### <a name="to-configure-settings-for-the-container-host---amazon-linux"></a>A tároló host - Amazon Linux beállításainak konfigurálása

1. /Etc/sysconfig/docker szerkesztése, és adja meg a következőket:

    ```
    OPTIONS="--log-driver=fluentd --log-opt fluentd-address=localhost:25225"
    ```

2. Mentse a fájlt, és indítsa újra a Docker szolgáltatást.

    ```
    sudo service docker restart
    ```

## <a name="configure-settings-for-coreos-containers"></a>Tárolók CoreOS beállításainak konfigurálása

Docker telepítése után az alábbi beállításokkal az CoreOS Docker futtatásához és tároló létrehozásához. Bármely támogatott verziójába Linux használható – például CoreOS, ezzel a módszerrel konfigurációs. A [munkaterület-azonosító MOBILE és a billentyűk](log-analytics-linux-agents.md)kell.

### <a name="to-use-oms-for-all-containers-with-coreos"></a>CoreOS az összes tárolók a MOBILE használata

- Indítsa el a figyelni kívánt MOBILE tároló. Módosíthatja, és használja az alábbi példa.

  ```
sudo docker run --privileged -d -v /var/run/docker.sock:/var/run/docker.sock -e WSID="your workspace id" -e KEY="your key" -h=`hostname` -p 127.0.0.1:25224:25224/udp -p 127.0.0.1:25225:25225 --name="omsagent" --log-driver=none --restart=always microsoft/oms
```

### <a name="switching-from-using-an-installed-agent-to-one-in-a-container"></a>Váltás az egyik tárolóban telepített ügynökszoftvert

Ha korábban használt a közvetlenül telepített agent és helyett használandó tároló futó ügynökkel, előbb el kell távolítania OMSAgent. Lásd: a [lépések követésével telepítse a MOBILE Agent Linux rendszerhez](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/OMS-Agent-for-Linux.md).

## <a name="containers-data-collection-details"></a>Tárolók Adatrészletek gyűjtemény

A tárolók megoldást tároló hosts és a MOBILE ügynökök használatának Linux, hogy engedélyezte a tárolók és tárolók futó OMSAgent gyűjti össze a különböző mértékek, és jelentkezzen be teljesítményadatokat.

A következő táblázat mutatja az adatgyűjtési módszerek, és hogyan adatgyűjtés tárolók más adatait.

| Platform | Linux MOBILE Agent | SCOM agent | Azure tárhely | Kötelező SCOM? | E-kezelés csoport elküldött SCOM ügynök adatok | a webhelycsoport gyakorisága |
|---|---|---|---|---|---|---|
|Linux|![igen](./media/log-analytics-containers/oms-bullet-green.png)|![nem](./media/log-analytics-containers/oms-bullet-red.png)|![nem](./media/log-analytics-containers/oms-bullet-red.png)|            ![nem](./media/log-analytics-containers/oms-bullet-red.png)|![nem](./media/log-analytics-containers/oms-bullet-red.png)| 3 percenként|


Az alábbi táblázat a tárolók megoldást által gyűjtött adattípusok példák megjelenítése:

| Adattípus | Mezők |
| --- | --- |
| A hosts és a tárolók teljesítmény | Számítógép, objektumnév és CounterName és #40; % processzor idő lemez felolvassa MB, lemezre írása MB-memória használatát MB, hálózati fogadott bájtok, hálózati küldése bájt processzor használatát sec, hálózati & #41; ellenértéknek, TimeGenerated, Számláló_elérési_útja, SourceSystem |
| A tároló leltár | TimeGenerated, a számítógépen, a tároló neve, ContainerHostname, kép, ImageTag, ContinerState, ExitCode, EnvironmentVar, parancs, CreatedTime, StartedTime, FinishedTime, SourceSystem, Tároló_azonosítója, ImageID |
| A tároló kép leltár | TimeGenerated, számítógép, kép, ImageTag, ImageSize, VirtualSize, futó, fel van függesztve, le, nem sikerült, SourceSystem, ImageID, TotalContainer |
| A tároló napló | TimeGenerated, a számítógépen, a kép azonosítója, a tároló neve, LogEntrySource, LogEntry, SourceSystem, Tároló_azonosítója |
| A tároló szolgáltatási napló | TimeGenerated, számítógép, TimeOfCommand, kép, parancsot, SourceSystem, Tároló_azonosítója |

## <a name="monitor-containers"></a>Tárolók monitor

Után a megoldást engedélyezve van a MOBILE portálon, látni fogja a **tárolók** csempét a tároló hosts és a hosts futó tárolók összefoglaló adatait megjelenítő.

![Tárolók csempe](./media/log-analytics-containers/containers-title.png)

A csempére jeleníti meg, hogy hány tárolók áttekintése a környezet, és hogy ezek esetén nem sikerült, futó vagy leállt.

### <a name="using-the-containers-dashboard"></a>Tárolók irányítópultról

Kattintson a **tárolók** csempére. Innen nézetek szerint rendezve jelenik meg:

- A tároló események
- Hibák
- Tárolók állapota
- A tároló kép leltár
- Processzor- és a teljesítmény

Minden egyes ablaktáblában az irányítópult vizuálisan ábrázolhatók az gyűjtött adatokat a futó keresés.

![Tárolók irányítópult](./media/log-analytics-containers/containers-dash01.png)

![Tárolók irányítópult](./media/log-analytics-containers/containers-dash02.png)

A **Tároló állapot** lap kattintással felső részen alább látható módon.

![Tárolók állapota](./media/log-analytics-containers/containers-status.png)

Log keresési megnyílik, és megjeleníti a hosts és a tárolók azokat az operációs rendszert futtató adatait.

![Jelentkezzen be a tárolók keresése](./media/log-analytics-containers/containers-log-search.png)

Itt módosíthatja a keresési lekérdezést módosítsa úgy, hogy a meghatározott adatok megkeresése, amely érdekli. Log keresések kapcsolatos további tudnivalókért olvassa el a [napló keresések napló Analytics](log-analytics-log-searches.md)című témakört.

A keresési lekérdezés például módosíthatja, hogy a leállítva tárolók helyett a futó tárolók módosításával **operációs rendszert futtató** **Leállítva** a keresési lekérdezés az jeleníti meg.

## <a name="troubleshoot-by-finding-a-failed-container"></a>Sikertelen a tároló keresése hibaelhárítása

MOBILE tároló megjelöléssel **sikertelen** , ha azt nullától hibakóddal leállt. Megtekintheti a hibák és a hibák, a **Tárolók sikertelen volt** a lap a környezetben.

### <a name="to-find-failed-containers"></a>Nem sikerült tárolók megkeresése

1. Kattintson a **Tároló események** lap.  
  ![tárolók események](./media/log-analytics-containers/containers-events.png)
2. Log keresési megnyílik, és megjeleníti a tárolók, az alábbihoz hasonló állapotát.  
  ![tárolók állam](./media/log-analytics-containers/containers-container-state.png)
3. Ezután kattintson a további információk, például kép mérete és a leállítva és sikertelen képek megtekintése sikertelen értéket. Bontsa ki a **több megjelenítése** a kép azonosítójának megtekintése  
  ![nem sikerült tárolók](./media/log-analytics-containers/containers-state-failed.png)
4. Ezután keresse meg a tároló kiszolgáló ezen a képen. Írja be a következő be a keresési lekérdezést.
  `Type=ContainerInventory <ImageID>`A naplók jeleníti meg. Görgessen le a sikertelen tároló látható.  
  ![nem sikerült tárolók](./media/log-analytics-containers/containers-failed04.png)


## <a name="search-logs-for-container-data"></a>Az adatokat tároló keresési naplók

Ha meghatározott hibák elhárításához esetén, hogy hol fordul elő a környezetben segíthetnek. A következő napló típusú lekérdezésekkel az adatok visszaadására, amelyet nyújt segítséget.

- **ContainerInventory** – használata ilyen típusú, ha azt szeretné, hogy adatait tároló helyét, Mik azok a nevük és milyen képek Windowst futtat.
- **ContainerImageInventory** – ilyen típusú, amikor megkísérli információk keresése a használata vannak rendezve, kép, és megtekintheti a kép információkat, például kép azonosítók vagy a betűméretet.
- **ContainerLog** – ilyen típusú, ha meg szeretné keresni, és a naplóadatokat konkrét hiba használata.
- **ContainerServiceLog** – ilyen típusú, amikor megkísérli keresse könyvvizsgálati ellenőrzés adatainak a Docker démon, például kezdő, leállítása, törlése vagy ki parancsok a használata.

### <a name="to-search-logs-for-container-data"></a>Keresés az adatokat tároló naplók

- Adja meg, amelyek tudja, hogy nem sikerült a legutóbb, és keresse meg a hibanaplóit. Kezdje azzal, hogy egy adott képhez **ContainerInventory** keresés rendszert futtató tároló nevének megállapítása. Ha például keresése`Type=ContainerInventory ubuntu Failed`  
    ![Keresés Ubuntu tárolók](./media/log-analytics-containers/search-ubuntu.png)

  Jegyezze fel a tároló **neve**mellett, és keresse meg azokat a naplók. Ebben a példában az `Type=ContainerLog adoring_meitner`.

**Teljesítmény adatainak megtekintése**

Lekérdezések összeállításához kezdetén jár, ha szeretné látni, hogy mi mindenre először segíthetnek. Például az összes teljesítményadatokat megtekintéséhez próbálja ki széles lekérdezés írja be a következő keresési lekérdezést.

```
Type=Perf
```

![tárolók teljesítmény](./media/log-analytics-containers/containers-perf01.png)



Megtekintheti a további grafikus űrlapokon a találatok között a **Mértékek** word kattintáskor.

![tárolók teljesítmény](./media/log-analytics-containers/containers-perf02.png)



A teljesítményadatok jelenik meg a kívánt tárolóhoz rá a jobb oldalán a lekérdezés neve beírásával is korlátozhatja.

```
Type=Perf <containerName>
```

A listáját, amely az egyes tároló begyűjtési teljesítménymutatók, amely jeleníti meg.

![tárolók teljesítmény](./media/log-analytics-containers/containers-perf03.png)

## <a name="example-log-search-queries"></a>Példa napló keresési lekérdezések

Érdemes gyakran készíthet egy példa vagy két kezdődő és módosításával őket a környezet igazítása lekérdezéseket. Kiindulási pontként segít speciális lekérdezés összeállítása a **Főbb lekérdezések** lap nyugodtan kísérletezhet.

![Tárolók lekérdezések](./media/log-analytics-containers/containers-queries.png)

## <a name="saving-log-search-queries"></a>Log keresési lekérdezések mentése

Lekérdezések mentheti, a naplófájl Analytics alapvető szolgáltatás. Ott, is azokat, amelyek megtalálta hasznos praktikus későbbi felhasználás céljából.

Miután létrehozott egy lekérdezést, amely akkor hasznosak, mentse a napló keresési lap tetején a **Kedvencek** gombra kattintva. Ezután könnyen hozzáférhető, később **Saját irányítópult** oldalán.

## <a name="next-steps"></a>Következő lépések

- [Keresés naplók](log-analytics-log-searches.md) részletes tároló adatok rekordjainak megtekintéséhez.
