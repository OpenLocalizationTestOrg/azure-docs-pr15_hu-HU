<properties
    pageTitle="Oktatóprogram: Azure Active Directory-integráció a Yonyx interaktív útmutatók |} Microsoft Azure"
    description="Megtudhatja, hogy miként konfigurálása az egyszeri bejelentkezés Azure Active Directory és Yonyx interaktív útmutatók között."
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
    ms.date="10/26/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-yonyx-interactive-guides"></a>Oktatóprogram: Azure Active Directory-integráció a Yonyx interaktív útmutatók

Ebben az oktatóanyagban célja mutatja, hogy miként Yonyx interaktív útmutatók integrálása az Azure Active Directory (Azure Active Directory).

Interaktív útmutatók Yonyx integrálása az Azure Active Directory biztosít a következő előnyökkel jár:

- Az Azure Active Directory, aki rendelkezik hozzáféréssel Yonyx interaktív útmutatók megadhatja, hogy
- Az Azure Active Directory-fiókok engedélyezheti a felhasználóknak, hogy automatikusan első jelentkezett-on való Yonyx interaktív útmutatók (Single Sign-On)
- A fiókokat az egy központi helyen – az Azure klasszikus portálra

Ha többet szeretne tudni a szoftver alkalmazás integrálása az Azure AD meg, hogy, ismerje meg, [az access alkalmazás és az egyszeri bejelentkezés az Azure Active Directory címtárral](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek

Azure Active Directory integráció a Yonyx interaktív útmutatók van szükség az alábbiakat:

- Az Azure Active Directory-előfizetéssel
- Egy interaktív útmutatók Yonyx egyszeri bejelentkezés engedélyezett előfizetés


> [AZURE.NOTE] Az oktatóprogram lépéseit teszteléséhez nem használata ajánlott munkakörnyezetben.


Az oktatóprogram lépéseit teszteléséhez célszerű követnie az alábbi javaslatokat:

- Erre akkor szükség, ne használja a termelési környezetén.
- Ha nincs telepítve az Azure Active Directory próba környezetben, kattint egy hónap próba [Itt](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban célja lehetővé teszi az Azure Active Directory egyszeri bejelentkezés tesztkörnyezetben tesztelése.

Az alkalmazási példát, ebben az oktatóanyagban tagolt két fő építőelemek áll:

1. Interaktív útmutatók Yonyx hozzáadása a gyűjteményből
2. Beállítása és tesztelése Azure Active Directory egyszeri bejelentkezés


## <a name="adding-yonyx-interactive-guides-from-the-gallery"></a>Interaktív útmutatók Yonyx hozzáadása a gyűjteményből
Konfigurálja az Azure Active Directory integrálása a Yonyx interaktív útmutatók, meg kell Yonyx interaktív útmutatók hozzáadása az felügyelt szoftver alkalmazáslistában a gyűjteményből.

**A gyűjteményből Yonyx interaktív útmutatók felvételéhez hajtsa végre az alábbi lépéseket:**

1. Az **Azure klasszikus portálon**kattintson a bal oldali navigációs területen kattintson az **Active Directory**. 

    ![Az Active Directory][1]

2. A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3. Nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson a felső menüben **alkalmazásokat** .
    
    ![Alkalmazások][2]

4. Kattintson a **Hozzáadás** az oldal alján.
    
    ![Alkalmazások][3]

5. **Milyen feladatot szeretne tenni** párbeszédpanelen kattintson **a gyűjteményből az alkalmazások felvétele**.

    ![Alkalmazások][4]

6. A Keresés mezőbe írja be a **Yonyx interaktív útmutatók**.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-yonyx-tutorial/tutorial_yonyx_01.png)

7. Az eredmények panelen válassza ki a **Yonyx interaktív útmutatók**, és válassza a **kész** , az alkalmazás hozzáadása.

    ![Az alkalmazás kijelöli a gyűjteményben](./media/active-directory-saas-yonyx-tutorial/tutorial_yonyx_0001.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Beállítása és tesztelése Azure Active Directory egyszeri bejelentkezés
Ez a szakasz célja mutatja, hogy beállítása és Azure Active Directory egyszeri bejelentkezés a "Britta Simon" nevű próba felhasználón alapuló Yonyx interaktív segédvonalakkal tesztelése.

Az egyszeri bejelentkezési a munkát az Azure Active Directory kell, hogy mi az a partner felhasználó Yonyx interaktív útmutatók az Azure Active Directory-felhasználónak. Ez azt jelenti Azure AD-felhasználó, és a kapcsolódó felhasználó Yonyx interaktív útmutatók hivatkozás viszonya kell létrehozni.

Ez a hivatkozás kapcsolat az Azure Active Directory, a program a **felhasználónév** Yonyx interaktív útmutatók a az érték, a **felhasználónév** hozzárendelésével jön létre.

Állítsa be és Azure Active Directory egyszeri bejelentkezés Yonyx interaktív segédvonalakkal teszteléséhez a következő építőelemek szükséges:

1. **[Azure Active Directory konfigurálása az egyszeri bejelentkezés](#configuring-azure-ad-single-single-sign-on)** - engedélyezése a felhasználóknak, hogy ezzel a szolgáltatással.
2. **[Felhasználó létrehozása az Azure Active Directory tesztelése](#creating-an-azure-ad-test-user)** - Azure Active Directory egyszeri bejelentkezéssel való Britta Simon ellenőrzéséhez.
3. **[Felhasználó létrehozása egy Yonyx interaktív útmutatók tesztelése](#creating-a-yonyx-interactive-guides-test-user)** - van-e egy megfelelője a Britta Simon Yonyx interaktív útmutatók, amelyen az Azure Active Directory ábrázolt csatolt.
4. **[Az Azure Active Directory hozzárendelése tesztelje a felhasználó](#assigning-the-azure-ad-test-user)** - ahhoz, hogy Britta Simon Azure AD az egyszeri bejelentkezés használata.
5. **[Tesztelés egyszeri bejelentkezés](#testing-single-sign-on)** - ellenőrzéséhez, hogy működik-e a konfigurációs.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure Active Directory egyszeri bejelentkezés beállítása

Ebben a részben Azure AD az egyszeri bejelentkezés a klasszikus portálon engedélyezése, és a Yonyx interaktív útmutatók alkalmazásban az egyszeri bejelentkezés beállítása.

**Állítson be Azure AD az egyszeri bejelentkezés Yonyx interaktív útmutatók, hajtsa végre az alábbi lépéseket:**

1. A klasszikus portálon **Yonyx interaktív útmutatók** alkalmazás integrációs lapon kattintson **konfigurálása az egyszeri bejelentkezés** a **Konfigurálása az egyszeri bejelentkezés** párbeszédpanel megnyitásához.
     
    ![Egyszeri bejelentkezés beállítása][6] 

2. **Hogyan szeretné, hogy jelentkezzen be az interaktív útmutatók Yonyx felhasználók** lapon válassza az **Azure Active Directory Single Sign-On**, és kattintson a **Tovább gombra**.
    
    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-yonyx-tutorial/tutorial_yonyx_03.png)

3. Az **Alkalmazás beállításainak megadása** párbeszédpanel lapon hajtsa végre az alábbi lépéseket, és kattintson a **Tovább**gombra:

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-yonyx-tutorial/tutorial_yonyx_04.png)

    egy. Az **Bejelentkezési a URL-cím** mezőbe írja be az URL-cím használata a következő mintát: `https://<company name>.yonyx.com/y/conversation/?id=<guid number>`.

    b. Az **azonosító** mezőbe írja be az URL-cím használata a következő mintát: `https://<company name>.yonyx.com`.

    c billentyűkombinációt. Kattintson a **Tovább** gombra

    > [AZURE.NOTE] Felhívjuk a figyelmét arra, hogy van-e ezek az értékek frissítése a tényleges bejelentkezési a URL-CÍMEK és azonosítója. Ha ezeket az értékeket, forduljon az interaktív útmutatók támogatási csoportjának, keresztül Yonyx <mailto:support@yonyx.com>.

4. A **beállítás az egyszeri bejelentkezés Yonyx interaktív útmutatók a** lapon kattintson a **tanúsítvány letöltése** , és mentse a fájlt a számítógépen:

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-yonyx-tutorial/tutorial_yonyx_05.png)

5. Az alkalmazás, a kapcsolattartó Yonyx interaktív útmutatók a támogatási csapat keresztül konfigurált SSO megszerezni <mailto:support@yonyx.com> , és küldje el nekik a következő:

    • A letöltött **tanúsítvány**

    • **Kibocsátó URL-címe**

    • Az **egyszeri bejelentkezéses szolgáltatás URL-címe**

    • **Egyetlen kijelentkezési szolgáltatás URL-címe**

6. A klasszikus portálon jelölje be a egyszeri bejelentkezéses konfigurációs megerősítés, és kattintson a **Tovább gombra**.
    
    ![Azure Active Directory Single Sign-On][10]

7. Az **egyszeri bejelentkezés megerősítés** lapon kattintson a **Befejezés**gombra.  
    
    ![Azure Active Directory Single Sign-On][11]



### <a name="creating-an-azure-ad-test-user"></a>Azure AD-próba felhasználó létrehozása
Ez a szakasz célja próba felhasználó létrehozása az úgynevezett Britta Simon klasszikus portálon.

![Azure Active Directory-felhasználó létrehozása][20]

**Teszt felhasználó létrehozása az Azure Active Directory, hajtsa végre az alábbi lépéseket:**

1. Az **Azure klasszikus portálon**kattintson a bal oldali navigációs területen kattintson az **Active Directory**.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-yonyx-tutorial/create_aaduser_09.png)

2. A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3. A menüben, a képernyő tetején, a felhasználók listájának megjelenítéséhez kattintson a **felhasználók**elemre.
    
    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-yonyx-tutorial/create_aaduser_03.png)

4. **Felhasználó hozzáadása**elemre az alsó eszköztáron a **Felhasználó hozzáadása** párbeszédpanel megnyitásához.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-yonyx-tutorial/create_aaduser_04.png)

5. A párbeszédpanel **közli velünk a felhasználóra vonatkozó** lapon hajtsa végre az alábbi lépéseket:

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-yonyx-tutorial/create_aaduser_05.png)

    egy. Felhasználó típusa jelölje be az új felhasználó a szervezet.

    b. A felhasználónév **szövegmezőbe**írja be a **BrittaSimon**.

    c billentyűkombinációt. Kattintson a **Tovább**gombra.

