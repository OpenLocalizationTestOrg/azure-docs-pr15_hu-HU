<properties
    pageTitle="Milyen munkaterhelésekből is védelme az Azure-webhely helyreállításhoz?"
    description="Azure webhely helyreállítási védi az munkaterhelésének és alkalmazások összehangolása a replikáció, feladatátvevő és helyreállítási helyszíni virtuális gépeken futó és fizikai kiszolgálók Azure vagy egy másodlagos helyszíni webhely"
    services="site-recovery"
    documentationCenter=""
    authors="rayne-wiselman"
    manager="cfreeman"
    editor=""/>

<tags
    ms.service="site-recovery"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.workload="storage-backup-recovery"
    ms.date="10/10/2016"
    ms.author="raynew"/>

# <a name="what-workloads-can-you-protect-with-azure-site-recovery"></a>Milyen munkaterhelésekből is védelme az Azure-webhely helyreállításhoz?


Ez a cikk ismerteti a munkaterhelésének és alkalmazásokat, hogy bizonyos az Azure webhely helyreállítás szolgáltatással.

Ez a cikk, illetve a [Azure helyreállítási szolgáltatások fórum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)alsó megjegyzések vagy kérdések tartalmakat tehet közzé.

## <a name="overview"></a>– Áttekintés

Szervezetek kell az üzleti folytonosságot és katasztrófa helyreállítási (BCDR) stratégia megtartása munkaterhelésének és adatok biztonságos és a rendelkezésre álló tervezett és a tervezett legrövidebb leállás során, és helyreállítása a normál munka feltétel minél korábban beállítást.

Webhely-helyreállítás egy Azure szolgáltatás, amely a BCDR vonatkozó stratégia beleszámít. Webhely helyreállítási használ, telepítheti a replikáció alkalmazás szem előtt a felhőben, illetve egy másodlagos webhelyen. Az alkalmazásokat, a Windows vagy Linux rendszerhez, a VMware vagy a Hyper-V fizikai kiszolgálón futó webhely helyreállítási való replikáció téve használhatja, hogy hajtsa végre a katasztrófa helyreállítási tesztelés, és futtatása feladatátadás és visszaállás.


Webhely helyreállítási integrálódik a Microsoft-alkalmazások, többek között a SharePoint, az Exchange, Dynamics, SQL Server és az Active Directory. A Microsoft is használható, szorosan vezető szállítókkal, beleértve az Oracle, SAP, IBM és piros kalap. Alkalmazás-szerint-app kombinálásával replikációs megoldások is testre szabhatja.

## <a name="why-use-site-recovery-for-application-replication"></a>Miért érdemes használni a webhely helyreállítási alkalmazás replikáció?

Webhely helyreállítási hozzájárul alkalmazás szintű védelmet és helyreállítási az alábbi képlettel történik:

- Alkalmazás-felismerése nélkül, bármilyen támogatott gépen futó feladatok a replikáció kezeléséről.
- Közelében szinkron replikációs RPOs legalacsonyabb 30 másodperc a legfontosabb üzleti alkalmazások igényeinek megfelelően.
- Alkalmazás egységes pillanatképek egy vagy több szálon alkalmazásait.
- Az SQL Server AlwaysOn és más alkalmazásszintű replikációs technológiákkal, például az Active Directory-replikáció, SQL AlwaysOn, Exchange-adatbázis elérhetősége csoportok (DAGs) és az Oracle-adatok Guard partnerség integráció.
- Rugalmas helyreállítási csomag esetén, amely lehetővé teszi a teljes alkalmazás Papírhalom egyetlen kattintással helyreállítása, és szeretné hozzáadni a terv külső parancsprogramokat és a kézi műveletek tartalmazza.
- Speciális hálózat kezelése az alkalmazás hálózati követelményeknek, az azt jelenti, hogy lefoglalása IP-címek, beleértve egyszerűsítése érdekében webhely helyreállítási és Azure beállítása terheléselosztás és integráció az Azure forgalom Manager alacsony RTO hálózati switchovers.
-  Gazdag automatizálási tárban, ahol a gyártási – kész, az alkalmazás-specifikus parancsprogramok letöltött, és helyreállítási tervek integrálva.



## <a name="workload-summary"></a>Összefoglaló terhelést

Webhely helyreállítási hogy bizonyos bármilyen támogatott a gépen futó alkalmazást. Ezenkívül azt már közösen további alkalmazás-specifikus tesztelés végrehajtása a termék csoportoknak a.

