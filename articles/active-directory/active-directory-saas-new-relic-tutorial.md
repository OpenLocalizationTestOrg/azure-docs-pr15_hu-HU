<properties 
    pageTitle="Oktatóprogram: Azure Active Directory-integráció a új Relic |} Microsoft Azure" 
    description="Megtudhatja, hogyan használhatja a új Relic az Azure Active Directory ahhoz, hogy az egyszeri bejelentkezés, automatikus kiépítési és az egyéb!" 
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

#<a name="tutorial-azure-active-directory-integration-with-new-relic"></a>Oktatóprogram: Azure Active Directory-integráció a új Relic
  
Ebben az oktatóanyagban célja bemutatják, hogyan állíthatja be az egyszeri bejelentkezés Azure Active Directory és az új Relic között.
  
Az alkalmazási példát, ebben az oktatóanyagban tagolt feltételezi, hogy már rendelkezik az alábbiakat:

-   Érvényes Azure előfizetés
-   Egy új Relic egyszeri bejelentkezés engedélyezve van az előfizetés
  
Ebben az oktatóanyagban befejeztével az Azure Active Directory-felhasználók rendelt új Relic fogja tudni az egyszeri bejelentkezés a AAD Access panelen.

1.  Az alkalmazás új Relic integrációját engedélyezése
2.  Egyszeri bejelentkezés beállítása
3.  Felhasználói kiépítési konfigurálása
4.  Felhasználók hozzárendelése

![Eset] (./media/active-directory-saas-new-relic-tutorial/IC797030.png "Eset")
##<a name="enabling-the-application-integration-for-new-relic"></a>Az alkalmazás új Relic integrációját engedélyezése
  
Ez a szakasz célja, hogyan lehet új Relic az alkalmazás alkalmazással való integráció engedélyezése a szerkezet.

###<a name="to-enable-the-application-integration-for-new-relic-perform-the-following-steps"></a>Ahhoz, hogy az alkalmazás új Relic integrációját, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon kattintson a bal oldali navigációs területen kattintson az **Active Directory**.

    ![Az Active Directory] (./media/active-directory-saas-new-relic-tutorial/IC700993.png "Az Active Directory")

2.  A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3.  Nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson a felső menüben **alkalmazások** .

    ![Alkalmazások] (./media/active-directory-saas-new-relic-tutorial/IC700994.png "Alkalmazások")

4.  Kattintson a **Hozzáadás** az oldal alján.

    ![Alkalmazás hozzáadása] (./media/active-directory-saas-new-relic-tutorial/IC749321.png "Alkalmazás hozzáadása")

5.  **Milyen feladatot szeretne tenni** párbeszédpanelen kattintson **a gyűjteményből az alkalmazások felvétele**.

    ![Gallerry az alkalmazás hozzáadása] (./media/active-directory-saas-new-relic-tutorial/IC749322.png "Gallerry az alkalmazás hozzáadása")

6.  A **Keresés mezőbe**írja be az **Új Relic**.

    ![Alkalmazás gyűjtemény] (./media/active-directory-saas-new-relic-tutorial/IC797031.png "Alkalmazás gyűjtemény")

7.  Az eredmény ablaktáblában jelölje ki az **Új Relic**, és válassza a **kész** , az alkalmazás hozzáadása parancsra.

    ![Új Relic] (./media/active-directory-saas-new-relic-tutorial/IC797032.png "Új Relic")
##<a name="configuring-single-sign-on"></a>Egyszeri bejelentkezés beállítása
  
Ez a szakasz ismerteti, hogyan engedélyezése a felhasználóknak új Relic az Azure Active Directory összevonási alapján a SAML protokoll használata a fiók hitelesítést végezni.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Egyszeri bejelentkezés beállításához hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon **Új Relic** alkalmazás integrációs lapon kattintson **konfigurálása az egyszeri bejelentkezés** a **Konfigurálása az egyszeri bejelentkezés** párbeszédpanel megnyitásához.

    ![Beállítás az egyszeri bejelentkezés] (./media/active-directory-saas-new-relic-tutorial/IC769534.png "Beállítás az egyszeri bejelentkezés")

2.  **Hogyan szeretné, hogy jelentkezzen be az új Relic felhasználók** lapon jelölje be **A Microsoft Azure Active Directory Single Sign-On**, és kattintson a **Tovább gombra**.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-new-relic-tutorial/IC797033.png "Egyszeri bejelentkezés beállítása")

3.  A **App URL-címe beállítása** lapon az **Új Relic bejelentkezési a URL-cím** mezőbe írja be az URL-címet, a felhasználók jelentkezzen be az új Relic alkalmazást használja, és kattintson a **Tovább gombra**. 

    Az alkalmazás URL-cím URL-címet jelenti az új Relic bérlői (pl.: *https://rpm.newrelic.com*):

    ![Állítsa be az App URL-címe] (./media/active-directory-saas-new-relic-tutorial/IC797034.png "Állítsa be az App URL-címe")

4.  **Konfigurálása az egyszeri bejelentkezés új Relic a** lapon a tanúsítvány letöltése, kattintson a **tanúsítvány letöltése**, és mentse a tanúsítványfájl helyileg a számítógépre.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-new-relic-tutorial/IC797035.png "Egyszeri bejelentkezés beállítása")