6.  A **Felhasználói profil** párbeszédpanel lapon hajtsa végre az alábbi lépéseket:
    
    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-yonyx-tutorial/create_aaduser_06.png)

    egy. Az **első neve** mezőbe írja be a **Britta**.  

    b. Az **Utolsó neve** mezőbe írja be a **Simon**.

    c billentyűkombinációt. A **Megjelenítendő név** mezőbe írja be a **Britta Simon**.

    d. A **szerepkör** listában jelölje ki a **felhasználót**.

    e. Kattintson a **Tovább**gombra.

7. A párbeszédpanel **ideiglenes jelszót kérjen** lapon kattintson a **Hozzon létre**.
    
    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-yonyx-tutorial/create_aaduser_07.png)

8. A párbeszédpanel **ideiglenes jelszót első** lapon hajtsa végre az alábbi lépéseket:
    
    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-yonyx-tutorial/create_aaduser_08.png)

    egy. Jegyezze fel az **Új jelszót**az érték.

    b. Kattintson a **kész**gombra.   



### <a name="creating-a-yonyx-interactive-guides-test-user"></a>Interaktív útmutatók Yonyx próba felhasználó létrehozása

Ez a szakasz célja Britta Simon nevű Yonyx interaktív útmutatók felhasználó létrehozása. Yonyx interaktív útmutatók támogatja a kiépítési just time, amely alapértelmezés szerint ki van engedélyezve van.

