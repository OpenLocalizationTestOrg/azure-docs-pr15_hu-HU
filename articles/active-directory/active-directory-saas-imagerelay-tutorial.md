<properties
    pageTitle="Oktatóprogram: Azure Active Directory-integráció a ImageRelay |} Microsoft Azure"
    description="Megtudhatja, hogy miként konfigurálása az egyszeri bejelentkezés Azure Active Directory és ImageRelay között."
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
    ms.date="08/15/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-imagerelay"></a>Oktatóprogram: Azure Active Directory-integráció a ImageRelay

Ebben az oktatóanyagban célja mutatja, hogy miként ImageRelay integrálása az Azure Active Directory (Azure Active Directory).  
ImageRelay integrálása az Azure Active Directory biztosít a következő előnyökkel jár:

- Az Azure Active Directory, aki rendelkezik hozzáféréssel ImageRelay megadhatja, hogy
- Engedélyezheti a felhasználóknak, hogy automatikusan első jelentkezett-on való ImageRelay (egyszeri bejelentkezéssel) az Azure Active Directory-fiókok
- A fiókokat az egy központi helyen – az Azure klasszikus portálra

Ha többet szeretne tudni a szoftver alkalmazás integrálása az Azure AD meg, hogy, ismerje meg, [az access alkalmazás és az egyszeri bejelentkezés az Azure Active Directory címtárral](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek

Azure Active Directory integráció a ImageRelay van szükség az alábbiakat:

- Az Azure Active Directory-előfizetéssel
- ImageRelay egyszeri bejelentkezéses engedélyezett előfizetés


> [AZURE.NOTE] Az oktatóprogram lépéseit teszteléséhez nem használata ajánlott munkakörnyezetben.


Az oktatóprogram lépéseit teszteléséhez célszerű követnie az alábbi javaslatokat:

- Erre akkor szükség, ne használja a termelési környezetén.
- Ha nincs telepítve az Azure Active Directory próba környezetben, kattint egy hónap próba [Itt](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban célja lehetővé teszi az Azure Active Directory egyszeri bejelentkezés tesztkörnyezetben tesztelése.  
Az alkalmazási példát, ebben az oktatóanyagban tagolt két fő építőelemek áll:

1. ImageRelay hozzáadása a gyűjteményből

2. Beállítása és tesztelése Azure Active Directory egyszeri bejelentkezés


## <a name="adding-imagerelay-from-the-gallery"></a>ImageRelay hozzáadása a gyűjteményből
Konfigurálja az Azure Active Directory integrálása a ImageRelay, meg kell ImageRelay hozzáadása az felügyelt szoftver alkalmazáslistában a gyűjteményből.

**A gyűjteményből ImageRelay felvételéhez hajtsa végre az alábbi lépéseket:**

1. Az Azure klasszikus portálon kattintson a bal oldali navigációs területen kattintson az **Active Directory**. 

    ![Az Active Directory][1]

2. A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3. Nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson a felső menüben **alkalmazások** .

    ![Alkalmazások][2]

4. Kattintson a **Hozzáadás** az oldal alján.

    ![Alkalmazások][3]

5. **Milyen feladatot szeretne tenni** párbeszédpanelen kattintson **a gyűjteményből az alkalmazások felvétele**.

    ![Alkalmazások][4]

6. A Keresés mezőbe írja be a **ImageRelay**.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_01.png)

7. Az eredmény ablaktáblában jelölje ki a **ImageRelay**, és válassza a **kész** , az alkalmazás hozzáadása gombra.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Beállítása és tesztelése Azure Active Directory egyszeri bejelentkezés
Ez a szakasz célja mutatja, hogy állítsa be, és a "Britta Simon" nevű vizsgálat felhasználón alapuló ImageRelay Azure AD az egyszeri bejelentkezés tesztelése.

Egyszeri bejelentkezés a munkát Azure AD meg kell adni egy felhasználói fiókot, amely a kapcsolódó felhasználó ImageRelay.  Más szóval Azure AD-felhasználó, és a kapcsolódó felhasználó ImageRelay hivatkozás viszonya kell létrehozni.  
A hivatkozás kapcsolat jön létre által az Azure Active Directory ImageRelay **felhasználónév** értékként a **felhasználó neve** értéket rendeli.

Állítsa be, és Azure Active Directory egyszeri bejelentkezéssel való ImageRelay tesztelése, a következő építőelemek szükséges:

1. **[Azure Active Directory konfigurálása az egyszeri bejelentkezés](#configuring-azure-ad-single-single-sign-on)** - engedélyezése a felhasználóknak, hogy ezzel a szolgáltatással.
2. **[Felhasználó létrehozása az Azure Active Directory tesztelése](#creating-an-azure-ad-test-user)** - Azure Active Directory egyszeri bejelentkezéssel való Britta Simon ellenőrzéséhez.
4. **[Felhasználó létrehozása egy ImageRelay tesztelése](#creating-a-userecho-test-user)** - van-e egy megfelelője a Britta Simon ImageRelay, amelyen az Azure Active Directory ábrázolt csatolt.
5. **[Az Azure Active Directory hozzárendelése tesztelje a felhasználó](#assigning-the-azure-ad-test-user)** - ahhoz, hogy Britta Simon Azure AD az egyszeri bejelentkezés használata.
5. **[Tesztelés egyszeri bejelentkezés](#testing-single-sign-on)** - ellenőrzéséhez, hogy működik-e a konfigurációs.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure Active Directory Single Sign-On konfigurálása

Ez a szakasz célja Azure AD az egyszeri bejelentkezés az Azure klasszikus portálon engedélyezése és a ImageRelay alkalmazásban az egyszeri bejelentkezés beállítása.


**Állítson be Azure AD az egyszeri bejelentkezés ImageRelay, hajtsa végre az alábbi lépéseket:**

1. Az Azure klasszikus portálon **ImageRelay** alkalmazás integrációs lapon kattintson **konfigurálása az egyszeri bejelentkezés** a **Konfigurálása az egyszeri bejelentkezés** párbeszédpanel megnyitásához.

     ![Egyszeri bejelentkezés beállítása][6] 

2. **Hogyan szeretné, hogy jelentkezzen be az ImageRelay felhasználók** lapon válassza az **Azure Active Directory Single Sign-On**, és kattintson a **Tovább gombra**.

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_03.png) 

3. Az **Alkalmazás beállításainak megadása** párbeszédpanel lapon hajtsa végre az alábbi lépéseket:

     ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_04.png) 

    egy. **Bejelentkezési a URL-cím** mezőbe írja be a ImageRelay alkalmazás bejelentkezni a felhasználók által használt URL-CÍMÉT (például: *https://fabrikam.ImageRelay.com/*).

    b. Kattintson a **Tovább**gombra.

4. A **beállítás az egyszeri bejelentkezés ImageRelay a** lapon hajtsa végre az alábbi lépéseket:

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_05.png) 

    egy. Kattintson a **tanúsítvány letöltése**, és mentse a fájlt a számítógépen.

    b. Kattintson a **Tovább**gombra.

5. Egy másik böngészőablakban jelentkezzen be a ImageRelay vállalati webhely rendszergazdaként.

1. Kattintson az eszköztár a képernyő tetején, a **felhasználók és engedélyek** terhelést.

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_06.png) 

1. Kattintson az **új jogosultsági létrehozása**gombra.

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_08.png) 

1. A terhelést a **Egyszeri bejelentkezési a beállításokat** jelölje be a **csak bejelentkezés egyszeri bejelentkezés keresztül is ennek a csoportnak a** jelölőnégyzetét, és kattintson a **Mentés**gombra.

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_09.png) 

1. Nyissa meg a **Fiókbeállítások elemre**.

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_10.png) 

