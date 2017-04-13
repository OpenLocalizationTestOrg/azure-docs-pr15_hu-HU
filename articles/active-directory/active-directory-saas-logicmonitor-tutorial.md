<properties 
    pageTitle="Oktatóprogram: Azure Active Directory-integráció a LogicMonitor |} Microsoft Azure" 
    description="Megtudhatja, hogyan használhatja a LogicMonitor az Azure Active Directory ahhoz, hogy az egyszeri bejelentkezés, automatikus kiépítési és az egyéb!" 
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

#<a name="tutorial-azure-active-directory-integration-with-logicmonitor"></a>Oktatóprogram: Azure Active Directory-integráció a LogicMonitor
  
Ebben az oktatóanyagban célja integrálása az Azure és LogicMonitor megjelenítéséhez.  
Az alkalmazási példát, ebben az oktatóanyagban tagolt feltételezi, hogy már rendelkezik az alábbiakat:

-   Érvényes Azure előfizetés
-   LogicMonitor bérlői webhelyre
  
A következő építőelemek az alkalmazási példát, ebben az oktatóanyagban tagolt áll:

1.  Az alkalmazás integrációját LogicMonitor engedélyezése
2.  Egyszeri bejelentkezés beállítása
3.  Felhasználói kiépítési konfigurálása
4.  Felhasználók hozzárendelése

![Eset] (./media/active-directory-saas-logicmonitor-tutorial/IC790045.png "Eset")
##<a name="enabling-the-application-integration-for-logicmonitor"></a>Az alkalmazás integrációját LogicMonitor engedélyezése
  
Ez a szakasz célja, hogyan LogicMonitor az alkalmazás alkalmazással való integráció engedélyezése a szerkezet.

###<a name="to-enable-the-application-integration-for-logicmonitor-perform-the-following-steps"></a>Ahhoz, hogy az alkalmazás integrációját LogicMonitor, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon kattintson a bal oldali navigációs területen kattintson az **Active Directory**.

    ![Az Active Directory] (./media/active-directory-saas-logicmonitor-tutorial/IC700993.png "Az Active Directory")

2.  A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3.  Nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson a felső menüben **alkalmazásokat** .

    ![Alkalmazások] (./media/active-directory-saas-logicmonitor-tutorial/IC700994.png "Alkalmazások")

4.  Kattintson a **Hozzáadás** az oldal alján.

    ![Alkalmazás hozzáadása] (./media/active-directory-saas-logicmonitor-tutorial/IC749321.png "Alkalmazás hozzáadása")

5.  **Milyen feladatot szeretne tenni** párbeszédpanelen kattintson **a gyűjteményből az alkalmazások felvétele**.

    ![Gallerry az alkalmazás hozzáadása] (./media/active-directory-saas-logicmonitor-tutorial/IC749322.png "Gallerry az alkalmazás hozzáadása")

6.  A **Keresés mezőbe**írja be a **logicmonitor**.

    ![Alkalmazás gyűjtemény] (./media/active-directory-saas-logicmonitor-tutorial/IC790046.png "Alkalmazás gyűjtemény")

7.  Az eredmény ablaktáblában jelölje ki a **LogicMonitor**, és válassza a **kész** , az alkalmazás hozzáadása gombra.

    ![LogicMonitor] (./media/active-directory-saas-logicmonitor-tutorial/IC790047.png "LogicMonitor")
##<a name="configuring-single-sign-on"></a>Egyszeri bejelentkezés beállítása
  
Ez a szakasz célja tagolása engedélyezése a felhasználóknak, hogy LogicMonitor hitelesíteni a fiókkal az összevonási alapján a SAML protokoll használatával Azure AD.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Egyszeri bejelentkezés beállításához hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon **LogicMonitor **alkalmazás integrációs lapon kattintson **konfigurálása az egyszeri bejelentkezés** a **Konfigurálása az egyszeri bejelentkezés** párbeszédpanel megnyitásához.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-logicmonitor-tutorial/IC790048.png "Egyszeri bejelentkezés beállítása")

2.  **Hogyan szeretné, hogy jelentkezzen be az LogicMonitor felhasználók** lapon jelölje be **A Microsoft Azure Active Directory Single Sign-On**, és kattintson a **Tovább gombra**.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-logicmonitor-tutorial/IC790049.png "Egyszeri bejelentkezés beállítása")

3.  Az **App URL-címe konfigurálása** lapon **Bejelentkezési a URL-cím** mezőbe írja be a jelentkezzen be az LogicMonitor a felhasználók által használt URL-cím \(e, g,: "*http://company.logicmonitor.com*"\), majd kattintson a **Tovább**gombra.

    ![Állítsa be az App URL-címe] (./media/active-directory-saas-logicmonitor-tutorial/IC790050.png "Állítsa be az App URL-címe")