Nincs teendő ebben a szakaszban a meg nem. Új felhasználóként Adobe kreatív felhő elérése, ha még nem létezik kísérlet során jön létre.

> [AZURE.NOTE] Ha egy felhasználó létrehozása manuálisan kell szeretne-e a Yonyx interaktív útmutatók támogatási csoport munkatársaitól kaphat keresztül <mailto:support@yonyx.com>.


### <a name="assigning-the-azure-ad-test-user"></a>Az Azure Active Directory-próba felhasználói hozzárendelése

Ez a szakasz célja Britta Simon használandó Azure egyszeri bejelentkezés a saját hozzáférést biztosít Yonyx interaktív útmutatók segítségével, így.
    
![Felhasználó hozzárendelése][200]

**Interaktív útmutatók Yonyx Britta Simon rendelni, hajtsa végre az alábbi lépéseket:**

1. A klasszikus portál kattintva nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson az **alkalmazások** parancsra a felső menüben.
    
    ![Felhasználó hozzárendelése][201]

2. Az alkalmazások listájában jelölje ki a **Yonyx interaktív útmutatók**.
    
    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-yonyx-tutorial/tutorial_yonyx_50.png)

3. A felső sávon kattintson a **felhasználók**elemre.
    
    ![Felhasználó hozzárendelése][203]

4. A felhasználók listában jelölje ki a **Britta Simon**.

5. Az alsó eszköztáron kattintson a **hozzárendelni**.
    
    ![Felhasználó hozzárendelése][205]

### <a name="testing-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

Ez a szakasz célja a Azure Active Directory egyszeri bejelentkezéses beállítások tesztelése a Access panelen.
 
Amikor az Access panel a Yonyx interaktív útmutatók mozaik gombra kattint, meg kell első automatikusan aláírt-on az interaktív útmutatók Yonyx alkalmazás.

## <a name="additional-resources"></a>További források

* [A szoftver alkalmazások integrálása az Azure Active Directory oktatóanyagok listája](active-directory-saas-tutorial-list.md)
* [Mi az access alkalmazás és az egyszeri bejelentkezés az Azure Active Directory címtárral?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-yonyx-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-yonyx-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-yonyx-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-yonyx-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-yonyx-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-yonyx-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-yonyx-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-yonyx-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-yonyx-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-yonyx-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-yonyx-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-yonyx-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-yonyx-tutorial/tutorial_general_205.png
