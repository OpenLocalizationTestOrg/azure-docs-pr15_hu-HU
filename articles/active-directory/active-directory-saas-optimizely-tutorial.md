<properties
    pageTitle="Oktatóprogram: Azure Active Directory-integráció a Optimizely |} Microsoft Azure"
    description="Megtudhatja, hogy miként konfigurálása az egyszeri bejelentkezés Azure Active Directory és Optimizely között."
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
    ms.date="09/11/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-optimizely"></a>Oktatóprogram: Azure Active Directory-integráció a Optimizely

Ebből az oktatóanyagból megismerheti, hogyan Optimizely integrálása az Azure Active Directory (Azure Active Directory).

Optimizely integrálása az Azure Active Directory biztosít a következő előnyökkel jár:

- Az Azure Active Directory, aki rendelkezik hozzáféréssel Optimizely megadhatja, hogy
- Engedélyezheti a felhasználóknak, hogy automatikusan első jelentkezett-on való Optimizely (Single Sign-On) az Azure Active Directory-fiókok
- A fiókokat az egy központi helyen – az Azure klasszikus portálra

Ha többet szeretne tudni a szoftver alkalmazás integrálása az Azure AD meg, hogy, ismerje meg, [az access alkalmazás és az egyszeri bejelentkezés az Azure Active Directory címtárral](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek

Azure Active Directory integráció a Optimizely van szükség az alábbiakat:

- Az Azure Active Directory-előfizetéssel
- Egy **Optimizely** egyszeri bejelentkezés engedélyezett előfizetés


> [AZURE.NOTE] Az oktatóprogram lépéseit teszteléséhez nem használata ajánlott munkakörnyezetben.


Az oktatóprogram lépéseit teszteléséhez célszerű követnie az alábbi javaslatokat:

- Erre akkor szükség, ne használja a termelési környezetén.
- Ha nincs telepítve az Azure Active Directory próba környezetben, kattint egy hónap próba [Itt](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban tesztelni Azure AD az egyszeri bejelentkezés tesztkörnyezetben. Az alkalmazási példát, ebben az oktatóanyagban tagolt két fő építőelemek áll:

1. Optimizely hozzáadása a gyűjteményből
2. Beállítása és tesztelése Azure Active Directory egyszeri bejelentkezés


## <a name="adding-optimizely-from-the-gallery"></a>Optimizely hozzáadása a gyűjteményből
Konfigurálja az Azure Active Directory integrálása a Optimizely, meg kell Optimizely hozzáadása az felügyelt szoftver alkalmazáslistában a gyűjteményből.

**A gyűjteményből Optimizely felvételéhez hajtsa végre az alábbi lépéseket:**

1. Az **Azure klasszikus portálon**kattintson a bal oldali navigációs területen kattintson az **Active Directory**. 

    ![Az Active Directory][1]

2. A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3. Nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson a felső menüben **alkalmazásokat** .

    ![Alkalmazások][2]

4. Kattintson a **Hozzáadás** az oldal alján.

    ![Alkalmazások][3]

5. **Milyen feladatot szeretne tenni** párbeszédpanelen kattintson **a gyűjteményből az alkalmazások felvétele**.

    ![Alkalmazások][4]

6. A Keresés mezőbe írja be a **Optimizely**.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-optimizely-tutorial/tutorial_optimizely_01.png)

7. Az eredmény ablaktáblában jelölje ki a **Optimizely**, és válassza a **kész** , az alkalmazás hozzáadása gombra.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-optimizely-tutorial/tutorial_optimizely_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Beállítása és tesztelése Azure Active Directory egyszeri bejelentkezés
Ebben a részben konfigurálása és a "Britta Simon" nevű próba felhasználón alapuló Optimizely az Azure Active Directory egyszeri bejelentkezés tesztelése.

Az egyszeri bejelentkezési a munkát az Azure Active Directory kell tudható, hogy mi a Optimizely megfelelőjük a felhasználó egy felhasználóhoz, az Azure Active Directory. Más szóval Azure AD-felhasználó, és a kapcsolódó felhasználó Optimizely hivatkozás viszonya kell létrehozni.
A hivatkozás kapcsolat jön létre által az Azure Active Directory Optimizely **felhasználónév** értékként a **felhasználó neve** értéket rendeli.

Állítsa be, és Azure Active Directory egyszeri bejelentkezéssel való Optimizely tesztelése, a következő építőelemek szükséges:

1. **[Azure Active Directory konfigurálása az egyszeri bejelentkezés](#configuring-azure-ad-single-single-sign-on)** - engedélyezése a felhasználóknak, hogy ezzel a szolgáltatással.
2. **[Felhasználó létrehozása az Azure Active Directory tesztelése](#creating-an-azure-ad-test-user)** - Azure Active Directory egyszeri bejelentkezéssel való Britta Simon ellenőrzéséhez.
4. **[Felhasználó létrehozása egy Optimizely tesztelése](#creating-an-optimizely-test-user)** - van-e egy megfelelője a Britta Simon Optimizely, amelyen az Azure Active Directory ábrázolt csatolt.
5. **[Az Azure Active Directory hozzárendelése tesztelje a felhasználó](#assigning-the-azure-ad-test-user)** - ahhoz, hogy Britta Simon Azure AD az egyszeri bejelentkezés használata.
5. **[Tesztelés egyszeri bejelentkezés](#testing-single-sign-on)** - ellenőrzéséhez, hogy működik-e a konfigurációs.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure Active Directory Single Sign-On konfigurálása

Ez a szakasz célja Azure AD az egyszeri bejelentkezés az Azure klasszikus portálon engedélyezése és a Optimizely alkalmazásban az egyszeri bejelentkezés beállítása.

Optimizely alkalmazás vár, a SAML Előfeltételek "e" nevű attribútumot tartalmazó. Az érték az "e" is Azure Active Directory első hitelesítette felismerte Optimizely e-mail kell lennie. Állítsa be az "e" állítást ehhez az alkalmazáshoz. A **"Atrributes"** lapon, az alkalmazás a következő attribútumok értékének kezelheti. Az alábbi képernyőképen látható, a. 


![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-optimizely-tutorial/tutorial_optimizely_03.png) 


**Állítson be Azure AD az egyszeri bejelentkezés Optimizely, hajtsa végre az alábbi lépéseket:**

1. Az Azure klasszikus portálon **Optimizely** alkalmazás integrációs lapján a menüben, a képernyő tetején kattintson a **attribútumok**elemre.
     
    ![Egyszeri bejelentkezés beállítása][5]

2. A SAML jogkivonat attribútumok párbeszédpanelen adja hozzá a "e" attribútumot.

    egy. Kattintson a **felhasználó attribútum hozzáadása** a **Felhasználó attribútum hozzáadása** párbeszédpanel megnyitásához. 
    
    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-optimizely-tutorial/tutorial_optimizely_05.png)

    b. Az **Attribútum neve** mezőbe írja be a attribútum neve "e".

    c billentyűkombinációt. Az **Attribútumérték** listából válassza ki a az érték "userprincipalname attribútum" vagy bármely érték, amely tartalmazza az e-mailben ismeri fel az Azure Active Directory és Optimizely.

    d. Kattintson a **kész**gombra.
3. A felső sávon kattintson a **Quick Start**.

    ![Egyszeri bejelentkezés beállítása][6]
4. A klasszikus portálon **Optimizely** alkalmazás integrációs lapon kattintson **konfigurálása az egyszeri bejelentkezés** a **Konfigurálása az egyszeri bejelentkezés** párbeszédpanel megnyitásához.

    ![Egyszeri bejelentkezés beállítása][7] 

5. **Hogyan szeretné, hogy jelentkezzen be az Optimizely felhasználók** lapon válassza az **Azure Active Directory Single Sign-On**, és kattintson a **Tovább gombra**.
    
    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-optimizely-tutorial/tutorial_optimizely_06.png)

6. Az **Alkalmazás beállításainak megadása** párbeszédpanel lapon hajtsa végre az alábbi lépéseket: 

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-optimizely-tutorial/tutorial_optimizely_07.png)


    egy. A **Bejelentkezési a URL-cím** mezőbe írja be:`https://app.optimizely.net/contoso`

    b. Az **azonosító** mezőbe írja be:`urn:auth0:optimizely:contoso`

    c billentyűkombinációt. Kattintson a **Tovább**gombra. 


    > [AZURE.NOTE] A **Bejelentkezési a URL-CÍMÉT** és **azonosító** értékei csak a tényleges értékek helyőrzőit. Talál utasításokat időigényes Optimizely tényleges értékeit később az oktatóprogram.

7. A **beállítás az egyszeri bejelentkezés Optimizely a** lapon hajtsa végre az alábbi lépéseket:

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-optimizely-tutorial/tutorial_optimizely_08.png)

    egy. Kattintson a **tanúsítvány letöltése**, és mentse a fájlt a számítógépen.

    b. **Egyszeri bejelentkezés szolgáltatás URL-cím**másolása.

8. Ha be van állítva az alkalmazás SSO, lépjen kapcsolatba a Optimizely fiókkezelő, és a következő adatokat:

    - A letöltött tanúsítvány 
    - Az egyszeri bejelentkezéses szolgáltatás URL-cím
 
    Az e-mailek válaszként Optimizely biztosít a bejelentkezési a URL-CÍMÉT (SP kezdeményezésére SSO), valamint az azonosító (Service Provider egységek) értéket.

9. Térjen vissza **Alkalmazás beállításainak megadása** párbeszédpanel lapra, és hajtsa végre az alábbi lépéseket:

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-optimizely-tutorial/tutorial_optimizely_07.png)

    egy. Az **Bejelentkezési a URL-cím** mezőbe írja be a Optimizely által biztosított **SP kezdeményezésére egyszeri bejelentkezési URL-CÍMÉT** .

    b. Az **azonosító** mezőbe írja be a Optimizely által biztosított **Service Provider entitás azonosítója** .

    c billentyűkombinációt. Kattintson a **Tovább**gombra.

10. A **beállítás az egyszeri bejelentkezés Optimizely a** lapon hajtsa végre az alábbi lépéseket:
    
    ![Azure Active Directory Single Sign-On][10]

    egy. Jelölje ki az egyszeri bejelentkezéses konfigurációs megerősítő.

    b. Kattintson a **Tovább**gombra.

11. Az **egyszeri bejelentkezés megerősítés** lapon kattintson a **Befejezés**gombra.  
    
    ![Azure Active Directory Single Sign-On][11]

12. Egy másik böngészőablakban bejelentkezéses az Optimizely alkalmazás.
13. Számla a nevére a jobb felső sarokban, majd a **Fiókbeállítások**gombra.

    ![Azure Active Directory Single Sign-On](./media/active-directory-saas-optimizely-tutorial/tutorial_optimizely_09.png)

14. A fiók lapon jelölje be **Engedélyezése egyszeri bejelentkezés** csoportban az egyszeri bejelentkezés **Áttekinté** szakaszában.

    ![Azure Active Directory Single Sign-On](./media/active-directory-saas-optimizely-tutorial/tutorial_optimizely_10.png)

### <a name="creating-an-azure-ad-test-user"></a>Azure AD-próba felhasználó létrehozása
Ebben a szakaszban a klasszikus portálon Britta Simon nevű próba felhasználó létrehozása.
A felhasználók listában jelölje ki a **Britta Simon**.

![Azure Active Directory-felhasználó létrehozása][20]

**Teszt felhasználó létrehozása az Azure Active Directory, hajtsa végre az alábbi lépéseket:**

1. Az **Azure klasszikus portálon**kattintson a bal oldali navigációs területen kattintson az **Active Directory**.
    
    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-optimizely-tutorial/create_aaduser_09.png) 

