<properties 
    pageTitle="Oktatóprogram: Azure Active Directory-integráció a Igloo szoftver |} Microsoft Azure" 
    description="Megtudhatja, hogy miként Igloo szoftver használata az Azure Active Directory ahhoz, hogy az egyszeri bejelentkezés, automatikus kiépítési és az egyéb!" 
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
    ms.date="10/20/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-igloo-software"></a>Oktatóprogram: Azure Active Directory-integráció a Igloo szoftver
  
Ebben az oktatóanyagban célja Azure és Igloo szoftver integrációja megjelenítése.  
Az alkalmazási példát, ebben az oktatóanyagban tagolt feltételezi, hogy már rendelkezik az alábbiakat:

-   Érvényes Azure előfizetés
-   Engedélyezett előfizetés- [Igloo szoftver](http://www.igloosoftware.com/) egyszeri bejelentkezés
  
Ebben az oktatóanyagban befejeztével az Azure Active Directory felhasználók Igloo szoftver rendelt tudnak egyetlen bejelentkezés az alkalmazásba, a Igloo szoftver vállalati webhely (service provider által kezdeményezett bejelentkezés) és használatáról a [Bevezetés az Access panelen](active-directory-saas-access-panel-introduction.md).
  
A következő építőelemek az alkalmazási példát, ebben az oktatóanyagban tagolt áll:

1.  Az alkalmazás integrálását Igloo szoftverek engedélyezése
2.  Egyszeri bejelentkezés beállítása
3.  Felhasználói kiépítési konfigurálása
4.  Felhasználók hozzárendelése

![Eset] (./media/active-directory-saas-igloo-software-tutorial/IC783961.png "Eset")
##<a name="enabling-the-application-integration-for-igloo-software"></a>Az alkalmazás integrálását Igloo szoftverek engedélyezése
  
Ez a szakasz célja, hogyan Igloo szoftverek alkalmazás alkalmazással való integráció engedélyezése a szerkezet.

###<a name="to-enable-the-application-integration-for-igloo-software-perform-the-following-steps"></a>Ahhoz, hogy az alkalmazás integrálását Igloo szoftverek, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon kattintson a bal oldali navigációs területen kattintson az **Active Directory**.

    ![Az Active Directory] (./media/active-directory-saas-igloo-software-tutorial/IC700993.png "Az Active Directory")

2.  A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3.  Nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson a felső menüben **alkalmazások** .

    ![Alkalmazások] (./media/active-directory-saas-igloo-software-tutorial/IC700994.png "Alkalmazások")

4.  Kattintson a **Hozzáadás** az oldal alján.

    ![Alkalmazás hozzáadása] (./media/active-directory-saas-igloo-software-tutorial/IC749321.png "Alkalmazás hozzáadása")

5.  **Milyen feladatot szeretne tenni** párbeszédpanelen kattintson **a gyűjteményből az alkalmazások felvétele**.

    ![Gallerry az alkalmazás hozzáadása] (./media/active-directory-saas-igloo-software-tutorial/IC749322.png "Gallerry az alkalmazás hozzáadása")

6.  A **Keresés mezőbe**írja be a **Szoftver Igloo**.

    ![Alkalmazás gyűjtemény] (./media/active-directory-saas-igloo-software-tutorial/IC783962.png "Alkalmazás gyűjtemény")

7.  Az eredmény ablaktáblában jelölje ki a **Igloo szoftver**, és válassza a **kész** , az alkalmazás hozzáadása gombra.

    ![Igloo] (./media/active-directory-saas-igloo-software-tutorial/IC783963.png "Igloo")
##<a name="configuring-single-sign-on"></a>Egyszeri bejelentkezés beállítása
  
Ez a szakasz célja, hogyan engedélyezheti a felhasználóknak, hogy a saját fiók az összevonási alapján a SAML protokoll használatával Azure Active Directory hitelesítő Igloo szoftver tagolása.  
Ez az eljárás részeként biztosan szükséges számrendszerben-64 kódolt tanúsítvány feltöltése az központi asztali bérlőhöz.  
Ha nem ismeri a ezt az eljárást, olvassa el a [szövegfájlba bináris tanúsítvány konvertálása](http://youtu.be/PlgrzUZ-Y1o)című témakört.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Egyszeri bejelentkezés beállításához hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon **Igloo szoftver** alkalmazás integrációs lapon kattintson **konfigurálása az egyszeri bejelentkezés** a **Konfigurálása az egyszeri bejelentkezés** párbeszédpanel megnyitásához.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-igloo-software-tutorial/IC783964.png "Egyszeri bejelentkezés beállítása")

2.  **Hogyan szeretné, hogy jelentkezzen be az Igloo szoftver felhasználók** lapon jelölje be **A Microsoft Azure Active Directory Single Sign-On**, és kattintson a **Tovább gombra**.

    ![Microsoft Azure Active Directory Single Sign-On] (./media/active-directory-saas-igloo-software-tutorial/IC783965.png "Microsoft Azure Active Directory Single Sign-On")

3.  A **App URL-címe beállítása** lapon az **Igloo szoftver bejelentkezési az URL-cím** mezőbe írja be az URL-t, a következő mintát "*https://company.igloocommunities.com/?signin*", és kattintson a **Tovább gombra**.

    ![Állítsa be az App URL-címe] (./media/active-directory-saas-igloo-software-tutorial/IC773625.png "Állítsa be az App URL-címe")

4.  A **Configure egyszeri bejelentkezés Igloo szoftver a** lapon a tanúsítvány letöltése, kattintson a **tanúsítvány letöltése**, és mentse a tanúsítvány fájlt a számítógépen helyben parancsra.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-igloo-software-tutorial/IC783966.png "Egyszeri bejelentkezés beállítása")

5.  A különböző webes böngészőablakban jelentkezzen be a szoftver Igloo vállalati webhely rendszergazdaként.

6.  Nyissa meg a **Vezérlőpult parancsra**.

    ![Vezérlőpult] (./media/active-directory-saas-igloo-software-tutorial/IC799949.png "Vezérlőpult")

7.  Kattintson a **Tagság** fülre, a **Bejelentkezési beállításainak**.

    ![Jelentkezzen be a beállítások] (./media/active-directory-saas-igloo-software-tutorial/IC783968.png "Jelentkezzen be a beállítások")

8.  A SAML konfigurálása szakaszban kattintson a **SAML-hitelesítés konfigurálása**elemre.

    ![SAML konfigurálása] (./media/active-directory-saas-igloo-software-tutorial/IC783969.png "SAML konfigurálása")

9.  **Általános konfigurációs** csoportban hajtsa végre az alábbi lépéseket:

    ![Általános konfigurálása] (./media/active-directory-saas-igloo-software-tutorial/IC783970.png "Általános konfigurálása")

    1.  A **Kapcsolat neve** mezőbe írja be egy egyedi nevet a konfigurációban.
    2.  Az Azure klasszikus portálon **konfigurálása az egyszeri bejelentkezés a Igloo szoftver** párbeszéd lapon a **Távoli bejelentkezési URL-cím** értéket másolja és illessze be a **IdP bejelentkezési URL-cím** mezőben lévő értéket.
    3.  Az Azure klasszikus portálon **konfigurálása az egyszeri bejelentkezés a Igloo szoftver** párbeszéd lapon a **Távoli kijelentkezés URL-címe** értéket másolja és illessze be a **IdP kijelentkezés URL-címe** mezőben lévő értéket.
    4.  **Jelentkezzen ki a válasz és a HTTP-típus kérése**jelölje ki a **BEJEGYZÉST**.
    5.  A letöltött tanúsítvány szövegfájl létrehozása
        
        >[AZURE.TIP]További információra kíváncsi olvassa el a [bináris tanúsítvány szövegfájlba konvertálása](http://youtu.be/PlgrzUZ-Y1o) című témakört.

    6.  Az első sor és az utolsó sor eltávolítása a tanúsítvány szöveges verziója, a hátralévő tanúsítvány szövegét a vágólapra másolhatja, és illessze be a **Nyilvános tanúsítvány** mezőben lévő értéket.

10. A **Válasz- és konfigurációs hitelesítés**hajtsa végre az alábbi lépéseket:

    ![Válasz és a hitelesítési konfigurálása] (./media/active-directory-saas-igloo-software-tutorial/IC783971.png "Válasz és a hitelesítési konfigurálása")

    1.  Jelölje ki a **Microsoft ADFS-alapú** **Identitásszolgáltató használatához**.
    2.  **Azonosító típusa**jelölje be az **E-mail címét**.
    3.  Az **E-mailek attribútum** mezőbe írja be a **EmailCím**.
    4.  Az **Utónév attribútum** mezőbe írja be a **givenname**.
    5.  Az **Utolsó név attribútum** mezőbe írja be a **vezetékneve**.

11. Az alábbi lépések elvégzésével fejezze be a konfigurálását preform:

    ![Bejelentkezés a felhasználók létrehozása] (./media/active-directory-saas-igloo-software-tutorial/IC783972.png "Bejelentkezés a felhasználók létrehozása")

    1.  **Felhasználók létrehozása bejelentkezési**jelölje ki a **Új felhasználó a webhelyen, ha bejelentkezni**.
    2.  **Jelentkezzen be a beállítások**jelölje ki a **"Jelentkezzen be a" képernyő használata SAML gombjára**.
    3.  Kattintson a **Mentés**gombra.

12. Az Azure klasszikus portálon jelölje be a egyszeri bejelentkezéses konfigurációs megerősítés, és válassza a **kész** , **Állítsa be az egyszeri bejelentkezés** párbeszédpanel bezárásához.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-igloo-software-tutorial/IC783973.png "Egyszeri bejelentkezés beállítása")
##<a name="configuring-user-provisioning"></a>Felhasználói kiépítési konfigurálása
  
Nincs teendő konfigurálható felhasználói kiépítési Igloo szoftverben nem.  
Egy tevékenységhez rendelt felhasználó megkísérel jelentkezzen be a hozzáférés panelen Igloo szoftver, a Igloo szoftver ellenőrzi, hogy létezik-e a felhasználó.  
Ha nincs még nincs felhasználói fiók érhető el, automatikusan létrehozott Igloo szoftverrel.
##<a name="assigning-users"></a>Felhasználók hozzárendelése
  
Tesztelje a konfigurációt, meg kell adni az Azure Active Directory-felhasználók, használja az alkalmazás elérési útvonalát társításával engedélyezni szeretné.

###<a name="to-assign-users-to-igloo-software-perform-the-following-steps"></a>Felhasználók hozzárendelése Igloo szoftver, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon próba-fiók létrehozása.

2.  A **Szoftver Igloo **alkalmazás integrációs lapon kattintson a **hozzárendelését a felhasználókhoz**.

    ![Hozzárendelését a felhasználókhoz] (./media/active-directory-saas-igloo-software-tutorial/IC783974.png "Adhatnak a felhasználóknak")

3.  Jelölje ki a próba-felhasználó, kattintson a **hozzárendelni**, és kattintson az **Igen gombra** kattintva erősítse meg a hozzárendelés.

    ![Igen] (./media/active-directory-saas-igloo-software-tutorial/IC767830.png "Igen")
  
Ha meg szeretné tesztelje a egyszeri bejelentkezéses, Access Panel megnyitásához. Az Access ablaktábla, lásd: [Bevezetés az Access panelre](active-directory-saas-access-panel-introduction.md).