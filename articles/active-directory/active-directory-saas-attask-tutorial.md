<properties
    pageTitle="Oktatóprogram: Azure Active Directory-integráció a @Task| Microsoft Azure"
    description="Megtudhatja, hogy miként egyszeri bejelentkezés közötti Azure Active Directory konfigurálása és @Task."
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


# <a name="tutorial-azure-active-directory-integration-with-task"></a>Oktatóprogram: Azure Active Directory integrációja@Task

Ebben az oktatóanyagban célja mutatja, hogy hogyan integrálja @Task az Azure Active Directory (Azure Active Directory).  
Integrálása @Task az Azure Active Directory biztosít a következő előnyökkel jár: 

- Azure Active Directory, aki rendelkezik a hozzáférést a megadhatja, hogy@Task
- Engedélyezheti a felhasználóknak, hogy automatikusan megkapja jelentkezett-on való @Task (Single Sign-On) az Azure Active Directory-fiókok
- A fiókokat az egy központi helyen – az Azure klasszikus portálra

Ha többet szeretne tudni a szoftver alkalmazás integrálása az Azure AD meg, hogy, ismerje meg, [az access alkalmazás és az egyszeri bejelentkezés az Azure Active Directory címtárral](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek 

Azure AD-integráció a konfigurálása @Task, van szüksége az alábbiakat:

- Az Azure Active Directory-előfizetéssel
- Egy @Task egyszeri bejelentkezés engedélyezett előfizetés


> [AZURE.NOTE] Az oktatóprogram lépéseit teszteléséhez nem használata ajánlott munkakörnyezetben.


Az oktatóprogram lépéseit teszteléséhez célszerű követnie az alábbi javaslatokat:

- Erre akkor szükség, ne használja a termelési környezetén.
- Ha nincs telepítve az Azure Active Directory próba környezetben, kattint egy hónap próba [Itt](https://azure.microsoft.com/pricing/free-trial/). 

 
## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban célja lehetővé teszi az Azure Active Directory egyszeri bejelentkezés tesztkörnyezetben tesztelése.  
Az alkalmazási példát, ebben az oktatóanyagban tagolt három fő építőelemek áll:

1. Hozzáadás @Task a gyűjteményből 
2. Beállítása és tesztelése Azure Active Directory egyszeri bejelentkezés


## <a name="adding-task-from-the-gallery"></a>Hozzáadás @Task a gyűjteményből
Integrációja konfigurálása @Task az Azure Active Directory, fel kell vennie @Task a felügyelt szoftver alkalmazásai a gyűjteményből.

**Hozzáadása @Task a gyűjteményből az alábbi lépések végrehajtásával:**

1. Az **Azure klasszikus portálon**kattintson a bal oldali navigációs területen kattintson az **Active Directory**. 

    ![Az Active Directory][1] 

2. A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3. Nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson a felső menüben **alkalmazásokat** .

    ![Alkalmazások][2] 

4. Kattintson a **Hozzáadás** az oldal alján.

    ![Alkalmazások][3] 

5. **Milyen feladatot szeretne tenni** párbeszédpanelen kattintson **a gyűjteményből az alkalmazások felvétele**.

    ![Alkalmazások][4] 

6. A Keresés mezőbe írja be a **@Task**.

    ![Alkalmazások][5] 

7. Az eredmény ablaktáblában válassza a **@Task**, és kattintson a **kész** , az alkalmazás hozzáadása.

    ![Alkalmazások][30] 



##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Beállítása és tesztelése Azure Active Directory egyszeri bejelentkezés

Ez a szakasz célja mutatja, hogy beállítása és tesztelése Azure Active Directory egyszeri bejelentkezéssel való @Task "Britta Simon" nevű vizsgálat felhasználón alapuló.

Az egyszeri bejelentkezési a munkát, Azure Active Directory kell, hogy milyen a megfelelőjük a felhasználó @Task az Azure Active Directory-felhasználónak van. Más szóval egy hivatkozás viszonya Azure AD-felhasználó, és a kapcsolódó felhasználó @Task létre kell hozni.   
A hivatkozás kapcsolat jön létre, a **felhasználónév** értéket rendeli az Azure Active Directory, a **felhasználónév** , a program által @Task.
 
Állítsa be, és Azure Active Directory egyszeri bejelentkezéssel való tesztelése @Task, hajtsa végre a következő építőelemek:

1. **[Azure Active Directory konfigurálása az egyszeri bejelentkezés](#configuring-azure-ad-single-single-sign-on)** - engedélyezése a felhasználóknak, hogy ezzel a szolgáltatással.
2. **[Felhasználó létrehozása az Azure Active Directory tesztelése](#creating-an-azure-ad-test-user)** - Azure Active Directory egyszeri bejelentkezéssel való Britta Simon ellenőrzéséhez.
4. **[Létrehozása egy @Tasktest felhasználói](#creating-a-halogen-software-test-user) ** - szeretné, hogy egy partner a Britta Simon a @Taskthat az Azure Active Directory ábrázolása amelyen van csatolva.
5. **[Az Azure Active Directory hozzárendelése tesztelje a felhasználó](#assigning-the-azure-ad-test-user)** - ahhoz, hogy Britta Simon Azure AD az egyszeri bejelentkezés használata.
5. **[Tesztelés egyszeri bejelentkezés](#testing-single-sign-on)** - ellenőrzéséhez, hogy működik-e a konfigurációs.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure Active Directory Single Sign-On konfigurálása

Ez a szakasz célja Azure AD az egyszeri bejelentkezés az Azure klasszikus portálon engedélyezése és konfigurálása az egyszeri bejelentkezés a a @Task alkalmazást.

**Azure AD az egyszeri bejelentkezés a konfigurálása @Task, hajtsa végre az alábbi lépéseket:**

1. Az Azure klasszikus portálon kattintson a **@Task** alkalmazás integrálását lapján kattintson a **Konfigurálás egyszeri bejelentkezés** **Konfigurálása az egyszeri bejelentkezés** párbeszédpanel megnyitása.

    ![Egyszeri bejelentkezés beállítása][6] 

2. Kattintson a **hogyan szeretné, hogy jelentkezzen be a felhasználók @Task ** oldalon, válassza az **Azure Active Directory Single Sign-On**, és kattintson a **Tovább gombra**.

    ![Azure Active Directory Single Sign-On][7] 

3. Az **Alkalmazás beállításainak megadása** párbeszédpanel lapon hajtsa végre az alábbi lépéseket:

    ![Alkalmazás beállításainak konfigurálása][8] 
 
     egy. **Jelentkezzen be URL-cím** mezőbe írja be a felhasználóknak, hogy bejelentkezésre által használt URL-CÍMÉT a @Task alkalmazás (például:*https://<Tenant name>.attask-ondemand.com*).

     b. Kattintson a **Tovább**gombra.

4. Kattintson a **egyszeri bejelentkezés a konfigurálása @Task ** lapon kattintson a **metaadat-alapú letöltése**, mentse a metaadat-fájlt a számítógépen helyben, és kattintson a **Tovább gombra**.

    ![Mi az Azure AD Connect][9] 



1. Bejelentkezés a a @Task vállalati webhely rendszergazdaként.

2. Nyissa meg az **egyszeri bejelentkezés beállítása**.


1. Az **Egyszeri bejelentkezés** párbeszédpanelen hajtsa végre az alábbi lépésekkel

    ![Egyszeri bejelentkezés beállítása][23]

    egy. **Típusa**csoportban jelölje ki a **SAML 2.0-s verziója**.

    b. Jelölje ki a **Service Provider azonosítója**.

    c billentyűkombinációt. A klasszikus Azure portálon **Távoli bejelentkezési URL-cím**másolása, és illessze be a **Bejelentkezési portál URL-cím** mezőben lévő értéket.

    d. A klasszikus Azure portálon **Egyetlen Sign-Out szolgáltatás URL-cím**másolása, és illessze be a **Sign-Out URL-címe** mezőben lévő értéket.

    e. Az Azure klasszikus portálon másolja a vágólapra az **URL-cím módosítása jelszót**, és illessze be az **URL-cím módosítása jelszó** szövegmezőt.

    f. Kattintson a **Mentés**gombra.

6. Az Azure klasszikus portálon jelölje be a egyszeri bejelentkezéses konfigurációs megerősítés, és kattintson a **Tovább gombra**. 

    ![Mi az Azure AD Connect][10]

7. Az **egyszeri bejelentkezés megerősítés** lapon kattintson a **Befejezés**gombra.  

    ![Mi az Azure AD Connect][11]




### <a name="creating-an-azure-ad-test-user"></a>Azure AD-próba felhasználó létrehozása
Ez a szakasz célja vizsgálat felhasználó létrehozása az Azure-Britta Simon nevű klasszikus portálon.  

![Azure Active Directory-felhasználó létrehozása][20]

**Teszt felhasználó létrehozása az Azure Active Directory, hajtsa végre az alábbi lépéseket:**

1. Az **Azure klasszikus portálon**kattintson a bal oldali navigációs területen kattintson az **Active Directory**.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-attask-tutorial/create_aaduser_02.png) 

2. A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3. A menüben, a képernyő tetején, a felhasználók listájának megjelenítéséhez kattintson a **felhasználók**elemre.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-attask-tutorial/create_aaduser_03.png) 
 
4. **Felhasználó hozzáadása**elemre az alsó eszköztáron a **Felhasználó hozzáadása** párbeszédpanel megnyitásához. 

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-attask-tutorial/create_aaduser_04.png) 

5. A párbeszédpanel **közli velünk a felhasználóra vonatkozó** lapon hajtsa végre az alábbi lépéseket: 

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-attask-tutorial/create_aaduser_05.png) 

    egy. Felhasználó típusa jelölje be az új felhasználó a szervezet.

    b. A felhasználónév **szövegmezőbe**írja be a **BrittaSimon**.

    c billentyűkombinációt. Kattintson a **Tovább**gombra.

6.  A **Felhasználói profil** párbeszédpanel lapon hajtsa végre az alábbi lépéseket: 

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-attask-tutorial/create_aaduser_06.png) 
 
    egy. Az **első neve** mezőbe írja be a **Britta**.  

    b. Az **Utolsó neve** mezőbe írja be, **Simon**.

    c billentyűkombinációt. A **Megjelenítendő név** mezőbe írja be a **Britta Simon**.

    d. A **szerepkör** listában jelölje ki a **felhasználót**.
    e. Kattintson a **Tovább**gombra.

7. A párbeszédpanel **ideiglenes jelszót kérjen** lapon kattintson a **Hozzon létre**.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-attask-tutorial/create_aaduser_07.png) 
 
8. A párbeszédpanel **ideiglenes jelszót első** lapon hajtsa végre az alábbi lépéseket:

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-attask-tutorial/create_aaduser_08.png) 
  
    egy. Jegyezze fel az **Új jelszót**az érték.

    b. Kattintson a **kész**gombra.   

  
 
### <a name="creating-an-task-test-user"></a>Létrehozása egy @Task teszt felhasználó

Ebben a szakaszban az a célja, hogy hozzon létre egy felhasználót Britta Simon nevű @Task.


**Egy felhasználó Britta Simon nevű létrehozása @Task, hajtsa végre az alábbi lépéseket:**

1. Jelentkezzen be a @Task vállalati webhely rendszergazdaként.

2. A felső sávon kattintson a **személyek**elemre.

3. Kattintson az **új személyt**. 

4. Az új személyt párbeszédpanelen hajtsa végre az alábbi lépéseket:

    ![Hozzon létre egy @Task vizsgálat felhasználói][21] 

    egy. Az **első neve** mezőbe írja be a "Britta".

    b. Az **Utolsó neve** mezőbe írja be a "Simon".

    c billentyűkombinációt. Az **E-mail cím** mezőbe írja be az Azure Active Directory Britta Simon e-mail címét.

    d. Kattintson a **személy hozzáadása**gombra.




### <a name="assigning-the-azure-ad-test-user"></a>Az Azure Active Directory-próba felhasználói hozzárendelése

Ez a szakasz célja Britta Simon Azure egyszeri bejelentkezés a saját hozzáférés engedélyezése használandó engedélyezése @Task.

![Felhasználó hozzárendelése][200] 

**Britta Simon szeretne hozzárendelni @Task, hajtsa végre az alábbi lépéseket:**

1. Az Azure klasszikus portál kattintva nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson az **alkalmazások** parancsra a felső menüben.

    ![Felhasználó hozzárendelése][201] 

2. Az alkalmazások listájában jelölje ki a **@Task**.

    ![Felhasználó hozzárendelése][202] 

1. A felső sávon kattintson a **felhasználók**elemre.

    ![Felhasználó hozzárendelése][203] 

1. A felhasználók listában jelölje ki a **Britta Simon**.

2. Az alsó eszköztáron kattintson a **hozzárendelni**.

    ![Felhasználó hozzárendelése][205]



### <a name="testing-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

Ez a szakasz célja a Azure Active Directory egyszeri bejelentkezéses beállítások tesztelése a Access panelen.  
Amikor rákattint a @Task csempével az Access panel kapja automatikusan jelentkezett-on való a @Task alkalmazást.


## <a name="additional-resources"></a>További források

* [A szoftver alkalmazások integrálása az Azure Active Directory oktatóanyagok listája](active-directory-saas-tutorial-list.md)
* [Mi az access alkalmazás és az egyszeri bejelentkezés az Azure Active Directory címtárral?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-attask-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-attask-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-attask-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-attask-tutorial/tutorial_general_04.png
[5]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_01.png
[30]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_02.png


[6]: ./media/active-directory-saas-attask-tutorial/tutorial_general_05.png
[7]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_03.png
[8]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_04.png
[9]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_05.png
[10]: ./media/active-directory-saas-attask-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-attask-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-attask-tutorial/tutorial_general_100.png
[21]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_08.png


[23]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_06.png

[200]: ./media/active-directory-saas-attask-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-attask-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_09.png
[203]: ./media/active-directory-saas-attask-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-attask-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-attask-tutorial/tutorial_general_205.png






