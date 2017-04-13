<properties 
    pageTitle="Oktatóprogram: Azure Active Directory-integráció a bambusz HR |} Microsoft Azure" 
    description="Megtudhatja, hogyan használhatja a bambusz HR az Azure Active Directory ahhoz, hogy az egyszeri bejelentkezés, automatikus kiépítési és az egyéb!" 
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

#<a name="tutorial-azure-active-directory-integration-with-bamboo-hr"></a>Oktatóprogram: Azure Active Directory-integráció a bambusz HR

Ebben az oktatóanyagban célja integrálása az Azure és BambooHR megjelenítéséhez.  
Az alkalmazási példát, ebben az oktatóanyagban tagolt feltételezi, hogy már rendelkezik az alábbiakat:

-   Érvényes Azure előfizetés
-   BambooHR egyszeri bejelentkezéses engedélyezett előfizetés

Ebben az oktatóanyagban befejeztével az Azure Active Directory-felhasználók BambooHR rendelt fogja tudni az alkalmazás a BambooHR vállalati webhely (service provider által kezdeményezett bejelentkezés) és használatáról a [Bevezetés az Access panelen](active-directory-saas-access-panel-introduction.md)az egyszeri bejelentkezési.

A következő építőelemek az alkalmazási példát, ebben az oktatóanyagban tagolt áll:

1.  Az alkalmazás integrációját BambooHR engedélyezése
2.  Egyszeri bejelentkezés beállítása
3.  Felhasználói kiépítési konfigurálása
4.  Felhasználók hozzárendelése

![Eset] (./media/active-directory-saas-bamboo-hr-tutorial/IC796685.png "Eset")
##<a name="enabling-the-application-integration-for-bamboohr"></a>Az alkalmazás integrációját BambooHR engedélyezése

Ez a szakasz célja, hogyan BambooHR az alkalmazás alkalmazással való integráció engedélyezése a szerkezet.

###<a name="to-enable-the-application-integration-for-bamboohr-perform-the-following-steps"></a>Ahhoz, hogy az alkalmazás integrációját BambooHR, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon kattintson a bal oldali navigációs területen kattintson az **Active Directory**.

    ![Az Active Directory] (./media/active-directory-saas-bamboo-hr-tutorial/IC700993.png "Az Active Directory")

2.  A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3.  Nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson a felső menüben **alkalmazások** .

    ![Alkalmazások] (./media/active-directory-saas-bamboo-hr-tutorial/IC700994.png "Alkalmazások")

4.  Kattintson a **Hozzáadás** az oldal alján.

    ![Alkalmazás hozzáadása] (./media/active-directory-saas-bamboo-hr-tutorial/IC749321.png "Alkalmazás hozzáadása")

5.  **Milyen feladatot szeretne tenni** párbeszédpanelen kattintson **a gyűjteményből az alkalmazások felvétele**.

    ![Gallerry az alkalmazás hozzáadása] (./media/active-directory-saas-bamboo-hr-tutorial/IC749322.png "Gallerry az alkalmazás hozzáadása")

6.  A **Keresés mezőbe**írja be a **BambooHR**.

    ![Alkalmazás gyűjtemény] (./media/active-directory-saas-bamboo-hr-tutorial/IC796686.png "Alkalmazás gyűjtemény")

7.  Az eredmény ablaktáblában jelölje ki a **BambooHR**, és válassza a **kész** , az alkalmazás hozzáadása gombra.

    ![BambooHR] (./media/active-directory-saas-bamboo-hr-tutorial/IC796687.png "BambooHR")
##<a name="configuring-single-sign-on"></a>Egyszeri bejelentkezés beállítása

