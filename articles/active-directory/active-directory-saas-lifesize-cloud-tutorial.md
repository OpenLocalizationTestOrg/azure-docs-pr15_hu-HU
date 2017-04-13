<properties
    pageTitle="Oktatóprogram: Azure Active Directory-integráció a Lifesize felhő |} Microsoft Azure"
    description="Megtudhatja, hogy miként konfigurálható az egyszeri bejelentkezés Azure Active Directory és a Lifesize felhő között."
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
    ms.date="10/04/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-lifesize-cloud"></a>Oktatóprogram: Azure Active Directory-integráció a Lifesize felhő

Ebből az oktatóanyagból megismerheti, hogyan Lifesize felhő integrálása az Azure Active Directory (Azure Active Directory).

Lifesize felhő integrálása az Azure Active Directory biztosít a következő előnyökkel jár:

- Az Azure Active Directory, aki rendelkezik hozzáféréssel a felhőbe Lifesize megadhatja, hogy
- Az Azure Active Directory-fiókok engedélyezheti a felhasználóknak, hogy automatikusan első jelentkezett-on Lifesize felhőbe (Single Sign-On)
- A fiókokat az egy központi helyen – az Azure klasszikus portálra

Ha többet szeretne tudni a szoftver alkalmazás integrálása az Azure AD meg, hogy, ismerje meg, [az access alkalmazás és az egyszeri bejelentkezés az Azure Active Directory címtárral](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek

Azure Active Directory integráció a felhőben Lifesize van szükség az alábbiakat:

- Az Azure Active Directory-előfizetéssel
- Egy Lifesize felhő egyszeri bejelentkezés engedélyezett előfizetés


> [AZURE.NOTE] Az oktatóprogram lépéseit teszteléséhez nem használata ajánlott munkakörnyezetben.


Az oktatóprogram lépéseit teszteléséhez célszerű követnie az alábbi javaslatokat:

- Erre akkor szükség, ne használja a termelési környezetén.
- Ha nincs telepítve az Azure Active Directory próba környezetben, kattint egy hónap próba [Itt](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban tesztelni Azure AD az egyszeri bejelentkezés tesztkörnyezetben.

Az alkalmazási példát, ebben az oktatóanyagban tagolt két fő építőelemek áll:

1. Lifesize felhő hozzáadása a gyűjteményből
2. Beállítása és tesztelése Azure Active Directory egyszeri bejelentkezés


## <a name="adding-lifesize-cloud-from-the-gallery"></a>Lifesize felhő hozzáadása a gyűjteményből
Azure Active Directory integrálása a Lifesize felhő konfigurálásához szüksége Lifesize felhő felügyelt szoftver alkalmazásai a gyűjteményből hozzáadása.

**A gyűjteményből Lifesize felhő felvételéhez hajtsa végre az alábbi lépéseket:**

1. Az **Azure klasszikus portálon**kattintson a bal oldali navigációs területen kattintson az **Active Directory**.

    ![Az Active Directory][1]
2. A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3. Nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson a felső menüben **alkalmazások** .

    ![Alkalmazások][2]

4. Kattintson a **Hozzáadás** az oldal alján.

    ![Alkalmazások][3]

5. **Milyen feladatot szeretne tenni** párbeszédpanelen kattintson **a gyűjteményből az alkalmazások felvétele**.

    ![Alkalmazások][4]

6. A Keresés mezőbe írja be a **Lifesize felhő**.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_01.png)

7. Az eredmény ablaktáblában jelölje ki a **Lifesize felhőben**, és válassza a **kész** , az alkalmazás hozzáadása gombra.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_02.png)


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Beállítása és tesztelése Azure Active Directory egyszeri bejelentkezés
Ebben a szakaszban beállíthatja és vizsgálat Azure AD az egyszeri bejelentkezés Lifesize felhő "Britta Simon" nevű vizsgálat felhasználón alapuló.

