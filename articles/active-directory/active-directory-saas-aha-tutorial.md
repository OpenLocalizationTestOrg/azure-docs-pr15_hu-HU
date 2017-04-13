<properties 
    pageTitle="Oktatóprogram: Azure Active Directory integrációja Aha! | Microsoft Azure" 
    description="Megtudhatja, hogy miként használhatja a Aha! az Azure Active Directory ahhoz, hogy az egyszeri bejelentkezés automatikus a kiépítési, és így tovább!" 
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

#<a name="tutorial-azure-active-directory-integration-with-aha"></a>Oktatóprogram: Azure Active Directory integrációja Aha!

Ebben az oktatóprogramban az a célja, hogy integrálása az Azure és Aha megjelenítése!  
Az alkalmazási példát, ebben az oktatóanyagban tagolt feltételezi, hogy már rendelkezik az alábbiakat:

-   Érvényes Azure előfizetés
-   Egy és tényleg! egyszeri bejelentkezés engedélyezett előfizetés

Miután elkészült a ebben az oktatóanyagban, az Azure Active Directory-felhasználók rendelt Aha! tudják az alkalmazásba, a Aha egyszeri bejelentkezési! Vállalati webhely (service provider által kezdeményezett bejelentkezés) és használatáról a [Bevezetés az Access panelen](active-directory-saas-access-panel-introduction.md).

A következő építőelemek az alkalmazási példát, ebben az oktatóanyagban tagolt áll:

1.  Az alkalmazás integrációját Aha engedélyezése!
2.  Egyszeri bejelentkezés beállítása
3.  Felhasználói kiépítési konfigurálása
4.  Felhasználók hozzárendelése

![Eset] (./media/active-directory-saas-aha-tutorial/IC798944.png "Eset")
##<a name="enabling-the-application-integration-for-aha"></a>Az alkalmazás integrációját Aha engedélyezése!

Ez a szakasz célja hogyan Aha az alkalmazás alkalmazással való integráció engedélyezése a szerkezet!.

###<a name="to-enable-the-application-integration-for-aha-perform-the-following-steps"></a>Ahhoz, hogy az alkalmazás integrációját Aha!, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon kattintson a bal oldali navigációs területen kattintson az **Active Directory**.

    ![Az Active Directory] (./media/active-directory-saas-aha-tutorial/IC700993.png "Az Active Directory")

2.  A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3.  Nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson a felső menüben **alkalmazások** .

    ![Alkalmazások] (./media/active-directory-saas-aha-tutorial/IC700994.png "Alkalmazások")

4.  Kattintson a **Hozzáadás** az oldal alján.

    ![Alkalmazás hozzáadása] (./media/active-directory-saas-aha-tutorial/IC749321.png "Alkalmazás hozzáadása")

5.  **Milyen feladatot szeretne tenni** párbeszédpanelen kattintson **a gyűjteményből az alkalmazások felvétele**.

    ![Gallerry az alkalmazás hozzáadása] (./media/active-directory-saas-aha-tutorial/IC749322.png "Gallerry az alkalmazás hozzáadása")

6.  A **Keresés mezőbe**írja be a **Aha!**.

    ![Alkalmazás gyűjtemény] (./media/active-directory-saas-aha-tutorial/IC798945.png "Alkalmazás gyűjtemény")

7.  Az eredmény ablaktáblában válassza a **Aha!**, és kattintson a **kész** , az alkalmazás hozzáadása.

    ![És tényleg!] (./media/active-directory-saas-aha-tutorial/IC802746.png "És tényleg!")
##<a name="configuring-single-sign-on"></a>Egyszeri bejelentkezés beállítása

Ez a szakasz célja engedélyezése a felhasználóknak, hogy Aha hitelesíteni a szerkezet! a fiók Azure AD-összevonási alapján a SAML protokoll használatával.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Egyszeri bejelentkezés beállításához hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon kattintson a **Aha!** alkalmazás integrálását lapján kattintson a **konfigurálása az egyszeri bejelentkezés** a **Konfigurálása az egyszeri bejelentkezés** párbeszédpanel megnyitásához.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-aha-tutorial/IC798946.png "Egyszeri bejelentkezés beállítása")

2.  Kattintson a **hogyan szeretné, hogy jelentkezzen be az Aha felhasználók!** lap, válassza a **Microsoft Azure Active Directory Single Sign-On**, és kattintson a **Tovább gombra**.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-aha-tutorial/IC798947.png "Egyszeri bejelentkezés beállítása")