**Terhelést** | **A Hyper-V VMs bizonyos másodlagos webhelyre** | **A Hyper-V VMs bizonyos az Azure** | **Bizonyos VMware VMs másodlagos webhelyre** | **Azure való replikáció a VMware VMs**
---|---|---|---|---
Az Active Directory, a DNS | Y | Y | Y | Y
Web Apps alkalmazások (IIS, SQL) | Y | Y | Y | Y
System Center Operations Manager | Y | Y | Y | Y
A SharePoint | Y | Y | Y | Y
SAP –<br/><br/>SAP – webhely bizonyos az Azure nem fürthöz | Y (a Microsoft által vizsgált) | Y (a Microsoft által vizsgált) | Y (a Microsoft által vizsgált) | Y (a Microsoft által vizsgált)
Exchange (nem DAG) | Y | hamarosan | Y | Y
Távoli asztali/VDI | Y | Y | Y | A #HIÁNYZIK
Linux (operációs rendszer és alkalmazások) | Y (a Microsoft által vizsgált) | Y (a Microsoft által vizsgált) | Y (a Microsoft által vizsgált) | Y (a Microsoft által vizsgált)
Dynamics AX ALKALMAZÁSHOZ | Y | Y | Y | Y
Dynamics CRM-hez | Y | hamarosan | Y | hamarosan
Az Oracle | Y (a Microsoft által vizsgált) | Y (a Microsoft által vizsgált) | Y (a Microsoft által vizsgált) | Y (a Microsoft által vizsgált)
A Windows Server-fájl | Y | Y | Y | Y


## <a name="replicate-active-directory-and-dns"></a>Az Active Directory és a DNS-replikáció

Az Active Directory és a DNS-infrastruktúrát fontosak a legtöbb vállalati alkalmazás között. Vészhelyreállítás, során kell védelme és helyreállítása az infrastruktúra-összetevők, a munkaterhelésének és alkalmazások helyreállítása előtt.

A teljes automatikus katasztrófa helyreállítási terv létrehozása az Active Directory és a DNS-webhely helyreállítási is használhatja. Tegyük fel például, nem sikerül kívánt keresztül a SharePoint és az SAP egy elsődleges, másodlagos webhelyre, beállíthatja a helyreállítási terv, amely nem sikerül az Active Directory fölé, majd a további alkalmazás-specifikus helyreállítási csomagjával átadni az alkalmazásokat, az Active Directory támaszkodó.

[Tudjon meg többet](site-recovery-active-directory.md) is megtudhat a jelszóval védenie az Active Directory és a DNS.

## <a name="protect-sql-server"></a>Az SQL Server védelme

Az SQL Server egy adatok szolgáltatások foundation biztosít adatszolgáltatások sok vállalati verziójának alkalmazásai egy helyszíni adatok központban.  Védelme a többszörös többszintű vállalati alkalmazások által használt SQL Server webhely helyreállítási használható, SQL Server HA/DR-technológiák együtt. Webhely helyreállítási nyújtja:

- Az SQL Server egyszerű és költséghatékony katasztrófa helyreállítási megoldás. Bizonyos több verziója és az SQL Server önálló kiszolgálók és fürt, kiadása Azure vagy másodlagos webhelyre.  
- Integráció a SQL AlwaysOn elérhetősége csoportokkal feladatátvétel és Azure webhely helyreállítási helyreállítási csomagok visszaállás kezeléséhez.
- Végpont helyreállítás tervet, az összes rétegek az alkalmazásokban, például az SQL Server-adatbázisból.
- Az SQL Server-csúcs méretezés tölt be a webhely-helyreállítás "felszakítási" őket az Azure-ban nagyobb IaaS virtuális gép méretek szerint.
- Egyszerű vizsgálata az SQL Server vészhelyreállítás. Adatok elemzése és megfelelés szempontú vizsgálatok, futtassa a termelési környezetén érintő nélkül próba feladatátadás futtathatja.

[Tudjon meg többet](site-recovery-sql.md) is megtudhat a jelszóval védenie az SQL server.

##<a name="protect-sharepoint"></a>A SharePoint védelme

Azure webhely helyreállításhoz szükséges adatok SharePoint-telepítésekről, az alábbi képlettel történik:

- Segítségre van szüksége, és a társított infrastruktúra költségeit vészhelyreállítás készenléti farm megszünteti. Webhely-helyreállítás segítségével bizonyos egy teljes farm (webhelyén, az alkalmazás és az adatbázis réteg), és Azure másodlagos webhelyre.
- Az alkalmazások telepítésének és kezelési leegyszerűsíti. Az elsődleges webhelyre telepíti, frissítések automatikusan replikált, és így feladatátvevő és egy másodlagos webhely farm helyreállítása után. Is csökkenti a kezelésének bonyolultsága és készenléti farm frissítése társított költségek.
- Egyszerűbbé teszi a SharePoint-alkalmazások fejlesztése, és a tesztelés, tesztelés és hibakeresés egy gyártási hasonló másolás igény szerinti replika környezet létrehozásával.
- Webhely-helyreállítás segítségével a SharePoint-környezetekben áttelepítése Azure leegyszerűsíti térhet át a felhőalapú levelezésre.

