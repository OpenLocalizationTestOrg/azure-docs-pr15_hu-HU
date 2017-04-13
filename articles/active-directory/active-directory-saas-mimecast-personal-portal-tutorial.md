<properties 
    pageTitle="Oktatóprogram: Azure Active Directory-integráció a Mimecast személyes portál |} Microsoft Azure" 
    description="Megtudhatja, hogy miként Mimecast személyes portál használata az Azure Active Directory ahhoz, hogy az egyszeri bejelentkezés, automatikus kiépítési és az egyéb!" 
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

#<a name="tutorial-azure-active-directory-integration-with-mimecast-personal-portal"></a>Oktatóprogram: Azure Active Directory-integráció a Mimecast személyes portál
  
Ebben az oktatóanyagban célja Azure és Mimecast személyes Portal integrációja megjelenítéséhez.  
Az alkalmazási példát, ebben az oktatóanyagban tagolt feltételezi, hogy már rendelkezik az alábbiakat:

-   Érvényes Azure előfizetés
-   A személyes portál Mimecast egyszeri bejelentkezés engedélyezve van az előfizetés
  
Ebben az oktatóanyagban befejeztével az Azure Active Directory felhasználók Mimecast személyes portál rendelt tudnak egyetlen bejelentkezés az alkalmazásba, a Mimecast személyes vállalat portálwebhely (service provider által kezdeményezett bejelentkezés) és használatáról a [Bevezetés az Access panelre](active-directory-saas-access-panel-introduction.md).
  
A következő építőelemek az alkalmazási példát, ebben az oktatóanyagban tagolt áll:

1.  Az alkalmazás integrálását Mimecast személyes portál engedélyezése
2.  Egyszeri bejelentkezés beállítása
3.  Felhasználói kiépítési konfigurálása
4.  Felhasználók hozzárendelése

![Eset] (./media/active-directory-saas-mimecast-personal-portal-tutorial/IC794991.png "Eset")
##<a name="enabling-the-application-integration-for-mimecast-personal-portal"></a>Az alkalmazás integrálását Mimecast személyes portál engedélyezése
  
Ez a szakasz célja, hogyan Mimecast személyes portál alkalmazás alkalmazással való integráció engedélyezése a szerkezet.

###<a name="to-enable-the-application-integration-for-mimecast-personal-portal-perform-the-following-steps"></a>Ahhoz, hogy az alkalmazás integrálását Mimecast személyes portál, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon kattintson a bal oldali navigációs területen kattintson az **Active Directory**.

    ![Az Active Directory] (./media/active-directory-saas-mimecast-personal-portal-tutorial/IC700993.png "Az Active Directory")

2.  A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3.  Nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson a felső menüben **alkalmazások** .

    ![Alkalmazások] (./media/active-directory-saas-mimecast-personal-portal-tutorial/IC700994.png "Alkalmazások")

4.  Kattintson a **Hozzáadás** az oldal alján.

    ![Alkalmazás hozzáadása] (./media/active-directory-saas-mimecast-personal-portal-tutorial/IC749321.png "Alkalmazás hozzáadása")

5.  **Milyen feladatot szeretne tenni** párbeszédpanelen kattintson **a gyűjteményből az alkalmazások felvétele**.

    ![Gallerry az alkalmazás hozzáadása] (./media/active-directory-saas-mimecast-personal-portal-tutorial/IC749322.png "Gallerry az alkalmazás hozzáadása")

6.  A **Keresés mezőbe**írja be a **Mimecast személyes portálon**.

    ![Alkalmazás gyűjtemény] (./media/active-directory-saas-mimecast-personal-portal-tutorial/IC794992.png "Alkalmazás gyűjtemény")

7.  Az eredmény ablaktáblában jelölje ki a **Mimecast személyes portál**, és válassza a **kész** , az alkalmazás hozzáadása gombra.

    ![Mimecast személyes portál] (./media/active-directory-saas-mimecast-personal-portal-tutorial/IC794993.png "Mimecast személyes portál")
##<a name="configuring-single-sign-on"></a>Egyszeri bejelentkezés beállítása
  
