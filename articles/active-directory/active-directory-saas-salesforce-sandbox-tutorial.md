<properties 
    pageTitle="Oktatóprogram: Azure Active Directory-integráció a Salesforce-védőfalas |} Microsoft Azure"
    description="Megtudhatja, hogyan használhatja a Salesforce-védőfalas az Azure Active Directory ahhoz, hogy az egyszeri bejelentkezés, automatikus kiépítési és az egyéb!." 
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
    ms.date="10/28/2016" 
    ms.author="jeedes" />


#<a name="tutorial-azure-active-directory-integration-with-salesforce-sandbox"></a>Oktatóprogram: Azure Active Directory-integráció a Salesforce-védőfalas
>[AZURE.TIP]Visszajelzés, kattintson [ide](http://go.microsoft.com/fwlink/?LinkId=521878).
  
Ebben az oktatóanyagban célja integrálása az Azure és a Salesforce-védőfalas megjelenítéséhez.  
Homokszóró ad az azt jelenti, hogy a szervezet több példányban létrehozása céljából, például fejlesztése, tesztelése és képzés, az adatok és a Salesforce-gyártási szervezet alkalmazások veszélyeztesse sokféle külön környezetben.  
További információ a [Védőfalas áttekintése](https://help.salesforce.com/HTViewHelpDoc?id=create_test_instance.htm&language=en_US) című témakörben találhat.
  
Az alkalmazási példát, ebben az oktatóanyagban tagolt feltételezi, hogy már rendelkezik az alábbiakat:

-   Érvényes Azure előfizetés
-   A védőfalas a Salesforce.com-hoz
  
Ha még nincs érvényes a védőfalas a Salesforce.com-hoz, hogy kapcsolatba kell lépnie Salesforce.
  
A következő építőelemek az alkalmazási példát, ebben az oktatóanyagban tagolt áll:

1.  Az alkalmazás-integráció a Salesforce-védőfalas engedélyezése
2.  Egyszeri bejelentkezés beállítása
3.  A tartomány engedélyezése
4.  Felhasználói kiépítési konfigurálása
5.  Felhasználók hozzárendelése

![Eset] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC769571.png "Eset")
##<a name="enabling-the-application-integration-for-salesforce-sandbox"></a>Az alkalmazás-integráció a Salesforce-védőfalas engedélyezése
  
Ez a szakasz célja, hogyan engedélyezhető az alkalmazás-integráció a Salesforce-védőfalas tagolása.

###<a name="to-enable-the-application-integration-for-salesforce-sandbox-perform-the-following-steps"></a>Ha engedélyezni szeretné a Salesforce-védőfalas alkalmazás integrációját, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon kattintson a bal oldali navigációs területen kattintson az **Active Directory**.

    ![Az Active Directory] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC700993.png "Az Active Directory")

2.  A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3.  Nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson a felső menüben **alkalmazások** .

    ![Alkalmazások] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC700994.png "Alkalmazások")

4.  Az **Alkalmazás gyűjtemény**megnyitásához kattintson **Az alkalmazás hozzáadása**, és kattintson a **Hozzáadás az alkalmazások használata a szervezet számára**.

    ![Milyen feladatot szeretne tenni?] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC700995.png "Milyen feladatot szeretne tenni?")

5.  A **Keresés mezőbe**írja be a **Salesforce-védőfalas**.

    ![Alkalmazás gyűjtemény] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC710978.png "Alkalmazás gyűjtemény")

6.  Az eredmény ablaktáblában jelölje ki a **Salesforce-védőfalas**, és válassza a **kész** , az alkalmazás hozzáadása parancsra.

    ![Salesforce védőfalas] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC746474.png "Salesforce védőfalas")
##<a name="configuring-single-sign-on"></a>Egyszeri bejelentkezés beállítása
  
Ez a szakasz célja tagolása engedélyezése a felhasználóknak, hogy a Salesforce hitelesíteni a fiókkal az összevonási alapján a SAML protokoll használatával Azure AD.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Egyszeri bejelentkezés beállításához hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon a **Salesforce-védőfalas** alkalmazás integrációs lapon kattintson **konfigurálása az egyszeri bejelentkezés** a **Konfigurálása az egyszeri bejelentkezés** párbeszédpanel megnyitásához.

    ![Beállítás az egyszeri bejelentkezés] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC749323.png "Beállítás az egyszeri bejelentkezés")

2.  **Hogyan szeretné, hogy jelentkezzen be Salesforce-védőfalas felhasználók** lapon jelölje be **A Microsoft Azure Active Directory Single Sign-On**, és kattintson a **Tovább gombra**.

    ![Salesforce védőfalas] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC746479.png "Salesforce védőfalas")

3.  Az **Alkalmazás URL-cím beállítása** lapon **Bejelentkezési a URL-cím** mezőbe írja be az URL-t, a következő mintát `http://company.my.salesforce.com`, majd kattintson a **Tovább**gombra.

    ![Állítsa be az App URL-címe] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC781022.png "Állítsa be az App URL-címe")

