<properties 
    pageTitle="Oktatóprogram: Azure Active Directory-integráció a két Zscaler |} Microsoft Azure" 
    description="Megtudhatja, hogy miként használata Zscaler két Azure Active Directory ahhoz, hogy az egyszeri bejelentkezés, automatikus kiépítési és az egyéb!." 
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

#<a name="tutorial-azure-active-directory-integration-with-zscaler-two"></a>Oktatóprogram: Azure Active Directory-integráció a két Zscaler

Ebben az oktatóanyagban célja integrálása az Azure és ZScaler két megjelenítése.  
Az alkalmazási példát, ebben az oktatóanyagban tagolt feltételezi, hogy már rendelkezik az alábbiakat:

-   Érvényes Azure előfizetés
-   A két ZScaler egyszeri bejelentkezés engedélyezve van az előfizetés
  
Ebben az oktatóanyagban befejeztével az Azure Active Directory-felhasználók rendelt ZScaler két tudják egyszeri bejelentkezési a ZScaler két vállalati webhely (service provider által kezdeményezett bejelentkezés) és használatáról a [Bevezetés az Access panelen](active-directory-saas-access-panel-introduction.md) az alkalmazásba
  
A következő építőelemek az alkalmazási példát, ebben az oktatóanyagban tagolt áll:

1.  Az alkalmazás integrációját ZScaler két engedélyezése
2.  Egyszeri bejelentkezés beállítása
3.  A proxykiszolgáló beállításainak konfigurálása
4.  Felhasználói kiépítési konfigurálása
5.  Felhasználók hozzárendelése

![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-zscaler-two-tutorial/IC800199.png "Egyszeri bejelentkezés beállítása")

##<a name="enabling-the-application-integration-for-zscaler-two"></a>Az alkalmazás integrációját ZScaler két engedélyezése
  
Ez a szakasz célja, hogyan engedélyezhető az alkalmazás integrációját ZScaler két tagolása.

###<a name="to-enable-the-application-integration-for-zscaler-two-perform-the-following-steps"></a>Ahhoz, hogy az alkalmazás integrációját ZScaler két, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon kattintson a bal oldali navigációs területen kattintson az **Active Directory**.

    ![Az Active Directory] (./media/active-directory-saas-zscaler-two-tutorial/IC700993.png "Az Active Directory")

2.  A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3.  Nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson a felső menüben **alkalmazások** .

    ![Alkalmazások] (./media/active-directory-saas-zscaler-two-tutorial/IC700994.png "Alkalmazások")

4.  Kattintson a **Hozzáadás** az oldal alján.

    ![Alkalmazás hozzáadása] (./media/active-directory-saas-zscaler-two-tutorial/IC749321.png "Alkalmazás hozzáadása")

5.  **Milyen feladatot szeretne tenni** párbeszédpanelen kattintson **a gyűjteményből az alkalmazások felvétele**.

    ![Gallerry az alkalmazás hozzáadása] (./media/active-directory-saas-zscaler-two-tutorial/IC749322.png "Gallerry az alkalmazás hozzáadása")

6.  A **Keresés mezőbe**írja be a **Két ZScaler**.

    ![Alkalmazás gyűjtemény] (./media/active-directory-saas-zscaler-two-tutorial/IC800200.png "Alkalmazás gyűjtemény")

7.  Az eredmény ablaktáblában jelölje ki a **Két ZScaler**, és válassza a **kész** , az alkalmazás hozzáadása parancsra.

    ![Két ZScaler] (./media/active-directory-saas-zscaler-two-tutorial/IC800201.png "Két ZScaler")

##<a name="configuring-single-sign-on"></a>Egyszeri bejelentkezés beállítása
  
Ez a szakasz célja engedélyezése a felhasználóknak, hogy hitelesíteni ZScaler két fiókjukhoz az összevonási alapján a SAML protokoll használatával Azure Active Directory tagolása.  
Ez az eljárás részeként biztosan szükséges számrendszerben-64 kódolt tanúsítvány feltölteni az ZScaler két bérlőhöz.  
Ha nem ismeri a ezt az eljárást, megtudhatja, [hogy miként szövegfájlba bináris tanúsítvány konvertálása](http://youtu.be/PlgrzUZ-Y1o)

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Egyszeri bejelentkezés beállításához hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon **ZScaler két** alkalmazás integrációs lapján kattintson **konfigurálása az egyszeri bejelentkezés** a **Konfigurálása az egyszeri bejelentkezés** párbeszédpanel megnyitásához.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-zscaler-two-tutorial/IC800202.png "Egyszeri bejelentkezés beállítása")

