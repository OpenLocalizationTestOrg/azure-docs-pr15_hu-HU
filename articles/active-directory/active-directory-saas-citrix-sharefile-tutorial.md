<properties 
    pageTitle="Oktatóprogram: Azure Active Directory-integráció a Citrix ShareFile |} Microsoft Azure" 
    description="Megtudhatja, hogyan használhatja a Citrix ShareFile az Azure Active Directory ahhoz, hogy az egyszeri bejelentkezés, automatikus kiépítési és az egyéb!" 
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

#<a name="tutorial-azure-active-directory-integration-with-citrix-sharefile"></a>Oktatóprogram: Azure Active Directory-integráció a Citrix ShareFile

Ebben az oktatóanyagban célja integrálása az Azure és Citrix ShareFile megjelenítéséhez.  
Az alkalmazási példát, ebben az oktatóanyagban tagolt feltételezi, hogy már rendelkezik az alábbiakat:

-   Érvényes Azure előfizetés
-   Citrix ShareFile bérlői webhelyre

Ebben az oktatóanyagban befejeztével az Azure Active Directory felhasználók Citrix ShareFile rendelt tudnak egyetlen bejelentkezés az alkalmazásba, a Citrix ShareFile vállalati webhely (service provider által kezdeményezett bejelentkezés) és használatáról a [Bevezetés az Access panelre](active-directory-saas-access-panel-introduction.md).

A következő építőelemek az alkalmazási példát, ebben az oktatóanyagban tagolt áll:

1.  Az alkalmazás integrációját Citrix ShareFile engedélyezése
2.  Egyszeri bejelentkezés beállítása
3.  Felhasználói kiépítési konfigurálása
4.  Felhasználók hozzárendelése

![Eset] (./media/active-directory-saas-citrix-sharefile-tutorial/IC773620.png "Eset")
##<a name="enabling-the-application-integration-for-citrix-sharefile"></a>Az alkalmazás integrációját Citrix ShareFile engedélyezése

Ez a szakasz célja, hogyan Citrix ShareFile az alkalmazás alkalmazással való integráció engedélyezése a szerkezet.

###<a name="to-enable-the-application-integration-for-citrix-sharefile-perform-the-following-steps"></a>Ahhoz, hogy az alkalmazás integrációját Citrix ShareFile, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon kattintson a bal oldali navigációs területen kattintson az **Active Directory**.

    ![Az Active Directory] (./media/active-directory-saas-citrix-sharefile-tutorial/IC700993.png "Az Active Directory")

2.  A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3.  Nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson a felső menüben **alkalmazásokat** .

    ![Alkalmazások] (./media/active-directory-saas-citrix-sharefile-tutorial/IC700994.png "Alkalmazások")

4.  Kattintson a **Hozzáadás** az oldal alján.

    ![Alkalmazás hozzáadása] (./media/active-directory-saas-citrix-sharefile-tutorial/IC749321.png "Alkalmazás hozzáadása")

5.  **Milyen feladatot szeretne tenni** párbeszédpanelen kattintson **a gyűjteményből az alkalmazások felvétele**.

    ![Gallerry az alkalmazás hozzáadása] (./media/active-directory-saas-citrix-sharefile-tutorial/IC749322.png "Gallerry az alkalmazás hozzáadása")

6.  A **Keresés mezőbe**írja be a **Citrix ShareFile**.

    ![Alkalmazás gyűjtemény] (./media/active-directory-saas-citrix-sharefile-tutorial/IC773621.png "Alkalmazás gyűjtemény")

7.  Az eredmény ablaktáblában jelölje ki a **Citrix ShareFile**, és válassza a **kész** , az alkalmazás hozzáadása gombra.

    ![Citrix ShareFile] (./media/active-directory-saas-citrix-sharefile-tutorial/IC773622.png "Citrix ShareFile")
##<a name="configuring-single-sign-on"></a>Egyszeri bejelentkezés beállítása

Ez a szakasz célja tagolása engedélyezése a felhasználóknak, hogy Citrix ShareFile hitelesíteni a fiókkal az összevonási alapján a SAML protokoll használatával Azure AD.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Egyszeri bejelentkezés beállításához hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon **Citrix ShareFile** alkalmazás integrációs lapon kattintson **konfigurálása az egyszeri bejelentkezés** a **Konfigurálása az egyszeri bejelentkezés** párbeszédpanel megnyitásához.

    ![Egyszeri bejelentkezés engedélyezése] (./media/active-directory-saas-citrix-sharefile-tutorial/IC773623.png "Egyszeri bejelentkezés engedélyezése")

2.  **Hogyan szeretné, hogy jelentkezzen be az Citrix ShareFile felhasználók** lapon jelölje be **A Microsoft Azure Active Directory Single Sign-On**, és kattintson a **Tovább gombra**.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-citrix-sharefile-tutorial/IC773624.png "Egyszeri bejelentkezés beállítása")

3.  Az **Alkalmazás URL-cím beállítása** lapon **Citrix ShareFile bejelentkezési a URL-cím** mezőbe írja be az URL-t, a következő mintát `https://<tenant-name>.shareFile.com`, majd kattintson a **Tovább**gombra.

    ![Állítsa be az App URL-címe] (./media/active-directory-saas-citrix-sharefile-tutorial/IC773625.png "Állítsa be az App URL-címe")

4.  A **Configure egyszeri bejelentkezés Citrix ShareFile a** lapon a tanúsítvány letöltése, kattintson a **tanúsítvány letöltése**, és mentse a tanúsítvány fájlt a számítógépen parancsra.

    ![ConfigureSingle Sign-On] (./media/active-directory-saas-citrix-sharefile-tutorial/IC773626.png "ConfigureSingle Sign-On")

