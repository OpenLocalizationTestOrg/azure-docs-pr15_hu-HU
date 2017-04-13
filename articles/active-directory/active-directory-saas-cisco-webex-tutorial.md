<properties 
    pageTitle="Oktatóprogram: Azure Active Directory-integráció a Cisco Webex |} Microsoft Azure" 
    description="Megtudhatja, hogyan használhatja a Cisco Webex az Azure Active Directory ahhoz, hogy az egyszeri bejelentkezés, automatikus kiépítési és az egyéb!" 
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

#<a name="tutorial-azure-active-directory-integration-with-cisco-webex"></a>Oktatóprogram: Azure Active Directory-integráció a Cisco Webex

Ebben az oktatóanyagban célja integrálása az Azure és Cisco Webex megjelenítéséhez.  
Az alkalmazási példát, ebben az oktatóanyagban tagolt feltételezi, hogy már rendelkezik az alábbiakat:

-   Érvényes Azure előfizetés
-   Cisco Webex bérlői webhelyre

Ebben az oktatóanyagban befejeztével az Azure Active Directory felhasználók Cisco Webex rendelt tudnak egyetlen bejelentkezés az alkalmazásba, a Cisco Webex vállalati webhely (service provider által kezdeményezett bejelentkezés) és használatáról a [Bevezetés az Access panelre](active-directory-saas-access-panel-introduction.md).

A következő építőelemek az alkalmazási példát, ebben az oktatóanyagban tagolt áll:

1.  Az alkalmazás integrációját Cisco Webex engedélyezése
2.  Egyszeri bejelentkezés beállítása
3.  Felhasználói kiépítési konfigurálása
4.  Felhasználók hozzárendelése

![Eset] (./media/active-directory-saas-cisco-webex-tutorial/IC777614.png "Eset")
##<a name="enabling-the-application-integration-for-cisco-webex"></a>Az alkalmazás integrációját Cisco Webex engedélyezése

Ez a szakasz célja, hogyan Cisco Webex az alkalmazás alkalmazással való integráció engedélyezése a szerkezet.

###<a name="to-enable-the-application-integration-for-cisco-webex-perform-the-following-steps"></a>Ahhoz, hogy az alkalmazás integrációját Cisco Webex, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon kattintson a bal oldali navigációs területen kattintson az **Active Directory**.

    ![Az Active Directory] (./media/active-directory-saas-cisco-webex-tutorial/IC700993.png "Az Active Directory")

2.  A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3.  Nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson a felső menüben **alkalmazásokat** .

    ![Alkalmazások] (./media/active-directory-saas-cisco-webex-tutorial/IC700994.png "Alkalmazások")

4.  Kattintson a **Hozzáadás** az oldal alján.

    ![Alkalmazás hozzáadása] (./media/active-directory-saas-cisco-webex-tutorial/IC749321.png "Alkalmazás hozzáadása")

5.  **Milyen feladatot szeretne tenni** párbeszédpanelen kattintson **a gyűjteményből az alkalmazások felvétele**.

    ![Gallerry az alkalmazás hozzáadása] (./media/active-directory-saas-cisco-webex-tutorial/IC749322.png "Gallerry az alkalmazás hozzáadása")

6.  A **Keresés mezőbe**írja be a **Cisco Webex**.

    ![Alkalmazás gyűjtemény] (./media/active-directory-saas-cisco-webex-tutorial/IC777615.png "Alkalmazás gyűjtemény")

7.  Az eredmény ablaktáblában jelölje ki a **Cisco Webex**, és válassza a **kész** , az alkalmazás hozzáadása gombra.

    ![Cisco Webex] (./media/active-directory-saas-cisco-webex-tutorial/IC777616.png "Cisco Webex")
##<a name="configuring-single-sign-on"></a>Egyszeri bejelentkezés beállítása

