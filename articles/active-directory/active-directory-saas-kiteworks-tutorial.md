<properties
    pageTitle="Oktatóprogram: Azure Active Directory-integráció a Kiteworks |} Microsoft Azure"
    description="Megtudhatja, hogy miként konfigurálása az egyszeri bejelentkezés Azure Active Directory és Kiteworks között."
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


# <a name="tutorial-azure-active-directory-integration-with-kiteworks"></a>Oktatóprogram: Azure Active Directory-integráció a Kiteworks


Ebben az oktatóanyagban célja mutatja, hogy miként Kiteworks integrálása az Azure Active Directory (Azure Active Directory).  
Kiteworks integrálása az Azure Active Directory biztosít a következő előnyökkel jár: 

- Az Azure Active Directory, aki rendelkezik hozzáféréssel Kiteworks megadhatja, hogy 
- Engedélyezheti a felhasználóknak, hogy automatikusan első jelentkezett-on való Kiteworks (Single Sign-On) az Azure Active Directory-fiókok
- A fiókokat az egy központi helyen – az Azure Active Directory 

Ha többet szeretne tudni a szoftver alkalmazás integrálása az Azure AD meg, hogy, ismerje meg, [az access alkalmazás és az egyszeri bejelentkezés az Azure Active Directory címtárral](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek 

Azure Active Directory integráció a Kiteworks van szükség az alábbiakat:

- Az Azure Active Directory-előfizetéssel
- Egy Kiteworks egyszeri bejelentkezés engedélyezett előfizetés


> [AZURE.NOTE] Az oktatóprogram lépéseit teszteléséhez nem használata ajánlott munkakörnyezetben.


Az oktatóprogram lépéseit teszteléséhez célszerű követnie az alábbi javaslatokat:

- Erre akkor szükség, ne használja a termelési környezetén.
- Ha nincs telepítve az Azure Active Directory próba környezetben, kattint egy hónap próba [Itt](https://azure.microsoft.com/pricing/free-trial/). 

 
## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban célja lehetővé teszi az Azure Active Directory egyszeri bejelentkezés tesztkörnyezetben tesztelése.  
Az alkalmazási példát, ebben az oktatóanyagban tagolt két fő építőelemek áll:

1. Kiteworks hozzáadása a gyűjteményből 
2. Beállítása és tesztelése Azure Active Directory egyszeri bejelentkezés


## <a name="adding-kiteworks-from-the-gallery"></a>Kiteworks hozzáadása a gyűjteményből
Konfigurálja az Azure Active Directory integrálása a Kiteworks, meg kell Kiteworks hozzáadása az felügyelt szoftver alkalmazáslistában a gyűjteményből.

**A gyűjteményből Kiteworks felvételéhez hajtsa végre az alábbi lépéseket:**

1. Az **Azure klasszikus portálon**kattintson a bal oldali navigációs területen kattintson az **Active Directory**. 

    ![Az Active Directory][1]

2. A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3. Nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson a felső menüben **alkalmazások** .

    ![Alkalmazások][2]

4. Kattintson a **Hozzáadás** az oldal alján.
 
    ![Alkalmazások][3]

5. **Milyen feladatot szeretne tenni** párbeszédpanelen kattintson **a gyűjteményből az alkalmazások felvétele**.

    ![Alkalmazások][4]

6. A Keresés mezőbe írja be a **Kiteworks**.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-kiteworks-tutorial/tutorial_kiteworks_01.png)

7. Az eredmény ablaktáblában jelölje ki a **Kiteworks**, és válassza a **kész** , az alkalmazás hozzáadása gombra.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-kiteworks-tutorial/tutorial_kiteworks_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Beállítása és tesztelése Azure Active Directory egyszeri bejelentkezés
Ez a szakasz célja mutatja, hogy állítsa be, és a "Britta Simon" nevű próba felhasználón alapuló Kiteworks Azure AD az egyszeri bejelentkezés tesztelése.

Az egyszeri bejelentkezési a munkát az Azure Active Directory kell, hogy mi az a partner felhasználó Kiteworks az Azure Active Directory-felhasználónak. Más szóval Azure AD-felhasználó, és a kapcsolódó felhasználó Kiteworks hivatkozás viszonya kell létrehozni.  
A hivatkozás kapcsolat jön létre által az Azure Active Directory Kiteworks **felhasználónév** értékként a **felhasználó neve** értéket rendeli.
 
Állítsa be, és Azure Active Directory egyszeri bejelentkezéssel való Kiteworks tesztelése, a következő építőelemek szükséges:

1. **[Azure Active Directory konfigurálása az egyszeri bejelentkezés](#configuring-azure-ad-single-single-sign-on)** - engedélyezése a felhasználóknak, hogy ezzel a szolgáltatással.
2. **[Felhasználó létrehozása az Azure Active Directory tesztelése](#creating-an-azure-ad-test-user)** - Azure Active Directory egyszeri bejelentkezéssel való Britta Simon ellenőrzéséhez.
4. **[Felhasználó létrehozása egy Kiteworks tesztelése](#creating-a-kiteworks-test-user)** - van-e egy megfelelője a Britta Simon Kiteworks, amelyen az Azure Active Directory ábrázolt csatolt.
5. **[Az Azure Active Directory hozzárendelése tesztelje a felhasználó](#assigning-the-azure-ad-test-user)** - ahhoz, hogy Britta Simon Azure AD az egyszeri bejelentkezés használata.
5. **[Tesztelés egyszeri bejelentkezés](#testing-single-sign-on)** - ellenőrzéséhez, hogy működik-e a konfigurációs.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure Active Directory Single Sign-On konfigurálása

Ez a szakasz célja Azure AD az egyszeri bejelentkezés az Azure klasszikus portálon engedélyezése és a Kiteworks alkalmazásban az egyszeri bejelentkezés beállítása. Ez az eljárás részeként biztosan szükséges számrendszerben-64 kódolt tanúsítvány fájlt. Ha nem ismeri a ezt az eljárást, olvassa el a [szövegfájlba bináris tanúsítvány konvertálása](http://youtu.be/PlgrzUZ-Y1o)című témakört.

Egyszeri bejelentkezés az Kiteworks konfigurálásához kell regisztrált tartományt. Ha még nincs regisztrált tartományt, a Kiteworks támogatási csoport munkatársaitól.  



**Állítson be Azure AD az egyszeri bejelentkezés Kiteworks, hajtsa végre az alábbi lépéseket:**

1. Az Azure klasszikus portálon **Kiteworks** alkalmazás integrációs lapon kattintson **konfigurálása az egyszeri bejelentkezés** a **Konfigurálása az egyszeri bejelentkezés** párbeszédpanel megnyitásához.

    ![Egyszeri bejelentkezés beállítása][6] 

2. **Hogyan szeretné, hogy jelentkezzen be az Kiteworks felhasználók** lapon válassza az **Azure Active Directory Single Sign-On**, és kattintson a **Tovább gombra**.

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-kiteworks-tutorial/tutorial_kiteworks_03.png) 

3. Az **Alkalmazás beállításainak megadása** párbeszédpanel lapon hajtsa végre az alábbi lépéseket:.

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-kiteworks-tutorial/tutorial_kiteworks_04.png) 


    egy. **Jelentkezzen be URL-cím** mezőbe írja be a felhasználóknak, hogy a bejelentkezés az Kiteworks alkalmazás által használt URL-CÍMÉT (például: *https://fabrikam.kiteworks.com/*).

    b. Kattintson a **Tovább**gombra.
 
 
4. A **beállítás az egyszeri bejelentkezés Kiteworks a** lapon hajtsa végre az alábbi lépéseket:

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-kiteworks-tutorial/tutorial_kiteworks_05.png) 

    egy. Kattintson a **tanúsítvány letöltése**, és mentse a fájlt a számítógépen.

    b. Kattintson a **Tovább**gombra.


1. Jelentkezzen be rendszergazdaként a Kiteworks vállalati webhely.

1. Kattintson az eszköztáron a képernyő tetején kattintson a **Beállítások**gombra.

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-kiteworks-tutorial/tutorial_kiteworks_06.png) 


1. **Hitelesítés és az engedélyezés** csoportban kattintson az **Egyszeri bejelentkezés beállítása**. 

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-kiteworks-tutorial/tutorial_kiteworks_07.png) 


1. Az egyszeri bejelentkezés beállítási lapon hajtsa végre az alábbi lépéseket:

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-kiteworks-tutorial/tutorial_kiteworks_09.png) 

    egy. Jelölje ki **egyszeri bejelentkezés hitelesíthet**.

    b. Jelölje ki a **AuthnRequest kezdeményezhet**.

    c billentyűkombinációt. Az Azure klasszikus portálon **konfigurálása az egyszeri bejelentkezés a Kiteworks** párbeszédpanel lapon a **Szervezet azonosítója** értéket másolja és illessze be a **IDP entitás azonosítója** mezőben lévő értéket. 

    d. Az Azure klasszikus portálon **konfigurálása az egyszeri bejelentkezés a Kiteworks** párbeszédpanel lapon az **Egyszeri bejelentkezéses szolgáltatás URL-címe** értéket másolja és illessze be a **Egyszeri bejelentkezéses szolgáltatás URL-címe** mezőben lévő értéket.

    e. Az Azure klasszikus portálon **konfigurálása az egyszeri bejelentkezés a Kiteworks** párbeszédpanel lapon az **Egyetlen Sign-Out szolgáltatás URL-címe** értéket másolja és illessze be a **Egyetlen kijelentkezés szolgáltatás URL-címe** mezőben lévő értéket.

    f. Nyissa meg a letöltött tanúsítvány a Jegyzettömbben, a tartalom másolása és beillesztése a **RSA nyilvános kulcs tanúsítvány** mezőben lévő értéket. 

    g. Kattintson a **Mentés**gombra.


6. Az Azure klasszikus portálon válassza ki az egyszeri bejelentkezéses konfigurációs megerősítő, és kattintson a **Tovább gombra**. 

    ![Azure Active Directory Single Sign-On][10]

7. Az **egyszeri bejelentkezés megerősítés** lapon kattintson a **Befejezés**gombra.  

    ![Azure Active Directory Single Sign-On][11]




### <a name="creating-an-azure-ad-test-user"></a>Azure AD-próba felhasználó létrehozása
Ez a szakasz célja próba felhasználó létrehozása az Azure-Britta Simon nevű klasszikus portálon.

![Azure Active Directory-felhasználó létrehozása][20]

**Teszt felhasználó létrehozása az Azure Active Directory, hajtsa végre az alábbi lépéseket:**

1. Az **Azure klasszikus portálon**kattintson a bal oldali navigációs területen kattintson az **Active Directory**.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-kiteworks-tutorial/create_aaduser_09.png)  

2. A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3. A menüben, a képernyő tetején, a felhasználók listájának megjelenítéséhez kattintson a **felhasználók**elemre.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-kiteworks-tutorial/create_aaduser_03.png) 
 
