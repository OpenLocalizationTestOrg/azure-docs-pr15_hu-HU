<properties
    pageTitle="Oktatóprogram: Azure Active Directory-integráció a nevezetesen |} Microsoft Azure"
    description="Egyszeri bejelentkezés közötti Azure Active Directory konfigurálásának ismertetése, nevezetesen."
    services="active-directory"
    documentationCenter=""
    authors="jeevansd"
    manager="prasannas"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/20/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-namely"></a>Oktatóprogram: Azure Active Directory-integráció a nevezetesen

Ebben az oktatóanyagban célja mutatja, hogy miként nevezetesen integrálása az Azure Active Directory (Azure Active Directory).

Nevezetesen integrálása az Azure Active Directory biztosít a következő előnyökkel jár: 

- Az Azure Active Directory, aki rendelkezik a hozzáférést nevezetesen megadhatja, hogy 
- Engedélyezheti a felhasználóknak, hogy automatikusan megkapja nevezetesen jelentkezett-on való (Single Sign-On) az Azure Active Directory-fiókok
- A fiókokat az egy központi helyen – az Azure klasszikus portálra

Ha többet szeretne tudni a szoftver alkalmazás integrálása az Azure AD meg, hogy, ismerje meg, [az access alkalmazás és az egyszeri bejelentkezés az Azure Active Directory címtárral](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek 

Állítsa be az Azure Active Directory-integráció a nevezetesen, akkor a következő elemek van szükség:

- Az Azure Active Directory-előfizetéssel
- Egy nevezetesen egyszeri bejelentkezés engedélyezett előfizetésekből


> [AZURE.NOTE] Az oktatóprogram lépéseit teszteléséhez nem használata ajánlott munkakörnyezetben.


Az oktatóprogram lépéseit teszteléséhez célszerű követnie az alábbi javaslatokat:

- Erre akkor szükség, ne használja a termelési környezetén.
- Ha nincs telepítve az Azure Active Directory próba környezetben, kattint egy hónap próba [Itt](https://azure.microsoft.com/pricing/free-trial/). 

 
## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban célja lehetővé teszi az Azure Active Directory egyszeri bejelentkezés tesztkörnyezetben tesztelése. 

Az alkalmazási példát, ebben az oktatóanyagban tagolt két fő építőelemek áll:

1. Hozzáadás, azaz a gyűjteményből 
2. Beállítása és tesztelése Azure Active Directory egyszeri bejelentkezés


## <a name="adding-namely-from-the-gallery"></a>Hozzáadás, azaz a gyűjteményből
Adja meg a integrálása a nevezetesen Azure Active Directory, szüksége hozzáadása nevezetesen a gyűjteményből az felügyelt szoftver alkalmazáslistát.

**Gyűjtemény használatával adhat hozzá nevezetesen a, hajtsa végre az alábbi lépéseket:**

1. Az **Azure klasszikus portálon**kattintson a bal oldali navigációs területen kattintson az **Active Directory**. 

    ![Az Active Directory][1]

2. A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3. Nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson a felső menüben **alkalmazások** .

    ![Alkalmazások][2]

4. Kattintson a **Hozzáadás** az oldal alján.

    ![Alkalmazások][3]

5. **Milyen feladatot szeretne tenni** párbeszédpanelen kattintson **a gyűjteményből az alkalmazások felvétele**.

    ![Alkalmazások][4]

6. A Keresés mezőbe írja be a **nevezetesen**.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-namely-tutorial/tutorial_namely_01.png)

7. Az eredmény ablaktáblában jelölje ki a **nevezetesen**pontra, és kattintson a **kész** az alkalmazás hozzáadása parancsra.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-namely-tutorial/tutorial_namely_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Beállítása és tesztelése Azure Active Directory egyszeri bejelentkezés
Ez a szakasz célja mutatja, hogy beállítása és tesztelése Azure AD az egyszeri bejelentkezés, azaz "Britta Simon" nevű vizsgálat felhasználón alapuló.

Az egyszeri bejelentkezési a munkát az Azure Active Directory kell, hogy mi az a partner felhasználó nevezetesen el az Azure Active Directory-felhasználók. Más szóval Azure AD-felhasználó, és a kapcsolódó felhasználó hivatkozás viszonya nevezetesen állapítani kell.

Ez a hivatkozás kapcsolat hozzárendelésével a értéket a **felhasználó nevét** a **felhasználónév** , a program Azure Active Directory nevezetesen jön létre.
 
Állítsa be és tesztelje az Azure Active Directory egyszeri bejelentkezés nevezetesen Önnel kell fejezze be a következő építőelemek:

1. **[Azure Active Directory konfigurálása az egyszeri bejelentkezés](#configuring-azure-ad-single-single-sign-on)** - engedélyezése a felhasználóknak, hogy ezzel a szolgáltatással.
2. **[Felhasználó létrehozása az Azure Active Directory tesztelése](#creating-an-azure-ad-test-user)** - Azure Active Directory egyszeri bejelentkezéssel való Britta Simon ellenőrzéséhez.
4. **[Létrehozása egy felhasználó nevezetesen tesztelése](#creating-a-namely-test-user)** - szeretné, hogy egy partner a Britta Simon a nevezetesen, hogy kapcsolódik az Azure Active Directory ábrázolása amelyen.
5. **[Az Azure Active Directory hozzárendelése tesztelje a felhasználó](#assigning-the-azure-ad-test-user)** - ahhoz, hogy Britta Simon Azure AD az egyszeri bejelentkezés használata.
5. **[Tesztelés egyszeri bejelentkezés](#testing-single-sign-on)** - ellenőrzéséhez, hogy működik-e a konfigurációs.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure Active Directory Single Sign-On konfigurálása

Ez a szakasz célja Azure AD az egyszeri bejelentkezés az Azure klasszikus portálon engedélyezése és beállítása az egyszeri bejelentkezés a nevezetesen alkalmazást. 




**Állítsa be az Azure Active Directory egyszeri bejelentkezéssel való nevezetesen, végezze el az alábbi lépéseket:**

1. Az Azure klasszikus portálon **nevezetesen** alkalmazás integrációs lapon kattintson **konfigurálása az egyszeri bejelentkezés** a **Konfigurálása az egyszeri bejelentkezés** párbeszédpanel megnyitásához.

    ![Egyszeri bejelentkezés beállítása][6] 

2. **Hogyan szeretné, hogy jelentkezzen be a nevezetesen a felhasználók** lapon válassza az **Azure Active Directory Single Sign-On**, és kattintson a **Tovább gombra**.
 
    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-namely-tutorial/tutorial_namely_03.png) 

3. Az **Alkalmazás beállításainak megadása** párbeszédpanel lapon hajtsa végre az alábbi lépéseket:.

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-namely-tutorial/tutorial_namely_04.png) 

    egy. A **Bejelentkezési a URL-cím** mezőbe írja be a bejelentkezni a nevezetesen alkalmazás a felhasználók által használt URL-CÍMÉT (például: *https://fabrikam.Namely.com/*).

    b. Kattintson a **Tovább**gombra.
 
 
4. **Egyszeri bejelentkezés, azaz konfigurálása** lapon hajtsa végre az alábbi lépéseket:

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-namely-tutorial/tutorial_namely_05.png) 

    egy. Kattintson a **tanúsítvány letöltése**, és mentse a fájlt a számítógépen.

    b. Kattintson a **Tovább**gombra.


1. Egy másik böngészőablakban, jelentkezzen be a nevezetesen vállalati webhely-rendszergazdaként.

1. Kattintson az eszköztáron a képernyő tetején kattintson a **vállalat**.

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-namely-tutorial/tutorial_namely_06.png) 

1. Kattintson a **Beállítások** fülre.

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-namely-tutorial/tutorial_namely_07.png) 


1. Kattintson a **SAML**.

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-namely-tutorial/tutorial_namely_08.png) 


1. A **SAML-beállítások** lapon hajtsa végre az alábbi lépéseket:

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-namely-tutorial/tutorial_namely_09.png) 

    egy. Kattintson a **SAML engedélyezése**gombra. 

    b. Az Azure klasszikus portálon párbeszédpanel **egyszeri bejelentkezés, azaz konfigurálása** lapon másolja az **Egyszeri bejelentkezéses szolgáltatás URL-címe** értéket, és illessze be a **identitás szolgáltató DDO URL-címe** mezőben lévő értéket. 

    c billentyűkombinációt. Nyissa meg a letöltött tanúsítvány a Jegyzettömbben, a tartalom másolása és beillesztése a **identitás szolgáltató tanúsítványának** mezőben lévő értéket.    

    d. Kattintson a **Mentés**gombra.


