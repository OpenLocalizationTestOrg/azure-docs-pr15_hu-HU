<properties 
    pageTitle="Oktatóprogram: Azure Active Directory-integráció a TOPdesk - biztonságos |} Microsoft Azure"
    description="Megtudhatja, hogy miként TOPdesk - használja az Azure Active Directory ahhoz, hogy az egyszeri bejelentkezés, automatikus kiépítési és más biztonságos!." 
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

#<a name="tutorial-azure-active-directory-integration-with-topdesk---secure"></a>Oktatóprogram: Azure Active Directory-integráció a TOPdesk - biztonságos
  
Ebben az oktatóanyagban célja Azure és TOPdesk - biztonságos integrációja megjelenítéséhez.  
Az alkalmazási példát, ebben az oktatóanyagban tagolt feltételezi, hogy már rendelkezik az alábbiakat:

-   Érvényes Azure előfizetés
-   A TOPdesk - biztonságos egyszeri bejelentkezés engedélyezve van az előfizetés
  
Ebben az oktatóanyagban befejeztével az Azure Active Directory felhasználók TOPdesk - biztonságos rendelt tudnak egyetlen bejelentkezés az alkalmazásba, a TOPdesk - biztonságos vállalati webhely (service provider által kezdeményezett bejelentkezés) és használatáról a [Bevezetés az Access panelre](active-directory-saas-access-panel-introduction.md).
  
A következő építőelemek az alkalmazási példát, ebben az oktatóanyagban tagolt áll:

1.  Az alkalmazás integrációját TOPdesk - biztonságos engedélyezése
2.  Egyszeri bejelentkezés beállítása
3.  Felhasználói kiépítési konfigurálása
4.  Felhasználók hozzárendelése

![Eset] (./media/active-directory-saas-topdesk-secure-tutorial/IC790596.png "Eset")

##<a name="enabling-the-application-integration-for-topdesk---secure"></a>Az alkalmazás integrációját TOPdesk - biztonságos engedélyezése
  
Ez a szakasz célja, hogyan TOPdesk - biztonságos az alkalmazás alkalmazással való integráció engedélyezése a szerkezet.

###<a name="to-enable-the-application-integration-for-topdesk---secure-perform-the-following-steps"></a>TOPdesk – az alkalmazás alkalmazással való integráció engedélyezése biztonságos, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon kattintson a bal oldali navigációs területen kattintson az **Active Directory**.

    ![Az Active Directory] (./media/active-directory-saas-topdesk-secure-tutorial/IC700993.png "Az Active Directory")

2.  A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3.  Nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson a felső menüben **alkalmazásokat** .

    ![Alkalmazások] (./media/active-directory-saas-topdesk-secure-tutorial/IC700994.png "Alkalmazások")

4.  Kattintson a **Hozzáadás** az oldal alján.

    ![Alkalmazás hozzáadása] (./media/active-directory-saas-topdesk-secure-tutorial/IC749321.png "Alkalmazás hozzáadása")

5.  **Milyen feladatot szeretne tenni** párbeszédpanelen kattintson **a gyűjteményből az alkalmazások felvétele**.

    ![Gallerry az alkalmazás hozzáadása] (./media/active-directory-saas-topdesk-secure-tutorial/IC749322.png "Gallerry az alkalmazás hozzáadása")

6.  A **Keresés mezőbe**írja be a **TOPdesk - biztonságos**.

    ![Alkalmazás gyűjtemény] (./media/active-directory-saas-topdesk-secure-tutorial/IC790597.png "Alkalmazás gyűjtemény")

7.  Az eredmény ablaktáblában jelölje ki a **TOPdesk - biztonságos**, és válassza a **kész** , az alkalmazás hozzáadása gombra.

    ![TOPdesk - biztonságos] (./media/active-directory-saas-topdesk-secure-tutorial/IC791933.png "TOPdesk - biztonságos")

##<a name="configuring-single-sign-on"></a>Egyszeri bejelentkezés beállítása
  
Ez a szakasz célja engedélyezése a felhasználóknak, hogy TOPdesk - hitelesítést tagolása az összevonási alapján a SAML protokoll használatával Azure AD a saját fiók biztonságos.  
Konfigurálása az egyszeri bejelentkezés az TOPdesk - biztonságos megköveteli embléma ikon fájl feltöltése. Úgy juthat az ikon fájl, a TOPdesk támogatási csoport munkatársaitól kaphat.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Egyszeri bejelentkezés beállításához hajtsa végre az alábbi lépéseket:

1.  Jelentkezzen be rendszergazdaként a **TOPdesk - biztonságos** vállalati webhely.

