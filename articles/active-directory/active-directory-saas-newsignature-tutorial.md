<properties
    pageTitle="Oktatóprogram: Azure Active Directory-integráció a Felhőbeli adatkezelési portál a Microsoft Azure |} Microsoft Azure"
    description="Megtudhatja, hogy miként konfigurálása az egyszeri bejelentkezés Azure Active Directory és a Felhőbeli adatkezelési portál a Microsoft Azure között."
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


# <a name="tutorial-azure-active-directory-integration-with-cloud-management-portal-for-microsoft-azure"></a>Oktatóprogram: Azure Active Directory-integráció a Microsoft Azure-Felhőbeli adatkezelési portál

Ebben az oktatóanyagban célja mutatja, hogy a Microsoft Azure Felhőbeli adatkezelési portál integrálása az Azure Active Directory (Azure Active Directory).  
A Microsoft Azure Felhőbeli adatkezelési portál integrálása az Azure Active Directory biztosít a következő előnyökkel jár:

- Az Azure Active Directory, aki rendelkezik hozzáféréssel a Microsoft Azure Felhőbeli adatkezelési portál megadhatja, hogy
- Az Azure Active Directory-fiókok engedélyezheti a felhasználóknak, hogy automatikusan első aláírással a Felhőbeli adatkezelési portálra a Microsoft Azure (Single Sign-On)
- A fiókokat az egy központi helyen – az Azure klasszikus portálra


