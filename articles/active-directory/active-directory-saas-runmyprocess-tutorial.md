<properties
    pageTitle="Oktatóprogram: Azure Active Directory-integráció a RunMyProcess |} Microsoft Azure"
    description="Megtudhatja, hogy miként konfigurálása az egyszeri bejelentkezés Azure Active Directory és RunMyProcess között."
    services="active-directory"
    documentationCenter=""
    authors="jeevansd"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/21/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-runmyprocess"></a>Oktatóprogram: Azure Active Directory-integráció a RunMyProcess

Ebben az oktatóanyagban célja mutatja, hogy miként RunMyProcess integrálása az Azure Active Directory (Azure Active Directory).

RunMyProcess integrálása az Azure Active Directory biztosít a következő előnyökkel jár:

- Az Azure Active Directory, aki rendelkezik hozzáféréssel RunMyProcess megadhatja, hogy
- Engedélyezheti a felhasználóknak, hogy automatikusan első jelentkezett-on való RunMyProcess (Single Sign-On) az Azure Active Directory-fiókok
- A fiókokat az egy központi helyen – az Azure klasszikus portálra

Ha többet szeretne tudni a szoftver alkalmazás integrálása az Azure AD meg, hogy, ismerje meg, [az access alkalmazás és az egyszeri bejelentkezés az Azure Active Directory címtárral](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek

Azure Active Directory integráció a RunMyProcess van szükség az alábbiakat:

- Az Azure Active Directory-előfizetéssel
- Egy RunMyProcess egyszeri bejelentkezés engedélyezett előfizetés


> [AZURE.NOTE] Az oktatóprogram lépéseit teszteléséhez nem használata ajánlott munkakörnyezetben.


Az oktatóprogram lépéseit teszteléséhez célszerű követnie az alábbi javaslatokat:

- Erre akkor szükség, ne használja a termelési környezetén.
- Ha nincs telepítve az Azure Active Directory próba környezetben, kattint egy hónap próba [Itt](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban célja lehetővé teszi az Azure Active Directory egyszeri bejelentkezés tesztkörnyezetben tesztelése.

Az alkalmazási példát, ebben az oktatóanyagban tagolt két fő építőelemek áll:

1. RunMyProcess hozzáadása a gyűjteményből
2. Beállítása és tesztelése Azure Active Directory egyszeri bejelentkezés


## <a name="adding-runmyprocess-from-the-gallery"></a>RunMyProcess hozzáadása a gyűjteményből
Konfigurálja az Azure Active Directory integrálása a RunMyProcess, meg kell RunMyProcess hozzáadása az felügyelt szoftver alkalmazáslistában a gyűjteményből.

**A gyűjteményből RunMyProcess felvételéhez hajtsa végre az alábbi lépéseket:**

1. Az **Azure klasszikus portálon**kattintson a bal oldali navigációs területen kattintson az **Active Directory**.

    ![Az Active Directory][1]

2. A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3. Nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson a felső menüben **alkalmazásokat** .

    ![Alkalmazások][2]

4. Kattintson a **Hozzáadás** az oldal alján.

    ![Alkalmazások][3]

5. **Milyen feladatot szeretne tenni** párbeszédpanelen kattintson **a gyűjteményből az alkalmazások felvétele**.

    ![Alkalmazások][4]

6. A Keresés mezőbe írja be a **RunMyProcess**.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_01.png)

7. Az eredmény ablaktáblában jelölje ki a **RunMyProcess**, és válassza a **kész** , az alkalmazás hozzáadása gombra.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_0001.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Beállítása és tesztelése Azure Active Directory egyszeri bejelentkezés
Ez a szakasz célja mutatja, hogy állítsa be, és a "Britta Simon" nevű próba felhasználón alapuló RunMyProcess Azure AD az egyszeri bejelentkezés tesztelése.

Az egyszeri bejelentkezési a munkát hogy mi a RunMyProcess megfelelőjük a felhasználó egy felhasználóhoz, az Azure Active Directory Azure Active Directory igények van. Más szóval Azure AD-felhasználó, és a kapcsolódó felhasználó RunMyProcess hivatkozás viszonya kell létrehozni.

A hivatkozás kapcsolat jön létre által az Azure Active Directory RunMyProcess **felhasználónév** értékként a **felhasználó neve** értéket rendeli.

Állítsa be, és Azure Active Directory egyszeri bejelentkezéssel való RunMyProcess tesztelése, a következő építőelemek szükséges:

1. **[Azure Active Directory konfigurálása az egyszeri bejelentkezés](#configuring-azure-ad-single-sign-on)** - engedélyezése a felhasználóknak, hogy ezzel a szolgáltatással.
2. **[Felhasználó létrehozása az Azure Active Directory tesztelése](#creating-an-azure-ad-test-user)** - Azure Active Directory egyszeri bejelentkezéssel való Britta Simon ellenőrzéséhez.
3. **[Felhasználó létrehozása egy RunMyProcess tesztelése](#creating-a-runmyprocess-test-user)** - van-e egy megfelelője a Britta Simon RunMyProcess, amelyen az Azure Active Directory ábrázolt csatolt.
4. **[Az Azure Active Directory hozzárendelése tesztelje a felhasználó](#assigning-the-azure-ad-test-user)** - ahhoz, hogy Britta Simon Azure AD az egyszeri bejelentkezés használata.
5. **[Tesztelés egyszeri bejelentkezés](#testing-single-sign-on)** - ellenőrzéséhez, hogy működik-e a konfigurációs.


### <a name="configuring-azure-ad-single-sign-on"></a>Azure Active Directory egyszeri bejelentkezés beállítása

Ebben a részben Azure AD az egyszeri bejelentkezés a klasszikus portálon engedélyezése, és a RunMyProcess alkalmazásban az egyszeri bejelentkezés beállítása.

**Állítson be Azure AD az egyszeri bejelentkezés RunMyProcess, hajtsa végre az alábbi lépéseket:**

1. A klasszikus portálon **RunMyProcess** alkalmazás integrációs lapon kattintson **konfigurálása az egyszeri bejelentkezés** a **Konfigurálása az egyszeri bejelentkezés** párbeszédpanel megnyitásához.
     
    ![Egyszeri bejelentkezés beállítása][6] 

2. **Hogyan szeretné, hogy jelentkezzen be az RunMyProcess felhasználók** lapon válassza az **Azure Active Directory Single Sign-On**, és kattintson a **Tovább gombra**.

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_03.png) 

3. Az **Alkalmazás beállításainak megadása** párbeszédpanel lapon hajtsa végre az alábbi lépéseket:

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_04.png) 

    egy. Az **Bejelentkezési a URL-cím** mezőbe írja be az URL-cím használata a következő mintát: `https://live.runmyprocess.com/live/<tenant id>`.
        
    b. Kattintson a **Tovább** gombra

    > [AZURE.NOTE] Felhívjuk a figyelmét arra, hogy van-e frissítse az értéket a tényleges bejelentkezési a URL-CÍMÉT. Ha ezt az értéket, RunMyProcess támogatási csoport munkatársaitól kaphat keresztül <mailto:support@runmyprocess.com>.
 
4. A **beállítás az egyszeri bejelentkezés RunMyProcess a** lapon kattintson a **Tanúsítvány letöltése** , és mentse a fájlt a számítógépen:

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_05.png)

5. A különböző webes böngészőablakban bejelentkezés az Ön RunMyProcess bérlői rendszergazdaként.

6. A bal oldali navigációs panel kattintson a **fiók** , és válassza a **konfigurációs**.

    ![Alkalmazás oldalán az egyszeri bejelentkezés beállítása](./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_001.png)

7. Ugorjon **hitelesítési módszer** , és végezze el a alábbi lépéseket:

    ![Alkalmazás oldalán az egyszeri bejelentkezés beállítása](./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_002.png)

    egy. **Módszer**jelölje be a **SSO Samlv2**.

    b. **Egyszeri bejelentkezés átirányítása** a szövegdoboz helyezi el a **SAML egyszeri bejelentkezési** URL-cím értéket az Azure Active Directory-alkalmazás konfigurálása varázsló.

    c billentyűkombinációt. **Kijelentkezés átirányítása** a szövegdoboz **Egyetlen Sign-Out szolgáltatás URL-címe** értékének helyezi az Azure Active Directory-alkalmazás konfigurálása varázsló.

    d. Az **Azonosító névformátum** szövegdoboz **Névformátum azonosító** értékét helyezi az Azure Active Directory-alkalmazás konfigurálása varázsló.

    e. Másolja a letöltött tanúsítvány-fájl tartalmát, és illessze be a **tanúsítvány** mezőben lévő értéket. 

    f. Kattintson a **Mentés** ikonra.
    
8. A klasszikus portálon jelölje be a egyszeri bejelentkezéses konfigurációs megerősítés, és kattintson a **Tovább gombra**.
    
    ![Azure Active Directory Single Sign-On][10]

9. Az **egyszeri bejelentkezés megerősítés** lapon kattintson a **Befejezés**gombra.  
 
    ![Azure Active Directory Single Sign-On][11]


### <a name="creating-an-azure-ad-test-user"></a>Azure AD-próba felhasználó létrehozása
Ez a szakasz célja próba felhasználó létrehozása az úgynevezett Britta Simon klasszikus portálon.

![Azure Active Directory-felhasználó létrehozása][20]

**Teszt felhasználó létrehozása az Azure Active Directory, hajtsa végre az alábbi lépéseket:**

1. Az **Azure klasszikus portálon**kattintson a bal oldali navigációs területen kattintson az **Active Directory**.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-runmyprocess-tutorial/create_aaduser_01.png) 

2. A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3. A menüben, a képernyő tetején, a felhasználók listájának megjelenítéséhez kattintson a **felhasználók**elemre.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-runmyprocess-tutorial/create_aaduser_02.png) 

4. **Felhasználó hozzáadása**elemre az alsó eszköztáron a **Felhasználó hozzáadása** párbeszédpanel megnyitásához.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-runmyprocess-tutorial/create_aaduser_03.png) 

