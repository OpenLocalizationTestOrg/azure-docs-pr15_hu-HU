<properties
    pageTitle="Oktatóprogram: Azure Active Directory-integráció a Kindling |} Microsoft Azure"
    description="Megtudhatja, hogy miként konfigurálása az egyszeri bejelentkezés Azure Active Directory és Kindling között."
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
    ms.date="09/09/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-kindling"></a>Oktatóprogram: Azure Active Directory-integráció a Kindling

Ebben az oktatóanyagban célja mutatja, hogy miként Kindling integrálása az Azure Active Directory (Azure Active Directory).  
Kindling integrálása az Azure Active Directory biztosít a következő előnyökkel jár: 

- Az Azure Active Directory, aki rendelkezik hozzáféréssel Kindling megadhatja, hogy 
- Engedélyezheti a felhasználóknak, hogy automatikusan megkapja jelentkezett-on való Kindling (Single Sign-On) az Azure Active Directory-fiókok
- A fiókokat az egy központi helyen – az Azure klasszikus portálra 


Ha többet szeretne tudni a szoftver alkalmazás integrálása az Azure AD meg, hogy, ismerje meg, [az access alkalmazás és az egyszeri bejelentkezés az Azure Active Directory címtárral](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek 

Azure Active Directory integráció a Kindling van szükség az alábbiakat:

- Az Azure Active Directory-előfizetéssel
- Kindling előfizetés


> [AZURE.NOTE] Az oktatóprogram lépéseit teszteléséhez nem használata ajánlott munkakörnyezetben.


Az oktatóprogram lépéseit teszteléséhez célszerű követnie az alábbi javaslatokat:

- Erre akkor szükség, ne használja a termelési környezetén.
- Ha nincs telepítve az Azure Active Directory próba környezetben, kattint egy hónap próba [Itt](https://azure.microsoft.com/pricing/free-trial/). 

 
## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban célja lehetővé teszi az Azure Active Directory egyszeri bejelentkezés tesztkörnyezetben tesztelése.  
Az alkalmazási példát, ebben az oktatóanyagban tagolt két fő építőelemek áll:

1. Kindling hozzáadása a gyűjteményből 
2. Beállítása és tesztelése Azure Active Directory egyszeri bejelentkezés


## <a name="adding-kindling-from-the-gallery"></a>Kindling hozzáadása a gyűjteményből
Adja meg a integrálása az Azure Active Directory Kindling, szüksége Kindling hozzáadása az felügyelt szoftver alkalmazáslistában a gyűjteményből.

**A gyűjteményből Kindling felvételéhez hajtsa végre az alábbi lépéseket:**

1. Az **Azure klasszikus portálon**kattintson a bal oldali navigációs területen kattintson az **Active Directory**.

    ![Az Active Directory][1]

2. A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3. Nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson a felső menüben **alkalmazásokat** .

    ![Alkalmazások][2]

4. Kattintson a **Hozzáadás** az oldal alján.

    ![Alkalmazások][3]

5. **Milyen feladatot szeretne tenni** párbeszédpanelen kattintson **a gyűjteményből az alkalmazások felvétele**.

    ![Alkalmazások][4]

6. A Keresés mezőbe írja be a **Kindling**.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-kindling-tutorial/tutorial_kindling_01.png)

7. Az eredmény ablaktáblában jelölje ki a **Kindling**, és válassza a **kész** , az alkalmazás hozzáadása parancsra.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-kindling-tutorial/tutorial_kindling_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Beállítása és tesztelése Azure Active Directory egyszeri bejelentkezés
Ez a szakasz célja mutatja, hogy állítsa be, és a "Britta Simon" nevű próba felhasználón alapuló Kindling Azure AD az egyszeri bejelentkezés tesztelése.

Az egyszeri bejelentkezési a munkát az Azure Active Directory kell, hogy mi az a partner felhasználó Kindling az Azure Active Directory-felhasználónak. Más szóval Azure AD-felhasználó, és a kapcsolódó felhasználó Kindling hivatkozás viszonya kell létrehozni.  
A hivatkozás kapcsolat jön létre által az Azure Active Directory Kindling **felhasználónév** értékként a **felhasználó neve** értéket rendeli.
 
Állítsa be, és Azure Active Directory egyszeri bejelentkezéssel való Kindling tesztelése, az alábbi építőelemeket szükséges:

1. **[Azure Active Directory konfigurálása az egyszeri bejelentkezés](#configuring-azure-ad-single-single-sign-on)** - engedélyezése a felhasználóknak, hogy ezzel a szolgáltatással.
2. **[Felhasználó létrehozása az Azure Active Directory tesztelése](#creating-an-azure-ad-test-user)** - Azure Active Directory egyszeri bejelentkezéssel való Britta Simon ellenőrzéséhez.
4. Az Azure Active Directory ábrázolása hogy csatolva **[Kindling próba felhasználó létrehozása](#creating-a-kindling-test-user)** – van-e egy megfelelőjük Britta Simon, azaz Kindling.
5. **[Az Azure Active Directory hozzárendelése tesztelje a felhasználó](#assigning-the-azure-ad-test-user)** - ahhoz, hogy Britta Simon Azure AD az egyszeri bejelentkezés használata.
6. **[Tesztelés egyszeri bejelentkezés](#testing-single-sign-on)** - ellenőrzéséhez, hogy működik-e a konfigurációs.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure Active Directory Single Sign-On konfigurálása

Ez a szakasz célja Azure AD az egyszeri bejelentkezés az Azure klasszikus portálon engedélyezése és a Kindling alkalmazásban az egyszeri bejelentkezés beállítása. Ez az eljárás részeként biztosan szükséges számrendszerben-64 kódolt tanúsítvány fájlt. Ha nem ismeri a ezt az eljárást, olvassa el a [szövegfájlba bináris tanúsítvány konvertálása](http://youtu.be/PlgrzUZ-Y1o)című témakört.

Egyszeri bejelentkezés az Kindling konfigurálásához kell regisztrált tartományt. Ha még nincs regisztrált tartományt, a Kindling támogatási csoport munkatársaitól keresztül [support@kindlingapp.com](mailto:support@kindlingapp.com).  



**Állítson be Azure AD az egyszeri bejelentkezés Kindling, hajtsa végre az alábbi lépéseket:**

1. Az Azure klasszikus portálon alkalmazás integrációs **Kindling** lapján kattintson **konfigurálása az egyszeri bejelentkezés** a **Konfigurálása az egyszeri bejelentkezés** párbeszédpanel megnyitásához.

    ![Egyszeri bejelentkezés beállítása][6] 

2. **Hogyan megtekinti Kindling jelentkezzen be felhasználók** lapon válassza az **Azure Active Directory Single Sign-On**, és kattintson a **Tovább gombra**.

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-kindling-tutorial/tutorial_kindling_03.png) 

3. Az **Alkalmazás beállításainak megadása** párbeszédpanel lapon hajtsa végre az alábbi lépéseket:

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-kindling-tutorial/tutorial_kindling_04.png) 


    egy. Az **Bejelentkezési a URL-cím** mezőbe írja be a bejelentkezés az Kindling alkalmazás a következő mintát használatával a felhasználók által használt URL-címe:`https://<company name>.kindlingapp.com/`

    b. Elrendezés Kindling támogatási csoport munkatársaitól kaphat keresztül [support@kindlingapp.com](mailto:support@kindlingapp.com) a **kibocsátó** és a **Válasz URL-cím** értéket.   

    c billentyűkombinációt. Az **kibocsátó** mezőbe írja be a kibocsátó URL-CÍMÉT.

    d. **Válasz URL-cím** mezőbe írja be a választ URL-CÍMÉT.   
 
    e. Kattintson a **Tovább**gombra.
 
 
4. A **beállítás az egyszeri bejelentkezés Kindling a** lapon hajtsa végre az alábbi lépéseket:

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-kindling-tutorial/tutorial_kindling_05.png) 

    egy. Kattintson a **tanúsítvány letöltése**, és mentse a fájlt a számítógépen.

    b. Kattintson a **Tovább**gombra.



1. A Kindling támogatási csoport munkatársaitól keresztül [support@kindlingapp.com](mailto:support@kindlingapp.com) , és küldje el nekik a következő:

    - A letöltött tanúsítvány
    - A **Kibocsátó URL-címe** értéket rendeli Kindling a **Szervezet azonosítója**
    - Az **Egyszeri bejelentkezéses szolgáltatás URL-címe** megfelelteti Kindling-féle **Egyszeri Bejelentkezést bejelentkezési a URL-CÍMÉT** 
    - Az **Egyetlen Sign-Out szolgáltatás URL-címe** , amely rendeli Kindling-féle **Egyszeri Bejelentkezést bejelentkezési meg URL-CÍMÉT**. 

6. Az Azure klasszikus portálon válassza ki az egyszeri bejelentkezéses konfigurációs megerősítő, és kattintson a **Tovább gombra**. 

    ![Azure Active Directory Single Sign-On][10]

7. Az **egyszeri bejelentkezés megerősítés** lapon kattintson a **Befejezés**gombra.  
    
    ![Azure Active Directory Single Sign-On][11]




### <a name="creating-an-azure-ad-test-user"></a>Azure AD-próba felhasználó létrehozása
Ez a szakasz célja vizsgálat felhasználó létrehozása az Azure-Britta Simon nevű klasszikus portálon.

![Azure Active Directory-felhasználó létrehozása][20]

**Teszt felhasználó létrehozása az Azure Active Directory, hajtsa végre az alábbi lépéseket:**

1. Az **Azure klasszikus portálon**kattintson a bal oldali navigációs területen kattintson az **Active Directory**.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-kindling-tutorial/create_aaduser_09.png) 

2. A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3. A menüben, a képernyő tetején, a felhasználók listájának megjelenítéséhez kattintson a **felhasználók**elemre.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-kindling-tutorial/create_aaduser_03.png) 
 
4. **Felhasználó hozzáadása**elemre az alsó eszköztáron a **Felhasználó hozzáadása** párbeszédpanel megnyitásához. 

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-kindling-tutorial/create_aaduser_04.png) 

5. A párbeszédpanel **közli velünk a felhasználóra vonatkozó** lapon hajtsa végre az alábbi lépéseket: 

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-kindling-tutorial/create_aaduser_05.png)  

    egy. Felhasználó típusa jelölje be az új felhasználó a szervezet.

    b. A felhasználónév **szövegmezőbe**írja be a **BrittaSimon**.

    c billentyűkombinációt. Kattintson a **Tovább**gombra.

