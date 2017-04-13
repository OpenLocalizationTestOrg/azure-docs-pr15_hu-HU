<properties 
    pageTitle="Oktatóprogram: Azure Active Directory-integráció a Zscaler |} Microsoft Azure" 
    description="Megtudhatja, hogyan használhatja a Zscaler az Azure Active Directory ahhoz, hogy az egyszeri bejelentkezés, automatikus kiépítési és az egyéb!." 
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
    ms.date="08/16/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-zscaler"></a>Oktatóprogram: Azure Active Directory-integráció a Zscaler
  
Ebben az oktatóanyagban célja integrálása az Azure és Zscaler megjelenítéséhez. Az alkalmazási példát, ebben az oktatóanyagban tagolt feltételezi, hogy már rendelkezik az alábbiakat:

-   Érvényes Azure előfizetés
-   A Zscaler bérlői webhelyre
  
A következő építőelemek az alkalmazási példát, ebben az oktatóanyagban tagolt áll:

1.  Az alkalmazás integrációját Zscaler engedélyezése
2.  Egyszeri bejelentkezés beállítása
3.  A proxykiszolgáló beállításainak konfigurálása
4.  Felhasználói kiépítési konfigurálása
5.  Felhasználók hozzárendelése

![Eset] (./media/active-directory-saas-zscaler-tutorial/IC769226.png "Eset")

##<a name="enabling-the-application-integration-for-zscaler"></a>Az alkalmazás integrációját Zscaler engedélyezése
  
Ez a szakasz célja, hogyan Zscaler az alkalmazás alkalmazással való integráció engedélyezése a szerkezet.

###<a name="to-enable-the-application-integration-for-zscaler-perform-the-following-steps"></a>Ahhoz, hogy az alkalmazás integrációját Zscaler, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon kattintson a bal oldali navigációs területen kattintson az **Active Directory**.

    ![Az Active Directory] (./media/active-directory-saas-zscaler-tutorial/IC700993.png "Az Active Directory")

2.  A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3.  Nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson a felső menüben **alkalmazásokat** .

    ![Alkalmazások] (./media/active-directory-saas-zscaler-tutorial/IC700994.png "Alkalmazások")

4.  Kattintson a **Hozzáadás** az oldal alján.

    ![Alkalmazás hozzáadása] (./media/active-directory-saas-zscaler-tutorial/IC749321.png "Alkalmazás hozzáadása")

5.  **Milyen feladatot szeretne tenni** párbeszédpanelen kattintson **a gyűjteményből az alkalmazások felvétele**.

    ![Gallerry az alkalmazás hozzáadása] (./media/active-directory-saas-zscaler-tutorial/IC749322.png "Gallerry az alkalmazás hozzáadása")

6.  A **Keresés mezőbe**írja be a **Zscaler**.

    ![Alkalmazás gyűjtemény] (./media/active-directory-saas-zscaler-tutorial/IC769227.png "Alkalmazás gyűjtemény")

7.  Az eredmény ablaktáblában jelölje ki a **Zscaler**, és válassza a **kész** , az alkalmazás hozzáadása gombra.

    ![Zscaler] (./media/active-directory-saas-zscaler-tutorial/IC769228.png "Zscaler")

##<a name="configuring-single-sign-on"></a>Egyszeri bejelentkezés beállítása
  
Ez a szakasz célja tagolása engedélyezése a felhasználóknak, hogy Zscaler hitelesíteni a fiókkal az összevonási alapján a SAML protokoll használatával Azure AD.  
Ez az eljárás részeként szükség van egy tanúsítványt feltöltése Zscaler.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Egyszeri bejelentkezés beállításához hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon **Zscaler** alkalmazás integrációs lapon kattintson **konfigurálása az egyszeri bejelentkezés** a **Konfigurálása az egyszeri bejelentkezés** párbeszédpanel megnyitásához.

    ![Egyszeri bejelentkezés engedélyezése] (./media/active-directory-saas-zscaler-tutorial/IC769229.png "Egyszeri bejelentkezés engedélyezése")

2.  **Hogyan szeretné, hogy jelentkezzen be az Zscaler felhasználók** lapon jelölje be **A Microsoft Azure Active Directory Single Sign-On**, és kattintson a **Tovább gombra**.

    ![Bejelentkezés egyszeri konfigurálása] (./media/active-directory-saas-zscaler-tutorial/IC769230.png "Bejelentkezés egyszeri konfigurálása")

