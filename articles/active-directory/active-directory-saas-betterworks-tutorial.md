<properties
    pageTitle="Oktatóprogram: Azure Active Directory-integráció a BetterWorks |} Microsoft Azure"
    description="Megtudhatja, hogy miként konfigurálása az egyszeri bejelentkezés Azure Active Directory és BetterWorks között."
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
    ms.date="09/29/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-betterworks"></a>Oktatóprogram: Azure Active Directory-integráció a BetterWorks

Ebben az oktatóanyagban célja mutatja, hogy miként BetterWorks integrálása az Azure Active Directory (Azure Active Directory).

BetterWorks integrálása az Azure Active Directory biztosít a következő előnyökkel jár:

- Az Azure Active Directory, aki rendelkezik hozzáféréssel BetterWorks megadhatja, hogy
- Engedélyezheti a felhasználóknak, hogy automatikusan első jelentkezett-on való BetterWorks (Single Sign-On) az Azure Active Directory-fiókok
- A fiókokat az egy központi helyen – az Azure klasszikus portálra

Ha többet szeretne tudni a szoftver alkalmazás integrálása az Azure AD meg, hogy, ismerje meg, [az access alkalmazás és az egyszeri bejelentkezés az Azure Active Directory címtárral](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek

Azure Active Directory integráció a BetterWorks van szükség az alábbiakat:

- Az Azure Active Directory-előfizetéssel
- Egy BetterWorks egyszeri bejelentkezés engedélyezett előfizetés


> [AZURE.NOTE] Az oktatóprogram lépéseit teszteléséhez nem használata ajánlott munkakörnyezetben.


Az oktatóprogram lépéseit teszteléséhez célszerű követnie az alábbi javaslatokat:

- Erre akkor szükség, ne használja a termelési környezetén.
- Ha nincs telepítve az Azure Active Directory próba környezetben, kattint egy hónap próba [Itt](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban célja lehetővé teszi az Azure Active Directory egyszeri bejelentkezés tesztkörnyezetben tesztelése.

Az alkalmazási példát, ebben az oktatóanyagban tagolt két fő építőelemek áll:

1. BetterWorks hozzáadása a gyűjteményből
2. Beállítása és tesztelése Azure Active Directory egyszeri bejelentkezés


## <a name="adding-betterworks-from-the-gallery"></a>BetterWorks hozzáadása a gyűjteményből
Konfigurálja az Azure Active Directory integrálása a BetterWorks, meg kell BetterWorks hozzáadása az felügyelt szoftver alkalmazáslistában a gyűjteményből.

**A gyűjteményből BetterWorks felvételéhez hajtsa végre az alábbi lépéseket:**

1. Az **Azure klasszikus portálon**kattintson a bal oldali navigációs területen kattintson az **Active Directory**. 

    ![Az Active Directory][1]

2. A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3. Nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson a felső menüben **alkalmazások** .
    
    ![Alkalmazások][2]

4. Kattintson a **Hozzáadás** az oldal alján.
    
    ![Alkalmazások][3]

5. **Milyen feladatot szeretne tenni** párbeszédpanelen kattintson **a gyűjteményből az alkalmazások felvétele**.

    ![Alkalmazások][4]

6. A Keresés mezőbe írja be a **BetterWorks**.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-betterworks-tutorial/tutorial_betterworks_01.png)

7. Az eredmény ablaktáblában jelölje ki a **BetterWorks**, és válassza a **kész** , az alkalmazás hozzáadása gombra.

    ![Az alkalmazás kijelöli a gyűjteményben](./media/active-directory-saas-betterworks-tutorial/tutorial_betterworks_001.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Beállítása és tesztelése Azure Active Directory egyszeri bejelentkezés
Ez a szakasz célja mutatja, hogy állítsa be, és a "Britta Simon" nevű vizsgálat felhasználón alapuló BetterWorks Azure AD az egyszeri bejelentkezés tesztelése.

Az egyszeri bejelentkezési a munkát az Azure Active Directory kell, hogy mi az a partner felhasználó BetterWorks az Azure Active Directory-felhasználónak. Más szóval Azure AD-felhasználó, és a kapcsolódó felhasználó BetterWorks hivatkozás viszonya kell létrehozni.

A hivatkozás kapcsolat jön létre által az Azure Active Directory BetterWorks **felhasználónév** értékként a **felhasználó neve** értéket rendeli.

Állítsa be, és Azure Active Directory egyszeri bejelentkezéssel való BetterWorks tesztelése, az alábbi építőelemeket szükséges:

1. **[Azure Active Directory konfigurálása az egyszeri bejelentkezés](#configuring-azure-ad-single-single-sign-on)** - engedélyezése a felhasználóknak, hogy ezzel a szolgáltatással.
2. **[Felhasználó létrehozása az Azure Active Directory tesztelése](#creating-an-azure-ad-test-user)** - Azure Active Directory egyszeri bejelentkezéssel való Britta Simon ellenőrzéséhez.
3. **[Felhasználó létrehozása egy BetterWorks tesztelése](#creating-a-betterworks-test-user)** - van-e egy megfelelője a Britta Simon BetterWorks, amelyen az Azure Active Directory ábrázolt csatolt.
4. **[Az Azure Active Directory hozzárendelése tesztelje a felhasználó](#assigning-the-azure-ad-test-user)** - ahhoz, hogy Britta Simon Azure AD az egyszeri bejelentkezés használata.
5. **[Tesztelés egyszeri bejelentkezés](#testing-single-sign-on)** - ellenőrzéséhez, hogy működik-e a konfigurációs.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure Active Directory Single Sign-On konfigurálása

Ez a szakasz célja Azure AD az egyszeri bejelentkezés az Azure klasszikus portálon engedélyezése és a BetterWorks alkalmazásban az egyszeri bejelentkezés beállítása.

BetterWorks alkalmazás a SAML Előfeltételek vár, egy bizonyos formátumban. Állítsa be a következő jogcímalapú ehhez az alkalmazáshoz. A "**Atrribute**" lapon, az alkalmazás a következő attribútumok értékének kezelheti. Az alábbi képernyőképen látható, a. 

![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-betterworks-tutorial/tutorial_betterworks_06.png)

**Állítson be Azure AD az egyszeri bejelentkezés BetterWorks, hajtsa végre az alábbi lépéseket:**

1. Az Azure klasszikus portálon **BetterWorks** alkalmazás integrációs lapján a menüben, a képernyő tetején kattintson a **attribútumok**elemre.

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-betterworks-tutorial/tutorial_betterworks_07.png)

2. A **SAML jogkivonat attribútumok** párbeszédpanelen minden egyes sorára, az alábbi táblázatban látható hajtsa végre az alábbi lépéseket:
    

  	| Attribútumnév | Attribútumérték |
  	| --- | --- |    
  	| saml_token | bd189cf6-1701-11e6-8f90-d26992eca2a5 |

    egy. Kattintson a **felhasználó attribútum hozzáadása** a **Felhasználó Attribure hozzáadása** párbeszédpanel megnyitásához.

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-betterworks-tutorial/tutorial_betterworks_12.png)
    
    b. Az **Attribútum neve** mezőbe írja be a attribútumnév meg abban a sorban látható.
    
    c billentyűkombinációt. Az **Attribútumérték** listából írja be a token saml-Azonosítóját, meg abban a sorban látható.
    
    d. Kattintson a **kész** gombra

3. A felső sávon kattintson a **Quick Start**.

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-betterworks-tutorial/tutorial_betterworks_11.png) 

4. **Hogyan szeretné, hogy jelentkezzen be az BetterWorks felhasználók** lapon válassza az **Azure Active Directory Single Sign-On**, és kattintson a **Tovább gombra**.
    
    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-betterworks-tutorial/tutorial_betterworks_03.png)

5. Az **Alkalmazás beállításainak megadása** párbeszédpanel lapon szeretne az alkalmazás beállítása **IDP kezdeményezett mód**, ha hajtsa végre az alábbi lépéseket:

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-betterworks-tutorial/tutorial_betterworks_04.png)


    egy. Az **azonosító** mezőbe írja be az URL-t a következő mintát:`https://app.betterworks.com/saml2/metadata/`


    b. **Válasz URL-cím** mezőbe írja be az URL-t a következő mintát:`https://app.betterworks.com/saml2/acs/`


    c billentyűkombinációt. Kattintson a **Tovább** gombra

6. Az **Alkalmazás beállításainak megadása** párbeszédpanel lapon Ha szeretne az alkalmazás beállítása **SP kezdeményezett mód**, hajtsa végre a be az alábbi lépéseket:

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-betterworks-tutorial/tutorial_betterworks_10.png)

    egy.  Válassza a **Megjelenítés a Speciális beállítások (nem kötelező)**.



    b. Az **Bejelentkezési a URL-cím** mezőbe írja be a bejelentkezés az BetterWorks alkalmazás a következő mintát használatával a felhasználók által használt URL-címe:`https://app.betterworks.com`

    b. Kattintson a **Tovább**gombra.

7. A **beállítás az egyszeri bejelentkezés BetterWorks a** lapon hajtsa végre az alábbi lépéseket, és kattintson a **Tovább**gombra:

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-betterworks-tutorial/tutorial_betterworks_05.png)

    egy. Kattintson a **metaadat-alapú letöltése**, és mentse a fájlt a számítógépen.

    b. Kattintson a **Tovább**gombra.