Az egyszeri bejelentkezési a munkát az Azure Active Directory kell tudható, hogy mi a megfelelőjük a felhasználó Lifesize felhőben egy felhasználóhoz, az Azure Active Directory. Más szóval Azure AD-felhasználó, és a kapcsolódó felhasználó Lifesize felhőben hivatkozás viszonya kell létrehozni.

A hivatkozás kapcsolat jön létre a értéket a **felhasználó nevét** a **felhasználónév** Lifesize felhőben értékként Azure Active Directory hozzárendelésével.

Állítsa be, és Azure Active Directory egyszeri bejelentkezés Lifesize felhőben való tesztelése, a következő építőelemek szükséges:

1. **[Azure Active Directory konfigurálása az egyszeri bejelentkezés](#configuring-azure-ad-single-sign-on)** - engedélyezése a felhasználóknak, hogy ezzel a szolgáltatással.
2. **[Felhasználó létrehozása az Azure Active Directory tesztelése](#creating-an-azure-ad-test-user)** - Azure Active Directory egyszeri bejelentkezéssel való Britta Simon ellenőrzéséhez.
3. **[Felhasználó létrehozása Lifesize felhő tesztelése](#creating-a-lifesize-cloud-test-user)** - szeretné, hogy egy partner a Britta Simon Lifesize a felhőben, hogy az Azure Active Directory ábrázolt csatolt.
4. **[Az Azure Active Directory hozzárendelése tesztelje a felhasználó](#assigning-the-azure-ad-test-user)** - ahhoz, hogy Britta Simon Azure AD az egyszeri bejelentkezés használata.
5. **[Tesztelés egyszeri bejelentkezés](#testing-single-sign-on)** - ellenőrzéséhez, hogy működik-e a konfigurációs.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure Active Directory egyszeri bejelentkezés beállítása

Ebben a részben Azure AD az egyszeri bejelentkezés a klasszikus portálon engedélyezése, és a felhő Lifesize alkalmazás egyszeri bejelentkezés beállítása.


**Állítson be Azure AD az egyszeri bejelentkezés Lifesize felhőben, hajtsa végre az alábbi lépéseket:**

1. Az klasszikus portálon **Lifesize felhő** alkalmazás integrációs lapon kattintson **konfigurálása az egyszeri bejelentkezés** a **Konfigurálása az egyszeri bejelentkezés** párbeszédpanel megnyitásához.
     
    ![Egyszeri bejelentkezés beállítása][6] 

2. **Hogyan szeretné, hogy jelentkezzen be az felhő Lifesize felhasználók** lapon válassza az **Azure Active Directory Single Sign-On**, és kattintson a **Tovább gombra**.

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_03.png) 

3. Az **Alkalmazás beállításainak megadása** párbeszédpanel lapon hajtsa végre az alábbi lépéseket:

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_04.png) 

    egy. **Bejelentkezési a URL-cím** mezőbe írja be a bejelentkezés az Lifesize felhő alkalmazás a következő mintát használatával a felhasználók által használt URL-címe: **https://login.lifesizecloud.com/ls/?acs**.
    
    b. Kattintson a **Tovább** gombra
 
4. **Konfigurálása az egyszeri bejelentkezés a Lifesize felhő** lapon hajtsa végre az alábbi lépéseket:

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_05.png)

    egy. Kattintson a **tanúsítvány letöltése**, és mentse a fájlt a számítógépen.

    b. Kattintson a **Tovább**gombra.


5. Az alkalmazás, jelentkezzen be az Lifesize felhő alkalmazásba rendszergazdai engedélyekkel konfigurált SSO URL.

6. A jobb felső sarokban kattintson a nevére, és válassza a **Speciális beállítások**

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_06.png)

7. A Speciális beállítások most kattintson az **Egyszeri bejelentkezés konfigurálása** hivatkozásra. Ekkor megnyílik az egyszeri bejelentkezés beállítása lapon az előfordulásra vonatkozóan.

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_07.png)

