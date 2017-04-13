<properties 
    pageTitle="Oktatóprogram: Azure Active Directory-integráció a Intacct |} Microsoft Azure" 
    description="Megtudhatja, hogyan használhatja a Intacct az Azure Active Directory ahhoz, hogy az egyszeri bejelentkezés, automatikus kiépítési és az egyéb!" 
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

#<a name="tutorial-azure-active-directory-integration-with-intacct"></a>Oktatóprogram: Azure Active Directory-integráció a Intacct
  
Ebben az oktatóanyagban célja integrálása az Azure és Intacct megjelenítése.  
Az alkalmazási példát, ebben az oktatóanyagban tagolt feltételezi, hogy már rendelkezik az alábbiakat:

-   Érvényes Azure előfizetés
-   Intacct bérlői webhelyre
  
Ebben az oktatóanyagban befejeztével az Azure Active Directory-felhasználók Intacct rendelt fogja tudni az alkalmazás a Intacct vállalati webhely (service provider által kezdeményezett bejelentkezés) és használatáról a [Bevezetés az Access panelen](active-directory-saas-access-panel-introduction.md)az egyszeri bejelentkezési.
  
A következő építőelemek az alkalmazási példát, ebben az oktatóanyagban tagolt áll:

1.  Az alkalmazás integrációját Intacct engedélyezése
2.  Egyszeri bejelentkezés beállítása
3.  Felhasználói kiépítési konfigurálása
4.  Felhasználók hozzárendelése

![Eset] (./media/active-directory-saas-intacct-tutorial/IC790030.png "Eset")
##<a name="enabling-the-application-integration-for-intacct"></a>Az alkalmazás integrációját Intacct engedélyezése
  
Ez a szakasz célja, hogyan Intacct az alkalmazás alkalmazással való integráció engedélyezése a szerkezet.

###<a name="to-enable-the-application-integration-for-intacct-perform-the-following-steps"></a>Ahhoz, hogy az alkalmazás integrációját Intacct, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon kattintson a bal oldali navigációs területen kattintson az **Active Directory**.

    ![Az Active Directory] (./media/active-directory-saas-intacct-tutorial/IC700993.png "Az Active Directory")

2.  A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3.  Nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson a felső menüben **alkalmazások** .

    ![Alkalmazások] (./media/active-directory-saas-intacct-tutorial/IC700994.png "Alkalmazások")

4.  Kattintson a **Hozzáadás** az oldal alján.

    ![Alkalmazás hozzáadása] (./media/active-directory-saas-intacct-tutorial/IC749321.png "Alkalmazás hozzáadása")

5.  **Milyen feladatot szeretne tenni** párbeszédpanelen kattintson **a gyűjteményből az alkalmazások felvétele**.

    ![Gallerry az alkalmazás hozzáadása] (./media/active-directory-saas-intacct-tutorial/IC749322.png "Gallerry az alkalmazás hozzáadása")

6.  A **Keresés mezőbe**írja be a **Intacct**.

    ![Alkalmazás gyűjtemény] (./media/active-directory-saas-intacct-tutorial/IC790031.png "Alkalmazás gyűjtemény")

7.  Az eredmény ablaktáblában jelölje ki a **Intacct**, és válassza a **kész** , az alkalmazás hozzáadása gombra.

    ![Intacct] (./media/active-directory-saas-intacct-tutorial/IC790032.png "Intacct")
##<a name="configuring-single-sign-on"></a>Egyszeri bejelentkezés beállítása
  
Ez a szakasz célja tagolása engedélyezése a felhasználóknak, hogy Intacct hitelesíteni a fiókkal az összevonási alapján a SAML protokoll használatával Azure AD.  
Ez az eljárás részeként biztosan szükséges számrendszerben-64 kódolt tanúsítvány fájlt.  
Ha nem ismeri a ezt az eljárást, olvassa el a [szövegfájlba bináris tanúsítvány konvertálása](http://youtu.be/PlgrzUZ-Y1o)című témakört.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Egyszeri bejelentkezés beállításához hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon **Intacct** alkalmazás integrációs lapon kattintson **konfigurálása az egyszeri bejelentkezés** a **Konfigurálása az egyszeri bejelentkezés** párbeszédpanel megnyitásához.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-intacct-tutorial/IC790033.png "Egyszeri bejelentkezés beállítása")

2.  **Hogyan szeretné, hogy jelentkezzen be az Intacct felhasználók** lapon jelölje be **A Microsoft Azure Active Directory Single Sign-On**, és kattintson a **Tovább gombra**.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-intacct-tutorial/IC790034.png "Egyszeri bejelentkezés beállítása")

3.  A **App URL-címe beállítása** lapon az **URL a bejelentkezési Intacct** mezőbe írja be az URL-t, a következő mintát "*https://Intacct.com/company*", és kattintson a **Tovább gombra**.

    ![Állítsa be az App URL-címe] (./media/active-directory-saas-intacct-tutorial/IC790035.png "Állítsa be az App URL-címe")

4.  A **beállítás az egyszeri bejelentkezés Intacct a** lapon kattintson a **tanúsítvány letöltése**, és mentse a tanúsítvány fájlt a számítógépen.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-intacct-tutorial/IC790036.png "Egyszeri bejelentkezés beállítása")

5.  A különböző webes böngészőablakban jelentkezzen be a Intacct vállalati webhely rendszergazdaként.

