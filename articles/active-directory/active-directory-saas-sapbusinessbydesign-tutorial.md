<properties
    pageTitle="Oktatóprogram: Azure Active Directory-integráció a SAP üzleti ByDesign |} Microsoft Azure"
    description="Megtudhatja, hogy miként konfigurálása az egyszeri bejelentkezés Azure Active Directory és az SAP üzleti ByDesign között."
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
    ms.date="09/09/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-sap-business-bydesign"></a>Oktatóprogram: Azure Active Directory-integráció a SAP üzleti ByDesign

Ebből az oktatóanyagból megismerheti, hogyan SAP üzleti ByDesign integrálása az Azure Active Directory (Azure Active Directory).

SAP – üzleti ByDesign integrálása az Azure Active Directory biztosít a következő előnyökkel jár:

- Az Azure Active Directory SAP üzleti ByDesign hozzáféréssel rendelkező személyek megadhatja, hogy
- Az Azure Active Directory-fiókok engedélyezheti a felhasználóknak, hogy automatikusan első jelentkezett-on való SAP üzleti ByDesign (Single Sign-On)
- A fiókokat az egy központi helyen – az Azure klasszikus portálra

Ha többet szeretne tudni a szoftver alkalmazás integrálása az Azure AD meg, hogy, ismerje meg, [az access alkalmazás és az egyszeri bejelentkezés az Azure Active Directory címtárral](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek

Azure Active Directory integráció az SAP üzleti ByDesign van szükség az alábbiakat:

- Az Azure Active Directory-előfizetéssel
- Egy SAP üzleti ByDesign egyszeri bejelentkezés engedélyezett előfizetés


> [AZURE.NOTE] Az oktatóprogram lépéseit teszteléséhez nem használata ajánlott munkakörnyezetben.


Az oktatóprogram lépéseit teszteléséhez célszerű követnie az alábbi javaslatokat:

- Erre akkor szükség, ne használja a termelési környezetén.
- Ha nincs telepítve az Azure Active Directory próba környezetben, kattint egy hónap próba [Itt](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban tesztelni Azure AD az egyszeri bejelentkezés tesztkörnyezetben.

Az alkalmazási példát, ebben az oktatóanyagban tagolt két fő építőelemek áll:

1. SAP – üzleti ByDesign hozzáadása a gyűjteményből
2. Beállítása és tesztelése Azure Active Directory egyszeri bejelentkezés


## <a name="adding-sap-business-bydesign-from-the-gallery"></a>SAP – üzleti ByDesign hozzáadása a gyűjteményből
Konfigurálja az Azure Active Directory integrálása a SAP üzleti ByDesign, szüksége SAP üzleti ByDesign hozzáadása felügyelt szoftver alkalmazásai a gyűjteményből.

**A gyűjteményből SAP üzleti ByDesign felvételéhez hajtsa végre az alábbi lépéseket:**

1. Az **Azure klasszikus portálon**kattintson a bal oldali navigációs területen kattintson az **Active Directory**.

    ![Az Active Directory][1]

2. A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.


3. Nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson a felső menüben **alkalmazások** .

    ![Alkalmazások][2]

4. Kattintson a **Hozzáadás** az oldal alján.

    ![Alkalmazások][3]

5. **Milyen feladatot szeretne tenni** párbeszédpanelen kattintson **a gyűjteményből az alkalmazások felvétele**.

    ![Alkalmazások][4]

6. A Keresés mezőbe írja be az **SAP üzleti ByDesign**.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_01.png)

7. Az eredmény ablaktáblában jelölje ki az **SAP üzleti ByDesign**, és válassza a **kész** , az alkalmazás hozzáadása parancsra.

    ![Az Active Directory](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_02.png)


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Beállítása és tesztelése Azure Active Directory egyszeri bejelentkezés
Ebben a részben állítsa be, és az SAP üzleti ByDesign "Britta Simon" nevű próba felhasználón alapuló Azure AD az egyszeri bejelentkezés tesztelje.

Az egyszeri bejelentkezési a munkát az Azure Active Directory kell tudható, hogy mi az SAP üzleti ByDesign megfelelőjük a felhasználó egy felhasználóhoz, az Azure Active Directory. Más szóval Azure AD-felhasználó, és a kapcsolódó felhasználó SAP üzleti ByDesign hivatkozás viszonya kell létrehozni.

A hivatkozás kapcsolat jön létre által az Azure AD a program az SAP üzleti ByDesign **felhasználónevét** a **felhasználónév** értéket rendeli.

Állítsa be, és Azure Active Directory egyszeri bejelentkezés az SAP üzleti ByDesign tesztelése, a következő építőelemek szükséges:

1. **[Azure Active Directory konfigurálása az egyszeri bejelentkezés](#configuring-azure-ad-single-sign-on)** - engedélyezése a felhasználóknak, hogy ezzel a szolgáltatással.
2. **[Felhasználó létrehozása az Azure Active Directory tesztelése](#creating-an-azure-ad-test-user)** - Azure Active Directory egyszeri bejelentkezéssel való Britta Simon ellenőrzéséhez.
3. **[Felhasználó létrehozása egy SAP üzleti ByDesign tesztelése](#creating-an-sap-business-bydesign-test-user)** - van-e egy megfelelője a Britta Simon SAP üzleti ByDesign, amelyen az Azure Active Directory ábrázolt csatolt.
4. **[Az Azure Active Directory hozzárendelése tesztelje a felhasználó](#assigning-the-azure-ad-test-user)** - ahhoz, hogy Britta Simon Azure AD az egyszeri bejelentkezés használata.
5. **[Tesztelés egyszeri bejelentkezés](#testing-single-sign-on)** - ellenőrzéséhez, hogy működik-e a konfigurációs.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure Active Directory egyszeri bejelentkezés beállítása

Ebben a részben Azure AD az egyszeri bejelentkezés a klasszikus portálon engedélyezése, és az SAP üzleti ByDesign alkalmazásban az egyszeri bejelentkezés beállítása.

SAP – üzleti ByDesign alkalmazás a SAML Előfeltételek vár, egy bizonyos formátumban. Állítsa be a következő jogcímalapú ehhez az alkalmazáshoz. A **"Atrribute"** lapon, az alkalmazás a következő attribútumok értékének kezelheti. Az alábbi képernyőképen látható, a. 


**Állítson be Azure AD az egyszeri bejelentkezés SAP üzleti ByDesign, hajtsa végre az alábbi lépéseket:**


1. Az Azure klasszikus portálon lapon az **SAP üzleti ByDesign** alkalmazás integráció a menüben, a képernyő tetején kattintson a **attribútumok**elemre.

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_80.png) 


2. Attribútumok SAML jogkivonat attribútumok listájában jelölje ki a nameidentifier attribútum, és kattintson a **Szerkesztés**gombra.

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_84.png) 

3. A felhasználó attribútum szerkesztése párbeszédpanelen végezze el az alábbi lépéseket:

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_85.png) 

    egy. Az attribútumérték listából válassza ki a **ExtractMailPrefix()** fuction

    b. A levelek listából válassza ki a felhasználó attribútum a végrehajtásához használni kívánt. 
    Például ha szeretné használni az Alkalmazottkód oszlop egyedi felhasználói azonosító, és a ExtensionAttribute2 az attribútumérték tárolt, majd válassza a **user.extensionattribute2**. 

    c billentyűkombinációt. Kattintson a **kész**gombra. 
    

4. Az **SAP üzleti ByDesign** alkalmazás integrációs lapon klasszikus-portálon kattintson **konfigurálása az egyszeri bejelentkezés** a **Konfigurálása az egyszeri bejelentkezés** párbeszédpanel megnyitásához.
     
    ![Egyszeri bejelentkezés beállítása][6] 

5. **Hogyan szeretné, hogy jelentkezzen be az SAP üzleti ByDesign felhasználók** lapon válassza az **Azure Active Directory Single Sign-On**, és kattintson a **Tovább gombra**.

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_03.png) 

6. Az **Alkalmazás beállításainak megadása** párbeszédpanel lapon hajtsa végre az alábbi lépéseket:

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_04.png) 

    egy. Az **Bejelentkezési a URL-cím** mezőbe írja be a való bejelentkezéshez az SAP üzleti ByDesign alkalmazás a következő mintát használatával a felhasználók által használt URL-címe:`https://<servername>.sapbydesign.com`
    
    b. Kattintson a **Tovább** gombra
 
7. A **beállítás az egyszeri bejelentkezés SAP üzleti ByDesign a** lapon hajtsa végre az alábbi lépéseket:

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_05.png)

    egy. Kattintson a **metaadat-alapú letöltése**, és mentse a fájlt a számítógépen.

    b. Kattintson a **Tovább**gombra.


