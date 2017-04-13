<properties
    pageTitle="Oktatóprogram: Azure Active Directory-integráció a Cisco külső |} Microsoft Azure"
    description="Megtudhatja, hogy miként konfigurálása az egyszeri bejelentkezés Azure Active Directory és a külső Cisco között."
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
    ms.date="10/20/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-cisco-spark"></a>Oktatóprogram: Azure Active Directory-integráció a Cisco külső

Ebből az oktatóanyagból megismerheti, hogyan Cisco külső integrálása az Azure Active Directory (Azure Active Directory).

A külső Cisco integrálása az Azure Active Directory biztosít a következő előnyökkel jár:

- Az Azure Active Directory, aki rendelkezik hozzáféréssel a Cisco külső megadhatja, hogy
- Az Azure Active Directory-fiókok engedélyezheti a felhasználóknak, hogy automatikusan első jelentkezett-on való Cisco külső (Single Sign-On)
- A fiókokat az egy központi helyen – az Azure klasszikus portálra

Ha többet szeretne tudni a szoftver alkalmazás integrálása az Azure AD meg, hogy, ismerje meg, [az access alkalmazás és az egyszeri bejelentkezés az Azure Active Directory címtárral](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek

Azure Active Directory integráció a Cisco külső van szükség az alábbiakat:

- Az Azure Active Directory-előfizetéssel
- Egy **Külső Cisco** egyszeri bejelentkezés engedélyezett előfizetés


> [AZURE.NOTE] Az oktatóprogram lépéseit teszteléséhez nem használata ajánlott munkakörnyezetben.


Az oktatóprogram lépéseit teszteléséhez célszerű követnie az alábbi javaslatokat:

- Erre akkor szükség, ne használja a termelési környezetén.
- Ha nincs telepítve az Azure Active Directory próba környezetben, kattint egy hónap próba [Itt](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban tesztelni Azure AD az egyszeri bejelentkezés tesztkörnyezetben. Az alkalmazási példát, ebben az oktatóanyagban tagolt két fő építőelemek áll:

1. A külső Cisco hozzáadása a gyűjteményből
2. Beállítása és tesztelése Azure Active Directory egyszeri bejelentkezés


## <a name="adding-cisco-spark-from-the-gallery"></a>A külső Cisco hozzáadása a gyűjteményből
Konfigurálja az Azure Active Directory integrálása a Cisco külső, szüksége Cisco külső hozzáadása felügyelt szoftver alkalmazásai a gyűjteményből.

**A gyűjteményből Cisco külső felvételéhez hajtsa végre az alábbi lépéseket:**

1. Az **Azure klasszikus portálon**kattintson a bal oldali navigációs területen kattintson az **Active Directory**. 

    ![Az Active Directory][1]

2. A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3. Nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson a felső menüben **alkalmazásokat** .

    ![Alkalmazások][2]

4. Kattintson a **Hozzáadás** az oldal alján.

    ![Alkalmazások][3]

5. **Milyen feladatot szeretne tenni** párbeszédpanelen kattintson **a gyűjteményből az alkalmazások felvétele**.

    ![Alkalmazások][4]

6. A Keresés mezőbe írja be **A Cisco külső**.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-cisco-spark-tutorial/tutorial_cisco_spark_01.png)

7. Az eredmény ablaktáblában jelölje ki a **Cisco külső**pontra, és kattintson a **kész** az alkalmazás hozzáadása parancsra.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-cisco-spark-tutorial/tutorial_cisco_spark_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Beállítása és tesztelése Azure Active Directory egyszeri bejelentkezés
Ebben a részben konfigurálása és a "Britta Simon" nevű próba felhasználón alapuló Cisco külső az Azure Active Directory egyszeri bejelentkezés tesztelése.

Egyszeri bejelentkezés a munkát Azure AD meg kell adni szeretné, hogy mi a külső Cisco megfelelőjük a felhasználó egy felhasználóhoz, az Azure Active Directory. Más szóval Azure AD-felhasználó, és a kapcsolódó Cisco külső felhasználó hivatkozás viszonya kell létrehozni.
A hivatkozás kapcsolat jön létre által az Azure Active Directory Cisco külső **felhasználónév** értékként a **felhasználó neve** értéket rendeli. Állítsa be, és Azure Active Directory egyszeri bejelentkezéssel való Cisco külső tesztelése, az alábbi építőelemeket szükséges:

1. **[Azure Active Directory konfigurálása az egyszeri bejelentkezés](#configuring-azure-ad-single-single-sign-on)** - engedélyezése a felhasználóknak, hogy ezzel a szolgáltatással.
2. **[Felhasználó létrehozása az Azure Active Directory tesztelése](#creating-an-azure-ad-test-user)** - Azure Active Directory egyszeri bejelentkezéssel való Britta Simon ellenőrzéséhez.
4. **[Felhasználó létrehozása egy Cisco külső tesztelése](#creating-a-cisco-spark-test-user)** - van-e egy megfelelője a Britta Simon Cisco külső, amelyen az Azure Active Directory ábrázolt csatolt.
5. **[Az Azure Active Directory hozzárendelése tesztelje a felhasználó](#assigning-the-azure-ad-test-user)** - ahhoz, hogy Britta Simon Azure AD az egyszeri bejelentkezés használata.
5. **[Tesztelés egyszeri bejelentkezés](#testing-single-sign-on)** - ellenőrzéséhez, hogy működik-e a konfigurációs.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure Active Directory egyszeri bejelentkezés beállítása

Ez a szakasz célja Azure AD az egyszeri bejelentkezés az Azure klasszikus portálon engedélyezése és a Cisco külső alkalmazásban az egyszeri bejelentkezés beállítása.

Cisco külső alkalmazás vár, a SAML Előfeltételek attribútumokkal tartalmaz. Állítsa be az alábbi attribútumok ehhez az alkalmazáshoz. A **"Atrributes"** lapon, az alkalmazás a következő attribútumok értékének kezelheti. Az alábbi képernyőképen látható, a.

![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-cisco-spark-tutorial/tutorial_cisco_spark_03.png) 

**Állítson be Azure AD az egyszeri bejelentkezés Cisco külső, hajtsa végre az alábbi lépéseket:**

1. Az Azure klasszikus portálon **Cisco külső** alkalmazás integrációs lapján a menüben, a képernyő tetején kattintson a **attribútumok**elemre.
     
    ![Egyszeri bejelentkezés beállítása][5]


2. A **SAML jogkivonat attribútumok** párbeszédpanelen hajtsa végre az alábbi lépéseket:

    egy. Kattintson a **felhasználó attribútum hozzáadása** a **Felhasználó Attribure hozzáadása** párbeszédpanel megnyitásához.

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-cisco-spark-tutorial/tutorial_cisco_spark_05.png)
    
    b. Az **Attribútum neve** mezőbe írja be a **uid**.
    
    c billentyűkombinációt. Az **Attribútumérték** listából válassza ki a **user.userprincipal**.
    
    d. Kattintson a **kész**gombra. Ezután **Módosítások alkalmazása** az oldal alján.

3. A felső sávon kattintson a **Quick Start**.

    ![Egyszeri bejelentkezés beállítása][6]

4. A klasszikus portálon **Cisco külső** alkalmazás integrációs lapon kattintson **konfigurálása az egyszeri bejelentkezés** a **Konfigurálása az egyszeri bejelentkezés** párbeszédpanel megnyitásához.

    ![Egyszeri bejelentkezés beállítása][7] 

5. **Hogyan szeretné, hogy jelentkezzen be az Cisco külső felhasználók** lapon válassza az **Azure Active Directory Single Sign-On**, és kattintson a **Tovább gombra**.
    
    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-cisco-spark-tutorial/tutorial_cisco_spark_06.png)

6. Az **Alkalmazás beállításainak megadása** párbeszédpanel lapon hajtsa végre az alábbi lépéseket:

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-cisco-spark-tutorial/tutorial_cisco_spark_07.png)


    egy. Az bejelentkezési a URL-cím mezőbe írja be az URL-cím használata a következő mintát: `https://web.ciscospark.com/#/signin`.

    b. Kattintson a **Tovább**gombra.


7. **Konfigurálása az egyszeri bejelentkezés a Cisco külső** lapján kattintson a **metaadat-alapú letöltése**, majd mentse a fájlt a számítógépen.

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-cisco-spark-tutorial/tutorial_cisco_spark_09.png)

8. Jelentkezzen be a teljes rendszergazdai hitelesítő adatok [Cisco felhő együttműködési kezelése](https://admin.ciscospark.com/) .
9. Válassza a **Beállítások** , és a **hitelesítési** csoportban kattintson a **Módosítás**gombra.

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-cisco-spark-tutorial/tutorial_cisco_spark_10.png)

10. Jelölje ki a **Integrate 3rd fél identitásszolgáltató. (Speciális)** és nyissa meg a következő képernyőn.
11. Kattintson a **Metaadat-fájl letöltése** , és mentse a fájlt a számítógépen.
12. **Idp metaadatok importálása** lapon húzza és a Azure AD-metaadatok fájlt alakzatot a rajzlapra, vagy a fájl böngészőben beállítás használatával keresse meg és az Azure Active Directory-metaadatok fájl feltöltése. Ezután jelölje be a **csak a metaadat-alapú (biztonságosabb) hitelesítésszolgáltató alá tanúsítvány** , és kattintson a **Tovább**gombra. 

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-cisco-spark-tutorial/tutorial_cisco_spark_11.png)

13. Jelölje ki az **Egyszeri bejelentkezés a kapcsolat tesztelése**, és amikor megnyit egy új böngészőlapon, hitelesítést végezni az Azure Active Directory jelentkezzen be.
14. Vissza a **Cisco felhő együttműködési** böngésző lapra. Ha a tesztelésének sikerességéről tájékoztat, jelölje be a **e tesztelésének sikerességéről tájékoztat. Egyszeri bejelentkezés beállítás engedélyezése** , és kattintson a **Tovább**gombra.

7. Az Azure Active Directory klasszikus portálon jelölje be a egyszeri bejelentkezéses konfigurációs megerősítés, és kattintson a **Tovább gombra**.
    
    ![Azure Active Directory Single Sign-On][10]

8. Az **egyszeri bejelentkezés megerősítés** lapon kattintson a **Befejezés**gombra.  
    
    ![Azure Active Directory Single Sign-On][11]

### <a name="creating-an-azure-ad-test-user"></a>Azure AD-próba felhasználó létrehozása
Ebben a szakaszban a klasszikus portálon Britta Simon nevű próba felhasználó létrehozása.

![Azure Active Directory-felhasználó létrehozása][20]

**Teszt felhasználó létrehozása az Azure Active Directory, hajtsa végre az alábbi lépéseket:**

1. Az **Azure klasszikus portálon**kattintson a bal oldali navigációs területen kattintson az **Active Directory**.
    
    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-cisco-spark-tutorial/create_aaduser_09.png) 

