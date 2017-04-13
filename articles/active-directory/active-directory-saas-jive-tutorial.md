<properties
    pageTitle="Oktatóprogram: Azure Active Directory-integráció a Jive |} Microsoft Azure"
    description="Megtudhatja, hogy miként konfigurálása az egyszeri bejelentkezés Azure Active Directory és Jive között."
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


# <a name="tutorial-azure-active-directory-integration-with-jive"></a>Oktatóprogram: Azure Active Directory-integráció a Jive

Ebből az oktatóanyagból megismerheti, hogyan Jive integrálása az Azure Active Directory (Azure Active Directory).

Jive integrálása az Azure Active Directory biztosít a következő előnyökkel jár:

- Az Azure Active Directory, aki rendelkezik hozzáféréssel Jive megadhatja, hogy
- Engedélyezheti a felhasználóknak, hogy automatikusan első jelentkezett-on való Jive (Single Sign-On) az Azure Active Directory-fiókok
- A fiókokat az egy központi helyen – az Azure klasszikus portálra

Ha többet szeretne tudni a szoftver alkalmazás integrálása az Azure AD meg, hogy, ismerje meg, [az access alkalmazás és az egyszeri bejelentkezés az Azure Active Directory címtárral](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek

Azure Active Directory integráció a Jive van szükség az alábbiakat:

- Az Azure Active Directory-előfizetéssel
- Egy Jive egyszeri bejelentkezés engedélyezett előfizetés


> [AZURE.NOTE] Az oktatóprogram lépéseit teszteléséhez nem használata ajánlott munkakörnyezetben.


Az oktatóprogram lépéseit teszteléséhez célszerű követnie az alábbi javaslatokat:

- Erre akkor szükség, ne használja a termelési környezetén.
- Ha nincs telepítve az Azure Active Directory próba környezetben, kattint egy hónap próba [Itt](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban tesztelni Azure AD az egyszeri bejelentkezés tesztkörnyezetben.

Az alkalmazási példát, ebben az oktatóanyagban tagolt két fő építőelemek áll:

1. Jive hozzáadása a gyűjteményből
2. Beállítása és tesztelése Azure Active Directory egyszeri bejelentkezés


## <a name="adding-jive-from-the-gallery"></a>Jive hozzáadása a gyűjteményből
Adja meg a integrálása az Azure Active Directory Jive, szüksége Jive hozzáadása az felügyelt szoftver alkalmazáslistában a gyűjteményből.

**A gyűjteményből Jive felvételéhez hajtsa végre az alábbi lépéseket:**

1. Az **Azure klasszikus portálon**kattintson a bal oldali navigációs területen kattintson az **Active Directory**.

    ![Az Active Directory][1]
2. A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3. Nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson a felső menüben **alkalmazások** .

    ![Alkalmazások][2]

4. Kattintson a **Hozzáadás** az oldal alján.

    ![Alkalmazások][3]

5. **Milyen feladatot szeretne tenni** párbeszédpanelen kattintson **a gyűjteményből az alkalmazások felvétele**.

    ![Alkalmazások][4]

6. A Keresés mezőbe írja be a **Jive**.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-jive-tutorial/tutorial_jive_01.png)
7. Az eredmény ablaktáblában jelölje ki a **Jive**, és válassza a **kész** , az alkalmazás hozzáadása gombra.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-jive-tutorial/tutorial_jive_02.png)


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Beállítása és tesztelése Azure Active Directory egyszeri bejelentkezés
Ebben a részben konfigurálása és a "Britta Simon" nevű próba felhasználón alapuló Jive az Azure Active Directory egyszeri bejelentkezés tesztelése.

Az egyszeri bejelentkezési a munkát az Azure Active Directory kell tudható, hogy mi a Jive megfelelőjük a felhasználó egy felhasználóhoz, az Azure Active Directory. Más szóval Azure AD-felhasználó, és a kapcsolódó felhasználó Jive hivatkozás viszonya kell létrehozni.

