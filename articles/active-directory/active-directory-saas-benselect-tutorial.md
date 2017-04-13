<properties
    pageTitle="Oktatóprogram: Azure Active Directory-integráció a BenSelect |} Microsoft Azure"
    description="Megtudhatja, hogy miként konfigurálása az egyszeri bejelentkezés Azure Active Directory és BenSelect között."
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
    ms.date="10/07/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-benselect"></a>Oktatóprogram: Azure Active Directory-integráció a BenSelect

Ebből az oktatóanyagból megismerheti, hogyan BenSelect integrálása az Azure Active Directory (Azure Active Directory).

BenSelect integrálása az Azure Active Directory biztosít a következő előnyökkel jár:

- Az Azure Active Directory, aki rendelkezik hozzáféréssel BenSelect megadhatja, hogy
- Engedélyezheti a felhasználóknak, hogy automatikusan első jelentkezett-on való BenSelect (Single Sign-On) az Azure Active Directory-fiókok
- A fiókokat az egy központi helyen – az Azure klasszikus portálra

Ha többet szeretne tudni a szoftver alkalmazás integrálása az Azure AD meg, hogy, ismerje meg, [az access alkalmazás és az egyszeri bejelentkezés az Azure Active Directory címtárral](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek

Azure Active Directory integráció a BenSelect van szükség az alábbiakat:

- Az Azure Active Directory-előfizetéssel
- Egy BenSelect egyszeri bejelentkezés engedélyezett előfizetés


> [AZURE.NOTE] Az oktatóprogram lépéseit teszteléséhez nem használata ajánlott munkakörnyezetben.


Az oktatóprogram lépéseit teszteléséhez célszerű követnie az alábbi javaslatokat:

- Erre akkor szükség, ne használja a termelési környezetén.
- Ha nincs telepítve az Azure Active Directory próba környezetben, kattint egy hónap próba [Itt](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban tesztelni Azure AD az egyszeri bejelentkezés tesztkörnyezetben.

Az alkalmazási példát, ebben az oktatóanyagban tagolt két fő építőelemek áll:

1. BenSelect hozzáadása a gyűjteményből
2. Beállítása és tesztelése Azure Active Directory egyszeri bejelentkezés


## <a name="adding-benselect-from-the-gallery"></a>BenSelect hozzáadása a gyűjteményből
Konfigurálja az Azure Active Directory integrálása a BenSelect, meg kell BenSelect hozzáadása az felügyelt szoftver alkalmazáslistában a gyűjteményből.

**A gyűjteményből BenSelect felvételéhez hajtsa végre az alábbi lépéseket:**

1. Az **Azure klasszikus portálon**kattintson a bal oldali navigációs területen kattintson az **Active Directory**.

    ![Az Active Directory][1]
2. A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3. Nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson a felső menüben **alkalmazásokat** .

    ![Alkalmazások][2]

4. Kattintson a **Hozzáadás** az oldal alján.

    ![Alkalmazások][3]

5. **Milyen feladatot szeretne tenni** párbeszédpanelen kattintson **a gyűjteményből az alkalmazások felvétele**.

    ![Alkalmazások][4]

6. A Keresés mezőbe írja be a **BenSelect**.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-benselect-tutorial/tutorial_benselect_01.png)

7. Az eredmény ablaktáblában jelölje ki a **BenSelect**, és válassza a **kész** , az alkalmazás hozzáadása gombra.

    ![Az alkalmazás kijelöli a gyűjteményben](./media/active-directory-saas-benselect-tutorial/tutorial_benselect_001.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Beállítása és tesztelése Azure Active Directory egyszeri bejelentkezés
Ebben a részben konfigurálása és a "Britta Simon" nevű próba felhasználón alapuló BenSelect az Azure Active Directory egyszeri bejelentkezés tesztelése.

Az egyszeri bejelentkezési a munkát az Azure Active Directory kell tudható, hogy mi a BenSelect megfelelőjük a felhasználó egy felhasználóhoz, az Azure Active Directory. Más szóval Azure AD-felhasználó, és a kapcsolódó felhasználó BenSelect hivatkozás viszonya kell létrehozni.

A hivatkozás kapcsolat jön létre által az Azure Active Directory BenSelect **felhasználónév** értékként a **felhasználó neve** értéket rendeli.

Állítsa be, és Azure Active Directory egyszeri bejelentkezéssel való BenSelect tesztelése, a következő építőelemek szükséges:

1. **[Azure Active Directory konfigurálása az egyszeri bejelentkezés](#configuring-azure-ad-single-sign-on)** - engedélyezése a felhasználóknak, hogy ezzel a szolgáltatással.
2. **[Felhasználó létrehozása az Azure Active Directory tesztelése](#creating-an-azure-ad-test-user)** - Azure Active Directory egyszeri bejelentkezéssel való Britta Simon ellenőrzéséhez.
3. **[Felhasználó létrehozása egy BenSelect tesztelése](#creating-a-benselect-test-user)** - van-e egy megfelelője a Britta Simon BenSelect, amelyen az Azure Active Directory ábrázolt csatolt.
4. **[Az Azure Active Directory hozzárendelése tesztelje a felhasználó](#assigning-the-azure-ad-test-user)** - ahhoz, hogy Britta Simon Azure AD az egyszeri bejelentkezés használata.
5. **[Tesztelés egyszeri bejelentkezés](#testing-single-sign-on)** - ellenőrzéséhez, hogy működik-e a konfigurációs.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure Active Directory Single Sign-On konfigurálása

Ez a szakasz célja Azure AD az egyszeri bejelentkezés az Azure klasszikus portálon engedélyezése és a BenSelect alkalmazásban az egyszeri bejelentkezés beállítása.

BenSelect alkalmazás a SAML Előfeltételek vár, egy bizonyos formátumban. Állítsa be a következő jogcímalapú ehhez az alkalmazáshoz. A "**Atrribute**" lapon, az alkalmazás a következő attribútumok értékének kezelheti. Az alábbi képernyőképen látható, a. 

![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-benselect-tutorial/tutorial_benselect_06.png)

**Állítson be Azure AD az egyszeri bejelentkezés BenSelect, hajtsa végre az alábbi lépéseket:**

1. Az Azure klasszikus portálon **BenSelect** alkalmazás integrációs lapján a menüben, a képernyő tetején kattintson a **attribútumok**elemre.

     ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-benselect-tutorial/tutorial_benselect_07.png)

2. A **SAML jogkivonat attribútumok** párbeszédpanelen minden egyes sorára, az alábbi táblázatban látható hajtsa végre az alábbi lépéseket:

  	| Attribútumnév | Attribútumérték |
  	| --- | --- |    
  	| http://schemas.xmlsoap.org/ws/2005/05/Identity/Claims/NameIdentifier    | extractmailprefix([userPrincipalName]) |

    egy. Kattintson a **felhasználó attribútum hozzáadása** a **Felhasználó Attribure hozzáadása** párbeszédpanel megnyitásához.

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-benselect-tutorial/tutorial_benselect_08.png)

    b. Az **Attribútum neve** mezőbe írja be a attribútumnév meg abban a sorban látható.

    c billentyűkombinációt. Az **Attribútumérték** listából írja be a ExtractMailPrefix().

    d. A **levelek** listából írja be a User.userprincipalname.
    
    e. Kattintson a **kész** gombra

3. A felső sávon kattintson a **Quick Start**.

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-benselect-tutorial/tutorial_benselect_09.png)

4. **Hogyan szeretné, hogy jelentkezzen be az BenSelect felhasználók** lapon válassza az **Azure Active Directory Single Sign-On**, és kattintson a **Tovább gombra**.

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-benselect-tutorial/tutorial_benselect_03.png) 

5. Az **Alkalmazás beállításainak megadása** párbeszédpanel lapon hajtsa végre az alábbi lépéseket:

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-benselect-tutorial/tutorial_benselect_04.png) 

    egy. **Válasz URL-cím** mezőbe írja be az URL-címet, az alábbi minta használatával:`https://www.benselect.com/enroll/login.aspx?Path={<tenant name>}`
    
    b. Kattintson a **Tovább** gombra
 
6. A **beállítás az egyszeri bejelentkezés BenSelect a** lapon hajtsa végre az alábbi lépéseket:

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-benselect-tutorial/tutorial_benselect_05.png)

    egy. Kattintson a **tanúsítvány letöltése**, és mentse a fájlt a számítógépen.

    b. Kattintson a **Tovább**gombra.


7. Ha be van állítva az alkalmazás SSO, a BenSelect támogatási csoport munkatársaitól keresztül [support@selerix.com](mailto:support@selerix.com) , és küldje el nekik a következő:

    - A letöltött tanúsítvány
    - A SAML egyszeri bejelentkezési URL-címe
    - A Kijelentkezés URL-címe 
    - A kibocsátó 

    > [AZURE.NOTE] Kell utalnia az, hogy a integrálás a SHA256 algoritmus (SHA1 nem támogatott) kell lennie az egyszeri bejelentkezés beállítása a megfelelő kiszolgálón például app2101 stb.

8. A klasszikus portálon jelölje be a egyszeri bejelentkezéses konfigurációs megerősítés, és kattintson a **Tovább gombra**.
    
    ![Azure Active Directory Single Sign-On][10]

9. Az **egyszeri bejelentkezés megerősítés** lapon kattintson a **Befejezés**gombra.  
 
    ![Azure Active Directory Single Sign-On][11]


### <a name="creating-an-azure-ad-test-user"></a>Azure AD-próba felhasználó létrehozása
Ebben a szakaszban a klasszikus portálon Britta Simon nevű próba felhasználó létrehozása.


![Azure Active Directory-felhasználó létrehozása][20]

**Teszt felhasználó létrehozása az Azure Active Directory, hajtsa végre az alábbi lépéseket:**

1. Az **Azure klasszikus portálon**kattintson a bal oldali navigációs területen kattintson az **Active Directory**.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-benselect-tutorial/create_aaduser_09.png) 

