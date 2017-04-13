<properties
    pageTitle="Oktatóprogram: Azure Active Directory-integráció a Wizergos hatékonyság szoftver |} Microsoft Azure"
    description="Megtudhatja, hogy miként konfigurálása az egyszeri bejelentkezés Azure Active Directory és a Wizergos hatékonyság szoftver között."
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
    ms.date="10/17/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-wizergos-productivity-software"></a>Oktatóprogram: Azure Active Directory-integráció a Wizergos hatékonyság szoftver 

Ebben az oktatóanyagban célja mutatja, hogy miként Wizergos hatékonyság szoftver integrálása az Azure Active Directory (Azure Active Directory).

Wizergos hatékonyság szoftver integrálása az Azure Active Directory biztosít a következő előnyökkel jár:

- Az Azure Active Directory Wizergos hatékonyság szoftver hozzáféréssel rendelkező személyek megadhatja, hogy
- Az Azure Active Directory-fiókok engedélyezheti a felhasználóknak, hogy automatikusan első jelentkezett-on Wizergos hatékonyság szoftver (Single Sign-On)
- A fiókokat az egy központi helyen – az Azure klasszikus portálra

Ha többet szeretne tudni a szoftver alkalmazás integrálása az Azure AD meg, hogy, ismerje meg, [az access alkalmazás és az egyszeri bejelentkezés az Azure Active Directory címtárral](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek

Azure Active Directory integráció Wizergos hatékonyság szoftverrel van szükség az alábbiakat:

- Az Azure Active Directory-előfizetéssel
- Egy Wizergos hatékonyság szoftver egyszeri bejelentkezés engedélyezett előfizetés


> [AZURE.NOTE] Az oktatóprogram lépéseit teszteléséhez nem használata ajánlott munkakörnyezetben.


Az oktatóprogram lépéseit teszteléséhez célszerű követnie az alábbi javaslatokat:

- Erre akkor szükség, ne használja a termelési környezetén.
- Ha nincs telepítve az Azure Active Directory próba környezetben, kattint egy hónap próba [Itt](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban célja lehetővé teszi az Azure Active Directory egyszeri bejelentkezés tesztkörnyezetben tesztelése.

Az alkalmazási példát, ebben az oktatóanyagban tagolt két fő építőelemek áll:

1. Wizergos hatékonyság szoftver hozzáadása a gyűjteményből
2. Beállítása és tesztelése Azure Active Directory egyszeri bejelentkezés


## <a name="adding-wizergos-productivity-software-from-the-gallery"></a>Wizergos hatékonyság szoftver hozzáadása a gyűjteményből
Azure Active Directory integrálása Wizergos hatékonyság szoftver konfigurálásához szeretne Wizergos hatékonyság szoftver a gyűjteményből hozzáadása a felügyelt szoftver alkalmazások listájában.

**A gyűjteményből Wizergos hatékonyság szoftver felvételéhez hajtsa végre az alábbi lépéseket:**

1. Az **Azure klasszikus portálon**kattintson a bal oldali navigációs területen kattintson az **Active Directory**. 

    ![Az Active Directory][1]

2. A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3. Nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson a felső menüben **alkalmazásokat** .
    
    ![Alkalmazások][2]

4. Kattintson a **Hozzáadás** az oldal alján.
    
    ![Alkalmazások][3]

5. **Milyen feladatot szeretne tenni** párbeszédpanelen kattintson **a gyűjteményből az alkalmazások felvétele**.

    ![Alkalmazások][4]

6. A Keresés mezőbe írja be a **Wizergos hatékonyság szoftvert**.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_wizergosproductivitysoftware_01.png)

7. Az eredmények panelen válassza ki a **Wizergos hatékonyság szoftver**, és válassza a **kész** , az alkalmazás hozzáadása.

    ![Az alkalmazás kijelöli a gyűjteményben](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_wizergosproductivitysoftware_001.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Beállítása és tesztelése Azure Active Directory egyszeri bejelentkezés
Ez a szakasz célja mutatja, hogy beállítása és Azure Active Directory egyszeri bejelentkezés a "Britta Simon" nevű próba felhasználón alapuló Wizergos hatékonyság szoftverrel tesztelése.

Az egyszeri bejelentkezési a munkát az Azure Active Directory kell tudható, hogy mi az Azure Active Directory-felhasználónak Wizergos hatékonyság szoftverben megfelelőjük a felhasználó. Más szóval Azure AD-felhasználó, és a kapcsolódó felhasználó Wizergos hatékonyság szoftverben hivatkozás viszonya kell létrehozni.

Ez a hivatkozás kapcsolat a értéket a **felhasználó nevét** a **felhasználónév** Wizergos hatékonyság szoftverben értékként Azure AD hozzárendelésével jön létre.

Állítsa be, és Azure Active Directory egyszeri bejelentkezés a BynWizergos hatékonyság Softwareder tesztelése, a következő építőelemek szükséges:

1. **[Azure Active Directory konfigurálása az egyszeri bejelentkezés](#configuring-azure-ad-single-single-sign-on)** - engedélyezése a felhasználóknak, hogy ezzel a szolgáltatással.
2. **[Felhasználó létrehozása az Azure Active Directory tesztelése](#creating-an-azure-ad-test-user)** - Azure Active Directory egyszeri bejelentkezéssel való Britta Simon ellenőrzéséhez.
3. **[Felhasználó létrehozása egy Wizergos hatékonyság szoftver tesztelése](#creating-a-wizergos-productivity-software-test-user)** - szeretné, hogy egy partner a Britta Simon Wizergos hatékonyság szoftverben, amelyen Azure Active Directory ábrázolása csatolt.
4. **[Az Azure Active Directory hozzárendelése tesztelje a felhasználó](#assigning-the-azure-ad-test-user)** - ahhoz, hogy Britta Simon Azure AD az egyszeri bejelentkezés használata.
5. **[Tesztelés egyszeri bejelentkezés](#testing-single-sign-on)** - ellenőrzéséhez, hogy működik-e a konfigurációs.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure Active Directory egyszeri bejelentkezés beállítása

Ebben a részben Azure AD az egyszeri bejelentkezés a klasszikus portálon engedélyezése, és az egyszeri bejelentkezés Wizergos hatékonyság szoftveralkalmazásban beállítása.

**Azure Active Directory egyszeri bejelentkezés beállítása Wizergos hatékonyság szoftverrel, hajtsa végre az alábbi lépéseket:**

1. Az alkalmazás integrációs **Wizergos hatékonyság szoftver** lapon klasszikus portálon kattintson **konfigurálása az egyszeri bejelentkezés** a **Konfigurálása az egyszeri bejelentkezés** párbeszédpanel megnyitásához.
     
    ![Egyszeri bejelentkezés beállítása][6] 

2. **Hogyan szeretné, hogy jelentkezzen be az Wizergos hatékonyság szoftver felhasználók** lapon válassza az **Azure Active Directory Single Sign-On**, és kattintson a **Tovább gombra**:
    
    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_wizergosproductivitysoftware_03.png)

3. Az **Alkalmazás beállításainak megadása** párbeszédpanel lapon kattintson a **Tovább**gombra:

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_wizergosproductivitysoftware_04.png)

4. A **beállítás az egyszeri bejelentkezés Wizergos hatékonyság szoftver a** lapon kattintson a **tanúsítvány letöltése**, és mentse a fájlt a számítógépen:

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_wizergosproductivitysoftware_05.png)

5. A különböző webes böngészőablakban bejelentkezés az Ön Wizergos hatékonyság szoftver bérlői rendszergazdaként.

6. A Hamburg menüből válassza a **rendszergazda**.

    ![Egyszeri bejelentkezés a alkalmazás egymás konfigurálása](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_wizergosproductivitysoftware_000.png)

7. A rendszergazda lap bal oldali menüben válassza a **hitelesítés** , majd kattintson a **Azure AD**.

    ![Egyszeri bejelentkezés a alkalmazás egymás konfigurálása](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_wizergosproductivitysoftware_002.png)

8. Hajtsa végre az alábbi lépéseket a **hitelesítési** szakaszban.

    ![Egyszeri bejelentkezés a alkalmazás egymás konfigurálása](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_wizergosproductivitysoftware_003.png)

    egy. Kattintson a letöltött tanúsítvány az Azure Active Directory feltöltése **FELTÖLTÉSE** gombra. 

    b. A **Kibocsátó URL-CÍMÉT** a szövegdoboz helyezi el a **Kibocsátó** URL-cím értéket az Azure Active Directory-alkalmazás konfigurálása varázsló.

    c billentyűkombinációt. Az **Egyszeri bejelentkezéses URL-CÍMÉT** a szövegdoboz **Egyszeri bejelentkezéses szolgáltatás URL-címe** értékének helyezi az Azure Active Directory-alkalmazás konfigurálása varázsló.

    d. Az **Egyetlen Sign-Out URL-CÍMÉT** a szövegdoboz **Egyetlen Sign-out szolgáltatás URL-címe** értékének helyezi az Azure Active Directory-alkalmazás konfigurálása varázsló.

    e. Kattintson a **Mentés** gombra.

9. A klasszikus portálon jelölje be a egyszeri bejelentkezéses konfigurációs megerősítés, és kattintson a **Tovább gombra**.
    
    ![Azure Active Directory Single Sign-On][10]

10. Az **egyszeri bejelentkezés megerősítés** lapon kattintson a **Befejezés**gombra.  
    
    ![Azure Active Directory Single Sign-On][11]



### <a name="creating-an-azure-ad-test-user"></a>Azure AD-próba felhasználó létrehozása
Ez a szakasz célja próba felhasználó létrehozása az úgynevezett Britta Simon klasszikus portálon.

![Azure Active Directory-felhasználó létrehozása][20]

**Teszt felhasználó létrehozása az Azure Active Directory, hajtsa végre az alábbi lépéseket:**

1. Az **Azure klasszikus portálon**kattintson a bal oldali navigációs területen kattintson az **Active Directory**.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/create_aaduser_09.png)

2. A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3. A menüben, a képernyő tetején, a felhasználók listájának megjelenítéséhez kattintson a **felhasználók**elemre.
    
    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/create_aaduser_03.png)

4. **Felhasználó hozzáadása**elemre az alsó eszköztáron a **Felhasználó hozzáadása** párbeszédpanel megnyitásához.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/create_aaduser_04.png)

5. A párbeszédpanel **közli velünk a felhasználóra vonatkozó** lapon hajtsa végre az alábbi lépéseket:

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/create_aaduser_05.png)

    egy. Felhasználó típusa jelölje be az új felhasználó a szervezet.

    b. A felhasználónév **szövegmezőbe**írja be a **BrittaSimon**.

    c billentyűkombinációt. Kattintson a **Tovább**gombra.

