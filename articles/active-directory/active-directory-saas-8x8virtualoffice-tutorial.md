<properties
    pageTitle="Oktatóprogram: Azure Active Directory-integráció a 8 x 8 virtuális Office |} Microsoft Azure"
    description="Megtudhatja, hogy miként konfigurálása az egyszeri bejelentkezés Azure Active könyvtár és a 8 x 8 virtuális Office között."
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
    ms.date="10/10/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-8x8-virtual-office"></a>Oktatóprogram: Azure Active Directory-integráció a 8 x 8 virtuális Office

Ebben az oktatóanyagban célja mutatja, hogy miként 8 x 8 virtuális Office integrálása az Azure Active Directory (Azure Active Directory).

8 x 8 integrálása az Azure Active Directory virtuális Office biztosít a következő előnyökkel jár:

- Az Azure Active Directory, aki rendelkezik hozzáféréssel a 8 x 8 virtuális Office megadhatja, hogy
- Az Azure Active Directory-fiókok engedélyezheti a felhasználóknak, hogy automatikusan első aláírással a 8 x 8 virtuális Office (Single Sign-On)
- A fiókokat az egy központi helyen – az Azure klasszikus portálra

Ha többet szeretne tudni a szoftver alkalmazás integrálása az Azure AD meg, hogy, ismerje meg, [az access alkalmazás és az egyszeri bejelentkezés az Azure Active Directory címtárral](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek

Azure Active Directory integráció 8 x 8 virtuális Office van szükség az alábbiakat:

- Az Azure Active Directory-előfizetéssel
- A 8 x 8 virtuális Office egyszeri bejelentkezés engedélyezett előfizetés


> [AZURE.NOTE] Az oktatóprogram lépéseit teszteléséhez nem használata ajánlott munkakörnyezetben.


Az oktatóprogram lépéseit teszteléséhez célszerű követnie az alábbi javaslatokat:

- Erre akkor szükség, ne használja a termelési környezetén.
- Ha nincs telepítve az Azure Active Directory próba környezetben, kattint egy hónap próba [Itt](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban célja lehetővé teszi a Microsoft Azure Active Directory egyszeri bejelentkezés tesztkörnyezetben tesztelése.

Az alkalmazási példát, ebben az oktatóanyagban tagolt két fő építőelemek áll:

1. 8 x 8 virtuális Office hozzáadása a gyűjteményből
2. Beállítása és tesztelése a Microsoft Azure Active Directory Single Sign-On


## <a name="adding-8x8-virtual-office-from-the-gallery"></a>8 x 8 virtuális Office hozzáadása a gyűjteményből
Azure Active Directory integrálása a 8 x 8 virtuális Office konfigurálásához szüksége 8 x 8 virtuális Office hozzáadása felügyelt szoftver alkalmazásai a gyűjteményből.

**8 x 8 virtuális Office a gyűjteményből felvételéhez hajtsa végre az alábbi lépéseket:**

1. Az **Azure klasszikus portálon**kattintson a bal oldali navigációs területen kattintson az **Active Directory**.

    ![Az Active Directory][1]

2. A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3. Nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson a felső menüben **alkalmazások** .
    
    ![Alkalmazások][2]

4. Kattintson a **Hozzáadás** az oldal alján.

    ![Alkalmazások][3]

5. **Milyen feladatot szeretne tenni** párbeszédpanelen kattintson **a gyűjteményből az alkalmazások felvétele**.

    ![Alkalmazások][4]

6. A Keresés mezőbe írja be a **8 x 8 virtuális Office**.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_01.png)
7. Az eredmény ablaktáblában jelölje ki a **8 x 8 virtuális Office**pontra, és kattintson a **kész** az alkalmazás hozzáadása parancsra.

    ![Az alkalmazás kijelöli a gyűjteményben](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_0001.png)


##  <a name="configuring-and-testing-microsoft-azure-ad-single-sign-on"></a>Beállítása és tesztelése a Microsoft Azure Active Directory Single Sign-On
Ez a szakasz célja mutatja, hogy beállítása és tesztelése a Microsoft Azure Active Directory Single Sign-On a virtuális Office "Britta Simon" nevű próba felhasználón alapuló 8 x 8 rendszerben.

Az egyszeri bejelentkezési a munkát az Azure Active Directory kell, hogy milyen megfelelőjük a felhasználó a 8 x 8-ban egy felhasználó az Azure Active Directory virtuális Office-e. Más szóval Azure AD-felhasználó, és a kapcsolódó felhasználó 8 x 8 hivatkozás viszonya virtuális Office kell létrehozni.

Ez a hivatkozás kapcsolat által a **felhasználónév** értéket rendeli az Azure Active Directory, a program a **felhasználónév** a 8 x 8 virtuális Office jön létre.

Beállítása és tesztelése a Microsoft Azure Active Directory egyszeri bejelentkezés a 8 x 8 virtuális Office, a következő építőelemek szükséges:

1. **[Microsoft Azure Active Directory konfigurálása az egyszeri bejelentkezés](#configuring-azure-ad-single-single-sign-on)** - engedélyezése a felhasználóknak, hogy ezzel a szolgáltatással.
2. **[Felhasználó létrehozása az Azure Active Directory tesztelése](#creating-an-azure-ad-test-user)** - kipróbálni a Microsoft Azure Active Directory egyszeri bejelentkezéssel való Britta Simon.
3. **[8 x 8 virtuális Office próba felhasználó létrehozása](#creating-a-8x8-virtual-office-test-user)** - 8 x 8 virtuális Office, amelyen az Azure Active Directory ábrázolt csatolt egy megfelelője a Britta Simon van-e.
4. **[Az Azure Active Directory hozzárendelése tesztelje a felhasználó](#assigning-the-azure-ad-test-user)** - ahhoz, hogy a Microsoft Azure Active Directory egyszeri bejelentkezés használata Britta Simon.
5. **[Tesztelés egyszeri bejelentkezés](#testing-single-sign-on)** - ellenőrzéséhez, hogy működik-e a konfigurációs.

### <a name="configuring-microsoft-azure-ad-single-sign-on"></a>Microsoft Azure Active Directory Single Sign-On konfigurálása

Ebben a részben a Microsoft Azure Active Directory Single Sign-On a klasszikus portálon engedélyezése, és a 8 x 8 virtuális Office-alkalmazást az egyszeri bejelentkezés beállítása.

**8 x 8 virtuális Office konfigurálása a Microsoft Azure Active Directory Single Sign-On, hajtsa végre az alábbi lépéseket:**

1. A klasszikus portálon **8 x 8 virtuális Office** alkalmazás integrációs lapon kattintson **konfigurálása az egyszeri bejelentkezés** a **Konfigurálása az egyszeri bejelentkezés** párbeszédpanel megnyitásához.
     
    ![Egyszeri bejelentkezés beállítása][6] 

2. A lapon **, például a felhasználóknak, hogy jelentkezzen be a 8 x 8 virtuális Office hogyan szeretné** jelölje be **A Microsoft Azure Active Directory Single Sign-On**, és kattintson a **Tovább gombra**.

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_03.png) 

3. Az **Alkalmazás beállításainak megadása** párbeszédpanel lapon hajtsa végre az alábbi lépéseket, és kattintson a **Tovább**gombra:

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_04.png)

    egy. A **Válasz URL-cím** mezőbe írja be:`https://sso.8x8.com/saml2`

    b. Kattintson a **Tovább** gombra

4. **Egyszeri bejelentkezés 8 x 8 virtuális Office konfigurálása** lapon hajtsa végre az alábbi lépéseket, és kattintson a **Tovább**gombra:

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_05.png)

    egy. Kattintson a **tanúsítvány letöltése**, és mentse a fájlt a számítógépen.

    b. Kattintson a **Tovább**gombra.

5. Bejelentkezés az Ön 8 x 8 virtuális Office bérlői rendszergazdaként.
6. Jelölje ki a **virtuális Office fiókot kezelő** alkalmazás panelen.

    ![Alkalmazás oldalon konfigurálása](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_001.png)

7. Jelölje ki a **Vállalati verziós** fiókban kezeléséhez, és kattintson a **Bejelentkezés** gombra.

    ![Alkalmazás oldalon konfigurálása](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_002.png)

8. Kattintson a menü listában a **fiókok** fülre.

    ![Alkalmazás oldalon konfigurálása](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_003.png)

9. Kattintson **Az egyszeri bejelentkezés** a fiókok listáját.

    ![Alkalmazás oldalon konfigurálása](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_004.png)

10. **Signle bejelentkezés** hitelesítési módszer csoportban válassza ki, majd kattintson a **SAML**.

    ![Alkalmazás oldalon konfigurálása](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_005.png)

11. Másolja **SAML egyszeri bejelentkezési URL-CÍMÉT**, **egyetlen folyamata szolgáltatás URL-CÍMÉT,** és a **kibocsátó URL-cím** Azure Active Directory **Jelentkezzen be az URL-CÍMÉT**, **URL-CÍMÉT a Kijelentkezés** és 8 x 8 virtuális Office **kibocsátó URL-CÍMÉT** . 

    ![Alkalmazás oldalon konfigurálása](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_006.png)

    ![Alkalmazás oldalon konfigurálása](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_007.png)

12. **Böngészőben** gombra kattintva töltse fel a tanúsítvány, amely letöltötte az Azure Active Directory.

13. Kattintson a **Mentés** gombra.

14. A klasszikus portálon válassza ki az egyszeri bejelentkezéses konfigurációs megerősítő, és kattintson a **Tovább gombra**.

    ![Azure Active Directory Single Sign-On][10]

15. Az **egyszeri bejelentkezés megerősítés** lapon kattintson a **Befejezés**gombra.  

    ![Azure Active Directory Single Sign-On][11]



### <a name="creating-an-azure-ad-test-user"></a>Azure AD-próba felhasználó létrehozása
Ez a szakasz célja próba felhasználó létrehozása az úgynevezett Britta Simon klasszikus portálon.
    
![Azure Active Directory-felhasználó létrehozása][20]

**Teszt felhasználó létrehozása az Azure Active Directory, hajtsa végre az alábbi lépéseket:**

1. Az **Azure klasszikus portálon**kattintson a bal oldali navigációs területen kattintson az **Active Directory**.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-8x8virtualoffice-tutorial/create_aaduser_09.png)

2. A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3. A menüben, a képernyő tetején, a felhasználók listájának megjelenítéséhez kattintson a **felhasználók**elemre.
    
    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-8x8virtualoffice-tutorial/create_aaduser_03.png)

4. **Felhasználó hozzáadása**elemre az alsó eszköztáron a **Felhasználó hozzáadása** párbeszédpanel megnyitásához.
    
    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-8x8virtualoffice-tutorial/create_aaduser_04.png)

