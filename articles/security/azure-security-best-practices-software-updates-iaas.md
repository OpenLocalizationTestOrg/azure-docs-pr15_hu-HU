<properties
   pageTitle="Gyakorlati tanácsok a Microsoft Azure IaaS szoftverfrissítések |} Microsoft Azure"
   description="A cikkben gyakorlati tanácsok a Microsoft Azure IaaS környezetben szoftverfrissítések gyűjteménye.  Az informatikai szakemberek számára szolgál, és a biztonsági elemzők, aki kezelni az módosítása vezérlő, szoftver frissítése és eszköz-kezelés naponta, köztük szervezetük biztonság és megfelelőség erőfeszítéseket felelős."
   services="security"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor=""/>

<tags
   ms.service="security"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/18/2016"
   ms.author="yurid"/>

# <a name="best-practices-for-software-updates-on-microsoft-azure-iaas"></a>Gyakorlati tanácsok a szoftverfrissítések a Microsoft Azure IaaS

Mielőtt bármilyen típusú vitafórum be van gyakorlati tanácsok az Azure [IaaS](https://azure.microsoft.com/overview/what-is-iaas/) környezetben, fontos megérti, hogy az esetek, amely lesz, ügyvezető szoftverfrissítések és feladatait. Az alábbi ábrán segít megérteni a következő határai:

![Felhőalapú modellek és felelősségüknek](./media/azure-security-best-practices-software-updates-iaas/sec-cloudstack-new.png)

A bal szélső oszlopa (a szakaszok megadva), a hét feladatkörök jeleníti meg, hogy szervezetek kell figyelembe vennie, ami a számítógépes környezet biztonságáért és bővíthessék.
 
Adatok osztályozás és felelősség ellenőrzését és ügyfél és a végpontot védelem a feladatokra kizárólag ügyfelek tartományban vannak, illetve fizikai, Host és hálózati felelősségi köre felhő szolgáltatók a PaaS és a szoftver modellekben a tartományban. 

A hátralévő felelősségi köre vevők közötti megosztott és a felhő szolgáltatók. Néhány felelősségi köre az CSP és az ügyfél, a felelős együtt felügyeletéhez többek között a naplózás a tartományok és kezeléséhez szükséges. Például érdemes identitást & management elérni az Azure Active Directory-szolgáltatások; használata esetén a szolgáltatások, például többtényezős hitelesítés konfigurálása felfelé az ügyfél, de hatékony funkció biztosítása feladata a Microsoft Azure.

> [AZURE.NOTE] A felhőben megosztott felelősségi köre kapcsolatos további tudnivalókért olvassa el az [A felhő számítások megosztott felelősségi köre](https://gallery.technet.microsoft.com/Shared-Responsibilities-81d0ff91/file/153019/1/Shared%20responsibilities%20for%20cloud%20computing.pdf) 

Ezek az azonos elvek hibrid helyzetben, ahol vállalata használja Azure IaaS VMs kommunikálni az alábbi ábrán látható módon a helyszíni erőforrások vonatkoznak.

![Tipikus hibrid eset: a Microsoft Azure](./media/azure-security-best-practices-software-updates-iaas/sec-azconnectonpre.png)

## <a name="initial-assessment"></a>Kezdeti értékelése

Akkor is, ha a vállalat már használ egy frissítés rendszer, és már szoftver frissítés házirendek már a helyükön, fontos gyakran előző házirend felmérések látogatnia és frissíteni őket, az aktuális igényeknek megfelelően alakítható. Ez azt jelenti, hogy szeretne-e a vállalat a források aktuális állapotát ismernie kell a. Ez az állapot eléréséhez Ehhez ismerni kell:

-   A fizikai virtuális számítógépeken és a vállalat.

-   Operációs rendszerek és ezek fizikai és virtuális számítógépen futó verziók.

-   A szoftverfrissítések (service pack verziók szoftverfrissítések és más módosítások) minden számítógépen telepítve.

-   A függvény minden számítógépet a vállalat hajt végre.

-   Az alkalmazások és az összes olyan számítógépen futó programot.

-   Minden számítógépet tulajdonjogot és elérhetőségi adatok.

-   Az eszközök a környezet és a relatív értékük megállapíthatja, hogy mely területeken van szüksége a legtöbbet a figyelmet és védelme szerepel.

-   Biztonsági ismert problémák és a folyamatok a vállalati rendelkezik azonosító új biztonsági problémákról vagy a biztonsági szint módosításokat.

-   A környezet biztonságos telepített ellenintézkedések.

Ez az információ rendszeresen kell frissíteni, és könnyen hozzáférhető, hogy a szoftver frissítés kezelési folyamat részt kell lennie.

## <a name="establish-a-baseline"></a>Alapterv létrehozása

A szoftver frissítési management folyamat fontos része operációs rendszerek, az alkalmazások és a hardver számítógépek kezdeti szabványos telepítések hoz létre a vállalat; Ezek az úgynevezett alaptervek. Alapterv beállítás a termék vagy egy adott pontján időben létrehozott rendszer. Az adott alkalmazás vagy az operációs rendszer eredeti, például lehetővé teszi a számítógép vagy egy adott állapotának szolgáltatás újraépítéséhez.

Alapterv megkeresése és az esetleges problémák megoldása az alap szolgálni, és egyszerűbbé szoftver frissítés kezelése, telepítenie kell a vállalat szoftverfrissítések számát csökkentésével és az azt jelenti, hogy megfelelésének növelésével is.

A kezdeti naplózás a vállalat elvégzése, után a megadása az IT-összetevők, a termelési környezetén belül egy műveleti alapterv ellenőrzésről kapott adatokat kell használnia. Alapterv számos szükség lehet, attól függően, hogy a különböző típusú hardver- és éles üzembe.

Például az egyes kiszolgálók megakadályozhatja, hogy az oldalon függő beírásakor azokat a Leállítás folyamat fut a Windows Server 2012 szoftver frissítés szükséges. Az alábbi kiszolgálókon alapterv tartalmaznia kell a frissítés.

A nagyobb szervezetek célszerű gyakran a vállalati számítógépekre osztása eszköz kategóriák és kategórián őrizni szabványos alapterv ugyanazt a szoftverek és -szoftverfrissítések használatával. Használhatja a szoftver frissítés eloszlás rangsorolása eszköz kategóriákkal.

## <a name="subscribe-to-the-appropriate-software-update-notification-services"></a>Feliratkozás a megfelelő szoftver frissítés értesítési szolgáltatások

A vállalat használja végrehajtása a szoftver első átvilágításban, után meg kell határoznia módszer a legmegfelelőbb az egyes szoftver és verzió új szoftverfrissítések értesítésekkel. Attól függően, hogy a szoftver termék módszer a legmegfelelőbb értesítő e-mailek, a webhelyek vagy a számítógép kiadványok lehet.

A Microsoft biztonsági válasz központ (Kialakításában) például a Microsoft termékeihez kapcsolódó biztonsági kétségei válaszol, és a Microsoft biztonsági közlemény szolgáltatás, ezek biztonsági megoldására kiadott szoftverfrissítések és újonnan azonosított biztonsági ingyenes e-mail értesítést. Ez a szolgáltatás, a http://www.microsoft.com/technet/security/bulletin/notify.mspx előfizethet.

## <a name="software-update-considerations"></a>Szoftver frissítése előtt megfontolandó kérdések

Végrehajtotta a szoftver első átvilágításban használja a vállalat, meg kell határoznia a szoftver frissítés rendszer, amely függ, hogy a szoftver frissítés rendszer használni kívánt beállítása a követelményeknek. WSUS, olvassa el a [Gyakorlati tanácsok a Windows Server Update Services](https://technet.microsoft.com/library/Cc708536), az System Center olvassa el [a szoftverfrissítések Configuration Manager megtervezése](https://technet.microsoft.com/library/gg712696).

Vannak azonban olyan néhány általános szempontok és a gyakorlati tanácsok, függetlenül attól, ahogy azt a szakaszok használni kívánt következő megoldás alkalmazható.

### <a name="setting-up-the-environment"></a>A környezet beállítása

Vegye figyelembe az alábbi tanácsokat amikor tervezi, hogy a szoftver beállítása frissítése az adatkezelési környezet:

-   **Létrehozás gyártási szoftver frissítés gyűjtemények stabil feltételek alapján**: az általános stabil feltételek használatával hozhat létre a készlet a szoftver frissítése és a terjesztési gyűjtemények segítséget nyújt a szoftver frissítés management folyamat minden lépései egyszerűsítése érdekében. A stabil feltételt tartalmazhat, a telepített ügyfél operációs rendszer verziója és a service pack szint, a rendszer szerepkör vagy a cél szervezetben.

-   **Hivatkozás számítógépek tartalmazó létrehozása előtti gyártási gyűjtemények**: A gyártást megelőző gyűjtemény kell tartalmaznia az operációs rendszer verziója, üzleti szoftver sor és más vállalati futó szoftver képviseleti konfigurációk.

Ha a szoftver frissítés kiszolgáló kerül, ha, akkor az Azure IaaS infrastruktúrára a felhőben, vagy ha, akkor a helyszíni is figyelembe. Ennek oka egy fontos döntés van szüksége a forgalmat a helyszíni erőforrások és Azure infrastruktúra között mennyiségét ki szeretné számítani. További információt a helyszíni infrastruktúra keresztüli Azure olvassa el a [Csatlakozás Microsoft Azure virtuális hálózathoz egy helyszíni hálózaton](https://technet.microsoft.com/library/Dn786406.aspx) .

A látványtervezési lehetőségről, amely meghatározza, hogy a frissítés kiszolgáló kerül hol is változhatnak aszerint, hogy az aktuális infrastruktúra- és a szoftver frissítés rendszer, amely jelenleg használ. WSUS olvassa el a [Telepítése Windows Server Update Services szervezete](https://technet.microsoft.com/library/hh852340.aspx) , és a System Center Configuration Manager, olvassa el [a webhelyek és a hierarchiák Configuration Manager megtervezése](https://technet.microsoft.com/library/Gg712681.aspx).

### <a name="backup"></a>Biztonsági másolat

Rendszeres biztonsági mentés fontosak nem csak a szoftver frissítés kezelése platform maga is a kiszolgálókat, frissülnek a. Azoknak a szervezeteknek, már a helyükön vannak [kezelési folyamat módosítása](https://technet.microsoft.com/library/cc543216.aspx) esetén kell IT-sorkizártra okairól miért a kiszolgáló frissítésre szorul, a becsült legrövidebb leállás és a lehetséges hatása. Ahhoz, hogy a visszaállítás konfiguráció helyen abban az esetben, ha a frissítés sikertelen, feltétlenül rendszeresen biztonsági másolatot a rendszer.

Az Azure IaaS bizonyos biztonsági mentés beállításait a következők:

-   [Azure IaaS terhelést védelem adatok védelme segítségével](https://azure.microsoft.com/blog/2014/09/08/azure-iaas-workload-protection-using-data-protection-manager/)

-   [Készítsen biztonsági másolatot az Azure virtuális gépeken futó](../backup/backup-azure-vms.md)

### <a name="monitoring"></a>Figyelése

Működnie kell figyelni a számot a hiányzó normál jelentések vagy, és frissítések telepítése nem teljes állapotú, az egyes engedélyezett frissítést. Hasonlóképpen jelentéskészítési, amely még nem engedélyezett szoftverfrissítések a megkönnyítheti a könnyebb telepítési döntéseket.

Akkor is vegye figyelembe az alábbi műveleteket:

-   Használata az Office telepített és vonatkozó biztonsági frissítések, a vállalat összes számítógépre történő ellenőrzését.

-   Engedélyezhetik, és a frissítéseket a megfelelő számítógépre.

-   Nyomon követheti a készletet, és frissítse a telepítés állapotát és előrehaladását a vállalat összes számítógépén.

Ezenkívül általános dolgot voltak magyarázata ebben a cikkben is érdemes megfontolni egyes termékek adatait a legjobb – a gyakorlás, például: Ha az SQL Server Azure-ban van egy virtuális, győződjön meg arról, hogy a szoftver frissítések ajánlása az adott termék követik.

## <a name="next-steps"></a>Következő lépések

A jelen cikkben ismertetett profilképek használja, amely segít a legjobb beállítások virtuális gépeken futó Azure IaaS belül frissítéseket meghatározásában. Nincsenek sok hasonlóságokat szoftver frissítés gyakorlati tanácsok az Azure IaaS és a hagyományos adatközponthoz között, ezért azt ajánljuk, hogy ki-e a jelenlegi frissítés házirendek, Azure VMs tartalmazza, és a vonatkozó gyakorlati tanácsokat Ez a cikk a szerepeltetése az általános szoftver frissítési folyamat.
