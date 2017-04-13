<properties
    pageTitle="Oktatóprogram: Azure Active Directory-integráció a Questetra BPM csomagja |} A Microsoft Aure"
    description="Megtudhatja, hogy miként konfigurálható az egyszeri bejelentkezés Azure Active Directory és Questetra BPM csomagja között."
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
    ms.date="10/28/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-questetra-bpm-suite"></a>Oktatóprogram: Azure Active Directory-integráció a Questetra BPM programcsomagban

Ebben az oktatóanyagban célja mutatja, hogy miként Questetra BPM csomagja integrálása az Azure Active Directory (Azure Active Directory).  
Questetra BPM csomagja integrálása az Azure Active Directory biztosít a következő előnyökkel jár: 

- Az Azure Active Directory, aki rendelkezik hozzáféréssel a Questetra BPM programcsomagban megadhatja, hogy 
- Az Azure Active Directory-fiókok engedélyezheti a felhasználóknak, hogy automatikusan első jelentkezett-on a Questetra BPM programcsomagban (Single Sign-On)
- A fiókokat az egy központi helyen – az Azure klasszikus portálra

Ha többet szeretne tudni a szoftver alkalmazás integrálása az Azure AD meg, hogy, ismerje meg, [az access alkalmazás és az egyszeri bejelentkezés az Azure Active Directory címtárral](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek 

Azure Active Directory integráció Questetra BPM Suite van szükség az alábbiakat:

- Az Azure Active Directory-előfizetéssel
- Egy [Questetra BPM csomagja](https://senbon-imadegawa-988.questetra.net/) egyszeri bejelentkezés engedélyezett előfizetés


> [AZURE.NOTE] Az oktatóprogram lépéseit teszteléséhez nem használata ajánlott munkakörnyezetben.


Az oktatóprogram lépéseit teszteléséhez célszerű követnie az alábbi javaslatokat:

- Erre akkor szükség, ne használja a termelési környezetén.
- Ha nincs telepítve az Azure Active Directory próba környezetben, kattint egy hónap próba [Itt](https://azure.microsoft.com/pricing/free-trial/). 

 
## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban célja lehetővé teszi az Azure Active Directory egyszeri bejelentkezés tesztkörnyezetben tesztelése.  
Az alkalmazási példát, ebben az oktatóanyagban tagolt három fő építőelemek áll:

1. Questetra BPM csomagja hozzáadása a gyűjteményből 
2. Beállítása és tesztelése Azure Active Directory egyszeri bejelentkezés


## <a name="adding-questetra-bpm-suite-from-the-gallery"></a>Questetra BPM csomagja hozzáadása a gyűjteményből
Konfigurálja az Azure Active Directory integrálása a Questetra BPM csomagja, szüksége Questetra BPM csomagja hozzáadása felügyelt szoftver alkalmazásai a gyűjteményből.

**A gyűjteményből Questetra BPM csomagja felvételéhez hajtsa végre az alábbi lépéseket:**

1. Az **Azure klasszikus portálon**kattintson a bal oldali navigációs területen kattintson az **Active Directory**. 

    ![Az Active Directory][1]

2. A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3. Nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson a felső menüben **alkalmazásokat** .

    ![Alkalmazások][2]

4. Kattintson a **Hozzáadás** az oldal alján.

    ![Alkalmazások][3]

5. **Milyen feladatot szeretne tenni** párbeszédpanelen kattintson **a gyűjteményből az alkalmazások felvétele**.

    ![Alkalmazások][4]

6. A Keresés mezőbe írja be a **Questetra BPM csomagja**.

    ![Alkalmazások][5]

7. Az eredmény ablaktáblában jelölje ki a **Questetra BPM csomagja**, és válassza a **kész** , az alkalmazás hozzáadása gombra.



##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Beállítása és tesztelése Azure Active Directory egyszeri bejelentkezés
Ez a szakasz célja mutatja, hogy beállítása és Azure Active Directory egyszeri bejelentkezés Questetra BPM Suite "Britta Simon" nevű próba felhasználón alapuló tesztelése.

Az egyszeri bejelentkezési a munkát az Azure Active Directory kell, hogy mi az a partner felhasználó Questetra BPM csomagja az Azure Active Directory-felhasználónak. Más szóval hivatkozás kapcsolat Azure AD-felhasználó, és a kapcsolódó felhasználó Questetra BPM csomagja között kell létrehozni.  
A hivatkozás kapcsolat jön létre által az Azure Active Directory Questetra BPM csomagja **felhasználónév** értékként a **felhasználó neve** értéket rendeli.
 
Állítsa be, és Azure Active Directory egyszeri bejelentkezés Questetra BPM Suite tesztelése, a következő építőelemek szükséges:

1. **[Azure Active Directory konfigurálása az egyszeri bejelentkezés](#configuring-azure-ad-single-single-sign-on)** - engedélyezése a felhasználóknak, hogy ezzel a szolgáltatással.
2. **[Felhasználó létrehozása az Azure Active Directory tesztelése](#creating-an-azure-ad-test-user)** - Azure Active Directory egyszeri bejelentkezéssel való Britta Simon ellenőrzéséhez.
4. **[Felhasználó létrehozása a Questetra BPM öt tesztelése](#creating-a-questetra-bpm-suite-test-user)** - van-e egy megfelelője a Britta Simon Questetra BPM csomagja, amelyen az Azure Active Directory ábrázolt csatolt.
5. **[Az Azure Active Directory hozzárendelése tesztelje a felhasználó](#assigning-the-azure-ad-test-user)** - ahhoz, hogy Britta Simon Azure AD az egyszeri bejelentkezés használata.
5. **[Tesztelés egyszeri bejelentkezés](#testing-single-sign-on)** - ellenőrzéséhez, hogy működik-e a konfigurációs.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure Active Directory Single Sign-On konfigurálása

Ez a szakasz célja Azure AD az egyszeri bejelentkezés az Azure klasszikus portálon engedélyezése és a Questetra BPM csomagja alkalmazásban az egyszeri bejelentkezés beállítása.

**Állítson be Azure AD az egyszeri bejelentkezés Questetra BPM csomagja, hajtsa végre az alábbi lépéseket:**

1. Az Azure klasszikus portálon **Questetra BPM csomagja** alkalmazás integrációs lapon kattintson **konfigurálása az egyszeri bejelentkezés** a **Konfigurálása az egyszeri bejelentkezés** párbeszédpanel megnyitásához.

    ![Egyszeri bejelentkezés beállítása][8]

2. **Hogyan szeretné, hogy jelentkezzen be az Questetra BPM csomagja felhasználók** lapon válassza az **Azure Active Directory Single Sign-On**, és kattintson a **Tovább gombra**.

    ![Azure Active Directory Single Sign-On][9]


3. A különböző webes böngészőablakban jelentkezzen be a **Questetra BPM csomagja** vállalati webhely rendszergazdaként.

4. A felső sávon kattintson a **Rendszer beállításai**parancsra. 

    ![Azure Active Directory Single Sign-On][10]

5. A **SingleSignOnSAML** lap megnyitásához kattintson az **Egyszeri bejelentkezés (SAML)**. 

    ![Azure Active Directory Single Sign-On][11]


6. Az Azure klasszikus portálon a **Alkalmazás beállításainak megadása** párbeszédpanel lapon hajtsa végre az alábbi lépéseket: 

    ![Alkalmazás beállításainak konfigurálása][13]
 
    egy. A **Questetra BPM csomagja** vállalati webhely SP adatok csoportban, **ACS URL-cím**másolása, és illessze be a **Bejelentkezési a URL-címe** mezőben lévő értéket.

    b. A **Questetra BPM csomagja** vállalati webhely SP adatok csoportban, másolja a **Szervezet azonosítója**, és illessze be a **Kibocsátó URL-címe** mezőben lévő értéket.

    c billentyűkombinációt. A **Questetra BPM csomagja** vállalati webhely SP adatok csoportban, **ACS URL-cím**másolása, és illessze be a **Válasz URL-címe** mezőben lévő értéket.

    d. Kattintson a **Tovább**gombra.

 
7. **Konfigurálása az egyszeri bejelentkezés Questetra BPM csomagja a** lapon kattintson a **tanúsítvány letöltése**gombra, és mentse a tanúsítvány fájlt a számítógépen helyben.

    ![Egyszeri bejelentkezés beállítása][14]


8. A **Questetra BPM csomagja** vállalati webhely hajtsa végre az alábbi lépéseket: 

    ![Egyszeri bejelentkezés beállítása][15]

    egy. Jelölje be **az egyszeri bejelentkezés engedélyezése**.
     
    b. Az Azure klasszikus portálon a **Kibocsátó URL-címe** értéket másolja és illessze be a **Szervezet azonosítója** mezőben lévő értéket.

    c billentyűkombinációt. A klasszikus Azure portálon másolja az **Egyszeri bejelentkezéses szolgáltatás URL-címe** értéket, és illessze be a **bejelentkezési URL-címe** mezőben lévő értéket.

    d. A klasszikus Azure portálon másolja az **Egyetlen Sign-Out szolgáltatás URL-címe** értéket, és illessze be a **kijelentkezési lap URL-címe** mezőben lévő értéket.

    e. Az **NameID formátum** mezőbe írja be a **urn: oasis: nevek: tc: SAML:1.1:nameid-formátum: emailAddress**.


    f. A letöltött tanúsítvány alap-64 kódolt fájl létrehozása 

    >[AZURE.TIP] További részletekért megtudhatja, [hogy miként szövegfájlba bináris tanúsítvány konvertálja](http://youtu.be/PlgrzUZ-Y1o).

    g. Nyissa meg az alap-64 kódolt tanúsítvány a Jegyzettömbben, a tartalmát a vágólapra másolja és illessze be a **tanúsítvány érvényességi** mezőben lévő értéket. 

    h. Kattintson a **Mentés**gombra.


9. Az Azure klasszikus portálon jelölje be a egyszeri bejelentkezéses konfigurációs megerősítés, és kattintson a **Tovább gombra**. 

    ![Mi az Azure AD Connect][17]


10. Az **egyszeri bejelentkezés megerősítés** lapon kattintson a **Befejezés**gombra.  

    ![Mi az Azure AD Connect][18]




### <a name="creating-an-azure-ad-test-user"></a>Azure AD-próba felhasználó létrehozása
Ez a szakasz célja vizsgálat felhasználó létrehozása az Azure-Britta Simon nevű klasszikus portálon.

**Teszt felhasználó létrehozása az Azure Active Directory, hajtsa végre az alábbi lépéseket:**

1. Az **Azure klasszikus portálon**kattintson a bal oldali navigációs területen kattintson az **Active Directory**.

    ![Azure AD-próba felhasználó létrehozása][100] 

2. A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3. A menüben, a képernyő tetején, a felhasználók listájának megjelenítéséhez kattintson a **felhasználók**elemre.

    ![Azure AD-próba felhasználó létrehozása][101] 

4. **Felhasználó hozzáadása**elemre az alsó eszköztáron a **Felhasználó hozzáadása** párbeszédpanel megnyitásához. 

    ![Azure AD-próba felhasználó létrehozása][102] 

5. A párbeszédpanel **közli velünk a felhasználóra vonatkozó** lapon hajtsa végre az alábbi lépéseket:

    ![Azure AD-próba felhasználó létrehozása][103]
 
    egy. **Írja be a felhasználó**jelölje ki **a szervezet új felhasználót**.
  
    b. A felhasználónév **szövegmezőbe**írja be a **BrittaSimon**.

    c billentyűkombinációt. Kattintson a Tovább gombra.

6.  A **Felhasználói profil** párbeszédpanel lapon hajtsa végre az alábbi lépéseket: 

    ![Azure AD-próba felhasználó létrehozása][104] 
  
    egy. Az **első neve** mezőbe írja be a **Britta**. 
 
    b. Az **Utolsó neve** mezőbe írja be, **Simon**.

    c billentyűkombinációt. A **Megjelenítendő név** mezőbe írja be a **Britta Simon**.

    d. A **szerepkör** listában jelölje ki a **felhasználót**.

    e. Kattintson a **Tovább**gombra.

7. A párbeszédpanel **ideiglenes jelszót kérjen** lapon kattintson a **Hozzon létre**.

    ![Azure AD-próba felhasználó létrehozása][105]  

8. A párbeszédpanel **ideiglenes jelszót első** lapon hajtsa végre az alábbi lépéseket:

    ![Azure AD-próba felhasználó létrehozása][106]   
  
    egy. Jegyezze fel az **Új jelszót**az érték.
  
    b. Kattintson a **kész**gombra.   
  
 
### <a name="creating-a-questetra-bpm-suite-test-user"></a>Questetra BPM csomagja vizsgálat felhasználó létrehozása

Ez a szakasz célja Britta Simon nevű Questetra BPM csomagja felhasználó létrehozása.

**A felhasználó Britta Simon nevű Questetra BPM csomagja létrehozásához tegye a következőket:**

1.  Bejelentkezés a Questetra BPM csomagja vállalati webhelyre rendszergazdaként.
2.  Nyissa meg a **Rendszerbeállítások > felhasználó lista > Új felhasználó**. 
3.  Az új felhasználó párbeszédpanelen hajtsa végre az alábbi lépéseket: 

    ![Próba-felhasználó létrehozása][300] 

    egy. Az **neve** mezőbe írja be az Azure Active Directory Britta-féle felhasználónév.

    b. Az **E-mail** mezőbe írja be az Azure Active Directory Britta-féle felhasználónév.

    c billentyűkombinációt. Írja be egy jelszót a **jelszó** mezőbe.

4.  Kattintson az **Új felhasználó hozzáadása**gombra.



### <a name="assigning-the-azure-ad-test-user"></a>Az Azure Active Directory-próba felhasználói hozzárendelése

Ez a szakasz célja Britta Simon Azure egyszeri bejelentkezés a saját hozzáférés engedélyezése a Questetra BPM programcsomagban használandó engedélyezése.

![Mi az Azure AD Connect][200]

**Britta Simon hozzárendelése Questetra BPM csomagja, hajtsa végre az alábbi lépéseket:**

1. Az Azure klasszikus portál kattintva nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson az **alkalmazások** parancsra a felső menüben.

    ![Mi az Azure AD Connect][201]

2. Az alkalmazások listájában jelölje ki a **Questetra BPM csomagot**.

    ![Mi az Azure AD Connect][205]

1. A felső sávon kattintson a **felhasználók**elemre.

    ![Mi az Azure AD Connect][202]

1. A felhasználók listában jelölje ki a **Britta Simon**.

    ![Mi az Azure AD Connect][203]

2. Az alsó eszköztáron kattintson a **hozzárendelni**.

    ![Mi az Azure AD Connect][204]



### <a name="testing-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

Ez a szakasz célja a Azure Active Directory egyszeri bejelentkezéses beállítások tesztelése a Access panelen.  
Ha az Access panel a Questetra BPM csomagja mozaik gombra kattint, meg kell első automatikusan aláírt-on az Questetra BPM csomagja alkalmazás.


## <a name="additional-resources"></a>További források

* [A szoftver alkalmazások integrálása az Azure Active Directory oktatóanyagok listája](active-directory-saas-tutorial-list.md)
* [Mi az access alkalmazás és az egyszeri bejelentkezés az Azure Active Directory címtárral?](active-directory-appssoaccess-whatis.md)

<!--Image references-->
[1]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_01.png
[2]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_02.png
[3]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_03.png
[4]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_04.png
[5]: ./media/active-directory-saas-questetra-bpm-suite/questera_bpm_suite_01.png


[8]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_06.png
[9]: ./media/active-directory-saas-questetra-bpm-suite/questera_bpm_suite_02.png
[10]: ./media/active-directory-saas-questetra-bpm-suite/questera_bpm_suite_03.png
[11]: ./media/active-directory-saas-questetra-bpm-suite/questera_bpm_suite_04.png
[12]: ./media/active-directory-saas-questetra-bpm-suite/questera_bpm_suite_05.png
[13]: ./media/active-directory-saas-questetra-bpm-suite/questera_bpm_suite_06.png
[14]: ./media/active-directory-saas-questetra-bpm-suite/questera_bpm_suite_07.png
[15]: ./media/active-directory-saas-questetra-bpm-suite/questera_bpm_suite_08.png
[16]: ./media/active-directory-saas-questetra-bpm-suite/questera_bpm_suite_09.png
[17]: ./media/active-directory-saas-questetra-bpm-suite/questera_bpm_suite_10.png
[18]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_08.png


[100]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_09.png 
[101]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_10.png 
[102]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_11.png 
[103]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_12.png 
[104]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_13.png 
[105]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_14.png 
[106]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_15.png 


[200]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_16.png 
[201]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_17.png 
[202]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_18.png
[203]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_19.png
[204]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_20.png
[205]: ./media/active-directory-saas-questetra-bpm-suite/questera_bpm_suite_12.png

[300]: ./media/active-directory-saas-questetra-bpm-suite/questera_bpm_suite_11.png 