6. Az Azure klasszikus portálon válassza ki az egyszeri bejelentkezéses konfigurációs megerősítő, és kattintson a **Tovább gombra**. 

    ![Azure Active Directory Single Sign-On][10]

7. Az **egyszeri bejelentkezés megerősítés** lapon kattintson a **Befejezés**gombra.  

    ![Azure Active Directory Single Sign-On][11]




### <a name="creating-an-azure-ad-test-user"></a>Azure AD-próba felhasználó létrehozása
Ez a szakasz célja próba felhasználó létrehozása az Azure-Britta Simon nevű klasszikus portálon.

![Azure Active Directory-felhasználó létrehozása][20]


**Teszt felhasználó létrehozása az Azure Active Directory, hajtsa végre az alábbi lépéseket:**

1. Az **Azure klasszikus portálon**kattintson a bal oldali navigációs területen kattintson az **Active Directory**.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-namely-tutorial/create_aaduser_09.png)  

2. A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3. A menüben, a képernyő tetején, a felhasználók listájának megjelenítéséhez kattintson a **felhasználók**elemre.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-namely-tutorial/create_aaduser_03.png) 
 
4. **Felhasználó hozzáadása**elemre az alsó eszköztáron a **Felhasználó hozzáadása** párbeszédpanel megnyitásához. 

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-namely-tutorial/create_aaduser_04.png) 

