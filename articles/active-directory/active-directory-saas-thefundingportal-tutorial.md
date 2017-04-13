<properties
    pageTitle="Oktatóprogram: Azure Active Directory-integráció a támogatás a portálon |} Microsoft Azure"
    description="Megtudhatja, hogy miként konfigurálása az egyszeri bejelentkezés Azure Active Directory és a támogatás portál között."
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
    ms.date="09/02/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-the-funding-portal"></a>Oktatóprogram: Azure Active Directory-integráció a támogatás a portálon

Ebből az oktatóanyagból megismerheti, hogyan a támogatás Portal integrálása az Azure Active Directory (Azure Active Directory).

A támogatás Portal integrálása az Azure Active Directory biztosít a következő előnyökkel jár:

- Az Azure Active Directory, aki rendelkezik hozzáféréssel a támogatás portálra megadhatja, hogy
- Az Azure Active Directory-fiókok engedélyezheti a felhasználóknak, hogy automatikusan első jelentkezett-on a támogatás portálra (Single Sign-On)
- A fiókokat az egy központi helyen – az Azure klasszikus portálra

Ha többet szeretne tudni a szoftver alkalmazás integrálása az Azure AD meg, hogy, ismerje meg, [az access alkalmazás és az egyszeri bejelentkezés az Azure Active Directory címtárral](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek

Azure Active Directory integráció a támogatás Portal segítségével van szükség az alábbiakat:

- Az Azure Active Directory-előfizetéssel
- A **Támogatás portál az** egyszeri bejelentkezés engedélyezett előfizetéshez tartozó


> [AZURE.NOTE] Az oktatóprogram lépéseit teszteléséhez nem használata ajánlott munkakörnyezetben.


Az oktatóprogram lépéseit teszteléséhez célszerű követnie az alábbi javaslatokat:

- Erre akkor szükség, ne használja a termelési környezetén.
- Ha nincs telepítve az Azure Active Directory próba környezetben, kattint egy hónap próba [Itt](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban tesztelni Azure AD az egyszeri bejelentkezés tesztkörnyezetben. Az alkalmazási példát, ebben az oktatóanyagban tagolt két fő építőelemek áll:

1. A támogatás portál hozzáadása a gyűjteményből
2. Beállítása és tesztelése Azure Active Directory egyszeri bejelentkezés


## <a name="adding-the-funding-portal-from-the-gallery"></a>A támogatás portál hozzáadása a gyűjteményből
Adja meg a támogatás Portal integrálása Azure Active Directory, szüksége a támogatás portál hozzáadása felügyelt szoftver alkalmazásai a gyűjteményből.

**A gyűjteményből az támogatás portál felvételéhez hajtsa végre az alábbi lépéseket:**

1. Az **Azure klasszikus portálon**kattintson a bal oldali navigációs területen kattintson az **Active Directory**. 

    ![Az Active Directory][1]

2. A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3. Nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson a felső menüben **alkalmazásokat** .

    ![Alkalmazások][2]

4. Kattintson a **Hozzáadás** az oldal alján.

    ![Alkalmazások][3]

5. **Milyen feladatot szeretne tenni** párbeszédpanelen kattintson **a gyűjteményből az alkalmazások felvétele**.

    ![Alkalmazások][4]

6. A Keresés mezőbe írja be **A támogatás portálon**.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-thefundingportal-tutorial/tutorial_thefundingportal_01.png)

7. Az eredmény ablaktáblában válassza **A támogatás portál**, és válassza a **kész** , az alkalmazás hozzáadása parancsra.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-thefundingportal-tutorial/tutorial_thefundingportal_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Beállítása és tesztelése Azure Active Directory egyszeri bejelentkezés
Ebben a részben konfigurálása és a támogatás Portal segítségével "Britta Simon" nevű vizsgálat felhasználón alapuló Azure AD az egyszeri bejelentkezés tesztelése.

Az egyszeri bejelentkezési a munkát az Azure Active Directory kell tudható, hogy mi a megfelelőjük a felhasználó a támogatás portálon egy felhasználóhoz, az Azure Active Directory. Más szóval Azure AD-felhasználó, és a kapcsolódó felhasználó a támogatás portálon hivatkozás viszonya kell létrehozni.
Ez a hivatkozás kapcsolat a értéket a **felhasználó nevét** a **felhasználónév** a támogatás portálon értékként Azure AD hozzárendelésével jön létre.

Beállítása és tesztelése Azure AD az egyszeri bejelentkezés a támogatás portálján, a következő építőelemek szükséges:

1. **[Azure Active Directory konfigurálása az egyszeri bejelentkezés](#configuring-azure-ad-single-single-sign-on)** - engedélyezése a felhasználóknak, hogy ezzel a szolgáltatással.
2. **[Felhasználó létrehozása az Azure Active Directory tesztelése](#creating-an-azure-ad-test-user)** - Azure Active Directory egyszeri bejelentkezéssel való Britta Simon ellenőrzéséhez.
4. **[A támogatás portál létrehozása tesztelje a felhasználó](#creating-a-the-funding-portal-test-user)** -, hogy egy partner a Britta Simon az Azure Active Directory ábrázolása amelyen csatolt a támogatás portálon.
5. **[Az Azure Active Directory hozzárendelése tesztelje a felhasználó](#assigning-the-azure-ad-test-user)** - ahhoz, hogy Britta Simon Azure AD az egyszeri bejelentkezés használata.
5. **[Tesztelés egyszeri bejelentkezés](#testing-single-sign-on)** - ellenőrzéséhez, hogy működik-e a konfigurációs.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure Active Directory egyszeri bejelentkezés beállítása

Ez a szakasz célja Azure AD az egyszeri bejelentkezés az Azure klasszikus portálon engedélyezése és beállítása az egyszeri bejelentkezés a támogatás portál alkalmazás.

A támogatás portál alkalmazás vár, a SAML Előfeltételek "externalId1" nevű attribútumot tartalmazó. Az érték az "externalId1" a felismert studentID kell lennie. Állítsa be az "externalId1" állítást ehhez az alkalmazáshoz. A **"Atrributes"** lapon, az alkalmazás a következő attribútumok értékének kezelheti. Az alábbi képernyőképen látható, a.

![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-thefundingportal-tutorial/tutorial_thefundingportal_03.png) 

**Azure Active Directory egyszeri bejelentkezés beállítása a támogatás Portal segítségével, hajtsa végre az alábbi lépéseket:**

1. Kattintson a lapon **A támogatás portál** alkalmazás integráció a menüben, a képernyő tetején, az Azure klasszikus portál **attribútumok**.
     
    ![Egyszeri bejelentkezés beállítása][5]

2. A SAML jogkivonat attribútumok párbeszédpanelen adja hozzá a "externalId1" attribútumot.

    egy. Kattintson a **felhasználó attribútum hozzáadása** a **Felhasználó attribútum hozzáadása** párbeszédpanel megnyitásához. 
    
    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-thefundingportal-tutorial/tutorial_thefundingportal_05.png)

    b. Az **Attribútum neve** mezőbe írja be a attribútum neve "externalId1".

    c billentyűkombinációt. Az **Attribútumérték** listából válassza ki a példányába használni kívánt attribútumot. Például ha StudentID értékét a ExtensionAttribute1 tárolt, majd jelölje be user.extensionattribute1.

    d. Kattintson a **kész**gombra. Kattintson a **Módosítások alkalmazása**.

1. A felső sávon kattintson a **Quick Start**.

    ![Egyszeri bejelentkezés beállítása][6]

2. A klasszikus portálon, **A támogatás portál** alkalmazás integrációs lapon kattintson **konfigurálása az egyszeri bejelentkezés** a **Konfigurálása az egyszeri bejelentkezés** párbeszédpanel megnyitásához.

    ![Egyszeri bejelentkezés beállítása][7] 

3. **Hogyan szeretné, hogy jelentkezzen be az támogatás portált a felhasználók** lapon válassza az **Azure Active Directory Single Sign-On**, és kattintson a **Tovább gombra**.
    
    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-thefundingportal-tutorial/tutorial_thefundingportal_06.png)

4. Az **Alkalmazás beállításainak megadása** párbeszédpanel lapon hajtsa végre az alábbi lépéseket: 

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-thefundingportal-tutorial/tutorial_thefundingportal_07.png)


    egy. Bejelentkezési a URL-cím mezőbe írja be az URL-cím használata a következő mintát: `https://<subdomain>.regenteducation.net/`.

    b. Kattintson a **Tovább**gombra.

5. Kattintson a **konfigurálása az egyszeri bejelentkezés a támogatás portálon a** lap, **Töltse le a metaadatok**, és mentse a fájlt a számítógépen.

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-thefundingportal-tutorial/tutorial_thefundingportal_08.png)

6. Ha be van állítva az alkalmazás SSO, a támogatás a portálon ügyfélszolgálatát. Azok segítséget a megfelelő csatorna egyszeri bejelentkezés beállítása. Kérjük, vegye figyelembe, hogy meg kell küldhet e-mailt, és csatolja letölti-e metaadat-fájlt szeretne info@regenteducation.com.

7. A klasszikus portálon jelölje be a egyszeri bejelentkezéses konfigurációs megerősítés, és kattintson a **Tovább gombra**.
    
    ![Azure Active Directory Single Sign-On][10]

8. Az **egyszeri bejelentkezés megerősítés** lapon kattintson a **Befejezés**gombra.  
    
    ![Azure Active Directory Single Sign-On][11]

### <a name="creating-an-azure-ad-test-user"></a>Azure AD-próba felhasználó létrehozása
Ebben a szakaszban a klasszikus portálon Britta Simon nevű próba felhasználó létrehozása.

![Azure Active Directory-felhasználó létrehozása][20]

**Teszt felhasználó létrehozása az Azure Active Directory, hajtsa végre az alábbi lépéseket:**

1. Az **Azure klasszikus portálon**kattintson a bal oldali navigációs területen kattintson az **Active Directory**.
    
    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-thefundingportal-tutorial/create_aaduser_09.png) 

2. A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3. A menüben, a képernyő tetején, a felhasználók listájának megjelenítéséhez kattintson a **felhasználók**elemre.
    
    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-thefundingportal-tutorial/create_aaduser_03.png) 