2. A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3. A menüben, a képernyő tetején, a felhasználók listájának megjelenítéséhez kattintson a **felhasználók**elemre.
    
    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-optimizely-tutorial/create_aaduser_03.png) 

4. **Felhasználó hozzáadása**elemre az alsó eszköztáron a **Felhasználó hozzáadása** párbeszédpanel megnyitásához.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-optimizely-tutorial/create_aaduser_04.png) 

5. A párbeszédpanel **közli velünk a felhasználóra vonatkozó** lapon hajtsa végre az alábbi lépéseket:
 
    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-optimizely-tutorial/create_aaduser_05.png) 

    egy. Felhasználó típusa jelölje be az új felhasználó a szervezet.

    b. A felhasználónév **szövegmezőbe**írja be a **BrittaSimon**.

    c billentyűkombinációt. Kattintson a **Tovább**gombra.

6.  A **Felhasználói profil** párbeszédpanel lapon hajtsa végre az alábbi lépéseket:

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-optimizely-tutorial/create_aaduser_06.png) 

    egy. Az **első neve** mezőbe írja be a **Britta**.  

    b. Az **Utolsó neve** mezőbe írja be, **Simon**.

    c billentyűkombinációt. A **Megjelenítendő név** mezőbe írja be a **Britta Simon**.

    d. A **szerepkör** listában jelölje ki a **felhasználót**.

    e. Kattintson a **Tovább**gombra.

