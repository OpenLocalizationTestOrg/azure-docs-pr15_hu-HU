<properties 
    pageTitle="Oktatóprogram: Azure Active Directory-integráció a MCM |} Microsoft Azure" 
    description="Megtudhatja, hogyan használhatja a MCM az Azure Active Directory ahhoz, hogy az egyszeri bejelentkezés, automatikus kiépítési és az egyéb!" 
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
    ms.date="08/30/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-mcm"></a>Oktatóprogram: Azure Active Directory-integráció a MCM
  
Ebben az oktatóanyagban célja mutatja, hogy miként MCM integrálása az Azure Active Directory (Azure Active Directory).

MCM integrálása az Azure Active Directory biztosít a következő előnyökkel jár:

- Az Azure Active Directory, aki rendelkezik hozzáféréssel MCM megadhatja, hogy
- Engedélyezheti a felhasználóknak, hogy automatikusan első jelentkezett-on való MCM (Single Sign-On) az Azure Active Directory-fiókok
- A fiókokat az egy központi helyen – az Azure klasszikus portálra

Ha többet szeretne tudni a szoftver alkalmazás integrálása az Azure AD meg, hogy, ismerje meg, [az access alkalmazás és az egyszeri bejelentkezés az Azure Active Directory címtárral](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek

Azure Active Directory integráció a MCM van szükség az alábbiakat:

- Érvényes Azure előfizetés
- Egy MCM egyszeri bejelentkezés engedélyezett előfizetés


> [AZURE.NOTE] Az oktatóprogram lépéseit teszteléséhez nem használata ajánlott munkakörnyezetben.


Az oktatóprogram lépéseit teszteléséhez célszerű követnie az alábbi javaslatokat:

- Erre akkor szükség, ne használja a termelési környezetén.
- Ha nincs telepítve az Azure Active Directory próba környezetben, kattint egy hónap próba [Itt](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban célja lehetővé teszi az Azure Active Directory egyszeri bejelentkezés tesztkörnyezetben tesztelése.

Az alkalmazási példát, ebben az oktatóanyagban tagolt két fő építőelemek áll:

1. MCM hozzáadása a gyűjteményből
2. Beállítása és tesztelése Azure Active Directory egyszeri bejelentkezés

## <a name="adding-mcm-from-the-gallery"></a>MCM hozzáadása a gyűjteményből
Konfigurálja az Azure Active Directory integrálása a MCM, meg kell MCM hozzáadása az felügyelt szoftver alkalmazáslistában a gyűjteményből.

**A gyűjteményből MCM felvételéhez hajtsa végre az alábbi lépéseket:**

1.  Az Azure klasszikus portálon kattintson a bal oldali navigációs területen kattintson az **Active Directory**.

    ![Az Active Directory] (./media/active-directory-saas-mcm-tutorial/tutorial_general_01.png "Az Active Directory")

2.  A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3.  Nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson a felső menüben **alkalmazások** .

    ![Alkalmazások] (./media/active-directory-saas-mcm-tutorial/tutorial_general_02.png "Alkalmazások")

4.  Kattintson a **Hozzáadás** az oldal alján.

    ![Alkalmazás hozzáadása] (./media/active-directory-saas-mcm-tutorial/tutorial_general_03.png "Alkalmazás hozzáadása")

5.  **Milyen feladatot szeretne tenni** párbeszédpanelen kattintson **a gyűjteményből az alkalmazások felvétele**.

    ![Gallerry az alkalmazás hozzáadása] (./media/active-directory-saas-mcm-tutorial/tutorial_general_04.png "Gallerry az alkalmazás hozzáadása")

6.  A **Keresés mezőbe**írja be a **MCM**.

    ![Alkalmazás gyűjtemény] (./media/active-directory-saas-mcm-tutorial/tutorial_mcm_01.png "Alkalmazás gyűjtemény")

7.  Az eredmény ablaktáblában jelölje ki a **MCM**, és válassza a **kész** , az alkalmazás hozzáadása gombra.

    ![MCM] (./media/active-directory-saas-mcm-tutorial/tutorial_mcm_001.png "MCM")

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Beállítása és tesztelése Azure Active Directory egyszeri bejelentkezés
Ez a szakasz célja mutatja, hogy állítsa be, és a "Britta Simon" nevű próba felhasználón alapuló MCM Azure AD az egyszeri bejelentkezés tesztelése.

Az egyszeri bejelentkezési a munkát az Azure Active Directory kell, hogy mi az a partner felhasználó MCM az Azure Active Directory-felhasználónak. Más szóval Azure AD-felhasználó, és a kapcsolódó felhasználó MCM hivatkozás viszonya kell létrehozni.

A hivatkozás kapcsolat jön létre által az Azure Active Directory MCM **felhasználónév** értékként a **felhasználó neve** értéket rendeli.

Állítsa be, és Azure Active Directory egyszeri bejelentkezéssel való MCM tesztelése, a következő építőelemek szükséges:

1. **[Azure Active Directory konfigurálása az egyszeri bejelentkezés](#configuring-azure-ad-single-single-sign-on)** - engedélyezése a felhasználóknak, hogy ezzel a szolgáltatással.
2. **[Felhasználó létrehozása az Azure Active Directory tesztelése](#creating-an-azure-ad-test-user)** - Azure Active Directory egyszeri bejelentkezéssel való Britta Simon ellenőrzéséhez.
3. **[Felhasználó létrehozása egy MCM tesztelése](#creating-a-mcm-test-user)** - van-e egy megfelelője a Britta Simon MCM, amelyen az Azure Active Directory ábrázolt csatolt.
4. **[Az Azure Active Directory hozzárendelése tesztelje a felhasználó](#assigning-the-azure-ad-test-user)** - ahhoz, hogy Britta Simon Azure AD az egyszeri bejelentkezés használata.
5. **[Tesztelés egyszeri bejelentkezés](#testing-single-sign-on)** - ellenőrzéséhez, hogy működik-e a konfigurációs.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure Active Directory egyszeri bejelentkezés beállítása
  
Ebben a részben Azure AD az egyszeri bejelentkezés a klasszikus portálon engedélyezése, és a MCM alkalmazásban az egyszeri bejelentkezés beállítása.

**Állítson be Azure AD az egyszeri bejelentkezés MCM, hajtsa végre az alábbi lépéseket:**

1.  Az Azure klasszikus portálon **MCM** alkalmazás integrációs lapon kattintson **konfigurálása az egyszeri bejelentkezés** a **Konfigurálása az egyszeri bejelentkezés** párbeszédpanel megnyitásához.

    ![Beállítás az egyszeri bejelentkezés] (./media/active-directory-saas-mcm-tutorial/tutorial_general_05.png "Beállítás az egyszeri bejelentkezés")

2.  **Hogyan szeretné, hogy jelentkezzen be az MCM felhasználók** lapon jelölje be **A Microsoft Azure Active Directory Single Sign-On**, és kattintson a **Tovább gombra**.

    ![Microsoft Azure Active Directory Single Sign-On] (./media/active-directory-saas-mcm-tutorial/tutorial_mcm_03.png "Microsoft Azure Active Directory Single Sign-On")

3.  Az alkalmazás beállításainak megadása párbeszédpanel lapon hajtsa végre az alábbi lépéseket:

    ![Állítsa be az App URL-címe] (./media/active-directory-saas-mcm-tutorial/tutorial_mcm_04.png "Állítsa be az App URL-címe")

    egy. A **Bejelentkezési a URL-cím** mezőbe írja be: `https://myaba.co.uk/client-access/<company name>/saml.php`.
    
    b. Kattintson a **Tovább** gombra

4.  **Konfigurálása az egyszeri bejelentkezés a MCM** lapon kattintson a **metaadat-alapú letöltése**elemre, és mentse a tanúsítvány fájlt a számítógépen.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-mcm-tutorial/tutorial_mcm_05.png "Egyszeri bejelentkezés beállítása")

5. Ha be van állítva az alkalmazás SSO, lépjen kapcsolatba a MCM támogatási csoporthoz. A metaadatok letöltött fájlt csatolni, és ossza meg MCM csoporttal való egyszeri bejelentkezés beállítása a oldalán.

6.  A klasszikus portálon jelölje be a egyszeri bejelentkezéses konfigurációs megerősítés, és kattintson a **Tovább gombra**.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-mcm-tutorial/tutorial_mcm_06.png "Egyszeri bejelentkezés beállítása")

7. Az **egyszeri bejelentkezés megerősítés** lapon kattintson a **Befejezés**gombra.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-mcm-tutorial/tutorial_mcm_07.png "Egyszeri bejelentkezés beállítása")


### <a name="creating-an-azure-ad-test-user"></a>Azure AD-próba felhasználó létrehozása

Ez a szakasz célja próba felhasználó létrehozása az úgynevezett Britta Simon klasszikus portálon.

![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-mcm-tutorial/create_aaduser_00.png)

**Teszt felhasználó létrehozása az Azure Active Directory, hajtsa végre az alábbi lépéseket:**

1. Az **Azure klasszikus portálon**kattintson a bal oldali navigációs területen kattintson az **Active Directory**.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-mcm-tutorial/create_aaduser_01.png)

2. A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3. A menüben, a képernyő tetején, a felhasználók listájának megjelenítéséhez kattintson a **felhasználók**elemre.
    
    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-mcm-tutorial/create_aaduser_02.png)

4. **Felhasználó hozzáadása**elemre az alsó eszköztáron a **Felhasználó hozzáadása** párbeszédpanel megnyitásához.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-mcm-tutorial/create_aaduser_03.png)

5. A párbeszédpanel **közli velünk a felhasználóra vonatkozó** lapon hajtsa végre az alábbi lépéseket:

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-mcm-tutorial/create_aaduser_04.png)

    egy. Felhasználó típusa jelölje be az új felhasználó a szervezet.

    b. A felhasználónév **szövegmezőbe**írja be a **BrittaSimon**.

    c billentyűkombinációt. Kattintson a **Tovább**gombra.

