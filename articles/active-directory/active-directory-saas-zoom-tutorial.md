<properties 
    pageTitle="Oktatóprogram: Azure Active Directory-integráció a Nagyítás |} Microsoft Azure" 
    description="Megtudhatja, hogyan használhatja a Nagyítás az Azure Active Directory ahhoz, hogy az egyszeri bejelentkezés, automatikus kiépítési és az egyéb!." 
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

#<a name="tutorial-azure-active-directory-integration-with-zoom"></a>Oktatóprogram: Azure Active Directory-integráció a Nagyítás
  
Ebben az oktatóanyagban célja integrálása az Azure és a nagyítási megjelenítéséhez.  
Az alkalmazási példát, ebben az oktatóanyagban tagolt feltételezi, hogy már rendelkezik az alábbiakat:

-   Érvényes Azure előfizetés
-   Nagyítás bérlői webhelyre
  
Ebben az oktatóanyagban befejeztével az Azure Active Directory felhasználók nagyítás rendelt tudnak egyetlen bejelentkezés az alkalmazásba, a Nagyítás vállalati webhely (service provider által kezdeményezett bejelentkezés) és használatáról a [Bevezetés az Access panelre](active-directory-saas-access-panel-introduction.md)
  
A következő építőelemek az alkalmazási példát, ebben az oktatóanyagban tagolt áll:

1.  A Nagyítás alkalmazás integrációját engedélyezése
2.  Egyszeri bejelentkezés beállítása
3.  Felhasználói kiépítési konfigurálása
4.  Felhasználók hozzárendelése

![Eset] (./media/active-directory-saas-zoom-tutorial/IC784693.png "Eset")

##<a name="enabling-the-application-integration-for-zoom"></a>A Nagyítás alkalmazás integrációját engedélyezése
  
Ez a szakasz célja, hogyan Nagyítás az alkalmazás alkalmazással való integráció engedélyezése a szerkezet.

###<a name="to-enable-the-application-integration-for-zoom-perform-the-following-steps"></a>Ha engedélyezni szeretné a Nagyítás alkalmazás integrációját, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon kattintson a bal oldali navigációs területen kattintson az **Active Directory**.

    ![Az Active Directory] (./media/active-directory-saas-zoom-tutorial/IC700993.png "Az Active Directory")

2.  A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3.  Nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson a felső menüben **alkalmazásokat** .

    ![Alkalmazások] (./media/active-directory-saas-zoom-tutorial/IC700994.png "Alkalmazások")

4.  Kattintson a **Hozzáadás** az oldal alján.

    ![Alkalmazás hozzáadása] (./media/active-directory-saas-zoom-tutorial/IC749321.png "Alkalmazás hozzáadása")

5.  **Milyen feladatot szeretne tenni** párbeszédpanelen kattintson **a gyűjteményből az alkalmazások felvétele**.

    ![Gallerry az alkalmazás hozzáadása] (./media/active-directory-saas-zoom-tutorial/IC749322.png "Gallerry az alkalmazás hozzáadása")

6.  A **Keresés mezőbe**írja be a **Nagyítás**parancsot.

    ![Alkalmazás gyűjtemény] (./media/active-directory-saas-zoom-tutorial/IC784694.png "Alkalmazás gyűjtemény")

7.  Az eredmény ablaktáblában jelölje be a **Nagyítás**, és válassza a **kész** , az alkalmazás hozzáadása parancsra.

    ![Nagyítás] (./media/active-directory-saas-zoom-tutorial/IC784695.png "Nagyítás")

##<a name="configuring-single-sign-on"></a>Egyszeri bejelentkezés beállítása
  