7. A párbeszédpanel **ideiglenes jelszót kérjen** lapon kattintson a **Hozzon létre**.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-optimizely-tutorial/create_aaduser_07.png) 

8. A párbeszédpanel **ideiglenes jelszót első** lapon hajtsa végre az alábbi lépéseket:

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-optimizely-tutorial/create_aaduser_08.png) 

    egy. Jegyezze fel az **Új jelszót**az érték.

    b. Kattintson a **kész**gombra.   



### <a name="creating-an-optimizely-test-user"></a>Egy Optimizely teszt felhasználó létrehozása

Ebben a részben hozzon létre egy felhasználót Britta Simon nevű Optimizely.

1. A Kezdőlap lapon válassza a **közreműködők** lap
2. Kattintson az **Új Collaborator** egy új collaborator hozzáadása a projekthez.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-optimizely-tutorial/create_aaduser_10.png)

3.  Töltse ki az e-mail címet, majd a szerepkör hozzárendelése őket. Kattintson a **Meghívás**gombra.


    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-optimizely-tutorial/create_aaduser_11.png)

4. Egy e-mailben meghívást kapnak. Az e-mail cím használatával. Optimizely való bejelentkezéshez rendelkeznek.


### <a name="assigning-the-azure-ad-test-user"></a>Az Azure Active Directory-próba felhasználói hozzárendelése

