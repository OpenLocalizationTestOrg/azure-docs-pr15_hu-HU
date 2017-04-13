<properties 
   pageTitle="A felhasználó kezelése a Microsoft-termékek figyelése |} Microsoft Azure"
   description="Értesítés azt jelzi, hogy bizonyos rendszergazdaként figyelmet igénylő probléma.  Ez a cikk ismerteti, hogyan értesítések létrehozása és kezelése a System Center műveletek Manager (SCOM) és a napló Analytics közötti különbségeket és gyakorlati tanácsok az feljebb helyezése a két termékek a hibrid riasztási kezelési stratégiát." 
   services="operations-management-suite"
   documentationCenter=""
   authors="bwren"
   manager="jwhit"
   editor="tysonn" />
<tags 
   ms.service="operations-management-suite"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/06/2016"
   ms.author="bwren" />

# <a name="managing-alerts-with-microsoft-monitoring"></a>A Microsoft figyelése értesítések kezelése 

Értesítés azt jelzi, hogy bizonyos rendszergazdaként figyelmet igénylő probléma.  Jelentős különbségek vannak System Center műveletek Manager (SCOM) és a napló Analytics közötti műveletek Management csomagja (MOBILE) hogyan jönnek létre az értesítések, hogyan azokat felügyelt és elemezheti és hogyan üzenetet kap arról, hogy a kritikus problémát észlelt.

## <a name="alerts-in-operations-manager"></a>Figyelmeztetés az Operations Managerhez
Figyelmeztetés SCOM az egyes szabályok vagy a monitor, hogy az egyes problémákra hozza létre.  Egy monitort hozhat létre értesítés, amikor azt hiba állapotba kerül, miközben egy szabályt, hogy néhány fontos probléma, amely nem kapcsolódik közvetlenül egy felügyelt objektum állapotát jelző értesítés parancs hozhatja létre.  Adatkezelési csomagok munkafolyamatok, az alkalmazás vagy szolgáltatás, hogy azok kezelése az értesítések létrehozása számos tartalmazzák.  A folyamat a egy új felügyeleti csomag része van finomhangolása biztosítható, hogy nem kapja meg, amelyet nem kritikus hibák fölösleges riasztásainak.

![SCOM riasztási megtekintése](media/operations-management-suite-monitoring-alerts/scom-alert-view.png)

SCOM teljes riasztási kezelési módon működnek-e a probléma megoldásához a rendszergazdák által módosított állapotú riasztások biztosít.  Ha a probléma megoldódott, a rendszergazda aktív értesítések megjelenítése nézetben állítja be a riasztást lezárt ekkor már nem jelenik meg.  A monitorok létrehozott figyelmeztetéseket a monitor visszaállítja a megfelelő állapotba automatikusan megoldható.

![Figyelmeztetés állapotának](media/operations-management-suite-monitoring-alerts/scom-alert-status.png)

## <a name="alerts-in-log-analytics"></a>Log Analytics riasztásai
Log Analytics értesítés rendszeres időközönként automatikusan futó napló lekérdezésből hoz létre.  Létrehozhat egy szabályt minden olyan napló lekérdezésből.  Ha a lekérdezés eredménye a megadott feltételeknek eleget tevő rekordjaira ad vissza, egy figyelmeztetés jön létre.  Ez lehet egy adott lekérdezése által létrehozott értesítés, ha egy adott esemény lép fel, vagy használhatja az általánosabb lekérdezés, amely egy adott alkalmazás kapcsolatos bármilyen hiba esemény keres.

Jelentkezzen be a riasztások eseményként MOBILE tárházba kerülnek, és tudja visszaszerezni Analytics-napló lekérdezés.  SCOM események jelennek állapota, hogy jelezheti, ha megoldódott a probléma nem rendelkező.

![MOBILE értesítés](media/operations-management-suite-monitoring-alerts/oms-alert.png)

SCOM napló elemzéséhez használja adatforrásként, amikor SCOM riasztások kerülnek a MOBILE tárat, létrehozása és módosítása.  

![SCOM értesítés](media/operations-management-suite-monitoring-alerts/scom-alert.png)