Ez a szakasz célja tagolása engedélyezése a felhasználóknak, hogy Cisco Webex hitelesíteni a fiókkal az összevonási alapján a SAML protokoll használatával Azure AD.  
Ez az eljárás részeként biztosan szükséges számrendszerben-64 kódolt tanúsítványok létrehozásáról.  
Ha nem ismeri a ezt az eljárást, megtudhatja, [hogy miként szövegfájlba bináris tanúsítvány konvertálása](http://youtu.be/PlgrzUZ-Y1o)

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Egyszeri bejelentkezés beállításához hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon **Cisco Webex** alkalmazás integrációs lapon kattintson **konfigurálása az egyszeri bejelentkezés** a **Konfigurálása az egyszeri bejelentkezés** párbeszédpanel megnyitásához.

    ![Beállítás az egyszeri bejelentkezés] (./media/active-directory-saas-cisco-webex-tutorial/IC777617.png "Beállítás az egyszeri bejelentkezés")

2.  **Hogyan szeretné, hogy jelentkezzen be az Cisco Webex felhasználók** lapon jelölje be **A Microsoft Azure Active Directory Single Sign-On**, és kattintson a **Tovább gombra**.

    ![Beállítás az egyszeri bejelentkezés] (./media/active-directory-saas-cisco-webex-tutorial/IC777618.png "Beállítás az egyszeri bejelentkezés")

3.  Az **Alkalmazás URL-cím beállítása** lapon hajtsa végre az alábbi lépéseket, és kattintson a **Tovább gombra**.

    ![Állítsa be az app URL-címe] (./media/active-directory-saas-cisco-webex-tutorial/IC777619.png "Állítsa be az app URL-címe")

    1.  Az **Folyamata a URL-cím** mezőbe írja be a Cisco Webex bérlői webhely URL-CÍMÉT (például: *http://contoso.webex.com*).
    2.  Az **Cisco Webex válasz URL-cím** mezőbe írja be a **Cisco Webex AssertionConsumerService URL-CÍMÉT** (például: *https://company.webex.com/dispatcher/SAML2AuthService?siteurl=company*).

4.  A **Configure egyszeri bejelentkezés Cisco Webex a** lapon a tanúsítvány letöltése, kattintson a **tanúsítvány letöltése**, és mentse a tanúsítvány fájlt a számítógépen parancsra.

    ![Beállítás az egyszeri bejelentkezés] (./media/active-directory-saas-cisco-webex-tutorial/IC777620.png "Beállítás az egyszeri bejelentkezés")

5.  A különböző webes böngészőablakban jelentkezzen be a Cisco Webex vállalati webhely rendszergazdaként.

6.  A felső sávon kattintson a **Hely felügyelete**.

    ![Hely felügyelete] (./media/active-directory-saas-cisco-webex-tutorial/IC777621.png "Hely felügyelete")

7.  A **Webhely kezelése** csoportban kattintson az **Egyszeri bejelentkezés konfigurációs**.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-cisco-webex-tutorial/IC777622.png "Egyszeri bejelentkezés beállítása")

8.  A szövetséges webes SSO beállítások csoportban hajtsa végre az alábbi lépéseket:

    ![Összevont egyszeri bejelentkezés beállítása] (./media/active-directory-saas-cisco-webex-tutorial/IC777623.png "Összevont egyszeri bejelentkezés beállítása")

    1.  **Összevonás Protocol (protokoll)** listában jelölje ki **a SAML 2.0**.
    2.  A letöltött tanúsítvány **Alap-64-címként kódolt** fájl létrehozása  

        >[AZURE.TIP] További információra kíváncsi olvassa el a [bináris tanúsítvány szövegfájlba konvertálása](http://youtu.be/PlgrzUZ-Y1o) című témakört.

    3.  Nyissa meg az alap-64 kódolt tanúsítvány a Jegyzettömbben, és másolja a tartalmát.
    4.  Kattintson a **SAML-metaadatok importálása**, és illessze be az alap-64 kódolt tanúsítvány.
    5.  Az Azure klasszikus portálon **konfigurálása az egyszeri bejelentkezés a Cisco Webex** párbeszédpanel lapon a **Kibocsátó URL-címe** értéket másolja és illessze be a **kiállító SAML (IdP azonosító)** mezőben lévő értéket.
    6.  Az Azure klasszikus portálon **konfigurálása az egyszeri bejelentkezés a Cisco Webex** párbeszédpanel lapon a **Távoli bejelentkezési URL-cím** értéket másolja és illessze be a **Felhasználói SSO szolgáltatás bejelentkezési URL-címe** mezőben lévő értéket.
    7.  A **NameID formátum** listájában jelölje ki az **E-mail címet**.
    8.  Az **AuthnContextClassRef** mezőbe írja be a **Urn: oasis: nevek: tc: SAML:2.0:ac:classes:Password**.
    9.  Az Azure klasszikus portálon **konfigurálása az egyszeri bejelentkezés a Cisco Webex** párbeszédpanel lapon a **Távoli kijelentkezés URL-címe** értéket másolja és illessze be a **Felhasználói SSO szolgáltatás kijelentkezés URL-címe** mezőben lévő értéket.
    10. Kattintson a **frissítés**gombra.

9.  Az Azure klasszikus portálon **konfigurálása az egyszeri bejelentkezés a Cisco Webex** párbeszédpanel lapon jelölje ki az egyszeri bejelentkezéses konfigurációs megerősítést kérő, és válassza a **kész**gombra.

    ![Beállítás az egyszeri bejelentkezés] (./media/active-directory-saas-cisco-webex-tutorial/IC777624.png "Beállítás az egyszeri bejelentkezés")
##<a name="configuring-user-provisioning"></a>Felhasználói kiépítési konfigurálása

Ahhoz, hogy az Azure Active Directory-felhasználóknak, hogy jelentkezzen be a Cisco Webex, hogy ki kell építenie Cisco Webex be.  
Cisco Webex, ha a feladat manuális kiépítési.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>A felhasználói fiókok kiépítése, hajtsa végre az alábbi lépéseket:

1.  Jelentkezzen be az **Cisco Webex** bérlői webhelyen.

2.  Nyissa meg a **felhasználókezelés \> felhasználó hozzáadása**.

    ![Felhasználók hozzáadása] (./media/active-directory-saas-cisco-webex-tutorial/IC777625.png "Felhasználók hozzáadása")

3.  A felhasználó hozzáadása csoportban hajtsa végre az alábbi lépéseket:

    ![Felhasználó hozzáadása] (./media/active-directory-saas-cisco-webex-tutorial/IC777626.png "Felhasználó hozzáadása")

    1.  **Fiók típusa**csoportban jelölje ki a **Host**.
    2.  Az adatokat meglévő Azure AD-felhasználó, írja be a következő szövegdobozok: **utónevet, utolsó**, **felhasználó nevét**, **E-mail**, **jelszót**, **A jelszó megerősítése**.
    3.  Kattintson a **hozzáadása**gombra.

>[AZURE.NOTE] Bármely más Cisco Webex felhasználói fiók létrehozása eszközöket is használhatja, illetve Cisco Webex rendelkezést AAD felhasználói fiókok által biztosított API-khoz.

##<a name="assigning-users"></a>Felhasználók hozzárendelése

Tesztelje a konfigurációt, meg kell adni az Azure Active Directory-felhasználók, használja az alkalmazás elérési útvonalát társításával engedélyezni szeretné.

###<a name="to-assign-users-to-cisco-webex-perform-the-following-steps"></a>Felhasználók hozzárendelése Cisco Webex, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon próba-fiók létrehozása.

2.  Kattintson a **Cisco Webex **alkalmazás integrációs lap **hozzárendelését a felhasználókhoz**.

    ![Adhatnak a felhasználóknak] (./media/active-directory-saas-cisco-webex-tutorial/IC777627.png "Adhatnak a felhasználóknak")

3.  Jelölje ki a próba-felhasználó, kattintson a **hozzárendelni**, és kattintson az **Igen gombra** kattintva erősítse meg a hozzárendelés.

    ![Igen] (./media/active-directory-saas-cisco-webex-tutorial/IC767830.png "Igen")

Ha meg szeretné tesztelje a egyszeri bejelentkezéses, Access Panel megnyitásához. Ha többet szeretne tudni az Access panelen című témakörben [az Access-panelen](active-directory-saas-access-panel-introduction.md).
