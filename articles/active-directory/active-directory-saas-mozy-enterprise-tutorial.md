<properties 
    pageTitle="Oktatóprogram: Azure Active Directory-integráció a Mozy vállalati |} Microsoft Azure" 
    description="Megtudhatja, hogy miként Mozy vállalati használata az Azure Active Directory ahhoz, hogy az egyszeri bejelentkezés, automatikus kiépítési és az egyéb!" 
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
    ms.date="09/29/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-mozy-enterprise"></a>Oktatóprogram: Azure Active Directory-integráció a Mozy vállalati
  
Ebben az oktatóanyagban célja a Azure és Mozy nagyvállalati integrációs megjelenítéséhez.  
Az alkalmazási példát, ebben az oktatóanyagban tagolt feltételezi, hogy már rendelkezik az alábbiakat:

-   Érvényes Azure előfizetés
-   Vállalati Mozy bérlői webhelyre
  
Ebben az oktatóanyagban befejeztével az Azure Active Directory felhasználók Mozy vállalati rendelt tudnak egyetlen bejelentkezés az alkalmazásba, a Mozy vállalati vállalati webhely (service provider által kezdeményezett bejelentkezés) és használatáról a [Bevezetés az Access panelen](active-directory-saas-access-panel-introduction.md).
  
A következő építőelemek az alkalmazási példát, ebben az oktatóanyagban tagolt áll:

1.  Az alkalmazás integrálását Mozy nagyvállalati engedélyezése
2.  Egyszeri bejelentkezés beállítása
3.  Felhasználói kiépítési konfigurálása
4.  Felhasználók hozzárendelése

![Eset] (./media/active-directory-saas-mozy-enterprise-tutorial/IC777308.png "Eset")
##<a name="enabling-the-application-integration-for-mozy-enterprise"></a>Az alkalmazás integrálását Mozy nagyvállalati engedélyezése
  
Ez a szakasz célja, hogyan Mozy vállalati alkalmazás alkalmazással való integráció engedélyezése a szerkezet.

###<a name="to-enable-the-application-integration-for-mozy-enterprise-perform-the-following-steps"></a>Ahhoz, hogy az alkalmazás integrálását Mozy nagyvállalati, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon kattintson a bal oldali navigációs területen kattintson az **Active Directory**.

    ![Az Active Directory] (./media/active-directory-saas-mozy-enterprise-tutorial/IC700993.png "Az Active Directory")

2.  A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3.  Nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson a felső menüben **alkalmazásokat** .

    ![Alkalmazások] (./media/active-directory-saas-mozy-enterprise-tutorial/IC700994.png "Alkalmazások")

4.  Kattintson a **Hozzáadás** az oldal alján.

    ![Alkalmazás hozzáadása] (./media/active-directory-saas-mozy-enterprise-tutorial/IC749321.png "Alkalmazás hozzáadása")

5.  **Milyen feladatot szeretne tenni** párbeszédpanelen kattintson **a gyűjteményből az alkalmazások felvétele**.

    ![Gallerry az alkalmazás hozzáadása] (./media/active-directory-saas-mozy-enterprise-tutorial/IC749322.png "Gallerry az alkalmazás hozzáadása")

6.  A **Keresés mezőbe**írja be a **Vállalati mozy**.

    ![Alkalmazás gyűjtemény] (./media/active-directory-saas-mozy-enterprise-tutorial/IC777309.png "Alkalmazás gyűjtemény")

7.  Az eredmény ablaktáblában jelölje ki a **Mozy vállalati**pontra, és kattintson a **kész** az alkalmazás hozzáadása parancsra.

    ![Vállalati Mozy] (./media/active-directory-saas-mozy-enterprise-tutorial/IC777310.png "Vállalati Mozy")
##<a name="configuring-single-sign-on"></a>Egyszeri bejelentkezés beállítása
  
