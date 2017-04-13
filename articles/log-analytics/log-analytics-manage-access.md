<properties
    pageTitle="Log Analytics való hozzáférés kezelése |} Microsoft Azure"
    description="Log Analytics felügyeleti feladatok számos használatával a felhasználók, fiókok, MOBILE munkaterületek és Azure hozzáférés kezelése."
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
    ms.topic="get-started-article"
    ms.date="08/16/2016"
    ms.author="banders"/>

# <a name="manage-access-to-log-analytics"></a>Log Analytics való hozzáférés kezelése

Log Analytics való hozzáférés kezelése, kell megadnia a különböző felügyeleti feladatok felhasználók, a partnerek, a MOBILE munkaterületek és a Azure fiókok. Új munkaterület létrehozása a tevékenységek kezelése csomagja (MOBILE), válassza ki a munkaterület nevét társítása azt a fiókot, és úgy dönt, hogy a földrajzi hely. Munkaterület lényegében, amely tartalmazza a fiókadatokat és egyszerű konfigurációt adatait a fiókhoz tároló. Ön vagy a szervezet más tagjai előfordulhat, hogy több MOBILE-munkaterületek használatával kezelheti a különféle adathalmazok minden összegyűjtött vagy az informatikai infrastruktúrát részeire.

Az [első lépések a napló Analytics](log-analytics-get-started.md) cikk megtudhatja, hogy miként gyorsan és fut, és ez a cikk a többi némelyikéről részletesebben a MOBILE való hozzáférés kezelése kell műveletek.

Előfordulhat, hogy nem kell először minden felügyeleti feladatokat, bár a gyakran használt feladatokat, az alábbi szakaszok használhat fogja foglalkozunk:

- A munkaterületek szükséges számának meghatározása
- Partnerek és a felhasználók kezelése
- Csoport hozzáadása meglévő munkaterülethez
- Hivatkozás: Azure-előfizetésbe meglévő munkaterülethez
- Munkaterület frissítése a kifizetett mobilinternet-előfizetéssel
- Terv adattípus módosítása
- Azure Active Directory szervezeti hozzáadása meglévő munkaterülethez
- Zárja be a MOBILE munkaterülete

## <a name="determine-the-number-of-workspaces-you-need"></a>A munkaterületek szükséges számának meghatározása

Munkaterület-Azure erőforrás és hol adatainak gyűjtött, összesíti, elemezni, és a MOBILE portál foglalt tároló.

Lehetséges több MOBILE napló Analytics-munkaterületek létrehozása és a felhasználók hozzáférnek egy vagy több munkaterületek. Az általános szeretne munkaterületek számának csökkentése érdekében, mint ez lehetővé teszi a lekérdezés és a lehető legtöbb adatot keresztül összehangolására. Ez a szakasz ismerteti, hogy mikor lehet hasznos, ha egynél több munkaterület létrehozása.

Ma a naplófájl Analytics munkaterület nyújtja:

- Adattárolás egy földrajzi helyét.
- A számlázási Granularitás
- Adatok elkülönítési

A fenti jellemzők alapján, érdemes több munkaterületek létrehozása, ha:

- A globális vállalati és felségterületéhez vagy megfelelőségi okok miatt adott régióban tárolt adatokra van szüksége.
- Azure használja, és azt szeretné, hogy úgy, hogy a napló Analytics munkaterület ugyanabban a régióban kezeli Azure erőforrásként kimenő átadás költségek elkerülésére.
- Szeretne költségeket különböző részlegek vagy vállalati csoportok, a használatuk alapján. Az egyes részlege vagy üzleti csoport munkaterület létrehozásakor a Azure számlázási és használatát utasítás szemlélteti, az egyes munkaterületeken díjai külön-külön.
- Egy felügyelt szolgáltatót, és van szüksége a naplóadatokat analytics megtartása ügyfelek kezelése más ügyféladatok elkülönítve.
- Több ügyfél felügyeletét látja el, és minden egyes felhasználói vagy részleg vagy a vállalati csoportnak más vevők vagy részlegek vagy csoportok üzleti saját adataikat, de nem az adatok megtekintéséhez.