A [riasztási projektmenedzsment megoldás](http://technet.microsoft.com/library/mt484092.aspx) összefoglalja az aktív értesítések, és több általános lekérdezések beolvasásához különböző riasztásokat állítja be.  Ez biztosít a riasztások hatékonyabb elemzésének SCOM jelentés-nál.  Lehatolás a a összefoglalók a részletes adatokat, és alkalmi lekérdezések beolvasásához különböző értékkészletet, értesítések létrehozása.

![Riasztási projektmenedzsment megoldás](media/operations-management-suite-monitoring-alerts/alert-management.png)

## <a name="notifications"></a>Értesítések
Értesítés SCOM küldjön Önnek, a Posta vagy a szöveg adott feltételeknek eleget tevő rekordjaira riasztás.  Attól függően, hogy az adott feltételek – például az objektum figyelni, a figyelmeztető súlyosságát, az észlelt probléma típusú értesítés különböző többen vannak más értesítésekre vagy idejét hozhat létre.

Néhány előfizetések kezelése csomagok nagyszámú teljes értesítés stratégia végrehajtásához használható.

![SCOM értesítések](media/operations-management-suite-monitoring-alerts/alerts-overview-scom.png)

Log Analytics felhívhatja a Posta keresztül, hogy jelzést lett létrehozva beállítása e-mailben értesítést művelet [szabályt](http://technet.microsoft.com/library/mt614775.aspx).  Nincs SCOM lehetőségét egyetlen szabályok nagyszámú értesítés az előfizetés.  Is kell a saját riasztási szabályok létrehozása, mivel MOBILE bármelyik előre nem nyújt.

![Jelentkezzen be a Analytics-értesítések](media/operations-management-suite-monitoring-alerts/alerts-overview-oms.png)

Nem teljesen SCOM értesítések kezelése a napló Analytics azonban csak módosíthatja azokat a műveleteket konzolban óta.  Log Analytics akkor hasznos, mint egy figyelmeztető kezelés bár folyamat számára, hogy egyedül SCOM kezeléséről a elemzőeszközök nincs.

## <a name="alert-remediation"></a>Riasztási Remediation
Valaki megpróbálja automatikusan jelzést jelölt probléma megoldására [Remediation](http://technet.microsoft.com/library/mt614775.aspx) hivatkozik.
  
SCOM lehetővé teszi, hogy egy sérült állapotba monitor válaszul diagnosztika és helyreállítást futtatni.  Ez történik egyidejű a monitor létrehozása az értesítésre.  Diagnosztikai és helyreállítása általában végrehajtani, egy parancsfájlt, amelyet a Agent futtat.  Diagnosztikai kísérel meg további információt az észlelt probléma gyűjtése, miközben a helyreállítás megpróbálja a hiba kijavításához.

Log Analytics lehetővé teszi az [Azure automatizálási runbook](https://azure.microsoft.com/documentation/services/automation/) indítása, vagy felhívhatja őket egy webhook egy napló Analytics-értesítés válaszként.  Runbooks összetett logikájának végrehajtása a PowerShell is tartalmazhat.  A parancsfájl Azure fut, és érhető el az Azure erőforrások vagy a rendelkezésre álló külső források a felhőben.  Azure automatizálási runbooks végrehajtása a helyi adatközpontban kiszolgálón lehetősége van, de ez a funkció jelenleg nem áll rendelkezésre a runbook napló Analytics riasztás indításakor.

A SCOM helyreállítása, mind a MOBILE runbooks PowerShell-parancsfájlokat is tartalmazhat, de helyreállítása nehezebben lehet létrehozni és kezelni, mivel a felügyeleti csomag belül szerepelnie kell.  Azure automatizálási, amely szolgáltatásokat nyújt a szerzői, tesztelése és runbooks kezelése Runbooks tárolja.

Ha SCOM adatforrásként napló Analytics használ, úgy lehetett létrehozni egy napló Analytics-értesítés beolvasni a MOBILE tárban tárolnak SCOM riasztások napló lekérdezéssel.  Ez lehetővé teszi az Azure automatizálási runbook egy SCOM értesítésre válaszul futtatásához.  Természetesen a runbook futtatható az Azure-ban, mivel ez nem lehet helyreállítása a helyszíni problémák életképes stratégia.

## <a name="next-steps"></a>Következő lépések

- Ismerje meg [riasztások a System Center műveletek Manager (SCOM)](https://technet.microsoft.com/library/hh212913.aspx)részleteit.