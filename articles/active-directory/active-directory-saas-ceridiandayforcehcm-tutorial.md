<properties
    pageTitle="Oktatóprogram: Azure Active Directory-integráció a Ceridian Dayforce táblabeli HCM |} Microsoft Azure"
    description="Megtudhatja, hogy miként konfigurálása az egyszeri bejelentkezés Azure Active Directory és Ceridian Dayforce táblabeli HCM között."
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
    ms.date="09/01/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-ceridian-dayforce-hcm"></a>Oktatóprogram: Azure Active Directory-integráció a Ceridian Dayforce táblabeli HCM

Ebben az oktatóanyagban célja mutatja, hogy miként Ceridian Dayforce táblabeli HCM integrálása az Azure Active Directory (Azure Active Directory).  
Ceridian Dayforce táblabeli HCM integrálása az Azure Active Directory biztosít a következő előnyökkel jár:

- Az Azure Active Directory, aki rendelkezik hozzáféréssel Ceridian Dayforce táblabeli HCM megadhatja, hogy
- Az Azure Active Directory-fiókok engedélyezheti a felhasználóknak, hogy automatikusan első jelentkezett-on való Ceridian Dayforce táblabeli HCM (Single Sign-On)
- A fiókokat az egy központi helyen – az Azure klasszikus portálra