8. Ha be van állítva az alkalmazás SSO, hajtsa végre az alábbi lépéseket:

    egy. Jelentkezzen az SAP üzleti ByDesign portál rendszergazdai engedélyekkel.

    b. Nyissa meg a **alkalmazás és a felhasználó Management közös feladatot** , és kattintson a **Identitásszolgáltató** fülre.

    c billentyűkombinációt. Kattintson az **Új identitásszolgáltató** , és jelölje ki a metaadatok XML-fájlt az Azure klasszikus portáljáról letöltött. A metaadatok importálásával a rendszer automatikusan hatékonyan feltölti a szükséges aláírás tanúsítvány és a titkosítási tanúsítvány.

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_54.png)

    d. A **Állítás fogyasztói szolgáltatás URL-CÍMÉT** a SAML-összehívásba, jelölje be **Olyan állítás fogyasztói szolgáltatás URL-CÍMÉT**.

    e. Kattintson **az egyszeri bejelentkezés aktiválása**.

    f. A módosítások mentéséhez.

    g. Kattintson a **Saját** lapon.

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_52.png)

    h. **Egyszeri bejelentkezés URL-cím** másolása, és illessze be a **Azure Active Directory bejelentkezési URL-cím** mezőben lévő értéket.

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_53.png)

    lehet. Adja meg, hogy az alkalmazott manuálisan választhat felhasználói azonosítója és jelszava vagy egyszeri bejelentkezés **Kézi identitás szolgáltató kijelölés**kiválasztásával bejelentkezés.

    j. **Egyszeri bejelentkezés URL-címe** csoportban adja meg az alkalmazott ahhoz, hogy a rendszer által használandó URL-CÍMÉT. 
    Az URL-cím küldött alkalmazott legördülő lista, a választhat a következő beállításokat:
    
    **Nem egyszeri bejelentkezési URL-címe**
 
    A rendszer elküldi alkalmazott, akinek a csak a normál rendszer URL-CÍMÉT. Az alkalmazott így nem jelentkezzen be használata az egyszeri bejelentkezés, és kell jelszóval, vagy inkább a tanúsítvány.

    **EGYSZERI BEJELENTKEZÉS URL-CÍME** 

    A rendszer elküldi az alkalmazott csak az egyszeri bejelentkezési URL-cím. Az alkalmazott bejelentkezhet használata az egyszeri Bejelentkezést. Hitelesítési kérelmet a rendszer átirányítja a IdP keresztül.

    **Automatikus kiválasztásához.**
 
    Ha egyszeri bejelentkezés nem aktív, a rendszer az alkalmazott küld normál rendszer URL-CÍMÉT. Ha egyszeri bejelentkezés aktív, a rendszer ellenőrzi, hogy van-e az alkalmazott jelszót. Jelszó érhető el, ha az alkalmazott elküldi egyszeri bejelentkezési URL-cím és a nem-féle egyszeri Bejelentkezést URL-CÍMÉT. Azonban a alkalmazott nem tartozik jelszó, ha csak az egyszeri bejelentkezési URL-címet a rendszer elküldi a alkalmazott.

    k. A módosítások mentéséhez.

