<properties
    pageTitle="Oktatóprogram: Azure Active Directory-integráció a felismerés |} Microsoft Azure"
    description="Megtudhatja, hogy miként konfigurálása az egyszeri bejelentkezés Azure Active Directory és a felismerés között."
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
    ms.date="10/27/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-recognize"></a>Oktatóprogram: Azure Active Directory-integráció a felismerése

Ebben az oktatóanyagban célja mutatja, hogy miként felismerése integrálása az Azure Active Directory (Azure Active Directory).

Felismerése integrálása az Azure Active Directory biztosít a következő előnyökkel jár:

- Beállíthatja, hogy, Azure Active Directory, aki rendelkezik hozzáféréssel felismerése
- Engedélyezheti a felhasználóknak, hogy automatikusan első aláírással a felismerés (Single Sign-On) való az Azure Active Directory-fiókok
- A fiókokat az egy központi helyen – az Azure klasszikus portálra

Ha többet szeretne tudni a szoftver alkalmazás integrálása az Azure AD meg, hogy, ismerje meg, [az access alkalmazás és az egyszeri bejelentkezés az Azure Active Directory címtárral](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek

Azure Active Directory integráció a felismerés van szükség az alábbiakat:

- Az Azure Active Directory-előfizetéssel
- Egy felismerése egyszeri bejelentkezés engedélyezett előfizetés


> [AZURE.NOTE] Az oktatóprogram lépéseit teszteléséhez nem használata ajánlott munkakörnyezetben.


Az oktatóprogram lépéseit teszteléséhez célszerű követnie az alábbi javaslatokat:

- Erre akkor szükség, ne használja a termelési környezetén.
- Ha nincs telepítve az Azure Active Directory próba környezetben, kattint egy hónap próba [Itt](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban célja lehetővé teszi az Azure Active Directory egyszeri bejelentkezés tesztkörnyezetben tesztelése.

Az alkalmazási példát, ebben az oktatóanyagban tagolt két fő építőelemek áll:

1. A gyűjtemény felvétele felismerése
2. Beállítása és tesztelése Azure Active Directory egyszeri bejelentkezés


## <a name="adding-recognize-from-the-gallery"></a>A gyűjtemény felvétele felismerése
Konfigurálja az Azure Active Directory integrálása a felismerés, szüksége felismerése hozzáadása az felügyelt szoftver alkalmazáslistában a gyűjteményből.

**A gyűjteményből felismerése felvételéhez hajtsa végre az alábbi lépéseket:**

1. Az **Azure klasszikus portálon**kattintson a bal oldali navigációs területen kattintson az **Active Directory**. 

    ![Az Active Directory][1]

2. A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3. Nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson a felső menüben **alkalmazásokat** .
    
    ![Alkalmazások][2]

4. Kattintson a **Hozzáadás** az oldal alján.
    
    ![Alkalmazások][3]

5. **Milyen feladatot szeretne tenni** párbeszédpanelen kattintson **a gyűjteményből az alkalmazások felvétele**.

    ![Alkalmazások][4]

6. A Keresés mezőbe írja be a **felismerés**.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_01.png)

7. Az eredmények panelen válassza ki a **felismerés**, és válassza a **kész** , az alkalmazás hozzáadása.

    ![Az alkalmazás kijelöli a gyűjteményben](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_0001.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Beállítása és tesztelése Azure Active Directory egyszeri bejelentkezés
Ez a szakasz célja mutatja, hogy beállítása és tesztelése Azure AD az egyszeri bejelentkezés, az úgynevezett "Britta Simon" próba felhasználón alapuló felismerése.

Az egyszeri bejelentkezési a munkát az Azure Active Directory kell, hogy mi az a partner felhasználó felismerése az Azure Active Directory-felhasználónak. Ez azt jelenti Azure AD-felhasználó, és a kapcsolódó felhasználó felismerése hivatkozás viszonya kell létrehozni.

Ez a hivatkozás kapcsolat a értéket a **felhasználó nevét** a program a **felhasználónév** a felismerés Azure Active Directory hozzárendelésével jön létre.

Állítsa be, és Azure Active Directory egyszeri bejelentkezéssel való felismerése tesztelése, a következő építőelemek szükséges:

1. **[Azure Active Directory konfigurálása az egyszeri bejelentkezés](#configuring-azure-ad-single-single-sign-on)** - engedélyezése a felhasználóknak, hogy ezzel a szolgáltatással.
2. **[Felhasználó létrehozása az Azure Active Directory tesztelése](#creating-an-azure-ad-test-user)** - Azure Active Directory egyszeri bejelentkezéssel való Britta Simon ellenőrzéséhez.
3. **[Felhasználó létrehozása egy felismerése tesztelése](#creating-a-recognize-test-user)** - felismerés, amelyen az Azure Active Directory ábrázolt csatolt egy megfelelője a Britta Simon van-e.
4. **[Az Azure Active Directory hozzárendelése tesztelje a felhasználó](#assigning-the-azure-ad-test-user)** - ahhoz, hogy Britta Simon Azure AD az egyszeri bejelentkezés használata.
5. **[Tesztelés egyszeri bejelentkezés](#testing-single-sign-on)** - ellenőrzéséhez, hogy működik-e a konfigurációs.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure Active Directory egyszeri bejelentkezés beállítása

Ebben a részben Azure AD az egyszeri bejelentkezés a klasszikus portálon engedélyezése, és a felismerés alkalmazásban az egyszeri bejelentkezés beállítása.

**Állítson be Azure AD az egyszeri bejelentkezés felismerése, hajtsa végre az alábbi lépéseket:**

1. Az klasszikus portálon **felismerése** alkalmazás integrációs lapon kattintson **konfigurálása az egyszeri bejelentkezés** a **Konfigurálása az egyszeri bejelentkezés** párbeszédpanel megnyitásához.
     
    ![Egyszeri bejelentkezés beállítása][6] 

2. **Hogyan szeretné, hogy jelentkezzen be az felismerése felhasználók** lapon válassza az **Azure Active Directory Single Sign-On**, és kattintson a **Tovább gombra**.
    
    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_03.png)

3. Az **Alkalmazás beállításainak megadása** párbeszédpanel lapon hajtsa végre az alábbi lépéseket, és kattintson a **Tovább**gombra:

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_04.png)

    egy. Az **Bejelentkezési a URL-cím** mezőbe írja be az URL-cím használata a következő mintát: `https://recognizeapp.com/<your-domain>/saml/sso`.

    b. Az **azonosító** mezőbe írja be az URL-cím használata a következő mintát: `https://recognizeapp.com/<your-domain>/saml/metadata`.

    c billentyűkombinációt. Kattintson a **Tovább** gombra

    > [AZURE.NOTE] Ha nem tudja, hogy az alábbi URL-kapcsolatban, írja be a minta URL-címeit, például mintázattal. Ha ezeket az értékeket, további részletekért olvassa el a lépés 9 vagy felismerése támogatási csoport munkatársaitól kaphat keresztül <mailto:support@recognizeapp.com>.

4. A **beállítás az egyszeri bejelentkezés felismerése a** lapon kattintson a **tanúsítvány letöltése** , és mentse a fájlt a számítógépen:

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_05.png)

5. A különböző webes böngészőablakban bejelentkezés az Ön felismerése bérlői rendszergazdaként.

6. Kattintson a jobb felső sarokban a **menü**. Kattintson a **vállalati rendszergazda**.

    ![Egyszeri bejelentkezés a alkalmazás egymás konfigurálása](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_000.png)

7. A bal oldali navigációs területen kattintson a **Beállítások**gombra.

    ![Egyszeri bejelentkezés a alkalmazás egymás konfigurálása](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_001.png)

8. A következő lépésekkel **SSO beállítások** részén.

    ![Egyszeri bejelentkezés a alkalmazás egymás konfigurálása](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_002.png)

    egy. **SSO engedélyezése**jelölje ki a **be**.

    b. A **IDP entitás azonosítója** szövegdoboz helyezi el a **Kibocsátó** URL-cím értéket az Azure Active Directory-alkalmazás konfigurálása varázsló.

    c billentyűkombinációt. Az **egyszeri bejelentkezés célhely URL-címét** a szövegdoboz **Egyszeri bejelentkezéses szolgáltatás URL-címe** értékének helyezi az Azure Active Directory-alkalmazás konfigurálása varázsló.

    d. A **zat célhely URL-címét** a szövegdoboz **Egyetlen Sign-Out szolgáltatás URL-címe** értékének elhelyezni az Azure Active Directory-alkalmazás konfigurálása varázsló.

    e. Nyissa meg a letöltött tanúsítvány-fájl a Jegyzettömbben, a tartalmát a vágólapra másolja és illessze be a **tanúsítvány** mezőben lévő értéket szeretné. 

    f. Kattintson a **Beállítások mentése** gombra. 

9. A **Service Provider metaadatok URL-címe**URL másolása másokkal az **Egyszeri bejelentkezés beállításai** csoportban.
    
    ![Egyszeri bejelentkezés a alkalmazás egymás konfigurálása](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_003.png)

10. Nyissa meg a metaadat-dokumentum letöltése üres böngészőben a **metaadat-alapú hivatkozásra** . Ezután használja EntityDescriptor felismerése megadott érték, az **azonosító** az **Alkalmazás beállításainak megadása** párbeszédpanelen.

    ![Egyszeri bejelentkezés a alkalmazás egymás konfigurálása](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_004.png)

11. A klasszikus portálon jelölje be a egyszeri bejelentkezéses konfigurációs megerősítés, és kattintson a **Tovább gombra**.
    
    ![Azure Active Directory Single Sign-On][10]

12. Az **egyszeri bejelentkezés megerősítés** lapon kattintson a **Befejezés**gombra.  
    
    ![Azure Active Directory Single Sign-On][11]



### <a name="creating-an-azure-ad-test-user"></a>Azure AD-próba felhasználó létrehozása
Ez a szakasz célja próba felhasználó létrehozása az úgynevezett Britta Simon klasszikus portálon.

![Azure Active Directory-felhasználó létrehozása][20]

**Teszt felhasználó létrehozása az Azure Active Directory, hajtsa végre az alábbi lépéseket:**

1. Az **Azure klasszikus portálon**kattintson a bal oldali navigációs területen kattintson az **Active Directory**.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-recognize-tutorial/create_aaduser_09.png)

2. A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3. A menüben, a képernyő tetején, a felhasználók listájának megjelenítéséhez kattintson a **felhasználók**elemre.
    
    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-recognize-tutorial/create_aaduser_03.png)

4. **Felhasználó hozzáadása**elemre az alsó eszköztáron a **Felhasználó hozzáadása** párbeszédpanel megnyitásához.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-recognize-tutorial/create_aaduser_04.png)

5. A párbeszédpanel **közli velünk a felhasználóra vonatkozó** lapon hajtsa végre az alábbi lépéseket:

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-recognize-tutorial/create_aaduser_05.png)

    egy. Felhasználó típusa jelölje be az új felhasználó a szervezet.

    b. A felhasználónév **szövegmezőbe**írja be a **BrittaSimon**.

    c billentyűkombinációt. Kattintson a **Tovább**gombra.

