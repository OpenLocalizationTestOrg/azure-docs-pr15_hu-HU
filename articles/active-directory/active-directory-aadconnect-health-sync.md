
<properties
    pageTitle="Azure Active Directory csatlakozás állapot használata szinkronizálási |} Microsoft Azure"
    description="Ez a az Azure Active Directory csatlakozás állapot lap, amely bemutatja, hogyan lehet Azure AD Connect szinkronizálási figyelése."
    services="active-directory"
    documentationCenter=""
    authors="karavar"
    manager="samueld"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/18/2016"
    ms.author="vakarand"/>

# <a name="using-azure-ad-connect-health-for-sync"></a>Azure Active Directory csatlakozás állapota szinkronizálás használata
A következő dokumentáció az Azure AD Connect (szinkronizálás) az Azure Active Directory csatlakozás rendszerállapot figyelése.  Témakörben [Használatával Azure Active Directory csatlakozás állapota az AD FS](active-directory-aadconnect-health-adfs.md)AD FS az Azure Active Directory csatlakozás rendszerállapot figyelése olvashat. Emellett a Active Directory tartományi szolgáltatások az Azure Active Directory csatlakozás állapot ellenőrzése című témakörből [Használatával Azure Active Directory csatlakozás állapota az Active Directory tartományi szolgáltatások](active-directory-aadconnect-health-adds.md).

![Azure AD Connect állapota szinkronizálás](./media/active-directory-aadconnect-health-sync/sync-blade.png)

## <a name="alerts-for-azure-ad-connect-health-for-sync"></a>Szinkronizálás az Azure Active Directory csatlakozás állapot vonatkozó értesítések
Az Azure Active Directory csatlakozás állapot értesítés szinkronizálás csoportban, a listát ad az aktív értesítések. Minden riasztás vonatkozó információkat, a megoldási lépéseinek és a kapcsolódó dokumentáció hivatkozásokat tartalmaz. Aktív vagy megoldott jelzést jelöljön ki egy további információk, valamint lépéssel sokat tehet a riasztás és a hivatkozások úgy, hogy további dokumentáció az új lap jelenik meg. Is megtekintheti figyelmeztetések, amelyek a múltbeli megoldott korábbi adatok.

További információk, valamint a lépéseket a megadott értesítés kiválasztásával készíthet a riasztás és a hivatkozások úgy, hogy dokumentációt.

![Azure AD Connect szinkronizálási hiba](./media/active-directory-aadconnect-health-sync/alert.png)

### <a name="limited-evaluation-of-alerts"></a>Értesítések korlátozott értékelése
Ha Azure AD Connect (például attribútum szűrés megfelelően módosul az alapértelmezett beállítások az egyéni konfigurálás) nem használja az alapértelmezett beállítások, majd az Azure Active Directory csatlakozás állapot ügynök nem feltöltődnek az Azure AD Connect kapcsolatos hiba eseményeket.

Ezzel a riasztások értékelése szolgáltatás korlátozza. Szeretné látni fogja a fejléc, ez a feltétel az Azure-portálon csoportban a szolgáltatás állapotát jelző.

![Azure AD Connect állapota szinkronizálás](./media/active-directory-aadconnect-health-sync/banner.png)

Módosíthatja a "Beállítások" gombra kattint, és lehetővé teszi az Azure Active Directory csatlakozás állapot ügynök összes hiba napló feltöltéséről.

![Azure AD Connect állapota szinkronizálás](./media/active-directory-aadconnect-health-sync/banner2.png)

## <a name="sync-insight"></a>Szinkronizálási betekintést
Rendszergazdák gyakran szeretne tudni az Azure Active Directory szinkronizáló módosításait, és módosításokat történő mennyiségét szükséges időt. Ez a funkció egyszerűvé ábrázolásához ez használata a diagramok alatt:   

- A szinkronizálási műveletek időtartama
- Objektum módosítása trend

### <a name="sync-latency"></a>Szinkronizálási időtartama
Ez a szolgáltatás, a szinkronizálási műveletek (importálás, exportálás stb.) az összekötőkhöz késés grafikus trend biztosít.  Ezzel a megoldással egy könnyen és gyorsan, nem csak a műveletek (ha vannak olyan egy nagy bekövetkezett változásokat nagyobb) késés megértéséhez, de rendellenességeinek feltárása a további vizsgálat megkövetelheti késleltetése lehetőséget.

![Szinkronizálási időtartama](./media/active-directory-aadconnect-health-sync/synclatency02.png)

Alapértelmezés szerint csak az "Exportálási" művelet az Azure Active Directory-összekötő a időtartam látható.  Az összekötő további műveletek megtekintéséhez, vagy az egyéb összekötők műveletek megtekintéséhez kattintson a jobb gombbal a diagramra, jelölje ki a diagram szerkesztése vagy kattintson a "Szerkesztés késés diagram" gombra, és válassza az adott művelet és összekötők.

### <a name="sync-object-changes"></a>Objektum szinkronizálja a módosításokat
Ez a szolgáltatás biztosít a módosításokat, amelyeket számát grafikus trend kiértékelt és Azure Active Directory exportált.  Ma ezek az információk összegyűjtése az a szinkronizálási naplók próbál akkor nehéz.  A diagram, nem csak egyszerűbb lehetőséget nyújt a számát a módosításokat, amelyeket a környezetben fordul elő, hanem a hibák történnek vizuális nézetének ellenőrzésére.

