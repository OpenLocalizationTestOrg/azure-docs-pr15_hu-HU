<properties
    pageTitle="Oktatóprogram: Azure Active Directory-integráció a Novatus |} Microsoft Azure"
    description="Megtudhatja, hogy miként konfigurálása az egyszeri bejelentkezés Azure Active Directory és LearnUpon között."
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
    ms.date="08/15/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-learnupon"></a>Oktatóprogram: Azure Active Directory-integráció a LearnUpon

Ebben az oktatóanyagban célja mutatja, hogy miként LearnUpon integrálása az Azure Active Directory (Azure Active Directory).  
LearnUpon integrálása az Azure Active Directory biztosít a következő előnyökkel jár:

- Az Azure Active Directory, aki rendelkezik hozzáféréssel LearnUpon megadhatja, hogy
- Engedélyezheti a felhasználóknak, hogy automatikusan első jelentkezett-on való LearnUpon (Single Sign-On) az Azure Active Directory-fiókok
- A fiókokat az egy központi helyen – az Azure Active Directory klasszikus 


Ha többet szeretne tudni a szoftver alkalmazás integrálása az Azure AD meg, hogy, ismerje meg, [az access alkalmazás és az egyszeri bejelentkezés az Azure Active Directory címtárral](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek

Azure Active Directory integráció a LearnUpon van szükség az alábbiakat:

- Az Azure Active Directory-előfizetéssel
- Egy LearnUpon egyszeri bejelentkezés engedélyezett előfizetés


> [AZURE.NOTE] Az oktatóprogram lépéseit teszteléséhez nem használata ajánlott munkakörnyezetben.


Az oktatóprogram lépéseit teszteléséhez célszerű követnie az alábbi javaslatokat:

- Erre akkor szükség, ne használja a termelési környezetén.
- Ha nincs telepítve az Azure Active Directory próba környezetben, kattint egy hónap próba [Itt](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban célja lehetővé teszi az Azure Active Directory egyszeri bejelentkezés tesztkörnyezetben tesztelése.  
Az alkalmazási példát, ebben az oktatóanyagban tagolt két fő építőelemek áll:

1. LearnUpon hozzáadása a gyűjteményből
2. Beállítása és tesztelése Azure Active Directory egyszeri bejelentkezés


## <a name="adding-learnupon-from-the-gallery"></a>LearnUpon hozzáadása a gyűjteményből
Konfigurálja az Azure Active Directory integrálása a LearnUpon, meg kell LearnUpon hozzáadása az felügyelt szoftver alkalmazáslistában a gyűjteményből.

**A gyűjteményből LearnUpon felvételéhez hajtsa végre az alábbi lépéseket:**

1. Az **Azure klasszikus portálon**kattintson a bal oldali navigációs területen kattintson az **Active Directory**. 

    ![Az Active Directory][1]

2. A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3. Nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson a felső menüben **alkalmazásokat** .

    ![Alkalmazások][2]

4. Kattintson a **Hozzáadás** az oldal alján.

    ![Alkalmazások][3]

5. **Milyen feladatot szeretne tenni** párbeszédpanelen kattintson **a gyűjteményből az alkalmazások felvétele**.

    ![Alkalmazások][4]

6. A Keresés mezőbe írja be a **LearnUpon**.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_01.png)

7. Az eredmény ablaktáblában jelölje ki a **LearnUpon**, és válassza a **kész** , az alkalmazás hozzáadása gombra.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Beállítása és tesztelése Azure Active Directory egyszeri bejelentkezés
Ez a szakasz célja mutatja, hogy állítsa be, és a "Britta Simon" nevű próba felhasználón alapuló LearnUpon Azure AD az egyszeri bejelentkezés tesztelése.

Az egyszeri bejelentkezési a munkát az Azure Active Directory kell, hogy mi az a partner felhasználó LearnUpon az Azure Active Directory-felhasználónak. Más szóval Azure AD-felhasználó, és a kapcsolódó felhasználó LearnUpon hivatkozás viszonya kell létrehozni.  
A hivatkozás kapcsolat jön létre által az Azure Active Directory LearnUpon **felhasználónév** értékként a **felhasználó neve** értéket rendeli.

Állítsa be, és Azure Active Directory egyszeri bejelentkezéssel való LearnUpon tesztelése, a következő építőelemek szükséges:

1. **[Azure Active Directory konfigurálása az egyszeri bejelentkezés](#configuring-azure-ad-single-single-sign-on)** - engedélyezése a felhasználóknak, hogy ezzel a szolgáltatással.
2. **[Felhasználó létrehozása az Azure Active Directory tesztelése](#creating-an-azure-ad-test-user)** - Azure Active Directory egyszeri bejelentkezéssel való Britta Simon ellenőrzéséhez.
4. **[Felhasználó létrehozása egy LearnUpon tesztelése](#creating-a-learnupon-test-user)** - van-e egy megfelelője a Britta Simon LearnUpon, amelyen az Azure Active Directory ábrázolt csatolt.
5. **[Az Azure Active Directory hozzárendelése tesztelje a felhasználó](#assigning-the-azure-ad-test-user)** - ahhoz, hogy Britta Simon Azure AD az egyszeri bejelentkezés használata.
5. **[Tesztelés egyszeri bejelentkezés](#testing-single-sign-on)** - ellenőrzéséhez, hogy működik-e a konfigurációs.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure Active Directory Single Sign-On konfigurálása

Ez a szakasz célja Azure AD az egyszeri bejelentkezés az Azure klasszikus portálon engedélyezése és a LearnUpon alkalmazásban az egyszeri bejelentkezés beállítása.



**Állítson be Azure AD az egyszeri bejelentkezés LearnUpon, hajtsa végre az alábbi lépéseket:**

1. Az Azure klasszikus portálon **LearnUpon** alkalmazás integrációs lapon kattintson **konfigurálása az egyszeri bejelentkezés** a **Konfigurálása az egyszeri bejelentkezés** párbeszédpanel megnyitásához.

    ![Egyszeri bejelentkezés beállítása][6] 

2. **Hogyan szeretné, hogy jelentkezzen be az LearnUpon felhasználók** lapon válassza az **Azure Active Directory Single Sign-On**, és kattintson a **Tovább gombra**.

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_03.png) 

3. Az **Alkalmazás beállításainak megadása** párbeszédpanel lapon hajtsa végre az alábbi lépéseket:.

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_04.png) 


    egy. **Válasz URL-cím** mezőbe írja be a állítás fogyasztói szolgáltatás URL-CÍMÉT az alábbi minta használatával:`https://\<companyname\>.learnupon.com/saml/consumer`

    b. Kattintson a **Tovább**gombra. 


4. A **beállítás az egyszeri bejelentkezés LearnUpon a** lapon hajtsa végre az alábbi lépéseket:

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_05.png) 

    egy. Kattintson a **tanúsítvány letöltése**, és mentse a fájlt a számítógépen. A tanúsítvány és a metaadat-alapú URL-ek (entitás azonosítója, Egyszeri bejelentkezési az URL-CÍMÉT és a bejelentkezési meg URL-CÍMÉT) egyszeri bejelentkezés beállítása a LearnUpon oldalon lesz szükség.

    b. Kattintson a **Tovább**gombra.




1. Nyissa meg a másik böngészőablakot, amelyben, és jelentkezzen be LearnUpon be egy rendszergazdai fiókkal. 

1. Kattintson a **Beállítások** fülre.

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_06.png) 


1. **Egyszeri bejelentkezés - SAML**gombra, és kattintson az **Általános beállítások** , hogy a SAML beállításainak konfigurálása.

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_07.png) 


5. **Az általános beállítások** csoportban hajtsa végre az alábbi lépéseket:

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_08.png) 

    egy. Jelölje ki, hogy **engedélyezve van**.

    b. Jelölje ki a **2.0-s** **verzió**.

    c billentyűkombinációt. **Ugrás feltételek**válassza a **nincs**lehetőséget.

    d. Az a **SAML jogkivonat közzé paraméter neve** mezőbe, amely a fent említett kérés bejegyzés paraméter a SAML fogyasztói URL-nevét írja be a SAML állítás ellenőrizendő és hitelesített – például **SAMLResponse**tartalmazza.

    e. **Névformátum azonosítója** mezőbe írja be az értéket, amely jelzi, hogy a SAML állítás az a felhasználói azonosító (E-mail cím) helye – például **urn: oasis: nevek: tc: SAML:1.1:nameid-formátum: emailAddress**.

    f. Az **Azonosításához szolgáltató helye** mezőbe írja be az érték, amely azt jelzi, ahol a felhasználók elküldi, ha a feltöltött ikonra az Azure klasszikus portál bejelentkezési képernyő kattintanak.

    az Azure klasszikus portál g.in **Egyetlen Sign-Out szolgáltatás URL-cím**másolása, és illessze be az **URL-CÍMÉT a Kijelentkezés** textbos.

    h. Kattintson a **kezelés ujjával nyomtatja ki**, és töltse ki az ujját a Nyomtatás a letöltött tanúsítvány. 


