<properties 
    pageTitle="Oktatóprogram: Azure Active Directory-integráció a központi asztali |} Microsoft Azure" 
    description="Megtudhatja, hogyan használhatja a központi asztali az Azure Active Directory ahhoz, hogy az egyszeri bejelentkezés, automatikus kiépítési és az egyéb!" 
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

#<a name="tutorial-azure-active-directory-integration-with-central-desktop"></a>Oktatóprogram: Azure Active Directory-integráció a központi asztali

Ebben az oktatóanyagban célja integrálása az Azure és központi asztali megjelenítéséhez. Az alkalmazási példát, ebben az oktatóanyagban tagolt feltételezi, hogy már rendelkezik az alábbiakat:

-   Érvényes Azure előfizetés
-   Egy központi asztali egyszeri bejelentkezéses engedélyezett előfizetés / központi asztali bérlői

A következő építőelemek az alkalmazási példát, ebben az oktatóanyagban tagolt áll:

1.  A központi asztali alkalmazás integráció engedélyezése
2.  Egyszeri bejelentkezés beállítása
3.  Felhasználói kiépítési konfigurálása
4.  Felhasználók hozzárendelése

![Eset] (./media/active-directory-saas-central-desktop-tutorial/IC769558.png "Eset")
##<a name="enabling-the-application-integration-for-central-desktop"></a>A központi asztali alkalmazás integráció engedélyezése

Ez a szakasz célja, hogyan központi asztali alkalmazás alkalmazással való integráció engedélyezése a szerkezet.

###<a name="to-enable-the-application-integration-for-central-desktop-perform-the-following-steps"></a>Ha engedélyezni szeretné a központi asztali alkalmazás integrációs, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon kattintson a bal oldali navigációs területen kattintson az **Active Directory**.

    ![Az Active Directory] (./media/active-directory-saas-central-desktop-tutorial/IC700993.png "Az Active Directory")

2.  A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3.  Nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson a felső menüben **alkalmazások** .

    ![Alkalmazások] (./media/active-directory-saas-central-desktop-tutorial/IC700994.png "Alkalmazások")

4.  Kattintson a **Hozzáadás** az oldal alján.

    ![Alkalmazás hozzáadása] (./media/active-directory-saas-central-desktop-tutorial/IC749321.png "Alkalmazás hozzáadása")

5.  **Milyen feladatot szeretne tenni** párbeszédpanelen kattintson **a gyűjteményből az alkalmazások felvétele**.

    ![Gallerry az alkalmazás hozzáadása] (./media/active-directory-saas-central-desktop-tutorial/IC749322.png "Gallerry az alkalmazás hozzáadása")

6.  A **Keresés mezőbe**írja be a **Központi asztali**.

    ![Alkalmazás gyűjtemény] (./media/active-directory-saas-central-desktop-tutorial/IC769559.png "Alkalmazás gyűjtemény")

7.  Az eredmény ablaktáblában jelölje ki a **Központi asztali**, és válassza a **kész** , az alkalmazás hozzáadása parancsra.

    ![A központi asztali] (./media/active-directory-saas-central-desktop-tutorial/IC769560.png "A központi asztali")
##<a name="configuring-single-sign-on"></a>Egyszeri bejelentkezés beállítása