2. A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3. A menüben, a képernyő tetején, a felhasználók listájának megjelenítéséhez kattintson a **felhasználók**elemre.
    
    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-cisco-spark-tutorial/create_aaduser_03.png) 

4. **Felhasználó hozzáadása**elemre az alsó eszköztáron a **Felhasználó hozzáadása** párbeszédpanel megnyitásához.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-cisco-spark-tutorial/create_aaduser_04.png) 

5. A párbeszédpanel **közli velünk a felhasználóra vonatkozó** lapon hajtsa végre az alábbi lépéseket:
 
    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-cisco-spark-tutorial/create_aaduser_05.png) 

    egy. Felhasználó típusa jelölje be az új felhasználó a szervezet.

    b. A felhasználónév **szövegmezőbe**írja be a **BrittaSimon**.

    c billentyűkombinációt. Kattintson a **Tovább**gombra.

6.  A **Felhasználói profil** párbeszédpanel lapon hajtsa végre az alábbi lépéseket:

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-cisco-spark-tutorial/create_aaduser_06.png) 

    egy. Az **első neve** mezőbe írja be a **Britta**.  

    b. Az **Utolsó neve** mezőbe írja be, **Simon**.

    c billentyűkombinációt. A **Megjelenítendő név** mezőbe írja be a **Britta Simon**.

    d. A **szerepkör** listában jelölje ki a **felhasználót**.

    e. Kattintson a **Tovább**gombra.