Amikor ügynökök adatokat szeretne gyűjteni, minden egyes ügynök el a szükséges munkaterület is beállíthatja.

System Center Operations manager használja, ha csak egy munkaterület minden Operations Manager-kezelés csoport kapcsolhat össze. Ügynököt a a Microsoft figyelése számítógépen futó Operations Manager kezeli, és a ügynök jelentés Operations Manager és a napló Analytics másik munkaterület van.

### <a name="workspace-information"></a>Munkaterület adatainak

A MOBILE portálon munkaterület adatainak megtekintése, és válassza ki, hogy szeretne információt kapni a Microsofttól.

#### <a name="view-workspace-information"></a>Munkaterület adatainak megtekintése

1. MOBILE kattintson a **Beállítások** csempére.
2. Kattintson a **fiókok** fülre.
3. Kattintson a **Munkaterület adatainak** fülre.  
  ![Munkaterület adatainak](./media/log-analytics-manage-access/workspace-information.png)

## <a name="manage-accounts-and-users"></a>Partnerek és a felhasználók kezelése

Egyes munkaterületeken is van társítva több felhasználói fiókot, és minden egyes felhasználói fiók (Microsoft-fiókkal vagy szervezeti fiókkal) is hozzáférhet több MOBILE munkaterületekre.

Alapértelmezés szerint a a Microsoft-fiókkal vagy szervezeti fiókkal, a munkaterület létrehozásához használt a munkaterület rendszergazdája lesz. A rendszergazda ezután meghívása további Microsoft-fiók, és válassza a felhasználók az Azure Active Directory.

A 2 helyek szabályozza hozzáférést biztosítva a személyek a MOBILE munkaterület:

- Azure, a szerepköralapú hozzáférés-vezérlés is használhatja az Azure előfizetést és a kapcsolódó Azure erőforrásokat hozzáférést biztosít. Is használja az PowerShell és a REST API-t.
- A MOBILE portál elérése csak a MOBILE portálra – a társított Azure előfizetést.

Hozzáférést ad a személyek a MOBILE-portálra, de nem az Azure előfizetés, amely van csatolva, ha kattintson az automatizálás, biztonsági mentése és webhely-helyreállítás megoldás csempék ne jelenjen meg semmilyen adatot felhasználók, amikor azok bejelentkezés a MOBILE portál.

Lásd: az adatok az alábbi lehetőségeket, győződjön meg róla, legalább rendelkeznek az összes felhasználójának engedélyezni a automatizálási partner, a biztonsági másolat tárolóból elemre és a webhely helyreállítási tárolóra a MOBILE munkaterület csatolt **olvasó** elérése.   

### <a name="managing-access-to-log-analytics-using-the-azure-portal"></a>Az Azure portálon napló Analytics való hozzáférés kezelése

Hozzáférést ad a felhasználóknak a napló Analytics-munkaterületre Azure engedélyek használata, ha az Azure-portálon például, majd a azonos felhasználói hozzáférhetnek a napló Analytics-portálon. Ha felhasználók az Azure-portálon, azok megkeresheti a MOBILE portál a **MOBILE portál** tevékenység kattint, a naplófájl Analytics munkaterület erőforrás megtekintésekor.

Néhány pont az Azure portál kapcsolatban szem előtt tartani:

- Ez nem *szerepköralapú hozzáférés-vezérlés*. Az Azure-portálon a napló Analytics munkaterület Ha *olvasó* hozzáférési engedélyek, majd módosíthatja a MOBILE portálon. A MOBILE portál rendszergazdai, a közös munka és az írásvédett felhasználói fogalma tartalmaz. Ha a fiók Ön jelentkezett be az Azure Active Directory, a munkaterület csatolva van a MOBILE portálon rendszergazda lesz, és egyébként munkatársi lesz.

- Amikor, bejelentkezés az MOBILE portálra http://mms.microsoft.com, majd a alapértelmezés szerint megjelenik a **Munkaterület kiválasztása** listában. Csak olyan tartalmaz, amelyeket felvettek a MOBILE portál használatával munkaterületek. A munkaterületek megtekintéséhez van hozzáférése az Azure előfizetések, majd meg kell adni a bérlő az URL-cím részeként. Példa:

  `mms.microsoft.com/?tenant=contoso.com`A bérlői azonosító része, gyakran utolsó az e-mail címet, akkor jelentkezzen be.

