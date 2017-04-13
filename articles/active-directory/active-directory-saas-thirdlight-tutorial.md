<properties 
    pageTitle="Oktatóprogram: Azure Active Directory-integráció a Thirdlight |} Microsoft Azure" 
    description="Megtudhatja, hogyan használhatja a Thirdlight az Azure Active Directory ahhoz, hogy az egyszeri bejelentkezés, automatikus kiépítési és az egyéb!" 
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
    ms.date="09/11/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-thirdlight"></a>Oktatóprogram: Azure Active Directory-integráció a Thirdlight
  
Ebben az oktatóanyagban célja integrálása az Azure és Thirdlight megjelenítéséhez.  
Az alkalmazási példát, ebben az oktatóanyagban tagolt feltételezi, hogy már rendelkezik az alábbiakat:

-   Érvényes Azure előfizetés
-   Thirdlight egyszeri bejelentkezéses engedélyezett előfizetés
  
Ebben az oktatóanyagban befejeztével az Azure Active Directory-felhasználók Thirdlight rendelt fogja tudni az alkalmazás a Thirdlight vállalati webhely (service provider által kezdeményezett bejelentkezés) és használatáról a [Bevezetés az Access panelen](active-directory-saas-access-panel-introduction.md)az egyszeri bejelentkezési.
  
A következő építőelemek az alkalmazási példát, ebben az oktatóanyagban tagolt áll:

1.  Az alkalmazás integrációját Thirdlight engedélyezése
2.  Egyszeri bejelentkezés beállítása
3.  Felhasználói kiépítési konfigurálása
4.  Felhasználók hozzárendelése

![Eset] (./media/active-directory-saas-thirdlight-tutorial/IC805836.png "Eset")

##<a name="enabling-the-application-integration-for-thirdlight"></a>Az alkalmazás integrációját Thirdlight engedélyezése
  
Ez a szakasz célja, hogyan Thirdlight az alkalmazás alkalmazással való integráció engedélyezése a szerkezet.

###<a name="to-enable-the-application-integration-for-thirdlight-perform-the-following-steps"></a>Ahhoz, hogy az alkalmazás integrációját Thirdlight, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon kattintson a bal oldali navigációs területen kattintson az **Active Directory**.

    ![Az Active Directory] (./media/active-directory-saas-thirdlight-tutorial/IC700993.png "Az Active Directory")

2.  A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3.  Nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson a felső menüben **alkalmazásokat** .

    ![Alkalmazások] (./media/active-directory-saas-thirdlight-tutorial/IC700994.png "Alkalmazások")

4.  Kattintson a **Hozzáadás** az oldal alján.

    ![Alkalmazás hozzáadása] (./media/active-directory-saas-thirdlight-tutorial/IC749321.png "Alkalmazás hozzáadása")

5.  **Milyen feladatot szeretne tenni** párbeszédpanelen kattintson **a gyűjteményből az alkalmazások felvétele**.

    ![Gallerry az alkalmazás hozzáadása] (./media/active-directory-saas-thirdlight-tutorial/IC749322.png "Gallerry az alkalmazás hozzáadása")

6.  A **Keresés mezőbe**írja be a **Thirdlight**.

    ![Alkalmazás gyűjtemény] (./media/active-directory-saas-thirdlight-tutorial/IC805837.png "Alkalmazás gyűjtemény")

7.  Az eredmény ablaktáblában jelölje ki a **Thirdlight**, és válassza a **kész** , az alkalmazás hozzáadása gombra.

    ![ThirdLight] (./media/active-directory-saas-thirdlight-tutorial/IC805838.png "ThirdLight")

##<a name="configuring-single-sign-on"></a>Egyszeri bejelentkezés beállítása
  
Ez a szakasz célja tagolása engedélyezése a felhasználóknak, hogy Thirdlight hitelesíteni a fiókkal az összevonási alapján a SAML protokoll használatával Azure AD.  
Egyszeri bejelentkezés az Thirdlight konfigurálása megköveteli ujjlenyomat érték beolvasása egy tanúsítványt.  
Ha nem ismeri a ezt az eljárást, megtudhatja, [hogy miként beolvasni a tanúsítvány ujjlenyomat értékét](http://youtu.be/YKQF266SAxI).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Egyszeri bejelentkezés beállításához hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon **Thirdlight** alkalmazás integrációs lapon kattintson **konfigurálása az egyszeri bejelentkezés** a **Konfigurálása az egyszeri bejelentkezés** párbeszédpanel megnyitásához.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-thirdlight-tutorial/IC805839.png "Egyszeri bejelentkezés beállítása")

2.  **Hogyan szeretné, hogy jelentkezzen be az Thirdlight felhasználók** lapon jelölje be **A Microsoft Azure Active Directory Single Sign-On**, és kattintson a **Tovább gombra**.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-thirdlight-tutorial/IC805840.png "Egyszeri bejelentkezés beállítása")

