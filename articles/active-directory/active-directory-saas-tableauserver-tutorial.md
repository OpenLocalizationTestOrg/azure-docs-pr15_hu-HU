<properties
    pageTitle="Oktatóprogram: Azure Active Directory-integráció a Tableau kiszolgáló |} Microsoft Azure"
    description="Megtudhatja, hogy miként konfigurálható az egyszeri bejelentkezés Azure Active Directory és a Tableau kiszolgáló közötti."
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


# <a name="tutorial-azure-active-directory-integration-with-tableau-server"></a>Oktatóprogram: Azure Active Directory-integráció a Tableau kiszolgáló

Ebben az oktatóanyagban célja mutatja, hogy miként Tableau Server integrálása az Azure Active Directory (Azure Active Directory).

Tableau kiszolgáló integrálása az Azure Active Directory biztosít a következő előnyökkel jár:

- Beállíthatja, hogy, aki hozzáfér Tableau Server Azure AD
- Az Azure Active Directory-fiókok engedélyezheti a felhasználóknak, hogy automatikusan első jelentkezett-on Tableau kiszolgálóhoz (Single Sign-On)
- A fiókokat az egy központi helyen – az Azure klasszikus portálra

Ha többet szeretne tudni a szoftver alkalmazás integrálása az Azure AD meg, hogy, ismerje meg, [az access alkalmazás és az egyszeri bejelentkezés az Azure Active Directory címtárral](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek

Azure Active Directory integráció Tableau kiszolgálóval van szükség az alábbiakat:

- Az Azure Active Directory-előfizetéssel
- A kiszolgáló Tableau egyszeri bejelentkezés engedélyezett előfizetés


> [AZURE.NOTE] Az oktatóprogram lépéseit teszteléséhez nem használata ajánlott munkakörnyezetben.


Az oktatóprogram lépéseit teszteléséhez célszerű követnie az alábbi javaslatokat:

- Erre akkor szükség, ne használja a termelési környezetén.
- Ha nincs telepítve az Azure Active Directory próba környezetben, kattint egy hónap próba [Itt](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban célja lehetővé teszi az Azure Active Directory egyszeri bejelentkezés tesztkörnyezetben tesztelése. 

Az alkalmazási példát, ebben az oktatóanyagban tagolt két fő építőelemek áll:

1. Tableau kiszolgáló hozzáadása a gyűjteményből
2. Beállítása és tesztelése Azure Active Directory egyszeri bejelentkezés


## <a name="adding-tableau-server-from-the-gallery"></a>Tableau kiszolgáló hozzáadása a gyűjteményből
Adja meg a integrálása a Tableau Server Azure Active Directory, szüksége Tableau kiszolgáló hozzáadása az felügyelt szoftver alkalmazáslistában a gyűjteményből.

**A gyűjteményből Tableau kiszolgáló felvételéhez hajtsa végre az alábbi lépéseket:**

1. Az **Azure klasszikus portálon**kattintson a bal oldali navigációs területen kattintson az **Active Directory**. 
 
    ![Az Active Directory][1]

2. A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3. Nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson a felső menüben **alkalmazások** .

    ![Alkalmazások][2]

4. Kattintson a **Hozzáadás** az oldal alján.

    ![Alkalmazások][3]

5. **Milyen feladatot szeretne tenni** párbeszédpanelen kattintson **a gyűjteményből az alkalmazások felvétele**.

    ![Alkalmazások][4]

6. A Keresés mezőbe írja be a **Tableau kiszolgáló**.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_01.png)

7. Az eredmény ablaktáblában jelölje be a **Kiszolgáló Tableau**, és válassza a **kész** , az alkalmazás hozzáadása parancsra.

    ![Az alkalmazás kijelöli a gyűjteményben](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Beállítása és tesztelése Azure Active Directory egyszeri bejelentkezés
Ez a szakasz célja mutatja, hogy beállítása és Azure Active Directory egyszeri bejelentkezés a "Britta Simon" nevű próba felhasználón alapuló Tableau kiszolgálóval tesztelése.

Az egyszeri bejelentkezési a munkát az Azure Active Directory kell, hogy mi az a megfelelőjük a felhasználó Tableau Server Azure AD-felhasználónak. Más szóval Azure AD-felhasználó és a kapcsolódó felhasználó Tableau kiszolgáló közötti kapcsolat kapcsolat kell létrehozni.

A hivatkozás kapcsolat jön létre a program a **felhasználónév** Tableau Server Azure AD az az érték, a **felhasználónév** hozzárendelésével.

Beállítása és tesztelése Azure AD az egyszeri bejelentkezés Tableau kiszolgálóval, a következő építőelemek szükséges:

1. **[Azure Active Directory konfigurálása az egyszeri bejelentkezés](#configuring-azure-ad-single-single-sign-on)** - engedélyezése a felhasználóknak, hogy ezzel a szolgáltatással.
2. **[Felhasználó létrehozása az Azure Active Directory tesztelése](#creating-an-azure-ad-test-user)** - Azure Active Directory egyszeri bejelentkezéssel való Britta Simon ellenőrzéséhez.
4. **[Tesztelje Tableau kiszolgáló létrehozása a felhasználó](#creating-a-tableauserver-test-user)** -, hogy egy partner a Britta Simon Tableau kiszolgáló, amelyen Azure Active Directory ábrázolása csatolt.
5. **[Az Azure Active Directory hozzárendelése tesztelje a felhasználó](#assigning-the-azure-ad-test-user)** - ahhoz, hogy Britta Simon Azure AD az egyszeri bejelentkezés használata.
5. **[Tesztelés egyszeri bejelentkezés](#testing-single-sign-on)** - ellenőrzéséhez, hogy működik-e a konfigurációs.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure Active Directory Single Sign-On konfigurálása

Ez a szakasz célja Azure AD az egyszeri bejelentkezés az Azure klasszikus portálon engedélyezése és a Tableau Server alkalmazásban az egyszeri bejelentkezés beállítása.

Tableau kiszolgáló alkalmazás a SAML Előfeltételek vár, egy bizonyos formátumban. Az alábbi képernyőképen látható, a. 

![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_51.png) 

**Azure Active Directory egyszeri bejelentkezés beállítása a Tableau kiszolgálóval, hajtsa végre az alábbi lépéseket:**


1. Az Azure klasszikus portálon **Tableau Server** alkalmazás integrációs lapján a menüben, a képernyő tetején kattintson a **attribútumok**elemre.

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-tableauserver-tutorial/tutorial_general_81.png) 


1. A **SAML jogkivonat attribútumok** párbeszédpanelen hajtsa végre az alábbi lépéseket:

    

    egy. Kattintson a **felhasználó attribútum hozzáadása** a **Felhasználó Attribure hozzáadása** párbeszédpanel megnyitásához.

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-tableauserver-tutorial/tutorial_general_82.png) 


    b. **Attrubute neve** mezőbe írja be **felhasználónevét**.

    c billentyűkombinációt. Az **Attribútumérték** listából selsect **user.displayname**.

    d. Kattintson a **kész**gombra.  
    



1. A felső sávon kattintson a **Quick Start**.

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-tableauserver-tutorial/tutorial_general_83.png)  








1. Kattintson a **Konfigurálás egyszeri bejelentkezés a** a **Konfigurálása az egyszeri bejelentkezés** párbeszédpanel megnyitásához.

    ![Egyszeri bejelentkezés beállítása][6] 



2. **Hogyan szeretné, hogy jelentkezzen be az Tableau kiszolgáló felhasználók** lapon válassza az **Azure Active Directory Single Sign-On**, és kattintson a **Tovább gombra**.

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_03.png) 


3. Az **Alkalmazás beállításainak megadása** párbeszédpanel lapon hajtsa végre az alábbi lépéseket, és kattintson a **Tovább**gombra:

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_04.png) 



    egy. **Bejelentkezés az URL-cím** mezőbe írja be a Tableau kiszolgáló URL-CÍMÉT. 

    b. Az azonosító mezőbe a Másolás a 

    c billentyűkombinációt. Kattintson a **Tovább** gombra


4. A **beállítás az egyszeri bejelentkezés Tableau kiszolgálón** lapon hajtsa végre az alábbi lépéseket, és kattintson a **Tovább**gombra:

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_05.png) 


    egy. Kattintson a **metaadat-alapú letöltése**, és mentse a fájlt a számítógépen.

    b. Kattintson a **Tovább**gombra.


6. Ha be van állítva az alkalmazás SSO, kell bejelentkezéses az Ön Tableau kiszolgáló bérlői rendszergazdaként.

    egy. A Tableau beállítása kattintson a **SAML** fülre.

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_001.png) 


    b. Jelölje be a jelölőnégyzetet, **Az egyszeri bejelentkezés használata SAML**.

    c billentyűkombinációt. Keresse meg a Azure klasszikus portáljáról letöltött összevonási metaadat-fájlt, és töltse fel a **SAML Idp metaadat-fájlt**.

    d. Tableau kiszolgáló vissza URL-cím – Tableau Server felhasználója hozzáférhet, például http://tableau_server az URL-CÍMÉT. Http://localhost használata nem ajánlott. URL-cím használata perjellel (például http://tableau_server/) nem támogatott. **Tableau kiszolgáló vissza URL-címet** másolja és illessze a szövegdoboz Azure Active Directory **Bejelentkezési a URL-CÍMÉT** , a 3 látható módon

    e. SAML entitás azonosító – a szervezet azonosítója egyedileg kell azonosítania a IdP Tableau Server telepítésének. A Tableau kiszolgáló URL-címe újra Itt adhatja, ha szeretné, de nem rendelkezik a Tableau kiszolgáló URL-címe lesz. Másolja a **SAML entitás azonosítója** , és illessze be az Azure Active Directory **MEGFELELTETVE** szövegdoboz a 3 látható módon.

    f. Kattintson a **Metaadat-fájl exportálása** , és nyissa meg a szöveg editor alkalmazásban. Keresse meg a állítás fogyasztói szolgáltatás URL-CÍMÉT a Http-bejegyzés és a tárgymutató-0, és másolja a vágólapra az URL-címet. Most már illessze be az Azure Active Directory- **Válasz URL-cím** szövegdoboz a 3 látható módon. 

    g. Kattintson **az OK gombra** a Tableau Server Configiuration lap gombra.

    > [AZURE.NOTE] Ha segítségre van szüksége konfigurálása a SAML Tableau kiszolgálón majd olvassa el ez a cikk [A SAML konfigurálása](http://onlinehelp.tableau.com/current/server/en-us/config_saml.htm) 

6. Az Azure klasszikus portálon válassza ki az egyszeri bejelentkezéses konfigurációs megerősítő, és kattintson a **Tovább gombra**.

    ![Azure Active Directory Single Sign-On][10]

7. Az **egyszeri bejelentkezés megerősítés** lapon kattintson a **Befejezés**gombra. 
 
    ![Azure Active Directory Single Sign-On][11]


### <a name="creating-an-azure-ad-test-user"></a>Azure AD-próba felhasználó létrehozása
Ez a szakasz célja próba felhasználó létrehozása az Azure-Britta Simon nevű klasszikus portálon.

A felhasználók listában jelölje ki a **Britta Simon**.

![Azure Active Directory-felhasználó létrehozása][20]

**Teszt felhasználó létrehozása az Azure Active Directory, hajtsa végre az alábbi lépéseket:**

1. Az **Azure klasszikus portálon**kattintson a bal oldali navigációs területen kattintson az **Active Directory**.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-tableauserver-tutorial/create_aaduser_09.png) 

