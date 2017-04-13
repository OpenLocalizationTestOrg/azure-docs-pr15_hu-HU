<properties 
    pageTitle="Oktatóprogram: Bejövő szinkronizálás kalk.munkanap beállítása |} Microsoft Azure" 
    description="Megtudhatja, hogyan használhatja a bejövő szinkronizálás az Azure Active Directory ahhoz, hogy az egyszeri bejelentkezés, automatikus kiépítési és az egyéb!" 
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
    ms.date="04/06/2016" 
    ms.author="jeedes" />

#<a name="tutorial-configuring-workday-for-inbound-synchronization"></a>Oktatóprogram: Bejövő szinkronizálás kalk.munkanap konfigurálása
>[AZURE.NOTE]Azure Active Directory (AD) prémium ügyfeleknek Kínában használja az Azure Active Directory világszerte példány érhető el.    
Azure Active Directory prémium jelenleg nem támogatott a Microsoft Azure Kínában a 21Vianet által üzemeltetett szolgáltatásban.    

Ebben az oktatóanyagban célja mutatja, hogy a lépéseket kell elvégeznie a kalk.munkanap és a Microsoft Azure Active Directory személyek kalk.munkanap importálása a Microsoft Azure AD.    
 Az alkalmazási példát, ebben az oktatóanyagban tagolt feltételezi, hogy már rendelkezik az alábbiakat:  

-   Érvényes Azure előfizetés  
-   A kalk.munkanap bérlői webhelyre  

A következő építőelemek az alkalmazási példát, ebben az oktatóanyagban tagolt áll:  

1.  A kalk.munkanap az alkalmazás integráció engedélyezése  
2.  Integrációs rendszer felhasználó létrehozása  
3.  Biztonsági csoport létrehozása  
4.  A biztonsági csoport számára oszt ki az integration rendszer felhasználói  
5.  Biztonsági csoport beállításaival  
6.  Biztonsági házirend-módosítások aktiválása  
7.  A Microsoft Azure Active Directory konfigurálása felhasználói importálása  

##<a name="enabling-the-application-integration-for-workday"></a>A kalk.munkanap az alkalmazás integráció engedélyezése

Ez a szakasz célja tagolása a Salesforce-alkalmazás integráció engedélyezése.    

###<a name="to-enable-the-application-integration-for-workday-perform-the-following-steps"></a>Ahhoz, hogy az alkalmazás integrációját Workday, hajtsa végre az alábbi lépéseket:

1.  Az Azure felügyeleti portálon kattintson a bal oldali navigációs területen kattintson az **Active Directory**.    

    ![Az Active Directory] (./media/active-directory-saas-inbound-synchronization-tutorial/IC700993.png "Az Active Directory")  

2.  A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.    

3.  Nyissa meg az alkalmazások nézetben, a címtár-nézetben, kattintson a felső menüben **alkalmazásokat** .    

    ![Alkalmazások] (./media/active-directory-saas-inbound-synchronization-tutorial/IC700994.png "Alkalmazások")  

4.  Az **Alkalmazás gyűjtemény**megnyitásához kattintson **Az alkalmazás hozzáadása**, és kattintson a **Hozzáadás az alkalmazások használata a szervezet számára**.    

    ![Milyen feladatot szeretne tenni?] (./media/active-directory-saas-inbound-synchronization-tutorial/IC700995.png "Milyen feladatot szeretne tenni?")  

5.  A **Keresés mezőbe**írja be a **kalk.munkanap**.    

    ![Kalk.munkanap] (./media/active-directory-saas-inbound-synchronization-tutorial/IC701021.png "Kalk.munkanap")  

6.  Az eredmény ablaktáblában jelölje ki a **kalk.munkanap**, és válassza a **kész** , az alkalmazás hozzáadása parancsra.    

    ![Kalk.munkanap] (./media/active-directory-saas-inbound-synchronization-tutorial/IC701022.png "Kalk.munkanap")  

##<a name="creating-an-integration-system-user"></a>Integrációs rendszer felhasználó létrehozása

1.  A **Kalk.munkanap munkaterület**írja be a **felhasználó létrehozása** a Keresés mezőbe, és kattintson a hivatkozásra, **Integráció a rendszer felhasználói létrehozása**gombjára.     

    ![felhasználó létrehozása] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750979.png "felhasználó létrehozása")  

