<properties 
    pageTitle="Oktatóprogram: Azure Active Directory-integráció a Tinfoil biztonsági |} Microsoft Azure"
    description="Megtudhatja, hogyan használhatja a Tinfoil biztonsági az Azure Active Directory ahhoz, hogy az egyszeri bejelentkezés, automatikus kiépítési és az egyéb!." 
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

#<a name="tutorial-azure-active-directory-integration-with-tinfoil-security"></a>Oktatóprogram: Azure Active Directory-integráció a Tinfoil biztonsági
  
Ebben az oktatóanyagban célja integrálása az Azure és Tinfoil biztonsági megjelenítése.  
Az alkalmazási példát, ebben az oktatóanyagban tagolt feltételezi, hogy már rendelkezik az alábbiakat:

-   Érvényes Azure előfizetés
-   Tinfoil biztonsági egyszeri bejelentkezéses engedélyezett előfizetés
  
Ebben az oktatóanyagban befejeztével az Azure Active Directory-felhasználók Tinfoil biztonsági rendelt fogja tudni egyszeri bejelentkezési a Tinfoil biztonsági vállalati webhely (identitás szolgáltató által kezdeményezett bejelentkezéses) vagy használatáról a [Bevezetés az Access panelen](active-directory-saas-access-panel-introduction.md)az alkalmazásba.
  
A következő építőelemek az alkalmazási példát, ebben az oktatóanyagban tagolt áll:

1.  Az alkalmazás integrációját Tinfoil biztonsági engedélyezése
2.  Egyszeri bejelentkezés beállítása
3.  Felhasználói kiépítési konfigurálása
4.  Felhasználók hozzárendelése

![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-tinfoil-security-tutorial/IC798965.png "Egyszeri bejelentkezés beállítása")

##<a name="enabling-the-application-integration-for-tinfoil-security"></a>Az alkalmazás integrációját Tinfoil biztonsági engedélyezése
  
Ez a szakasz célja, hogyan Tinfoil biztonsági az alkalmazás alkalmazással való integráció engedélyezése a szerkezet.

###<a name="to-enable-the-application-integration-for-tinfoil-security-perform-the-following-steps"></a>Ahhoz, hogy az alkalmazás integrációját Tinfoil biztonsági, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon kattintson a bal oldali navigációs területen kattintson az **Active Directory**.

    ![Az Active Directory] (./media/active-directory-saas-tinfoil-security-tutorial/IC700993.png "Az Active Directory")

2.  A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3.  Nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson a felső menüben **alkalmazások** .

    ![Alkalmazások] (./media/active-directory-saas-tinfoil-security-tutorial/IC700994.png "Alkalmazások")

4.  Kattintson a **Hozzáadás** az oldal alján.

    ![Alkalmazás hozzáadása] (./media/active-directory-saas-tinfoil-security-tutorial/IC749321.png "Alkalmazás hozzáadása")

5.  **Milyen feladatot szeretne tenni** párbeszédpanelen kattintson **a gyűjteményből az alkalmazások felvétele**.

    ![Gallerry az alkalmazás hozzáadása] (./media/active-directory-saas-tinfoil-security-tutorial/IC749322.png "Gallerry az alkalmazás hozzáadása")

6.  A **Keresés mezőbe**írja be a **Tinfoil biztonsága**.

    ![Alkalmazás gyűjtemény] (./media/active-directory-saas-tinfoil-security-tutorial/IC798966.png "Alkalmazás gyűjtemény")

7.  Az eredmény ablaktáblában jelölje ki a **Tinfoil biztonsági**pontra, és kattintson a **kész** az alkalmazás hozzáadása parancsra.

    ![Tinfoil biztonsági] (./media/active-directory-saas-tinfoil-security-tutorial/IC802771.png "Tinfoil biztonsági")

##<a name="configuring-single-sign-on"></a>Egyszeri bejelentkezés beállítása
  