9. A klasszikus portálon jelölje be a egyszeri bejelentkezéses konfigurációs megerősítés, és kattintson a **Tovább gombra**.
    
    ![Azure Active Directory Single Sign-On][10]

10. Az **egyszeri bejelentkezés megerősítés** lapon kattintson a **Befejezés**gombra.  
 
    ![Azure Active Directory Single Sign-On][11]


### <a name="creating-an-azure-ad-test-user"></a>Azure AD-próba felhasználó létrehozása
Ebben a szakaszban a klasszikus portálon Britta Simon nevű próba felhasználó létrehozása.


![Azure Active Directory-felhasználó létrehozása][20]

**Teszt felhasználó létrehozása az Azure Active Directory, hajtsa végre az alábbi lépéseket:**

1. Az **Azure klasszikus portálon**kattintson a bal oldali navigációs területen kattintson az **Active Directory**.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-sapbusinessbydesign-tutorial/create_aaduser_09.png) 

2. A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3. A menüben, a képernyő tetején, a felhasználók listájának megjelenítéséhez kattintson a **felhasználók**elemre.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-sapbusinessbydesign-tutorial/create_aaduser_03.png) 

4. **Felhasználó hozzáadása**elemre az alsó eszköztáron a **Felhasználó hozzáadása** párbeszédpanel megnyitásához.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-sapbusinessbydesign-tutorial/create_aaduser_04.png) 

