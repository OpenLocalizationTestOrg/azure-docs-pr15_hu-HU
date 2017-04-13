<properties
   pageTitle="Műveletek Management csomagja (MOBILE) alkalmazás függőség Monitor (OPA) beállítása |} Microsoft Azure"
   description="Alkalmazás függőség Monitor (OPA) egy műveletek Management csomagja (MOBILE) megoldást, amely automatikusan a Windows és Linux rendszerben alkalmazásösszetevők találja, és szolgáltatások közötti kommunikáció társít.  Ebben a cikkben ADM üzembe helyezése a környezet és a használat esetek különböző részleteit."
   services="operations-management-suite"
   documentationCenter=""
   authors="daseidma"
   manager="jwhit"
   editor="tysonn" />
<tags
   ms.service="operations-management-suite"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/28/2016"
   ms.author="daseidma;bwren" />

# <a name="configuring-application-dependency-monitor-solution-in-operations-management-suite-oms"></a>Alkalmazás függőség Monitor megoldás műveletek Management csomagja (MOBILE) beállítása
![Figyelmeztető Management ikon](media/operations-management-suite-application-dependency-monitor/icon.png) Alkalmazás függőség Monitor (OPA) automatikusan találja a Windows és Linux rendszerben alkalmazásösszetevők és társít a szolgáltatások közötti kommunikációt. Lehetővé teszi a megtekintheti a kiszolgálóját úgy gondolja, hogy őket – az összekapcsolt kritikus szolgáltatások előadása rendszerek.  Alkalmazás függőség képernyője, folyamatok, kiszolgálók közötti kapcsolatokat, és a szükséges portok bármely TCP-kompatibilis architektúra nincs beállításokkal különböző, telepített ügynökszoftvert más.

Ez a cikk ismerteti az alkalmazás függőség Monitor és bevezetési ügynökök beállítása részleteit.  ADM használatával kapcsolatos további tudnivalókért lásd: [alkalmazás függőség Monitor használatával megoldás műveletek Management csomagja (MOBILE)](operations-management-suite-application-dependency-monitor.md)

