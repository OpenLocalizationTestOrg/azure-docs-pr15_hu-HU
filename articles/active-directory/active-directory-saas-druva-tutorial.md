<properties 
    pageTitle="Oktatóprogram: Azure Active Directory integrációs integrációja Druva |} Microsoft Azure" 
    description="Megtudhatja, hogyan használhatja a Druva az Azure Active Directory ahhoz, hogy az egyszeri bejelentkezés, automatikus kiépítési és az egyéb!" 
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

#<a name="tutorial-azure-active-directory-integration-integration-with-druva"></a>Oktatóprogram: Azure Active Directory integrációs Druva integrációja

Ebben az oktatóanyagban célja integrálása az Azure és Druva megjelenítése.  
Az alkalmazási példát, ebben az oktatóanyagban tagolt feltételezi, hogy már rendelkezik az alábbiakat:

-   Érvényes Azure előfizetés
-   Druva egyszeri bejelentkezéses engedélyezett előfizetés

Ebben az oktatóanyagban befejeztével az Azure Active Directory-felhasználók Druva rendelt fogja tudni az alkalmazás a Druva vállalati webhely (service provider által kezdeményezett bejelentkezés) és használatáról a [Bevezetés az Access panelen](active-directory-saas-access-panel-introduction.md)az egyszeri bejelentkezési.

A következő építőelemek az alkalmazási példát, ebben az oktatóanyagban tagolt áll:

1.  Az alkalmazás integrációját Druva engedélyezése
2.  Egyszeri bejelentkezés beállítása
3.  Felhasználói kiépítési konfigurálása
4.  Felhasználók hozzárendelése

![Eset] (./media/active-directory-saas-druva-tutorial/IC795084.png "Eset")
##<a name="enabling-the-application-integration-for-druva"></a>Az alkalmazás integrációját Druva engedélyezése

Ez a szakasz célja, hogyan Druva az alkalmazás alkalmazással való integráció engedélyezése a szerkezet.

###<a name="to-enable-the-application-integration-for-druva-perform-the-following-steps"></a>Ahhoz, hogy az alkalmazás integrációját Druva, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon kattintson a bal oldali navigációs területen kattintson az **Active Directory**.

    ![Az Active Directory] (./media/active-directory-saas-druva-tutorial/IC700993.png "Az Active Directory")

2.  A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3.  Nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson a felső menüben **alkalmazásokat** .

    ![Alkalmazások] (./media/active-directory-saas-druva-tutorial/IC700994.png "Alkalmazások")

4.  Kattintson a **Hozzáadás** az oldal alján.

    ![Alkalmazás hozzáadása] (./media/active-directory-saas-druva-tutorial/IC749321.png "Alkalmazás hozzáadása")

5.  **Milyen feladatot szeretne tenni** párbeszédpanelen kattintson **a gyűjteményből az alkalmazások felvétele**.

    ![Gallerry az alkalmazás hozzáadása] (./media/active-directory-saas-druva-tutorial/IC749322.png "Gallerry az alkalmazás hozzáadása")

6.  A **Keresés mezőbe**írja be a **Druva**.

    ![Alkalmazás gyűjtemény] (./media/active-directory-saas-druva-tutorial/IC795085.png "Alkalmazás gyűjtemény")

7.  Az eredmény ablaktáblában jelölje ki a **Druva**, és válassza a **kész** , az alkalmazás hozzáadása gombra.

    ![Druva] (./media/active-directory-saas-druva-tutorial/IC795086.png "Druva")
##<a name="configuring-single-sign-on"></a>Egyszeri bejelentkezés beállítása

