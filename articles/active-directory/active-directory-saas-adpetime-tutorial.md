<properties
    pageTitle="Oktatóprogram: Azure Active Directory-integráció a ADP eTime |} Microsoft Azure"
    description="Megtudhatja, hogy miként konfigurálása az egyszeri bejelentkezés Azure Active Directory és az Access-Adatprojektet eTime között."
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
    ms.date="10/17/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-adp-etime"></a>Oktatóprogram: Azure Active Directory-integráció a ADP eTime

Ebben az oktatóanyagban célja mutatja, hogy az Access-Adatprojektet eTime integrálása az Azure Active Directory (Azure Active Directory).  
Access-Adatprojektet eTime integrálása az Azure Active Directory biztosít a következő előnyökkel jár:

- Az Azure Active Directory Access-Adatprojektet eTime hozzáféréssel rendelkező személyek megadhatja, hogy
- Engedélyezheti a felhasználóknak, hogy automatikusan első jelentkezett-on az Access-Adatprojektet eTime (Single Sign-On) az Azure Active Directory-fiókok
- A fiókokat az egy központi helyen – az Azure klasszikus portálra


Ha többet szeretne tudni a szoftver alkalmazás integrálása az Azure AD meg, hogy, ismerje meg, [az access alkalmazás és az egyszeri bejelentkezés az Azure Active Directory címtárral](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek

Azure Active Directory integráció az Access-Adatprojektet eTime van szükség az alábbiakat:

- Az Azure Active Directory-előfizetéssel
- Egy Access-Adatprojektet eTime egyszeri bejelentkezés engedélyezett előfizetés


> [AZURE.NOTE] Az oktatóprogram lépéseit teszteléséhez nem használata ajánlott munkakörnyezetben.


Az oktatóprogram lépéseit teszteléséhez célszerű követnie az alábbi javaslatokat:

- Erre akkor szükség, ne használja a termelési környezetén.
- Ha nincs telepítve az Azure Active Directory próba környezetben, kattint egy hónap próba [Itt](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban célja lehetővé teszi az Azure Active Directory egyszeri bejelentkezés tesztkörnyezetben tesztelése.  
Az alkalmazási példát, ebben az oktatóanyagban tagolt két fő építőelemek áll:

1. Access-Adatprojektet eTime hozzáadása a gyűjteményből
2. Beállítása és tesztelése Azure Active Directory egyszeri bejelentkezés


## <a name="adding-adp-etime-from-the-gallery"></a>Access-Adatprojektet eTime hozzáadása a gyűjteményből
Adja meg az Access-Adatprojektet eTime integrálása Azure Active Directory, szüksége ADP eTime hozzáadása felügyelt szoftver alkalmazásai a gyűjteményből.

**A gyűjteményből ADP eTime felvételéhez hajtsa végre az alábbi lépéseket:**

1. Az **Azure klasszikus portálon**kattintson a bal oldali navigációs területen kattintson az **Active Directory**. 

    ![Az Active Directory][1]

2. A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3. Nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson a felső menüben **alkalmazásokat** .

    ![Alkalmazások][2]

4. Kattintson a **Hozzáadás** az oldal alján.

    ![Alkalmazások][3]

5. **Milyen feladatot szeretne tenni** párbeszédpanelen kattintson **a gyűjteményből az alkalmazások felvétele**.

    ![Alkalmazások][4]

6. A Keresés mezőbe írja be az **Access-Adatprojektet eTime**.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_01.png)

7. Az eredmény ablaktáblában jelölje ki az **Access-Adatprojektet eTime**, és válassza a **kész** , az alkalmazás hozzáadása parancsra.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_06.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Beállítása és tesztelése Azure Active Directory egyszeri bejelentkezés
Ez a szakasz célja mutatja, hogy beállítása és tesztelése Azure AD az egyszeri bejelentkezés, az Access-Adatprojektet eTime "Britta Simon" nevű próba felhasználón alapuló.

Az egyszeri bejelentkezési a munkát az Azure Active Directory kell, hogy mi az a partner felhasználó az Access-Adatprojektet eTime az Azure Active Directory-felhasználónak. Más szóval Azure AD-felhasználó, és a kapcsolódó felhasználó az Access-Adatprojektet eTime hivatkozás viszonya kell létrehozni.  
A hivatkozás kapcsolat jön létre által az Azure Active Directory Access-Adatprojektet eTime **felhasználónév** értékként a **felhasználó neve** értéket rendeli.

Állítsa be, és Azure Active Directory egyszeri bejelentkezés az Access-Adatprojektet eTime tesztelése, az alábbi építőelemeket szükséges:

1. **[Azure Active Directory konfigurálása az egyszeri bejelentkezés](#configuring-azure-ad-single-single-sign-on)** - engedélyezése a felhasználóknak, hogy ezzel a szolgáltatással.
2. **[Felhasználó létrehozása az Azure Active Directory tesztelése](#creating-an-azure-ad-test-user)** - Azure Active Directory egyszeri bejelentkezéssel való Britta Simon ellenőrzéséhez.
4. **[Egy Access-Adatprojektet eTime létrehozása tesztelje a felhasználó](#creating-a-adpetime-test-user)** -, hogy az Access-Adatprojektet eTime, amelyen az Azure Active Directory ábrázolt csatolt Britta Simon megfelelőjük szolgáltatásban.
5. **[Az Azure Active Directory hozzárendelése tesztelje a felhasználó](#assigning-the-azure-ad-test-user)** - ahhoz, hogy Britta Simon Azure AD az egyszeri bejelentkezés használata.
5. **[Tesztelés egyszeri bejelentkezés](#testing-single-sign-on)** - ellenőrzéséhez, hogy működik-e a konfigurációs.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure Active Directory Single Sign-On konfigurálása

Ez a szakasz célja Azure AD az egyszeri bejelentkezés az Azure klasszikus portálon engedélyezése és konfigurálása az egyszeri bejelentkezés, az Access-Adatprojektet eTime alkalmazásban.

Az Access-Adatprojektet eTime alkalmazás a SAML Előfeltételek vár, egy bizonyos formátumban, amelyhez egyéni attribútum megfeleltetések hozzáadása a SAML jogkivonat attribútumok konfigurációs. Az alábbi képernyőképen látható, a. A állítást neve mindig lesz **"PersonImmutableID"** és az érték, amelynek ExtensionAttribute2, amely tartalmazza az Alkalmazottkód oszlop a felhasználó azt van megfeleltetve. Itt az Access-Adatprojektet eTime felhasználói-megfeleltetés Előrehozás Azure AD meg az Alkalmazottkód történik, de képezhető Ez egy másik értékre a Alkalmazásbeállítások is alapján. Így adja meg munkahelyi ADP eTime csoporthoz először szeretne használni egy felhasználó a megfelelő azonosító, és az **"PersonImmutableID"** állítás értékkel.  

![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_02.png) 

Beállíthatja, hogy a SAML állítás, mielőtt kell a bérlő az a ADP eTime támogatási csoport munkatársaitól és az egyedi azonosító attribútum értékét. Ezt az értéket állítsa be az egyéni állítást az alkalmazáshoz szükséges.


**Azure Active Directory egyszeri bejelentkezés adja meg az Access-Adatprojektet eTime, hajtsa végre az alábbi lépéseket:**

1. Az Azure klasszikus portálon **ADP eTime** alkalmazás integrációs lapon kattintson **konfigurálása az egyszeri bejelentkezés** a **Konfigurálása az egyszeri bejelentkezés** párbeszédpanel megnyitásához.

    ![Egyszeri bejelentkezés beállítása][6] 

2. **Hogyan szeretné, hogy jelentkezzen be az Access-Adatprojektet eTime felhasználók** lapon válassza az **Azure Active Directory Single Sign-On**, és kattintson a **Tovább gombra**.

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_03.png) 

3. Az **Alkalmazás beállításainak megadása** párbeszédpanel lapon hajtsa végre az alábbi lépéseket:.

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_04.png) 


    egy. **Válasz URL-cím** mezőbe írja be a bejelentkezés az Access-Adatprojektet eTime alkalmazás a következő mintát használatával a felhasználók által használt URL-címe: `https://<server name>.adp.com/affwebservices/public/saml2assertionconsumer`.

    b. Kattintson a **Tovább**gombra.

4. A **Configure egyszeri bejelentkezés az Access-Adatprojektet eTime** lapon hajtsa végre az alábbi lépéseket:

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_05.png) 

    egy. Kattintson a **metaadat-alapú letöltése**, és mentse a fájlt a számítógépen.

    b. Kattintson a **Tovább**gombra.


5. Ha be van állítva az alkalmazás SSO, a ADP eTime támogatási csoport munkatársaitól, és e-mailben a letöltött csatolása metaadat-fájlt, hogy egyszeri bejelentkezési integrációs beállíthatók.

    > [AZURE.NOTE] **Access-Adatprojektet eTime** csoport konfigurálása a példány, miután **RelayState** érték beolvasása a velük szemben. Hajtsa végre az alábbi említett lépéseket követve adja meg. Ez a beállítás után a integrációt tesztelheti. Így Felhívjuk a figyelmét arra, hogy ez a beállítás fontos az Ez az alkalmazás integráció a munkát.

6. Állítsa be a RelayState értéket Azure Active Directory, hajtsa végre az alábbi lépéseket: 
    
    egy. Bejelentkezés az [Azure Kezelőportálja](https://portal.azure.com) rendszergazdaként.

    b. A bal oldali navigációs ablaktáblában kattintson a **További szolgáltatások**elemre. 
    
    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_07.png)

    c billentyűkombinációt. A **Keresés** mezőbe írja be az **Azure Active Directory**, és kattintson a kapcsolódó hivatkozásra.
    
    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_08.png)

    d. Kattintson a **vállalati alkalmazások**.

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_09.png)

    e. A **kezelés** csoportban kattintson az **Összes alkalmazás**.
    
    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_10.png)

    f. A **Keresés** mezőbe írja be az **Access-Adatprojektet eTime**, és kattintson a kapcsolódó hivatkozásra. 
    
    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_11.png)

    g. A **kezelés** csoportban kattintson **az egyszeri bejelentkezés**.

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_12.png)

    h. Válassza a **Megjelenítés speciális beállítások URL-CÍMÉT**.
    
    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_13.png)
    
    lehet. Az **Továbbítási állapotot** mezőbe írjon be egy értéket, az alábbi minta használatával:
    
    - Munkakörnyezetben:`https://fed.adp.com/saml/fedlanding.html?<id>` 
    - Fejlesztői környezet:`https://fed-stag.adp.com/saml/fedlanding.html?PORTAL`

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_14.png)

    j. A beállítások mentéséhez.

