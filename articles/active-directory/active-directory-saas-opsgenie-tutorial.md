<properties
    pageTitle="Oktatóprogram: Azure Active Directory-integráció a OpsGenie |} Microsoft Azure"
    description="Megtudhatja, hogy miként konfigurálása az egyszeri bejelentkezés Azure Active Directory és OpsGenie között."
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
    ms.date="10/07/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-opsgenie"></a>Oktatóprogram: Azure Active Directory-integráció a OpsGenie

Ebben az oktatóanyagban célja mutatja, hogy miként OpsGenie integrálása az Azure Active Directory (Azure Active Directory).

OpsGenie integrálása az Azure Active Directory biztosít a következő előnyökkel jár:

- Az Azure Active Directory, aki rendelkezik hozzáféréssel OpsGenie megadhatja, hogy
- Engedélyezheti a felhasználóknak, hogy automatikusan első jelentkezett-on való OpsGenie (Single Sign-On) az Azure Active Directory-fiókok
- A fiókokat az egy központi helyen – az Azure klasszikus portálra

Ha többet szeretne tudni a szoftver alkalmazás integrálása az Azure AD meg, hogy, ismerje meg, [az access alkalmazás és az egyszeri bejelentkezés az Azure Active Directory címtárral](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek

Azure Active Directory integráció a OpsGenie van szükség az alábbiakat:

- Az Azure Active Directory-előfizetéssel
- Egy OpsGenie egyszeri bejelentkezés engedélyezve van az előfizetés


> [AZURE.NOTE] Az oktatóprogram lépéseit teszteléséhez nem használata ajánlott munkakörnyezetben.


Az oktatóprogram lépéseit teszteléséhez célszerű követnie az alábbi javaslatokat:

- Erre akkor szükség, ne használja a termelési környezetén.
- Ha nincs telepítve az Azure Active Directory próba környezetben, kattint egy hónap próba [Itt](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban célja lehetővé teszi az Azure Active Directory egyszeri bejelentkezés tesztkörnyezetben tesztelése. 

Az alkalmazási példát, ebben az oktatóanyagban tagolt két fő építőelemek áll:

1. OpsGenie hozzáadása a gyűjteményből
2. Beállítása és tesztelése Azure Active Directory egyszeri bejelentkezés


## <a name="adding-opsgenie-from-the-gallery"></a>OpsGenie hozzáadása a gyűjteményből
Konfigurálja az Azure Active Directory integrálása a OpsGenie, meg kell OpsGenie hozzáadása az felügyelt szoftver alkalmazáslistában a gyűjteményből.

**A gyűjteményből OpsGenie felvételéhez hajtsa végre az alábbi lépéseket:**

1. Az **Azure klasszikus portálon**kattintson a bal oldali navigációs területen kattintson az **Active Directory**. 

    ![Az Active Directory][1]

2. A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3. Nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson a felső menüben **alkalmazásokat** .

    ![Alkalmazások][2]

4. Kattintson a **Hozzáadás** az oldal alján.

    ![Alkalmazások][3]

5. **Milyen feladatot szeretne tenni** párbeszédpanelen kattintson **a gyűjteményből az alkalmazások felvétele**.

    ![Alkalmazások][4]

6. A Keresés mezőbe írja be a **OpsGenie**.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_01.png)

7. Az eredmény ablaktáblában jelölje ki a **OpsGenie**, és válassza a **kész** , az alkalmazás hozzáadása gombra.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_02.png)


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Beállítása és tesztelése Azure Active Directory egyszeri bejelentkezés
Ez a szakasz célja mutatja, hogy állítsa be, és a "Britta Simon" nevű próba felhasználón alapuló OpsGenie Azure AD az egyszeri bejelentkezés tesztelése.

Egyszeri bejelentkezés a munkát Azure AD meg kell adni a partner felhasználó OpsGenie tudnivalók egy felhasználóhoz, az Azure Active Directory. Más szóval Azure AD-felhasználó, és a kapcsolódó felhasználó OpsGenie hivatkozás viszonya kell létrehozni.

A hivatkozás kapcsolat jön létre által az Azure Active Directory OpsGenie **felhasználónév** értékként a **felhasználó neve** értéket rendeli.

Állítsa be, és Azure Active Directory egyszeri bejelentkezéssel való OpsGenie tesztelése, a következő építőelemek szükséges:

