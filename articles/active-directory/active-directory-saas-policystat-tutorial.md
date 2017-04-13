<properties 
    pageTitle="Oktatóprogram: Azure Active Directory-integráció a PolicyStat |} Microsoft Azure" 
    description="Megtudhatja, hogyan használhatja a PolicyStat az Azure Active Directory ahhoz, hogy az egyszeri bejelentkezés, automatikus kiépítési és az egyéb!" 
    services="active-directory" 
    authors="jeevansd"  
    documentationCenter="na" 
    manager="femila"/>
<tags 
    ms.service="active-directory" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="identity" 
    ms.date="09/26/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-policystat"></a>Oktatóprogram: Azure Active Directory-integráció a PolicyStat
  
Ebben az oktatóanyagban célja integrálása az Azure és PolicyStat megjelenítéséhez.  
Az alkalmazási példát, ebben az oktatóanyagban tagolt feltételezi, hogy már rendelkezik az alábbiakat:

-   Érvényes Azure előfizetés
-   PolicyStat bérlői webhelyre
  
Ebben az oktatóanyagban befejeztével az Azure Active Directory-felhasználók PolicyStat rendelt fogja tudni az alkalmazás a PolicyStat vállalati webhely (service provider által kezdeményezett bejelentkezés) és használatáról a [Bevezetés az Access panelen](active-directory-saas-access-panel-introduction.md)az egyszeri bejelentkezési.
  
A következő építőelemek az alkalmazási példát, ebben az oktatóanyagban tagolt áll:

1.  Az alkalmazás integrációját PolicyStat engedélyezése
2.  Egyszeri bejelentkezés beállítása
3.  Felhasználói kiépítési konfigurálása
4.  Felhasználók hozzárendelése

![Eset] (./media/active-directory-saas-policystat-tutorial/IC808662.png "Eset")
##<a name="enabling-the-application-integration-for-policystat"></a>Az alkalmazás integrációját PolicyStat engedélyezése
  
Ez a szakasz célja, hogyan PolicyStat az alkalmazás alkalmazással való integráció engedélyezése a szerkezet.

###<a name="to-enable-the-application-integration-for-policystat-perform-the-following-steps"></a>Ahhoz, hogy az alkalmazás integrációját PolicyStat, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon kattintson a bal oldali navigációs területen kattintson az **Active Directory**.

    ![Az Active Directory] (./media/active-directory-saas-policystat-tutorial/IC700993.png "Az Active Directory")

2.  A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3.  Nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson a felső menüben **alkalmazások** .

    ![Alkalmazások] (./media/active-directory-saas-policystat-tutorial/IC700994.png "Alkalmazások")

4.  Kattintson a **Hozzáadás** az oldal alján.

    ![Alkalmazás hozzáadása] (./media/active-directory-saas-policystat-tutorial/IC749321.png "Alkalmazás hozzáadása")

5.  **Milyen feladatot szeretne tenni** párbeszédpanelen kattintson **a gyűjteményből az alkalmazások felvétele**.

    ![Gallerry az alkalmazás hozzáadása] (./media/active-directory-saas-policystat-tutorial/IC749322.png "Gallerry az alkalmazás hozzáadása")

6.  A **Keresés mezőbe**írja be a **PolicyStat**.

    ![Alkalmazás gyűjtemény] (./media/active-directory-saas-policystat-tutorial/IC808627.png "Alkalmazás gyűjtemény")

7.  Az eredmény ablaktáblában jelölje ki a **PolicyStat**, és válassza a **kész** , az alkalmazás hozzáadása gombra.

    ![PolicyStat] (./media/active-directory-saas-policystat-tutorial/IC810430.png "PolicyStat")
##<a name="configuring-single-sign-on"></a>Egyszeri bejelentkezés beállítása
  
Ez a szakasz célja tagolása engedélyezése a felhasználóknak, hogy PolicyStat hitelesíteni a fiókkal az összevonási alapján a SAML protokoll használatával Azure AD.  
A PolicyStat alkalmazása egy bizonyos formátumban, amelyhez egyéni attribútum megfeleltetések hozzáadása a **saml jogkivonat attribútumok** konfigurációs várja meg a SAML előfeltételek.  
Az alábbi képernyőképen látható, a.

![Attribútumok] (./media/active-directory-saas-policystat-tutorial/IC808628.png "Attribútumok")

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Egyszeri bejelentkezés beállításához hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon **PolicyStat** alkalmazás integrációs lapon kattintson **konfigurálása az egyszeri bejelentkezés** a **Konfigurálása az egyszeri bejelentkezés** párbeszédpanel megnyitásához.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-policystat-tutorial/IC808629.png "Egyszeri bejelentkezés beállítása")

2.  **Hogyan szeretné, hogy jelentkezzen be az PolicyStat felhasználók** lapon jelölje be **A Microsoft Azure Active Directory Single Sign-On**, és kattintson a **Tovább gombra**.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-policystat-tutorial/IC808630.png "Egyszeri bejelentkezés beállítása")

3.  Az **Alkalmazás-beállítások megadása** lap **Bejelentkezési a URL-cím** mezőbe írja be a felhasználóknak, hogy a bejelentkezés az URL-cím PolicyStat alkalmazás által használt URL-CÍMÉT (például: *"https://demo-azure.policystat.com"*), majd kattintson a **Tovább**gombra.

    ![Alkalmazás beállításainak konfigurálása] (./media/active-directory-saas-policystat-tutorial/IC808631.png "Alkalmazás beállításainak konfigurálása")

