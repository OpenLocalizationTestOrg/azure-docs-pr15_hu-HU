<properties
    pageTitle="Oktatóprogram: Azure Active Directory-integráció halogén szoftverrel"
    description="Megtudhatja, hogy miként konfigurálása az egyszeri bejelentkezés Azure Active Directory és a halogén szoftver között."
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


# <a name="tutorial-azure-active-directory-integration-with-halogen-software"></a>Oktatóprogram: Azure Active Directory-integráció a halogén szoftver

Ebben az oktatóanyagban célja mutatja, hogy miként halogén szoftver integrálása az Azure Active Directory (Azure Active Directory).

Halogén szoftver integrálása az Azure Active Directory biztosít a következő előnyökkel jár: 

- Az Azure Active Directory halogén szoftver hozzáféréssel rendelkező személyek megadhatja, hogy 
- Az Azure Active Directory-fiókok engedélyezheti a felhasználóknak, hogy automatikusan első jelentkezett-on halogén szoftver (Single Sign-On)
- A fiókokat az egy központi helyen – az Azure klasszikus portálra

Ha többet szeretne tudni a szoftver alkalmazás integrálása az Azure AD meg, hogy, ismerje meg, [az access alkalmazás és az egyszeri bejelentkezés az Azure Active Directory címtárral](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek 

Azure Active Directory integráció halogén szoftverrel van szükség az alábbiakat:

- Az Azure Active Directory-előfizetéssel
- A szoftver halogén egyszeri bejelentkezés engedélyezett előfizetésekből


> [AZURE.NOTE] Az oktatóprogram lépéseit teszteléséhez nem használata ajánlott munkakörnyezetben.


Az oktatóprogram lépéseit teszteléséhez célszerű követnie az alábbi javaslatokat:

- Erre akkor szükség, ne használja a termelési környezetén.
- Ha nincs telepítve az Azure Active Directory próba környezetben, kattint egy hónap próba [Itt](https://azure.microsoft.com/pricing/free-trial/). 

 
## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban célja lehetővé teszi az Azure Active Directory egyszeri bejelentkezés tesztkörnyezetben tesztelése. 

Az alkalmazási példát, ebben az oktatóanyagban tagolt két fő építőelemek áll:

1. Halogén szoftver hozzáadása a gyűjteményből 
2. Beállítása és tesztelése Azure Active Directory egyszeri bejelentkezés


## <a name="adding-halogen-software-from-the-gallery"></a>Halogén szoftver hozzáadása a gyűjteményből
Azure Active Directory integrálása halogén szoftver konfigurálásához szeretne halogén szoftver hozzáadása felügyelt szoftver alkalmazásai a gyűjteményből.

**A gyűjteményből halogén szoftver felvételéhez hajtsa végre az alábbi lépéseket:**

1. Az **Azure klasszikus portálon**kattintson a bal oldali navigációs területen kattintson az **Active Directory**. 

    ![Az Active Directory][1]

2. A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3. Nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson a felső menüben **alkalmazásokat** .

    ![Alkalmazások][2]

4. Kattintson a **Hozzáadás** az oldal alján. 

    ![Alkalmazások][3]

5. **Milyen feladatot szeretne tenni** párbeszédpanelen kattintson **a gyűjteményből az alkalmazások felvétele**.

    ![Alkalmazások][4]

6. A Keresés mezőbe írja be a **szoftver halogén**.

    ![Alkalmazások][5]

7. Az eredmény ablaktáblában jelölje ki a **Halogén szoftver**, és válassza a **kész** , az alkalmazás hozzáadása gombra.



##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Beállítása és tesztelése Azure Active Directory egyszeri bejelentkezés
Ez a szakasz célja mutatja, hogy beállítása és Azure Active Directory egyszeri bejelentkezés a "Britta Simon" nevű vizsgálat felhasználón alapuló halogén szoftverrel tesztelése.

Az egyszeri bejelentkezési a munkát az Azure Active Directory kell tudható, hogy mi az Azure Active Directory-felhasználónak halogén szoftverben megfelelőjük a felhasználó. Más szóval Azure AD-felhasználó, és a kapcsolódó felhasználó halogén szoftverben hivatkozás viszonya kell létrehozni.

Ez a hivatkozás kapcsolat Azure AD a **felhasználónév** halogén szoftverben a program az az érték, a **felhasználónév** hozzárendelésével jön létre.
 
Állítsa be, és Azure Active Directory egyszeri bejelentkezés halogén szoftverrel tesztelése, a következő építőelemek szükséges:

1. **[Azure Active Directory beállítása egyetlen egyszeri bejelentkezés](#configuring-azure-ad-single-single-sign-on)** - engedélyezése a felhasználóknak, hogy ezzel a szolgáltatással.
2. **[Felhasználó létrehozása az Azure Active Directory tesztelése](#creating-an-azure-ad-test-user)** - Azure Active Directory egyszeri bejelentkezéssel való Britta Simon ellenőrzéséhez.
4. **[Felhasználó létrehozása egy halogén szoftver tesztelése](#creating-a-halogen-software-test-user)** - szeretné, hogy egy partner a Britta Simon halogén szoftverben, amelyen az Azure Active Directory ábrázolt csatolt.
5. **[Az Azure Active Directory hozzárendelése tesztelje a felhasználó](#assigning-the-azure-ad-test-user)** - ahhoz, hogy Britta Simon Azure AD az egyszeri bejelentkezés használata.
5. **[Tesztelés egyszeri bejelentkezés](#testing-single-sign-on)** - ellenőrzéséhez, hogy működik-e a konfigurációs.

### <a name="configuring-azure-ad-single-single-sign-on"></a>Azure Active Directory egyetlen egyszeri bejelentkezés beállítása

Ez a szakasz célja Azure AD az egyszeri bejelentkezés az Azure klasszikus portálon engedélyezése és beállítása az egyszeri bejelentkezés halogén szoftveralkalmazásban.


**Azure Active Directory egyszeri bejelentkezés beállítása a halogén szoftverrel, hajtsa végre az alábbi lépéseket:**

1. Az Azure klasszikus portálon **Halogén szoftver** alkalmazás integrációs lapon kattintson **konfigurálása az egyszeri bejelentkezés** a **Konfigurálása az egyszeri bejelentkezés** párbeszédpanel megnyitásához.

    ![Egyszeri bejelentkezés beállítása][8]

2. **Hogyan szeretné, hogy jelentkezzen be az halogén szoftver felhasználók** lapon válassza az **Azure Active Directory Single Sign-On**, és kattintson a **Tovább gombra**.

    ![Azure Active Directory Single Sign-On][9]

3. Az **Alkalmazás beállításainak megadása** párbeszédpanel lapon hajtsa végre az alábbi lépéseket:  ![alkalmazás beállításainak konfigurálása][10]
 
     egy. **Jelentkezzen be URL-cím** mezőbe írja be a jelentkezzen be az alábbi minta halogén szoftveralkalmazásban a felhasználók által használt URL-cím: *https://global.hgncloud.com/fabrikam/welcome.jsp*

     b. Kattintson a **Tovább**gombra.
 
4. **Konfigurálása az egyszeri bejelentkezés halogén szoftver a** lapon kattintson a **metaadat-alapú letöltése**elemre, és mentse a fájlt a számítógépen helyben metaadat.
    
    ![Mi az Azure AD Connect][11]

5. Egy másik böngészőablakban bejelentkezésre **Halogén** szoftveralkalmazásban rendszergazdaként.

6. Kattintson a **Beállítások** fülre. 

    ![Mi az Azure AD Connect][12]


7. A bal oldali navigációs területen kattintson a **SAML konfiguráció**. 

    ![Mi az Azure AD Connect][13]

8. A **SAML beállítása** lapon hajtsa végre az alábbi lépéseket:  ![Mi az Azure AD Connect][14]

    egy. **Egyedi azonosító**jelölje ki a **NameID**.

    b. **Egyedi azonosító térképeket a szeretné**jelölje ki a **felhasználónév**.

    c billentyűkombinációt. A metaadatok letöltött fájl feltöltéséhez kattintson a jelölje ki a fájlt a **Tallózás gombra** , majd a **Fájl feltöltése**elemre.

    d. Tesztelje a konfigurációt, kattintson a **Futtatás tesztelése**gombra. 

    > [AZURE.NOTE] Meg kell várnia az üzenet "*a SAML teszt befejeződött. Zárja be az ablak*". Ezután zárja be a megnyitott böngészőablakban. A **SAML engedélyezése** jelölőnégyzet csak akkor érhető el, ha a teszt befejeződött.

    e. Jelölje ki a **SAML engedélyezése**.
    
    f. A **módosítások mentése**gombra. 


9. Az Azure klasszikus portálon jelölje be a egyszeri bejelentkezéses konfigurációs megerősítés, és válassza a **kész** , **Állítsa be az egyszeri bejelentkezés** párbeszédpanel bezárásához. 

    ![Mi az Azure AD Connect][15]

10. Az **egyszeri bejelentkezés megerősítés** lapon kattintson a **Befejezés**gombra.  

    ![Mi az Azure AD Connect][16]




### <a name="creating-an-azure-ad-test-user"></a>Azure AD-próba felhasználó létrehozása
Ez a szakasz célja vizsgálat felhasználó létrehozása az Azure-Britta Simon nevű klasszikus portálon.

**Teszt felhasználó létrehozása az Azure Active Directory, hajtsa végre az alábbi lépéseket:**

1. Az **Azure klasszikus portálon**kattintson a bal oldali navigációs területen kattintson az **Active Directory**.

    ![Mi az Azure AD Connect][100] 

2. A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3. A menüben, a képernyő tetején, a felhasználók listájának megjelenítéséhez kattintson a **felhasználók**elemre.

    ![Mi az Azure AD Connect][101] 

4. **Felhasználó hozzáadása**elemre az alsó eszköztáron a **Felhasználó hozzáadása** párbeszédpanel megnyitásához. 

    ![Mi az Azure AD Connect][102] 

5. A párbeszédpanel **közli velünk a felhasználóra vonatkozó** lapon hajtsa végre az alábbi lépéseket:

    ![Mi az Azure AD Connect][103] 
 
    egy. **Írja be a felhasználó**jelölje ki **a szervezet új felhasználót**.

    b. A felhasználónév **szövegmezőbe**írja be a **BrittaSimon**.

    c billentyűkombinációt. Kattintson a Tovább gombra.

6.  A **Felhasználói profil** párbeszédpanel lapon hajtsa végre az alábbi lépéseket: 

    ![Mi az Azure AD Connect][104] 

    egy. Az **első neve** mezőbe írja be a **Britta**.  

    b. Kattintson a **Vezetéknév** txtbox, típusa, **Simon**.

    c billentyűkombinációt. A **Megjelenítendő név** mezőbe írja be a **Britta Simon**.

    d. A **szerepkör** listában jelölje ki a **felhasználót**.

    e. Kattintson a **Tovább**gombra.

7. A párbeszédpanel **ideiglenes jelszót kérjen** lapon kattintson a **Hozzon létre**.

    ![Mi az Azure AD Connect][105]  

8. A párbeszédpanel **ideiglenes jelszót első** lapon hajtsa végre az alábbi lépéseket:

    ![Mi az Azure AD Connect][106]   

    egy. Jegyezze fel az **Új jelszót**az érték.
    b. Kattintson a **kész**gombra.   
  
 
### <a name="creating-a-halogen-software-test-user"></a>Halogén szoftver vizsgálat felhasználó létrehozása

Ez a szakasz célja Britta Simon nevű halogén szoftverben felhasználó létrehozása.

**A felhasználó neve Britta Simon halogén szoftverben létrehozásához tegye a következőket:**

1. Jelentkezzen rendszergazdaként **Halogén** szoftveralkalmazásban.

2. Kattintson a **Felhasználó központ** fülre, és kattintson a **Felhasználó létrehozása**gombra.

    ![Mi az Azure AD Connect][300]  

3. Az **Új felhasználó** párbeszédpanel lapon hajtsa végre az alábbi lépéseket:

    ![Mi az Azure AD Connect][301]

    egy. Az **első neve** mezőbe írja be a **Britta**. 
  
    b. Az **Utolsó neve** mezőbe írja be a **Simon**.
  
    c billentyűkombinációt. A **felhasználónév** mezőbe írja be **az Azure klasszikus portálon Brita Simon felhasználónév**.
  
    d. A **jelszó** mezőbe írjon be egy jelszót Britta.
  
    e. Kattintson a **Mentés**gombra.


### <a name="assigning-the-azure-ad-test-user"></a>Az Azure Active Directory-próba felhasználói hozzárendelése

Ez a szakasz célja Britta Simon Azure egyszeri bejelentkezés a saját hozzáférést biztosít halogén szoftver használandó engedélyezése.

![Mi az Azure AD Connect][200]

**Britta Simon hozzárendelése halogén szoftver, hajtsa végre az alábbi lépéseket:**

1. Az Azure klasszikus portál kattintva nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson az **alkalmazások** parancsra a felső menüben.

    ![Mi az Azure AD Connect][201]

2. Az alkalmazások listájában jelölje ki a **Halogén szoftvert**.

    ![Mi az Azure AD Connect][202]

1. A felső sávon kattintson a **felhasználók**elemre.

    ![Mi az Azure AD Connect][203]

1. A felhasználók listában jelölje ki a **Britta Simon**.

    ![Mi az Azure AD Connect][204]

2. Az alsó eszköztáron kattintson a **hozzárendelni**.

    ![Mi az Azure AD Connect][205]



### <a name="testing-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

Ez a szakasz célja a Azure Active Directory egyszeri bejelentkezéses beállítások tesztelése a Access panelen.

Ha a halogén szoftver csempére az Access panel gombra kattint, meg kell kérjen automatikusan aláírt-a halogén szoftveralkalmazásban.


## <a name="additional-resources"></a>További források

* [A szoftver alkalmazások integrálása az Azure Active Directory oktatóanyagok listája](active-directory-saas-tutorial-list.md)
* [Mi az access alkalmazás és az egyszeri bejelentkezés az Azure Active Directory címtárral?](active-directory-appssoaccess-whatis.md)

<!--Image references-->
[1]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_01.png
[2]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_02.png
[3]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_03.png
[4]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_04.png
[5]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_05.png
[6]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_06.png
[7]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_07.png
[8]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_08.png
[9]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_09.png
[10]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_10.png
[11]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_11.png
[12]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_12.png
[13]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_13.png
[14]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_14.png
[15]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_15.png
[16]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_16.png
[100]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_100.png 
[101]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_101.png 
[102]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_102.png 
[103]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_103.png 
[104]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_104.png 
[105]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_105.png 
[106]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_106.png 
[200]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_200.png 
[201]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_201.png 
[202]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_202.png
[203]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_203.png
[204]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_204.png
[205]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_205.png
[300]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_300.png
[301]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_301.png