<properties 
    pageTitle="Oktatóprogram: Azure Active Directory-integráció a Concur |} Microsoft Azure" 
    description="Megtudhatja, hogyan használhatja a Concur az Azure Active Directory ahhoz, hogy az egyszeri bejelentkezés, automatikus kiépítési és az egyéb!" 
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

#<a name="tutorial-azure-active-directory-integration-with-concur"></a>Oktatóprogram: Azure Active Directory-integráció a Concur  


Ebben az oktatóanyagban célja integrálása az Azure és Concur megjelenítése.  
Az alkalmazási példát, ebben az oktatóanyagban tagolt feltételezi, hogy már rendelkezik az alábbiakat:

-   Érvényes Azure előfizetés
-   A Concur bérlői webhelyre

A következő építőelemek az alkalmazási példát, ebben az oktatóanyagban tagolt áll:

1.  Az alkalmazás integrációját Concur engedélyezése
2.  Egyszeri bejelentkezés beállítása
3.  Felhasználói kiépítési konfigurálása
4.  Felhasználók hozzárendelése

![Eset] (./media/active-directory-saas-concur-tutorial/IC769766.png "Eset")

>[AZURE.NOTE] A SAML keresztül szövetséges SSO Concur előfizetése beállítás egy külön feladat elvégzéséhez Concur kapcsolatba kell lépnie.

##<a name="enabling-the-application-integration-for-concur"></a>Az alkalmazás integrációját Concur engedélyezése

Ez a szakasz célja, hogyan Concur az alkalmazás alkalmazással való integráció engedélyezése a szerkezet.

###<a name="to-enable-the-application-integration-for-concur-perform-the-following-steps"></a>Ahhoz, hogy az alkalmazás integrációját Concur, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon kattintson a bal oldali navigációs területen kattintson az **Active Directory**.

    ![Az Active Directory] (./media/active-directory-saas-concur-tutorial/IC700993.png "Az Active Directory")

2.  A **címtár** listában válassza a címtár-integrációs engedélyezni szeretné a címtárban keres, amelyek.

3.  Nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson a felső menüben **alkalmazásokat** .

    ![Alkalmazások] (./media/active-directory-saas-concur-tutorial/IC700994.png "Alkalmazások")

4.  Az **Alkalmazás gyűjtemény**megnyitásához kattintson **Az alkalmazás hozzáadása**, és kattintson a **Hozzáadás az alkalmazások használata a szervezet számára**.

    ![Milyen feladatot szeretne tenni?] (./media/active-directory-saas-concur-tutorial/IC700995.png "Milyen feladatot szeretne tenni?")

5.  A **Keresés mezőbe**írja be a **Concur**.

    ![Alkalmazás gyűjtemény] (./media/active-directory-saas-concur-tutorial/IC721727.png "Alkalmazás gyűjtemény")

6.  Az eredmény ablaktáblában jelölje ki a **Concur**, és válassza a **kész** , az alkalmazás hozzáadása gombra.

    ![Hajózásához] (./media/active-directory-saas-concur-tutorial/IC721728.png "Hajózásához")
##<a name="configuring-single-sign-on"></a>Egyszeri bejelentkezés beállítása

Ez a szakasz célja tagolása engedélyezése a felhasználóknak, hogy Concur hitelesíteni a fiókkal az összevonási alapján a SAML protokoll használatával Azure AD.

>[AZURE.NOTE] A SAML keresztül szövetséges SSO Concur előfizetése beállítás egy külön feladat elvégzéséhez Concur kapcsolatba kell lépnie.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Egyszeri bejelentkezés beállításához hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon **Concur **alkalmazás integrációs lapon kattintson **konfigurálása az egyszeri bejelentkezés** a **Konfigurálása az egyszeri bejelentkezés** párbeszédpanel megnyitásához.

    ![Beállítás az egyszeri bejelentkezés] (./media/active-directory-saas-concur-tutorial/IC769767.png "Beállítás az egyszeri bejelentkezés")

