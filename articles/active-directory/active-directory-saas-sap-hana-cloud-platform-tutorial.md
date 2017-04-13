<properties 
    pageTitle="Oktatóprogram: Azure Active Directory-integráció a SAP HANA felhő Platform |} Microsoft Azure" 
    description="Megtudhatja, hogy miként SAP HANA felhő Platform használata az Azure Active Directory ahhoz, hogy az egyszeri bejelentkezés, automatikus kiépítési és az egyéb!" 
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

#<a name="tutorial-azure-active-directory-integration-with-sap-hana-cloud-platform"></a>Oktatóprogram: Azure Active Directory-integráció a SAP HANA felhő Platform
  
Ebben az oktatóanyagban célja Azure és az SAP HANA felhő Platform integrációja megjelenítéséhez.  
Az alkalmazási példát, ebben az oktatóanyagban tagolt feltételezi, hogy már rendelkezik az alábbiakat:

-   Érvényes Azure előfizetés
-   Egy SAP HANA felhő Platform-fiókkal
  
Ebben az oktatóanyagban befejeztével az SAP HANA felhő Platform rendelt Azure AD-felhasználók fogja tudni az alkalmazás használatáról a [Bevezetés az Access panelen](active-directory-saas-access-panel-introduction.md)egyszeri bejelentkezési.

>[AZURE.IMPORTANT]Kell a saját alkalmazások telepítése vagy az alkalmazáshoz a SAP HANA felhő Platform-fiókban egyszeri bejelentkezési tesztelje a feliratkozás. Ebben az oktatóprogramban az alkalmazás telepítve van a fiók.
  
A következő építőelemek az alkalmazási példát, ebben az oktatóanyagban tagolt áll:

1.  Az SAP HANA felhő platform alkalmazás integráció engedélyezése
2.  Egyszeri bejelentkezés beállítása
3.  Szerepkör hozzárendelése egy felhasználóhoz
4.  Felhasználók hozzárendelése

![Eset] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790795.png "Eset")
##<a name="enabling-the-application-integration-for-sap-hana-cloud-platform"></a>Az SAP HANA felhő platform alkalmazás integráció engedélyezése
  
Ez a szakasz célja, hogyan SAP HANA felhő platform alkalmazás alkalmazással való integráció engedélyezése a szerkezet.

###<a name="to-enable-the-application-integration-for-sap-hana-cloud-platform-perform-the-following-steps"></a>Ahhoz, hogy az alkalmazás integrálását SAP HANA felhő platform, hajtsa végre az alábbi lépéseket:

1.  Az Azure felügyeleti portálon kattintson a bal oldali navigációs területen kattintson az **Active Directory**.

    ![Az Active Directory] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC700993.png "Az Active Directory")

2.  A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3.  Nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson a felső menüben **alkalmazásokat** .

    ![Alkalmazások] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC700994.png "Alkalmazások")

4.  Kattintson a **Hozzáadás** az oldal alján.

    ![Alkalmazás hozzáadása] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC749321.png "Alkalmazás hozzáadása")

5.  **Milyen feladatot szeretne tenni** párbeszédpanelen kattintson **a gyűjteményből az alkalmazások felvétele**.

    ![Gallerry az alkalmazás hozzáadása] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC749322.png "Gallerry az alkalmazás hozzáadása")

6.  A **Keresés mezőbe**írja be az **SAP HANA felhő Platform**.

    ![Alkalmazás gyűjtemény] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790796.png "Alkalmazás gyűjtemény")

7.  Az eredmény ablaktáblában jelölje ki az **SAP HANA felhő Platform**, és válassza a **kész** , az alkalmazás hozzáadása parancsra.

    ![SAP – Hana] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC793929.png "SAP – Hana")
##<a name="configuring-single-sign-on"></a>Egyszeri bejelentkezés beállítása
  
Ez a szakasz célja tagolása engedélyezése a felhasználóknak, hogy az SAP HANA felhő Platform hitelesíteni a fiókkal az összevonási alapján a SAML protokoll használatával Azure AD.  
Ez az eljárás részeként biztosan szükséges számrendszerben-64 kódolt tanúsítvány feltöltése az SAP HANA felhő Platform bérlői webhelyen.  
Ha nem ismeri a ezt az eljárást, megtudhatja, [hogy miként szövegfájlba bináris tanúsítvány konvertálása](http://youtu.be/PlgrzUZ-Y1o)

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Egyszeri bejelentkezés beállításához hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon **SAP HANA felhő Platform** alkalmazás integrációs lapon kattintson **konfigurálása az egyszeri bejelentkezés** a **Konfigurálása az egyszeri bejelentkezés** párbeszédpanel megnyitásához.

    ![Beállítás az egyszeri bejelentkezés] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC778552.png "Beállítás az egyszeri bejelentkezés")

2.  **Hogyan szeretné, hogy jelentkezzen be az SAP HANA felhő Platform felhasználók** lapon jelölje be **A Microsoft Azure Active Directory Single Sign-On**, és kattintson a **Tovább gombra**.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790797.png "Egyszeri bejelentkezés beállítása")