A hivatkozás kapcsolat jön létre által az Azure Active Directory Jive **felhasználónév** értékként a **felhasználó neve** értéket rendeli.

Állítsa be, és Azure Active Directory egyszeri bejelentkezéssel való Jive tesztelése, a következő építőelemek szükséges:

1. **[Azure Active Directory konfigurálása az egyszeri bejelentkezés](#configuring-azure-ad-single-sign-on)** - engedélyezése a felhasználóknak, hogy ezzel a szolgáltatással.
2. **[Felhasználó létrehozása az Azure Active Directory tesztelése](#creating-an-azure-ad-test-user)** - Azure Active Directory egyszeri bejelentkezéssel való Britta Simon ellenőrzéséhez.
3. **[Felhasználó létrehozása egy Jive tesztelése](#creating-a-jive-test-user)** - van-e egy megfelelője a Britta Simon Jive, amelyen az Azure Active Directory ábrázolt csatolt.
4. Számlák **[konfigurálása a felhasználói kiépítési](#configuring-user-provisioning)** – hogyan engedélyezhető a felhasználó Active Directory-felhasználó kiépítési tagolása Jive.
5. **[Az Azure Active Directory hozzárendelése tesztelje a felhasználó](#assigning-the-azure-ad-test-user)** - ahhoz, hogy Britta Simon Azure AD az egyszeri bejelentkezés használata.
6. **[Tesztelés egyszeri bejelentkezés](#testing-single-sign-on)** - ellenőrzéséhez, hogy működik-e a konfigurációs.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure Active Directory Single Sign-On konfigurálása

Ez a szakasz célja Azure AD az egyszeri bejelentkezés az Azure klasszikus portálon engedélyezése és a Jive alkalmazásban az egyszeri bejelentkezés beállítása.

**Állítson be Azure AD az egyszeri bejelentkezés Jive, hajtsa végre az alábbi lépéseket:**

1. A klasszikus portálon **Jive** alkalmazás integrációs lapon kattintson **konfigurálása az egyszeri bejelentkezés** a **Konfigurálása az egyszeri bejelentkezés** párbeszédpanel megnyitásához.
     
    ![Egyszeri bejelentkezés beállítása][6] 

2. **Hogyan megtekinti Jive jelentkezzen be felhasználók** lapon válassza az **Azure Active Directory Single Sign-On**, és kattintson a **Tovább gombra**.

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-jive-tutorial/tutorial_jive_03.png) 

3. Az **Alkalmazás beállításainak megadása** párbeszédpanel lapon hajtsa végre az alábbi lépéseket:

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-jive-tutorial/tutorial_jive_04.png) 

    egy. **Bejelentkezési a URL-cím** mezőbe írja be a bejelentkezés az Jive alkalmazás a következő mintát használatával a felhasználók által használt URL-címe: **https://\<Vevőnév\>. jivecustom.com**.
    
    b. Kattintson a **Tovább** gombra
 
4. A **beállítás az egyszeri bejelentkezés Jive a** lapon hajtsa végre az alábbi lépéseket:

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-jive-tutorial/tutorial_jive_05.png)

    egy. Kattintson a **tanúsítvány letöltése**, és mentse a fájlt a számítógépen.

    b. Kattintson a **Tovább**gombra.


5. Bejelentkezés az Jive bérlőhöz a számítógépre rendszergazdaként.

6. A felső sávon kattintson a "**Saml**".

    ![Alkalmazás oldalán az egyszeri bejelentkezés beállítása](./media/active-directory-saas-jive-tutorial/tutorial_jive_002.png)

    egy. Az **Általános** lapon jelölje be a **engedélyezve** .

    b. Kattintson a "**minden saml beállítások mentése**" gomb.

7. Nyissa meg a "**Idp metaadatok**" fülre.

    ![Alkalmazás oldalán az egyszeri bejelentkezés beállítása](./media/active-directory-saas-jive-tutorial/tutorial_jive_003.png)

    egy. A letöltött metaadatok XML-fájl tartalmának másolja, és illessze be a **metaadat-alapú identitás-szolgáltató (IDP)** mezőben lévő értéket.

    b. Kattintson a "**minden saml beállítások mentése**" gomb. 

8. Nyissa meg a "**Felhasználó attribútum-hozzárendelés**" fülre.

    ![Alkalmazás oldalán az egyszeri bejelentkezés beállítása](./media/active-directory-saas-jive-tutorial/tutorial_jive_004.png)

    egy. Az **E-mail** mezőbe másolja és illessze be a **Posta** érték attribútum neve.

    b. A **Vezetéknév** mezőben lévő értéket másolja és illessze be a **givenname** value attribútum nevét.

    c billentyűkombinációt. A **Vezetéknév** mezőben lévő értéket másolja és illessze be a **Vezetéknév** value attribútum nevét.
    
9. Az Azure Active Directory-portálon válassza ki az egyszeri bejelentkezéses konfigurációs megerősítő, és kattintson a **Tovább gombra**.
![Azure Active Directory Single Sign-On][10]

10. Az **egyszeri bejelentkezés megerősítés** lapon kattintson a **Befejezés**gombra.  
  ![Azure Active Directory Single Sign-On][11]


### <a name="creating-an-azure-ad-test-user"></a>Azure AD-próba felhasználó létrehozása
Ebben a szakaszban a klasszikus portálon Britta Simon nevű próba felhasználó létrehozása.


![Azure Active Directory-felhasználó létrehozása][20]

**Teszt felhasználó létrehozása az Azure Active Directory, hajtsa végre az alábbi lépéseket:**

1. Az **Azure klasszikus portálon**kattintson a bal oldali navigációs területen kattintson az **Active Directory**.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-jive-tutorial/create_aaduser_09.png) 

2. A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3. A menüben, a képernyő tetején, a felhasználók listájának megjelenítéséhez kattintson a **felhasználók**elemre.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-jive-tutorial/create_aaduser_03.png) 

4. **Felhasználó hozzáadása**elemre az alsó eszköztáron a **Felhasználó hozzáadása** párbeszédpanel megnyitásához.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-jive-tutorial/create_aaduser_04.png) 

