<properties 
    pageTitle="Oktatóprogram: Azure Active Directory-integráció a Coupa |} Microsoft Azure" 
    description="Megtudhatja, hogyan használhatja a Coupa az Azure Active Directory ahhoz, hogy az egyszeri bejelentkezés, automatikus kiépítési és az egyéb!" 
    services="active-directory" 
    authors="jeevansd"  
    documentationCenter="na" 
    manager="femila"/>
<tags 
    ms.service="active-directory" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="identity" 
    ms.date="09/29/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-coupa"></a>Oktatóprogram: Azure Active Directory-integráció a Coupa

Ebben az oktatóanyagban célja integrálása az Azure és Coupa megjelenítéséhez.  
Az alkalmazási példát, ebben az oktatóanyagban tagolt feltételezi, hogy már rendelkezik az alábbiakat:

-   Érvényes Azure előfizetés
-   Coupa egyszeri bejelentkezéses engedélyezett előfizetés

Ebben az oktatóanyagban befejeztével az Azure Active Directory-felhasználók Coupa rendelt fogja tudni az alkalmazás használatáról a [Bevezetés az Access panelen](active-directory-saas-access-panel-introduction.md)egyszeri bejelentkezési.

A következő építőelemek az alkalmazási példát, ebben az oktatóanyagban tagolt áll:

1.  Az alkalmazás integrációját Coupa engedélyezése
2.  Egyszeri bejelentkezés beállítása
3.  Felhasználói kiépítési konfigurálása
4.  Felhasználók hozzárendelése

![Eset] (./media/active-directory-saas-coupa-tutorial/IC791897.png "Eset")
##<a name="enabling-the-application-integration-for-coupa"></a>Az alkalmazás integrációját Coupa engedélyezése

Ez a szakasz célja, hogyan Coupa az alkalmazás alkalmazással való integráció engedélyezése a szerkezet.

###<a name="to-enable-the-application-integration-for-coupa-perform-the-following-steps"></a>Ahhoz, hogy az alkalmazás integrációját Coupa, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon kattintson a bal oldali navigációs területen kattintson az **Active Directory**.

    ![Az Active Directory] (./media/active-directory-saas-coupa-tutorial/IC700993.png "Az Active Directory")

2.  A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3.  Nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson a felső menüben **alkalmazásokat** .

    ![Alkalmazások] (./media/active-directory-saas-coupa-tutorial/IC700994.png "Alkalmazások")

4.  Kattintson a **Hozzáadás** az oldal alján.

    ![Alkalmazás hozzáadása] (./media/active-directory-saas-coupa-tutorial/IC749321.png "Alkalmazás hozzáadása")

5.  **Milyen feladatot szeretne tenni** párbeszédpanelen kattintson **a gyűjteményből az alkalmazások felvétele**.

    ![Gallerry az alkalmazás hozzáadása] (./media/active-directory-saas-coupa-tutorial/IC749322.png "Gallerry az alkalmazás hozzáadása")

6.  A **Keresés mezőbe**írja be a **Coupa**.

    ![Alkalmazás gyűjtemény] (./media/active-directory-saas-coupa-tutorial/IC791898.png "Alkalmazás gyűjtemény")

7.  Az eredmény ablaktáblában jelölje ki a **Coupa**, és válassza a **kész** , az alkalmazás hozzáadása gombra.

    ![Coupa] (./media/active-directory-saas-coupa-tutorial/IC791899.png "Coupa")
##<a name="configuring-single-sign-on"></a>Egyszeri bejelentkezés beállítása

Ez a szakasz célja tagolása engedélyezése a felhasználóknak, hogy Coupa hitelesíteni a fiókkal az összevonási alapján a SAML protokoll használatával Azure AD.  
Egyszeri bejelentkezés az Coupa konfigurálása megköveteli ujjlenyomat érték beolvasása egy tanúsítványt.  
Ha nem ismeri a ezt az eljárást, megtudhatja, [hogy miként beolvasni a tanúsítvány ujjlenyomat értékét](http://youtu.be/YKQF266SAxI).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Egyszeri bejelentkezés beállításához hajtsa végre az alábbi lépéseket:

1.  Jelentkezzen be rendszergazdaként a Coupa vállalati webhely.

2.  Nyissa meg a **beállítási \> biztonsági vezérlő**.

    ![Biztonsági funkciók] (./media/active-directory-saas-coupa-tutorial/IC791900.png "Biztonsági funkciók")

3.  **Töltse le és SP-metaadatok importálása**kattintva töltse le a Coupa metaadat-fájlt a számítógépére.

    ![Coupa SP-metaadat] (./media/active-directory-saas-coupa-tutorial/IC791901.png "Coupa SP-metaadat")

4.  Egy másik böngészőablakban jelentkezzen az Azure klasszikus portálra.

5.  **Coupa** alkalmazás integrációs lapon kattintson **konfigurálása az egyszeri bejelentkezés** a **Konfigurálása az egyszeri bejelentkezés** párbeszédpanel megnyitásához.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-coupa-tutorial/IC791902.png "Egyszeri bejelentkezés beállítása")

6.  **Hogyan szeretné, hogy jelentkezzen be az Coupa felhasználók** lapon jelölje be **A Microsoft Azure Active Directory Single Sign-On**, és kattintson a **Tovább gombra**.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-coupa-tutorial/IC791903.png "Egyszeri bejelentkezés beállítása")

