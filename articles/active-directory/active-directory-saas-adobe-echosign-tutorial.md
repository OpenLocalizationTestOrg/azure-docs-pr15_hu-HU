<properties 
    pageTitle="Oktatóprogram: Azure Active Directory-integráció a Adobe EchoSign |} Microsoft Azure" 
    description="Megtudhatja, hogyan használhatja a Adobe EchoSign az Azure Active Directory ahhoz, hogy az egyszeri bejelentkezés, automatikus kiépítési és az egyéb!" 
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

#<a name="tutorial-azure-active-directory-integration-with-adobe-echosign"></a>Oktatóprogram: Azure Active Directory-integráció a Adobe EchoSign

Ebben az oktatóanyagban célja kattintva jelenítse meg Adobe EchoSign és Azure integrációja.  
Az alkalmazási példát, ebben az oktatóanyagban tagolt feltételezi, hogy már rendelkezik az alábbiakat:

-   Érvényes Azure előfizetés
-   Egy egyetlen Adobe EchoSign jelentkezzen be a engedélyezett előfizetés

Ebben az oktatóanyagban befejeztével az Azure Active Directory-felhasználók meg Adobe EchoSign rendelt fogja tudni egyetlen jelentkezzen be az Adobe EchoSign vállalat webhelye (service provider által kezdeményezett bejelentkezés) és használatáról a [Bevezetés az Access panelen](active-directory-saas-access-panel-introduction.md)az alkalmazást.

A következő építőelemek az alkalmazási példát, ebben az oktatóanyagban tagolt áll:

1.  Az alkalmazás integrációját Adobe EchoSign engedélyezése
2.  Egyszeri bejelentkezés beállítása
3.  Felhasználói kiépítési konfigurálása
4.  Felhasználók hozzárendelése

![Eset] (./media/active-directory-saas-adobe-echosign-tutorial/IC789511.png "Eset")
##<a name="enabling-the-application-integration-for-adobe-echosign"></a>Az alkalmazás integrációját Adobe EchoSign engedélyezése

Ez a szakasz célja, hogyan Adobe EchoSign az alkalmazás alkalmazással való integráció engedélyezése a szerkezet.

###<a name="to-enable-the-application-integration-for-adobe-echosign-perform-the-following-steps"></a>Ahhoz, hogy az alkalmazás integrációját Adobe EchoSign, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon kattintson a bal oldali navigációs területen kattintson az **Active Directory**.

    ![Az Active Directory] (./media/active-directory-saas-adobe-echosign-tutorial/IC700993.png "Az Active Directory")

2.  A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3.  Nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson a felső menüben **alkalmazások** .

    ![Alkalmazások] (./media/active-directory-saas-adobe-echosign-tutorial/IC700994.png "Alkalmazások")

4.  Kattintson a **Hozzáadás** az oldal alján.

    ![Alkalmazás hozzáadása] (./media/active-directory-saas-adobe-echosign-tutorial/IC749321.png "Alkalmazás hozzáadása")

5.  **Milyen feladatot szeretne tenni** párbeszédpanelen kattintson **a gyűjteményből az alkalmazások felvétele**.

    ![Gallerry az alkalmazás hozzáadása] (./media/active-directory-saas-adobe-echosign-tutorial/IC749322.png "Gallerry az alkalmazás hozzáadása")

6.  A **Keresés mezőbe**írja be az **Adobe EchoSign**.

    ![Alkalmazás gyűjtemény] (./media/active-directory-saas-adobe-echosign-tutorial/IC789514.png "Alkalmazás gyűjtemény")

7.  Az eredmény ablaktáblában jelölje ki az **Adobe EchoSign**, és válassza a **kész** , az alkalmazás hozzáadása parancsra.

    ![Adobe EchoSign] (./media/active-directory-saas-adobe-echosign-tutorial/IC789515.png "Adobe EchoSign")
##<a name="configuring-single-sign-on"></a>Egyszeri bejelentkezés beállítása