4. **Felhasználó hozzáadása**elemre az alsó eszköztáron a **Felhasználó hozzáadása** párbeszédpanel megnyitásához.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-thefundingportal-tutorial/create_aaduser_04.png) 

5. A párbeszédpanel **közli velünk a felhasználóra vonatkozó** lapon hajtsa végre az alábbi lépéseket:
 
    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-thefundingportal-tutorial/create_aaduser_05.png) 

    egy. Felhasználó típusa jelölje be az új felhasználó a szervezet.

    b. A felhasználónév **szövegmezőbe**írja be a **BrittaSimon**.

    c billentyűkombinációt. Kattintson a **Tovább**gombra.

6.  A **Felhasználói profil** párbeszédpanel lapon hajtsa végre az alábbi lépéseket:

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-thefundingportal-tutorial/create_aaduser_06.png) 

    egy. Az **első neve** mezőbe írja be a **Britta**.  

    b. Az **Utolsó neve** mezőbe írja be, **Simon**.

    c billentyűkombinációt. A **Megjelenítendő név** mezőbe írja be a **Britta Simon**.

    d. A **szerepkör** listában jelölje ki a **felhasználót**.

    e. Kattintson a **Tovább**gombra.

7. A párbeszédpanel **ideiglenes jelszót kérjen** lapon kattintson a **Hozzon létre**.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-thefundingportal-tutorial/create_aaduser_07.png) 

