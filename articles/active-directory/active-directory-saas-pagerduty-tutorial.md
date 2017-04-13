<properties 
    pageTitle="Oktatóprogram: Azure Active Directory-integráció a Pagerduty |} Microsoft Azure" 
    description="Megtudhatja, hogyan használhatja a Pagerduty az Azure Active Directory ahhoz, hogy az egyszeri bejelentkezés, automatikus kiépítési és az egyéb!" 
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

#<a name="tutorial-azure-active-directory-integration-with-pagerduty"></a>Oktatóprogram: Azure Active Directory-integráció a Pagerduty
  
Ebben az oktatóanyagban célja integrálása az Azure és Pagerduty megjelenítéséhez.  
Az alkalmazási példát, ebben az oktatóanyagban tagolt feltételezi, hogy már rendelkezik az alábbiakat:

-   Érvényes Azure előfizetés
-   Pagerduty bérlői webhelyre
  
Ebben az oktatóanyagban befejeztével az Azure Active Directory-felhasználók Pagerduty rendelt fogja tudni az alkalmazás a Pagerduty vállalati webhely (service provider által kezdeményezett bejelentkezés) és használatáról a [Bevezetés az Access panelen](active-directory-saas-access-panel-introduction.md)az egyszeri bejelentkezési.
  
A következő építőelemek az alkalmazási példát, ebben az oktatóanyagban tagolt áll:

1.  Az alkalmazás integrációját Pagerduty engedélyezése
2.  Egyszeri bejelentkezés beállítása
3.  Felhasználói kiépítési konfigurálása
4.  Felhasználók hozzárendelése

![Eset] (./media/active-directory-saas-pagerduty-tutorial/IC778528.png "Eset")
##<a name="enabling-the-application-integration-for-pagerduty"></a>Az alkalmazás integrációját Pagerduty engedélyezése
  
Ez a szakasz célja, hogyan Pagerduty az alkalmazás alkalmazással való integráció engedélyezése a szerkezet.

###<a name="to-enable-the-application-integration-for-pagerduty-perform-the-following-steps"></a>Ahhoz, hogy az alkalmazás integrációját Pagerduty, hajtsa végre az alábbi lépéseket:

1.  Az Azure felügyeleti portálon kattintson a bal oldali navigációs területen kattintson az **Active Directory**.

    ![Az Active Directory] (./media/active-directory-saas-pagerduty-tutorial/IC700993.png "Az Active Directory")

2.  A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3.  Nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson a felső menüben **alkalmazásokat** .

    ![Alkalmazások] (./media/active-directory-saas-pagerduty-tutorial/IC700994.png "Alkalmazások")

4.  Kattintson a **Hozzáadás** az oldal alján.

    ![Alkalmazás hozzáadása] (./media/active-directory-saas-pagerduty-tutorial/IC749321.png "Alkalmazás hozzáadása")

5.  **Milyen feladatot szeretne tenni** párbeszédpanelen kattintson **a gyűjteményből az alkalmazások felvétele**.

    ![Gallerry az alkalmazás hozzáadása] (./media/active-directory-saas-pagerduty-tutorial/IC749322.png "Gallerry az alkalmazás hozzáadása")

6.  A **Keresés mezőbe**írja be a **Pagerduty**.

    ![Alkalmazás gyűjtemény] (./media/active-directory-saas-pagerduty-tutorial/IC778529.png "Alkalmazás gyűjtemény")

7.  Az eredmény ablaktáblában jelölje ki a **Pagerduty**, és válassza a **kész** , az alkalmazás hozzáadása gombra.

    ![PagerDuty] (./media/active-directory-saas-pagerduty-tutorial/IC778530.png "PagerDuty")
##<a name="configuring-single-sign-on"></a>Egyszeri bejelentkezés beállítása
  