2. A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3. A menüben, a képernyő tetején, a felhasználók listájának megjelenítéséhez kattintson a **felhasználók**elemre.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-benselect-tutorial/create_aaduser_03.png) 

4. **Felhasználó hozzáadása**elemre az alsó eszköztáron a **Felhasználó hozzáadása** párbeszédpanel megnyitásához.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-benselect-tutorial/create_aaduser_04.png) 

5. A **mondja el a felhasználóra vonatkozó** párbeszédpanel lapon hajtsa végre az alábbi lépéseket:  ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-benselect-tutorial/create_aaduser_05.png) 

    egy. Felhasználó típusa jelölje be az új felhasználó a szervezet.

    b. A felhasználónév **szövegmezőbe**írja be a **BrittaSimon**.

    c billentyűkombinációt. Kattintson a **Tovább**gombra.

6.  A **Felhasználói profil** párbeszédpanel lapon hajtsa végre az alábbi lépéseket: ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-benselect-tutorial/create_aaduser_06.png) 

    egy. Az **első neve** mezőbe írja be a **Britta**.  

    b. Az **Utolsó neve** mezőbe írja be, **Simon**.

    c billentyűkombinációt. A **Megjelenítendő név** mezőbe írja be a **Britta Simon**.

    d. A **szerepkör** listában jelölje ki a **felhasználót**.

    e. Kattintson a **Tovább**gombra.

