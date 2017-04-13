<properties
    pageTitle="Oktatóprogram: Azure Active Directory-integráció a helyettes |} Microsoft Azure"
    description="Megtudhatja, hogy miként konfigurálása az egyszeri bejelentkezés Azure Active Directory és helyettes között."
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
    ms.date="09/28/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-deputy"></a>Oktatóprogram: Azure Active Directory-integráció a helyettes

Ebben az oktatóanyagban célja mutatja, hogy miként helyettes integrálása az Azure Active Directory (Azure Active Directory).

Helyettes integrálása az Azure Active Directory biztosít a következő előnyökkel jár:

- Az Azure Active Directory, aki rendelkezik hozzáféréssel helyettes megadhatja, hogy
- Engedélyezheti a felhasználóknak, hogy automatikusan első jelentkezett-on való helyettes (Single Sign-On) az Azure Active Directory-fiókok
- A fiókokat az egy központi helyen – az Azure klasszikus portálra

Ha többet szeretne tudni a szoftver alkalmazás integrálása az Azure AD meg, hogy, ismerje meg, [az access alkalmazás és az egyszeri bejelentkezés az Azure Active Directory címtárral](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek

Azure Active Directory integráció a helyettes van szükség az alábbiakat:

- Az Azure Active Directory-előfizetéssel
- Egy helyettes egyszeri bejelentkezés engedélyezett előfizetésekből


> [AZURE.NOTE] Az oktatóprogram lépéseit teszteléséhez nem használata ajánlott munkakörnyezetben.


Az oktatóprogram lépéseit teszteléséhez célszerű követnie az alábbi javaslatokat:

- Erre akkor szükség, ne használja a termelési környezetén.
- Ha nincs telepítve az Azure Active Directory próba környezetben, kattint egy hónap próba [Itt](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban célja lehetővé teszi az Azure Active Directory egyszeri bejelentkezés tesztkörnyezetben tesztelése.

Az alkalmazási példát, ebben az oktatóanyagban tagolt két fő építőelemek áll:

1. Helyettes hozzáadása a gyűjteményből
2. Beállítása és tesztelése Azure Active Directory egyszeri bejelentkezés


## <a name="adding-deputy-from-the-gallery"></a>Helyettes hozzáadása a gyűjteményből
Helyettes integrálása Azure Active Directory konfigurálásához szüksége helyettes hozzáadása az felügyelt szoftver alkalmazáslistában a gyűjteményből.

**Helyettes a gyűjteményből felvételéhez hajtsa végre az alábbi lépéseket:**

1. Az **Azure klasszikus portálon**kattintson a bal oldali navigációs területen kattintson az **Active Directory**. 

    ![Az Active Directory][1]

2. A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3. Nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson a felső menüben **alkalmazásokat** .
    
    ![Alkalmazások][2]

4. Kattintson a **Hozzáadás** az oldal alján.
    
    ![Alkalmazások][3]

5. **Milyen feladatot szeretne tenni** párbeszédpanelen kattintson **a gyűjteményből az alkalmazások felvétele**.

    ![Alkalmazások][4]

6. A Keresés mezőbe írja be a **helyettes**.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_01.png)

7. Az eredmények panel **helyettes**jelölje ki, és válassza a **kész** , az alkalmazás hozzáadása parancsra.

    ![Az alkalmazás kijelöli a gyűjteményben](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_0001.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Beállítása és tesztelése Azure Active Directory egyszeri bejelentkezés
Ez a szakasz célja mutatja, hogy állítsa be, és a "Britta Simon" nevű próba felhasználón alapuló helyettes Azure AD az egyszeri bejelentkezés tesztelése.

Az egyszeri bejelentkezési a munkát az Azure Active Directory kell, hogy mi az a partner felhasználó helyettes az Azure Active Directory-felhasználónak. Más szóval Azure AD-felhasználó, és a kapcsolódó felhasználó helyettes hivatkozás viszonya kell létrehozni.

A hivatkozás kapcsolat jön létre által az Azure Active Directory helyettes **felhasználónév** értékként a **felhasználó neve** értéket rendeli.

Állítsa be, és Azure Active Directory egyszeri bejelentkezéssel való helyettes tesztelése, a következő építőelemek szükséges:

1. **[Azure Active Directory konfigurálása az egyszeri bejelentkezés](#configuring-azure-ad-single-single-sign-on)** - engedélyezése a felhasználóknak, hogy ezzel a szolgáltatással.
2. **[Felhasználó létrehozása az Azure Active Directory tesztelése](#creating-an-azure-ad-test-user)** - Azure Active Directory egyszeri bejelentkezéssel való Britta Simon ellenőrzéséhez.
3. **[Helyettes létrehozása tesztelje a felhasználó](#creating-a-deputy-test-user)** - van-e egy megfelelője a Britta Simon helyettes, amelyen az Azure Active Directory ábrázolt csatolt.
4. **[Az Azure Active Directory hozzárendelése tesztelje a felhasználó](#assigning-the-azure-ad-test-user)** - ahhoz, hogy Britta Simon Azure AD az egyszeri bejelentkezés használata.
5. **[Tesztelés egyszeri bejelentkezés](#testing-single-sign-on)** - ellenőrzéséhez, hogy működik-e a konfigurációs.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure Active Directory egyszeri bejelentkezés beállítása

Ebben a részben Azure AD az egyszeri bejelentkezés a klasszikus portálon engedélyezése, és a helyettes alkalmazásban az egyszeri bejelentkezés beállítása.

**Állítson be Azure AD az egyszeri bejelentkezés helyettes, hajtsa végre az alábbi lépéseket:**

1. A klasszikus portálon **helyettes** alkalmazás integrációs lapon kattintson **konfigurálása az egyszeri bejelentkezés** a **Konfigurálása az egyszeri bejelentkezés** párbeszédpanel megnyitásához.
     
    ![Egyszeri bejelentkezés beállítása][6] 

2. **Hogyan szeretné, hogy jelentkezzen be az helyettes felhasználók** lapon válassza az **Azure Active Directory Single Sign-On**, és kattintson a **Tovább gombra**.
    
    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_03.png)

3. Az **Alkalmazás beállításainak megadása** párbeszédpanel lapon szeretne az alkalmazás beállítása **IDP kezdeményezett mód**, ha hajtsa végre az alábbi lépéseket, és kattintson a **Tovább**gombra:

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_04.png)

    egy. Az **azonosító** mezőbe írja be az URL-cím használata a következő mintát: `https://<your-subdomain>.<region>.deputy.com`.

    b. Az **Válasz URL-cím** mezőbe írja be az URL-cím használata a következő mintát: `https://<your-subdomain>.<region>.deputy.com/exec/devapp/samlacs`.

    c billentyűkombinációt. Kattintson a **Tovább**gombra.

4. Ha ki szeretne beállítása az alkalmazás **SP kezdeményezett mód** az **Alkalmazás beállításainak megadása** párbeszédpanel oldalon, kattintson a **"Megjelenítése speciális beállítások (nem kötelező)"** , és írja be a **Bejelentkezési a URL-CÍMÉT** , és kattintson a **Tovább**gombra.

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_05.png)

    egy. Az **Bejelentkezési a URL-cím** mezőbe írja be az URL-cím használata a következő mintát: `https://<your-subdomain>.<region>.deputy.com`.

    b. Kattintson a **Tovább**gombra.

    > [AZURE.NOTE] Helyettes régió utótag opitional, vagy használjon az alábbiak egyike: au |} hiányzik |} Európai Unió |}, |} la |} af |}-|} ent-au |} ent-na |} ent-Európai Unió |} ent-mint |} ent-la |} ent-af |} ent egy

