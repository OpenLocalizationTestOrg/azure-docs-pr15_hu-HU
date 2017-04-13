<properties 
    pageTitle="Oktatóprogram: Azure Active Directory-integráció a TOPdesk – nyilvános |} Microsoft Azure" 
    description="Megtudhatja, hogy miként használhatja TOPdesk - és az Azure Active Directory ahhoz, hogy az egyszeri bejelentkezés, automatikus kiépítési és egyéb nyilvános!" 
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

#<a name="tutorial-azure-directory-integration-with-topdesk---public"></a>Oktatóprogram: Azure címtár-integráció a TOPdesk – nyilvános

Ebben az oktatóanyagban célja integrálása az Azure és TOPdesk - nyilvános megjelenítéséhez.  
Az alkalmazási példát, ebben az oktatóanyagban tagolt feltételezi, hogy már rendelkezik az alábbiakat:

-   Érvényes Azure előfizetés
-   A TOPdesk – nyilvános egyszeri bejelentkezés engedélyezve van az előfizetés
  
Ebben az oktatóanyagban befejeztével az Azure Active Directory-felhasználók rendelt TOPdesk – nyilvános tudják egyetlen jelentkezzen be az a TOPdesk - vállalat nyilvános webhely (service provider által kezdeményezett bejelentkezés) és használatáról a [Bevezetés az Access panelen](active-directory-saas-access-panel-introduction.md)az alkalmazást.
  
A következő építőelemek az alkalmazási példát, ebben az oktatóanyagban tagolt áll:

1.  Az alkalmazás integrációját TOPdesk – nyilvános engedélyezése
2.  Egyszeri bejelentkezés beállítása
3.  Felhasználói kiépítési konfigurálása
4.  Felhasználók hozzárendelése

![Eset] (./media/active-directory-saas-topdesk-public-tutorial/IC790613.png "Eset")

##<a name="enabling-the-application-integration-for-topdesk---public"></a>Az alkalmazás integrációját TOPdesk – nyilvános engedélyezése
  
Ez a szakasz célja, hogyan TOPdesk – nyilvános az alkalmazás alkalmazással való integráció engedélyezése a szerkezet.

###<a name="to-enable-the-application-integration-for-topdesk---public-perform-the-following-steps"></a>TOPdesk – az alkalmazás alkalmazással való integráció engedélyezése nyilvános, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon kattintson a bal oldali navigációs területen kattintson az **Active Directory**.

    ![Az Active Directory] (./media/active-directory-saas-topdesk-public-tutorial/IC700993.png "Az Active Directory")

2.  A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3.  Nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson a felső menüben **alkalmazásokat** .

    ![Alkalmazások] (./media/active-directory-saas-topdesk-public-tutorial/IC700994.png "Alkalmazások")

4.  Kattintson a **Hozzáadás** az oldal alján.

    ![Alkalmazás hozzáadása] (./media/active-directory-saas-topdesk-public-tutorial/IC749321.png "Alkalmazás hozzáadása")

5.  **Milyen feladatot szeretne tenni** párbeszédpanelen kattintson **a gyűjteményből az alkalmazások felvétele**.

    ![Gallerry az alkalmazás hozzáadása] (./media/active-directory-saas-topdesk-public-tutorial/IC749322.png "Gallerry az alkalmazás hozzáadása")

6.  A **Keresés mezőbe**írja be a **TOPdesk – nyilvános**.

    ![Alkalmazás gyűjtemény] (./media/active-directory-saas-topdesk-public-tutorial/IC790614.png "Alkalmazás gyűjtemény")

7.  Az eredmény ablaktáblában jelölje ki a **TOPdesk - nyilvános**, és válassza a **kész** , az alkalmazás hozzáadása parancsra.

    ![TOPdesk nyilvános] (./media/active-directory-saas-topdesk-public-tutorial/IC791317.png "TOPdesk nyilvános")

##<a name="configuring-single-sign-on"></a>Egyszeri bejelentkezés beállítása
  
Ez a szakasz célja engedélyezése a felhasználóknak, hogy TOPdesk – az összevonási alapján a SAML protokoll használatával Azure AD-fiókjukkal nyilvános hitelesíteni a szerkezet.  
Konfigurálása az egyszeri bejelentkezés TOPdesk – a nyilvánosság számára megköveteli embléma ikon fájl feltöltése. Úgy juthat az ikon fájl, a TOPdesk támogatási csoport munkatársaitól kaphat.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Egyszeri bejelentkezés beállításához hajtsa végre az alábbi lépéseket:

1.  Jelentkezzen be vállalat **TOPdesk – nyilvános** webhelyéhez rendszergazdaként.