3.  A **App URL-címe beállítása** lapon az **Zscaler bejelentkezési az URL** mezőbe írja be a bejelentkezési Zscaler kapott URL-CÍMÉT, és kattintson a **Tovább gombra**: 

    >[AZURE.NOTE] Ha nem tudja, hogy mi az a bejelentkezési URL-CÍMÉT, forduljon a Zscaler támogatási csoport.

    ![Állítsa be az app URL-címe] (./media/active-directory-saas-zscaler-tutorial/IC769231.png "Állítsa be az app URL-címe")

4.  A **beállítás az egyszeri bejelentkezés Zscaler a** lapon hajtsa végre az alábbi lépéseket:

    ![Beállítás az egyszeri bejelentkezés] (./media/active-directory-saas-zscaler-tutorial/IC769232.png "Beállítás az egyszeri bejelentkezés")

    1.  Kattintson a **tanúsítvány letöltése**, és mentse a tanúsítvány fájlként helyileg **c:\\Zscaler.cer**.
    2.  **Hitelesítési kérelem URL-cím** másolása a vágólapra.

5.  Jelentkezzen be az Zscaler bérlőhöz.

6.  A felső sávon kattintson a **felügyeleti**.

    ![Felügyelete] (./media/active-directory-saas-zscaler-tutorial/IC769486.png "Felügyelete")

7.  A **rendszergazdák kezelése és szerepkörök**a **felhasználók kezelése és a hitelesítési**gombra.

    ![A rendszergazdák és szerepkörök kezelése] (./media/active-directory-saas-zscaler-tutorial/IC769487.png "A rendszergazdák és szerepkörök kezelése")

8.  A **Hitelesítési beállítás kiválasztása a szervezet** csoportban hajtsa végre az alábbi lépéseket:

    ![Hitelesítési beállítások kiválasztása] (./media/active-directory-saas-zscaler-tutorial/IC769488.png "Hitelesítési beállítások kiválasztása")

    1.  Jelölje be a **hitelesítés használatával SAML Single Sign-on**.
    2.  Kattintson a **SAML egyszeri bejelentkezéses paraméterek beállítása**gombra.

9.  A **Konfigurálása SAML egyszeri bejelentkezéses paraméterek** párbeszédpanelen lapon hajtsa végre az alábbi lépéseket, és válassza a **Kész gombra**:

    ![Töltse fel a tanúsítvány] (./media/active-directory-saas-zscaler-tutorial/IC769489.png "Töltse fel a tanúsítvány")

    1.  Az **URL-címet, amelyre felhasználók hitelesítéshez elküldi a SAML-portál** mezőbe illessze be az Azure klasszikus portálról a **hitelesítési kérelem URL-címe** mező értékét.
    2.  Az **attribútumot tartalmazó bejelentkezési neve** mezőbe írja be a **NameID**.
    3.  A **Feltöltés nyilvános SSL-tanúsítvány** mezőben töltse fel a az Azure klasszikus portáljáról letöltött tanúsítvány.
    4.  Jelölje ki a **SAML automatikus – kiépítési engedélyezése**.

10. A **Felhasználói hitelesítés konfigurálása** párbeszédpanel lapon hajtsa végre az alábbi lépéseket:

    ![Felhasználói hitelesítés konfigurálása] (./media/active-directory-saas-zscaler-tutorial/IC769490.png "Felhasználói hitelesítés konfigurálása")

    1.  Kattintson a **Mentés**gombra.
    2.  Kattintson az **Aktiválás**gombra.

11. Az Azure klasszikus portálon jelölje be a egyszeri bejelentkezéses konfigurációs megerősítés, és válassza a **kész** , **Állítsa be az egyszeri bejelentkezés** párbeszédpanel bezárásához.

    ![Beállítás az egyszeri bejelentkezés] (./media/active-directory-saas-zscaler-tutorial/IC769491.png "Beállítás az egyszeri bejelentkezés")

##<a name="configuring-proxy-settings"></a>A proxykiszolgáló beállításainak konfigurálása

###<a name="to-configure-the-proxy-settings-in-internet-explorer"></a>A Proxybeállítások konfigurálása az Internet Explorerben

1.  **Az Internet Explorer**indítása.