5. A **beállítás az egyszeri bejelentkezés a helyettes** lapon hajtsa végre az alábbi lépéseket, és kattintson a **Tovább**gombra:

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_06.png)

    egy. Kattintson a **tanúsítvány letöltése**, és mentse a fájlt a számítógépen.

    
6. Nyissa meg az alábbi URL-címre: https://(your-subdomain).deputy.com/exec/config/system_config. Nyissa meg a **Biztonsági beállítások** , és kattintson a **Szerkesztés**gombra.

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_004.png)

7. Az Azure klasszikus portálon kattintson a konfigurálás egyszeri bejelentkezés helyettes lapon, a SAML egyszeri bejelentkezési URL másolása másokkal. 

8. A **Biztonsági beállítások** lapon hajtsa végre a alábbi lépéseket.

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_005.png)

    egy. Engedélyezze a **közösségi Login**.

    b. Nyissa meg a kódolt Base64 tanúsítvány a Jegyzettömbben, azt tartalmát a vágólapra másolja és illessze be a **OpenSSL tanúsítvány** mezőben lévő értéket szeretné.

    c billentyűkombinációt. Az SAM egyszeri bejelentkezési URL-cím mezőbe írja be a`https://<your subdomain>.deputy.com/exec/devapp/samlacs?dpLoginTo=<saml sso url>`
    
    d. Az SAM egyszeri bejelentkezési URL-címe mezőbe cseréje `<your subdomain>` az altartomány a.

    e. Az SAM egyszeri bejelentkezési URL-címe mezőbe cseréje `<saml sso url>` SAML egyszeri bejelentkezési URL-címét az Azure klasszikus portálról másolta.

    f. Kattintson a **beállítások mentéséhez**.

