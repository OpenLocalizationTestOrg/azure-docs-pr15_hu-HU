<properties 
    pageTitle="Oktatóprogram: Azure Active Directory-integráció a AirWatch |} Microsoft Azure" 
    description="Megtudhatja, hogyan használhatja a AirWatch az Azure Active Directory ahhoz, hogy az egyszeri bejelentkezés, automatikus kiépítési és az egyéb!" 
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

#<a name="tutorial-azure-active-directory-integration-with-airwatch"></a>Oktatóprogram: Azure Active Directory-integráció a AirWatch

Ebben az oktatóanyagban célja integrálása az Azure és AirWatch megjelenítése.  
Az alkalmazási példát, ebben az oktatóanyagban tagolt feltételezi, hogy már rendelkezik az alábbiakat:

-   Érvényes Azure előfizetés
-   Egy AirWatch egyszeri bejelentkezés engedélyezve van az előfizetés

Ebben az oktatóanyagban befejeztével az Azure Active Directory-felhasználók AirWatch rendelt fogja tudni az alkalmazás a AirWatch vállalati webhely (service provider által kezdeményezett bejelentkezés) és használatáról a [Bevezetés az Access panelen](active-directory-saas-access-panel-introduction.md)az egyszeri bejelentkezési.

A következő építőelemek az alkalmazási példát, ebben az oktatóanyagban tagolt áll:

1.  Az alkalmazás integrációját AirWatch engedélyezése
2.  Egyszeri bejelentkezés beállítása
3.  Felhasználói kiépítési konfigurálása
4.  Felhasználók hozzárendelése

![AirWatch] (./media/active-directory-saas-airwatch-tutorial/IC791913.png "AirWatch")
##<a name="enabling-the-application-integration-for-airwatch"></a>Az alkalmazás integrációját AirWatch engedélyezése

Ez a szakasz célja, hogyan AirWatch az alkalmazás alkalmazással való integráció engedélyezése a szerkezet.

###<a name="to-enable-the-application-integration-for-airwatch-perform-the-following-steps"></a>Ahhoz, hogy az alkalmazás integrációját AirWatch, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon kattintson a bal oldali navigációs területen kattintson az **Active Directory**.

    ![Az Active Directory] (./media/active-directory-saas-airwatch-tutorial/IC700993.png "Az Active Directory")

2.  A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3.  Nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson a felső menüben **alkalmazások** .

    ![Alkalmazások] (./media/active-directory-saas-airwatch-tutorial/IC700994.png "Alkalmazások")

4.  Kattintson a **Hozzáadás** az oldal alján.

    ![Alkalmazás hozzáadása] (./media/active-directory-saas-airwatch-tutorial/IC749321.png "Alkalmazás hozzáadása")

5.  **Milyen feladatot szeretne tenni** párbeszédpanelen kattintson **a gyűjteményből az alkalmazások felvétele**.

    ![Gallerry az alkalmazás hozzáadása] (./media/active-directory-saas-airwatch-tutorial/IC749322.png "Gallerry az alkalmazás hozzáadása")

6.  A **Keresés mezőbe**írja be a **AirWatch**.

    ![Alkalmazás gyűjtemény] (./media/active-directory-saas-airwatch-tutorial/IC791914.png "Alkalmazás gyűjtemény")

7.  Az eredmény ablaktáblában jelölje ki a **AirWatch**, és válassza a **kész** , az alkalmazás hozzáadása gombra.

    ![AirWatch] (./media/active-directory-saas-airwatch-tutorial/IC791915.png "AirWatch")
##<a name="configuring-single-sign-on"></a>Egyszeri bejelentkezés beállítása