2.  **Hogyan szeretné, hogy jelentkezzen be az ZScaler két felhasználók** lapon jelölje be **A Microsoft Azure Active Directory Single Sign-On**, és kattintson a **Tovább gombra**.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-zscaler-two-tutorial/IC800203.png "Egyszeri bejelentkezés beállítása")

3.  A **App URL-címe beállítása** lapon az **ZScaler két bejelentkezési a URL-cím** mezőbe írja be a felhasználóknak, hogy a bejelentkezés az ZScaler két alkalmazás által használt URL-CÍMÉT, és kattintson a **Tovább gombra**.

    ![Állítsa be az App URL-címe] (./media/active-directory-saas-zscaler-two-tutorial/IC800204.png "Állítsa be az App URL-címe")

    >[AZURE.NOTE] Amely letölthető a tényleges érték környezetben a ZScaler két támogatási csoportnak, ha szüksége lenne rá.

4.  A **Configure egyszeri bejelentkezés ZScaler két a** lapon a tanúsítvány letöltése, kattintson a **tanúsítvány letöltése**, és mentse a tanúsítvány fájlt a számítógépen parancsra.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-zscaler-two-tutorial/IC800205.png "Egyszeri bejelentkezés beállítása")

5.  A különböző webes böngészőablakban jelentkezzen be a ZScaler két vállalati webhely rendszergazdaként.

6.  A felső sávon kattintson a **felügyeleti**.

    ![Felügyelete] (./media/active-directory-saas-zscaler-two-tutorial/IC800206.png "Felügyelete")

7.  A **rendszergazdák kezelése és szerepkörök**csoportban kattintson a **felhasználók kezelése és a hitelesítési**lehetőséget.

    ![Felhasználók és a hitelesítési kezelése] (./media/active-directory-saas-zscaler-two-tutorial/IC800207.png "Felhasználók és a hitelesítési kezelése")

8.  **Hitelesítési beállítások kiválasztása a szervezet** csoportban hajtsa végre az alábbi lépéseket:

    ![Hitelesítés] (./media/active-directory-saas-zscaler-two-tutorial/IC800208.png "Hitelesítés")

    1.  Jelölje be a **hitelesítés használatával SAML Single Sign-On**.
    2.  Kattintson a **SAML egyszeri bejelentkezéses paraméterek beállítása**gombra.

9.  A **Konfigurálása SAML egyszeri bejelentkezéses paraméterek** párbeszédpanelen lapon hajtsa végre az alábbi lépéseket, és válassza a **Kész gombra**:

    ![Egyszeri bejelentkezés] (./media/active-directory-saas-zscaler-two-tutorial/IC800209.png "Egyszeri bejelentkezés")

    1.  Az Azure klasszikus portálon **konfigurálása az egyszeri bejelentkezés a ZScaler két** párbeszédpanel lapon a **Hitelesítési kérése URL-címe** értéket másolja és illessze be az **URL-címet, amelyre felhasználók hitelesítéshez elküldi a SAML-portál** szövegdoboz.
    2.  Az **attribútumot tartalmazó bejelentkezési neve** mezőbe írja be a **NameID**.
    3.  A letöltött tanúsítvány feltölteni, kattintson a **Zscaler pem**.
    4.  Jelölje ki a **SAML automatikus – kiépítési engedélyezése**.

10. A **Felhasználói hitelesítés konfigurálása** párbeszédpanel lapon hajtsa végre az alábbi lépéseket:

    ![Felügyelete] (./media/active-directory-saas-zscaler-two-tutorial/IC800210.png "Felügyelete")

    1.  Kattintson a **Mentés**gombra.
    2.  Kattintson az **Aktiválás**gombra.

11. Az Azure klasszikus portálon **konfigurálása az egyszeri bejelentkezés a ZScaler két** párbeszédpanel lapon jelölje ki az egyszeri bejelentkezéses konfigurációs megerősítést kérő, és válassza a **kész**gombra.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-zscaler-two-tutorial/IC800211.png "Egyszeri bejelentkezés beállítása")

##<a name="configuring-proxy-settings"></a>A proxykiszolgáló beállításainak konfigurálása

###<a name="to-configure-the-proxy-settings-in-internet-explorer"></a>A Proxybeállítások konfigurálása az Internet Explorerben

1.  **Az Internet Explorer**indítása.