8. A párbeszédpanel **ideiglenes jelszót első** lapon hajtsa végre az alábbi lépéseket:

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-thefundingportal-tutorial/create_aaduser_08.png) 

    egy. Jegyezze fel az **Új jelszót**az érték.

    b. Kattintson a **kész**gombra.   



### <a name="creating-a-the-funding-portal-test-user"></a>A támogatás portál vizsgálat felhasználó létrehozása

Ebben a részben hozzon létre egy felhasználót Britta Simon nevű támogatás a portálon. Ha nem tudja, hogy a támogatás portál Britta Simon hozzáadását, kérje a próba-felhasználó hozzáadása és az egyszeri bejelentkezés engedélyezése a támogatás portál részlegtől. Kapcsolatba léphet az illetővel, info@regenteducation.com.

### <a name="assigning-the-azure-ad-test-user"></a>Az Azure Active Directory-próba felhasználói hozzárendelése

Ebben a részben engedélyezése Britta Simon használandó Azure egyszeri bejelentkezés a saját hozzáférés engedélyezése a támogatás portálra.

![Felhasználó hozzárendelése][200] 

**A támogatás portál Britta Simon rendelni, hajtsa végre az alábbi lépéseket:**

1. A klasszikus portál kattintva nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson az **alkalmazások** parancsra a felső menüben.

    ![Felhasználó hozzárendelése][201] 

2. Alkalmazások listájában válassza **A támogatás portálon**.

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-thefundingportal-tutorial/tutorial_thefundingportal_09.png) 

1. A felső sávon kattintson a **felhasználók**elemre.

    ![Felhasználó hozzárendelése][203] 

1. Az összes felhasználó listában válassza a **Britta Simon**.

2. Az alsó eszköztáron kattintson a **hozzárendelni**.

    ![Felhasználó hozzárendelése][205]


### <a name="testing-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

Ez a szakasz célja a Azure Active Directory egyszeri bejelentkezéses beállítások tesztelése a Access panelen.

A támogatás a portálon csempére az Access panel gombra kattintva meg kell első automatikusan jelentkezett-on az a támogatás portál alkalmazás.

## <a name="additional-resources"></a>További források

* [A szoftver alkalmazások integrálása az Azure Active Directory oktatóanyagok listája](active-directory-saas-tutorial-list.md)
* [Mi az access alkalmazás és az egyszeri bejelentkezés az Azure Active Directory címtárral?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-thefundingportal-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-thefundingportal-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-thefundingportal-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-thefundingportal-tutorial/tutorial_general_04.png


[5]: ./media/active-directory-saas-thefundingportal-tutorial/tutorial_general_05.png
[6]: ./media/active-directory-saas-thefundingportal-tutorial/tutorial_general_06.png
[7]:  ./media/active-directory-saas-thefundingportal-tutorial/tutorial_general_050.png
[10]: ./media/active-directory-saas-thefundingportal-tutorial/tutorial_general_060.png
[11]: ./media/active-directory-saas-thefundingportal-tutorial/tutorial_general_070.png
[20]: ./media/active-directory-saas-thefundingportal-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-thefundingportal-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-thefundingportal-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-thefundingportal-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-thefundingportal-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-thefundingportal-tutorial/tutorial_general_205.png
