<properties 
    pageTitle="Oktatóprogram: Azure Active Directory-integráció a Work.com |} Microsoft Azure" 
    description="Megtudhatja, hogyan használhatja a Work.com az Azure Active Directory ahhoz, hogy az egyszeri bejelentkezés, automatikus kiépítési és az egyéb!." 
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

#<a name="tutorial-azure-active-directory-integration-with-workcom"></a>Oktatóprogram: Azure Active Directory-integráció a Work.com
  
Ebben az oktatóanyagban célja integrálása az Azure és Work.com megjelenítéséhez.  
Az alkalmazási példát, ebben az oktatóanyagban tagolt feltételezi, hogy már rendelkezik az alábbiakat:

-   Érvényes Azure előfizetés
-   Work.com egyszeri bejelentkezéses engedélyezett előfizetés
  
Ebben az oktatóanyagban befejeztével hozzárendelése a Work.com hozzáférés lesz a AAD felhasználókat, akiknek van egyetlen tudja bejelentkezési a Work.com vállalati webhely (service provider által kezdeményezett bejelentkezés) és használatáról a [Bevezetés az Access panelen](active-directory-saas-access-panel-introduction.md)a alkalmazásba.
  
A következő építőelemek az alkalmazási példát, ebben az oktatóanyagban tagolt áll:

1.  Az alkalmazás integrációját Work.com engedélyezése
2.  Egyszeri bejelentkezés beállítása
3.  Felhasználói kiépítési konfigurálása
4.  Felhasználók hozzárendelése

![Eset] (./media/active-directory-saas-work-com-tutorial/IC794105.png "Eset")

##<a name="enabling-the-application-integration-for-workcom"></a>Az alkalmazás integrációját Work.com engedélyezése
  
Ez a szakasz célja, hogyan Work.com az alkalmazás alkalmazással való integráció engedélyezése a szerkezet.

###<a name="to-enable-the-application-integration-for-workcom-perform-the-following-steps"></a>Ahhoz, hogy az alkalmazás integrációját Work.com, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon kattintson a bal oldali navigációs területen kattintson az **Active Directory**.

    ![Az Active Directory] (./media/active-directory-saas-work-com-tutorial/IC700993.png "Az Active Directory")

2.  A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3.  Nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson a felső menüben **alkalmazásokat** .

    ![Alkalmazások] (./media/active-directory-saas-work-com-tutorial/IC700994.png "Alkalmazások")

4.  Kattintson a **Hozzáadás** az oldal alján.

    ![Alkalmazás hozzáadása] (./media/active-directory-saas-work-com-tutorial/IC749321.png "Alkalmazás hozzáadása")

5.  **Milyen feladatot szeretne tenni** párbeszédpanelen kattintson **a gyűjteményből az alkalmazások felvétele**.

    ![Gallerry az alkalmazás hozzáadása] (./media/active-directory-saas-work-com-tutorial/IC749322.png "Gallerry az alkalmazás hozzáadása")

6.  A **Keresés mezőbe**írja be a **Work.com**.

    ![Alkalmazás gyűjtemény] (./media/active-directory-saas-work-com-tutorial/IC794106.png "Alkalmazás gyűjtemény")

7.  Az eredmény ablaktáblában jelölje ki a **Work.com**, és válassza a **kész** , az alkalmazás hozzáadása gombra.

    ![Work.com] (./media/active-directory-saas-work-com-tutorial/IC794107.png "Work.com")

##<a name="configuring-single-sign-on"></a>Egyszeri bejelentkezés beállítása
  
Ez a szakasz célja tagolása engedélyezése a felhasználóknak, hogy Work.com hitelesíteni a fiókkal az összevonási alapján a SAML protokoll használatával Azure AD.  
Ez az eljárás részeként szükség van egy tanúsítványt feltöltése Work.com.com.

>[AZURE.NOTE] Egyszeri bejelentkezés konfigurálásához kell Work.com egyéni tartománynév még beállítása. Kell legalább a tartománynév megadása, tesztelése a tartománynevét, és az egész szervezet számára beállítaná őket.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Egyszeri bejelentkezés beállításához hajtsa végre az alábbi lépéseket:

1.  Jelentkezzen be a Work.com bérlői rendszergazdaként.

2.  Nyissa meg **a telepítő**.

    ![A telepítő] (./media/active-directory-saas-work-com-tutorial/IC794108.png "A telepítő")

3.  A bal oldali navigációs területén **felügyelete** szakaszában kattintson a **Domain Management** bontsa ki a kapcsolódó szakaszt, és válassza a **Saját tartomány** a **Saját tartomány** lap megnyitásához. 

    ![A tartomány] (./media/active-directory-saas-work-com-tutorial/IC767825.png "A tartomány")