4. **Felhasználó hozzáadása**elemre az alsó eszköztáron a **Felhasználó hozzáadása** párbeszédpanel megnyitásához. 

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-kiteworks-tutorial/create_aaduser_04.png) 

5. A párbeszédpanel **közli velünk a felhasználóra vonatkozó** lapon hajtsa végre az alábbi lépéseket: 

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-kiteworks-tutorial/create_aaduser_05.png)  

    egy. Felhasználó típusa jelölje be az új felhasználó a szervezet.

    b. A felhasználónév **szövegmezőbe**írja be a **BrittaSimon**.

    c billentyűkombinációt. Kattintson a **Tovább**gombra.

6.  A **Felhasználói profil** párbeszédpanel lapon hajtsa végre az alábbi lépéseket: 

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-kiteworks-tutorial/create_aaduser_06.png) 
 
    egy. Az **első neve** mezőbe írja be a **Britta**.  

    b. Az **Utolsó neve** mezőbe írja be, **Simon**.

    c billentyűkombinációt. A **Megjelenítendő név** mezőbe írja be a **Britta Simon**.

    d. A **szerepkör** listában jelölje ki a **felhasználót**.
    e. Kattintson a **Tovább**gombra.

7. A párbeszédpanel **ideiglenes jelszót kérjen** lapon kattintson a **Hozzon létre**.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-kiteworks-tutorial/create_aaduser_07.png) 
 
