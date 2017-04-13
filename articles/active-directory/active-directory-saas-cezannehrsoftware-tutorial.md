<properties
    pageTitle="Oktatóprogram: Azure Active Directory-integráció a Cezanne HR szoftver |} Microsoft Azure"
    description="Megtudhatja, hogy miként konfigurálása az egyszeri bejelentkezés Azure Active Directory és a Cezanne HR szoftver között."
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
    ms.date="10/26/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-cezanne-hr-software"></a>Oktatóprogram: Azure Active Directory-integráció Cezanne HR szoftverrel

Ebben az oktatóanyagban célja mutatja, hogy miként Cezanne HR szoftver integrálása az Azure Active Directory (Azure Active Directory).

Cezanne HR szoftver integrálása az Azure Active Directory biztosít a következő előnyökkel jár:

- Az Azure Active Directory, aki rendelkezik hozzáféréssel Cezanne HR szoftver megadhatja, hogy
- Az Azure Active Directory-fiókok engedélyezheti a felhasználóknak, hogy automatikusan kérjen jelentkezett-on Cezanne HR szoftver (Single Sign-On)
- A fiókokat az egy központi helyen – az Azure klasszikus portálra

Ha többet szeretne tudni a szoftver alkalmazás integrálása az Azure AD meg, hogy, ismerje meg, [az access alkalmazás és az egyszeri bejelentkezés az Azure Active Directory címtárral](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek

Azure Active Directory integráció Cezanne HR szoftverrel van szükség az alábbiakat:

- Az Azure Active Directory-előfizetéssel
- Egy Cezanne HR szoftver egyszeri bejelentkezés engedélyezett előfizetés


> [AZURE.NOTE] Az oktatóprogram lépéseit teszteléséhez nem használata ajánlott munkakörnyezetben.


Az oktatóprogram lépéseit teszteléséhez célszerű követnie az alábbi javaslatokat:

- Erre akkor szükség, ne használja a termelési környezetén.
- Ha nincs telepítve az Azure Active Directory próba környezetben, kattint egy hónap próba [Itt](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban célja lehetővé teszi az Azure Active Directory egyszeri bejelentkezés tesztkörnyezetben tesztelése.

Az alkalmazási példát, ebben az oktatóanyagban tagolt két fő építőelemek áll:

1. Cezanne HR szoftver hozzáadása a gyűjteményből
2. Beállítása és tesztelése Azure Active Directory egyszeri bejelentkezés


## <a name="adding-cezanne-hr-software-from-the-gallery"></a>Cezanne HR szoftver hozzáadása a gyűjteményből
Konfigurálja az Azure Active Directory integrálása Cezanne HR szoftver, meg kell Cezanne HR szoftver az felügyelt szoftver alkalmazáslistában a gyűjteményből hozzáadása.

**A gyűjteményből Cezanne HR szoftver felvételéhez hajtsa végre az alábbi lépéseket:**

1. Az **Azure klasszikus portálon**kattintson a bal oldali navigációs területen kattintson az **Active Directory**. 

    ![Az Active Directory][1]

2. A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3. Nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson a felső menüben **alkalmazások** .
    
    ![Alkalmazások][2]

4. Kattintson a **Hozzáadás** az oldal alján.
    
    ![Alkalmazások][3]

5. **Milyen feladatot szeretne tenni** párbeszédpanelen kattintson **a gyűjteményből az alkalmazások felvétele**.

    ![Alkalmazások][4]

6. A Keresés mezőbe írja be a **Cezanne HR szoftvert**.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_01.png)

7. Az eredmények panel **Cezanne HR szoftverek**kiválasztása, és válassza a **kész** , az alkalmazás hozzáadása parancsra.

    ![Az alkalmazás kijelöli a gyűjteményben](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_0001.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Beállítása és tesztelése Azure Active Directory egyszeri bejelentkezés
Ez a szakasz célja mutatja, hogy beállítása és Azure Active Directory egyszeri bejelentkezés a "Britta Simon" nevű próba felhasználón alapuló Cezanne HR szoftverrel tesztelése.

Az egyszeri bejelentkezési a munkát az Azure Active Directory kell tudható, hogy mi az Azure Active Directory-felhasználónak Cezanne HR szoftverben megfelelőjük a felhasználó. Más szóval Azure AD-felhasználó, és a kapcsolódó felhasználó Cezanne HR szoftverben hivatkozás viszonya kell létrehozni.

Ez a hivatkozás kapcsolat Azure AD a **felhasználónév** Cezanne HR szoftverben a program az az érték, a **felhasználónév** hozzárendelésével jön létre.

Állítsa be, és Azure Active Directory egyszeri bejelentkezés Cezanne HR szoftverrel tesztelése, a következő építőelemek szükséges:

1. **[Azure Active Directory konfigurálása az egyszeri bejelentkezés](#configuring-azure-ad-single-single-sign-on)** - engedélyezése a felhasználóknak, hogy ezzel a szolgáltatással.
2. **[Felhasználó létrehozása az Azure Active Directory tesztelése](#creating-an-azure-ad-test-user)** - Azure Active Directory egyszeri bejelentkezéssel való Britta Simon ellenőrzéséhez.
3. **[Felhasználó létrehozása egy Cezanne HR szoftver tesztelése](#creating-a-cezanne-hr-software-test-user)** - szeretné, hogy egy partner a Britta Simon Cezanne HR szoftverben, amelyen az Azure Active Directory ábrázolt csatolt.
4. **[Az Azure Active Directory hozzárendelése tesztelje a felhasználó](#assigning-the-azure-ad-test-user)** - ahhoz, hogy Britta Simon Azure AD az egyszeri bejelentkezés használata.
5. **[Tesztelés egyszeri bejelentkezés](#testing-single-sign-on)** - ellenőrzéséhez, hogy működik-e a konfigurációs.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure Active Directory egyszeri bejelentkezés beállítása

Ebben a részben Azure AD az egyszeri bejelentkezés a klasszikus portálon engedélyezése, és az egyszeri bejelentkezés Cezanne HR szoftveralkalmazásban beállítása.

**Azure Active Directory egyszeri bejelentkezés beállítása a Cezanne HR szoftverrel, hajtsa végre az alábbi lépéseket:**

1. Az alkalmazás integrációs **Cezanne HR szoftver** lapon a klasszikus portálon kattintson **konfigurálása az egyszeri bejelentkezés** a **Konfigurálása az egyszeri bejelentkezés** párbeszédpanel megnyitásához.
     
    ![Egyszeri bejelentkezés beállítása][6] 

2. **Hogyan szeretné, hogy jelentkezzen be az Cezanne HR szoftver felhasználók** lapon válassza az **Azure Active Directory Single Sign-On**, és kattintson a **Tovább gombra**.
    
    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_03.png)

3. Az **Alkalmazás beállításainak megadása** párbeszédpanel lapon hajtsa végre az alábbi lépéseket, és kattintson a **Tovább**gombra:

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_04.png)

    egy. Az **Bejelentkezési a URL-cím** mezőbe írja be az URL-cím használata a következő mintát: `https://w3.cezanneondemand.com/cezannehr/-/<tenant id>`.

    b. Az **Válasz URL-cím** mezőbe írja be az URL-cím használata a következő mintát: `https://w3.cezanneondemand.com:443/CezanneOnDemand/-/<tenant id>/Saml/samlp`.

    c billentyűkombinációt. Kattintson a **Tovább** gombra

    > [AZURE.NOTE] Felhívjuk a figyelmét arra, hogy van-e ezek az értékek frissítése a tényleges bejelentkezési a URL-CÍMÉT és a válasz URL-CÍMÉT. Ezek az értékek juthat, Cezanne HR szoftver támogatási csoport munkatársaitól kaphat keresztül <mailto:info@cezannehr.com>.

4. A **beállítás az egyszeri bejelentkezés Cezanne HR szoftver a** lapon hajtsa végre az alábbi lépéseket, és kattintson a **Tovább**gombra:

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_05.png)

    egy. Kattintson a **tanúsítvány letöltése**, és mentse a fájlt a számítógépen.

    b. Kattintson a **Tovább**gombra.

5. A különböző webes böngészőablakban bejelentkezés az Ön Cezanne HR szoftver bérlői rendszergazdaként.

6. Kattintson a bal oldali navigációs területen a **Rendszer a telepítő**. Nyissa meg a **biztonsági beállítások**. Nyissa meg **Egyszeri bejelentkezéses konfigurációs**.

    ![Egyszeri bejelentkezés a alkalmazás egymás konfigurálása](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_000.png)

7. **Jelentkezzen be az alábbi egyszeri bejelentkezés (SSO) szolgáltatással engedélyezése a felhasználóknak** panel a **SAML 2.0-s** jelölőnégyzet, és jelölje be a mellette a **Speciális konfiguráció** lehetőséget.

    ![Egyszeri bejelentkezés a alkalmazás egymás konfigurálása](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_001.png)

8. Kattintson az **Új hozzáadása** gombra.

    ![Egyszeri bejelentkezés a alkalmazás egymás konfigurálása](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_002.png)

9. Hajtsa végre az alábbi lépéseket a **SAML 2.0 IDENTITÁSSZOLGÁLTATÓ** szakaszban.

    ![Egyszeri bejelentkezés a alkalmazás egymás konfigurálása](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_003.png)

    egy. Adja meg a identitásszolgáltató nevét a **Megjelenítendő nevet**.

    b. A **Szervezet azonosítója** szövegdoboz az Azure Active Directory-alkalmazás konfigurálása varázsló helyezze a **Szervezet azonosítója** értékét.

    c billentyűkombinációt. Állítsa be a **SAML kötelező** "Bejegyzés".

    d. A **Biztonsági jogkivonat szolgáltatás végpontjának** szövegdoboz **Egyszeri bejelentkezéses szolgáltatás URL-címe** értékének helyezi az Azure Active Directory-alkalmazás konfigurálása varázsló.

    e. Írja be a **felhasználói azonosító attribútumnév**"http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name".

    f. Kattintson a letöltött tanúsítvány az Azure Active Directory feltöltése **feltöltése** ikonra.

    g. Kattintson az **Ok** gombra. 

10. Kattintson a **Mentés** gombra.

    ![Egyszeri bejelentkezés a alkalmazás egymás konfigurálása](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_004.png)

11. A klasszikus portálon jelölje be a egyszeri bejelentkezéses konfigurációs megerősítés, és kattintson a **Tovább gombra**.
    
    ![Azure Active Directory Single Sign-On][10]

12. Az **egyszeri bejelentkezés megerősítés** lapon kattintson a **Befejezés**gombra.  
    
    ![Azure Active Directory Single Sign-On][11]



### <a name="creating-an-azure-ad-test-user"></a>Azure AD-próba felhasználó létrehozása
Ez a szakasz célja próba felhasználó létrehozása az úgynevezett Britta Simon klasszikus portálon.

![Azure Active Directory-felhasználó létrehozása][20]

**Teszt felhasználó létrehozása az Azure Active Directory, hajtsa végre az alábbi lépéseket:**

1. Az **Azure klasszikus portálon**kattintson a bal oldali navigációs területen kattintson az **Active Directory**.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-cezannehrsoftware-tutorial/create_aaduser_09.png)

2. A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3. A menüben, a képernyő tetején, a felhasználók listájának megjelenítéséhez kattintson a **felhasználók**elemre.
    
    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-cezannehrsoftware-tutorial/create_aaduser_03.png)

4. **Felhasználó hozzáadása**elemre az alsó eszköztáron a **Felhasználó hozzáadása** párbeszédpanel megnyitásához.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-cezannehrsoftware-tutorial/create_aaduser_04.png)

5. A párbeszédpanel **közli velünk a felhasználóra vonatkozó** lapon hajtsa végre az alábbi lépéseket:

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-cezannehrsoftware-tutorial/create_aaduser_05.png)

    egy. Felhasználó típusa jelölje be az új felhasználó a szervezet.

    b. A felhasználónév **szövegmezőbe**írja be a **BrittaSimon**.

    c billentyűkombinációt. Kattintson a **Tovább**gombra.

