<properties
    pageTitle="Oktatóprogram: Azure Active Directory-integráció a Skydesk E-mail |} Microsoft Azure"
    description="Megtudhatja, hogy miként konfigurálása az egyszeri bejelentkezés Azure Active Directory és a Skydesk E-mail között."
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


# <a name="tutorial-azure-active-directory-integration-with-skydesk-email"></a>Oktatóprogram: Azure Active Directory-integráció a Skydesk e-mailben

Ebben az oktatóanyagban célja mutatja, hogy miként Skydesk E-mail integrálása az Azure Active Directory (Azure Active Directory).

Skydesk E-mail integrálása az Azure Active Directory biztosít a következő előnyökkel jár:

- Az Azure Active Directory Skydesk E-mail hozzáféréssel rendelkező személyek megadhatja, hogy
- Engedélyezheti a felhasználóknak, hogy automatikusan első jelentkezett-on (Single Sign-On) Skydesk e-mailt az Azure Active Directory-fiókok
- A fiókokat az egy központi helyen – a klasszikus Azure Active Directory-portál

Ha többet szeretne tudni a szoftver alkalmazás integrálása az Azure AD meg, hogy, ismerje meg, [az access alkalmazás és az egyszeri bejelentkezés az Azure Active Directory címtárral](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek

Azure AD-integráció a Skydesk levelezés beállításához szükséges az alábbiakat:

- Az Azure Active Directory-előfizetéssel
- Egy e-mailek Skydesk egyszeri bejelentkezés engedélyezett előfizetés


> [AZURE.NOTE] Az oktatóprogram lépéseit teszteléséhez nem használata ajánlott munkakörnyezetben.


Az oktatóprogram lépéseit teszteléséhez célszerű követnie az alábbi javaslatokat:

- Erre akkor szükség, ne használja a termelési környezetén.
- Ha nincs telepítve az Azure Active Directory próba környezetben, kattint egy hónap próba [Itt](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban célja lehetővé teszi az Azure Active Directory egyszeri bejelentkezés tesztkörnyezetben tesztelése. 

Az alkalmazási példát, ebben az oktatóanyagban tagolt két fő építőelemek áll:

1. E-mailek Skydesk hozzáadása a gyűjteményből
2. Beállítása és tesztelése Azure Active Directory egyszeri bejelentkezés


## <a name="adding-skydesk-email-from-the-gallery"></a>E-mailek Skydesk hozzáadása a gyűjteményből
Azure Active Directory Skydesk E-mail integrálása konfigurálásához szeretne Skydesk levelezés felvétele felügyelt szoftver alkalmazásai a gyűjteményből.

**A gyűjteményből Skydesk E-mail felvételéhez hajtsa végre az alábbi lépéseket:**

1. Az **Azure klasszikus portálon**kattintson a bal oldali navigációs területen kattintson az **Active Directory**. 

    ![Az Active Directory][1]

2. A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3. Nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson a felső menüben **alkalmazásokat** .

    ![Alkalmazások][2]

4. Kattintson a **Hozzáadás** az oldal alján.

    ![Alkalmazások][3]

5. **Milyen feladatot szeretne tenni** párbeszédpanelen kattintson **a gyűjteményből az alkalmazások felvétele**.

    ![Alkalmazások][4]

6. A Keresés mezőbe írja be **E-mail Skydesk**.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_01.png)

7. Az eredmény ablaktáblában jelölje ki a **Skydesk e-mailben**, és válassza a **kész** , az alkalmazás hozzáadása gombra.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Beállítása és tesztelése Azure Active Directory egyszeri bejelentkezés
Ez a szakasz célja mutatja, hogy beállítása és tesztelése Azure AD az egyszeri bejelentkezés Skydesk levelezés beállítása a próba-felhasználó "Britta Simon" nevű alapján.

Az egyszeri bejelentkezési a munkát az Azure Active Directory kell tudható, hogy mi az Azure Active Directory-felhasználónak Skydesk e-mailben megfelelőjük a felhasználó. Ez azt jelenti Azure AD-felhasználó, és a kapcsolódó felhasználó Skydesk e-mailben egy hivatkozást viszonya kell létrehozni.

Ez a hivatkozás kapcsolat a értéket a **felhasználó nevét** a **felhasználónév** Skydesk e-mailben értékként Azure AD hozzárendelésével jön létre.

Beállítása és tesztelése Azure AD az egyszeri bejelentkezés a Skydesk levelezés, a következő építőelemek szükséges:

1. **[Azure Active Directory konfigurálása az egyszeri bejelentkezés](#configuring-azure-ad-single-single-sign-on)** - engedélyezése a felhasználóknak, hogy ezzel a szolgáltatással.
2. **[Felhasználó létrehozása az Azure Active Directory tesztelése](#creating-an-azure-ad-test-user)** - Azure Active Directory egyszeri bejelentkezéssel való Britta Simon ellenőrzéséhez.
3. **[Skydesk e-mailek létrehozása tesztelje a felhasználó](#creating-a-Skydesk-Email-test-user)** -, hogy egy partner a Britta Simon kapcsolódik az Azure Active Directory ábrázolása amelyen Skydesk e-mailben.
4. **[Az Azure Active Directory hozzárendelése tesztelje a felhasználó](#assigning-the-azure-ad-test-user)** - ahhoz, hogy Britta Simon Azure AD az egyszeri bejelentkezés használata.
5. **[Tesztelés egyszeri bejelentkezés](#testing-single-sign-on)** - ellenőrzéséhez, hogy működik-e a konfigurációs.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure Active Directory Single Sign-On konfigurálása

Ez a szakasz célja Azure AD az egyszeri bejelentkezés az Azure klasszikus portálon engedélyezése és a Skydesk levelezőprogramban egyszeri bejelentkezés beállítása.



**Állítson be Azure AD az egyszeri bejelentkezés Skydesk e-mailek, hajtsa végre az alábbi lépéseket:**

1. Az Azure klasszikus portálon alkalmazás integrációs **Skydesk E-mail** lapján kattintson **konfigurálása az egyszeri bejelentkezés** a **Konfigurálása az egyszeri bejelentkezés** párbeszédpanel megnyitásához.

    ![Egyszeri bejelentkezés beállítása][6] 

2. **Hogyan szeretné, hogy jelentkezzen be az e-mailek Skydesk felhasználók** lapon válassza az **Azure Active Directory Single Sign-On**, és kattintson a **Tovább gombra**.

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_03.png) 


3. Az **Alkalmazás beállításainak megadása** párbeszédpanel lapon hajtsa végre az alábbi lépéseket:
 
    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_04.png) 


    egy. Bejelentkezési a URL-cím mezőbe írja be a felhasználóknak, hogy a bejelentkezés az e-mailek Skydesk alkalmazás használatáról a következő mintát által használt URL-címe: **"https://mail.skydesk.jp/portal/\<vállalatnév\>"**.

    b. Kattintson a **Tovább**gombra.


4. A **konfigurálása az egyszeri bejelentkezés a Skydesk e-mailben** lapon hajtsa végre az alábbi lépéseket:

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_05.png) 

    egy. Kattintson a **tanúsítvány letöltése**, és mentse a fájlt a számítógépen.

    b. Kattintson a **Tovább**gombra.


5. Ahhoz, hogy egyszeri bejelentkezési **Skydesk**e-mailben, hajtsa végre az alábbi lépéseket:
 
    egy. Bejelentkezés az Skydesk E-mail fiók rendszergazdaként.

    b. A felső sávon kattintson a beállítás gombra, és válassza a. szervezeti 

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_51.png)

    c billentyűkombinációt. Kattintson a tartományok, a bal oldali panelről.

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_53.png)

    d. A gombra kattintva vegye fel a tartományát.

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_54.png)

    e. Írja be a tartománynevét, és kattintson a tartomány ellenőrzéséhez hivatkozásra.

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_55.png)

    f. Kattintson a bal oldali ablaktáblában a **SAML-hitelesítés**

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_52.png)