Ez a szakasz célja tagolása engedélyezése a felhasználóknak, hogy a saját fiók az összevonási alapján a SAML protokoll használatával Azure Active Directory hitelesítő Mozy nagyvállalati verzióra.  
Ez az eljárás részeként alap-64 kódolt tanúsítvány feltöltése a Mozy vállalati bérlői szükség.  
Ha nem ismeri a ezt az eljárást, megtudhatja, [hogy miként szövegfájlba bináris tanúsítvány konvertálása](http://youtu.be/PlgrzUZ-Y1o)

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Egyszeri bejelentkezés beállításához hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon **Mozy vállalati** alkalmazás integrációs lapon kattintson **konfigurálása az egyszeri bejelentkezés** a **Konfigurálása az egyszeri bejelentkezés** párbeszédpanel megnyitásához.

    ![Beállítás az egyszeri bejelentkezés] (./media/active-directory-saas-mozy-enterprise-tutorial/IC771709.png "Beállítás az egyszeri bejelentkezés")

2.  **Hogyan szeretné, hogy jelentkezzen be az Mozy vállalati felhasználók** lapon jelölje be **A Microsoft Azure Active Directory Single Sign-On**, és kattintson a **Tovább gombra**.

    ![Beállítás az egyszeri bejelentkezés] (./media/active-directory-saas-mozy-enterprise-tutorial/IC777311.png "Beállítás az egyszeri bejelentkezés")

3.  Az **Alkalmazás URL-cím beállítása** lapon **Mozy vállalati bejelentkezési az URL-cím** mezőbe írja be az URL-t, a következő mintát "*https://\<bérlői neve\>. Mozyenterprise.com*", majd kattintson a **Tovább**gombra.

    ![Állítsa be az app URL-címe] (./media/active-directory-saas-mozy-enterprise-tutorial/IC777312.png "Állítsa be az app URL-címe")

4.  A **konfigurálása az egyszeri bejelentkezés Mozy vállalati a** lapon a tanúsítvány letöltése, kattintson a **tanúsítvány letöltése**, és mentse a tanúsítvány fájlt a számítógépen parancsra.

    ![Beállítás az egyszeri bejelentkezés] (./media/active-directory-saas-mozy-enterprise-tutorial/IC777313.png "Beállítás az egyszeri bejelentkezés")

5.  A különböző webes böngészőablakban jelentkezzen be a vállalati Mozy vállalati webhely rendszergazdaként.

6.  **Konfigurálása** szakaszban kattintson a **Hitelesítés házirend**parancsra.

    ![Hitelesítés házirend] (./media/active-directory-saas-mozy-enterprise-tutorial/IC777314.png "Hitelesítés házirend")

7.  A **Hitelesítési házirend** csoportban hajtsa végre az alábbi lépéseket:

    ![Hitelesítés házirend] (./media/active-directory-saas-mozy-enterprise-tutorial/IC777315.png "Hitelesítés házirend")

    1.  Jelölje ki a **címtár-szinkronizálás eszközének** **szolgáltatóként**.
    2.  Jelölje be **a leküldéses LDAP használata**.
    3.  Kattintson a **SAML hitelesítés** fülre.
    4.  Az Azure klasszikus portálon **konfigurálása az egyszeri bejelentkezés a Mozy vállalati** párbeszédpanel lapon a **Hitelesítési kérése URL-címe** értéket másolja és illessze be a **Hitelesítés URL-címe** mezőben lévő értéket.
    5.  Az Azure klasszikus portálon **konfigurálása az egyszeri bejelentkezés a Mozy vállalati** párbeszédpanel lapon másolja az **Identitás-szolgáltató azonosítója** értéket, és illessze be a **SAML végpont** mezőben lévő értéket.
    6.  A letöltött tanúsítvány **Alap-64-címként kódolt** fájl létrehozása  

        >[AZURE.TIP]További információra kíváncsi olvassa el a [bináris tanúsítvány szövegfájlba konvertálása](http://youtu.be/PlgrzUZ-Y1o) című témakört.

    7.  Nyissa meg az alap-64 kódolt tanúsítvány a Jegyzettömbben, a tartalmát a vágólapra másolja és illessze be a teljes tanúsítvány be **SAML tanúsítvány** szövegmezőt.
    8.  Jelölje ki **Egyszeri bejelentkezés engedélyezése rendszergazdáknak követve jelentkezzen be a hálózati hitelesítő adatait**.
    9.  A **módosítások mentése**gombra.

8.  Az Azure klasszikus portálon **konfigurálása az egyszeri bejelentkezés a Mozy vállalati** párbeszédpanel lapon jelölje ki az egyszeri bejelentkezéses konfigurációs megerősítést kérő, és válassza a **kész**gombra.

    ![Beállítás az egyszeri bejelentkezés] (./media/active-directory-saas-mozy-enterprise-tutorial/IC777316.png "Beállítás az egyszeri bejelentkezés")
##<a name="configuring-user-provisioning"></a>Felhasználói kiépítési konfigurálása
  
Ahhoz, hogy az Azure Active Directory-felhasználók Mozy vállalati való bejelentkezéshez, hogy ki kell építenie Mozy vállalati sablonba.  
Mozy vállalati, ha a feladat manuális kiépítési.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>A felhasználói fiókok kiépítése, hajtsa végre az alábbi lépéseket:

1.  Jelentkezzen be az **Mozy vállalati** bérlői webhelyen.

2.  Kattintson a **felhasználók**elemre, és kattintson az **Új felhasználó hozzáadása**gombra.

    ![Felhasználók] (./media/active-directory-saas-mozy-enterprise-tutorial/IC777317.png "Felhasználók")

    >[AZURE.NOTE]Az **Új felhasználó hozzáadása** a beállítás csak akkor jelenik meg, csak ha **Mozy** be van jelölve a **hitelesítési házirend**-konferenciaszolgáltatóként. Ha van beállítva a SAML hitelesítés majd a felhasználók vannak felvéve automatikusan meg az első bejelentkezéskor egyetlen regisztrációnak a.

3.  Az új felhasználó párbeszédpanelen hajtsa végre az alábbi lépéseket:

    ![Felhasználók hozzáadása] (./media/active-directory-saas-mozy-enterprise-tutorial/IC777318.png "Felhasználók hozzáadása")

    1.  A **csoport kiválasztása** listájában jelöljön ki egy csoportot.
    2.  A **felhasználók milyen típusú** listából válasszon egy típust.
    3.  A **felhasználónév** mezőbe írja be az Azure Active Directory-felhasználó nevét.
    4.  Az **E-mail** mezőbe írja be az Azure Active Directory-felhasználó e-mail címét.
    5.  Jelölje ki a **felhasználó utasítás e-mailek küldése**.
    6.  Kattintson a **felhasználó(k) hozzáadása**gombra.

    >[AZURE.NOTE]Miután létrehozta a felhasználót, e-mailt küld a Azure AD-felhasználó, mielőtt aktívvá válik, győződjön meg arról, hogy a fiók mutató hivatkozást tartalmazó.

>[AZURE.NOTE]Bármely más Mozy vállalati felhasználói fiók létrehozása eszközöket is használhatja, illetve Mozy vállalati rendelkezést AAD felhasználói fiókok által biztosított API-khoz.

##<a name="assigning-users"></a>Felhasználók hozzárendelése
 
Tesztelje a konfigurációt, meg kell adni az Azure Active Directory-felhasználók, használja az alkalmazás elérési útvonalát társításával engedélyezni szeretné.

###<a name="to-assign-users-to-mozy-enterprise-perform-the-following-steps"></a>Felhasználók hozzárendelése Mozy vállalati, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon próba-fiók létrehozása.

2.  A **Vállalati Mozy **alkalmazás integrációs lapon kattintson a **hozzárendelését a felhasználókhoz**.

    ![Adhatnak a felhasználóknak] (./media/active-directory-saas-mozy-enterprise-tutorial/IC777319.png "Adhatnak a felhasználóknak")

3.  Jelölje ki a próba-felhasználó, kattintson a **hozzárendelni**, és kattintson az **Igen gombra** kattintva erősítse meg a hozzárendelés.

    ![Igen] (./media/active-directory-saas-mozy-enterprise-tutorial/IC767830.png "Igen")
  
Ha meg szeretné tesztelje a egyszeri bejelentkezéses, Access Panel megnyitásához. Ha többet szeretne tudni az Access panelen című témakörben [az Access-panelen](active-directory-saas-access-panel-introduction.md).