- Ha a fiók, jelentkezzen be a bérlői webhelyemen Azure Active Directory, amely általában az esetre hacsak egy CSP, a bejelentkezés, fiók majd fogja a *rendszergazda* a MOBILE portálon. Ha a fiók nem a bérlői webhelyemen Azure Active Directory, majd fogja a *felhasználó* a MOBILE portálon.

- Ha szeretne közvetlenül a portál, hogy van hozzáférése Azure engedélyek használatával keresse meg, majd meg kell adja meg az erőforráshoz az URL-cím részeként. Ez az URL PowerShell get lehetőség.

  Ha például `(Get-AzureRmOperationalInsightsWorkspace).PortalUrl`.

  Az URL-cím fog kinézni:`https://eus.mms.microsoft.com/?tenant=contoso.com&resource=%2fsubscriptions%2faaa5159e-dcf6-890a-a702-2d2fee51c102%2fresourcegroups%2fdb-resgroup%2fproviders%2fmicrosoft.operationalinsights%2fworkspaces%2fmydemo12`


### <a name="managing-users-in-the-oms-portal"></a>A MOBILE portálon-felhasználók kezelése

Felhasználók és a **Felhasználók kezelése** lapon a **fiókok** lapon a beállítások lapon a csoport kezelése Itt a tevékenységeket hajthatják végre az alábbi szakaszok.  

![felhasználókezelés](./media/log-analytics-manage-access/setup-workspace-manage-users.png)

#### <a name="add-a-user-to-an-existing-workspace"></a>Felhasználó felvétele a meglévő munkaterülethez

Felhasználó vagy csoport hozzáadása egy MOBILE munkaterület kövesse az alábbi lépéseket. A felhasználó vagy csoport lesz tekintheti meg és nem minden riasztást, a munkaterület társított működésbe lépnek.

