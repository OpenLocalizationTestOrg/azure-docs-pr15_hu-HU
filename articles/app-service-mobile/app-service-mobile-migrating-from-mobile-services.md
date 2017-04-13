<properties
    pageTitle="Mobil szolgáltatások áttelepítése-szolgáltatási alkalmazás mobilalkalmazásban"
    description="Megtudhatja, hogy miként egyszerűen áttelepítése a Mobile Services alkalmazás egy alkalmazás Mobile-alkalmazás"
    services="app-service\mobile"
    documentationCenter=""
    authors="adrianhall"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="adrianha"/>

# <a name="article-top"></a>A meglévő Azure mobilszolgáltatási áttelepítése az Azure alkalmazás szolgáltatás

Az [Általános elérhetősége Azure alkalmazás szolgáltatás]Azure Mobile Services webhelyek telepíthető egyszerűen át helyi kihasználhatja az Azure alkalmazás szolgáltatás összes funkcióját.  A dokumentum megtudhatja, mire számíthat, amikor a webhely áttelepítése az Azure Mobile szolgáltatások Azure alkalmazás szolgáltatásba.

## <a name="what-does-migration-do"></a>Áttelepítési mire szolgál a webhelyre

Az Azure mobilszolgáltatási áttelepítése, azzal kikapcsolja a mobilszolgáltatási- [Azure alkalmazás szolgáltatás] alkalmazásba a kód megtartásával.  Az értesítés hubok, SQL származó adatok, hitelesítési beállítások, ütemezett feladat, és a tartománynév maradjanak nem változik.  Az Azure mobilszolgáltatás használatba mobilügyfelek további rendes működését.  Áttelepítési újraindul a szolgáltatásban, miután Azure alkalmazás szolgáltatás átkerül.

[AZURE.INCLUDE [app-service-mobile-migrate-vs-upgrade](../../includes/app-service-mobile-migrate-vs-upgrade.md)]

## <a name="why-migrate"></a>Miért át kell telepítenie a webhelyen

A Microsoft ajánlása van, telepítse az Azure mobilszolgáltatási kihasználhatja az alkalmazás Azure szolgáltatás, beleértve a funkciók:

  *  Új host funkcióiról, például [WebJobs] , és az [Egyéni tartomány neve].
  *  Kapcsolat a helyszíni erőforrásoknak [VNet] [Hibrid kapcsolatok]mellett.
  *  Figyelés, és hibaelhárítási új Relic vagy az [Alkalmazás az összefüggéseket].
  *  Beépített DevOps szerszámok, beleértve a [helyek átmeneti tárolására], visszaállítási, és a gyártási vizsgálata.
  *  [Automatikus méretezése]terheléselosztás és [teljesítményét figyelve].

Azure alkalmazás szolgáltatás előnyeiről további információt az [Alkalmazás mobilszolgáltatási szolgáltatások összehasonlítása] témakörben talál.

## <a name="before-you-begin"></a>Első lépések

Mielőtt elkezdené fő munkáját a webhelyen, érdemes [biztonsági másolatot készíteni a mobilszolgáltatási] parancsfájlok és SQL-adatbázis.

## <a name="migrating-site"></a>A webhelyek áttelepítése

Az áttelepítési folyamatot telepít egy egyetlen Azure régión belüli összes webhely.

A webhely funkció vagy beállítás

  1.  Jelentkezzen be az [Azure klasszikus portálon].
  2.  Jelölje ki az áttelepíteni kívánt terület Mobile szolgáltatás.
  3.  Kattintson az **alkalmazás szolgáltatás áthelyezése** gombra.

    ![Az áttelepítés gomb][0]

  4.  Olvassa el a Migrate alkalmazás szolgáltatási párbeszédpanel.
  5.  A megfelelő mezőbe, adja meg a mobilszolgáltatási nevét.  Például ha tartománynevét contoso.azure-mobile.net, majd adja meg _a contoso_ a megfelelő mezőbe.
  6.  Kattintson a osztásjelek gombra.

Figyelje az áttelepítés a tevékenységfigyelő kifejezést az állapotát. A webhely *áttelepítése* az Azure klasszikus portálon szerepel.

  ![Áttelepítési tevékenységfelügyelő][1]

