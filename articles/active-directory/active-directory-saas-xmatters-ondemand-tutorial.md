<properties 
    pageTitle="Oktatóprogram: Azure Active Directory-integráció a xMatters OnDemand |} Microsoft Azure"
    description="Megtudhatja, hogy miként xMatters OnDemand használata az Azure Active Directory ahhoz, hogy az egyszeri bejelentkezés, automatikus kiépítési és az egyéb!" 
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
    ms.date="09/09/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-xmatters-ondemand"></a>Oktatóprogram: Azure Active Directory-integráció a xMatters OnDemand
  
Ebben az oktatóanyagban célja Azure és xMatters OnDemand integrációja megjelenítése. Az alkalmazási példát, ebben az oktatóanyagban tagolt feltételezi, hogy már rendelkezik az alábbiakat:

-   Érvényes Azure előfizetés
-   XMatters OnDemand bérlői webhelyre
  
Ebben az oktatóanyagban befejeztével az Azure Active Directory felhasználók xMatters OnDemand rendelt tudnak egyetlen bejelentkezés az alkalmazásba, a xMatters OnDemand vállalati webhely (service provider által kezdeményezett bejelentkezés) és használatáról a [Bevezetés az Access panelen](active-directory-saas-access-panel-introduction.md).
  
A következő építőelemek az alkalmazási példát, ebben az oktatóanyagban tagolt áll:

1.  Az alkalmazás integrációját xMatters OnDemand engedélyezése
2.  Egyszeri bejelentkezés beállítása
3.  Felhasználói kiépítési konfigurálása
4.  Felhasználók hozzárendelése

![Eset] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC776788.png "Eset")

##<a name="enabling-the-application-integration-for-xmatters-ondemand"></a>Az alkalmazás integrációját xMatters OnDemand engedélyezése
  
Ez a szakasz célja, hogyan xMatters OnDemand az alkalmazás alkalmazással való integráció engedélyezése a szerkezet.

###<a name="to-enable-the-application-integration-for-xmatters-ondemand-perform-the-following-steps"></a>Ahhoz, hogy az alkalmazás integrációját xMatters OnDemand, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon kattintson a bal oldali navigációs területen kattintson az **Active Directory**.

    ![Az Active Directory] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC700993.png "Az Active Directory")

2.  A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3.  Nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson a felső menüben **alkalmazásokat** .

    ![Alkalmazások] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC700994.png "Alkalmazások")

4.  Kattintson a **Hozzáadás** az oldal alján.

    ![Alkalmazás hozzáadása] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC749321.png "Alkalmazás hozzáadása")

5.  **Milyen feladatot szeretne tenni** párbeszédpanelen kattintson **a gyűjteményből az alkalmazások felvétele**.

    ![Gallerry az alkalmazás hozzáadása] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC749322.png "Gallerry az alkalmazás hozzáadása")

6.  A **Keresés mezőbe**írja be a **xMatters OnDemand**.

    ![Alkalmazás gyűjtemény] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC776789.png "Alkalmazás gyűjtemény")

7.  Az eredmény ablaktáblában jelölje ki a **XMatters OnDemand**, és válassza a **kész** , az alkalmazás hozzáadása gombra.

    ![xMatters OnDemand] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC776790.png "xMatters OnDemand")

##<a name="configuring-single-sign-on"></a>Egyszeri bejelentkezés beállítása
  
Ez a szakasz célja tagolása engedélyezése a felhasználóknak, hogy XMatters OnDemand hitelesíteni a fiókkal az összevonási alapján a SAML protokoll használatával Azure AD.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Egyszeri bejelentkezés beállításához hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon **XMatters OnDemand** alkalmazás integrációs lapon kattintson **konfigurálása az egyszeri bejelentkezés** a **Konfigurálása az egyszeri bejelentkezés** párbeszédpanel megnyitásához.

    ![Beállítás az egyszeri bejelentkezés] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC776791.png "Beállítás az egyszeri bejelentkezés")

2.  **Hogyan szeretné, hogy jelentkezzen be az XMatters OnDemand felhasználók** lapon jelölje be **A Microsoft Azure Active Directory Single Sign-On**, és kattintson a **Tovább gombra**.

    ![Beállítás az egyszeri bejelentkezés] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC776792.png "Beállítás az egyszeri bejelentkezés")

3.  Az **Alkalmazás URL-cím beállítása** lapon hajtsa végre az alábbi lépéseket:

    ![Állítsa be az app URL-címe] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC776793.png "Állítsa be az app URL-címe")

    egy. Az **XMatters OnDemand bejelentkezési az URL-cím** mezőbe írja be az URL-CÍMÉT az alábbi minta használatával:`https://<tenant-name>.XMattersOnDemandapp.com`

    b. Kattintson a **Tovább**gombra.


