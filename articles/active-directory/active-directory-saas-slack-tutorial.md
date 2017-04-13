<properties 
    pageTitle="Oktatóprogram: Azure Active Directory-integráció a tartalékidő |} Microsoft Azure" 
    description="Megtudhatja, hogyan használhatja a tartalékidő az Azure Active Directory ahhoz, hogy az egyszeri bejelentkezés, automatikus kiépítési és az egyéb!" 
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
    ms.date="09/19/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-slack"></a>Oktatóprogram: Azure Active Directory-integráció a tartalékidő
  
Ebben az oktatóanyagban célja integrálása az Azure és tartalékidő megjelenítéséhez.  
Az alkalmazási példát, ebben az oktatóanyagban tagolt feltételezi, hogy már rendelkezik az alábbiakat:

-   Érvényes Azure előfizetés
-   Tartalékidő egyszeri bejelentkezéses engedélyezett előfizetés
  
Ebben az oktatóanyagban befejeztével az Azure Active Directory felhasználók tartalékidő rendelt tudnak egyetlen bejelentkezés az alkalmazásba, a tartalékidő vállalati webhely (service provider által kezdeményezett bejelentkezés) és használatáról a [Bevezetés az Access panelre](active-directory-saas-access-panel-introduction.md).
  
A következő építőelemek az alkalmazási példát, ebben az oktatóanyagban tagolt áll:

1.  Az alkalmazás integrációját tartalékidő engedélyezése
2.  Egyszeri bejelentkezés beállítása
3.  Felhasználói kiépítési konfigurálása
4.  Felhasználók hozzárendelése

![Eset] (./media/active-directory-saas-slack-tutorial/IC794980.png "Eset")

##<a name="enabling-the-application-integration-for-slack"></a>Az alkalmazás integrációját tartalékidő engedélyezése
  
Ez a szakasz célja, hogyan tartalékidő az alkalmazás alkalmazással való integráció engedélyezése a szerkezet.

###<a name="to-enable-the-application-integration-for-slack-perform-the-following-steps"></a>Ahhoz, hogy az alkalmazás integrációját tartalékidő, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon kattintson a bal oldali navigációs területen kattintson az **Active Directory**.

    ![Az Active Directory] (./media/active-directory-saas-slack-tutorial/IC700993.png "Az Active Directory")

2.  A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3.  Nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson a felső menüben **alkalmazások** .

    ![Alkalmazások] (./media/active-directory-saas-slack-tutorial/IC700994.png "Alkalmazások")

4.  Kattintson a **Hozzáadás** az oldal alján.

    ![Alkalmazás hozzáadása] (./media/active-directory-saas-slack-tutorial/IC749321.png "Alkalmazás hozzáadása")

5.  **Milyen feladatot szeretne tenni** párbeszédpanelen kattintson **a gyűjteményből az alkalmazások felvétele**.

    ![Gallerry az alkalmazás hozzáadása] (./media/active-directory-saas-slack-tutorial/IC749322.png "Gallerry az alkalmazás hozzáadása")

6.  A **Keresés mezőbe**írja be a **tartalékidő**.

    ![Alkalmazás gyűjtemény] (./media/active-directory-saas-slack-tutorial/IC794981.png "Alkalmazás gyűjtemény")

7.  Az eredmény ablaktáblában válassza a **tartalékidő**, és válassza a **kész** , az alkalmazás hozzáadása parancsra.

    ![Eset] (./media/active-directory-saas-slack-tutorial/IC796925.png "Eset")

##<a name="configuring-single-sign-on"></a>Egyszeri bejelentkezés beállítása
  
Ez a szakasz célja tagolása engedélyezése a felhasználóknak, hogy tartalékidő hitelesíteni a fiókkal az összevonási alapján a SAML protokoll használatával Azure AD.  
Ez az eljárás részeként biztosan szükséges számrendszerben-64 kódolt tanúsítvány fájlt.  
Ha nem ismeri a ezt az eljárást, megtudhatja, [hogy miként szövegfájlba bináris tanúsítvány konvertálása](http://youtu.be/PlgrzUZ-Y1o)

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Egyszeri bejelentkezés beállításához hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon **tartalékidő** alkalmazás integrációs lapon kattintson **konfigurálása az egyszeri bejelentkezés** a **Konfigurálása az egyszeri bejelentkezés** párbeszédpanel megnyitásához.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-slack-tutorial/IC794982.png "Egyszeri bejelentkezés beállítása")

2.  **Hogyan megtekinti tartalékidő jelentkezzen be felhasználók** lapon jelölje be **A Microsoft Azure Active Directory Single Sign-On**, és kattintson a **Tovább gombra**.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-slack-tutorial/IC794983.png "Egyszeri bejelentkezés beállítása")

3.  **App URL-címe beállítása** lapon az **Tartalékidő bejelentkezési az URL** mezőbe írja be a tartalékidő bérlői webhely URL-CÍMÉT (például: "*https://azuread.slack.com*"), majd kattintson a **Tovább**gombra.

    ![Állítsa be az App URL-címe] (./media/active-directory-saas-slack-tutorial/IC794984.png "Állítsa be az App URL-címe")