6.  A **Felhasználói profil** párbeszédpanel lapon hajtsa végre az alábbi lépéseket:
    
    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-recognize-tutorial/create_aaduser_06.png)

    egy. Az **első neve** mezőbe írja be a **Britta**.  

    b. Az **Utolsó neve** mezőbe írja be a **Simon**.

    c billentyűkombinációt. A **Megjelenítendő név** mezőbe írja be a **Britta Simon**.

    d. A **szerepkör** listában jelölje ki a **felhasználót**.

    e. Kattintson a **Tovább**gombra.

7. A párbeszédpanel **ideiglenes jelszót kérjen** lapon kattintson a **Hozzon létre**.
    
    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-recognize-tutorial/create_aaduser_07.png)

8. A párbeszédpanel **ideiglenes jelszót első** lapon hajtsa végre az alábbi lépéseket:
    
    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-recognize-tutorial/create_aaduser_08.png)

    egy. Jegyezze fel az **Új jelszót**az érték.

    b. Kattintson a **kész**gombra.   



### <a name="creating-a-recognize-test-user"></a>Felismerése próba felhasználó létrehozása

Ahhoz, hogy az Azure Active Directory-felhasználóknak, hogy jelentkezzen be a felismerés, hogy ki kell építenie felismerése be. Felismerése, ha a feladat manuális kiépítési.

