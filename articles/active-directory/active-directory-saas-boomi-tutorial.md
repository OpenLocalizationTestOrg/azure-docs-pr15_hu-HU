<properties 
    pageTitle="Oktatóprogram: Azure Active Directory-integráció a Boomi |} Microsoft Azure" 
    description="Megtudhatja, hogyan használhatja a Boomi az Azure Active Directory ahhoz, hogy az egyszeri bejelentkezés, automatikus kiépítési és az egyéb!" 
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

#<a name="tutorial-azure-active-directory-integration-with-boomi"></a>Oktatóprogram: Azure Active Directory-integráció a Boomi

Ebben az oktatóanyagban célja integrálása az Azure és Boomi megjelenítéséhez.  
Az alkalmazási példát, ebben az oktatóanyagban tagolt feltételezi, hogy már rendelkezik az alábbiakat:

-   Érvényes Azure előfizetés
-   Boomi egyszeri bejelentkezéses engedélyezett előfizetés

Ebben az oktatóanyagban befejeztével az Azure Active Directory-felhasználók Boomi rendelt fogja tudni az alkalmazás a Boomi vállalati webhely (service provider által kezdeményezett bejelentkezés) és használatáról a [Bevezetés az Access panelen](active-directory-saas-access-panel-introduction.md)az egyszeri bejelentkezési.

A következő építőelemek az alkalmazási példát, ebben az oktatóanyagban tagolt áll:

1.  Az alkalmazás integrációját Boomi engedélyezése
2.  Egyszeri bejelentkezés beállítása
3.  Felhasználói kiépítési konfigurálása
4.  Felhasználók hozzárendelése

![Eset] (./media/active-directory-saas-boomi-tutorial/IC791134.png "Eset")
##<a name="enabling-the-application-integration-for-boomi"></a>Az alkalmazás integrációját Boomi engedélyezése

Ez a szakasz célja, hogyan Boomi az alkalmazás alkalmazással való integráció engedélyezése a szerkezet.

###<a name="to-enable-the-application-integration-for-boomi-perform-the-following-steps"></a>Ahhoz, hogy az alkalmazás integrációját Boomi, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon kattintson a bal oldali navigációs területen kattintson az **Active Directory**.

    ![Az Active Directory] (./media/active-directory-saas-boomi-tutorial/IC700993.png "Az Active Directory")

2.  A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3.  Nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson a felső menüben **alkalmazásokat** .

    ![Alkalmazások] (./media/active-directory-saas-boomi-tutorial/IC700994.png "Alkalmazások")

4.  Kattintson a **Hozzáadás** az oldal alján.

    ![Alkalmazás hozzáadása] (./media/active-directory-saas-boomi-tutorial/IC749321.png "Alkalmazás hozzáadása")

5.  **Milyen feladatot szeretne tenni** párbeszédpanelen kattintson **a gyűjteményből az alkalmazások felvétele**.

    ![Gallerry az alkalmazás hozzáadása] (./media/active-directory-saas-boomi-tutorial/IC749322.png "Gallerry az alkalmazás hozzáadása")

6.  A **Keresés mezőbe**írja be a **Boomi**.

    ![Alkalmazás gyűjtemény] (./media/active-directory-saas-boomi-tutorial/IC790822.png "Alkalmazás gyűjtemény")

7.  Az eredmény ablaktáblában jelölje ki a **Boomi**, és válassza a **kész** , az alkalmazás hozzáadása gombra.

    ![Boomi] (./media/active-directory-saas-boomi-tutorial/IC790823.png "Boomi")
##<a name="configuring-single-sign-on"></a>Egyszeri bejelentkezés beállítása

Ez a szakasz célja tagolása engedélyezése a felhasználóknak, hogy Boomi hitelesíteni a fiókkal az összevonási alapján a SAML protokoll használatával Azure AD.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Egyszeri bejelentkezés beállításához hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon **Boomi** alkalmazás integrációs lapon kattintson **konfigurálása az egyszeri bejelentkezés** a **Konfigurálása az egyszeri bejelentkezés** párbeszédpanel megnyitásához.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-boomi-tutorial/IC790824.png "Egyszeri bejelentkezés beállítása")

2.  **Hogyan szeretné, hogy jelentkezzen be az Boomi felhasználók** lapon jelölje be **A Microsoft Azure Active Directory Single Sign-On**, és kattintson a **Tovább gombra**.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-boomi-tutorial/IC790825.png "Egyszeri bejelentkezés beállítása")

3.  A **App URL-cím beállítása** lapon az **Boomi válasz URL-cím** mezőbe írja be a **Boomi AtomSphere bejelentkezési URL-cím** (például: "*https://platform.boomi.com/sso/AccountName/saml*"), majd kattintson a **Tovább**gombra.

    ![Állítsa be az App URL-címe] (./media/active-directory-saas-boomi-tutorial/IC790826.png "Állítsa be az App URL-címe")

