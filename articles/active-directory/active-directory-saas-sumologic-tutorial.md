<properties 
    pageTitle="Oktatóprogram: Azure Active Directory-integráció a SumoLogic |} Microsoft Azure" 
    description="Megtudhatja, hogyan használhatja a SumoLogic az Azure Active Directory ahhoz, hogy az egyszeri bejelentkezés, automatikus kiépítési és az egyéb!" 
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
    ms.date="09/11/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-sumologic"></a>Oktatóprogram: Azure Active Directory-integráció a SumoLogic
  
Ebben az oktatóanyagban célja integrálása az Azure és SumoLogic megjelenítéséhez.  
Az alkalmazási példát, ebben az oktatóanyagban tagolt feltételezi, hogy már rendelkezik az alábbiakat:

-   Érvényes Azure előfizetés
-   SumoLogic bérlői webhelyre
  
Ebben az oktatóanyagban befejeztével a a rendelt SumoLogicwill egyetlen tudja bejelentkezési kell az alkalmazásba, a SumoLogic Azure AD-felhasználók vállalati webhely (service provider által kezdeményezett bejelentkezés) és használatáról a [Bevezetés az Access panelen](active-directory-saas-access-panel-introduction.md).
  
A következő építőelemek az alkalmazási példát, ebben az oktatóanyagban tagolt áll:

1.  Az alkalmazás integrációját SumoLogic engedélyezése
2.  Egyszeri bejelentkezés beállítása
3.  Felhasználói kiépítési konfigurálása
4.  Felhasználók hozzárendelése

![Eset] (./media/active-directory-saas-sumologic-tutorial/IC778549.png "Eset")

##<a name="enabling-the-application-integration-for-sumologic"></a>Az alkalmazás integrációját SumoLogic engedélyezése
  
Ez a szakasz célja, hogyan SumoLogic az alkalmazás alkalmazással való integráció engedélyezése a szerkezet.

###<a name="to-enable-the-application-integration-for-sumologic-perform-the-following-steps"></a>Ahhoz, hogy az alkalmazás integrációját SumoLogic, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon kattintson a bal oldali navigációs területen kattintson az **Active Directory**.

    ![Az Active Directory] (./media/active-directory-saas-sumologic-tutorial/IC700993.png "Az Active Directory")

2.  A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3.  Nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson a felső menüben **alkalmazások** .

    ![Alkalmazások] (./media/active-directory-saas-sumologic-tutorial/IC700994.png "Alkalmazások")

4.  Kattintson a **Hozzáadás** az oldal alján.

    ![Alkalmazás hozzáadása] (./media/active-directory-saas-sumologic-tutorial/IC749321.png "Alkalmazás hozzáadása")

5.  **Milyen feladatot szeretne tenni** párbeszédpanelen kattintson **a gyűjteményből az alkalmazások felvétele**.

    ![Gallerry az alkalmazás hozzáadása] (./media/active-directory-saas-sumologic-tutorial/IC749322.png "Gallerry az alkalmazás hozzáadása")

6.  A **Keresés mezőbe**írja be a **sumologic**.

    ![Alkalmazás gyűjtemény] (./media/active-directory-saas-sumologic-tutorial/IC778550.png "Alkalmazás gyűjtemény")

7.  Az eredmény ablaktáblában jelölje ki a **SumoLogic**, és válassza a **kész** , az alkalmazás hozzáadása gombra.

    ![SumoLogic] (./media/active-directory-saas-sumologic-tutorial/IC778551.png "SumoLogic")

##<a name="configuring-single-sign-on"></a>Egyszeri bejelentkezés beállítása
  