Az alkalmazás nem támogatja a kiépítési SCIM, de van egy másik felhasználó szinkronizálási, hogy a felhasználók rendelkezések. 

####<a name="to-provision-a-user-account-perform-the-following-steps"></a>Építse egy felhasználói fiókot, hajtsa végre az alábbi lépéseket:

1.  Jelentkezzen be a felismerés vállalati webhely rendszergazdaként.

2.  Kattintson a jobb felső sarokban a **menü**. Kattintson a **vállalati rendszergazda**.

3.  A bal oldali navigációs területen kattintson a **Beállítások**gombra.

4.  Hajtsa végre az alábbi lépéseket a **Felhasználói szinkronizálás** csoportban.
    
    ![Új felhasználó] (./media/active-directory-saas-recognize-tutorial/tutorial_recognize_005.png "Új felhasználó")

    egy. **Szinkronizálás engedélyezve van**-e jelölje ki a **be**.

    b. **Választható szinkronizálási szolgáltató**, jelölje ki a **Microsoft és az Office 365**.

    c billentyűkombinációt. Kattintson a **felhasználó-szinkronizálás futtatása**

### <a name="assigning-the-azure-ad-test-user"></a>Az Azure Active Directory-próba felhasználói hozzárendelése

Ez a szakasz célja Britta Simon Azure egyszeri bejelentkezés a saját hozzáférést biztosít felismerése használandó engedélyezése.
    
![Felhasználó hozzárendelése][200]

**Britta Simon hozzárendelése felismerése, hajtsa végre az alábbi lépéseket:**

1. A klasszikus portál kattintva nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson az **alkalmazások** parancsra a felső menüben.
    
    ![Felhasználó hozzárendelése][201]

2. Az alkalmazások listájában jelölje ki a **felismerés**.
    
    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_50.png)

3. A felső sávon kattintson a **felhasználók**elemre.
    
    ![Felhasználó hozzárendelése][203]

4. A felhasználók listában jelölje ki a **Britta Simon**.

5. Az alsó eszköztáron kattintson a **hozzárendelni**.
    
    ![Felhasználó hozzárendelése][205]

### <a name="testing-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

Ez a szakasz célja a Azure Active Directory egyszeri bejelentkezéses beállítások tesztelése a Access panelen.
 
Amikor az Access panel a felismerés mozaik gombra kattint, meg kell első automatikusan aláírt-on az felismerése alkalmazás.

## <a name="additional-resources"></a>További források

* [A szoftver alkalmazások integrálása az Azure Active Directory oktatóanyagok listája](active-directory-saas-tutorial-list.md)
* [Mi az access alkalmazás és az egyszeri bejelentkezés az Azure Active Directory címtárral?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_205.png
