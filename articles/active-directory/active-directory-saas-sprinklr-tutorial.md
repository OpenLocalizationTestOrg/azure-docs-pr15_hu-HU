<properties 
    pageTitle="Oktatóprogram: Azure Active Directory-integráció a Sprinklr |} Microsoft Azure" 
    description="Megtudhatja, hogyan használhatja a Sprinklr az Azure Active Directory ahhoz, hogy az egyszeri bejelentkezés, automatikus kiépítési és az egyéb!" 
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

#<a name="tutorial-azure-active-directory-integration-with-sprinklr"></a>Oktatóprogram: Azure Active Directory-integráció a Sprinklr
  
Ebben az oktatóanyagban célja integrálása az Azure és Sprinklr megjelenítéséhez.  
Az alkalmazási példát, ebben az oktatóanyagban tagolt feltételezi, hogy már rendelkezik az alábbiakat:

-   Érvényes Azure előfizetés
-   Sprinklr bérlői webhelyre
  
Ebben az oktatóanyagban befejeztével az Azure Active Directory-felhasználók Sprinklr rendelt fogja tudni az alkalmazás a Sprinklr vállalati webhely (service provider által kezdeményezett bejelentkezés) és használatáról a [Bevezetés az Access panelen](active-directory-saas-access-panel-introduction.md)az egyszeri bejelentkezési.
  
A következő építőelemek az alkalmazási példát, ebben az oktatóanyagban tagolt áll:

1.  Az alkalmazás integrációját Sprinklr engedélyezése
2.  Egyszeri bejelentkezés beállítása
3.  Felhasználói kiépítési konfigurálása
4.  Felhasználók hozzárendelése

![Eset] (./media/active-directory-saas-sprinklr-tutorial/IC782900.png "Eset")

##<a name="enabling-the-application-integration-for-sprinklr"></a>Az alkalmazás integrációját Sprinklr engedélyezése
  
Ez a szakasz célja, hogyan Sprinklr az alkalmazás alkalmazással való integráció engedélyezése a szerkezet.

###<a name="to-enable-the-application-integration-for-sprinklr-perform-the-following-steps"></a>Ahhoz, hogy az alkalmazás integrációját Sprinklr, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon kattintson a bal oldali navigációs területen kattintson az **Active Directory**.

    ![Az Active Directory] (./media/active-directory-saas-sprinklr-tutorial/IC700993.png "Az Active Directory")

2.  A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3.  Nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson a felső menüben **alkalmazásokat** .

    ![Alkalmazások] (./media/active-directory-saas-sprinklr-tutorial/IC700994.png "Alkalmazások")

4.  Kattintson a **Hozzáadás** az oldal alján.

    ![Alkalmazás hozzáadása] (./media/active-directory-saas-sprinklr-tutorial/IC749321.png "Alkalmazás hozzáadása")

5.  **Milyen feladatot szeretne tenni** párbeszédpanelen kattintson **a gyűjteményből az alkalmazások felvétele**.

    ![Gallerry az alkalmazás hozzáadása] (./media/active-directory-saas-sprinklr-tutorial/IC749322.png "Gallerry az alkalmazás hozzáadása")

6.  A **Keresés mezőbe**írja be a **Sprinklr**.

    ![Alkalmazás gyűjtemény] (./media/active-directory-saas-sprinklr-tutorial/IC782901.png "Alkalmazás gyűjtemény")

7.  Az eredmény ablaktáblában jelölje ki a **Sprinklr**, és válassza a **kész** , az alkalmazás hozzáadása gombra.

    ![Sprinklr] (./media/active-directory-saas-sprinklr-tutorial/IC782902.png "Sprinklr")

##<a name="configuring-single-sign-on"></a>Egyszeri bejelentkezés beállítása
  