5. A **mondja el a felhasználóra vonatkozó** párbeszédpanel lapon hajtsa végre az alábbi lépéseket:  ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-runmyprocess-tutorial/create_aaduser_04.png) 

    egy. Felhasználó típusa jelölje be az új felhasználó a szervezet.

    b. A felhasználónév **szövegmezőbe**írja be a **BrittaSimon**.

    c billentyűkombinációt. Kattintson a **Tovább**gombra.

6.  A **Felhasználói profil** párbeszédpanel lapon hajtsa végre az alábbi lépéseket: ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-runmyprocess-tutorial/create_aaduser_05.png) 

    egy. Az **első neve** mezőbe írja be a **Britta**.  

    b. Az **Utolsó neve** mezőbe írja be, **Simon**.

    c billentyűkombinációt. A **Megjelenítendő név** mezőbe írja be a **Britta Simon**.

    d. A **szerepkör** listában jelölje ki a **felhasználót**.

    e. Kattintson a **Tovább**gombra.

7. A párbeszédpanel **ideiglenes jelszót kérjen** lapon kattintson a **Hozzon létre**.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-runmyprocess-tutorial/create_aaduser_06.png) 

8. A párbeszédpanel **ideiglenes jelszót első** lapon hajtsa végre az alábbi lépéseket:

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-runmyprocess-tutorial/create_aaduser_07.png) 

    egy. Jegyezze fel az **Új jelszót**az érték.

    b. Kattintson a **kész**gombra.   