7.  Az **Alkalmazás URL-cím beállítása** lapon hajtsa végre az alábbi lépéseket:

    ![Állítsa be az App URL-címe] (./media/active-directory-saas-coupa-tutorial/IC791904.png "Állítsa be az App URL-címe")

    1.  **Bejelentkezési a URL-cím** mezőbe írja be a Coupa alkalmazás bejelentkezni a felhasználók által használt URL-CÍMÉT (például: "*http://company.Coupa.com*").
    2.  Nyissa meg a letöltött Coupa metaadat-fájlt, és kattintson a **AssertionConsumerService index/URL-cím**másolása.
    3.  Az **Coupa válasz URL-címe** mezőbe illessze be a **AssertionConsumerService index/URL-cím** értéket.
    4.  Kattintson a **Tovább**gombra.

8.  **Konfigurálása az egyszeri bejelentkezés Coupa a** lapon a metaadat-alapú fájl letöltéséhez kattintson a **metaadat-alapú letöltése**, és mentse a fájlt a számítógépen helyben parancsra.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-coupa-tutorial/IC791905.png "Egyszeri bejelentkezés beállítása")

9.  A vállalat Coupa webhelyen lépjen **beállítási \> biztonsági vezérlő**.

    ![Biztonsági funkciók] (./media/active-directory-saas-coupa-tutorial/IC791900.png "Biztonsági funkciók")

10. **Jelentkezzen be Coupa hitelesítő adatok** csoportban hajtsa végre az alábbi lépéseket:

    ![Jelentkezzen be Coupa hitelesítő adataival.] (./media/active-directory-saas-coupa-tutorial/IC791906.png "Jelentkezzen be Coupa hitelesítő adataival.")

    1.  Jelölje be, **Jelentkezzen be SAML**.
    2.  **Tallózással keresse meg** a letöltött Azure Active metaadat-fájl feltöltése gombra.
    3.  Kattintson a **Mentés**gombra.

11. Az Azure klasszikus portálon jelölje be a egyszeri bejelentkezéses konfigurációs megerősítés, és válassza a **kész** , **Állítsa be az egyszeri bejelentkezés** párbeszédpanel bezárásához.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-coupa-tutorial/IC791907.png "Egyszeri bejelentkezés beállítása")
##<a name="configuring-user-provisioning"></a>Felhasználói kiépítési konfigurálása

Ahhoz, hogy az Azure Active Directory-felhasználóknak, hogy jelentkezzen be a Coupa, hogy ki kell építenie Coupa be.  
Coupa, ha a feladat manuális kiépítési.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Felhasználói kiépítési beállításához hajtsa végre az alábbi lépéseket:

1.  Jelentkezzen be rendszergazdaként a **Coupa** vállalati webhely.

2.  A felső sávon kattintson a **beállítás**gombra, és kattintson a **felhasználók**.

    ![Felhasználók] (./media/active-directory-saas-coupa-tutorial/IC791908.png "Felhasználók")

3.  Kattintson a **létrehozása**gombra.

    ![Felhasználók létrehozása] (./media/active-directory-saas-coupa-tutorial/IC791909.png "Felhasználók létrehozása")

4.  **Felhasználó létrehozása** csoportjában hajtsa végre az alábbi lépéseket:

    ![Felhasználó adatai] (./media/active-directory-saas-coupa-tutorial/IC791910.png "Felhasználó adatai")

    1.  Írja be a **bejelentkezési**, **Utónév**, **Vezetéknév**, **Egyszeri bejelentkezéses azonosítója**, egy érvényes Azure Active Directory-fiókját, kiépítése a kapcsolódó szövegdobozok be a kívánt **E-mail** attribútumainak.
    2.  Kattintson a **létrehozása**gombra.

    >[AZURE.NOTE] Az Azure Active Directory fióktulajdonos megismerheti az e-mail megelőzően aktívvá válik, győződjön meg arról, hogy a fiók mutató hivatkozással.

>[AZURE.NOTE] Bármely más Coupa felhasználói fiók létrehozása eszközöket is használhatja, illetve Coupa rendelkezést AAD felhasználói fiókok által biztosított API-khoz.

##<a name="assigning-users"></a>Felhasználók hozzárendelése

Tesztelje a konfigurációt, meg kell adni az Azure Active Directory-felhasználók, használja az alkalmazás elérési útvonalát társításával engedélyezni szeretné.

###<a name="to-assign-users-to-coupa-perform-the-following-steps"></a>Felhasználók hozzárendelése Coupa, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon próba-fiók létrehozása.

2.  Kattintson a **Coupa **alkalmazás integrációs lap **hozzárendelését a felhasználókhoz**.

    ![Adhatnak a felhasználóknak] (./media/active-directory-saas-coupa-tutorial/IC791911.png "Adhatnak a felhasználóknak")

3.  Jelölje ki a próba-felhasználó, kattintson a **hozzárendelni**, és kattintson az **Igen gombra** kattintva erősítse meg a hozzárendelés.

    ![Igen] (./media/active-directory-saas-coupa-tutorial/IC767830.png "Igen")

Ha meg szeretné tesztelje a egyszeri bejelentkezéses, Access Panel megnyitásához. Ha többet szeretne tudni az Access panelen című témakörben [az Access-panelen](active-directory-saas-access-panel-introduction.md).