Ebben a részben Britta Simon Azure egyszeri bejelentkezés a saját hozzáférést biztosít Optimizely használandó engedélyezése.

![Felhasználó hozzárendelése][200] 

**Britta Simon hozzárendelése Optimizely, hajtsa végre az alábbi lépéseket:**

1. A klasszikus portál kattintva nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson az **alkalmazások** parancsra a felső menüben.

    ![Felhasználó hozzárendelése][201] 

2. Az alkalmazások listájában jelölje ki a **Optimizely**.

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-optimizely-tutorial/tutorial_optimizely_50.png) 

1. A felső sávon kattintson a **felhasználók**elemre.

    ![Felhasználó hozzárendelése][203] 

1. Az összes felhasználó listában válassza a **Britta Simon**.

2. Az alsó eszköztáron kattintson a **hozzárendelni**.

    ![Felhasználó hozzárendelése][205]


### <a name="testing-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

Ez a szakasz célja a Azure Active Directory egyszeri bejelentkezéses beállítások tesztelése a Access panelen.

Amikor az Access panel a Optimizely mozaik gombra kattint, meg kell első automatikusan aláírt-on az Optimizely alkalmazás.

## <a name="additional-resources"></a>További források

* [A szoftver alkalmazások integrálása az Azure Active Directory oktatóanyagok listája](active-directory-saas-tutorial-list.md)
* [Mi az access alkalmazás és az egyszeri bejelentkezés az Azure Active Directory címtárral?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_04.png


[5]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_05.png
[6]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_06.png
[7]:  ./media/active-directory-saas-optimizely-tutorial/tutorial_general_050.png
[10]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_060.png
[11]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_070.png
[20]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_205.png