Ha többet szeretne tudni a szoftver alkalmazás integrálása az Azure AD meg, hogy, ismerje meg, [az access alkalmazás és az egyszeri bejelentkezés az Azure Active Directory címtárral](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek

Azure Active Directory integráció a Ceridian Dayforce táblabeli HCM van szükség az alábbiakat:

- Az Azure Active Directory-előfizetéssel
- Egy Ceridian Dayforce táblabeli HCM egyszeri bejelentkezés engedélyezett előfizetés


> [AZURE.NOTE] Az oktatóprogram lépéseit teszteléséhez nem használata ajánlott munkakörnyezetben.


Az oktatóprogram lépéseit teszteléséhez célszerű követnie az alábbi javaslatokat:

- Erre akkor szükség, ne használja a termelési környezetén.
- Ha nincs telepítve az Azure Active Directory próba környezetben, kattint egy hónap próba [Itt](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban célja lehetővé teszi az Azure Active Directory egyszeri bejelentkezés tesztkörnyezetben tesztelése.  
Az alkalmazási példát, ebben az oktatóanyagban tagolt két fő építőelemek áll:

1. Ceridian Dayforce táblabeli HCM hozzáadása a gyűjteményből
2. Beállítása és tesztelése Azure Active Directory egyszeri bejelentkezés


## <a name="adding-ceridian-dayforce-hcm-from-the-gallery"></a>Ceridian Dayforce táblabeli HCM hozzáadása a gyűjteményből
Konfigurálja az Azure Active Directory integrálása a Ceridian Dayforce táblabeli HCM, meg kell Ceridian Dayforce táblabeli HCM hozzáadása az felügyelt szoftver alkalmazáslistában a gyűjteményből.

**A gyűjteményből Ceridian Dayforce táblabeli HCM felvételéhez hajtsa végre az alábbi lépéseket:**

1. Az **Azure klasszikus portálon**kattintson a bal oldali navigációs területen kattintson az **Active Directory**. 

    ![Az Active Directory][1]

2. A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3. Nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson a felső menüben **alkalmazások** .

    ![Alkalmazások][2]

4. Kattintson a **Hozzáadás** az oldal alján.

    ![Alkalmazások][3]

5. **Milyen feladatot szeretne tenni** párbeszédpanelen kattintson **a gyűjteményből az alkalmazások felvétele**.

    ![Alkalmazások][4]

6. A Keresés mezőbe írja be a **Ceridian Dayforce táblabeli HCM**.

    ![Alkalmazások](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_01.png)

7. Az eredmény ablaktáblában jelölje ki a **Ceridian Dayforce táblabeli HCM**pontra, és kattintson a **kész** az alkalmazás hozzáadása parancsra.

    ![Alkalmazások](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_07.png)


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Beállítása és tesztelése Azure Active Directory egyszeri bejelentkezés
Ez a szakasz célja mutatja, hogy beállítása és Ceridian Dayforce táblabeli HCM "Britta Simon" nevű vizsgálat felhasználón alapuló az Azure Active Directory egyszeri bejelentkezés tesztelése.

Az egyszeri bejelentkezési a munkát az Azure Active Directory kell, hogy mi az a partner felhasználó Ceridian Dayforce táblabeli HCM az Azure Active Directory-felhasználónak. Ez azt jelenti Azure AD-felhasználó, és a kapcsolódó felhasználó Ceridian Dayforce táblabeli HCM hivatkozás viszonya kell létrehozni.

Állítsa be, és Azure Active Directory egyszeri bejelentkezéssel való Ceridian Dayforce táblabeli HCM tesztelése, a következő építőelemek szükséges:

1. **[Azure Active Directory konfigurálása az egyszeri bejelentkezés](#configuring-azure-ad-single-sign-on)** - engedélyezése a felhasználóknak, hogy ezzel a szolgáltatással.
2. **[Felhasználó létrehozása az Azure Active Directory tesztelése](#creating-an-azure-ad-test-user)** - Azure Active Directory egyszeri bejelentkezéssel való Britta Simon ellenőrzéséhez.
4. **[Felhasználó létrehozása egy Ceridian Dayforce táblabeli HCM tesztelése](#creating-a-ceridian-dayforce-hcm-test-user)** - van-e egy megfelelője a Britta Simon Ceridian Dayforce táblabeli HCM, amelyen az Azure Active Directory ábrázolt csatolt.
5. **[Az Azure Active Directory hozzárendelése tesztelje a felhasználó](#assigning-the-azure-ad-test-user)** - ahhoz, hogy Britta Simon Azure AD az egyszeri bejelentkezés használata.
5. **[Tesztelés egyszeri bejelentkezés](#testing-single-sign-on)** - ellenőrzéséhez, hogy működik-e a konfigurációs.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure Active Directory egyszeri bejelentkezés beállítása

Ez a szakasz célja Azure AD az egyszeri bejelentkezés az Azure klasszikus portálon engedélyezése és a Ceridian Dayforce táblabeli HCM alkalmazásban az egyszeri bejelentkezés beállítása.

A Ceridian Dayforce táblabeli HCM alkalmazás a SAML Előfeltételek vár, egy bizonyos formátumban. Kérje Dayforce táblabeli HCM csoport először azonosítsa a megfelelő felhasználói azonosítója. A Microsoft javasolja a **"név"** attribútum felhasználói azonosítóként. Az érték az **"Atrribute"** párbeszédpanelen az attribútum kezelheti. Az alábbi képernyőképen látható, a. 

![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_02.png) 

**Állítson be Azure AD az egyszeri bejelentkezés Ceridian Dayforce táblabeli HCM, hajtsa végre az alábbi lépéseket:**




1. Az Azure klasszikus portálon **Ceridian Dayforce táblabeli HCM** alkalmazás integrációs lapján a menüben, a képernyő tetején kattintson a **attribútumok**elemre.

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_81.png) 


1. Attribútumok **saml jogkivonat attribútumok** listájában jelölje be a név attribútum, és kattintson a **Szerkesztés**gombra.

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_06.png) 


1. A **Felhasználó attribútum szerkesztése** párbeszédpanelen végezze el az alábbi lépéseket:
 
    egy. Az **Attribútumérték** listából válassza ki a felhasználó attribútum a végrehajtásához használni kívánt.  
    Például ha szeretné használni az Alkalmazottkód oszlop egyedi felhasználói azonosító, és a ExtensionAttribute2 az attribútumérték tárolt, majd válassza a **user.extensionattribute2**. 

    b. Kattintson a **kész**gombra.  
    



1. A felső sávon kattintson a **Quick Start**.

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-hightail-tutorial/tutorial_general_83.png)  





1. Az Azure klasszikus portálon **Ceridian Dayforce táblabeli HCM** alkalmazás integrációs lapon kattintson a **konfigurálása az egyszeri bejelentkezés**.

    ![Egyszeri bejelentkezés beállítása][6] 

2. **Hogyan szeretné, hogy jelentkezzen be az Ceridian Dayforce táblabeli HCM felhasználók** lapon válassza az **Azure Active Directory Single Sign-On**, és kattintson a **Tovább gombra**.

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_03.png) 

3. Az **Alkalmazás beállításainak megadása** párbeszédpanel lapon hajtsa végre az alábbi lépéseket:.

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_04.png) 


    egy. **Jelentkezzen be URL-cím** mezőbe írja be a felhasználóknak, hogy a bejelentkezés az Ceridian Dayforce táblabeli HCM alkalmazás által használt URL-CÍMÉT. Gyártási környezetekben használja az alábbi URL-formátuma:`https://sso.dayforcehcm.com/DayforcehcmNamespace`

    Áttekinthetőség cserélje DayforcehcmNamespace a környezet és a vállalat azonosítója; névtere Példa:`https://sso.dayforcehcm.com/contoso`
    
    Próba-környezetekkel használja az alábbi URL-formátuma:`https://ssotest.dayforcehcm.com/DayforcehcmNamespace` 

    b. **Válasz URL-cím** mezőbe írja be a választ a közzétételi Azure Active Directory által használt URL-CÍMÉT.  
    A gyártási környezetekben használhatja:`https://ncpingfederate.dayforcehcm.com/sp/ACS.saml2`  
    Próba-környezetben használja:`https://fs-test.dayforcehcm.com/sp/ACS.saml2`  
   

4. A **Configure egyszeri bejelentkezés Ceridian Dayforce táblabeli HCM a** lapon hajtsa végre az alábbi lépéseket:

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_05.png) 

    egy. Kattintson a **tanúsítvány letöltése**, és mentse a fájlt a számítógépen.

    b. Kattintson a **Tovább**gombra.


5. Ha be van állítva az alkalmazás SSO, a Ceridian Dayforce táblabeli HCM támogatási csoport munkatársaitól mailben, és küldje el nekik a következő:

    - A letöltött tanúsítvány-fájl
    - A **kibocsátó URL-címe**
    - A **SAML egyszeri bejelentkezési URL-címe** 
    - A **szolgáltatás URL-címe egyetlen kijelentkezés** 


6. Az Azure klasszikus portálon válassza ki az egyszeri bejelentkezéses konfigurációs megerősítő, és kattintson a **Tovább gombra**.

    ![Azure Active Directory Single Sign-On][10]

7. Az **egyszeri bejelentkezés megerősítés** lapon kattintson a **Befejezés**gombra.  

    ![Azure Active Directory Single Sign-On][11]



### <a name="creating-an-azure-ad-test-user"></a>Azure AD-próba felhasználó létrehozása
Ez a szakasz célja próba felhasználó létrehozása az Azure-Britta Simon nevű klasszikus portálon.

![Azure Active Directory-felhasználó létrehozása][20]

**Teszt felhasználó létrehozása az Azure Active Directory, hajtsa végre az alábbi lépéseket:**

1. Az **Azure klasszikus portálon**kattintson a bal oldali navigációs területen kattintson az **Active Directory**.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-ceridiandayforcehcm-tutorial/create_aaduser_09.png) 

