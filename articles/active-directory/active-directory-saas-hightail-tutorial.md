<properties
    pageTitle="Oktatóprogram: Azure Active Directory-integráció a Hightail |} Microsoft Azure"
    description="Megtudhatja, hogy miként konfigurálása az egyszeri bejelentkezés Azure Active Directory és Hightail között."
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


# <a name="tutorial-azure-active-directory-integration-with-hightail"></a>Oktatóprogram: Azure Active Directory-integráció a Hightail

Ebben az oktatóanyagban célja mutatja, hogy miként Hightail integrálása az Azure Active Directory (Azure Active Directory).

Hightail integrálása az Azure Active Directory biztosít a következő előnyökkel jár:

- Az Azure Active Directory, aki rendelkezik hozzáféréssel Hightail megadhatja, hogy
- Engedélyezheti a felhasználóknak, hogy automatikusan kérjen jelentkezett-on való Hightail (Single Sign-On) az Azure Active Directory-fiókok
- A fiókokat az egy központi helyen – az Azure klasszikus portálra

Ha többet szeretne tudni a szoftver alkalmazás integrálása az Azure AD meg, hogy, ismerje meg, [az access alkalmazás és az egyszeri bejelentkezés az Azure Active Directory címtárral](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek

Azure Active Directory integráció a Hightail van szükség az alábbiakat:

- Az Azure Active Directory-előfizetéssel
- Egy Hightail egyszeri bejelentkezés engedélyezett előfizetés


> [AZURE.NOTE] Az oktatóprogram lépéseit teszteléséhez nem használata ajánlott munkakörnyezetben.


Az oktatóprogram lépéseit teszteléséhez célszerű követnie az alábbi javaslatokat:

- Erre akkor szükség, ne használja a termelési környezetén.
- Ha nincs telepítve az Azure Active Directory próba környezetben, kattint egy hónap próba [Itt](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban célja lehetővé teszi az Azure Active Directory egyszeri bejelentkezés tesztkörnyezetben tesztelése. 

Az alkalmazási példát, ebben az oktatóanyagban tagolt két fő építőelemek áll:

1. Hightail hozzáadása a gyűjteményből
2. Beállítása és tesztelése Azure Active Directory egyszeri bejelentkezés


## <a name="adding-hightail-from-the-gallery"></a>Hightail hozzáadása a gyűjteményből
Konfigurálja az Azure Active Directory integrálása a Hightail, meg kell Hightail hozzáadása az felügyelt szoftver alkalmazáslistában a gyűjteményből.

**A gyűjteményből Hightail felvételéhez hajtsa végre az alábbi lépéseket:**

1. Az **Azure klasszikus portálon**kattintson a bal oldali navigációs területen kattintson az **Active Directory**. 
 
    ![Az Active Directory][1]

2. A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3. Nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson a felső menüben **alkalmazások** .

    ![Alkalmazások][2]

4. Kattintson a **Hozzáadás** az oldal alján.

    ![Alkalmazások][3]

5. **Milyen feladatot szeretne tenni** párbeszédpanelen kattintson **a gyűjteményből az alkalmazások felvétele**.

    ![Alkalmazások][4]

6. A Keresés mezőbe írja be a **Hightail**.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_01.png)

7. Az eredmény ablaktáblában jelölje ki a **Hightail**, és válassza a **kész** , az alkalmazás hozzáadása gombra.

    ![Az alkalmazás kijelöli a gyűjteményben](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Beállítása és tesztelése Azure Active Directory egyszeri bejelentkezés
Ez a szakasz célja mutatja, hogy állítsa be, és a "Britta Simon" nevű próba felhasználón alapuló Hightail Azure AD az egyszeri bejelentkezés tesztelése.

Az egyszeri bejelentkezési a munkát az Azure Active Directory kell, hogy mi az a partner felhasználó Hightail az Azure Active Directory-felhasználónak. Más szóval Azure AD-felhasználó, és a kapcsolódó felhasználó Hightail hivatkozás viszonya kell létrehozni.

A hivatkozás kapcsolat jön létre által az Azure Active Directory Hightail **felhasználónév** értékként a **felhasználó neve** értéket rendeli.

Állítsa be, és Azure Active Directory egyszeri bejelentkezéssel való Hightail tesztelése, a következő építőelemek szükséges:

1. **[Azure Active Directory konfigurálása az egyszeri bejelentkezés](#configuring-azure-ad-single-sign-on)** - engedélyezése a felhasználóknak, hogy ezzel a szolgáltatással.
2. **[Felhasználó létrehozása az Azure Active Directory tesztelése](#creating-an-azure-ad-test-user)** - Azure Active Directory egyszeri bejelentkezéssel való Britta Simon ellenőrzéséhez.
4. **[Felhasználó létrehozása egy Hightail tesztelése](#creating-a-hightail-test-user)** - van-e egy megfelelője a Britta Simon Hightail, amelyen az Azure Active Directory ábrázolt csatolt.
5. **[Az Azure Active Directory hozzárendelése tesztelje a felhasználó](#assigning-the-azure-ad-test-user)** - ahhoz, hogy Britta Simon Azure AD az egyszeri bejelentkezés használata.
5. **[Tesztelés egyszeri bejelentkezés](#testing-single-sign-on)** - ellenőrzéséhez, hogy működik-e a konfigurációs.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure Active Directory Single Sign-On konfigurálása

Ez a szakasz célja Azure AD az egyszeri bejelentkezés az Azure klasszikus portálon engedélyezése és a Hightail alkalmazásban az egyszeri bejelentkezés beállítása.

Hightail alkalmazás vár a SAML Előfeltételek egy bizonyos formátumban. Állítsa be a következő jogcímalapú ehhez az alkalmazáshoz. A **"Atrribute"** lapon, az alkalmazás a következő attribútumok értékének kezelheti. Az alábbi képernyőképen látható, a. 

![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_51.png) 

**Állítson be Azure AD az egyszeri bejelentkezés Hightail, hajtsa végre az alábbi lépéseket:**


1. Az Azure klasszikus portálon **Hightail** alkalmazás integrációs lapján a menüben, a képernyő tetején kattintson a **attribútumok**elemre.

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-hightail-tutorial/tutorial_general_81.png) 


1. A **SAML jogkivonat attribútumok** párbeszédpanelen minden egyes sorára, az alábbi táblázatban látható hajtsa végre az alábbi lépéseket:

  	| Attribútumnév | Attribútumérték |
  	| --- | --- |    
  	| Utónév | User.givenName |
  	| Utónév  | User.surname |
  	| E-mailben | User.mail |
  	| Felhasználói identitáshoz | User.mail |

    egy. Kattintson a **felhasználó attribútum hozzáadása** a **Felhasználó Attribure hozzáadása** párbeszédpanel megnyitásához.

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-hightail-tutorial/tutorial_general_82.png) 


    b. **Attrubute neve** mezőbe írja be a attribútumnév meg abban a sorban látható.

    c billentyűkombinációt. Az **Attribútumérték** listából selsect az attribútumérték meg abban a sorban látható.

    d. Kattintson a **kész**gombra.  
    



1. A felső sávon kattintson a **Quick Start**.

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-hightail-tutorial/tutorial_general_83.png)  



2. **Hogyan szeretné, hogy jelentkezzen be az Hightail felhasználók** lapon válassza az **Azure Active Directory Single Sign-On**, és kattintson a **Tovább gombra**.

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_03.png) 


3. Az **Alkalmazás beállításainak megadása** párbeszédpanel lapon szeretne az alkalmazás beállítása **IDP kezdeményezett mód**, ha hajtsa végre az alábbi lépéseket, és kattintson a **Tovább**gombra:

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_04.png) 



    egy. **Válasz URL-cím** mezőbe írja be az URL-t a következő mintát: **"https://www.hightail.com/samlLogin?phi_action=app/samlLogin & részműveletet = handleSamlResponse"**

    b. Kattintson a **Tovább** gombra

4. Ha ki szeretne beállítása az alkalmazás **SP kezdeményezett mód** az **Alkalmazás beállításainak megadása** párbeszédpanel oldalon, kattintson a **"Megjelenítése speciális beállítások (nem kötelező)"** , és írja be a **Bejelentkezési a URL-CÍMÉT** , és kattintson a **Tovább**gombra.

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_06.png) 

    egy. Bejelentkezési a URL-cím mezőbe írja be a bejelentkezés az Hightail alkalmazás a következő mintát használatával a felhasználók által használt URL-címe: **https://www.hightail.com/loginSSO**. Az összes a felhasználókat szeretné használni az egyszeri bejelentkezés gyakori bejelentkezési lapját.

    b. Kattintson a **Tovább** gombra

5. A **beállítás az egyszeri bejelentkezés Hightail a** lapon hajtsa végre az alábbi lépéseket, és kattintson a **Tovább**gombra:

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_05.png) 


    egy. Kattintson a **tanúsítvány letöltése**, és mentse az alap-64 kódolt biztonságitanúsítvány-fájl a számítógépen.

    b. Kattintson a **Tovább**gombra.

    > [AZURE.NOTE] Előtt az egyszeri bejelentkezés beállítása a Hightail alkalmazást, kérjük, fehér listában a levelezési tartomány Hightail a csapattagok, hogy ezt a tartományt használó összes felhasználója is kihasználhatja az egyszeri bejelentkezés funkciókat.

