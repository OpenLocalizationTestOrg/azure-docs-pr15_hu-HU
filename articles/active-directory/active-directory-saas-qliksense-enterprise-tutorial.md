<properties
    pageTitle="Oktatóprogram: Azure Active Directory-integráció a Qlik értelemben vállalati |} Microsoft Azure"
    description="Megtudhatja, hogy miként konfigurálható az egyszeri bejelentkezés Azure Active Directory és a Qlik értelemben nagyvállalati verzió között."
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
    ms.date="08/31/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-qlik-sense-enterprise"></a>Oktatóprogram: Azure Active Directory-integráció a Qlik értelemben vállalati

Ebből az oktatóanyagból megismerheti, hogyan Qlik értelemben vállalati integrálása az Azure Active Directory (Azure Active Directory).

Qlik értelemben vállalati integrálása az Azure Active Directory biztosít a következő előnyökkel jár:

- Az Azure Active Directory, aki rendelkezik hozzáféréssel Qlik értelemben vállalati megadhatja, hogy
- Az Azure Active Directory-fiókok engedélyezheti a felhasználóknak, hogy automatikusan első jelentkezett-on (Single Sign-On) Qlik értelemben nagyvállalati verzióra
- A fiókokat az egy központi helyen – az Azure klasszikus portálra