2.  A felhasználónév és jelszó megadásával integráció a rendszer új felhasználó a integráció a rendszer felhasználói létrehozása-feladat végrehajtása  Hagyja üresen a szükséges új jelszavát a következő bejelentkezés lehetőséget, mivel ez a felhasználó fog kell bejelentkezés programozás útján.    
    Kilépés a munkamenet-időtúllépés perc az alapértelmezett értékére 0, ami megakadályozza, hogy a felhasználó munkamenetek időtúllépés idő.    

    ![Integráció a rendszer felhasználói létrehozása] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750980.png "Integráció a rendszer felhasználói létrehozása")  

##<a name="creating-a-security-group"></a>Biztonsági csoport létrehozása

Ebben az oktatóanyagban tagolt esetben szeretne megkötés nélkül integrációs rendszer biztonsági csoport létrehozása, és rendelje hozzá a felhasználót.    

1.  Írja be biztonsági csoport létrehozása a Keresés mezőbe, és kattintson a biztonsági csoport létrehozása hivatkozásra.     

    ![CreateSecurity csoport] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750981.png "CreateSecurity csoport")  

2.  A biztonsági csoport létrehozása-feladat végrehajtása  Válassza a integrációs rendszer biztonsági csoport – megkötés nélkül hozhat létre egy olyan biztonsági csoportot, amelyhez tagokat kifejezetten hozzáadódik, a típus, központjaként biztonsági csoport legördülő listából.     

    ![CreateSecurity csoport] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750982.png "CreateSecurity csoport")  

##<a name="assigning-the-integration-system-user-to-the-security-group"></a>A biztonsági csoport számára oszt ki az integration rendszer felhasználói

1.  Adja meg a biztonsági csoport szerkesztése a Keresés mezőbe, és kattintson a **Biztonsági csoport szerkesztése**hivatkozásra.     

    ![Biztonsági csoport szerkesztése] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750983.png "Biztonsági csoport szerkesztése")  

2.  Keres, és jelölje be az új integrációs biztonsági csoport név szerint    

    ![Biztonsági csoport szerkesztése] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750984.png "Biztonsági csoport szerkesztése")  

3.  Az új integrációs rendszer felhasználó hozzáadása az új biztonsági csoport.       

    ![Rendszer biztonsági csoport] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750985.png "Rendszer biztonsági csoport")  

##<a name="configuring-security-group-options"></a>Biztonsági csoport beállításaival

Ebben a lépésben, adja meg az új biztonsági csoport engedélyeket az objektumok, az alábbi tartomány biztonsági házirendek által biztosított Get és helyezése műveleteket:  

-   Külső fiók kiépítése  
-   Dolgozói adatokat: Nyilvános dolgozó-jelentések  
-   Dolgozói adatokat: Az összes beosztások  
-   Adatok dolgozó: Aktuális munkaerő-információ  
-   Dolgozói adatokat: Dolgozói profil üzleti cím  

&nbsp;  

1.  A Keresés mezőbe írja be a tartomány biztonsági házirendeket, és válassza a tartomány biztonsági házirendek működési terület hivatkozásra.     

    ![Tartomány biztonsági házirendek] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750986.png "Tartomány biztonsági házirendek")  

2.  Keresse meg a rendszer, és jelölje ki a rendszer funkcionális területet.  Kattintson az OK gombra, címkézett gombra.     

    ![Tartomány biztonsági házirendek] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750987.png "Tartomány biztonsági házirendek")  

3.  A listában, a rendszer funkcionális terület biztonsági házirendeket bontsa ki a biztonsági felügyelet, és jelölje be a tartomány biztonsági házirendje külső fiókot kiépítési.     

    ![Tartomány biztonsági házirendek] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750988.png "Tartomány biztonsági házirendek")  

4.  Kattintson a engedélyeinek módosítása gombra, és ezt követően a engedélyeinek módosítása képernyőn az új biztonsági csoport hozzáadása a Get- és helyezése integrációs engedélyekkel rendelkező biztonsági csoportok listája.     

    ![Engedélyek szerkesztése] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750989.png "Engedélyek szerkesztése")  