Ez a szakasz célja tagolása engedélyezése a felhasználóknak, hogy BambooHR hitelesíteni a fiókkal az összevonási alapján a SAML protokoll használatával Azure AD.  
Ez az eljárás részeként biztosan szükséges számrendszerben-64 kódolt tanúsítvány fájlt.  
Ha nem ismeri a ezt az eljárást, olvassa el a [szövegfájlba bináris tanúsítvány konvertálása](http://youtu.be/PlgrzUZ-Y1o)című témakört.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Egyszeri bejelentkezés beállításához hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon **BambooHR** alkalmazás integrációs lapon kattintson **konfigurálása az egyszeri bejelentkezés** a **Konfigurálása az egyszeri bejelentkezés** párbeszédpanel megnyitásához.

    ![Eset] (./media/active-directory-saas-bamboo-hr-tutorial/IC796685.png "Eset")

2.  **Hogyan szeretné, hogy jelentkezzen be az BambooHR felhasználók** lapon jelölje be **A Microsoft Azure Active Directory Single Sign-On**, és kattintson a **Tovább gombra**.

    ![Beállítás az egyszeri bejelentkezés] (./media/active-directory-saas-bamboo-hr-tutorial/IC796688.png "Beállítás az egyszeri bejelentkezés")

3.  Az **Alkalmazás URL-cím beállítása** lapon az **URL a bejelentkezési BambooHR** mezőbe írja be a bejelentkezni a BambooHR alkalmazás a felhasználók által használt URL-címet (például: https://company.bamboohr.com), és kattintson a **Tovább gombra**.

    ![Állítsa be az app URL-címe] (./media/active-directory-saas-bamboo-hr-tutorial/IC796689.png "Állítsa be az app URL-címe")

4.  A **beállítás az egyszeri bejelentkezés BambooHR a** lapon kattintson a **tanúsítvány letöltése**, és mentse a tanúsítvány fájlt a számítógépen.

    ![Beállítás az egyszeri bejelentkezés] (./media/active-directory-saas-bamboo-hr-tutorial/IC796690.png "Beállítás az egyszeri bejelentkezés")

5.  A különböző webes böngészőablakban jelentkezzen be a BambooHR vállalati webhely rendszergazdaként.

6.  A kezdőlapján hajtsa végre az alábbi lépéseket:

    ![Egyszeri bejelentkezés] (./media/active-directory-saas-bamboo-hr-tutorial/IC796691.png "Egyszeri bejelentkezés")

    1.  Kattintson az **alkalmazások hivatkozásra**.
    2.  Az alkalmazások menüt, kattintson a bal oldalon kattintson **Az egyszeri bejelentkezés**.
    3.  Kattintson a **SAML Single Sign-On**.

7.  A **SAML egyszeri bejelentkezés** csoportban hajtsa végre az alábbi lépéseket:

    ![SAML Single Sign-On] (./media/active-directory-saas-bamboo-hr-tutorial/IC796692.png "SAML Single Sign-On")

    1.  Az Azure klasszikus portálon **konfigurálása az egyszeri bejelentkezés a BambooHR** párbeszédpanel lapon az **Egyszeri bejelentkezéses szolgáltatás URL-címe** értéket másolja és illessze be a **Egyszeri bejelentkezési URL-cím** mezőben lévő értéket.
    2.  A letöltött tanúsítvány **Alap-64-címként kódolt** fájl létrehozása  

        >[AZURE.TIP] További információra kíváncsi olvassa el a [bináris tanúsítvány szövegfájlba konvertálása](http://youtu.be/PlgrzUZ-Y1o) című témakört.

    3.  Nyissa meg az alap-64 kódolt tanúsítvány a Jegyzettömbben, azt tartalmát a vágólapra másolja és illessze be a **X.509-es tanúsítvány** mezőben lévő értéket szeretné
    4.  Kattintson a **Mentés**gombra.

8.  Az Azure klasszikus portálon jelölje be a egyszeri bejelentkezéses konfigurációs megerősítés, és válassza a **kész** , **Állítsa be az egyszeri bejelentkezés** párbeszédpanel bezárásához.

    ![Beállítás az egyszeri bejelentkezés] (./media/active-directory-saas-bamboo-hr-tutorial/IC796693.png "Beállítás az egyszeri bejelentkezés")
##<a name="configuring-user-provisioning"></a>Felhasználói kiépítési konfigurálása

Ahhoz, hogy az Azure Active Directory-felhasználóknak, hogy jelentkezzen be a BambooHR, hogy ki kell építenie BambooHR be.  
BambooHR, ha a feladat manuális kiépítési.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>A felhasználói fiókok kiépítése, hajtsa végre az alábbi lépéseket:

1.  Jelentkezzen be rendszergazdaként a **BambooHR** webhelyen.

2.  Kattintson az eszköztáron a képernyő tetején kattintson a **Beállítások**gombra.

    ![Beállítás] (./media/active-directory-saas-bamboo-hr-tutorial/IC796694.png "Beállítás")

3.  Az **Áttekintés**hivatkozásra.

4.  A bal oldali navigációs területen válassza a **biztonsági \> felhasználók**.

5.  Írja be a kapcsolódó szövegdobozok kiépítése kívánt érvényes AAD fiókkal felhasználó nevét, a jelszó és az e-mail címe.

6.  Kattintson a **Mentés**gombra.

>[AZURE.NOTE] Bármely más BambooHR felhasználói fiók létrehozása eszközöket is használhatja, illetve BambooHR rendelkezést AAD felhasználói fiókok által biztosított API-khoz.

##<a name="assigning-users"></a>Felhasználók hozzárendelése

Tesztelje a konfigurációt, meg kell adni az Azure Active Directory-felhasználók, használja az alkalmazás elérési útvonalát társításával engedélyezni szeretné.

###<a name="to-assign-users-to-bamboohr-perform-the-following-steps"></a>Felhasználók hozzárendelése BambooHR, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon próba-fiók létrehozása.

2.  Kattintson a **BambooHR **alkalmazás integrációs lap **hozzárendelését a felhasználókhoz**.

    ![Adhatnak a felhasználóknak] (./media/active-directory-saas-bamboo-hr-tutorial/IC796695.png "Adhatnak a felhasználóknak")

3.  Jelölje ki a próba-felhasználó, kattintson a **hozzárendelni**, és kattintson az **Igen gombra** kattintva erősítse meg a hozzárendelés.

    ![Igen] (./media/active-directory-saas-bamboo-hr-tutorial/IC767830.png "Igen")

Ha meg szeretné tesztelje a egyszeri bejelentkezéses, Access Panel megnyitásához. Az Access ablaktábla, lásd: [Bevezetés az Access panelre](active-directory-saas-access-panel-introduction.md).