1. Nyissa meg a terhelést a **Egyszeri bejelentkezési a beállításai** .

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_11.png)

1. A **SAML beállításai** párbeszédpanelen hajtsa végre az alábbi lépéseket:

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_12.png)

    egy. Az Azure klasszikus portálon **Egyszeri bejelentkezéses szolgáltatás URL-cím**másolása, és illessze be a **Bejelentkezési URL-cím** mezőben lévő értéket.


    b. Az Azure klasszikus portálon **Egyetlen Sign-Out szolgáltatás URL-cím**másolása, és illessze be a **Kijelentkezés URL-címe** mezőben lévő értéket.

    c billentyűkombinációt. **Névformátum azonosítója**, jelölje ki a **urn: oasis: nevek: tc: SAML:1.1:nameid-formátum: emailAddress**.

    
    d. **Kötelező beállítások iránti kérelmek a szolgáltató (kép továbbítási)**jelölje ki a **BEJEGYZÉST kötelező**.
   

    e. **X.509-es tanúsítvány**, csoportban kattintson a **Frissítés tanúsítvány**.

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_17.png)

    f. Nyissa meg a letöltött tanúsítvány a Jegyzettömbben, a tartalom másolása és beillesztése a x.509-es tanúsítvány mezőben lévő értéket.
  
    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_18.png)

    g. **Programjának dinamikus fordítója felhasználói kiépítési** csoportjában jelölje be a **Engedélyezése programjának dinamikus fordítója felhasználói kiépítési**.

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_19.png)

    h. Jelölje ki az engedélycsoportot (például **Egyszerű SSO**) csak használatával egyszeri bejelentkezés a bejelentkezéshez engedett.

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_20.png)

    lehet. Kattintson a **Mentés**gombra.

6. Az Azure klasszikus portálon válassza ki az egyszeri bejelentkezéses konfigurációs megerősítő, és kattintson a **Tovább gombra**.

    ![Azure Active Directory Single Sign-On][10]

7. Az **egyszeri bejelentkezés megerősítés** lapon kattintson a **Befejezés**gombra.

    ![Azure Active Directory Single Sign-On][11]