2. A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3. A menüben, a képernyő tetején, a felhasználók listájának megjelenítéséhez kattintson a **felhasználók**elemre.
 
    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-tableauserver-tutorial/create_aaduser_03.png) 


4. **Felhasználó hozzáadása**elemre az alsó eszköztáron a **Felhasználó hozzáadása** párbeszédpanel megnyitásához.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-tableauserver-tutorial/create_aaduser_04.png)

5. A párbeszédpanel **közli velünk a felhasználóra vonatkozó** lapon hajtsa végre az alábbi lépéseket:

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-tableauserver-tutorial/create_aaduser_05.png) 

    egy. **Írja be a felhasználó**jelölje ki **a szervezet új felhasználót**.

    b. Az a **Felhasználónév** mezőbe írja be a **BrittaSimon**.

    c billentyűkombinációt. Kattintson a **Tovább**gombra.

6.  A **Felhasználói profil** párbeszédpanel lapon hajtsa végre az alábbi lépéseket:

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-tableauserver-tutorial/create_aaduser_06.png) 

    egy. Az **első neve** mezőbe írja be a **Britta**.  

    b. Az **Utolsó neve** mezőbe írja be, **Simon**.

    c billentyűkombinációt. A **Megjelenítendő név** mezőbe írja be a **Britta Simon**.

    d. A **szerepkör** listában jelölje ki a **felhasználót**.

    e. Kattintson a **Tovább**gombra.

