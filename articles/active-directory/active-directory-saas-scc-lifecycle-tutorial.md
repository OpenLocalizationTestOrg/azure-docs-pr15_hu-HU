<properties 
    pageTitle="Oktatóprogram: Azure Active Directory-integráció a SCC életciklusra |} Microsoft Azure" 
    description="Megtudhatja, hogy miként SCC életciklus használata az Azure Active Directory ahhoz, hogy az egyszeri bejelentkezés, automatikus kiépítési és az egyéb!" 
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

#<a name="tutorial-azure-active-directory-integration-with-scc-lifecycle"></a>Oktatóprogram: Azure Active Directory-integráció a SCC életciklus
  
Ebben az oktatóanyagban célja Azure és SCC életciklus integrációja megjelenítéséhez.  
Az alkalmazási példát, ebben az oktatóanyagban tagolt feltételezi, hogy már rendelkezik az alábbiakat:

-   Érvényes Azure előfizetés
-   SCC életciklus egyszeri bejelentkezéses engedélyezett előfizetés
  
Ebben az oktatóanyagban befejeztével az Azure Active Directory felhasználók SCC életciklus rendelt tudnak egyszeri bejelentkezés a SCC életciklus vállalati webhely (a szolgáltatás által kezdeményezett szolgáltató bejelentkezési) vagy használatáról a [Bevezetés az Access panelen](active-directory-saas-access-panel-introduction.md)az alkalmazásba.
  
A következő építőelemek az alkalmazási példát, ebben az oktatóanyagban tagolt áll:

1.  Az alkalmazás integrációját SCC életciklusra engedélyezése
2.  Egyszeri bejelentkezés beállítása
3.  Felhasználói kiépítési konfigurálása
4.  Felhasználók hozzárendelése

![Eset] (./media/active-directory-saas-scc-lifecycle-tutorial/IC794120.png "Eset")
##<a name="enabling-the-application-integration-for-scc-lifecycle"></a>Az alkalmazás integrációját SCC életciklusra engedélyezése
  
Ez a szakasz célja, hogyan SCC életciklus az alkalmazás alkalmazással való integráció engedélyezése a szerkezet.

###<a name="to-enable-the-application-integration-for-scc-lifecycle-perform-the-following-steps"></a>Ahhoz, hogy az alkalmazás integrációját SCC életciklus, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon kattintson a bal oldali navigációs területen kattintson az **Active Directory**.

    ![Az Active Directory] (./media/active-directory-saas-scc-lifecycle-tutorial/IC700993.png "Az Active Directory")

2.  A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3.  Nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson a felső menüben **alkalmazások** .

    ![Alkalmazások] (./media/active-directory-saas-scc-lifecycle-tutorial/IC700994.png "Alkalmazások")

4.  Kattintson a **Hozzáadás** az oldal alján.

    ![Alkalmazás hozzáadása] (./media/active-directory-saas-scc-lifecycle-tutorial/IC749321.png "Alkalmazás hozzáadása")

5.  **Milyen feladatot szeretne tenni** párbeszédpanelen kattintson **a gyűjteményből az alkalmazások felvétele**.

    ![Gallerry az alkalmazás hozzáadása] (./media/active-directory-saas-scc-lifecycle-tutorial/IC749322.png "Gallerry az alkalmazás hozzáadása")

6.  A **Keresés mezőbe**írja be a **SCC életciklusra**.

    ![Alkalmazás gyűjtemény] (./media/active-directory-saas-scc-lifecycle-tutorial/IC794121.png "Alkalmazás gyűjtemény")

7.  Az eredmény ablaktáblában jelölje ki a **SCC életciklusra**, és válassza a **kész** , az alkalmazás hozzáadása gombra.

    ![SCC életciklus] (./media/active-directory-saas-scc-lifecycle-tutorial/IC795082.png "SCC életciklus")
##<a name="configuring-single-sign-on"></a>Egyszeri bejelentkezés beállítása
  
