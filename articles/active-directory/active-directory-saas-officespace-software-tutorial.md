<properties 
    pageTitle="Oktatóprogram: Azure Active Directory-integráció a OfficeSpace szoftver |} Microsoft Azure" 
    description="Megtudhatja, hogy miként OfficeSpace szoftver használata az Azure Active Directory ahhoz, hogy az egyszeri bejelentkezés, automatikus kiépítési és az egyéb!" 
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

#<a name="tutorial-azure-active-directory-integration-with-officespace-software"></a>Oktatóprogram: Azure Active Directory-integráció a OfficeSpace szoftver
  
Ebben az oktatóanyagban célja Azure és OfficeSpace szoftver integrációja megjelenítéséhez.  
Az alkalmazási példát, ebben az oktatóanyagban tagolt feltételezi, hogy már rendelkezik az alábbiakat:

-   Érvényes Azure előfizetés
-   Egy OfficeSpace szoftver egyszeri bejelentkezés engedélyezve van az előfizetés
  
Ebben az oktatóanyagban befejeztével az Azure Active Directory felhasználók OfficeSpace szoftver rendelt tudnak egyetlen bejelentkezés az alkalmazásba, a OfficeSpace szoftver vállalati webhely (service provider által kezdeményezett bejelentkezés) és használatáról a [Bevezetés az Access panelen](active-directory-saas-access-panel-introduction.md).
  
A következő építőelemek az alkalmazási példát, ebben az oktatóanyagban tagolt áll:

1.  Az alkalmazás integrálását OfficeSpace szoftverek engedélyezése
2.  Egyszeri bejelentkezés beállítása
3.  Felhasználói kiépítési konfigurálása
4.  Felhasználók hozzárendelése

![Eset] (./media/active-directory-saas-officespace-software-tutorial/IC777764.png "Eset")
##<a name="enabling-the-application-integration-for-officespace-software"></a>Az alkalmazás integrálását OfficeSpace szoftverek engedélyezése
  
Ez a szakasz célja, hogyan OfficeSpace szoftverek alkalmazás alkalmazással való integráció engedélyezése a szerkezet.

###<a name="to-enable-the-application-integration-for-officespace-software-perform-the-following-steps"></a>Ahhoz, hogy az alkalmazás integrálását OfficeSpace szoftverek, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon kattintson a bal oldali navigációs területen kattintson az **Active Directory**.

    ![Az Active Directory] (./media/active-directory-saas-officespace-software-tutorial/IC700993.png "Az Active Directory")

2.  A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3.  Nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson a felső menüben **alkalmazások** .

    ![Alkalmazások] (./media/active-directory-saas-officespace-software-tutorial/IC700994.png "Alkalmazások")

4.  Kattintson a **Hozzáadás** az oldal alján.

    ![Alkalmazás hozzáadása] (./media/active-directory-saas-officespace-software-tutorial/IC749321.png "Alkalmazás hozzáadása")

5.  **Milyen feladatot szeretne tenni** párbeszédpanelen kattintson **a gyűjteményből az alkalmazások felvétele**.

    ![Gallerry az alkalmazás hozzáadása] (./media/active-directory-saas-officespace-software-tutorial/IC749322.png "Gallerry az alkalmazás hozzáadása")

6.  A **Keresés mezőbe**írja be a **Szoftver OfficeSpace**.

    ![Alkalmazás gyűjtemény] (./media/active-directory-saas-officespace-software-tutorial/IC777765.png "Alkalmazás gyűjtemény")

7.  Az eredmény ablaktáblában jelölje ki a **OfficeSpace szoftver**, és válassza a **kész** , az alkalmazás hozzáadása gombra.

    ![OfficeSpace szoftver] (./media/active-directory-saas-officespace-software-tutorial/IC781007.png "OfficeSpace szoftver")
##<a name="configuring-single-sign-on"></a>Egyszeri bejelentkezés beállítása
  