3.  A különböző webes böngészőablakban jelentkezzen be az SAP HANA felhő Platform vezérlőpultot https://account. \<fekvő host\>.ondemand.com/cockpit (pl.: *https://account.hanatrial.ondemand.com/cockpit*).

4.  Kattintson a **megbízható** fülre.

    ![A megbízható gombra] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790800.png "A megbízható gombra")

5.  Az adatvédelmi kezelése csoportban hajtsa végre az alábbi lépéseket:

    ![Metaadat-alapú beszerzése] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC793930.png "Metaadat-alapú beszerzése")

    1.  Kattintson a **Helyi szolgáltatónak** fülre.
    2.  Az SAP HANA felhő Platform metaadatokat tartalmazó fájl letöltéséhez kattintson az **Első metaadat-alapú**.

6.  Az Azure Active klasszikus portálon **App URL-címe konfigurálása** lapon hajtsa végre az alábbi lépéseket, és kattintson a **Tovább gombra**.

    ![Állítsa be az App URL-címe] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790798.png "Állítsa be az App URL-címe")

    1.  **Bejelentkezési a URL-cím** mezőbe írja be az **SAP HANA felhő Platform** alkalmazásba bejelentkezni a felhasználók által használt URL-CÍMÉT. Ez a fiók-specifikus URL-CÍMÉT az SAP HANA felhő Platform alkalmazásban védett erőforrás. Az URL-cím alapján a következő mintát: *https://\<applicationName\>\<fióknév\>.\< fekvő host\>.ondemand.com/\<elérési út\_való\_védett\_erőforrás\> * (pl.: *https://xleavep1941203872trial.hanatrial.ondemand.com/xleave*)

        >[AZURE.NOTE]Ez az URL-CÍMÉT az SAP HANA felhő Platform alkalmazásban, a felhasználónak hitelesítést végezni.

    2.  Nyissa meg a letöltött SAP HANA felhő Platform metaadat-fájlt, és keresse meg a **ns3:AssertionConsumerService** címke.
    3.  A **hely** attribútum értékét másolja és illessze be a **SAP HANA felhő Platform válasz URL-címe** mezőben lévő értéket.

7.  **Konfigurálása az egyszeri bejelentkezés SAP HANA felhő Platform a** lapon a metaadat-alapú letöltéséhez kattintson a **metaadat-alapú letöltése**, és mentse a fájlt a számítógépen parancsra.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790799.png "Egyszeri bejelentkezés beállítása")

8.  Az SAP HANA felhő Platform Vezérlőpultot, a **Helyi szolgáltató** csoportban hajtsa végre az alábbi lépéseket:

    ![A megbízható kezelése] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC793931.png "A megbízható kezelése")

    1.  Kattintson a **Szerkesztés**gombra.
    2.  **Konfigurációs típusa**válassza az **egyéni**.
    3.  **Helyi szolgáltató neve**hagyja meg az alapértelmezett értéket.
    4.  Egy **Aláírási billentyűt** , és két fő **Aláíró tanúsítvány** szeretne, kattintson a **Kulcs párt készítése**.
    5.  **Egyszerű propagálása**jelölje ki a **letiltott**.
    6.  **Kötelező hitelesítés**jelölje ki a **tiltva**.
    7.  Kattintson a **Mentés**gombra.

9.  Kattintson a **Megbízható identitásszolgáltató** fülre, és kattintson a **Megbízható identitásszolgáltató hozzáadása**gombra.

    ![A megbízható kezelése] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790802.png "A megbízható kezelése")

    >[AZURE.NOTE]Megbízható Identitásszolgáltatók listájának kezelése, meg kell az egyéni konfigurációs típus választotta, a helyi szolgáltató szakaszban. Az alapértelmezett konfigurációs típusa, ha egy nem szerkeszthetők és implicit hozzáférés az SAP Dokumentumazonosító szolgáltatás. Ha nincs nincs adatvédelmi beállításaitól.

10. Kattintson az **Általános** fülre, és kattintson a **Tallózás gombra** a metaadat-alapú letöltött fájl feltöltése gombra.

    ![A megbízható kezelése] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC793932.png "A megbízható kezelése")

    >[AZURE.NOTE] A metaadat-alapú feltöltést követően az értékek **egyszeri bejelentkezéses URL-CÍMÉT**, a **Egyetlen kijelentkezés URL-cím** és az **Aláíró tanúsítvány** automatikusan bekerülnek.

11. Kattintson a **attribútumok** fülre.

