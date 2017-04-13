<properties
    pageTitle="Oktatóprogram: Azure Active Directory-integráció a HackerOne |} Microsoft Azure"
    description="Megtudhatja, hogy miként konfigurálása az egyszeri bejelentkezés Azure Active Directory és HackerOne között."
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


# <a name="tutorial-azure-active-directory-integration-with-hackerone"></a>Oktatóprogram: Azure Active Directory-integráció a HackerOne

Ebben az oktatóanyagban, integráció HackerOne az Azure Active Directory (Azure Active Directory).

HackerOne integrálása az Azure Active Directory biztosít a következő előnyökkel jár:

- Az Azure Active Directory, aki rendelkezik hozzáféréssel HackerOne megadhatja, hogy
- Engedélyezheti a felhasználóknak, hogy automatikusan első jelentkezett-on való HackerOne (Single Sign-On) az Azure Active Directory-fiókok
- A fiókokat az egy központi helyen – az Azure klasszikus portálra

Ha többet szeretne tudni a szoftver alkalmazás integrálása az Azure AD meg, hogy, ismerje meg, [az access alkalmazás és az egyszeri bejelentkezés az Azure Active Directory címtárral](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek

Azure Active Directory integráció a HackerOne van szükség az alábbiakat:

- Az Azure előfizetéssel
- Egy HackerOne egyszeri bejelentkezés engedélyezett előfizetés


> [AZURE.NOTE] Az oktatóprogram lépéseit teszteléséhez nem használata ajánlott munkakörnyezetben.


Az oktatóprogram lépéseit teszteléséhez célszerű követnie az alábbi javaslatokat:

- Erre akkor szükség, ne használja a termelési környezetén.
- Ha nincs telepítve az Azure Active Directory próba környezetben, kattint egy hónap próba [Itt](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban, beállítása és tesztelése Azure Active Directory egyszeri bejelentkezés a tesztkörnyezetben.  
Az alkalmazási példát, ebben az oktatóanyagban tagolt két fő építőelemek áll:

1. HackerOne hozzáadása a gyűjteményből
2. Beállítása és tesztelése Azure Active Directory egyszeri bejelentkezés


## <a name="adding-hackerone-from-the-gallery"></a>HackerOne hozzáadása a gyűjteményből
HackerOne integrálása az Azure Active Directory, kell HackerOne hozzáadása az felügyelt szoftver alkalmazáslistában a gyűjteményből.

**A gyűjteményből HackerOne felvételéhez hajtsa végre az alábbi lépéseket:**

1. Az **Azure klasszikus portálon**kattintson a bal oldali navigációs területen kattintson az **Active Directory**. 

    ![Az Active Directory][1]

2. A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3. Nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson a felső menüben **alkalmazásokat** .

    ![Alkalmazások][2]

4. Kattintson a **Hozzáadás** az oldal alján.

    ![Alkalmazások][3]

5. **Milyen feladatot szeretne tenni** párbeszédpanelen kattintson **a gyűjteményből az alkalmazások felvétele**.

![Alkalmazások][4]

6. A Keresés mezőbe írja be a **HackerOne**.

![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-hackerone-tutorial/tutorial_hackerone_01.png)

7. Az eredmény ablaktáblában jelölje ki a **HackerOne**, és válassza a **kész** , az alkalmazás hozzáadása gombra.


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Beállítása és tesztelése Azure Active Directory egyszeri bejelentkezés
Ezután konfigurálása és a "Britta Simon" nevű próba felhasználón alapuló HackerOne Azure AD az egyszeri bejelentkezés tesztelése.

Az egyszeri bejelentkezési a munkát az Azure Active Directory kell, hogy mi az a partner felhasználó HackerOne az Azure Active Directory-felhasználónak. Más szóval Azure AD-felhasználó, és a kapcsolódó felhasználó HackerOne hivatkozás viszonya kell létrehozni.  
A hivatkozás kapcsolat jön létre által az Azure Active Directory HackerOne **felhasználónév** értékként a **felhasználó neve** értéket rendeli.

Állítsa be, és Azure Active Directory egyszeri bejelentkezéssel való HackerOne tesztelése, a következő építőelemek szükséges:

1. **[Azure Active Directory konfigurálása az egyszeri bejelentkezés](#configuring-azure-ad-single-single-sign-on)** - engedélyezése a felhasználóknak, hogy ezzel a szolgáltatással.
2. **[Felhasználó létrehozása az Azure Active Directory tesztelése](#creating-an-azure-ad-test-user)** - Azure Active Directory egyszeri bejelentkezéssel való Britta Simon ellenőrzéséhez.
3. **[Felhasználó létrehozása egy HackerOne tesztelése](#creating-a-hackerone-test-user)** - van-e egy megfelelője a Britta Simon tanúsítása, amelyen az Azure Active Directory ábrázolt csatolt.
4. **[Az Azure Active Directory hozzárendelése tesztelje a felhasználó](#assigning-the-azure-ad-test-user)** - ahhoz, hogy Britta Simon Azure AD az egyszeri bejelentkezés használata.
5. **[Tesztelés egyszeri bejelentkezés](#testing-single-sign-on)** - ellenőrzéséhez, hogy működik-e a konfigurációs.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure Active Directory Single Sign-On konfigurálása

Ezután engedélyezése Azure AD az egyszeri bejelentkezés a klasszikus portál és az HackerOne alkalmazásban az egyszeri bejelentkezés beállítása.

Ez az eljárás részeként biztosan szükséges számrendszerben-64 kódolt tanúsítvány fájlt.  
Ha nem ismeri a ezt az eljárást, olvassa el a [szövegfájlba bináris tanúsítvány konvertálása](http://youtu.be/PlgrzUZ-Y1o)című témakört.

**Állítson be Azure AD az egyszeri bejelentkezés HackerOne, hajtsa végre az alábbi lépéseket:**

1. Az Azure klasszikus portálon **HackerOne** alkalmazás integrációs lapon kattintson **konfigurálása az egyszeri bejelentkezés** a **Konfigurálása az egyszeri bejelentkezés** párbeszédpanel megnyitásához.

    ![Egyszeri bejelentkezés beállítása][6] 

2. **Hogyan szeretné, hogy jelentkezzen be az HackerOne felhasználók** lapon válassza az **Azure Active Directory Single Sign-On**, és kattintson a **Tovább gombra**.

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-hackerone-tutorial/tutorial_hackerone_03.png) 

3. Az **Alkalmazás beállításainak megadása** párbeszédpanel lapon hajtsa végre az alábbi lépéseket, és kattintson a **Tovább gombra**:

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-hackerone-tutorial/tutorial_hackerone_04.png) 


    egy. **Bejelentkezési a URL-cím** mezőbe írja be a bejelentkezés az HackerOne alkalmazás a következő mintát használatával a felhasználók által használt URL-címe: **"https://hackerone.com/\<vállalatnév\>/authentication"**. 

    b. A HackerOne támogatási csoport munkatársaitól kaphat keresztül [support@hackerone.com](mailto:support@hackerone.com) megszerezni a bérlői webhely URL-CÍMÉT, ha nem tudja, hogy.

    c billentyűkombinációt. Az **azonosító** mezőbe írja be a bérlői webhely URL-CÍMÉT. 

    d. Kattintson a **Tovább**gombra.


4. A **beállítás az egyszeri bejelentkezés HackerOne a** lapon hajtsa végre az alábbi lépéseket, és kattintson a **Tovább gombra**:

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-hackerone-tutorial/tutorial_hackerone_05.png) 

    egy. Kattintson a **tanúsítvány letöltése**, és mentse a fájlt a számítógépen.

    b. Kattintson a **Tovább**gombra.


1. Bejelentkezés az Ön HackerOne bérlői rendszergazdaként.

1. A felső sávon kattintson a **Beállítások**.

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-hackerone-tutorial/tutorial_hackerone_001.png) 

1. Nyissa meg azt a "**hitelesítés**", és kattintson a "**hozzáadása SAML beállítások**".

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-hackerone-tutorial/tutorial_hackerone_003.png) 


1. A **SAML beállításai** párbeszédpanelen hajtsa végre az alábbi lépéseket:

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-hackerone-tutorial/tutorial_hackerone_004.png) 

    egy. Az **E-mailek tartomány** mezőbe írja be a regisztrált tartományt.

    b. A klasszikus Azure portálon **Egyszeri bejelentkezéses szolgáltatás URL-cím**másolása, és illessze be a egyszeri bejelentkezési a URL-címe mezőben lévő értéket.

    c billentyűkombinációt. A letöltött tanúsítvány **Alap-64-címként kódolt** fájl létrehozása  

       >[AZURE.TIP] További információra kíváncsi olvassa el a [bináris tanúsítvány szövegfájlba konvertálása](http://youtu.be/PlgrzUZ-Y1o) című témakört.
    
    d. Nyissa meg az alap-64 kódolt tanúsítvány a Jegyzettömbben, a tartalmát a vágólapra másolja és illessze be a a **X509 tanúsítvány** szövegdoboz.

    e. Kattintson a **Mentés** gombra


1. Hitelesítési beállítások párbeszédpanelen hajtsa végre az alábbi lépéseket:

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-hackerone-tutorial/tutorial_hackerone_005.png) 

    egy. Kattintson a **Futtatás vizsgálat**gombra.

    b. Ha az érték az **állapot** mező egyenlő **utolsó teszt állapota: létrehozott**, a HackerOne támogatási csoport munkatársaitól keresztül [support@hackerone.com](mailto:support@hackerone.com) kérhető a konfigurációs ismerkedést.


6. Az Azure klasszikus portálon válassza ki az egyszeri bejelentkezéses konfigurációs megerősítő, és kattintson a **Tovább gombra**.

    ![Azure Active Directory Single Sign-On][10]

7. Az **egyszeri bejelentkezés megerősítés** lapon kattintson a **Befejezés**gombra.  
 
    ![Azure Active Directory Single Sign-On][11]




### <a name="creating-an-azure-ad-test-user"></a>Azure AD-próba felhasználó létrehozása

Ezután új felhasználót hoz létre próba az úgynevezett Britta Simon klasszikus portálon.  

![Azure Active Directory-felhasználó létrehozása][20]

**BIZTONSÁGOS ELŐADÁSA próba felhasználó létrehozása az Azure Active Directory, hajtsa végre az alábbi lépéseket:**

1. Az **Azure klasszikus portálon**kattintson a bal oldali navigációs területen kattintson az **Active Directory**.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-hackerone-tutorial/create_aaduser_09.png) 

2. A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3. A menüben, a képernyő tetején, a felhasználók listájának megjelenítéséhez kattintson a **felhasználók**elemre.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-hackerone-tutorial/create_aaduser_03.png) 