>[AZURE.NOTE] Ha szeretne egy felhasználó vagy csoport hozzáadása az Azure Active Directory-ös szervezeti fiókjával, először győződjön meg arról, hogy az Active Directory-tartománya MOBILE-fiókjához tartozó. Lásd: [Azure Active Directory szervezeti meglévő munkaterülethez](#add-an-azure-active-directory-organization-to-an-existing-workspace).

1. MOBILE kattintson a **Beállítások** csempére.
2. Kattintson a **fiókok** fülre, és válassza a **Felhasználók kezelése** lapon.
3. A **Felhasználók kezelése** csoportban válassza ki a fiók hozzáadása: **Szervezeti fiókkal**, a **Microsoft-fiókkal**, **A Microsoft támogatási**.
    - Ha úgy dönt, hogy a Microsoft-Account, írja be annak a felhasználónak a Microsoft Account társított e-mail címét.
    - Ha úgy dönt, hogy a szervezeti fiókkal, adhatja meg a felhasználó vagy csoport nevét vagy e-mail aliasát részét, és megjelenik a felhasználók és csoportok listája. Jelölje ki a felhasználót vagy csoportot.
    - Microsoft Support segítségével egy Microsoft Support ad vissza, ideiglenes access hibaelhárítási segítség a munkaterületre.

    >[AZURE.NOTE] A legjobb teljesítményt, az Active Directory-csoportok egy egyetlen MOBILE-fiókkal három társított számának korlátozása – egy rendszergazdái, munkatársak egyet, és egy írásvédett felhasználók számára. További csoportok használatával is hatással lehet a napló Analytics működésére.

5. Válassza ki a felhasználó vagy csoport hozzáadása típusát: **rendszergazda**, a **közös munka**vagy a **Felhasználó csak olvasható** .  
6. Kattintson a **hozzáadása**gombra.

  Ha egy Microsoft-fiókot hozzáadni, csatlakozás a munkaterületre szóló meghívás az e-mailt a megadott küldi. A felhasználó követi a meghívót, ha be szeretne kapcsolódni MOBILE utasításait, a felhasználó megtekintheti a riasztások és a fiókadatok MOBILE ehhez a fiókhoz, és fogja tudni megtekinteni a felhasználói adatok az **ügyfelek** lapon a **Beállítások** lap.
  Szervezeti fiók hozzáadásakor a felhasználók Log Analytics azonnal hozzáférhet lesz.  
  ![Felkérés e-mailben](./media/log-analytics-manage-access/setup-workspace-invitation-email.png)

#### <a name="edit-an-existing-user-type"></a>Egy meglévő felhasználó típusa szerkesztése

A fiók szerepkör a MOBILE-fiókjához tartozó felhasználó módosíthatja. A következő szerepkört lehetőség közül választhat:

 - *Rendszergazda*: is felhasználók kezelése, megtekintése és működésbe lépnek minden riasztást, és hozzáadása és eltávolítása kiszolgálók

 - *Közös munka*: is megtekintheti és működésbe lépnek minden riasztást, és hozzáadása és eltávolítása kiszolgálók

 - *Csak olvasható felhasználói*: a felhasználók írásvédettként megjelölt nem fogja tudni:
   1. Megoldások hozzáadása/eltávolítása. A megoldástár el van rejtve.
   2. **Saját irányítópult**csempék hozzáadása és módosítása vagy eltávolítása.
   3. A **Beállítások** lapok megtekintéséhez. A lapok el vannak rejtve.
   4. A Keresés nézetben, a PowerBI konfigurációja, a mentett és a riasztások feladatok el vannak rejtve.


#### <a name="to-edit-an-account"></a>A fiók szerkesztése

1. MOBILE kattintson a **Beállítások** csempére.
2. Kattintson a **fiókok** fülre, és válassza a **Felhasználók kezelése** lapon.
3. Jelölje ki a felhasználót, hogy meg szeretné változtatni a szerepkörnek.
2. A megerősítést kérő párbeszédpanelen kattintson az **Igen**gombra.

### <a name="remove-a-user-from-a-oms-workspace"></a>Felhasználó törlése a MOBILE munkaterület

A felhasználó eltávolítása egy MOBILE munkaterület kövesse az alábbi lépéseket. Megjegyzés: Ez nem zárja be a felhasználó munkaterület. Ehelyett a felhasználó számára, és a munkaterület közötti társítás eltávolítja. Ha egy felhasználó több munkaterületek társítva, a felhasználó számára alkalmazásait továbbra is használhatja, jelentkezzen be a MOBILE és a többi munkaterület.

1. MOBILE kattintson a **Beállítások** csempére.
2. Kattintson a **fiókok** fülre, és válassza a **Felhasználók kezelése** lapon.
3. A felhasználó nevét, amely az eltávolítani kívánt mellett kattintson az **Eltávolítás** gombra.
4. A megerősítést kérő párbeszédpanelen kattintson az **Igen**gombra.


### <a name="add-a-group-to-an-existing-workspace"></a>Csoport hozzáadása meglévő munkaterülethez

1.  Kövesse a lépéseket a "A felhasználó hozzáadása meglévő munkaterülethez" 1 -4, a fenti.
2.  **Válassza a felhasználó vagy csoport**csoportban jelölje ki a **csoportot**.
    ![csoport hozzáadása meglévő munkaterülethez](./media/log-analytics-manage-access/add-group.png)
3.  Írja be a megjelenítendő neve vagy E-mail címét a csoportot, amelyhez hozzá szeretné adni.
4.  Jelölje ki a csoportot a listában a találatok között, és kattintson a **Hozzáadás**gombra.

## <a name="link-an-existing-workspace-to-an-azure-subscription"></a>Hivatkozás: Azure-előfizetésbe meglévő munkaterülethez

Ajánlatos munkaterület létrehozása a [microsoft.com/oms](https://microsoft.com/oms) webhelyről.  Jó helyen jár bizonyos korlátozások léteznek ezeken a munkaterületeken legfeljebb 500MB/adatok feltöltések napját jelentő, ingyenes-fiók használata a legtöbb lényeges. Ha módosítani szeretné a munkaterület szüksége lesz fűznie *a meglévő munkaterület Azure-előfizetésbe*.

>[AZURE.IMPORTANT] Annak érdekében, hogy a munkaterület hivatkozás, az Azure-fiók már hozzáféréssel kell rendelkeznie a csatolni kívánt munkaterületre.  Ez azt jelenti, a fióknak az Azure portál hozzáféréssel kell lennie **azonos** hozzáféréssel a MOBILE munkaterület fiókként. Ha ez nem látható a [meglévő munkaterülethez felhasználó hozzáadása](#add-a-user-to-an-existing-workspace).

### <a name="to-link-a-workspace-to-an-azure-subscription-in-the-oms-portal"></a>Munkaterület hivatkozni szeretne egy Azure-előfizetést a MOBILE portálon

Annak érdekében, hogy a munkaterület csatolása az Azure előfizetéssel a MOBILE portálon, a bejelentkezett felhasználó már fizetett Azure fiókkal kell rendelkeznie. A munkaterület, amelyet aktívan használ az Azure-fiók kap csatolva.

1. MOBILE kattintson a **Beállítások** csempére.
2. Kattintson a **fiókok** fülre, és válassza a az **Azure-előfizetés és az adatok megtervezése** lapot.
3. Kattintson a mobilinternet-előfizetéssel, amellyel meg szeretné.
4. Kattintson a **Mentés**gombra.  
  ![adatok és előfizetési csomagok](./media/log-analytics-manage-access/subscription-tab.png)

Az új mobilinternet-előfizetéssel a MOBILE portál menüszalag, a weblap tetején jelenik meg.

![MOBILE menüszalagján](./media/log-analytics-manage-access/data-plan-changed.png)

### <a name="to-link-a-workspace-to-an-azure-subscription-in-the-azure-portal"></a>Munkaterület hivatkozni szeretne egy Azure-előfizetést az Azure-portálon

1.  Jelentkezzen be az [Azure-portálon](http://portal.azure.com).
2.  Tallózás a **Napló Analytics (MOBILE)** , és kattintson rá.
3.  Láthatja, hogy a meglévő munkaterületek listájában. Kattintson a **hozzáadása**gombra.  
    ![munkaterületek listája](./media/log-analytics-manage-access/manage-access-link-azure01.png)
4.  A **MOBILE munkaterület**kattintson a **hivatkozás: meglévő vagy**.  
    ![meglévő hivatkozás](./media/log-analytics-manage-access/manage-access-link-azure02.png)
5.  Kattintson a **konfigurálás szükséges beállításai**.  
    ![szükséges beállításainak konfigurálása](./media/log-analytics-manage-access/manage-access-link-azure03.png)
6.  Láthatja, hogy a munkaterületek, amely még nem kapcsolódó az Azure-fiók listáját. Jelölje ki a munkaterület.  
    ![Jelölje ki a munkaterületek](./media/log-analytics-manage-access/manage-access-link-azure04.png)
7.  Ha szükséges, módosíthatja az értékeket a következő elemek:
    - Előfizetés
    - Erőforráscsoport
    - Hely
    - Réteg árak  
        ![értékek módosítása](./media/log-analytics-manage-access/manage-access-link-azure05.png)
8.  Kattintson a **létrehozása**gombra. A munkaterület a program csatolja az Azure-fiókjába.

>[AZURE.NOTE] Ha nem látható a munkaterület hivatkozni szeretne, majd az Azure előfizetés nincs hozzáférése a MOBILE munkaterület a MOBILE webhely használatával létrehozott.  Szüksége lesz, ha szeretne engedélyt adni a fiókot, a belül a MOBILE munkaterület a MOBILE webhelyet használ. Erre, olvassa el a [meglévő munkaterülethez felhasználó hozzáadása](#add-a-user-to-an-existing-workspace)című témakört.



## <a name="upgrade-a-workspace-to-a-paid-data-plan"></a>Munkaterület frissítése a kifizetett mobilinternet-előfizetéssel

Vannak olyan három Groove-munkaterület adatainak típusok megtervezése a MOBILE: **szabad**, **normál**és **Premium**.  Ha egy *ingyenes* csomag, előfordulhat, hogy van gombra kattint az adatok vonalvég 500 MB-os.  Meg kell frissíteni a munkaterület ***befizetések terv*** annak érdekében, hogy a határértéket meghaladó adatgyűjtés. Bármikor alakíthatja a terv típusát.  További információt a MOBILE árak az [Ár részletei](https://www.microsoft.com/en-us/server-cloud/operations-management-suite/pricing.aspx)című részben találja.

>[AZURE.IMPORTANT] Munkaterület-csomagok csak módosíthatók, ha *csatolt* Azure-előfizetésbe.  A munkaterület *már* csatolva, ez az üzenet figyelmen kívül hagyhatja a munkaterület Azure-ban létrehozott, vagy, ha már.  A munkaterület a [MOBILE webhelyen](http://www.microsoft.com/oms)hozta létre, ha szüksége lesz a lépések hivatkozásra [egy meglévő munkaterülethez Azure-előfizetésbe](#link-an-existing-workspace-to-an-azure-subscription).

### <a name="using-entitlements-from-the-oms-add-on-for-system-center"></a>A MOBILE bővítmény a jogosultságok System Center használata

A MOBILE bővítmény System Center a MOBILE napló Analitikát, a [MOBILE árak](https://www.microsoft.com/en-us/server-cloud/operations-management-suite/pricing.aspx)leírt Premium csomagra vonatkozó jogosultságot tartalmaz.

A MOBILE bővítmény System Center vásárlásakor a MOBILE bővítmény egy jogosultság a System Center szerződés is összegként. Bármely Azure előfizetés, a jelen szerződés létrehozott fel, hogy a jogosultság használja. Ez teszi lehetővé, például, hogy a jogosultság a MOBILE bővítmény a használó több MOBILE-munkaterületek.

Annak érdekében, hogy egy MOBILE munkaterület használatát van hozzárendelve a jogosultságok a MOBILE bővítményt, kell:

1. Hivatkozás a MOBILE munkaterület Azure-előfizetésbe, amely az Enterprise Agreement a MOBILE bővítmény vásárlás és az Azure előfizetés használatának tartalmazó része
2. Jelölje ki a munkaterület prémium megtervezése

Érdemes tanulmányozni a használatát az Azure vagy MOBILE portálon, ha nem látható a MOBILE bővítmény jogosultságok. A vállalati portál jogosultságok is megjelenik.  

Ha módosítania kell az Azure előfizetés a MOBILE munkaterület csatolt, az [Áthelyezés-AzureRmResource](https://msdn.microsoft.com/library/mt652516.aspx) Azure PowerShell-parancsmag is használhatja.

### <a name="using-azure-commitment-from-an-enterprise-agreement"></a>Egy Enterprise Agreement az Azure lekötési használatával

Ha úgy dönt, hogy használja a különálló árak MOBILE-összetevők, kell fizetnie mindegyik MOBILE-összetevő külön-külön, és a használatát az Azure számlán fognak szerepelni.

A vállalati tagság, amelyhez az Azure előfizetések kapcsolódnak a Ha egy pénzbeli Azure jóváhagyás, napló Analytics bármely használatát fog automatikusan tartozik bármilyen fennmaradó pénzbeli jóváhagyás szemben.

Ha módosítani szeretné, hogy a MOBILE munkaterület van csatolva, az Azure előfizetés használhatja az [Áthelyezés-AzureRmResource](https://msdn.microsoft.com/library/mt652516.aspx) Azure PowerShell-parancsmag.  



### <a name="to-change-a-workspace-to-a-paid-data-plan"></a>Munkaterület átállítása fizetett mobilinternet-előfizetéssel

1.  Jelentkezzen be az [Azure-portálon](http://portal.azure.com).
2.  Tallózás a **Napló Analytics (MOBILE)** , és kattintson rá.
3.  Láthatja, hogy a meglévő munkaterületek listájában. Jelölje ki a munkaterület.  
    ![munkaterületek listája](./media/log-analytics-manage-access/manage-access-change-plan01.png)
4.  A **Beállítások**csoportban kattintson a **árak réteg**.  
    ![réteg árak](./media/log-analytics-manage-access/manage-access-change-plan02.png)
5.  A **réteg árak**jelölje be a mobilinternet-előfizetéssel, és kattintson a **Jelölje ki**.  
    ![Jelölje ki a terv](./media/log-analytics-manage-access/manage-access-change-plan03.png)
6.  Amikor frissíti a nézetet az Azure-portálon, látni fogja a **árak réteg** frissíti a kijelölt terv.  
    ![réteg árak frissítése](./media/log-analytics-manage-access/manage-access-change-plan04.png)

Most már összegyűjtheti adatok túl a "ingyenesen" kap.


## <a name="add-an-azure-active-directory-organization-to-an-existing-workspace"></a>Azure Active Directory szervezeti hozzáadása meglévő munkaterülethez

A napló Analytics (MOBILE) munkaterület társíthatja az Azure Active Directory tartományi. Lehetővé teszi, hogy a felhasználók hozzáadása az Active Directoryból közvetlenül a MOBILE munkaterület anélkül, hogy külön Microsoft-fiókkal.

Munkaterület létrehozása az Azure portálról, vagy hivatkozás a munkaterület Azure-előfizetésbe az Azure Active Directory, a szervezeti fiók csatolható.

A munkaterület létrehozásakor a MOBILE portálról a rendszer kéri egy Azure-előfizetést, és egy szervezeti fiók mutató hivatkozás.

### <a name="to-add-an-azure-active-directory-organization-to-an-existing-workspace"></a>Azure Active Directory szervezeti hozzáadása meglévő munkaterülethez

1. MOBILE beállítások lapon kattintson a **fiókok** elemre, és válassza a a **Munkaterület** fülre.  
2. Tekintse át a szervezeti fiókok, és kattintson a **Szervezet hozzáadása**gombra.  
    ![szervezeti hozzáadása](./media/log-analytics-manage-access/manage-access-add-adorg01.png)
3. Adja meg a személyes adatait a rendszergazda az Azure Active Directory-tartomány. A későbbiekben talál arról, hogy a munkaterület Azure Active Directory-tartománya van csatolva csoporttagoktól.
    ![csatolt munkaterület nyugta](./media/log-analytics-manage-access/manage-access-add-adorg02.png)

>[AZURE.NOTE] A fiók szervezeti fiókkal van csatolva, miután csatolása nem eltávolítható vagy megváltozott.

## <a name="close-your-oms-workspace"></a>Zárja be a MOBILE munkaterülete

Egy MOBILE munkaterület bezárásakor a munkaterület kapcsolódó összes adat is törlődik a a MOBILE szolgáltatás bezárása a munkaterület 30 napon belül.

Ha Ön rendszergazda, és néhány a munkaterület társított több felhasználó, ezek a felhasználók és a munkaterület közötti társítás megszakad. Ha a felhasználók más munkaterületek társítva, majd továbbra is azokat más munkaterületek a MOBILE használja. Jó helyen jár Ha még nem társított más munkaterületek majd azok kell MOBILE használata új munkaterület létrehozása.

### <a name="to-close-an-oms-workspace"></a>Egy MOBILE munkaterület bezárása

1. MOBILE kattintson a **Beállítások** csempére.
2. Kattintson a **fiókok** fülre, és válassza a a **Munkaterület** fülre.
3. Kattintson a **Bezárás munkaterület**.
4. Jelölje ki a munkaterület lezárás okait közül, vagy a szövegmezőben adja meg, hogy más.
5. Kattintson a **Bezárás munkaterület**.

## <a name="next-steps"></a>Következő lépések

- [Log Analytics csatlakoztatása a Windows számítógépek](log-analytics-windows-agents.md) ügynökök hozzáadása és a szükséges adatok összegyűjtése című részben.
- [Log Analytics hozzáadása megoldások a megoldástárból](log-analytics-add-solutions.md) funkciókat és a szükséges adatok összegyűjtése.
- [A proxykiszolgáló és a tűzfal beállításainak napló Analytics](log-analytics-proxy-firewall.md) Ha szervezete használja a proxykiszolgáló vagy a tűzfalat, hogy ügynökök kommunikálni tudjanak a napló Analytics szolgáltatás.
