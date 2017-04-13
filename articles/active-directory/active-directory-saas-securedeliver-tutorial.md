<properties
    pageTitle="Oktatóprogram: Azure Active Directory-integráció a Novatus |} Microsoft Azure"
    description="Megtudhatja, hogy miként konfigurálása az egyszeri bejelentkezés Azure Active Directory és a biztonságos ELŐADÁSA között."
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
    ms.date="09/26/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-secure-deliver"></a>Oktatóprogram: Azure Active Directory integráció a biztonságos OLVASÁSA

Ebben az oktatóanyagban célja mutatja, hogy a biztonságos ELŐADÁSA integrálása az Azure Active Directory (Azure Active Directory) módját.  
Azure Active Directory biztonságos ELŐADÁSA integrálása biztosít a következő előnyökkel jár:

- Az Azure Active Directory előadásához biztonságos hozzáféréssel rendelkező személyek megadhatja, hogy
- Az Azure Active Directory-fiókok engedélyezheti a felhasználóknak, hogy automatikusan első aláírással a biztonságos használhat az előadáshoz (Single Sign-On)
- A fiókokat az egy központi helyen – az Azure klasszikus portálra

Ha többet szeretne tudni a szoftver alkalmazás integrálása az Azure AD meg, hogy, ismerje meg, [az access alkalmazás és az egyszeri bejelentkezés az Azure Active Directory címtárral](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek

Azure Active Directory integráció és biztonságos ELŐADÁSA van szükség az alábbiakat:

- Az Azure előfizetéssel
- A biztonságos ELŐADÁSA egyszeri bejelentkezés engedélyezett előfizetés


> [AZURE.NOTE] Az oktatóprogram lépéseit teszteléséhez nem használata ajánlott munkakörnyezetben.


Az oktatóprogram lépéseit teszteléséhez célszerű követnie az alábbi javaslatokat:

- Erre akkor szükség, ne használja a termelési környezetén.
- Ha nincs telepítve az Azure Active Directory próba környezetben, kattint egy hónap próba [Itt](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban célja lehetővé teszi az Azure Active Directory egyszeri bejelentkezés tesztkörnyezetben tesztelése.  
Az alkalmazási példát, ebben az oktatóanyagban tagolt két fő építőelemek áll:

1. A gyűjteményből hozzáadása a biztonságos OLVASÁSA
2. Beállítása és tesztelése Azure Active Directory egyszeri bejelentkezés


## <a name="adding-secure-deliver-from-the-gallery"></a>A gyűjteményből hozzáadása a biztonságos OLVASÁSA
Konfigurálja az Azure Active Directory integrálása a biztonságos OLVASÁSA, szüksége hozzáadása biztonságos ELŐADÁSA a gyűjteményből az felügyelt szoftver alkalmazáslistát.

**A gyűjteményből hozzáadása a biztonságos OLVASÁSA, hajtsa végre az alábbi lépéseket:**

1. Az **Azure klasszikus portálon**kattintson a bal oldali navigációs területen kattintson az **Active Directory**. 

    ![Az Active Directory][1]

2. A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3. Nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson a felső menüben **alkalmazásokat** .

    ![Alkalmazások][2]

4. Kattintson a **Hozzáadás** az oldal alján.

    ![Alkalmazások][3]

5. **Milyen feladatot szeretne tenni** párbeszédpanelen kattintson **a gyűjteményből az alkalmazások felvétele**.

    ![Alkalmazások][4]

6. A Keresés mezőbe írja be a **Biztonságos ELŐADÁSÁHOZ**.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-securedeliver-tutorial/tutorial_securedeliver_01.png)

7. Az eredmény ablaktáblában jelölje be a **Biztonságos OLVASÁSA**, és válassza a **kész** , az alkalmazás hozzáadása parancsra.
    
    ![Alkalmazás emblémája és a csoport neve](./media/active-directory-saas-securedeliver-tutorial/tutorial_securedeliver_06.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Beállítása és tesztelése Azure Active Directory egyszeri bejelentkezés
Ez a szakasz célja mutatja, hogy állítsa be, és a biztonságos ELŐADÁSA "Britta Simon" nevű próba felhasználón alapuló Azure AD az egyszeri bejelentkezés tesztelése.

Az egyszeri bejelentkezési a munkát az Azure Active Directory kell, hogy mi az a partner felhasználó biztonságos ELŐADÁSA az Azure Active Directory-felhasználónak. Más szóval Azure AD-felhasználó, és a kapcsolódó felhasználó biztonságos ELŐADÁSA hivatkozás viszonya kell létrehozni.  
A hivatkozás kapcsolat jön létre a értéket a **felhasználó nevét** a **felhasználónév** biztonságos ELŐADÁSA értékként Azure Active Directory hozzárendelésével.

Beállítása és tesztelése Azure AD az egyszeri bejelentkezés a biztonságos OLVASÁSA, a következő építőelemek szükséges:

1. **[Azure Active Directory konfigurálása az egyszeri bejelentkezés](#configuring-azure-ad-single-single-sign-on)** - engedélyezése a felhasználóknak, hogy ezzel a szolgáltatással.
2. **[Felhasználó létrehozása az Azure Active Directory tesztelése](#creating-an-azure-ad-test-user)** - Azure Active Directory egyszeri bejelentkezéssel való Britta Simon ellenőrzéséhez.
3. **[Felhasználó létrehozása a biztonságos ELŐADÁSA tesztelése](#creating-a-secure-deliver-test-user)** - szeretné, hogy egy partner a Britta Simon a személyek nézetben, amelyen az Azure Active Directory ábrázolt csatolt.
5. **[Az Azure Active Directory hozzárendelése tesztelje a felhasználó](#assigning-the-azure-ad-test-user)** - ahhoz, hogy Britta Simon Azure AD az egyszeri bejelentkezés használata.
5. **[Tesztelés egyszeri bejelentkezés](#testing-single-sign-on)** - ellenőrzéséhez, hogy működik-e a konfigurációs.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure Active Directory Single Sign-On konfigurálása

Ez a szakasz célja Azure AD az egyszeri bejelentkezés az Azure klasszikus portálon engedélyezése és a biztonságos ELŐADÁSA alkalmazásban az egyszeri bejelentkezés beállítása.



**Állítson be Azure AD az egyszeri bejelentkezés biztonságos OLVASÁSA, hajtsa végre az alábbi lépéseket:**

1. Az Azure klasszikus portálon alkalmazás integrációs **Biztonságos ELŐADÁSA** lapján kattintson **konfigurálása az egyszeri bejelentkezés** a **Konfigurálása az egyszeri bejelentkezés** párbeszédpanel megnyitásához.

    ![Egyszeri bejelentkezés beállítása][6] 

2. **Hogyan szeretné, hogy jelentkezzen be biztonságos ELŐADÁSA felhasználók** lapon válassza az **Azure Active Directory Single Sign-On**, és kattintson a **Tovább gombra**.
 
    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-securedeliver-tutorial/tutorial_securedeliver_03.png) 

3. Az **Alkalmazás beállításainak megadása** párbeszédpanel lapon hajtsa végre az alábbi lépéseket, és kattintson a **Tovább gombra**:
 
    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-securedeliver-tutorial/tutorial_securedeliver_04.png) 

    egy. **Bejelentkezési a URL-cím** mezőbe írja be a bejelentkezés az ELŐADÁSA biztonságos alkalmazás a következő mintát használatával a felhasználók által használt URL-címe: **"https://i-securedeliver.jp/sd/\<cégnév\>/jsf/login/sso"**.

    b. A biztonságos ELŐADÁSA támogatási csoport munkatársaitól kaphat keresztül [iw-sd-support@fujifilm.com](mailto:iw-sd-support@fujifilm.com) a bérlői webhely URL-cím szeretnénk, ha nem tudja, hogy az érték.

    c billentyűkombinációt. Az **azonosító** mezőbe írja be a bérlői webhely URL-CÍMÉT. 

    d. Kattintson a **Tovább** gombra


4. A **beállítás egyszeri bejelentkezés biztonságos ELŐADÁSA a** lapon hajtsa végre az alábbi lépéseket, és kattintson a **Tovább gombra**:

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-securedeliver-tutorial/tutorial_securedeliver_05.png) 

    egy. Kattintson a **tanúsítvány letöltése**, és mentse a fájlt a számítógépen.

    b. Kattintson a **Tovább**gombra.


5. Ha be van állítva az alkalmazás SSO, a biztonságos ELŐADÁSA támogatási csoport munkatársaitól keresztül [iw-sd-support@fujifilm.com](mailto:iw-sd-support@fujifilm.com) , és küldje el nekik a következő:
    
    • A letöltött tanúsítvány-fájl

    • A **szervezet azonosítója**

    • Az **egyszeri bejelentkezéses szolgáltatás URL-címe**

    • **Egyetlen kijelentkezési szolgáltatás URL-címe**



6. Az Azure klasszikus portálon válassza ki az egyszeri bejelentkezéses konfigurációs megerősítő, és kattintson a **Tovább gombra**.

    ![Azure Active Directory Single Sign-On][10]

7. Az **egyszeri bejelentkezés megerősítés** lapon kattintson a **Befejezés**gombra.  
  
    ![Azure Active Directory Single Sign-On][11]




### <a name="creating-an-azure-ad-test-user"></a>Azure AD-próba felhasználó létrehozása
Ez a szakasz célja vizsgálat felhasználó létrehozása az Azure-Britta Simon nevű klasszikus portálon.

![Azure Active Directory-felhasználó létrehozása][20]

**BIZTONSÁGOS ELŐADÁSA próba felhasználó létrehozása az Azure Active Directory, hajtsa végre az alábbi lépéseket:**

1. Az **Azure klasszikus portálon**kattintson a bal oldali navigációs területen kattintson az **Active Directory**.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-securedeliver-tutorial/create_aaduser_09.png) 

2. A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3. A menüben, a képernyő tetején, a felhasználók listájának megjelenítéséhez kattintson a **felhasználók**elemre.
  
    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-securedeliver-tutorial/create_aaduser_03.png) 

4. **Felhasználó hozzáadása**elemre az alsó eszköztáron a **Felhasználó hozzáadása** párbeszédpanel megnyitásához.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-securedeliver-tutorial/create_aaduser_04.png) 

5. A párbeszédpanel **közli velünk a felhasználóra vonatkozó** lapon hajtsa végre az alábbi lépéseket:

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-securedeliver-tutorial/create_aaduser_05.png) 

    egy. Felhasználó típusa jelölje be az új felhasználó a szervezet.

    b. A felhasználónév **szövegmezőbe**írja be a **BrittaSimon**.

    c billentyűkombinációt. Kattintson a **Tovább**gombra.