4.  **Konfigurálása az egyszeri bejelentkezés XMatters OnDemand a** lapon a tanúsítvány letöltése, kattintson a **tanúsítvány letöltése**, és mentse a tanúsítvány fájlként helyileg **c:\\XMatters OnDemand.cer**.

    >[AZURE.IMPORTANT] Kell továbbítani a tanúsítvány a xMatters támogatási csoportnak. A tanúsítvány kell a xMatters támogatási csoport által fel kell tölteni a egyszeri bejelentkezéses konfiguráció is véglegesítése előtt.

    ![Bejelentkezés egyszeri konfigurálása] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC776794.png "Bejelentkezés egyszeri konfigurálása")

5.  A különböző webes böngészőablakban jelentkezzen be a XMatters OnDemand vállalati webhely rendszergazdaként.

6.  Kattintson az eszköztáron a képernyő tetején kattintson a **rendszergazda**, és a bal oldali navigációs sávon kattintson a **Vállalat részletek** .

    ![Rendszergazda] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC776795.png "Rendszergazda")

7.  A **SAML beállítása** lapon hajtsa végre az alábbi lépéseket:

    ![SAML konfigurálása] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC776796.png "SAML konfigurálása")

    1.  Jelölje ki a **SAML engedélyezése**.
    2.  Az Azure klasszikus portálon **konfigurálása az egyszeri bejelentkezés a XMatters OnDemand** párbeszédpanel lapon másolja az **Identitás-szolgáltató azonosítója** értéket, és illessze be a **Identitás szolgáltató azonosítója** mezőben lévő értéket.
    3.  Az Azure klasszikus portálon **konfigurálása az egyszeri bejelentkezés a XMatters OnDemand** párbeszédpanel lapon az **Egyszeri bejelentkezéses szolgáltatás URL-címe** értéket másolja és illessze be a **Egyszeri bejelentkezési a URL-címe** mezőben lévő értéket.
    4.  Az Azure klasszikus portálon **konfigurálása az egyszeri bejelentkezés a XMatters OnDemand** párbeszédpanel lapon az **Egyetlen Sign-Out szolgáltatás URL-címe** értéket másolja és illessze be a **Egyetlen kijelentkezés URL-címe** mezőben lévő értéket.
    5.  A vállalat részletei lapon, a képernyő tetején a **Módosítások mentése**gombra.
        ![Vállalati részletei] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC776797.png "Vállalati részletei")

8.  Az Azure klasszikus portálon jelölje be a egyszeri bejelentkezéses konfigurációs megerősítés, és válassza a **kész** , **Állítsa be az egyszeri bejelentkezés** párbeszédpanel bezárásához.

    ![Bejelentkezés egyszeri konfigurálása] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC776798.png "Bejelentkezés egyszeri konfigurálása")

##<a name="configuring-user-provisioning"></a>Felhasználói kiépítési konfigurálása
  
Ahhoz, hogy az Azure Active Directory-felhasználóknak, hogy jelentkezzen be a XMatters OnDemand, hogy ki kell építenie XMatters OnDemand be.  
XMatters OnDemand, ha a feladat manuális kiépítési.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>A felhasználói fiókok kiépítése, hajtsa végre az alábbi lépéseket:

1.  Jelentkezzen be az **XMatters OnDemand** bérlői webhelyen.

2.  Kattintson a **felhasználók** fülre.

3.  Kattintson a **felhasználó hozzáadása**elemre.

    ![Felhasználók] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC781048.png "Felhasználók")

4.  Válassza az **aktív**.

5.  A **felhasználó hozzáadása** csoportban hajtsa végre az alábbi lépéseket:

    ![Felhasználó hozzáadása] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC781049.png "Felhasználó hozzáadása")

    1.  Adja meg a **felhasználóazonosító**, **utónevet**, **vezetéknevét**, **webhely** kiépítése kívánt érvényes AAD fiók.
    2.  Kattintson a **Mentés**gombra.

>[AZURE.NOTE] Bármely más XMatters OnDemand felhasználói fiók létrehozása eszközöket is használhatja, illetve XMatters OnDemand rendelkezést AAD felhasználói fiókok által biztosított API-khoz.

##<a name="assigning-users"></a>Felhasználók hozzárendelése
  
Tesztelje a konfigurációt, meg kell adni az Azure Active Directory-felhasználók, használja az alkalmazás elérési útvonalát társításával engedélyezni szeretné.

###<a name="to-assign-users-to-xmatters-ondemand-perform-the-following-steps"></a>Felhasználók hozzárendelése XMatters OnDemand, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon próba-fiók létrehozása.

2.  Kattintson a **XMatters OnDemand **alkalmazás integrációs lap **hozzárendelését a felhasználókhoz**.

    ![Adhatnak a felhasználóknak] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC776799.png "Adhatnak a felhasználóknak")

3.  Jelölje ki a próba-felhasználó, kattintson a **hozzárendelni**, és kattintson az **Igen gombra** kattintva erősítse meg a hozzárendelés.

    ![Igen] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC767830.png "Igen")
  
Ha meg szeretné tesztelje a egyszeri bejelentkezéses, Access Panel megnyitásához. Ha többet szeretne tudni az Access panelen című témakörben [az Access-panelen](active-directory-saas-access-panel-introduction.md).