8. A párbeszédpanel **ideiglenes jelszót első** lapon hajtsa végre az alábbi lépéseket:

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-kiteworks-tutorial/create_aaduser_08.png) 
  
    egy. Jegyezze fel az **Új jelszót**az érték.

    b. Kattintson a **kész**gombra.   

  
 
### <a name="creating-a-kiteworks-test-user"></a>Kiteworks próba felhasználó létrehozása

Ez a szakasz célja Britta Simon nevű Kiteworks felhasználó létrehozása.
Kiteworks támogatja a kiépítési közvetlenül az idő, amely alapértelmezés szerint ki van engedélyezve van.

Nincs teendő ebben a szakaszban a meg nem.
Új felhasználóként Kitewors eléréséhez, ha még nem létezik kísérlet során jön létre.

> [AZURE.NOTE] Ha egy felhasználó létrehozása manuálisan kell szüksége a Kiteworks támogatási csoport munkatársaitól kaphat.


### <a name="assigning-the-azure-ad-test-user"></a>Az Azure Active Directory-próba felhasználói hozzárendelése

Ez a szakasz célja Britta Simon Azure egyszeri bejelentkezés a saját hozzáférést biztosít Kiteworks használandó engedélyezése.

![Felhasználó hozzárendelése][200] 

**Britta Simon hozzárendelése Kiteworks, hajtsa végre az alábbi lépéseket:**

1. Az Azure klasszikus portál kattintva nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson az **alkalmazások** parancsra a felső menüben.

    ![Felhasználó hozzárendelése][201] 

2. Az alkalmazások listájában jelölje ki a **Kiteworks**.

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-kiteworks-tutorial/tutorial_kiteworks_50.png) 

1. A felső sávon kattintson a **felhasználók**elemre.

    ![Felhasználó hozzárendelése][203] 

1. A felhasználók listában jelölje ki a **Britta Simon**.

2. Az alsó eszköztáron kattintson a **hozzárendelni**.

    ![Felhasználó hozzárendelése][205]



### <a name="testing-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

Ez a szakasz célja a Azure Active Directory egyszeri bejelentkezéses beállítások tesztelése a Access panelen.  
Amikor az Access panel a Kiteworks mozaik gombra kattint, meg kell első automatikusan aláírt-on az Kiteworks alkalmazás.


## <a name="additional-resources"></a>További források

* [A szoftver alkalmazások integrálása az Azure Active Directory oktatóanyagok listája](active-directory-saas-tutorial-list.md)
* [Mi az access alkalmazás és az egyszeri bejelentkezés az Azure Active Directory címtárral?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-kiteworks-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-kiteworks-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-kiteworks-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-kiteworks-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-kiteworks-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-kiteworks-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-kiteworks-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-kiteworks-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-kiteworks-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-kiteworks-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-kiteworks-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-kiteworks-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-kiteworks-tutorial/tutorial_general_205.png






