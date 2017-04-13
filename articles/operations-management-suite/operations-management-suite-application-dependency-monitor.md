<properties
   pageTitle="Alkalmazás függőség Monitor (OPA) a tevékenységek kezelése rendszerben (MOBILE) |} Microsoft Azure"
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

# <a name="using-application-dependency-monitor-solution-in-operations-management-suite-oms"></a>Alkalmazás függőség Monitor megoldás műveletek Management csomagja (MOBILE) használata
![Figyelmeztető Management ikon](media/operations-management-suite-application-dependency-monitor/icon.png) Alkalmazás függőség Monitor (OPA) automatikusan találja a Windows és Linux rendszerben alkalmazásösszetevők és társít a szolgáltatások közötti kommunikációt. Lehetővé teszi a megtekintheti a kiszolgálóját úgy gondolja, hogy őket – az összekapcsolt kritikus szolgáltatások előadása rendszerek.  Alkalmazás függőség képernyője, folyamatok, kiszolgálók közötti kapcsolatokat, és a szükséges portok bármely TCP-kompatibilis architektúra nincs beállításokkal különböző, telepített ügynökszoftvert más.

Ez a cikk ismerteti az alkalmazás függőség monitort használ részleteit.  ADM és bevezetési ügynökök konfigurálásával kapcsolatos további tudnivalókért lásd [konfigurálása alkalmazás függőség Monitor megoldás műveletek Management csomagja (MOBILE)](operations-management-suite-application-dependency-monitor-configure.md)

