<properties 
    pageTitle="Oktatóprogram: Azure Active Directory-integráció a Kudos |} Microsoft Azure" 
    description="Megtudhatja, hogyan használhatja a Kudos az Azure Active Directory ahhoz, hogy az egyszeri bejelentkezés, automatikus kiépítési és az egyéb!" 
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

#<a name="tutorial-azure-active-directory-integration-with-kudos"></a>Oktatóprogram: Azure Active Directory-integráció a Kudos
  
Ebben az oktatóanyagban célja integrálása az Azure és Kudos megjelenítéséhez.  
Az alkalmazási példát, ebben az oktatóanyagban tagolt feltételezi, hogy már rendelkezik az alábbiakat:

-   Érvényes Azure előfizetés
-   Kudos bérlői webhelyre
  
Ebben az oktatóanyagban befejeztével az Azure Active Directory-felhasználók Kudos rendelt fogja tudni az alkalmazás a Kudos vállalati webhely (service provider által kezdeményezett bejelentkezés) és használatáról a [Bevezetés az Access panelen](active-directory-saas-access-panel-introduction.md)az egyszeri bejelentkezési.
  
A következő építőelemek az alkalmazási példát, ebben az oktatóanyagban tagolt áll:

1.  Az alkalmazás integrációját Kudos engedélyezése
2.  Egyszeri bejelentkezés beállítása
3.  Felhasználói kiépítési konfigurálása
4.  Felhasználók hozzárendelése

![Eset] (./media/active-directory-saas-kudos-tutorial/IC787799.png "Eset")
##<a name="enabling-the-application-integration-for-kudos"></a>Az alkalmazás integrációját Kudos engedélyezése
  
Ez a szakasz célja, hogyan Kudos az alkalmazás alkalmazással való integráció engedélyezése a szerkezet.

###<a name="to-enable-the-application-integration-for-kudos-perform-the-following-steps"></a>Ahhoz, hogy az alkalmazás integrációját Kudos, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon kattintson a bal oldali navigációs területen kattintson az **Active Directory**.

    ![Az Active Directory] (./media/active-directory-saas-kudos-tutorial/IC700993.png "Az Active Directory")

2.  A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3.  Nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson a felső menüben **alkalmazások** .

    ![Alkalmazások] (./media/active-directory-saas-kudos-tutorial/IC700994.png "Alkalmazások")

4.  Kattintson a **Hozzáadás** az oldal alján.

    ![Alkalmazás hozzáadása] (./media/active-directory-saas-kudos-tutorial/IC749321.png "Alkalmazás hozzáadása")

5.  **Milyen feladatot szeretne tenni** párbeszédpanelen kattintson **a gyűjteményből az alkalmazások felvétele**.

    ![Gallerry az alkalmazás hozzáadása] (./media/active-directory-saas-kudos-tutorial/IC749322.png "Gallerry az alkalmazás hozzáadása")

6.  A **Keresés mezőbe**írja be a **Kudos**.

    ![Alkalmazás gyűjtemény] (./media/active-directory-saas-kudos-tutorial/IC787800.png "Alkalmazás gyűjtemény")

7.  Az eredmény ablaktáblában jelölje ki a **Kudos**, és válassza a **kész** , az alkalmazás hozzáadása gombra.

    ![Kudos] (./media/active-directory-saas-kudos-tutorial/IC787801.png "Kudos")
##<a name="configuring-single-sign-on"></a>Egyszeri bejelentkezés beállítása
  
