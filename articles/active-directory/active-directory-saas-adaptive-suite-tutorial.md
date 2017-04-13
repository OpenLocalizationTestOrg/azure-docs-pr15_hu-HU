<properties 
    pageTitle="Oktatóprogram: Azure Active Directory-integráció a adaptív csomagja |} Microsoft Azure"
    description="Megtudhatja, hogy miként adaptív csomagja használata az Azure Active Directory ahhoz, hogy az egyszeri bejelentkezés, automatikus kiépítési és az egyéb!" 
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

#<a name="tutorial-azure-active-directory-integration-with-adaptive-suite"></a>Oktatóprogram: Azure Active Directory-integráció a adaptív programcsomagban

Ebben az oktatóanyagban célja Azure és adaptív Suite integrációja megjelenítéséhez.  
Az alkalmazási példát, ebben az oktatóanyagban tagolt feltételezi, hogy már rendelkezik az alábbiakat:

-   Érvényes Azure előfizetés
-   Egy adaptív csomagja bérlői

Ebben az oktatóanyagban befejeztével az Azure Active Directory felhasználók adaptív csomagja rendelt tudnak egyetlen bejelentkezés az alkalmazásba, a adaptív csomagja vállalati webhely (service provider által kezdeményezett bejelentkezés) és használatáról a [Bevezetés az Access panelen](active-directory-saas-access-panel-introduction.md).

A következő építőelemek az alkalmazási példát, ebben az oktatóanyagban tagolt áll:

1.  Az alkalmazás integrálását adaptív csomagja engedélyezése
2.  Egyszeri bejelentkezés beállítása
3.  Felhasználói kiépítési konfigurálása
4.  Felhasználók hozzárendelése

![Eset] (./media/active-directory-saas-adaptive-suite-tutorial/IC805637.png "Eset")
##<a name="enabling-the-application-integration-for-adaptive-suite"></a>Az alkalmazás integrálását adaptív csomagja engedélyezése

Ez a szakasz célja, hogyan adaptív csomagja alkalmazás alkalmazással való integráció engedélyezése a szerkezet.

###<a name="to-enable-the-application-integration-for-adaptive-suite-perform-the-following-steps"></a>Ahhoz, hogy az alkalmazás integrálását adaptív csomagja, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon kattintson a bal oldali navigációs területen kattintson az **Active Directory**.

    ![Az Active Directory] (./media/active-directory-saas-adaptive-suite-tutorial/IC700993.png "Az Active Directory")

2.  A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3.  Nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson a felső menüben **alkalmazásokat** .

    ![Alkalmazások] (./media/active-directory-saas-adaptive-suite-tutorial/IC700994.png "Alkalmazások")

4.  Kattintson a **Hozzáadás** az oldal alján.

    ![Alkalmazás hozzáadása] (./media/active-directory-saas-adaptive-suite-tutorial/IC749321.png "Alkalmazás hozzáadása")

5.  **Milyen feladatot szeretne tenni** párbeszédpanelen kattintson **a gyűjteményből az alkalmazások felvétele**.

    ![Gallerry az alkalmazás hozzáadása] (./media/active-directory-saas-adaptive-suite-tutorial/IC749322.png "Gallerry az alkalmazás hozzáadása")

6.  A **Keresés mezőbe**írja be a **Adaptív csomagja**.

    ![Alkalmazás gyűjtemény] (./media/active-directory-saas-adaptive-suite-tutorial/IC805638.png "Alkalmazás gyűjtemény")

7.  Az eredmény ablaktáblában jelölje ki a **Adaptív csomagja**, és válassza a **kész** , az alkalmazás hozzáadása gombra.

    ![Adaptív programcsomagban] (./media/active-directory-saas-adaptive-suite-tutorial/IC805639.png "Adaptív programcsomagban")
##<a name="configuring-single-sign-on"></a>Egyszeri bejelentkezés beállítása