2.  **Hogyan szeretné, hogy jelentkezzen be az Concur felhasználók** lapon jelölje be **A Microsoft Azure Active Directory Single Sign-On**, és kattintson a **Tovább gombra**.

    ![Beállítás az egyszeri bejelentkezés] (./media/active-directory-saas-concur-tutorial/IC769768.png "Beállítás az egyszeri bejelentkezés")

3.  A **App URL-címe beállítása** lapon az **Hajózásához bejelentkezési az URL-cím** mezőbe írja be a concur bérlői bejelentkezési URL-CÍMÉT, és kattintson a **Tovább gombra**: 

    ![Jelentkezzen be az URL-CÍMÉT a konfigurálása] (./media/active-directory-saas-concur-tutorial/IC769769.png "Jelentkezzen be az URL-CÍMÉT a konfigurálása")

4.  A **Configure egyszeri bejelentkezés Concur a** lapon hajtsa végre az alábbi lépéseket.

    ![Jelentkezzen be az URL-CÍMÉT a konfigurálása] (./media/active-directory-saas-concur-tutorial/IC769770.png "Jelentkezzen be az URL-CÍMÉT a konfigurálása")

    1.  Kattintson a metaadat-alapú, és az adatfájl majd biztonságos töltse le a számítógépére.
    2.  Az egyszeri bejelentkezés beállítása a bérlő Concur támogatási csoport munkatársaitól kaphat.
    3.  Jelölje ki az egyszeri bejelentkezéses konfigurációs megerősítő, és kattintson a **kész** , **Állítsa be az egyszeri bejelentkezés** párbeszédpanel bezárásához.  

    >[AZURE.NOTE] A SAML keresztül szövetséges SSO Concur előfizetése beállítás egy külön feladat elvégzéséhez Concur kapcsolatba kell lépnie.

##<a name="configuring-user-provisioning"></a>Felhasználói kiépítési konfigurálása

Ez a szakasz célja, hogyan engedélyezhető az Active Directory felhasználói fiókok Concur kiépítési tagolása.

Ahhoz, hogy a költség szolgáltatásban van alkalmazások nem lehet a megfelelő beállítása és használata a webes szolgáltatás-rendszergazda profil. A WS rendszergazdai szerep egyszerűen ne legyen a meglévő, T & E felügyeleti függvények által használt rendszergazdai profiljára.

Tanácsadók hajózásához vagy az ügyfél rendszergazda a distinct webes szolgáltatás rendszergazdája-profilt kell létrehozniuk, és az ügyfél-rendszergazdának kell használnia a profil a Web Services rendszergazdája függvények (például engedélyezése alkalmazások). Ezek a profilok profilból az ügyfél rendszergazda napi T & E rendszergazdai (a T & E rendszergazdai profil nem kell a WSAdmin szerepkört) külön kell tartani.

A profil használható az alkalmazás engedélyezése létrehozásakor írja be a felhasználói profilok mezőinek a az ügyfél rendszergazda nevét. A tulajdonjog rendelhet a profilt. A profil létrehozása után az ügyfél gombra a "*engedélyezése*" belül a webes szolgáltatások menü egy Partner alkalmazás profil be kell jelentkeznie.

Az alábbi okok miatt ez a művelet nem azzal a profillal, azok a normál T & E felügyeleti használni kell végezni.