2.  A **TOPdesk** menüben kattintson a **Beállítások**gombra.

    ![Beállítások] (./media/active-directory-saas-topdesk-secure-tutorial/IC790598.png "Beállítások")

3.  Kattintson a **Login beállítások**gombra.

    ![Bejelentkezési beállításait] (./media/active-directory-saas-topdesk-secure-tutorial/IC790599.png "Bejelentkezési beállításait")

4.  Bontsa ki a **Bejelentkezési beállítások** menüt, és válassza az **Általános**.

    ![Általános] (./media/active-directory-saas-topdesk-secure-tutorial/IC790600.png "Általános")

5.  A **SAML login** konfigurációs szakasz **biztonságos** csoportjában hajtsa végre az alábbi lépéseket:

    ![Technikai beállítások] (./media/active-directory-saas-topdesk-secure-tutorial/IC790855.png "Technikai beállítások")

    1.  Kattintson a **Letöltés** a metaadat-alapú nyilvános fájl letöltése gombra, és mentse azt helyileg a számítógépen.
    2.  Nyissa meg a metaadat-fájlt, és keresse meg a **AssertionConsumerService** csomópontot.
        ![Állítás fogyasztói szolgáltatás] (./media/active-directory-saas-topdesk-secure-tutorial/IC790856.png "Állítás fogyasztói szolgáltatás")
    3.  Másolja a **AssertionConsumerService** értékét.  

        >[AZURE.NOTE] Az oktatóprogram később szüksége lesz az **App URL-címe konfigurálása** szakaszban található érték.

6.  A különböző webes böngészőablakban jelentkezzen be az **Azure klasszikus portál** rendszergazdaként.

7.  A **TOPdesk - biztonságos** alkalmazás integrációs lapon kattintson a **konfigurálása az egyszeri bejelentkezés** a **Konfigurálása az egyszeri bejelentkezés** párbeszédpanel megnyitásához.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-topdesk-secure-tutorial/IC790602.png "Egyszeri bejelentkezés beállítása")

8.  **Hogyan szeretné, hogy jelentkezzen be az TOPdesk - biztonságos felhasználók** lapon jelölje be **A Microsoft Azure Active Directory Single Sign-On**, és kattintson a **Tovább gombra**.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-topdesk-secure-tutorial/IC790603.png "Egyszeri bejelentkezés beállítása")

9.  Az **Alkalmazás URL-cím beállítása** lapon hajtsa végre az alábbi lépéseket:

    ![Állítsa be az App URL-címe] (./media/active-directory-saas-topdesk-secure-tutorial/IC790604.png "Állítsa be az App URL-címe")

    1.  Az **TOPdesk - bejelentkezési biztonságos az URL-cím** mezőbe írja be a jelentkezhet be a TOPdesk - biztonságos alkalmazás a felhasználók által használt URL-CÍMÉT (például: "*https://qssolutions.topdesk.net*").
    2.  Az **TOPdesk – nyilvános válasz URL-címe** mezőbe illessze be a **TOPdesk - biztonságos AssertionConsumerService URL-CÍMÉT** (például: "*https://qssolutions.topdesk.net/tas/public/login/saml*")
    3.  Kattintson a **Tovább**gombra.

10. **Konfigurálása az egyszeri bejelentkezés TOPdesk - biztonságos a** lapon a metaadat-alapú fájl letöltéséhez kattintson a **metaadat-alapú letöltése**, és mentse a fájlt a számítógépen helyben parancsra.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-topdesk-secure-tutorial/IC790605.png "Egyszeri bejelentkezés beállítása")

11. Biztonságitanúsítvány-fájl létrehozásához hajtsa végre az alábbi lépéseket:

    ![Tanúsítvány] (./media/active-directory-saas-topdesk-secure-tutorial/IC790606.png "Tanúsítvány")

    1.  Nyissa meg a letöltött metaadat-fájlt.
    2.  Bontsa ki a **RoleDescriptor** csomópontot, amely tartalmazza az egy **xsi: type** **géppel: ApplicationServiceType**.
    3.  Másolja a **X509Certificate** csomópontot értékét.
    4.  Az egy fájlt a számítógépen helyben menteni a másolt **X509Certificate** értékét.

12. A TOPdesk - biztonságos vállalati webhely **TOPdesk** menüben kattintson a **Beállítások**gombra.

    ![Beállítások] (./media/active-directory-saas-topdesk-secure-tutorial/IC790598.png "Beállítások")

13. Kattintson a **Login beállítások**gombra.

    ![Bejelentkezési beállításait] (./media/active-directory-saas-topdesk-secure-tutorial/IC790599.png "Bejelentkezési beállításait")