1. Kattintson a **Felhasználói beállításokat**, és hajtsa végre az alábbi lépéseket:

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_11.png) 

    egy. Az **Első névformátum azonosító** mezőbe írja be az értéket, amely közli velünk a SAML állítás az a felhasználók Utónév helye – például: **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/ givenname**.

    b. Az **Utolsó névformátum azonosítója** mezőbe írja be az értéket, amely közli velünk a SAML állítás az a felhasználók Utónév helye – például: **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/ vezetékneve**.


6. Az Azure klasszikus portálon válassza ki az egyszeri bejelentkezéses konfigurációs megerősítő, és kattintson a **Tovább gombra**.

    ![Azure Active Directory Single Sign-On][10]

7. Az **egyszeri bejelentkezés megerősítés** lapon kattintson a **Befejezés**gombra.  

    ![Azure Active Directory Single Sign-On][11]




### <a name="creating-an-azure-ad-test-user"></a>Azure AD-próba felhasználó létrehozása
Ez a szakasz célja vizsgálat felhasználó létrehozása az Azure-Britta Simon nevű klasszikus portálon.

![Azure Active Directory-felhasználó létrehozása][20]

**Teszt felhasználó létrehozása az Azure Active Directory, hajtsa végre az alábbi lépéseket:**

1. Az **Azure klasszikus portálon**kattintson a bal oldali navigációs területen kattintson az **Active Directory**.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-learnupon-tutorial/create_aaduser_09.png) 