Minden áttelepítési készíthet bárhol a 3 / áttelepítendő mobilszolgáltatás 15 perccel.  A webhely továbbra is elérhető marad az áttelepítés során.
A webhely újraindítása végén található az áttelepítési folyamatot.  A webhely az újraindítás tarthatnak pár másodpercre folyamat során nem érhető el.

## <a name="finalizing-migration"></a>Az áttelepítés véglegesítése

Tervezze meg tesztelje a webhelyről a mobil ügyfélalkalmazásba befejezésekor az áttelepítési folyamatot.  Gondoskodjon arról, hogy a mobil ügyfélalkalmazásba módosítások nélkül az összes közös ügyfél műveletek hajthatók végre.  

### <a name="update-app-service-tier"></a>Jelölje ki a megfelelő alkalmazás szolgáltatásainak réteg árak

Ha nagyobb rugalmasság árak Azure alkalmazás szolgáltatás való áttelepítése után.

  1.  Jelentkezzen be az [Azure-portálon].
  2.  Jelölje ki **az összes erőforrás** vagy **Alkalmazás szolgáltatások** , majd kattintással jelölje ki azt az áttelepített mobilszolgáltatási.
  3.  Alapértelmezés szerint a beállítások lap nyílik meg.
  4.  Kattintson a beállítások menüben a **Alkalmazás szolgáltatás megtervezése** .
  5.  Kattintson a **Réteg árak** csempére.
  6.  Kattintson az egyéni igényeknek megfelelő csempét, majd **Jelölje ki**.  Előfordulhat, hogy kattintson az **összes megjelenítése** a rendelkezésre álló árak tiers.

Kiindulási pontként azt javasoljuk, a következő rétegek:

| Mobilszolgáltatás árak szint | Alkalmazás szolgáltatási árak réteg |
| :-------------------------- | :----------------------- |
| Ingyenes                        | Ingyenes F1                  |
| Egyszerű                       | B1 Basic                 |
| Normál                    | S1 Standard              |

Van nagymértékben rugalmassá kiválasztani a réteg az alkalmazás árak.  Olvassa el az [Alkalmazás szolgáltatás árak] teljes részletes tudnivalókat a az új alkalmazás szolgáltatás árak.

> [AZURE.TIP]Az alkalmazás szolgáltatás szabványos réteg a [helyek átmeneti tárolására], automatikus biztonsági mentést, beleértve és automatikus méretezés tartalmaz a sok olyan szolgáltatást használja, előfordulhat, hogy kívánt való hozzáférést.  Megtudhatja, hogy az új funkciók használata közben van.

### <a name="review-migration-scheduler-jobs"></a>Tekintse át az áttelepített ütemező feladatok

A Feladatütemező szolgáltatás nem lesz látható körülbelül 30 perccel az áttelepítés után.  Ütemezett feladat továbbra is futtathatók a háttérben.
Az ütemezett feladat megtekintése, miután láthatók újra:

  1.  Jelentkezzen be az [Azure-portálon].
  2.  Válassza a **Tallózás >**, írja be az **Ütemezés** _az mezőben_ , majd jelölje ki **A Feladatütemező csoportokat**.

Vannak ingyenes ütemező feladatok elérhető áttelepítés utáni korlátozott számú.  Tekintse át a használatát és [Azure ütemezőt tervek].

### <a name="configure-cors"></a>Ha szükséges, állítsa be CORS

Idegen származási erőforrások megosztása a engedélyezése egy webhelynek a másik tartományt a webes API eléréséhez az technika.  Ha Azure Mobile Services-társított webhelyhez használja, majd kell CORS konfigurálása az áttelepítés részeként.  Ha kizárólag mobileszközökről érik el Azure Mobile szolgáltatások, majd CORS nem kell ritkán konfigurálhatók kivételével.

A **MS_CrossDomainWhitelist** alkalmazás beállítás a áttelepített CORS beállítások érhetők el.  A webhely áttelepítése az alkalmazás szolgáltatás CORS létesítmény:

  1.  Jelentkezzen be az [Azure-portálon].
  2.  Jelölje ki **az összes erőforrás** vagy **Alkalmazás szolgáltatások** , majd kattintással jelölje ki azt az áttelepített mobilszolgáltatási.
  3.  Alapértelmezés szerint a beállítások lap nyílik meg.
  4.  Kattintson a **CORS** API menü.
  5.  Írja be a bármely engedélyezett forrásokból a megfelelő mezőbe, mindegyik után nyomja le az ENTER billentyűt.
  6.  Miután forrásokból engedélyezett a listájának megfelelő, kattintson a Mentés gombra.