Ez a szakasz célja tagolása engedélyezése a felhasználóknak, hogy Druva hitelesíteni a fiókkal az összevonási alapján a SAML protokoll használatával Azure AD.  
Ez az eljárás részeként biztosan szükséges számrendszerben-64 kódolt tanúsítvány fájlt.  
Ha nem ismeri a ezt az eljárást, olvassa el a [szövegfájlba bináris tanúsítvány konvertálása](http://youtu.be/PlgrzUZ-Y1o)című témakört.

A Druva alkalmazása egy bizonyos formátumban, amelyhez egyéni attribútum megfeleltetések hozzáadása a **saml jogkivonat attribútumok** konfigurációs várja meg a SAML előfeltételek.  
Az alábbi képernyőképen látható, a.

![SAML jogkivonat attribútumok] (./media/active-directory-saas-druva-tutorial/IC795087.png "SAML jogkivonat attribútumok")

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Egyszeri bejelentkezés beállításához hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon **Druva** alkalmazás integrációs lapon kattintson **konfigurálása az egyszeri bejelentkezés** a **Konfigurálása az egyszeri bejelentkezés** párbeszédpanel megnyitásához.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-druva-tutorial/IC795027.png "Egyszeri bejelentkezés beállítása")

2.  **Hogyan szeretné, hogy jelentkezzen be az Druva felhasználók** lapon jelölje be **A Microsoft Azure Active Directory Single Sign-On**, és kattintson a **Tovább gombra**.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-druva-tutorial/IC795088.png "Egyszeri bejelentkezés beállítása")

3.  Az **Alkalmazás URL-cím beállítása** lapon az **URL a bejelentkezési Druva** mezőbe írja be a bejelentkezni a Druva alkalmazás a felhasználók által használt URL-CÍMÉT (például: "*https://cloud.druva.com/home/*"), és kattintson a **Tovább gombra**.

    ![Állítsa be az App URL-címe] (./media/active-directory-saas-druva-tutorial/IC795089.png "Állítsa be az App URL-címe")

4.  A **Configure egyszeri bejelentkezés Druva a** lapon a tanúsítvány letöltése, kattintson a **tanúsítvány letöltése**, és mentse a tanúsítvány fájlt a számítógépen helyben parancsra.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-druva-tutorial/IC795090.png "Egyszeri bejelentkezés beállítása")

5.  A különböző webes böngészőablakban jelentkezzen be a Druva vállalati webhely rendszergazdaként.

6.  Nyissa meg a **kezelése \> beállítások**.

    ![Beállítások] (./media/active-directory-saas-druva-tutorial/IC795091.png "Beállítások")

7.  Az egyszeri bejelentkezéses beállításai párbeszédpanelen hajtsa végre az alábbi lépéseket:

    ![Egy bejelentkezés beállításai] (./media/active-directory-saas-druva-tutorial/IC795092.png "Egy bejelentkezés beállításai")

    1.  Az Azure klasszikus portálon **konfigurálása az egyszeri bejelentkezés a Druva** párbeszédpanel lapon a **Távoli bejelentkezési URL-cím** értéket másolja és illessze be a **Azonosító szolgáltató bejelentkezési URL-cím** mezőben lévő értéket.
    2.  Az Azure klasszikus portálon **konfigurálása az egyszeri bejelentkezés a Druva** párbeszédpanel lapon a **Távoli kijelentkezés URL-címe** értéket másolja és illessze be a **Azonosító szolgáltató kijelentkezés URL-címe** mezőben lévő értéket.
    3.  A letöltött tanúsítvány **Alap-64-címként kódolt** fájl létrehozása  

        >[AZURE.TIP] További információra kíváncsi olvassa el a [bináris tanúsítvány szövegfájlba konvertálása](http://youtu.be/PlgrzUZ-Y1o) című témakört.

    4.  Nyissa meg az alap-64 kódolt tanúsítvány a Jegyzettömbben, azt tartalmát a vágólapra másolja és illessze be a **Azonosító szolgáltató tanúsítvány** mezőben lévő értéket szeretné
    5.  A **Beállítások** lap megnyitásához kattintson a **Mentés**gombra.

8.  A **Beállítások** lapon kattintson a **SSO jogkivonat készítése**.

    ![Beállítások] (./media/active-directory-saas-druva-tutorial/IC795093.png "Beállítások")

9.  Az **Egyszeri bejelentkezéses hitelesítési jogkivonat** párbeszédpanelen hajtsa végre az alábbi lépéseket:

    ![Egyszeri bejelentkezés jogkivonat] (./media/active-directory-saas-druva-tutorial/IC795094.png "Egyszeri bejelentkezés jogkivonat")

    1.  Kattintson a **Másolás**parancsra.
    2.  Kattintson a **Bezárás**gombra.

10. Az Azure klasszikus portálon jelölje be a egyszeri bejelentkezéses konfigurációs megerősítés, és válassza a **kész** , **Állítsa be az egyszeri bejelentkezés** párbeszédpanel bezárásához.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-druva-tutorial/IC795095.png "Egyszeri bejelentkezés beállítása")

