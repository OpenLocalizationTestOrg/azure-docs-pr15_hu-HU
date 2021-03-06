<properties
    pageTitle="Oktatóprogram: Azure Active Directory-integráció a Tangoe parancs prémium Mobile |} Microsoft Azure"
    description="Megtudhatja, hogy miként konfigurálása az egyszeri bejelentkezés Azure Active Directory és Tangoe parancs prémium Mobile között."
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


# <a name="tutorial-azure-active-directory-integration-with-tangoe-command-premium-mobile"></a>Oktatóprogram: Azure Active Directory-integráció a Tangoe parancs prémium Mobile

Ebből az oktatóanyagból megismerheti, hogyan Tangoe parancs prémium Mobile integrálása az Azure Active Directory (Azure Active Directory).

Tangoe parancs prémium Mobile integrálása az Azure Active Directory biztosít a következő előnyökkel jár:

- Az Azure Active Directory Tangoe parancs prémium mobil hozzáféréssel rendelkező személyek megadhatja, hogy
- Az Azure Active Directory-fiókok engedélyezheti a felhasználóknak, hogy automatikusan első jelentkezett-on Tangoe parancs prémium Mobile (Single Sign-On)
- A fiókokat az egy központi helyen – az Azure klasszikus portálra

Ha többet szeretne tudni a szoftver alkalmazás integrálása az Azure AD meg, hogy, ismerje meg, [az access alkalmazás és az egyszeri bejelentkezés az Azure Active Directory címtárral](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek

Azure Active Directory integráció a Tangoe parancs prémium Mobile van szükség az alábbiakat:

- Az Azure előfizetéssel
- Egy Tangoe parancs prémium Mobile egyszeri bejelentkezés engedélyezett előfizetés


> [AZURE.NOTE] Az oktatóprogram lépéseit teszteléséhez nem használata ajánlott munkakörnyezetben.


Az oktatóprogram lépéseit teszteléséhez célszerű követnie az alábbi javaslatokat:

- Erre akkor szükség, ne használja a termelési környezetén.
- Ha nincs telepítve az Azure Active Directory próba környezetben, kattint egy hónap próba [Itt](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban tesztelni Azure AD az egyszeri bejelentkezés tesztkörnyezetben. 

Az alkalmazási példát, ebben az oktatóanyagban tagolt két fő építőelemek áll:

1. Tangoe parancs prémium Mobile hozzáadása a gyűjteményből
2. Beállítása és tesztelése Azure Active Directory egyszeri bejelentkezés


## <a name="adding-tangoe-command-premium-mobile-from-the-gallery"></a>Tangoe parancs prémium Mobile hozzáadása a gyűjteményből
Konfigurálja az Azure Active Directory Tangoe parancs prémium Mobile integrálása, szüksége Tangoe parancs prémium Mobile hozzáadása felügyelt szoftver alkalmazásai a gyűjteményből.

**A gyűjteményből Tangoe parancs prémium Mobile felvételéhez hajtsa végre az alábbi lépéseket:**

1. Az **Azure klasszikus portálon**kattintson a bal oldali navigációs területen kattintson az **Active Directory**. 

    ![Az Active Directory][1]

2. A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3. Nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson a felső menüben **alkalmazásokat** .

    ![Alkalmazások][2]

4. Kattintson a **Hozzáadás** az oldal alján.

    ![Alkalmazások][3]

5. **Milyen feladatot szeretne tenni** párbeszédpanelen kattintson **a gyűjteményből az alkalmazások felvétele**.

    ![Alkalmazások][4]

6. A Keresés mezőbe írja be a **Tangoe parancs prémium Mobile**.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-tangoe-tutorial/tutorial_tangoe_01.png)

7. Az eredmény ablaktáblában jelölje ki a **Tangoe parancs prémium Mobile**pontra, és kattintson a **kész** az alkalmazás hozzáadása parancsra.

![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-tangoe-tutorial/tutorial_tangoe_02.png)




##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Beállítása és tesztelése Azure Active Directory egyszeri bejelentkezés
Ebben a részben konfigurálása és a "Britta Simon" nevű próba felhasználón alapuló Tangoe parancs prémium Mobile Azure AD az egyszeri bejelentkezés tesztelése.

Egyszeri bejelentkezés a munkát Azure AD meg kell adni szeretné, hogy mi a megfelelőjük a felhasználó az Tangoe parancs prémium Mobile-ban egy felhasználó az Azure Active Directory. Más szóval Azure AD-felhasználó, és a kapcsolódó felhasználó Tangoe parancs prémium Mobile hivatkozás viszonya kell létrehozni.

A hivatkozás kapcsolat jön létre által az Azure Active Directory Tangoe parancs prémium Mobile **felhasználónév** értékként a **felhasználó neve** értéket rendeli.

Beállítása és tesztelése Azure AD az egyszeri bejelentkezés Tangoe parancs prémium Mobile használatával, a következő építőelemek szükséges:

1. **[Azure Active Directory konfigurálása az egyszeri bejelentkezés](#configuring-azure-ad-single-single-sign-on)** - engedélyezése a felhasználóknak, hogy ezzel a szolgáltatással.
2. **[Felhasználó létrehozása az Azure Active Directory tesztelése](#creating-an-azure-ad-test-user)** - Azure Active Directory egyszeri bejelentkezéssel való Britta Simon ellenőrzéséhez.
4. **[Felhasználó létrehozása egy Tangoe parancs prémium Mobile tesztelése](#creating-an-tangoe-test-user)** - van-e egy megfelelője a Britta Simon Tangoe parancs prémium Mobile rendszerbe Azure Active Directory ábrázolása csatolt.
5. **[Az Azure Active Directory hozzárendelése tesztelje a felhasználó](#assigning-the-azure-ad-test-user)** - ahhoz, hogy Britta Simon Azure AD az egyszeri bejelentkezés használata.
5. **[Tesztelés egyszeri bejelentkezés](#testing-single-sign-on)** - ellenőrzéséhez, hogy működik-e a konfigurációs.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure Active Directory Single Sign-On konfigurálása

Ebben a részben Azure AD az egyszeri bejelentkezés a klasszikus portálon engedélyezése, majd állítsa az egyszeri bejelentkezés a Tangoe parancs prémium Mobile alkalmazást.


**Azure Active Directory egyszeri bejelentkezés beállítása Tangoe parancs prémium Mobile használatával, hajtsa végre az alábbi lépéseket:**

1. A klasszikus portálon **Tangoe parancs prémium Mobile** alkalmazást integrációs lapon kattintson **konfigurálása az egyszeri bejelentkezés** a **Konfigurálása az egyszeri bejelentkezés** párbeszédpanel megnyitásához.

    ![Egyszeri bejelentkezés beállítása][6]


2. **Hogyan szeretné, hogy jelentkezzen be az Tangoe parancs prémium Mobile a felhasználók** lapon válassza az **Azure Active Directory Single Sign-On**, és kattintson a **Tovább gombra**.

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-tangoe-tutorial/tutorial_tangoe_03.png) 


3. Az **Alkalmazás beállításainak megadása** párbeszédpanel lapon hajtsa végre az alábbi lépéseket:
 
    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-tangoe-tutorial/tutorial_tangoe_04.png) 


    egy. **Bejelentkezési a URL-cím** mezőbe írja be az URL-cím való bejelentkezésre a Tangoe parancs prémium Mobile alkalmazást, az alábbi minta használatával a felhasználók által használt: **"https://sso.tangoe.com/sp/startSSO.ping?PartnerIdpId=\<bérlői kibocsátó\>& cél =\<lap URL-címe Megcélozhat\>"**.

    b. **Válasz URL-cím** mezőbe írja be a következő mintát az URL-cím: **"https://sso.tangoe.com/sp/ACS.saml2"**

    > [AZURE.NOTE]  Ha nem tudja, hogy a megfelelő értékeket az URL-címek, használhatja a fenti értékeket helyőrzők és kérése a megfelelő értékeket a Tangoe támogatási társítani.


4. A **beállítás az egyszeri bejelentkezés Tangoe parancs prémium mobilon** lapon hajtsa végre az alábbi lépéseket:
 
    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-tangoe-tutorial/tutorial_tangoe_05.png) 

    egy. Kattintson a **metaadat-alapú letöltése**, és mentse a fájlt a számítógépen.

    b. Kattintson a **Tovább**gombra.


5.  Ha be van állítva az alkalmazás SSO, lépjen kapcsolatba a Tangoe ügyfélszolgálat rendelni, és küldje el az alábbi:


    - A metaadatok letöltött fájl
    - A **kibocsátó URL-címe**
    - A **SAML egyszeri bejelentkezési URL-címe**
    - Az **egyetlen kijelentkezési szolgáltatás URL-címe**


  
6. A klasszikus portálon jelölje be a egyszeri bejelentkezéses konfigurációs megerősítés, és kattintson a **Tovább gombra**.

    ![Azure Active Directory Single Sign-On][10]

7. Az **egyszeri bejelentkezés megerősítés** lapon kattintson a **Befejezés**gombra.  
  
    ![Azure Active Directory Single Sign-On][11]




### <a name="creating-an-azure-ad-test-user"></a>Azure AD-próba felhasználó létrehozása
Ebben a szakaszban a klasszikus portálon Britta Simon nevű próba felhasználó létrehozása.

A felhasználók listában jelölje ki a **Britta Simon**.

![Azure Active Directory-felhasználó létrehozása][20]

**Teszt felhasználó létrehozása az Azure Active Directory, hajtsa végre az alábbi lépéseket:**

1. Az **Azure klasszikus portálon**kattintson a bal oldali navigációs területen kattintson az **Active Directory**.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-tangoe-tutorial/create_aaduser_09.png) 

2. A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3. A menüben, a képernyő tetején, a felhasználók listájának megjelenítéséhez kattintson a **felhasználók**elemre.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-tangoe-tutorial/create_aaduser_03.png) 

4. **Felhasználó hozzáadása**elemre az alsó eszköztáron a **Felhasználó hozzáadása** párbeszédpanel megnyitásához.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-tangoe-tutorial/create_aaduser_04.png)