### <a name="creating-an-azure-ad-test-user"></a>Azure AD-próba felhasználó létrehozása
Ez a szakasz célja vizsgálat felhasználó létrehozása az Azure-Britta Simon nevű klasszikus portálon.

![Azure Active Directory-felhasználó létrehozása][20]

**Teszt felhasználó létrehozása az Azure Active Directory, hajtsa végre az alábbi lépéseket:**

1. Az **Azure klasszikus portálon**kattintson a bal oldali navigációs területen kattintson az **Active Directory**.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-imagerelay-tutorial/create_aaduser_09.png) 

2. A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3. A menüben, a képernyő tetején, a felhasználók listájának megjelenítéséhez kattintson a **felhasználók**elemre.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-imagerelay-tutorial/create_aaduser_03.png) 

4. **Felhasználó hozzáadása**elemre az alsó eszköztáron a **Felhasználó hozzáadása** párbeszédpanel megnyitásához.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-imagerelay-tutorial/create_aaduser_04.png) 

5. A párbeszédpanel **közli velünk a felhasználóra vonatkozó** lapon hajtsa végre az alábbi lépéseket:

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-imagerelay-tutorial/create_aaduser_05.png) 

    egy. Felhasználó típusa jelölje be az új felhasználó a szervezet.

    b. A felhasználónév **szövegmezőbe**írja be a **BrittaSimon**.

    c billentyűkombinációt. Kattintson a **Tovább**gombra.

6.  A **Felhasználói profil** párbeszédpanel lapon hajtsa végre az alábbi lépéseket:

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-imagerelay-tutorial/create_aaduser_06.png) 

    egy. Az **első neve** mezőbe írja be a **Britta**.  

    b. Az **Utolsó neve** mezőbe írja be, **Simon**.

    c billentyűkombinációt. A **Megjelenítendő név** mezőbe írja be a **Britta Simon**.

    d. A **szerepkör** listában jelölje ki a **felhasználót**.

    e. Kattintson a **Tovább**gombra.

7. A párbeszédpanel **ideiglenes jelszót kérjen** lapon kattintson a **Hozzon létre**.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-imagerelay-tutorial/create_aaduser_07.png) 

8. A párbeszédpanel **ideiglenes jelszót első** lapon hajtsa végre az alábbi lépéseket:

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-imagerelay-tutorial/create_aaduser_08.png) 

    egy. Jegyezze fel az **Új jelszót**az érték.

    b. Kattintson a **kész**gombra.   



### <a name="creating-a-imagerelay-test-user"></a>ImageRelay próba felhasználó létrehozása

Ez a szakasz célja Britta Simon nevű ImageRelay felhasználó létrehozása.

**Britta Simon nevű ImageRelay felhasználó létrehozása, hajtsa végre az alábbi lépéseket:**

1. Bejelentkezés a ImageRelay vállalati webhelyre rendszergazdaként.

1. Nyissa meg a **felhasználók és engedélyek** , és válassza ki az **Egyszeri bejelentkezés felhasználó létrehozása**.

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_21.png) 

1. Írja be a **e-mailek**és az **Utónév** **Vezetéknév** és **Vállalati** annak a felhasználónak szeretne kiépítése, és jelölje ki az engedélycsoportot (például egyszeri bejelentkezés egyszerű), csak használatával az egyszeri bejelentkezés jelentkezhet be a csoport vagyis.

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_22.png) 

1. Kattintson a **létrehozása**gombra.

### <a name="assigning-the-azure-ad-test-user"></a>Az Azure Active Directory-próba felhasználói hozzárendelése

Ez a szakasz célja Britta Simon Azure egyszeri bejelentkezés a saját hozzáférést biztosít ImageRelay használandó engedélyezése.

![Felhasználó hozzárendelése][200] 

**Britta Simon hozzárendelése ImageRelay, hajtsa végre az alábbi lépéseket:**

1. Az Azure klasszikus portálon nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintva **alkalmazások** felső menüjében.

    ![Felhasználó hozzárendelése][201] 

2. Az alkalmazások listájában jelölje ki a **ImageRelay**.

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_23.png) 

1. A felső sávon kattintson a **felhasználók**elemre.

    ![Felhasználó hozzárendelése][203] 

1. A felhasználók listában jelölje ki a **Britta Simon**.

2. Az alsó eszköztáron kattintson a **hozzárendelni**.

    ![Felhasználó hozzárendelése][205]


### <a name="testing-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

Ez a szakasz célja a Azure Active Directory egyszeri bejelentkezéses beállítások tesztelése a Access panelen.  
Amikor az Access panel a ImageRelay mozaik gombra kattint, meg kell első automatikusan aláírt-on az ImageRelay alkalmazás.


## <a name="additional-resources"></a>További források

* [A szoftver alkalmazások integrálása az Azure Active Directory oktatóanyagok listája](active-directory-saas-tutorial-list.md)
* [Mi az access alkalmazás és az egyszeri bejelentkezés az Azure Active Directory címtárral?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_205.png
