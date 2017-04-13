<properties
    pageTitle="Oktatóprogram: Azure Active Directory-integráció a által Merces HR2day |} Microsoft Azure"
    description="Megtudhatja, hogy miként konfigurálása az egyszeri bejelentkezés Azure Active Directory és HR2day által Merces között."
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


# <a name="tutorial-azure-active-directory-integration-with-hr2day-by-merces"></a>Oktatóprogram: Azure Active Directory-integráció a HR2day Merces szerint

Ebben az oktatóanyagban célja mutatja, hogy hogyan által Merces HR2day integrálása az Azure Active Directory (Azure Active Directory).  
HR2day által Merces integrálása az Azure Active Directory biztosít a következő előnyökkel jár:

- Az Azure Active Directory által Merces HR2day hozzáféréssel rendelkező személyek megadhatja, hogy
- Engedélyezheti a felhasználóknak, hogy automatikusan első jelentkezett-on való HR2day Merces (Single Sign-On) szerint az Azure Active Directory-fiókok
- A fiókokat az egy központi helyen – az Azure klasszikus portálra

Ha többet szeretne tudni a szoftver alkalmazás integrálása az Azure AD meg, hogy, ismerje meg, [az access alkalmazás és az egyszeri bejelentkezés az Azure Active Directory címtárral](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek

Azure Active Directory integráció a HR2day által Merces van szükség az alábbiakat:

- Az Azure Active Directory-előfizetéssel
- Egy HR2day Merces egyszeri bejelentkezés engedélyezett előfizetéshez tartozó szerint


> [AZURE.NOTE] Az oktatóprogram lépéseit teszteléséhez nem használata ajánlott munkakörnyezetben.


Az oktatóprogram lépéseit teszteléséhez célszerű követnie az alábbi javaslatokat:

- Erre akkor szükség, ne használja a termelési környezetén.
- Ha nincs telepítve az Azure Active Directory próba környezetben, kattint egy hónap próba [Itt](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban célja lehetővé teszi az Azure Active Directory egyszeri bejelentkezés tesztkörnyezetben tesztelése.  
Az alkalmazási példát, ebben az oktatóanyagban tagolt két fő építőelemek áll:

1. HR2day által Merces hozzáadása a gyűjteményből
2. Beállítása és tesztelése Azure Active Directory egyszeri bejelentkezés


## <a name="adding-hr2day-by-merces-from-the-gallery"></a>HR2day által Merces hozzáadása a gyűjteményből
Azure Active Directory integrálása a Merces által HR2day konfigurálásához szüksége HR2day Merces által felügyelt szoftver alkalmazásai a gyűjteményből hozzáadása.

**A gyűjteményből által Merces HR2day felvételéhez hajtsa végre az alábbi lépéseket:**

1. Az **Azure klasszikus portálon**kattintson a bal oldali navigációs területen kattintson az **Active Directory**. 

    ![Az Active Directory][1]

2. A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3. Nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson a felső menüben **alkalmazásokat** .
 
    ![Alkalmazások][2]

4. Kattintson a **Hozzáadás** az oldal alján.

    ![Alkalmazások][3]

5. **Milyen feladatot szeretne tenni** párbeszédpanelen kattintson **a gyűjteményből az alkalmazások felvétele**.

    ![Alkalmazások][4]

6. A Keresés mezőbe írja be a **HR2day Merces szerint**.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-hr2day-tutorial/tutorial_hr2day_01.png)

7. Az eredmény ablaktáblában jelölje ki a **HR2day Merces által**, és válassza a **kész** , az alkalmazás hozzáadása gombra.


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Beállítása és tesztelése Azure Active Directory egyszeri bejelentkezés
Ez a szakasz célja mutatja, hogy beállítása és tesztelése Azure AD az egyszeri bejelentkezés a "Britta Simon" nevű próba felhasználón alapuló Merces által HR2day.

Az egyszeri bejelentkezési a munkát az Azure Active Directory kell, hogy mi az a megfelelőjük a felhasználó által Merces HR2day az Azure Active Directory-felhasználónak. Más szóval Azure AD-felhasználó, és a kapcsolódó felhasználó által Merces HR2day hivatkozás viszonya kell létrehozni.  
A hivatkozás kapcsolat jön létre által az Azure AD a program által Merces HR2day **felhasználónevét** a **felhasználónév** értéket rendeli.

Állítsa be, és Azure Active Directory egyszeri bejelentkezés HR2day Merces által a tesztelése, a következő építőelemek szükséges:

1. **[Azure Active Directory konfigurálása az egyszeri bejelentkezés](#configuring-azure-ad-single-single-sign-on)** - engedélyezése a felhasználóknak, hogy ezzel a szolgáltatással.
2. **[Felhasználó létrehozása az Azure Active Directory tesztelése](#creating-an-azure-ad-test-user)** - Azure Active Directory egyszeri bejelentkezéssel való Britta Simon ellenőrzéséhez.
4. **[Felhasználó által Merces egy HR2day létrehozása tesztelése](#creating-a-hr2day-by-merces-test-user)** - van-e egy megfelelője a Britta Simon HR2day Merces, amelyen az Azure Active Directory ábrázolt csatolt szerint.
5. **[Az Azure Active Directory hozzárendelése tesztelje a felhasználó](#assigning-the-azure-ad-test-user)** - ahhoz, hogy Britta Simon Azure AD az egyszeri bejelentkezés használata.
5. **[Tesztelés egyszeri bejelentkezés](#testing-single-sign-on)** - ellenőrzéséhez, hogy működik-e a konfigurációs.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure Active Directory Single Sign-On konfigurálása

Ez a szakasz célja tagolása engedélyezése a felhasználók által Merces HR2day hitelesíteni a fiókkal az összevonási alapján a SAML protokoll használatával Azure AD.

A HR2day Merces alkalmazás a SAML Előfeltételek vár, egy bizonyos formátumban, amelyhez egyéni attribútum megfeleltetések hozzáadása a SAML jogkivonat attribútumok konfigurációs. Az alábbi képernyőképen látható, a. 

![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-hr2day-tutorial/tutorial_hr2day_00.png) 

Beállíthatja, hogy a SAML állítás, mielőtt be kell-e a HR2day támogatási csoport munkatársaitól keresztül [servicedesk@merces.nl](mailto:servicedesk@merces.nl) , és kérelmezze az egyedi azonosító attribútum értékét a bérlői webhelyen. Ezt az értéket a következő szakaszban szereplő lépéseket befejezéséhez szükséges.


**Állítson be Azure AD az egyszeri bejelentkezés által Merces HR2day, hajtsa végre az alábbi lépéseket:**

1. Kattintson az Azure klasszikus portal **által Merces HR2day** alkalmazás integrációs lapján a menüben, a képernyő tetején, **attribútumok** a **SAML jogkivonat attribútumok** párbeszédpanel megnyitásához. 

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-hr2day-tutorial/tutorial_hr2day_06.png) 

2. Kötelező attribútum megfeleltetések beszúrásához hajtsa végre az alábbi lépéseket, hajtsa végre az alábbi lépéseket: 

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-hr2day-tutorial/tutorial_hr2day_07.png) 


    egy. Kattintson a **felhasználó attribútum hozzáadása**gombra.

    b. Az **Attribútum neve** mezőbe írja be a **"ATTR_LOGINCLAIM"**.

    c billentyűkombinációt. Az **Attribútumérték** listából válassza ki a **Join()**. 

    d. A **szöveg1** listából válassza ki a **User.mail**. 

    e. **Karakterlánc2** mezőbe írja be a HR2day csoport által megadott **egyedi azonosítója** . 

    f. Az **elválasztó** mezőbe írja be a **@**.

    g. Kattintson a **kész**gombra.

  
3. Kattintson a **módosítások alkalmazásához**.


1. A felső sávon kattintson a **Rövid útmutató** az **Első lépések** párbeszédpanel megnyitásához.

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-hr2day-tutorial/tutorial_general_08.png) 



1. Kattintson a **Konfigurálás egyszeri bejelentkezés a** a **Konfigurálása az egyszeri bejelentkezés** párbeszédpanel megnyitásához.

    ![Egyszeri bejelentkezés beállítása][6] 

2. **Hogyan szeretné, hogy jelentkezzen be az HR2day Merces által a felhasználók** lapon válassza az **Azure Active Directory Single Sign-On**, és kattintson a **Tovább gombra**.

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-hr2day-tutorial/tutorial_hr2day_03.png) 

3. Az **Alkalmazás beállításainak megadása** párbeszédpanel lapon hajtsa végre az alábbi lépéseket: 

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-hr2day-tutorial/tutorial_hr2day_04.png) 


    egy. Bejelentkezési a URL-cím mezőbe írja be a következő mintát Merces alkalmazást a felhasználóknak, hogy bejelentkezésre a HR2day által használt URL-címe: **"https://\<bérlői neve\>.force.com/\<példányának neve\>"**.

    b. Kattintson a **Tovább**gombra.

4. A **beállítás az egyszeri bejelentkezés HR2day Merces által a** lapon hajtsa végre az alábbi lépéseket:

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-hr2day-tutorial/tutorial_hr2day_05.png) 

    egy. Kattintson a **tanúsítvány letöltése**, és mentse a fájlt a számítógépen.

    b. Kattintson a **Tovább**gombra.


