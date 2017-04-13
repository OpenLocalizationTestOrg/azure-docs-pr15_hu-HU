<properties 
    pageTitle="Operációs rendszer funkciók Azure alkalmazás szolgáltatása" 
    description="További tudnivalók a OS web Apps alkalmazások, a mobilalkalmazásban háttérkiszolgálókon és az API alkalmazások Azure alkalmazás szolgáltatásokhoz elérhető funkciókat:" 
    services="app-service" 
    documentationCenter="" 
    authors="cephalin" 
    manager="wpickett" 
    editor="mollybos"/>

<tags 
    ms.service="app-service" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="07/01/2016" 
    ms.author="cephalin"/>

# <a name="operating-system-functionality-on-azure-app-service"></a>Operációs rendszer funkciók Azure alkalmazás szolgáltatása #

Ez a cikk ismerteti a gyakran használt eredeti operációs rendszer szolgáltatásait elérhető az összes alkalmazás [Azure alkalmazás szolgáltatást](http://go.microsoft.com/fwlink/?LinkId=529714)futtató. Ezt a funkciót tartalmaz a fájlt, hálózati, és a beállításjegyzék-hozzáférés és a diagnosztikai naplók és eseményeket. 

<a id="tiers"></a>
## <a name="app-service-plan-tiers"></a>Alkalmazás szolgáltatási terv rétegek

Alkalmazás szolgáltatás fut a több elem bérlői üzemeltetési környezet ügyfél alkalmazások. Az **ingyenes** , **megosztott** rétegek telepített alkalmazások dolgozó folyamatokban megosztott virtuális gépeken közben futtatni telepítését a **normál** és **prémium** rétegek az alkalmazások virtuális gép(ek) dedikált kifejezetten az egyetlen ügyfél társított alkalmazások futtatását.

Alkalmazás szolgáltatás lehetővé teszi, a zökkenőmentes méretezési használhatóság különböző rétegek között, mert a biztonsági beállítások vannak érvényben-szolgáltatási alkalmazás alkalmazások változatlan marad. Ezzel biztosíthatja, hogy alkalmazások nem váratlanul korábbi verziókban eltérően működnek, nem várt módon sikertelen, amikor az alkalmazás szolgáltatáscsomagja vált az egy réteg között.

<a id="developmentframeworks"></a>
## <a name="development-frameworks"></a>Fejlesztési keretek

Alkalmazás szolgáltatási árak rétegek szabályozhatják számítási rendelkezésre álló erőforrások (Processzor, lemezterülettel, memória és hálózati kilépési) alkalmazás között. Alkalmazások keretrendszer funkciót szélessége azonban a méretezési rétegek függetlenül változatlan marad.

Alkalmazás szolgáltatás lehetővé teszi a fejlesztés keretek, például a ASP.NET, a klasszikus ASP, node.js, PHP és Python –, ami futtatása, bővítmények belül az IIS számos. Egyszerűsítése és a biztonsági beállítások optimalizálása alkalmazás szolgáltatás alkalmazások általában futtassa a különböző fejlesztési keretek saját alapértelmezett beállításait. Alkalmazások beállítása egy megközelítése lehetett volna API-felület és funkciók minden egyes fejlesztési keretrendszer testreszabásához. Alkalmazás inkább szolgáltatásnak a több általános megközelítés, mivel a operációs rendszer használható funkciók körét, függetlenül attól, az alkalmazás fejlesztési keretrendszer közös alapterv.

Az alábbiakban összefoglaljuk az operációs rendszer funkció elérhető App szolgáltatás alkalmazások általános típusú.

<a id="FileAccess"></a>
##<a name="file-access"></a>Elérése

Különböző meghajtók alkalmazás szolgáltatás, beleértve a helyi meghajtón, és a hálózati meghajtók tartalmaz.

<a id="LocalDrives"></a>
### <a name="local-drives"></a>Helyi lévő

A saját a fő alkalmazás szolgáltatás az Azure PaaS (platform szolgáltatásként) infrastruktúra fölött futó szolgáltatás. Eredményt adja a helyi meghajtón, "csatolt" virtuális gép az azonos meghajtó típusok bármely Azure-ban futó dolgozó szerepkörhöz. Ide tartoznak egy operációs rendszer meghajtó (D:\ meghajtó), az alkalmazás meghajtó, amely tartalmazza az Azure-csomag cspkg fájlok kizárólag alkalmazás szolgáltatás által használt (és az ügyfelek nem érhető el) és a "felhasználó" meghajtó (C:\ meghajtó), amelyek mérete a virtuális méretétől függően változik.

<a id="NetworkDrives"></a>
### <a name="network-drives-aka-unc-shares"></a>Hálózati meghajtók (más néven UNC megosztja)

Az alkalmazás szolgáltatás, amely megkönnyíti a alkalmazások telepítésének és karbantartás egyszerű egyedi tulajdonságát egyik, hogy minden felhasználó tartalom UNC megosztások halmazának tárolja. A modell nagyon megfelelően legyenek rendezve rendel a közös minta szolgáltatója környezetek több terheléselosztás kiszolgálóval rendelkezik a helyszíni webhely webhelytartalom tárhelyet. 

Alkalmazás szolgáltatás belül is meg vannak létrehozott minden egyes adatközpont UNC megosztások száma. Minden egyes adatközpont vevők felhasználói tartalom százalékos felosztott minden UNC megosztani. Ezenkívül minden tartalom számára egy egyetlen ügyfél előfizetésének mindig helyezett azonos UNC fájl megosztása. 

Figyelje meg, hogy hogyan cloud services munka, miatt egy megosztott UNC szolgáltatója felelős adott virtuális gép fog időről időre változnak. A garantált, hogy szerint különböző virtuális gépeken futó UNC megosztások fog csatlakoztatott, ahogy azok a normál felhő műveletek során felfelé és lefelé viszik. Emiatt az alkalmazások soha nem kell tennie, hogy a gép adatai UNC elérési út az adott idő alatt stabil marad csomagolásukkor feltételezések. Ehelyett használják a kényelmes *ál* abszolút elérési út **D:\home\site** alkalmazás szolgáltatás. Az ál abszolút elérési út a saját alkalmazás célalkalmazásnak hordozható és az alkalmazás-és-felhasználó-felismerése nélkül módszert kínál. **D:\home\site**használatával egy átviheti a megosztott fájlok alkalmazás alkalmazás anélkül, hogy állítsa be az új abszolút elérési út minden.

<a id="TypesOfFileAccess"></a>
### <a name="types-of-file-access-granted-to-an-app"></a>Fájl access alkalmazás nyújtott típusai

Minden egyes ügyfél előfizetésének szerkezete? fenntartott címtár egy adott UNC megosztott egy adatközpont belül. Előfordulhat, hogy az ügyfél megosztása több alkalmazások létrehozott egy meghatározott adatokat központon belül, így a könyvtárak egyetlen ügyfél-előfizetés tartozó összes azonos UNC jönnek létre. A megosztás könyvtárak, például a tartalmat, hiba és a diagnosztikai naplók és az adatforrás-vezérlő által létrehozott alkalmazás korábbi verziói lehetnek. Megfelelően működjön, egy ügyfél alkalmazás könyvtárak érhetők el az olvasási és írási hozzáféréssel a alkalmazás alkalmazás kóddal futásidőben.

A virtuális gépen futó alkalmazás csatolt helyi meghajtón alkalmazás szolgáltatás fenntartja területet az alkalmazás-specifikus ideiglenes helyi tároló C:\ meghajtón adattömb. Bár az alkalmazás saját ideiglenes helyi tároló teljes olvasási/írási hozzáféréssel rendelkezik, tárolási valójában nem célja, hogy közvetlenül az alkalmazás kódja való használatra. Ehelyett a cél, hogy a szükséges ideiglenes fájltároló az IIS és a webes alkalmazás keretek. Alkalmazás szolgáltatás is korlátozza ideiglenes helyi lemezterület minden alkalmazáshoz, és megakadályozhatja, hogy az egyes alkalmazások használata a helyi fájltárolás túl nagy mennyiségű más.

Két példa arra, hogyan alkalmazás szolgáltatás az ideiglenes helyi tároló használja ideiglenes ASP.NET-fájlok a könyvtár és az IIS Directoryban tömörített fájlok. A ASP.NET fordítási rendszer a "Ideiglenes fájlokat ASP.NET" könyvtár használja a gyorsítótár ideiglenes fordítási helyként. IIS használja a "IIS ideiglenes tömörített" könyvtár kimeneti tömörített választ tárolni. A fájl használatát (, valamint mások) az ilyen típusú vannak újra társítani App szolgáltatásban az alkalmazás átmeneti helyi tárolóba. A újbóli hozzárendelés biztosíthatja, hogy funkció továbbra is a várt módon működik.

Minden alkalmazás alkalmazás szolgáltatás fut a "alkalmazáskészlet-identitásának", továbbá itt leírt nevű véletlenszerűen egyedi alacsony szintű jogosultsággal rendelkező munkafolyamat identitásának: [http://www.iis.net/learn/manage/configuring-security/application-pool-identities](http://www.iis.net/learn/manage/configuring-security/application-pool-identities). Alkalmazás kódja használja az identitás alapvető csak olvasható hozzáféréssel az operációs rendszer meghajtóra (D:\ meghajtó). Ez azt jelenti, hogy alkalmazás kódot közös címtár struktúrák lista és olvassa el a gyakori operációs rendszer meghajtón. Bár ezzel tűnhet kiépítése, ha elérhetők az access, az azonos könyvtárak és fájlok némileg széles körben a dolgozó szerepkör az Azure-ban tárolt szolgáltatás, és olvassa el a meghajtó tartalmát. 

<a name="multipleinstances"></a>
### <a name="file-access-across-multiple-instances"></a>Több példányon keresztül elérése

Az otthoni címtár-alkalmazás tartalommal, és az alkalmazás kódja tud írni. Ha az alkalmazás több példánya fut, az otthoni címtár meg van osztva összes előfordulását között úgy, hogy az összes előfordulását ugyanabban a könyvtárban. Igen például az alkalmazás menti az otthoni címtár feltöltött fájlokat, ha azokat a fájlokat elérhetők azonnal összes előfordulását. 

<a id="NetworkAccess"></a>
## <a name="network-access"></a>Hálózati hozzáférés
Alkalmazás kódja használhatja a TCP/IP elemre, és UDP-alapú protokollok, hogy a kimenő kapcsolatokat az internetes elérhető végpontok elérhetővé tenni a külső szolgáltatások hálózati. Alkalmazások ugyanazokat a protokollokat használatával csatlakozhat szolgáltatásokhoz Azure és #151, például SQL-adatbázishoz HTTPS-kapcsolatok létrehozásával.

A korlátozott képességek-alkalmazást egy helyi visszacsatolási kapcsolatot létesíteni, és kell figyelni a helyi visszacsatolási szoftvercsatornán alkalmazás is van. Ez a funkció elsősorban ahhoz, hogy alkalmazásai funkcióik részeként sockets helyi visszacsatolási figyelésére létezik. Figyelje meg, hogy minden alkalmazás látja "magánjellegű" visszacsatolási kapcsolat; alkalmazás "A" nem lehet figyelni a "B" alkalmazás által létrehozott helyi visszacsatolási szoftvercsatornához.

A folyamatok közötti kommunikációt (IPC) alkalmazás együttesen futó különböző folyamatok közötti nevesített csövek is támogatottak. Az IIS FastCGI modul például a PHP-lapok futó folyamatok összehangolására nevesített csövek támaszkodik.


<a id="Code"></a>
## <a name="code-execution-processes-and-memory"></a>Programkód, folyamatok és memória
Korábbi amint alkalmazások használatával a gyorsjegyzetekkel alkalmazáskészlet-identitásának alacsony szintű jogosultsággal rendelkező dolgozó folyamatok rendszer futtatása Alkalmazás kódja a memória, a munkafolyamat, valamint a gyermek folyamatokat is lehet van CGI folyamatok és más alkalmazásokkal társított hozzáféréssel rendelkezik. Azonban egy alkalmazás nem tud hozzáférni a memória vagy egy másik alkalmazás adatok esetén is virtuális ugyanazon a gépen.

Alkalmazások parancsfájlok vagy a lapokat a támogatott webes fejlesztési keretek írt futtathatók. Alkalmazás szolgáltatás nem beállításainak bármely webes keretrendszer szűkebb módban. ASP.NET-alkalmazások futó alkalmazás szolgáltatás például "teljes" adatvédelmi, nem pedig egy szűkebb adatvédelmi mód futtathatók. Webes keretek, beleértve mind a klasszikus ASP ASP.NET, például ADO (ActiveX Data Objects) regisztrált alapértelmezés szerint a Windows operációs rendszer a folyamat a COM-összetevők (de nem ki a COM-összetevők feldolgozása) is felhívhatja.

Alkalmazások elindítanak és a tetszőleges kód futtatását. Érdemes a megengedett az alkalmazásokban, például spawn dolgot kell tennie a parancs rendszerhéj, és egy PowerShell-parancsprogramot futtatásához. Azonban annak ellenére, hogy az alkalmazás tetszőleges kódot és folyamatok is van, végrehajtható programok és parancsfájlok olyan továbbra is korlátozni kívánja a szülő alkalmazáskészlet adni. Például az alkalmazásokban is elindítanak kimenő HTTP-híváshoz, amely végrehajtható azonban, hogy nem azonos végrehajtható megpróbálja szüntesse meg a saját adaptert virtuális gép IP-címe Alacsony szintű jogosultsággal rendelkező kód egy kimenő hálózati telefonhívás hozhatnak, de a hálózati beállítások virtuális gépen kísérel meg rendszergazdai jogosultság szükséges.


<a id="Diagnostics"></a>
## <a name="diagnostics-logs-and-events"></a>Diagnosztikai naplók és események
Naplóadatok az adatokat, amely az egyes alkalmazások próbálnak hozzáférni egy másik csoportja. Naplóadatok futtatását App szolgáltatásban elérhető típusú tartalmazza a diagnosztikai, és az alkalmazás, amely is könnyen hozzáférhető, hogy az alkalmazás által létrehozott információk naplózása. 

Például az aktív alkalmazás által létrehozott W3C HTTP naplók érhetők el vagy a hálózati megosztás mappában jön létre az alkalmazást, vagy blob-tárolóhoz érhető el, ha egy ügyfél W3C naplózás tárolóhoz beállított napló könyvtárában. Az utóbbi beállítással anélkül, hogy a hálózati megosztáson társított fájl tárterületkorlátok meghaladó gyűjtendő naplók nagy mennyiségű.

Egy hasonló tekintettel a .NET-alkalmazásokból diagnosztika valós idejű adatokat is bejelentkezhet a .NET-nyomkövetés és diagnosztika infrastruktúra – az alkalmazás hálózati megosztáshoz, írja be a nyomkövetési adatok lehetőséget, vagy másik lehetőségként blob-tároló helyre.

Diagnosztikai naplózás és a nyomkövetés, amelyek nem érhetők el az alkalmazások részeinek, hogy Windows esemény-nyomkövetés események és közös Windows-eseménynaplók (pl. rendszer, az alkalmazás és a biztonsági eseménynaplók). Esemény-nyomkövetés nyomkövetési adatok esetleg lehet megtekinthető gépi szintű (a hozzáférés-vezérlési listák jobbra), mivel az olvasási és írási hozzáféréssel esemény-nyomkövetés események le. A fejlesztők megfigyelheti, hogy API hívások olvasása és írása esemény-nyomkövetés, események és közös Windows-eseménynaplók jelennek meg a munkát, de ennek az oka alkalmazás szolgáltatás van "faking" a hívást, hogy a sikeres jelennek meg. A valóságban az alkalmazás kódja nem fér hozzá az esemény adatokat.

<a id="RegistryAccess"></a>
## <a name="registry-access"></a>Hozzáférés a beállításjegyzék
Alkalmazások hozzáférhetnek csak olvasható nagy (de nem az összes) a beállításjegyzék a virtuális gép a futnak. A gyakorlatban ez azt jelenti, amely a helyi felhasználók csoport csak olvasható elérésének beállításkulcsainak alkalmazások által elérhető. A beállításjegyzék, amely jelenleg nem támogatja az olvasási vagy a írási egy területe a HKEY\_aktuális\_felhasználói struktúra.

A beállításjegyzék írási le van tiltva, beleértve a felhasználónkénti beállításkulcsok való hozzáférést. A szempontjából az alkalmazás a beállításjegyzék írási hozzáféréssel kell soha nem lehet hivatkozni a Azure környezetben óta alkalmazások (van, illetve) települnek különböző virtuális gépeken futó keresztül. Kattintson az alkalmazás által is függő csak állandó írható tárolására az alkalmazás tartalom directory struktúrát, az alkalmazás szolgáltatás UNC megosztások-on tárolt. 

>[AZURE.NOTE] Ha azt szeretné, mielőtt feliratkozna az Azure-fiók használatbavételéhez Azure alkalmazás szolgáltatás, [Próbálja meg alkalmazás szolgáltatás](http://go.microsoft.com/fwlink/?LinkId=523751), ahol azonnal létrehozhat egy rövid életű starter web app alkalmazás szolgáltatásban megnyitásához. Nem kötelező, hitelkártyák Nincs nyilatkozatát.

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]
 
 