Ez a szakasz célja tagolása engedélyezése a felhasználóknak, hogy SumoLogic hitelesíteni a fiókkal az összevonási alapján a SAML protokoll használatával Azure AD.  
Ez az eljárás részeként szükséges számrendszerben-64 kódolt tanúsítvány feltölteni a SumoLogictenant.  
Ha nem ismeri a ezt az eljárást, megtudhatja, [hogy miként szövegfájlba bináris tanúsítvány konvertálása](http://youtu.be/PlgrzUZ-Y1o)

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Egyszeri bejelentkezés beállításához hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon **SumoLogic** alkalmazás integrációs lapon kattintson **konfigurálása az egyszeri bejelentkezés** a **Konfigurálása az egyszeri bejelentkezés** párbeszédpanel megnyitásához.

    ![Beállítás az egyszeri bejelentkezés] (./media/active-directory-saas-sumologic-tutorial/IC778552.png "Beállítás az egyszeri bejelentkezés")

2.  **Hogyan szeretné, hogy jelentkezzen be az SumoLogic felhasználók** lapon jelölje be **A Microsoft Azure Active Directory Single Sign-On**, és kattintson a **Tovább gombra**.

    ![Beállítás az egyszeri bejelentkezés] (./media/active-directory-saas-sumologic-tutorial/IC778553.png "Beállítás az egyszeri bejelentkezés")

3.  Az **App URL-címe konfigurálása** lapon **SumoLogic bejelentkezési az URL** mezőbe írja be az URL-t, a következő mintát "*https://\<bérlői neve\>. SumoLogic.com*", majd kattintson a **Tovább**gombra.

    ![Konfigurálása aoo URL-címe] (./media/active-directory-saas-sumologic-tutorial/IC778554.png "Konfigurálása aoo URL-címe")

4.  A **Configure egyszeri bejelentkezés SumoLogic a** lapon a tanúsítvány letöltése, kattintson a **tanúsítvány letöltése**, és mentse a tanúsítvány fájlt a számítógépen parancsra.

    ![Beállítás az egyszeri bejelentkezés] (./media/active-directory-saas-sumologic-tutorial/IC778555.png "Beállítás az egyszeri bejelentkezés")

5.  A különböző webes böngészőablakban jelentkezzen be a SumoLogic vállalati webhely rendszergazdaként.

6.  Nyissa meg a **kezelése \> biztonsági**.

    ![Kezelése] (./media/active-directory-saas-sumologic-tutorial/IC778556.png "Kezelése")

7.  Kattintson a **SAML**.

    ![Globális biztonsági beállítások] (./media/active-directory-saas-sumologic-tutorial/IC778557.png "Globális biztonsági beállítások")

8.  **Jelölje ki a konfiguráció, vagy hozzon létre egy újat** listából jelölje be az **Azure Active Directory**, és kattintson a **beállítás**.

    ![Állítsa be a SAML 2.0-s verziója] (./media/active-directory-saas-sumologic-tutorial/IC778558.png "Állítsa be a SAML 2.0-s verziója")

9.  A **SAML 2.0 beállítása** párbeszédpanelen hajtsa végre az alábbi lépéseket:

    ![Állítsa be a SAML 2.0-s verziója] (./media/active-directory-saas-sumologic-tutorial/IC778559.png "Állítsa be a SAML 2.0-s verziója")

    1.  **Konfigurációs neve** mezőbe írja be az **Azure Active Directory**.
    2.  Jelölje be a **hibakeresési mód**.
    3.  Az Azure klasszikus portálon **konfigurálása az egyszeri bejelentkezés a SumoLogic** párbeszéd lapon a **Kibocsátó URL-címe** értéket másolja és illessze be a **kibocsátó** mezőben lévő értéket.
    4.  Az Azure klasszikus portálon **konfigurálása az egyszeri bejelentkezés a SumoLogic** párbeszéd lapon a **Hitelesítési kérése URL-címe** értéket másolja és illessze be a **Authn kérése URL-címe** mezőben lévő értéket.
    5.  A letöltött tanúsítvány **Alap-64-címként kódolt** fájl létrehozása  

        >[AZURE.TIP] További információra kíváncsi olvassa el a [bináris tanúsítvány szövegfájlba konvertálása](http://youtu.be/PlgrzUZ-Y1o) című témakört.

    6.  Nyissa meg az alap-64 kódolt tanúsítvány a Jegyzettömbben, a tartalmát a vágólapra másolja és illessze be a teljes tanúsítvány be **X.509-es tanúsítvány** szövegdoboz.
    7.  **E-mailek attribútum**jelölje ki a **Tárgy használata SAML**.
    8.  Jelölje ki a **SP kezdeményezett bejelentkezési konfigurációs**.
    9.  Az **Bejelentkezési elérési út** mezőbe írja be az **Azure**.
    10. Kattintson a **Mentés**gombra.

10. Az Azure klasszikus portálon **konfigurálása az egyszeri bejelentkezés a SumoLogic** párbeszéd lapon jelölje ki az egyszeri bejelentkezéses konfigurációs megerősítést kérő, és válassza a **kész**gombra.

    ![Beállítás az egyszeri bejelentkezés] (./media/active-directory-saas-sumologic-tutorial/IC778560.png "Beállítás az egyszeri bejelentkezés")

##<a name="configuring-user-provisioning"></a>Felhasználói kiépítési konfigurálása
  
Ahhoz, hogy az Azure Active Directory-felhasználóknak, hogy jelentkezzen be a SumoLogic, hogy ki kell építenie SumoLogic.  
SumoLogic, ha a feladat manuális kiépítési.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>A felhasználói fiókok kiépítése, hajtsa végre az alábbi lépéseket:

1.  Jelentkezzen be az **SumoLogic** bérlői webhelyen.

2.  Nyissa meg a **kezelése \> felhasználók**.

    ![Felhasználók] (./media/active-directory-saas-sumologic-tutorial/IC778561.png "Felhasználók")

3.  Kattintson a **hozzáadása**gombra.

    ![Felhasználók] (./media/active-directory-saas-sumologic-tutorial/IC778562.png "Felhasználók")

4.  Az **Új felhasználó** párbeszédpanelen hajtsa végre az alábbi lépéseket:

    ![Új felhasználó] (./media/active-directory-saas-sumologic-tutorial/IC778563.png "Új felhasználó")

    1.  Írja be a kapcsolódó információt szeretne rendelkezni az **Utónév**és a **Vezetéknevet** , valamint a **e-mailek** szövegdobozok az Azure AD-fiók.
    2.  Válasszon egy szerepkört.
    3.  **Állapot**válassza az **aktív**.
    4.  Kattintson a **Mentés**gombra.

>[AZURE.NOTE] Bármely más SumoLogic felhasználói fiók létrehozása eszközöket is használhatja, illetve SumoLogic rendelkezést AAD felhasználói fiókok által biztosított API-khoz.

##<a name="assigning-users"></a>Felhasználók hozzárendelése
  
Tesztelje a konfigurációt, meg kell adni az Azure Active Directory-felhasználók, használja az alkalmazás elérési útvonalát társításával engedélyezni szeretné.

###<a name="to-assign-users-to-sumologic-perform-the-following-steps"></a>Felhasználók hozzárendelése SumoLogic, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon próba-fiók létrehozása.

2.  Kattintson a **SumoLogic** alkalmazás integrációs lap **hozzárendelését a felhasználókhoz**.

    ![Adhatnak a felhasználóknak] (./media/active-directory-saas-sumologic-tutorial/IC778564.png "Adhatnak a felhasználóknak")

3.  Jelölje ki a próba-felhasználó, kattintson a **hozzárendelni**, és kattintson az **Igen gombra** kattintva erősítse meg a hozzárendelés.

    ![Igen] (./media/active-directory-saas-sumologic-tutorial/IC767830.png "Igen")
  
Ha meg szeretné tesztelje a egyszeri bejelentkezéses, Access Panel megnyitásához. Az Access ablaktábla, lásd: [Bevezetés az Access panelre](active-directory-saas-access-panel-introduction.md).