<properties
    pageTitle="Oktatóprogram: Azure Active Directory-integráció a laposabb fájlok |} Microsoft Azure"
    description="Egyszeri bejelentkezés Azure Active Directory és laposabb fájlok közötti konfigurálásának ismertetése."
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


# <a name="tutorial-azure-active-directory-integration-with-flatter-files"></a>Oktatóprogram: Azure Active Directory-integráció a laposabb fájlok

Ebben az oktatóanyagban célja mutatja, hogy miként laposabb fájlok integrálása az Azure Active Directory (Azure Active Directory).  
Laposabb fájlok integrálása az Azure Active Directory biztosít a következő előnyökkel jár: 

- Az Azure Active Directory, aki rendelkezik hozzáféréssel a laposabb fájlok megadhatja, hogy 
- Az Azure Active Directory-fiókok engedélyezheti a felhasználóknak, hogy automatikusan első jelentkezett-on laposabb fájlok (Single Sign-On)
- A fiókokat az egy központi helyen – a klasszikus Azure Active Directory-portál

Ha többet szeretne tudni a szoftver alkalmazás integrálása az Azure AD meg, hogy, ismerje meg, [az access alkalmazás és az egyszeri bejelentkezés az Azure Active Directory címtárral](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek 

Azure Active Directory integráció laposabb fájlokkal van szükség az alábbiakat:

- Az Azure Active Directory-előfizetéssel
- A fájlok laposabb egyszeri bejelentkezés engedélyezett előfizetés


> [AZURE.NOTE] Az oktatóprogram lépéseit teszteléséhez nem használata ajánlott munkakörnyezetben.


Az oktatóprogram lépéseit teszteléséhez célszerű követnie az alábbi javaslatokat:

- Erre akkor szükség, ne használja a termelési környezetén.
- Ha nincs telepítve az Azure Active Directory próba környezetben, kattint egy hónap próba [Itt](https://azure.microsoft.com/pricing/free-trial/). 

 
## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban célja lehetővé teszi az Azure Active Directory egyszeri bejelentkezés tesztkörnyezetben tesztelése.  
Az alkalmazási példát, ebben az oktatóanyagban tagolt két fő építőelemek áll:

1. Laposabb fájlok hozzáadása a gyűjteményből 
2. Beállítása és tesztelése Azure Active Directory egyszeri bejelentkezés


## <a name="adding-flatter-files-from-the-gallery"></a>Laposabb fájlok hozzáadása a gyűjteményből
Konfigurálja az Azure Active Directory integrálása a laposabb fájlokat, szüksége laposabb fájlok hozzáadása az felügyelt szoftver alkalmazáslistában a gyűjteményből.

**Fájlokat szeretne hozzáadni laposabb a gyűjteményből, hajtsa végre az alábbi lépéseket:**

1. Az **Azure klasszikus portálon**kattintson a bal oldali navigációs területen kattintson az **Active Directory**. 

    ![Az Active Directory][1]

2. A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3. Nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson a felső menüben **alkalmazások** .

    ![Alkalmazások][2]

4. Kattintson a **Hozzáadás** az oldal alján.

    ![Alkalmazások][3]

5. **Milyen feladatot szeretne tenni** párbeszédpanelen kattintson **a gyűjteményből az alkalmazások felvétele**.

    ![Alkalmazások][4]

6. A Keresés mezőbe írja be a **Laposabb fájlokat**.


    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatter_files_01.png)

7. Az eredmény ablaktáblában jelölje ki a **Laposabb fájlokat**, és válassza a **kész** , az alkalmazás hozzáadása gombra.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatter_files_500.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Beállítása és tesztelése Azure Active Directory egyszeri bejelentkezés
Ez a szakasz célja mutatja, hogy beállítása és Azure Active Directory egyszeri bejelentkezés a "Britta Simon" nevű próba felhasználón alapuló laposabb fájlokkal tesztelése.

Az egyszeri bejelentkezési a munkát az Azure Active Directory kell, hogy mi az a megfelelőjük felhasználó laposabb fájlokat egy felhasználó az Azure Active Directory. Más szóval Azure AD-felhasználó, és a kapcsolódó felhasználó laposabb fájlok közötti kapcsolat kapcsolat kell létrehozni.  
Ez a hivatkozás kapcsolat az Azure Active Directory, a program a **felhasználónév** laposabb fájlokat a az érték, a **felhasználónév** hozzárendelésével jön létre.
 
Állítsa be és Azure Active Directory egyszeri bejelentkezés laposabb fájlok teszteléséhez az alábbi építőelemeket szükséges:

1. **[Azure Active Directory konfigurálása az egyszeri bejelentkezés](#configuring-azure-ad-single-single-sign-on)** - engedélyezése a felhasználóknak, hogy ezzel a szolgáltatással.
2. **[Felhasználó létrehozása az Azure Active Directory tesztelése](#creating-an-azure-ad-test-user)** - Azure Active Directory egyszeri bejelentkezéssel való Britta Simon ellenőrzéséhez.
4. **[Teszt laposabb-fájlok létrehozása a felhasználó](#creating-a-halogen-software-test-user)** -, hogy egy partner a Britta Simon laposabb fájlokban, amelyen az Azure Active Directory ábrázolt csatolt.
5. **[Az Azure Active Directory hozzárendelése tesztelje a felhasználó](#assigning-the-azure-ad-test-user)** - ahhoz, hogy Britta Simon Azure AD az egyszeri bejelentkezés használata.
5. **[Tesztelés egyszeri bejelentkezés](#testing-single-sign-on)** - ellenőrzéséhez, hogy működik-e a konfigurációs.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure Active Directory egyszeri bejelentkezés beállítása

Ez a szakasz célja Azure AD az egyszeri bejelentkezés az Azure Active Directory klasszikus portálon engedélyezése és a laposabb fájlok alkalmazásban az egyszeri bejelentkezés beállítása. Ez az eljárás részeként biztosan szükséges számrendszerben-64 kódolt tanúsítvány fájlt. Ha nem ismeri a ezt az eljárást, olvassa el a [szövegfájlba bináris tanúsítvány konvertálása](http://youtu.be/PlgrzUZ-Y1o)című témakört.

Egyszeri bejelentkezés laposabb fájlok beállításához szükséges regisztrált tartományt. Ha nincs olyan regisztrált tartományt még, kapcsolattartó laposabb fájlok támogatja-e a csapat keresztül [support@flatterfiles.com](mailto:support@flatterfiles.com).  



**Azure Active Directory egyszeri bejelentkezés beállítása a laposabb fájlokkal, hajtsa végre az alábbi lépéseket:**

1. Az Azure Active Directory klasszikus portálon **Laposabb fájlok** alkalmazás integrációs lapon kattintson **konfigurálása az egyszeri bejelentkezés** a **Konfigurálása az egyszeri bejelentkezés** párbeszédpanel megnyitásához.

    ![Egyszeri bejelentkezés beállítása][6] 

2. **Hogyan szeretné, hogy jelentkezzen be az laposabb fájlok felhasználók** lapon válassza az **Azure Active Directory Single Sign-On**, és kattintson a **Tovább gombra**.
 
    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatter_files_02.png) 

3. Az **Alkalmazás beállításainak megadása** párbeszédpanel lapon kattintson a **Tovább**gombra.

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatter_files_03.png) 

    > [AZURE.NOTE] Laposabb fájlok használja az azonos Egyszeri bejelentkezési URL-cím összes az ügyfeleknek: [https://www.flatterfiles.com/site/login/sso/](https://www.flatterfiles.com/site/login/sso/).
.
 
 
4. A **Configure egyszeri bejelentkezés laposabb fájlokat** lapon hajtsa végre az alábbi lépéseket:

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatter_files_04.png)  

    egy. Kattintson a **tanúsítvány letöltése**, és mentse a fájlt a számítógépen.

    b. Kattintson a **Tovább**gombra.


1. Bejelentkezés az laposabb fájlok alkalmazás rendszergazdaként.

2. Az irányítópult elemre. 

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatter_files_05.png)  



2. Kattintson a **Beállítások**gombra, és majd a **vállalat** lapon hajtsa végre az alábbi lépéseket: 

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatter_files_06.png)  

    egy. Jelölje be **a SAML 2.0-s hitelesítéshez**.

    b. Kattintson a **SAML megadása**elemre.



2. A **SAML konfigurációs** párbeszédpanelen hajtsa végre az alábbi lépéseket: 

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatter_files_08.png)  

    egy. A tartomány mezőbe írja be a regisztrált tartományt.

    > [AZURE.NOTE] Ha nincs olyan regisztrált tartományt még, kapcsolattartó laposabb fájlok támogatja-e a csapat keresztül [support@flatterfiles.com](mailto:support@flatterfiles.com).
    
    b. Az Azure-ban a egyetlen beállítás klasszikus portálon bejelentkezéses laposabb fájlok párbeszédpanelen, copt egyszeri bejelentkezéses szolgáltatás URL-CÍMÉT, és illessze be a identitás szolgáltató URL-címe mezőben lévő értéket.

    c billentyűkombinációt.  A letöltött tanúsítvány **Alap-64-címként kódolt** fájl létrehozása  

    >[AZURE.TIP] További információra kíváncsi olvassa el a [bináris tanúsítvány szövegfájlba konvertálása](http://youtu.be/PlgrzUZ-Y1o) című témakört.

    d.  Nyissa meg az alap-64 kódolt tanúsítvány a Jegyzettömbben, azt tartalmát a vágólapra másolja és illessze be a **FlatterFiles identitás szolgáltató tanúsítványának** mezőben lévő értéket szeretné.

    e. Kattintson a **frissítés**gombra.

6. Az Azure Active Directory-klasszikus portálon válassza ki az egyszeri bejelentkezéses konfigurációs megerősítő, és kattintson a **Tovább gombra**. 

    ![Azure Active Directory Single Sign-On][10]

7. Az **egyszeri bejelentkezés megerősítés** lapon kattintson a **Befejezés**gombra.  

    ![Azure Active Directory Single Sign-On][11]




### <a name="creating-an-azure-ad-test-user"></a>Azure AD-próba felhasználó létrehozása
Ez a szakasz célja próba felhasználó létrehozása az Azure-Britta Simon nevű klasszikus portálon.

![Azure Active Directory-felhasználó létrehozása][20]

**Teszt felhasználó létrehozása az Azure Active Directory, hajtsa végre az alábbi lépéseket:**

1. Az **Azure klasszikus portálon**kattintson a bal oldali navigációs területen kattintson az **Active Directory**.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-flatter-files-tutorial/create_aaduser_09.png) 

2. A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3. A menüben, a képernyő tetején, a felhasználók listájának megjelenítéséhez kattintson a **felhasználók**elemre.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-flatter-files-tutorial/create_aaduser_03.png) 
 
4. **Felhasználó hozzáadása**elemre az alsó eszköztáron a **Felhasználó hozzáadása** párbeszédpanel megnyitásához. 

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-flatter-files-tutorial/create_aaduser_04.png) 