4. **Felhasználó hozzáadása**elemre az alsó eszköztáron a **Felhasználó hozzáadása** párbeszédpanel megnyitásához.
    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-hackerone-tutorial/create_aaduser_04.png) 

5. A párbeszédpanel **közli velünk a felhasználóra vonatkozó** lapon hajtsa végre az alábbi lépéseket:

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-hackerone-tutorial/create_aaduser_05.png) 

    egy. Felhasználó típusa jelölje be az új felhasználó a szervezet.

    b. A felhasználónév **szövegmezőbe**írja be a **BrittaSimon**.

    c billentyűkombinációt. Kattintson a **Tovább**gombra.

6.  A **Felhasználói profil** párbeszédpanel lapon hajtsa végre az alábbi lépéseket:

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-hackerone-tutorial/create_aaduser_06.png) 

    egy. Az **első neve** mezőbe írja be a **Britta**.  

    b. Az **Utolsó neve** mezőbe írja be, **Simon**.

    c billentyűkombinációt. A **Megjelenítendő név** mezőbe írja be a **Britta Simon**.

    d. A **szerepkör** listában jelölje ki a **felhasználót**.

    e. Kattintson a **Tovább**gombra.

7. A párbeszédpanel **ideiglenes jelszót kérjen** lapon kattintson a **Hozzon létre**.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-hackerone-tutorial/create_aaduser_07.png) 