6. Ha be van állítva az alkalmazás SSO, kell bejelentkezéses az Ön Hightail bérlői rendszergazdaként.

    egy. A felső sávon kattintson a **fiók** fülre, majd válassza a **SAML konfigurálása**.

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_001.png) 


    b. Jelölje be a **SAML**-hitelesítés engedélyezése jelölőnégyzetet.

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_002.png) 

    c billentyűkombinációt. Nyissa meg az alap-64 kódolt tanúsítvány a Jegyzettömbben, a tartalmát másolja a vágólapra, és illessze be a **SAML jogkivonat aláíró tanúsítvány** mezőben lévő értéket szeretné.

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_003.png) 


    d. A Másolás Hightail távoli bejelentkezési URL-cím az Azure Active Directory **SAML hitelesítésszolgáltató (identitásszolgáltató)** .

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_005.png)
    
    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_004.png)

    e. Ha az alkalmazás beállítása **IDP kezdeményezett mód** szeretne jelölje be a **"Identitásszolgáltató (IdP) által kezdeményezett bejelentkezve"**. Ha **SP kezdeményezett mód** kiválasztása **"(SP) szolgáltató által kezdeményezett bejelentkezve"**.
    
    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_006.png)

    f. Másolja a példány SAML fogyasztói URL-CÍMÉT, és illessze be a **Válasz URL-cím** mezőbe, lépés: 4 látható módon. 

    g. Kattintson a **Mentés**gombra.