7. A párbeszédpanel **ideiglenes jelszót kérjen** lapon kattintson a **Hozzon létre**.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-benselect-tutorial/create_aaduser_07.png) 

8. A párbeszédpanel **ideiglenes jelszót első** lapon hajtsa végre az alábbi lépéseket:

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-benselect-tutorial/create_aaduser_08.png) 

    egy. Jegyezze fel az **Új jelszót**az érték.

    b. Kattintson a **kész**gombra.   



### <a name="creating-an-benselect-test-user"></a>Egy BenSelect teszt felhasználó létrehozása

Ez a szakasz célja Britta Simon nevű BenSelect felhasználó létrehozása. Kérje meg a felhasználók felvétele az BenSelect fiók BenSelect támogatási csapattal.

> [AZURE.NOTE] Ha egy felhasználó létrehozása manuálisan kell szeretne-e a BenSelect támogatási csoport munkatársaitól kaphat keresztül <mailto:support@selerix.com>.


### <a name="assigning-the-azure-ad-test-user"></a>Az Azure Active Directory-próba felhasználói hozzárendelése

Ebben a részben Britta Simon Azure egyszeri bejelentkezés a saját hozzáférést biztosít BenSelect használandó engedélyezése.

![Felhasználó hozzárendelése][200] 

**Britta Simon hozzárendelése BenSelect, hajtsa végre az alábbi lépéseket:**

1. A klasszikus portál kattintva nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson az **alkalmazások** parancsra a felső menüben.

    ![Felhasználó hozzárendelése][201] 

2. Az alkalmazások listájában jelölje ki a **BenSelect**.

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-benselect-tutorial/tutorial_benselect_50.png) 

3. A felső sávon kattintson a **felhasználók**elemre.

    ![Felhasználó hozzárendelése][203]

4. A felhasználók listában jelölje ki a **Britta Simon**.

5. Az alsó eszköztáron kattintson a **hozzárendelni**.

    ![Felhasználó hozzárendelése][205]


### <a name="testing-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

Ebben a részben tesztelje az Azure Active Directory egyetlen bejelentkezés beállítási a Access panelen.

Ha a BenSelect csempére az Access panel gombra kattint, meg kell kérjen automatikusan aláírt-on az BenSelect alkalmazás.


## <a name="additional-resources"></a>További források

* [A szoftver alkalmazások integrálása az Azure Active Directory oktatóanyagok listája](active-directory-saas-tutorial-list.md)
* [Mi az access alkalmazás és az egyszeri bejelentkezés az Azure Active Directory címtárral?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-benselect-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-benselect-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-benselect-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-benselect-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-benselect-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-benselect-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-benselect-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-benselect-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-benselect-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-benselect-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-benselect-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-benselect-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-benselect-tutorial/tutorial_general_205.png
