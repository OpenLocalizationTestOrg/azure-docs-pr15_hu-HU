<properties
    pageTitle="Első lépések az adatkatalógusban |} Microsoft Azure"
    description="Az esetek és Azure adatkatalógus lehetőségeit bemutató végpont oktatóanyag."
    documentationCenter=""
    services="data-catalog"
    authors="steelanddata"
    manager="jhubbard"
    editor=""
    tags=""/>
<tags
    ms.service="data-catalog"
    ms.devlang="NA"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="NA"
    ms.workload="data-catalog"
    ms.date="09/20/2016"
    ms.author="spelluru"/>

# <a name="get-started-with-azure-data-catalog"></a>Első lépések az Azure-adatkatalógus
Azure adatkatalógus egy teljes körű felügyelt felhőalapú szolgáltatásba, amely a bejegyzés és a rendszer a vállalati adatok eszközök feltárás szolgál. Részletes áttekintés olvassa el a [Mi az Azure adatkatalógus](data-catalog-what-is-data-catalog.md)című témakört.

Ebben az oktatóanyagban segítséget nyújt az Azure adatkatalógus használatának megkezdéséhez. Az alábbi eljárások ebben az oktatóanyagban hajtsa végre:

| Az eljárás | Leírás |
| :--- | :---------- |
| [Adatkatalógus kiépítése](#provision-data-catalog) | Ez az eljárás kiépítése, vagy Azure adatkatalógus beállítása. Ebben a lépésben csak akkor, ha a katalógus nem lett beállítva előtt végezze el. Egy szervezet (a Microsoft Azure Active Directory tartományi) csak egy adatkatalógus akkor is, ha vannak olyan több előfizetéssel társított fiókja Azure is. |
| [Regisztráljon az adatok eszközök](#register-data-assets) | Ez az eljárás a AdventureWorks2014 minta-adatbázisból származó adatok eszközök regisztrálhatja adatkatalógus adataival. Regisztráció az fő szerkezeti metaadatokat, például nevek, -típusok és helyek kiolvasó az adatforrásból, és a metaadat-alapú másolása a katalógus folyamatát. Az adatforrás és az adatok eszközök továbbra is a hol vannak, de a metaadatokat, hogy könnyebben felfedezhetőségét, valamint érthető a katalógus által használt. |
| [Adatok eszközök felfedezése](#discover-data-assets) | Ez az eljárás használatával az Azure adatkatalógus portál Fedezze fel, hogy az előző lépésben regisztráltak adatok eszközök. Miután az adatforrás Azure adatkatalógusban van regisztrálva, a metaadatai indexelt szolgáltatás, hogy a felhasználók könnyen is kereshet a szükséges adatok. |
| [Jegyzetelés adatok eszközök](#annotate-data-assets) | Az eljárás a széljegyzetek (információt, például a leírásokat, címkék, dokumentáció vagy szakértői) adatok eszközök megadnia. Ez az információ egészíti ki a metaadatokat az adatforrásból, és végezze el az adatforrás több érthető további személyeket kibontása. |
| [Csatlakozás adatok eszközök](#connect-to-data-assets) | Az eljárás nyissa meg az integrált ügyféleszközökben (például az Excel és az SQL Server Data Tools) adatok eszközök és nem integrált eszköz (SQL Server Management Studio). |
| [Adatok eszközök kezelése](#manage-data-assets) | Ez az eljárás a beállítása biztonsági az adatok eszközeit. Adatkatalógus nem felhasználók hozzáférésének biztosítása az adatok magát. Az adatforrás tulajdonosának adatokhoz való hozzáférés szabályozása <br/><br/> Az adatkatalógusban felfedezheti az adatforrások és a **metaadat-alapú** kapcsolódó regisztrált be a forrás megtekintése. Lehetnek esetek, amikor, azonban, ahol adatforrások legyenek láthatók, csak az adott felhasználókhoz vagy adott csoport tagjai. Ez a helyzet az adatkatalógus segítségével tulajdonosává regisztrált adatok eszközök a katalógus belül, és szabályozhatja az eszközöket, hogy Öné a olvashatóságát. |
| [Távolítsa el az adatok eszközök](#remove-data-assets) | Ezt az eljárást, megtudhatja, hogyan törölheti az adatkatalógusban adatok eszközök. |  

## <a name="tutorial-prerequisites"></a>Oktatóanyag vonatkozó követelmények

### <a name="azure-subscription"></a>Azure előfizetés
Azure adatkatalógus beállítani, a tulajdonos vagy egy Azure előfizetés társtulajdonos kell lennie.

Azure előfizetések hatékonyabban rendezheti a felhőalapú szolgáltatás erőforrások, mint a Azure adatkatalógus való hozzáférést. Azok is szabályozhatja, hogy hogyan erőforrás-kihasználtság jelenteni, Súgó számlát kapni, és a kifizetett. Egyes előfizetések van különböző számlázási és fizetési beállítás, így a különböző előfizetések és különböző tervek részleg, project, regionális iroda és így tovább. Előfizetéshez tartozik minden felhőalapú szolgáltatásba, és Azure adatkatalógus beállítása előtt előfizetés van szükség. További tudnivalókért olvassa el a [fiókok kezelése, az előfizetések elemre, és a rendszergazdai szerepkörök](../active-directory/active-directory-how-subscriptions-associated-directory.md)című témakört.

Ha nem előfizetés, létrehozhat egy ingyenes próba-fiókkal, mindössze néhány perc az. [Ingyenes próbaverzió](https://azure.microsoft.com/pricing/free-trial/) talál részleteket.

### <a name="azure-active-directory"></a>Azure Active Directory
Azure adatkatalógus beállítani, be kell jelentkezni az Azure Active Directory (Azure Active Directory) felhasználói fiókkal. A tulajdonos vagy egy Azure előfizetés társtulajdonos kell lennie.  

Azure Active Directory egyszerűvé a vállalati identitás- és az access, mind a felhőben, és a helyszíni kezeléséhez. Egy egyetlen munkahelyi vagy iskolai fiók segítségével jelentkezzen be a bármely felhő vagy a helyszíni webalkalmazást. Azure adatkatalógus Azure AD a bejelentkezési hitelesítő használja. További tudnivalókért olvassa el a [Mi az Azure Active Directory](../active-directory/active-directory-whatis.md)című témakört.

### <a name="azure-active-directory-policy-configuration"></a>Azure Active Directory-házirend beállítása

Ha jelentkezzen az Azure adatkatalógus portálon, de amikor megpróbálja jelentkezzen be az adatok forrása regisztrációs eszköz olyan helyzet előfordulhatnak, fel, amelyekkel megakadályozza, hogy bejelentkezés során hibaüzenet. Ez a hiba akkor fordulhat elő, amikor Ön a vállalati hálózaton, vagy a vállalati hálózatán kívülről érkező csatlakozik.

A regisztráció használja *űrlapok hitelesítési* felhasználói bejelentkezések ellen Azure Active Directory érvényesítéséhez. Sikeres bejelentkezés, az Azure Active Directory-rendszergazdának engedélyeznie kell a űrlapok hitelesítési a *globális hitelesítési házirend*.

A globális hitelesítési házirend engedélyezheti hitelesítés külön-külön intranetes és extranetes kapcsolatot, az alábbi képen látható módon. Bejelentkezési hibák akkor fordulhat elő, ha űrlapok hitelesítés nincs engedélyezve a hálózat, amelyhez csatlakozik.

 ![Azure Active Directory authentication globális házirend](./media/data-catalog-prerequisites/global-auth-policy.png)

További tudnivalókért lásd: a [konfigurálása hitelesítési házirendek](https://technet.microsoft.com/library/dn486781.aspx).

## <a name="provision-data-catalog"></a>Adatkatalógus kiépítése
Egy szervezet (Azure Active Directory tartományi) csak egy adatkatalógus is rendelkezni. Emiatt a tulajdonos vagy társtulajdonos az Azure előfizetés a Azure Active Directory-tartományhoz tartozik, akik már létrehozott egy katalógushoz, ha nem tudja ismét létrehozni egy katalógushoz, még akkor is, ha több Azure előfizetéssel rendelkezik. Tesztelése, hogy egy adatkatalógus létrehozott egy felhasználóhoz az Azure Active Directory-tartománya, nyissa meg az [Azure adatkatalógus kezdőlapja](http://azuredatacatalog.com) , és ellenőrizze, hogy látható-e a katalógus. Ha már hozott létre a katalógus meg, ugorja át a következő eljárással, és nyissa meg a következő szakaszban.    

1. Nyissa meg az [adatkatalógusban szolgáltatás lapon](https://azure.microsoft.com/services/data-catalog) , és kattintson az **első lépések**.

    ![Azure adatkatalógus – marketing kezdőlapja](media/data-catalog-get-started/data-catalog-marketing-landing-page.png)
2. Jelentkezzen be egy felhasználói fiókot, amely a tulajdonos vagy egy Azure előfizetés társtulajdonos. Bejelentkezés után megjelenik a következő lapra.

    ![Azure adatkatalógus – rendelkezést adatkatalógus](media/data-catalog-get-started/data-catalog-create-azure-data-catalog.png)
3. Adja meg az adatkatalógusban, az **előfizetés** használni kívánt és a **helyet** a katalógus **nevét** .
4. Bontsa ki a **árak** , és válassza az Azure adatkatalógus **edition** (szabad és normál).
    ![Azure adatkatalógus – választó edition](media/data-catalog-get-started/data-catalog-create-catalog-select-edition.png)
5. Bontsa ki a **Katalógus felhasználókat** , és kattintson a **Hozzáadás** gombra az adatkatalógusban felhasználók. Automatikusan megjelennek a csoporthoz.
    ![Azure adatkatalógus – felhasználók](media/data-catalog-get-started/data-catalog-add-catalog-user.png)
6. Bontsa ki a **Katalógus rendszergazdák** , és kattintson a **Hozzáadás** gombra az adatkatalógusban további rendszergazdák. Automatikusan megjelennek a csoporthoz.
    ![Azure adatkatalógus – rendszergazdáknak](media/data-catalog-get-started/data-catalog-add-catalog-admins.png)
7. Kattintson a **Katalógus létrehozása** adatkatalógusban, hogy a szervezet létrehozása parancsra. Létrehozása után megjelenik a Kezdőlap lap az adatkatalógusban.
    ![Azure adatkatalógus – létrehozott](media/data-catalog-get-started/data-catalog-created.png)    

### <a name="find-a-data-catalog-in-the-azure-portal"></a>Keresse meg a adatkatalógus az Azure-portálon
1. Kattintson egy külön lap a böngészőben, vagy a webes külön böngészőablakban az [Azure-portálra](https://portal.azure.com) , és jelentkezzen be ugyanazzal a fiókkal, akkor az előző lépésben az adatkatalógusban létrehozásához használt.
2. Válassza a **Tallózás gombra** , és kattintson az **Adatkatalógusban**.

    ![Azure adatkatalógus – Tallózás Azure](media/data-catalog-get-started/data-catalog-browse-azure-portal.png) megjelenik az adatkatalógusban hozott létre.

    ![Azure adatkatalógus – listában katalógus megtekintése](media/data-catalog-get-started/data-catalog-azure-portal-show-catalog.png)
4.  Kattintson a katalógus létrehozott. Megjelenik az **Adatkatalógusban** fel a portálon.

    ![Azure adatkatalógus – portálon a lap ](media/data-catalog-get-started/data-catalog-blade-azure-portal.png)
5. Az adatkatalógus tulajdonságainak megtekintése, és frissítheti az adataikat. Például **árak réteg** gombra, és a edition módosítása.

    ![Azure adatkatalógus – a réteg árak](media/data-catalog-get-started/data-catalog-change-pricing-tier.png)

### <a name="adventure-works-sample-database"></a>Adventure Works mintaadatbázis
Ebben az oktatóanyagban adatok eszközök (táblák) a AdventureWorks2014 mintavállalati adatbázisról a SQL Server adatbázismotor regisztrálni, de bármilyen támogatott adatforrás Ha inkább a jól ismert és szerepének fontos adatokkal végzett munkához használható. A támogatott adatforrások listáját a [támogatott adatforrások](data-catalog-dsr.md)című szakaszban.

### <a name="install-the-adventure-works-2014-oltp-database"></a>Az Adventure Works 2014-es OLTP adatbázis telepítése
Az Adventure Works adatbázis támogatja a normál online tranzakció feldolgozási esetek kitalált kerékpárok gyártó (Adventure Works ciklus), amely magában foglalja a termékek értékesítési és vásárlási. Ebben az oktatóanyagban rögzítheti az Azure adatkatalógus termékek adatait.

Az Adventure Works mintaadatbázis telepítése:

1. Töltse le az [Adventure Works 2014-es teljes adatbázist Backup.zip](https://msftdbprodsamples.codeplex.com/downloads/get/880661) funkcióhoz a Codeplex webhelyen.
2. Visszaállítása az adatbázis a számítógépre, kövesse az [SQL Server Management Studio segítségével adatbázis biztonsági másolatának visszaállítása](http://msdn.microsoft.com/library/ms177429.aspx), illetve ezeket a lépéseket követve:
    1. Nyissa meg az SQL Server Management Studio, majd az SQL Server adatbázismotor csatlakozzon.
    2. Kattintson a jobb gombbal az **adatbázisok** , és kattintson az **Adatbázis visszaállítása**parancsra.
    3. **Adatbázis visszaállítása**az csoportban kattintson az **adatforrás** **eszköz** beállításra, és kattintson a **Tallózás gombra**.
    4. **Jelölje ki a biztonsági másolat eszközöket**, csoportban kattintson a **Hozzáadás**gombra.
    5. Nyissa meg a mappát, ahol a **AdventureWorks2014.bak** nélkül, jelölje ki a fájlt, és a **Biztonságimásolat-fájl megkereséséhez** párbeszédpanel bezárásához kattintson **az OK** gombra.
    6. Kattintson **az OK gombra** kattintva zárja be a **biztonsági másolat eszközök kijelölése** párbeszédpanel.    
    7. Kattintson **az OK gombra** kattintva zárja be az **Adatbázis visszaállítása** párbeszédpanelen.

Most már rögzítheti az Adventure Works minta-adatbázisból származó adatok eszközök Azure adatkatalógus használatával.

## <a name="register-data-assets"></a>Regisztráljon az adatok eszközök

Ebben a gyakorlatban használatával a regisztrációs eszköz az Adventure Works-adatbázisból származó adatok eszközök regisztrálása a katalógus. Regisztráció az fő szerkezeti metaadatokat, például nevek, -típusok és helyek kinyerése az adatforrás és az eszközöket tartalmaz, és a metaadat-alapú másolása a katalógus folyamatát. Az adatforrás és az adatok eszközök továbbra is a hol vannak, de a metaadatokat, hogy könnyebben felfedezhetőségét, valamint érthető a katalógus által használt.

### <a name="register-a-data-source"></a>Adatforrás regisztrálása

1.  Nyissa meg az [Azure adatkatalógus kezdőlapja](https://azuredatacatalog.com) , és kattintson az **Adatok közzététele**.

    ![Azure adatok katalógus – adatok közzététele gomb](media/data-catalog-get-started/data-catalog-publish-data.png)

2.  Kattintson az **Alkalmazás indítása** letöltése, telepítése, és a regisztrációs eszköz futtatása a számítógépen.

    ![Azure adatkatalógus – Indítás gomb](media/data-catalog-get-started/data-catalog-launch-application.png)

3. A **Kezdőlap** lapon kattintson a **Bejelentkezés** gombra, és írja be hitelesítő adatait.    

    ![Azure adatkatalógus – kezdőlap](media/data-catalog-get-started/data-catalog-welcome-dialog.png)

4. A **Microsoft Azure adatkatalógus** lapon kattintson az **SQL Server** és a **következő**.

    ![Azure adatkatalógus – adatforrások](media/data-catalog-get-started/data-catalog-data-sources.png)

5.  Írja be az SQL Server-kapcsolat tulajdonságai **AdventureWorks2014** (lásd az alábbi példa), és kattintson a **Csatlakozás**gombra.

    ![Azure adatkatalógus – az SQL Server-kapcsolat beállításai](media/data-catalog-get-started/data-catalog-sql-server-connection.png)

6.  Regisztráljon az adatok eszköz a metaadatokat. Ebben a példában az AdventureWorks gyártási névtér **** Szer/objektumok rögzítése:

    1. A **Kiszolgáló hierarchia** fában bontsa ki a **AdventureWorks2014** és **munkakörnyezeti**.
    2. Jelölje ki a **terméket**, **ProductCategory**, **ProductDescription**és **ProductPhoto** a Ctrl + kattintás használatával.
    3. Kattintson az **Ugrás a kijelölt nyíl** (**>**). Ez a művelet az összes kijelölt objektum elmozdítása, az **objektumok regisztrálását** listába.

        ![Azure adatkatalógus oktatóanyag – Tallózás gombra, és objektumok kijelölése](media/data-catalog-get-started/data-catalog-server-hierarchy.png)
    4. Jelölje ki a **Belefoglalás előnézetét** felvenni az adatok pillanatkép előnézetét. A pillanatkép minden táblából legfeljebb 20 rekordokat tartalmazza, és azt érvényes értékét másolja a katalógus.
    5. Jelölje ki az **Adatok profillal együtt** az objektum statisztikák az adatok profil pillanatképét felvenni (például: az oszlop, a sorok száma minimum, a legnagyobb és a átlagos értékeket).
    6. A **Címkék hozzáadása** mezőbe írja be a **adventure works ciklus**elemre. Ez a művelet hozzáadja az adatok eszközök keresés címkék. Címkék nagyszerű módját segíti a felhasználókat, keresse meg a bejegyzett adatforrást.
    7. Az adatok (nem kötelező) egy **szakértői** nevét adja meg.

        ![Azure adatkatalógus oktatóanyag – regisztrálását objektumok](media/data-catalog-get-started/data-catalog-objects-register.png)

    8. Kattintson a **REGISZTRÁLÁS**gombra. Azure adatkatalógus regisztrálja a kijelölt objektumok. A következő gyakorlatban a kijelölt objektumok Adventure Works elemeinek regisztrálva van. A regisztráció eszköz metaadatok olvas az adatok eszköz, és másolja az adatokat az Azure adatkatalógus szolgáltatás. Az adatok marad, ahol jelenleg található, és csoportban a a rendszergazdák és a jelenlegi rendszer házirendek marad.

        ![Azure adatkatalógus – regisztrált objektumok](media/data-catalog-get-started/data-catalog-registered-objects.png)

    9. A bejegyzett adatforrás-objektumok megtekintéséhez kattintson a **Nézet portálon**. Az adatkatalógus Azure-portálon győződjön meg arról, látható, négy minden olyan tábla és a rács nézetben az adatbázist.

        ![Az Azure adatkatalógus portálon objektumok ](media/data-catalog-get-started/data-catalog-view-portal.png)


A következő gyakorlatban regisztrálta objektumok az Adventure Works minta adatbázisból, hogy azok könnyen megtalálják a felhasználók által a szervezeten belül. A következő gyakorló megismerheti, hogyan regisztrált adatok eszközök fel.

## <a name="discover-data-assets"></a>Adatok eszközök felfedezése
Azure adatkatalógus felderítése használja a két elsődleges mechanizmusok: keresés és a szűrést.

Keresés lett tervezve intuitív és hatékony is. Alapértelmezés szerint a keresési kifejezések teljesül a katalógus, beleértve a felhasználó által megadott jegyzetek bármely tulajdonság alapján.

Szűréssel lett tervezve kiegészítése keresése. Adott jellemzők, például szakértőket, adatforrástípus, objektum típusa és a címkék egyező adatok eszközök megtekintése és korlátozhatja a találatokat, hogy a megfelelő eszközökkel is választhat.

Keresés és szűrés használatával gyorsan megkeresheti Azure adatkatalógusban fel a szükséges adatok eszközök regisztrálta adatforrások.

Ebben a gyakorlatban használatával az Azure adatkatalógus portál felfedezése adatok eszközök előző ellátása regisztrálta. Lásd: a [Keresés az adatkatalógusban szintaxisához](https://msdn.microsoft.com/library/azure/mt267594.aspx) keresési szintaxisa kapcsolatban további tájékoztatást.

Az alábbiakban néhány példa az adatok eszközök a katalógus felfedezése.  

### <a name="discover-data-assets-with-basic-search"></a>Egyszerű keresési adatok eszközök felfedezése
Egyszerű keresési segít, hogy a katalógus keresése egy vagy több keresési kifejezések használatával. Eredménye bármely eszközök, amelyek megegyeznek bármely tulajdonság egy vagy több megadott feltételek alapján.

1. Az adatkatalógus Azure-portálon kattintson a **Kezdőlap fülre** . Ha a webböngészőben van megnyitva, lépjen az [Azure adatkatalógus kezdőlapja](https://www.azuredatacatalog.com).
2. A Keresés mezőbe írja be a `cycles` , és nyomja le az **ENTER BILLENTYŰT**.

    ![Azure adatkatalógus – egyszerű szöveg keresése](media/data-catalog-get-started/data-catalog-basic-text-search.png)
3. Győződjön meg arról, hogy látni minden négy tábla és az adatbázis (AdventureWorks2014) a találatok között. Kattintson az eszköztár gombjainak, az alábbi képen látható módon válthat **Rács** és **lista nézet** között. Figyelje meg, hogy a keresett szót a keresési eredmények kiemelten, mivel a **Kiemelés** beállítás be **KAPCSOLVA**. **Találatok oldalankénti** száma a keresési eredmények között is megadhatja.

    ![Azure adatkatalógus – egyszerű szöveg keresési találatok](media/data-catalog-get-started/data-catalog-basic-text-search-results.png)

    A **Keresés** panel a bal oldalon pedig a **tulajdonságai** panel a jobb oldalon. A **Keresés** panel a keresési feltételek módosítása és a szűrést. A **Tulajdonságok** ablaktábla a rács és a lista kijelölt objektum tulajdonságai jeleníti meg.

4. A keresési eredmények között kattintson a **terméket** . Kattintson az **Előnézet**, **oszlopok**, **Adatok profil**és **dokumentáció** lapok, vagy kattintson a nyílra kattintva bontsa ki az alsó ablaktáblában.  

    ![Azure adatkatalógus – alsó ablaktáblában](media/data-catalog-get-started/data-catalog-data-asset-preview.png)

    A **kép** lap megjelenik a **termékek** tábla adatainak előnézetét.  
5. Kattintson az oszlopok (például a **nevét** és **adattípusát**) tájékoztatás az adatok eszköz található **oszlopok** fülre.
6. Kattintson a **Adatok profil** fülre kattintva megtekintheti az adatok adatainak összegyűjtése (például: adatok vagy egy oszlopban lévő legkisebb érték méretét, a sorok száma) az adatok eszközt.
7. Találatok szűrése a bal oldali **szűrők** használatával. Ha például **táblázat** az **Objektum típusa**, és: csak a négy táblákat, nem az adatbázis.

    ![Azure adatkatalógus – találatok szűrése](media/data-catalog-get-started/data-catalog-filter-search-results.png)

### <a name="discover-data-assets-with-property-scoping"></a>Adatok eszközök tulajdonság hatókör meghatározása felfedezése
Hatókör meghatározása a tulajdonság segít, hogy adatokat eszközök, ahol a keresett kifejezést a megadott tulajdonság megegyezik.

1. A **tábla** szűrő **Objektumtípusnak** a **szűrők**csoportban törölje a jelet.  
2. A Keresés mezőbe írja be a `tags:cycles` , és nyomja le az **ENTER BILLENTYŰT**. [Keresés az adatkatalógusban szintaxisához](https://msdn.microsoft.com/library/azure/mt267594.aspx) talál, amellyel keresés az adatkatalógusban Tulajdonságok parancsot.
3. Győződjön meg arról, hogy látni minden négy tábla és az adatbázis (AdventureWorks2014) a találatok között.  

    ![Adatkatalógus – hatókör meghatározása a keresési eredmények tulajdonság](media/data-catalog-get-started/data-catalog-property-scoping-results.png)

### <a name="save-the-search"></a>A Keresés mentése
1. Az **Aktuális keresés** szakasz **Keresés** panelen adjon egy nevet a keresést, és kattintson a **Mentés**gombra.

    ![Azure adatkatalógus – Keresés mentése](media/data-catalog-get-started/data-catalog-save-search.png)
2. Győződjön meg arról, hogy a mentett keresés jeleníti meg a **Mentett**.

    ![Azure adatkatalógus – mentett keresések](media/data-catalog-get-started/data-catalog-saved-search.png)
3. Jelölje ki azt a műveletek meg a mentett keresés (**átnevezése**, **törlése**, **Mentse az alapértelmezett** keresési).

    ![Azure adatkatalógus – a keresési beállítások mentése](media/data-catalog-get-started/data-catalog-saved-search-options.png)

### <a name="boolean-operators"></a>Logikai operátorok
Bővítése, illetve a logikai operátorokat a keresés szűkítéséhez.

1. A Keresés mezőbe írja be a `tags:cycles AND objectType:table`, és nyomja le az **ENTER BILLENTYŰT**.
2. Győződjön meg arról, hogy láthatóvá csak táblázatok (nem az adatbázis) a találatok között.  

    ![Azure adatkatalógus – Keresés logikai operátorok](media/data-catalog-get-started/data-catalog-search-boolean-operator.png)

### <a name="grouping-with-parentheses"></a>Zárójelek a csoportosításhoz
A zárójelek között csoportosítva csoportosíthatja a logikai elkülönítési, különösen a logikai operátorokat együtt eléréséhez a lekérdezés részei.

1. A Keresés mezőbe írja be a `name:product AND (tags:cycles AND objectType:table)` , és nyomja le az **ENTER BILLENTYŰT**.
2. Győződjön meg arról, hogy csak a keresési eredmények között a **termékek** tábla látható.

    ![Azure adatkatalógus – csoportosítási keresés](media/data-catalog-get-started/data-catalog-grouping-search.png)   

### <a name="comparison-operators"></a>Összehasonlító operátorok
Összehasonlító operátorokkal nem egyenlő összehasonlítása az tulajdonságaik vannak, amelyek numerikus és dátum típusú adatokat is használhatja.

1. A Keresés mezőbe írja be a `lastRegisteredTime:>"06/09/2016"`.
2. A **tábla** szűrő **Objektum típusa**csoportban törölje a jelet.
3. Nyomja le az **ENTER BILLENTYŰT**.
4. Győződjön meg arról, hogy látható, a **termék** **ProductCategory**, **ProductDescription**és **ProductPhoto** táblák és a AdventureWorks2014 adatbázis regisztrálta a keresési eredmények között.

    ![Azure adatkatalógus – összehasonlító keresési találatok](media/data-catalog-get-started/data-catalog-comparison-operator-results.png)

Megtudhatja, [hogy miként adatok eszközök felfedezése](data-catalog-how-to-discover.md) adatok eszközök és a [Keresés az adatkatalógusban szintaxisához](https://msdn.microsoft.com/library/azure/mt267594.aspx) felfedezése keresési szintaxis részletes információt.

## <a name="annotate-data-assets"></a>Jegyzetelés adatok eszközök
Ebben a gyakorlatban használatával az Azure adatkatalógus portál széljegyzetekkel lássanak el (például leírások, címkék vagy szakértői információ hozzáadása) korábban regisztrálta a katalógus adatok eszközök. A széljegyzetek kiegészítése és tartalmasabbá teszi a regisztráció során az adatforrás kiolvasott szerkezeti metaadatokat, és lehetővé teszi az adatok eszközök sokkal egyszerűbb Fedezze fel, és megértette.

A következő gyakorlatban jegyzetelhet a egyetlen adatok eszköz (ProductPhoto). Egy rövid nevet, és a leírás hozzáadása a ProductPhoto adatok eszköz.  

1.  Nyissa meg az [Azure adatkatalógus kezdőlapja](https://www.azuredatacatalog.com) , és a keresési `tags:cycles` keresése az adatok eszközök regisztrálta.  
2. Kattintson a **ProductPhoto** a keresési eredmények között.  
3. Írja be **termék képek** **Rövid nevet** , és **marketinganyagok fényképek termék** **leírását**.

    ![Azure adatkatalógus – ProductPhoto leírása](media/data-catalog-get-started/data-catalog-productphoto-description.png)

    A **Leírás** segít megelőzni mások felderíteni és megértéséhez és a kijelölt adatok eszköz használatával kapcsolatban. Is hozzáadhat további címkéket és a hasábokba. Most megpróbálhatja keresése és -szűrés adatok eszközök fel a felvett a katalógus leíró metaadatok használatával.

Megteheti azt is, ezen a lapon a következőket:

- Az adatok eszköz szakértői hozzáadása. **Szakértői** területén a **Hozzáadás** gombra.
- Címkék hozzáadása az adatkészlet szintjén. Kattintson a **Hozzáadás** a **címkék** területen. Címke lehet egy felhasználó vagy egy szószedet kód. A Standard Edition az adatkatalógusban egy üzleti szószedet, amely segít a katalógus rendszergazdák megadása egy központi üzleti besorolás tartalmazza. Katalógus felhasználók majd széljegyzetelheti adatok eszközök szószedet adatokkal. További információért megtudhatja, [hogy miként állíthatja be a vállalati szószedet szabályozott címkézéshez](data-catalog-how-to-business-glossary.md)
- Címkék hozzáadása oszlop szintre. Kattintson a **Hozzáadás** a **címkék** annak az oszlopnak a jegyzet.
- Adjon meg leírást az oszlop szintjén. Oszlop **leírásának** megadása A leírás metaadatokat az adatforrásból kibontása is megtekintheti.
- Adjon **hozzáférési kérelmek** , amely ismerteti, hogy hogyan az adatok eszköz hozzáférést kérni.

    ![Azure adatkatalógus – címkék, leírások hozzáadása](media/data-catalog-get-started/data-catalog-add-tags-experts-descriptions.png)

- Válassza a **dokumentáció** lapot, majd az adatok eszköz bizonylatot. Azure adatkatalógus dokumentációt is használhatja az adatkatalógusban egy tartalomtár, hozzon létre egy teljes szövegrész az adatok eszközeit.

    ![Azure adatkatalógus – dokumentáció lap](media/data-catalog-get-started/data-catalog-documentation.png)


Több adat eszközök is felveheti egy jegyzetet. Például regisztrálta adatok eszközök kiválasztásához, és adjon meg egy szakértővel azokat.

![Azure adatkatalógus – széljegyzetekkel lássanak el a több adat eszközök](media/data-catalog-get-started/data-catalog-multi-select-annotate.png)

Azure adatkatalógus széljegyzetek tömegből forrás megközelítése támogatja. Adatkatalógus bárki hozzáadhat címkék (a felhasználó vagy szószedet), leírás és más metaadat-alapú, hogy bármelyik felhasználó egy adatok eszköz és tartalmaz egy perspektivikus, hogy perspektíva rögzített és más felhasználók számára is.

Megtudhatja, [hogy miként széljegyzetekkel lássanak el az adatok eszközök](data-catalog-how-to-annotate.md) jegyzetet fűz az adatok eszközök részletes információt.

## <a name="connect-to-data-assets"></a>Csatlakozás adatok eszközök
Ebben a gyakorlatban, nyissa meg az integrált ügyfél eszköz (Excel) és a nem integrált eszköz (SQL Server Management Studio) adatok eszközök segítségével a kapcsolat adatait.

> [AZURE.NOTE] Fontos, hogy ne feledje, hogy Azure adatkatalógus nem hozzáférést biztosít a tényleges adatforrás – egyszerűen egyszerűbbé teszi Fedezze fel, és megértette. Amikor csatlakozik egy adatforráshoz, a kiválasztott ügyfélalkalmazás használja a Windows hitelesítő adatait, vagy szükség szerint hitelesítő adatokat kér. Ha Ön nem korábban megkapta az adatforrás eléréséhez, való csatlakozás előtt hozzáférési jogosultsággal kell.

### <a name="connect-to-a-data-asset-from-excel"></a>Csatlakozás egy adatok eszköz az Excel alkalmazásból

1. Keresési eredmények közül válassza ki a **terméket** . **Nyissa meg a** kattintson az eszköztáron, és kattintson az **Excelben**.

    ![Azure adatkatalógus – adatok eszköz csatlakoztatása](media/data-catalog-get-started/data-catalog-connect1.png)
2. Kattintson a **Megnyitás** a letöltés előugró ablakban. Ez a módja attól függően, hogy a böngészőben is függhet.

    ![Azure adatkatalógus – az Excel-kapcsolat letöltött fájl](media/data-catalog-get-started/data-catalog-download-open.png)
3. A **Microsoft Excel biztonsági értesítés** ablakában kattintson a **engedélyezése**lehetőséget.

    ![Azure adatkatalógus – Excel biztonsági helyi menüje](media/data-catalog-get-started/data-catalog-excel-security-popup.png)
4. Az alapértelmezett megtartása az **Adatimportálás** párbeszédpanelen, és kattintson az **OK gombra**.

    ![Azure adatkatalógus – az Excel-adatok importálása](media/data-catalog-get-started/data-catalog-excel-import-data.png)
5. Az adatforrás megtekintése az Excel programban.

    ![Azure adatkatalógus – a termékek tábla az Excel programban](media/data-catalog-get-started/data-catalog-connect2.png)

A következő gyakorlatban csatlakozva adatokat eszközök Azure adatkatalógus használatának által talált. Az Azure adatkatalógus portálon csatlakoztathatja közvetlenül beépül a menü **megnyitása az** ügyfél-alkalmazások használatával. Bármely alkalmazásban, kiválaszthatja, hogy a kapcsolat helyére vonatkozó adatok a digitáliseszköz-metaadatok szereplő használatával is csatlakozhat. Ha például az adatok eszközök, ebben az oktatóanyagban regisztrált adatok eléréséhez a AdventureWorks2014 adatbázishoz való csatlakozáshoz SQL Server Management Studio is használhatja.

1. Nyissa meg az **SQL Server Management Studio eszközben**.
2. A **Csatlakozás a kiszolgálóhoz** párbeszédpanelen adja meg az Azure adatkatalógus portálon **tulajdonságai** munkaablakból a kiszolgáló nevét.
3. Az adatok eszköz eléréséhez használja a megfelelő és hitelesítő adatokat. Ha nem rendelkezik hozzáféréssel, információk segítségével a **Hozzáférés kérése** mező el.

    ![Azure adatkatalógus – a hozzáférési kérelmek](media/data-catalog-get-started/data-catalog-request-access.png)

Kattintson a **Nézet Csatlakozási_karakterlánc** megtekintése és ADF.NET, az ODBC és OLEDB a csatlakozási_karakterlánc másolja a vágólapra az alkalmazásban használható.

## <a name="manage-data-assets"></a>Adatok eszközök kezelése
Ebben a lépésben látni, hogy miként állíthatja be az adatok eszközök biztonsági. Adatkatalógus nem felhasználók hozzáférésének biztosítása az adatok magát. Az adatforrás tulajdonosának adatokhoz való hozzáférés szabályozása

Adatkatalógus felfedezése adatforrások és megtekintheti a metaadat-alapú regisztrált a katalógus forrásainak kapcsolódó használhatja. Lehetnek esetek, amikor, azonban hol adatforrások csak akkor látható, az adott felhasználókhoz vagy adott csoport tagjai. Ez a helyzet a bejegyzett adatok eszközök belül a katalógus tulajdonlásának átvétele adatkatalógus is használhatja, és majd szabályozhatja az eszközök láthatóságát tulajdonjogának gombra.

> [AZURE.NOTE] Az ebben a gyakorlatban ismertetett kezelési funkciók csak a Standard Edition az Azure adatkatalógus, ingyenes kiadásában nem érhetők el.
Azure adatkatalógusában adatok eszközök tulajdonlásának átvétele, közös tulajdonosok hozzáadása adatok eszközök és a láthatóság adatok eszközök beállítása.

### <a name="take-ownership-of-data-assets-and-restrict-visibility"></a>Adatok eszközök tulajdonlásának átvétele és láthatóság korlátozása

1. Nyissa meg az [Azure adatkatalógus kezdőlapján](https://www.azuredatacatalog.com). A **keresett** szöveg mezőbe írja be a `tags:cycles` , és nyomja le az **ENTER BILLENTYŰT**.
2. A találatlistában a kívánt elemre, és **Tulajdonosi készítése** kattintson az eszköztáron.
3. A **Tulajdonságok** ablaktábla **kezelés** csoportjában kattintson a **Tulajdonjog készítése**lehetőséget.

    ![Azure adatkatalógus – take tulajdonjogát](media/data-catalog-get-started/data-catalog-take-ownership.png)
4. Ha korlátozni szeretné a láthatóság, válassza a **tulajdonosok és a felhasználók** a **láthatóság** csoport, és kattintson a **Hozzáadás**gombra. Adja meg a felhasználói e-mail címet a szövegmezőbe, és nyomja le az **ENTER BILLENTYŰT**.

    ![Azure adatkatalógus – hozzáférés korlátozása](media/data-catalog-get-started/data-catalog-ownership.png)

## <a name="remove-data-assets"></a>Távolítsa el az adatok eszközök

A következő gyakorlatban használatával az Azure adatkatalógus portál előzetes adat törlése a bejegyzett adatok eszközök és adatok eszközök törlése a katalógusról.

Azure adatkatalógus egyes tárgyi eszköz törlése vagy több eszközök törlése.

1. Nyissa meg az [Azure adatkatalógus kezdőlapján](https://www.azuredatacatalog.com).
2. A **keresett** szöveg mezőbe írja be a `tags:cycles` , és kattintson az **ENTER BILLENTYŰT**.
3. Az eredmény listában válasszon ki egy elemet, és kattintson a **Törlés** parancsra az eszköztáron az alábbi képen látható módon:

    ![Azure adatkatalógus – rács elem törlése lehetőség](media/data-catalog-get-started/data-catalog-delete-grid-item.png)

    A listanézet használja, ha a jelölőnégyzet be van bal oldalán a cikk az alábbi képen látható módon:

    ![Azure adatkatalógus – elemének törlése](media/data-catalog-get-started/data-catalog-delete-list-item.png)

    Jelölje be a több adat eszközök is, és törölheti őket, az alábbi képen látható módon:

    ![Azure adatkatalógus – több adat eszközök törlése](media/data-catalog-get-started/data-catalog-delete-assets.png)


> [AZURE.NOTE] Az alapértelmezett működés a katalógus minden felhasználó bármely adatforrás regisztrálása, és engedélyezze, hogy bármelyik felhasználó törlése bármely adatok eszköz, amely regisztrálva. A Standard Edition az Azure adatkatalógus szereplő szolgáltatásairól további lehetőségeket eszközök korlátozása, észleljék az eszközök, akik tulajdonjogát, és korlátozása, aki törölheti az eszközök biztosítanak.


## <a name="summary"></a>Összefoglalás

Ebben az oktatóanyagban megismerkedett Azure adatkatalógus, például hogy regisztrált, jegyzetet fűz, felfedezése és vállalati adatok eszközök kezelése alapvető lehetőségeit. Most, hogy befejezte az oktatóanyagot, pedig a kezdéshez. Előkészületek nélkül használhatja a ma Ön és csapata támaszkodhat adatforrás regisztrálása és a katalógus használata felkéréséről munkatársak segítségével.

## <a name="references"></a>Hivatkozások

- [Hogyan lehet rögzíteni az adatok eszközök](data-catalog-how-to-register.md)
- [Hogyan adatok eszközök felfedezése](data-catalog-how-to-discover.md)
- [Hogyan lehet a jegyzet adatok eszközök](data-catalog-how-to-annotate.md)
- [Hogyan lehet a dokumentumhoz adatok eszközök](data-catalog-how-to-documentation.md)
- [Adatok eszközök keresztüli](data-catalog-how-to-connect.md)
- [Adatok eszközök kezelése](data-catalog-how-to-manage.md)
