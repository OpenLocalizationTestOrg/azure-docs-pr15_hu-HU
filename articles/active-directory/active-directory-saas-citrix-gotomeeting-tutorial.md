<properties 
    pageTitle="Oktatóprogram: Azure Active Directory-integráció a Citrix GoToMeeting |} Microsoft Azure" 
    description="Megtudhatja, hogyan használhatja a Citrix GoToMeeting az Azure Active Directory ahhoz, hogy az egyszeri bejelentkezés, automatikus kiépítési és az egyéb!." 
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
    ms.date="08/16/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-citrix-gotomeeting"></a>Oktatóprogram: Azure Active Directory-integráció a Citrix GoToMeeting  
Vonatkozik: Azure

>[AZURE.TIP]Visszajelzés, kattintson [ide](http://go.microsoft.com/fwlink/?LinkId=522412).

Ebben az oktatóanyagban célja integrálása az Azure és Citrix GoToMeeting megjelenítéséhez. Az alkalmazási példát, ebben az oktatóanyagban tagolt feltételezi, hogy már rendelkezik az alábbiakat:

-   Érvényes Azure előfizetés
-   A Citrix GoToMeeting bérlői webhelyre

A következő építőelemek az alkalmazási példát, ebben az oktatóanyagban tagolt áll:

1.  Az alkalmazás integrációját Citrix GoToMeeting engedélyezése
2.  Egyszeri bejelentkezés beállítása
3.  Felhasználói kiépítési konfigurálása
4.  Felhasználók hozzárendelése

![Konfiguráció] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC768996.png "Konfiguráció")



##<a name="enabling-the-application-integration-for-citrix-gotomeeting"></a>Az alkalmazás integrációját Citrix GoToMeeting engedélyezése

Ez a szakasz célja, hogyan Citrix GoToMeeting az alkalmazás alkalmazással való integráció engedélyezése a szerkezet.

###<a name="to-enable-the-application-integration-for-citrix-gotomeeting-perform-the-following-steps"></a>Ahhoz, hogy az alkalmazás integrációját Citrix GoToMeeting, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon kattintson a bal oldali navigációs területen kattintson az **Active Directory**.

    ![Az Active Directory] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC700993.png "Az Active Directory")

2.  A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3.  Nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson a felső menüben **alkalmazásokat** .

    ![Alkalmazások] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC700994.png "Alkalmazások")

4.  Kattintson a **Hozzáadás** az oldal alján.

    ![Alkalmazás hozzáadása] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC749321.png "Alkalmazás hozzáadása")

5.  **Milyen feladatot szeretne tenni** párbeszédpanelen kattintson **a gyűjteményből az alkalmazások felvétele**.

    ![Gyűjteményből alkalmazás hozzáadása] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC749322.png "Gyűjteményből alkalmazás hozzáadása")

6.  A **Keresés mezőbe**írja be a **Citrix GoToMeeting**.

    ![Citrix GoToMeeting] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC701006.png "Citrix GoToMeeting")

7.  Az eredmény ablaktáblában jelölje ki a **Citrix GoToMeeting**, és válassza a **kész** , az alkalmazás hozzáadása gombra.

    ![Citrix GoToMeeting] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC701012.png "Citrix GoToMeeting")
##<a name="configuring-single-sign-on"></a>Egyszeri bejelentkezés beállítása

Ez a szakasz célja tagolása engedélyezése a felhasználóknak, hogy Citrix GoToMeeting hitelesíteni a fiókkal az összevonási alapján a SAML protokoll használatával Azure AD.  
Ez az eljárás részeként szükséges számrendszerben-64 kódolt tanúsítvány feltöltése a Citrix GoToMeeting bérlő.  
Ha nem ismeri a ezt az eljárást, olvassa el a [szövegfájlba bináris tanúsítvány konvertálása](http://youtu.be/PlgrzUZ-Y1o)című témakört.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Egyszeri bejelentkezés beállításához hajtsa végre az alábbi lépéseket:

1.  **Citrix GoToMeeting** alkalmazás integrációs lapon kattintson **konfigurálása az egyszeri bejelentkezés** a **EGYSZERI bejelentkezési tovább beállítása** párbeszédpanel megnyitásához.

    ![Egyszeri bejelentkezés engedélyezése] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC768997.png "Egyszeri bejelentkezés engedélyezése")

2.  **Hogyan szeretné, hogy jelentkezzen be az Citrix GoToMeeting felhasználók** lapon jelölje be **A Microsoft Azure Active Directory Single Sign-On**.

    ![Beállítás az egyszeri bejelentkezés] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC768998.png "Beállítás az egyszeri bejelentkezés")


3. Az **Alkalmazás beállításainak konfigurálása** lapon kattintson a **Tovább**gombra. 

    ![Egyszeri bejelentkezés engedélyezése] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC7689981.png "Egyszeri bejelentkezés engedélyezése")

4.  A **beállítás az egyszeri bejelentkezés Citrix GoToMeeting a** lapon kattintson a **tanúsítvány letöltése**, és mentse a tanúsítvány fájlt a számítógépen.

    ![Beállítás az egyszeri bejelentkezés] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC768999.png "Beállítás az egyszeri bejelentkezés")