2. A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3. A menüben, a képernyő tetején, a felhasználók listájának megjelenítéséhez kattintson a **felhasználók**elemre.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-learnupon-tutorial/create_aaduser_03.png) 

4. **Felhasználó hozzáadása**elemre az alsó eszköztáron a **Felhasználó hozzáadása** párbeszédpanel megnyitásához.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-learnupon-tutorial/create_aaduser_04.png) 

5. A párbeszédpanel **közli velünk a felhasználóra vonatkozó** lapon hajtsa végre az alábbi lépéseket:

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-learnupon-tutorial/create_aaduser_05.png) 

    egy. Felhasználó típusa jelölje be az új felhasználó a szervezet.

    b. A felhasználónév **szövegmezőbe**írja be a **BrittaSimon**.

    c billentyűkombinációt. Kattintson a **Tovább**gombra.

6.  A **Felhasználói profil** párbeszédpanel lapon hajtsa végre az alábbi lépéseket:

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-learnupon-tutorial/create_aaduser_06.png) 

    egy. Az **első neve** mezőbe írja be a **Britta**.  

    b. Az **Utolsó neve** mezőbe írja be, **Simon**.

    c billentyűkombinációt. A **Megjelenítendő név** mezőbe írja be a **Britta Simon**.

    d. A **szerepkör** listában jelölje ki a **felhasználót**.

    e. Kattintson a **Tovább**gombra.

7. A párbeszédpanel **ideiglenes jelszót kérjen** lapon kattintson a **Hozzon létre**.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-learnupon-tutorial/create_aaduser_07.png) 

8. A párbeszédpanel **ideiglenes jelszót első** lapon hajtsa végre az alábbi lépéseket:

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-learnupon-tutorial/create_aaduser_08.png) 

    egy. Jegyezze fel az **Új jelszót**az érték.

    b. Kattintson a **kész**gombra.   



### <a name="creating-a-learnupon-test-user"></a>LearnUpon vizsgálat felhasználó létrehozása

Ez a szakasz célja Britta Simon nevű LearnUpon felhasználó létrehozása. LearnUpon támogatja a kiépítési közvetlenül az idő, amely alapértelmezés szerint ki van engedélyezve van.

Nincs teendő ebben a szakaszban a meg nem. Új felhasználóként LearnUpon eléréséhez, ha még nem létezik kísérlet során jön létre. Az [Azure Active Directory Single Sign-On konfigurálása](#configuring-azure-ad-single-single-sign-on).

> [AZURE.NOTE] Ha egy felhasználó létrehozása manuálisan kell szüksége a LearnUpon támogatási csoport munkatársaitól kaphat.


### <a name="assigning-the-azure-ad-test-user"></a>Az Azure Active Directory-próba felhasználói hozzárendelése

Ez a szakasz célja Britta Simon Azure egyszeri bejelentkezés a saját hozzáférést biztosít LearnUpon használandó engedélyezése.

![Felhasználó hozzárendelése][200] 

**Britta Simon hozzárendelése LearnUpon, hajtsa végre az alábbi lépéseket:**

1. Az Azure klasszikus portál kattintva nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson az **alkalmazások** parancsra a felső menüben.

    ![Felhasználó hozzárendelése][201] 

2. Az alkalmazások listájában jelölje ki a **LearnUpon**.

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_50.png) 

1. A felső sávon kattintson a **felhasználók**elemre.

    ![Felhasználó hozzárendelése][203] 

1. A felhasználók listában jelölje ki a **Britta Simon**.

2. Az alsó eszköztáron kattintson a **hozzárendelni**.

    ![Felhasználó hozzárendelése][205]



### <a name="testing-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

Ez a szakasz célja a Azure Active Directory egyszeri bejelentkezéses beállítások tesztelése a Access panelen.  
Amikor az Access panel a LearnUpon mozaik gombra kattint, meg kell első automatikusan aláírt-on az LearnUpon alkalmazás.


## <a name="additional-resources"></a>További források

* [A szoftver alkalmazások integrálása az Azure Active Directory oktatóanyagok listája](active-directory-saas-tutorial-list.md)
* [Mi az access alkalmazás és az egyszeri bejelentkezés az Azure Active Directory címtárral?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_205.png