6. Az Azure klasszikus portálon válassza ki az egyszeri bejelentkezéses konfigurációs megerősítő, és kattintson a **Tovább gombra**.

    ![Azure Active Directory Single Sign-On][10]

7. Az **egyszeri bejelentkezés megerősítés** lapon kattintson a **Befejezés**gombra. 
 
    ![Azure Active Directory Single Sign-On][11]




### <a name="creating-an-azure-ad-test-user"></a>Azure AD-próba felhasználó létrehozása
Ez a szakasz célja vizsgálat felhasználó létrehozása az Azure-Britta Simon nevű klasszikus portálon.

A felhasználók listában jelölje ki a **Britta Simon**.

![Azure Active Directory-felhasználó létrehozása][20]

**Teszt felhasználó létrehozása az Azure Active Directory, hajtsa végre az alábbi lépéseket:**

1. Az **Azure klasszikus portálon**kattintson a bal oldali navigációs területen kattintson az **Active Directory**.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-hightail-tutorial/create_aaduser_09.png) 

2. A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3. A menüben, a képernyő tetején, a felhasználók listájának megjelenítéséhez kattintson a **felhasználók**elemre.
 
    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-hightail-tutorial/create_aaduser_03.png) 


4. **Felhasználó hozzáadása**elemre az alsó eszköztáron a **Felhasználó hozzáadása** párbeszédpanel megnyitásához.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-hightail-tutorial/create_aaduser_04.png)