6.  A **Felhasználói profil** párbeszédpanel lapon hajtsa végre az alábbi lépéseket:
    
    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-mcm-tutorial/create_aaduser_05.png)

    egy. Az **első neve** mezőbe írja be a **Britta**.  

    b. Az **Utolsó neve** mezőbe írja be, **Simon**.

    c billentyűkombinációt. A **Megjelenítendő név** mezőbe írja be a **Britta Simon**.

    d. A **szerepkör** listában jelölje ki a **felhasználót**.

    e. Kattintson a **Tovább**gombra.

7. A párbeszédpanel **ideiglenes jelszót kérjen** lapon kattintson a **Hozzon létre**.
    
    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-mcm-tutorial/create_aaduser_06.png)

8. A párbeszédpanel **ideiglenes jelszót első** lapon hajtsa végre az alábbi lépéseket:
    
    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-mcm-tutorial/create_aaduser_07.png)

    egy. Jegyezze fel az **Új jelszót**az érték.

    b. Kattintson a **kész**gombra.   

###<a name="creating-a-mcm-test-user"></a>MCM próba felhasználó létrehozása
  
Ebben a részben hozzon létre egy felhasználót Britta Simon nevű MCM. Kérje meg a felhasználók felvétele a MCM platform MCM támogatási csapattal.