Ez a szakasz célja tagolása engedélyezése a felhasználóknak, hogy az Adobe EchoSign hitelesíteni a fiókkal az összevonási alapján a SAML protokoll használatával Azure AD.  
Ez az eljárás részeként biztosan szükséges számrendszerben-64 kódolt tanúsítvány fájlt.  
Ha nem ismeri a ezt az eljárást, olvassa el a [szövegfájlba bináris tanúsítvány konvertálása](http://youtu.be/PlgrzUZ-Y1o)című témakört.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Egyszeri bejelentkezés beállításához hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon **Adobe EchoSign** alkalmazás integrációs lapon kattintson **konfigurálása az egyszeri bejelentkezés** a **Konfigurálása az egyszeri bejelentkezés** párbeszédpanel megnyitásához.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-adobe-echosign-tutorial/IC789516.png "Egyszeri bejelentkezés beállítása")

2.  **Hogyan szeretné, hogy jelentkezzen be az Adobe EchoSign felhasználók** lapon jelölje be **A Microsoft Azure Active Directory Single Sign-On**, és kattintson a **Tovább gombra**.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-adobe-echosign-tutorial/IC789517.png "Egyszeri bejelentkezés beállítása")

3.  A **App URL-címe beállítása** lapon az **Adobe EchoSign bejelentkezési a URL-cím** mezőbe írja be az URL-t, a következő mintát "*https://company.echosign.com/*", és kattintson a **Tovább gombra**.

    ![Állítsa be az App URL-címe] (./media/active-directory-saas-adobe-echosign-tutorial/IC789518.png "Állítsa be az App URL-címe")

4.  A **beállítás az egyszeri bejelentkezés Adobe EchoSign a** lapon kattintson a **tanúsítvány letöltése**, és mentse a tanúsítvány fájlt a számítógépen.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-adobe-echosign-tutorial/IC789519.png "Egyszeri bejelentkezés beállítása")

5.  A különböző webes böngészőablakban jelentkezzen be az Adobe EchoSign vállalati webhely rendszergazdaként.

6.  A felső sávon kattintson a **fiók**, és, a navigációs ablakban kattintson a bal oldali die, kattintson a **SAML beállítások** csoportban a **Fiókbeállítások**parancsra.

    ![Fiók] (./media/active-directory-saas-adobe-echosign-tutorial/IC789520.png "Fiók")

7.  A SAML beállításai csoportban hajtsa végre az alábbi lépéseket:

    ![SAML-beállítások] (./media/active-directory-saas-adobe-echosign-tutorial/IC789521.png "SAML-beállítások")

    1.  **SAML mód**jelölje ki a **SAML kötelező**.
    2.  Jelölje ki a **Engedélyezése EchoSign fiók számára a jelentkezzen be a EchoSign hitelesítő adataival**.
    3.  **Felhasználók létrehozása**jelölje ki a **automatikusan hozzáadja a SAML keresztül hitelesített felhasználók**.

8.  Áthelyezése, hajt végre az alábbi lépéseket:

    ![SAML-beállítások] (./media/active-directory-saas-adobe-echosign-tutorial/IC789522.png "SAML-beállítások")

    1.  Az Azure klasszikus portálon **konfigurálása az egyszeri bejelentkezés a Adobe EchoSign** párbeszédpanel lapon a **Szervezet azonosítója** értéket másolja és illessze be a **IdP entitás azonosítója** mezőben lévő értéket.
    2.  Az Azure klasszikus portálon **konfigurálása az egyszeri bejelentkezés a Adobe EchoSign** párbeszédpanel lapon a **Távoli bejelentkezési URL-cím** értéket másolja és illessze be a **IdP bejelentkezési URL-cím** mezőben lévő értéket.
    3.  Az Azure klasszikus portálon **konfigurálása az egyszeri bejelentkezés a Adobe EchoSign** párbeszédpanel lapon a **Távoli kijelentkezés URL-címe** értéket másolja és illessze be a **IdP kijelentkezés URL-címe** mezőben lévő értéket.
    4.  A letöltött tanúsítvány **Alap-64-címként kódolt** fájl létrehozása  

        >[AZURE.TIP] További részletekért lásd [szövegfájlba bináris tanúsítvány konvertálása](http://youtu.be/PlgrzUZ-Y1o) 
 5.  Nyissa meg az alap-64 kódolt tanúsítvány a Jegyzettömbben, a tartalmát másolja a vágólapra, és illessze be a 6 **IdP tanúsítvány** szöveg.  A **módosítások mentése**gombra.

9.  Az Azure klasszikus portálon jelölje be a egyszeri bejelentkezéses konfigurációs megerősítés, és válassza a **kész** , **Állítsa be az egyszeri bejelentkezés** párbeszédpanel bezárásához.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-adobe-echosign-tutorial/IC789523.png "Egyszeri bejelentkezés beállítása")
##<a name="configuring-user-provisioning"></a>Felhasználói kiépítési konfigurálása