Ez a szakasz célja tagolása engedélyezése a felhasználóknak, hogy a saját fiók az összevonási alapján a SAML protokoll használatával Azure Active Directory hitelesítő központi asztalra.  
Ez az eljárás részeként biztosan szükséges számrendszerben-64 kódolt tanúsítvány feltöltése az központi asztali bérlőhöz.  
Ha nem ismeri a ezt az eljárást, olvassa el a [szövegfájlba bináris tanúsítvány konvertálása](http://youtu.be/PlgrzUZ-Y1o)című témakört.



###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Egyszeri bejelentkezés beállításához hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon **Központi asztali** alkalmazás integrációs lapon kattintson **konfigurálása az egyszeri bejelentkezés** a **Konfigurálása az egyszeri bejelentkezés** párbeszédpanel megnyitásához.

    ![Beállítás az egyszeri bejelentkezés] (./media/active-directory-saas-central-desktop-tutorial/IC749323.png "Beállítás az egyszeri bejelentkezés")

2.  **Hogyan szeretné, hogy jelentkezzen be az központi asztali felhasználók** lapon jelölje be **A Microsoft Azure Active Directory Single Sign-On**, és kattintson a **Tovább gombra**.

    ![Beállítás az egyszeri bejelentkezés] (./media/active-directory-saas-central-desktop-tutorial/IC777628.png "Beállítás az egyszeri bejelentkezés")

3.  Az **Alkalmazás URL-cím beállítása** lapon hajtsa végre az alábbi lépéseket, és kattintson a **Tovább gombra**: 

    -   **A központi asztali bejelentkezési az URL-cím** mezőbe írja be a központi asztali bérlői webhely URL-CÍMÉT (például: *http://contoso.centraldesktop.com*).
    -   Az a központi asztali válasz URL-cím mezőbe írja be a központi asztali AssertionConsumerService URL-címe (például: https://contoso.centraldesktop.com/saml2-assertion.php).

    >[AZURE.NOTE] Az érték beolvasása a központi asztali metaadat-alapú (pl.: *http://contoso.centraldesktop.com*).

    ![Állítsa be az app URL-címe] (./media/active-directory-saas-central-desktop-tutorial/IC769561.png "Állítsa be az app URL-címe")

4.  A **Configure egyszeri bejelentkezés a központi asztal** lapon a tanúsítvány letöltése, kattintson a **tanúsítvány letöltése**, és mentse a tanúsítvány fájlt a számítógépen parancsra.

    ![Beállítás az egyszeri bejelentkezés] (./media/active-directory-saas-central-desktop-tutorial/IC769562.png "Beállítás az egyszeri bejelentkezés")

5.  Jelentkezzen be az **Központi asztali** bérlői webhelyen.

6.  Válassza a **Beállítások**, kattintson a **Speciális**gombra, és válassza **Az egyszeri bejelentkezés**.

    ![Telepítés – speciális] (./media/active-directory-saas-central-desktop-tutorial/IC769563.png "Telepítés – speciális")

7.  Az **Egyszeri bejelentkezési a beállítások** lapon hajtsa végre az alábbi lépéseket:

    ![Egyszeri bejelentkezés beállításai] (./media/active-directory-saas-central-desktop-tutorial/IC769564.png "Egyszeri bejelentkezés beállításai")

    1.  Jelölje be az **Engedélyezés SAML v2 egyszeri bejelentkezéses**.
    2.  Az Azure klasszikus portálon **konfigurálása az egyszeri bejelentkezés a központi asztali** lapon a **Kibocsátó URL-címe** értéket másolja és illessze be a **Egyszeri bejelentkezési URL-címe** mezőben lévő értéket.
    3.  Az Azure klasszikus portálon **konfigurálása az egyszeri bejelentkezés központi asztal** lapon a **Távoli bejelentkezési URL-cím** értéket másolja és illessze be a **Egyszeri bejelentkezési URL-cím** mezőben lévő értéket.
    4.  Az Azure klasszikus portálon **konfigurálása az egyszeri bejelentkezés a központi asztali** lapon az **Egyetlen Sign-Out szolgáltatás URL-címe** értéket másolja és illessze be a **SSO kijelentkezés URL-címe** mezőben lévő értéket.

8.  Az **Üzenet aláírása ellenőrzési módszer** csoportban hajtsa végre az alábbi lépéseket:

    ![Üzenet aláírása ellenőrzési módszer] (./media/active-directory-saas-central-desktop-tutorial/IC769565.png "Üzenet aláírása ellenőrzési módszer")

    1.  Jelölje ki a **tanúsítványt**.
    2.  Az **Egyszeri bejelentkezés tanúsítvány** listából válassza ki a **RSH SHA256**.
    3.  Szövegfájl a letöltött tanúsítvány létrehozása, a szövegfájl a tartalom másolása és beillesztése az **Egyszeri bejelentkezés tanúsítvány** mező.  

        >[AZURE.TIP] További információra kíváncsi olvassa el a [bináris tanúsítvány szövegfájlba konvertálása](http://youtu.be/PlgrzUZ-Y1o) című témakört.

    4.  Válassza ki a **megjelenítést a SAMLv2 bejelentkezési lapra mutató hivatkozás**.

9.  Kattintson a **frissítés**gombra.

10. Az Azure klasszikus portálon jelölje be a egyszeri bejelentkezéses konfigurációs megerősítés, és válassza a **kész** , **Állítsa be az egyszeri bejelentkezés** párbeszédpanel bezárásához.

    ![Beállítás az egyszeri bejelentkezés] (./media/active-directory-saas-central-desktop-tutorial/IC769566.png "Beállítás az egyszeri bejelentkezés")
##<a name="configuring-user-provisioning"></a>Felhasználói kiépítési konfigurálása

AAD felhasználóknak engedélyezni szeretné, hogy jelentkezzen be hogy ki kell építenie a központi asztali alkalmazások. Ez a szakasz ismerteti, hogyan AAD felhasználói fiókokat létrehozni a központi programban.

###<a name="to-provision-user-accounts-to-central-desktop"></a>Felhasználói fiókok központi asztalra kiépítése:

1.  Jelentkezzen be az központi asztali bérlői webhelyen.

2.  Nyissa meg a **személyek \> belső tagok**.

3.  Kattintson a **belső tagok hozzáadása**gombra.

    ![Személyek] (./media/active-directory-saas-central-desktop-tutorial/IC781051.png "Személyek")

4.  Az **E-mail címet az új tagok** mezőbe írja be a AAD fiók rendelkezést szeretne, és kattintson a **Tovább gombra**.

    ![Az új tagok e-mail címek] (./media/active-directory-saas-central-desktop-tutorial/IC781052.png "Az új tagok e-mail címek")

5.  Kattintson a **tag hozzáadása belső**.

    ![Belső tag hozzáadása] (./media/active-directory-saas-central-desktop-tutorial/IC781053.png "Belső tag hozzáadása")

    >[AZURE.NOTE] Hozzáadta a felhasználók elemre, hogy fiókja aktiválására szükségük megerősítő hivatkozást tartalmazó e-mailt kap.

>[AZURE.NOTE] Bármely más központi asztali felhasználói fiók létrehozása eszközöket is használhatja, illetve központi asztali rendelkezést AAD felhasználói fiókok által biztosított API-hoz

##<a name="assigning-users"></a>Felhasználók hozzárendelése

Tesztelje a konfigurációt, meg kell adni az Azure Active Directory-felhasználók, használja az alkalmazás elérési útvonalát társításával engedélyezni szeretné.

###<a name="to-assign-users-to-central-desktop-perform-the-following-steps"></a>Felhasználók hozzárendelése központi asztali, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon próba-fiók létrehozása.

2.  A **Központi asztali** alkalmazás integrációs lapon kattintson a **hozzárendelését a felhasználókhoz**.

    ![Adhatnak a felhasználóknak] (./media/active-directory-saas-central-desktop-tutorial/IC769567.png "Adhatnak a felhasználóknak")

3.  Jelölje ki a próba-felhasználó, kattintson a **hozzárendelni**, és kattintson az **Igen gombra** kattintva erősítse meg a hozzárendelés.

    ![Igen] (./media/active-directory-saas-central-desktop-tutorial/IC767830.png "Igen")

Ha meg szeretné tesztelje a egyszeri bejelentkezéses, Access Panel megnyitásához. Az Access ablaktábla, lásd: [Bevezetés az Access panelre](active-directory-saas-access-panel-introduction.md).
