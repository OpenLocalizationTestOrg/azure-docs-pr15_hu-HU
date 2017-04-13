<properties 
    pageTitle="Oktatóprogram: Azure Active Directory-integráció a mező |} Microsoft Azure" 
    description="Megtudhatja, hogyan használhatja a mezőbe az Azure Active Directory ahhoz, hogy az egyszeri bejelentkezés, automatikus kiépítési és az egyéb!" 
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




#<a name="tutorial-azure-active-directory-integration-with-box"></a>Oktatóprogram: Azure Active Directory-integráció a mezőben


  
Ebben az oktatóanyagban célja integrációja Azure és a párbeszédpanel megjelenítése.  
Az alkalmazási példát, ebben az oktatóanyagban tagolt feltételezi, hogy már rendelkezik az alábbiakat:

-   Érvényes Azure előfizetés
-   A próba bérlő egy mezőben
  
Ebben az oktatóanyagban befejeztével az Azure Active Directory felhasználók mezőben rendelt tudnak való egyszeri bejelentkezési a mezőben vállalati webhely (a szolgáltatás által kezdeményezett szolgáltató bejelentkezési) vagy használatáról a [Bevezetés az Access panelen](active-directory-saas-access-panel-introduction.md)az alkalmazásba.
  
A következő építőelemek az alkalmazási példát, ebben az oktatóanyagban tagolt áll:

1.  Az alkalmazás integrációját mezőben engedélyezése
2.  Egyszeri bejelentkezés beállítása
3.  Felhasználói és a csoport kiépítési konfigurálása
4.  Felhasználók hozzárendelése

![Eset] (./media/active-directory-saas-box-tutorial/IC769537.png "Eset")



##<a name="enabling-the-application-integration-for-box"></a>Az alkalmazás integrációját mezőben engedélyezése
  
Ez a szakasz célja, hogyan mezőbe az alkalmazás alkalmazással való integráció engedélyezése a szerkezet.

###<a name="to-enable-the-application-integration-for-box-perform-the-following-steps"></a>Ahhoz, hogy az alkalmazás integráció a jelölőnégyzetet, végezze el az alábbi lépéseket:

1.  Az Azure klasszikus portálon kattintson a bal oldali navigációs területen kattintson az **Active Directory**.

    ![Az Active Directory] (./media/active-directory-saas-box-tutorial/IC700993.png "Az Active Directory")

2.  A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3.  Nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson a felső menüben **alkalmazásokat** .

    ![Alkalmazások] (./media/active-directory-saas-box-tutorial/IC700994.png "Alkalmazások")

4.  Kattintson a **Hozzáadás** az oldal alján.

    ![Alkalmazás hozzáadása] (./media/active-directory-saas-box-tutorial/IC749321.png "Alkalmazás hozzáadása")

5.  **Milyen feladatot szeretne tenni** párbeszédpanelen kattintson **a gyűjteményből az alkalmazások felvétele**.

    ![Gallerry az alkalmazás hozzáadása] (./media/active-directory-saas-box-tutorial/IC749322.png "Gallerry az alkalmazás hozzáadása")

6.  A **Keresés mezőbe**írja be a **jelölőnégyzetet**.

    ![Alkalmazás gyűjtemény] (./media/active-directory-saas-box-tutorial/IC701023.png "Alkalmazás gyűjtemény")

7.  Az eredmény ablaktáblában jelölje **be**, és válassza a **kész** , az alkalmazás hozzáadása parancsra.

    ![Mező] (./media/active-directory-saas-box-tutorial/IC701024.png "Mező")



##<a name="configuring-single-sign-on"></a>Egyszeri bejelentkezés beállítása
  
Ez a szakasz célja tagolása engedélyezése a felhasználóknak, hogy a saját fiók az összevonási alapján a SAML protokoll használatával Azure Active Directory hitelesítő mezőre. Ez az eljárás részeként szükségesek feltöltése a metaadat-alapú Box.com.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Egyszeri bejelentkezés beállításához hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon **mezőben** alkalmazás integrációs lapon kattintson **konfigurálása az egyszeri bejelentkezés** a **Konfigurálása az egyszeri bejelentkezés** párbeszédpanel megnyitásához.

    ![Beállítás az egyszeri bejelentkezés] (./media/active-directory-saas-box-tutorial/IC769538.png "Beállítás az egyszeri bejelentkezés")

