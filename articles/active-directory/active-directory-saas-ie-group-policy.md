<properties
    pageTitle="Az Internet Explorer csoportházirend segítségével az Access Vezérlőpult-kiterjesztés telepítéséről |} Microsoft Azure"
    description="Hogyan lehet az Internet Explorer bővítmény a saját alkalmazások portál csoportházirend használatával."
    services="active-directory"
    documentationCenter=""
    authors="MarkusVi"
    manager="femila"
    editor=""/>
<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="08/16/2016"
    ms.author="markvi"/>

#<a name="how-to-deploy-the-access-panel-extension-for-internet-explorer-using-group-policy"></a>Az Access Vezérlőpult-kiterjesztés telepítéséről az Internet Explorer csoportházirend használatával

Ebből az oktatóanyagból megtudhatja, hogy hogyan csoportházirend távolról telepítéséhez felhasználói gépen az Internet Explorer az Access Vezérlőpult-kiterjesztés. A bővítmény szükség az Internet Explorer felhasználók, akik kell bejelentkezni a [jelszó-alapú egyszeri bejelentkezés](active-directory-appssoaccess-whatis.md#password-based-single-sign-on)használata beállított alkalmazásokat.

Javasoljuk, hogy a rendszergazdák automatizálható a erre a mellékre. Egyéb esetben felhasználók kell töltse le és telepítse a bővítmény maguk, amely jobban felhasználói hiba, és csak a rendszergazdai engedélyekkel. Ebben az oktatóanyagban bemutatja a szoftver telepítések automatizálása csoportházirenden keresztül egy metódusát. [További tudnivalók a csoportházirend.](https://technet.microsoft.com/windowsserver/bb310732.aspx)

Az Access Vezérlőpult-kiterjesztés érhető el a [Chrome](https://go.microsoft.com/fwLink/?LinkID=311859) és a [Firefox](https://go.microsoft.com/fwLink/?LinkID=626998), amelyek egyike sem megkövetelése telepítéséhez rendszergazdai engedélyekkel.

##<a name="prerequisites"></a>Előfeltételek

- [Active Directory tartományi szolgáltatások](https://msdn.microsoft.com/library/aa362244%28v=vs.85%29.aspx)be van állítva, és a tartományába felhasználói gépek tagja.
- A csoportházirend-objektumok (GPO) szerkesztéséhez "Beállítások szerkesztése" engedéllyel kell rendelkeznie. Alapértelmezés szerint a következő biztonsági csoportok tagjai ezzel az engedéllyel rendelkezik: tartomány Rendszergazdák, a vállalati rendszergazdák és a csoportházirend-készítő tulajdonosok. [tudj meg többet.](https://technet.microsoft.com/library/cc781991%28v=ws.10%29.aspx)

##<a name="step-1-create-the-distribution-point"></a>Lépés: 1: A terjesztési pont létrehozása

Először be kell jelölnie a a telepítőcsomagot meg egy hálózati helyet a gép távolról telepítése a bővítmény használni kívánt minden is elérhető. Ehhez tegye a következőket:

1. Rendszergazdaként jelentkezzen be a kiszolgáló

2. A **Kiszolgálókezelő** ablakban nyissa meg a **fájlokat**és tárolása.

    ![Fájlok megnyitása és tárolása](./media/active-directory-saas-ie-group-policy/files-services.png)

3. Nyissa meg a **megosztás** fülre. Kattintson a **tevékenységek** > **Új megosztás...**

    ![Fájlok megnyitása és tárolása](./media/active-directory-saas-ie-group-policy/shares.png)

4. Fejezze be az **Új megosztási varázslóban** , és adja meg annak érdekében, hogy elérhető legyen a számítógépek felhasználói engedélyek. [További tudnivalók a megosztások.](https://technet.microsoft.com/library/cc753175.aspx)

5. A következő Microsoft Windows Installer (MSI-fájl) csomag letöltése: [Access Panel Extension.msi](https://account.activedirectory.windowsazure.com/Applications/Installers/x64/Access Panel Extension.msi)

6. Másolja a kívánt helyre a megosztott a telepítőcsomagot.

    ![Másolása az .msi fájlt, hogy a megosztás.](./media/active-directory-saas-ie-group-policy/copy-package.png)

8. Győződjön meg róla, hogy a telepítőcsomag elérheti a megosztásról az ügyfél gépek. 

##<a name="step-2-create-the-group-policy-object"></a>Lépés: 2: A csoportházirend-objektum létrehozása

1. Jelentkezzen be a kiszolgáló, amely az Active Directory tartományi szolgáltatások (AD DS) telepítése.

2. A kiszolgáló-kezelőben válassza az **eszközök** > **A Csoportházirend kezelése**.

    ![Kattintson az eszközök > házirend felügyeleti csoport](./media/active-directory-saas-ie-group-policy/tools-gpm.png)

3. A bal oldali ablaktáblában a **Csoportházirend kezelése** ablak a szervezeti egységre (OU) hierarchia megtekintése, és meghatározzák, hogy a hatókör szeretné alkalmazni a csoportházirend. Például dönthet a Válasszon egy kis OU teszteléshez néhány felhasználót szeretne telepíteni, vagy egy felső szintű OU való telepítéséhez az egész szervezet számára is választhat.

    > [AZURE.NOTE] Ha azt szeretné, hogy létrehozása vagy szerkesztése a szervezet egységeket, váltson vissza szeretné a Kiszolgálókezelő, és válassza az **eszközök** > **Active Directory-felhasználók és a számítógépen**.

4. Miután kiválasztotta a szervezeti egység, kattintson rá a jobb gombbal, és válassza a **Hozzon létre egy csoportházirend-objektum, ebben a tartományban, és csatolja az alábbi...**

    ![Hozzon létre egy új csoportházirend-objektum](./media/active-directory-saas-ie-group-policy/create-gpo.png)

5. **Új csoportházirend -objektum** kérdésnél írja be az új csoportházirend-objektum nevét.

    ![Az új csoportházirend-objektum neve](./media/active-directory-saas-ie-group-policy/name-gpo.png)

6. Kattintson a jobb gombbal az imént létrehozott csoportházirend-objektum, és válassza a **Szerkesztés**gombra.

    ![Az új csoportházirend-objektum szerkesztése](./media/active-directory-saas-ie-group-policy/edit-gpo.png)

##<a name="step-3-assign-the-installation-package"></a>3 lépés: A telepítőcsomag hozzárendelése

1. Határozza meg, hogy szeretne-e a **vagy a **Számítógép konfigurációja** **alapján bővítmény telepítése. [Emlékeztetők](https://technet.microsoft.com/library/cc736413%28v=ws.10%29.aspx)használata esetén a bővítmény függetlenül attól, mely felhasználók log megjelenítheti, hogy a számítógépen telepíthető. A [Felhasználó konfigurációja](https://technet.microsoft.com/library/cc781953%28v=ws.10%29.aspx), azonban, felhasználók lesz a bővítmény telepítve van az függetlenül attól, mely számítógépek jelentkeznek be őket.

2. A bal oldali ablaktáblában a **Csoportházirendkezelés-szerkesztőben** ablak lépjen a következő mappa elérési utak, attól függően, hogy milyen típusú konfigurációs kiválasztott közül választhat:
    - `Computer Configuration/Policies/Software Settings/`
    - `User Configuration/Policies/Software Settings/`

3. Kattintson a jobb gombbal a **Szoftvertelepítés**, majd válassza az **Új** > **csomag...**

    ![Hozzon létre egy új telepítési szoftvercsomag](./media/active-directory-saas-ie-group-policy/new-package.png)

4. Nyissa meg a megosztott mappára, amely tartalmazza a telepítőcsomagot a [lépés 1: a terjesztési pont létrehozása](#step-1-create-the-distribution-point), jelölje be a azokat az .msi fájlokat, és kattintson a **Megnyitás**gombra.

    > [AZURE.IMPORTANT] Ha a megosztás ezen a kiszolgálón található, győződjön meg arról, hogy elérése az .msi keresztül a helyi fájl elérési útját, hanem a network fájl elérési útját.

    ![Jelölje ki a telepítőcsomag a megosztott mappára.](./media/active-directory-saas-ie-group-policy/select-package.png)

5. A **Szoftver telepítése** kérdésnél válassza ki a telepítési mód **hozzárendelt** . Kattintson **az OK**gombra.

    ![Jelölje ki a hozzárendelt, majd kattintson az OK gombra.](./media/active-directory-saas-ie-group-policy/deployment-method.png)

A bővítmény most már telepítve van a szervezeti kijelölt. [További tudnivalók a Csoportházirend szoftver telepítése.](https://technet.microsoft.com/library/cc738858%28v=ws.10%29.aspx)

##<a name="step-4-auto-enable-the-extension-for-internet-explorer"></a>Lépés: 4: Automatikus engedélyezése az Internet Explorer a bővítmény 

A telepítő fut, kívül minden bővítmény az Internet Explorer kifejezetten előtt engedélyeznie kell is használható. Ahhoz, hogy az Access Vezérlőpult-kiterjesztés csoportházirend használatával az alábbi lépésekkel:

1. A **Csoportházirendkezelés-szerkesztőben** ablakban nyissa meg a következő elérési, ha a kiválasztott attól függően, hogy milyen típusú konfigurációs közül [a 3: hozzárendelése a telepítőcsomag](#step-3-assign-the-installation-package):
    - `Computer Configuration/Policies/Administrative Templates/Windows Components/Internet Explorer/Security Features/Add-on Management`
    - `User Configuration/Policies/Administrative Templates/Windows Components/Internet Explorer/Security Features/Add-on Management`

2. **Bővítmények**listában kattintson a jobb gombbal, és válassza a **Szerkesztés**gombra.
    ![Bővítmény lista szerkesztése elemre.](./media/active-directory-saas-ie-group-policy/edit-add-on-list.png)

3. A **Bővítmény lista** ablakában jelölje ki a **engedélyezve**. A **Beállítások** csoportban kattintson a **… megjelenítése**.

    ![Kattintson az Engedélyezés gombra, majd kattintson a megjelenítés...](./media/active-directory-saas-ie-group-policy/edit-add-on-list-window.png)

4. **Tartalom megjelenítése** ablakban végezze el az alábbi lépéseket:

    1. Az első oszlop (az **Azonosító neve** mezőbe) másolja és illessze be az alábbi osztály azonosító:`{030E9A3F-7B18-4122-9A60-B87235E4F59E}`

    2. A második oszlophoz ( **érték** -mező) írja be a következő értéket:`1`

    3. Kattintson az **OK gombra** a **Tartalom megjelenítése** ablak bezárásához.

    ![Töltse ki az értékeket a fent ismertetett módon.](./media/active-directory-saas-ie-group-policy/show-contents.png)

5. Alkalmazza a változtatásokat, és a **Bővítmény lista** ablak bezárásához az **OK** gombra.

A bővítmény most már engedélyezni kell a a kijelölt szervezeti egység gépek. [További tudnivalók a Csoportházirend használatának engedélyezése és letiltása az Internet Explorer bővítmények.](https://technet.microsoft.com/library/dn454941.aspx)

##<a name="step-5-optional-disable-remember-password-prompt"></a>5 lépésben (nem kötelező): tiltsa le a "Ne feledje, hogy a jelszó" kérdés

Amikor a felhasználók jelentkezzen be az-webhelyek használata az Access Vezérlőpult-kiterjesztés, előfordulhat, hogy az Internet Explorer megjelenítése az az alábbi, "Szeretne tárolni a jelszavát?" kérő üzenet

![](./media/active-directory-saas-ie-group-policy/remember-password-prompt.png)

Ha szeretné megakadályozni, hogy a felhasználóknak jelenik meg ez az üzenet, kövesse az alábbi lépéseket, hogy az automatikus kiegészítési a jelszavak megjegyzése:

1. A **Csoportházirendkezelés-szerkesztőben** ablakban nyissa meg az elérési út az alább felsorolt. Figyelje meg, hogy a konfigurációs beállítás csak akkor érhető el, a **Felhasználó konfigurációja**.
    - `User Configuration/Policies/Administrative Templates/Windows Components/Internet Explorer/`

2. Keresse meg a nevű, **a felhasználóneveket és jelszavakat űrlapok automatikus kiegészítési szolgáltatás bekapcsolása**beállítást.

    > [AZURE.NOTE] Az Active Directory korábbi verzióinak sorolhatja **esetén nem engedélyezik automatikus kiegészítési jelszavak mentése**nevű ezt a beállítást. Az adott beállítás beállításai különböznek egymástól az ebben az oktatóanyagban leírt beállítás.

    ![Ne feledje, hogy a felhasználói beállítások ez keres.](./media/active-directory-saas-ie-group-policy/disable-auto-complete.png)

3. A fenti beállítások kattintson a jobb gombbal, és válassza a **Szerkesztés**gombra.

4. **Kapcsolja be az automatikus kiegészítési szolgáltatás, a felhasználóneveket és jelszavakat űrlapok**című ablakában válassza a **Letiltva**.

    ![Jelölje ki a letiltása](./media/active-directory-saas-ie-group-policy/disable-passwords.png)

5. Kattintson az **OK gombra** a módosítások alkalmazása és az ablak bezárásához.

Felhasználók már nem tudja a hitelesítő adatok vagy az automatikus kiegészítési használata korábban tárolt hitelesítő adatok eléréséhez. Jó helyen jár ezzel a házirenddel felhasználóimnak, hogy továbbra is használhatja az automatikus kiegészítési más típusú, például kereséshez használható mezők a mezőket.

> [AZURE.WARNING] Ha ezzel a házirenddel engedélyezve van a követően a felhasználók döntött, hogy bizonyos a házirend fog hitelesítő adatainak tárolása *nem* már tárolt hitelesítő adatok törlése.

##<a name="step-6-testing-the-deployment"></a>Lépés a 6: a központi telepítés tesztelése

Ellenőrizze, hogy a bővítmény telepítő sikeres volt-e az alábbi lépésekkel:

1. Ha telepíti a **Számítógép**konfigurációval, jelentkezzen be az ügyfél számítógépén, amelyhez tartozik a kiválasztott szervezeti [2 lépés: a csoportházirend-objektum létrehozása](#step-2-create-the-group-policy-object). Ha telepíti a **Felhasználó konfigurációja**használ, győződjön meg róla, hogy a felhasználók, akik a szervezeti egységre tartozik rendszergazdaként jelentkezzen be.

2. Eltarthat néhány bejelentkezési modulok esetében a csoportházirend teljesen módosításaira frissítése az ezen a számítógépen. A frissítés nyisson meg **egy parancssorablakot** , és futtassa az alábbi parancsot:`gpupdate /force`

3. Meg kell indítsa újra a telepítést helyébe keresése a számítógépen. Indítási jelentős mértékben tovább tarthat, mint a szokásos, miközben a bővítmény telepítése.

4. Újraindítása után nyissa meg az **Internet Explorerben**. Kattintson az ablak jobb felső sarkában kattintson az **eszközök** (fogaskerék ikon), és válassza a **Bővítmények kezelése**parancsra.

    ![Kattintson az eszközök > Bővítmények kezelése](./media/active-directory-saas-ie-group-policy/manage-add-ons.png)

5. A **Bővítmények kezelése** párbeszédpanelen ellenőrizze, hogy a **Vezérlőpult-kiterjesztés Access** telepítve van, hogy az **állapot** **engedélyezve**van beállítva.

    ![Győződjön meg arról, hogy az Access Vezérlőpult-kiterjesztés telepítve és engedélyezve van.](./media/active-directory-saas-ie-group-policy/verify-install.png)

## <a name="related-articles"></a>Kapcsolódó cikkek

- [A cikk az Azure Active Directory Alkalmazáskezelés Index](active-directory-apps-index.md)
- [Access alkalmazás és az egyszeri bejelentkezés az Azure Active Directory címtárral](active-directory-appssoaccess-whatis.md)
- [Az Access Vezérlőpult-kiterjesztés hibaelhárítás az Internet Explorer](active-directory-saas-ie-troubleshooting.md)