5.  A különböző webes böngészőablakban jelentkezzen be az **Új Relic** vállalati webhely rendszergazdaként.

6.  A felső sávon kattintson a **Fiókbeállítások menügombra**.

    ![Fiókbeállítások] (./media/active-directory-saas-new-relic-tutorial/IC797036.png "Fiókbeállítások")

7.  Kattintson a **Biztonság és a hitelesítéshez** fülre, és kattintson az **egyszeri bejelentkezés** fülre.

    ![Egyszeri bejelentkezés] (./media/active-directory-saas-new-relic-tutorial/IC797037.png "Egyszeri bejelentkezés")

8.  A SAML párbeszédpanel lapon hajtsa végre az alábbi lépéseket:

    ![SAML] (./media/active-directory-saas-new-relic-tutorial/IC797038.png "SAML")

    1.  **Válassza a fájl** feltöltése a letöltött Azure Active Directory-tanúsítvány parancsára.
    2.  Az Azure klasszikus portálon **konfigurálása az egyszeri bejelentkezés új Relic a** lapon a **Távoli bejelentkezési URL-cím** értéket másolja és illessze be a **távoli bejelentkezési URL-cím** mezőben lévő értéket.
    3.  Az Azure klasszikus portálon **konfigurálása az egyszeri bejelentkezés új Relic a** lapon a **Távoli kijelentkezés URL-címe** értéket másolja és illessze be a **Kijelentkezés jelenjenek meg URL-címe** mezőben lévő értéket.
    4.  Kattintson **a módosítások mentése**gombra.

9.  Az Azure klasszikus portálon jelölje be a egyszeri bejelentkezéses konfigurációs megerősítés, és válassza a **kész** , **Állítsa be az egyszeri bejelentkezés** párbeszédpanel bezárásához.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-new-relic-tutorial/IC797039.png "Egyszeri bejelentkezés beállítása")
##<a name="configuring-user-provisioning"></a>Felhasználói kiépítési konfigurálása
  
Ahhoz, hogy az Azure Active Directory-felhasználók számára, hogy jelentkezzen be az új Relic, hogy ki kell építenie be új Relic.  
Új Relic, ha a feladat manuális kiépítési.

###<a name="to-provision-a-user-account-to-new-relic-perform-the-following-steps"></a>Hozhatók létre új Relic egy felhasználói fiókot, hajtsa végre az alábbi lépéseket:

1.  Jelentkezzen be rendszergazdaként az **Új Relic** vállalati webhely.

2.  A felső sávon kattintson a **Fiókbeállítások menügombra**.

    ![Fiókbeállítások] (./media/active-directory-saas-new-relic-tutorial/IC797040.png "Fiókbeállítások")

3.  A **fiók** ablakban, a bal oldalon kattintson az **összefoglaló**elemre, és válassza a **felhasználó hozzáadása**.

    ![Fiókbeállítások] (./media/active-directory-saas-new-relic-tutorial/IC797041.png "Fiókbeállítások")

4.  Az **aktív felhasználók** párbeszédpanelen hajtsa végre az alábbi lépéseket:

    ![Az aktív felhasználók] (./media/active-directory-saas-new-relic-tutorial/IC797042.png "Az aktív felhasználók")

    1.  Az **E-mail** mezőbe írja be a kívánt kiépítése érvényes Azure Active Directory-felhasználó e-mail címét.
    2.  Jelölje ki a **felhasználói** **szerepkör** .
    3.  Kattintson **a felhasználó hozzáadása**elemre.

>[AZURE.NOTE]Bármely más Relic új felhasználói fiók létrehozása eszközöket is használhatja, vagy új Relic rendelkezést AAD felhasználói fiókok által biztosított API-khoz.

##<a name="assigning-users"></a>Felhasználók hozzárendelése
  
Tesztelje a konfigurációt, meg kell adni az Azure Active Directory-felhasználók, használja az alkalmazás elérési útvonalát társításával engedélyezni szeretné.

###<a name="to-assign-users-to-new-relic-perform-the-following-steps"></a>Új Relic szeretne hozzárendelni a felhasználók, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon próba-fiók létrehozása.

2.  Az **Új Relic** alkalmazás integrációs lapon kattintson a **hozzárendelését a felhasználókhoz**.

    ![Hozzárendelését a felhasználókhoz] (./media/active-directory-saas-new-relic-tutorial/IC797043.png "Adhatnak a felhasználóknak")

3.  Jelölje ki a próba-felhasználó, kattintson a **hozzárendelni**, és kattintson az **Igen gombra** kattintva erősítse meg a hozzárendelés.

    ![Igen] (./media/active-directory-saas-new-relic-tutorial/IC767830.png "Igen")
  
Ha meg szeretné tesztelje a egyszeri bejelentkezéses, Access Panel megnyitásához. Az Access ablaktábla, lásd: [Bevezetés az Access panelre](active-directory-saas-access-panel-introduction.md).