1. **[Azure Active Directory konfigurálása az egyszeri bejelentkezés](#configuring-azure-ad-single-single-sign-on)** - engedélyezése a felhasználóknak, hogy ezzel a szolgáltatással.
2. **[Felhasználó létrehozása az Azure Active Directory tesztelése](#creating-an-azure-ad-test-user)** - Azure Active Directory egyszeri bejelentkezéssel való Britta Simon ellenőrzéséhez.
4. **[Felhasználó létrehozása egy OpsGenie tesztelése](#creating-a-opsgenie-test-user)** - van-e egy megfelelője a Britta Simon OpsGenie, amelyen az Azure Active Directory ábrázolt csatolt.
5. **[Az Azure Active Directory hozzárendelése tesztelje a felhasználó](#assigning-the-azure-ad-test-user)** - ahhoz, hogy Britta Simon Azure AD az egyszeri bejelentkezés használata.
5. **[Tesztelés egyszeri bejelentkezés](#testing-single-sign-on)** - ellenőrzéséhez, hogy működik-e a konfigurációs.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure Active Directory egyszeri bejelentkezés beállítása

Ez a szakasz célja Azure AD az egyszeri bejelentkezés az Azure klasszikus portálon engedélyezése és a OpsGenie alkalmazásban az egyszeri bejelentkezés beállítása.



**Állítson be Azure AD az egyszeri bejelentkezés OpsGenie, hajtsa végre az alábbi lépéseket:**

1. Az Azure klasszikus portálon **OpsGenie** alkalmazás integrációs lapon kattintson **konfigurálása az egyszeri bejelentkezés** a **Konfigurálása az egyszeri bejelentkezés** párbeszédpanel megnyitásához.

    ![Egyszeri bejelentkezés beállítása][6] 

2. **Hogyan szeretné, hogy jelentkezzen be az OpsGenie felhasználók** lapon válassza az **Azure Active Directory Single Sign-On**, és kattintson a **Tovább gombra**.

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_03.png) 

3. Az **Alkalmazás beállításainak megadása** párbeszédpanel lapon hajtsa végre az alábbi lépéseket:

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_04.png) 


    egy. Bejelentkezési a URL-cím mezőbe írja be a bejelentkezés az OpsGenie alkalmazás a következő mintát használatával a felhasználók által használt URL-címe: **"https://app.opsgenie.com/auth/login"**.

    > [AZURE.NOTE] Kérjük, forduljon a [OpsGenie támogatási csoportjának](mailto:support@opsgenie.com) , ha szüksége van a bejelentkezési a URL-CÍMÉT.

    b. Kattintson a **Tovább**gombra.


4. A **beállítás az egyszeri bejelentkezés OpsGenie a** lapon hajtsa végre az alábbi lépéseket:

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_05.png) 

    egy. A **Tanúsítvány letöltése**gombra, és mentse a fájlt a számítógépen. A tanúsítvány és a metaadat-alapú URL-ek (szervezet azonosítója, Egyszeri bejelentkezési az URL-CÍMÉT és a bejelentkezési meg URL-CÍMÉT) egyszeri bejelentkezés beállítása a OpsGenie oldalon lesz szükség.

    b. Kattintson a **Tovább**gombra.


5. Nyissa meg a böngészőablakot, amelyben egy másik, és ezután napló az-OpsGenie rendszergazdaként.

6. Kattintson a **Beállítások**gombra, és kattintson az **Egyszeri bejelentkezés** fülre.
 
    ![OpsGenie Single Sign-On](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_06.png) 

7. Egyszeri bejelentkezés engedélyezéséhez jelölje be az **engedélyezett**.

    ![OpsGenie beállításai](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_07.png) 
   
8. A **szolgáltató** csoportban kattintson az **Azure Active Directory** fülre.

    ![OpsGenie beállításai](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_08.png) 
    
9. Az Azure Active Directory párbeszédpanel lapon hajtsa végre az alábbi lépéseket:
 
    ![OpsGenie beállításai](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_09.png) 

    egy. Az Azure klasszikus portálon **konfigurálása az egyszeri bejelentkezés a OpsGenie** párbeszédpanel lapon az **Egyszeri bejelentkezési a szolgáltatás URL-címe** értéket másolja és illessze be a **SAML 2.0-s végpont** mezőben lévő értéket.

    b. A letöltött tanúsítvány alap-64 kódolt fájl létrehozása      
    
    > [AZURE.NOTE] További részletekért megtudhatja, [hogy miként szövegfájlba bináris tanúsítvány konvertálja](https://www.youtube.com/watch?v=PlgrzUZ-Y1o&feature=youtu.be).

    c billentyűkombinációt. Nyissa meg az alap-64 kódolt tanúsítvány a Jegyzettömbben, a tartalmát a vágólapra másolja és illessze be a **X.500 tanúsítvány** mezőben lévő értéket.

    d. A **módosítások mentése**gombra.


6. Az Azure klasszikus portálon válassza ki az egyszeri bejelentkezéses konfigurációs megerősítő, és kattintson a **Tovább gombra**.

    ![Azure Active Directory Single Sign-On][10]

7. Az **egyszeri bejelentkezés megerősítés** lapon kattintson a **Befejezés**gombra.  

    ![Azure Active Directory Single Sign-On][11]




### <a name="creating-an-azure-ad-test-user"></a>Azure AD-próba felhasználó létrehozása
Ez a szakasz célja vizsgálat felhasználó létrehozása az Azure-Britta Simon nevű klasszikus portálon.



![Azure Active Directory-felhasználó létrehozása][20]

**Teszt felhasználó létrehozása az Azure Active Directory, hajtsa végre az alábbi lépéseket:**

1. Az **Azure klasszikus portálon**kattintson a bal oldali navigációs területen kattintson az **Active Directory**.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-opsgenie-tutorial/create_aaduser_09.png) 

2. A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3. A menüben, a képernyő tetején, a felhasználók listájának megjelenítéséhez kattintson a **felhasználók**elemre.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-opsgenie-tutorial/create_aaduser_03.png) 

4. **Felhasználó hozzáadása**elemre az alsó eszköztáron a **Felhasználó hozzáadása** párbeszédpanel megnyitásához.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-opsgenie-tutorial/create_aaduser_04.png) 

