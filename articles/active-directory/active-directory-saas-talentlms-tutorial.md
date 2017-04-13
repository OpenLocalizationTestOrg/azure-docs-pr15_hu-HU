<properties 
    pageTitle="Oktatóprogram: Azure Active Directory-integráció a TalentLMS |} Microsoft Azure" 
    description="Megtudhatja, hogyan használhatja a TalentLMS az Azure Active Directory ahhoz, hogy az egyszeri bejelentkezés, automatikus kiépítési és az egyéb!" 
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

#<a name="tutorial-azure-active-directory-integration-with-talentlms"></a>Oktatóprogram: Azure Active Directory-integráció a TalentLMS
  
Ebben az oktatóanyagban célja integrálása az Azure és TalentLMS megjelenítéséhez.  
Az alkalmazási példát, ebben az oktatóanyagban tagolt feltételezi, hogy már rendelkezik az alábbiakat:

-   Érvényes Azure előfizetés
-   TalentLMS bérlői webhelyre
  
Ebben az oktatóanyagban befejeztével az Azure Active Directory-felhasználók TalentLMS rendelt fogja tudni az alkalmazás a TalentLMS vállalati webhely (service provider által kezdeményezett bejelentkezés) és használatáról a [Bevezetés az Access panelen](active-directory-saas-access-panel-introduction.md)az egyszeri bejelentkezési.
  
A következő építőelemek az alkalmazási példát, ebben az oktatóanyagban tagolt áll:

1.  Az alkalmazás integrációját TalentLMS engedélyezése
2.  Egyszeri bejelentkezés beállítása
3.  Felhasználói kiépítési konfigurálása
4.  Felhasználók hozzárendelése

![Eset] (./media/active-directory-saas-talentlms-tutorial/IC777289.png "Eset")

##<a name="enabling-the-application-integration-for-talentlms"></a>Az alkalmazás integrációját TalentLMS engedélyezése
  
Ez a szakasz célja, hogyan TalentLMS az alkalmazás alkalmazással való integráció engedélyezése a szerkezet.

###<a name="to-enable-the-application-integration-for-talentlms-perform-the-following-steps"></a>Ahhoz, hogy az alkalmazás integrációját TalentLMS, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon kattintson a bal oldali navigációs területen kattintson az **Active Directory**.

    ![Az Active Directory] (./media/active-directory-saas-talentlms-tutorial/IC700993.png "Az Active Directory")

2.  A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3.  Nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson a felső menüben **alkalmazások** .

    ![Alkalmazások] (./media/active-directory-saas-talentlms-tutorial/IC700994.png "Alkalmazások")

4.  Kattintson a **Hozzáadás** az oldal alján.

    ![Alkalmazás hozzáadása] (./media/active-directory-saas-talentlms-tutorial/IC749321.png "Alkalmazás hozzáadása")

5.  **Milyen feladatot szeretne tenni** párbeszédpanelen kattintson **a gyűjteményből az alkalmazások felvétele**.

    ![Gallerry az alkalmazás hozzáadása] (./media/active-directory-saas-talentlms-tutorial/IC749322.png "Gallerry az alkalmazás hozzáadása")

6.  A **Keresés mezőbe**írja be a **TalentLMS**.

    ![Alkalmazás gyűjtemény] (./media/active-directory-saas-talentlms-tutorial/IC777290.png "Alkalmazás gyűjtemény")

7.  Az eredmény ablaktáblában jelölje ki a **TalentLMS**, és válassza a **kész** , az alkalmazás hozzáadása gombra.

    ![TalentLMS] (./media/active-directory-saas-talentlms-tutorial/IC777291.png "TalentLMS")

##<a name="configuring-single-sign-on"></a>Egyszeri bejelentkezés beállítása
  
Ez a szakasz célja tagolása engedélyezése a felhasználóknak, hogy TalentLMS hitelesíteni a fiókkal az összevonási alapján a SAML protokoll használatával Azure AD. .  
Egyszeri bejelentkezés az TalentLMS konfigurálása megköveteli ujjlenyomat érték beolvasása egy tanúsítványt.  
Ha nem ismeri a ezt az eljárást, megtudhatja, [hogy miként beolvasni a tanúsítvány ujjlenyomat értékét](http://youtu.be/YKQF266SAxI).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Egyszeri bejelentkezés beállításához hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon **TalentLMS** alkalmazás integrációs lapon kattintson **konfigurálása az egyszeri bejelentkezés** a **Konfigurálása az egyszeri bejelentkezés** párbeszédpanel megnyitásához.

    ![Beállítás az egyszeri bejelentkezés] (./media/active-directory-saas-talentlms-tutorial/IC777292.png "Beállítás az egyszeri bejelentkezés")

2.  **Hogyan szeretné, hogy jelentkezzen be az TalentLMS felhasználók** lapon jelölje be **A Microsoft Azure Active Directory Single Sign-On**, és kattintson a **Tovább gombra**.

    ![Beállítás az egyszeri bejelentkezés] (./media/active-directory-saas-talentlms-tutorial/IC777293.png "Beállítás az egyszeri bejelentkezés")

3.  Az **App URL-címe konfigurálása** lapon **TalentLMS bejelentkezési az URL** mezőbe írja be az URL-t, a következő mintát "*https://\<bérlői neve\>. TalentLMSapp.com*", majd kattintson a **Tovább**gombra.

    ![Jelentkezzen be az URL-címe] (./media/active-directory-saas-talentlms-tutorial/IC777294.png "Jelentkezzen be az URL-címe")

4.  **Konfigurálása az egyszeri bejelentkezés TalentLMS a** lapon a tanúsítvány letöltése, kattintson a **tanúsítvány letöltése**, és mentse a tanúsítvány fájlként helyileg **c:\\TalentLMS.cer**.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-talentlms-tutorial/IC777295.png "Egyszeri bejelentkezés beállítása")