7. A párbeszédpanel **ideiglenes jelszót kérjen** lapon kattintson a **Hozzon létre**.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-tableauserver-tutorial/create_aaduser_07.png) 


8. A párbeszédpanel **ideiglenes jelszót első** lapon hajtsa végre az alábbi lépéseket:
 
    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-tableauserver-tutorial/create_aaduser_08.png) 

    egy. Jegyezze fel az **Új jelszót**az érték.

    b. Kattintson a **kész**gombra.   



### <a name="creating-a-tableau-server-test-user"></a>Tableau kiszolgáló próba felhasználó létrehozása

Ez a szakasz célja Britta Simon nevű Tableau Server felhasználó létrehozása. Kiépítése Tableau Server minden felhasználónak van szükség. Tartsa szem előtt, hogy a felhasználó felhasználónév meg kell egyeznie az érték, amely az Azure Active Directory **felhasználónév**egyéni attribútuma konfigurálta. A helyes hozzárendeléssel integrációja [Konfigurálása Azure AD az egyszeri bejelentkezés](#configuring-azure-ad-single-single-sign-on)fog működni.

> [AZURE.NOTE] Ha egy felhasználó létrehozása manuálisan kell szüksége a szervezet a Tableau kiszolgáló rendszergazdájától.


### <a name="assigning-the-azure-ad-test-user"></a>Az Azure Active Directory-próba felhasználói hozzárendelése

Ez a szakasz célja Britta Simon használandó Azure egyszeri bejelentkezés a saját hozzáférést biztosít Tableau kiszolgáló engedélyezése.

![Felhasználó hozzárendelése][200] 

**Britta Simon hozzárendelése Tableau Server, hajtsa végre az alábbi lépéseket:**

1. Az Azure klasszikus portál kattintva nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson az **alkalmazások** parancsra a felső menüben.
 
    ![Felhasználó hozzárendelése][201] 

2. Az alkalmazások listájában jelölje ki a **Tableau kiszolgálót**.

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_50.png) 


1. A felső sávon kattintson a **felhasználók**elemre.

    ![Felhasználó hozzárendelése][203]

1. A felhasználók listában jelölje ki a **Britta Simon**.

2. Az alsó eszköztáron kattintson a **hozzárendelni**.

![Felhasználó hozzárendelése][205]



### <a name="testing-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

Ez a szakasz célja a Azure Active Directory egyszeri bejelentkezéses beállítások tesztelése a Access panelen.

Ha a kiszolgáló Tableau csempére az Access panel gombra kattint, meg kell első automatikusan aláírt-on az Tableau Server alkalmazás.


## <a name="additional-resources"></a>További források

* [A szoftver alkalmazások integrálása az Azure Active Directory oktatóanyagok listája](active-directory-saas-tutorial-list.md)
* [Mi az access alkalmazás és az egyszeri bejelentkezés az Azure Active Directory címtárral?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_205.png