5. Ha be van állítva az alkalmazás SSO, lépjen kapcsolatba a HR2day Merces támogatási csoport keresztül [servicedesk@merces.nl](emailTo:servicedesk@merces.nl) és a letöltött tanúsítvány-fájl csatolása e-mailhez. Is adja meg a SAML egyszeri bejelentkezési URL-CÍMÉT, a bejelentkezési meg URL-CÍMÉT és a kibocsátó URL-CÍMÉT, hogy egyszeri bejelentkezési integrációs beállíthatók.


> [AZURE.NOTE] Kérjük, említeni Merces csoportnak, hogy ez az integráció a minta **https://hr2day.force.com/INSTANCENAME** beállítandó entitás Azonosítóval kell rendelkeznie



6. Az Azure klasszikus portálon válassza ki az egyszeri bejelentkezéses konfigurációs megerősítő, és kattintson a **Tovább gombra**.

    ![Azure Active Directory Single Sign-On][10]

7. Az **egyszeri bejelentkezés megerősítés** lapon kattintson a **Befejezés**gombra.  

    ![Azure Active Directory Single Sign-On][11]



### <a name="creating-an-azure-ad-test-user"></a>Azure AD-próba felhasználó létrehozása
Ez a szakasz célja vizsgálat felhasználó létrehozása az Azure-Britta Simon nevű klasszikus portálon.  