![Szinkronizálási időtartama](./media/active-directory-aadconnect-health-sync/syncobjectchanges02.png)

## <a name="object-level-synchronization-error-report-preview"></a>Objektum szintű szinkronizálási hibára vonatkozó jelentést (előzetes verzió)
Ez a szolgáltatás biztosít egy jelentést, akkor fordulhat elő, ha Windows Server Active Directory és Azure AD-Azure AD Connect segítségével szinkronizálja a személyes adatok szinkronizálási hibákat.

- A jelentés magában foglalja a szinkronizálási ügyfélprogramot által rögzített hibák (Azure AD Connect verzió 1.1.281.0 vagy újabb)
- Kattintson a szinkronizálás motor az utolsó szinkronizálás művelet a hibákról tartalmazza. ("Exportálása" az Azure Active Directory-összekötő.)
- Azure Active Directory csatlakozás állapot agent szinkronizálása a jelentést, és a legfrissebb adatok szükséges végpontjait kimenő kapcsolattal kell rendelkeznie. További információ a [vonatkozó követelményei című](active-directory-aadconnect-health-agent-install.md#Requirements) talál.
- A jelentés meg **frissített 30 percenként után** az adatok szinkronizálása az Azure Active Directory csatlakozás állapot ügynök által feltöltött használatával.
Az alábbi főbb funkciókat biztosít

    - Kategorizálás hibák
    - Objektumok hibával kategória szerinti listája
    - Egy adott helyen a hibáról az adatok
    - Által hiba miatt egy ütközést tartalmazó objektumok összehasonlítása egymás mellett
    - Töltse le a hibákról szóló jelentéseket a CVS (hamarosan) szerint

### <a name="categorization-of-errors"></a>Kategorizálás hibák
A jelentés kategorizálja meglévő a szinkronizálási hibákat, a következő kategóriákból:

| Kategória | Leírás |
| -------------- | ----------- |
| Ismétlődő attribútum | Azure AD Connect alkalommal, amikor hibák létrehozása vagy ismétlődő értékek egy vagy több attribútumok objektumok frissítse az Azure Active Directory, amely egy bérlői webhelyen, például a proxyAddresses, UserPrincipalName egyedinek kell lennie. |
| Adatok eltérés | Ha a finom hol.van megfelelően, amelyeknél a szinkronizálási hibákat objektumok nem sikerül hibákat. |
| Adatérvényesítési hiba | Hibák miatt érvénytelen adatot, például a kritikus attribútumok UserPrincipalName, például nem támogatott karakterek, amelyek az Azure Active Directory írt előtt ellenőrzése meghiúsul hibák formázhatja.|
| Nagy attribútum | Ha egy vagy több attribútumok nagyobb, mint a megengedett méretét, a hossz vagy a darabszám hibákat.|
| Más | Minden más hibákat, amely nem fér el a fenti kategóriák. Visszajelzései alapján ebbe a kategóriába sub kategóriák felosztása a program.

![Hiba jelentés összegzése szinkronizálása](./media/active-directory-aadconnect-health-sync/errorreport01.png)
![hiba jelentés kategóriák szinkronizálása](./media/active-directory-aadconnect-health-sync/errorreport02.png)

### <a name="list-of-objects-with-error-per-category"></a>Objektumok hibával kategória szerinti listája
Kimutatáshierarchiában való egyes kategóriákban a problémát a hiba kategóriákba tartozó objektumok listáját adja meg.
![Szinkronizálási hibák jelentés lista](./media/active-directory-aadconnect-health-sync/errorreport03.png)

### <a name="error-details"></a>Részletek
A következő adatok érhető el az egyes hibák részletes nézetben

- Az *Active Directory-objektum* érintett azonosítója
- Azonosítók az *Azure Active Directory-objektum* résztvevő (alkalmazható)
- Hiba leírását és javítás
- Kapcsolódó cikkek

![Szinkronizálási hibák jelentés részletei](./media/active-directory-aadconnect-health-sync/errorreport04.png)

### <a name="download-the-error-report-as-csv"></a>Töltse le a hibákról szóló jelentéseket a CSV-ként
Ez a képesség hamarosan. Figyelje az ezzel kapcsolatos további frissítéseket.



## <a name="related-links"></a>Kapcsolódó hivatkozások
* [A szinkronizálás során hibáinak elhárítása](active-directory-aadconnect-troubleshoot-sync-errors.md)
* [Ismétlődő attribútum Tűrőképessége](active-directory-aadconnectsyncservice-duplicate-attribute-resiliency.md)
* [Azure AD Connect állapota](active-directory-aadconnect-health.md)
* [Azure AD Connect állapot ügynök telepítése](active-directory-aadconnect-health-agent-install.md)
* [Azure AD Connect állapot műveletek](active-directory-aadconnect-health-operations.md)
* [Azure Active Directory segítségével kapcsolatba állapotjelző Active Directory összevonási szolgáltatások](active-directory-aadconnect-health-adfs.md)
* [Azure Active Directory segítségével kapcsolatba állapot Active Directory tartományi szolgáltatások](active-directory-aadconnect-health-adds.md)
* [Azure AD Connect állapotjelző – gyakori kérdések](active-directory-aadconnect-health-faq.md)
* [Azure AD Connect állapotjelző korábbi verziók](active-directory-aadconnect-health-version-history.md)