Ez a szakasz célja tagolása engedélyezése a felhasználóknak, hogy Kudos hitelesíteni a fiókkal az összevonási alapján a SAML protokoll használatával Azure AD.  
Ez az eljárás részeként biztosan szükséges számrendszerben-64 kódolt tanúsítvány fájlt.  
Ha nem ismeri a ezt az eljárást, olvassa el a [szövegfájlba bináris tanúsítvány konvertálása](http://youtu.be/PlgrzUZ-Y1o)című témakört.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Egyszeri bejelentkezés beállításához hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon **Kudos** alkalmazás integrációs lapon kattintson **konfigurálása az egyszeri bejelentkezés** a **Konfigurálása az egyszeri bejelentkezés** párbeszédpanel megnyitásához.

    ![Beállítás az egyszeri bejelentkezés] (./media/active-directory-saas-kudos-tutorial/IC787802.png "Beállítás az egyszeri bejelentkezés")

2.  **Hogyan szeretné, hogy jelentkezzen be az Kudos felhasználók** lapon jelölje be **A Microsoft Azure Active Directory Single Sign-On**, és kattintson a **Tovább gombra**.

    ![Beállítás az egyszeri bejelentkezés] (./media/active-directory-saas-kudos-tutorial/IC787803.png "Beállítás az egyszeri bejelentkezés")

3.  A **App URL-címe beállítása** lapon az **URL a bejelentkezési Kudos** mezőbe írja be az URL-t, a következő mintát "*https://company.kudosnow.com*", és kattintson a **Tovább gombra**.

    ![Állítsa be az App URL-címe] (./media/active-directory-saas-kudos-tutorial/IC787804.png "Állítsa be az App URL-címe")

4.  A **beállítás az egyszeri bejelentkezés Kudos a** lapon kattintson a **tanúsítvány letöltése**, és mentse a tanúsítvány fájlt a számítógépen.

    ![Beállítás az egyszeri bejelentkezés] (./media/active-directory-saas-kudos-tutorial/IC787805.png "Beállítás az egyszeri bejelentkezés")

5.  A különböző webes böngészőablakban jelentkezzen be a Kudos vállalati webhely rendszergazdaként.

6.  A felső sávon kattintson a **Beállítások**gombra.

    ![Beállítások] (./media/active-directory-saas-kudos-tutorial/IC787806.png "Beállítások")

7.  Kattintson a **integrációs \> SSO**.

8.  **Egyszeri bejelentkezés** csoportban hajtsa végre az alábbi lépéseket:

    ![Egyszeri bejelentkezés] (./media/active-directory-saas-kudos-tutorial/IC787807.png "Egyszeri bejelentkezés")

    1.  Az Azure klasszikus portálon **konfigurálása az egyszeri bejelentkezés a Kudos** párbeszédpanel lapon az **Egyszeri bejelentkezéses szolgáltatás URL-címe** értéket másolja és illessze be a **bejelentkezési URL-címe** mezőben lévő értéket.
    2.  A letöltött tanúsítvány **Alap-64-címként kódolt** fájl létrehozása  

        >[AZURE.TIP]
        További információra kíváncsi olvassa el a [bináris tanúsítvány szövegfájlba konvertálása](http://youtu.be/PlgrzUZ-Y1o) című témakört.

    3.  Nyissa meg az alap-64 kódolt tanúsítvány a Jegyzettömbben, azt tartalmát a vágólapra másolja és illessze be a **x.509-es tanúsítvány** mezőben lévő értéket szeretné
    4.  Az Azure klasszikus portálon **konfigurálása az egyszeri bejelentkezés a Kudos** párbeszédpanel lapon az **Egyetlen Sign-Out szolgáltatás URL-címe** értéket másolja és illessze be a **Kijelentkezés az URL-címe** mezőben lévő értéket.
    5.  **A Kudos URL-cím** mezőbe írja be a vállalat nevét.
    6.  Kattintson a **Mentés**gombra.

9.  Az Azure klasszikus portálon jelölje be a egyszeri bejelentkezéses konfigurációs megerősítés, és válassza a **kész** , **Állítsa be az egyszeri bejelentkezés** párbeszédpanel bezárásához.

    ![Beállítás az egyszeri bejelentkezés] (./media/active-directory-saas-kudos-tutorial/IC787808.png "Beállítás az egyszeri bejelentkezés")
##<a name="configuring-user-provisioning"></a>Felhasználói kiépítési konfigurálása
  
Ahhoz, hogy az Azure Active Directory-felhasználóknak, hogy jelentkezzen be a Kudos, hogy ki kell építenie Kudos be.  
Kudos, ha a feladat manuális kiépítési.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>A felhasználói fiókok kiépítése, hajtsa végre az alábbi lépéseket:

1.  Jelentkezzen be rendszergazdaként a **Kudos** vállalati webhely.

2.  A felső sávon kattintson a **Beállítások**gombra.

    ![Beállítások] (./media/active-directory-saas-kudos-tutorial/IC787806.png "Beállítások")

3.  Kattintson a **felhasználó rendszergazda**.

4.  Kattintson a **felhasználók** fülre, és kattintson a **felhasználó hozzáadása**gombra.

    ![Felhasználói rendszergazda] (./media/active-directory-saas-kudos-tutorial/IC787809.png "Felhasználói rendszergazda")

5.  A **felhasználó hozzáadása** csoportban hajtsa végre az alábbi lépéseket:

    ![Felhasználó hozzáadása] (./media/active-directory-saas-kudos-tutorial/IC787810.png "Felhasználó hozzáadása")

    1.  Írja be az **utónevet**, **Vezetéknév**, **e-mailek** és egyéb részletek be a kapcsolódó szövegdobozok kiépítése kívánt érvényes Azure Active Directory-fiók.
    2.  Kattintson a **felhasználó létrehozása**gombra.

>[AZURE.NOTE]Bármely más Kudos felhasználói fiók létrehozása eszközöket is használhatja, illetve Kudos rendelkezést AAD felhasználói fiókok által biztosított API-khoz.

##<a name="assigning-users"></a>Felhasználók hozzárendelése
  
Tesztelje a konfigurációt, meg kell adni az Azure Active Directory-felhasználók, használja az alkalmazás elérési útvonalát társításával engedélyezni szeretné.

###<a name="to-assign-users-to-kudos-perform-the-following-steps"></a>Felhasználók hozzárendelése Kudos, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon próba-fiók létrehozása.

2.  Kattintson a **Kudos **alkalmazás integrációs lap **hozzárendelését a felhasználókhoz**.

    ![Adhatnak a felhasználóknak] (./media/active-directory-saas-kudos-tutorial/IC787811.png "Adhatnak a felhasználóknak")

3.  Jelölje ki a próba-felhasználó, kattintson a **hozzárendelni**, és kattintson az **Igen gombra** kattintva erősítse meg a hozzárendelés.

    ![Igen] (./media/active-directory-saas-kudos-tutorial/IC767830.png "Igen")
  
Ha meg szeretné tesztelje a egyszeri bejelentkezéses, Access Panel megnyitásához. Az Access ablaktábla, lásd: [Bevezetés az Access panelre](active-directory-saas-access-panel-introduction.md).