<properties
    pageTitle="Oktatóprogram: Azure Active Directory-integráció a GaggleAMP |} Microsoft Azure"
    description="Megtudhatja, hogy miként konfigurálása az egyszeri bejelentkezés Azure Active Directory és GaggleAMP között."
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
    ms.date="10/10/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-gaggleamp"></a>Oktatóprogram: Azure Active Directory-integráció a GaggleAMP

Ebben az oktatóanyagban célja mutatja, hogy miként GaggleAMP integrálása az Azure Active Directory (Azure Active Directory).

GaggleAMP integrálása az Azure Active Directory biztosít a következő előnyökkel jár:

- Az Azure Active Directory, aki rendelkezik hozzáféréssel GaggleAMP megadhatja, hogy
- Engedélyezheti a felhasználóknak, hogy automatikusan első jelentkezett-on való GaggleAMP (Single Sign-On) az Azure Active Directory-fiókok
- A fiókokat az egy központi helyen – az Azure klasszikus portálra 

Ha többet szeretne tudni a szoftver alkalmazás integrálása az Azure AD meg, hogy, ismerje meg, [az access alkalmazás és az egyszeri bejelentkezés az Azure Active Directory címtárral](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek

Azure Active Directory integráció a GaggleAMP van szükség az alábbiakat:

- Az Azure Active Directory-előfizetéssel
- Egy GaggleAMP egyszeri bejelentkezés engedélyezett előfizetés


> [AZURE.NOTE] Az oktatóprogram lépéseit teszteléséhez nem használata ajánlott munkakörnyezetben.


Az oktatóprogram lépéseit teszteléséhez célszerű követnie az alábbi javaslatokat:

- Erre akkor szükség, ne használja a termelési környezetén.
- Ha nincs telepítve az Azure Active Directory próba környezetben, kattint egy hónap próba [Itt](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban célja lehetővé teszi az Azure Active Directory egyszeri bejelentkezés tesztkörnyezetben tesztelése. 

Az alkalmazási példát, ebben az oktatóanyagban tagolt két fő építőelemek áll:

1. GaggleAMP hozzáadása a gyűjteményből
2. Beállítása és tesztelése Azure Active Directory egyszeri bejelentkezés


## <a name="adding-gaggleamp-from-the-gallery"></a>GaggleAMP hozzáadása a gyűjteményből

Konfigurálja az Azure Active Directory integrálása a GaggleAMP, meg kell GaggleAMP hozzáadása az felügyelt szoftver alkalmazáslistában a gyűjteményből.

**A gyűjteményből GaggleAMP felvételéhez hajtsa végre az alábbi lépéseket:**

1. Az **Azure klasszikus portálon**kattintson a bal oldali navigációs területen kattintson az **Active Directory**. 

    ![Az Active Directory][1]

2. A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3. Nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson a felső menüben **alkalmazásokat** .

    ![Alkalmazások][2]

4. Kattintson a **Hozzáadás** az oldal alján.

    ![Alkalmazások][3]

5. **Milyen feladatot szeretne tenni** párbeszédpanelen kattintson **a gyűjteményből az alkalmazások felvétele**.

    ![Alkalmazások][4]

6. A Keresés mezőbe írja be a **GaggleAMP**.
    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-gaggleamp-tutorial/tutorial_gaggleamp_01.png)

7. Az eredmény ablaktáblában jelölje ki a **GaggleAMP**, és válassza a **kész** , az alkalmazás hozzáadása gombra.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-gaggleamp-tutorial/tutorial_gaggleamp_02.png)>

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Beállítása és tesztelése Azure Active Directory egyszeri bejelentkezés
Ez a szakasz célja mutatja, hogy állítsa be, és a "Britta Simon" nevű próba felhasználón alapuló GaggleAMP Azure AD az egyszeri bejelentkezés tesztelése.

Az egyszeri bejelentkezési a munkát az Azure Active Directory kell a GaggleAMP megfelelőjük a felhasználó egy felhasználónak, hogy az Azure Active Directory. Más szóval Azure AD-felhasználó, és a kapcsolódó felhasználó GaggleAMP hivatkozás viszonya kell létrehozni.

A hivatkozás kapcsolat jön létre által az Azure Active Directory GaggleAMP **felhasználónév** értékként a **felhasználó neve** értéket rendeli.

Állítsa be, és Azure Active Directory egyszeri bejelentkezéssel való GaggleAMP tesztelése, az alábbi építőelemeket szükséges:

1. **[Azure Active Directory konfigurálása az egyszeri bejelentkezés](#configuring-azure-ad-single-single-sign-on)** - engedélyezése a felhasználóknak, hogy ezzel a szolgáltatással.
2. **[Felhasználó létrehozása az Azure Active Directory tesztelése](#creating-an-azure-ad-test-user)** - Azure Active Directory egyszeri bejelentkezéssel való Britta Simon ellenőrzéséhez.
4. **[Felhasználó létrehozása egy GaggleAMP tesztelése](#creating-a-GaggleAMP-test-user)** - van-e egy megfelelője a Britta Simon GaggleAMP, amelyen az Azure Active Directory ábrázolt csatolt.
5. **[Az Azure Active Directory hozzárendelése tesztelje a felhasználó](#assigning-the-azure-ad-test-user)** - ahhoz, hogy Britta Simon Azure AD az egyszeri bejelentkezés használata.
5. **[Tesztelés egyszeri bejelentkezés](#testing-single-sign-on)** - ellenőrzéséhez, hogy működik-e a konfigurációs.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure Active Directory egyszeri bejelentkezés beállítása

Ez a szakasz célja Azure AD az egyszeri bejelentkezés az Azure klasszikus portálon engedélyezése és a GaggleAMP alkalmazásban az egyszeri bejelentkezés beállítása.



**Állítson be Azure AD az egyszeri bejelentkezés GaggleAMP, hajtsa végre az alábbi lépéseket:**

1. Az Azure klasszikus portálon **GaggleAMP** alkalmazás integrációs lapon kattintson **konfigurálása az egyszeri bejelentkezés** a **Konfigurálása az egyszeri bejelentkezés** párbeszédpanel megnyitásához.

    ![Egyszeri bejelentkezés beállítása][6] 

2. **Hogyan szeretné, hogy jelentkezzen be az GaggleAMP felhasználók** lapon válassza az **Azure Active Directory Single Sign-On**, és kattintson a **Tovább gombra**.
 
    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-gaggleamp-tutorial/tutorial_gaggleamp_03.png) 

3. Az **Alkalmazás beállításainak megadása** párbeszédpanel lapon hajtsa végre az alábbi lépéseket:

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-gaggleamp-tutorial/tutorial_gaggleamp_04.png) 


    egy. Bejelentkezési a URL-cím mezőbe írja be a bejelentkezés az GaggleAMP alkalmazás a következő mintát használatával a felhasználók által használt URL-címe: **"https://secure4.gaggleamp.com"**.

    > [AZURE.NOTE]Kérjük, forduljon az [GaggleAMP értékesítési csoport](mailto:sales@gaggleamp.com) **Bejelentkezési a URL-CÍMÉT** az alkalmazás igény."

    b. Kattintson a **Tovább**gombra.


4. A **beállítás az egyszeri bejelentkezés GaggleAMP a** lapon hajtsa végre az alábbi lépéseket:

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-gaggleamp-tutorial/tutorial_gaggleamp_05.png) 

    egy. A **Tanúsítvány letöltése**gombra, és mentse a fájlt a számítógépen. A tanúsítvány és a metaadat-alapú URL-ek (entitás azonosítója, Egyszeri bejelentkezési az URL-CÍMÉT és a bejelentkezési meg URL-CÍMÉT) egyszeri bejelentkezés beállítása a GaggleAMP oldalon lesz szükség.

    b. Kattintson a **Tovább**gombra.


5. Másik böngésző példány, nyissa meg a SAML-féle egyszeri Bejelentkezést lapot, az Ön által a Gaggle támogatási csoportjától létrehozott (pl.: *https://accounts.gaggleamp.com/saml_configurations/oXH8sQcP79dOzgFPqrMTyw/edit*).

6. A **SAML-féle egyszeri Bejelentkezést** lapon hajtsa végre az alábbi lépéseket:  
   
    ![GaggleAMP Single Sign-On](./media/active-directory-saas-gaggleamp-tutorial/tutorial_gaggleamp_06.png) 

    egy. A klasszikus Azure portálon **Kibocsátó URL-cím**másolása, és illessze be a **Identitás szolgáltató kibocsátó** mezőben lévő értéket. 

    b. A klasszikus Azure portálon **Egyszeri bejelentkezéses szolgáltatás URL-cím**másolása, és illessze be a **Identitás szolgáltató egyszeri bejelentkezéses URL-címe** mezőben lévő értéket. 

    c billentyűkombinációt. Kattintson a **Mentés** gombra.      
    
    d. A letöltött tanúsítvány küldése a [GaggleAMP értékesítési csoport](mailto:sales@gaggleamp.com).


6. Az Azure klasszikus portálon válassza ki az egyszeri bejelentkezéses konfigurációs megerősítő, és kattintson a **Tovább gombra**.

    ![Azure Active Directory Single Sign-On][10]

7. Az **egyszeri bejelentkezés megerősítés** lapon kattintson a **Befejezés**gombra.  
  
    ![Azure Active Directory Single Sign-On][11]




### <a name="creating-an-azure-ad-test-user"></a>Azure AD-próba felhasználó létrehozása
Ez a szakasz célja vizsgálat felhasználó létrehozása az Azure-Britta Simon nevű klasszikus portálon.

![Azure Active Directory-felhasználó létrehozása][20]

**Teszt felhasználó létrehozása az Azure Active Directory, hajtsa végre az alábbi lépéseket:**

1. Az **Azure klasszikus portálon**kattintson a bal oldali navigációs területen kattintson az **Active Directory**.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-gaggleamp-tutorial/create_aaduser_09.png) 

2. A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3. A menüben, a képernyő tetején, a felhasználók listájának megjelenítéséhez kattintson a **felhasználók**elemre.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-gaggleamp-tutorial/create_aaduser_03.png) 

4. **Felhasználó hozzáadása**elemre az alsó eszköztáron a **Felhasználó hozzáadása** párbeszédpanel megnyitásához.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-gaggleamp-tutorial/create_aaduser_04.png) 