![Azure Active Directory-felhasználó létrehozása][20]

**Teszt felhasználó létrehozása az Azure Active Directory, hajtsa végre az alábbi lépéseket:**

1. Az **Azure klasszikus portálon**kattintson a bal oldali navigációs területen kattintson az **Active Directory**.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-hr2day-tutorial/create_aaduser_09.png) 

2. A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3. A menüben, a képernyő tetején, a felhasználók listájának megjelenítéséhez kattintson a **felhasználók**elemre.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-hr2day-tutorial/create_aaduser_03.png) 

4. **Felhasználó hozzáadása**elemre az alsó eszköztáron a **Felhasználó hozzáadása** párbeszédpanel megnyitásához.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-hr2day-tutorial/create_aaduser_04.png) 

5. A párbeszédpanel **közli velünk a felhasználóra vonatkozó** lapon hajtsa végre az alábbi lépéseket:

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-hr2day-tutorial/create_aaduser_05.png) 

    egy. Felhasználó típusa jelölje be az új felhasználó a szervezet.

    b. A felhasználónév **szövegmezőbe**írja be a **BrittaSimon**.

    c billentyűkombinációt. Kattintson a **Tovább**gombra.

6.  A **Felhasználói profil** párbeszédpanel lapon hajtsa végre az alábbi lépéseket:

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-hr2day-tutorial/create_aaduser_06.png) 

    egy. Az **első neve** mezőbe írja be a **Britta**.  

    b. Az **Utolsó neve** mezőbe írja be, **Simon**.

    c billentyűkombinációt. A **Megjelenítendő név** mezőbe írja be a **Britta Simon**.

    d. A **szerepkör** listában jelölje ki a **felhasználót**.

    e. Kattintson a **Tovább**gombra.

7. A párbeszédpanel **ideiglenes jelszót kérjen** lapon kattintson a **Hozzon létre**.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-hr2day-tutorial/create_aaduser_07.png) 

8. A párbeszédpanel **ideiglenes jelszót első** lapon hajtsa végre az alábbi lépéseket:

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-hr2day-tutorial/create_aaduser_08.png) 

    egy. Jegyezze fel az **Új jelszót**az érték.

    b. Kattintson a **kész**gombra.   



### <a name="creating-a-hr2day-by-merces-test-user"></a>Egy HR2day Merces vizsgálat felhasználó létrehozása

Ez a szakasz célja Britta Simon hívja meg a HR2day Merces felhasználó létrehozása. Kérje HR2day Merces támogatási csoport a felhasználók felvétele az HR2day fiók. 


> [AZURE.NOTE]Ha egy felhasználó létrehozása manuálisan kell, kapcsolatba kell lépnie a HR2day Merces támogatási csoport által.


### <a name="assigning-the-azure-ad-test-user"></a>Az Azure Active Directory-próba felhasználói hozzárendelése

Ez a szakasz célja Britta Simon Azure egyszeri bejelentkezés a saját hozzáférést biztosít HR2day Merces által használandó engedélyezése.

![Felhasználó hozzárendelése][200] 

**Britta Simon hozzárendelése által Merces HR2day, hajtsa végre az alábbi lépéseket:**

1. Az Azure klasszikus portál kattintva nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson az **alkalmazások** parancsra a felső menüben.

    ![Felhasználó hozzárendelése][201] 

2. Az alkalmazások listájában jelölje ki a **HR2day Merces szerint**.

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-hr2day-tutorial/tutorial_hr2day_50.png) 

1. A felső sávon kattintson a **felhasználók**elemre.

    ![Felhasználó hozzárendelése][203] 

1. A felhasználók listában jelölje ki a **Britta Simon**.

2. Az alsó eszköztáron kattintson a **hozzárendelni**.

    ![Felhasználó hozzárendelése][205]



### <a name="testing-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

Ez a szakasz célja a Azure Active Directory egyszeri bejelentkezéses beállítások tesztelése a Access panelen.  
A HR2day által Merces csempe az Access panel kattintáskor, kell első automatikusan aláírt-a a HR2day Merces alkalmazásával.


## <a name="additional-resources"></a>További források

* [A szoftver alkalmazások integrálása az Azure Active Directory oktatóanyagok listája](active-directory-saas-tutorial-list.md)
* [Mi az access alkalmazás és az egyszeri bejelentkezés az Azure Active Directory címtárral?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_205.png