8. Ha be van állítva az alkalmazás SSO, a BetterWorks támogatási csoport munkatársaitól keresztül <mailto:support@betterworks.com>. A metaadatok letöltött fájlt csatolni, és ossza meg BetterWorks csoporttal való egyszeri bejelentkezés beállítása a oldalán.

9. A klasszikus portálon jelölje be a egyszeri bejelentkezéses konfigurációs megerősítés, és kattintson a **Tovább gombra**.
    
    ![Azure Active Directory Single Sign-On][10]

10. Az **egyszeri bejelentkezés megerősítés** lapon kattintson a **Befejezés**gombra.  
    
    ![Azure Active Directory Single Sign-On][11]



### <a name="creating-an-azure-ad-test-user"></a>Azure AD-próba felhasználó létrehozása
Ez a szakasz célja próba felhasználó létrehozása az úgynevezett Britta Simon klasszikus portálon.

![Azure Active Directory-felhasználó létrehozása][20]

**Teszt felhasználó létrehozása az Azure Active Directory, hajtsa végre az alábbi lépéseket:**

1. Az **Azure klasszikus portálon**kattintson a bal oldali navigációs területen kattintson az **Active Directory**.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-betterworks-tutorial/create_aaduser_09.png)

2. A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3. A menüben, a képernyő tetején, a felhasználók listájának megjelenítéséhez kattintson a **felhasználók**elemre.
    
    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-betterworks-tutorial/create_aaduser_03.png)