2.  Kattintson az **Internetbeállítások** párbeszédpanel megnyitása az **eszközök** menü **Internetbeállítások** kijelölése

    ![Internetbeállítások párbeszédpanel] (./media/active-directory-saas-zscaler-tutorial/IC769492.png "Internetbeállítások párbeszédpanel")

3.  Kattintson a **kapcsolatok** fülre.

    ![Kapcsolatok] (./media/active-directory-saas-zscaler-tutorial/IC769493.png "Kapcsolatok")

4.  Kattintson a **helyi hálózati beállítások** , a **Helyi hálózati beállítások** párbeszédpanel megnyitásához.

5.  A Proxy server csoportban hajtsa végre az alábbi lépéseket:

    ![Proxykiszolgáló] (./media/active-directory-saas-zscaler-tutorial/IC769494.png "Proxykiszolgáló")

    1.  Jelölje ki a proxykiszolgáló használata a helyi hálózaton.
    2.  Az a cím mezőbe írja be a **gateway.zscalertwo.net**.
    3.  A Port mezőbe írja be a **80**.
    4.  Jelölje ki a **proxy figyelmen kívül hagyása helyi címeknél**.
    5.  Kattintson **az OK gombra** kattintva zárja be a **helyi hálózat (LAN) beállításai** párbeszédpanelen.

6.  Kattintson az **OK gombra** az **Internetbeállítások** párbeszédpanel bezárásához.

##<a name="configuring-user-provisioning"></a>Felhasználói kiépítési konfigurálása
  
Ahhoz, hogy az Azure Active Directory-felhasználóknak, hogy jelentkezzen be a Zscaler, hogy ki kell építenie Zscaler be.  
Zscaler, ha a feladat manuális kiépítési.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Felhasználói kiépítési beállításához hajtsa végre az alábbi lépéseket:

1.  Jelentkezzen be az **Zscaler** bérlőhöz.

2.  Kattintson a **felügyeleti**.

    ![Felügyelete] (./media/active-directory-saas-zscaler-tutorial/IC781035.png "Felügyelete")

3.  Kattintson a **felhasználók kezelése**gombra.

    ![Felhasználókezelés] (./media/active-directory-saas-zscaler-tutorial/IC781036.png "Felhasználókezelés")

4.  A **felhasználók** lapon kattintson a **Hozzáadás**gombra.

    ![Hozzáadása] (./media/active-directory-saas-zscaler-tutorial/IC781037.png "Hozzáadása")

5.  A felhasználó hozzáadása csoportban hajtsa végre az alábbi lépéseket:

    ![Felhasználó hozzáadása] (./media/active-directory-saas-zscaler-tutorial/IC781038.png "Felhasználó hozzáadása")

    1.  Írja be a **felhasználóazonosító**, a **Felhasználó megjelenítendő név**, a **jelszót**, a **Jelszó megerősítése**, és válassza a **csoportok** és a **részleg** kiépítése kívánt érvényes AAD fiók.
    2.  Kattintson a **Mentés**gombra.

>[AZURE.NOTE] Bármely más Zscaler felhasználói fiók létrehozása eszközzel vagy Zscaler rendelkezést AAD felhasználói fiókok által biztosított API-khoz.

##<a name="assigning-users"></a>Felhasználók hozzárendelése
  
Tesztelje a konfigurációt, meg kell adni az Azure Active Directory-felhasználók, használja az alkalmazás elérési útvonalát társításával engedélyezni szeretné.

###<a name="to-assign-users-to-zscaler-perform-the-following-steps"></a>Felhasználók hozzárendelése Zscaler, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon próba-fiók létrehozása.

2.  Kattintson a **Zscaler** alkalmazás integrációs lap **hozzárendelését a felhasználókhoz**.

    ![Adhatnak a felhasználóknak] (./media/active-directory-saas-zscaler-tutorial/IC769495.png "Adhatnak a felhasználóknak")

3.  Jelölje ki a próba-felhasználó, kattintson a **hozzárendelni**, és kattintson az **Igen gombra** kattintva erősítse meg a hozzárendelés.

    ![Igen] (./media/active-directory-saas-zscaler-tutorial/IC767830.png "Igen")
  
Ha meg szeretné tesztelje a egyszeri bejelentkezéses, Access Panel megnyitásához. Ha többet szeretne tudni az Access panelen című témakörben [az Access-panelen](active-directory-saas-access-panel-introduction.md).