4.  **Konfigurálása az egyszeri bejelentkezés LogicMonitor a** lapon kattintson a **metaadat-alapú letöltése**elemre, és mentse a számítógépre.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-logicmonitor-tutorial/IC790051.png "Egyszeri bejelentkezés beállítása")

5.  Jelentkezzen be rendszergazdaként a **LogicMonitor** vállalati webhely.

6.  A felső sávon kattintson a **Beállítások**gombra.

    ![Beállítások] (./media/active-directory-saas-logicmonitor-tutorial/IC790052.png "Beállítások")

7.  Kattintson a bal oldali navigációs Bát, **Egyszeri bejelentkezés**

    ![Egyszeri bejelentkezés] (./media/active-directory-saas-logicmonitor-tutorial/IC790053.png "Egyszeri bejelentkezés")

8.  **Egyszeri bejelentkezés (SSO) beállításai** csoportban hajtsa végre az alábbi lépéseket:

    ![Egyszeri bejelentkezés beállításai] (./media/active-directory-saas-logicmonitor-tutorial/IC790054.png "Egyszeri bejelentkezés beállításai")

    1.  Jelölje be **az egyszeri bejelentkezés engedélyezése**.
    2.  **Szerepkör-hozzárendelés alapértelmezett**jelölje ki a **csak olvasható**.
    3.  Nyissa meg a letöltött metaadat-fájlt a Jegyzettömbben, és kattintson a fájl tartalmát illessze be a **Szolgáltató metaadat-alapú személyes** mezőben lévő értéket.
    4.  A **módosítások mentése**gombra.

9.  Az Azure klasszikus portálon jelölje be a egyszeri bejelentkezéses konfigurációs megerősítés, és válassza a **kész** , **Állítsa be az egyszeri bejelentkezés** párbeszédpanel bezárásához.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-logicmonitor-tutorial/IC790055.png "Egyszeri bejelentkezés beállítása")
##<a name="configuring-user-provisioning"></a>Felhasználói kiépítési konfigurálása
  
AAD felhasználóknak engedélyezni szeretné, hogy jelentkezzen be azok ki kell építenie Azure Active Directory-felhasználó nevük LogicMonitor alkalmazást.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Felhasználói kiépítési beállításához hajtsa végre az alábbi lépéseket:

1.  Jelentkezzen be rendszergazdaként a LogicMonitor vállalati webhely.

2.  A felső sávon kattintson a **Beállítások**gombra, és kattintson a **szerepkörök és a felhasználók**.

    ![Szerepkörök és a felhasználók] (./media/active-directory-saas-logicmonitor-tutorial/IC790056.png "Szerepkörök és a felhasználók")

3.  Kattintson a **hozzáadása**gombra.

4.  A **fiók hozzáadása** csoportban hajtsa végre az alábbi lépéseket:

    ![Fiók hozzáadása] (./media/active-directory-saas-logicmonitor-tutorial/IC790057.png "Fiók hozzáadása")

    1.  Írja be a **felhasználónevét**, **e-mailek**, **jelszót** , és **Írja be újra a jelszót** az Azure Active Directory-felhasználó kiépítése a kapcsolódó szövegdobozok be a kívánt értékeket.
    2.  Jelölje ki a **szerepkörök**, **engedélyek megtekintése** és az **állapot**.
    3.  Kattintson a **Küldés**gombra.

>[AZURE.NOTE]Bármely más LogicMonitor felhasználói fiók létrehozása eszközöket is használhatja, illetve API-k által biztosított LogicMonitor kiépítése Azure Active Directory felhasználói fiókok.

##<a name="assigning-users"></a>Felhasználók hozzárendelése
  
Tesztelje a konfigurációt, meg kell adni az Azure Active Directory-felhasználók, használja az alkalmazás elérési útvonalát társításával engedélyezni szeretné.

###<a name="to-assign-users-to-logicmonitor-perform-the-following-steps"></a>Felhasználók hozzárendelése LogicMonitor, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon próba-fiók létrehozása.

2.  Kattintson a **LogicMonitor** alkalmazás integrációs lap **hozzárendelését a felhasználókhoz**.

    ![Adhatnak a felhasználóknak] (./media/active-directory-saas-logicmonitor-tutorial/IC790058.png "Adhatnak a felhasználóknak")

3.  Jelölje ki a próba-felhasználó, kattintson a **hozzárendelni**, és kattintson az **Igen gombra** kattintva erősítse meg a hozzárendelés.

    ![Igen] (./media/active-directory-saas-logicmonitor-tutorial/IC767830.png "Igen")
  
Ha meg szeretné tesztelje a egyszeri bejelentkezéses, Access Panel megnyitásához. Ha többet szeretne tudni az Access panelen című témakörben [az Access-panelen](active-directory-saas-access-panel-introduction.md).