5. A párbeszédpanel **közli velünk a felhasználóra vonatkozó** lapon hajtsa végre az alábbi lépéseket:

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-8x8virtualoffice-tutorial/create_aaduser_05.png)

    egy. Felhasználó típusa jelölje be az új felhasználó a szervezet.

    b. A felhasználónév **szövegmezőbe**írja be a **BrittaSimon**.

    c billentyűkombinációt. Kattintson a **Tovább**gombra.

6.  A **Felhasználói profil** párbeszédpanel lapon hajtsa végre az alábbi lépéseket:
    
    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-8x8virtualoffice-tutorial/create_aaduser_06.png)

    egy. Az **első neve** mezőbe írja be a **Britta**.  

    b. Az **Utolsó neve** mezőbe írja be, **Simon**.

    c billentyűkombinációt. A **Megjelenítendő név** mezőbe írja be a **Britta Simon**.

    d. A **szerepkör** listában jelölje ki a **felhasználót**.

    e. Kattintson a **Tovább**gombra.

7. A párbeszédpanel **ideiglenes jelszót első** lapon kattintson a **Hozzon létre**.
    
    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-8x8virtualoffice-tutorial/create_aaduser_07.png)

8. A párbeszédpanel **ideiglenes jelszót első** lapon hajtsa végre az alábbi lépéseket:
    
    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-8x8virtualoffice-tutorial/create_aaduser_08.png)

    egy. Jegyezze fel az **Új jelszót**az érték.

    b. Kattintson a **kész**gombra.   