6.  A **Felhasználói profil** párbeszédpanel lapon hajtsa végre az alábbi lépéseket:

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-securedeliver-tutorial/create_aaduser_06.png) 

    egy. Az **első neve** mezőbe írja be a **Britta**.  

    b. Az **Utolsó neve** mezőbe írja be, **Simon**.

    c billentyűkombinációt. A **Megjelenítendő név** mezőbe írja be a **Britta Simon**.

    d. A **szerepkör** listában jelölje ki a **felhasználót**.

    e. Kattintson a **Tovább**gombra.

7. A párbeszédpanel **ideiglenes jelszót kérjen** lapon kattintson a **Hozzon létre**.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-securedeliver-tutorial/create_aaduser_07.png) 

8. A párbeszédpanel **ideiglenes jelszót első** lapon hajtsa végre az alábbi lépéseket:

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-securedeliver-tutorial/create_aaduser_08.png) 

    egy. Jegyezze fel az **Új jelszót**az érték.

    b. Kattintson a **kész**gombra.   



### <a name="creating-a-secure-deliver-test-user"></a>BIZTONSÁGOS ELŐADÁSA próba felhasználó létrehozása

Ez a szakasz célja Britta Simon nevű biztonságos ELŐADÁSA a felhasználó létrehozása. Kérje meg a felhasználók hozzáadása a biztonságos ELŐADÁSA fiók biztonságos ELŐADÁSA támogatási csoporthoz.