5. A párbeszédpanel **közli velünk a felhasználóra vonatkozó** lapon hajtsa végre az alábbi lépéseket: 

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-flatter-files-tutorial/create_aaduser_05.png)  

    egy. Felhasználó típusa jelölje be az új felhasználó a szervezet.

    b. A felhasználónév **szövegmezőbe**írja be a **BrittaSimon**.

    c billentyűkombinációt. Kattintson a **Tovább**gombra.

6.  A **Felhasználói profil** párbeszédpanel lapon hajtsa végre az alábbi lépéseket: 

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-flatter-files-tutorial/create_aaduser_06.png) 
 
    egy. Az **első neve** mezőbe írja be a **Britta**.  

    b. Az **Utolsó neve** mezőbe írja be, **Simon**.

    c billentyűkombinációt. A **Megjelenítendő név** mezőbe írja be a **Britta Simon**.

    d. A **szerepkör** listában jelölje ki a **felhasználót**.
    e. Kattintson a **Tovább**gombra.

7. A párbeszédpanel **ideiglenes jelszót első** lapon kattintson a **Hozzon létre**.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-flatter-files-tutorial/create_aaduser_07.png) 
 
8. A párbeszédpanel **ideiglenes jelszót első** lapon hajtsa végre az alábbi lépéseket:

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-flatter-files-tutorial/create_aaduser_08.png) 
  
    egy. Jegyezze fel az **Új jelszót**az érték.

    b. Kattintson a **kész**gombra.   

  
 