### <a name="creating-a-runmyprocess-test-user"></a>RunMyProcess próba felhasználó létrehozása
  
Ahhoz, hogy az Azure Active Directory-felhasználóknak, hogy jelentkezzen be a RunMyProcess, hogy ki kell építenie RunMyProcess be. RunMyProcess, ha a feladat manuális kiépítési.

#### <a name="to-provision-a-user-accounts-perform-the-following-steps"></a>A felhasználói fiókok kiépítése, hajtsa végre az alábbi lépéseket:

1.  Jelentkezzen be rendszergazdaként a RunMyProcess vállalati webhely.

2.  Kattintson a **fiók** , majd válassza a **felhasználók** a bal oldali navigációs panel, kattintson az **Új felhasználó**gombra.

    ![Új felhasználó] (./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_003.png "Új felhasználó")

3.  **Felhasználói beállításai** csoportban hajtsa végre az alábbi lépéseket:

    ![Profil] (./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_004.png "Profil")

    egy. Írja be a **nevét** és **E-mail** be a kapcsolódó szövegdobozok kiépítése kívánt érvényes AAD fiók.

    b. Jelölje ki az **IDE nyelvi**, **nyelvi** és egy **profilt**.

    c billentyűkombinációt. Válassza a **fiók létrehozása e-mail számomra küldése**.

    d. Kattintson a **Mentés**gombra.

    >[AZURE.NOTE] Bármely más RunMyProcess felhasználói fiók létrehozása eszközöket is használhatja, illetve API-k által biztosított RunMyProcess kiépítése Azure Active Directory felhasználói fiókok.
    

### <a name="assigning-the-azure-ad-test-user"></a>Az Azure Active Directory-próba felhasználói hozzárendelése

Ez a szakasz célja Britta Simon Azure egyszeri bejelentkezés a saját hozzáférést biztosít RunMyProcess használandó engedélyezése.

![Felhasználó hozzárendelése][200] 

**Britta Simon hozzárendelése RunMyProcess, hajtsa végre az alábbi lépéseket:**

1. A klasszikus portál kattintva nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson az **alkalmazások** parancsra a felső menüben.

    ![Felhasználó hozzárendelése][201] 

2. Az alkalmazások listájában jelölje ki a **RunMyProcess**.

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_50.png) 

3. A felső sávon kattintson a **felhasználók**elemre.

    ![Felhasználó hozzárendelése][203]

4. A felhasználók listában jelölje ki a **Britta Simon**.

5. sáv alján, kattintson a **hozzárendelni**.

    ![Felhasználó hozzárendelése][205]


### <a name="testing-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

Ez a szakasz célja a Azure Active Directory egyszeri bejelentkezéses beállítások tesztelése a Access panelen.
 
Ha a RunMyProcess csempére az Access panel gombra kattint, meg kell kérjen automatikusan aláírt-on az RunMyProcess alkalmazás.


## <a name="additional-resources"></a>További források

* [A szoftver alkalmazások integrálása az Azure Active Directory oktatóanyagok listája](active-directory-saas-tutorial-list.md)
* [Mi az access alkalmazás és az egyszeri bejelentkezés az Azure Active Directory címtárral?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_205.png