Ez a szakasz célja tagolása engedélyezése a felhasználóknak, hogy Sprinklr hitelesíteni a fiókkal az összevonási alapján a SAML protokoll használatával Azure AD.  
Ez az eljárás részeként biztosan szükséges számrendszerben-64 kódolt tanúsítvány fájlt.  
Ha nem ismeri a ezt az eljárást, olvassa el a [szövegfájlba bináris tanúsítvány konvertálása](http://youtu.be/PlgrzUZ-Y1o)című témakört.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Egyszeri bejelentkezés beállításához hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon **Sprinklr** alkalmazás integrációs lapon kattintson **konfigurálása az egyszeri bejelentkezés** a **Konfigurálása az egyszeri bejelentkezés** párbeszédpanel megnyitásához.

    ![Beállítás az egyszeri bejelentkezés] (./media/active-directory-saas-sprinklr-tutorial/IC782903.png "Beállítás az egyszeri bejelentkezés")

2.  **Hogyan szeretné, hogy jelentkezzen be az Sprinklr felhasználók** lapon jelölje be **A Microsoft Azure Active Directory Single Sign-On**, és kattintson a **Tovább gombra**.

    ![Beállítás az egyszeri bejelentkezés] (./media/active-directory-saas-sprinklr-tutorial/IC782904.png "Beállítás az egyszeri bejelentkezés")

3.  **App URL-címe beállítása** lapon az **Sprinklr bejelentkezési az URL** mezőbe írja be az URL-t, a következő mintát "*https://\<bérlői neve\>. sprinklr.com*", majd kattintson a **Tovább**gombra.

    ![Állítsa be az App URL-címe] (./media/active-directory-saas-sprinklr-tutorial/IC782905.png "Állítsa be az App URL-címe")

4.  A **Configure egyszeri bejelentkezés Sprinklr a** lapon a tanúsítvány letöltése, kattintson a **tanúsítvány letöltése**, és mentse a tanúsítvány fájlt a számítógépen parancsra.

    ![Beállítás az egyszeri bejelentkezés] (./media/active-directory-saas-sprinklr-tutorial/IC782906.png "Beállítás az egyszeri bejelentkezés")

5.  A különböző webes böngészőablakban jelentkezzen be a Sprinklr vállalati webhely rendszergazdaként.

6.  Nyissa meg a **felügyeleti \> beállítások**.

    ![Felügyelete] (./media/active-directory-saas-sprinklr-tutorial/IC782907.png "Felügyelete")

7.  Nyissa meg a **Partner kezelése \> egyszeri bejelentkezési** kattintson a bal oldali ablaktáblából.

    ![Partnerek kezelése] (./media/active-directory-saas-sprinklr-tutorial/IC782908.png "Partnerek kezelése")

8.  Válassza a **+ egyszeri bejelentkezési bővítmények**.

    ![Egyszeri bejelentkezés-ikonok] (./media/active-directory-saas-sprinklr-tutorial/IC782909.png "Egyszeri bejelentkezés-ikonok")

9.  **Egyszeri bejelentkezés** lapon hajtsa végre az alábbi lépéseket:

    ![Egyszeri bejelentkezés-ikonok] (./media/active-directory-saas-sprinklr-tutorial/IC782910.png "Egyszeri bejelentkezés-ikonok")

    1.  Az **neve** mezőbe írja be a konfigurációban nevét (például: *WAADSSOTest*).
    2.  Jelölje ki, hogy **engedélyezve van**.
    3.  Jelölje be az **Új SSO tanúsítványt**.
    4.  A letöltött tanúsítvány **Alap-64-címként kódolt** fájl létrehozása  

        >[AZURE.TIP] További információra kíváncsi olvassa el a [bináris tanúsítvány szövegfájlba konvertálása](http://youtu.be/PlgrzUZ-Y1o) című témakört.

    5.  Nyissa meg az alap-64 kódolt tanúsítvány a Jegyzettömbben, a tartalmát másolja a vágólapra és beilleszti az **Identitás-szolgáltató tanúsítványának** mezőbe
    6.  Az Azure klasszikus portálon **konfigurálása az egyszeri bejelentkezés a Sprinklr** párbeszédpanel lapon másolja az **Identitás-szolgáltató azonosítója** értéket, és illessze be a **Szervezet azonosítója** mezőben lévő értéket.
    7.  Az Azure klasszikus portálon **konfigurálása az egyszeri bejelentkezés a Sprinklr** párbeszédpanel lapon a **Távoli bejelentkezési URL-cím** értéket másolja és illessze be a **Identitás szolgáltató bejelentkezési URL-cím** mezőben lévő értéket.
    8.  Az Azure klasszikus portálon **konfigurálása az egyszeri bejelentkezés a Sprinklr** párbeszédpanel lapon a **Távoli kijelentkezés URL-címe** értéket másolja és illessze be a **Identitás szolgáltató kijelentkezés URL-címe** mezőben lévő értéket.
    9.  **SAML felhasználói azonosító típusa**csoportban jelölje ki a **állítás tartalmaz felhasználó "s sprinklr.com felhasználónév**.
    10. **SAML felhasználói azonosító helyét**jelölje ki a **felhasználói azonosító szerepel-e a tárgy kimutatás az azonosító neve elemet**.
    11. Zárja be **Mentés**.

        ![SAML] (./media/active-directory-saas-sprinklr-tutorial/IC782911.png "SAML")

10. Az Azure klasszikus portálon jelölje be a egyszeri bejelentkezéses konfigurációs megerősítés, és válassza a **kész** , **Állítsa be az egyszeri bejelentkezés** párbeszédpanel bezárásához.

    ![Beállítás az egyszeri bejelentkezés] (./media/active-directory-saas-sprinklr-tutorial/IC782912.png "Beállítás az egyszeri bejelentkezés")

##<a name="configuring-user-provisioning"></a>Felhasználói kiépítési konfigurálása
  
AAD felhasználóknak engedélyezni szeretné, hogy jelentkezzen be hogy ki kell építenie belül az Sprinklr alkalmazás hozzáférést.  
Ez a szakasz ismerteti, hogyan hozhat létre AAD felhasználói fiókot Sprinklr belül.

###<a name="to-provision-a-user-account-in-sprinklr-perform-the-following-steps"></a>A felhasználói fiókok Sprinklr kiépítése, hajtsa végre az alábbi lépéseket:

1.  Jelentkezzen be a Sprinklr vállalati webhely rendszergazdaként.

2.  Nyissa meg a **felügyeleti \> beállítások**.

    ![Felügyelete] (./media/active-directory-saas-sprinklr-tutorial/IC782907.png "Felügyelete")

3.  Nyissa meg a **ügyfél kezelése \> felhasználók** a bal oldali ablaktáblából.

    ![Beállítások] (./media/active-directory-saas-sprinklr-tutorial/IC782914.png "Beállítások")

4.  Kattintson a **felhasználó hozzáadása**elemre.

    ![Beállítások] (./media/active-directory-saas-sprinklr-tutorial/IC782915.png "Beállítások")

5.  A **felhasználó szerkesztése** párbeszédpanelen végezze el az alábbi lépéseket:

    ![Felhasználó szerkesztése] (./media/active-directory-saas-sprinklr-tutorial/IC782916.png "Felhasználó szerkesztése")

    1.  **E-mailek**, **Utónév** és **Vezetéknév** szövegdobozok, és írja be az adatokat az Azure Active Directory felhasználói fiók kívánt rendelkezés.
    2.  Jelölje be a **jelszót tiltható le**.
    3.  Válasszon egy **nyelvet**.
    4.  Jelölje ki a **felhasználó típusa**.
    5.  Kattintson a **frissítés**gombra.

    >[AZURE.IMPORTANT] **Jelszó letiltott** kell jelölni ahhoz, hogy a felhasználót, hogy egy identitásszolgáltató keresztül jelentkezzen be.

6.  Nyissa meg a **szerepkört**, és hajtsa végre az alábbi lépéseket:

    ![Partner szerepkörök] (./media/active-directory-saas-sprinklr-tutorial/IC782917.png "Partner szerepkörök")

    1.  A **globális** listából válassza ki a **összes\_engedélyek**.
    2.  Kattintson a **frissítés**gombra.

>[AZURE.NOTE] Bármely más Sprinklr felhasználói fiók létrehozása eszközöket is használhatja, vagy Sprinklr hozhatók létre az Azure Active Directory-fiókokkal által biztosított API-khoz.

##<a name="assigning-users"></a>Felhasználók hozzárendelése
  
Tesztelje a konfigurációt, meg kell adni az Azure Active Directory-felhasználók, használja az alkalmazás elérési útvonalát társításával engedélyezni szeretné.

###<a name="to-assign-users-to-sprinklr-perform-the-following-steps"></a>Felhasználók hozzárendelése Sprinklr, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon próba-fiók létrehozása.

2.  Kattintson a **Sprinklr **alkalmazás integrációs lap **hozzárendelését a felhasználókhoz**.

    ![Adhatnak a felhasználóknak] (./media/active-directory-saas-sprinklr-tutorial/IC782918.png "Adhatnak a felhasználóknak")

3.  Jelölje ki a próba-felhasználó, kattintson a **hozzárendelni**, és kattintson az **Igen gombra** kattintva erősítse meg a hozzárendelés.

    ![Igen] (./media/active-directory-saas-sprinklr-tutorial/IC767830.png "Igen")
  
Ha meg szeretné tesztelje a egyszeri bejelentkezéses, Access Panel megnyitásához. Ha többet szeretne tudni az Access panelen című témakörben [az Access-panelen](active-directory-saas-access-panel-introduction.md).