### <a name="creating-a-flatter-files-test-user"></a>Laposabb fájlok próba felhasználó létrehozása

Ez a szakasz célja Britta Simon nevű laposabb fájlokat egy felhasználó létrehozása.

**A felhasználó Britta Simon nevű laposabb fájlok létrehozásához hajtsa végre az alábbi lépéseket:**

1. Jelentkezzen be rendszergazdaként a **Laposabb fájlok** vállalati webhely.

2. A bal oldali navigációs ablakban kattintson a **Beállítások**gombra, és válassza a felhasználók **lapon**.

    ![Cfreate laposabb fájlok felhasználó](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatter_files_09.png)

3. Kattintson a **felhasználó hozzáadása**elemre. 

4. A **Felhasználó hozzáadása** párbeszédpanelen hajtsa végre az alábbi lépéseket:

    ![Cfreate laposabb fájlok felhasználó](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatter_files_10.png)

    egy. Az **első neve** mezőbe írja be a **Britta**.

    b. Az **Utolsó neve** mezőbe írja be a **Simon**. 

    c billentyűkombinációt. Az **E-mail cím** mezőbe írja be a Britta meg e-mail címét az Azure klasszikus portálon.

    d. Kattintson a **Küldés**gombra.   


### <a name="assigning-the-azure-ad-test-user"></a>Az Azure Active Directory-próba felhasználói hozzárendelése