Ha többet szeretne tudni a szoftver alkalmazás integrálása az Azure AD meg, hogy, ismerje meg, [az access alkalmazás és az egyszeri bejelentkezés az Azure Active Directory címtárral](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek

Azure Active Directory integráció Qlik értelemben Enterprise van szükség az alábbiakat:

- Az Azure Active Directory-előfizetéssel
- Egy Qlik értelemben vállalati egyszeri bejelentkezés engedélyezett előfizetésekből


> [AZURE.NOTE] Az oktatóprogram lépéseit teszteléséhez nem használata ajánlott munkakörnyezetben.


Az oktatóprogram lépéseit teszteléséhez célszerű követnie az alábbi javaslatokat:

- Erre akkor szükség, ne használja a termelési környezetén.
- Ha nincs telepítve az Azure Active Directory próba környezetben, kattint egy hónap próba [Itt](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban tesztelni Azure AD az egyszeri bejelentkezés tesztkörnyezetben.

Az alkalmazási példát, ebben az oktatóanyagban tagolt két fő építőelemek áll:

1. Qlik értelemben vállalati hozzáadása a gyűjteményből
2. Beállítása és tesztelése Azure Active Directory egyszeri bejelentkezés


## <a name="adding-qlik-sense-enterprise-from-the-gallery"></a>Qlik értelemben vállalati hozzáadása a gyűjteményből
Konfigurálja az Azure Active Directory integrálása a Qlik értelemben vállalati, meg kell Qlik értelemben vállalati felügyelt szoftver alkalmazásai a gyűjteményből hozzáadása.

**A gyűjteményből Qlik értelemben vállalati felvételéhez hajtsa végre az alábbi lépéseket:**

1. Az **Azure klasszikus portálon**kattintson a bal oldali navigációs területen kattintson az **Active Directory**.

    ![Az Active Directory][1]
2. A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3. Nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson a felső menüben **alkalmazások** .

    ![Alkalmazások][2]

4. Kattintson a **Hozzáadás** az oldal alján.

    ![Alkalmazások][3]

5. **Milyen feladatot szeretne tenni** párbeszédpanelen kattintson **a gyűjteményből az alkalmazások felvétele**.

    ![Alkalmazások][4]

6. A Keresés mezőbe írja be a **Qlik értelemben vállalati**.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_01.png)

7. Az eredmény ablaktáblában jelölje ki a **Qlik értelemben vállalati**pontra, és kattintson a **kész** az alkalmazás hozzáadása parancsra.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Beállítása és tesztelése Azure Active Directory egyszeri bejelentkezés
Ebben a részben, beállítása és tesztelése Azure Active Directory egyszeri bejelentkezés a Qlik értelemben Enterprise "Britta Simon" nevű próba felhasználón alapuló.

Az egyszeri bejelentkezési a munkát az Azure Active Directory kell tudható, hogy mi a Qlik értelemben vállalati megfelelőjük a felhasználó egy felhasználóhoz, az Azure Active Directory. Más szóval Azure AD-felhasználó, és a kapcsolódó felhasználó Qlik értelemben vállalaton belüli hivatkozás viszonya kell létrehozni.

A hivatkozás kapcsolat jön létre által az Azure Active Directory Qlik értelemben vállalati **felhasználónév** értékként a **felhasználó neve** értéket rendeli.

Állítsa be, és Azure Active Directory egyszeri bejelentkezés Qlik értelemben Enterprise tesztelése, a következő építőelemek szükséges:

1. **[Azure Active Directory konfigurálása az egyszeri bejelentkezés](#configuring-azure-ad-single-sign-on)** - engedélyezése a felhasználóknak, hogy ezzel a szolgáltatással.
2. **[Felhasználó létrehozása az Azure Active Directory tesztelése](#creating-an-azure-ad-test-user)** - Azure Active Directory egyszeri bejelentkezéssel való Britta Simon ellenőrzéséhez.
3. **[Felhasználó létrehozása egy Qlik értelemben vállalati tesztelése](#creating-a-qliksense-enterprise-test-user)** - szeretné, hogy egy partner a Britta Simon Qlik értelemben vállalati, amelyen az Azure Active Directory ábrázolt csatolt.
4. **[Az Azure Active Directory hozzárendelése tesztelje a felhasználó](#assigning-the-azure-ad-test-user)** - ahhoz, hogy Britta Simon Azure AD az egyszeri bejelentkezés használata.
5. **[Tesztelés egyszeri bejelentkezés](#testing-single-sign-on)** - ellenőrzéséhez, hogy működik-e a konfigurációs.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure Active Directory egyszeri bejelentkezés beállítása

Ebben a részben Azure AD az egyszeri bejelentkezés a klasszikus portálon engedélyezése, és a Qlik értelemben vállalati alkalmazás egyszeri bejelentkezés beállítása.


**Azure Active Directory egyszeri bejelentkezés beállítása a Qlik értelemben Enterprise, hajtsa végre az alábbi lépéseket:**

1. A klasszikus portálon **Qlik értelemben vállalati** alkalmazás integrációs lapon kattintson **konfigurálása az egyszeri bejelentkezés** a **Konfigurálása az egyszeri bejelentkezés** párbeszédpanel megnyitásához.
     
    ![Egyszeri bejelentkezés beállítása][6] 

2. **Hogyan szeretné, hogy jelentkezzen be az Qlik értelemben vállalati felhasználók** lapon válassza az **Azure Active Directory Single Sign-On**, és kattintson a **Tovább gombra**.

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_03.png) 

3. Az **Alkalmazás beállításainak megadása** párbeszédpanel lapon hajtsa végre az alábbi lépéseket:

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_04.png) 

    egy. **Bejelentkezési a URL-cím** mezőbe írja be a bejelentkezés az Qlik értelemben vállalati alkalmazás a következő mintát használatával a felhasználók által használt URL-címe: **https://\<Qlik értelemben teljesen Qualifed hostname (állomásnév)\>: 443-as / < virtuális Proxy előtag\>/samlauthn/**.
    
    > [AZURE.NOTE] Megjegyzés: a ferde e URI végén.  A szükség.

    b. Kattintson a **Tovább** gombra
 
4. A **beállítás az egyszeri bejelentkezés Qlik értelemben vállalati a** lapon hajtsa végre az alábbi lépéseket:

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_05.png)

    egy. Kattintson a **metaadat-alapú letöltése**, és mentse a fájlt a számítógépen.  Készüljön fel szerkesztése a metaadat-fájl feltöltése a Qlik értelemben kiszolgálóval előtt.

    b. Kattintson a **Tovább**gombra.

5. Felkészülés az összevonási metaadatok XML-fájlt, hogy tud feltölteni, amely Qlik értelemben-kiszolgálóhoz.

    > [AZURE.NOTE] Előtt a IdP metaadatok feltöltése a Qlik értelemben kiszolgáló, a fájl kell eltávolítani a megfelelő működéshez Azure Active Directory közötti adatokat szerkeszthetők és Qlik értelemben kiszolgáló.

    ![QlikSense][qs24]

    egy. Nyissa meg a letöltött Azure szövegszerkesztőben FederationMetaData.xml fájlt.

    b. Keresse meg a **RoleDescriptor**értékét.  Négy bejegyzés (nyitó és záró címkék elem két pár) lesz.

    c billentyűkombinációt. Törölje a RoleDescriptor címkéket és az összes információ a kettő között a fájlt.

    d. Mentse a fájlt, és importálható megőrzése a dokumentum későbbi használatra.

6. Nyissa meg a a Qlik értelemben Qlik Management Console (QMC) hozhat létre virtuális proxy konfigurációk felhasználóként.

7. A QMC kattintson a virtuális Proxy parancsot.

    ![QlikSense][qs6] 

8. A képernyő alján kattintson a létrehozás új gombra.

    ![QlikSense][qs7]

9. A virtuális proxy Szerkesztés képernyő jelenik meg.  A képernyő jobb oldalán található számára láthatóvá tétele a beállítások menüt.

    ![QlikSense][qs9]

10. Ellenőrizni azonosító menü beállítást választja írja be az Azure virtuális proxy konfigurációjának azonosító adatait.

    ![QlikSense][qs8]  
    
    egy. A Leírás mező egy rövid nevet, a virtuális proxy konfiguráció is.  Megad egy értéket a leírását.
    
    b. Az előtag mező azonosítja a virtuális proxy végpont Qlik értelemben az Azure Active Directory egyszeri bejelentkezéssel való csatlakozáshoz.  Írja be a virtuális proxybeállításait egyedi előtag nevét.

    c billentyűkombinációt. Munkamenet tétlenség időtúllépése (perc) a virtuális proxyn keresztül kapcsolatok időtúllépés.

    d. A munkamenet cookie-k táblázatfejlécek nevét a cookie-k nevét a munkamenet-azonosító tárolja a felhasználó megkapja a sikeres hitelesítés után Qlik értelemben munkamenet.  Ez a név egyedinek kell lennie.

11. Kattintson a hitelesítés menüpont meg szeretné jeleníteni.  A hitelesítés képernyő jelenik meg.

    ![QlikSense][qs10]

    egy. A **Névtelen hozzáférés mód** legördülő határozza meg, ha névtelen felhasználók férnek Qlik ilyesmire lehetőség a virtuális proxyn keresztül.  Az alapértelmezett beállítás a névtelen senki nem.

    b. A **hitelesítési módszer** legördülő határozza meg, hogy a virtuális proxy hitelesítési séma fogja használni.  A legördülő listában jelölje ki a SAML.  További lehetőségek eredményt fog megjelenni.

    c billentyűkombinációt. A **SAML host URI mező**adja meg a hostname (állomásnév) felhasználók adja meg a SAML virtuális proxyn keresztül Qlik értelemben eléréséhez.  A hostname (állomásnév) az uri Qlik értelemben kiszolgáló fut.

    d. Írja be a **SAML entitás azonosítója**, ugyanazt a SAML host URI mezőben megadott érték.

    e. A **SAML IdP metaadatok** egy korábban szerkesztett **Összevonási metaadatok szerkesztése az Azure Active Directory-konfigurációs** szakaszában fájl.  **A IdP metaadatok feltöltése előtt a fájl kell szerkeszthetők** eltávolíthatja a megfelelő működéshez közötti Azure Active Directory és Qlik értelemben kiszolgáló.  **Olvassa el a fenti utasításokat, ha a fájlt még nem szerkeszthetők.**  Ha a szerkesztette a fájlt a Tallózás gombra, majd jelölje ki a szerkesztett metaadat-fájlt, töltse fel a virtuális proxy konfigurációjának parancsára.

    f. Adja meg az attribútum nevét, vagy séma részletes ismertetése a SAML attribútum, amely az Azure Active Directory **felhasználóazonosító** küld a Qlik értelemben kiszolgálóra.  Séma hivatkozás adatait az Azure alkalmazás képernyőkön bejegyzés beállítás érhető el.  A név attribútum, **Írja be a http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name**használni.

    g. Ha az azok Qlik értelemben kiszolgálón keresztül Azure Active Directory hitelesítő felhasználók csatolt **felhasználói** az értéket kell megadni.  Szoftveresen kötött értékek **szögletes zárójelek []**táblanevet kell tenni.  Az Azure Active Directory SAML állítás küldött attribútum használatához a név megadása az attribútum a szöveg mezőben **nélkül** szögletes zárójelek között

    h. A **SAML aláírási algoritmus** a service provider (ebben az esetben Qlik értelemben kiszolgáló) tanúsítvány-aláírási a virtuális proxy konfiguráció állítja be.  Ha Qlik értelemben kiszolgáló használja a Microsoft fokozott RSA és AES Cryptographic Provider használatával létrehozott megbízható tanúsítvány, módosítsa a SAML aláírási algoritmus **SHA-256**.

    lehet. A SAML attribútum hozzárendelési szakaszban lehetővé teszi például csoportok további attribútumokat el lehet küldeni a Qlik ilyesmire lehetőség a biztonsági szabályok való használatra.

12. Kattintson a terheléselosztási menüpont meg szeretné jeleníteni.  A terheléselosztás képernyő jelenik meg.

    ![QlikSense][qs11]

13. Kattintson a Hozzáadás új kiszolgáló csomópont gombra, válassza motor csomópontot, vagy csomópontokat Qlik értelemben tanfolyamainkat, a terheléselosztás célra küldeni, és kattintson a Hozzáadás gombra.

    ![QlikSense][qs12]

14. Kattintson a speciális menüpont meg szeretné jeleníteni. A Speciális képernyő jelenik meg.

    ![QlikSense][qs13]

    egy. A Host fehér listát, amely a Qlik értelemben kiszolgálóval csatlakozáskor elfogadott állomásnevekké azonosítja.  **Írja be a hostname (állomásnév) felhasználók Qlik értelemben kiszolgálóhoz való csatlakozáskor határozza meg.** A hostname (állomásnév) érték, a SAML host uri a https:// nélkül.

15. Kattintson az Alkalmaz gombra.

    ![QlikSense][qs14]

16. Kattintson az OK gombra koppintva fogadja el a figyelmeztető üzenet, amely arról tájékoztat, hogy a virtuális proxy csatolva proxyk újra kell indítani.

    ![QlikSense][qs15]

17. A képernyő jobb oldalán megjelenik a társított elemek menü.  Kattintson a proxyk menüpontot.

    ![QlikSense][qs16]

18. A proxy képernyőn jelenik meg.  Kattintson a képernyő alján a proxy csatolni virtuális proxy a hivatkozás gombra.

    ![QlikSense][qs17]

19. Jelölje ki a proxy csomópontot, amelyek támogatják ezt a virtuális proxy kapcsolatot, és kattintson a hivatkozás gombra.  Csatolása, után a proxy látható társított proxyk csoportban.

    ![QlikSense][qs18]
    ![QlikSense][qs19]

20. Körülbelül 5 és 10 másodperc után a frissítése QMC üzenet jelenik meg.  Kattintson a frissítés QMC gombra.

    ![QlikSense][qs20]

21. Amikor frissíti a QMC, kattintson a virtuális proxyk parancsot. Az új bejegyzés a SAML virtuális proxy képernyőn a táblázatban felsorolt.  A virtuális proxy tétel egyetlen kattintással.

    ![QlikSense][qs51]

22. A képernyő alján aktiválja a SP töltse le a metaadatok gombra.  Kattintson a fájl mentése a metaadat-alapú letöltése SP-metaadat-alapú gombra.

    ![QlikSense][qs52]

23. Nyissa meg a sp metaadat-fájlt.  Tekintse át a **entityid beállítást** és a **AssertionConsumerService** tételt.  Ezeket az értékeket az **azonosító** és a **bejelentkezési URL-CÍMÉT** az Azure Active Directory-alkalmazás konfigurációjának megfelelői. Ha nem egyeznek, kell le őket az Azure Active Directory-alkalmazás Fiókkonfiguráló varázslóban.

    ![QlikSense][qs53]

24. A klasszikus portálon jelölje be a egyszeri bejelentkezéses konfigurációs megerősítés, és kattintson a **Tovább gombra**.
    
    ![Azure Active Directory Single Sign-On][10]

25. Az **egyszeri bejelentkezés megerősítés** lapon kattintson a **Befejezés**gombra.  
 
    ![Azure Active Directory Single Sign-On][11]


### <a name="creating-an-azure-ad-test-user"></a>Azure AD-próba felhasználó létrehozása
Ebben a szakaszban a klasszikus portálon Britta Simon nevű próba felhasználó létrehozása.


![Azure Active Directory-felhasználó létrehozása][20]

**Teszt felhasználó létrehozása az Azure Active Directory, hajtsa végre az alábbi lépéseket:**

1. Az **Azure klasszikus portálon**kattintson a bal oldali navigációs területen kattintson az **Active Directory**.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-qliksense-enterprise-tutorial/create_aaduser_09.png) 

2. A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3. A menüben, a képernyő tetején, a felhasználók listájának megjelenítéséhez kattintson a **felhasználók**elemre.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-qliksense-enterprise-tutorial/create_aaduser_03.png) 

4. **Felhasználó hozzáadása**elemre az alsó eszköztáron a **Felhasználó hozzáadása** párbeszédpanel megnyitásához.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-qliksense-enterprise-tutorial/create_aaduser_04.png) 