2.  A **TOPdesk** menüben kattintson a **Beállítások**gombra.

    ![Beállítások] (./media/active-directory-saas-topdesk-public-tutorial/IC790598.png "Beállítások")

3.  Kattintson a **Login beállítások**gombra.

    ![Bejelentkezési beállításait] (./media/active-directory-saas-topdesk-public-tutorial/IC790599.png "Bejelentkezési beállításait")

4.  Bontsa ki a **Bejelentkezési beállítások** menüt, és válassza az **Általános**.

    ![Általános] (./media/active-directory-saas-topdesk-public-tutorial/IC790600.png "Általános")

5.  A **SAML bejelentkezési** konfigurációs szakasz **nyilvános** csoportjában hajtsa végre az alábbi lépéseket:

    ![Technikai beállítások] (./media/active-directory-saas-topdesk-public-tutorial/IC790601.png "Technikai beállítások")

    1.  Kattintson a **Letöltés** a metaadat-alapú nyilvános fájl letöltése gombra, és mentse azt helyileg a számítógépen.
    2.  Nyissa meg a metaadat-fájlt, és keresse meg a **AssertionConsumerService** csomópontot.
        ![AssertionConsumerService] (./media/active-directory-saas-topdesk-public-tutorial/IC790619.png "AssertionConsumerService")
    3.  Másolja a **AssertionConsumerService** értékét.  

        >[AZURE.NOTE] Az oktatóprogram később szüksége lesz az **App URL-címe konfigurálása** szakaszban található érték.

6.  A különböző webes böngészőablakban jelentkezzen be az **Azure klasszikus portál** rendszergazdaként.

7.  A **TOPdesk - nyilvános** alkalmazás integrációs lapon kattintson a **konfigurálása az egyszeri bejelentkezés** a **Konfigurálása az egyszeri bejelentkezés** párbeszédpanel megnyitásához.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-topdesk-public-tutorial/IC790620.png "Egyszeri bejelentkezés beállítása")

8.  **Hogyan szeretné, hogy jelentkezzen be az TOPdesk – nyilvános felhasználók** lapon jelölje be **A Microsoft Azure Active Directory Single Sign-On**, és kattintson a **Tovább gombra**.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-topdesk-public-tutorial/IC790621.png "Egyszeri bejelentkezés beállítása")

9.  Az **Alkalmazás URL-cím beállítása** lapon hajtsa végre az alábbi lépéseket:

    ![Állítsa be az App URL-címe] (./media/active-directory-saas-topdesk-public-tutorial/IC790622.png "Állítsa be az App URL-címe")

    1.  Az **TOPdesk – nyilvános bejelentkezési a URL-cím** mezőbe írja be a jelentkezhet be a TOPdesk – nyilvános alkalmazás a felhasználók által használt URL-CÍMÉT (például: "*https://qssolutions.topdesk.net*").
    2.  Az **TOPdesk – nyilvános válasz URL-címe** mezőbe illessze be a **TOPdesk – nyilvános AssertionConsumerService URL-CÍMÉT** (például: "*https://qssolutions.topdesk.net/tas/public/login/saml*")
    3.  Kattintson a **Tovább**gombra.

10. **Konfigurálása az egyszeri bejelentkezés TOPdesk – nyilvános a** lapon a metaadat-alapú fájl letöltéséhez kattintson a **metaadat-alapú letöltése**, és mentse a fájlt a számítógépen helyben parancsra.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-topdesk-public-tutorial/IC790623.png "Egyszeri bejelentkezés beállítása")

11. Biztonságitanúsítvány-fájl létrehozásához hajtsa végre az alábbi lépéseket:

    ![Tanúsítvány] (./media/active-directory-saas-topdesk-public-tutorial/IC790606.png "Tanúsítvány")

    1.  Nyissa meg a letöltött metaadat-fájlt.
    2.  Bontsa ki a **RoleDescriptor** csomópontot, amely tartalmazza az egy **xsi: type** **géppel: ApplicationServiceType**.
    3.  Másolja a **X509Certificate** csomópontot értékét.
    4.  Az egy fájlt a számítógépen helyben menteni a másolt **X509Certificate** értékét.

12. A TOPdesk - vállalat nyilvános webhely **TOPdesk** menüben kattintson a **Beállítások**gombra.

    ![Beállítások] (./media/active-directory-saas-topdesk-public-tutorial/IC790598.png "Beállítások")

13. Kattintson a **Login beállítások**gombra.

    ![Bejelentkezési beállításait] (./media/active-directory-saas-topdesk-public-tutorial/IC790599.png "Bejelentkezési beállításait")