>[AZURE.NOTE]Bármely más MCM felhasználói fiók létrehozása eszközöket is használhatja, illetve MCM rendelkezést AAD felhasználói fiókok által biztosított API-khoz.


###<a name="assigning-the-azure-ad-test-user"></a>Az Azure Active Directory-próba felhasználói hozzárendelése
  
Ez a szakasz célja Britta Simon Azure egyszeri bejelentkezés a saját hozzáférést biztosít MCM használandó engedélyezése.
    
![Adhatnak a felhasználóknak] (./media/active-directory-saas-mcm-tutorial/assign_aaduser_00.png "Adhatnak a felhasználóknak")

**Britta Simon hozzárendelése MCM, hajtsa végre az alábbi lépéseket:**

1. A klasszikus portál kattintva nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson az **alkalmazások** parancsra a felső menüben.
    
    ![Adhatnak a felhasználóknak] (./media/active-directory-saas-mcm-tutorial/assign_aaduser_01.png "Adhatnak a felhasználóknak")

2. Az alkalmazások listájában jelölje ki a **MCM**.
    
    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-mcm-tutorial/tutorial_mcm_08.png)

1. A felső sávon kattintson a **felhasználók**elemre.
    
    ![Adhatnak a felhasználóknak] (./media/active-directory-saas-mcm-tutorial/assign_aaduser_02.png "Adhatnak a felhasználóknak")

1. A felhasználók listában jelölje ki a **Britta Simon**.

2. Az alsó eszköztáron kattintson a **hozzárendelni**.
    
    ![Adhatnak a felhasználóknak] (./media/active-directory-saas-mcm-tutorial/assign_aaduser_03.png "Adhatnak a felhasználóknak")


### <a name="testing-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

Ez a szakasz célja a Azure Active Directory egyszeri bejelentkezéses beállítások tesztelése a Access panelen.
 
Amikor az Access panel a MCM mozaik gombra kattint, meg kell első automatikusan aláírt-on az MCM alkalmazás.


## <a name="additional-resources"></a>További források

* [A szoftver alkalmazások integrálása az Azure Active Directory oktatóanyagok listája](active-directory-saas-tutorial-list.md)
* [Mi az access alkalmazás és az egyszeri bejelentkezés az Azure Active Directory címtárral?](active-directory-appssoaccess-whatis.md)