Ez a szakasz célja engedélyezése a felhasználóknak, hogy az összevonási alapján a SAML protokoll használatával Azure Active Directory fiókjukat hitelesíteni a adaptív programcsomagban tagolása.  
Egyszeri bejelentkezés adaptív csomagja konfigurálása megköveteli ujjlenyomat érték beolvasása egy tanúsítványt.  
Ha nem ismeri a ezt az eljárást, megtudhatja, [hogy miként beolvasni a tanúsítvány ujjlenyomat értékét](http://youtu.be/YKQF266SAxI).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Egyszeri bejelentkezés beállításához hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon **Adaptív csomagja** alkalmazás integrációs lapon kattintson **konfigurálása az egyszeri bejelentkezés** a **Konfigurálása az egyszeri bejelentkezés** párbeszédpanel megnyitásához.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-adaptive-suite-tutorial/IC805640.png "Egyszeri bejelentkezés beállítása")

2.  **Hogyan szeretné, hogy jelentkezzen be az adaptív csomagja felhasználók** lapon jelölje be **A Microsoft Azure Active Directory Single Sign-On**, és kattintson a **Tovább gombra**.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-adaptive-suite-tutorial/IC805641.png "Egyszeri bejelentkezés beállítása")

3.  Az **Alkalmazás-beállítások megadása** lap **Válasz URL-cím** mezőbe írja be az URL-t, a következő mintát "*https://login.adaptiveinsights.com:443/samlsso/RlJFRVRSSUFMMTI3MTE =*", majd kattintson a **Tovább**gombra.

    >[AZURE.NOTE] Ez az érték elérheti az adaptív csomagot **SAML SSO beállításai** lapon.

    ![Alkalmazás beállításainak konfigurálása] (./media/active-directory-saas-adaptive-suite-tutorial/IC805642.png "Alkalmazás beállításainak konfigurálása")

4.  **Beállítás az egyszeri bejelentkezés adaptív csomagja a** lapon a tanúsítvány letöltése, kattintson a **tanúsítvány letöltése**, és mentse a tanúsítvány fájlt a számítógépen helyben parancsra.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-adaptive-suite-tutorial/IC805643.png "Egyszeri bejelentkezés beállítása")

5.  A különböző webes böngészőablakban jelentkezzen be a adaptív csomagja vállalati webhely rendszergazdaként.

6.  Kattintson a **rendszergazda**.

    ![Rendszergazda] (./media/active-directory-saas-adaptive-suite-tutorial/IC805644.png "Rendszergazda")

7.  A **felhasználók és szerepkörök** csoportban kattintson a **SAML-féle egyszeri Bejelentkezést beállításainak kezelése**elemre.

    ![SAML-féle egyszeri Bejelentkezést beállításainak kezelése] (./media/active-directory-saas-adaptive-suite-tutorial/IC805645.png "SAML-féle egyszeri Bejelentkezést beállításainak kezelése")

8.  A **SAML-féle egyszeri Bejelentkezést beállításai** lapon hajtsa végre az alábbi lépéseket:

    ![SAML-féle egyszeri Bejelentkezést beállításai] (./media/active-directory-saas-adaptive-suite-tutorial/IC805646.png "SAML-féle egyszeri Bejelentkezést beállításai")

    1.  Az **identitás-szolgáltató neve** mezőbe írja be a konfigurációban nevét.
    2.  Az Azure klasszikus portálon **konfigurálása az egyszeri bejelentkezés a adaptív csomagja** párbeszédpanel lapon a **Szervezet azonosítója** értéket másolja és illessze be a **szervezet azonosítója identitásszolgáltató** mezőben lévő értéket.
    3.  Az Azure klasszikus portálon **konfigurálása az egyszeri bejelentkezés a adaptív csomagja** párbeszédpanel lapon a **SAML egyszeri bejelentkezési URL-címe** értéket másolja és illessze be a **identitásszolgáltató egyszeri bejelentkezési URL-címe** mezőben lévő értéket.
    4.  Az Azure klasszikus portálon **konfigurálása az egyszeri bejelentkezés a adaptív csomagja** párbeszédpanel lapon a **SAML egyszeri bejelentkezési URL-címe** értéket másolja és illessze be a **egyéni kijelentkezés URL-címe** mezőben lévő értéket.
    5.  A letöltött tanúsítvány feltölteni, kattintson a **fájl kiválasztása**.
    6.  **SAML felhasználói azonosító**jelölje be a **felhasználó adaptív háttérismeretek nevét**.
    7.  **SAML felhasználói azonosító helyét**jelölje ki a **felhasználói azonosító NameID a Tárgy mezőben**.
    8.  **SAML NameID formátumban**jelölje be az **E-mail címét**.
    9.  **SAML engedélyezése**jelölje ki a **SAML SSO engedélyezése és közvetlen adaptív háttérismeretek jelentkezzen be**.
    10. Kattintson a **Mentés**gombra.

