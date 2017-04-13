<properties
    pageTitle="Oktatóprogram: Azure Active Directory-integráció a QuickHelp |} Microsoft Azure"
    description="Egyszeri bejelentkezés között az Azure Active Directory és QuickHelp konfigurálásának ismertetése."
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
    ms.date="08/16/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-quickhelp"></a>Oktatóprogram: Azure Active Directory-integráció a QuickHelp

Ebben az oktatóanyagban célja mutatja, hogy miként QuickHelp integrálása az Azure Active Directory (Azure Active Directory).  
QuickHelp integrálása az Azure Active Directory biztosít a következő előnyökkel jár: 

- Az Azure Active Directory, aki rendelkezik hozzáféréssel QuickHelp megadhatja, hogy 
- Engedélyezheti a felhasználóknak, hogy automatikusan első jelentkezett-on való QuickHelp (Single Sign-On) az Azure Active Directory-fiókok
- A fiókokat az egy központi helyen – az Azure klasszikus portálra

Ha többet szeretne tudni a szoftver alkalmazás integrálása az Azure AD meg, hogy, ismerje meg, [az access alkalmazás és az egyszeri bejelentkezés az Azure Active Directory címtárral](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek 

Azure Active Directory integráció a QuickHelp van szükség az alábbiakat:

- Az Azure Active Directory-előfizetéssel
- Egy QuickHelp egyszeri bejelentkezés engedélyezett előfizetés


> [AZURE.NOTE] Az oktatóprogram lépéseit teszteléséhez nem használata ajánlott munkakörnyezetben.


Az oktatóprogram lépéseit teszteléséhez célszerű követnie az alábbi javaslatokat:

- Erre akkor szükség, ne használja a termelési környezetén.
- Ha nincs telepítve az Azure Active Directory próba környezetben, kattint egy hónap próba [Itt](https://azure.microsoft.com/pricing/free-trial/). 

 
## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban célja lehetővé teszi az Azure Active Directory egyszeri bejelentkezés tesztkörnyezetben tesztelése.  
Az alkalmazási példát, ebben az oktatóanyagban tagolt két fő építőelemek áll:

1. QuickHelp hozzáadása a gyűjteményből 
2. Beállítása és tesztelése Azure Active Directory egyszeri bejelentkezés


## <a name="adding-quickhelp-from-the-gallery"></a>QuickHelp hozzáadása a gyűjteményből
Konfigurálja az Azure Active Directory integrálása a QuickHelp, meg kell QuickHelp hozzáadása az felügyelt szoftver alkalmazáslistában a gyűjteményből.

**A gyűjteményből QuickHelp felvételéhez hajtsa végre az alábbi lépéseket:**

1. Az **Azure klasszikus portálon**kattintson a bal oldali navigációs területen kattintson az **Active Directory**. 

    ![Az Active Directory][1]

2. A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3. Nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson a felső menüben **alkalmazások** .

    ![Alkalmazások][2]

4. Kattintson a **Hozzáadás** az oldal alján.

    ![Alkalmazások][3]

5. **Milyen feladatot szeretne tenni** párbeszédpanelen kattintson **a gyűjteményből az alkalmazások felvétele**.

    ![Alkalmazások][4]

6. A Keresés mezőbe írja be a **QuickHelp**.

    ![Alkalmazások][5]

7. Az eredmény ablaktáblában jelölje ki a **QuickHelp**, és válassza a **kész** , az alkalmazás hozzáadása gombra.

    ![Alkalmazások][500]


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Beállítása és tesztelése Azure Active Directory egyszeri bejelentkezés
Ez a szakasz célja mutatja, hogy állítsa be, és a "Britta Simon" nevű próba felhasználón alapuló QuickHelp Azure AD az egyszeri bejelentkezés tesztelése.


Állítsa be, és Azure Active Directory egyszeri bejelentkezéssel való QuickHelp tesztelése, a következő építőelemek szükséges:

1. **[Azure Active Directory konfigurálása az egyszeri bejelentkezés](#configuring-azure-ad-single-single-sign-on)** - engedélyezése a felhasználóknak, hogy ezzel a szolgáltatással.
2. **[Felhasználó létrehozása az Azure Active Directory tesztelése](#creating-an-azure-ad-test-user)** - Azure Active Directory egyszeri bejelentkezéssel való Britta Simon ellenőrzéséhez.
4. **[Felhasználó létrehozása egy QuickHelp tesztelése](#creating-a-quickhelp-test-user)** - van-e egy megfelelője a Britta Simon QuickHelp, amelyen az Azure Active Directory ábrázolt csatolt.
5. **[Az Azure Active Directory hozzárendelése tesztelje a felhasználó](#assigning-the-azure-ad-test-user)** - ahhoz, hogy Britta Simon Azure AD az egyszeri bejelentkezés használata.
5. **[Tesztelés egyszeri bejelentkezés](#testing-single-sign-on)** - ellenőrzéséhez, hogy működik-e a konfigurációs.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure Active Directory Single Sign-On konfigurálása

Ez a szakasz célja Azure AD az egyszeri bejelentkezés az Azure klasszikus portálon engedélyezése és a QuickHelp alkalmazásban az egyszeri bejelentkezés beállítása.


**Állítson be Azure AD az egyszeri bejelentkezés QuickHelp, hajtsa végre az alábbi lépéseket:**

1. Az Azure klasszikus portálon **QuickHelp** alkalmazás integrációs lapon kattintson **konfigurálása az egyszeri bejelentkezés** a **Konfigurálása az egyszeri bejelentkezés** párbeszédpanel megnyitásához.

    ![Egyszeri bejelentkezés beállítása][6] 

2. **Hogyan szeretné, hogy jelentkezzen be az QuickHelp felhasználók** lapon válassza az **Azure Active Directory Single Sign-On**, és kattintson a **Tovább gombra**.

    ![Azure Active Directory Single Sign-On][7] 

3. Az **Alkalmazás beállításainak megadása** párbeszédpanel lapon hajtsa végre az alábbi lépéseket:

    ![Alkalmazás beállításainak konfigurálása][8] 
 
     egy. **Jelentkezzen be URL-cím** mezőbe írja be a felhasználóknak, hogy bejelentkezés QuickHelp webhelyéhez által használt URL-CÍMÉT (például:* https://quickhelp.com/bsiazure/*).

     > [AZURE.NOTE] Ha nem tudja, hogy az érték, a bejelentkezési a URL-címének, forduljon a QuickHelp támogatási csoporthoz.

     b. Kattintson a **Tovább**gombra.

 
4. **Beállítás az egyszeri bejelentkezés a QuickHelp** lapon hajtsa végre az alábbi lépéseket: kattintson a **metaadat-alapú letöltése**, és mentse a fájlt a számítógépen helyben metaadat.

    ![Mi az Azure AD Connect][9] 



1. Bejelentkezés a QuickHelp vállalati webhelyre rendszergazdaként.

2. A felső sávon kattintson a **rendszergazda**.

    ![Egyszeri bejelentkezés beállítása][21]


1. A **QuickHelp Admin** menüben kattintson a **Beállítások**gombra.

    ![Egyszeri bejelentkezés beállítása][22]

1. **Hitelesítési beállítások**gombra.

1. **Hitelesítési beállítások** lapján hajtsa végre a következő lépések

    ![Egyszeri bejelentkezés beállítása][23]

    egy. **Egyszeri bejelentkezés típusa**jelölje ki a **WSFederation**.

    b. A letöltött Azure metaadatok fájl feltöltéséhez kattintson a **Tallózás gombra**, keresse meg a fájlt, befejezése, majd kattintson a **Metaadatok feltöltése**.

    c billentyűkombinációt. Az **E-mail** mezőbe írja be a **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**.

    d. Az **Utónév** mezőbe **Írja be http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname**.

    e. Az **Utónév** mezőbe **Írja be http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname**

    f. A **Műveletsáv**kattintson a **Mentés**gombra.







6. Az Azure klasszikus portálon jelölje be a egyszeri bejelentkezéses konfigurációs megerősítés, és kattintson a **Tovább gombra**. 

    ![Mi az Azure AD Connect][10]

7. Az **egyszeri bejelentkezés megerősítés** lapon kattintson a **Befejezés**gombra.  

    ![Mi az Azure AD Connect][11]




### <a name="creating-an-azure-ad-test-user"></a>Azure AD-próba felhasználó létrehozása
Ez a szakasz célja próba felhasználó létrehozása az Azure-Britta Simon nevű klasszikus portálon.  
A felhasználók listában jelölje ki a **Britta Simon**.

![Azure Active Directory-felhasználó létrehozása][20]

**Teszt felhasználó létrehozása az Azure Active Directory, hajtsa végre az alábbi lépéseket:**

1. Az **Azure klasszikus portálon**kattintson a bal oldali navigációs területen kattintson az **Active Directory**.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-quickhelp-tutorial/create_aaduser_02.png) 

2. A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3. A menüben, a képernyő tetején, a felhasználók listájának megjelenítéséhez kattintson a **felhasználók**elemre.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-quickhelp-tutorial/create_aaduser_03.png) 
 
4. **Felhasználó hozzáadása**elemre az alsó eszköztáron a **Felhasználó hozzáadása** párbeszédpanel megnyitásához. 

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-quickhelp-tutorial/create_aaduser_04.png) 

5. A párbeszédpanel **közli velünk a felhasználóra vonatkozó** lapon hajtsa végre az alábbi lépéseket: 
 
    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-quickhelp-tutorial/create_aaduser_05.png) 

    egy. Felhasználó típusa jelölje be az új felhasználó a szervezet.

    b. A felhasználónév **szövegmezőbe**írja be a **BrittaSimon**.

    c billentyűkombinációt. Kattintson a **Tovább**gombra.

6.  A **Felhasználói profil** párbeszédpanel lapon hajtsa végre az alábbi lépéseket: 

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-quickhelp-tutorial/create_aaduser_06.png) 
 
    egy. Az **első neve** mezőbe írja be a **Britta**.  

    b. Az **Utolsó neve** mezőbe írja be, **Simon**.

    c billentyűkombinációt. A **Megjelenítendő név** mezőbe írja be a **Britta Simon**.

    d. A **szerepkör** listában jelölje ki a **felhasználót**.
    e. Kattintson a **Tovább**gombra.

7. A párbeszédpanel **ideiglenes jelszót első** lapon kattintson a **Hozzon létre**.

![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-quickhelp-tutorial/create_aaduser_07.png) 
 
8. A párbeszédpanel **ideiglenes jelszót első** lapon hajtsa végre az alábbi lépéseket:

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-quickhelp-tutorial/create_aaduser_08.png) 
  
    egy. Jegyezze fel az **Új jelszót**az érték.

    b. Kattintson a **kész**gombra.   

  
 
### <a name="creating-a-quickhelp-test-user"></a>QuickHelp próba felhasználó létrehozása

Ez a szakasz célja Britta Simon nevű QuickHelp felhasználó létrehozása.
Az egyszeri bejelentkezési a munkát az Azure Active Directory kell, hogy mi az a partner felhasználó QuickHelp az Azure Active Directory-felhasználónak. Ez azt jelenti Azure AD-felhasználó, és a kapcsolódó felhasználó QuickHelp hivatkozás viszonya kell létrehozni.

QuickHelp támogatja a kiépítési közvetlenül az idő. Ez azt jelenti, ha szükséges, a felhasználói fiók automatikusan létrejön QuickHelp és a fiók kapcsolódik az Azure Active Directory-fiókjába.

Nincs teendő ebben a szakaszban a meg nem.


### <a name="assigning-the-azure-ad-test-user"></a>Az Azure Active Directory-próba felhasználói hozzárendelése

Ez a szakasz célja Britta Simon Azure egyszeri bejelentkezés a saját hozzáférést biztosít QuickHelp használandó engedélyezése.

![Felhasználó hozzárendelése][200] 

**Britta Simon hozzárendelése QuickHelp, hajtsa végre az alábbi lépéseket:**

1. Az Azure klasszikus portál kattintva nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson az **alkalmazások** parancsra a felső menüben.

    ![Felhasználó hozzárendelése][201] 

2. Az alkalmazások listájában jelölje ki a **QuickHelp**.

    ![Felhasználó hozzárendelése][202] 

1. A felső sávon kattintson a **felhasználók**elemre.

    ![Felhasználó hozzárendelése][203] 

1. A felhasználók listában jelölje ki a **Britta Simon**.

2. Az alsó eszköztáron kattintson a **hozzárendelni**.

    ![Felhasználó hozzárendelése][205]



### <a name="testing-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

Ez a szakasz célja a Azure Active Directory egyszeri bejelentkezéses beállítások tesztelése a Access panelen.  
Amikor az Access panel a QuickHelp mozaik gombra kattint, meg kell első automatikusan aláírt-on az QuickHelp alkalmazás.


## <a name="additional-resources"></a>További források

* [A szoftver alkalmazások integrálása az Azure Active Directory oktatóanyagok listája](active-directory-saas-tutorial-list.md)
* [Mi az access alkalmazás és az egyszeri bejelentkezés az Azure Active Directory címtárral?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_general_04.png
[5]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_quickhelp_01.png
[500]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_quickhelp_14.png


[6]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_general_05.png
[7]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_quickhelp_02.png
[8]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_quickhelp_03.png
[9]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_quickhelp_04.png
[10]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_general_100.png
[21]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_quickhelp_05.png
[22]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_quickhelp_06.png
[23]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_quickhelp_07.png
[24]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_quickhelp_08.png
[25]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_quickhelp_09.png
[26]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_quickhelp_10.png
[27]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_quickhelp_11.png
[28]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_quickhelp_12.png

[200]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_quickhelp_13.png
[203]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_general_205.png


[400]: ./media/active-directory-saas-QuickHelp-tutorial/tutorial_QuickHelp_400.png
[401]: ./media/active-directory-saas-QuickHelp-tutorial/tutorial_QuickHelp_401.png
[402]: ./media/active-directory-saas-QuickHelp-tutorial/tutorial_QuickHelp_402.png