6. A **SAML hitelesítési** párbeszédpanel lapon hajtsa végre az alábbi lépéseket:

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_56.png)

    > [AZURE.NOTE] SAML-alapú hitelesítés használatára, vagy rendelkeznie kell **tartomány ellenőrizve** vagy **portál URL-cím** beállítása. Beállíthatja, hogy a portál URL-címe egyedi neve.

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_57.png)


    egy. Az Azure Active Directory-klasszikus portálon a **SAML egyszeri bejelentkezési URL-címe** értéket másolja és illessze be a **Bejelentkezési URL-cím** mezőben lévő értéket.

    b. Az Azure Active Directory-klasszikus portálon másolja az **Egyetlen Sign-Out szolgáltatás URL-címe** értéket, és illessze be a **Kijelentkezés** URL-címe mezőben lévő értéket.

    c billentyűkombinációt. **Jelszó URL-cím módosítása** nem kötelező, hagyja üresen a mezőt.

    d. Kattintson a **Fájl kulcs első** jelölje be a letöltött Skydesk E-mail tanúsítvány, és kattintson a **Megnyitás** a tanúsítvány feltöltése.

    e. Jelölje ki a **RSA** **algoritmus**.

    f. Kattintson az **Ok** gombra a módosítások mentéséhez.


7. Az Azure klasszikus portálon válassza ki az egyszeri bejelentkezéses konfigurációs megerősítő, és kattintson a **Tovább gombra**.

    ![Azure Active Directory Single Sign-On][10]