7. A párbeszédpanel **ideiglenes jelszót kérjen** lapon kattintson a **Hozzon létre**.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-cisco-spark-tutorial/create_aaduser_07.png) 

8. A párbeszédpanel **ideiglenes jelszót első** lapon hajtsa végre az alábbi lépéseket:

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-cisco-spark-tutorial/create_aaduser_08.png) 

    egy. Jegyezze fel az **Új jelszót**az érték.

    b. Kattintson a **kész**gombra.   



### <a name="creating-a-cisco-spark-test-user"></a>Teszt Cisco külső felhasználó létrehozása

Ebben a szakaszban a Britta Simon nevű Cisco külső felhasználó létrehozása. Ebben a szakaszban a Britta Simon nevű Cisco külső felhasználó létrehozása.

1. Nyissa meg a [Cisco felhő együttműködési kezelése](https://admin.ciscospark.com/) a teljes rendszergazdai hitelesítő adataival.
2. Kattintson a **felhasználók** majd a **felhasználók kezelése**.

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-cisco-spark-tutorial/tutorial_cisco_spark_12.png) 

3. A **Felhasználók kezelése** ablakban jelölje be a **kézi hozzáadása vagy módosítása a felhasználók** , és kattintson a **Tovább**gombra.
4. Válassza a **neveket és E-mail cím**. Ezután adja meg a szövegdoboz, hajtsa végre:

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-cisco-spark-tutorial/tutorial_cisco_spark_13.png) 

    egy. Az **első neve** mezőbe írja be a **Britta**

    b. Az **Utolsó neve** mezőbe írja be a **Simon**

    c billentyűkombinációt. Az **E-mail cím** mezőbe írja be a**britta.simon@contoso.com**