### <a name="creating-a-8x8-virtual-office-test-user"></a>8 x 8 virtuális Office próba felhasználó létrehozása

Ez a szakasz célja Britta Simon nevű 8 x 8 virtuális Office felhasználó létrehozása. 8 x 8 virtuális Office támogatja a kiépítési just time, amely alapértelmezés szerint ki van engedélyezve van.

Nincs teendő ebben a szakaszban a meg nem. Ha még nem létezik a 8 x 8 virtuális Office eléréséhez kísérlet során új felhasználót hoz létre. 

> [AZURE.NOTE] Ha egy felhasználó létrehozása manuálisan kell szüksége a 8 x 8 virtuális Office támogatási csoport munkatársaitól kaphat.


### <a name="assigning-the-azure-ad-test-user"></a>Az Azure Active Directory-próba felhasználói hozzárendelése

Ez a szakasz célja Britta Simon használandó Azure egyszeri bejelentkezés a saját hozzáférést biztosít 8 x 8 virtuális Office engedélyezése.
    
![Felhasználó hozzárendelése][200]

**8 x 8 virtuális Office Britta Simon rendelni, hajtsa végre az alábbi lépéseket:**

1. A klasszikus portál kattintva nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson az **alkalmazások** parancsra a felső menüben.

    ![Felhasználó hozzárendelése][201]

2. Az alkalmazások listájában jelölje ki a **8 x 8 virtuális Office**.

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_50.png)

3. A felső sávon kattintson a **felhasználók**elemre.
    
    ![Felhasználó hozzárendelése][203]

4. A felhasználók listában jelölje ki a **Britta Simon**.

5. Az alsó eszköztáron kattintson a **hozzárendelni**.

    ![Felhasználó hozzárendelése][205]



### <a name="testing-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

Ez a szakasz célja a Microsoft Azure Active Directory egyszeri bejelentkezés beállítások tesztelése a Access panelen.

A 8 x 8 virtuális Office csempére az Access panel gombra kattintva meg kell első automatikusan aláírt-on a 8 x 8 virtuális Office alkalmazáshoz.


## <a name="additional-resources"></a>További források

* [A szoftver alkalmazások integrálása az Azure Active Directory oktatóanyagok listája](active-directory-saas-tutorial-list.md)
* [Mi az access alkalmazás és az egyszeri bejelentkezés az Azure Active Directory címtárral?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_205.png
