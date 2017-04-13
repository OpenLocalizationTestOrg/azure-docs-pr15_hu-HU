<properties 
    pageTitle="Oktatóprogram: Azure Active Directory-integráció a ArcGIS |} Microsoft Azure" 
    description="Megtudhatja, hogyan használhatja a ArcGIS az Azure Active Directory ahhoz, hogy az egyszeri bejelentkezés, automatikus kiépítési és az egyéb!" 
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

#<a name="tutorial-azure-active-directory-integration-with-arcgis"></a>Oktatóprogram: Azure Active Directory-integráció a ArcGIS

Ebben az oktatóanyagban célja integrálása az Azure és ArcGIS megjelenítéséhez. Az alkalmazási példát, ebben az oktatóanyagban tagolt feltételezi, hogy már rendelkezik az alábbiakat:

-   Érvényes Azure előfizetés
-   Egy ArcGIS egyszeri bejelentkezés engedélyezve van az előfizetés

Ebben az oktatóanyagban befejeztével az Azure Active Directory-felhasználók ArcGIS rendelt fogja tudni az alkalmazás a ArcGIS vállalati webhely (service provider által kezdeményezett bejelentkezés) és használatáról a [Bevezetés az Access panelen](active-directory-saas-access-panel-introduction.md)az egyszeri bejelentkezési.

A következő építőelemek az alkalmazási példát, ebben az oktatóanyagban tagolt áll:

1.  Az alkalmazás integrációját ArcGIS engedélyezése
2.  Egyszeri bejelentkezés beállítása
3.  Felhasználói kiépítési konfigurálása
4.  Felhasználók hozzárendelése

![Eset] (./media/active-directory-saas-arcgis-tutorial/IC784735.png "Eset")
##<a name="enabling-the-application-integration-for-arcgis"></a>Az alkalmazás integrációját ArcGIS engedélyezése

Ez a szakasz célja, hogyan ArcGIS az alkalmazás alkalmazással való integráció engedélyezése a szerkezet.

###<a name="to-enable-the-application-integration-for-arcgis-perform-the-following-steps"></a>Ahhoz, hogy az alkalmazás integrációját ArcGIS, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon kattintson a bal oldali navigációs területen kattintson az **Active Directory**.

    ![Az Active Directory] (./media/active-directory-saas-arcgis-tutorial/IC700993.png "Az Active Directory")

2.  A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3.  Nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson a felső menüben **alkalmazások** .

    ![Alkalmazások] (./media/active-directory-saas-arcgis-tutorial/IC700994.png "Alkalmazások")

4.  Kattintson a **Hozzáadás** az oldal alján.

    ![Alkalmazás hozzáadása] (./media/active-directory-saas-arcgis-tutorial/IC749321.png "Alkalmazás hozzáadása")

5.  **Milyen feladatot szeretne tenni** párbeszédpanelen kattintson **a gyűjteményből az alkalmazások felvétele**.

    ![Gallerry az alkalmazás hozzáadása] (./media/active-directory-saas-arcgis-tutorial/IC749322.png "Gallerry az alkalmazás hozzáadása")

6.  A **Keresés mezőbe**írja be a **ArcGIS**.

    ![Applcation gyűjtemény] (./media/active-directory-saas-arcgis-tutorial/IC784736.png "Applcation gyűjtemény")

7.  Az eredmény ablaktáblában jelölje ki a **ArcGIS**, és válassza a **kész** , az alkalmazás hozzáadása gombra.

    ![ArcGIS] (./media/active-directory-saas-arcgis-tutorial/IC784737.png "ArcGIS")
##<a name="configuring-single-sign-on"></a>Egyszeri bejelentkezés beállítása

Ez a szakasz célja tagolása engedélyezése a felhasználóknak, hogy ArcGIS hitelesíteni a fiókkal az összevonási alapján a SAML protokoll használatával Azure AD.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Egyszeri bejelentkezés beállításához hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon **ArcGIS** alkalmazás integrációs lapon kattintson **konfigurálása az egyszeri bejelentkezés** a **Konfigurálása az egyszeri bejelentkezés** párbeszédpanel megnyitásához.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-arcgis-tutorial/IC784738.png "Egyszeri bejelentkezés beállítása")

2.  **Hogyan szeretné, hogy jelentkezzen be az ArcGIS felhasználók** lapon jelölje be **A Microsoft Azure Active Directory Single Sign-On**, és kattintson a **Tovább gombra**.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-arcgis-tutorial/IC784739.png "Egyszeri bejelentkezés beállítása")

3.  A **App URL-címe beállítása** lapon az **ArcGIS bejelentkezési az URL** mezőbe írja be az URL-cím jelentkezzen be az alábbi minta "*https://company.maps.arcgis.com*" használatával a felhasználók által használt, és kattintson a **Tovább gombra**.

    ![Állítsa be az App URL-címe] (./media/active-directory-saas-arcgis-tutorial/IC784740.png "Állítsa be az App URL-címe")

