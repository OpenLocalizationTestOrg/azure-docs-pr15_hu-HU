<properties 
    pageTitle="Oktatóprogram: Azure Active Directory-integráció a TeamSeer |} Microsoft Azure" 
    description="Megtudhatja, hogyan használhatja a TeamSeer az Azure Active Directory ahhoz, hogy az egyszeri bejelentkezés, automatikus kiépítési és az egyéb!" 
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

#<a name="tutorial-azure-active-directory-integration-with-teamseer"></a>Oktatóprogram: Azure Active Directory-integráció a TeamSeer
  
Ebben az oktatóanyagban célja integrálása az Azure és TeamSeer megjelenítéséhez.  
Az alkalmazási példát, ebben az oktatóanyagban tagolt feltételezi, hogy már rendelkezik az alábbiakat:

-   Érvényes Azure előfizetés
-   TeamSeer bérlői webhelyre
  
Ebben az oktatóanyagban befejeztével az Azure Active Directory-felhasználók TeamSeer rendelt fogja tudni az alkalmazás a TeamSeer vállalati webhely (service provider által kezdeményezett bejelentkezés) és használatáról a [Bevezetés az Access panelen](active-directory-saas-access-panel-introduction.md)az egyszeri bejelentkezési.
  
A következő építőelemek az alkalmazási példát, ebben az oktatóanyagban tagolt áll:

1.  Az alkalmazás integrációját TeamSeer engedélyezése
2.  Egyszeri bejelentkezés beállítása
3.  Felhasználói kiépítési konfigurálása
4.  Felhasználók hozzárendelése

![Eset] (./media/active-directory-saas-teamseer-tutorial/IC789618.png "Eset")

##<a name="enabling-the-application-integration-for-teamseer"></a>Az alkalmazás integrációját TeamSeer engedélyezése
  
Ez a szakasz célja, hogyan TeamSeer az alkalmazás alkalmazással való integráció engedélyezése a szerkezet.

###<a name="to-enable-the-application-integration-for-teamseer-perform-the-following-steps"></a>Ahhoz, hogy az alkalmazás integrációját TeamSeer, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon kattintson a bal oldali navigációs területen kattintson az **Active Directory**.

    ![Az Active Directory] (./media/active-directory-saas-teamseer-tutorial/IC700993.png "Az Active Directory")

2.  A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3.  Nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson a felső menüben **alkalmazások** .

    ![Alkalmazások] (./media/active-directory-saas-teamseer-tutorial/IC700994.png "Alkalmazások")

4.  Kattintson a **Hozzáadás** az oldal alján.

    ![Alkalmazás hozzáadása] (./media/active-directory-saas-teamseer-tutorial/IC749321.png "Alkalmazás hozzáadása")

5.  **Milyen feladatot szeretne tenni** párbeszédpanelen kattintson **a gyűjteményből az alkalmazások felvétele**.

    ![Gallerry az alkalmazás hozzáadása] (./media/active-directory-saas-teamseer-tutorial/IC749322.png "Gallerry az alkalmazás hozzáadása")

6.  A **Keresés mezőbe**írja be a **TeamSeer**.

    ![Alkalmazás gyűjtemény] (./media/active-directory-saas-teamseer-tutorial/IC789619.png "Alkalmazás gyűjtemény")

7.  Az eredmény ablaktáblában jelölje ki a **TeamSeer**, és válassza a **kész** , az alkalmazás hozzáadása gombra.

    ![TeamSeer] (./media/active-directory-saas-teamseer-tutorial/IC789620.png "TeamSeer")

##<a name="configuring-single-sign-on"></a>Egyszeri bejelentkezés beállítása
  
Ez a szakasz célja tagolása engedélyezése a felhasználóknak, hogy TeamSeer hitelesíteni a fiókkal az összevonási alapján a SAML protokoll használatával Azure AD.  
Ez az eljárás részeként biztosan szükséges számrendszerben-64 kódolt tanúsítvány fájlt.  
Ha nem ismeri a ezt az eljárást, olvassa el a [szövegfájlba bináris tanúsítvány konvertálása](http://youtu.be/PlgrzUZ-Y1o)című témakört.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Egyszeri bejelentkezés beállításához hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon **TeamSeer** alkalmazás integrációs lapon kattintson **konfigurálása az egyszeri bejelentkezés** a **Konfigurálása az egyszeri bejelentkezés** párbeszédpanel megnyitásához.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-teamseer-tutorial/IC789621.png "Egyszeri bejelentkezés beállítása")

2.  **Hogyan szeretné, hogy jelentkezzen be az TeamSeer felhasználók** lapon jelölje be **A Microsoft Azure Active Directory Single Sign-On**, és kattintson a **Tovább gombra**.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-teamseer-tutorial/IC789628.png "Egyszeri bejelentkezés beállítása")

3.  A **App URL-címe beállítása** lapon az **TeamSeer bejelentkezési az URL** mezőbe írja be az URL-t, a következő mintát "*http://www.teamseer.com/companyid*", és kattintson a **Tovább gombra**.

    ![Állítsa be az App URL-címe] (./media/active-directory-saas-teamseer-tutorial/IC789629.png "Állítsa be az App URL-címe")

4.  A **Configure egyszeri bejelentkezés TeamSeer a** lapon a tanúsítvány letöltése, kattintson a **tanúsítvány letöltése**, és mentse a tanúsítvány fájlt a számítógépen parancsra.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-teamseer-tutorial/IC789630.png "Egyszeri bejelentkezés beállítása")