14. Bontsa ki a **Bejelentkezési beállítások** menüt, és válassza az **Általános**.

    ![Általános] (./media/active-directory-saas-topdesk-secure-tutorial/IC790600.png "Általános")

15. A **nyilvános** csoportban kattintson a **Hozzáadás**gombra.

    ![Hozzáadása] (./media/active-directory-saas-topdesk-secure-tutorial/IC790607.png "Hozzáadása")

16. A **SAML konfigurációs Segéd** párbeszédpanel lapon hajtsa végre az alábbi lépéseket:

    ![SAML konfigurációs Segéd] (./media/active-directory-saas-topdesk-secure-tutorial/IC790608.png "SAML konfigurációs Segéd")

    1.  Töltse fel a metaadat-alapú letöltött fájlt a **Metaadat-alapú összevonás**, kattintson a **Tallózás gombra**.
    2.  Töltse fel a tanúsítvány fájlt a **Tanúsítvány (RSA)**, kattintson a **Tallózás gombra**.
    3.  Töltse fel a embléma fájlt szerezte be a TOPdesk támogatási szolgálatához, az **embléma ikonra**, kattintson a **Tallózás gombra**.
    4.  Az **attribútum a felhasználó neve** mezőbe írja be a **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**.
    5.  A **megjelenítendő név** mezőbe írja be a konfigurációban nevét.
    6.  Kattintson a **Mentés**gombra.

17. Az Azure klasszikus portálon jelölje be a egyszeri bejelentkezéses konfigurációs megerősítés, és válassza a **kész** , **Állítsa be az egyszeri bejelentkezés** párbeszédpanel bezárásához.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-topdesk-secure-tutorial/IC790609.png "Egyszeri bejelentkezés beállítása")

##<a name="configuring-user-provisioning"></a>Felhasználói kiépítési konfigurálása
  
Ahhoz, hogy Azure AD-felhasználóknak, hogy jelentkezzen be a TOPdesk - biztonságos, azok ki kell építenie TOPdesk - biztonságos be.  
Ha TOPdesk - biztonságos, kiépítési kézi tevékenység.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Felhasználói kiépítési beállításához hajtsa végre az alábbi lépéseket:

1.  Jelentkezzen be rendszergazdaként a **TOPdesk - biztonságos** vállalati webhely.

2.  Kattintson a menü felső **TOPdesk \> új \> terméktámogatási fájlok \> operátor**.

    ![Műveleti jel] (./media/active-directory-saas-topdesk-secure-tutorial/IC790610.png "Műveleti jel")

3.  Az **Új művelet** párbeszédpanelen hajtsa végre az alábbi lépéseket:

    ![Új operátor] (./media/active-directory-saas-topdesk-secure-tutorial/IC790611.png "Új operátor")

    1.  Kattintson az Általános fülre.
    2.  A **Vezetéknév** mezőben lévő értéket, az **Általános** szakaszt írja be egy érvényes Azure Active Directory-fiókját, kiépítése kívánt utolsó nevét.
    3.  A **hely** csoportban válasszon egy **webhely** a fiókhoz.
    4.  A **Bejelentkezési TOPdesk** szakasz **Bejelentkezési neve** mezőbe írja be a felhasználó bejelentkezési nevét.
    5.  Kattintson a **Mentés**gombra.

>[AZURE.NOTE] Bármely más TOPdesk - biztonságos felhasználói fiók létrehozása eszközök vagy TOPdesk - biztonságos AAD felhasználói fiókok kiépítése által biztosított API-khoz is használhatja.

##<a name="assigning-users"></a>Felhasználók hozzárendelése
  
Tesztelje a konfigurációt, meg kell adni az Azure Active Directory-felhasználók, használja az alkalmazás elérési útvonalát társításával engedélyezni szeretné.

###<a name="to-assign-users-to-topdesk---secure-perform-the-following-steps"></a>Felhasználók hozzárendelése TOPdesk - biztonságos, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon próba-fiók létrehozása.

2.  Kattintson a **TOPdesk - biztonságos **alkalmazás integráció lap **hozzárendelését a felhasználókhoz**.

    ![Adhatnak a felhasználóknak] (./media/active-directory-saas-topdesk-secure-tutorial/IC790612.png "Adhatnak a felhasználóknak")

3.  Jelölje ki a próba-felhasználó, kattintson a **hozzárendelni**, és kattintson az **Igen gombra** kattintva erősítse meg a hozzárendelés.

    ![Igen] (./media/active-directory-saas-topdesk-secure-tutorial/IC767830.png "Igen")
  
Ha meg szeretné tesztelje a egyszeri bejelentkezéses, Access Panel megnyitásához. Ha többet szeretne tudni az Access panelen című témakörben [az Access-panelen](active-directory-saas-access-panel-introduction.md).