4.  **Konfigurálása az egyszeri bejelentkezés ArcGIS a** lapon kattintson a **metaadat-alapú letöltése**elemre, és mentse a fájlt a számítógépen helyben metaadat.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-arcgis-tutorial/IC784741.png "Egyszeri bejelentkezés beállítása")

5.  A különböző webes böngészőablakban jelentkezzen be a ArcGIS vállalati webhely rendszergazdaként.

6.  Kattintson a **Beállítások szerkesztése**gombra.

    ![Beállításainak szerkesztése] (./media/active-directory-saas-arcgis-tutorial/IC784742.png "Beállításainak szerkesztése")

7.  Kattintson a **Biztonság**gombra.

    ![Biztonsági] (./media/active-directory-saas-arcgis-tutorial/IC784743.png "Biztonsági")

8.  A **Vállalati bejelentkezések**kattintson a **Identitásszolgáltató beállítása**.

    ![Vállalati bejelentkezések] (./media/active-directory-saas-arcgis-tutorial/IC784744.png "Vállalati bejelentkezések")

9.  A **Set identitásszolgáltató** beállítása lapon hajtsa végre az alábbi lépéseket:

    ![Identitásszolgáltató beállítása] (./media/active-directory-saas-arcgis-tutorial/IC784745.png "Identitásszolgáltató beállítása")

    1.  Neve mezőbe írja be a szervezet nevére.
    2.  **Használatával a vállalati identitásszolgáltató metaadatait fog megadott**válassza ki azt **A fájlt**.
    3.  A metaadatok letöltött fájl feltöltéséhez kattintson a **fájl kiválasztása**.
    4.  Kattintson a **beállítás identitásszolgáltató**.

10. Az Azure klasszikus portálon jelölje be a egyszeri bejelentkezéses konfigurációs megerősítés, és válassza a **kész** , **Állítsa be az egyszeri bejelentkezés** párbeszédpanel bezárásához.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-arcgis-tutorial/IC784746.png "Egyszeri bejelentkezés beállítása")
##<a name="configuring-user-provisioning"></a>Felhasználói kiépítési konfigurálása

Ahhoz, hogy az Azure Active Directory-felhasználóknak, hogy jelentkezzen be a ArcGIS, hogy ki kell építenie ArcGIS be.  
ArcGIS, ha a feladat manuális kiépítési.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Felhasználói kiépítési beállításához hajtsa végre az alábbi lépéseket:

1.  Jelentkezzen be az **ArcGIS** bérlői webhelyen.

2.  Kattintson a **tagok felkérése**gombra.

    ![A tagok meghívása] (./media/active-directory-saas-arcgis-tutorial/IC784747.png "A tagok meghívása")

3.  Jelölje ki a **Tagok hozzáadása automatikusan a Küldés e-mailben nélkül**, és kattintson a **Tovább gombra**.

    ![Automatikusan a tagok hozzáadása] (./media/active-directory-saas-arcgis-tutorial/IC784748.png "Automatikusan a tagok hozzáadása")

4.  A **tagok** párbeszédpanel lapon hajtsa végre az alábbi lépéseket:

    ![Hozzáadás és véleményezés] (./media/active-directory-saas-arcgis-tutorial/IC784749.png "Hozzáadás és véleményezés")

    1.  Írja be a **Vezetéknév**és **Vezetéknevet** , valamint **e-mailben** , amelyet érvényes AAD fiók kiépítése.
    2.  Kattintson a gombra, **és nyomon**.

5.  Tekintse át az adatokat, adta meg, és kattintson a **Tagok hozzáadása**gombra.

    ![Tag felvétele] (./media/active-directory-saas-arcgis-tutorial/IC784750.png "Tag felvétele")

>[AZURE.NOTE] Bármely más ArcGIS felhasználói fiók létrehozása eszközöket is használhatja, illetve ArcGIS rendelkezést AAD felhasználói fiókok által biztosított API-khoz.

##<a name="assigning-users"></a>Felhasználók hozzárendelése

Tesztelje a konfigurációt, meg kell adni az Azure Active Directory-felhasználók, használja az alkalmazás elérési útvonalát társításával engedélyezni szeretné.

###<a name="to-assign-users-to-arcgis-perform-the-following-steps"></a>Felhasználók hozzárendelése ArcGIS, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon próba-fiók létrehozása.

2.  Kattintson a **ArcGIS **alkalmazás integrációs lap **hozzárendelését a felhasználókhoz**.

    ![Hozzárendelését a felhasználókhoz] (./media/active-directory-saas-arcgis-tutorial/IC784751.png "Adhatnak a felhasználóknak")

3.  Jelölje ki a próba-felhasználó, kattintson a **hozzárendelni**, és kattintson az **Igen gombra** kattintva erősítse meg a hozzárendelés.

    ![Igen] (./media/active-directory-saas-arcgis-tutorial/IC767830.png "Igen")

Ha meg szeretné tesztelje a egyszeri bejelentkezéses, Access Panel megnyitásához. Az Access ablaktábla, lásd: [Bevezetés az Access panelre](active-directory-saas-access-panel-introduction.md).