6.  A **Felhasználói profil** párbeszédpanel lapon hajtsa végre az alábbi lépéseket: 

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-kindling-tutorial/create_aaduser_06.png) 
 
    egy. Az **első neve** mezőbe írja be a **Britta**.  

    b. Az **Utolsó neve** mezőbe írja be, **Simon**.

    c billentyűkombinációt. A **Megjelenítendő név** mezőbe írja be a **Britta Simon**.

    d. A **szerepkör** listában jelölje ki a **felhasználót**.
    e. Kattintson a **Tovább**gombra.

7. A párbeszédpanel **ideiglenes jelszót kérjen** lapon kattintson a **Hozzon létre**.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-kindling-tutorial/create_aaduser_07.png) 
 
8. A párbeszédpanel **ideiglenes jelszót első** lapon hajtsa végre az alábbi lépéseket:

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-kindling-tutorial/create_aaduser_08.png) 
  
    egy. Jegyezze fel az **Új jelszót**az érték.

    b. Kattintson a **kész**gombra.   

  
 
### <a name="creating-a-kindling-test-user"></a>Kindling próba felhasználó létrehozása

Ez a szakasz célja Britta Simon nevű Kindling felhasználó létrehozása.
Kindling támogatja a kiépítési just time. Már engedélyezte ezt az [Azure Active Directory konfigurálása Single Sign-On](#configuring-azure-ad-single-single-sign-on).

Nincs teendő ebben a szakaszban a meg nem.




### <a name="assigning-the-azure-ad-test-user"></a>Az Azure Active Directory-próba felhasználói hozzárendelése

Ez a szakasz célja Britta Simon Azure egyszeri bejelentkezés a saját hozzáférést biztosít Kindling használandó engedélyezése.

![Felhasználó hozzárendelése][200] 

**Britta Simon hozzárendelése Kindling, hajtsa végre az alábbi lépéseket:**

1. Az Azure klasszikus portál kattintva nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson az **alkalmazások** parancsra a felső menüben.

    ![Felhasználó hozzárendelése][201] 

2. Az alkalmazások listájában jelölje ki a **Kindling**.

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-kindling-tutorial/tutorial_kindling_50.png) 

1. A felső sávon kattintson a **felhasználók**elemre.

    ![Felhasználó hozzárendelése][203] 

1. A felhasználók listában jelölje ki a **Britta Simon**.

2. Az alsó eszköztáron kattintson a **hozzárendelni**.

    ![Felhasználó hozzárendelése][205]



### <a name="testing-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

Ez a szakasz célja a Azure Active Directory egyszeri bejelentkezéses beállítások tesztelése a Access panelen.  
Amikor az Access panel a Kindling mozaik gombra kattint, meg kell első automatikusan aláírt-on az Kindling alkalmazás.


## <a name="additional-resources"></a>További források

* [A szoftver alkalmazások integrálása az Azure Active Directory oktatóanyagok listája](active-directory-saas-tutorial-list.md)
* [Mi az access alkalmazás és az egyszeri bejelentkezés az Azure Active Directory címtárral?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-kindling-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-kindling-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-kindling-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-kindling-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-kindling-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-kindling-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-kindling-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-kindling-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-kindling-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-kindling-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-kindling-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-kindling-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-kindling-tutorial/tutorial_general_205.png