5. A párbeszédpanel **közli velünk a felhasználóra vonatkozó** lapon hajtsa végre az alábbi lépéseket:

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-tangoe-tutorial/create_aaduser_05.png) 

    egy. Felhasználó típusa jelölje be az új felhasználó a szervezet.

    b. A felhasználónév **szövegmezőbe**írja be a **BrittaSimon**.

    c billentyűkombinációt. Kattintson a **Tovább**gombra.

6.  A **Felhasználói profil** párbeszédpanel lapon hajtsa végre az alábbi lépéseket:

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-tangoe-tutorial/create_aaduser_06.png) 

    egy. Az **első neve** mezőbe írja be a **Britta**.  

    b. Az **Utolsó neve** mezőbe írja be, **Simon**.

    c billentyűkombinációt. A **Megjelenítendő név** mezőbe írja be a **Britta Simon**.

    d. A **szerepkör** listában jelölje ki a **felhasználót**.

    e. Kattintson a **Tovább**gombra.

7. A párbeszédpanel **ideiglenes jelszót kérjen** lapon kattintson a **Hozzon létre**.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-tangoe-tutorial/create_aaduser_07.png) 

8. A párbeszédpanel **ideiglenes jelszót első** lapon hajtsa végre az alábbi lépéseket:
 
    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-tangoe-tutorial/create_aaduser_08.png) 

    egy. Jegyezze fel az **Új jelszót**az érték.

    b. Kattintson a **kész**gombra.   



