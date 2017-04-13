<properties
    pageTitle="Azure AD Connect szinkronizálása: hogyan lehet módosítani az alapértelmezett beállítások |} Microsoft Azure"
    description="Az alábbiakban, hogyan lehet módosítani a konfiguráció Azure AD Connect szinkronizálás."
    services="active-directory"
    documentationCenter=""
    authors="andkjell"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/31/2016"
    ms.author="billmath"/>


# <a name="azure-ad-connect-sync-how-to-make-a-change-to-the-default-configuration"></a>Azure AD Connect szinkronizálási: hogyan lehet módosítani az alapértelmezett beállítás
Ez a témakör célja ismerteti, hogy hogyan módosíthatja az alapértelmezett konfiguráció Azure AD Connect szinkronizálás. Néhány gyakori alkalmazási területek lépéseket biztosít. Ez a ismeretekkel kell néhány egyszerű módosíthassanak a saját konfigurációt saját üzleti szabályok alapján.

## <a name="synchronization-rules-editor"></a>Szinkronizálás szabályokat szerkesztő
A szinkronizálás szabályokat szerkesztő és ki módosíthatja az alapértelmezett beállítás szolgál. Megtalálja a Start menüben a **Azure AD Connect** csoportban.  
![Start menü szinkronizálási szabály szerkesztő](./media/active-directory-aadconnectsync-change-the-configuration/startmenu2.png)

Amikor megnyit, az alapértelmezett out kész szabályokat látható.

![Szinkronizálási szabály szerkesztő](./media/active-directory-aadconnectsync-change-the-configuration/sre2.png)

### <a name="navigating-in-the-editor"></a>Navigálás a szerkesztőben
Legördülő menük a szerkesztő tetején gyorsan megtalálhatja az adott szabály teszi lehetővé. Szeretne látni a szabályokat, ha az attribútum proxyAddresses megtalálható, lenne a következő például módosítása a legördülő menük:  
![SRE szűrése](./media/active-directory-aadconnectsync-change-the-configuration/filtering.png)  
Szűrés alaphelyzetbe, és töltse be a friss konfiguráció, nyomja le az **F5** billentyűkombinációt.

A jobb felső sarokban meg kell a **Hozzáadás új szabály**gombra. Ez a gomb segítségével saját egyéni szabály létrehozása.

A képernyő alján eljáró gombbal egy kijelölt szinkronizálási szabály van. **Szerkesztése** és **törlése** a várt módon működnek őket. Egy PowerShell-parancsprogramot újbóli létrehozására a szinkronizálási szabály **exportálása** hoz létre. Ez az eljárás váltani szinkronizálási szabály egy kiszolgáló teszi lehetővé.

## <a name="create-your-first-custom-rule"></a>Az első egyéni szabály létrehozása
A leggyakoribb módosítása a attribútum flow módosítások. Az adatok forrása címtárában nem feltétlenül ahogy Azure AD. Ez a szakasz a példában szereplő kívánt ellenőrizze, hogy a megadott névvel, hogy egy felhasználó mindig **kezdőbetűvel**jelenik meg.

### <a name="disable-the-scheduler"></a>Le az ütemezőt
Az [Ütemező](active-directory-aadconnectsync-feature-scheduler.md) 30 percenként alapértelmezés szerint fut. Győződjön meg arról, hogy nem indul, miközben a módosításokat, és az új szabályok – problémamegoldás szeretne. Ideiglenesen tiltsa le az ütemezőt, indítsa el a Powershellt, és futtatása`Set-ADSyncScheduler -SyncCycleEnabled $false`

![Le az ütemezőt](./media/active-directory-aadconnectsync-change-the-configuration/schedulerdisable.png)  

### <a name="create-the-rule"></a>A szabály létrehozása

1. **Hozzáadás új szabály**gombra.
2. A **Leírás** lapon adja meg az alábbiakat:  
![Bejövő szabály szűrése](./media/active-directory-aadconnectsync-change-the-configuration/description2.png)  
    - Név: Adjon a szabály egy jól megjegyezhető nevet.
    - Leírás: Bizonyos tisztázására, hogy valaki más érthető Mi az a szabályt.
    - Csatlakozik a rendszer: A program az objektumot a rendszer. Ebben az esetben akkor jelölje ki az Active Directory-összekötő.
    - Csatlakoztatott rendszer/Metaverse objektumtípus: Válassza a **felhasználók** és **személy** rendre.
    - Hivatkozás típusa: Ez az érték módosítása **csatlakozni**szeretne.
    - Végrehajtási sorrendje: Adjon meg egy értéket, a rendszer egyedi. Alsó numerikus érték azt jelzi, hogy újabb elsőbbségi.
    - Címke: Hagyja üresen. Csak olyan Microsoft out kész szabályokat kell érték szerepel ebben a mezőben.