> [AZURE.NOTE] Ha egy felhasználó létrehozása manuálisan kell szüksége a biztonságos ELŐADÁSA támogatási csoport munkatársaitól kaphat.


### <a name="assigning-the-azure-ad-test-user"></a>Az Azure Active Directory-próba felhasználói hozzárendelése

Ez a szakasz célja Britta Simon Azure egyszeri bejelentkezés a saját hozzáférés engedélyezése a biztonságos előadásához használandó engedélyezése.

![Felhasználó hozzárendelése][200] 

**Britta Simon hozzárendelése biztonságos OLVASÁSA, hajtsa végre az alábbi lépéseket:**

1. Az Azure klasszikus portál kattintva nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson az **alkalmazások** parancsra a felső menüben.

    ![Felhasználó hozzárendelése][201] 

2. Az alkalmazások listájában jelölje ki a **Biztonságos OLVASÁSA**.

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-securedeliver-tutorial/tutorial_securedeliver_50.png) 

1. A felső sávon kattintson a **felhasználók**elemre.

    ![Felhasználó hozzárendelése][203] 

1. A felhasználók listában jelölje ki a **Britta Simon**.

2. Az alsó eszköztáron kattintson a **hozzárendelni**.

    ![Felhasználó hozzárendelése][205]



### <a name="testing-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

Ez a szakasz célja a Azure Active Directory egyszeri bejelentkezéses beállítások tesztelése a Access panelen.  
Ha az Access panel biztonságos ELŐADÁSA mozaik gombra kattint, meg kell első automatikusan aláírt-on az ELŐADÁSÁHOZ biztonságos alkalmazás.


## <a name="additional-resources"></a>További források

* [A szoftver alkalmazások integrálása az Azure Active Directory oktatóanyagok listája](active-directory-saas-tutorial-list.md)
* [Mi az access alkalmazás és az egyszeri bejelentkezés az Azure Active Directory címtárral?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-securedeliver-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-securedeliver-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-securedeliver-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-securedeliver-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-securedeliver-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-securedeliver-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-securedeliver-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-securedeliver-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-securedeliver-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-securedeliver-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-securedeliver-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-securedeliver-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-securedeliver-tutorial/tutorial_general_205.png