2. A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3. A menüben, a képernyő tetején, a felhasználók listájának megjelenítéséhez kattintson a **felhasználók**elemre.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-ceridiandayforcehcm-tutorial/create_aaduser_03.png) 

4. **Felhasználó hozzáadása**elemre az alsó eszköztáron a **Felhasználó hozzáadása** párbeszédpanel megnyitásához.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-ceridiandayforcehcm-tutorial/create_aaduser_04.png) 

5. A párbeszédpanel **közli velünk a felhasználóra vonatkozó** lapon hajtsa végre az alábbi lépéseket:

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-ceridiandayforcehcm-tutorial/create_aaduser_05.png) 

    egy. Felhasználó típusa jelölje be az új felhasználó a szervezet.

    b. A felhasználónév **szövegmezőbe**írja be a **BrittaSimon**.

    c billentyűkombinációt. Kattintson a **Tovább**gombra.

6.  A **Felhasználói profil** párbeszédpanel lapon hajtsa végre az alábbi lépéseket:

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-ceridiandayforcehcm-tutorial/create_aaduser_06.png) 

    egy. Az **első neve** mezőbe írja be a **Britta**.  

    b. Az **Utolsó neve** mezőbe írja be, **Simon**.

    c billentyűkombinációt. A **Megjelenítendő név** mezőbe írja be a **Britta Simon**.

    d. A **szerepkör** listában jelölje ki a **felhasználót**.

    e. Kattintson a **Tovább**gombra.