3. A **Scoping szűrő** lapon adja meg a **givenName ISNOTNULL**.  
![Bejövő szabály szűrő hatókör meghatározása](./media/active-directory-aadconnectsync-change-the-configuration/scopingfilter.png)  
Ebben a szakaszban határozza meg, mely objektumai a szabály alkalmazása kell szolgál. Ha üres, a szabályt alkalmazni szeretné az összes felhasználó objektumok. De, amely magában foglalja a üléstermek, szolgáltatás fiókok és más nem személyek felhasználói objektumokat.
4. Kattintson a **Bekapcsolódás szabályokat**hagyja üresen.
5. **Átalakítások** lapján módosítsa a FlowType **kifejezés**. Jelölje be a cél attribútum **givenName**, és írja be a forrás `PCase([givenName])`.
![A bejövő szabályok átalakítások](./media/active-directory-aadconnectsync-change-the-configuration/transformations.png)  
A sync engine a kis-és nagybetűket a függvény neve és a attribútum neve. Ha valami hiba történt, figyelmeztetést a szabály hozzáadásakor. A szerkesztő lehetővé teszi, hogy mentse, és továbbra is, így azt szeretné, hogy megnyitja a szabályt, és javítsa ki a szabály.
6. Kattintson a **Hozzáadás gombra** kattintva mentheti a szabályt.

A rendszer az új egyéni szabály kell a szinkronizálási szabályoknak látható.

### <a name="verify-the-change"></a>Ellenőrizze a módosítása
A új módosítás szeretne ellenőrizze, hogy a várt módon működik, és nem az értesítő a hibák. Attól függően, hogy rendelkezik objektumok számát kétféleképpen különböző lépés végrehajtásához.

1. A teljes szinkronizálást futtassa az összes objektum
2. Egy kép és a teljes szinkronizálás futtatása egyetlen objektum

Indítsa el a **Szinkronizálási szolgáltatás** a start menüből. Ez a szakasz lépéseit az összes az eszközben nem.

1. **Az összes objektum teljes szinkronizálás**  
Jelölje ki az **összekötők** tetején. Azonosítsa az összekötő megváltoztatta a az előző szakaszban, ebben az esetben az Active Directory tartományi szolgáltatások, és jelölje ki. Válassza a **Futtatás** a műveleteket, és válassza ki a **Teljes szinkronizálást** és **az OK gombra**.
![Teljes szinkronizálása](./media/active-directory-aadconnectsync-change-the-configuration/fullsync.png)  
Az objektumok ekkor frissülnek a metaverse. Most szeretne tekintse meg a metaverse az objektumot.

2. **A körlevél megtekintése és teljes szinkronizálás egy objektumra**  
Jelölje ki az **összekötők** tetején. Azonosítsa az összekötő megváltoztatta a az előző szakaszban, ebben az esetben az Active Directory tartományi szolgáltatások, és jelölje ki. Jelölje ki a **keresési összekötő területet**. Hatókör segítségével megkeresheti a tesztelje a változás használni kívánt objektum. Jelölje ki az objektumot, és kattintson az **Előnézet**gombra. Az új képernyőn válassza a **Véglegesítése előzetes**.
![Kiválasztási előzetes verzió](./media/active-directory-aadconnectsync-change-the-configuration/commitpreview.png)  
A Módosítás gombra a metaverse elkötelezte magát.

**Tekintse meg az objektumot a metaverse**  
Most szeretne adja meg, hogy az érték várhatóan néhány példa objektumok és, hogy a szabályt alkalmazza. Jelölje ki a felső **Metaverz keresés** . Minden szűrő szeretne megtalálni lényeges objektumok hozzáadása. Nyissa meg a találatok között egy objektumot. Tekintse meg az attribútum értékek és a **Szinkronizálási szabályokat** oszlopban, amely a szabályt alkalmazza a várt módon is ellenőrizheti.  
![Metaverz keresés](./media/active-directory-aadconnectsync-change-the-configuration/mvsearch.png)  
### <a name="enable-the-scheduler"></a>Az ütemező engedélyezése
Ha minden várt módon működik, újra engedélyezheti az ütemezőt. A PowerShell, futtassa a `Set-ADSyncScheduler -SyncCycleEnabled $true`.