### <a name="creating-an-tangoe-command-premium-mobile-test-user"></a>Egy Tangoe parancs prémium Mobile teszt felhasználó létrehozása

Ebben a részben hozzon létre egy felhasználót Britta Simon nevű Tangoe parancs prémium Mobile alkalmazásban. Tangoe parancs prémium Mobile alkalmazást kell azon felhasználók számára, hogy az alkalmazás az egyszeri bejelentkezés végrehajtása előtt meg kell építenie. Így tájékoztassa a munkát, a Tangoe ügyfél-támogatási társítása ezek a felhasználók létrehozására az alkalmazásba. 


> [AZURE.NOTE] Ha módosítani szeretné a felhasználó létrehozása manuálisan vagy a köteg felhasználók kell a Tangoe parancs prémium Mobile támogatási csoport munkatársaitól kaphat.


### <a name="assigning-the-azure-ad-test-user"></a>Az Azure Active Directory-próba felhasználói hozzárendelése

Ebben a részben Britta Simon Azure egyszeri bejelentkezés a saját hozzáférést biztosít Tangoe parancs prémium Mobile használandó engedélyezése.

![Felhasználó hozzárendelése][200] 

**Britta Simon hozzárendelése Tangoe parancs prémium Mobile, hajtsa végre az alábbi lépéseket:**

1. A klasszikus portál kattintva nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson az **alkalmazások** parancsra a felső menüben.

![Felhasználó hozzárendelése][201] 

2. Az alkalmazások listájában jelölje ki a **Tangoe parancs prémium Mobile**.

![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-tangoe-tutorial/tutorial_tangoe_50.png) 

1. A felső sávon kattintson a **felhasználók**elemre.

![Felhasználó hozzárendelése][203] 

1. A felhasználók listában jelölje ki a **Britta Simon**.

2. Az alsó eszköztáron kattintson a **hozzárendelni**.

![Felhasználó hozzárendelése][205]



### <a name="testing-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

Ebben a részben tesztelje az Azure Active Directory egyetlen bejelentkezés beállítási a Access panelen.

Amikor az Access panel a Tangoe parancs prémium Mobile mozaik gombra kattint, meg kell első automatikusan aláírt-on a Tangoe parancs prémium Mobile alkalmazásba.


## <a name="additional-resources"></a>További források

* [A szoftver alkalmazások integrálása az Azure Active Directory oktatóanyagok listája](active-directory-saas-tutorial-list.md)
* [Mi az access alkalmazás és az egyszeri bejelentkezés az Azure Active Directory címtárral?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_205.png