Ez a szakasz célja tagolása engedélyezése a felhasználóknak, hogy AirWatch hitelesíteni a fiókkal az összevonási alapján a SAML protokoll használatával Azure AD.  
Ez az eljárás részeként biztosan szükséges számrendszerben-64 kódolt tanúsítvány fájlt.  
Ha nem ismeri a ezt az eljárást, olvassa el a [szövegfájlba bináris tanúsítvány konvertálása](http://youtu.be/PlgrzUZ-Y1o)című témakört.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Egyszeri bejelentkezés beállításához hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon **AirWatch** alkalmazás integrációs lapon kattintson **konfigurálása az egyszeri bejelentkezés** a **Konfigurálása az egyszeri bejelentkezés** párbeszédpanel megnyitásához.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-airwatch-tutorial/IC791916.png "Egyszeri bejelentkezés beállítása")

2.  **Hogyan szeretné, hogy jelentkezzen be az AirWatch felhasználók** lapon jelölje be **A Microsoft Azure Active Directory Single Sign-On**, és kattintson a **Tovább gombra**.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-airwatch-tutorial/IC791917.png "Egyszeri bejelentkezés beállítása")

3.  **App URL-címe konfigurálása** lapon az **URL a bejelentkezési AirWatch** mezőbe írja be a AirWatch alkalmazásba bejelentkezni a felhasználók által használt URL-címet (például: "*https:// companycode.awmdm.com/AirWatch/Login?gid=companycode*"), majd kattintson a **Tovább**gombra.

    ![Állítsa be az App URL-címe] (./media/active-directory-saas-airwatch-tutorial/IC791918.png "Állítsa be az App URL-címe")

4.  A **beállítás az egyszeri bejelentkezés AirWatch a** lapon kattintson a **tanúsítvány letöltése**, és mentse a tanúsítvány fájlt a számítógépen.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-airwatch-tutorial/IC791919.png "Egyszeri bejelentkezés beállítása")

5.  A különböző webes böngészőablakban jelentkezzen be a AirWatch vállalati webhely rendszergazdaként.

6.  A bal oldali navigációs ablaktáblában kattintson a **fiókok**elemre, és válassza a **rendszergazda**.

    ![A rendszergazdák] (./media/active-directory-saas-airwatch-tutorial/IC791920.png "A rendszergazdák")

7.  Bontsa ki a **Beállítások** menüt, és kattintson a **Címtárszolgáltatásaival**.

    ![Beállítások] (./media/active-directory-saas-airwatch-tutorial/IC791921.png "Beállítások")

8.  Kattintson a **felhasználó** lap az **Alap DN** textfield, írja be a tartománynevét, és kattintson a **Mentés**gombra.

    ![Felhasználói] (./media/active-directory-saas-airwatch-tutorial/IC791922.png "Felhasználói")

9.  Kattintson a **kiszolgáló** fülre.

    ![Kiszolgáló] (./media/active-directory-saas-airwatch-tutorial/IC791923.png "Kiszolgáló")

10. Hajtsa végre az alábbi lépéseket:

    ![Töltse fel] (./media/active-directory-saas-airwatch-tutorial/IC791924.png "Töltse fel")

    1.  **Címtár-típust**válassza a **nincs**lehetőséget.
    2.  Jelölje be **a hitelesítést SAML**.
    3.  A letöltött tanúsítvány feltölteni, kattintson a **Feltöltés**elemre.

11. A **kérés** csoportban hajtsa végre az alábbi lépéseket:

    ![Kérés] (./media/active-directory-saas-airwatch-tutorial/IC791925.png "Kérés")

    1.  **Kötelező típus kérése**jelölje ki a **BEJEGYZÉST**.
    2.  Az Azure klasszikus portálon **konfigurálása az egyszeri bejelentkezés a Airwatch** párbeszédpanel lapon az **Egyszeri bejelentkezéses szolgáltatás URL-címe** értéket másolja és illessze be a **Identitás szolgáltató egyszeri bejelentkezési a URL-címe** mezőben lévő értéket.
    3.  **NameID formátumban**jelölje be az **E-mail címét**.
    4.  Kattintson a **Mentés**gombra.

12. Kattintson ismét a **felhasználó** fülre.

    ![Felhasználói] (./media/active-directory-saas-airwatch-tutorial/IC791926.png "Felhasználói")