Ez a szakasz célja tagolása engedélyezése a felhasználóknak, hogy a saját fiók az összevonási alapján a SAML protokoll használatával Azure Active Directory hitelesítő Mimecast személyes portálra.  
Ez az eljárás részeként biztosan szükséges számrendszerben-64 kódolt tanúsítvány fájlt.  
Ha nem ismeri a ezt az eljárást, olvassa el a [szövegfájlba bináris tanúsítvány konvertálása](http://youtu.be/PlgrzUZ-Y1o)című témakört.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Egyszeri bejelentkezés beállításához hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon **Mimecast személyes portál** alkalmazás integrációs lapon kattintson **konfigurálása az egyszeri bejelentkezés** a **Konfigurálása az egyszeri bejelentkezés** párbeszédpanel megnyitásához.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-mimecast-personal-portal-tutorial/IC794994.png "Egyszeri bejelentkezés beállítása")

2.  **Hogyan szeretné, hogy jelentkezzen be az Mimecast személyes portál felhasználók** lapon jelölje be **A Microsoft Azure Active Directory Single Sign-On**, és kattintson a **Tovább gombra**.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-mimecast-personal-portal-tutorial/IC794995.png "Egyszeri bejelentkezés beállítása")

3.  Az **Alkalmazás URL-cím beállítása** lapon az **Mimecast személyes portál bejelentkezési a URL-cím** mezőbe írja be a bejelentkezni az Mimecast személyes portál alkalmazást a felhasználók által használt URL-CÍMÉT (például: "https://webmail-uk.mimecast.com" vagy "https://webmail-us.mimecast.com"), majd kattintson a **Tovább**gombra.

    >[AZURE.NOTE] A bejelentkezési URL-címet adott régióban.

    ![Állítsa be az App URL-címe] (./media/active-directory-saas-mimecast-personal-portal-tutorial/IC794996.png "Állítsa be az App URL-címe")

4.  A **konfigurálása az egyszeri bejelentkezés Mimecast személyes portált a** lapon a tanúsítvány letöltése, kattintson a **tanúsítvány letöltése**, és mentse a tanúsítvány fájlt a számítógépen helyben parancsra.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-mimecast-personal-portal-tutorial/IC794997.png "Egyszeri bejelentkezés beállítása")

5.  A különböző webes böngészőablakban jelentkezzen be az Mimecast személyes portál rendszergazdaként.

6.  Nyissa meg a **szolgáltatások \> alkalmazás**.

    ![Alkalmazások] (./media/active-directory-saas-mimecast-personal-portal-tutorial/IC794998.png "Alkalmazások")

7.  Kattintson a **hitelesítés profilok**gombra.

    ![Hitelesítés profilok] (./media/active-directory-saas-mimecast-personal-portal-tutorial/IC794999.png "Hitelesítés profilok")

8.  Kattintson az **új hitelesítési profilt**.

    ![Új hitelesítési Profil] (./media/active-directory-saas-mimecast-personal-portal-tutorial/IC795000.png "Új hitelesítési Profil")

9.  **Hitelesítés profil** csoportban hajtsa végre az alábbi lépéseket:

    ![Hitelesítés Profil] (./media/active-directory-saas-mimecast-personal-portal-tutorial/IC795001.png "Hitelesítés Profil")

    1.  A **Leírás** mezőbe írja be a konfigurációban nevét.
    2.  Jelölje be **a hivatkozási SAML hitelesítési Mimecast személyes portál**.
    3.  Jelölje ki a **szolgáltató**, **Azure Active Directory**.
    4.  Az Azure klasszikus portálon **konfigurálása az egyszeri bejelentkezés Mimecast személyes portált a** párbeszédpanel lapon a **Kibocsátó URL-címe** értéket másolja és illessze be a **Kibocsátó URL-címe** mezőben lévő értéket.
    5.  Az Azure klasszikus portálon **konfigurálása az egyszeri bejelentkezés Mimecast személyes portált a** párbeszédpanel lapon a **Távoli bejelentkezési URL-cím** értéket másolja és illessze be a **Bejelentkezési URL-cím** mezőben lévő értéket.
    6.  Az Azure klasszikus portálon **konfigurálása az egyszeri bejelentkezés Mimecast személyes portált a** párbeszédpanel lapon a **Távoli bejelentkezési URL-cím** értéket másolja és illessze be a **Kijelentkezés URL-címe** mezőben lévő értéket.  

        >[AZURE.NOTE] A bejelentkezési URL-cím értéket, és a Kijelentkezés URL-címe értéket az - a Mimecast személyes portált a azonos.

    7.  A letöltött tanúsítvány **Alap-64-címként kódolt** fájl létrehozása  

        >[AZURE.TIP]További részletekért megtudhatja, [hogy miként szövegfájlba bináris tanúsítvány konvertálja](http://youtu.be/PlgrzUZ-Y1o).

    8.  Nyissa meg az alap-64 kódolt tanúsítvány a Jegyzettömb alkalmazásban, távolítsa el az első sor ("*--*") és az utolsó sor ("*--*"), a hátralévő tartalmát másolja a vágólapra, és illessze be a **Identitás szolgáltató tanúsítvány (metaadatok)** mezőben lévő értéket szeretné.
    9.  **Engedélyezze, hogy egyszeri bejelentkezési a**kijelölése
    10. Kattintson a **Mentés**gombra.

10. Az Azure klasszikus portálon jelölje be a egyszeri bejelentkezéses konfigurációs megerősítés, és válassza a **kész** , **Állítsa be az egyszeri bejelentkezés** párbeszédpanel bezárásához.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-mimecast-personal-portal-tutorial/IC795002.png "Egyszeri bejelentkezés beállítása")
##<a name="configuring-user-provisioning"></a>Felhasználói kiépítési konfigurálása
  