7. Az Azure klasszikus portálon válassza ki az egyszeri bejelentkezéses konfigurációs megerősítő, és kattintson a **Tovább gombra**.

    ![Azure Active Directory Single Sign-On][10]

8. Az **egyszeri bejelentkezés megerősítés** lapon kattintson a **Befejezés**gombra.  

    ![Azure Active Directory Single Sign-On][11]



### <a name="creating-an-azure-ad-test-user"></a>Azure AD-próba felhasználó létrehozása
Ez a szakasz célja vizsgálat felhasználó létrehozása az Azure-Britta Simon nevű klasszikus portálon.  
A felhasználók listában jelölje ki a **Britta Simon**.

![Azure Active Directory-felhasználó létrehozása][20]

**Teszt felhasználó létrehozása az Azure Active Directory, hajtsa végre az alábbi lépéseket:**

1. Az **Azure klasszikus portálon**kattintson a bal oldali navigációs területen kattintson az **Active Directory**.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-adpetime-tutorial/create_aaduser_09.png) 

2. A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3. A menüben, a képernyő tetején, a felhasználók listájának megjelenítéséhez kattintson a **felhasználók**elemre.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-adpetime-tutorial/create_aaduser_03.png) 

4. **Felhasználó hozzáadása**elemre az alsó eszköztáron a **Felhasználó hozzáadása** párbeszédpanel megnyitásához.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-adpetime-tutorial/create_aaduser_04.png) 