5.  Ismételje meg a lépés: 1, a fenti vissza szeretne térni a képernyőhöz funkcionális területeket, kijelölésére szolgáló, keressen a munkaerő-, jelölje be a személyzeti funkcionális területre, és kattintson a felcímkézett, az OK gombra.    

    ![Tartomány biztonsági házirendek] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750990.png "Tartomány biztonsági házirendek")  

6.  Bontsa ki a személyzeti funkcionális terület biztonsági házirendek listájának dolgozó adatok: személyzeti és ismételt lépés: 4, a fenti ezekre a hátralévő biztonsági házirendek:    

    -   Dolgozói adatokat: Nyilvános dolgozó-jelentések  
    -   Dolgozói adatokat: Az összes beosztások  
    -   Adatok dolgozó: Aktuális munkaerő-információ  
    -   Dolgozói adatokat: Dolgozói profil üzleti cím    

    ![Tartomány biztonsági házirendek] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750991.png "Tartomány biztonsági házirendek")  

##<a name="activating-security-policy-changes"></a>Biztonsági házirend-módosítások aktiválása

1.  Írja be a keresőmezőbe aktiválása, és kattintson a hivatkozásra, függőben lévő biztonsági módosítások aktiválása gombra.    

    ![Aktiválása] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750992.png "Aktiválása")  

2.   Kezdje el a függőben lévő biztonsági módosítások aktiválása tevékenység: Megjegyzés megadása naplózási céljából, és válassza a gombon címkézni, az OK gombra.      

    ![Függőben lévő biztonsági aktiválása] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750993.png "Függőben lévő biztonsági aktiválása")  

3.  A következő képernyőn a tevékenység befejezéséhez, jelölje be a jelölőnégyzetet, címkézett jóváhagyása és gombjára kattintva címkézni, az OK gombra.     

    ![Függőben lévő biztonsági aktiválása] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750994.png "Függőben lévő biztonsági aktiválása")  

##<a name="configuring-user-import-in-microsoft-azure-ad"></a>A Microsoft Azure Active Directory konfigurálása felhasználói importálása

Ez a szakasz célja konfigurálása a Microsoft Azure AD-felhasználók importálása a kalk.munkanap tagolása.    

###<a name="to-configure-user-import-in-microsoft-azure-ad-perform-the-following-steps"></a>Adja meg a felhasználó importálása a Microsoft Azure Active Directory, hajtsa végre az alábbi lépéseket:

1.  A **kalk.munkanap** alkalmazás integrációs lapon kattintson a **konfigurálás felhasználói importálása** a **Kiépítési beállítása** párbeszédpanel megnyitásához.    

2.  A **Beállítások és a rendszergazdai hitelesítő adatait** lapon hajtsa végre az alábbi lépéseket, és kattintson a Tovább gombra:    

    ![Beállítások és a rendszergazdai hitelesítő adatok] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750995.png "Beállítások és a rendszergazdai hitelesítő adatok")    

    1.  **Kalk.munkanap felügyeleti felhasználónév** mezőbe írja be a felhasználót a [rendszer integrációs felhasználó létrehozása](https://msdn.microsoft.com/library/azure/Dn762434.aspx#BKMK_CreateUser) szakaszban létrehozott nevét.    
    2.  **Kalk.munkanap rendszergazdai jelszó** mezőbe írja be annak a felhasználónak, a [rendszer integrációs felhasználó létrehozása](https://msdn.microsoft.com/library/azure/Dn762434.aspx#BKMK_CreateUser) szakaszban létrehozott jelszót.    
    3.  **Kalk.munkanap bérlői webhely URL-cím** mezőbe írja be az URL-cím vagy a kalk.munkanap bérlő.    

3.  A **kapcsolat tesztelése** lapon kattintson az **Indítás** erősítse meg a kapcsolat gombra, és kattintson a **Tovább gombra**.    

    ![A kapcsolat tesztelése] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750996.png "A kapcsolat tesztelése")  

4.  A **létesítése beállítások** lapon kattintson a **Tovább**gombra.    

    ![Kiépítési beállításai] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750997.png "Kiépítési beállításai")  

5.  A **kiépítési indítása** párbeszédpanelen kattintson a **Befejezés**gombra.    

    ![Indítsa el a kiépítési] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750998.png "Indítsa el a kiépítési")  

Ezután nyissa meg a **felhasználók** szakaszban, és ellenőrizze, hogy importálta munkanap felhasználói.    