4. **Felhasználó hozzáadása**elemre az alsó eszköztáron a **Felhasználó hozzáadása** párbeszédpanel megnyitásához.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-betterworks-tutorial/create_aaduser_04.png)

5. A párbeszédpanel **közli velünk a felhasználóra vonatkozó** lapon hajtsa végre az alábbi lépéseket:

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-betterworks-tutorial/create_aaduser_05.png)

    egy. Felhasználó típusa jelölje be az új felhasználó a szervezet.

    b. A felhasználónév **szövegmezőbe**írja be a **BrittaSimon**.

    c billentyűkombinációt. Kattintson a **Tovább**gombra.

6.  A **Felhasználói profil** párbeszédpanel lapon hajtsa végre az alábbi lépéseket:
    
    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-betterworks-tutorial/create_aaduser_06.png)

    egy. Az **első neve** mezőbe írja be a **Britta**.  

    b. Az **Utolsó neve** mezőbe írja be, **Simon**.

    c billentyűkombinációt. A **Megjelenítendő név** mezőbe írja be a **Britta Simon**.

    d. A **szerepkör** listában jelölje ki a **felhasználót**.

    e. Kattintson a **Tovább**gombra.

7. A párbeszédpanel **ideiglenes jelszót kérjen** lapon kattintson a **Hozzon létre**.
    
    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-betterworks-tutorial/create_aaduser_07.png)

8. A párbeszédpanel **ideiglenes jelszót első** lapon hajtsa végre az alábbi lépéseket:
    
    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-betterworks-tutorial/create_aaduser_08.png)

    egy. Jegyezze fel az **Új jelszót**az érték.

    b. Kattintson a **kész**gombra.   



### <a name="creating-a-betterworks-test-user"></a>BetterWorks próba felhasználó létrehozása

Ebben a részben hozzon létre egy felhasználót Britta Simon nevű BetterWorks. 

Kérje meg a BetterWorks támogatási csoporthoz keresztül <mailto:support@betterworks.com> a felhasználók felvétele a BetterWorks platform.


### <a name="assigning-the-azure-ad-test-user"></a>Az Azure Active Directory-próba felhasználói hozzárendelése

Ez a szakasz célja Britta Simon Azure egyszeri bejelentkezés a saját hozzáférést biztosít BetterWorks használandó engedélyezése.
    
   ![Felhasználó hozzárendelése][200]

**Britta Simon hozzárendelése BetterWorks, hajtsa végre az alábbi lépéseket:**

1. A klasszikus portál kattintva nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson az **alkalmazások** parancsra a felső menüben.
    
    ![Felhasználó hozzárendelése][201]

2. Az alkalmazások listájában jelölje ki a **BetterWorks**.
    
    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-betterworks-tutorial/tutorial_betterworks_50.png)

1. A felső sávon kattintson a **felhasználók**elemre.
    
    ![Felhasználó hozzárendelése][203]

1. A felhasználók listában jelölje ki a **Britta Simon**.

2. Az alsó eszköztáron kattintson a **hozzárendelni**.
    
    ![Felhasználó hozzárendelése][205]



### <a name="testing-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

Ez a szakasz célja a Azure Active Directory egyszeri bejelentkezéses beállítások tesztelése a Access panelen.
 
Ha az Access panel a BetterWorks mozaik gombra kattint, meg kell első automatikusan aláírt-on az BetterWorks alkalmazás.


## <a name="additional-resources"></a>További források

* [A szoftver alkalmazások integrálása az Azure Active Directory oktatóanyagok listája](active-directory-saas-tutorial-list.md)
* [Mi az access alkalmazás és az egyszeri bejelentkezés az Azure Active Directory címtárral?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-betterworks-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-betterworks-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-betterworks-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-betterworks-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-betterworks-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-betterworks-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-betterworks-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-betterworks-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-betterworks-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-betterworks-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-betterworks-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-betterworks-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-betterworks-tutorial/tutorial_general_205.png