5. A párbeszédpanel **közli velünk a felhasználóra vonatkozó** lapon hajtsa végre az alábbi lépéseket:

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-adpetime-tutorial/create_aaduser_05.png) 

    egy. **Írja be a felhasználó**jelölje ki **a szervezet új felhasználót**.

    b. Az a **Felhasználónév** mezőbe írja be a **BrittaSimon**.

    c billentyűkombinációt. Kattintson a **Tovább**gombra.

6.  A **Felhasználói profil** párbeszédpanel lapon hajtsa végre az alábbi lépéseket:

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-adpetime-tutorial/create_aaduser_06.png) 

    egy. Az **első neve** mezőbe írja be a **Britta**.  

    b. Az **Utolsó neve** mezőbe írja be, **Simon**.

    c billentyűkombinációt. A **Megjelenítendő név** mezőbe írja be a **Britta Simon**.

    d. A **szerepkör** listában jelölje ki a **felhasználót**.

    e. Kattintson a **Tovább**gombra.

7. A párbeszédpanel **ideiglenes jelszót kérjen** lapon kattintson a **Hozzon létre**.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-adpetime-tutorial/create_aaduser_07.png) 

8. A párbeszédpanel **ideiglenes jelszót első** lapon hajtsa végre az alábbi lépéseket:

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-adpetime-tutorial/create_aaduser_08.png) 

    egy. Jegyezze fel az **Új jelszót**az érték.

    b. Kattintson a **kész**gombra.   