8. Most már az egyszeri bejelentkezés konfigurációs Kezelőfelület állítsa be az alábbi értékeket.    

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_08.png)

    • Azure AD a kibocsátó URL-cím értéket másolja, és illessze be a **Identitás szolgáltató kibocsátó** szövegdoboz.

    • Azure Active Directory távoli bejelentkezési URL-cím értékét másolja és illessze be a **Bejelentkezési URL-cím** mezőbe.

    • A Jegyzettömb alkalmazásban nyissa meg a letöltött tanúsítvány és a tanúsítvány, kivéve a lépések tanúsítvány és a záró tanúsítvány vonalak tartalom másolása, illessze be ezt a **X.509-es tanúsítvány** mezőben lévő értéket.

    • A **Vezetéknév** mező a SAML attribútum hozzárendelésben **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname** , írja be az értéket

    • A **Vezetéknév** mező a SAML attribútum hozzárendelésben **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname** , írja be az értéket

    • A SAML attribútum hozzárendelésben az **E-mail** mezőbe írja be az érték **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**

9. Ellenőrizze a konfigurációt a **vizsgálat** gombra kattinthat.

    > [AZURE.NOTE] A sikeres teszteléshez kell a konfigurálása varázsló az Azure Active Directory és is hozzáférést biztosítanak végezheti el a tesztet jogosult felhasználók vagy csoportok.
    
10. Engedélyezze az egyszeri bejelentkezés, jelölje be a **SSO engedélyezése** gombra.

11. Kattintson a **frissítés** gombra, hogy menti a program minden beállítást. A művelet létrehozza a RelayState értéket. Másolja az RelayState érték, amely a szövegmezőben jön létre. Ezt az értéket a következő lépésekkel rendszer szükséges.

12. A klasszikus portálon jelölje be a egyszeri bejelentkezéses konfigurációs megerősítés, és kattintson a **Tovább gombra**.
    
    ![Azure Active Directory Single Sign-On][10]

13. Az **egyszeri bejelentkezés megerősítés** lapon kattintson a **Befejezés**gombra.  
 
    ![Azure Active Directory Single Sign-On][11]

14. Most már be az Azure Kezelőportálja **https://portal.azure.com** a rendszergazdai hitelesítő adataival jelentkezzen be

15. Kattintson a **További szolgáltatások** hivatkozásra a bal oldali navigációs ablakban
    
    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_09.png)

16. Azure Active Directory és az **Azure Active Directory** hivatkozásra kattintás keresése
    
    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_10.png)

17. A **Vállalati alkalmazások** gomb alatti összes szoftver alkalmazás találja.

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_11.png)

18. Most kattintson a Tovább gombra a lap **Összes alkalmazás** hivatkozására
    
    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_12.png)

19. Keresse meg a Lifesize-alkalmazást, amelynek a RelayState beállítása. 
    
    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_13.png)

20. Kattintson **az egyszeri bejelentkezés** hivatkozásra a lap

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_14.png)

21. Ekkor megjelenik a **Megjelenítés speciális beállítások URL-címe** jelölőnégyzetet. Jelölje be a jelölőnégyzetet.
    
    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_15.png)
    
22. Most állítsa be az alkalmazás, amely akkor látható, az Lifesize alkalmazás SSO beállítása lapon az a RelayState. 

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_16.png)

23. A beállítások mentéséhez.

### <a name="creating-an-azure-ad-test-user"></a>Azure AD-próba felhasználó létrehozása
Ebben a szakaszban a klasszikus portálon Britta Simon nevű próba felhasználó létrehozása.


![Azure Active Directory-felhasználó létrehozása][20]

**Teszt felhasználó létrehozása az Azure Active Directory, hajtsa végre az alábbi lépéseket:**

1. Az **Azure klasszikus portálon**kattintson a bal oldali navigációs területen kattintson az **Active Directory**.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-lifesize-cloud-tutorial/create_aaduser_09.png) 