## <a name="other-common-attribute-flow-changes"></a>Egyéb gyakori attribútum folyamat változások
Az előző szakaszban ismertetett hogyan módosíthatja a egy attribútum folyamat. Ebben a részben néhány további példa állnak rendelkezésre. A szinkronizálási szabály létrehozásához lépéseit rövidítve van, de a teljes lépéseket az előző szakaszban találhat.

### <a name="use-another-attribute-than-the-default"></a>Az alapértelmezett egy másik attribútum használata
A Fabrikam van egy erdő, ha a helyi ábécé használatos Utónév Vezetéknév és megjelenítendő név. A következő attribútumok Latin karakter ábrázolása a bővítmény attribútumok találhatók. A globális címlistában készítésekor az Azure Active Directory és az Office 365-be, a szervezet csevegni következő attribútumok helyett használható.

Az alapértelmezett beállításokkal a helyi erdőből objektum néz ki:  
![A folyamat 1 attribútum](./media/active-directory-aadconnectsync-change-the-configuration/attributeflowjp1.png)

Egyéb attribútum folyamatok szabály létrehozásához tegye a következőket:

- Indítsa el a **Szinkronizálási szabály szerkesztőt** a start menüből.
- A **bejövő** balra legyen kijelölve kattintson a **Hozzáadás új szabály**gombra.
- Adjon meg a szabály nevét és leírását. Jelölje ki a helyszíni Active Directory és a megfelelő objektumtípusok.  **Kapcsolat típusa**csoportban válassza a **Csatlakozás**. Végrehajtási sorrendje válassza a szám, amely nem használja egy másik szabály. A kimenő kész szabályokat kezdje 100, hogy az 50 érték is használható, ebben a példában.
![2 attribútum továbbításához](./media/active-directory-aadconnectsync-change-the-configuration/attributeflowjp2.png)
- Hagyja üresen a hatókör (Ez azt jelenti, hogy kell alkalmazása az összes felhasználó objektumok erdőben).
- Csatlakozás szabályokat üresen hagyja (Ez azt jelenti, hogy legyen a kimenő kész szabály kapcsolások kezelése).
- Az átalakítás a következő folyamatok létrehozása:  
![3-as munkafolyamat attribútum](./media/active-directory-aadconnectsync-change-the-configuration/attributeflowjp3.png)
- Kattintson a **Hozzáadás gombra** kattintva mentheti a szabályt.
- Nyissa meg a **Szinkronizálást szolgáltatáskezelő**. **Összekötők**jelölje be az összekötő, ahol jelöltük a szabályt. Válassza a **Futtatás**, **teljes szinkronizálást**. Teljes szinkronizálást újraszámítja a jelenlegi szabályokat használó összes objektumot.

Az eredmény az azonos objektum egyéni Ez a szabály:  
![4 attribútum továbbításához](./media/active-directory-aadconnectsync-change-the-configuration/attributeflowjp4.png)

### <a name="length-of-attributes"></a>Attribútumok hossza
Karakterlánc attribútumok beállított példánycímkének alapértelmezés szerint ki legyenek, és a maximális hossza 448 karaktereket. Ha több vélhetően tartalmazó karakterlánc attribútumokkal rendelkező dolgozik, majd győződjön meg arról, szeretné hozzáadni a attribútum folyamat a következő:  
`attributeName` <- `Left([attributeName],448)`

### <a name="changing-the-userprincipalsuffix"></a>A userPrincipalSuffix módosítása
A userPrincipalName attribútum, az Active Directory azok a felhasználók nem mindig ismert, és nem is alkalmas, mint a bejelentkezési azonosítóját. A szinkronizálási telepítővarázslóban lehetővé teszi, hogy egy másik attribútum vesz Azure AD Connect például mail. De bizonyos esetekben az attribútum ki kell számítani. A vállalat Contoso például két Azure AD-könyvtárak, gyártási és a befogadó teszteléshez tartalmaz. Azok a felhasználóknak a próba-ös bérlői használni egy másik utótag a bejelentkezési azonosítóját.  
`userPrincipalName` <- `Word([userPrincipalName],1,"@") & "@contosotest.com"`