13. **Az attribútumok** csoportban hajtsa végre az alábbi lépéseket:

    ![Attribútum] (./media/active-directory-saas-airwatch-tutorial/IC791927.png "Attribútum")

    1.  Az **Objektum azonosító** mezőbe írja be a **http://schemas.microsoft.com/identity/claims/objectidentifier**.
    2.  Az a **felhasználónév** mezőbe írja be a **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**.
    3.  A **Megjelenítendő név** mezőbe írja be a **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname**.
    4.  Az **első neve** mezőbe írja be a **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname**.
    5.  Az **Utolsó neve** mezőbe írja be a **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname**.
    6.  Az **E-mail** mezőbe írja be a **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**.
    7.  Kattintson a **Mentés**gombra.

14. Az Azure klasszikus portálon jelölje be a egyszeri bejelentkezéses konfigurációs megerősítés, és válassza a **kész** , **Állítsa be az egyszeri bejelentkezés** párbeszédpanel bezárásához.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-airwatch-tutorial/IC791928.png "Egyszeri bejelentkezés beállítása")
##<a name="configuring-user-provisioning"></a>Felhasználói kiépítési konfigurálása

Ahhoz, hogy az Azure Active Directory-felhasználóknak, hogy jelentkezzen be a AirWatch, hogy ki kell építenie AirWatch be.  
AirWatch, ha a feladat manuális kiépítési.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>A felhasználói fiókok kiépítése, hajtsa végre az alábbi lépéseket:

1.  Jelentkezzen be rendszergazdaként a **AirWatch** vállalati webhely.

2.  A bal oldali navigációs ablakban kattintson a **fiókok**elemre, és kattintson a **felhasználók**.

    ![Felhasználók] (./media/active-directory-saas-airwatch-tutorial/IC791929.png "Felhasználók")

3.  A **felhasználók** menüben kattintson a **Lista nézet**gombra, és kattintson **hozzáadása \> felhasználó hozzáadása**.

    ![Felhasználó hozzáadása] (./media/active-directory-saas-airwatch-tutorial/IC791930.png "Felhasználó hozzáadása")

4.  Az **hozzáadása / felhasználó szerkesztése** párbeszédpanelen végezze el az alábbi lépéseket:

    ![Felhasználó hozzáadása] (./media/active-directory-saas-airwatch-tutorial/IC791931.png "Felhasználó hozzáadása")

    1.  Írja be a **felhasználónevét**, **jelszót**, **A jelszó megerősítése**, **utónevet**, **Vezetéknév**, egy érvényes Azure Active Directory-fiókját, kiépítése a kapcsolódó szövegdobozok be a kívánt **E-mail címet** .
    2.  Kattintson a **Mentés**gombra.

>[AZURE.NOTE] Bármely más AirWatch felhasználói fiók létrehozása eszközöket is használhatja, illetve AirWatch rendelkezést AAD felhasználói fiókok által biztosított API-khoz.

##<a name="assigning-users"></a>Felhasználók hozzárendelése

Tesztelje a konfigurációt, meg kell adni az Azure Active Directory-felhasználók, használja az alkalmazás elérési útvonalát társításával engedélyezni szeretné.

###<a name="to-assign-users-to-airwatch-perform-the-following-steps"></a>Felhasználók hozzárendelése AirWatch, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon próba-fiók létrehozása.

2.  Kattintson a **AirWatch **alkalmazás integrációs lap **hozzárendelését a felhasználókhoz**.

    ![Hozzárendelését a felhasználókhoz] (./media/active-directory-saas-airwatch-tutorial/IC791932.png "Adhatnak a felhasználóknak")

3.  Jelölje ki a próba-felhasználó, kattintson a **hozzárendelni**, és kattintson az **Igen gombra** kattintva erősítse meg a hozzárendelés.

    ![Igen] (./media/active-directory-saas-airwatch-tutorial/IC767830.png "Igen")

Ha meg szeretné tesztelje a egyszeri bejelentkezéses, Access Panel megnyitásához. Az Access ablaktábla, lásd: [Bevezetés az Access panelre](active-directory-saas-access-panel-introduction.md).