2. A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3. A menüben, a képernyő tetején, a felhasználók listájának megjelenítéséhez kattintson a **felhasználók**elemre.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-lifesize-cloud-tutorial/create_aaduser_03.png) 

4. **Felhasználó hozzáadása**elemre az alsó eszköztáron a **Felhasználó hozzáadása** párbeszédpanel megnyitásához.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-lifesize-cloud-tutorial/create_aaduser_04.png) 

5. A **mondja el a felhasználóra vonatkozó** párbeszédpanel lapon hajtsa végre az alábbi lépéseket:  ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-lifesize-cloud-tutorial/create_aaduser_05.png) 

    egy. Felhasználó típusa jelölje be az új felhasználó a szervezet.

    b. A felhasználónév **szövegmezőbe**írja be a **BrittaSimon**.

    c billentyűkombinációt. Kattintson a **Tovább**gombra.

6.  A **Felhasználói profil** párbeszédpanel lapon hajtsa végre az alábbi lépéseket: ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-lifesize-cloud-tutorial/create_aaduser_06.png) 

    egy. Az **első neve** mezőbe írja be a **Britta**.  

    b. Az **Utolsó neve** mezőbe írja be, **Simon**.

    c billentyűkombinációt. A **Megjelenítendő név** mezőbe írja be a **Britta Simon**.

    d. A **szerepkör** listában jelölje ki a **felhasználót**.

    e. Kattintson a **Tovább**gombra.

7. A párbeszédpanel **ideiglenes jelszót első** lapon kattintson a **Hozzon létre**.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-lifesize-cloud-tutorial/create_aaduser_07.png) 

8. A párbeszédpanel **ideiglenes jelszót első** lapon hajtsa végre az alábbi lépéseket:

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-lifesize-cloud-tutorial/create_aaduser_08.png) 

    egy. Jegyezze fel az **Új jelszót**az érték.

    b. Kattintson a **kész**gombra.   



### <a name="creating-an-lifesize-cloud-test-user"></a>Egy Lifesize felhő teszt felhasználó létrehozása

Ebben a részben hozzon létre egy felhasználót Britta Simon nevű Lifesize felhőben. Lifesize felhő támogatja a kiépítési automatikus felhasználói. Azure Active Directory sikeres hitelesítése, után a felhasználó fog lehet automatikusan kiépítve alkalmazásban. 


### <a name="assigning-the-azure-ad-test-user"></a>Az Azure Active Directory-próba felhasználói hozzárendelése

Ebben a részben Britta Simon Azure egyszeri bejelentkezés a saját hozzáférés engedélyezése a felhőbe Lifesize használandó engedélyezése.

![Felhasználó hozzárendelése][200] 

**Britta Simon hozzárendelése Lifesize felhőben, hajtsa végre az alábbi lépéseket:**

1. A klasszikus portál kattintva nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson az **alkalmazások** parancsra a felső menüben.

    ![Felhasználó hozzárendelése][201] 

2. Az alkalmazások listájában jelölje ki a **Lifesize felhő**.

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_50.png) 

3. A felső sávon kattintson a **felhasználók**elemre.

    ![Felhasználó hozzárendelése][203]

4. A felhasználók listában jelölje ki a **Britta Simon**.

5. Az alsó eszköztáron kattintson a **hozzárendelni**.

    ![Felhasználó hozzárendelése][205]


### <a name="testing-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

Ebben a részben tesztelje az Azure Active Directory egyetlen bejelentkezés beállítási a Access panelen.

A felhő Lifesize csempére az Access panel gombra kattintva meg kell első automatikusan aláírt-on az Lifesize felhő alkalmazás.


## <a name="additional-resources"></a>További források

* [A szoftver alkalmazások integrálása az Azure Active Directory oktatóanyagok listája](active-directory-saas-tutorial-list.md)
* [Mi az access alkalmazás és az egyszeri bejelentkezés az Azure Active Directory címtárral?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_205.png