2.  Kattintson az **Internetbeállítások** párbeszédpanel megnyitása az **eszközök** menü **Internetbeállítások** kijelölése

    ![Internetbeállítások párbeszédpanel] (./media/active-directory-saas-zscaler-two-tutorial/IC769492.png "Internetbeállítások párbeszédpanel")

3.  Kattintson a **kapcsolatok** fülre.

    ![Kapcsolatok] (./media/active-directory-saas-zscaler-two-tutorial/IC769493.png "Kapcsolatok")

4.  Kattintson a **helyi hálózati beállítások** , a **Helyi hálózati beállítások** párbeszédpanel megnyitásához.

5.  A Proxy server csoportban hajtsa végre az alábbi lépéseket:

    ![Proxykiszolgáló] (./media/active-directory-saas-zscaler-two-tutorial/IC769494.png "Proxykiszolgáló")

    1.  Jelölje ki a proxykiszolgáló használata a helyi hálózaton.
    2.  Az a cím mezőbe írja be a **gateway.zscalerone.net**.
    3.  A Port mezőbe írja be a **80**.
    4.  Jelölje ki a **proxy figyelmen kívül hagyása helyi címeknél**.
    5.  Kattintson **az OK gombra** kattintva zárja be a **helyi hálózat (LAN) beállításai** párbeszédpanelen.

6.  Kattintson az **OK gombra** az **Internetbeállítások** párbeszédpanel bezárásához.

##<a name="configuring-user-provisioning"></a>Felhasználói kiépítési konfigurálása
  
Ahhoz, hogy az Azure Active Directory-felhasználóknak, hogy jelentkezzen be a két ZScaler, hogy ki kell építenie ZScaler két.  
Az ZScaler két esetében kézi tevékenység kiépítési.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Felhasználói kiépítési beállításához hajtsa végre az alábbi lépéseket:

1.  Jelentkezzen be az **Zscaler** bérlői webhelyen.

2.  Kattintson a **felügyeleti**.

    ![Felügyelete] (./media/active-directory-saas-zscaler-two-tutorial/IC781035.png "Felügyelete")

3.  Kattintson a **felhasználók kezelése**gombra.

    ![Hozzáadása] (./media/active-directory-saas-zscaler-two-tutorial/IC781037.png "Hozzáadása")

4.  A **felhasználók** lapon kattintson a **Hozzáadás**gombra.

    ![Hozzáadása] (./media/active-directory-saas-zscaler-two-tutorial/IC781037.png "Hozzáadása")

5.  A felhasználó hozzáadása csoportban hajtsa végre az alábbi lépéseket:

    ![Felhasználó hozzáadása] (./media/active-directory-saas-zscaler-two-tutorial/IC781038.png "Felhasználó hozzáadása")

    1.  Írja be a **felhasználóazonosító**, a **Felhasználó megjelenítendő név**, a **jelszót**, a **Jelszó megerősítése**, és válassza a **csoportok** és a **részleg** kiépítése kívánt érvényes AAD fiók.
    2.  Kattintson a **Mentés**gombra.

>[AZURE.NOTE] Bármely más ZScaler két felhasználói fiók létrehozása eszközöket is használhatja, illetve API-khoz ZScaler két által biztosított rendelkezést AAD felhasználói fiókok.

##<a name="assigning-users"></a>Felhasználók hozzárendelése
  
Tesztelje a konfigurációt, meg kell adni az Azure Active Directory-felhasználók, használja az alkalmazás elérési útvonalát társításával engedélyezni szeretné.

###<a name="to-assign-users-to-zscaler-two-perform-the-following-steps"></a>Felhasználók hozzárendelése ZScaler két, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon próba-fiók létrehozása.

2.  Az alkalmazás **ZScaler két** integrációs lapján kattintson a **hozzárendelését a felhasználókhoz**.

    ![Adhatnak a felhasználóknak] (./media/active-directory-saas-zscaler-two-tutorial/IC800212.png "Adhatnak a felhasználóknak")

3.  Jelölje ki a próba-felhasználó, kattintson a **hozzárendelni**, és kattintson az **Igen gombra** kattintva erősítse meg a hozzárendelés.

    ![Igen] (./media/active-directory-saas-zscaler-two-tutorial/IC767830.png "Igen")
  
Ha meg szeretné tesztelje a egyszeri bejelentkezéses, Access Panel megnyitásához. Ha többet szeretne tudni az Access panelen című témakörben [az Access-panelen](active-directory-saas-access-panel-introduction.md).