6.  A **Felhasználói profil** párbeszédpanel lapon hajtsa végre az alábbi lépéseket:
    
    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-cezannehrsoftware-tutorial/create_aaduser_06.png)

    egy. Az **első neve** mezőbe írja be a **Britta**.  

    b. Az **Utolsó neve** mezőbe írja be a **Simon**.

    c billentyűkombinációt. A **Megjelenítendő név** mezőbe írja be a **Britta Simon**.

    d. A **szerepkör** listában jelölje ki a **felhasználót**.

    e. Kattintson a **Tovább**gombra.

7. A párbeszédpanel **ideiglenes jelszót kérjen** lapon kattintson a **Hozzon létre**.
    
    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-cezannehrsoftware-tutorial/create_aaduser_07.png)

8. A párbeszédpanel **ideiglenes jelszót első** lapon hajtsa végre az alábbi lépéseket:
    
    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-cezannehrsoftware-tutorial/create_aaduser_08.png)

    egy. Jegyezze fel az **Új jelszót**az érték.

    b. Kattintson a **kész**gombra.   



### <a name="creating-a-cezanne-hr-software-test-user"></a>Cezanne HR szoftver próba felhasználó létrehozása

Ahhoz, hogy az Azure Active Directory-felhasználóknak, hogy jelentkezzen be a Cezanne HR szoftver, hogy ki kell építenie Cezanne HR szoftver. Cezanne HR szoftver, ha a feladat manuális kiépítési.