6.  Kattintson a **vállalat** fülre, és kattintson a **Vállalati adatok**gombra.

    ![Vállalati] (./media/active-directory-saas-intacct-tutorial/IC790037.png "Vállalati")

7.  Kattintson a **Biztonság** fülre, és kattintson a **Szerkesztés**gombra.

    ![Biztonsági] (./media/active-directory-saas-intacct-tutorial/IC790038.png "Biztonsági")

8.  **Egyszeri bejelentkezés (SSO)** csoportban végezze el az alábbi lépéseket:

    ![Egyszeri bejelentkezés] (./media/active-directory-saas-intacct-tutorial/IC790039.png "Egyszeri bejelentkezés")

    1.  Jelölje ki, hogy **egyszeri bejelentkezési engedélyezi a**.
    2.  **Identitás szolgáltató típusa**csoportban jelölje ki a **SAML 2.0-s verziója**.
    3.  Az Azure klasszikus portálon **konfigurálása az egyszeri bejelentkezés a Intacct** párbeszédpanel lapon a **Kibocsátó URL-címe** értéket másolja és illessze be a **Kibocsátó URL-címe** mezőben lévő értéket.
    4.  Az Azure klasszikus portálon **konfigurálása az egyszeri bejelentkezés a Intacct** párbeszédpanel lapon a **Távoli bejelentkezési URL-cím** értéket másolja és illessze be a **Bejelentkezési URL-cím** mezőben lévő értéket.
    5.  A letöltött tanúsítvány **Alap-64-címként kódolt** fájl létrehozása
        
        >[AZURE.TIP]További információra kíváncsi olvassa el a [bináris tanúsítvány szövegfájlba konvertálása](http://youtu.be/PlgrzUZ-Y1o) című témakört.

    6.  Nyissa meg az alap-64 kódolt tanúsítvány a Jegyzettömbben, a tartalmát a vágólapra másolja és illessze be a **tanúsítvány** mezőben lévő értéket szeretné
    7.  Kattintson a **Mentés**gombra.

9.  Az Azure klasszikus portálon jelölje be a egyszeri bejelentkezéses konfigurációs megerősítés, és válassza a **kész** , **Állítsa be az egyszeri bejelentkezés** párbeszédpanel bezárásához.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-intacct-tutorial/IC790040.png "Egyszeri bejelentkezés beállítása")
##<a name="configuring-user-provisioning"></a>Felhasználói kiépítési konfigurálása
  
Ahhoz, hogy az Azure Active Directory-felhasználóknak, hogy jelentkezzen be a Intacct, hogy ki kell építenie Intacct be.  
Intacct, ha a feladat manuális kiépítési.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>A felhasználói fiókok kiépítése, hajtsa végre az alábbi lépéseket:

1.  Jelentkezzen be az **Intacct** bérlői webhelyen.

2.  Kattintson a **vállalat** fülre, és kattintson a **felhasználók**.

    ![Felhasználók] (./media/active-directory-saas-intacct-tutorial/IC790041.png "Felhasználók")

3.  Kattintson a **Hozzáadás** fülre.

    ![Hozzáadása] (./media/active-directory-saas-intacct-tutorial/IC790042.png "Hozzáadása")

4.  A **Felhasználói adatok** csoportban hajtsa végre az alábbi lépéseket:

    ![Felhasználói adatok] (./media/active-directory-saas-intacct-tutorial/IC790043.png "Felhasználói adatok")

    1.  Írja be a **Felhasználói azonosító**, a **vezetéknevet**, **utónevet**, az **E-mail címét**, a **cím** és a **Telefon** -be a kapcsolódó szövegdobozok kiépítése kívánt Azure AD-fiók.
    2.  Jelölje ki a kívánt kiépítése Azure AD-fiók **rendszergazdai jogosultságait** .
    3.  Kattintson a **Mentés**gombra.
        
        >[AZURE.NOTE] A AAD fióktulajdonos a fog egy e-mailt, és kövesse a előtt aktívvá válik, győződjön meg arról, hogy a fiók mutató hivatkozást.

>[AZURE.NOTE] Bármely más Intacct felhasználói fiók létrehozása eszközöket is használhatja, illetve Intacct rendelkezést AAD felhasználói fiókok által biztosított API-khoz.

##<a name="assigning-users"></a>Felhasználók hozzárendelése
  
Tesztelje a konfigurációt, meg kell adni az Azure Active Directory-felhasználók, használja az alkalmazás elérési útvonalát társításával engedélyezni szeretné.

###<a name="to-assign-users-to-intacct-perform-the-following-steps"></a>Felhasználók hozzárendelése Intacct, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon próba-fiók létrehozása.

2.  Kattintson a **Intacct **alkalmazás integrációs lap **hozzárendelését a felhasználókhoz**.

    ![Hozzárendelését a felhasználókhoz] (./media/active-directory-saas-intacct-tutorial/IC790044.png "Adhatnak a felhasználóknak")

3.  Jelölje ki a próba-felhasználó, kattintson a **hozzárendelni**, és kattintson az **Igen gombra** kattintva erősítse meg a hozzárendelés.

    ![Igen] (./media/active-directory-saas-intacct-tutorial/IC767830.png "Igen")
  
Ha meg szeretné tesztelje a egyszeri bejelentkezéses, Access Panel megnyitásához. Az Access ablaktábla, lásd: [Bevezetés az Access panelre](active-directory-saas-access-panel-introduction.md).