[Tudjon meg többet](https://gallery.technet.microsoft.com/SharePoint-DR-Solution-f6b4aeae) is megtudhat a SharePoint védelme.


## <a name="protect-dynamics-ax"></a>Dynamics AX védelme

Azure webhely helyreállításhoz szükséges adatok a Dynamics AX ERP megoldás szerint:

- A teljes Dynamics AX környezet (webes és AOS rétegek, adatbázis rétegek, a SharePoint) Azure, illetve egy másodlagos webhely replikációs orchestrating.
- A Dynamics AX környezetekben a felhőbe (Azure) áttelepítését egyszerűsítése.
- Egyszerűsítése Dynamics AX alkalmazások fejlesztése, és egy gyártási hasonló másolás igény, tesztelése és hibakeresés létrehozásával vizsgálata.

[Tudjon meg többet](https://gallery.technet.microsoft.com/Dynamics-AX-DR-Solution-b2a76281) is megtudhat a dinamikus AX védelme.

## <a name="protect-rds"></a>Távoli asztali védelme

Távoli asztali szolgáltatásokat (a távoli asztali szolgáltatások) lehetővé teszi, hogy virtuális asztali infrastruktúra (VDI), a munkamenet-alapú asztalok és az alkalmazás, amely lehetővé teszi a felhasználóknak, hogy munka bárhonnan. A Azure webhely helyreállítási lehetőségeket nyújtja:

- Bizonyos kezelt vagy nem felügyelt készletezett virtuális asztali számítógépek egy másodlagos webhelyen, és a távoli alkalmazások és a másodlagos webhely vagy Azure-munkamenetek.
- Az alábbiakban mi, hogy bizonyos:

**TÁVOLI ASZTALI** | **A Hyper-V VMs bizonyos másodlagos webhelyre** | **A Hyper-V VMs bizonyos az Azure** | **Bizonyos VMware VMs másodlagos webhelyre** | **Azure való replikáció a VMware VMs** | **Fizikai kiszolgálók bizonyos másodlagos webhelyre** | **Azure párhuzamos fizikai kiszolgálók**
---|---|---|---|---|---|---
**(Nem felügyelt) készletezett virtuális asztali** | igen | nem | igen | nem | igen | nem
**Súlyozott virtuális asztali (felügyelt és UPD nélkül)** | igen | nem | igen | nem | igen | nem
**Távoli alkalmazások és az asztali munkamenetek (nélkül UPD)** | igen | igen | igen | igen | igen | igen


[Tudjon meg többet](https://gallery.technet.microsoft.com/Remote-Desktop-DR-Solution-bdf6ddcb) is megtudhat a RDS védelme


## <a name="protect-exchange"></a>Exchange védelme

Webhely helyreállításhoz szükséges adatok Exchange, az alábbi képlettel történik:

- Kis Exchange-telepítések egyetlen vagy különálló kiszolgálók, például a webhely helyreállítási bizonyos és Azure vagy másodlagos webhelyre átveszi.
- Nagyobb telepítésekhez webhely helyreállítási Exchange DAGS integrálódik.
- Exchange DAGs a vállalati Exchange vészhelyreállításáról javasolt megoldás.  Webhely helyreállítási helyreállítási csomagok DAGs való DAG feladatátvevő téve webhelyek közötti is tartalmazhat.


[Tudjon meg többet](https://gallery.technet.microsoft.com/Exchange-DR-Solution-using-11a7dcb6) is megtudhat a Exchange védelme.

## <a name="protect-sap"></a>SAP – védelme

Webhely helyreállítási segítségével védelme az SAP telepítéssel az alábbi képlettel történik:

- Védelme a teljes SAP-telepítést, engedélyezze a megjegyzésekről a különböző telepítési rétegek Azure, illetve egy másodlagos webhely úgy.
- Egyszerűbbé teszik a felhő áttelepítést, az SAP telepítési áttelepítése Azure webhely helyreállítási használatával.
- SAP fejlesztési egyszerűsítése, és egy gyártási hasonló létrehozásával másolása tesztelés, tesztelés és hibakeresés alkalmazásokat az igény.

[Tudjon meg többet](http://aka.ms/asr-sap) is megtudhat a SAP védelme.

## <a name="next-steps"></a>Következő lépések

[Felkészülés a webhely helyreállítási telepítéshez](site-recovery-best-practices.md) 