####<a name="to-provision-a-user-account-perform-the-following-steps"></a>Építse egy felhasználói fiókot, hajtsa végre az alábbi lépéseket:

1.  Jelentkezzen be a Cezanne HR szoftver vállalati webhely rendszergazdaként.

2.  Kattintson a bal oldali navigációs területen a **Rendszer a telepítő**. Nyissa meg a **felhasználók kezelése**. Nyissa meg az **Új felhasználó hozzáadása**.

    ![Új felhasználó] (./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_005.png "Új felhasználó")

3.  A **Személy adatai** csoportban végezze el a alatti lépéseket:

    ![Új felhasználó] (./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_006.png "Új felhasználó")

    egy. **Belső felhasználói** beállítás ki.

    b. Az **első neve** mezőbe írja be a **Britta**.  

    c billentyűkombinációt. Az **Utolsó neve** mezőbe írja be a **Simon**.

    d. Az **E-mail** mezőbe írja be a Britta Simon fiók e-mail címét.

4.  A **Fiókadatok** csoportban végezze el a alábbi lépéseket:

    ![Új felhasználó] (./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_007.png "Új felhasználó")

    egy. A **felhasználónév** mezőbe írja be e-mail címét a Britta Simon.

    b. A **jelszó** mezőbe írja be az Britta Simon fiók jelszavát.

    c billentyűkombinációt. Jelölje ki a **HR Professional** **biztonsági**szerep.

    d. Kattintson az **OK gombra**.