Ez a szakasz célja, hogyan engedélyezheti a felhasználóknak, hogy a saját fiók az összevonási alapján a SAML protokoll használatával Azure Active Directory hitelesítő OfficeSpace szoftver tagolása.  
Egyszeri bejelentkezés OfficeSpace szoftverek beállítása megköveteli ujjlenyomat érték beolvasása egy tanúsítványt.  
Ha nem ismeri a ezt az eljárást, megtudhatja, [hogy miként beolvasni a tanúsítvány ujjlenyomat értékét](http://youtu.be/YKQF266SAxI).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Egyszeri bejelentkezés beállításához hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon **OfficeSpace szoftver** alkalmazás integrációs lapon kattintson **konfigurálása az egyszeri bejelentkezés** a **Konfigurálása az egyszeri bejelentkezés** párbeszédpanel megnyitásához.

    ![Egyszeri bejelentkezés beállítása a =] (./media/active-directory-saas-officespace-software-tutorial/IC777766.png "Egyszeri bejelentkezés beállítása a =")

2.  **Hogyan szeretné, hogy jelentkezzen be az OfficeSpace szoftver felhasználók** lapon jelölje be **A Microsoft Azure Active Directory Single Sign-On**, és kattintson a **Tovább gombra**.

    ![Beállítás az egyszeri bejelentkezés] (./media/active-directory-saas-officespace-software-tutorial/IC777767.png "Beállítás az egyszeri bejelentkezés")

3.  Az **App URL-címe beállítása** lapon az **OfficeSpace szoftver bejelentkezési a URL-cím** mezőbe írja be a OfficeSpace szoftveralkalmazásban bejelentkezni a felhasználók által használt URL-CÍMÉT (például: "*https://company.officespacesoftware.com*"), majd kattintson a **Tovább**gombra.

    ![Állítsa be az App URL-címe] (./media/active-directory-saas-officespace-software-tutorial/IC775556.png "Állítsa be az App URL-címe")

4.  A **Configure egyszeri bejelentkezés OfficeSpace szoftver a** lapon a tanúsítvány letöltése, kattintson a **tanúsítvány letöltése**, és mentse a tanúsítvány fájlt a számítógépen helyben parancsra.

    ![Beállítás az egyszeri bejelentkezés] (./media/active-directory-saas-officespace-software-tutorial/IC793769.png "Beállítás az egyszeri bejelentkezés")

5.  A különböző webes böngészőablakban jelentkezzen be a szoftver OfficeSpace vállalati webhely rendszergazdaként.

6.  Nyissa meg a **felügyeleti \> összekötők**.

    ![Rendszergazda] (./media/active-directory-saas-officespace-software-tutorial/IC777769.png "Rendszergazda")

7.  Kattintson a **SAML engedélyt**.

    ![Összekötők] (./media/active-directory-saas-officespace-software-tutorial/IC777770.png "Összekötők")

8.  A **SAML engedélyezés** csoportban hajtsa végre az alábbi lépéseket:

    ![SAML konfigurálása] (./media/active-directory-saas-officespace-software-tutorial/IC777771.png "SAML konfigurálása")

    1.  Az Azure klasszikus portálon **konfigurálása az egyszeri bejelentkezés a OfficeSpace szoftver** párbeszéd lapon a **Távoli bejelentkezési URL-cím** értéket másolja és illessze be a **Kijelentkezés szolgáltató URL-címe** mezőben lévő értéket.
    2.  Az Azure klasszikus portálon **konfigurálása az egyszeri bejelentkezés a OfficeSpace szoftver** párbeszéd lapon a **Távoli kijelentkezés URL-címe** értéket másolja és illessze be a **ügyfél idp cél URL-címe** mezőben lévő értéket.
    3.  Az exportált tanúsítványt az **ujjlenyomatot** értéket másolja és illessze be az **ügyfél idp tanúsítvány ujjlenyomat** szövegdoboz.  

        >[AZURE.TIP]
        További részletekért megtudhatja, [hogy miként egy tanúsítványt ujjlenyomat érték beolvasása](http://youtu.be/YKQF266SAxI)

    4.  Kattintson a **beállítások mentéséhez**.

9.  Az Azure klasszikus portálon jelölje be a egyszeri bejelentkezéses konfigurációs megerősítés, és válassza a **kész** , **Állítsa be az egyszeri bejelentkezés** párbeszédpanel bezárásához.

    ![Beállítás az egyszeri bejelentkezés] (./media/active-directory-saas-officespace-software-tutorial/IC777772.png "Beállítás az egyszeri bejelentkezés")
##<a name="configuring-user-provisioning"></a>Felhasználói kiépítési konfigurálása
  
Ahhoz, hogy az Azure Active Directory-felhasználóknak, hogy jelentkezzen be a szoftver OfficeSpace, hogy ki kell építenie OfficeSpace szoftver. OfficeSpace szoftver, amíg az automatizált feladat kiépítési.  
Nincs teendő meg nem.  
Felhasználók automatikusan létrejön egy szükség esetén az első egyszeri bejelentkezéses kísérlet során.

>[AZURE.NOTE]Bármely más OfficeSpace szoftver felhasználói fiók létrehozása eszközöket is használhatja, illetve API-hoz megadott OfficeSpace szoftverrel rendelkezést AAD felhasználói fiókok.

##<a name="assigning-users"></a>Felhasználók hozzárendelése
  
Tesztelje a konfigurációt, meg kell adni az Azure Active Directory-felhasználók, használja az alkalmazás elérési útvonalát társításával engedélyezni szeretné.

###<a name="to-assign-users-to-officespace-software-perform-the-following-steps"></a>Felhasználók hozzárendelése OfficeSpace szoftver, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon próba-fiók létrehozása.

2.  A **Szoftver OfficeSpace **alkalmazás integrációs lapon kattintson a **hozzárendelését a felhasználókhoz**.

    ![Adhatnak a felhasználóknak] (./media/active-directory-saas-officespace-software-tutorial/IC777773.png "Adhatnak a felhasználóknak")

3.  Jelölje ki a próba-felhasználó, kattintson a **hozzárendelni**, és kattintson az **Igen gombra** kattintva erősítse meg a hozzárendelés.

    ![Igen] (./media/active-directory-saas-officespace-software-tutorial/IC767830.png "Igen")
  
Ha meg szeretné tesztelje a egyszeri bejelentkezéses, Access Panel megnyitásához. Az Access ablaktábla, lásd: [Bevezetés az Access panelre](active-directory-saas-access-panel-introduction.md).