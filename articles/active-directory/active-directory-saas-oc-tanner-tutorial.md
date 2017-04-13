<properties
    pageTitle="Oktatóprogram: Azure Active Directory-integráció a O.C. Péter - AppreciateHub |} Microsoft Azure"
    description="Egyszeri bejelentkezés Azure Active Directory és O.C. között konfigurálásának ismertetése Péter - AppreciateHub."
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


# <a name="tutorial-azure-active-directory-integration-with-oc-tanner---appreciatehub"></a>Oktatóprogram: Azure Active Directory-integráció a O.C. Péter - AppreciateHub

Ebben az oktatóanyagban célja mutatja, hogy hogyan O.C. integrálása Péter - AppreciateHub az Azure Active Directory (Azure Active Directory).  
O.C. integrálása Péter – az Azure Active Directory AppreciateHub biztosít a következő előnyökkel jár: 

- Az Azure Active Directory, aki rendelkezik hozzáféréssel O.C. megadhatja, hogy Péter - AppreciateHub 
- Engedélyezheti a felhasználóknak, hogy automatikusan első jelentkezett-on való O.C. Péter - AppreciateHub (Single Sign-On) az Azure Active Directory-fiókok
- A fiókokat az egy központi helyen – az Azure klasszikus portálra

Ha többet szeretne tudni a szoftver alkalmazás integrálása az Azure AD meg, hogy, ismerje meg, [az access alkalmazás és az egyszeri bejelentkezés az Azure Active Directory címtárral](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek 

Azure AD-integráció O.C. konfigurálása Péter - AppreciateHub, akkor az alábbiakat:

- Az Azure Active Directory-előfizetéssel
- EGY O.C. Péter - AppreciateHub egyszeri bejelentkezés engedélyezett előfizetés


> [AZURE.NOTE] Az oktatóprogram lépéseit teszteléséhez nem használata ajánlott munkakörnyezetben.


Az oktatóprogram lépéseit teszteléséhez célszerű követnie az alábbi javaslatokat:

- Erre akkor szükség, ne használja a termelési környezetén.
- Ha nincs telepítve az Azure Active Directory próba környezetben, kattint egy hónap próba [Itt](https://azure.microsoft.com/pricing/free-trial/). 

 
## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban célja lehetővé teszi az Azure Active Directory egyszeri bejelentkezés tesztkörnyezetben tesztelése.  
Az alkalmazási példát, ebben az oktatóanyagban tagolt három fő építőelemek áll:

1. O.C. hozzáadása Péter - AppreciateHub a gyűjteményből 
2. Beállítása és tesztelése Azure Active Directory egyszeri bejelentkezés


## <a name="adding-oc-tanner---appreciatehub-from-the-gallery"></a>O.C. hozzáadása Péter - AppreciateHub a gyűjteményből
O.C. integrációja konfigurálása Péter - AppreciateHub az Azure Active Directory, fel kell vennie O.C. Péter - AppreciateHub a gyűjteményből a felügyelt szoftver alkalmazások listájára.

**A gyűjteményből AppreciateHub O.C. Péter - hozzáadása hajtsa végre az alábbi lépéseket:**

1. Az **Azure klasszikus portálon**kattintson a bal oldali navigációs területen kattintson az **Active Directory**. 

    ![Az Active Directory][1] 

2. A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3. Nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson a felső menüben **alkalmazásokat** .

    ![Alkalmazások][2] 

4. Kattintson a **Hozzáadás** az oldal alján.

    ![Alkalmazások][3] 

5. **Milyen feladatot szeretne tenni** párbeszédpanelen kattintson **a gyűjteményből az alkalmazások felvétele**.

    ![Alkalmazások][4] 

6. A Keresés mezőbe írja be a **O.C. Péter - AppreciateHub**.

    ![Alkalmazások][5] 

7. Az eredmény ablaktáblában jelölje ki a **O.C. Péter - AppreciateHub**, és válassza a **kész** , az alkalmazás hozzáadása gombra.

    ![Alkalmazások][25] 




##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Beállítása és tesztelése Azure Active Directory egyszeri bejelentkezés

Ez a szakasz célja mutatja, hogy beállítása és Azure Active Directory egyszeri bejelentkezéssel való O.C. tesztelése Péter - AppreciateHub "Britta Simon" nevű próba felhasználó alapján.

Egyszeri bejelentkezés a munkát, az Azure Active Directory kell, hogy milyen a megfelelőjük a felhasználó O.C. Péter - AppreciateHub az Azure Active Directory-felhasználónak van. Más szóval Azure AD-felhasználó, és a kapcsolódó felhasználó O.C. közötti hivatkozás kapcsolat Péter - AppreciateHub kell létrehozni.  
A hivatkozás kapcsolat jön létre, a program a **felhasználónév** a O.C. Azure AD az az érték, a **felhasználónév** hozzárendelésével Péter - AppreciateHub.
 
Állítsa be, és Azure Active Directory egyszeri bejelentkezéssel való O.C. tesztelése Péter - AppreciateHub, szükséges a következő építőelemek:

1. **[Azure Active Directory konfigurálása az egyszeri bejelentkezés](#configuring-azure-ad-single-single-sign-on)** - engedélyezése a felhasználóknak, hogy ezzel a szolgáltatással.
2. **[Felhasználó létrehozása az Azure Active Directory tesztelése](#creating-an-azure-ad-test-user)** - Azure Active Directory egyszeri bejelentkezéssel való Britta Simon ellenőrzéséhez.
4. **[Egy O.C. Péter - AppreciateHub teszt felhasználó létrehozása](#creating-a-halogen-software-test-user)** – van-e egy megfelelője a Britta Simon O.C. Péter - AppreciateHub, amelyen az Azure Active Directory ábrázolt csatolt.
5. **[Az Azure Active Directory hozzárendelése tesztelje a felhasználó](#assigning-the-azure-ad-test-user)** - ahhoz, hogy Britta Simon Azure AD az egyszeri bejelentkezés használata.
5. **[Tesztelés egyszeri bejelentkezés](#testing-single-sign-on)** - ellenőrzéséhez, hogy működik-e a konfigurációs.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure Active Directory Single Sign-On konfigurálása

Ez a szakasz célja Azure AD az egyszeri bejelentkezés az Azure klasszikus portálon engedélyezése és beállítása a O.C. egyszeri bejelentkezés Péter - AppreciateHub alkalmazást.


**Azure Active Directory egyszeri bejelentkezés állítson be O.C. Péter - AppreciateHub, hajtsa végre az alábbi lépéseket:**

1. Az Azure klasszikus portálon **O.C. Péter - AppreciateHub** alkalmazás integrációs lapon kattintson **konfigurálása az egyszeri bejelentkezés** a **Konfigurálása az egyszeri bejelentkezés** párbeszédpanel megnyitásához.

    ![Egyszeri bejelentkezés beállítása][6]

2. **Hogyan szeretné, hogy jelentkezzen be az O.C. Péter - AppreciateHub felhasználók** lapon válassza az **Azure Active Directory Single Sign-On**, és kattintson a **Tovább gombra**.

    ![Azure Active Directory Single Sign-On][7]

3. Az **Alkalmazás beállításainak megadása** párbeszédpanel lapon hajtsa végre az alábbi lépéseket:

    ![Alkalmazás beállításainak konfigurálása][8]
 
     egy. Nyissa meg a metaadat-fájlt, a következő hivatkozásra: [https://fed.appreciatehub.com/fed/sp/metadata](https://fed.appreciatehub.com/fed/sp/metadata).

     b. Keresse meg a **md:AssertionConsumerService** csomópontot. 

     c billentyűkombinációt. A **hely** attribútumot értékének másolása. 

     ![Alkalmazás beállításainak konfigurálása][12]
     
     d. Az **Bejelentkezési a URL-címe** mezőbe az érték a múltbeli beszerzett az előző lépésben.

     > [AZURE.NOTE] Ha Ön expiriencing problémák ahhoz, hogy a válasz URL-CÍMÉT a metaadat-fájlt, lépjen kapcsolatba a O.C. Péter - AppreciateHub támogatási csoport keresztül [sso@octanner.com](mailto:sso@octanner.com).

     e. Kattintson a **Tovább**gombra.
 
4. **Konfigurálása az egyszeri bejelentkezés O.C. Péter - AppreciateHub a** lapon kattintson a **metaadat-alapú letöltése**elemre, és mentse a fájlt a számítógépen helyben metaadat.

    ![Mi az Azure AD Connect][9]

5. Lépjen kapcsolatba a O.C. Péter - AppreciateHub támogatási csoport keresztül xyz, küldje el nekik a metaadat-fájlt, és őket arról, hogy vele, hogy azokat meg engedélyezze egyszeri Bejelentkezést.


6. Az Azure klasszikus portálon jelölje be a egyszeri bejelentkezéses konfigurációs megerősítés, és kattintson a **Tovább gombra**. 

    ![Mi az Azure AD Connect][10]

7. Az **egyszeri bejelentkezés megerősítés** lapon kattintson a **Befejezés**gombra.  

    ![Mi az Azure AD Connect][11]




### <a name="creating-an-azure-ad-test-user"></a>Azure AD-próba felhasználó létrehozása
Ez a szakasz célja vizsgálat felhasználó létrehozása az Azure-Britta Simon nevű klasszikus portálon.  

![Azure Active Directory-felhasználó létrehozása][20]

**Teszt felhasználó létrehozása az Azure Active Directory, hajtsa végre az alábbi lépéseket:**

1. Az **Azure klasszikus portálon**kattintson a bal oldali navigációs területen kattintson az **Active Directory**.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-oc-tanner-tutorial/create_aaduser_02.png) 

2. A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3. A menüben, a képernyő tetején, a felhasználók listájának megjelenítéséhez kattintson a **felhasználók**elemre.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-oc-tanner-tutorial/create_aaduser_03.png) 
 
4. **Felhasználó hozzáadása**elemre az alsó eszköztáron a **Felhasználó hozzáadása** párbeszédpanel megnyitásához. 

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-oc-tanner-tutorial/create_aaduser_04.png) 

5. A párbeszédpanel **közli velünk a felhasználóra vonatkozó** lapon hajtsa végre az alábbi lépéseket: 

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-oc-tanner-tutorial/create_aaduser_05.png) 

    egy. Felhasználó típusa jelölje be az új felhasználó a szervezet.

    b. A felhasználónév **szövegmezőbe**írja be a **BrittaSimon**.

    c billentyűkombinációt. Kattintson a **Tovább**gombra.

6.  A **Felhasználói profil** párbeszédpanel lapon hajtsa végre az alábbi lépéseket: 

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-oc-tanner-tutorial/create_aaduser_06.png)
 
    egy. Az **első neve** mezőbe írja be a **Britta**.  

    b. Az **Utolsó neve** mezőbe írja be, **Simon**.

    c billentyűkombinációt. A **Megjelenítendő név** mezőbe írja be a **Britta Simon**.

    d. A **szerepkör** listában jelölje ki a **felhasználót**.
    e. Kattintson a **Tovább**gombra.

7. A párbeszédpanel **ideiglenes jelszót kérjen** lapon kattintson a **Hozzon létre**.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-oc-tanner-tutorial/create_aaduser_07.png) 
 
8. A párbeszédpanel **ideiglenes jelszót első** lapon hajtsa végre az alábbi lépéseket:

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-oc-tanner-tutorial/create_aaduser_08.png) 
  
    egy. Jegyezze fel az **Új jelszót**az érték.

    b. Kattintson a **kész**gombra.   

  
 
### <a name="creating-a-oc-tanner---appreciatehub-test-user"></a>Egy O.C. létrehozása Péter - AppreciateHub teszt felhasználó

Ez a szakasz az a célja, hogy Britta Simon nevű O.C. felhasználó létrehozása Péter - AppreciateHub.

**Hozzon létre egy felhasználót Britta Simon nevű O.C. Péter - AppreciateHub, hajtsa végre az alábbi lépéseket:**

1. Kérje meg a OC Péter támogatási csoport hozhat létre, amelyben nameID attribútum értéke Britta Simon felhasználó neve megegyezik az Azure Active Directory felhasználót.


### <a name="assigning-the-azure-ad-test-user"></a>Az Azure Active Directory-próba felhasználói hozzárendelése

Ez a szakasz az a célja, hogy Britta Simon Azure egyszeri bejelentkezés a saját hozzáférést biztosít O.C. használandó engedélyezése Péter - AppreciateHub.

![Felhasználó hozzárendelése][200]

**Britta Simon hozzárendelése O.C. Péter - AppreciateHub, hajtsa végre az alábbi lépéseket:**

1. Az Azure klasszikus portál kattintva nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson az **alkalmazások** parancsra a felső menüben.

    ![Felhasználó hozzárendelése][201]

2. Az alkalmazások listájában jelölje ki a **O.C. Péter - AppreciateHub**.

    ![Felhasználó hozzárendelése][202]

1. A felső sávon kattintson a **felhasználók**elemre.

    ![Felhasználó hozzárendelése][203]

1. A felhasználók listában jelölje ki a **Britta Simon**.

2. Az alsó eszköztáron kattintson a **hozzárendelni**.

    ![Felhasználó hozzárendelése][205]



### <a name="testing-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

Ez a szakasz célja a Azure Active Directory egyszeri bejelentkezéses beállítások tesztelése a Access panelen.  
Ha a O.C. kattint. Péter - AppreciateHub csempe az Access panel, meg kell első automatikusan aláírt-on való a O.C. Péter - AppreciateHub alkalmazást.


## <a name="additional-resources"></a>További források

* [A szoftver alkalmazások integrálása az Azure Active Directory oktatóanyagok listája](active-directory-saas-tutorial-list.md)
* [Mi az access alkalmazás és az egyszeri bejelentkezés az Azure Active Directory címtárral?](active-directory-appssoaccess-whatis.md)


<!--Image references-->
[1]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_general_04.png
[5]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_octanner_01.png
[25]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_octanner_25.png


[6]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_general_05.png
[7]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_octanner_02.png
[8]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_octanner_03.png
[9]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_octanner_04.png
[10]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_octanner_05.png
[11]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_octanner_06.png
[12]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_octanner_08.png
[20]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_octanner_07.png
[203]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_general_205.png