5.  A különböző webes böngészőablakban jelentkezzen be a **Citrix ShareFile** vállalati webhely rendszergazdaként.

6.  Az eszköztár a képernyő tetején kattintson a **rendszergazda**elemre.

7.  A bal oldali navigációs területen válassza **Konfigurálása Single Sign-On**.

    ![Fiók felügyelete] (./media/active-directory-saas-citrix-sharefile-tutorial/IC773627.png "Fiók felügyelete")

8.  Kattintson a **Single Sign-On / SAML 2.0-s konfigurációs** párbeszédpanelt **Alapvető beállításai**csoportban hajtsa végre az alábbi lépéseket:

    ![Egyszeri bejelentkezés] (./media/active-directory-saas-citrix-sharefile-tutorial/IC773628.png "Egyszeri bejelentkezés")

    1.  Kattintson a **SAML engedélyezése**gombra.
    2.  Az Azure klasszikus portálon **konfigurálása az egyszeri bejelentkezés a Citrix ShareFile** párbeszédpanel lapon a **Szervezet azonosítója** értéket másolja és illessze be azokat a **a IDP kibocsátó / entitás azonosítója** szövegdoboz.
    3.  Az Azure klasszikus portálon **konfigurálása az egyszeri bejelentkezés a Citrix ShareFile** párbeszédpanel lapon a **Távoli bejelentkezési URL-cím** értéket másolja és illessze be a **Bejelentkezési URL-cím** mezőben lévő értéket.
    4.  Az Azure klasszikus portálon lapon **a Configure egyszeri bejelentkezés a Citrix ShareFile** párbeszédpanelen másolja a **Távoli kijelentkezés URL-címe** értéket, és illessze be a **Kijelentkezés URL-címe** mezőben lévő értéket.
    5.  A **X.509-es tanúsítvány** mező mellett kattintson a **Módosítás** gombra, és töltse ki az az Azure Active Directory klasszikus portáljáról letöltött tanúsítványát.
        ![Alapvető beállításai] (./media/active-directory-saas-citrix-sharefile-tutorial/IC773629.png "Alapvető beállításai")

9.  A Citrix ShareFile a felügyeleti portálon kattintson a **Mentés** gombra.

10. Az Azure klasszikus portálon jelölje be a egyszeri bejelentkezéses konfigurációs megerősítés, és válassza a **kész** , **Állítsa be az egyszeri bejelentkezés** párbeszédpanel bezárásához.

    ![Beállítás az egyszeri bejelentkezés] (./media/active-directory-saas-citrix-sharefile-tutorial/IC773630.png "Beállítás az egyszeri bejelentkezés")
##<a name="configuring-user-provisioning"></a>Felhasználói kiépítési konfigurálása

Ahhoz, hogy az Azure Active Directory-felhasználóknak, hogy jelentkezzen be a Citrix ShareFile, hogy ki kell építenie Citrix ShareFile be.  
Citrix ShareFile, ha a feladat manuális kiépítési.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>A felhasználói fiókok kiépítése, hajtsa végre az alábbi lépéseket:

1.  Jelentkezzen be az **Citrix ShareFile** bérlői webhelyen.

2.  Kattintson a **felhasználókezelés \> felhasználók Kezdőlap kezelése \> + alkalmazott létrehozása**.

    ![Alkalmazott létrehozása] (./media/active-directory-saas-citrix-sharefile-tutorial/IC781050.png "Alkalmazott létrehozása")

3.  Adja meg az **e-mailek**, az **Utónév** és **Vezetéknév** egy érvényes Azure AD a fiók kiépítése szeretné.

    ![Az alapvető információk] (./media/active-directory-saas-citrix-sharefile-tutorial/IC799951.png "Az alapvető információk")

4.  Kattintson a **felhasználó hozzáadása**elemre.

    >[AZURE.NOTE] A AAD fióktulajdonos a fog egy e-mailt, és kövesse a előtt aktívvá válik, győződjön meg arról, hogy a fiók mutató hivatkozást.

>[AZURE.NOTE] Bármely más Citrix ShareFile felhasználói fiók létrehozása eszközöket is használhatja, illetve Citrix ShareFile rendelkezést AAD felhasználói fiókok által biztosított API-khoz.

##<a name="assigning-users"></a>Felhasználók hozzárendelése

Tesztelje a konfigurációt, meg kell adni az Azure Active Directory-felhasználók, használja az alkalmazás elérési útvonalát társításával engedélyezni szeretné.

###<a name="to-assign-users-to-citrix-sharefile-perform-the-following-steps"></a>Felhasználók hozzárendelése Citrix ShareFile, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon próba-fiók létrehozása.

2.  Kattintson a **Citrix ShareFile **alkalmazás integrációs lap **hozzárendelését a felhasználókhoz**.

    ![Adhatnak a felhasználóknak] (./media/active-directory-saas-citrix-sharefile-tutorial/IC773631.png "Adhatnak a felhasználóknak")

3.  Jelölje ki a próba-felhasználó, kattintson a **hozzárendelni**, és kattintson az **Igen gombra** kattintva erősítse meg a hozzárendelés.

    ![Igen] (./media/active-directory-saas-citrix-sharefile-tutorial/IC767830.png "Igen")

Ha meg szeretné tesztelje a egyszeri bejelentkezéses, Access Panel megnyitásához. Ha többet szeretne tudni az Access panelen című témakörben [az Access-panelen](active-directory-saas-access-panel-introduction.md).
