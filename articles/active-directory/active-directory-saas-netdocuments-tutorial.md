<properties 
    pageTitle="Oktatóprogram: Azure Active Directory-integráció a NetDocuments |} Microsoft Azure" 
    description="Megtudhatja, hogyan használhatja a NetDocuments az Azure Active Directory ahhoz, hogy az egyszeri bejelentkezés, automatikus kiépítési és az egyéb!" 
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

#<a name="tutorial-azure-active-directory-integration-with-netdocuments"></a>Oktatóprogram: Azure Active Directory-integráció a NetDocuments
  
Ebben az oktatóanyagban célja integrálása az Azure és NetDocuments megjelenítéséhez.  
Az alkalmazási példát, ebben az oktatóanyagban tagolt feltételezi, hogy már rendelkezik az alábbiakat:

-   Érvényes Azure előfizetés
-   NetDocuments bérlői webhelyre
  
Ebben az oktatóanyagban befejeztével az Azure Active Directory-felhasználók NetDocuments rendelt fogja tudni az alkalmazás a NetDocuments vállalati webhely (service provider által kezdeményezett bejelentkezés) és használatáról a [Bevezetés az Access panelen](active-directory-saas-access-panel-introduction.md)az egyszeri bejelentkezési.
  
A következő építőelemek az alkalmazási példát, ebben az oktatóanyagban tagolt áll:

1.  Az alkalmazás integrációját NetDocuments engedélyezése
2.  Egyszeri bejelentkezés beállítása
3.  Felhasználói kiépítési konfigurálása
4.  Felhasználók hozzárendelése

![Eset] (./media/active-directory-saas-netdocuments-tutorial/IC795040.png "Eset")
##<a name="enabling-the-application-integration-for-netdocuments"></a>Az alkalmazás integrációját NetDocuments engedélyezése
  
Ez a szakasz célja, hogyan NetDocuments az alkalmazás alkalmazással való integráció engedélyezése a szerkezet.

###<a name="to-enable-the-application-integration-for-netdocuments-perform-the-following-steps"></a>Ahhoz, hogy az alkalmazás integrációját NetDocuments, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon kattintson a bal oldali navigációs területen kattintson az **Active Directory**.

    ![Az Active Directory] (./media/active-directory-saas-netdocuments-tutorial/IC700993.png "Az Active Directory")

2.  A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3.  Nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson a felső menüben **alkalmazások** .

    ![Alkalmazások] (./media/active-directory-saas-netdocuments-tutorial/IC700994.png "Alkalmazások")

4.  Kattintson a **Hozzáadás** az oldal alján.

    ![Alkalmazás hozzáadása] (./media/active-directory-saas-netdocuments-tutorial/IC749321.png "Alkalmazás hozzáadása")

5.  **Milyen feladatot szeretne tenni** párbeszédpanelen kattintson **a gyűjteményből az alkalmazások felvétele**.

    ![Gallerry az alkalmazás hozzáadása] (./media/active-directory-saas-netdocuments-tutorial/IC749322.png "Gallerry az alkalmazás hozzáadása")

6.  A **Keresés mezőbe**írja be a **NetDocuments**.

    ![Alkalmazás gyűjtemény] (./media/active-directory-saas-netdocuments-tutorial/IC795041.png "Alkalmazás gyűjtemény")

7.  Az eredmény ablaktáblában jelölje ki a **NetDocuments**, és válassza a **kész** , az alkalmazás hozzáadása gombra.

    ![NetDocuments] (./media/active-directory-saas-netdocuments-tutorial/IC795042.png "NetDocuments")
##<a name="configuring-single-sign-on"></a>Egyszeri bejelentkezés beállítása
  
Ez a szakasz célja tagolása engedélyezése a felhasználóknak, hogy NetDocuments hitelesíteni a fiókkal az összevonási alapján a SAML protokoll használatával Azure AD.  
Egyszeri bejelentkezés az NetDocuments konfigurálása megköveteli ujjlenyomat érték beolvasása egy tanúsítványt.  
Ha nem ismeri a ezt az eljárást, megtudhatja, [hogy miként beolvasni a tanúsítvány ujjlenyomat értékét](http://youtu.be/YKQF266SAxI).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Egyszeri bejelentkezés beállításához hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon **NetDocuments** alkalmazás integrációs lapon kattintson **konfigurálása az egyszeri bejelentkezés** a **Konfigurálása az egyszeri bejelentkezés** párbeszédpanel megnyitásához.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-netdocuments-tutorial/IC795043.png "Egyszeri bejelentkezés beállítása")

2.  **Hogyan szeretné, hogy jelentkezzen be az NetDocuments felhasználók** lapon jelölje be **A Microsoft Azure Active Directory Single Sign-On**, és kattintson a **Tovább gombra**.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-netdocuments-tutorial/IC795044.png "Egyszeri bejelentkezés beállítása")