9. A klasszikus portálon jelölje be a egyszeri bejelentkezéses konfigurációs megerősítés, és kattintson a **Tovább gombra**.
    
    ![Azure Active Directory Single Sign-On][10]

10. Az **egyszeri bejelentkezés megerősítés** lapon kattintson a **Befejezés**gombra.  
    
    ![Azure Active Directory Single Sign-On][11]

### <a name="creating-an-azure-ad-test-user"></a>Azure AD-próba felhasználó létrehozása
Ez a szakasz célja próba felhasználó létrehozása az úgynevezett Britta Simon klasszikus portálon.

![Azure Active Directory-felhasználó létrehozása][20]

**Teszt felhasználó létrehozása az Azure Active Directory, hajtsa végre az alábbi lépéseket:**

1. Az **Azure klasszikus portálon**kattintson a bal oldali navigációs területen kattintson az **Active Directory**.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-deputy-tutorial/create_aaduser_09.png)

2. A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3. A menüben, a képernyő tetején, a felhasználók listájának megjelenítéséhez kattintson a **felhasználók**elemre.
    
    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-deputy-tutorial/create_aaduser_03.png)

4. **Felhasználó hozzáadása**elemre az alsó eszköztáron a **Felhasználó hozzáadása** párbeszédpanel megnyitásához.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-deputy-tutorial/create_aaduser_04.png)

5. A párbeszédpanel **közli velünk a felhasználóra vonatkozó** lapon hajtsa végre az alábbi lépéseket:

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-deputy-tutorial/create_aaduser_05.png)

    egy. Felhasználó típusa jelölje be az új felhasználó a szervezet.

    b. A felhasználónév **szövegmezőbe**írja be a **BrittaSimon**.

    c billentyűkombinációt. Kattintson a **Tovább**gombra.

6.  A **Felhasználói profil** párbeszédpanel lapon hajtsa végre az alábbi lépéseket:
    
    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-deputy-tutorial/create_aaduser_06.png)

    egy. Az **első neve** mezőbe írja be a **Britta**.  

    b. Az **Utolsó neve** mezőbe írja be, **Simon**.

    c billentyűkombinációt. A **Megjelenítendő név** mezőbe írja be a **Britta Simon**.

    d. A **szerepkör** listában jelölje ki a **felhasználót**.

    e. Kattintson a **Tovább**gombra.