4.  **Konfigurálása az egyszeri bejelentkezés a PolicyStat** lapon kattintson a **metaadat-alapú letöltése**elemre, és mentse a fájlt a számítógépen a metaadat.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-policystat-tutorial/IC808632.png "Egyszeri bejelentkezés beállítása")

5.  A különböző webes böngészőablakban jelentkezzen be a PolicyStat vállalati webhely rendszergazdaként.

6.  Kattintson a **rendszergazda** fülre, és válassza a bal oldali navigációs ablaktáblában **Egyszeri bejelentkezéses konfigurációs** .

    ![Rendszergazdai menü] (./media/active-directory-saas-policystat-tutorial/IC808633.png "Rendszergazdai menü")

7.  A **telepítő** csoportban jelölje be a **engedélyezése egyszeri bejelentkezés integrációja**.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-policystat-tutorial/IC808634.png "Egyszeri bejelentkezés beállítása")

8.  **Attribútumok beállítása**gombra, és ezután **Attribútumok konfigurálása** csoportban tegye a következőket:

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-policystat-tutorial/IC808635.png "Egyszeri bejelentkezés beállítása")

    1.  Az **Attribútum a felhasználónév** mezőbe írja be a **uid**.
    2.  Az **Utónév attribútum** mezőbe írja be az **Utónév**.
    3.  Az **Utolsó név attribútum** mezőbe írja be az **Utónév**.
    4.  Az **E-mailek attribútum** mezőbe írja be a **EmailCím**.
    5.  A **módosítások mentése**gombra.

9.  Kattintson a **Your IDP metaadatok**, és ezt követően **A IDP metaadatok** csoportban tegye a következőket:

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-policystat-tutorial/IC808635.png "Egyszeri bejelentkezés beállítása")

    1.  Nyissa meg a letöltött metaadat-fájlt, a tartalom másolása és beillesztése **Az identitás szolgáltató metaadatok** szöveg
    2.  A **módosítások mentése**gombra.

10. Az Azure klasszikus portálon jelölje be a egyszeri bejelentkezéses konfigurációs megerősítés, és válassza a **kész** , **Állítsa be az egyszeri bejelentkezés** párbeszédpanel bezárásához.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-policystat-tutorial/IC771723.png "Egyszeri bejelentkezés beállítása")

11. 12. Kattintson a menü felső **attribútumok** a **SAML jogkivonat attribútumok** párbeszédpanel megnyitásához.

    ![Attribútumok] (./media/active-directory-saas-policystat-tutorial/IC795920.png "Attribútumok")

13. Kötelező attribútum megfeleltetések felvenni, hajtsa végre az alábbi lépéseket:

    ![Attribútumok] (./media/active-directory-saas-policystat-tutorial/IC804823.png "Attribútumok")

    1.  Kattintson a **felhasználó attribútum hozzáadása**gombra.
    2.  Az **Attribútum neve** mezőbe írja be a **uid**.
    3.  Az **Attribútumérték** mezőbe **ExtractMailPrefix()**kijelölése
    4.  A **levelek** listából válassza ki a **User.mail**.
    5.  Kattintson a **kész**gombra.
##<a name="configuring-user-provisioning"></a>Felhasználói kiépítési konfigurálása
  
Ahhoz, hogy az Azure Active Directory-felhasználóknak, hogy jelentkezzen be a PolicyStat, hogy ki kell építenie PolicyStat be.  
PolicyStat csak az idő felhasználói kiépítési támogatja. Ez azt jelenti, akkor nem kell a felhasználók hozzáadása kézzel PolicyStat.  
A felhasználók fog első bejegyezte automatikusan meg az első bejelentkezéskor egyetlen regisztrációnak.

>[AZURE.NOTE]Bármely más PolicyStat felhasználói fiók létrehozása eszközöket is használhatja, illetve PolicyStat rendelkezést AAD felhasználói fiókok által biztosított API-khoz.

##<a name="assigning-users"></a>Felhasználók hozzárendelése
  
Tesztelje a konfigurációt, meg kell adni az Azure Active Directory-felhasználók, használja az alkalmazás elérési útvonalát társításával engedélyezni szeretné.

###<a name="to-assign-users-to-policystat-perform-the-following-steps"></a>Felhasználók hozzárendelése PolicyStat, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon próba-fiók létrehozása.

2.  Kattintson a **PolicyStat **alkalmazás integrációs lap **hozzárendelését a felhasználókhoz**.

    ![Hozzárendelését a felhasználókhoz] (./media/active-directory-saas-policystat-tutorial/IC808636.png "Adhatnak a felhasználóknak")

3.  Jelölje ki a próba-felhasználó, kattintson a **hozzárendelni**, és kattintson az **Igen gombra** kattintva erősítse meg a hozzárendelés.

    ![Igen] (./media/active-directory-saas-policystat-tutorial/IC767830.png "Igen")
  
Ha meg szeretné tesztelje a egyszeri bejelentkezéses, Access Panel megnyitásához. Az Access ablaktábla, lásd: [Bevezetés az Access panelre](active-directory-saas-access-panel-introduction.md).