8. Az **egyszeri bejelentkezés megerősítés** lapon kattintson a **Befejezés**gombra.  
  
    ![Azure Active Directory Single Sign-On][11]




### <a name="creating-an-azure-ad-test-user"></a>Azure AD-próba felhasználó létrehozása
Ez a szakasz célja vizsgálat felhasználó létrehozása az Azure-Britta Simon nevű klasszikus portálon.

![Azure Active Directory-felhasználó létrehozása][20]

**Teszt felhasználó létrehozása az Azure Active Directory, hajtsa végre az alábbi lépéseket:**

1. Az **Azure klasszikus portálon**kattintson a bal oldali navigációs területen kattintson az **Active Directory**.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-skydeskemail-tutorial/create_aaduser_09.png) 

2. A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3. A menüben, a képernyő tetején, a felhasználók listájának megjelenítéséhez kattintson a **felhasználók**elemre.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-skydeskemail-tutorial/create_aaduser_03.png) 

4. **Felhasználó hozzáadása**elemre az alsó eszköztáron a **Felhasználó hozzáadása** párbeszédpanel megnyitásához.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-skydeskemail-tutorial/create_aaduser_04.png) 

5. A párbeszédpanel **közli velünk a felhasználóra vonatkozó** lapon hajtsa végre az alábbi lépéseket:

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-skydeskemail-tutorial/create_aaduser_05.png) 

    egy. Felhasználó típusa jelölje be az új felhasználó a szervezet.

    b. A felhasználónév **szövegmezőbe**írja be a **BrittaSimon**.

    c billentyűkombinációt. Kattintson a **Tovább**gombra.

6.  A **Felhasználói profil** párbeszédpanel lapon hajtsa végre az alábbi lépéseket:

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-skydeskemail-tutorial/create_aaduser_06.png) 

    egy. Az **első neve** mezőbe írja be a **Britta**.  

    b. Az **Utolsó neve** mezőbe írja be, **Simon**.

    c billentyűkombinációt. A **Megjelenítendő név** mezőbe írja be a **Britta Simon**.

    d. A **szerepkör** listában jelölje ki a **felhasználót**.

    e. Kattintson a **Tovább**gombra.

7. A párbeszédpanel **ideiglenes jelszót kérjen** lapon kattintson a **Hozzon létre**.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-skydeskemail-tutorial/create_aaduser_07.png) 

8. A párbeszédpanel **ideiglenes jelszót első** lapon hajtsa végre az alábbi lépéseket:
 
    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-skydeskemail-tutorial/create_aaduser_08.png) 

    egy. Jegyezze fel az **Új jelszót**az érték.

    b. Kattintson a **kész**gombra.   



### <a name="creating-a-skydesk-email-test-user"></a>E-mailek Skydesk vizsgálat felhasználó létrehozása

Ebben a részben hozzon létre egy felhasználót Britta Simon nevű Skydesk e-mailben.

egy. **Felhasználói hozzáférés** kattintson a bal oldali panelen Skydesk e-mailben, és írja be felhasználónevét. 

![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_58.png)


[AZURE.NOTE] Felhasználók tömeges létrehozásához szükséges, ha meg kell az e-mailek Skydesk támogatási csoport munkatársaitól kaphat.


### <a name="assigning-the-azure-ad-test-user"></a>Az Azure Active Directory-próba felhasználói hozzárendelése

Ez a szakasz célja Britta Simon használandó Azure egyszeri bejelentkezés a saját hozzáférést biztosít Skydesk e-mailek engedélyezése.

![Felhasználó hozzárendelése][200] 

**Britta Simon hozzárendelése Skydesk e-mailek, hajtsa végre az alábbi lépéseket:**

1. Az Azure klasszikus portál kattintva nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson az **alkalmazások** parancsra a felső menüben.
 
    ![Felhasználó hozzárendelése][201] 

2. Az alkalmazások listájában jelölje ki a **Skydesk e-mailben**.

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_50.png) 

1. A felső sávon kattintson a **felhasználók**elemre.

    ![Felhasználó hozzárendelése][203] 

1. A felhasználók listában jelölje ki a **Britta Simon**.

2. Az alsó eszköztáron kattintson a **hozzárendelni**.

    ![Felhasználó hozzárendelése][205]



### <a name="testing-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

Ez a szakasz célja a Azure Active Directory egyszeri bejelentkezéses beállítások tesztelése a Access panelen.

Amikor a Skydesk E-mail csempére az Access panel gombra kattint, meg kell első automatikusan aláírt-on az e-mailek Skydesk alkalmazás.


## <a name="additional-resources"></a>További források

* [A szoftver alkalmazások integrálása az Azure Active Directory oktatóanyagok listája](active-directory-saas-tutorial-list.md)
* [Mi az access alkalmazás és az egyszeri bejelentkezés az Azure Active Directory címtárral?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_205.png