6.  A **Felhasználói profil** párbeszédpanel lapon hajtsa végre az alábbi lépéseket:
    
    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/create_aaduser_06.png)

    egy. Az **első neve** mezőbe írja be a **Britta**.  

    b. Az **Utolsó neve** mezőbe írja be, **Simon**.

    c billentyűkombinációt. A **Megjelenítendő név** mezőbe írja be a **Britta Simon**.

    d. A **szerepkör** listában jelölje ki a **felhasználót**.

    e. Kattintson a **Tovább**gombra.

7. A párbeszédpanel **ideiglenes jelszót kérjen** lapon kattintson a **Hozzon létre**.
    
    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/create_aaduser_07.png)

8. A párbeszédpanel **ideiglenes jelszót első** lapon hajtsa végre az alábbi lépéseket:
    
    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/create_aaduser_08.png)

    egy. Jegyezze fel az **Új jelszót**az érték.

    b. Kattintson a **kész**gombra.   



### <a name="creating-a-wizergos-productivity-software-test-user"></a>Wizergos hatékonyság szoftver próba felhasználó létrehozása

Ebben a részben hozzon létre egy felhasználót Britta Simon nevű Wizergos hatékonyság szoftverben. Kérje Wizergos hatékonyság szoftver támogatási csoport keresztül [support@wizergos.com](emailTo:support@wizergos.com) a felhasználók felvétele a Wizergos hatékonyság szoftver platform.