3.  Az **Alkalmazás URL-címet konfigurálni** lapon a **Aha! Jelentkezzen be URL-cím** szövegdoboz, írja be az URL-címet használja a felhasználók bejelentkezésre a Aha! Alkalmazás (például: "*https://company.aha.io/session/new*"), majd kattintson a **Tovább**gombra.

    ![Állítsa be az App URL-címe] (./media/active-directory-saas-aha-tutorial/IC798948.png "Állítsa be az App URL-címe")

4.  Kattintson a **konfigurálni az egyszeri bejelentkezés Aha!** lap, töltse le a metaadat-fájlt, kattintson a **metaadat-alapú letöltése**és mentse a fájlt a számítógépen helyben metaadat.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-aha-tutorial/IC798949.png "Egyszeri bejelentkezés beállítása")

5.  A különböző webes böngészőablakban jelentkezzen be a Aha! Vállalati webhely rendszergazdaként.

6.  A felső sávon kattintson a **Beállítások**gombra.

    ![Beállítások] (./media/active-directory-saas-aha-tutorial/IC798950.png "Beállítások")

7.  Kattintson a **fiókra**.

    ![Profil] (./media/active-directory-saas-aha-tutorial/IC798951.png "Profil")

8.  Kattintson a **Biztonság és az egyszeri bejelentkezés**.

    ![Biztonság és az egyszeri bejelentkezés] (./media/active-directory-saas-aha-tutorial/IC798952.png "Biztonság és az egyszeri bejelentkezés")

9.  **Egyszeri bejelentkezés** csoportban az **Identitásszolgáltató**, jelölje ki a **SAML2.0**.

    ![Biztonság és az egyszeri bejelentkezés] (./media/active-directory-saas-aha-tutorial/IC798953.png "Biztonság és az egyszeri bejelentkezés")

10. Az **Egyszeri bejelentkezés** beállítása lapon hajtsa végre az alábbi lépéseket:

    ![Egyszeri bejelentkezés] (./media/active-directory-saas-aha-tutorial/IC798954.png "Egyszeri bejelentkezés")

    1.  Az **neve** mezőbe írja be a konfigurációban nevét.
    2.  A **beállítás használatával**jelölje ki a **Metaadat-fájlt**.
    3.  A metaadatok letöltött fájl feltöltéséhez kattintson a **Tallózás gombra**.
    4.  Kattintson a **frissítés**gombra.

11. Az Azure klasszikus portálon jelölje be a egyszeri bejelentkezéses konfigurációs megerősítés, és válassza a **kész** , **Állítsa be az egyszeri bejelentkezés** párbeszédpanel bezárásához.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-aha-tutorial/IC798955.png "Egyszeri bejelentkezés beállítása")
##<a name="configuring-user-provisioning"></a>Felhasználói kiépítési konfigurálása

Ahhoz, hogy Azure AD-felhasználóknak, hogy jelentkezzen be a Aha!, Aha be kell építenie!.  
Ha a Aha!, az automatizált feladat kiépítési.  
Nincs teendő meg nem.
  
Felhasználók automatikusan létrejön egy szükség esetén az első egyszeri bejelentkezéses kísérlet során.

>[AZURE.NOTE] Bármely más Aha használható! felhasználói fiók létrehozása eszközök vagy API-khoz Aha által biztosított! hozhatók létre AAD felhasználói fiókok.

##<a name="assigning-users"></a>Felhasználók hozzárendelése

Tesztelje a konfigurációt, meg kell adni az Azure Active Directory-felhasználók, használja az alkalmazás elérési útvonalát társításával engedélyezni szeretné.

###<a name="to-assign-users-to-aha-perform-the-following-steps"></a>Felhasználók hozzárendelése Aha!, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon próba-fiók létrehozása.

2.  Kattintson a **és tényleg!** alkalmazás integrálását lapján kattintson a **hozzárendelését a felhasználókhoz**.

    ![Hozzárendelését a felhasználókhoz] (./media/active-directory-saas-aha-tutorial/IC798956.png "Adhatnak a felhasználóknak")

3.  Jelölje ki a próba-felhasználó, kattintson a **hozzárendelni**, és kattintson az **Igen gombra** kattintva erősítse meg a hozzárendelés.

    ![Igen] (./media/active-directory-saas-aha-tutorial/IC767830.png "Igen")

Ha meg szeretné tesztelje a egyszeri bejelentkezéses, Access Panel megnyitásához. Az Access ablaktábla, lásd: [Bevezetés az Access panelre](active-directory-saas-access-panel-introduction.md).