>[AZURE.NOTE]Alkalmazás függőség Monitor jelenleg személyes előzetes verzióban.  A ADM magánjellegű előzetes [https://aka.ms/getadm](https://aka.ms/getadm)a hozzáférést kérhet.
>
>A magánjellegű villámnézetben minden MOBILE fiók van ADM. korlátlan hozzáférés  ADM csomópontok ingyenes, de napló Analytics adatainak AdmComputer_CL és AdmProcess_CL típusnál fog forgalmi, mint bármely más megoldás.
>
>Miután ADM nyilvános előzetes lép, csak a betekintést és a MOBILE árak megtervezése az analitikai ingyenes és a díjköteles ügyfelek számára érhető el lesz.  Ingyenes réteg fiókok 5 ADM csomópontok korlátozódik.  Ha a személyes megtekintés vesz és vannak nem tehát a MOBILE árak tervet, amikor ADM belép nyilvános előzetes, ADM adott időpontban le lesznek tiltva. 


## <a name="use-cases-make-your-it-processes-dependency-aware"></a>Használati eset: Ellenőrizze a informatikai feldolgozásával tartsa szem előtt függőség

### <a name="discovery"></a>Feltárás
ADM függőségek közös hivatkozás térképe automatikusan létrehozza a kiszolgálókat, folyamatok és 3 külső szolgáltatásokra.  Találja, és a megfeleltetések összes TCP függőségek, jelzés nélküli kapcsolatok távoli azonosító 3 külső rendszerek, attól függenek, és a hálózat, például a DNS- és az Active Directory hagyományos sötét területeire való függőségek.  ADM feltárja a hibás hálózati kapcsolatok, a felügyelt rendszerek létesíteni, ezzel megkönnyítve a potenciális kiszolgáló helytelen konfigurálása, a szolgáltatás kimaradások és a hálózati problémák azonosításában.

### <a name="incident-management"></a>Az esemény kezelése
ADM segítséget nyújt a döntési feladatokat probléma elkülönítési kiküszöbölheti jeleníti meg, hogyan rendszerek kapcsolódik, és érintő egymást.  Nemcsak a hibás kapcsolatok a csatlakoztatott ügyfelek adatainak helytelenül vannak beállítva terheléselosztókkal, kritikus szolgáltatások meglepő vagy túl nagy terhelés azonosítása és engedélyezetlen ügyfelek, mint például a fejlesztői gépek munkakörnyezetben beszélgetésben segítséget nyújt.  Beépített munkafolyamatok a MOBILE követést módosítása lehetővé teszi is látni szeretné, hogy a change esemény háttéradatbázist gépre vagy szolgáltatás ismerteti a kiváltóok-esemény.

### <a name="migration-assurance"></a>Áttelepítési garancia
ADM lehetővé teszi hatékony megtervezéséhez, felgyorsítsa és Azure áttelepítések, ellenőrizze, hogy semmi nem marad, és nincsenek nincs jelzés nélküli kimaradások biztosítása.  Összefüggő rendszerek kell közös áttelepítése, mérje fel, hogy a rendszer konfigurációja, és a beosztását, és adja meg, hogy a futó rendszer felhasználók továbbra is egyszerre szolgál, vagy egy jelölt az áttelepítés helyett leszerelje fedezhet.  Miután végzett az áthelyezés, érdemes a ügyfél betöltése és -Identitáskezelés vizsgálati rendszerek és ügyfelek csatlakozás ellenőrzéséhez.  Az alhálózathoz tervezés és a tűzfalat definíciók problémák merültek, ha sikertelen kapcsolatok ADM mértékben fog mutatni, a rendszerek, amely a kapcsolat szükséges.

### <a name="business-continuity"></a>Üzleti folytonosságot
Ha Azure webhely helyreállítási használ, és van szüksége a helyreállítási sorrendjének alkalmazás környezetben, ADM definiáló súgó automatikusan tekintheti meg, hogyan rendszerek támaszkodhat egymást ahhoz, hogy a helyreállítási csomagja megbízható.  Válassza a kritikus kiszolgáló, és az ügyfelek megtekintése, az előtér-rendszerek, amely csak azt követően, hogy kritikus kiszolgáló, visszaállított, és a rendelkezésre álló állíthatók azonosíthatja.  Viszont a kritikus kiszolgáló háttéradatbázist függőségek megnézve megállapítható, hogy a fókusz rendszer előtt lehet helyreállítani javításuk.

### <a name="patch-management"></a>Javítások kezelése
ADM jeleníti meg, amelyek más csapatok és kiszolgálók attól függenek, a szolgáltatást, így akkor is értesíteni őket előre mielőtt a rendszer az javítása hatékonyabb MOBILE rendszer Update szolgáltatás használatát.  ADM is hatékonyabb közös munkát MOBILE javítás kezelését jeleníti meg, hogy a szolgáltatások-e a rendelkezésre álló és megfelelően csatlakoztatva van javítással és újraindítása után. 


## <a name="mapping-overview"></a>Hozzárendelés – áttekintés
ADM ügynökök gyűjtse össze a TCP-kompatibilis folyamatok információi a kiszolgálón, amelyen telepítve van, valamint a bejövő és kimenő kapcsolatok minden folyamathoz olvashat.  A gép listákkal a ADM megoldást bal oldalán, ADM anyagokkal gépek kiválasztott a függőségeket javasol a kijelölt időtartomány fölé.  Gépi függőség rendeli hozzá a fókuszt egy adott számítógépre, és a közvetlen TCP-ügyfelek és a kiszolgálón, hogy a gép gépek megjelenítése.

![ADM – áttekintés](media/operations-management-suite-application-dependency-monitor/adm-overview.png)

Gépek kibontható az aktív hálózati kapcsolat futó folyamatok megjelenítése a kijelölt időtartomány során a térképen.  A távoli számítógép ADM ügynökszoftvert a folyamat részletes adatainak megjelenítéséhez kibontva csak a kommunikáció a fókusz gép folyamatok jelennek meg.  A fókusz gépen való csatlakozás agentless előtér-gépek számát jelzi a folyamatok csatlakoznak bal oldalán.  Ha a fókusz gép kapcsolat nélkül ügynökkel, háttéradatbázist géphez lehetővé teszi a, a háttéradatbázist jeleníti meg a térképen IPv4-címének megjelenítő csomóponton, és a csomópont kibontható szeretne megjeleníteni az egyes portokat és szolgáltatásokat, a fókusz gép kommunikál.

Alapértelmezés szerint ADM térképek megjelenítése az utolsó 10 perc az objektumfüggőségekre vonatkozó információk.  Az idő vezérlők használja a bal felső sarokban, térképek lehet lekérdezni előzményidő tartományok, akár egy óra széles körben, hogyan függőségek volt a múltbeli, például egy esemény során, vagy megváltozott előtt megjelenítéséhez.    A kifizetett munkaterületek 30 nap, és a szabad-munkaterületek 7 napig ADM adatokat tárolja.

![A kijelölt gépi tulajdonságok gépi térkép](media/operations-management-suite-application-dependency-monitor/machine-map.png)

## <a name="failed-connections"></a>Hibás kapcsolatok
Hibás kapcsolatok együtt jelennek meg a térképeken ADM folyamatok és a számítógépen, egy piros vonal szaggatott megjelenítő, ha egy ügyfél rendszer hibás egy folyamatban vagy port eléréséhez.  Hibás kapcsolatok kísérel meg a sikertelen kapcsolattal azzal, hogy a rendszer esetén a telepített ADM ügynök bármely rendszerből jelentik.  ADM méri, ez nem sikerül kapcsolatot létesíteni, TCP sockets megfigyelésével.  Ennek oka lehet tűzfalat, az ügyfél vagy a kiszolgáló vagy a távoli szolgáltatás nem érhető el a helytelen beállítása. 

![Hibás kapcsolatok](media/operations-management-suite-application-dependency-monitor/failed-connections.png)

Hibás kapcsolatok segítséget hibaelhárítás áttelepítési érvényességi biztonsági elemzés és teljes építészeti ismertetése ismertetése.  Előfordul, hogy nem sikerült kapcsolatok érintetlen, de ezeket gyakran mutasson a közvetlenül a problémát, például váratlanul a nem érhető el, valamit feladatátvevő környezetben egy vagy két alkalmazás rétegek nem tudják beszélhet az a felhő az áttelepítés után.  A fenti képen az IIS és WebSphere mindkét futtatja, de azok nem tud csatlakozni. 

## <a name="computer-and-process-properties"></a>Számítógép és a folyamat tulajdonságai
Navigálás a ADM interaktív, gépek és folyamatok tulajdonságaik kapcsolatos további információkat szerezhet választhatja ki.  Gépeken futó azokat tartománynév, IPv4-címei, Processzor- és beosztását, virtuális típusa, operációs rendszer verziója, idő utolsó indítsa újra, és a MOBILE és a ADM ügynökök azonosítóit.

Részletes folyamatábra vannak gyűjtött rendszeren futó folyamatok, beleértve a folyamat neve, folyamat leírása, felhasználónév és -tartományában (Windows), vállalatnév, terméknév, a termék verziója, munkakönyvtárat, parancssori és folyamat kezdetének metaadatait.

![Folyamat tulajdonságai](media/operations-management-suite-application-dependency-monitor/process-properties.png)

A folyamat összefoglalása panel kapcsolódási ezt a folyamatot, például a kötött portok, a bejövő és kimenő kapcsolatok további információval szolgál, és nem sikerült a kapcsolatok. 

![Folyamat összefoglalása](media/operations-management-suite-application-dependency-monitor/process-summary.png)

## <a name="oms-change-tracking-integration"></a>Változáskövetési integrációs MOBILE
ADM's-integráció a változások követése esetén automatikus két megoldás engedélyezve és beállítva a MOBILE munkaterületen.

A gép Összegzés ablaktábla azt jelzi, hogy változások követése események lépett fel a kijelölt gépen a kijelölt időtartomány.

![Gépi az Összegzés lapon](media/operations-management-suite-application-dependency-monitor/machine-summary.png)

A gép módosítása követés panelen az összes módosítás, a legutóbbi első, és hatoljon le további információt a napló keresési mutató hivatkozást tartalmazó listáját jeleníti meg.
![Gépi módosítása a nyomon követés Panel](media/operations-management-suite-application-dependency-monitor/machine-change-tracking.png)

Következő konfigurációs módosítást nézetét egy Lehatolás a **napló Analytics megjelenítése**kijelölése után.
![Konfigurációs módosítást](media/operations-management-suite-application-dependency-monitor/configuration-change-event.png)


## <a name="log-analytics-records"></a>Naplóbejegyzések Analytics
[Keresés](../log-analytics/log-analytics-log-searches.md) a napló Analytics ADM a számítógép és a folyamat készlet adatokat érhető el.  Ez áttelepítési tervezés, kapacitás elemzés, feltárás és alkalmi teljesítmény hibaelhárítási esetek alkalmazhatók. 

Egy rekordot minden egyedi számítógépen óránként jön létre, és nemcsak a rekordok folyamat mikor jönnek létre, hogy folyamat vagy a számítógép indítása vagy a-szállt való ADM.  Ezeket a rekordokat tárolja az a tulajdonságokat az alábbi táblázatokban. 

Vannak olyan egyedi folyamatok és számítógépek használható belső létrehozott tulajdonságok:

- PersistentKey_s egyedileg határozzák meg a folyamat konfigurációja, például a parancssor és a felhasználói azonosító  Egy adott számítógéphez egyedi, de megismételhető számítógépei között.
- ProcessId_s és ComputerId_s egyediek globálisan a ADM modell.



### <a name="admcomputercl-records"></a>AdmComputer_CL rekordok
Egy **AdmComputer_CL** típusú rekordok készlet adatokat ADM anyagokkal kiszolgálók van.  Ezeket a rekordokat az alábbi táblázat az jellemzőkkel rendelkeznek.  

| A tulajdonság | Leírás |
|:--|:--|
| Típus | *AdmComputer_CL* |
| SourceSystem | *OpsManager* |
| ComputerName_s | Windows vagy Linux rendszerhez számítógép nevét. |
| CPUSpeed_d | Processzor sebességét MHz-ben |
| DnsNames_s | A számítógép összes DNS-nevek listája |
| IPv4s_s | Ezen a számítógépen használja az összes IPv4-címek listája |
| IPv6s_s | Ezen a számítógépen használja az összes IPv6-címek listája.  (ADM IPv6-címei azonosítja, de nem talál az IPv6-függőségeket.) |
| Is64Bit_b | IGAZ vagy hamis alapján operációs rendszer típusa |
| MachineId_s | Belső GUID, egyedi át egy MOBILE munkaterülethez  |
| OperatingSystemFamily_s | Windows vagy Linux rendszerhez |
| OperatingSystemVersion_s | Hosszú karakterláncok a operációs rendszer verziója |
| TimeGenerated | Dátum és idő, amely a rekord létrehozásának. |
| TotalCPUs_d | Processzormagok száma |
| TotalPhysicalMemory_d | A memória kapacitás MB-ban |
| VirtualMachine_b | IGAZ vagy hamis, hogy-e operációs rendszer virtuális vendégként alapján |
| VirtualMachineID_g | A Hyper-V virtuális azonosító |
| VirtualMachineName_g | A Hyper-V virtuális neve |
| VirtualMachineType_s | Hyperv, Vmware, Xen, Kvm, Ldom, Lpar, Virtualpc |


### <a name="admprocesscl-type-records"></a>AdmProcess_CL típusú rekordok 
Egy **AdmProcess_CL** típusú rekordok szerepelnek a TCP-kompatibilis folyamatok készlet adatok ADM anyagokkal kiszolgálókon.  Ezeket a rekordokat az alábbi táblázat az jellemzőkkel rendelkeznek.

| A tulajdonság | Leírás |
|:--|:--|
| Típus | *AdmProcess_CL* |
| SourceSystem | *OpsManager* |
| CommandLine_s | A folyamat Sormagasság parancs |
| CompanyName_s | Vállalat nevét (a Windows PE vagy Linux RPM) |
| Description_s | Hosszú folyamat leírása (a Windows PE vagy Linux RPM) |
| FileVersion_s | Végrehajtható fájl verzióját (a Windows PE, csak Windows) |
| FirstPid_d | Operációs rendszer folyamat azonosítója |
| InternalName_s | Végrehajtható fájl belső neve (a Windows PE, csak Windows) |
| MachineId_s | Belső globálisan egyedi azonosítója egyedi át egy MOBILE munkaterülethez  |
| Name_s | A folyamat végrehajtható neve |
| Path_s | A folyamat végrehajtható fájl elérési rendszer |
| PersistentKey_s | Belső globálisan egyedi azonosítója egyedi belül ezen a számítógépen |
| PoolId_d | Belső értékek alapján hasonló parancssorokat folyamatok azonosítója. |
| ProcessId_s | Belső globálisan egyedi azonosítója egyedi át egy MOBILE munkaterülethez  |
| ProductName_s | Termék neve karakterlánccal (Windows PE vagy Linux RPM) |
| ProductVersion_s | Termék verziója karakterlánccal (Windows PE vagy Linux RPM) |
| StartTime_t | A helyi számítógép óráját folyamat kezdetének |
| TimeGenerated | Dátum és idő, amely a rekord létrehozásának. |
| UserDomain_s | Tartomány folyamat tulajdonosának (csak Windows) |
| UserName_s | (Csak Windows) folyamat tulajdonosának neve |
| WorkingDirectory_s | Folyamat munkakönyvtár |


## <a name="sample-log-searches"></a>Minta napló keresések

### <a name="list-the-physical-memory-capacity-of-all-managed-computers"></a>A lista összes felügyelt számítógépek fizikai memória kapacitása. 
Típus = AdmComputer_CL |} Jelölje ki a TotalPhysicalMemory_d, ComputerName_s |} Dedup ComputerName_s

