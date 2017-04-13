<properties
    pageTitle="Oktatóprogram: Azure Active Directory-integráció a Litmos |} Microsoft Azure"
    description="Megtudhatja, hogy miként konfigurálása az egyszeri bejelentkezés Azure Active Directory és Litmos között."
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


# <a name="tutorial-azure-active-directory-integration-with-litmos"></a>Oktatóprogram: Azure Active Directory-integráció a Litmos

Ebben az oktatóanyagban célja mutatja, hogy miként Litmos integrálása az Azure Active Directory (Azure Active Directory).  
Litmos integrálása az Azure Active Directory biztosít a következő előnyökkel jár: 

- Az Azure Active Directory, aki rendelkezik hozzáféréssel Litmos megadhatja, hogy 
- Engedélyezheti a felhasználóknak, hogy automatikusan első jelentkezett-on való Litmos (Single Sign-On) az Azure Active Directory-fiókok
- A fiókokat az egy központi helyen – az Azure Active Directory 

Ha többet szeretne tudni a szoftver alkalmazás integrálása az Azure AD meg, hogy, ismerje meg, [az access alkalmazás és az egyszeri bejelentkezés az Azure Active Directory címtárral](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek 

Azure Active Directory integráció a Litmos van szükség az alábbiakat:

- Az Azure Active Directory-előfizetéssel
- Egy Litmos egyszeri bejelentkezés engedélyezett előfizetés


> [AZURE.NOTE] Az oktatóprogram lépéseit teszteléséhez nem használata ajánlott munkakörnyezetben.


Az oktatóprogram lépéseit teszteléséhez célszerű követnie az alábbi javaslatokat:

- Erre akkor szükség, ne használja a termelési környezetén.
- Ha nincs telepítve az Azure Active Directory próba környezetben, kattint egy hónap próba [Itt](https://azure.microsoft.com/pricing/free-trial/). 

 
## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban célja lehetővé teszi az Azure Active Directory egyszeri bejelentkezés tesztkörnyezetben tesztelése.  
Az alkalmazási példát, ebben az oktatóanyagban tagolt három fő építőelemek áll:

1. Litmos hozzáadása a gyűjteményből 
2. Beállítása és tesztelése Azure Active Directory egyszeri bejelentkezés


## <a name="adding-litmos-from-the-gallery"></a>Litmos hozzáadása a gyűjteményből
Konfigurálja az Azure Active Directory integrálása a Litmos, meg kell Litmos hozzáadása az felügyelt szoftver alkalmazáslistában a gyűjteményből.

**A gyűjteményből Litmos felvételéhez hajtsa végre az alábbi lépéseket:**

1. Az **Azure klasszikus portálon**kattintson a bal oldali navigációs területen kattintson az **Active Directory**. 

    ![Az Active Directory][1]

2. A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3. Nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson a felső menüben **alkalmazásokat** .

    ![Alkalmazások][2]

4. Kattintson a **Hozzáadás** az oldal alján.

    ![Alkalmazások][3]

5. **Milyen feladatot szeretne tenni** párbeszédpanelen kattintson **a gyűjteményből az alkalmazások felvétele**.

    ![Alkalmazások][4]

6. A Keresés mezőbe írja be a **Litmos**.

    ![Alkalmazások][5]

7. Az eredmény ablaktáblában jelölje ki a **Litmos**, és válassza a **kész** , az alkalmazás hozzáadása gombra.

    ![Alkalmazások][500]


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Beállítása és tesztelése Azure Active Directory egyszeri bejelentkezés
Ez a szakasz célja mutatja, hogy állítsa be, és a "Britta Simon" nevű próba felhasználón alapuló Litmos Azure AD az egyszeri bejelentkezés tesztelése.

Az egyszeri bejelentkezési a munkát az Azure Active Directory kell, hogy mi az a partner felhasználó Litmos az Azure Active Directory-felhasználónak. Más szóval Azure AD-felhasználó, és a kapcsolódó felhasználó Litmos hivatkozás viszonya kell létrehozni.  
A hivatkozás kapcsolat jön létre által az Azure Active Directory Litmos **felhasználónév** értékként a **felhasználó neve** értéket rendeli.
 
Állítsa be, és Azure Active Directory egyszeri bejelentkezéssel való Litmos tesztelése, az alábbi építőelemeket szükséges:

1. **[Azure Active Directory konfigurálása az egyszeri bejelentkezés](#configuring-azure-ad-single-single-sign-on)** - engedélyezése a felhasználóknak, hogy ezzel a szolgáltatással.
2. **[Felhasználó létrehozása az Azure Active Directory tesztelése](#creating-an-azure-ad-test-user)** - Azure Active Directory egyszeri bejelentkezéssel való Britta Simon ellenőrzéséhez.
4. **[Felhasználó létrehozása egy Litmos tesztelése](#creating-a-halogen-software-test-user)** - van-e egy megfelelője a Britta Simon Litmos, amelyen az Azure Active Directory ábrázolt csatolt.
5. **[Az Azure Active Directory hozzárendelése tesztelje a felhasználó](#assigning-the-azure-ad-test-user)** - ahhoz, hogy Britta Simon Azure AD az egyszeri bejelentkezés használata.
5. **[Tesztelés egyszeri bejelentkezés](#testing-single-sign-on)** - ellenőrzéséhez, hogy működik-e a konfigurációs.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure Active Directory Single Sign-On konfigurálása

Ez a szakasz célja Azure AD az egyszeri bejelentkezés az Azure Active Directory klasszikus portálon engedélyezése és a Litmos alkalmazásban az egyszeri bejelentkezés beállítása.  
Ez az eljárás részeként biztosan szükséges számrendszerben-64 kódolt tanúsítvány fájlt.  
Ha nem ismeri a ezt az eljárást, olvassa el a [szövegfájlba bináris tanúsítvány konvertálása](http://youtu.be/PlgrzUZ-Y1o)című témakört.

A konfiguráció részeként testre szeretné szabni a **SAML jogkivonat attribútumok** Litmos alkalmazás.  

![Azure Active Directory Single Sign-On][17] 

**Állítson be Azure AD az egyszeri bejelentkezés Litmos, hajtsa végre az alábbi lépéseket:**

1. Az Azure Active Directory klasszikus portálon **Litmos** alkalmazás integrációs lapon kattintson **konfigurálása az egyszeri bejelentkezés** a **Konfigurálása az egyszeri bejelentkezés** párbeszédpanel megnyitásához.

    ![Egyszeri bejelentkezés beállítása][6] 

2. **Hogyan szeretné, hogy jelentkezzen be az Litmos felhasználók** lapon válassza az **Azure Active Directory Single Sign-On**, és kattintson a **Tovább gombra**.
 
    ![Azure Active Directory Single Sign-On][7] 


1. Bejelentkezés a Litmos vállalat webhelyére (például: *https://azureapptest.litmos.com/account/Login*) rendszergazdaként.

    ![Azure Active Directory Single Sign-On][21] 


1. A bal oldali navigációs sávon kattintson a **fiókok**fülre.

    ![Azure Active Directory Single Sign-On][22] 


1. Kattintson a **integrációs eszközök** fülre.

    ![Azure Active Directory Single Sign-On][23] 


1. A **integrációs** lapon görgessen le a **3 fél integrációs**, és kattintson **a SAML 2.0** fülre.

    ![Azure Active Directory Single Sign-On][24] 

1. Másolja a vágólapra az értéket a **a SAML endoiint az litmos van:**.

    ![Azure Active Directory Single Sign-On][26] 


3. Az Azure klasszikus portálon a **Alkalmazás beállításainak megadása** párbeszédpanel lapon hajtsa végre az alábbi lépéseket:

    ![Azure Active Directory Single Sign-On][8] 
 
    egy. Az **azonosító** mezőbe írja be a bejelentkezés az Litmos alkalmazás a felhasználók által használt URL-CÍMÉT (például: *https://azureapptest.litmos.com/account/Login*).
     
    b. **Válasz URL-címe** mezőbe illessze be a az Litmos alkalmazásból, az előző lépésben kimásolt értéket.

    c billentyűkombinációt. Kattintson a **Tovább**gombra.
 
4. A **beállítás az egyszeri bejelentkezés Litmos a** lapon hajtsa végre az alábbi lépéseket:

    ![Azure Active Directory Single Sign-On][2] 

    egy. Kattintson a letöltés tanúsítvány, és mentse a fájlt a számítógépen.


1. A **Litmos** alkalmazásban hajtsa végre az alábbi lépéseket:

    ![Azure Active Directory Single Sign-On][25] 

    egy. Kattintson a **SAML engedélyezése**gombra.

    b. A letöltött tanúsítvány **Alap-64-címként kódolt** fájl létrehozása  

    >[AZURE.TIP] További információra kíváncsi olvassa el a [bináris tanúsítvány szövegfájlba konvertálása](http://youtu.be/PlgrzUZ-Y1o) című témakört.

    c billentyűkombinációt. Nyissa meg az alap-64 kódolt tanúsítvány a Jegyzettömbben, azt tartalmát a vágólapra másolja és illessze be a **SAML x.509-es tanúsítvány** mezőben lévő értéket szeretné.

    d. A **módosítások mentése**gombra.


6. Az Azure Active Directory klasszikus portálon jelölje be a egyszeri bejelentkezéses konfigurációs megerősítés, és kattintson a **Tovább gombra**. 

    ![Azure Active Directory Single Sign-On][10]

7. Az **egyszeri bejelentkezés megerősítés** lapon kattintson a **Befejezés**gombra.  
  
    ![Azure Active Directory Single Sign-On][11]


20. Kattintson a menü felső **attribútumok** a **SAML jogkivonat attribútumok** párbeszédpanel megnyitásához. 

    ![Egyszeri bejelentkezés beállítása][12]


24. A **Felhasználó attribútum hozzáadása** párbeszédpanelen hajtsa végre az alábbi lépéseket: 

    ![Egyszeri bejelentkezés beállítása][14]

  	| Attribútumnév | Attribútumérték |
  	| ---            | ---             |
  	| E-mailben          | User.mail       |
  	| Utónév      | User.givenName  |
  	| Utónév       | User.surname    |

    Az adatok tábla minden sorához a fenti hajtsa végre az alábbi lépéseket:
   
    egy. Kattintson a **felhasználó attribútum hozzáadása**gombra. 

    ![Egyszeri bejelentkezés beállítása][15]


    egy. **Attribútum neve** mezőbe írja be a **Attribútumnév** meg abban a sorban látható.

    b. Válassza a **Attribútumérték** meg abban a sorban látható.

    c billentyűkombinációt. Válassza a **kész** , a **Felhasználó attribútum hozzáadása** párbeszédpanel bezárásához.


25. Kattintson a **módosítások alkalmazásához**. 

    ![Egyszeri bejelentkezés beállítása][16]




### <a name="creating-an-azure-ad-test-user"></a>Azure AD-próba felhasználó létrehozása
Ez a szakasz célja vizsgálat felhasználó létrehozása az Azure-Britta Simon nevű klasszikus portálon.  

![Azure Active Directory-felhasználó létrehozása][20]

**Teszt felhasználó létrehozása az Azure Active Directory, hajtsa végre az alábbi lépéseket:**

1. Az **Azure clasic portálon**kattintson a bal oldali navigációs területen kattintson az **Active Directory**.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-litmos-tutorial/create_aaduser_09.png)  

2. A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3. A menüben, a képernyő tetején, a felhasználók listájának megjelenítéséhez kattintson a **felhasználók**elemre.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-litmos-tutorial/create_aaduser_03.png) 
 
4. **Felhasználó hozzáadása**elemre az alsó eszköztáron a **Felhasználó hozzáadása** párbeszédpanel megnyitásához. 

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-litmos-tutorial/create_aaduser_04.png) 

5. A párbeszédpanel **közli velünk a felhasználóra vonatkozó** lapon hajtsa végre az alábbi lépéseket: 

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-litmos-tutorial/create_aaduser_05.png)  

    egy. **Írja be a felhasználó**jelölje ki **a szervezet új felhasználót**.

    b. A felhasználónév **szövegmezőbe**írja be a **BrittaSimon**.

    c billentyűkombinációt. Kattintson a **Tovább**gombra.

6.  A **Felhasználói profil** párbeszédpanel lapon hajtsa végre az alábbi lépéseket: 

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-litmos-tutorial/create_aaduser_06.png) 
 
    egy. Az **első neve** mezőbe írja be a **Britta**.  

    b. Az **Utolsó neve** mezőbe írja be, **Simon**.

    c billentyűkombinációt. A **Megjelenítendő név** mezőbe írja be a **Britta Simon**.

    d. A **szerepkör** listában jelölje ki a **felhasználót**.
    e. Kattintson a **Tovább**gombra.

7. A párbeszédpanel **ideiglenes jelszót kérjen** lapon kattintson a **Hozzon létre**.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-litmos-tutorial/create_aaduser_07.png) 
 
8. A párbeszédpanel **ideiglenes jelszót első** lapon hajtsa végre az alábbi lépéseket:

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-litmos-tutorial/create_aaduser_08.png) 
  
    egy. Jegyezze fel az **Új jelszót**az érték.

    b. Kattintson a **kész**gombra.   

  
 
### <a name="creating-a-litmos-test-user"></a>Litmos próba felhasználó létrehozása

Ez a szakasz célja Britta Simon nevű Litmos felhasználó létrehozása.  
Az Litmos alkalmazás támogatja az csupán az idő a kiépítési. Ez azt jelenti, egy felhasználói fiókot automatikusan létrejön szükség esetén az alkalmazás használata az Access Panel eléréséhez kísérlet során.

**Britta Simon nevű Litmos felhasználó létrehozása, hajtsa végre az alábbi lépéseket:**


1. Bejelentkezés a Litmos vállalat webhelyére (például: *https://azureapptest.litmos.com/account/Login*) rendszergazdaként.

    ![Azure Active Directory Single Sign-On][21] 


1. A bal oldali navigációs sávon kattintson a **fiókok**fülre.

    ![Azure Active Directory Single Sign-On][22] 


1. Kattintson a **integrációs eszközök** fülre.

    ![Azure Active Directory Single Sign-On][23] 


1. A **integrációs** lapon görgessen le a **3 fél integrációs**, és kattintson **a SAML 2.0** fülre.

    ![Azure Active Directory Single Sign-On][24] 

1. Válassza a **időpontjához felhasználók:**.

    ![Azure Active Directory Single Sign-On][27] 


### <a name="assigning-the-azure-ad-test-user"></a>Az Azure Active Directory-próba felhasználói hozzárendelése

Ez a szakasz célja Britta Simon Azure egyszeri bejelentkezés a saját hozzáférést biztosít Litmos használandó engedélyezése.

![Felhasználó hozzárendelése][200] 

**Britta Simon hozzárendelése Litmos, hajtsa végre az alábbi lépéseket:**

1. Az Azure klasszikus portál kattintva nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson az **alkalmazások** parancsra a felső menüben.

    ![Felhasználó hozzárendelése][201] 

2. Az alkalmazások listájában jelölje ki a **Litmos**.

    ![Felhasználó hozzárendelése][202] 

1. A felső sávon kattintson a **felhasználók**elemre.

    ![Felhasználó hozzárendelése][203] 

1. A felhasználók listában jelölje ki a **Britta Simon**.

2. Az alsó eszköztáron kattintson a **hozzárendelni**.

    ![Felhasználó hozzárendelése][205]



### <a name="testing-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

Ez a szakasz célja a Azure Active Directory egyszeri bejelentkezéses beállítások tesztelése a Access panelen.  
Amikor az Access panel a Litmos mozaik gombra kattint, meg kell első automatikusan aláírt-on az Litmos alkalmazás.


## <a name="additional-resources"></a>További források

* [A szoftver alkalmazások integrálása az Azure Active Directory oktatóanyagok listája](active-directory-saas-tutorial-list.md)
* [Mi az access alkalmazás és az egyszeri bejelentkezés az Azure Active Directory címtárral?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_04.png
[5]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_01.png
[500]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_02.png

[6]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_05.png
[7]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_03.png
[8]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_04.png
[9]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_05.png
[10]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_07.png
[12]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_80.png
[13]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_81.png
[14]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_82.png
[15]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_81.png
[16]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_19.png
[17]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_67.png


[20]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_100.png
[21]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_60.png
[22]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_61.png
[23]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_62.png
[24]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_63.png
[25]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_64.png
[26]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_65.png
[27]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_66.png

[200]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_68.png
[203]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_205.png


[400]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_400.png
[401]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_401.png
[402]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_402.png