7. A párbeszédpanel **ideiglenes jelszót kérjen** lapon kattintson a **Hozzon létre**.
    
    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-deputy-tutorial/create_aaduser_07.png)

8. A párbeszédpanel **ideiglenes jelszót első** lapon hajtsa végre az alábbi lépéseket:
    
    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-deputy-tutorial/create_aaduser_08.png)

    egy. Jegyezze fel az **Új jelszót**az érték.

    b. Kattintson a **kész**gombra.   



### <a name="creating-a-deputy-test-user"></a>Helyettes próba felhasználó létrehozása

Ahhoz, hogy az Azure Active Directory-felhasználóknak, hogy jelentkezzen be a helyettes, hogy ki kell építenie helyettes be. Helyettes, ha a feladat manuális kiépítési.

####<a name="to-provision-a-user-account-perform-the-following-steps"></a>Építse egy felhasználói fiókot, hajtsa végre az alábbi lépéseket:

1.  Jelentkezzen be a helyettes vállalati webhely rendszergazdaként.

2.  A felső navigációs ablakban kattintson a **személyek**elemre.

    ![Személyek] (./media/active-directory-saas-deputy-tutorial/tutorial_deputy_001.png "Személyek")

3.  Kattintson a **Személyek hozzáadása** gombra, és kattintson a **Hozzáadás egy személy**.

    ![Személyek hozzáadása] (./media/active-directory-saas-deputy-tutorial/tutorial_deputy_002.png "Személyek hozzáadása")

4.  Hajtsa végre az alábbi lépéseket, és kattintson a **Mentés és felkérése**.

    ![Új felhasználó] (./media/active-directory-saas-deputy-tutorial/tutorial_deputy_003.png "Új felhasználó")

    egy. Az **neve** mezőbe írja be a **Britta** és **Simon**.  

    b. Az **E-mail** mezőbe írja be a kívánt kiépítése Azure AD-fiók e-mail címét.

    c billentyűkombinációt. **A munka** mezőbe írja be a bussniess nevét.

    d. Kattintson a **Mentés és felkérése** gombra.

    >[AZURE.NOTE]A AAD fióktulajdonos a fog egy e-mailt, és kövesse a előtt aktívvá válik, győződjön meg arról, hogy a fiók mutató hivatkozást. Bármely más helyettes felhasználói fiók létrehozása eszközöket is használhatja, illetve helyettes rendelkezést AAD felhasználói fiókok által biztosított API-khoz.


### <a name="assigning-the-azure-ad-test-user"></a>Az Azure Active Directory-próba felhasználói hozzárendelése

Ez a szakasz célja Britta Simon Azure egyszeri bejelentkezés a saját hozzáférést biztosít helyettes használandó engedélyezése.
    
![Felhasználó hozzárendelése][200]

**Helyettes Britta Simon rendelni, hajtsa végre az alábbi lépéseket:**

1. A klasszikus portál kattintva nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson az **alkalmazások** parancsra a felső menüben.
    
    ![Felhasználó hozzárendelése][201]

2. Az alkalmazások listájában jelölje ki a **helyettes**.
    
    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_50.png)

3. A felső sávon kattintson a **felhasználók**elemre.
    
    ![Felhasználó hozzárendelése][203]

4. A felhasználók listában jelölje ki a **Britta Simon**.

5. Az alsó eszköztáron kattintson a **hozzárendelni**.
    
    ![Felhasználó hozzárendelése][205]

### <a name="testing-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

Ez a szakasz célja a Azure Active Directory egyszeri bejelentkezéses beállítások tesztelése a Access panelen.
 
Amikor az Access panel a helyettes mozaik gombra kattint, meg kell első automatikusan jelentkezett-on az helyettes alkalmazás.

## <a name="additional-resources"></a>További források

* [A szoftver alkalmazások integrálása az Azure Active Directory oktatóanyagok listája](active-directory-saas-tutorial-list.md)
* [Mi az access alkalmazás és az egyszeri bejelentkezés az Azure Active Directory címtárral?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_205.png