4.  Ha ellenőrizni szeretné, hogy a tartomány van beállítva megfelelően, győződjön meg arról, hogy a "**Felhasználók rendszerbe 4 lépésben**" az, és tekintse át a "**Saját tartomány beállításai**".

    ![A tartomány felhasználónak rendszerbe] (./media/active-directory-saas-work-com-tutorial/IC784377.png "A tartomány felhasználónak rendszerbe")

5.  A különböző webes böngészőablakban jelentkezzen be az Azure klasszikus portál.

6.  **Work.com **alkalmazás integrációs lapon kattintson **konfigurálása az egyszeri bejelentkezés** a **Konfigurálása az egyszeri bejelentkezés** párbeszédpanel megnyitásához.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-work-com-tutorial/IC794109.png "Egyszeri bejelentkezés beállítása")

7.  **Hogyan szeretné, hogy jelentkezzen be az Work.com felhasználók** lapon jelölje be **A Microsoft Azure Active Directory Single Sign-On**, és kattintson a **Tovább gombra**.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-work-com-tutorial/IC794110.png "Egyszeri bejelentkezés beállítása")

8.  Az **Alkalmazás URL-cím beállítása** lapon az **URL a bejelentkezési Work.com** mezőbe írja be a bejelentkezni a Work.com alkalmazás a felhasználók által használt URL-CÍMÉT (például: " *http://company.my.salesforce.com*"), majd kattintson a **Tovább**gombra: 

    ![Állítsa be az App URL-címe] (./media/active-directory-saas-work-com-tutorial/IC794111.png "Állítsa be az App URL-címe")

9.  A **Configure egyszeri bejelentkezés Work.com a** lapon a tanúsítvány letöltése, kattintson a **tanúsítvány letöltése**, és mentse a tanúsítvány fájlt a számítógépen helyben parancsra.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-work-com-tutorial/IC794112.png "Egyszeri bejelentkezés beállítása")

10. Jelentkezzen be az Work.com bérlőhöz.

11. Nyissa meg **a telepítő**.

    ![A telepítő] (./media/active-directory-saas-work-com-tutorial/IC794108.png "A telepítő")

12. Bontsa ki a **Biztonsági funkciók** menüt, és kattintson a **Egyszeri bejelentkezéses beállításai**gombra.

    ![Egyszeri bejelentkezés beállításai] (./media/active-directory-saas-work-com-tutorial/IC794113.png "Egyszeri bejelentkezés beállításai")

13. Az **Egyszeri bejelentkezéses beállításai** párbeszédpanel lapon hajtsa végre az alábbi lépéseket:

    ![SAML engedélyezve] (./media/active-directory-saas-work-com-tutorial/IC781026.png "SAML engedélyezve")

    1.  Jelölje ki a **SAML engedélyezve van**.
    2.  Kattintson az **Új**gombra.

14. **A SAML egyszeri bejelentkezéses beállításai** csoportban hajtsa végre az alábbi lépéseket:

    ![SAML egyszeri bejelentkezéses beállítás] (./media/active-directory-saas-work-com-tutorial/IC794114.png "SAML egyszeri bejelentkezéses beállítás")

    1.  Az **neve** mezőbe írja be a konfigurációban nevét.  

        >[AZURE.NOTE] Érték kezeléséről **neve** automatikusan feltölti a **API neve** mezőben lévő értéket.

    2.  Az Azure klasszikus portálon **konfigurálása az egyszeri bejelentkezés a Work.com** párbeszédpanel lapon a **Kibocsátó URL-címe** értéket másolja és illessze be a **kibocsátó** mezőben lévő értéket.
    3.  A letöltött tanúsítvány feltölteni, kattintson a **Tallózás gombra**.
    4.  A **Szervezet azonosítója** mezőbe írja be a **https://salesforce-work.com**.
    5.  **SAML azonosító típusa**csoportban jelölje ki a **állítás a User objektumban az összevonási Azonosítóját tartalmazza**.
    6.  **SAML identitás helyét**jelölje ki a **identitás a tárgy utasítás NameIdentfier eleme**.
    7.  Az Azure klasszikus portálon **konfigurálása az egyszeri bejelentkezés a Work.com** párbeszédpanel lapon a **Távoli bejelentkezési URL-cím** értéket másolja és illessze be a **Identitás szolgáltató bejelentkezési URL-cím** mezőben lévő értéket.
    8.  Az Azure klasszikus portálon **konfigurálása az egyszeri bejelentkezés a Work.com** párbeszédpanel lapon a **Távoli kijelentkezés URL-címe** értéket másolja és illessze be a **Identitás szolgáltató kijelentkezés URL-címe** mezőben lévő értéket.
    9.  A **Service Provider által kezdeményezett kérése kötés**jelölje ki a **HTTP-bejegyzést**.
    10. Kattintson a **Mentés**gombra.