5. A **mondja el a felhasználóra vonatkozó** párbeszédpanel lapon hajtsa végre az alábbi lépéseket:  ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-qliksense-enterprise-tutorial/create_aaduser_05.png) 

    egy. Felhasználó típusa jelölje be az új felhasználó a szervezet.

    b. A felhasználónév **szövegmezőbe**írja be a **BrittaSimon**.

    c billentyűkombinációt. Kattintson a **Tovább**gombra.

6.  A **Felhasználói profil** párbeszédpanel lapon hajtsa végre az alábbi lépéseket: ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-qliksense-enterprise-tutorial/create_aaduser_06.png) 

    egy. Az **első neve** mezőbe írja be a **Britta**.  

    b. Az **Utolsó neve** mezőbe írja be, **Simon**.

    c billentyűkombinációt. A **Megjelenítendő név** mezőbe írja be a **Britta Simon**.

    d. A **szerepkör** listában jelölje ki a **felhasználót**.

    e. Kattintson a **Tovább**gombra.

7. A párbeszédpanel **ideiglenes jelszót kérjen** lapon kattintson a **Hozzon létre**.

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-qliksense-enterprise-tutorial/create_aaduser_07.png) 

8. A párbeszédpanel **ideiglenes jelszót első** lapon hajtsa végre az alábbi lépéseket:

    ![Azure AD-próba felhasználó létrehozása](./media/active-directory-saas-qliksense-enterprise-tutorial/create_aaduser_08.png) 

    egy. Jegyezze fel az **Új jelszót**az érték.

    b. Kattintson a **kész**gombra.   