Ha többet szeretne tudni a szoftver alkalmazás integrálása az Azure AD meg, hogy, ismerje meg, [az access alkalmazás és az egyszeri bejelentkezés az Azure Active Directory címtárral](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek

Azure Active Directory integráció a Felhőbeli adatkezelési portál a Microsoft Azure van szükség az alábbiakat:

- Az Azure Active Directory-előfizetéssel
- A Microsoft Azure egyszeri bejelentkezés engedélyezett előfizetéshez tartozó Felhőbeli adatkezelési portál


> [AZURE.NOTE] Az oktatóprogram lépéseit teszteléséhez nem használata ajánlott munkakörnyezetben.


Az oktatóprogram lépéseit teszteléséhez célszerű követnie az alábbi javaslatokat:

- Erre akkor szükség, ne használja a termelési környezetén.
- Ha nincs telepítve az Azure Active Directory próba környezetben, kattint egy hónap próba [Itt](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban célja lehetővé teszi az Azure Active Directory egyszeri bejelentkezés tesztkörnyezetben tesztelése.  
Az alkalmazási példát, ebben az oktatóanyagban tagolt két fő építőelemek áll:

1. A Microsoft Azure Felhőbeli adatkezelési portál hozzáadása a gyűjteményből
2. Beállítása és tesztelése Azure Active Directory egyszeri bejelentkezés


## <a name="adding-cloud-management-portal-for-microsoft-azure-from-the-gallery"></a>A Microsoft Azure Felhőbeli adatkezelési portál hozzáadása a gyűjteményből
Konfigurálja az Azure Active Directory integrálása a Felhőbeli adatkezelési portál a Microsoft Azure, szüksége a Microsoft Azure Felhőbeli adatkezelési portál hozzáadása felügyelt szoftver alkalmazásai a gyűjteményből.

**A gyűjteményből Felhőbeli adatkezelési portál a Microsoft Azure felvételéhez hajtsa végre az alábbi lépéseket:**

1. Az **Azure klasszikus portálon**kattintson a bal oldali navigációs területen kattintson az **Active Directory**. 

    ![Az Active Directory][1]

2. A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3. Nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson a felső menüben **alkalmazásokat** .

    ![Alkalmazások][2]

4. Kattintson a **Hozzáadás** az oldal alján.

    ![Alkalmazások][3]

5. **Milyen feladatot szeretne tenni** párbeszédpanelen kattintson **a gyűjteményből az alkalmazások felvétele**.

    ![Alkalmazások][4]

6. A Keresés mezőbe írja be **A Microsoft Azure Felhőbeli adatkezelési portál**.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-newsignature-tutorial/tutorial_newsignature_01.png)

7. Az eredmény ablaktáblában jelölje ki a **Felhőbeli adatkezelési portál a Microsoft Azure**, és válassza a **kész** , az alkalmazás hozzáadása parancsra.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-newsignature-tutorial/tutorial_newsignature_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Beállítása és tesztelése Azure Active Directory egyszeri bejelentkezés
Ez a szakasz célja mutatja, hogy beállítása és tesztelése Azure AD az egyszeri bejelentkezés a Felhőbeli adatkezelési portál a Microsoft Azure "Britta Simon" nevű vizsgálat felhasználón alapuló.

Az egyszeri bejelentkezési a munkát az Azure Active Directory kell tudható, hogy mi az Azure Active Directory-felhasználónak a Felhőbeli adatkezelési portál a Microsoft Azure megfelelőjük a felhasználó. Más szóval Azure AD-felhasználó, és a kapcsolódó felhasználó a Felhőbeli adatkezelési portál Microsoft Azure-hivatkozás viszonya kell létrehozni.  
Ez a hivatkozás kapcsolat által a **felhasználónév** értéket rendeli az Azure Active Directory, a program a **felhasználónév** a Felhőbeli adatkezelési portál a Microsoft Azure jön létre.

Állítsa be, és Azure Active Directory egyszeri bejelentkezés a Microsoft Azure-Felhőbeli adatkezelési portál tesztelése, a következő építőelemek szükséges:

1. **[Azure Active Directory konfigurálása az egyszeri bejelentkezés](#configuring-azure-ad-single-single-sign-on)** - engedélyezése a felhasználóknak, hogy ezzel a szolgáltatással.
2. **[Felhasználó létrehozása az Azure Active Directory tesztelése](#creating-an-azure-ad-test-user)** - Azure Active Directory egyszeri bejelentkezéssel való Britta Simon ellenőrzéséhez.
4. **[A Microsoft Azure Felhőbeli adatkezelési portál létrehozása tesztelje a felhasználó](#creating-a-newsignature-test-user)** - van-e egy Britta Simon megfelelője a Felhőbeli adatkezelési portál a Microsoft Azure az Azure Active Directory ábrázolása amelyen csatolt.
5. **[Az Azure Active Directory hozzárendelése tesztelje a felhasználó](#assigning-the-azure-ad-test-user)** - ahhoz, hogy Britta Simon Azure AD az egyszeri bejelentkezés használata.
5. **[Tesztelés egyszeri bejelentkezés](#testing-single-sign-on)** - ellenőrzéséhez, hogy működik-e a konfigurációs.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure Active Directory Single Sign-On konfigurálása

Ez a szakasz célja Azure AD az egyszeri bejelentkezés az Azure klasszikus portálon engedélyezése és beállítása egyszeri bejelentkezés a Microsoft Azure alkalmazáshoz a Felhőbeli adatkezelési portál.



**Azure Active Directory egyszeri bejelentkezés beállítása a Felhőbeli adatkezelési portál a Microsoft Azure, hajtsa végre az alábbi lépéseket:**

1. Az Azure klasszikus portálon a **Felhőbeli adatkezelési portál Microsoft Azure-** alkalmazás integrációs lapon kattintson **konfigurálása az egyszeri bejelentkezés** **Beállítása az egyszeri bejelentkezés** párbeszédpanel megnyitásához.

    ![Egyszeri bejelentkezés beállítása][6] 

2. **Hogyan szeretné, hogy jelentkezzen be Felhőbeli adatkezelési portál Microsoft Azure-felhasználók** lapon válassza az **Azure Active Directory Single Sign-On**, és kattintson a **Tovább gombra**.

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-newsignature-tutorial/tutorial_newsignature_03.png) 

3. Az **Alkalmazás beállításainak megadása** párbeszédpanel lapon hajtsa végre az alábbi lépéseket:.

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-newsignature-tutorial/tutorial_newsignature_04.png) 

    egy. Az **Bejelentkezési a URL-cím** mezőbe írja be az URL-címet, a felhasználóknak, hogy a bejelentkezés a Felhőbeli adatkezelési portálra a Microsoft Azure-alkalmazást a következő mintát használható:`https://portal.igcm.com/<instance name>`

    b. Kattintson a **Tovább**gombra.


4. A **Configure egyszeri bejelentkezés a Felhőbeli adatkezelési portál a Microsoft Azure** lapon hajtsa végre az alábbi lépéseket:

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-newsignature-tutorial/tutorial_newsignature_05.png) 

    egy. Kattintson a **tanúsítvány letöltése**, és mentse a fájlt a számítógépen.

    b. Kattintson a **Tovább**gombra.


5. Ha be van állítva az alkalmazás SSO, lépjen kapcsolatba a Felhőbeli adatkezelési portált a Microsoft Azure-támogatási csoport [jczernuszka@newsignature.com](mailTo:jczernuszka@newsignature.com) és e-mailben a csatolása letöltött tanúsítvány-fájlt. Is adja meg a kibocsátó URL-CÍMÉT, a SAML egyszeri bejelentkezési URL-cím és az egyszeri bejelentkezési ki szolgáltatás URL-címe, hogy egyszeri bejelentkezési integrációs beállíthatók.


6. Az Azure klasszikus portálon válassza ki az egyszeri bejelentkezéses konfigurációs megerősítő, és kattintson a **Tovább gombra**.

    ![Azure Active Directory Single Sign-On][10]

7. Az **egyszeri bejelentkezés megerősítés** lapon kattintson a **Befejezés**gombra.  

    ![Azure Active Directory Single Sign-On][11]



### <a name="creating-an-azure-ad-test-user"></a>Azure AD-próba felhasználó létrehozása
Ez a szakasz célja vizsgálat felhasználó létrehozása az Azure-Britta Simon nevű klasszikus portálon.  

![Azure Active Directory-felhasználó létrehozása][20]

**Teszt felhasználó létrehozása az Azure Active Directory, hajtsa végre az alábbi lépéseket:**

1. Az **Azure klasszikus portálon**kattintson a bal oldali navigációs területen kattintson az **Active Directory**.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-newsignature-tutorial/create_aaduser_09.png) 

2. A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3. A menüben, a képernyő tetején, a felhasználók listájának megjelenítéséhez kattintson a **felhasználók**elemre.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-newsignature-tutorial/create_aaduser_03.png) 

4. **Felhasználó hozzáadása**elemre az alsó eszköztáron a **Felhasználó hozzáadása** párbeszédpanel megnyitásához.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-newsignature-tutorial/create_aaduser_04.png) 