5. A **mondja el a felhasználóra vonatkozó** párbeszédpanel lapon hajtsa végre az alábbi lépéseket:  ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-jive-tutorial/create_aaduser_05.png) 

    egy. Felhasználó típusa jelölje be az új felhasználó a szervezet.

    b. A felhasználónév **szövegmezőbe**írja be a **BrittaSimon**.

    c billentyűkombinációt. Kattintson a **Tovább**gombra.

6.  A **Felhasználói profil** párbeszédpanel lapon hajtsa végre az alábbi lépéseket: ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-jive-tutorial/create_aaduser_06.png) 

    egy. Az **első neve** mezőbe írja be a **Britta**.  

    b. Az **Utolsó neve** mezőbe írja be, **Simon**.

    c billentyűkombinációt. A **Megjelenítendő név** mezőbe írja be a **Britta Simon**.

    d. A **szerepkör** listában jelölje ki a **felhasználót**.

    e. Kattintson a **Tovább**gombra.

7. A párbeszédpanel **ideiglenes jelszót első** lapon kattintson a **Hozzon létre**.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-jive-tutorial/create_aaduser_07.png) 

8. A párbeszédpanel **ideiglenes jelszót első** lapon hajtsa végre az alábbi lépéseket:

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-jive-tutorial/create_aaduser_08.png) 

    egy. Jegyezze fel az **Új jelszót**az érték.

    b. Kattintson a **kész**gombra.   



###<a name="creating-a-jive-test-user"></a>Jive próba felhasználó létrehozása

Ebben a részben hozzon létre egy felhasználót Britta Simon nevű Jive. Kérje meg a felhasználók felvétele a Jive platform Jive támogatási csapattal.


###<a name="configuring-user-provisioning"></a>Felhasználói kiépítési konfigurálása
  
