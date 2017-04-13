<properties 
    pageTitle="Oktatóprogram: Azure Active Directory-integráció a ClickTime |} Microsoft Azure" 
    description="Megtudhatja, hogyan használhatja a ClickTime az Azure Active Directory ahhoz, hogy az egyszeri bejelentkezés, automatikus kiépítési és az egyéb!" 
    services="active-directory" 
    authors="jeevansd"
    documentationCenter="na" 
    manager="femila" />
<tags
    ms.service="active-directory" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="identity" 
    ms.date="08/16/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-clicktime"></a>Oktatóprogram: Azure Active Directory-integráció a ClickTime

Ebből az oktatóanyagból megismerheti, hogyan ClickTime integrálása az Azure Active Directory (Azure Active Directory).

ClickTime integrálása az Azure Active Directory biztosít a következő előnyökkel jár:

- Az Azure Active Directory, aki rendelkezik hozzáféréssel ClickTime megadhatja, hogy
- Engedélyezheti a felhasználóknak, hogy automatikusan első jelentkezett-on való ClickTime (Single Sign-On) az Azure Active Directory-fiókok
- A fiókokat az egy központi helyen – az Azure klasszikus portálra

Ha többet szeretne tudni a szoftver alkalmazás integrálása az Azure AD meg, hogy, ismerje meg, [az access alkalmazás és az egyszeri bejelentkezés az Azure Active Directory címtárral](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek

Azure Active Directory integráció a ClickTime van szükség az alábbiakat:

- Az Azure Active Directory-előfizetéssel
- Egy ClickTime egyszeri bejelentkezés engedélyezett előfizetés


> [AZURE.NOTE] Az oktatóprogram lépéseit teszteléséhez nem használata ajánlott munkakörnyezetben.


Az oktatóprogram lépéseit teszteléséhez célszerű követnie az alábbi javaslatokat:

- Erre akkor szükség, ne használja a termelési környezetén.
- Ha nincs telepítve az Azure Active Directory próba környezetben, kattint egy hónap próba [Itt](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban tesztelni Azure AD az egyszeri bejelentkezés tesztkörnyezetben.

Az alkalmazási példát, ebben az oktatóanyagban tagolt két fő építőelemek áll:

1. ClickTime hozzáadása a gyűjteményből
2. Beállítása és tesztelése Azure Active Directory egyszeri bejelentkezés

##<a name="adding-clicktime-from-the-gallery"></a>ClickTime hozzáadása a gyűjteményből

Ez a szakasz célja, hogyan ClickTime az alkalmazás alkalmazással való integráció engedélyezése a szerkezet.

###<a name="to-enable-the-application-integration-for-clicktime-perform-the-following-steps"></a>Ahhoz, hogy az alkalmazás integrációját ClickTime, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon kattintson a bal oldali navigációs területen kattintson az **Active Directory**.

    ![Az Active Directory] (./media/active-directory-saas-clicktime-tutorial/tic700993.png "Az Active Directory")

2.  A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3.  Nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson a felső menüben **alkalmazások** .

    ![Alkalmazások] (./media/active-directory-saas-clicktime-tutorial/tic700994.png "Alkalmazások")

4.  Kattintson a **Hozzáadás** az oldal alján.

    ![Alkalmazás hozzáadása] (./media/active-directory-saas-clicktime-tutorial/tic749321.png "Alkalmazás hozzáadása")

5.  **Milyen feladatot szeretne tenni** párbeszédpanelen kattintson **a gyűjteményből az alkalmazások felvétele**.

    ![Gallerry az alkalmazás hozzáadása] (./media/active-directory-saas-clicktime-tutorial/tic749322.png "Gallerry az alkalmazás hozzáadása")

6.  A **Keresés mezőbe**írja be a **ClickTime**.

    ![Alkalmazás gyűjtemény] (./media/active-directory-saas-clicktime-tutorial/tic777275.png "Alkalmazás gyűjtemény")

7.  Az eredmény ablaktáblában jelölje ki a **ClickTime**, és válassza a **kész** , az alkalmazás hozzáadása gombra.

    ![ClickTime] (./media/active-directory-saas-clicktime-tutorial/tic777276.png "ClickTime")

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Beállítása és tesztelése Azure Active Directory egyszeri bejelentkezés
Ebben a részben konfigurálása és a "Britta Simon" nevű próba felhasználón alapuló ClickTime az Azure Active Directory egyszeri bejelentkezés tesztelése.

Az egyszeri bejelentkezési a munkát az Azure Active Directory kell tudható, hogy mi a ClickTime megfelelőjük a felhasználó egy felhasználóhoz, az Azure Active Directory. Más szóval Azure AD-felhasználó, és a kapcsolódó felhasználó ClickTime hivatkozás viszonya kell létrehozni.

A hivatkozás kapcsolat jön létre által az Azure Active Directory ClickTime **felhasználónév** értékként a **felhasználó neve** értéket rendeli.

Állítsa be, és Azure Active Directory egyszeri bejelentkezéssel való ClickTime tesztelése, az alábbi építőelemeket szükséges:

1. **[Azure Active Directory konfigurálása az egyszeri bejelentkezés](#configuring-azure-ad-single-sign-on)** - engedélyezése a felhasználóknak, hogy ezzel a szolgáltatással.
2. **[Felhasználó létrehozása az Azure Active Directory tesztelése](#creating-an-azure-ad-test-user)** - Azure Active Directory egyszeri bejelentkezéssel való Britta Simon ellenőrzéséhez.
3. **[Felhasználó létrehozása egy ClickTime tesztelése](#creating-a-clicktime-test-user)** - van-e egy megfelelője a Britta Simon ClickTime, amelyen az Azure Active Directory ábrázolt csatolt.
4. **[Az Azure Active Directory hozzárendelése tesztelje a felhasználó](#assigning-the-azure-ad-test-user)** - ahhoz, hogy Britta Simon Azure AD az egyszeri bejelentkezés használata.
5. **[Tesztelés egyszeri bejelentkezés](#testing-single-sign-on)** - ellenőrzéséhez, hogy működik-e a konfigurációs.


### <a name="configuring-azure-ad-single-sign-on"></a>Azure Active Directory egyszeri bejelentkezés beállítása

Ez a szakasz célja tagolása engedélyezése a felhasználóknak, hogy ClickTime hitelesíteni a fiókkal az összevonási alapján a SAML protokoll használatával Azure AD.  


>[AZURE.IMPORTANT] Ahhoz, hogy a egyszeri bejelentkezés a ClickTime bérlő konfigurálása, kell először lépjen kapcsolatba az első ezzel a funkcióval a ClickTime technikai támogatási.

**Állítson be Azure AD az egyszeri bejelentkezés ClickTime, hajtsa végre az alábbi lépéseket:**

1.  Az Azure klasszikus portálon **ClickTime** alkalmazás integrációs lapon kattintson **konfigurálása az egyszeri bejelentkezés** a **Konfigurálása az egyszeri bejelentkezés** párbeszédpanel megnyitásához.

    ![Egyszeri bejelentkezés engedélyezése] (./media/active-directory-saas-clicktime-tutorial/tic777277.png "Egyszeri bejelentkezés engedélyezése")

2.  **Hogyan szeretné, hogy jelentkezzen be az ClickTime felhasználók** lapon jelölje be **A Microsoft Azure Active Directory Single Sign-On**, és kattintson a **Tovább gombra**.

    ![Beállítás az egyszeri bejelentkezés] (./media/active-directory-saas-clicktime-tutorial/tic777278.png "Beállítás az egyszeri bejelentkezés")

3. Az **Alkalmazás beállításainak megadása** párbeszédpanel lapon hajtsa végre az alábbi lépéseket:

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-clicktime-tutorial/tic777286.png) 

    egy. Az **IdentifierL** mezőbe írja be az URL-cím használata a következő mintát: **https://app.clicktime.com/sp/**
    
    b. **Válasz URL-cím** mezőbe írja be az URL-cím használata a következő mintát: **https://app.clicktime.com/Login/**

    c billentyűkombinációt. Kattintson a **Tovább** gombra

4.  A **Configure egyszeri bejelentkezés ClickTime a** lapon a tanúsítvány letöltése, kattintson a **tanúsítvány letöltése**, és mentse a tanúsítvány fájlt a számítógépen parancsra.

    ![Beállítás az egyszeri bejelentkezés] (./media/active-directory-saas-clicktime-tutorial/tic777279.png "Beállítás az egyszeri bejelentkezés")

4.  A különböző webes böngészőablakban jelentkezzen be a ClickTime vállalati webhely rendszergazdaként.

5.  Kattintson az eszköztáron a képernyő tetején kattintson a **Beállítások**gombra, és kattintson a **Biztonsági beállítások**gombra.

6.  **Egyszeri bejelentkezés beállításainak** konfigurálása csoportban hajtsa végre az alábbi lépéseket:

    ![Biztonsági beállítások] (./media/active-directory-saas-clicktime-tutorial/tic777280.png "Biztonsági beállítások")

    egy.  Válassza az **Engedélyezés lehetőséget** bejelentkezés egyszeri bejelentkezés (SSO) használata **Azure AD**.
    
    b.  Az Azure klasszikus portálon **konfigurálása az egyszeri bejelentkezés a ClickTime** párbeszédpanel lapon másolja az **Egyszeri bejelentkezéses szolgáltatás URL-címe** értéket, és illessze be a **Identitás szolgáltató végpont** mezőben lévő értéket.

    c billentyűkombinációt.  Nyissa meg az alap-64 kódolt tanúsítványát a **Jegyzettömbben**, a tartalom másolása és beillesztése a **X.509-es tanúsítvány** mezőben lévő értéket.
    
    d.  Kattintson a **Mentés**gombra.

7.  Az Azure klasszikus portálon jelölje be a egyszeri bejelentkezéses konfigurációs megerősítés, és válassza a **kész** , **Állítsa be az egyszeri bejelentkezés** párbeszédpanel bezárásához.

    ![Beállítás az egyszeri bejelentkezés] (./media/active-directory-saas-clicktime-tutorial/tic777281.png "Beállítás az egyszeri bejelentkezés")

##<a name="configuring-user-provisioning"></a>Felhasználói kiépítési konfigurálása

Ahhoz, hogy az Azure Active Directory-felhasználóknak, hogy jelentkezzen be a ClickTime, hogy ki kell építenie ClickTime be.  
ClickTime, ha a feladat manuális kiépítési.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>A felhasználói fiókok kiépítése, hajtsa végre az alábbi lépéseket:

1.  Jelentkezzen be az **ClickTime** bérlői webhelyen.

2.  Kattintson az eszköztáron a képernyő tetején kattintson a **vállalat**, és válassza a **személyek**gombra.

    ![Személyek] (./media/active-directory-saas-clicktime-tutorial/tic777282.png "Személyek")

3.  Kattintson a **személy hozzáadása**gombra.

    ![Személy hozzáadása] (./media/active-directory-saas-clicktime-tutorial/tic777283.png "Személy hozzáadása")

4.  Új személy csoportban hajtsa végre az alábbi lépéseket:

    ![Személyek] (./media/active-directory-saas-clicktime-tutorial/tic777284.png "Személyek")

    egy.  Az **e-mail cím** mezőbe írja be az e-mail címet az Azure Active Directory-fiókját.
    
    b.  A **teljes neve** mezőbe írja be a nevét az Azure Active Directory-fiókját.  

    >[AZURE.NOTE] Ha azt szeretné, hogy, beállíthatja, hogy az új személy objektum további tulajdonságokat.

    c billentyűkombinációt.  Kattintson a **Mentés**gombra.

>[AZURE.NOTE] Bármely más ClickTime felhasználói fiók létrehozása eszközöket is használhatja, illetve ClickTime hozhatók létre az Azure Active Directory-fiókokkal által biztosított API-khoz.

### <a name="assigning-the-azure-ad-test-user"></a>Az Azure Active Directory-próba felhasználói hozzárendelése

Ebben a részben Britta Simon Azure egyszeri bejelentkezés a saját hozzáférést biztosít ClickTime használandó engedélyezése.

![Felhasználó hozzárendelése][200]

Tesztelje a konfigurációt, meg kell adni az Azure Active Directory-felhasználók, használja az alkalmazás elérési útvonalát társításával engedélyezni szeretné.

**Britta Simon hozzárendelése ClickTime, hajtsa végre a következő lépések**

1. A klasszikus portál kattintva nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson az **alkalmazások** parancsra a felső menüben.

    ![Felhasználó hozzárendelése][201] 

2. Az alkalmazások listájában jelölje ki a **ClickTime**.

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-clicktime-tutorial/tutorial_clicktime_50.png) 

3. A felső sávon kattintson a **felhasználók**elemre.

    ![Felhasználó hozzárendelése][203]

4. A felhasználók listában jelölje ki a **Britta Simon**.

5. Az alsó eszköztáron kattintson a **hozzárendelni**.

    ![Felhasználó hozzárendelése][205]

## <a name="testing-single-sign-on"></a>Egyszeri bejelentkezés tesztelése
Ebben a részben tesztelje az Azure Active Directory egyetlen bejelentkezés beállítási a Access panelen.

Amikor az Access panel a ClickTime mozaik gombra kattint, meg kell első automatikusan aláírt-on az ClickTime alkalmazás.


## <a name="additional-resources"></a>További források

* [A szoftver alkalmazások integrálása az Azure Active Directory oktatóanyagok listája](active-directory-saas-tutorial-list.md)
* [Mi az access alkalmazás és az egyszeri bejelentkezés az Azure Active Directory címtárral?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[200]: ./media/active-directory-saas-clicktime-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-clicktime-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-clicktime-tutorial/tutorial_general_203.png
[205]: ./media/active-directory-saas-clicktime-tutorial/tutorial_general_205.png