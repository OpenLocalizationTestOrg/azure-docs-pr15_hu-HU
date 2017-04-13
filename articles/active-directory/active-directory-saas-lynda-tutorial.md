<properties 
    pageTitle="Oktatóprogram: Azure Active Directory-integráció a Lynda.com |} Microsoft Azure" 
    description="Megtudhatja, hogyan használhatja a Lynda.com az Azure Active Directory ahhoz, hogy az egyszeri bejelentkezés, automatikus kiépítési és az egyéb!" 
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

#<a name="tutorial-azure-active-directory-integration-with-lyndacom"></a>Oktatóprogram: Azure Active Directory-integráció a Lynda.com
  
Ebben az oktatóanyagban célja integrálása az Azure és Lynda.com megjelenítéséhez.  
Az alkalmazási példát, ebben az oktatóanyagban tagolt feltételezi, hogy már rendelkezik az alábbiakat:

-   Érvényes Azure előfizetés
-   Lynda.com bérlői webhelyre
  
Ebben az oktatóanyagban befejeztével az Azure Active Directory-felhasználók Lynda.com rendelt fogja tudni az alkalmazás a Lynda.com vállalati webhely (a szolgáltatás által kezdeményezett szolgáltató bejelentkezési) és használatáról a [Bevezetés az Access panelen](active-directory-saas-access-panel-introduction.md)az egyszeri bejelentkezési.
  
A következő építőelemek az alkalmazási példát, ebben az oktatóanyagban tagolt áll:

1.  Az alkalmazás integrációját Lynda.com engedélyezése
2.  Egyszeri bejelentkezés beállítása
3.  Felhasználói kiépítési konfigurálása
4.  Felhasználók hozzárendelése

![Eset] (./media/active-directory-saas-lynda-tutorial/IC781046.png "Eset")
##<a name="enabling-the-application-integration-for-lyndacom"></a>Az alkalmazás integrációját Lynda.com engedélyezése
  
Ez a szakasz célja, hogyan Lynda.com az alkalmazás alkalmazással való integráció engedélyezése a szerkezet.

###<a name="to-enable-the-application-integration-for-lyndacom-perform-the-following-steps"></a>Ahhoz, hogy az alkalmazás integrációját Lynda.com, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon kattintson a bal oldali navigációs területen kattintson az **Active Directory**.

    ![Az Active Directory] (./media/active-directory-saas-lynda-tutorial/IC700993.png "Az Active Directory")

2.  A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3.  Nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson a felső menüben **alkalmazások** .

    ![Alkalmazások] (./media/active-directory-saas-lynda-tutorial/IC700994.png "Alkalmazások")

4.  Kattintson a **Hozzáadás** az oldal alján.

    ![Alkalmazás hozzáadása] (./media/active-directory-saas-lynda-tutorial/IC749321.png "Alkalmazás hozzáadása")

5.  **Milyen feladatot szeretne tenni** párbeszédpanelen kattintson **a gyűjteményből az alkalmazások felvétele**.

    ![Gallerry az alkalmazás hozzáadása] (./media/active-directory-saas-lynda-tutorial/IC749322.png "Gallerry az alkalmazás hozzáadása")

6.  A **Keresés mezőbe**írja be a **Lynda.com**.

    ![Alkalmazás gyűjtemény] (./media/active-directory-saas-lynda-tutorial/IC777524.png "Alkalmazás gyűjtemény")

7.  Az eredmény ablaktáblában jelölje ki a **Lynda.com**, és válassza a **kész** , az alkalmazás hozzáadása gombra.

    ![Lynda.com] (./media/active-directory-saas-lynda-tutorial/IC777525.png "Lynda.com")
##<a name="configuring-single-sign-on"></a>Egyszeri bejelentkezés beállítása
  
Ez a szakasz célja tagolása engedélyezése a felhasználóknak, hogy Lynda.com hitelesíteni a fiókkal az összevonási alapján a SAML protokoll használatával Azure AD.

>[AZURE.IMPORTANT]Ahhoz, hogy a egyszeri bejelentkezés a Lynda.com bérlő konfigurálása, kell először lépjen kapcsolatba az első ezzel a funkcióval a Lynda.com technikai támogatási.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Egyszeri bejelentkezés beállításához hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon **Lynda.com** alkalmazás integrációs lapon kattintson **konfigurálása az egyszeri bejelentkezés** a **Konfigurálása az egyszeri bejelentkezés** párbeszédpanel megnyitásához.

    ![Beállítás az egyszeri bejelentkezés] (./media/active-directory-saas-lynda-tutorial/IC777526.png "Beállítás az egyszeri bejelentkezés")