5. A párbeszédpanel **közli velünk a felhasználóra vonatkozó** lapon hajtsa végre az alábbi lépéseket:

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-newsignature-tutorial/create_aaduser_05.png) 

    egy. Felhasználó típusa jelölje be az új felhasználó a szervezet.

    b. A felhasználónév **szövegmezőbe**írja be a **BrittaSimon**.

    c billentyűkombinációt. Kattintson a **Tovább**gombra.

6.  A **Felhasználói profil** párbeszédpanel lapon hajtsa végre az alábbi lépéseket:

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-newsignature-tutorial/create_aaduser_06.png) 

    egy. Az **első neve** mezőbe írja be a **Britta**.  

    b. Az **Utolsó neve** mezőbe írja be, **Simon**.

    c billentyűkombinációt. A **Megjelenítendő név** mezőbe írja be a **Britta Simon**.

    d. A **szerepkör** listában jelölje ki a **felhasználót**.

    e. Kattintson a **Tovább**gombra.

7. A párbeszédpanel **ideiglenes jelszót kérjen** lapon kattintson a **Hozzon létre**.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-newsignature-tutorial/create_aaduser_07.png) 

8. A párbeszédpanel **ideiglenes jelszót első** lapon hajtsa végre az alábbi lépéseket:

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-newsignature-tutorial/create_aaduser_08.png) 

    egy. Jegyezze fel az **Új jelszót**az érték.

    b. Kattintson a **kész**gombra.   



### <a name="creating-a-cloud-management-portal-for-microsoft-azure-test-user"></a>Microsoft Azure vizsgálat felhasználó egy Felhőbeli adatkezelési portál létrehozása

Ez a szakasz célja Britta Simon meghívott Felhőbeli adatkezelési portált a Microsoft Azure felhasználó létrehozása. Kérje a felhasználók felvétele az a felhő Kezelőportálja segítségével a Microsoft Azure-fiókjába a Microsoft Azure-támogatási csoport Felhőbeli adatkezelési portál. 


> [AZURE.NOTE]Ha egy felhasználó létrehozása manuálisan kell a Felhőbeli adatkezelési portálon lépjen kapcsolatba a Microsoft Azure-támogatási csoport szeretne.


### <a name="assigning-the-azure-ad-test-user"></a>Az Azure Active Directory-próba felhasználói hozzárendelése

Ez a szakasz célja Britta Simon Azure egyszeri bejelentkezés a saját hozzáférés engedélyezése a Felhőbeli adatkezelési portálra a Microsoft Azure használandó engedélyezése.

![Felhasználó hozzárendelése][200] 

**Felhőbeli adatkezelési portál a Microsoft Azure Britta Simon rendelni, hajtsa végre az alábbi lépéseket:**

1. Az Azure klasszikus portál kattintva nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson az **alkalmazások** parancsra a felső menüben.

    ![Felhasználó hozzárendelése][201] 

2. Az alkalmazások listájában jelölje ki a **Felhőbeli adatkezelési portál a Microsoft Azure**.

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-newsignature-tutorial/tutorial_newsignature_50.png) 

1. A felső sávon kattintson a **felhasználók**elemre.

    ![Felhasználó hozzárendelése][203] 

1. A felhasználók listában jelölje ki a **Britta Simon**.

2. Az alsó eszköztáron kattintson a **hozzárendelni**.

    ![Felhasználó hozzárendelése][205]



### <a name="testing-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

Ez a szakasz célja a Azure Active Directory egyszeri bejelentkezéses beállítások tesztelése a Access panelen.  
A Felhőbeli adatkezelési portál Microsoft Azure csempét az Access panel gombra kattintva meg kell első automatikusan aláírt-on a Felhőbeli adatkezelési portálra Microsoft Azure alkalmazáshoz.


## <a name="additional-resources"></a>További források

* [A szoftver alkalmazások integrálása az Azure Active Directory oktatóanyagok listája](active-directory-saas-tutorial-list.md)
* [Mi az access alkalmazás és az egyszeri bejelentkezés az Azure Active Directory címtárral?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-newsignature-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-newsignature-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-newsignature-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-newsignature-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-newsignature-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-newsignature-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-newsignature-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-newsignature-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-newsignature-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-newsignature-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-newsignature-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-newsignature-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-newsignature-tutorial/tutorial_general_205.png