12. Az **attribútumok** lapon hajtsa végre az alábbi lépéseket:

    ![Attribútumok] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790804.png "Attribútumok")

    1.  **Add Assertion-Based attribútum**gombra kattintva adja hozzá a következő állítás-alapú attribútumok:

        |Állítás attribútum| Egyszerű attribútum|
        |-------------------|--------------------|
        |http://schemas.xmlsoap.org/ws/2005/05/Identity/Claims/givenName|   Utónév|--------------------|--------------------|
        |http://schemas.xmlsoap.org/ws/2005/05/Identity/Claims/surname|        Utónév|-----------|
        |http://schemas.xmlsoap.org/ws/2005/05/Identity/Claims/emailaddress|e-mailben|

    >[AZURE.NOTE]A konfiguráció attribútumai attól függ, hogyan az alkalmazásokat, a HCP kifejlesztett, azaz mely attribútum, a SAML válasz elvárt, és mely néven (egyszerű attribútum) próbálnak hozzáférni az attribútum a kódot.
    >  
    >egy.  Az **Attribútum alapértelmezett** a képernyőképet csak ábra célra van. Ellenőrizze az alkalmazási példát használata nem kötelező.  
    >
    >b.  A nevek és **Egyszerű attribútum** a képernyőképet látható értékeket függenek, hogy az alkalmazás fejlesztése. Akkor lehet, hogy az alkalmazás kell lennie a különböző hozzárendeléseket.

13. Az Azure klasszikus portálon **konfigurálása az egyszeri bejelentkezés az SAP HANA felhő Platform** párbeszéd lapon jelölje ki az egyszeri bejelentkezéses konfigurációs megerősítést kérő, és válassza a **kész**gombra.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC796933.png "Egyszeri bejelentkezés beállítása")
  
Nem kötelező lépésként beállítható csoportok állítás-alapú az Azure Active Directory identitásszolgáltató

>[AZURE.NOTE]Csoportok SAP HANA felhő platformon eszközével használ egy vagy több felhasználó dinamikusan hozzárendelése egy vagy több szerepkör határozza meg a SAML 2.0-s állítás attribútumok értékeket az SAP HANA felhő Platform alkalmazásban. Ha például a állítás attribútumot tartalmazó "*Szerződés = ideiglenes*", célszerű lehet hozzáadni a "*ideiglenes*" csoport minden érintett felhasználónál. A "*ideiglenes*" csoport egy vagy több szerepkörök telepítését SAP HANA felhő Platform-fiókját az egy vagy több alkalmazásokból is tartalmazhatnak.
>  
>Ha szeretne egy vagy több szerepkörök SAP HANA felhő Platform-fiókját az alkalmazások sok felhasználó tömeges hozzárendelése, használja a csoportok állítás-alapú. Ha csak egyetlen vagy kis számos felhasználó hozzárendelése (a) adott szerepkör(ök), javasoljuk, hogy közvetlenül az "**engedélyek**" lapjának az SAP HANA felhő Platform vezérlőpultot hozzárendelésükre.

##<a name="assigning-a-role-to-a-user"></a>Szerepkör hozzárendelése egy felhasználóhoz
  
Ahhoz, hogy az Azure Active Directory-felhasználóknak, hogy jelentkezzen be az SAP HANA felhő Platform, akkor is hozzá kell rendelnie az SAP HANA felhő Platform szerepkörök őket.

###<a name="to-assign-a-role-to-a-user-perform-the-following-steps"></a>A szerepkör hozzárendelése egy felhasználóhoz, hajtsa végre az alábbi lépéseket:

1.  Jelentkezzen be az **SAP HANA felhő Platform** vezérlőpultot.

2.  Hajtsa végre az alábbi lépéseket:

    ![Engedélyek] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790805.png "Engedélyek")

    1.  Kattintson az **Engedélyezés**gombra.
    2.  Kattintson a **felhasználók** fülre.
    3.  A **felhasználó** mezőbe írja be a felhasználó e-mail címét.
    4.  Jelölje be a felhasználó hozzárendelése a szerepkör **hozzárendelése** .
    5.  Kattintson a **Mentés**gombra.

##<a name="assigning-users"></a>Felhasználók hozzárendelése
  
Tesztelje a konfigurációt, meg kell adni az Azure Active Directory-felhasználók, használja az alkalmazás elérési útvonalát társításával engedélyezni szeretné.

###<a name="to-assign-users-to-sap-hana-cloud-platform-perform-the-following-steps"></a>Felhasználók hozzárendelése SAP HANA felhő Platform, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon próba-fiók létrehozása.

2.  Az **SAP HANA felhő Platform** alkalmazás integrációs lapon kattintson a **hozzárendelését a felhasználókhoz**.

    ![Adhatnak a felhasználóknak] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790806.png "Adhatnak a felhasználóknak")

3.  Jelölje ki a próba-felhasználó, kattintson a **hozzárendelni**, és kattintson az **Igen gombra** kattintva erősítse meg a hozzárendelés.

    ![Igen] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC767830.png "Igen")
  
Ha meg szeretné tesztelje a egyszeri bejelentkezéses, Access Panel megnyitásához. Ha többet szeretne tudni az Access panelen című témakörben [az Access-panelen](active-directory-saas-access-panel-introduction.md).