Ez a szakasz célja tagolása engedélyezése a felhasználóknak, hogy a Nagyítás hitelesíteni a fiókkal az összevonási alapján a SAML protokoll használatával Azure AD.  
Ez az eljárás részeként biztosan szükséges számrendszerben-64 kódolt tanúsítvány fájlt.  
Ha nem ismeri a ezt az eljárást, olvassa el a [szövegfájlba bináris tanúsítvány konvertálása](http://youtu.be/PlgrzUZ-Y1o)című témakört.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Egyszeri bejelentkezés beállításához hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon alkalmazás integrációs **Nagyítás** lapján kattintson **konfigurálása az egyszeri bejelentkezés** a **Konfigurálása az egyszeri bejelentkezés** párbeszédpanel megnyitásához.

    ![Beállítás az egyszeri bejelentkezés] (./media/active-directory-saas-zoom-tutorial/IC784696.png "Beállítás az egyszeri bejelentkezés")

2.  **Hogyan megtekinti jelentkezzen be Nagyítás felhasználók** lapon jelölje be **A Microsoft Azure Active Directory Single Sign-On**, és kattintson a **Tovább gombra**.

    ![Beállítás az egyszeri bejelentkezés] (./media/active-directory-saas-zoom-tutorial/IC784697.png "Beállítás az egyszeri bejelentkezés")

3.  Az **Alkalmazás URL-cím beállítása** lapon, a **Nagyítás bejelentkezési az URL-cím** mezőbe írja be az URL-t, a következő mintát "*http://company.zoom.us*", és kattintson a **Tovább gombra**.

    ![Állítsa be az App URL-címe] (./media/active-directory-saas-zoom-tutorial/IC784698.png "Állítsa be az App URL-címe")

4.  A **beállítás az egyszeri bejelentkezés a Nagyítás** lapon kattintson a **tanúsítvány letöltése**, és mentse a tanúsítvány fájlt a számítógépen.

    ![Beállítás az egyszeri bejelentkezés] (./media/active-directory-saas-zoom-tutorial/IC784699.png "Beállítás az egyszeri bejelentkezés")

5.  A különböző webes böngészőablakban jelentkezzen be a Nagyítás vállalati webhely rendszergazdaként.

6.  Kattintson az **Egyszeri bejelentkezés** fülre.

    ![Egyszeri bejelentkezés] (./media/active-directory-saas-zoom-tutorial/IC784700.png "Egyszeri bejelentkezés")

7.  Kattintson a **Biztonsági ellenőrzés** fülre, és kattintson a az **Egyszeri bejelentkezés a** beállításokat.

8.  Egyszeri bejelentkezés csoportban hajtsa végre az alábbi lépéseket:

    ![Egyszeri bejelentkezés] (./media/active-directory-saas-zoom-tutorial/IC784701.png "Egyszeri bejelentkezés")

    1.  Az Azure klasszikus portálon **konfigurálása az egyszeri bejelentkezés a Nagyítás** párbeszédpanel lapon az **Egyszeri bejelentkezéses szolgáltatás URL-címe** értéket másolja és illessze be a **bejelentkezési URL-címe** mezőben lévő értéket.
    2.  Az Azure klasszikus portálon **konfigurálása az egyszeri bejelentkezés a Nagyítás** párbeszédpanel lapon az **Egyetlen Sign-Out szolgáltatás URL-címe** értéket másolja és illessze be a **kijelentkezési lap URL-címe** mezőben lévő értéket.
    3.  A letöltött tanúsítvány **Alap-64-címként kódolt** fájl létrehozása  

        >[AZURE.TIP] További információra kíváncsi olvassa el a [bináris tanúsítvány szövegfájlba konvertálása](http://youtu.be/PlgrzUZ-Y1o) című témakört.

    4.  Nyissa meg az alap-64 kódolt tanúsítvány a Jegyzettömbben, a tartalmát másolja a vágólapra, és beilleszti az **identitás-szolgáltató tanúsítványának** szövegdoboz
    5.  Az Azure klasszikus portálon **konfigurálása az egyszeri bejelentkezés a Nagyítás** párbeszédpanel lapon a **Kibocsátó URL-címe** értéket másolja és illessze be a **kibocsátó** mezőben lévő értéket.
    6.  Kattintson a **Mentés**gombra.

9.  Az Azure klasszikus portálon jelölje be a egyszeri bejelentkezéses konfigurációs megerősítés, és válassza a **kész** , **Állítsa be az egyszeri bejelentkezés** párbeszédpanel bezárásához.

    ![Beállítás az egyszeri bejelentkezés] (./media/active-directory-saas-zoom-tutorial/IC784702.png "Beállítás az egyszeri bejelentkezés")

##<a name="configuring-user-provisioning"></a>Felhasználói kiépítési konfigurálása
  
Ahhoz, hogy az Azure Active Directory-felhasználóknak, hogy jelentkezzen be a nagyítás, hogy ki kell építenie Nagyítás be.  
Nagyítás, amíg egy manuális feladat kiépítési.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>A felhasználói fiókok kiépítése, hajtsa végre az alábbi lépéseket:

1.  Jelentkezzen be rendszergazdaként a **Nagyítás** vállalati webhely.

2.  Kattintson a **Fiókok kezelése** lapra, és kattintson a **Felhasználók kezelése**gombra.

3.  A felhasználók kezelése csoportban kattintson a **felhasználók hozzáadása**gombra.

    ![Felhasználókezelés] (./media/active-directory-saas-zoom-tutorial/IC784703.png "Felhasználókezelés")

4.  A **felhasználók hozzáadása** lapon hajtsa végre az alábbi lépéseket:

    ![Felhasználók hozzáadása] (./media/active-directory-saas-zoom-tutorial/IC784704.png "Felhasználók hozzáadása")

    1.  **Felhasználó típusa**csoportban jelölje ki a **egyszerű**.
    2.  Az **e-mailek** mezőbe írja be a kívánt kiépítése érvényes AAD fiókkal annak az e-mail címét.
    3.  Kattintson a **hozzáadása**gombra.

>[AZURE.NOTE] Bármely más nagyítási felhasználói fiók létrehozása eszközök vagy a Nagyítás kiépítése AAD által biztosított API-khoz felhasználói fiókok.

##<a name="assigning-users"></a>Felhasználók hozzárendelése
  
Tesztelje a konfigurációt, meg kell adni az Azure Active Directory-felhasználók, használja az alkalmazás elérési útvonalát társításával engedélyezni szeretné.

###<a name="to-assign-users-to-zoom-perform-the-following-steps"></a>Felhasználók hozzárendelése nagyítás, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon próba-fiók létrehozása.

2.  A **Nagyítás **alkalmazás integrációs lapján kattintson **hozzárendelését a felhasználókhoz**.

    ![Adhatnak a felhasználóknak] (./media/active-directory-saas-zoom-tutorial/IC784705.png "Adhatnak a felhasználóknak")

3.  Jelölje ki a próba-felhasználó, kattintson a **hozzárendelni**, és kattintson az **Igen gombra** kattintva erősítse meg a hozzárendelés.

    ![Igen] (./media/active-directory-saas-zoom-tutorial/IC767830.png "Igen")
  
Ha meg szeretné tesztelje a egyszeri bejelentkezéses, Access Panel megnyitásához. Ha többet szeretne tudni az Access panelen című témakörben [az Access-panelen](active-directory-saas-access-panel-introduction.md).