5. A párbeszédpanel **közli velünk a felhasználóra vonatkozó** lapon hajtsa végre az alábbi lépéseket:

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-opsgenie-tutorial/create_aaduser_05.png) 

    egy. Felhasználó típusa jelölje be az új felhasználó a szervezet.

    b. A felhasználónév **szövegmezőbe**írja be a **BrittaSimon**.

    c billentyűkombinációt. Kattintson a **Tovább**gombra.

6.  A **Felhasználói profil** párbeszédpanel lapon hajtsa végre az alábbi lépéseket:

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-opsgenie-tutorial/create_aaduser_06.png) 

    egy. Az **első neve** mezőbe írja be a **Britta**.  

    b. Az **Utolsó neve** mezőbe írja be, **Simon**.

    c billentyűkombinációt. A **Megjelenítendő név** mezőbe írja be a **Britta Simon**.

    d. A **szerepkör** listában jelölje ki a **felhasználót**.

    e. Kattintson a **Tovább**gombra.

7. A párbeszédpanel **ideiglenes jelszót kérjen** lapon kattintson a **Hozzon létre**.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-opsgenie-tutorial/create_aaduser_07.png) 

8. A párbeszédpanel **ideiglenes jelszót első** lapon hajtsa végre az alábbi lépéseket:

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-opsgenie-tutorial/create_aaduser_08.png) 

    egy. Jegyezze fel az **Új jelszót**az érték.

    b. Kattintson a **kész**gombra.   



### <a name="creating-a-opsgenie-test-user"></a>OpsGenie vizsgálat felhasználó létrehozása

Ez a szakasz célja Britta Simon nevű OpsGenie felhasználó létrehozása. 

1.  Egy webböngészőablakban jelentkezzen be a OpsGenie bérlői rendszergazdaként.

2.  Nyissa meg a bal oldali panelen kattintson a **felhasználói** felhasználók listában.
   
    ![OpsGenie beállításai](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_10.png) 

3.  Kattintson a "**felhasználó hozzáadása**".

3.  A **Felhasználó hozzáadása** párbeszédpanelen hajtsa végre az alábbi lépéseket:

    ![OpsGenie beállításai](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_11.png) 

    egy. Az **E-mail** mezőbe írja be a Britta meg e-mail címét az Azure Active Directory.

    b. A **Teljes neve** mezőbe írja be a **Britta Simon**.

    c billentyűkombinációt. Kattintson a **Mentés**gombra. 

Saját profil beállítása tartalmazó e-mail Britta jelenik meg.


### <a name="assigning-the-azure-ad-test-user"></a>Az Azure Active Directory-próba felhasználói hozzárendelése

Ez a szakasz célja Britta Simon Azure egyszeri bejelentkezés a saját hozzáférést biztosít OpsGenie használandó engedélyezése.

![Felhasználó hozzárendelése][200] 

**Britta Simon hozzárendelése OpsGenie, hajtsa végre az alábbi lépéseket:**

1. Az Azure klasszikus portál kattintva nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson az **alkalmazások** parancsra a felső menüben.
 
    ![Felhasználó hozzárendelése][201] 

2. Az alkalmazások listájában jelölje ki a **OpsGenie**.

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_50.png) 

1. A felső sávon kattintson a **felhasználók**elemre.

    ![Felhasználó hozzárendelése][203] 

1. A felhasználók listában jelölje ki a **Britta Simon**.

2. Az alsó eszköztáron kattintson a **hozzárendelni**.

    ![Felhasználó hozzárendelése][205]



### <a name="testing-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

Ez a szakasz célja a Azure Active Directory egyszeri bejelentkezéses beállítások tesztelése a Access panelen.

Ha a OpsGenie csempére az Access panel gombra kattint, meg kell kérjen automatikusan aláírt-on az OpsGenie alkalmazás.


## <a name="additional-resources"></a>További források

* [A szoftver alkalmazások integrálása az Azure Active Directory oktatóanyagok listája](active-directory-saas-tutorial-list.md)
* [Mi az access alkalmazás és az egyszeri bejelentkezés az Azure Active Directory címtárral?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_205.png
