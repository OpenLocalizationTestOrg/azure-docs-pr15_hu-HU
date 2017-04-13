<properties 
    pageTitle="Rugalmas adatbázis ügyfél-tár használata entitás keretrendszer |} Microsoft Azure" 
    description="Adatbázisok coding rugalmas adatbázis ügyfél és a szervezet keretrendszer használata" 
    services="sql-database" 
    documentationCenter="" 
    manager="jhubbard" 
    authors="torsteng" 
    editor=""/>

<tags 
    ms.service="sql-database" 
    ms.workload="sql-database" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="05/27/2016" 
    ms.author="torsteng"/>

# <a name="elastic-database-client-library-with-entity-framework"></a>Rugalmas adatbázis entitás keretrendszer ügyfél tárban 
 
Ehhez a dokumentumhoz a módosításokat a [rugalmas Adatbáziseszközök](sql-database-elastic-scale-introduction.md)integrálása szükséges entitás keretrendszer céljával jeleníti meg. A fókusz van szerkesztési [shard management megfeleltetése](sql-database-elastic-scale-shard-map-management.md) és entitás keretrendszer **Kód első** megoldása [adatok függő útválasztás](sql-database-elastic-scale-data-dependent-routing.md) . A [Kód első – új adatbázis](http://msdn.microsoft.com/data/jj193542.aspx) oktatóprogramja EF e dokumentumban futó példáinkban szolgál. Az ehhez a dokumentumhoz tartozó példakódot része rugalmas adatbázis eszközök állítsa be a Visual Studio mintakódok minták.
  
## <a name="downloading-and-running-the-sample-code"></a>Le, és a példakódot fut
Ez a cikk kódját letöltése:

* Visual Studio 2012 vagy újabb verziója szükség. 
* Indítsa el a Visual Studio. 
* A Visual Studióban-jelölje ki a fájl > új projektet. 
* Az "Új projekt" párbeszédpanelen keresse meg az **Online minták** a **Visual C#** és típus "rugalmas db" szót a Keresés mezőbe a jobb felső sarokban.
    
    ![Rugalmas adatbázis minta alkalmazás és a szervezet keretében][1] 

    Jelölje ki a minta **Rugalmas DB Azure SQL-entitás keretrendszer integrációs eszközök**című. A licenc elfogadása, után a minta betölti. 

A minta futtatásához kell három üres adatbázis létrehozása az Azure SQL-adatbázis:

* Shard térkép Manager-adatbázis
* Shard 1 adatbázis
* Shard 2 adatbázis

Miután létrehozott adatbázisok, töltse ki a hely tulajdonosainak **Program.cs** az Azure SQL-adatbázis-kiszolgáló neve, az adatbázis nevét, és a hitelesítő adatait a adatbázisokhoz való csatlakozáshoz. A megoldás a Visual Studio build. Visual Studio letölti a szükséges NuGet csomagok a rugalmas adatbázis ügyfél tár entitás keretrendszer és tranziens hibafa kezelése a beépített folyamat részeként. Győződjön meg arról, hogy NuGet csomagok visszaállítása engedélyezve van a megoldás. Ez a beállítás a megoldásfájlt a Visual Studio megoldás Intéző a jobb gombbal kattintva engedélyezheti. 

## <a name="entity-framework-workflows"></a>Személy keretrendszer munkafolyamatok 

Személy keretrendszer fejlesztők végre az alábbi négy munkafolyamatokat alkalmazásait és az alkalmazás objektumok adatmegőrzési biztosítása érdekében a támaszkodhat: 

* **Kód első (új adatbázis)**: A EF Fejlesztőeszközök hoz létre a modell az alkalmazás kódja és majd EF hoz létre az adatbázis belőle. 
* **Kód első (a meglévő adatbázis)**: A fejlesztői lehetővé teszi, hogy az alkalmazás-kód, a modell létrehozása egy meglévő adatbázis EF.
* **Az első modellt**: A fejlesztői hoz létre a modell a EF tervezőben és EF létrehozza az adatbázist a modellből.
* **Az első adatbázis**: A fejlesztő EF szerszámok előállítani alapján a modellben meglévő adatbázisból. 

E két megközelítés átlátszó kezelheti az adatbázis-kapcsolatokat, és az alkalmazás adatbázissémát DbContext osztály támaszkodhat. Szerint fog ölelik részletesebben a dokumentumot, a különböző szinteket rendelkezési jogot a kapcsolat létrehozása, engedélyezze a DbContext alap osztály különböző konstruktorait adatbázis rendszerindításáért és a séma létrehozását. Kihívásokkal kapcsolatban felmerülő elsősorban a arra, hogy az adatbázis-kapcsolat EF által biztosított kezelése és metszi egymást, az adatok függő útválasztási felületek kapcsolat kezelési lehetőségeit az a rugalmas adatbázis ügyfél Library. 

## <a name="elastic-database-tools-assumptions"></a>Rugalmas adatbázis eszközök feltételezések 

Kifejezés definíciókat [Rugalmas adatbázis eszközök szószedet](sql-database-elastic-scale-glossary.md)című témakör tartalmaz.

Rugalmas adatbázis ügyfél tárban az alkalmazás adatok nevű shardlets partíciót definiálhatók. Shardlets azonosítják egy sharding billentyűt, és bizonyos adatbázisok vannak megfeleltetve. Az alkalmazások valószínűleg tetszőleges számú adatbázist és terjesztése a shardlets elegendő kapacitása vagy a teljesítmény aktuális üzleti követelmények megadását. Az adatbázisok fő értékek sharding hozzárendelése a rugalmas adatbázis ügyfélprogram API-k által biztosított shard térkép tárolja. Ez a képesség **Shard Map-kezelés**vagy SMM rövid hívása. A shard térkép is figyelmezteti arra az adatbázis-kapcsolatokat végrehajtása egy sharding kulcs kérelmekhez közvetítő. Ez a lehetőség szerint **útválasztási adatok függő**hivatkozunk. 
 
A shard térkép kezelő felhasználók megakadályozza a shardlet adatokat, amely akkor fordulhat elő, amikor egyidejűleg shardlet adatkezelési műveletek (például az adatok áthelyezése egy shard a között) vannak történik, nem következetes nézetek. Ehhez a shard térképek kezeli ügyfél tár közvetítő az adatbázis-kapcsolatokat az alkalmazáshoz. Ezzel a shard térkép funkció automatikusan törlése egy adatbázis-kapcsolatot, amikor shard adatkezelési műveletek hatással lehetnek a shardlet, amely a kapcsolat a lettek létrehozva. Ezt a megközelítést kell néhány EF a szolgáltatások, például egy meglévő adatbázis létezésének ellenőrzése, hogy az új kapcsolatok létrehozása integrálása. Az általános a megfigyelés volt, hogy működik-e a szabványos DbContext konstruktorok csak munkahelyi biztos, hogy az, amely EF biztonságosan klónozható lezárt adatbázis-kapcsolatot. A tervezés elve rugalmas adatbázis helyette, hogy csak a megnyitott kapcsolatok ügynök. Egy előfordulhat, hogy tekintsen úgy, hogy az ügyfél Library előtt szeretné a EF DbContext átadását brokered kapcsolat bezárása előfordulhat, hogy a probléma megoldása. Azonban a kapcsolat bezárásával, majd nyissa meg újra a EF használna, egy foregoes az a tár érvényességi és következetességét elvégzett. Az áttelepítés funkciói EF, azonban ezeket a kapcsolatokat használja az alapul szolgáló adatbázissémát oly módon, hogy az alkalmazás átlátszó kezeléséhez. Ideális esetben azt szeretné megőrizni, és kapcsolja össze az összes e ezekre a lehetőségekre a rugalmas adatbázis ügyfél tár és a EF ugyanabban az alkalmazásban. Az alábbi részekből megtudhatja, ezek tulajdonságok és a részletes követelményeket. 


## <a name="requirements"></a>Követelmények 

A rugalmas adatbázis ügyfél tár és a szervezet keretrendszer API-k használata, ha azt szeretné őrizni a következő tulajdonságokat: 

* **Méretezési**: hozzáadásához vagy az adatbázisok eltávolítása a adatok réteg alkalmazásának sharded kapacitás igényeivel szükség szerint az alkalmazás. Ez azt jelenti, hogy vezérlőelem fölé a manager API-k kezelése az adatbázisok és shardlets hozzárendeléseket feleltesse meg a létrehozásakor vagy törlésekor az adatbázisok és a rugalmas adatbázis shard használatával. 

* **Egységesebb**: az alkalmazás sharding alkalmaz, és használja az ügyfél tár adatok függő útválasztási képességeit. Sérült vagy a nem megfelelő lekérdezési eredmények elkerülése érdekében a kapcsolatok brokered vannak a shard térkép kezelő keresztül. Ez is megtartja érvényességi és következetességét.
 
* **Az első kódot**: megőrzése érdekében az EF a kód első mintákat kényelme mellett használják. Az első kódot az alkalmazás az osztályok vannak megfeleltetve átlátszó az alapul szolgáló adatbázis struktúrák. Az alkalmazás kódot, amely a legtöbb jellemzőjét részt az alapul szolgáló adatbázis feldolgozása maszk DbSets kommunikáljon.
 
* **Séma**: entitás keretrendszer kezeli a kezdeti adatbázis séma létrehozását és a későbbi séma fejlesztés áttelepítések keresztül. Ezek a funkciók megőrzése, által kiigazítása az alkalmazás megegyezik könnyen követi az adatokat. 

Az alábbi útmutatás utasítja, hogy miként ezeknek a követelményeknek rugalmas Adatbáziseszközök kód első alkalmazásokhoz. 

## <a name="data-dependent-routing-using-ef-dbcontext"></a>Adatok függő útválasztás EF DbContext használata 

Az adatbázis-kapcsolatokat entitás keretrendszerrel általában kezelhetők **DbContext**alosztályai keresztül. Létrehozhat e alosztályok **DbContext**eredő. Az Itt adhatja meg a **DbSets** megvalósítása CLR-objektumok, az alkalmazás az adatbázis biztonsági csoportokat. Függő útválasztási adatok környezetében azonosítjuk is, amelyek nem feltétlenül rendelkeznek más EF kód első alkalmazás felhasználási területei számos hasznos tulajdonságok: 

* Az adatbázis már létezik, és van regisztrálva a rugalmas adatbázis shard térképen. 
* A séma az alkalmazás már telepítve van az adatbázisban (az alábbiakban ismertetett). 
* Az adatbázis-kapcsolatot útválasztási adatok függő a shard térkép szerint vannak brokered. 

**DbContexts** integrálása a méretezési útválasztási adatok függő:

1. Fizikai az adatbázis-kapcsolatokat az rugalmas adatbázis ügyfél felületeken keresztül a shard térkép vezető létrehozása 
2. A kapcsolatot a **DbContext** alosztályra tördelése
3. A kapcsolat átadásakor a **DbContext** alap osztályok annak érdekében, hogy a EF oldalon összes feldolgozása, valamint történik. 

A következő példa bemutatja ezt a megközelítést. (Ez a kód egyben a mellékelt Visual Studio projekt)

    public class ElasticScaleContext<T> : DbContext
    {
    public DbSet<Blog> Blogs { get; set; }
    …

        // C'tor for data dependent routing. This call will open a validated connection 
        // routed to the proper shard by the shard map manager. 
        // Note that the base class c'tor call will fail for an open connection
        // if migrations need to be done and SQL credentials are used. This is the reason for the 
        // separation of c'tors into the data-dependent routing case (this c'tor) and the internal c'tor for new shards.
        public ElasticScaleContext(ShardMap shardMap, T shardingKey, string connectionStr)
            : base(CreateDDRConnection(shardMap, shardingKey, connectionStr), 
            true /* contextOwnsConnection */)
        {
        }

        // Only static methods are allowed in calls into base class c'tors.
        private static DbConnection CreateDDRConnection(
        ShardMap shardMap, 
        T shardingKey, 
        string connectionStr)
        {
            // No initialization
            Database.SetInitializer<ElasticScaleContext<T>>(null);

            // Ask shard map to broker a validated connection for the given key
            SqlConnection conn = shardMap.OpenConnectionForKey<T>
                                (shardingKey, connectionStr, ConnectionOptions.Validate);
            return conn;
        }    

## <a name="main-points"></a>Főbb pontjairól
* Új konstruktorban felváltja az alapértelmezett konstruktor a DbContext alosztály 
* Az új konstruktort hajtja végre a szükséges adatok függő útválasztás keresztül rugalmas adatbázis ügyfél tár argumentumokat:
    * az adatok függő útválasztási felületek eléréséhez a shard térkép
    * a sharding billentyűt a shardlet, azonosítása
    * kapcsolati karakterlánc a adatok függő útválasztási kapcsolatot a shard hitelesítő adataival. 
 
* Alap osztálykonstruktor hívás veszi egy detour egy statikus módszer, amelyet lépéseinek végrehajtása szükséges adatokat függő útválasztás. 
   * Egy megnyitott kapcsolatot létesíteni a shard térképen használ a OpenConnectionForKey hívás rugalmas adatbázis ügyfél kapcsolódási.
   * A shard térkép a nyissa meg a shard, amely tartalmazza az adott sharding billentyű a shardlet kapcsolatot hoz létre.
   * A megnyitott kapcsolat vissza DbContext jelzi, hogy a kapcsolat EF automatikus létrehozása új kapcsolatot EF engedélyezem helyett használható az alap osztálykonstruktor átadott. Ezzel a módszerrel a kapcsolat van megjelölt által a rugalmas adatbázis ügyfélprogram API, hogy azt is garantálni konzisztencia shard térkép adatkezelési műveletek keretében.
 
  
Az új konstruktort használja az alapértelmezett konstruktor a kód helyett a DbContext alosztály. Lássunk egy példát: 

    // Create and save a new blog.

    Console.Write("Enter a name for a new blog: "); 
    var name = Console.ReadLine(); 

    using (var db = new ElasticScaleContext<int>( 
                            sharding.ShardMap,  
                            tenantId1,  
                            connStrBldr.ConnectionString)) 
    { 
        var blog = new Blog { Name = name }; 
        db.Blogs.Add(blog); 
        db.SaveChanges(); 

        // Display all Blogs for tenant 1 
        var query = from b in db.Blogs 
                    orderby b.Name 
                    select b; 
     … 
    }

Az új konstruktort megnyílik a kapcsolatot a shard, amely a **tenantid1**értékének jelölt shardlet adatait tartalmazza. A kód **használatával** Tiltás maradjon a **DbSet** EF használni a shard a **tenantid1**blogokhoz eléréséhez nem változik. Ekkor a kód a használata szemantikáját letiltása, hogy az adatbázis-műveletek most az adott **tenantid1** legyen az egy shard korlátozódik. Például a blogokhoz **DbSet** fölé LINQ lekérdezés csak adja vissza az aktuális shard-on tárolt blogok, de nem a más shards-on tárolt lehetőségekből.  

#### <a name="transient-faults-handling"></a>Ideiglenes (tranziens) hibák kezelése
A Microsoft Patterns és eljárások csapatának [Az ideiglenes (tranziens) hiba kezelésének alkalmazás blokkokból álló](https://msdn.microsoft.com/library/dn440719.aspx)közzé. A tár skála rugalmas ügyfél tárral EF együtt használatos. Jó helyen jár győződjön meg arról, hogy minden olyan tranziens kivétel egy pontjára, ahol azt is győződjön meg arról, hogy az új konstruktort használja ideiglenes (tranziens) hiba után, hogy a konstruktorok, hogy rendelkezik tweaked minden új kapcsolódási kísérletre lett adja eredményül. Egyéb esetben a megfelelő shard kapcsolat nem biztos, illetve hogy nincs kapcsolat karbantartása megváltozik a shard térkép biztosítékai. 

A következő kódot minta bemutatja, hogy egy SQL újrapróbálkozási házirend hogyan használható az új **DbContext** alosztály konstruktorok körül: 

    SqlDatabaseUtils.SqlRetryPolicy.ExecuteAction(() => 
    { 
        using (var db = new ElasticScaleContext<int>( 
                                sharding.ShardMap,  
                                tenantId1,  
                                connStrBldr.ConnectionString)) 
            { 
                    var blog = new Blog { Name = name }; 
                    db.Blogs.Add(blog); 
                    db.SaveChanges(); 
            … 
            } 
        }); 

**SqlDatabaseUtils.SqlRetryPolicy** a kódban definíciója egy **SqlDatabaseTransientErrorDetectionStrategy** a 10 és 5 másodperc várakozás újrapróbálkozások között eltelt idő kísérletek számát. Ezt a megközelítést hasonlít az útmutató EF és a felhasználó által kezdeményezett tranzakciók (lásd: [a végrehajtási stratégiák újbóli próbálkozás (EF6-től) korlátozások](http://msdn.microsoft.com/data/dn307226). Mindkét helyzetekben kell, hogy az alkalmazás szabályozza a hatókör a tranziens kivételt adja vissza: Nyissa meg a tranzakció vagy (látható módon) hozza létre újból a környezet a megfelelő konstruktorának használó a rugalmas adatbázis ügyfél tárat.

Szabályozhatja, hogy hol tranziens kivételek eltarthat, amíg újra a hatókör kell is kizárja a beépített **SqlAzureExecutionStrategy** EF épített felhasználása. **SqlAzureExecutionStrategy** volna nyissa meg újra a kapcsolat, de nem használja **OpenConnectionForKey** , és ezért átlépése az érvényesítési, amely a **OpenConnectionForKey** hívás részeként történik. Helyette a kód minta használja a beépített **DefaultExecutionStrategy** is részét képező EF. **SqlAzureExecutionStrategy**, nem pedig megfelelően működik együtt az ideiglenes (tranziens) hiba kezelésének a újrapróbálkozási házirend. A végrehajtás házirend értéke a **ElasticScaleDbConfiguration** osztály. Figyelje meg, hogy azt úgy döntött, nem használni a **DefaultSqlExecutionStrategy** , mert azt sugallja, **SqlAzureExecutionStrategy** használata, ha a tranziens kivételek előfordulásainak - vezessenek megfelelő viselkedést tárgyalt. A különböző újrapróbálkozási házirendek és EF további tudnivalókért lásd: [A EF kapcsolat Tűrőképessége](http://msdn.microsoft.com/data/dn456835.aspx).     

#### <a name="constructor-rewrites"></a>A felülírja a konstruktor
A fenti kód példák bemutatják, hogy az alapértelmezett konstruktor újra beírja az alkalmazás függő a szervezet keretrendszerrel útválasztási adatok használatához szükséges. Az alábbi táblázatban a többi konstruktorok megközelítése generalizes. 


Aktuális konstruktor  | Az adatok Átdolgozta konstruktor | Alap konstruktor | Jegyzetek
---------- | ----------- | ------------|----------
MyContext() |ElasticScaleContext (ShardMap, TKey) |DbContext (DbConnection logikai) |A kapcsolat kell lennie a shard térkép és az adatok függő útválasztási kulcs függvényt. Azt automatikus kapcsolat létrehozásának EF szükség, és helyette használhatja shard broker a kapcsolatot. 
MyContext(string)|ElasticScaleContext (ShardMap, TKey) |DbContext (DbConnection logikai) |A kapcsolatot, akkor a függvény a shard térkép és az adatok függő útválasztási billentyűt. Rögzített adatbázis neve vagy a csatlakozási karakterlánc nem működnek, ahogy azt érvényességi által a shard térképen. 
MyContext(DbCompiledModel) |ElasticScaleContext (ShardMap, TKey, DbCompiledModel) |DbContext (DbConnection, DbCompiledModel, logikai) |A kapcsolat első létrejön az adott shard térkép és sharding billentyű és a modellt, feltéve. A lefordított modell átkerülnek az alap c'tor.
MyContext (DbConnection logikai) |ElasticScaleContext (ShardMap, TKey, logikai) |DbContext (DbConnection logikai) |A kapcsolat kell a shard térképen és a kulcsot kell következtetett. Azt nem adhatók meg (kivéve, ha a bevitel már használta a shard térkép a kulcsot). A logikai érték fog továbbítja. 
MyContext (karakterlánc, DbCompiledModel) |ElasticScaleContext (ShardMap, TKey, DbCompiledModel) |DbContext (DbConnection, DbCompiledModel, logikai) |A kapcsolat kell a shard térképen és a kulcsot kell következtetett. Azt nem adhatók meg (kivéve, ha a bevitel használta a shard térkép a kulcsot). A lefordított modell fog továbbítja. 
MyContext (ObjectContext logikai) |ElasticScaleContext (ShardMap TKey, ObjectContext, logikai) |DbContext (ObjectContext logikai) |Az új konstruktort kell győződjön meg arról, hogy a egy bemenetként továbbított ObjectContext minden kapcsolat skála rugalmas kezeli kapcsolatra átirányítását. ObjectContexts részletes információt a jelen dokumentum túlra van.
MyContext (DbConnection, DbCompiledModel, logikai) |ElasticScaleContext (ShardMap TKey, DbCompiledModel, logikai)| DbContext (DbConnection, DbCompiledModel, logikai); |A kapcsolat kell a shard térképen és a kulcsot kell következtetett. A kapcsolat nem meg kell adni egy beviteli (kivéve, ha a bevitel már használta a shard térkép a kulcsot). Az adatmodelleket és a logikai átadni viszonyítási osztálykonstruktor. 

## <a name="shard-schema-deployment-through-ef-migrations"></a>Shard séma telepítési keresztül EF áttelepítés 

Az automatikus séma kezelése a szervezet keretében által biztosított könnyebb. Az alkalmazások használatával rugalmas Adatbáziseszközök környezetben szeretnénk őrizni automatikusan hozhatók létre az újonnan létrehozott shards séma adatbázisok sharded alkalmazás felvétele után ezt a lehetőséget. Az elsődleges használatieset, az adatok réteg minőségben EF sharded alkalmazásokhoz növeléséhez. Az adatbázis felügyeleti munkamennyiség séma kezelése a EF funkciókhoz használna csökkenti a EF épül egy sharded alkalmazással. 

Séma telepítési keresztül EF áttelepítések leghatékonyabb **olvasatlan kapcsolatok**. Ez az alkalmazással szemben az alkalmazási példát, az adatok függő útválasztás, amely a kapcsolat megnyitva a rugalmas adatbázis ügyfélprogram API által biztosított támaszkodik. Egy másik különbség konzisztencia kötelező: szükséges az összes adat-függő útválasztási kapcsolatot egyidejű shard map kezelő vírusvédelmet összhangot, miközben még nem egy új adatbázishoz, amely még nem regisztrált a shard térképen, és tartsa lenyomva az ujját shardlets lefoglalt még nem rendelkezik a kezdeti séma telepítés problémát. Azt ezért számíthat a felhasználási területei, nem pedig a útválasztási adatok függő normál az adatbázis-kapcsolatokat.  

Mindezeket megközelítés hol séma telepítési keresztül EF áttelepítések szorosan kapcsolódik ahhoz az új adatbázis bejegyzését, mint egy shard az alkalmazás shard térkép. Ez az alábbi előfeltételek alapul: 

* Már létezik az adatbázist. 
* Az adatbázis üres – tart fenn, nincs felhasználói séma és a felhasználói adatok nélkül.
* Az adatbázis nem még érhető el a rugalmas adatbázis ügyfélprogram API-khoz keresztül adatok függő útválasztás. 

Az alábbi előfeltételek helyen létrehozhatunk egy normál nem megnyitott **SqlConnection** EF áttelepítések séma telepítés elindításához. A következő kódot minta, amely bemutatja ezt a módszert. 

        // Enter a new shard - i.e. an empty database - to the shard map, allocate a first tenant to it  
        // and kick off EF intialization of the database to deploy schema 

        public void RegisterNewShard(string server, string database, string connStr, int key) 
        { 

            Shard shard = this.ShardMap.CreateShard(new ShardLocation(server, database)); 

            SqlConnectionStringBuilder connStrBldr = new SqlConnectionStringBuilder(connStr); 
            connStrBldr.DataSource = server; 
            connStrBldr.InitialCatalog = database; 

            // Go into a DbContext to trigger migrations and schema deployment for the new shard. 
            // This requires an un-opened connection. 
            using (var db = new ElasticScaleContext<int>(connStrBldr.ConnectionString)) 
            { 
                // Run a query to engage EF migrations 
                (from b in db.Blogs 
                    select b).Count(); 
            } 

            // Register the mapping of the tenant to the shard in the shard map. 
            // After this step, data-dependent routing on the shard map can be used 

            this.ShardMap.CreatePointMapping(key, shard); 
        } 
 

Ez a példa bemutatja a módszerrel **RegisterNewShard** regisztrálja a shard shard térképen, üzembe helyezése a séma EF áttelepítések keresztül, és egy sharding billentyűt a shard megfeleltetésének tárolja. Megnyitja a kapcsolati karakterlánc SQL bemeneteként **DbContext** alosztály (a minta**ElasticScaleContext** .) a konstruktorban támaszkodik. Ez a konstruktor kódját az egyenes továbbítás, ahogy az alábbi példában látható: 


        // C'tor to deploy schema and migrations to a new shard 
        protected internal ElasticScaleContext(string connectionString) 
            : base(SetInitializerForConnection(connectionString)) 
        { 
        } 

        // Only static methods are allowed in calls into base class c'tors 
        private static string SetInitializerForConnection(string connnectionString) 
        { 
            // We want existence checks so that the schema can get deployed 
            Database.SetInitializer<ElasticScaleContext<T>>( 
        new CreateDatabaseIfNotExists<ElasticScaleContext<T>>()); 

            return connnectionString; 
        } 
 
Előfordulhat, hogy egy használt öröklik az alap osztály konstruktorának verzióját. De a kódot kell biztosítására, hogy az alapértelmezett inicializálója az EF csatlakozáskor használják-e. Ezért a rövid detour be a statikus módszer – tárcsáz be a kapcsolati karakterláncot az alap osztálykonstruktor előtt. Figyelje meg, hogy egy másik alkalmazás tartományban vagy a folyamat annak érdekében, hogy nem ütköznek: EF inicializálója beállításainak shards nyilvántartása fusson. 


## <a name="limitations"></a>Korlátozások 

Az ehhez a dokumentumhoz a ismertetett eljárások korlátozások, néhány foglalja magában: 

* **LocalDb** használó EF alkalmazások először kell normál SQL Server-adatbázis áttelepítése az ügyfél-tár rugalmas adatbázis használata előtt. Ki az alkalmazások között sharding skála rugalmas a méretezés esetén nem lehet a **LocalDb**. Figyelje meg, hogy fejlesztési továbbra is használhatók lesznek **LocalDb**. 

* Adatbázis sémája módosítások szemléltetnek az alkalmazás módosításainak kell lévő összes shards EF áttelepítések folyamatát. A minta kódot a dokumentum nem szemléltetik ehhez. Fontolja meg az adatbázis frissítése ConnectionString paraméterrel az ismétlés összes shards; a T-SQL nyelvben parancsfájl használatával adatbázis frissítése függőben áttelepítésre kibontása vagy a – forgatókönyvvel lehetőséget, és a T-SQL nyelvben parancsfájl alkalmazása a shards.  

* Adott kérést, feltételezett értéke a sharding billentyűt a kérelem által biztosított által meghatározott összes az adatbázis feldolgozása van tartalmaz egy egyetlen shard belül. Azonban a feltételezésen nem mindig tartsa igaz. Ha például ha még nem lehetséges, hogy egy sharding kulcs elérhetővé. Ez az a cím, az ügyfél-tár biztosít a **MultiShardQuery** osztály, amely a kapcsolat hardverabsztrakciós felett több shards lekérdezésére. A dokumentum túlra van EF együtt a a **MultiShardQuery** használatának elsajátítása

## <a name="conclusion"></a>Elfogadásáról

Című cikk lépéseit a dokumentumban, keresztül EF alkalmazások adatok függő útválasztás által használt EF alkalmazásban **DbContext** alosztályát konstruktorok újrabontása használja a rugalmas adatbázis ügyfél tár szolgáltatását. Ezzel a korlátozza a e helyek, ahol már létezik **DbContext** osztályok szükséges módosításokat. Ezeken kívül EF alkalmazások továbbra is indítható el a szükséges EF áttelepítések a regisztráció az új shards és a hozzárendelések shard térképen lépések kombinálva automatikus séma telepítési összekapcsolhatók. 


[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-scale-use-entity-framework-applications-visual-studio/sample.png
 