![ADM lekérdezési példája](media/operations-management-suite-application-dependency-monitor/adm-example-01.png)

### <a name="list-computer-name-dns-ip-and-os-version"></a>Lista számítógép neve, a DNS-, IP és operációs rendszer verziója.
Típus = AdmComputer_CL |} Válassza a ComputerName_s, OperatingSystemVersion_s, DnsNames_s, IPv4s_s |} dedup ComputerName_s

![ADM lekérdezési példája](media/operations-management-suite-application-dependency-monitor/adm-example-02.png)

### <a name="find-all-processes-with-sql-in-the-command-line"></a>Keresse meg a parancssorból "sql" az összes folyamat
Típus = AdmProcess_CL CommandLine_s = \*sql\* |} dedup ProcessId_s

![ADM lekérdezési példája](media/operations-management-suite-application-dependency-monitor/adm-example-03.png)

### <a name="after-viewing-event-data-for-given-process-use-its-machine-id-to-retrieve-the-computers-name"></a>Esemény adatai megtekintése az adott után feldolgozása, akkor használja a gép Azonosítójával beolvasni a számítógép neve
Típus = "m!m-9bb187fa-e522-5f73-66d2-211164dc4e2b" AdmComputer_CL |} Eltérők ComputerName_s

![ADM lekérdezési példája](media/operations-management-suite-application-dependency-monitor/adm-example-04.png)