5. A párbeszédpanel **közli velünk a felhasználóra vonatkozó** lapon hajtsa végre az alábbi lépéseket:

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-hightail-tutorial/create_aaduser_05.png) 

    egy. **Írja be a felhasználó**jelölje ki **a szervezet új felhasználót**.

    b. Az a **Felhasználónév** mezőbe írja be a **BrittaSimon**.

    c billentyűkombinációt. Kattintson a **Tovább**gombra.

6.  A **Felhasználói profil** párbeszédpanel lapon hajtsa végre az alábbi lépéseket:

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-hightail-tutorial/create_aaduser_06.png) 

    egy. Az **első neve** mezőbe írja be a **Britta**.  

    b. Az **Utolsó neve** mezőbe írja be, **Simon**.

    c billentyűkombinációt. A **Megjelenítendő név** mezőbe írja be a **Britta Simon**.

    d. A **szerepkör** listában jelölje ki a **felhasználót**.

    e. Kattintson a **Tovább**gombra.

7. A párbeszédpanel **ideiglenes jelszót kérjen** lapon kattintson a **Hozzon létre**.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-hightail-tutorial/create_aaduser_07.png) 


8. A párbeszédpanel **ideiglenes jelszót első** lapon hajtsa végre az alábbi lépéseket:
 
    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-hightail-tutorial/create_aaduser_08.png) 

    egy. Jegyezze fel az **Új jelszót**az érték.

    b. Kattintson a **kész**gombra.   



### <a name="creating-a-hightail-test-user"></a>Hightail vizsgálat felhasználó létrehozása

Ez a szakasz célja Britta Simon elnevezésű Hightail felhasználó létrehozása. 

Nincs teendő ebben a szakaszban a meg nem. Támogatja az egyéni jogcímeken alapuló közvetlenül az idő felhasználói kiépítési hightail. Ha az egyéni követelések van beállítva, ahogy azt az **[Azure Active Directory konfigurálása egyszeri bejelentkezés a](#configuring-azure-ad-single-sign-on)** fenti szakasz, a felhasználó automatikusan létrejön az alkalmazásban, ha még nem létezik. 

> [AZURE.NOTE] Ha egy felhasználó létrehozása manuálisan kell szeretne-e a Hightail támogatási csoport munkatársaitól kaphat keresztül [support@hightail.com](mailto:support@hightail.com).


### <a name="assigning-the-azure-ad-test-user"></a>Az Azure Active Directory-próba felhasználói hozzárendelése

Ez a szakasz célja Britta Simon Azure egyszeri bejelentkezés a saját hozzáférést biztosít Hightail használandó engedélyezése.

![Felhasználó hozzárendelése][200] 

**Britta Simon hozzárendelése Hightail, hajtsa végre az alábbi lépéseket:**

1. Az Azure klasszikus portál kattintva nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson az **alkalmazások** parancsra a felső menüben.
 
    ![Felhasználó hozzárendelése][201] 

2. Az alkalmazások listájában jelölje ki a **Hightail**.

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_50.png) 


1. A felső sávon kattintson a **felhasználók**elemre.

    ![Felhasználó hozzárendelése][203]

1. A felhasználók listában jelölje ki a **Britta Simon**.

2. Az alsó eszköztáron kattintson a **hozzárendelni**.

![Felhasználó hozzárendelése][205]



### <a name="testing-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

Ez a szakasz célja a Azure Active Directory egyszeri bejelentkezéses beállítások tesztelése a Access panelen.

Amikor az Access panel a Hightail mozaik gombra kattint, meg kell első automatikusan aláírt-on az Hightail alkalmazás.


## <a name="additional-resources"></a>További források

* [A szoftver alkalmazások integrálása az Azure Active Directory oktatóanyagok listája](active-directory-saas-tutorial-list.md)
* [Mi az access alkalmazás és az egyszeri bejelentkezés az Azure Active Directory címtárral?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-hightail-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-hightail-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-hightail-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-hightail-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-hightail-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-hightail-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-hightail-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-hightail-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-hightail-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-hightail-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-hightail-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-hightail-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-hightail-tutorial/tutorial_general_205.png
