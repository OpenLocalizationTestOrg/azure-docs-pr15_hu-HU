<properties 
    pageTitle="Oktatóprogram: Azure Active Directory-integráció a Cherwell |} Microsoft Azure" 
    description="Megtudhatja, hogyan használhatja a Cherwell az Azure Active Directory ahhoz, hogy az egyszeri bejelentkezés, automatikus kiépítési és az egyéb!" 
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
    ms.date="10/14/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-cherwell"></a>Oktatóprogram: Azure Active Directory-integráció a Cherwell

Ebben az oktatóanyagban célja integrálása az Azure és Cherwell megjelenítéséhez. Az alkalmazási példát, ebben az oktatóanyagban tagolt feltételezi, hogy már rendelkezik az alábbiakat:

-   Érvényes Azure előfizetés
-   Cherwell egyszeri bejelentkezéses engedélyezett előfizetés

Ebben az oktatóanyagban befejeztével az Azure Active Directory-felhasználók Cherwell rendelt fogja tudni az alkalmazás a Cherwell vállalati webhely (a szolgáltatás által kezdeményezett szolgáltató bejelentkezési) és használatáról a [Bevezetés az Access panelen](active-directory-saas-access-panel-introduction.md)az egyszeri bejelentkezési.

A következő építőelemek az alkalmazási példát, ebben az oktatóanyagban tagolt áll:

1.  Az alkalmazás integrációját Cherwell engedélyezése
2.  Egyszeri bejelentkezés beállítása
3.  Felhasználói kiépítési konfigurálása
4.  Felhasználók hozzárendelése

![Eset] (./media/active-directory-saas-cherwell-tutorial/IC798988.png "Eset")
##<a name="enabling-the-application-integration-for-cherwell"></a>Az alkalmazás integrációját Cherwell engedélyezése

Ez a szakasz célja, hogyan Cherwell az alkalmazás alkalmazással való integráció engedélyezése a szerkezet.

###<a name="to-enable-the-application-integration-for-cherwell-perform-the-following-steps"></a>Ahhoz, hogy az alkalmazás integrációját Cherwell, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon kattintson a bal oldali navigációs területen kattintson az **Active Directory**.

    ![Az Active Directory] (./media/active-directory-saas-cherwell-tutorial/IC700993.png "Az Active Directory")

2.  A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3.  Nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson a felső menüben **alkalmazások** .

    ![Alkalmazások] (./media/active-directory-saas-cherwell-tutorial/IC700994.png "Alkalmazások")

4.  Kattintson a **Hozzáadás** az oldal alján.

    ![Alkalmazás hozzáadása] (./media/active-directory-saas-cherwell-tutorial/IC749321.png "Alkalmazás hozzáadása")

5.  **Milyen feladatot szeretne tenni** párbeszédpanelen kattintson **a gyűjteményből az alkalmazások felvétele**.

    ![Gallerry az alkalmazás hozzáadása] (./media/active-directory-saas-cherwell-tutorial/IC749322.png "Gallerry az alkalmazás hozzáadása")

6.  A **Keresés mezőbe**írja be a **Cherwell**.

    ![Cherwell] (./media/active-directory-saas-cherwell-tutorial/IC798989.png "Cherwell")

7.  Az eredmény ablaktáblában jelölje ki a **Cherwell**, és válassza a **kész** , az alkalmazás hozzáadása gombra.
##<a name="configuring-single-sign-on"></a>Egyszeri bejelentkezés beállítása

    ![Cherwell](./media/active-directory-saas-cherwell-tutorial/IC798996.png "Cherwell")

Ez a szakasz célja tagolása engedélyezése a felhasználóknak, hogy Cherwell hitelesíteni a fiókkal az összevonási alapján a SAML protokoll használatával Azure AD.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Egyszeri bejelentkezés beállításához hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon **Cherwell** alkalmazás integrációs lapon kattintson **konfigurálása az egyszeri bejelentkezés** a **Konfigurálása az egyszeri bejelentkezés** párbeszédpanel megnyitásához.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-cherwell-tutorial/IC798990.png "Egyszeri bejelentkezés beállítása")

