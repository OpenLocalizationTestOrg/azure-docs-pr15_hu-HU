<properties
    pageTitle="Oktatóprogram: Azure Active Directory-integráció a 23 videó |} Microsoft Azure"
    description="Megtudhatja, hogy miként konfigurálható az egyszeri bejelentkezés Azure Active Directory és a videó 23 közötti."
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
    ms.date="10/24/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-23-video"></a>Oktatóprogram: Azure Active Directory-integráció a 23-videó

Ebben az oktatóanyagban célja mutatja, hogy miként 23 videó integrálása az Azure Active Directory (Azure Active Directory).  
23 integrálása az Azure Active Directory videó biztosít a következő előnyökkel jár: 

- Az Azure Active Directory, 23 videó hozzáféréssel rendelkező személyek megadhatja, hogy 
- Engedélyezheti a felhasználóknak, hogy automatikusan első jelentkezett-on az Azure Active Directory-fiókok (Single Sign-On) 23 videó

Ha többet szeretne tudni a szoftver alkalmazás integrálása az Azure AD meg, hogy, ismerje meg, [az access alkalmazás és az egyszeri bejelentkezés az Azure Active Directory címtárral](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek 

Azure Active Directory integráció 23 videofájllal van szükség az alábbiakat:

- Az Azure Active Directory-előfizetéssel
- A 23 videó egyszeri bejelentkezés engedélyezett előfizetés


> [AZURE.NOTE] Az oktatóprogram lépéseit teszteléséhez nem használata ajánlott munkakörnyezetben.


Az oktatóprogram lépéseit teszteléséhez célszerű követnie az alábbi javaslatokat:

- Erre akkor szükség, ne használja a termelési környezetén.
- Ha nincs telepítve az Azure Active Directory próba környezetben, kattint egy hónap próba [Itt](https://azure.microsoft.com/pricing/free-trial/). 

 
## <a name="scenario-description"></a>Forgatókönyv leírás
Ebben az oktatóanyagban célja lehetővé teszi az Azure Active Directory egyszeri bejelentkezés tesztkörnyezetben tesztelése.  
Az alkalmazási példát, ebben az oktatóanyagban tagolt két fő építőelemek áll:

1. A gyűjteményből 23 videók hozzáadása 
2. Beállítása és tesztelése Azure Active Directory egyszeri bejelentkezés


## <a name="adding-23-video-from-the-gallery"></a>A gyűjteményből 23 videók hozzáadása
Konfigurálja az Azure Active Directory integrálása a 23 videó, szüksége 23 videoképet adhat az felügyelt szoftver alkalmazáslistában a gyűjteményből.

**A gyűjteményből 23 a videó felvételéhez hajtsa végre az alábbi lépéseket:**

1. Az **Azure klasszikus portálon**kattintson a bal oldali navigációs területen kattintson az **Active Directory**. 

    ![Az Active Directory][1]

2. A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3. Nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson a felső menüben **alkalmazások** .

    ![Alkalmazások][2]

4. Kattintson a **Hozzáadás** az oldal alján.

    ![Alkalmazások][3]

5. **Milyen feladatot szeretne tenni** párbeszédpanelen kattintson **a gyűjteményből az alkalmazások felvétele**.

    ![Alkalmazások][4]

6. A Keresés mezőbe írja be **a videó 23**.

    ![Alkalmazások][5]

7. Az eredmény ablaktáblában válassza a **Videó 23**, és válassza a **kész** , az alkalmazás hozzáadása gombra.

    ![Alkalmazások][25]

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Beállítása és tesztelése Azure Active Directory egyszeri bejelentkezés
Ez a szakasz célja mutatja, hogy beállítása és Azure Active Directory egyszeri bejelentkezés a "Britta Simon" nevű próba felhasználón alapuló 23 videofájllal tesztelése.

Az egyszeri bejelentkezési a munkát az Azure Active Directory kell tudható, hogy mi az Azure Active Directory-felhasználónak 23 videóban megfelelőjük a felhasználó. Más szóval Azure AD-felhasználó, és a kapcsolódó felhasználó 23 közötti kapcsolat kapcsolat videó kell létrehozni.  
Ez a hivatkozás kapcsolat által a **felhasználónév** értéket rendeli az Azure Active Directory, a program a **felhasználónév** , a 23 videó jön létre.
 
Állítsa be, és Azure Active Directory egyszeri bejelentkezés a 23 videofájllal tesztelése, az alábbi építőelemeket szükséges:

1. **[Azure Active Directory konfigurálása az egyszeri bejelentkezés](#configuring-azure-ad-single-single-sign-on)** - engedélyezése a felhasználóknak, hogy ezzel a szolgáltatással.
2. **[Felhasználó létrehozása az Azure Active Directory tesztelése](#creating-an-azure-ad-test-user)** - Azure Active Directory egyszeri bejelentkezéssel való Britta Simon ellenőrzéséhez.
4. **[Tesztelése 23 videó létrehozása a felhasználó](#creating-a-23-video-test-user)** -, hogy egy partner a Britta Simon 23 videóban, amelyen az Azure Active Directory ábrázolt csatolt.
5. **[Az Azure Active Directory hozzárendelése tesztelje a felhasználó](#assigning-the-azure-ad-test-user)** - ahhoz, hogy Britta Simon Azure AD az egyszeri bejelentkezés használata.
5. **[Tesztelés egyszeri bejelentkezés](#testing-single-sign-on)** - ellenőrzéséhez, hogy működik-e a konfigurációs.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure Active Directory Single Sign-On konfigurálása

Ez a szakasz célja Azure AD az egyszeri bejelentkezés az Azure klasszikus portálon engedélyezése és a 23 videó alkalmazásban az egyszeri bejelentkezés beállítása.

**Állítson be Azure AD az egyszeri bejelentkezés 23 videó, hajtsa végre az alábbi lépéseket:**

1. Az Azure klasszikus portálon **23 Video** alkalmazás integrációs lapon kattintson **konfigurálása az egyszeri bejelentkezés** a **Konfigurálása az egyszeri bejelentkezés** párbeszédpanel megnyitásához.

    ![Egyszeri bejelentkezés beállítása][6] 

2. **Hogyan szeretne bejelentkezni, 23 videó alkalmazásba című témakör tartalmaz a felhasználók** lapon válassza az **Azure Active Directory Single Sign-On**, és kattintson a **Tovább gombra**.

    ![Azure Active Directory Single Sign-On][7] 

3. Az **Alkalmazás beállításainak megadása** párbeszédpanel lapon hajtsa végre az alábbi lépéseket:

    ![Azure Active Directory Single Sign-On][8] 
 
     egy. **Válasz URL-cím** mezőbe írja be a bejelentkezés a 23 videó webhelyre a felhasználók által használt URL-CÍMÉT (például: *https://britta-simon.23Video.com/saml/login*).

     > [AZURE.NOTE] Active Directory-integráció a SAML 2.0 használata az összes 23 videó felhasználó számára érhető el. Kérjük, forduljon a támogatási szolgálathoz [support@23company.com](mailto:support@23company.com) Ha szüksége van a kapcsolódó metaadatok.

     b. Kattintson a **Tovább**gombra.
 
4. A **konfigurálása az egyszeri bejelentkezés a 23 videó** lapon hajtsa végre az alábbi lépéseket:

    ![Azure Active Directory Single Sign-On][9] 

    egy. Kattintson a letöltés tanúsítvány, és mentse a fájlt a számítógépen.

    b. A 23 videó támogatási csoport munkatársaitól keresztül [support@23company.com](mailto:support@23company.com), küldje el nekik a letöltött tanúsítvány, a **Kibocsátó URL-címet**, az **Egyszeri bejelentkezéses szolgáltatás URL-CÍMÉT**, **Egyetlen Sign-Out URL-CÍMÉT**, és ezután kérje meg, hogy egyszeri bejelentkezés beállítása a 23 Video alkalmazás. 

    c billentyűkombinációt. Kattintson a **Tovább**gombra.


6. Az Azure klasszikus portálon jelölje be a egyszeri bejelentkezéses konfigurációs megerősítés, és kattintson a **Tovább gombra**. 

    ![Azure Active Directory Single Sign-On][10]

7. Az **egyszeri bejelentkezés megerősítés** lapon kattintson a **Befejezés**gombra.  
  
    ![Azure Active Directory Single Sign-On][11]




### <a name="creating-an-azure-ad-test-user"></a>Azure AD-próba felhasználó létrehozása
Ez a szakasz célja próba felhasználó létrehozása az Azure-Britta Simon nevű klasszikus portálon.

![Azure Active Directory-felhasználó létrehozása][20]

**Teszt felhasználó létrehozása az Azure Active Directory, hajtsa végre az alábbi lépéseket:**

1. Az **Azure klasszikus portálon**kattintson a bal oldali navigációs területen kattintson az **Active Directory**.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-23video-tutorial/create_aaduser_09.png)  

2. A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3. A menüben, a képernyő tetején, a felhasználók listájának megjelenítéséhez kattintson a **felhasználók**elemre.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-23video-tutorial/create_aaduser_03.png) 
 
4. **Felhasználó hozzáadása**elemre az alsó eszköztáron a **Felhasználó hozzáadása** párbeszédpanel megnyitásához. 

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-23video-tutorial/create_aaduser_04.png) 

5. A párbeszédpanel **közli velünk a felhasználóra vonatkozó** lapon hajtsa végre az alábbi lépéseket: 

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-23video-tutorial/create_aaduser_05.png)  

    egy. Felhasználó típusa jelölje be az új felhasználó a szervezet.

    b. A felhasználónév **szövegmezőbe**írja be a **BrittaSimon**.

    c billentyűkombinációt. Kattintson a **Tovább**gombra.

6.  A **Felhasználói profil** párbeszédpanel lapon hajtsa végre az alábbi lépéseket: 

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-23video-tutorial/create_aaduser_06.png) 
 
    egy. Az **első neve** mezőbe írja be a **Britta**.  

    b. Az **Utolsó neve** mezőbe írja be, **Simon**.

    c billentyűkombinációt. A **Megjelenítendő név** mezőbe írja be a **Britta Simon**.

    d. A **szerepkör** listában jelölje ki a **felhasználót**.
    e. Kattintson a **Tovább**gombra.

7. A párbeszédpanel **ideiglenes jelszót első** lapon kattintson a **Hozzon létre**.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-23video-tutorial/create_aaduser_07.png) 
 
8. A párbeszédpanel **ideiglenes jelszót első** lapon hajtsa végre az alábbi lépéseket:

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-23video-tutorial/create_aaduser_08.png) 
  
    egy. Jegyezze fel az **Új jelszót**az érték.

    b. Kattintson a **kész**gombra.   

  
 
### <a name="creating-a-23-video-test-user"></a>Videó 23 próba felhasználó létrehozása

Ez a szakasz célja Britta Simon című videóban 23 felhasználó létrehozása.

**Egy felhasználó Britta Simon nevezik 23 a videó létrehozásához tegye a következőket:**

1. Jelentkezzen be rendszergazdaként a 23 videó vállalati webhely.

1. Nyissa meg a **beállításokat**.


2. **Felhasználók** csoportban kattintson a **Konfigurálás**gombra.

    ![Felhasználó hozzárendelése][400]

3. Kattintson a **Hozzáadás új felhasználót**. 

    ![Felhasználó hozzárendelése][401]

4. A **Meghívó küldése Bekapcsolódás a webhely** csoportban hajtsa végre az alábbi lépéseket:

    ![Felhasználó hozzárendelése][402]

    egy. Az **E-mail cím** mezőbe írja be az Azure Active Directory Britta Simon e-mail címét.

    b. Kattintson **a felhasználó hozzáadása**elemre.   


### <a name="assigning-the-azure-ad-test-user"></a>Az Azure Active Directory-próba felhasználói hozzárendelése

Ez a szakasz célja Britta Simon Azure egyszeri bejelentkezés a saját hozzáférést biztosít 23 videó használandó engedélyezése.

![Felhasználó hozzárendelése][200] 

**Britta Simon hozzárendelése 23 videó, hajtsa végre az alábbi lépéseket:**

1. Az Azure klasszikus portál kattintva nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson az **alkalmazások** parancsra a felső menüben.

    ![Felhasználó hozzárendelése][201] 

2. Az alkalmazások listájában válassza a **23 videó**.

    ![Felhasználó hozzárendelése][202] 

1. A felső sávon kattintson a **felhasználók**elemre.

    ![Felhasználó hozzárendelése][203] 

1. A felhasználók listában jelölje ki a **Britta Simon**.

2. Az alsó eszköztáron kattintson a **hozzárendelni**.

    ![Felhasználó hozzárendelése][205]



### <a name="testing-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

Ez a szakasz célja a Azure Active Directory egyszeri bejelentkezéses beállítások tesztelése a Access panelen.  
A 23 videó csempére az Access panel gombra kattintva meg kell első automatikusan aláírt-on a 23 videó alkalmazásba.


## <a name="additional-resources"></a>További források

* [A szoftver alkalmazások integrálása az Azure Active Directory oktatóanyagok listája](active-directory-saas-tutorial-list.md)
* [Mi az access alkalmazás és az egyszeri bejelentkezés az Azure Active Directory címtárral?](active-directory-appssoaccess-whatis.md)


<!--Image references-->
[1]: ./media/active-directory-saas-23video-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-23video-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-23video-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-23video-tutorial/tutorial_general_04.png
[5]: ./media/active-directory-saas-23video-tutorial/tutorial_23video_01.png
[25]: ./media/active-directory-saas-23video-tutorial/tutorial_23video_02.png

[6]: ./media/active-directory-saas-23video-tutorial/tutorial_general_05.png
[7]: ./media/active-directory-saas-23video-tutorial/tutorial_23video_03.png
[8]: ./media/active-directory-saas-23video-tutorial/tutorial_23video_04.png
[9]: ./media/active-directory-saas-23video-tutorial/tutorial_23video_05.png
[10]: ./media/active-directory-saas-23video-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-23video-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-23video-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-23video-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-23video-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-23video-tutorial/tutorial_23video_07.png
[203]: ./media/active-directory-saas-23video-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-23video-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-23video-tutorial/tutorial_general_205.png

[400]: ./media/active-directory-saas-23video-tutorial/tutorial_23video_10.png
[401]: ./media/active-directory-saas-23video-tutorial/tutorial_23video_11.png
[402]: ./media/active-directory-saas-23video-tutorial/tutorial_23video_12.png