5.  Egy másik böngészőablakban jelentkezzen be a Citrix szervezeti központ ([https://account.citrixonline.com/organization/administration/](https://account.citrixonline.com/organization/administration/)).

6. Kattintson a **Identitásszolgáltató** fülre, és hajtsa végre az alábbi lépéseket:  

    ![SAML beállítása] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC6892321.png "SAML beállítása")

    egy. Válassza a **manuális**

    
    b. Az Azure klasszikus portálon **konfigurálása az egyszeri bejelentkezés a Citrix GoToMeeting** párbeszédpanel lapon a **Bejelentkezési URL-címe** értéket másolja és illessze be a **bejelentkezési URL-címe** mezőben lévő értéket. 

    
    c billentyűkombinációt. Az Azure klasszikus portálon **konfigurálása az egyszeri bejelentkezés a Citrix GoToMeeting** párbeszédpanel lapon a **Sign-Out lap URL-címe** értéket másolja és illessze be a **kijelentkezési lap URL-címe** mezőben lévő értéket.

    
    d. Az Azure klasszikus portálon **konfigurálása az egyszeri bejelentkezés a Citrix GoToMeeting** párbeszédpanel lapon a **Szervezet azonosítója** értéket másolja és illessze be a **Identitás szolgáltató szervezet azonosítója** mezőben lévő értéket.

   
    e. A letöltött tanúsítvány feltölteni, kattintson a **Tanúsítvány feltöltése**.

    
    f. Kattintson a **Mentés**gombra.

6.  Az Azure klasszikus portálon jelölje be a egyszeri bejelentkezéses konfigurációs megerősítés, és kattintson a **Tovább gombra**.

    ![Beállítás az egyszeri bejelentkezés] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC769000.png "Beállítás az egyszeri bejelentkezés")


7. Az **egyszeri bejelentkezés megerősítés** lapon kattintson a **Befejezés**gombra.

    ![SAML beállítása] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC7689982.png "SAML beállítása")





##<a name="configuring-user-provisioning"></a>Felhasználói kiépítési konfigurálása

Ez a szakasz célja, hogyan engedélyezhető az Active Directory felhasználói fiókok Citrix GoToMeeting kiépítési tagolása.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Felhasználói kiépítési beállításához hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon **Citrix GoToMeeting** alkalmazás integrációs lapon kattintson a **konfigurálása felhasználói kiépítési** a **Felhasználó kiépítési beállítása** párbeszédpanel megnyitásához.

    ![Konfigurálása felhasználói kiépítése] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC769001.png "Konfigurálása felhasználói kiépítése")

2.  A **Beállítások és a rendszergazdai hitelesítő adatait** lapon hajtsa végre az alábbi lépéseket:

    ![Konfigurálása felhasználói kiépítése] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC769002.png "Konfigurálása felhasználói kiépítése")

    egy. Az **Citrix GoToMeeting felügyeleti felhasználónév** mezőbe írja be a felhasználó nevét a rendszergazda.

    
    b. Az **Citrix GoToMeeting rendszergazdai jelszó** mezőbe a rendszergazdai jelszavát.

    
    c billentyűkombinációt. Kattintson a **Tovább**gombra.

3.  A **Megerősítés** lapon kattintson a jelet a konfiguráció mentéséhez.

4.  Kattintson **az érvényesítés** gombra a beállításainak ellenőrzése.


##<a name="assigning-users"></a>Felhasználók hozzárendelése

Tesztelje a konfigurációt, meg kell adni az Azure Active Directory-felhasználók, használja az alkalmazás elérési útvonalát társításával engedélyezni szeretné.

###<a name="to-assign-users-to-citrix-gotomeeting-perform-the-following-steps"></a>Felhasználók hozzárendelése Citrix GoToMeeting, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon próba-fiók létrehozása.

2.  Kattintson a **Citrix GoToMeeting** alkalmazás integrációs lap **hozzárendelését a felhasználókhoz**.

    ![Adhatnak a felhasználóknak] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC769003.png "Adhatnak a felhasználóknak")

3.  Jelölje ki a próba-felhasználó, kattintson a **hozzárendelni**, és kattintson az **Igen gombra** kattintva erősítse meg a hozzárendelés.

    ![Igen] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC767830.png "Igen")

Érdemes most várja meg a 10 perc, és ellenőrizze, hogy a fiók lett szinkronizálva a Dropbox vállalati verziójával.

Első lépésként ellenőrzés jelölje be a kiépítési állapotát, az oldalon **Citrix GoToMeeting** alkalmazás integrálása az Azure klasszikus portálon d irányítópult gombra kattintva.

![Irányítópult] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC769004.png "Irányítópult")

Ciklikus kiépítési sikeresen befejeződött felhasználó egy kapcsolódó állapotát jelzi:

![Integráció állapotát] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC769005.png "Integráció állapotát")

Ha meg szeretné tesztelje a egyszeri bejelentkezéses, Access Panel megnyitásához.

Ha többet szeretne tudni az Access panelen című témakörben [az Access-panelen](https://msdn.microsoft.com/library/dn308586).