Ez a szakasz célja tagolása engedélyezése a felhasználóknak, hogy Pagerduty hitelesíteni a fiókkal az összevonási alapján a SAML protokoll használatával Azure AD.  
Ez az eljárás részeként biztosan szükséges számrendszerben-64 kódolt tanúsítvány fájlt.  
Ha nem ismeri a ezt az eljárást, olvassa el a [szövegfájlba bináris tanúsítvány konvertálása](http://youtu.be/PlgrzUZ-Y1o)című témakört.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Egyszeri bejelentkezés beállításához hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon **Pagerduty** alkalmazás integrációs lapon kattintson **konfigurálása az egyszeri bejelentkezés** a **Konfigurálása az egyszeri bejelentkezés** párbeszédpanel megnyitásához.

    ![Beállítás az egyszeri bejelentkezés] (./media/active-directory-saas-pagerduty-tutorial/IC778531.png "Beállítás az egyszeri bejelentkezés")

2.  **Hogyan szeretné, hogy jelentkezzen be az Pagerduty felhasználók** lapon jelölje be **A Microsoft Azure Active Directory Single Sign-On**, és kattintson a **Tovább gombra**.

    ![Beállítás az egyszeri bejelentkezés] (./media/active-directory-saas-pagerduty-tutorial/IC778532.png "Beállítás az egyszeri bejelentkezés")

3.  Az **App URL-címe konfigurálása** lapon **Pagerduty bejelentkezési az URL** mezőbe írja be az URL-t, a következő mintát "*https://\<bérlői neve\>. Pagerduty.com*", majd kattintson a **Tovább**gombra.

    ![Konfigurálása app URL-címe] (./media/active-directory-saas-pagerduty-tutorial/IC778533.png "Konfigurálása app URL-címe")

4.  A **beállítás az egyszeri bejelentkezés Pagerduty a** lapon kattintson a **tanúsítvány letöltése**, és mentse a tanúsítvány fájlt a számítógépen.

    ![Beállítás az egyszeri bejelentkezés] (./media/active-directory-saas-pagerduty-tutorial/IC778534.png "Beállítás az egyszeri bejelentkezés")

5.  A különböző webes böngészőablakban jelentkezzen be a Pagerduty vállalati webhely rendszergazdaként.

6.  A felső sávon kattintson a **Fiókbeállítások menügombra**.

    ![Fiókbeállítások] (./media/active-directory-saas-pagerduty-tutorial/IC778535.png "Fiókbeállítások")

7.  Kattintson **az egyszeri bejelentkezés**.

    ![Egyszeri bejelentkezés] (./media/active-directory-saas-pagerduty-tutorial/IC778536.png "Egyszeri bejelentkezés")

8.  Engedélyezze az egyszeri bejelentkezés (SSO) lapon végezze el az alábbi lépéseket:

    ![Egyszeri bejelentkezés engedélyezése] (./media/active-directory-saas-pagerduty-tutorial/IC778537.png "Egyszeri bejelentkezés engedélyezése")

    1.  A letöltött tanúsítvány **Alap-64-címként kódolt** fájl létrehozása  

        >[AZURE.TIP] További információra kíváncsi olvassa el a [bináris tanúsítvány szövegfájlba konvertálása](http://youtu.be/PlgrzUZ-Y1o) című témakört.

    2.  Nyissa meg az alap-64 kódolt tanúsítvány a Jegyzettömbben, azt tartalmát a vágólapra másolja és illessze be a **X.509-es tanúsítvány** mezőben lévő értéket szeretné
    3.  Az Azure klasszikus portálon **konfigurálása az egyszeri bejelentkezés a Pagerduty** párbeszéd lapon a **Távoli bejelentkezési URL-cím** értéket másolja és illessze be a **Bejelentkezési URL-cím** mezőben lévő értéket.
    4.  Az Azure klasszikus portálon **konfigurálása az egyszeri bejelentkezés a Pagerduty** párbeszéd lapon a **Távoli kijelentkezés URL-címe** értéket másolja és illessze be a **Kijelentkezés URL-címe** mezőben lévő értéket.
    5.  Jelölje ki **az egyszeri bejelentkezés bekapcsolása**.
    6.  A **módosítások mentése**gombra.

9.  Az Azure klasszikus portálon jelölje be a egyszeri bejelentkezéses konfigurációs megerősítés, és válassza a **kész** , **Állítsa be az egyszeri bejelentkezés** párbeszédpanel bezárásához.

    ![Beállítás az egyszeri bejelentkezés] (./media/active-directory-saas-pagerduty-tutorial/IC778538.png "Beállítás az egyszeri bejelentkezés")
##<a name="configuring-user-provisioning"></a>Felhasználói kiépítési konfigurálása
  
Ahhoz, hogy az Azure Active Directory-felhasználóknak, hogy jelentkezzen be a Pagerduty, hogy ki kell építenie Pagerduty be.  
Pagerduty, ha a feladat manuális kiépítési.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>A felhasználói fiókok kiépítése, hajtsa végre az alábbi lépéseket:

1.  Jelentkezzen be az **Pagerduty** bérlői webhelyen.

2.  A felső sávon kattintson a **felhasználók**elemre.

3.  Kattintson a **felhasználók hozzáadása**gombra.

    ![Felhasználók hozzáadása] (./media/active-directory-saas-pagerduty-tutorial/IC778539.png "Felhasználók hozzáadása")

4.  Írja be az **első és utolsó nevét** és **E-mail** címét az Azure Active Directory felhasználót kiépítése, kattintson a **Hozzáadás**gombra, és kattintson a **Küldés felkéri**a **csapata meghívása** párbeszédpanel.

    ![A csapat meghívása] (./media/active-directory-saas-pagerduty-tutorial/IC778540.png "A csapat meghívása")

    >[AZURE.NOTE] A felvett összes felhasználó kap-meghívót PagerDuty fiók létrehozása.

>[AZURE.NOTE] Bármely más Pagerduty felhasználói fiók létrehozása eszközöket is használhatja, illetve Pagerduty rendelkezést AAD felhasználói fiókok által biztosított API-khoz.

##<a name="assigning-users"></a>Felhasználók hozzárendelése
  
Tesztelje a konfigurációt, meg kell adni az Azure Active Directory-felhasználók, használja az alkalmazás elérési útvonalát társításával engedélyezni szeretné.

###<a name="to-assign-users-to-pagerduty-perform-the-following-steps"></a>Felhasználók hozzárendelése Pagerduty, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon próba-fiók létrehozása.

2.  Kattintson a **Pagerduty **alkalmazás integrációs lap **hozzárendelését a felhasználókhoz**.

    ![Adhatnak a felhasználóknak] (./media/active-directory-saas-pagerduty-tutorial/IC778541.png "Adhatnak a felhasználóknak")

3.  Jelölje ki a próba-felhasználó, kattintson a **hozzárendelni**, és kattintson az **Igen gombra** kattintva erősítse meg a hozzárendelés.

    ![Igen] (./media/active-directory-saas-pagerduty-tutorial/IC767830.png "Igen")
  
Ha meg szeretné tesztelje a egyszeri bejelentkezéses, Access Panel megnyitásához. Ha többet szeretne tudni az Access panelen című témakörben [az Access-panelen](active-directory-saas-access-panel-introduction.md).