2.  **Hogyan szeretné, hogy jelentkezzen be az Lynda.com felhasználók** lapon jelölje be **A Microsoft Azure Active Directory Single Sign-On**, és kattintson a **Tovább gombra**.

    ![Beállítás az egyszeri bejelentkezés] (./media/active-directory-saas-lynda-tutorial/IC777527.png "Beállítás az egyszeri bejelentkezés")

3.  Az **Alkalmazás URL-cím beállítása** lapon az **Lynda.com bejelentkezési az URL** mezőbe írja be a Lynda.com bérlői webhely URL-címet (például: *https://shib.lynda.com/Shibboleth.sso/InCommon?providerId=https://sts.windows-ppe.net/6247032d-9415-403c-b72b-277e3fb6f2c8/&target= https://shib.lynda.com/InCommon*), és kattintson a **Tovább gombra**.

    ![Állítsa be az app URL-címe] (./media/active-directory-saas-lynda-tutorial/IC781047.png "Állítsa be az app URL-címe")

4.  **Konfigurálása az egyszeri bejelentkezés Lynda.com a** lapon a metaadat-alapú letöltéséhez kattintson a **metaadat-alapú letöltése**, és mentse a tanúsítvány fájlt a számítógépen helyben parancsra.

    ![Beállítás az egyszeri bejelentkezés] (./media/active-directory-saas-lynda-tutorial/IC777529.png "Beállítás az egyszeri bejelentkezés")

5.  A metaadatok letöltött fájl küldése a Lynda.com támogatási csoportnak. A Lynda.com támogatási csoport meg az egyszeri bejelentkezés konfigurációs tartalmaz.

6.  Az Azure klasszikus portálon jelölje be a egyszeri bejelentkezéses konfigurációs megerősítés, és válassza a **kész** , **Állítsa be az egyszeri bejelentkezés** párbeszédpanel bezárásához.

    ![Beállítás az egyszeri bejelentkezés] (./media/active-directory-saas-lynda-tutorial/IC777530.png "Beállítás az egyszeri bejelentkezés")
##<a name="configuring-user-provisioning"></a>Felhasználói kiépítési konfigurálása
  
Nincs teendő konfigurálható kiépítési Lynda.com a felhasználó nem.  
Egy tevékenységhez rendelt felhasználó megkísérel jelentkezzen be a hozzáférés panelen Lynda.com, a Lynda.com ellenőrzi, hogy létezik-e a felhasználó.  
Ha nincs még nincs felhasználói fiók érhető el, a Lynda.com automatikusan létrehozott.

>[AZURE.NOTE]Bármely más Lynda.com felhasználói fiók létrehozása eszközöket is használhatja, illetve Lynda.com rendelkezést AAD felhasználói fiókok által biztosított API-khoz.

##<a name="assigning-users"></a>Felhasználók hozzárendelése
  
Tesztelje a konfigurációt, meg kell adni az Azure Active Directory-felhasználók, használja az alkalmazás elérési útvonalát társításával engedélyezni szeretné.

###<a name="to-assign-users-to-lyndacom-perform-the-following-steps"></a>Felhasználók hozzárendelése Lynda.com, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon próba-fiók létrehozása.

2.  Kattintson a **Lynda.com **alkalmazás integrációs lap **hozzárendelését a felhasználókhoz**.

    ![Adhatnak a felhasználóknak] (./media/active-directory-saas-lynda-tutorial/IC777531.png "Adhatnak a felhasználóknak")

3.  Jelölje ki a próba-felhasználó, kattintson a **hozzárendelni**, és kattintson az **Igen gombra** kattintva erősítse meg a hozzárendelés.

    ![Igen] (./media/active-directory-saas-lynda-tutorial/IC767830.png "Igen")
  
Ha meg szeretné tesztelje a egyszeri bejelentkezéses, Access Panel megnyitásához. Az Access ablaktábla, lásd: [Bevezetés az Access panelre](active-directory-saas-access-panel-introduction.md).