7. A párbeszédpanel **ideiglenes jelszót kérjen** lapon kattintson a **Hozzon létre**.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-ceridiandayforcehcm-tutorial/create_aaduser_07.png) 

8. A párbeszédpanel **ideiglenes jelszót első** lapon hajtsa végre az alábbi lépéseket:

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-ceridiandayforcehcm-tutorial/create_aaduser_08.png) 

    egy. Jegyezze fel az **Új jelszót**az érték.

    b. Kattintson a **kész**gombra.   



### <a name="creating-a-ceridian-dayforce-hcm-test-user"></a>Ceridian Dayforce táblabeli HCM próba felhasználó létrehozása

Ez a szakasz célja Britta Simon nevű Ceridian Dayforce táblabeli HCM felhasználó létrehozása. 

Kérje meg a Ceridian Dayforce táblabeli HCM támogatási csoporthoz, hogy a felhasználók hozzá a Ceridian Dayforce táblabeli HCM alkalmazásban. 





### <a name="assigning-the-azure-ad-test-user"></a>Az Azure Active Directory-próba felhasználói hozzárendelése

Ez a szakasz célja Britta Simon Azure egyszeri bejelentkezés a saját hozzáférést biztosít Ceridian Dayforce táblabeli HCM használandó engedélyezése.

![Felhasználó hozzárendelése][200] 

**Britta Simon hozzárendelése Ceridian Dayforce táblabeli HCM, hajtsa végre az alábbi lépéseket:**

1. Az Azure klasszikus portál kattintva nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson az **alkalmazások** parancsra a felső menüben.

    ![Felhasználó hozzárendelése][201] 

2. Az alkalmazások listájában jelölje ki a **Ceridian Dayforce táblabeli HCM**.

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_50.png) 

1. A felső sávon kattintson a **felhasználók**elemre.

    ![Felhasználó hozzárendelése][203] 

1. A felhasználók listában jelölje ki a **Britta Simon**.

2. Az alsó eszköztáron kattintson a **hozzárendelni**.

    ![Felhasználó hozzárendelése][205]



### <a name="testing-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

Ez a szakasz célja a Azure Active Directory egyszeri bejelentkezéses beállítások tesztelése a Access panelen.  
Ha az Access panel a Ceridian Dayforce táblabeli HCM mozaik gombra kattint, meg kell első automatikusan aláírt-on az Ceridian Dayforce táblabeli HCM alkalmazás.


## <a name="additional-resources"></a>További források

* [A szoftver alkalmazások integrálása az Azure Active Directory oktatóanyagok listája](active-directory-saas-tutorial-list.md)
* [Mi az access alkalmazás és az egyszeri bejelentkezés az Azure Active Directory címtárral?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_205.png