4. Ha már konfigurált egyszeri bejelentkezés a másik védőfalas Salesforce-példány a címtárában található, majd is konfigurálnia kell az **azonosító** értéke megegyezik, **Jelentkezzen be az URL-címe**van. A párbeszédpanel a **App URL-címe konfigurálása** lapon **a Speciális beállítások megjelenítése** jelölőnégyzet bejelölésével az **azonosító** mező is található.

4.  A **beállítás az egyszeri bejelentkezés a Salesforce-védőfalas** lapon kattintson a **tanúsítvány letöltése**, és mentse a tanúsítvány fájlt a számítógépen.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC781023.png "Egyszeri bejelentkezés beállítása")

5.  A különböző webes böngészőablakban jelentkezzen be a Salesforce-védőfalas rendszergazdaként.

6.  A felső sávon kattintson a **beállítás**gombra.

    ![A telepítő] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC781024.png "A telepítő")

7.  A bal oldali navigációs ablakban kattintson a **Biztonsági vezérlők**, és kattintson a **Egyszeri bejelentkezéses beállítások**.

    ![Egyszeri bejelentkezés beállításai] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC781025.png "Egyszeri bejelentkezés beállításai")

8.  A egyszeri bejelentkezéses beállításai csoportban hajtsa végre az alábbi lépéseket:

    ![Egyszeri bejelentkezés beállításai] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC781026.png "Egyszeri bejelentkezés beállításai")

    egy.  Jelölje ki a **SAML engedélyezve van**.
    
    b.  Kattintson az **Új**gombra.

9.  Kattintson a SAML egyszeri bejelentkezéses beállításai csoportban hajtsa végre az alábbi lépéseket:

    ![SAML egyszeri bejelentkezéses beállításai] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC781027.png "SAML egyszeri bejelentkezéses beállításai")

    egy.  Neve mezőbe írja be a nevét a Configuration (pl.: *SPSSOWAAD\_vizsgálat*).
    
    b.  Az Azure klasszikus portálon **konfigurálása az egyszeri bejelentkezés a Salesforce-védőfalas** párbeszéd lapon a **Kibocsátó URL-címe** értéket másolja és illessze be a **kibocsátó** mezőben lévő értéket.
    
    c billentyűkombinációt.  Ha most hozzáadni a könyvtár Salesforce védőfalas elsősorban **Entitás azonosítója** mezőbe írja be **https://test.salesforce.com** . Ha egy példány Salesforce védőfalas, majd a **Szervezet azonosítója** típus már hozzáadta a **Bejelentkezési a URL-CÍMÉT**, amely a következő formátumban kell:`http://company.my.salesforce.com`
    
    d.  **Tallózással keresse meg** a letöltött tanúsítvány feltöltése gombra.
    
    e.  **SAML azonosító típusa**csoportban jelölje ki a **állítás a User objektumban az összevonási Azonosítóját tartalmazza**.
    
    f.  **SAML identitás helyét**jelölje ki a **identitás a tárgy utasítás NameIdentifier eleme**.
    
    g.  Az Azure klasszikus portálon **konfigurálása az egyszeri bejelentkezés a Salesforce-védőfalas** párbeszéd lapon a **Távoli bejelentkezési URL-cím** értéket másolja és illessze be a **Identitás szolgáltató bejelentkezési URL-cím** mezőben lévő értéket.
    
    h.  SFDC nem támogatja a SAML jelentkezzen ki.  Kerülő illessze be a "https://login.windows.net/common/wsfederation?wa=wsignout1.0" azt be a **Identitás szolgáltató kijelentkezés URL-címe** mezőben lévő értéket.
    
    lehet.  A **Service Provider által kezdeményezett kérése kötés**jelölje ki a **HTTP-BEJEGYZÉST**.
    
    j. Kattintson a **Mentés**gombra.

10. Az Azure klasszikus portálon jelölje be a egyszeri bejelentkezéses konfigurációs megerősítés, és válassza a **kész** , **Állítsa be az egyszeri bejelentkezés** párbeszédpanel bezárásához.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC781028.png "Egyszeri bejelentkezés beállítása")

##<a name="enabling-your-domain"></a>A tartomány engedélyezése
  
Ez a szakasz feltételezi, hogy már létrehozott egy tartományt.  
További részletekért olvassa el [A tartománynév megadása](https://help.salesforce.com/HTViewHelpDoc?id=domain_name_define.htm&language=en_US).

###<a name="to-enable-your-domain-perform-the-following-steps"></a>Ha engedélyezni szeretné a tartomány, hajtsa végre az alábbi lépéseket:

1.  A bal oldali navigációs ablaktáblában kattintson a **Domain Management**, és kattintson a **saját tartományát.**

    ![A tartomány] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC781029.png "A tartomány")

    >[AZURE.NOTE]Győződjön meg arról, hogy a tartomány helyesen van beállítva.