### <a name="list-all-computers-running-sql"></a>SQL futtató számítógépeknek lista
Típus = AdmComputer_CL MachineId_s IN {típus = AdmProcess_CL \*sql\* |} Eltérők MachineId_s} |} Eltérők ComputerName_s

![ADM lekérdezési példája](media/operations-management-suite-application-dependency-monitor/adm-example-05.png)

### <a name="list-of-all-unique-product-versions-of-curl-in-my-datacenter"></a>A adatközpontban curl összes termék egyedi verziójának listája
Típus = AdmProcess_CL Name_s = curl |} Eltérők ProductVersion_s

![ADM lekérdezési példája](media/operations-management-suite-application-dependency-monitor/adm-example-06.png)

### <a name="create-a-computer-group-of-all-computers-running-centos"></a>Az összes futtató CentOS számítógép csoport létrehozása

![ADM lekérdezési példája](media/operations-management-suite-application-dependency-monitor/adm-example-07.png)



## <a name="diagnostic-and-usage-data"></a>Diagnosztikai és használati adatainak
A Microsoft automatikusan összegyűjti a használja az alkalmazás függőség Monitor szolgáltatás használatát és a teljesítmény adatoknak. Microsoft adja meg, és javíthatja minőségét, biztonság és az alkalmazás függőség Monitor szolgáltatás integritását használja ezeket az adatokat. Adatok tartalmazza az operációs rendszer és verziója például szoftver a konfigurációs adatait és IP-cím, a DNS-neve és a munkaállomásnév pontos és hatékony hibaelhárítási lehetőségek biztosítása érdekében. Azt nem gyűjt a neveket, címeket és egyéb kapcsolattartási adatait.

Adatok gyűjtése és használatát a további tudnivalókért lásd a [Microsoft Online szolgáltatások adatvédelmi nyilatkozata](hhttps://go.microsoft.com/fwlink/?LinkId=512132).



## <a name="next-steps"></a>Következő lépések
- További információ a [kereséseket jelentkezzen](../log-analytics/log-analytics-log-searches.md] in Log Analytics to retrieve data collected by Application Dependency Monitor.)