### <a name="assigning-the-azure-ad-test-user"></a>Az Azure Active Directory-próba felhasználói hozzárendelése

Ez a szakasz célja Britta Simon Azure egyszeri bejelentkezés a saját hozzáférést biztosít Wizergos hatékonyság szoftver használandó engedélyezése.
    
   ![Felhasználó hozzárendelése][200]

**Britta Simon hozzárendelése Wizergos hatékonyság szoftver, hajtsa végre az alábbi lépéseket:**

1. A klasszikus portál kattintva nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson az **alkalmazások** parancsra a felső menüben.
    
    ![Felhasználó hozzárendelése][201]

2. Az alkalmazások listájában jelölje ki a **Wizergos hatékonyság szoftvert**.
    
    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_wizergosproductivitysoftware_50.png)

1. A felső sávon kattintson a **felhasználók**elemre.
    
    ![Felhasználó hozzárendelése][203]

1. A felhasználók listában jelölje ki a **Britta Simon**.

2. Az alsó eszköztáron kattintson a **hozzárendelni**.
    
    ![Felhasználó hozzárendelése][205]



### <a name="testing-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

Ez a szakasz célja a Azure Active Directory egyszeri bejelentkezéses beállítások tesztelése a Access panelen.
 
Amikor az Access panel a Wizergos hatékonyság szoftver mozaik gombra kattint, meg kell első automatikusan aláírt-a Wizergos hatékonyság szoftveralkalmazásban.


## <a name="additional-resources"></a>További források

* [A szoftver alkalmazások integrálása az Azure Active Directory oktatóanyagok listája](active-directory-saas-tutorial-list.md)
* [Mi az access alkalmazás és az egyszeri bejelentkezés az Azure Active Directory címtárral?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_205.png