### <a name="creating-a-adp-etime-test-user"></a>Egy Access-Adatprojektet eTime teszt felhasználó létrehozása

Ez a szakasz célja hozzon létre egy Access-Adatprojektet eTime Britta Simon nevű felhasználót. Kérje meg a felhasználók felvétele az Access-Adatprojektet eTime fiók ADP eTime támogatási csoporthoz. 


> [AZURE.NOTE]Ha egy felhasználó létrehozása manuálisan kell szüksége az Access-Adatprojektet eTime támogatási csoport munkatársaitól kaphat.


### <a name="assigning-the-azure-ad-test-user"></a>Az Azure Active Directory-próba felhasználói hozzárendelése

Ez a szakasz célja Britta Simon Azure egyszeri bejelentkezés a saját ADP eTime hozzáférés engedélyezése használandó engedélyezése.

![Felhasználó hozzárendelése][200] 

**Access-Adatprojektet eTime Britta Simon rendelni, hajtsa végre az alábbi lépéseket:**

1. Az Azure klasszikus portál kattintva nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson az **alkalmazások** parancsra a felső menüben.

    ![Felhasználó hozzárendelése][201] 

2. Az alkalmazások listájában jelölje ki az **Access-Adatprojektet eTime**.

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_50.png) 

1. A felső sávon kattintson a **felhasználók**elemre.

    ![Felhasználó hozzárendelése][203] 

1. A felhasználók listában jelölje ki a **Britta Simon**.

2. Az alsó eszköztáron kattintson a **hozzárendelni**.

    ![Felhasználó hozzárendelése][205]



### <a name="testing-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

Ez a szakasz célja a Azure Active Directory egyszeri bejelentkezéses beállítások tesztelése a Access panelen.  
Ha az Access-Adatprojektet eTime csempe az Access panel gombra kattint, meg kell első automatikusan aláírt-on az Access-Adatprojektet eTime alkalmazás.


## <a name="additional-resources"></a>További források

* [A szoftver alkalmazások integrálása az Azure Active Directory oktatóanyagok listája](active-directory-saas-tutorial-list.md)
* [Mi az access alkalmazás és az egyszeri bejelentkezés az Azure Active Directory címtárral?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_205.png