11. Kattintson a menü felső **attribútumok** a **SAML jogkivonat attribútumok** párbeszédpanel megnyitásához.

    ![Attribútumok] (./media/active-directory-saas-druva-tutorial/IC795096.png "Attribútumok")

12. Kötelező attribútum megfeleltetések felvenni, hajtsa végre az alábbi lépéseket:

  	|Attribútumnév|Attribútumérték|
  	|---|---|
  	|insync\_auth\_jogkivonat|<*a vágólap érték*>|

    1.  Az adatok tábla minden sorához a fenti kattintson a **felhasználó attribútum hozzáadása**gombra.
    2.  Az **Attribútum neve** mezőbe írja be a attribútumnév meg abban a sorban látható.
    3.  Az **Attribútumérték** mezőbe írja be az attribútumérték meg abban a sorban látható.
    4.  Kattintson a **kész**gombra.

13. Kattintson a **módosítások alkalmazásához**.
##<a name="configuring-user-provisioning"></a>Felhasználói kiépítési konfigurálása

Ahhoz, hogy az Azure Active Directory-felhasználóknak, hogy jelentkezzen be a Druva, hogy ki kell építenie Druva be.  
Druva, ha a feladat manuális kiépítési.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Felhasználói kiépítési beállításához hajtsa végre az alábbi lépéseket:

1.  Jelentkezzen be rendszergazdaként a **Druva** vállalati webhely.

2.  Nyissa meg a **kezelése \> felhasználók**.

    ![Felhasználókezelés] (./media/active-directory-saas-druva-tutorial/IC795097.png "Felhasználókezelés")

3.  Kattintson az **Új**gombra.

    ![Felhasználókezelés] (./media/active-directory-saas-druva-tutorial/IC795098.png "Felhasználókezelés")

4.  Az új felhasználó létrehozása párbeszédpanelen hajtsa végre az alábbi lépéseket:

    ![Új_felhasználó létrehozása] (./media/active-directory-saas-druva-tutorial/IC795099.png "Új_felhasználó létrehozása")

    1.  Írja be az e-mail címet, és azt szeretné, hogy be a kapcsolódó szövegdobozok kiépítése érvényes Azure Active Directory felhasználói fiók nevét.
    2.  Kattintson a **felhasználó létrehozása**gombra.

>[AZURE.NOTE] Bármely más Druva felhasználói fiók létrehozása eszközöket is használhatja, illetve Druva rendelkezést AAD felhasználói fiókok által biztosított API-khoz.

##<a name="assigning-users"></a>Felhasználók hozzárendelése

Tesztelje a konfigurációt, meg kell adni az Azure Active Directory-felhasználók, használja az alkalmazás elérési útvonalát társításával engedélyezni szeretné.

###<a name="to-assign-users-to-druva-perform-the-following-steps"></a>Felhasználók hozzárendelése Druva, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon próba-fiók létrehozása.

2.  Kattintson a **Druva **alkalmazás integrációs lap **hozzárendelését a felhasználókhoz**.

    ![Adhatnak a felhasználóknak] (./media/active-directory-saas-druva-tutorial/IC795100.png "Adhatnak a felhasználóknak")

3.  Jelölje ki a próba-felhasználó, kattintson a **hozzárendelni**, és kattintson az **Igen gombra** kattintva erősítse meg a hozzárendelés.

    ![Igen] (./media/active-directory-saas-druva-tutorial/IC767830.png "Igen")

Ha meg szeretné tesztelje a egyszeri bejelentkezéses, Access Panel megnyitásához. Ha többet szeretne tudni az Access panelen című témakörben [az Access-panelen](active-directory-saas-access-panel-introduction.md).