5.  A különböző webes böngészőablakban jelentkezzen be a TeamSeer vállalati webhely rendszergazdaként.

6.  Nyissa meg a **rendszergazda HR**.

    ![HR rendszergazda] (./media/active-directory-saas-teamseer-tutorial/IC789634.png "HR rendszergazda")

7.  Kattintson a **telepítés**gombra.

    ![A telepítő] (./media/active-directory-saas-teamseer-tutorial/IC789635.png "A telepítő")

8.  Kattintson a **SAML szolgáltató részletek beállítása**gombra.

    ![SAML-beállítások] (./media/active-directory-saas-teamseer-tutorial/IC789636.png "SAML-beállítások")

9.  A SAML szolgáltató adatai csoportban végezze el az alábbi lépéseket:

    ![SAML-beállítások] (./media/active-directory-saas-teamseer-tutorial/IC789637.png "SAML-beállítások")

    1.  Az Azure klasszikus portálon **konfigurálása az egyszeri bejelentkezés a TeamSeer** párbeszédpanel lapon másolja az **Egyszeri bejelentkezéses szolgáltatás URL-címe** értéket, és illessze be a **URL-címe** mezőben lévő értéket.
    2.  A letöltött tanúsítvány **Alap-64-címként kódolt** fájl létrehozása  

        >[AZURE.TIP] További információra kíváncsi olvassa el a [bináris tanúsítvány szövegfájlba konvertálása](http://youtu.be/PlgrzUZ-Y1o) című témakört.

    3.  Nyissa meg az alap-64 kódolt tanúsítvány a Jegyzettömbben, a tartalmát a vágólapra másolja és illessze be a **Nyilvános tanúsítvány IdP** mezőben lévő értéket szeretné.

10. A SAML-szolgáltató konfiguráció befejezéséhez végezze el az alábbi lépéseket:

    ![SAML-beállítások] (./media/active-directory-saas-teamseer-tutorial/IC789638.png "SAML-beállítások")

    1.  Az **E-mail címek vizsgálat**írja be a próba-felhasználó e-mail címét.
    2.  Az **kibocsátó** mezőbe írja be a szolgáltató kibocsátó URL-CÍMÉT.
    3.  Kattintson a **Mentés**gombra.

11. Az Azure klasszikus portálon jelölje be a egyszeri bejelentkezéses konfigurációs megerősítés, és válassza a **kész** , **Állítsa be az egyszeri bejelentkezés** párbeszédpanel bezárásához.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-teamseer-tutorial/IC789639.png "Egyszeri bejelentkezés beállítása")

##<a name="configuring-user-provisioning"></a>Felhasználói kiépítési konfigurálása
  
Ahhoz, hogy az Azure Active Directory-felhasználóknak, hogy jelentkezzen be a TeamSeer, hogy ki kell építenie ShiftPlanning be.  
TeamSeer, ha a feladat manuális kiépítési.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>A felhasználói fiókok kiépítése, hajtsa végre az alábbi lépéseket:

1.  Jelentkezzen be rendszergazdaként a **TeamSeer** vállalati webhely.

2.  Hajtsa végre az alábbi lépéseket:

    ![HR rendszergazda] (./media/active-directory-saas-teamseer-tutorial/IC789640.png "HR rendszergazda")

    1.  Nyissa meg a **HR felügyeleti \> felhasználók**.
    2.  Kattintson **az új felhasználó varázslót**.

3.  A **Felhasználó adatai** csoportban végezze el az alábbi lépéseket:

    ![Felhasználó adatai] (./media/active-directory-saas-teamseer-tutorial/IC789641.png "Felhasználó adatai")

    1.  Írja be a kívánt be a kapcsolódó szövegdobozok kiépítése érvényes AAD fiókkal **utónevet**, **Vezetéknév**, **felhasználónév (E-mail cím)** .
    2.  Kattintson a **Tovább**gombra.

4.  Az új felhasználó hozzáadása a képernyő útmutatást követve, és kattintson a **Befejezés gombra**.

>[AZURE.NOTE] Bármely más TeamSeer felhasználói fiók létrehozása eszközöket is használhatja, vagy TeamSeer hozhatók létre az Azure Active Directory-fiókokkal által biztosított API-khoz.

##<a name="assigning-users"></a>Felhasználók hozzárendelése
  
Tesztelje a konfigurációt, meg kell adni az Azure Active Directory-felhasználók, használja az alkalmazás elérési útvonalát társításával engedélyezni szeretné.

###<a name="to-assign-users-to-teamseer-perform-the-following-steps"></a>Felhasználók hozzárendelése TeamSeer, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon próba-fiók létrehozása.

2.  Kattintson a **TeamSeer **alkalmazás integrációs lap **hozzárendelését a felhasználókhoz**.

    ![Hozzárendelését a felhasználókhoz] (./media/active-directory-saas-teamseer-tutorial/IC789642.png "Adhatnak a felhasználóknak")

3.  Jelölje ki a próba-felhasználó, kattintson a **hozzárendelni**, és kattintson az **Igen gombra** kattintva erősítse meg a hozzárendelés.

    ![Igen] (./media/active-directory-saas-teamseer-tutorial/IC767830.png "Igen")
  
Ha meg szeretné tesztelje a egyszeri bejelentkezéses, Access Panel megnyitásához. Az Access ablaktábla, lásd: [Bevezetés az Access panelre](active-directory-saas-access-panel-introduction.md).