4.  A **Configure egyszeri bejelentkezés Boomi a** lapon a tanúsítvány letöltése, kattintson a **tanúsítvány letöltése**, és mentse a tanúsítvány fájlt a számítógépen helyben parancsra.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-boomi-tutorial/IC790827.png "Egyszeri bejelentkezés beállítása")

5.  A különböző webes böngészőablakban jelentkezzen be a Boomi vállalati webhely rendszergazdaként.

6.  Kattintson az eszköztár a képernyő tetején, a vállalat nevét, és **a telepítő**.

    ![A telepítő] (./media/active-directory-saas-boomi-tutorial/IC790828.png "A telepítő")

7.  **Egyszeri bejelentkezés beállítások**parancsára.

    ![Egyszeri bejelentkezés beállításai] (./media/active-directory-saas-boomi-tutorial/IC790829.png "Egyszeri bejelentkezés beállításai")

8.  **Egyszeri bejelentkezés beállításai** csoportban hajtsa végre az alábbi lépéseket:

    ![Egyszeri bejelentkezés beállításai] (./media/active-directory-saas-boomi-tutorial/IC790830.png "Egyszeri bejelentkezés beállításai")

    1.  Engedélyezze a **SAML Single Sign-On**.
    2.  **Importálás**a letöltött tanúsítvány feltöltése gombra.
    3.  Az Azure klasszikus portálon **konfigurálása az egyszeri bejelentkezés a Boomi** párbeszédpanel lapon a **Távoli bejelentkezési URL-cím** értéket másolja és illessze be a **Identitás szolgáltató bejelentkezési URL-cím** mezőben lévő értéket.
    4.  **Összevonás azonosító helyét**válassza az **összevonási azonosító NameID eleme tárgyát**.
    5.  Kattintson a **Mentés**gombra.

9.  Az Azure klasszikus portálon jelölje be a egyszeri bejelentkezéses konfigurációs megerősítés, és válassza a **kész** , **Állítsa be az egyszeri bejelentkezés** párbeszédpanel bezárásához.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-boomi-tutorial/IC775560.png "Egyszeri bejelentkezés beállítása")
##<a name="configuring-user-provisioning"></a>Felhasználói kiépítési konfigurálása

Ahhoz, hogy az Azure Active Directory-felhasználóknak, hogy jelentkezzen be a Boomi, hogy ki kell építenie Boomi be.  
Boomi, ha a feladat manuális kiépítési.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Felhasználói kiépítési beállításához hajtsa végre az alábbi lépéseket:

1.  Jelentkezzen be rendszergazdaként a **Boomi** vállalati webhely.

2.  Nyissa meg a **felhasználókezelés \> felhasználók**.

    ![Felhasználók] (./media/active-directory-saas-boomi-tutorial/IC790831.png "Felhasználók")

3.  Kattintson a **felhasználó hozzáadása**elemre.

    ![Felhasználó hozzáadása] (./media/active-directory-saas-boomi-tutorial/IC790832.png "Felhasználó hozzáadása")

4.  A **Felhasználói szerepkörök hozzáadása** párbeszédpanel lapon hajtsa végre az alábbi lépéseket:

    ![Felhasználó hozzáadása] (./media/active-directory-saas-boomi-tutorial/IC790833.png "Felhasználó hozzáadása")

    1.  Írja be az **Utónév** **Vezetéknév** és építse be a kapcsolódó szövegdobozok kívánt **E-mail** fiók érvényes AAD.
    2.  Kattintson az OK gombra.

>[AZURE.NOTE] Bármely más Boomi felhasználói fiók létrehozása eszközöket is használhatja, illetve Boomi rendelkezést AAD felhasználói fiókok által biztosított API-khoz.

##<a name="assigning-users"></a>Felhasználók hozzárendelése

Tesztelje a konfigurációt, meg kell adni az Azure Active Directory-felhasználók, használja az alkalmazás elérési útvonalát társításával engedélyezni szeretné.

###<a name="to-assign-users-to-boomi-perform-the-following-steps"></a>Felhasználók hozzárendelése Boomi, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon próba-fiók létrehozása.

2.  Kattintson a **Boomi **alkalmazás integrációs lap **hozzárendelését a felhasználókhoz**.

    ![Adhatnak a felhasználóknak] (./media/active-directory-saas-boomi-tutorial/IC790834.png "Adhatnak a felhasználóknak")

3.  Jelölje ki a próba-felhasználó, kattintson a **hozzárendelni**, és kattintson az **Igen gombra** kattintva erősítse meg a hozzárendelés.

    ![Igen] (./media/active-directory-saas-boomi-tutorial/IC767830.png "Igen")

Ha meg szeretné tesztelje a egyszeri bejelentkezéses, Access Panel megnyitásához. Ha többet szeretne tudni az Access panelen című témakörben [az Access-panelen](active-directory-saas-access-panel-introduction.md).