8. A párbeszédpanel **ideiglenes jelszót első** lapon hajtsa végre az alábbi lépéseket:

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-hackerone-tutorial/create_aaduser_08.png) 

    egy. Jegyezze fel az **Új jelszót**az érték.

    b. Kattintson a **kész**gombra.   


### <a name="creating-a-hackerone-test-user"></a>HackerOne próba felhasználó létrehozása

Ezután hozzon létre egy felhasználó Britta Simon nevű HackerOne. HackerOne támogatja a kiépítési közvetlenül az idő, amely alapértelmezés szerint engedélyezve van.

Nincs teendő ebben a szakaszban a meg nem. HackerOne nyitja meg, amikor egy új felhasználót hoznak létre, ha még nem létezik. Az [Azure Active Directory Single Sign-On konfigurálása](#configuring-azure-ad-single-single-sign-on).

> [AZURE.NOTE] Ha egy felhasználó létrehozása manuálisan kell szüksége a tanúsítása támogatási csoport munkatársaitól kaphat.




### <a name="assigning-the-azure-ad-test-user"></a>Az Azure Active Directory-próba felhasználói hozzárendelése

Ezután Britta Simon Azure egyszeri bejelentkezés a saját hozzáférést biztosít HackerOne használandó engedélyezi.

![Felhasználó hozzárendelése][200] 

**Britta Simon hozzárendelése HackerOne, hajtsa végre az alábbi lépéseket:**

1. Az Azure klasszikus portál kattintva nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson az **alkalmazások** parancsra a felső menüben.

    ![Felhasználó hozzárendelése][201] 

2. Az alkalmazások listájában jelölje ki a **HackerOne**.

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-hackerone-tutorial/tutorial_hackerone_50.png) 

1. A felső sávon kattintson a **felhasználók**elemre.

    ![Felhasználó hozzárendelése][203] 

1. A felhasználók listában jelölje ki a **Britta Simon**.

2. Az alsó eszköztáron kattintson a **hozzárendelni**.

    ![Felhasználó hozzárendelése][205]



### <a name="testing-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

Végül, a Azure AD egyszeri bejelentkezés beállítások tesztelése a Access panelen.  
Amikor az Access panel a HackerOne mozaik gombra kattint, meg kell első automatikusan aláírt-on az HackerOne alkalmazás.


## <a name="additional-resources"></a>További források

* [A szoftver alkalmazások integrálása az Azure Active Directory oktatóanyagok listája](active-directory-saas-tutorial-list.md)
* [Mi az access alkalmazás és az egyszeri bejelentkezés az Azure Active Directory címtárral?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-hackerone-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-hackerone-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-hackerone-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-hackerone-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-hackerone-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-hackerone-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-hackerone-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-hackerone-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-hackerone-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-hackerone-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-hackerone-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-hackerone-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-hackerone-tutorial/tutorial_general_205.png