5. Keresse meg **Az egyszeri bejelentkezés** fülre, és területen válassza az **Új hozzáadása** a **SAML 2.0-s azonosítóját** .

    ![Felhasználói] (./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_008.png "Felhasználói")

6. Válasszon a identitásszolgáltató, az **Identitásszolgáltató** és a szövegmezőbe a **Felhasználói**azonosító, és adja meg a Britta Simon fiók e-mail címét.

    ![Felhasználói] (./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_009.png "Felhasználói")
    
7. Kattintson a **Mentés** gombra.

    ![Felhasználói] (./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_010.png "Felhasználói")


### <a name="assigning-the-azure-ad-test-user"></a>Az Azure Active Directory-próba felhasználói hozzárendelése

Ez a szakasz célja Britta Simon Azure egyszeri bejelentkezés a saját hozzáférést biztosít Cezanne HR szoftver használandó engedélyezése.
    
![Felhasználó hozzárendelése][200]

**Britta Simon hozzárendelése Cezanne HR szoftver, hajtsa végre az alábbi lépéseket:**

1. A klasszikus portál kattintva nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson az **alkalmazások** parancsra a felső menüben.
    
    ![Felhasználó hozzárendelése][201]

2. Az alkalmazások listájában jelölje ki a **Cezanne HR szoftvert**.
    
    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_50.png)

3. A felső sávon kattintson a **felhasználók**elemre.
    
    ![Felhasználó hozzárendelése][203]

4. A felhasználók listában jelölje ki a **Britta Simon**.

5. Az alsó eszköztáron kattintson a **hozzárendelni**.
    
    ![Felhasználó hozzárendelése][205]

### <a name="testing-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

Ez a szakasz célja a Azure Active Directory egyszeri bejelentkezéses beállítások tesztelése a Access panelen.
 
Ha a Cezanne HR szoftver csempére az Access panel gombra kattint, meg kell kérjen automatikusan aláírt-a Cezanne HR szoftveralkalmazásban.

## <a name="additional-resources"></a>További források

* [A szoftver alkalmazások integrálása az Azure Active Directory oktatóanyagok listája](active-directory-saas-tutorial-list.md)
* [Mi az access alkalmazás és az egyszeri bejelentkezés az Azure Active Directory címtárral?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_205.png