5. A párbeszédpanel **közli velünk a felhasználóra vonatkozó** lapon hajtsa végre az alábbi lépéseket:

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-gaggleamp-tutorial/create_aaduser_05.png) 

    egy. Felhasználó típusa jelölje be az új felhasználó a szervezet.

    b. A felhasználónév **szövegmezőbe**írja be a **BrittaSimon**.

    c billentyűkombinációt. Kattintson a **Tovább**gombra.

6.  A **Felhasználói profil** párbeszédpanel lapon hajtsa végre az alábbi lépéseket:

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-gaggleamp-tutorial/create_aaduser_06.png) 

    egy. Az **első neve** mezőbe írja be a **Britta**.  

    b. Az **Utolsó neve** mezőbe írja be, **Simon**.

    c billentyűkombinációt. A **Megjelenítendő név** mezőbe írja be a **Britta Simon**.

    d. A **szerepkör** listában jelölje ki a **felhasználót**.

    e. Kattintson a **Tovább**gombra.

7. A párbeszédpanel **ideiglenes jelszót kérjen** lapon kattintson a **Hozzon létre**.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-gaggleamp-tutorial/create_aaduser_07.png) 

8. A párbeszédpanel **ideiglenes jelszót első** lapon hajtsa végre az alábbi lépéseket:

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-gaggleamp-tutorial/create_aaduser_08.png) 

    egy. Jegyezze fel az **Új jelszót**az érték.

    b. Kattintson a **kész**gombra.   



### <a name="creating-a-gaggleamp-test-user"></a>GaggleAMP próba felhasználó létrehozása

Ez a szakasz célja Britta Simon nevű GaggleAMP felhasználó létrehozása. GaggleAMP támogatja a kiépítési közvetlenül az idő, amely alapértelmezés szerint ki van engedélyezve van.

Nincs teendő ebben a szakaszban a meg nem. Új felhasználóként GaggleAMP eléréséhez, ha még nem létezik kísérlet során jön létre. 


### <a name="assigning-the-azure-ad-test-user"></a>Az Azure Active Directory-próba felhasználói hozzárendelése

Ez a szakasz célja Britta Simon Azure egyszeri bejelentkezés a saját hozzáférést biztosít GaggleAMP használandó engedélyezése.

![Felhasználó hozzárendelése][200] 

**Britta Simon hozzárendelése GaggleAMP, hajtsa végre az alábbi lépéseket:**

1. Az Azure portál kattintva nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson az **alkalmazások** parancsra a felső menüben.

    ![Felhasználó hozzárendelése][201] 

2. Az alkalmazások listájában jelölje ki a **GaggleAMP**.
    ![Azure lista](./media/active-directory-saas-gaggleamp-tutorial/tutorial_gaggleamp_50.png)

1. A felső sávon kattintson a **felhasználók**elemre.

    ![Felhasználó hozzárendelése][203] 

1. A felhasználók listában jelölje ki a **Britta Simon**.

2. Az alsó eszköztáron kattintson a **hozzárendelni**.

    ![Felhasználó hozzárendelése][205]



### <a name="testing-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

Ez a szakasz célja a Azure Active Directory egyszeri bejelentkezéses beállítások tesztelése a Access panelen.

Ha a GaggleAMP csempére az Access panel gombra kattint, meg kell kérjen automatikusan aláírt-on az GaggleAMP alkalmazás.


## <a name="additional-resources"></a>További források

* [A szoftver alkalmazások integrálása az Azure Active Directory oktatóanyagok listája](active-directory-saas-tutorial-list.md)
* [Mi az access alkalmazás és az egyszeri bejelentkezés az Azure Active Directory címtárral?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-gaggleamp-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-gaggleamp-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-gaggleamp-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-gaggleamp-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-gaggleamp-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-gaggleamp-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-gaggleamp-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-gaggleamp-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-gaggleamp-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-gaggleamp-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-gaggleamp-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-gaggleamp-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-gaggleamp-tutorial/tutorial_general_205.png