Ez a szakasz célja Britta Simon használandó Azure egyszeri bejelentkezés a saját hozzáférést biztosít laposabb fájlok engedélyezése.

![Felhasználó hozzárendelése][200] 

**Britta Simon hozzárendelése laposabb fájlokat, hajtsa végre az alábbi lépéseket:**

1. Az Azure klasszikus portál kattintva nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson az **alkalmazások** parancsra a felső menüben.

    ![Felhasználó hozzárendelése][201] 

2. Az alkalmazások listájában jelölje ki a **Laposabb fájlokat**.

    ![Felhasználó hozzárendelése](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatter_files_11.png) 

1. A felső sávon kattintson a **felhasználók**elemre.

    ![Felhasználó hozzárendelése][203] 

1. A felhasználók listában jelölje ki a **Britta Simon**.

2. Az alsó eszköztáron kattintson a **hozzárendelni**.

    ![Felhasználó hozzárendelése][205]



### <a name="testing-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

Ez a szakasz célja a Azure Active Directory egyszeri bejelentkezéses beállítások tesztelése a Access panelen.  
Amikor az Access panel a laposabb fájlok mozaik gombra kattint, meg kell első automatikusan aláírt-on az laposabb fájlok alkalmazás.


## <a name="additional-resources"></a>További források

* [A szoftver alkalmazások integrálása az Azure Active Directory oktatóanyagok listája](active-directory-saas-tutorial-list.md)
* [Mi az access alkalmazás és az egyszeri bejelentkezés az Azure Active Directory címtárral?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_205.png