3.  Az **Alkalmazás URL-cím beállítása** lapon hajtsa végre az alábbi lépéseket:

    ![Állítsa be az App URL-címe] (./media/active-directory-saas-netdocuments-tutorial/IC795045.png "Állítsa be az App URL-címe")

    1.  **Jelentkezzen be URL-cím** mezőbe írja be a jelentkezzen be az alkalmazás NetDocuments a felhasználók által használt URL-cím (például: "*https://vault.netvoyage.com/neWeb2/docCent.aspx?whr=CA-JI1BG3H1*").
    2.  **NetDocuments válasz URL-cím** mezőbe írja be a beírt azonos értéket az a **Bejelentkezési a URL-címe** mezőben lévő értéket.  

        >[AZURE.NOTE]A helyes értéket, az **Identitáskezelés összevont** párbeszédpanel végén található (lásd: a képernyőképet lépés 9).

    3.  Kattintson a **Tovább** gombra

4.  A **Configure egyszeri bejelentkezés NetDocuments a** lapon a tanúsítvány letöltése, kattintson a **tanúsítvány letöltése**, és mentse a tanúsítvány fájlt a számítógépen helyben parancsra.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-netdocuments-tutorial/IC795046.png "Egyszeri bejelentkezés beállítása")

5.  A különböző webes böngészőablakban jelentkezzen be a NetDocuments vállalati webhely rendszergazdaként.

6.  Kattintson a **rendszergazda**.

7.  Kattintson a **Hozzáadás és eltávolítás-felhasználók és csoportok**elemre.

    ![Tárházba] (./media/active-directory-saas-netdocuments-tutorial/IC795047.png "Tárházba")

8.  Kattintson a **Konfigurálás speciális hitelesítési beállítások**gombra.

    ![A speciális hitelesítési beállítások konfigurálása] (./media/active-directory-saas-netdocuments-tutorial/IC795048.png "A speciális hitelesítési beállítások konfigurálása")

9.  **A szövetséges identitás** párbeszédpanelen hajtsa végre az alábbi lépéseket:

    ![Összevont Identitty] (./media/active-directory-saas-netdocuments-tutorial/IC795049.png "Összevont Identitty")

    1.  **Külső identitás kiszolgáló típusa**csoportban jelölje ki a **Active Directory összevonási szolgáltatásokat**.
    2.  Kattintson a **fájl kiválasztása**a metaadat-alapú letöltött fájl feltöltése.
    3.  Kattintson az **OK gombra**.

10. Az Azure klasszikus portálon jelölje be a egyszeri bejelentkezéses konfigurációs megerősítés, és válassza a **kész** , **Állítsa be az egyszeri bejelentkezés** párbeszédpanel bezárásához.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-netdocuments-tutorial/IC795050.png "Egyszeri bejelentkezés beállítása")
##<a name="configuring-user-provisioning"></a>Felhasználói kiépítési konfigurálása
  
Ahhoz, hogy az Azure Active Directory-felhasználóknak, hogy jelentkezzen be a NetDocuments, hogy ki kell építenie NetDocuments be.  
NetDocuments, ha a feladat manuális kiépítési.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Felhasználói kiépítési beállításához hajtsa végre az alábbi lépéseket:

1.  Rendszergazdaként folyamata a **NetDocuments** vállalati webhely alkalmazásba című témakör tartalmaz.

2.  A felső sávon kattintson a **rendszergazda**.

    ![Rendszergazda] (./media/active-directory-saas-netdocuments-tutorial/IC795051.png "Rendszergazda")

3.  Kattintson a **Hozzáadás és eltávolítás-felhasználók és csoportok**elemre.

    ![Tárházba] (./media/active-directory-saas-netdocuments-tutorial/IC795047.png "Tárházba")

4.  Az **E-mail cím** mezőbe írja be e-mail címét a érvényes Azure Active Directory-fiókkal szeretné rendelkezni, és válassza a **Felhasználó hozzáadása**.

    ![E-mail cím] (./media/active-directory-saas-netdocuments-tutorial/IC795053.png "E-mail cím")

    >[AZURE.NOTE]Az Azure Active Directory fióktulajdonos előtt aktívvá válik, győződjön meg arról, hogy a fiók mutató hivatkozást tartalmazó e-mailben kapja eredményül.

>[AZURE.NOTE]Bármely más NetDocuments felhasználói fiók létrehozása eszközöket is használhatja, illetve NetDocuments rendelkezést AAD felhasználói fiókok által biztosított API-khoz.

##<a name="assigning-users"></a>Felhasználók hozzárendelése
  
Tesztelje a konfigurációt, meg kell adni az Azure Active Directory-felhasználók, használja az alkalmazás elérési útvonalát társításával engedélyezni szeretné.

###<a name="to-assign-users-to-netdocuments-perform-the-following-steps"></a>Felhasználók hozzárendelése NetDocuments, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon próba-fiók létrehozása.

2.  Kattintson a **NetDocuments **alkalmazás integrációs lap **hozzárendelését a felhasználókhoz**.

    ![Hozzárendelését a felhasználókhoz] (./media/active-directory-saas-netdocuments-tutorial/IC795054.png "Adhatnak a felhasználóknak")

3.  Jelölje ki a próba-felhasználó, kattintson a **hozzárendelni**, és kattintson az **Igen gombra** kattintva erősítse meg a hozzárendelés.

    ![Igen] (./media/active-directory-saas-netdocuments-tutorial/IC767830.png "Igen")
  
Ha meg szeretné tesztelje a egyszeri bejelentkezéses, Access Panel megnyitásához. Az Access ablaktábla, lásd: [Bevezetés az Access panelre](active-directory-saas-access-panel-introduction.md).