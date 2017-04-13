<properties 
    pageTitle="Oktatóprogram: Azure Active Directory-integráció a Abintegro |} Microsoft Azure" 
    description="Megtudhatja, hogyan használhatja a Abintegro az Azure Active Directory ahhoz, hogy az egyszeri bejelentkezés, automatikus kiépítési és az egyéb!" 
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

#<a name="tutorial-azure-active-directory-integration-with-abintegro"></a>Oktatóprogram: Azure Active Directory-integráció a Abintegro

Ebben az oktatóanyagban célja integrálása az Azure és Abintegro megjelenítéséhez.  
Az alkalmazási példát, ebben az oktatóanyagban tagolt feltételezi, hogy már rendelkezik az alábbiakat:

-   Érvényes Azure előfizetés
-   Egy Abintegro egyszeri bejelentkezés engedélyezve van az előfizetés

Ebben az oktatóanyagban befejeztével az Azure Active Directory-felhasználók Abintegro rendelt fogja tudni az alkalmazás a Abintegro vállalati webhely (service provider által kezdeményezett bejelentkezés) és használatáról a [Bevezetés az Access panelen](active-directory-saas-access-panel-introduction.md)az egyszeri bejelentkezési.

Az alábbi építőelemeket az alkalmazási példát, ebben az oktatóanyagban tagolt áll:

1.  Az alkalmazás integrációját Abintegro engedélyezése
2.  Egyszeri bejelentkezés beállítása
3.  Felhasználói kiépítési konfigurálása
4.  Felhasználók hozzárendelése

![Eset] (./media/active-directory-saas-abintegro-tutorial/IC790076.png "Eset")
##<a name="enabling-the-application-integration-for-abintegro"></a>Az alkalmazás integrációját Abintegro engedélyezése

Ez a szakasz célja, hogyan Abintegro az alkalmazás alkalmazással való integráció engedélyezése a szerkezet.

###<a name="to-enable-the-application-integration-for-abintegro-perform-the-following-steps"></a>Ahhoz, hogy az alkalmazás integrációját Abintegro, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon kattintson a bal oldali navigációs területen kattintson az **Active Directory**.

    ![Az Active Directory] (./media/active-directory-saas-abintegro-tutorial/IC700993.png "Az Active Directory")

2.  A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3.  Nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson a felső menüben **alkalmazások** .

    ![Alkalmazások] (./media/active-directory-saas-abintegro-tutorial/IC700994.png "Alkalmazások")

4.  Kattintson a **Hozzáadás** az oldal alján.

    ![Alkalmazás hozzáadása] (./media/active-directory-saas-abintegro-tutorial/IC749321.png "Alkalmazás hozzáadása")

5.  **Milyen feladatot szeretne tenni** párbeszédpanelen kattintson **a gyűjteményből az alkalmazások felvétele**.

    ![Gallerry az alkalmazás hozzáadása] (./media/active-directory-saas-abintegro-tutorial/IC749322.png "Gallerry az alkalmazás hozzáadása")

6.  A **Keresés mezőbe**írja be a **abintegro**.

    ![Alkalmazás gyűjtemény] (./media/active-directory-saas-abintegro-tutorial/IC790077.png "Alkalmazás gyűjtemény")

7.  Az eredmény ablaktáblában jelölje ki a **Abintegro**, és válassza a **kész** , az alkalmazás hozzáadása gombra.

    ![Abintegro] (./media/active-directory-saas-abintegro-tutorial/IC790078.png "Abintegro")
##<a name="configuring-single-sign-on"></a>Egyszeri bejelentkezés beállítása

Ez a szakasz célja tagolása engedélyezése a felhasználóknak, hogy Abintegro hitelesíteni a fiókkal az összevonási alapján a SAML protokoll használatával Azure AD.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Egyszeri bejelentkezés beállításához hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon **Abintegro** alkalmazás integrációs lapon kattintson **konfigurálása az egyszeri bejelentkezés** a **Konfigurálása az egyszeri bejelentkezés** párbeszédpanel megnyitásához.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-abintegro-tutorial/IC790079.png "Egyszeri bejelentkezés beállítása")

2.  **Hogyan szeretné, hogy jelentkezzen be az Abintegro felhasználók** lapon jelölje be **A Microsoft Azure Active Directory Single Sign-On**, és kattintson a **Tovább gombra**.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-abintegro-tutorial/IC790080.png "Egyszeri bejelentkezés beállítása")

3.  Az **Alkalmazás URL-cím beállítása** lapon az **URL a bejelentkezési Abintegro** mezőbe írja be a Abintegro bejelentkezni a felhasználók által használt URL-CÍMÉT (például: `https://dev.abintegro.com/Shibboleth.sso/Login?entityID=<Issuer>&target=https://dev.abintegro.com/secure/`), majd kattintson a **Tovább**gombra.

    ![Állítsa be az App URL-címe] (./media/active-directory-saas-abintegro-tutorial/IC790081.png "Állítsa be az App URL-címe")

4.  **Konfigurálása az egyszeri bejelentkezés a Abintegro** lapon kattintson a **metaadat-alapú letöltése**elemre, és mentse a fájlt a számítógépen a metaadat.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-abintegro-tutorial/IC790082.png "Egyszeri bejelentkezés beállítása")

5.  Küldje el a metadatafile a Abintegro részlegtől.

    >[AZURE.NOTE] Egyszeri bejelentkezés konfiguráció a Abintegro támogatási csoportja által végrehajtandó tartalmaz. Értesítést kaphat arról, amint a konfiguráció befejeződött.

6.  Az Azure klasszikus portálon jelölje be a egyszeri bejelentkezéses konfigurációs megerősítés, és válassza a **kész** , **Állítsa be az egyszeri bejelentkezés** párbeszédpanel bezárásához.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-abintegro-tutorial/IC790083.png "Egyszeri bejelentkezés beállítása")
##<a name="configuring-user-provisioning"></a>Felhasználói kiépítési konfigurálása

Nincs teendő konfigurálható kiépítési Abintegro a felhasználó nem.  
Egy tevékenységhez rendelt felhasználó megkísérel jelentkezzen be a hozzáférés panelen Abintegro, a Abintegro ellenőrzi, hogy létezik-e a felhasználó.  
Ha nincs még nincs felhasználói fiók érhető el, a Abintegro automatikusan létrehozott.
##<a name="assigning-users"></a>Felhasználók hozzárendelése

Tesztelje a konfigurációt, meg kell adni az Azure Active Directory-felhasználók, használja az alkalmazás elérési útvonalát társításával engedélyezni szeretné.

###<a name="to-assign-users-to-abintegro-perform-the-following-steps"></a>Felhasználók hozzárendelése Abintegro, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon próba-fiók létrehozása.

2.  Kattintson a **Abintegro **alkalmazás integrációs lap **hozzárendelését a felhasználókhoz**.

    ![Adhatnak a felhasználóknak] (./media/active-directory-saas-abintegro-tutorial/IC790084.png "Adhatnak a felhasználóknak")

3.  Jelölje ki a próba-felhasználó, kattintson a **hozzárendelni**, és kattintson az **Igen gombra** kattintva erősítse meg a hozzárendelés.

    ![Igen] (./media/active-directory-saas-abintegro-tutorial/IC767830.png "Igen")

Ha meg szeretné tesztelje a egyszeri bejelentkezéses, Access Panel megnyitásához. Az Access ablaktábla, lásd: [Bevezetés az Access panelre](active-directory-saas-access-panel-introduction.md).