14. Bontsa ki a **Bejelentkezési beállítások** menüt, és válassza az **Általános**.

    ![Általános] (./media/active-directory-saas-topdesk-public-tutorial/IC790600.png "Általános")

15. A **nyilvános** csoportban kattintson a **Hozzáadás**gombra.

    ![SAML bejelentkezés] (./media/active-directory-saas-topdesk-public-tutorial/IC790625.png "SAML bejelentkezés")

16. A **SAML konfigurációs Segéd** párbeszédpanel lapon hajtsa végre az alábbi lépéseket:

    ![SAML konfigurációs Segéd] (./media/active-directory-saas-topdesk-public-tutorial/IC790608.png "SAML konfigurációs Segéd")

    1.  Töltse fel a metaadat-alapú letöltött fájlt a **Metaadat-alapú összevonás**, kattintson a **Tallózás gombra**.
    2.  Töltse fel a tanúsítvány fájlt a **Tanúsítvány (RSA)**, kattintson a **Tallózás gombra**.
    3.  Töltse fel a embléma fájlt szerezte be a TOPdesk támogatási szolgálatához, az **embléma ikonra**, kattintson a **Tallózás gombra**.
    4.  Az **attribútum a felhasználó neve** mezőbe írja be a **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**.
    5.  A **megjelenítendő név** mezőbe írja be a konfigurációban nevét.
    6.  Kattintson a **Mentés**gombra.

17. Az Azure klasszikus portálon jelölje be a egyszeri bejelentkezéses konfigurációs megerősítés, és válassza a **kész** , **Állítsa be az egyszeri bejelentkezés** párbeszédpanel bezárásához.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-topdesk-public-tutorial/IC790627.png "Egyszeri bejelentkezés beállítása")

##<a name="configuring-user-provisioning"></a>Felhasználói kiépítési konfigurálása
  
Azure AD-felhasználóknak, hogy jelentkezzen be a TOPdesk – annak érdekében, hogy nyilvános TOPdesk – nyilvános be kell építenie.  
TOPdesk – Ha manuális feladata kiépítési nyilvános.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Felhasználói kiépítési beállításához hajtsa végre az alábbi lépéseket:

1.  Jelentkezzen be vállalat **TOPdesk – nyilvános** webhelyéhez rendszergazdaként.

2.  Kattintson a menü felső **TOPdesk \> új \> terméktámogatási fájlok \> személy**.

    ![Személy] (./media/active-directory-saas-topdesk-public-tutorial/IC790628.png "Személy")

3.  Az új személyt párbeszédpanelen hajtsa végre az alábbi lépéseket:

    ![Új személy] (./media/active-directory-saas-topdesk-public-tutorial/IC790629.png "Új személy")

    1.  Kattintson az Általános fülre.
    2.  A Vezetéknév mezőben lévő értéket írja be egy érvényes Azure Active Directory-fiókját, kiépítése kívánt utolsó nevét.
    3.  Válasszon egy **helyet** a fiókhoz.
    4.  Kattintson a **Mentés**gombra.

>[AZURE.NOTE] Használhatja a bármely más TOPdesk – nyilvános felhasználói fiók létrehozása eszközök, vagy TOPdesk - rendelkezést AAD felhasználói fiókok nyilvános által biztosított API-khoz.

##<a name="assigning-users"></a>Felhasználók hozzárendelése
  
Tesztelje a konfigurációt, meg kell adni az Azure Active Directory-felhasználók, használja az alkalmazás elérési útvonalát társításával engedélyezni szeretné.

###<a name="to-assign-users-to-topdesk---public-perform-the-following-steps"></a>Felhasználók hozzárendelése TOPdesk – nyilvános, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon próba-fiók létrehozása.

2.  A **TOPdesk – nyilvános **alkalmazás integrációs lapon kattintson a **hozzárendelését a felhasználókhoz**.

    ![Adhatnak a felhasználóknak] (./media/active-directory-saas-topdesk-public-tutorial/IC790630.png "Adhatnak a felhasználóknak")

3.  Jelölje ki a próba-felhasználó, kattintson a **hozzárendelni**, és kattintson az **Igen gombra** kattintva erősítse meg a hozzárendelés.

    ![Igen] (./media/active-directory-saas-topdesk-public-tutorial/IC767830.png "Igen")
  
Ha meg szeretné tesztelje a egyszeri bejelentkezéses, Access Panel megnyitásához. Ha többet szeretne tudni az Access panelen című témakörben [az Access-panelen](active-directory-saas-access-panel-introduction.md).