3.  Az **App URL-címe konfigurálása** lapon **Thirdlight bejelentkezési az URL** mezőbe írja be a jelentkezzen be az alkalmazás Thirdlight a felhasználók által használt URL-cím (például: "*http://azuresso2.thirdlight.com/*"), majd kattintson a **Tovább**gombra.

    ![Állítsa be az App URL-címe] (./media/active-directory-saas-thirdlight-tutorial/IC805841.png "Állítsa be az App URL-címe")

4.  **Konfigurálása az egyszeri bejelentkezés Thirdlight a** lapon a metaadat-alapú letöltéséhez kattintson a **metaadat-alapú letöltése**, és mentse a fájlt a számítógépen helyben metaadat parancsra.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-thirdlight-tutorial/IC805842.png "Egyszeri bejelentkezés beállítása")

5.  A különböző webes böngészőablakban jelentkezzen be a Thirdlight vállalati webhely rendszergazdaként.

6.  Nyissa meg a **konfigurációs \> rendszergazdai**, és kattintson a **SAML2**.

    ![Rendszergazdai] (./media/active-directory-saas-thirdlight-tutorial/IC805843.png "Rendszergazdai")

7.  A SAML2 konfigurálása csoportban hajtsa végre az alábbi lépéseket:

    ![SAML Single Sign-On] (./media/active-directory-saas-thirdlight-tutorial/IC805844.png "SAML Single Sign-On")

    1.  Engedélyezze a **SAML2 Single Sign-On**.
    2.  **Forrás IdP metaadatok**jelölje ki a **Betöltés IdP metaadatok XML-ből**.
    3.  Nyissa meg a letöltött metaadat-fájlt, a tartalom másolása és beillesztése a **IdP metaadatok XML** mezőben lévő értéket.
    4.  Kattintson a **Mentés SAML2 beállítások**gombra.

8.  Az Azure klasszikus portálon jelölje be a egyszeri bejelentkezéses konfigurációs megerősítés, és válassza a **kész** , **Állítsa be az egyszeri bejelentkezés** párbeszédpanel bezárásához.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-thirdlight-tutorial/IC805845.png "Egyszeri bejelentkezés beállítása")

##<a name="configuring-user-provisioning"></a>Felhasználói kiépítési konfigurálása
  
Ahhoz, hogy az Azure Active Directory-felhasználóknak, hogy jelentkezzen be a Thirdlight, hogy ki kell építenie Thirdlight be.  
Thirdlight, ha a feladat manuális kiépítési.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Felhasználói kiépítési beállításához hajtsa végre az alábbi lépéseket:

1.  Jelentkezzen be rendszergazdaként a **Thirdlight** vállalati webhely.

2.  Nyissa meg a **felhasználók** lapon.

3.  Válassza a **felhasználók és csoportok ablakot**.

4.  Kattintson az **Új felhasználó hozzáadása** gombra.

5.  Írja be **a felhasználónév, nevét vagy leírását, e-mailek, válasszon egy készletet vagy új tagok csoport** kívánt érvényes AAD fiók kiépítése.

6.  Kattintson a **létrehozása**gombra.

>[AZURE.NOTE] Bármely más Thirdlight felhasználói fiók létrehozása eszközöket is használhatja, illetve Thirdlight rendelkezést AAD felhasználói fiókok által biztosított API-khoz.

##<a name="assigning-users"></a>Felhasználók hozzárendelése
  
Tesztelje a konfigurációt, meg kell adni az Azure Active Directory-felhasználók, használja az alkalmazás elérési útvonalát társításával engedélyezni szeretné.

###<a name="to-assign-users-to-thirdlight-perform-the-following-steps"></a>Felhasználók hozzárendelése Thirdlight, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon próba-fiók létrehozása.

2.  Kattintson a **Thirdlight **alkalmazás integrációs lap **hozzárendelését a felhasználókhoz**.

    ![Adhatnak a felhasználóknak] (./media/active-directory-saas-thirdlight-tutorial/IC805846.png "Adhatnak a felhasználóknak")

3.  Jelölje ki a próba-felhasználó, kattintson a **hozzárendelni**, és kattintson az **Igen gombra** kattintva erősítse meg a hozzárendelés.

    ![Igen] (./media/active-directory-saas-thirdlight-tutorial/IC767830.png "Igen")
  
Ha meg szeretné tesztelje a egyszeri bejelentkezéses, Access Panel megnyitásához. Ha többet szeretne tudni az Access panelen című témakörben [az Access-panelen](active-directory-saas-access-panel-introduction.md).