2.  **Hogyan szeretné, hogy jelentkezzen be jelölőnégyzetet a felhasználók** lapon jelölje be **A Microsoft Azure Active Directory Single Sign-On**, és kattintson a **Tovább gombra**.

    ![Beállítás az egyszeri bejelentkezés] (./media/active-directory-saas-box-tutorial/IC769539.png "Beállítás az egyszeri bejelentkezés")

3.  **App URL-címe beállítása** lapon az **Be bérlői webhely URL-cím** mezőbe írja be a mezőben bérlői webhely URL-címet (például: https://<mydomainname>. box.com), majd kattintson a **Tovább**gombra.

    ![Állítsa be az app URL-címe] (./media/active-directory-saas-box-tutorial/IC669826.png "Állítsa be az app URL-címe")

4.  A lapon **konfigurálása egyszeri bejelentkezés a mezőbe** a metaadat-alapú letöltéséhez kattintson **metaadat-alapú letöltése**, majd kattintson az adatfájlt a számítógépen helyben.

    ![Beállítás az egyszeri bejelentkezés] (./media/active-directory-saas-box-tutorial/IC669824.png "Beállítás az egyszeri bejelentkezés")

5.  Előre a metaadat-alapú fájl mezőben támogatási csoporthoz. A támogatási csapat igényeinek beállítja az egyszeri bejelentkezés meg.

6.  Jelölje ki az egyszeri bejelentkezéses konfigurációs megerősítő, és kattintson a **kész** , **Állítsa be az egyszeri bejelentkezés** párbeszédpanel bezárásához.

    ![Beállítás az egyszeri bejelentkezés] (./media/active-directory-saas-box-tutorial/IC769540.png "Beállítás az egyszeri bejelentkezés")
##<a name="configuring-user-provisioning"></a>Felhasználói kiépítési konfigurálása
  
Ez a szakasz célja, hogyan engedélyezhető a mezőbe az Active Directory-felhasználói fiókok kiépítésének tagolása.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Egyszeri bejelentkezés beállításához hajtsa végre az alábbi lépéseket:

1. Az Azure klasszikus portálon **mezőben** alkalmazás integrációs lapon kattintson a **konfigurálása felhasználói kiépítési** a **Felhasználó kiépítési beállítása** párbeszédpanel megnyitásához. 

    ![Az automatikus felhasználói kiépítési engedélyezése] (./media/active-directory-saas-box-tutorial/IC769541.png "Az automatikus felhasználói kiépítési engedélyezése")

2. A **felhasználó mezőre kiépítési engedélyezése** párbeszédpanel lapján kattintson **engedélyezése a felhasználók kiépítési**. 

    ![Az automatikus felhasználói kiépítési engedélyezése] (./media/active-directory-saas-box-tutorial/IC769544.png "Az automatikus felhasználói kiépítési engedélyezése")

3. A **mezőben adjon hozzáférést a bejelentkezés** lapon adja meg a szükséges hitelesítő adatokat, és kattintson az **Engedélyezés**gombra. 

    ![Az automatikus felhasználói kiépítési engedélyezése] (./media/active-directory-saas-box-tutorial/IC769546.png "Az automatikus felhasználói kiépítési engedélyezése")


4. Kattintson a **mezőre a hozzáférés** engedélyezése Ezt a műveletet, és térjen vissza az Azure klasszikus portálon. 

    ![Az automatikus felhasználói kiépítési engedélyezése] (./media/active-directory-saas-box-tutorial/IC769549.png "Az automatikus felhasználói kiépítési engedélyezése")


5. A **Kiépítési beállításai** lapon a **Objektumtípusok kiépítése** jelölőnégyzetek teszi függetlenül attól, objektumok csoportosítása kívül felhasználói objektumok kiépített mezőben jelölje ki.  "Hozzárendelése felhasználók és csoportok szakaszban" alatti további információt talál.


6. A konfiguráció befejezéséhez kattintson a Kész gombra. 

    ![Az automatikus felhasználói kiépítési engedélyezése] (./media/active-directory-saas-box-tutorial/IC769551.png "Az automatikus felhasználói kiépítési engedélyezése")



##<a name="assigning-a-test-user"></a>A próba-felhasználói hozzárendelése
  
Tesztelje a konfigurációt, meg kell adni az Azure Active Directory-felhasználók, használja az alkalmazás elérési útvonalát társításával engedélyezni szeretné.

###<a name="to-assign-users-to-box-perform-the-following-steps"></a>Felhasználók hozzárendelése mezőben, hajtsa végre az alábbi lépéseket:

1. Az Azure klasszikus portálon próba-fiók létrehozása.

2. A **mezőben **alkalmazás integrációs lapon kattintson a **hozzárendelését a felhasználókhoz**. 

    ![Adhatnak a felhasználóknak] (./media/active-directory-saas-box-tutorial/IC769552.png "Adhatnak a felhasználóknak")

3.  Jelölje ki a próba-felhasználó, kattintson a **hozzárendelni**, és kattintson az **Igen gombra** kattintva erősítse meg a hozzárendelés. 

    ![Igen] (./media/active-directory-saas-box-tutorial/IC767830.png "Igen")
  
Érdemes most megvárja, amíg 10 perc, és ellenőrizze, hogy a fiók lett szinkronizálva mezőre.

Első lépésként ellenőrzése jelölje be a kiépítési állapotát, a D az oldalon mezőben alkalmazás integrálása az Azure klasszikus portálon az irányítópult gombra kattintva.

![Irányítópult] (./media/active-directory-saas-box-tutorial/IC769553.png "Irányítópult")

Ciklikus kiépítési sikeresen befejeződött felhasználó egy kapcsolódó állapotát jelzi:

![Integráció állapotát] (./media/active-directory-saas-box-tutorial/IC769555.png "Integráció állapotát")


A párbeszédpanel-bérlő szinkronizált felhasználók csoportban **A felhasználók kezelése** a **Felügyeleti konzolban**szerepelnek.

![Integráció állapotát] (./media/active-directory-saas-box-tutorial/IC769556.png "Integráció állapotát")


##<a name="assigning-users-and-groups"></a>Felhasználók és csoportok hozzárendelése

A **mezőben > felhasználók és csoportok** az Azure klasszikus portálon lapon adhatja meg, megadhatja, hogy mely felhasználók és csoportok kell hozzáférési jogosultsággal mezőre. A felhasználó vagy csoport hozzárendelés okoz fennáll a következő műveleteket:

* Azure Active Directory lehetővé teszi, a kijelölt felhasználó (akár közvetlen hozzárendelés vagy a csoporttagság) mező hitelesítést végezni. Ha egy felhasználó nem van hozzárendelve, Azure Active Directory nem engedélyezi őket a jelentkezzen be a jelölőnégyzetet, és hibát ad vissza, a bejelentkezési lapon Azure AD.

* A felhasználó [alkalmazásindító](active-directory-appssoaccess-whatis.md#deploying-azure-ad-integrated-applications-to-users)megjelenik egy alkalmazáscsempére mezőbe.

* Ha az automatikus kiépítési engedélyezve van, majd a hozzárendelt felhasználók és csoportok hozzáadódnak a kiépítési várakozási sorban található automatikusan kell építenie.

    * Ha csak a felhasználó-objektumok nem állított be kell építenie, majd kerül közvetlenül hozzárendelt összes felhasználó a kiépítési várakozási sorban található, és a kiépítési várakozási sorban található összes felhasználóra, amelyek bármilyen tevékenységhez rendelt csoportok tagjai kerülnek. 
    
    * Ha objektumok csoportosítása nem állított be kell építenie, majd az összes hozzárendelt objektumok kiépített mezőben, valamint az összes felhasználó, amelyek ezekhez a csoportokhoz tagjai az. A csoportok és felhasználók tagságot frissítéskor mezőbe írt megőrződnek.
    
Használhatja a **attribútumok > egyszeri bejelentkezés** fülre, és milyen felhasználói attribútumok (vagy jogcímeken) mutatnak be mezőben során SAML-alapú hitelesítés konfigurálása és a **attribútumok > létesítése** fülre, és állítsa be, hogyan felhasználók és csoportok attribútumok flow az Azure Active Directory mezőre a kiépítési művelet során. További információt az alábbi forrásokban talál.


## <a name="additional-resources"></a>További források

* [A SAML jogkivonat kiadott jogcímeken testreszabása](active-directory-saml-claims-customization.md)
* [Kiépítési: Attribútum hozzárendelések testreszabása](active-directory-saas-customizing-attribute-mappings.md)
* [A szoftver alkalmazások integrálása az Azure Active Directory oktatóanyagok listája](active-directory-saas-tutorial-list.md)
* [Mi az access alkalmazás és az egyszeri bejelentkezés az Azure Active Directory címtárral?](active-directory-appssoaccess-whatis.md)