Ahhoz, hogy az Azure Active Directory-felhasználóknak, hogy jelentkezzen be az Adobe EchoSign, hogy ki kell építenie Adobe EchoSign be.  
Adobe EchoSign, ha a feladat manuális kiépítési.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>A felhasználói fiókok kiépítése, hajtsa végre az alábbi lépéseket:

1.  Jelentkezzen be rendszergazdaként az **Adobe EchoSign** vállalati webhely.

2.  A menüben, a képernyő tetején jelölje ki azt a **fiókot**, és a navigációs ablakban kattintson a bal oldali die, válassza a **felhasználók és csoportok**, és kattintson a **Új felhasználó**.

    ![Fiók] (./media/active-directory-saas-adobe-echosign-tutorial/IC789524.png "Fiók")

3.  **Hozzon létre új felhasználót** csoportban hajtsa végre az alábbi lépéseket:

    ![Felhasználó létrehozása] (./media/active-directory-saas-adobe-echosign-tutorial/IC789525.png "Felhasználó létrehozása")

    1.  Írja be az **E-mail címét**, **Utónév** és **Vezetéknév** be a kapcsolódó szövegdobozok kiépítése kívánt érvényes AAD fiókkal.
    2.  Kattintson a **felhasználó létrehozása**gombra.

        >[AZURE.NOTE] Az Azure Active Directory fióktulajdonos előtt aktívvá válik, győződjön meg arról, hogy a fiók mutató hivatkozást tartalmazó e-mailt kap.

>[AZURE.NOTE] Bármely más Adobe EchoSign felhasználói fiók létrehozása eszközöket is használhatja, illetve Adobe EchoSign rendelkezést AAD felhasználói fiókok által biztosított API-khoz.

##<a name="assigning-users"></a>Felhasználók hozzárendelése

Tesztelje a konfigurációt, meg kell adni az Azure Active Directory-felhasználók, használja az alkalmazás elérési útvonalát társításával engedélyezni szeretné.

###<a name="to-assign-users-to-adobe-echosign-perform-the-following-steps"></a>Felhasználók hozzárendelését meg Adobe EchoSign, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon próba-fiók létrehozása.

2.  Az **Adobe EchoSign **alkalmazás integrációs lapon kattintson a **hozzárendelését a felhasználókhoz**.

    ![Hozzárendelését a felhasználókhoz] (./media/active-directory-saas-adobe-echosign-tutorial/IC789526.png "Adhatnak a felhasználóknak")

3.  Jelölje ki a próba-felhasználó, kattintson a **hozzárendelni**, és kattintson az **Igen gombra** kattintva erősítse meg a hozzárendelés.

    ![Igen] (./media/active-directory-saas-adobe-echosign-tutorial/IC767830.png "Igen")

Ha meg szeretné tesztelje a egyszeri bejelentkezéses, Access Panel megnyitásához. Az Access ablaktábla, lásd: [Bevezetés az Access panelre](active-directory-saas-access-panel-introduction.md).