Ahhoz, hogy az Azure Active Directory-felhasználóknak, hogy jelentkezzen be a személyes portál Mimecast, hogy ki kell építenie őket Mimecast személyes portálra.  
Mimecast személyes portál, ha a feladat manuális kiépítési.
  
Regisztrálnia kell tartományt felhasználók létrehozása előtt.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Felhasználói kiépítési beállításához hajtsa végre az alábbi lépéseket:

1.  Jelentkezzen be rendszergazdaként az **Mimecast személyes portálon** .

2.  Nyissa meg a **könyvtárak \> belső**.

    ![Könyvtárak] (./media/active-directory-saas-mimecast-personal-portal-tutorial/IC795003.png "Könyvtárak")

3.  Kattintson az **Új tartomány regisztrálását**.

    ![Új tartomány regisztrálását] (./media/active-directory-saas-mimecast-personal-portal-tutorial/IC795004.png "Új tartomány regisztrálását")

4.  Új tartományhoz a létrehozása után kattintson az **Új címet**.

    ![Új cím] (./media/active-directory-saas-mimecast-personal-portal-tutorial/IC795005.png "Új cím")

5.  Az új címet párbeszédpanelen hajtsa végre az alábbi lépéseket:

    ![Mentés] (./media/active-directory-saas-mimecast-personal-portal-tutorial/IC795006.png "Mentés")

    1.  Írja be a kapcsolódó szövegdobozok kiépítése kívánt érvényes AAD fiók **E-mail címét**, a **Globális nevét**, a **jelszó** és a **Jelszó megerősítése** attribútumok.
    2.  Kattintson a **Mentés**gombra.

>[AZURE.NOTE]Személyes portál Mimecast felhasználói fiók létrehozása eszközök és rendelkezést AAD felhasználói fiókok Mimecast személyes portálja által megadott API-khoz is használhatja.

##<a name="assigning-users"></a>Felhasználók hozzárendelése

Tesztelje a konfigurációt, meg kell adni az Azure Active Directory-felhasználók, használja az alkalmazás elérési útvonalát társításával engedélyezni szeretné.

###<a name="to-assign-users-to-mimecast-personal-portal-perform-the-following-steps"></a>Felhasználók hozzárendelése Mimecast személyes portál, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon próba-fiók létrehozása.

2.  A **Személyes portál Mimecast **alkalmazás integrációs lapon kattintson a **hozzárendelését a felhasználókhoz**.

    ![Hozzárendelését a felhasználókhoz] (./media/active-directory-saas-mimecast-personal-portal-tutorial/IC795007.png "Adhatnak a felhasználóknak")

3.  Jelölje ki a próba-felhasználó, kattintson a **hozzárendelni**, és kattintson az **Igen gombra** kattintva erősítse meg a hozzárendelés.

    ![Igen] (./media/active-directory-saas-mimecast-personal-portal-tutorial/IC767830.png "Igen")
  
Ha meg szeretné tesztelje a egyszeri bejelentkezéses, Access Panel megnyitásához. Az Access ablaktábla, lásd: [Bevezetés az Access panelre](active-directory-saas-access-panel-introduction.md).