5. A párbeszédpanel **közli velünk a felhasználóra vonatkozó** lapon hajtsa végre az alábbi lépéseket: 

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-namely-tutorial/create_aaduser_05.png)  

    egy. Felhasználó típusa jelölje be az új felhasználó a szervezet.

    b. A felhasználónév **szövegmezőbe**írja be a **BrittaSimon**.

    c billentyűkombinációt. Kattintson a **Tovább**gombra.

6.  A **Felhasználói profil** párbeszédpanel lapon hajtsa végre az alábbi lépéseket: 

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-namely-tutorial/create_aaduser_06.png) 
 
    egy. Az **első neve** mezőbe írja be a **Britta**.  

    b. Az **Utolsó neve** mezőbe írja be, **Simon**.

    c billentyűkombinációt. A **Megjelenítendő név** mezőbe írja be a **Britta Simon**.

    d. A **szerepkör** listában jelölje ki a **felhasználót**.
    e. Kattintson a **Tovább**gombra.

7. A párbeszédpanel **ideiglenes jelszót első** lapon kattintson a **Hozzon létre**.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-namely-tutorial/create_aaduser_07.png) 
 
8. A párbeszédpanel **ideiglenes jelszót első** lapon hajtsa végre az alábbi lépéseket:

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-namely-tutorial/create_aaduser_08.png) 
  
    egy. Jegyezze fel az **Új jelszót**az érték.

    b. Kattintson a **kész**gombra.   

  
 
### <a name="creating-a-namely-test-user"></a>Létrehozása egy felhasználó nevezetesen tesztelése

Ez a szakasz célja nevezetesen Britta Simon nevű felhasználó létrehozása.

**Hozzon létre egy felhasználót, azaz Britta Simon nevű, hajtsa végre az alábbi lépéseket:**

1. Bejelentkezés a a nevezetesen vállalati webhely-rendszergazdaként.

1. Az eszköztár a képernyő tetején kattintson a **személyek**elemre.

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-namely-tutorial/tutorial_namely_10.png) 

1. Kattintson a **címtár** fülre.

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-namely-tutorial/tutorial_namely_11.png) 

1. Hozzáadás **új személyt**.



1. **Új személy hozzáadása** párbeszédpanelen hajtsa végre az alábbi lépéseket:

    egy. Az **első neve** mezőbe írja be a **Britta**.

    b. Az **utolsó neve** mezőbe írja be a **Simon**.

    c billentyűkombinációt. Az **E-mail** mezőbe írja be a Britta meg e-mail címét az Azure klasszikus portálon.

    d. Kattintson a **Mentés**gombra.





### <a name="assigning-the-azure-ad-test-user"></a>Az Azure Active Directory-próba felhasználói hozzárendelése

Ez a szakasz célja Britta Simon Azure egyszeri bejelentkezés a saját hozzáférés engedélyezése nevezetesen használandó engedélyezése.

![Felhasználó hozzárendelése][200] 

**Hozzárendelése Britta Simon nevezetesen végre az alábbi lépéseket:**

1. Az Azure klasszikus portál kattintva nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson az **alkalmazások** parancsra a felső menüben.

    ![Felhasználó hozzárendelése][201] 

2. Az alkalmazások listájában válassza a **nevezetesen**.

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-namely-tutorial/tutorial_namely_50.png) 

1. A felső sávon kattintson a **felhasználók**elemre.

    ![Felhasználó hozzárendelése][203] 

1. A felhasználók listában jelölje ki a **Britta Simon**.

2. Az alsó eszköztáron kattintson a **hozzárendelni**.

    ![Felhasználó hozzárendelése][205]



### <a name="testing-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

Ez a szakasz célja a Azure Active Directory egyszeri bejelentkezéses beállítások tesztelése a Access panelen.

Ha a gombra kattint a nevezetesen csempe az Access panel, meg kell kérjen automatikusan aláírt-on való a nevezetesen alkalmazás.


## <a name="additional-resources"></a>További források

* [A szoftver alkalmazások integrálása az Azure Active Directory oktatóanyagok listája](active-directory-saas-tutorial-list.md)
* [Mi az access alkalmazás és az egyszeri bejelentkezés az Azure Active Directory címtárral?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-namely-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-namely-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-namely-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-namely-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-namely-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-namely-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-namely-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-namely-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-namely-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-namely-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-namely-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-namely-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-namely-tutorial/tutorial_general_205.png