> [AZURE.TIP]Az Azure alkalmazás szolgáltatás előnyei egyik futtathatja mobilszolgáltatás és a webhely ugyanazon a helyen.  További tudnivalókért lásd: a [következő lépésekkel](#next-steps) szakaszban.

### <a name="download-publish-profile"></a>Új közzétételi profil letöltése

A közzétételi profilt a webhely megváltozik, amikor az áttelepítés az Azure alkalmazás szolgáltatás.  Ha azt szeretné, a webhely a Visual Studio közzététele, új közzétételi profil szüksége.  Az új közzétételi profil letöltése:

  1.  Jelentkezzen be az [Azure-portálon].
  2.  Jelölje ki **az összes erőforrás** vagy **Alkalmazás szolgáltatások** , majd kattintással jelölje ki azt az áttelepített mobilszolgáltatási.
  3.  Kattintson a **Get-profil közzététele**.

A PublishSettings fájl letöltése a számítógépre.  A szokásos módon _sitename_nevezik. PublishSettings.  A közzététel-beállítások importálása a meglévő projektbe:

  1.  Nyissa meg a Visual Studio és Azure mobilszolgáltatási projektjét.
  2.  Kattintson a jobb gombbal a projekt az **Explorer megoldást** , és válassza a **Közzététel...**
  3.  Kattintson az **Importálás** gombra
  4.  Kattintson a **Tallózás gombra** , és válassza a letöltött fájl közzé.  Kattintson az **OK gombra**
  5.  Ahhoz, hogy a Közzététel beállításai munka a **Kapcsolat ellenőrzése** gombra.
  6.  Kattintson a **Közzététel** közzététele a webhelyen.


## <a name="working-with-your-site"></a>Az áttelepítés utáni webhely használata

Indítsa el az új alkalmazás szolgáltatás az áttelepítés utáni [Azure portál] használata.  Az alábbiakban néhány jegyzetek műveleteket hajtsa végre az [Azure klasszikus portál]alkalmazás szolgáltatás megfelelője együtt használva.

### <a name="publishing-your-site"></a>Le, és a közzétételi az áttelepített webhely

A webhely mely számjegy vagy FTP-keresztül érhető el, és a különböző különböző mechanizmusok, beleértve a WebDeploy, TFS, Mercurial, GitHub és FTP szolgálhat felvilágosítással.  A telepítési hitelesítő adatokat a többi a webhely áttelepítése.  Ha nem adta meg a telepítési hitelesítő adatait, vagy nem emlékszik őket, akkor kérhet új őket:

  1. Jelentkezzen be az [Azure-portálon].
  2. Jelölje ki **az összes erőforrás** vagy **Alkalmazás szolgáltatások** , majd kattintással jelölje ki azt az áttelepített mobilszolgáltatási.
  3. Alapértelmezés szerint a beállítások lap nyílik meg.
  4. Kattintson a **telepítés hitelesítő adatait** , kattintson a közzététel menü.
  5. A rendelkezésre álló mezőkbe írja be az új telepítési hitelesítő adatokat, majd kattintson a Mentés gombra.

A webhelyhez és mely számjegy klónozhatja vagy GitHub, TFS vagy Mercurial automatikus telepítések beállítása a hitelesítő adatokat is használhatja.  További tudnivalókért lásd: [Azure alkalmazás szolgáltatás üzembe helyezési dokumentációja].

### <a name="appsettings"></a>Az alkalmazásbeállítások

A legtöbb beállításainak áttelepített mobilszolgáltatás alkalmazás beállításainak érhetők el.  Az alkalmazás beállítások listája kaphat az [Azure-portálon].
Megjelenítése, és az alkalmazás beállításainak módosítása:

  1. Jelentkezzen be az [Azure-portálon].
  2. Jelölje ki **az összes erőforrás** vagy **Alkalmazás szolgáltatások** , majd kattintással jelölje ki azt az áttelepített mobilszolgáltatási.
  3. Alapértelmezés szerint a beállítások lap nyílik meg.
  4. Az általános menüjében kattintson az **alkalmazás beállításai** parancsra.
  5. Görgessen le az alkalmazás Settings szakaszig, és keresse meg az app beállítása.
  6. Kattintson az alkalmazás beállítás értékét a szerkesztendő értékét.  Kattintson a **Mentés** mentheti az értéket.

Egyszerre több alkalmazás beállításainak frissítheti.

> [AZURE.TIP]Vannak a két alkalmazás beállítása az azonos értékkel rendelkező.  Ha például _ApplicationKey_ jelenhetnek meg és _MS\_ApplicationKey_.  Egyszerre mindkét alkalmazás beállításainak frissítése

### <a name="authentication"></a>Hitelesítés

Az összes hitelesítési beállítások áttelepített webhelyéhez alkalmazás beállítások érhetők el.  Ha frissíteni szeretné a hitelesítési beállítások, meg kell változtatnia a megfelelő alkalmazás beállításait.  A következő táblázat mutatja a hitelesítési szolgáltató a megfelelő alkalmazás beállításait:

| Szolgáltató          | Ügyfél-azonosító                 | Ügyfél titkos kulcs                | Egyéb beállítások             |
| :---------------- | :------------------------ | :--------------------------- | :------------------------- |
| Microsoft-fiók | **MS\_MicrosoftClientID**  | **MS\_MicrosoftClientSecret** | **MS\_MicrosoftPackageSID** |
| Facebook          | **MS\_FacebookAppID**      | **MS\_FacebookAppSecret**     |                            |
| Twitter           | **MS\_TwitterConsumerKey** | **MS\_TwitterConsumerSecret** |                            |
| A Google            | **MS\_GoogleClientID**     | **MS\_GoogleClientSecret**    |                            |
| Azure Active Directory          | **MS\_AadClientID**        |                              | **MS\_AadTenants**          |

Megjegyzés: **MS\_AadTenants** bérlői tartományai (a "Engedélyezett bérlők" mezők a Mobile Services portál) vesszővel tagolt listáját tárolja.

> [AZURE.WARNING] **Ne használja a hitelesítési mechanizmusok a beállítások menüben**
>
> Azure alkalmazás szolgáltatása egy külön "kódmentes" hitelesítési és engedélyezési rendszer csoportban a _hitelesítési / engedélyezési_ beállítások menü és a (elavult) _Mobile hitelesítési_ lehetőséget a beállítások menüben.  A beállításokkal kompatibilisek áttelepített Azure Mobile szolgáltatás.  Azt is megteheti, hogy [a webhely frissítése](app-service-mobile-net-upgrading-from-mobile-services.md) az Azure alkalmazás szolgáltatás hitelesítési előnyeit.

### <a name="easytables"></a>Adatok

Az _adatok_ lap, a mobil szolgáltatások váltja _Egyszerűen táblázatokat_ belül az Azure-portálra.  Egyszerű táblák elérése:

  1. Jelentkezzen be az [Azure-portálon].
  2. Jelölje ki **az összes erőforrás** vagy **Alkalmazás szolgáltatások** , majd kattintással jelölje ki azt az áttelepített mobilszolgáltatási.
  3. Alapértelmezés szerint a beállítások lap nyílik meg.
  4. Kattintson a mobil menü **egyszerűen táblázatokat** .

Tábla hozzáadása kattintson a **Hozzáadás** gombra, vagy elérheti a táblázatok nevét, ha a meglévő tábla.  Vannak a különböző műveleteket végezheti el a lap, például:

* Táblázat engedélyek módosítása
* A műveleti parancsfájlok szerkesztése
* A táblázat séma kezelése
* A táblázat törlése
* A táblázatok tartalmának törlése
* A táblázat adott sorok törlése

### <a name="easyapis"></a>API

A mobil szolgáltatások _API_ lapjának váltja _Egyszerűen API -khoz_ belül az Azure-portálra.  Egyszerű API-khoz elérése:

  1. Jelentkezzen be az [Azure-portálon].
  2. Jelölje ki **az összes erőforrás** vagy **Alkalmazás szolgáltatások** , majd kattintással jelölje ki azt az áttelepített mobilszolgáltatási.
  3. Alapértelmezés szerint a beállítások lap nyílik meg.
  4. Kattintson a mobil menü **Egyszerűen API -khoz** .

A lap már jelennek meg az áttelepített API-khoz.  Ez a lap felvehet egy API-t is.  Egy adott API kezeléséhez kattintson a API-t.
Az új lap az módosíthatja az engedélyeket, és a parancsfájlok szerkesztése az API.

### <a name="on-demand-jobs"></a>A Feladatütemező szolgáltatás

Az összes ütemezőt feladatok keresztül az ütemező feladat gyűjtemények szakasz érhetők el.  A Feladatütemező feladatok elérése:

  1. Jelentkezzen be az [Azure-portálon].
  2. Válassza a **Tallózás >**, írja be az **Ütemezés** _az mezőben_ , majd jelölje ki **A Feladatütemező csoportokat**.
  3. Jelölje be a feladat gyűjtemény a webhelyen.  _Sitename_nevű-feladatokat.
  4. Kattintson a **Beállítások**gombra.
  5. Kattintson a **Feladatütemező projektek** kezelése csoportban.

Az áttelepítés előtt megadott gyakorisággal jelennek meg ütemezett feladatokat.  Igény szerinti feladatok le vannak tiltva.  Az igény szerinti feladat futtatása:

  1. Jelölje be a futtatni kívánt feladatot.
  2. Ha szükséges, kattintson a **engedélyezése** ahhoz, hogy a feladat.
  3. Kattintson a **Beállítások**, majd az **Ütemezés**gombra.
  4. Jelölje ki a megismétlődése **egyszer**, majd kattintson a **Mentés** gombra

Az igény szerinti feladatok olvassa el a `App_Data/config/scripts/scheduler post-migration`.  Azt javasoljuk, hogy vagy alakít át az összes igény szerinti feladat [WebJobs] [függvények].  Új ütemező feladatok [WebJobs] vagy [függvények]írni.

### <a name="notification-hubs"></a>Értesítés hubok

Mobil támogatási szolgálata értesítés hubok leküldéses értesítéseket.  A következő alkalmazás beállításainak használatával az értesítési-központban csatolása a mobilszolgáltatási az áttelepítés után:

| Alkalmazásbeállítással                    | Leírás                              |
| :------------------------------------- | :--------------------------------------- |
| **MS\_PushEntityNamespace**             | Az értesítés központi Namespace           |
| **MS\_NotificationHubName**             | Az értesítés központi neve                |
| **MS\_NotificationHubConnectionString** | Az értesítés központi kapcsolati karakterlánc   |
| **MS\_NamespaceName**                   | Alias MS_PushEntityNamespace      |

Az értesítési központi kezelik az [Azure portálon]keresztül.  Jegyezze fel, értesítés központi (találja meg az App beállításokkal):

  1. Jelentkezzen be az [Azure-portálon].
  2. Válassza a **Tallózás**>, válassza az **Értesítési hubok**
  3. Kattintson a mobil szolgáltatással kapcsolatos értesítési központi nevére.

> [AZURE.NOTE]Ha az értesítési központi "Vegyes" típus, még nem látható.  "Több" írja be a hubok csatlakozást értesítés hubok és a régi szolgáltatás Bus szolgáltatások értesítés.  [A vegyes névtér konvertálja] a továbblépés előtt.  A konvertálás befejezése után az értesítési központi az [Azure portál]jelenik meg.

További tudnivalókért tekintse át az [Értesítési hubok] dokumentáció.

> [AZURE.TIP]Értesítés hubok kezelése az [Azure portál] szolgáltatásai továbbra is a előzetes verzióban.  Az [Azure klasszikus portál] elérhető összes értesítés hubra kezelésére szolgáló marad.

### <a name="legacy-push"></a>Régi leküldéses beállításai

Leküldéses értesítést hubok a megjelenése előtt a mobilszolgáltatás konfigurált, _régebbi leküldéses_használata.  Ha nem látható a konfigurációban felsorolt értesítési hubon leküldéses használja, akkor valószínű _régebbi leküldéses_használja.  Ez a funkció van áttelepített összes egyéb funkcióival.  Jó helyen jár azt javasoljuk, hogy frissítsen értesítés hubok hamarosan az áttelepítés befejeződése után.

Időközben (az APN-tanúsítványt a főbb kivétel) régebbi leküldéses beállítások érhetők el az alkalmazás beállításai.  Frissítse az APN-tanúsítványt a megfelelő fájlt a fájlrendszer képre cserélésével.

### <a name="app-settings"></a>App egyéb beállításairól

A következő további alkalmazás beállításainak, hogy a Mobile szolgáltatásból áttelepített és elérhető *Beállítások*a > *Alkalmazás beállításainak*:

| Alkalmazásbeállítással              | Leírás                             |
| :------------------------------- | :-------------------------------------- |
| **MS\_MobileServiceName**         | Az alkalmazás neve                    |
| **MS\_MobileServiceDomainSuffix** | A tartomány előtagot. azaz Azure-mobile.net |
| **MS\_ApplicationKey**            | A helyi menüt megjelenítő billentyű                    |
| **MS\_főkulcsos**                 | Az alkalmazás fő billentyűt                     |

A helyi menüt megjelenítő billentyűt, és a fő kulcs megegyeznek az alkalmazás kulcsok a eredeti Mobile szolgáltatásból.  Különösen a helyi menüt megjelenítő billentyű küldi mobilügyfelek által a mobil API is használatuk érvényesítéséhez.

### <a name="cliequivalents"></a>Parancssori egymásnak megfelelő elemei

A _mobil azure_ paranccsal már az Azure Mobile Services hely kezelését.  Ehelyett számos függvény került a helyére az _azure webhely_ parancsot.  Az alábbi táblázat segítségével megkeresheti a egyenértékű funkcióiról a gyakori parancsok:

| _Azure Mobile_ Parancs                     | Egyenértékű _Azure webhely_ parancs            |
| :----------------------------------------- | :----------------------------------------- |
| mobil helyek                           | webhely helylistájában                         |
| mobil lista                                | webhelyen tárolt listában                                  |
| mobil Megjelenítés _neve_                         | Megjelenítés _neve_                           |
| mobil újraindítás _neve_                      | Indítsa újra _neve_                        |
| mobil újratelepítés _neve_                     | telepítési újratelepítés _commitId_ _neve_ |
| mobil kulcs _név_ _típusú_ _érték_ megadása       | webhely appsetting delete _billentyű_ _neve_ <br/> webhely appsetting _kulcs_hozzáadása=_azonosító_ _neve_ |
| mobil config lista _neve_                  | webhely appsetting lista _neve_                |
| mobil config _neve_ _kulcs_ beszerzése             | webhely appsetting _kulcs_ Megjelenítés _neve_          |
| mobil config _neve_ _kulcs_ beállítása             | webhely appsetting delete _billentyű_ _neve_ <br/> webhely appsetting _kulcs_hozzáadása=_azonosító_ _neve_ |
| mobil tartomány lista _neve_                  | tartomány lista _neve_                    |
| mobil tartomány _neve_ _tartomány_ hozzáadása          | webhely tartomány hozzáadása a _tartomány_ _neve_            |
| mobil tartomány Törlés _neve_                | tartomány Törlés _tartomány_ _neve_         |
| mobil skála Megjelenítés _neve_                   | Megjelenítés _neve_                           |
| mobil skála _nevének_ módosítása                 | webhely skála mód _mód_ _neve_ <br /> méretezés példányok _példányok_ _neve_ |
| mobil appsetting lista _neve_              | webhely appsetting lista _neve_                |
| mobil appsetting _név_ _kulcs_ _értékét_ hozzáadása | webhely appsetting _kulcs_hozzáadása=_azonosító_ _neve_   |
| mobil appsetting delete _neve_ _billentyűt_      | webhely appsetting delete _billentyű_ _neve_        |
| mobil appsetting megjelenítése _neve_ _billentyűt_        | webhely appsetting delete _billentyű_ _neve_        |

A megfelelő Alkalmazásbeállítással frissítésével hitelesítési vagy leküldéses értesítési beállítások módosítása
Fájlok szerkesztése és közzététele a webhelyen keresztül ftp vagy mely számjegy.

### <a name="diagnostics"></a>Diagnosztikai és naplózási

Diagnosztikai naplózás az Azure alkalmazás szolgáltatásainak általában le van tiltva.  Diagnosztikai naplózás engedélyezése:

  1. Jelentkezzen be az [Azure-portálon].
  2. Jelölje ki **az összes erőforrás** vagy **Alkalmazás szolgáltatások** , majd kattintással jelölje ki azt az áttelepített mobilszolgáltatási.
  3. Alapértelmezés szerint a beállítások lap nyílik meg.
  4. A szolgáltatások menüben válassza a **Diagnosztikai naplók** .
  5. **Kattintson a következő naplók** : **Alkalmazás naplózás (formázáshoz)**, a **hiba részletes üzenetek**és a **Hibás kérés nyomkövetés**
  6. Kattintson a **Fájlrendszerben** webkiszolgáló naplózásának
  7. Kattintson a **Mentés** gombra.

A naplók megtekintése:

  1. Jelentkezzen be az [Azure-portálon].
  2. Jelölje ki **az összes erőforrás** vagy **Alkalmazás szolgáltatások** , majd kattintással jelölje ki azt az áttelepített mobilszolgáltatási.
  3. Kattintson az **eszközök** gombra
  4. Jelölje ki a **napló adatfolyam** a OBSERVE menüben.

Naplók az ablak jelenik meg, a program hozza létre.  Is letöltheti a naplókat, újabb elemzéshez, a telepítési hitelesítő adataival. További információ a [naplózás] dokumentációjában.

## <a name="known-issues"></a>Ismert problémák

### <a name="deleting-a-migrated-mobile-app-clone-causes-a-site-outage"></a>Egy webhely üzemszünetek Mobile alkalmazást áttelepített átirattal törlése okoz

Ha az áttelepített mobilszolgáltatás Azure PowerShell használatával klónozhatja, majd törölje az adatfeliratsor, a termelési szolgáltatásbeli a DNS-bejegyzés törlődik.  A webhely már nem érhető el az internetről.  

Megoldás: Ha szeretné a webhely klónozhatja, tegye a portálon keresztül.

### <a name="changing-webconfig-does-not-work"></a>Web.config módosítása nem működik

Ha egy ASP.NET-webhelyre, módosításaira a `Web.config` fájl nem alkalmazza.  Az Azure alkalmazás szolgáltatás hoz létre a megfelelő `Web.config` támogató mobil futtatókörnyezetének rendszerindításkor fájl.  Bizonyos beállításokat (például az egyéni élőfejet) felülbírálhatja átalakítás XML-fájl használatával.  Hozzon létre egy fájlt az úgynevezett `applicationHost.xdt` – ezzel a fájllal kell végződnie a `D:\home\site` könyvtár az Azure szolgáltatása.  Töltse fel a `applicationHost.xdt` fájl keresztül egy egyéni telepítési parancsfájlt, vagy közvetlenül a Kudu használatával.  Az alábbi példában látható egy példa dokumentumot:

```
<?xml version="1.0" encoding="utf-8"?>
<configuration xmlns:xdt="http://schemas.microsoft.com/XML-Document-Transform">
  <system.webServer>
    <httpProtocol>
      <customHeaders>
        <add name="X-Frame-Options" value="DENY" xdt:Transform="Replace" />
        <remove name="X-Powered-By" xdt:Transform="Insert" />
      </customHeaders>
    </httpProtocol>
    <security>
      <requestFiltering removeServerHeader="true" xdt:Transform="SetAttributes(removeServerHeader)" />
    </security>
  </system.webServer>
</configuration>
```

További információ az [XDT átalakítása minták] dokumentációjában a GitHub.

### <a name="migrated-mobile-services-cannot-be-added-to-traffic-manager"></a>Áttelepített Mobile szolgáltatások nem adhatók hozzá a forgalom Manager

A forgalom Manager profilok létrehozásakor nem közvetlenül választhatja ki a profilhoz áttelepített mobilszolgáltatás.  Használja a "külső zárólap."  A külső végpont csak felvehetők Powershellen keresztül.  További tudnivalókért lásd: a [forgalom Manager oktatóprogram](https://azure.microsoft.com/blog/azure-traffic-manager-external-endpoints-and-weighted-round-robin-via-powershell/).

## <a name="next-steps"></a>Következő lépések

Most, hogy az alkalmazás áttelepítése alkalmazás szolgáltatás, vannak még több funkció használható:

  * Telepítési [helyek átmeneti] lehetővé teszi a szakaszban a webhely módosításait, és végezze el A és B tesztelése.
  * [WebJobs] adja meg az igény szerinti ütemezett feladat helyettesíti.
  * Azt is megteheti [folyamatosan telepítése] a webhelyen a webhely hivatkozással GitHub, TFS vagy Mercurial.
  * A webhely Lync [Alkalmazás háttérismeretek] is használhatja.
  * A webhely és a Mobile API ugyanazt kódot a szolgálnak.

### <a name="upgrading-your-site"></a>Azure Mobile alkalmazások SDK a mobil szolgáltatások webhely frissítése

  * Az új [Mobile alkalmazások Node.js SDK] Node.js-alapú kiszolgálón projektek, számos új szolgáltatást biztosít. Például most helyi fejlesztés és hibakeresés, 0,10 feletti bármely Node.js verzióját használja, és bármelyik Express.js köztes testre is.

  * A. NETTÓ-alapú kiszolgálón projektek, az új [Mobile alkalmazások SDK NuGet csomagok](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Server/) arról rugalmasabb NuGet függőségek.  Ezekről a csomagokról az új alkalmazás szolgáltatás hitelesítés támogatására, és bármelyik ASP.NET projekttel írása. További információk frissítése, olvassa el a [frissíteni a meglévő .NET mobilszolgáltatási alkalmazás szolgáltatás](app-service-mobile-net-upgrading-from-mobile-services.md)című témakört.

<!-- Images -->
[0]: ./media/app-service-mobile-migrating-from-mobile-services/migrate-to-app-service-button.PNG
[1]: ./media/app-service-mobile-migrating-from-mobile-services/migration-activity-monitor.png
[2]: ./media/app-service-mobile-migrating-from-mobile-services/triggering-job-with-postman.png

<!-- Links -->
[Árak alkalmazás szolgáltatás]: https://azure.microsoft.com/en-us/pricing/details/app-service/
[Alkalmazás Hírcsatornájában]: ../application-insights/app-insights-overview.md
[Automatikus méretezése]: ../app-service-web/web-sites-scale.md
[Azure alkalmazás szolgáltatás]: ../app-service/app-service-value-prop-what-is.md
[Azure alkalmazás szolgáltatás üzembe helyezési dokumentációja]: ../app-service-web/web-sites-deploy.md
[Azure klasszikus portál]: https://manage.windowsazure.com
[Azure portál]: https://portal.azure.com
[Azure Region]: https://azure.microsoft.com/en-us/regions/
[Azure ütemezőt csomagok]: ../scheduler/scheduler-plans-billing.md
[folyamatosan terjesztése]: ../app-service-web/app-service-continuous-deployment.md
[A vegyes névtér konvertálása]: https://azure.microsoft.com/en-us/blog/updates-from-notification-hubs-independent-nuget-installation-model-pmt-and-more/
[curl]: http://curl.haxx.se/
[Egyéni tartomány neve]: ../app-service-web/web-sites-custom-domain-name.md
[Fiddler]: http://www.telerik.com/fiddler
[Azure alkalmazás szolgáltatás általános elérhetősége]: https://azure.microsoft.com/blog/announcing-general-availability-of-app-service-mobile-apps/
[Hibrid kapcsolatok]: ../app-service-web/web-sites-hybrid-connection-get-started.md
[Naplózás]: ../app-service-web/web-sites-enable-diagnostic-log.md
[Mobilalkalmazások Node.js SDK]: https://github.com/azure/azure-mobile-apps-node
[Szolgáltatások összehasonlítása alkalmazás mobilszolgáltatás]: app-service-mobile-value-prop-migration-from-mobile-services.md
[Értesítés hubok]: ../notification-hubs/notification-hubs-push-notification-overview.md
[a teljesítmény figyelése]: ../app-service-web/web-sites-monitor.md
[Postman]: http://www.getpostman.com/
[Készítsen biztonsági másolatot a mobilszolgáltatás]: ../mobile-services/mobile-services-disaster-recovery.md
[átmeneti tárolásra szolgáló helyek]: ../app-service-web/web-sites-staged-publishing.md
[VNet]: ../app-service-web/web-sites-integrate-with-vnet.md
[WebJobs]: ../app-service-web/websites-webjobs-resources.md
[XDT átalakítás minták]: https://github.com/projectkudu/kudu/wiki/Xdt-transform-samples
[Függvények]: ../azure-functions/functions-overview.md
