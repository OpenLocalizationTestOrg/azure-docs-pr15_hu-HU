<properties
    pageTitle="Log Analytics VMware figyelése megoldás |} Microsoft Azure"
    description="Tudnivalók arról, hogy a VMware figyelése megoldást kezelni naplók, és figyelemmel követheti a ESXi hosts."
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
    ms.date="10/28/2016"
    ms.author="banders"/>

# <a name="vmware-monitoring-preview-solution-in-log-analytics"></a>Log Analytics megoldást VMware figyelése (előzetes verzió)

Log Analytics VMware figyelése megoldás megoldást, amely segít a hozzon létre egy központi naplózás és felügyeleti megközelítés nagy VMware naplók. Ez a cikk ismerteti, hogyan kapcsolatos hibák elhárítása, rögzítése és kezelése a ESXi állomások egyetlen helyen, használja a megoldást. A megoldás lásd: a részletes adatok egyetlen helyet biztosít a ESXi állomások. Felső események száma, az állapot és a trendek szolgáltatáson keresztül a ESXi host naplók virtuális és ESXi állomások megjelenik. Elháríthatja a megtekintésével és központosított ESXi host naplók keresése. És értesítések napló keresési lekérdezések alapján hozhat létre.

A megoldás leküldéses adatokat a cél virtuális, amelynek MOBILE ügynök ESXi állomás natív syslog funkciót használja. Azonban a megoldást nem fájlok írása be a cél virtuális belül syslog. A MOBILE agent port 1514 megnyílik, és figyeli a. Az adatok kap, amikor a MOBILE agent be MOBILE verembe küldi az adatokat.

## <a name="installing-and-configuring-the-solution"></a>Való telepítéséről és konfigurálásáról a megoldás

Az alábbi információk segítségével telepítse és állítsa be a megoldást.

- A figyelés VMware megoldás hozzáadása a MOBILE munkaterület [hozzáadása napló Analytics-megoldások a megoldástárból](log-analytics-add-solutions.md)leírt eljárással.

#### <a name="supported-vmware-esxi-hosts"></a>Támogatott VMware ESXi hosts
vSphere ESXi Host 5.5 és 6.0 alkalmazásban

#### <a name="prepare-a-linux-server"></a>Linux kiszolgáló előkészítése
Hozzon létre egy Linux operációs rendszer összes syslog adatokat fogadjon a ESXi hosts virtuális. A [MOBILE Linux ügynök](log-analytics-linux-agents.md) található a webhelycsoport összes ESXi host syslog adatot. Több ESXi hosts továbbítani naplók egyetlen Linux-kiszolgálón, ahogy a következő példában is használhatja.  

   ![Syslog továbbításához](./media/log-analytics-vmware/diagram.png)

### <a name="configure-syslog-collection"></a>A webhelycsoport syslog konfigurálása