15. Az Work.com klasszikus portálján, a bal oldali navigációs területén kattintson a **Domain Management** bontsa ki a kapcsolódó szakaszt, és válassza a **Saját tartomány** a **Saját tartomány** lap megnyitásához. 

    ![A tartomány] (./media/active-directory-saas-work-com-tutorial/IC794115.png "A tartomány")

16. A **Saját tartomány** lapon, a **Bejelentkezési oldal védjegyzés** csoportban kattintson a **Szerkesztés**gombra.

    ![Márka bejelentkezési lapja] (./media/active-directory-saas-work-com-tutorial/IC767826.png "Márka bejelentkezési lapja")

17. A **Bejelentkezési oldal védjegyzés** lapon a **Hitelesítési szolgáltatás** csoportban jelenik meg a **SAML-féle egyszeri Bejelentkezést beállítások** neve. Jelölje ki azt, és kattintson a **Mentés**gombra.

    ![Védjegyzés bejelentkezési lapja] (./media/active-directory-saas-work-com-tutorial/IC784366.png "Védjegyzés bejelentkezési lapja")

18. Az Azure klasszikus portálon jelölje be a egyszeri bejelentkezéses konfigurációs megerősítés, és válassza a **kész** , **Állítsa be az egyszeri bejelentkezés** párbeszédpanel bezárásához.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-work-com-tutorial/IC794116.png "Egyszeri bejelentkezés beállítása")

##<a name="configuring-user-provisioning"></a>Felhasználói kiépítési konfigurálása
  
Azure Active Directory-felhasználóknak engedélyezni szeretné, hogy jelentkezzen be hogy ki kell építenie Work.com.  
Work.com, ha a feladat manuális kiépítési.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Felhasználói kiépítési beállításához hajtsa végre az alábbi lépéseket:

1.  Jelentkezzen be rendszergazdaként a Work.com vállalati webhely.

2.  Nyissa meg **a telepítő**.

    ![A telepítő] (./media/active-directory-saas-work-com-tutorial/IC794108.png "A telepítő")

3.  Nyissa meg a **felhasználókezelés \> felhasználók**.

    ![Felhasználókezelés] (./media/active-directory-saas-work-com-tutorial/IC784369.png "Felhasználókezelés")

4.  Kattintson az **Új felhasználó**gombra.

    ![Minden felhasználónak] (./media/active-directory-saas-work-com-tutorial/IC794117.png "Minden felhasználónak")

5.  A felhasználó szerkesztheti a csoportban hajtsa végre az alábbi lépéseket:

    ![Felhasználó szerkesztése] (./media/active-directory-saas-work-com-tutorial/IC794118.png "Felhasználó szerkesztése")

    1.  Írja be a **Vezetéknevet**, **Alias**, **e-mailek**, **felhasználónév** és **Becenév** attribútumok be a kapcsolódó szövegdobozok kiépítése kívánt érvényes Azure Active Directory-fiók.
    2.  Jelölje ki a **szerepkört**, a **felhasználói licenc** és a **profilban**.
    3.  Kattintson a **Mentés**gombra.  

        >[AZURE.NOTE] Az Azure Active Directory fióktulajdonos kap e-mailben, beleértve a előtt aktívvá válik, győződjön meg arról, hogy a fiók mutató hivatkozást.

>[AZURE.NOTE] Bármely más Work.com felhasználói fiók létrehozása eszközöket is használhatja, illetve Work.com rendelkezést AAD felhasználói fiókok által biztosított API-khoz.

##<a name="assigning-users"></a>Felhasználók hozzárendelése
  
Tesztelje a konfigurációt, meg kell adni az Azure Active Directory-felhasználók, használja az alkalmazás elérési útvonalát társításával engedélyezni szeretné.

###<a name="to-assign-users-to-workcom-perform-the-following-steps"></a>Felhasználók hozzárendelése Work.com, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon próba-fiók létrehozása.

2.  Kattintson a Work.com alkalmazás integrációs lap **hozzárendelését a felhasználókhoz**.

    ![Adhatnak a felhasználóknak] (./media/active-directory-saas-work-com-tutorial/IC794119.png "Adhatnak a felhasználóknak")

3.  Jelölje ki a próba-felhasználó, kattintson a **hozzárendelni**, és kattintson az **Igen gombra** kattintva erősítse meg a hozzárendelés.

    ![Igen] (./media/active-directory-saas-work-com-tutorial/IC767830.png "Igen")
  
Érdemes most várja meg a 10 perc, és ellenőrizze, hogy a fiók Work.com.com lett-e szinkronizálva.
  
Ha meg szeretné tesztelje a egyszeri bejelentkezéses, Access Panel megnyitásához. Ha többet szeretne tudni az Access panelen című témakörben [az Access-panelen](active-directory-saas-access-panel-introduction.md).