### <a name="creating-an-qlik-sense-enterprise-test-user"></a>Egy Qlik értelemben vállalati teszt felhasználó létrehozása

Ebben a szakaszban a Britta Simon nevű Qlik értelemben vállalati felhasználó létrehozása. Kérje a felhasználók hozzáadása a Qlik értelemben vállalati platform Qlik értelemben vállalati részlegtől.


### <a name="assigning-the-azure-ad-test-user"></a>Az Azure Active Directory-próba felhasználói hozzárendelése

Ebben a részben Britta Simon Azure egyszeri bejelentkezés a saját hozzáférést biztosít Qlik értelemben vállalati használandó engedélyezése.

![Felhasználó hozzárendelése][200] 

**Britta Simon hozzárendelése Qlik értelemben vállalati, hajtsa végre az alábbi lépéseket:**

1. A klasszikus portál kattintva nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson az **alkalmazások** parancsra a felső menüben.

    ![Felhasználó hozzárendelése][201] 

2. Az alkalmazások listájában jelölje ki a **Qlik értelemben vállalati**.

    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_50.png) 

3. A felső sávon kattintson a **felhasználók**elemre.

    ![Felhasználó hozzárendelése][203]

4. A felhasználók listában jelölje ki a **Britta Simon**.

5. Az alsó eszköztáron kattintson a **hozzárendelni**.

    ![Felhasználó hozzárendelése][205]


## <a name="testing-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

Ebben a részben tesztelje az Azure Active Directory egyetlen bejelentkezés beállítási a Access panelen.

Ha az Access panel a Qlik értelemben vállalati mozaik gombra kattint, meg kell első automatikusan aláírt-on az Qlik értelemben vállalati alkalmazás.


## <a name="additional-resources"></a>További források

* [A szoftver alkalmazások integrálása az Azure Active Directory oktatóanyagok listája](active-directory-saas-tutorial-list.md)
* [Mi az access alkalmazás és az egyszeri bejelentkezés az Azure Active Directory címtárral?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_205.png

[qs6]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_06.png
[qs7]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_07.png
[qs8]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_08.png
[qs9]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_09.png
[qs10]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_10.png
[qs11]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_11.png
[qs12]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_12.png
[qs13]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_13.png
[qs14]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_14.png
[qs15]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_15.png
[qs16]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_16.png
[qs17]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_17.png
[qs18]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_18.png
[qs19]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_19.png
[qs20]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_20.png
[qs21]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_21.png
[qs22]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_22.png
[qs23]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_23.png
[qs24]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_24.png
[qs25]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_25.png
[qs26]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_26.png
[qs51]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_51.png
[qs52]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_52.png
[qs53]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_53.png