>[AZURE.NOTE]Alkalmazás függőség Monitor jelenleg személyes előzetes verzióban.  A ADM magánjellegű előzetes [https://aka.ms/getadm](https://aka.ms/getadm)a hozzáférést kérhet.
>
>A magánjellegű villámnézetben minden MOBILE fiók van ADM. korlátlan hozzáférés  ADM csomópontok ingyenes, de napló Analytics adatainak AdmComputer_CL és AdmProcess_CL típusnál fog forgalmi, mint bármely más megoldás.
>
>Miután ADM nyilvános előzetes lép, csak a betekintést és a MOBILE árak megtervezése az analitikai ingyenes és a díjköteles ügyfelek számára érhető el lesz.  Ingyenes réteg fiókok 5 ADM csomópontok korlátozódik.  Ha a személyes megtekintés vesz és vannak nem tehát a MOBILE árak tervet, amikor ADM belép nyilvános előzetes, ADM adott időpontban le lesznek tiltva. 



## <a name="connected-sources"></a>Kapcsolódó források
Az alábbi táblázat ismerteti a kapcsolódó források a ADM megoldást által támogatott.

| Csatlakoztatott adatforrás | Támogatott | Leírás |
|:--|:--|:--|
| [A Windows ügynökök beállítása](../log-analytics/log-analytics-windows-agents.md) | igen | ADM elemzi és által gyűjtött adatok Windows ügynök számítógépekről.  <br><br>Figyelje meg, hogy a MOBILE agent kívül Windows ügynökök a Microsoft függőség Agent szükséges.  Olvassa el a [támogatott operációs rendszerek](#supported-operating-systems) az operációs rendszer verziója teljes listáját. |
| [Linux ügynökök beállítása](../log-analytics/log-analytics-linux-agents.md) | igen | ADM elemzi és által gyűjtött adatok Linux ügynök számítógépekről.  <br><br>Figyelje meg, hogy a MOBILE agent kívül Linux ügynökök a Microsoft függőség Agent szükséges.  Olvassa el a [támogatott operációs rendszerek](#supported-operating-systems) az operációs rendszer verziója teljes listáját. |
| [SCOM kezelése csoport](../log-analytics/log-analytics-om-agents.md) | igen | ADM elemzi és adatait gyűjti össze a Windows és Linux ügynökök egy csatlakoztatott SCOM kezelése csoportban. <br><br>Közvetlen kapcsolat létesítése a SCOM ügynök számítógépről MOBILE szükség. Adatok a kezelés csoportban a MOBILE tárházba átirányítása közvetlenül a küldi.|
| [Azure tárterület-fiók](../log-analytics/log-analytics-azure-storage.md) | nem | ADM adatokat gyűjt a ügynök számítógépekről szándékos, nincs adat gyűjthetők össze az Azure tárhelyről belőle. |

Figyelje meg, hogy csak ADM támogatja a 64 bites platformokon.

A Windows, a Microsoft figyelése ügynök (MMA) segítségével SCOM és a MOBILE gyűjtse össze, és küldje el nyomon követési adatok.  (Ez az ügynök nevezzük a SCOM ügynök, MOBILE ügynök, MMA vagy közvetlen ügynök, attól függően, hogy a környezetben.)  SCOM és a MOBILE nyújtanak különböző MMA mezőben verziói ki, de ezeket a verziókat is minden alkalommal SCOM, MOBILE vagy mindkettőt.  A Linux rendszerhez, a MOBILE Agent Linux gyűjti össze és a MOBILE-adatok figyelése küld.  ADM MOBILE közvetlen anyagokkal kiszolgálókon vagy MOBILE SCOM felügyeleti csoportok keresztül csatolt kiszolgálókon használható.  Ez a dokumentáció céljából hivatkozik az összes ügynökök – a Linux vagy a Windows, hogy egy SCOM MG vagy csatlakozik közvetlenül MOBILE – "MOBILE ügynökök", kivéve, ha a agent speciális telepítési neve környezetben van szükség.

A ADM agent nincs Küldés semmilyen adatot magát, és nincs szükség a tűzfal vagy portok módosításait.  ADM meg mindig adatátvitel MOBILE, hogy a MOBILE ügynök vagy közvetlenül a MOBILE átjáró keresztül.

![ADM ügynökök beállítása](media/operations-management-suite-application-dependency-monitor/agents.png)

Ha Ön egy felügyeleti csoportot SCOM vevő kapcsolódik MOBILE:

- Ha a SCOM ügynökök férhet hozzá MOBILE csatlakozhat az internethez, nincs további beállítási szükség.  
- Ha a SCOM ügynökök az interneten keresztül nem tud hozzáférni a MOBILE, szüksége lesz a MOBILE átjáró SCOM végezhető beállítása.
  
A MOBILE közvetlen Agent használatakor konfigurálása a MOBILE Agent magát MOBILE és a MOBILE átjáró csatlakozni szeretne.  A MOBILE átjáró letölthető [https://www.microsoft.com/en-us/download/details.aspx?id=52666](https://www.microsoft.com/en-us/download/details.aspx?id=52666)


### <a name="avoiding-duplicate-data"></a>Ismétlődő adatok elkerülése

Ha egy SCOM ügyfél, nem konfigurálja a MOBILE szeretne közvetlenül kapcsolatba lépni a SCOM ügynökök, vagy adatok kétszer jelenteni.  A ADM ez kétszer a gép listában szereplő számítógépek eredményez.

MOBILE konfigurálása történjen csak egy, a következő helyeken:

- A felügyelt számítógépek SCOM konzol műveletek Management csomagja panel
- Azure műveleti háttérismeretek konfigurációs MMA tulajdonságok

Mindkét konfiguráció használata az egyes azonos munkaterületen elindítja az adatok másolása. Mindkét különböző munkaterületek használata eredményezhet adatok megakadályozhatják ADM teljesen halad ütköző konfigurációs (egy ADM megoldás engedélyezett és a másik nélkül).  

Figyelje meg, hogy akkor is, ha a számítógép magát a SCOM konzol MOBILE konfigurációban nincs megadva, ha egy példány csoport, például a "Windows Server-példányok csoport" aktív, akkor továbbra is vonhat a gép fogadó MOBILE konfiguráció SCOM keresztül.



## <a name="management-packs"></a>Adatkezelési csomagok
ADM MOBILE-munkaterületen aktív, amikor egy 300KB felügyeleti csomag a rendszer elküldi az összes a Microsoft figyelése ügynökök munkaterületen.  Alkalmazás használatakor SCOM ügynökök a [kezelés csoport csatlakozik](../log-analytics/log-analytics-om-agents.md), a ADM felügyeleti csomag az SCOM telepíthető.  Ha a ügynökök közvetlenül csatlakozik, a MP hozta MOBILE.

A MP Microsoft.IntelligencePacks.ApplicationDependencyMonitor*neve.  Az oktatóprogram *%Programfiles%\Microsoft figyelése Agent\Agent\Health State\Management szervizcsomagot\*.  Az adatforrás a felügyeleti csomag által használt *% Program files%\Microsoft figyelés Agent\Agent\Health szolgáltatás State\Resources\<AutoGeneratedID > \Microsoft.EnterpriseManagement.Advisor.ApplicationDependencyMonitorDataSource.dll*.



## <a name="configuration"></a>Konfiguráció
Nemcsak a Windows és Linux számítógépben ügynökszoftvert telepítve van, és a MOBILE csatlakozik, a függőség ügynök telepítő kell töltődnek le a ADM megoldást és legfelső szintű vagy a rendszergazda, majd minden egyes felügyelt kiszolgálóra telepített.  A jelentés MOBILE kiszolgálón a ADM agent telepítése után a ADM függőség térképek 10 percen belül jelenik meg.  Ha hibát tapasztal, kérjük, e-mailben [oms-adm-support@microsoft.com](mailto:oms-adm-support@microsoft.com).


### <a name="migrating-from-bluestripe-factfinder"></a>Áttérés a BlueStripe FactFinder
Alkalmazás függőség Monitor fog használhat az előadáshoz BlueStripe technológia be MOBILE fázisaiban. FactFinder meglévő ügyfelek esetén továbbra is támogatott, de már nem kapható egyes.  A függőséget Agent előzetes verziójában MOBILE csak kommunikálhat.  Ha a jelenlegi FactFinder ügyfele, kérjük, nevezze próba-kiszolgálók halmazának esetében, amelyek nem kezelhetők FactFinder ADM. 

### <a name="download-the-dependency-agent"></a>A függőséget Agent letöltése
A Microsoft adatkezelési ügynök (MMA) és a MOBILE Linux ügynök, amelyek a számítógép és a MOBILE közötti kapcsolat kívül alkalmazás függőség Monitor elemezheti összes számítógépre történő a függőség Agent kell rendelkeznie.  Linux a MOBILE Agent Linux a függőség Agent előtt kell telepíteni. 

![Alkalmazás függőség Monitor csempe](media/operations-management-suite-application-dependency-monitor/tile.png)

Annak érdekében, hogy a függőség Agent letöltéséhez kattintson az **Alkalmazás függőség Monitor** csempére kattintva nyissa meg a **Függőség ügynök** lap **Konfigurálása megoldás** .  A függőséget ügynök lap hivatkozásokat tartalmaz a Windows és az Linux ügynökök. A megfelelő hivatkozásra kattintva töltse le az egyes ügynök. További információt az alábbi szakaszok nézze meg a agent telepít különböző rendszerek.

### <a name="install-the-dependency-agent"></a>A függőséget Agent telepítése

#### <a name="microsoft-windows"></a>Microsoft Windows rendszerben
Rendszergazdai jogosultságokkal telepítése és eltávolítása a agent van szükség.

A függőséget Agent ADM-Agent-Windows.exe Windows számítógépen telepítve van. Ha ezt a végrehajtható fájlt nélkül azokat a beállításokat, majd elindul, a varázsló követő interaktív elvégezni a telepítést.  

A függőséget ügynök egyes Windows gépre telepítéséhez kövesse az alábbi lépéseket.

1.  Győződjön meg arról, hogy a MOBILE agent van telepítve, közvetlenül a MOBILE való csatlakozás mögött leírtak.
2.  Töltse le a Windows-ügynök, és indítsa el az alábbi paranccsal.<br>*ADM-Agent-Windows.exe*
3.  Kövesse a varázsló lépéseit a ügynököt.
4.  A függőséget Agent nem indul el, ha jelölje be a naplókat a hiba részletes információt. A naplózási könyvtár Windows ügynökök, az *C:\Program Files\BlueStripe\Collector\logs*. 

A Windows függőség ügynök el kell távolítani a Vezérlőpulton keresztül rendszergazda.


#### <a name="linux"></a>Linux
Rendszergazdai hozzáférés telepítése és konfigurálása a agent van szükség.

A függőséget Agent ADM-Agent-Linux64.bin, egy egy önálló kibontása bináris parancsprogram Linux számítógépen telepítve van. Futtathatja a fájl sh vagy hozzáadása végrehajtása engedélyekkel magát a fájlt.
 
A függőséget Agent minden Linux gépre telepítéséhez kövesse az alábbi lépéseket.

1.  Győződjön meg arról, hogy a MOBILE agent leírtak [vegyük fel a telepítve, és adatok kezelése Linux számítógépekről.  Meg kell a Linux függőség Agent előtt telepíteni](https://technet.microsoft.com/library/mt622052.aspx).
2.  A Linux függőség ügynököt a legfelső szintű a következő parancsot.<br>*sh ADM-Agent-Linux64.bin*.
3.  A függőséget Agent nem indul el, ha jelölje be a naplókat a hiba részletes információt. A naplózási könyvtár Linux ügynökök, az */var/opt/microsoft/dependency-agent/log*.

### <a name="uninstalling-the-dependency-agent-on-linux"></a>A függőséget Agent Linux eltávolítása
Távolítsa el teljesen az függőség ügynök Linux, el kell távolítania a agent magát, és a proxy, amely automatikusan települ ügynök.  Az alábbi egyetlen paranccsal mindkét távolíthatja el.

    rpm -e dependency-agent dependency-agent-connector


### <a name="installing-from-a-command-line"></a>A parancssorból telepítése
Az előző szakaszban nyújt útmutatást az alapértelmezett beállításokkal függőség Monitor ügynök telepítésével.  Az alábbi szakaszokban útmutatásokat való telepítésének a agent egyéni beállításokkal parancssorából.

#### <a name="windows"></a>A Windows
Az alábbi táblázat a beállítások segítségével a parancssorból elvégezhető a telepítést. A telepítés listájának jelölők futtassa a telepítő a a /? megjelölheti az alábbiak szerint.

    ADM-Agent-Windows.exe /?

| Jelölő | Leírás |
|:--|:--|
| /S | Felhasználói beavatkozás nélküli telepítés végrehajtásához. |

Alapértelmezés szerint a Windows függőség Agent fájlok *C:\Program Files\BlueStripe\Collector* kerülnek.


#### <a name="linux"></a>Linux
Az alábbi táblázat a beállítások segítségével elvégezni a telepítést. A telepítés jelölők listájának megtekintéséhez a telepítés alkalmazások – segítségével jelző az alábbi képlettel történik.

    ADM-Agent-Linux64.bin -help

| Jelölő
|:--|:--|
| -s | Felhasználói beavatkozás nélküli telepítés végrehajtásához. |
| – ellenőrzése | Engedélyek és az operációs rendszer ellenőrzi, de nem telepíti az ügynök. |

A függőséget Agent fájlokat a következő könyvtárak helyezi.

| Fájlok | Hely |
|:--|:--|
| Alapvető fájlok | /usr/lib/bluestripe-collector |
| Naplófájlok | /var/OPT/Microsoft/Dependency-Agent/log |
| Konfigurációs fájl | /ETC/OPT/Microsoft/Dependency-Agent/config |
| Szolgáltatás végrehajtható fájl | /sbin/bluestripe-collector<br>/sbin/bluestripe-collector-Manager |
| Bináris tárolófájlok | /var/OPT/Microsoft/Dependency-Agent/Storage |



## <a name="troubleshooting"></a>Hibaelhárítás
Ha függőség monitorral problémákat tapasztal, akkor is hibaelhárítási információk feljegyzése több összetevők, az alábbi információk alapján

### <a name="windows-agents"></a>A Windows ügynökök beállítása

#### <a name="microsoft-dependency-agent"></a>Függőség Microsoft Agent
A függőséget ügynök hibaelhárítási adatok létrehozása, nyisson meg egy parancssorablakot rendszergazdaként, és futtassa a CollectBluestripeData.vbs a következő parancsot.  Hozzáadhat a – Súgó flag kifejezéssel a további beállítások megjelenítéséhez.

    cd C:\Program Files\Bluestripe\Collector\scripts
    cscript CollectDependencyData.vbs

A támogatás adatok csomag menti a program a % használata % könyvtárban az aktuális felhasználó számára.  Használhatja a--fájl <filename> szeretné menteni a bemutatót egy másik helyre lehetőséget.

#### <a name="microsoft-dependency-agent-management-pack-for-mma"></a>A Microsoft függőség ügynök felügyeleti csomag az MMA
A függőséget ügynök felügyeleti csomag belül a Microsoft adatkezelési Agent fut.  Kapott adatokat az függőség ügynök és továbbítja a ADM felhőalapú szolgáltatáshoz.
  
Győződjön meg arról, hogy a felügyeleti csomag letöltése a következő lépések.

1.  Keresse meg a C:\Program Files\Microsoft figyelése Agent\Agent\Health State\Management szervizcsomagjai Microsoft.IntelligencePacks.ApplicationDependencyMonitor.mp nevű fájl.  
2.  Ha nem szerepel a fájlt, és a agent SCOM kezelése csoport csatlakozik, majd ellenőrizze, hogy azt importálta az SCOM, jelölje be a műveletek konzol a felügyeleti munkaterületi Management csomagok.

A ADM MP események a műveletek Manager Windows eseménynaplójába ír.  A napló lehet [szeretne keresni, a MOBILE](../log-analytics/log-analytics-log-searches.md) keresztül, a rendszer napló megoldást, amelyen megadhatja, melyik naplófájlok tölthet fel.  A hibakeresési események engedélyezve vannak, ha az alkalmazás eseménynaplójába a forrás *AdmProxy*az írás.

#### <a name="microsoft-monitoring-agent"></a>A Microsoft figyelése Agent
Diagnosztikai halad összegyűjtése, nyisson meg egy parancssorablakot rendszergazdaként, és futtassa az alábbi parancsokat: 

    cd \Program Files\Microsoft Monitoring Agent\Agent\Tools
    net stop healthservice 
    StartTracing.cmd ERR
    net start healthservice

Halad c:\Windows\Logs\OpsMgrTrace kerülnek.  A nyomkövetés a StopTracing.cmd leállíthatja.


### <a name="linux-agents"></a>Linux ügynökök beállítása

#### <a name="microsoft-dependency-agent"></a>Függőség Microsoft Agent
Hibaelhárítási adatok létrehozása a függőség ügynök, sudo vagy a legfelső szintű jogosultsággal rendelkezik fiókkal jelentkezzen be, és futtassa a következő parancsot.  Hozzáadhat a – Súgó flag kifejezéssel a további beállítások megjelenítéséhez.

    /usr/lib/bluestripe-collector/scripts/collect-dependency-agent-data.sh

A támogatás adatok csomag van mentve /var/opt/microsoft/dependency-agent/log (Ha a legfelső szintű) csoportban a Agent telepítési könyvtár, vagy az otthoni Directoryhoz annak a felhasználónak a parancsprogram futtatása (Ha nem legfelső szintű).  Használhatja a--fájl <filename> szeretné menteni a bemutatót egy másik helyre lehetőséget.

#### <a name="microsoft-dependency-agent-fluentd-plug-in-for-linux"></a>A Microsoft függőség ügynök Fluentd Linux beépülő modul
A függőséget ügynök Fluentd beépülő modul futtatja a MOBILE Linux Agent belül.  Kapott adatokat az függőség ügynök és továbbítja a ADM felhőalapú szolgáltatáshoz.  

A következő két fájlok naplók kerülnek.

- /var/OPT/Microsoft/omsagent/log/omsagent.log
- /var/Log/Messages

#### <a name="oms-agent-for-linux"></a>Linux MOBILE Agent
Hibaelhárítási erőforrás Linux kiszolgálók csatlakozhat MOBILE megtalálható itt: [https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/Troubleshooting.md](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/Troubleshooting.md) 

A naplókban a MOBILE Agent Linux olvassa el a */var és kiválaszthatják/a microsoft/omsagent/log/*.  

A naplókban omsconfig (ügynök konfigurációja) olvassa el a */var és kiválaszthatják/a microsoft/omsconfig/log/*.
 
A napló a OMI és SCX összetevők, amelyek mértékek teljesítményadatokat olvassa el a */var és kiválaszthatják/omi/log/* és */var/opt/microsoft/scx/log*.


## <a name="data-collection"></a>Adatok gyűjtése
Napi, attól függően, hogy hogyan összetett a rendszer függőségek olyan nagyjából 25MB küldött minden ügynök számíthat.  ADM függőség adatokat küldi el minden ügynök minden 15 másodperc.  

A ADM Agent általában a 0,1 rendszer memória 0,1 %-rendszer Processzor fogyaszt.


## <a name="supported-operating-systems"></a>Támogatott operációs rendszerek
A következő szakaszokban a támogatott operációs rendszerek az függőség ügynök.   a 32 bites architektúrákban nem támogatottak olyan operációs rendszerekhez.

### <a name="windows-server"></a>A Windows Server
- A Windows Server 2012 R2
- A Windows Server 2012
- A Windows Server 2008 R2 SP1

### <a name="windows-desktop"></a>Windows asztali
- Megjegyzés: A Windows 10 még nem támogatott
- Windows 8.1
- Windows 8-ban
- Windows 7 rendszerben

### <a name="red-hat-enterprise-linux-centos-linux-and-oracle-linux-with-rhel-kernel"></a>Piros kalap vállalati Linux, CentOS Linux és Oracle Linux (a RHEL Kernel)
- Csak alapértelmezett és SMP Linux kernel verziókban támogatottak.
- Szabványos kernel elengedi, például a nem támogatott bármely Linux eloszlás Xen, és a fizikai. Például a Megjelenés "2.6.16.21-0.8-xen" karakterláncot a rendszer nem támogatott.
- Egyéni mag, például a szokásos mag, újrafordításokat nem támogatottak.
- Centos plusz kernel nem támogatott.
- Az Oracle szoros Kernel (UEK) egy másik az alábbi szakasz vonatkozik.


#### <a name="red-hat-linux-7"></a>Piros kalap Linux 7
| Operációs rendszer | Kernelverziót |
|:--|:--|
| 7.0-s | 3.10.0-123 |
| 7.1. | 3.10.0-229 |
| 7.2. | 3.10.0-327 |

#### <a name="red-hat-linux-6"></a>Piros kalap Linux 6
| Operációs rendszer | Kernelverziót |
|:--|:--|
| 6.0 alkalmazásban | 2.6.32-71 |
| 6.1. | 2.6.32-131 |
| 6.2. | 2.6.32-220 |
| 6.3. | 2.6.32-279 |
| 6.4 | 2.6.32-358 |
| 6.5 rendszeren | 2.6.32-431 |
| 6.6. | 2.6.32-504 |
| a 6,7 | 2.6.32-573 |
| 6.8 | 2.6.32-642 |

#### <a name="red-hat-linux-5"></a>Piros kalap Linux 5
| Operációs rendszer | Kernelverziót |
|:--|:--|
| 5.8. | 2.6.18-308 |
| 5.9. | 2.6.18-348 |
| 5.10. | 2.6.18-371 |
| 5.11. | 2.6.18-398<br>2.6.18-400<br>2.6.18-402<br>2.6.18-404<br>2.6.18-406<br>2.6.18-407<br>2.6.18-408<br>2.6.18-409<br>2.6.18-410<br>2.6.18-411 |

#### <a name="oracle-enterprise-linux-w-unbreakable-kernel-uek"></a>Az Oracle vállalati Linux w/ szoros Kernel (UEK)

#### <a name="oracle-linux-6"></a>Az Oracle Linux 6
| Operációs rendszer | Kernelverziót
|:--|:--|
| 6.2. | Az Oracle 2.6.32-300 (UEK R1) |
| 6.3. | Az Oracle 2.6.39-200 (UEK R2) |
| 6.4 | Az Oracle 2.6.39-400 (UEK R2) |
| 6.5 rendszeren | Az Oracle 2.6.39-400 (UEK R2 i386) |
| 6.6. | Az Oracle 2.6.39-400 (UEK R2 i386) |

#### <a name="oracle-linux-5"></a>Az Oracle Linux 5

| Operációs rendszer | Kernelverziót
|:--|:--|
| 5.8. | Az Oracle 2.6.32-300 (UEK R1) |
| 5.9. | Az Oracle 2.6.39-300 (UEK R2) |
| 5.10. | Az Oracle 2.6.39-400 (UEK R2) |
| 5.11. | Az Oracle 2.6.39-400 (UEK R2) |

#### <a name="suse-linux-enterprise-server"></a>SUSE Linux vállalati kiszolgáló

#### <a name="suse-linux-11"></a>SUSE Linux 11
| Operációs rendszer | Kernelverziót
|:--|:--|
| 11 | 2.6.27 |
| 11 SP1 | 2.6.32 |
| 11 SP2 | 3.0.13 |
| 11 SP3 | 3.0.76 |
| 11 SP4 | 3.0.101 |

#### <a name="suse-linux-10"></a>SUSE Linux 10
| Operációs rendszer | Kernelverziót
|:--|:--|
| 10 SP4 | 2.6.16.60 |

## <a name="diagnostic-and-usage-data"></a>Diagnosztikai és használati adatainak
A Microsoft automatikusan összegyűjti a használja az alkalmazás függőség Monitor szolgáltatás használatát és a teljesítmény adatoknak. Microsoft adja meg, és javíthatja minőségét, biztonság és az alkalmazás függőség Monitor szolgáltatás integritását használja ezeket az adatokat. Adatok tartalmazza az operációs rendszer és verziója például szoftver a konfigurációs adatait és IP-cím, a DNS-neve és a munkaállomásnév pontos és hatékony hibaelhárítási lehetőségek biztosítása érdekében. Azt nem gyűjt a neveket, címeket és egyéb kapcsolattartási adatait.

Adatok gyűjtése és használatát a további tudnivalókért lásd a [Microsoft Online szolgáltatások adatvédelmi nyilatkozata](https://go.microsoft.com/fwlink/?LinkId=512132).



## <a name="next-steps"></a>Következő lépések
- Megtudhatja, hogyan [alkalmazás függőség Monitor használata](operations-management-suite-application-dependency-monitor.md) egyszer azt telepítése és beállítása megtörtént.