4.  **Beállítás az egyszeri bejelentkezés tartalékidő a** lapon a tanúsítvány letöltése, kattintson a **tanúsítvány letöltése**, és mentse a tanúsítvány fájlt a számítógépen helyben parancsra.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-slack-tutorial/IC794985.png "Egyszeri bejelentkezés beállítása")

5.  A különböző webes böngészőablakban jelentkezzen be a tartalékidő vállalati webhely rendszergazdaként.

6.  Nyissa meg a **a Microsoft Azure Active Directory \> beállításai csoport**.

    ![Csapat-beállítások] (./media/active-directory-saas-slack-tutorial/IC794986.png "Csapat-beállítások")

7.  **Csoport beállításai** csoportban kattintson a **hitelesítés** fülre, és válassza a **Beállítások módosítása hivatkozásra**.

    ![Csapat-beállítások] (./media/active-directory-saas-slack-tutorial/IC794987.png "Csapat-beállítások")

8.  A **SAML hitelesítési beállítások** párbeszédpanelen hajtsa végre az alábbi lépéseket:

    ![SAML-beállítások] (./media/active-directory-saas-slack-tutorial/IC794988.png "SAML-beállítások")

    1.  Az Azure klasszikus portálon **konfigurálása az egyszeri bejelentkezés a tartalékidő** párbeszédpanel lapon a **SAML egyszeri bejelentkezési URL-címe** értéket másolja és illessze be a **SAML 2.0-s végpont HTTP ()** mezőben lévő értéket.
    2.  Az Azure klasszikus portálon **konfigurálása az egyszeri bejelentkezés a tartalékidő** párbeszédpanel lapon másolja a **Kibocsátó URL-címe** értéket, és illessze be a **Identitás szolgáltató kibocsátó** mezőben lévő értéket.
    3.  A letöltött tanúsítvány **Alap-64-címként kódolt** fájl létrehozása
    
        >[AZURE.TIP] További információra kíváncsi olvassa el a [bináris tanúsítvány szövegfájlba konvertálása](http://youtu.be/PlgrzUZ-Y1o) című témakört.

    4.  Nyissa meg az alap-64 kódolt tanúsítvány a Jegyzettömbben, a tartalmát a vágólapra másolja és illessze be a **Nyilvános tanúsítvány** mezőben lévő értéket szeretné.
    5.  Törölje az **engedélyezése a felhasználóknak az e-mail cím módosítása lehetőséget**.
    6.  Jelölje ki a **engedélyezése a felhasználók számára, válassza a saját felhasználónevét**.
    7.  **A csapat számára hitelesítési szerint kell használni**jelölje ki a **nem kötelező**.
    8.  **Konfigurációs a Mentés**gombra.

9.  Az Azure klasszikus portálon jelölje be a egyszeri bejelentkezéses konfigurációs megerősítés, és válassza a **kész** , **Állítsa be az egyszeri bejelentkezés** párbeszédpanel bezárásához.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-slack-tutorial/IC794989.png "Egyszeri bejelentkezés beállítása")

##<a name="configuring-user-provisioning"></a>Felhasználói kiépítési konfigurálása
  
Ahhoz, hogy az Azure Active Directory-felhasználóknak, hogy jelentkezzen be a tartalékidő, hogy ki kell építenie tartalékidő be.
  
Nincs teendő konfigurálható a felhasználó létesítése való tartalékidő nem.  
Egy tevékenységhez rendelt felhasználó megkísérel bejelentkezni a tartalékidő, amikor egy tartalékidő fiók automatikusan létrejön, ha szükséges.

##<a name="assigning-users"></a>Felhasználók hozzárendelése
  
Tesztelje a konfigurációt, meg kell adni az Azure Active Directory-felhasználók, használja az alkalmazás elérési útvonalát társításával engedélyezni szeretné.

###<a name="to-assign-users-to-slack-perform-the-following-steps"></a>Felhasználók hozzárendelése tartalékidő, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon próba-fiók létrehozása.

2.  Kattintson a **tartalékidő **alkalmazás integrációs lap **hozzárendelését a felhasználókhoz**.

    ![Hozzárendelését a felhasználókhoz] (./media/active-directory-saas-slack-tutorial/IC794990.png "Adhatnak a felhasználóknak")

3.  Jelölje ki a próba-felhasználó, kattintson a **hozzárendelni**, és kattintson az **Igen gombra** kattintva erősítse meg a hozzárendelés.

    ![Igen] (./media/active-directory-saas-slack-tutorial/IC767830.png "Igen")
  
Ha meg szeretné tesztelje a egyszeri bejelentkezéses, Access Panel megnyitásához. Az Access ablaktábla, lásd: [Bevezetés az Access panelre](active-directory-saas-access-panel-introduction.md).