9.  Az Azure klasszikus portálon jelölje be a egyszeri bejelentkezéses konfigurációs megerősítés, és válassza a **kész** , **Állítsa be az egyszeri bejelentkezés** párbeszédpanel bezárásához.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-adaptive-suite-tutorial/IC805647.png "Egyszeri bejelentkezés beállítása")
##<a name="configuring-user-provisioning"></a>Felhasználói kiépítési konfigurálása

Ahhoz, hogy az Azure Active Directory-felhasználóknak, hogy jelentkezzen be az adaptív csomagja, hogy ki kell építenie adaptív csomagja be.  
Adaptív csomagot, ha a feladat manuális kiépítési.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Felhasználói kiépítési beállításához hajtsa végre az alábbi lépéseket:

1.  Jelentkezzen be rendszergazdaként a **Adaptív csomagja** vállalati webhely.

2.  Kattintson a **rendszergazda**.

    ![Rendszergazda] (./media/active-directory-saas-adaptive-suite-tutorial/IC805644.png "Rendszergazda")

3.  A **felhasználók és szerepkörök** csoportjában a **Felhasználó hozzáadása**elemre.

    ![Felhasználó hozzáadása] (./media/active-directory-saas-adaptive-suite-tutorial/IC805648.png "Felhasználó hozzáadása")

4.  **Új felhasználói** csoportban hajtsa végre az alábbi lépéseket:

    ![Elküldése] (./media/active-directory-saas-adaptive-suite-tutorial/IC805649.png "Elküldése")

    1.  Írja be a **nevét**, **Login**, **e-mailben**, egy érvényes Azure Active Directory-felhasználó kiépítése a kapcsolódó szövegdobozok be a kívánt **jelszót** .
    2.  Válasszon egy **szerepkört**.
    3.  Kattintson a **Küldés**gombra.

>[AZURE.NOTE] Bármely más adaptív csomagja felhasználói fiók létrehozása eszközöket is használhatja, illetve adaptív csomagja rendelkezést AAD felhasználói fiókok által biztosított API-khoz.

##<a name="assigning-users"></a>Felhasználók hozzárendelése

Tesztelje a konfigurációt, meg kell adni az Azure Active Directory-felhasználók, használja az alkalmazás elérési útvonalát társításával engedélyezni szeretné.

###<a name="to-assign-users-to-adaptive-suite-perform-the-following-steps"></a>Felhasználók hozzárendelése adaptív csomagja, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon próba-fiók létrehozása.

2.  A **Adaptív csomagja **alkalmazás integrációs lapon kattintson a **hozzárendelését a felhasználókhoz**.

    ![Adhatnak a felhasználóknak] (./media/active-directory-saas-adaptive-suite-tutorial/IC805650.png "Adhatnak a felhasználóknak")

3.  Jelölje ki a próba-felhasználó, kattintson a **hozzárendelni**, és kattintson az **Igen gombra** kattintva erősítse meg a hozzárendelés.

    ![Igen] (./media/active-directory-saas-adaptive-suite-tutorial/IC767830.png "Igen")

Ha meg szeretné tesztelje a egyszeri bejelentkezéses, Access Panel megnyitásához. Ha többet szeretne tudni az Access panelen című témakörben [az Access-panelen](active-directory-saas-access-panel-introduction.md).