Ez a kifejezés, hogy várnia kell mindent az első bal oldali @-sign (Word) és a rögzített karakterlánc ÖSSZEFŰZ.

### <a name="convert-a-multi-value-to-a-single-value"></a>A több érték konvertálása egy értéket
Néhány az Active Directory attribútumokat a sémában többértékű akkor is, ha azokat meg egyetlen értékelni, az Active Directory-felhasználók és a számítógépen. Példa a leírás attribútum.  
`description` <- `IIF(IsNullOrEmpty([description]),NULL,Left(Trim(Item([description],1)),448))`

A kifejezésben abban az esetben, ha az attribútum értéke, azt az első elem () készítése az attribútum, távolítsa el a kezdő és záró szóközöket (Körülvágás), és majd tartsa a karakterlánc első 448 karaktereket (balra).

### <a name="do-not-flow-an-attribute"></a>Nem enged a tulajdonság
Az alkalmazási példát, ebben a szakaszban a [vezérlő az attribútum üzenetkezelési folyamat](active-directory-aadconnectsync-understanding-declarative-provisioning.md#control-the-attribute-flow-process)hátteréről.

Kétféleképpen beállítást nem értékre állítja attribútum. Az első érhető el a telepítés varázslót, és lehetővé teszi, hogy [távolítsa el a kijelölt attribútumok](active-directory-aadconnect-get-started-custom.md#azure-ad-app-and-attribute-filtering). Ezt a lehetőséget akkor működik, ha még nem szinkronizálta a attribútum előtt. Azonban elindítását követően attribútum szinkronizálni, és később távolíthatja el ezt a szolgáltatást, majd a szinkronizálás motor leáll a attribútum és a meglévő értékeket kezelése maradnak az Azure Active Directory.

Ha szeretné eltávolítani a tulajdonság értékét, és győződjön meg arról, hogy a jövőben nem enged, egyéni szabály inkább szükséges létrehozása.

A Fabrikam azt van történjen a realizálása, hogy néhány olyan azt a szinkronizálás a felhőbe attribútum nem kell van. Győződjön meg arról, hogy a következő attribútumok törlődjenek Azure AD szeretnénk.  
![Hibás kiterjesztés attribútumok](./media/active-directory-aadconnectsync-change-the-configuration/badextensionattribute.png)

- Hozzon létre egy új bejövő szinkronizálás szabályt, és a leírás feltöltése ![ismertetése](./media/active-directory-aadconnectsync-change-the-configuration/syncruledescription.png)
- Hozzon létre attribútum flow típusú **kifejezés** és a forrás **AuthoritativeNull**. A konstans **AuthoritativeNull** azt jelzi, hogy az értéket kell a té Elemet az üres, ha egy alsó elsőbbségi szinkronizálási szabály megkísérli feltölteni az érték.
![Bővítmény attribútumok transzformációt hajt végre.](./media/active-directory-aadconnectsync-change-the-configuration/syncruletransformations.png)
- Mentse a szinkronizálási szabályt. **Szinkronizálási**szolgáltatás, keresse meg az összekötő, jelölje be a **Futtatás**és a **Teljes szinkronizálást**. Ezt a lépést az összes attribútum flow újraszámolja.
- Győződjön meg róla, hogy a kívánt módosításokat készül, hogy a összekötő szóköz kereséssel exportálhatók.
![Szakaszos törlése](./media/active-directory-aadconnectsync-change-the-configuration/deletetobeexported.png)

## <a name="next-steps"></a>Következő lépések

- További információ a konfigurációs modellt [Deklaráció kiépítési ismertetése](active-directory-aadconnectsync-understanding-declarative-provisioning.md).
- További információ a kifejezésekben [ismertetése deklaráció kiépítési](active-directory-aadconnectsync-understanding-declarative-provisioning-expressions.md)nyelvén.

**Témakörök – áttekintés**

- [Azure AD Connect szinkronizálása: dátumtáblázatok ismertetése és szinkronizálás testreszabása](active-directory-aadconnectsync-whatis.md)
- [A helyszíni identitások integrálása az Azure Active Directory](active-directory-aadconnect.md)