5.  A különböző webes böngészőablakban jelentkezzen be a TalentLMS vállalati webhely rendszergazdaként.

6.  A **fiók és a beállítások** csoportban kattintson a **felhasználók** fülre.

    ![Fiók és beállítások] (./media/active-directory-saas-talentlms-tutorial/IC777296.png "Fiók és beállítások")

7.  **Egyszeri bejelentkezés (SSO)**,

8.  Egyszeri bejelentkezés csoportban hajtsa végre az alábbi lépéseket:

    ![Egyszeri bejelentkezés] (./media/active-directory-saas-talentlms-tutorial/IC777297.png "Egyszeri bejelentkezés")

    1.  Az **egyszeri bejelentkezés integráció típusa** listából válassza ki **a SAML 2.0**.
    2.  Az Azure klasszikus portálon **konfigurálása az egyszeri bejelentkezés a TalentLMS** párbeszédpanel lapon másolja az **Identitás-szolgáltató azonosítója** értéket, és illessze be a **identitásszolgáltató (IdP)** mezőben lévő értéket.
    3.  Az exportált tanúsítványt az **ujjlenyomatot** értéket másolja és illessze be a **Tanúsítvány ujjlenyomat** mezőben lévő értéket.

        >[AZURE.TIP] További részletekért megtudhatja, [hogy miként egy tanúsítványt ujjlenyomat érték beolvasása](http://youtu.be/YKQF266SAxI)

    4.  Az Azure klasszikus portálon **konfigurálása az egyszeri bejelentkezés a TalentLMS** párbeszédpanel lapon a **Távoli bejelentkezési URL-cím** értéket másolja és illessze be a **távoli bejelentkezési URL-címe** mezőben lévő értéket.
    5.  Az Azure klasszikus portálon **konfigurálása az egyszeri bejelentkezés a TalentLMS** párbeszédpanel lapon a **Távoli kijelentkezés URL-címe** értéket másolja és illessze be a **távoli kijelentkezési URL-címe** mezőben lévő értéket.
    6.  Az **TargetedID** mezőbe írja be a **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name**
    7.  Az **első neve** mezőbe írja be a **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname**
    8.  Az **utolsó neve** mezőbe írja be a **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname**
    9.  Az **E-mail** mezőbe írja be a **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**
    10. Kattintson a **Mentés**gombra.

9.  Az Azure klasszikus portálon jelölje be a egyszeri bejelentkezéses konfigurációs megerősítés, és válassza a **kész** , **Állítsa be az egyszeri bejelentkezés** párbeszédpanel bezárásához.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-talentlms-tutorial/IC777298.png "Egyszeri bejelentkezés beállítása")

##<a name="configuring-user-provisioning"></a>Felhasználói kiépítési konfigurálása
  
Ahhoz, hogy az Azure Active Directory-felhasználóknak, hogy jelentkezzen be a TalentLMS, hogy ki kell építenie TalentLMS be.  
TalentLMS, ha a feladat manuális kiépítési.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>A felhasználói fiókok kiépítése, hajtsa végre az alábbi lépéseket:

1.  Jelentkezzen be az **TalentLMS** bérlői webhelyen.

2.  Kattintson a **felhasználók**elemre, és kattintson a **Felhasználó hozzáadása**gombra.

3.  A **felhasználó hozzáadása** párbeszédpanel lapon hajtsa végre az alábbi lépéseket:

    ![Felhasználó hozzáadása] (./media/active-directory-saas-talentlms-tutorial/IC777299.png "Felhasználó hozzáadása")

    1.  Írja be a következő szövegdobozok a kapcsolódó attribútum értékeket, a Azure AD-felhasználói fiók: **utónevet**, **vezetéknevét**, **E-mail címét**.
    2.  Kattintson a **felhasználó hozzáadása**elemre.

>[AZURE.NOTE] Bármely más TalentLMS felhasználói fiók létrehozása eszközöket is használhatja, illetve TalentLMS rendelkezést AAD felhasználói fiókok által biztosított API-khoz.

##<a name="assigning-users"></a>Felhasználók hozzárendelése
  
Tesztelje a konfigurációt, meg kell adni az Azure Active Directory-felhasználók, használja az alkalmazás elérési útvonalát társításával engedélyezni szeretné.

###<a name="to-assign-users-to-talentlms-perform-the-following-steps"></a>Felhasználók hozzárendelése TalentLMS, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon próba-fiók létrehozása.

2.  Kattintson a **TalentLMS **alkalmazás integrációs lap **hozzárendelését a felhasználókhoz**.

    ![Adhatnak a felhasználóknak] (./media/active-directory-saas-talentlms-tutorial/IC777300.png "Adhatnak a felhasználóknak")

3.  Jelölje ki a próba-felhasználó, kattintson a **hozzárendelni**, és kattintson az **Igen gombra** kattintva erősítse meg a hozzárendelés.

    ![Igen] (./media/active-directory-saas-talentlms-tutorial/IC767830.png "Igen")
  
Ha meg szeretné tesztelje a egyszeri bejelentkezéses, Access Panel megnyitásához. Az Access ablaktábla, lásd: [Bevezetés az Access panelre](active-directory-saas-access-panel-introduction.md).