1. Állítsa be az átirányítást syslog VSphere. Részletes információ érdekében syslog átirányítás beállítása [syslog konfigurálása ESXi 5.x és 6.0-s (2003322)](https://kb.vmware.com/selfservice/microsites/search.do?language=en_US&cmd=displayKC&externalId=2003322). Nyissa meg a **ESXi Host konfigurációs** > **szoftver** > **Speciális beállítások** > **Syslog**.
  ![vsphereconfig](./media/log-analytics-vmware/vsphere1.png)  

2. A *Syslog.global.logHost* mezőben adja hozzá a Linux server és a port *1514*. Ha például `tcp://hostname:1514` vagy`tcp://123.456.789.101:1514`

3. Nyissa meg a ESXi host syslog tűzfal. **ESXi Host konfigurációs** > **szoftver** > **Biztonsági profilt** > **tűzfal** és a **Tulajdonságok**megnyitása.  

    ![vspherefw](./media/log-analytics-vmware/vsphere2.png)  

    ![vspherefwproperties](./media/log-analytics-vmware/vsphere3.png)  

4. Jelölje be a vSphere konzol, ha ellenőrizni szeretné, hogy adott syslog helyes-e beállítva. A ESXI állomáson erősítse meg, hogy a port **1514** van beállítva.

5. Tesztelje a Linux kiszolgáló és a ESXi host közötti kapcsolatot használatával a `nc` a ESXi állomáson parancsot. Példa:

    ```
    [root@ESXiHost:~] nc -z 123.456.789.101 1514
    Connection to 123.456.789.101 1514 port [tcp/*] succeeded!
    ```

6. Töltse le és a MOBILE Agent Linux Linux kiszolgálói telepítése. További tudnivalókért lásd: a [MOBILE Agent Linux dokumentációját](https://github.com/Microsoft/OMS-Agent-for-Linux).

7. A MOBILE Agent Linux telepítése után nyissa meg a /etc/opt/microsoft/omsagent/sysconf/omsagent.d könyvtár, és másolja a vmware_esxi.conf fájlt a /etc/opt/microsoft/omsagent/conf/omsagent.d könyvtár és az új a tulajdonos vagy csoport és a fájl engedélyek. Példa:

    ```
    sudo cp /etc/opt/microsoft/omsagent/sysconf/omsagent.d/vmware_esxi.conf /etc/opt/microsoft/omsagent/conf/omsagent.d
sudo chown omsagent:omiusers /etc/opt/microsoft/omsagent/conf/omsagent.d/vmware_esxi.conf
    ```

8.  Indítsa újra a MOBILE Agent az Linux futtatásával `sudo /opt/microsoft/omsagent/bin/service_control restart`.

9. A MOBILE portálon végezze el a naplót keresése `Type=VMware_CL`. MOBILE syslog adatokat gyűjt, megmaradnak a syslog formátumot. A portálon bizonyos területeken rögzítendő, például a *Hostname (állomásnév)* és a *Folyamatnév*.  

    ![típus](./media/log-analytics-vmware/type.png)  

    A nézet napló keresési eredmény hasonlítanak a fenti képet, ha nem kell semmit a MOBILE VMware figyelése megoldás irányítópult használja.  

## <a name="vmware-data-collection-details"></a>A webhelycsoport VMware Adatrészletek.

A VMware figyelése megoldást különböző mértékek, és jelentkezzen be teljesítményadatokat Linux, hogy engedélyezve van a MOBILE ügynökök használatának ESXi hosts gyűjti össze.

A következő táblázat mutatja az adatgyűjtési módszerek és más adatgyűjtés hogyan adatait.

| Platform | Linux MOBILE Agent | SCOM agent | Azure tárhely | Kötelező SCOM? | E-kezelés csoport elküldött SCOM ügynök adatok | a webhelycsoport gyakorisága |
|---|---|---|---|---|---|---|
|Linux|![igen](./media/log-analytics-vmware/oms-bullet-green.png)|![nem](./media/log-analytics-vmware/oms-bullet-red.png)|![nem](./media/log-analytics-vmware/oms-bullet-red.png)|            ![nem](./media/log-analytics-containers/oms-bullet-red.png)|![nem](./media/log-analytics-vmware/oms-bullet-red.png)| 3 percenként|


Az alábbi táblázat a VMware figyelése megoldást által gyűjtött adatok mezők példák megjelenítése:

| a mező neve | Leírás |
| --- | --- |
| Device_s| Tárolók VMware |
| ESXIFailure_s | Hiba típusa |
| EventTime_t | Ha az esemény bekövetkezett idő |
| HostName_s | Állomásnév ESXi |
| Operation_s | Virtuális létrehozása vagy törlése a virtuális |
| ProcessName_s | esemény neve |
| ResourceId_s | VMware állomás neve |
| ResourceLocation_s | VMware |
| ResourceName_s | VMware |
| ResourceType_s | A Hyper-V |
| SCSIStatus_s | VMware SCSI állapota |
| SyslogMessage_s | Syslog adatok |
| UserName_s | felhasználó létrehozása vagy törlése a virtuális |
| VMName_s | Virtuális neve |
| Számítógép | gazdaszámítógép |
| TimeGenerated | az adatok jött létre idő |
| DataCenter_s | VMware adatközponthoz |
| StorageLatency_s | tárterület időtartama (ms) |

## <a name="vmware-monitoring-solution-overview"></a>VMware figyelés megoldás áttekintése

A VMware csempe megjelenik a MOBILE portálon. Hibák magas szintű nézetének biztosít. Ha a mozaik gombra kattint, akkor egy irányítópult megjelenítése ismertetőt találhat.

![csempe](./media/log-analytics-vmware/tile.png)

#### <a name="navigate-the-dashboard-view"></a>Nyissa meg az irányítópult megtekintése

Az irányítópult **VMware** nézetben pengéit szerint vannak rendezve:

- Hiba állapot száma
- Felső állomás események száma
- Felső események száma
- Virtuális gép tevékenységek
- A Host ESXi lemez események


![solution1](./media/log-analytics-vmware/solutionview1-1.png)

![solution2](./media/log-analytics-vmware/solutionview1-2.png)

Kattintson a minden lap kattintva nyissa meg a napló Analytics keresés panel, amely a lap adott részletes információkat jelenít meg.

További lehetőségek szerkesztheti a keresési lekérdezést, amit adott módosítására. Alapvető tudnivalók a MOBILE keresés oktatóanyagot, olvassa el a [MOBILE napló keresési oktatóprogram.](log-analytics-log-searches.md)

#### <a name="find-esxi-host-events"></a>ESXi host események megkeresése

Egyetlen ESXi állomás több naplók, ezek a folyamatok alapján készít. A figyelés VMware megoldást központosítja őket, és áttekintheti az események száma. A központi nézet segít, hogy melyik ESXi állomás nagy mennyiségű esemény van, és milyen események leggyakrabban előforduló a környezet ismertetése.

![esemény](./media/log-analytics-vmware/events.png)

Felhatolás további egy ESXi állomáson, vagy egy esemény típusa gombra kattintva.

Ha egy ESXi állomásnév gombra kattint, az adott ESXi állomástól adatokat tekintheti meg. Ha az esemény típusa az eredmények szűkítéséhez, vegye fel a `“ProcessName_s=EVENT TYPE”` a keresési lekérdezés. A keresési szűrő **Folyamatnév** is választhat. Az információk szűkíthető, amely.

![kevésbé részletes adatok megjelenítése](./media/log-analytics-vmware/eventhostdrilldown.png)

#### <a name="find-high-vm-activities"></a>Magas virtuális tevékenységek megkeresése

A virtuális gép lehet létrehozni és bármely ESXi host törli. Célszerű a rendszergazda létrehoz egy ESXi host hány VMs azonosításához. Hogy a-viszont segít megérteni a teljesítmény és a kapacitás tervezés. Virtuális tevékenység események szerinti nyomon követést elengedhetetlen kezelése a környezettől.

![kevésbé részletes adatok megjelenítése](./media/log-analytics-vmware/vmactivities1.png)

Ha azt szeretné, hogy további ESXi host virtuális létrehozási adatokat, válassza a ESXi host.

![kevésbé részletes adatok megjelenítése](./media/log-analytics-vmware/createvm.png)

#### <a name="common-search-queries"></a>Közös keresési lekérdezések

A megoldás egyéb hasznos segítséget nyújtanak a ESXi hosts, például magas tárterület, tároló időtartama és elérési útját hiba kezelése lekérdezések tartalmazza.

![lekérdezések](./media/log-analytics-vmware/queries.png)

#### <a name="save-queries"></a>Mentse a lekérdezést

Keresési lekérdezések mentése a MOBILE szabványos szolgáltatása, és lehet segítségével bármilyen hasznos megtalálta lekérdezéseket. Miután létrehozott egy lekérdezést, amely akkor hasznosak, mentse a **Kedvencek**gombra kattintva. Mentett lekérdezés segítségével egyszerűen felhasználhatja később [Saját irányítópult](log-analytics-dashboards.md) oldalán a saját egyéni irányítópultok tartalomelemeinek létrehozására.

![DockerDashboardView](./media/log-analytics-vmware/dockerdashboardview.png)

#### <a name="create-alerts-from-queries"></a>A lekérdezések értesítések létrehozása

Miután létrehozta a lekérdezéseket, érdemes lehet a lekérdezésekkel figyelmeztet, ha adott események. Lásd: a [napló Analytics riasztásai](log-analytics-alerts.md) értesítések létrehozása olvashat. Példák a riasztási lekérdezések és más lekérdezés példákat talál a [Monitor VMware használatával MOBILE napló Analytics](https://blogs.technet.microsoft.com/msoms/2016/06/15/monitor-vmware-using-oms-log-analytics) blogbejegyzésben.

## <a name="frequently-asked-questions"></a>Gyakori kérdések

### <a name="what-do-i-need-to-do-on-the-esxi-host-setting-what-impact-will-it-have-on-my-current-environment"></a>Mit kell a ESXi a szolgáltató beállítása? Milyen hatással lesz rá elhelyezni a az aktuális környezet?
A megoldás továbbítása mechanizmusa natív ESXi Host Syslog használja. A ESXi állomáson, amellyel rögzítheti a naplók bármilyen további Microsoft-szoftverek nem szükséges. Meg kell hatást alacsony a meglévő környezetbe. Azonban kell syslog átirányításának, amely ESXI funkciók beállítása.

### <a name="do-i-need-to-restart-my-esxi-host"></a>Szükség van indítsa újra a ESXi host?
nem. Ez a folyamat nem kell indítani. Néha vSphere nem megfelelően frissül a syslog. Ebben az esetben jelentkezzen be a ESXi host, és frissítse a syslog. Újra nem kell, hogy ez a folyamat nem zavaró az környezetbe, indítsa újra az állomásnév oszlopban.

### <a name="can-i-increase-or-decrease-the-volume-of-log-data-sent-to-oms"></a>Növelheti vagy csökkentheti a hangerőt a naplóadatok küldött MOBILE?
Igen, akkor is. A vSphere a ESXi Host naplózási szintjének beállításokat is használhatja. Log webhelycsoport alapuló *információ* szintjét. Igen ha naplózni virtuális létrehozása vagy törlése, szeretne rögzíteni a Hostd *információ* szintet. További tudnivalókért lásd: a [VMware Tudásbázis](https://kb.vmware.com/selfservice/microsites/search.do?&cmd=displayKC&externalId=1017658).

### <a name="why-is-hostd-not-providing-data-to-oms-my-log-setting-is-set-to-info"></a>Miért Hostd nem biztosít adatok a MOBILE? Információ a napló beállítás értéke.
Esetében a syslog időbélyeg-ESXi host hiba történt. További tudnivalókért lásd: a [VMware Tudásbázis](https://kb.vmware.com/selfservice/microsites/search.do?language=en_US&cmd=displayKC&externalId=2111202). Miután telepítette a megoldáshoz, Hostd kell a szokásos módon működik.

### <a name="can-i-have-multiple-esxi-hosts-forwarding-syslog-data-to-a-single-vm-with-omsagent"></a>Felvehetek-e több ESXi hosts továbbít syslog adatokat tartalmazó omsagent egy egyetlen virtuális?
igen. Egy egyetlen virtuális a omsagent továbbít több ESXi hosts is.

### <a name="why-dont-i-see-data-flowing-into-oms"></a>Miért nem látható a MOBILE szét adatok?

Több oka lehet:

- A ESXi host van nem megfelelően adatok küldése a virtuális gép omsagent futtatása. Ha tesztelni szeretné, végezze el az alábbi lépéseket:
    1. Győződjön meg arról, hogy jelentkezzen be az ESXi állomáshoz ssh, és futtassa az alábbi parancsot:`nc -z ipaddressofVM 1514`

        Ha ez nem jár sikerrel, a speciális konfigurációs beállítások vSphere valószínűleg nem javítja ki. Információ arról, hogy miként állíthatja be a továbbítás syslog ESXi host [konfigurálása a webhelycsoport syslog](#configure-syslog-collection) talál.

    2. Syslog port kapcsolódási sikeres, de még nem látható minden adat, ha majd töltse be újra a syslog a ESXi állomáson segítségével ssh, futtassa a következő parancsot:` esxcli system syslog reload`

- A virtuális a MOBILE Agent nincs megfelelően beállítva. Ennek ellenőrzéséhez végezze el az alábbi lépéseket:
    1. MOBILE figyeli a porthoz 1514, és ezt adatok nyújtja MOBILE be. Ha ellenőrizni szeretné, hogy meg nyitva, a következő parancsot:`netstat -a | grep 1514`
    2. Meg kell jelennie port `1514/tcp` nyissa meg. Ha nem, ellenőrizze, hogy a omsagent megfelelően van-e telepítve. Ha nem látható a port adatai, majd a syslog port nincs nyitva a virtuális.
        1. Győződjön meg róla, hogy fut-e a MOBILE Agent használatával `ps -ef | grep oms`. Ha nem fut, a folyamat megkezdéséhez a parancs futtatásával` sudo /opt/microsoft/omsagent/bin/service_control start`
        2. Nyissa meg a `/etc/opt/microsoft/omsagent/conf/omsagent.d/vmware_esxi.conf` fájlt.

            Győződjön meg arról, hogy a megfelelő felhasználó és a csoport beállítás érvényes, hasonlóan:`-rw-r--r-- 1 omsagent omiusers 677 Sep 20 16:46 vmware_esxi.conf`

            Ha a fájl nem létezik, vagy a felhasználó és a csoport beállítás nem megfelelő, [Linux kiszolgáló](#prepare-a-linux-server)előkészítése korrekciós lépéseket.

## <a name="next-steps"></a>Következő lépések

- Log Analytics [Napló keresések](log-analytics-log-searches.md) segítségével részletes VMware host adatok megtekintéséhez.
- [Hozzon létre saját irányítópultjai](log-analytics-dashboards.md) VMware host adatokat tartalmazó.
- [Értesítések létrehozása](log-analytics-alerts.md) esetén az adott VMware host események.