1.  Az ügyfél azt kell azt, amely a "*Igen*" kattint alkalmazás engedélyezése után megjelenő párbeszéd ablakban. A kattintás nyugtázza az ügyfél nem hajlandó a Partner alkalmazás adataikat, eléréséhez, így Ön vagy a Partner nem kattintson az Igen gombot.
2.  Ha engedélyezve van az alkalmazás egy ügyfél rendszergazda profil használata a T & E rendszergazdai elhagyja a vállalat (így a profil inaktiválni alatt), minden olyan alkalmazások használatával, hogy profil nem működik mindaddig, amíg az app engedélyezve van egy másik aktív WS felügyeleti profillal engedélyezett. Ez az az oka annak, hogy meg kellene különböző WS felügyeleti profilok létrehozása.
3.  A rendszergazda elhagyja a vállalatot, ha WS Admin profiljához tartozó név megváltoztathatja a csere rendszergazda tetszés érintő nélkül az engedélyezett alkalmazást, mert nem szükséges, hogy a profil inaktiválni

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Felhasználói kiépítési beállításához hajtsa végre az alábbi lépéseket:

1.  Az **Concur** bérlői webhelyen bejelentkezve.

2.  A **felügyeleti** menüből válassza a **Webes szolgáltatások**.

    ![Concur bérlői] (./media/active-directory-saas-concur-tutorial/IC721729.png "Concur bérlői")

3.  A bal oldalon a **Webes szolgáltatások** ablakból jelölje ki a **Partner alkalmazást engedélyezése**elemet.

    ![Partner alkalmazás engedélyezése] (./media/active-directory-saas-concur-tutorial/IC721730.png "Partner alkalmazás engedélyezése")

4.  Az **Alkalmazás engedélyezése** listában válassza az **Azure Active Directory**, és kattintson a **engedélyezése**gombra.

    ![Microsoft Azure Active Directory] (./media/active-directory-saas-concur-tutorial/IC721731.png "Microsoft Azure Active Directory")

5.  Kattintson az **Igen gombra** kattintva zárja be a **Művelet megerősítése** párbeszédpanelen.

    ![Erősítse meg műveletet] (./media/active-directory-saas-concur-tutorial/IC721732.png "Erősítse meg műveletet")

6.  Az Azure klasszikus portálon jelölje ki az alkalmazások listában nyissa meg a **Concur** párbeszédpanel lapot **Concur** .

7.  Nyissa meg a **Felhasználó kiépítési beállítása** párbeszédpanel lapot, kattintson a **konfigurálás felhasználói kiépítési**.

8.  Adja meg a felhasználónevet és a Concur rendszergazdai jelszavát, és kattintson a **Tovább**gombra.

9.  A konfiguráció a **Megerősítés** lapon befejezéséhez kattintson a **kész** gombra.

Most már létrehozhat fiók egy tesztfiók, várja meg a 10 perc, és ellenőrizze, hogy a fiók Concur lett-e szinkronizálva.
##<a name="assigning-users"></a>Felhasználók hozzárendelése

Tesztelje a konfigurációt, meg kell adni az Azure Active Directory-felhasználók, használja az alkalmazás elérési útvonalát társításával engedélyezni szeretné.

###<a name="to-assign-users-to-concur-perform-the-following-steps"></a>Felhasználók hozzárendelése Concur, hajtsa végre az alábbi lépéseket:

1.  Az Azure klasszikus portálon próba-fiók létrehozása.

2.  Kattintson a **Concur **alkalmazás integrációs lap **hozzárendelését a felhasználókhoz**.

    ![Adhatnak a felhasználóknak] (./media/active-directory-saas-concur-tutorial/IC769771.png "Adhatnak a felhasználóknak")

3.  Jelölje ki a próba-felhasználó, kattintson a **hozzárendelni**, és kattintson az **Igen gombra** kattintva erősítse meg a hozzárendelés.

    ![Igen] (./media/active-directory-saas-concur-tutorial/IC767830.png "Igen")

Érdemes most várja meg a 10 perc, és ellenőrizze, hogy a fiók Concur lett-e szinkronizálva.

Ha meg szeretné tesztelje a egyszeri bejelentkezéses, Access Panel megnyitásához. Ha többet szeretne tudni az Access panelen című témakörben [az Access-panelen](active-directory-saas-access-panel-introduction.md).