5. Kattintson a pluszjelre kattintva adja hozzá a Britta Simon. Kattintson a **Tovább gombra**.
6. A **Felhasználók-szolgáltatás hozzáadása** ablakban kattintson a **Mentés** , majd **befejezési**.

### <a name="assigning-the-azure-ad-test-user"></a>Az Azure Active Directory-próba felhasználói hozzárendelése

Ebben a részben Britta Simon Azure egyszeri bejelentkezés a saját hozzáférést biztosít Cisco külső használandó engedélyezése.

![Felhasználó hozzárendelése][200] 

**Britta Simon hozzárendelése Cisco külső, hajtsa végre az alábbi lépéseket:**

1. A klasszikus portál kattintva nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson az **alkalmazások** parancsra a felső menüben.

    ![Felhasználó hozzárendelése][201] 

2. Az alkalmazások listájában jelölje ki a **Cisco külső**.

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-cisco-spark-tutorial/tutorial_cisco_spark_14.png) 

1. A felső sávon kattintson a **felhasználók**elemre.

    ![Felhasználó hozzárendelése][203] 

1. Az összes felhasználó listában válassza a **Britta Simon**.

2. Az alsó eszköztáron kattintson a **hozzárendelni**.

    ![Felhasználó hozzárendelése][205]


### <a name="testing-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

Ez a szakasz célja a Azure Active Directory egyszeri bejelentkezéses beállítások tesztelése a Access panelen.

Ha a Cisco külső csempére az Access panel gombra kattint, meg kell kérjen automatikusan aláírt-on az Cisco külső alkalmazás.

## <a name="additional-resources"></a>További források

* [A szoftver alkalmazások integrálása az Azure Active Directory oktatóanyagok listája](active-directory-saas-tutorial-list.md)
* [Mi az access alkalmazás és az egyszeri bejelentkezés az Azure Active Directory címtárral?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_04.png


[5]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_05.png
[6]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_06.png
[7]:  ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_050.png
[10]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_060.png
[11]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_070.png
[20]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_205.png