5. A párbeszédpanel **közli velünk a felhasználóra vonatkozó** lapon hajtsa végre az alábbi lépéseket:
    
    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-sapbusinessbydesign-tutorial/create_aaduser_05.png) 

    egy. Felhasználó típusa jelölje be az új felhasználó a szervezet.

    b. A felhasználónév **szövegmezőbe**írja be a **BrittaSimon**.

    c billentyűkombinációt. Kattintson a **Tovább**gombra.

6.  A **Felhasználói profil** párbeszédpanel lapon hajtsa végre az alábbi lépéseket:
    
    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-sapbusinessbydesign-tutorial/create_aaduser_06.png) 

    egy. Az **első neve** mezőbe írja be a **Britta**.  

    b. Az **Utolsó neve** mezőbe írja be, **Simon**.

    c billentyűkombinációt. A **Megjelenítendő név** mezőbe írja be a **Britta Simon**.

    d. A **szerepkör** listában jelölje ki a **felhasználót**.

    e. Kattintson a **Tovább**gombra.

7. A párbeszédpanel **ideiglenes jelszót első** lapon kattintson a **Hozzon létre**.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-sapbusinessbydesign-tutorial/create_aaduser_07.png) 

8. A párbeszédpanel **ideiglenes jelszót első** lapon hajtsa végre az alábbi lépéseket:

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-sapbusinessbydesign-tutorial/create_aaduser_08.png) 

    egy. Jegyezze fel az **Új jelszót**az érték.

    b. Kattintson a **kész**gombra.   



### <a name="creating-an-sap-business-bydesign-test-user"></a>Az SAP üzleti ByDesign teszt felhasználó létrehozása

Ebben a részben egy SAP üzleti ByDesign Britta Simon nevű felhasználót hoz létre. SAP – üzleti ByDesign támogatási csoporthoz, a felhasználók felvétele az SAP üzleti ByDesign platform működik. 

> [AZURE.NOTE] Győződjön meg arról, hogy NameID értékét meg kell egyeznie a SAP üzleti ByDesign platform felhasználónév mezővel.

### <a name="assigning-the-azure-ad-test-user"></a>Az Azure Active Directory-próba felhasználói hozzárendelése

Ebben a részben Britta Simon Azure egyszeri bejelentkezés a saját hozzáférést biztosít SAP üzleti ByDesign használandó engedélyezése.

![Felhasználó hozzárendelése][200] 

**SAP üzleti ByDesign Britta Simon rendelni, hajtsa végre az alábbi lépéseket:**

1. A klasszikus portál kattintva nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson az **alkalmazások** parancsra a felső menüben.

    ![Felhasználó hozzárendelése][201] 

2. Az alkalmazások listájában jelölje ki az **SAP üzleti ByDesign**.

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_50.png) 

3. A felső sávon kattintson a **felhasználók**elemre.

    ![Felhasználó hozzárendelése][203]

4. A felhasználók listában jelölje ki a **Britta Simon**.

5. Az alsó eszköztáron kattintson a **hozzárendelni**.

    ![Felhasználó hozzárendelése][205]


### <a name="testing-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

Ebben a részben tesztelje az Azure Active Directory egyetlen bejelentkezés beállítási a Access panelen.

Amikor az Access panelen az SAP üzleti ByDesign csempe gombra kattint, meg kell első automatikusan aláírt-on az SAP üzleti ByDesign alkalmazás.


## <a name="additional-resources"></a>További források

* [A szoftver alkalmazások integrálása az Azure Active Directory oktatóanyagok listája](active-directory-saas-tutorial-list.md)
* [Mi az access alkalmazás és az egyszeri bejelentkezés az Azure Active Directory címtárral?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_205.png