2.  **Bejelentkezési lap beállítások** csoportjában kattintson a **Szerkesztés**gombra, majd a **Hitelesítési szolgáltatás**, válassza ki a SAML egyszeri bejelentkezéses beállítás neve az előző szakaszban, és végül kattintson a **Mentés**gombra.

    ![A tartomány] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC781030.png "A tartomány")
  
Amint egy konfigurált tartománnyal rendelkezik, a felhasználók az a tartomány jelentkezzen be a Salesforce-védőfalas URL-címet kell használni.  
Az URL-cím értékének megjelenítéséhez kattintson a az előző részben létrehozott SSO profilt.
##<a name="configuring-user-provisioning"></a>Felhasználói kiépítési konfigurálása
  
Ez a szakasz célja, hogyan engedélyezhető a felhasználó Active Directory felhasználói fiókok Salesforce védőfalas kiépítési tagolása.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Felhasználói kiépítési beállításához hajtsa végre az alábbi lépéseket:

1.  A Salesforce portál felső navigációs sávon válassza ki a nevét, bontsa ki a felhasználó menü:

    ![Saját beállítások] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC698773.png "Saját beállítások")

2.  A felhasználó menüből válassza a **Saját beállításai** a **Saját beállításai** lap megnyitásához.

3.  A bal oldali ablaktáblában kattintson a **személyes** bontsa ki a személyes szakaszt, és kattintson a **Alaphelyzetbe saját biztonsági jogkivonat**:

    ![Saját beállítások] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC698774.png "Saját beállítások")

4.  A **Alaphelyzetbe saját biztonsági jogkivonat** lapon kattintson a **Biztonsági jogkivonat alaphelyzetbe** egy e-mailt kapnak a Salesforce.com biztonsági jogkivonat kérhet.

    ![Új jogkivonat] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC698776.png "Új jogkivonat")

5.  Jelölje be a Beérkezett üzenetek mappájának egy e-mailt érkező Salesforce.com with "**esetbejegyzéseinek biztonsági megerősítő**" tárgyát.

6.  Tekintse át a levelezés és a biztonsági jogkivonat értékét másolja.

7.  Az Azure klasszikus portálon **salesforce védőfalas** alkalmazás integrációs lapon kattintson a **konfigurálása felhasználói kiépítési** a **Felhasználó kiépítési beállítása** párbeszédpanel megnyitásához.

    ![Konfigurálása felhasználói kiépítése] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC769573.png "Konfigurálása felhasználói kiépítése")

8.  **Írja be a ahhoz, hogy a felhasználó automatikus kiépítési a Salesforce-védőfalas hitelesítő adatait** lapon adja meg a következő beállítások:

    ![Salesforce védőfalas] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC746476.png "Salesforce védőfalas")

    egy.  Az **Salesforce védőfalas felügyeleti felhasználónév** mezőbe írja be a Salesforce védőfalas fiók nevét, amelynek a **Rendszergazda** profil rendelt Salesforce.com.

    b.  **Salesforce védőfalas rendszergazdai jelszó** mezőbe írja be a fiókhoz tartozó jelszót.

    c billentyűkombinációt.  A **Felhasználó biztonsági jogkivonat** mezőbe illessze be a biztonsági jogkivonat értékét.

    d.  Kattintson az **érvényesítés** a beállításainak ellenőrzése.

    e.  Kattintson a **Tovább** gombra kattintva nyissa meg a **megerősítő** lapon.

9.  A **Megerősítés** lapon kattintson a **kész** menti a konfigurációt.
##<a name="assigning-users"></a>Felhasználók hozzárendelése
  
Tesztelje a konfigurációt, meg kell adni az Azure Active Directory-felhasználók, használja az alkalmazás elérési útvonalát társításával engedélyezni szeretné.

###<a name="to-assign-users-to-salesforce-sandbox-perform-the-following-steps"></a>Felhasználók hozzárendelése Salesforce védőfalas, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon próba-fiók létrehozása.

2.  A **Salesforce-védőfalas **alkalmazás integrációs lapon kattintson a **hozzárendelését a felhasználókhoz**.

    ![Adhatnak a felhasználóknak] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC769574.png "Adhatnak a felhasználóknak")

3.  Jelölje ki a próba-felhasználó, kattintson a **hozzárendelni**, és kattintson az **Igen gombra** kattintva erősítse meg a hozzárendelés.

    ![Igen] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC767830.png "Igen")
  
Érdemes most várja meg a 10 perc, és ellenőrizze, hogy a fiók lett szinkronizálva a Salesforce-védőfalas.
  
Ha meg szeretné tesztelje a egyszeri bejelentkezéses, Access Panel megnyitásához. Ha többet szeretne tudni az Access panelen című témakörben [az Access-panelen](https://msdn.microsoft.com/library/dn308586).