2.  **Hogyan szeretné, hogy jelentkezzen be az Cherwell felhasználók** lapon jelölje be **A Microsoft Azure Active Directory Single Sign-On**, és kattintson a **Tovább gombra**.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-cherwell-tutorial/IC798991.png "Egyszeri bejelentkezés beállítása")

3.  Az **Alkalmazás URL-cím beállítása** lapon hajtsa végre az alábbi lépéseket:

    ![Állítsa be az App URL-címe] (./media/active-directory-saas-cherwell-tutorial/IC798992.png "Állítsa be az App URL-címe")

    egy.  A **Bejelentkezési a URL-cím** mezőbe írja be a jelentkezhet be a **Cherwell** a felhasználók által használt URL-CÍMÉT (például: *https://\<vállalatnév\>.cherwellondemand.com/cherwellclient*).

    b.  Kattintson a **Tovább** gombra

4.  A **beállítás az egyszeri bejelentkezés Cherwell a** lapon hajtsa végre az alábbi lépéseket:

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-cherwell-tutorial/IC798993.png "Egyszeri bejelentkezés beállítása")

    egy.  Kattintson a **tanúsítvány letöltése**gombra, és mentse a tanúsítvány helyileg a számítógépen.

    b.  Az **identitás-szolgáltató URL-cím**másolása

    c billentyűkombinációt.  **Egyszeri bejelentkezés szolgáltatás URL-cím**másolása.

    d.  Kattintson a **Tovább**gombra.

5.  A letöltött tanúsítvány, az **Identitás-szolgáltató URL-je** és az **Egyszeri bejelentkezéses szolgáltatás URL-CÍMÉT** a Cherwell támogatási csoportnak nyújt.

    >[AZURE.NOTE] Végezze el a tényleges SSO konfigurációs rendelkezik a Cherwell támogatási csoporthoz.
Értesítést fog kapni, amikor az egyszeri bejelentkezés engedélyezve van-előfizetéséhez.

6.  Az Azure klasszikus portálon jelölje be a egyszeri bejelentkezéses konfigurációs megerősítés, és válassza a **kész** , **Állítsa be az egyszeri bejelentkezés** párbeszédpanel bezárásához.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-cherwell-tutorial/IC798994.png "Egyszeri bejelentkezés beállítása")

##<a name="configuring-user-provisioning"></a>Felhasználói kiépítési konfigurálása

Ahhoz, hogy az Azure Active Directory-felhasználóknak, hogy jelentkezzen be a Cherwell, hogy ki kell építenie Cherwell be.  
Cherwell, amíg a felhasználói fiókok kell a Cherwell támogatási csoport által létrehozott.

>[AZURE.NOTE] Bármely más Cherwell felhasználói fiók létrehozása eszközöket is használhatja, illetve API-k által biztosított Cherwell kiépítése Azure Active Directory felhasználói fiókok.

##<a name="assigning-users"></a>Felhasználók hozzárendelése

Tesztelje a konfigurációt, meg kell adni az Azure Active Directory-felhasználók, használja az alkalmazás elérési útvonalát társításával engedélyezni szeretné.

###<a name="to-assign-users-to-cherwell-perform-the-following-steps"></a>Felhasználók hozzárendelése Cherwell, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon próba-fiók létrehozása.

2.  Kattintson a **Cherwell** alkalmazás integrációs lap **hozzárendelését a felhasználókhoz**.

    ![Hozzárendelését a felhasználókhoz] (./media/active-directory-saas-cherwell-tutorial/IC798995.png "Adhatnak a felhasználóknak")

3.  Jelölje ki a próba-felhasználó, kattintson a **hozzárendelni**, és kattintson az **Igen gombra** kattintva erősítse meg a hozzárendelés.

    ![Igen] (./media/active-directory-saas-cherwell-tutorial/IC767830.png "Igen")

Ha meg szeretné tesztelje a egyszeri bejelentkezéses, Access Panel megnyitásához. Az Access ablaktábla, lásd: [Bevezetés az Access panelre](active-directory-saas-access-panel-introduction.md).