Ez a szakasz célja engedélyezése a felhasználóknak, hogy a saját fiók az összevonási alapján a SAML protokoll használatával Azure Active Directory hitelesítő Tinfoil biztonság tagolása.  
Egyszeri bejelentkezés Tinfoil biztonság beállítása megköveteli ujjlenyomat érték beolvasása egy tanúsítványt.  
Ha nem ismeri a ezt az eljárást, megtudhatja, [hogy miként beolvasni a tanúsítvány ujjlenyomat értékét](http://youtu.be/YKQF266SAxI).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Egyszeri bejelentkezés beállításához hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon **Tinfoil biztonsági** alkalmazás integrációs lapon kattintson **konfigurálása az egyszeri bejelentkezés** a **Konfigurálása az egyszeri bejelentkezés** párbeszédpanel megnyitásához.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-tinfoil-security-tutorial/IC798967.png "Egyszeri bejelentkezés beállítása")

2.  **Hogyan szeretné, hogy jelentkezzen be az Tinfoil biztonsági felhasználók** lapon jelölje be **A Microsoft Azure Active Directory Single Sign-On**, és kattintson a **Tovább gombra**.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-tinfoil-security-tutorial/IC798968.png "Egyszeri bejelentkezés beállítása")

3.  Az **Alkalmazás URL-cím beállítása** lapon az **Tinfoil biztonsági válasz URL-cím** mezőbe írja be a Tinfoil biztonsági állítás fogyasztói szolgáltatást (ACS) URL-címet (például: "*https://www.tinfoilsecurity.com/saml/consume*", és kattintson a **Tovább gombra**.

    >[AZURE.NOTE] Látnia kell úgy juthat az ACS URL-cím Tinfoil biztonsági metaadatokat (https://www.tinfoilsecurity.com/saml/metadata).

    ![Állítsa be az App URL-címe] (./media/active-directory-saas-tinfoil-security-tutorial/IC798969.png "Állítsa be az App URL-címe")

4.  **Konfigurálása az egyszeri bejelentkezés Tinfoil biztonsági a** lapon a tanúsítvány letöltése, kattintson a **tanúsítvány letöltése**, és mentse a tanúsítvány fájlként helyileg **c:\\Tinfoil Security.cer**.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-tinfoil-security-tutorial/IC798970.png "Egyszeri bejelentkezés beállítása")

5.  A különböző webes böngészőablakban jelentkezzen be a Tinfoil biztonsági vállalati webhely rendszergazdaként.

6.  Az eszköztár a képernyő tetején kattintson a **Saját fiók**elemre.

    ![Irányítópult] (./media/active-directory-saas-tinfoil-security-tutorial/IC798971.png "Irányítópult")

7.  Kattintson a **Biztonság**gombra.

    ![Biztonsági] (./media/active-directory-saas-tinfoil-security-tutorial/IC798972.png "Biztonsági")

8.  Az **Egyszeri bejelentkezés** beállítása lapon hajtsa végre az alábbi lépéseket:

    ![Egyszeri bejelentkezés] (./media/active-directory-saas-tinfoil-security-tutorial/IC798973.png "Egyszeri bejelentkezés")

    1.  Jelölje ki a **SAML engedélyezése**.
    2.  Kattintson a **manuális konfiguráció**.
    3.  Az Azure klasszikus portálon **konfigurálása az egyszeri bejelentkezés a Tinfoil biztonság** párbeszédpanel lapon a **SAML egyszeri bejelentkezési URL-címe** értéket másolja és illessze be a **SAML bejegyzés URL-címe** mezőben lévő értéket.
    4.  Az exportált tanúsítványt az **ujjlenyomatot** értéket másolja és illessze be a **SAML tanúsítvány ujjlenyomat** mezőben lévő értéket.  

        >[AZURE.TIP] További részletekért megtudhatja, [hogy miként egy tanúsítványt ujjlenyomat érték beolvasása](http://youtu.be/YKQF266SAxI)

    5.  Másolja **az Account ID**.
    6.  Kattintson a **Mentés**gombra.

9.  Az Azure klasszikus portálon jelölje be a egyszeri bejelentkezéses konfigurációs megerősítés, és válassza a **kész** , **Állítsa be az egyszeri bejelentkezés** párbeszédpanel bezárásához.

    ![Egyszeri bejelentkezés beállítása] (./media/active-directory-saas-tinfoil-security-tutorial/IC798974.png "Egyszeri bejelentkezés beállítása")

10. Kattintson a menü felső **attribútumok** a **SAML jogkivonat attribútumok** párbeszédpanel megnyitásához.

    ![Attribútumok] (./media/active-directory-saas-tinfoil-security-tutorial/IC795920.png "Attribútumok")

11. Kötelező attribútum megfeleltetések felvenni, hajtsa végre az alábbi lépéseket:

    ![Attribútumok] (./media/active-directory-saas-tinfoil-security-tutorial/IC798975.png "Attribútumok")

    1.  Kattintson a **felhasználó attribútum hozzáadása**gombra.
    2.  Az **Attribútum neve** mezőbe írja be a **accountid**.
    3.  Az **Attribútumérték** mezőbe illessze be az előző szakaszban másolta fiók azonosító értékét.
    4.  Kattintson a **kész**gombra.

12. Kattintson a **módosítások alkalmazásához**.

##<a name="configuring-user-provisioning"></a>Felhasználói kiépítési konfigurálása
  
Ahhoz, hogy az Azure Active Directory-felhasználóknak, hogy jelentkezzen be a Tinfoil biztonsági, hogy ki kell építenie Tinfoil biztonsági be.  
Tinfoil biztonsági, ha a feladat manuális kiépítési.

###<a name="to-get-a-user-provisioned-perform-the-following-steps"></a>A felhasználó kiépítéstől eléréséhez, hajtsa végre az alábbi lépéseket:

1.  Ha a felhasználó a vállalati fiók része, meg kell a Tinfoil biztonsági támogatási csoport munkatársaitól kaphat a létrehozott felhasználói fiók eléréséhez.

2.  Ha a felhasználó egy normál Tinfoil biztonsági szoftver felhasználója, majd a felhasználó vehet egy collaborator a felhasználó helyek közül. Ez elindítja a megadott e-mail Tinfoil biztonsági új felhasználói fiók létrehozása szóló meghívás küldése egy folyamat.

>[AZURE.NOTE] Bármely más Tinfoil biztonsági felhasználói fiók létrehozása eszközöket is használhatja, vagy biztonsági Tinfoil rendelkezést AAD felhasználói fiókok által biztosított API-khoz.

##<a name="assigning-users"></a>Felhasználók hozzárendelése
  
Tesztelje a konfigurációt, meg kell adni az Azure Active Directory-felhasználók, használja az alkalmazás elérési útvonalát társításával engedélyezni szeretné.

###<a name="to-assign-users-to-tinfoil-security-perform-the-following-steps"></a>Felhasználók hozzárendelése Tinfoil biztonsági, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon próba-fiók létrehozása.

2.  Kattintson a **Tinfoil biztonsági **alkalmazás integrációs lap **hozzárendelését a felhasználókhoz**.

    ![Hozzárendelését a felhasználókhoz] (./media/active-directory-saas-tinfoil-security-tutorial/IC798976.png "Adhatnak a felhasználóknak")

3.  Jelölje ki a próba-felhasználó, kattintson a **hozzárendelni**, és kattintson az **Igen gombra** kattintva erősítse meg a hozzárendelés.

    ![Igen] (./media/active-directory-saas-tinfoil-security-tutorial/IC767830.png "Igen")
  
Ha meg szeretné tesztelje a egyszeri bejelentkezéses, Access Panel megnyitásához. Az Access ablaktábla, lásd: [Bevezetés az Access panelre](active-directory-saas-access-panel-introduction.md).