Ez a szakasz célja, hogyan engedélyezheti a felhasználó Active Directory felhasználói fiókok Jive kiépítési tagolása.  
Ez az eljárás részeként adja meg a felhasználó biztonsági jogkivonat kell kérnie Jive.com szükség.
  
Az alábbi képernyőképen látható, a kapcsolódó párbeszédpanelen az Azure Active Directory:

![Felhasználói kiépítési konfigurálása] (./media/active-directory-saas-jive-tutorial/IC698794.png "Felhasználói kiépítési konfigurálása")

####<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Felhasználói kiépítési beállításához hajtsa végre az alábbi lépéseket:

1.  Az Azure adatkezelési portálon **Jive** alkalmazás integrációs lapon kattintson a **konfigurálása felhasználói kiépítési** a **Felhasználó kiépítési beállítása** párbeszédpanel megnyitásához.

2.  Az **Adja meg ahhoz, hogy a felhasználó automatikus kiépítési Jive hitelesítő adatait** lapon adja meg a következő beállítások:

    1.  Az **Jive felügyeleti felhasználónév** mezőbe írja be egy Jive fiók nevét, amelynek a **Rendszergazda** profil rendelt Jive.com.

    2.  Az **Jive rendszergazdai jelszó** mezőbe írja be a fiókhoz tartozó jelszót.

    3.  Az **Jive bérlői webhely URL-cím** mezőbe írja be a Jive bérlői webhely URL-CÍMÉT.

        >[AZURE.NOTE]Jive bérlői webhely URL-CÍMÉT az URL-CÍMÉT, a szervezet által Jive való bejelentkezéshez használt.  
        Általában az URL-cím van-e a következő formátumban: **www.\< szervezeti\>. jive.com**.

    4.  Kattintson **az érvényesítés** a beállításainak ellenőrzése.

    5.  Kattintson a **Tovább** gombra kattintva nyissa meg a **megerősítő** lapon.

3.  A **Megerősítés** lapon kattintson a jelet a konfiguráció mentéséhez.
  
Most már létrehozhat fiók egy tesztfiók, várja meg a 10 perc, és ellenőrizze, hogy a fiók Jive.com lett-e szinkronizálva.




### <a name="assigning-the-azure-ad-test-user"></a>Az Azure Active Directory-próba felhasználói hozzárendelése

Ebben a részben Britta Simon Azure egyszeri bejelentkezés a saját hozzáférést biztosít Jive használandó engedélyezése.

![Felhasználó hozzárendelése][200] 

**Britta Simon hozzárendelése Jive, hajtsa végre az alábbi lépéseket:**

1. A klasszikus portál kattintva nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson az **alkalmazások** parancsra a felső menüben.

    ![Felhasználó hozzárendelése][201] 

2. Az alkalmazások listájában jelölje ki a **Jive**.

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-jive-tutorial/tutorial_jive_50.png) 

3. A felső sávon kattintson a **felhasználók**elemre.

    ![Felhasználó hozzárendelése][203]

4. A felhasználók listában jelölje ki a **Britta Simon**.

5. Az alsó eszköztáron kattintson a **hozzárendelni**.

    ![Felhasználó hozzárendelése][205]


### <a name="testing-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

Ebben a részben tesztelje az Azure Active Directory egyetlen bejelentkezés beállítási a Access panelen.

Ha a Jive csempére az Access panel gombra kattint, meg kell kérjen automatikusan aláírt-on az Jive alkalmazás.


## <a name="additional-resources"></a>További források

* [A szoftver alkalmazások integrálása az Azure Active Directory oktatóanyagok listája](active-directory-saas-tutorial-list.md)
* [Mi az access alkalmazás és az egyszeri bejelentkezés az Azure Active Directory címtárral?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-jive-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-jive-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-jive-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-jive-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-jive-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-jive-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-jive-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-jive-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-jive-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-jive-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-jive-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-jive-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-jive-tutorial/tutorial_general_205.png