Ez a szakasz célja tagolása engedélyezése a felhasználóknak, hogy SCC életciklus hitelesíteni a fiókkal az összevonási alapján a SAML protokoll használatával Azure AD.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Egyszeri bejelentkezés beállításához hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon **SCC életciklus** alkalmazás integrációs lapon kattintson **konfigurálása az egyszeri bejelentkezés** a **Konfigurálása az egyszeri bejelentkezés** párbeszédpanel megnyitásához.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-scc-lifecycle-tutorial/IC794122.png "Egyszeri bejelentkezés beállítása")

2.  **Hogyan szeretné, hogy jelentkezzen be az SCC életciklus felhasználók** lapon jelölje be **A Microsoft Azure Active Directory Single Sign-On**, és kattintson a **Tovább gombra**.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-scc-lifecycle-tutorial/IC794123.png "Egyszeri bejelentkezés beállítása")

3.  Az **Alkalmazás URL-cím beállítása** lapon, a **Bejelentkezési a URL-cím** mezőbe írja be a SCC életciklus-alkalmazást a következő mintát "*https://bs1.scc.com/lc7/welcome/customer/PICTtest.aspx*" bejelentkezni a felhasználók által használt URL-CÍMÉT, és kattintson a **Tovább gombra**.

    ![Állítsa be az App URL-címe] (./media/active-directory-saas-scc-lifecycle-tutorial/IC794124.png "Állítsa be az App URL-címe")

4.  **Konfigurálása az egyszeri bejelentkezés SCC életciklus a** lapon kattintson a **metaadat-alapú letöltése**elemre, és mentse a fájlt a számítógépen helyben metaadat.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-scc-lifecycle-tutorial/IC795083.png "Egyszeri bejelentkezés beállítása")

5.  A metaadat-alapú fájl SCC életciklusra támogatási csoportjának továbbítja.

    >[AZURE.NOTE]Egyszeri bejelentkezés a SCC életciklus-támogatási csoport által engedélyezésük tartalmaz.

6.  Az Azure klasszikus portálon jelölje be a egyszeri bejelentkezéses konfigurációs megerősítés, és válassza a **kész** , **Állítsa be az egyszeri bejelentkezés** párbeszédpanel bezárásához.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-scc-lifecycle-tutorial/IC794125.png "Egyszeri bejelentkezés beállítása")
##<a name="configuring-user-provisioning"></a>Felhasználói kiépítési konfigurálása
  
Ahhoz, hogy az Azure Active Directory-felhasználók SCC életciklus való bejelentkezéshez, azok ki kell építenie SCC életciklus be.
  
Nem állíthatja be a felhasználó létesítése való SCC életciklus-nincs teendő.  
Amikor egy kiosztott felhasználó megkísérel bejelentkezni a SCC életciklus, SCC életciklus-fiók automatikusan létrejön, szükség esetén.

>[AZURE.NOTE]Bármely más SCC életciklusra vonatkozó felhasználói fiók létrehozása eszközöket is használhatja, illetve SCC életciklusra rendelkezést AAD felhasználói fiókok által biztosított API-khoz.

##<a name="assigning-users"></a>Felhasználók hozzárendelése
  
Tesztelje a konfigurációt, meg kell adni az Azure Active Directory-felhasználók, használja az alkalmazás elérési útvonalát társításával engedélyezni szeretné.

###<a name="to-assign-users-to-scc-lifecycle-perform-the-following-steps"></a>Felhasználók hozzárendelése SCC életciklus, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon próba-fiók létrehozása.

2.  A **SCC életciklus **alkalmazás integrációs lapon kattintson a **hozzárendelését a felhasználókhoz**.

    ![Hozzárendelését a felhasználókhoz] (./media/active-directory-saas-scc-lifecycle-tutorial/IC794126.png "Adhatnak a felhasználóknak")

3.  Jelölje ki a próba-felhasználó, kattintson a **hozzárendelni**, és kattintson az **Igen gombra** kattintva erősítse meg a hozzárendelés.

    ![Igen] (./media/active-directory-saas-scc-lifecycle-tutorial/IC767830.png "Igen")
  
Ha meg szeretné tesztelje a egyszeri bejelentkezéses, Access Panel megnyitásához. Az Access ablaktábla, lásd: [Bevezetés az Access panelre](active-directory-saas-access-panel-introduction.md).