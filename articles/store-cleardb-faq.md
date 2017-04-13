<properties
    pageTitle="Gyakori kérdések az ClearDB MySql-adatbázishoz Azure alkalmazás szolgáltatással |} Microsoft Azure"
    description="ClearDB MySQL-adatbázisok használatával Azure alkalmazás szolgáltatással kapcsolatos gyakori kérdésekre adott válaszok."
    documentationCenter="php"
    services=""
    authors="sunbuild"
    manager="yochayk"
    editor=""
    tags="mysql"/>

<tags
    ms.service="multiple"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/27/2016"
    ms.author="sumuth"/>

# <a name="faq-for-cleardb-mysql-databases-with-azure-app-service"></a>Azure alkalmazás szolgáltatással ClearDB MySql-adatbázis – gyakori kérdések

Ez a cikk az Azure Web Apps alkalmazások adatbázisok ClearDB MySQL vásárlásával és használatával kapcsolatos gyakori kérdésekre ad választ.

## <a name="what-options-do-i-have-for-mysql-on-azure"></a>Milyen lehetőségeket Azure a MySQL-van?

Több lehetőség közül választhat:

* [ClearDB megosztott MySQL-adatbázishoz](/marketplace/partners/cleardb/databases/)

* [ClearDB MySQL prémium fürt](/marketplace/partners/cleardb-clusters/cluster/)

* [Az Azure virtuális futó MySQL fürthöz](https://github.com/azure/azure-quickstart-templates/tree/master/mysql-replication)

* [A MySQL-Azure virtuális futó példányát egyetlen](virtual-machines/virtual-machines-windows-classic-mysql-2008r2.md)

ClearDB a MySQL-szolgáltatója és kezeli a MySQL-infrastruktúrát meg. Futtatásakor a saját MySQL fürt vagy az adatbázis-Azure virtuális gép, akkor a MySQL-kiszolgáló beállítása, és tartsa naprakészen javítások.

## <a name="do-i-need-a-credit-card-for-the-web-app--mysql-template-in-the-azure-marketplace"></a>A Web app + MySQL-sablon a Microsoft Azure piactéren van szükségem hitelkártyát?

Ez attól függ, hogy milyen típusú előfizetés esetén. Íme néhány elterjedt előfizetés típusa:

* [Díj folyamatos rendezését](/offers/ms-azr-0003p/): a hitelkártya, és a hitelkártya terheli fizetett MySQL-adatbázishoz vásárlásakor van szükség.

* [Ingyenes próbaverzió](https://azure.microsoft.com/pricing/free-trial/): jóváírások tartalmaz, ezzel a Microsoft Azure-szolgáltatások, de nem engedélyezi a külső erőforrások vásárlás. Meg kell vásárolnia, akkor használja a hitelkártya engedélyezett előfizetést Térítéses MySQL-adatbázishoz, vagy külső szolgáltatásokra támaszkodik. Web Apps alkalmazások ingyenes ClearDB MySQL-adatbázishoz is létrehozhat.

* [MSDN előfizetés](/pricing/member-offers/msdn-benefits/) és **MSDN fejlesztők próba fizet közben Ugrás**: ingyenes próbaverziót hasonlóan egy MSDN-előfizetést igényel kell rendelkeznie a hitelkártyaadatait, hogy vásároljon fizetett MySQL megoldást ClearDB.

* [Vállalati szerződés (EA)](/pricing/enterprise-agreement/): EA ügyfelek vannak számlát kapni a EA elleni minden negyedév összes a Microsoft Azure piactéren (külső) vásárlások külön, összevont számlán. Vannak, a pénzügyi lekötési bármely piactér vásárlására kívüli számlát kapni. Ne feledje, hogy adott időben, Azure áruházból vevőknek Azerbajdzsán, Horvátország, Norvégia és Puerto Ricó-i tehát nem érhető el. 

*  [DreamSpark](https://www.dreamspark.com/Product/Product.aspx?productid=99): csak az ingyenes ClearDB adatbázisok Web Apps alkalmazások hozhat létre. Nincs korlát a hozhat létre ingyenes ClearDB MySQL-adatbázisok nem. Ne feledje, hogy ingyenes adatbázisok nem használandó gyártási web Apps alkalmazások, ez a szolgáltatás csak próbaverzió szól.

## <a name="why-was-i-charged-350-for-a-web-app--mysql-from-the-azure-marketplace"></a>Miért történt a $3.50 webalkalmazást + MySQL a Microsoft Azure piactéren lévő fel?

Az alapértelmezett beállítás a adatbázis Titan $3.50. Az adatbázis létrehozása során a költség nem bemutatjuk, és előfordulhat, hogy véletlenül megvásárlása nem kíván adatbázis. Azt próbál meg oly módon, hogy tökéletesíthesse a működését, de addig ellenőriznie kell minden a kijelölt árak rétegek web App alkalmazásban és az adatbázis **létrehozása** parancsra, és az erőforrások telepítését indítása előtt.

## <a name="i-am-running-mysql-on-my-own-azure-virtual-machine-can-i-connect-my-azure-web-app-to-my-database"></a>A saját Azure virtuális gépen fut MySQL. Tudok csatlakozni a Azure web App alkalmazásban a adatbázis?

igen. Mindaddig, amíg az Azure virtuális távelérési meghatalmazta a web App alkalmazásban, a webalkalmazásban csatlakozhat az adatbázis. További tudnivalókért olvassa el a [Virtuális gépre telepítéséhez MySQL](virtual-machines/virtual-machines-windows-classic-mysql-2008r2.md)című témakört.

## <a name="in-which-countries-are-cleardb-premium-mysql-clusters-supported"></a>Amelyben országok is ClearDB prémium MySQL fürt támogatott?

[ClearDB prémium MySQL fürt](/marketplace/partners/cleardb-clusters/cluster/) világszerte India, Ausztrália, Brazil déli és Kína kivételével az összes Azure régióban érhetők el.

## <a name="can-i-create-a-new-cluster-prior-to-creating-a-database-with-cleardb-premium-cluster-solution"></a>Hozhat létre egy új fürt ClearDB prémium fürt megoldás az adatbázis létrehozása előtt?

Üres ClearDB fürt létrehozása nem, nem támogatott. Az Azure portal segítségével hozza létre az adatbázisokat hozhat létre új fürt egyszerre fürt.

## <a name="will-i-be-warned-if-i-try-to-delete-a-cleardb-mysql-database-that-is-in-use-by-one-of-my-applications"></a>E figyelmeztetést kap Ha megpróbálom használatban lévő ClearDB MySQL-adatbázishoz törlése egy az alkalmazásokat?

Nem, Azure nem figyelmeztet, ha törli a piactér vásárol, az alkalmazás függ.

## <a name="which-regions-can-i-create-cleardb-databases-in"></a>Azokat a területeket létrehozhatok ClearDB-adatbázisok?

Azure piactér vevőknek Azerbajdzsán, Horvátország, Norvégia vagy Puerto Ricó-i tehát nem érhető el. ClearDB nem érhető el az alábbi régiókban.

## <a name="what-pricing-tier-should-i-choose-for-a-production-web-app-and-database"></a>Milyen árak réteg adjak meg egy gyártási web App alkalmazásban és az adatbázis?

Web Apps alkalmazások használata a Basic vagy egy újabb árak réteg. Az ClearDB azt javasoljuk Szaturnusznak vagy Jupiter csomagot. Tekintse át a szolgáltatások és korlátai minden árak réteg a [Web Apps alkalmazások](/pricing/details/app-service/) és a [ClearDB MySQL esetén](/marketplace/partners/cleardb/databases/) válassza ki azt, amely az igényeinek.

## <a name="how-do-i-upgrade-my-cleardb-database-from-one-plan-to-another"></a>Hogyan frissíthetem a ClearDB adatbázis a csomagok között?

Az [Azure portál](https://portal.azure.com)méretezheti ClearDB megosztott üzemeltetési adatbázis létrehozása. Olvassa el a [cikk](https://blogs.msdn.microsoft.com/appserviceteam/2016/10/06/upgrade-your-cleardb-mysql-database-in-azure-portal/) további információt. Jelenleg nem támogatott diagramtípusról frissítés ClearDB prémium fürtre az Azure-portálon vonatkozóan.

## <a name="i-cant-see-my-cleardb-database-in-azure-portal"></a>Nem látható a ClearDB adatbázis az Azure-portálon?

Erőforrás-kezelő Azure vagy [Új Azure portál](https://portal.azure.com)ClearDB adatbázis hozzunk létre, ha nem lesz látható a [Régi Azure-portálon](https://manage.windowsazure.com). Munka-kerülheti jelenti, hogy az adatbázis kézi csatolása a web App alkalmazásban. Hasonlóképpen ha ClearDB adatbázis létrehozása a [régi portálon](https://manage.windowsazure.com) nem látja az [Új Azure portál](https://portal.azure.com)az adatbázist. Nincs a munka körül az utóbbi esetben az nem.

## <a name="who-do-i-contact-for-support-when-my-database-is-down"></a>Ki érhető el a támogatási lefelé a-adatbázis esetén?

Kapcsolattartó [ClearDB támogatja](https://www.cleardb.com/developers/help/support) bármilyen adatbázis kapcsolatos problémákat. Készüljön fel adja meg az Azure előfizetés információkat.

## <a name="can-i-create-additional-users-for-my-cleardb-mysql-database-cluster-solution"></a>Létrehozhatok-e a többi felhasználó a ClearDB MySQL-adatbázis fürt megoldás? 

nem. A többi felhasználó nem hozhat létre, de a ClearDB adatbázis fürt további adatbázisokat hozhat létre.  

## <a name="can-basicpro-series-databases-be-upgraded-in-place-similar-to-planetary-plans-today-on-cleardb-portal"></a>Adatbázis Basic/Pro sorozat frissített is helyi ClearDB portálon ma planetáris csomagok hasonló?

Igen, a adatbázisok lehet egyszerű számozási helyi (egyszerű 60 keresztül egyszerű 500) frissíteni. Pro sorozat lehet frissíteni a helyi (Pro 125 keresztül Pro 1000) Pro 60 kivételével. Pro 60 adatbázis frissítése jelenleg nem támogatja. 

## <a name="when-i-migrate-my-resources-from-one-subscription-to-another-does-my-cleardb-mysql-database-get-migrated-as-well"></a>Az erőforrások tudom áttelepíteni egyik előfizetésről a másikra, ha nem a ClearDB MySQL-adatbázis települnek, valamint? 

Végrehajtásakor áttelepítése az erőforrás-előfizetésekben, bizonyos [korlátozások](./app-service-web/app-service-move-resources.md) érvényesek. Egy harmadik fél szolgáltatás ClearDB MySQL-adatbázishoz, és így nem települnek Azure előfizetés áttelepítés során. Ha a MySQL-adatbázis áttelepítése Azure erőforrások előtt áttelepítése nem kezeli, az ClearDB MySQL-adatbázisok is tiltható le. Manuális áttelepítése először az adatbázisok, és kattintson a web App az Azure előfizetés-áttelepítés végrehajtása. 

## <a name="can-i-transfer-a-cleardb-database-from-a-credit-card-subscription-to-an-ea-subscription"></a>Átvihetem ClearDB adatbázis hitelkártya előfizetésből EA előfizetésbe?

Meglévő ClearDB adatbázisok a meglévő előfizetés társított hitelkártya használata. Egy EA-előfizetést, az adatok áttelepítése az új adatbázist szeretne használni:

* Vásároljon új ClearDB adatbázis-EA előfizetése.
* Az új adatbázis áttelepítése az adatok.
* Frissítse az alkalmazás az új adatbázis használhatja.
* Törölje a régi ClearDB adatbázist.

Hozzon létre egy új webalkalmazás MySQL (ClearDB), vagy hozzon létre a MySQL-adatbázishoz (ClearDB), ha az előfizetést, úgy dönt, határozza meg, hogyan kell fizetnie a szolgáltatás. Egy EA előfizetéssel azt nem blokkolja a külső szolgáltatások, például ClearDB az Azure-portálon beszerzéséhez. EA előfizetések kívüli pénzbeli lekötési számlázva vannak, és vannak a negyedéves és utólag a számlát kapni. Állítsa be a fizetési mód, például hitelkártya minden olyan külső piactéren elérhető szolgáltatások fizetés EA ügyfél kellene.

## <a name="where-can-i-see-the-charges-for-cleardb-resources-in-an-ea-subscription"></a>Hol jelennek meg a költségek ClearDB erőforrások EA-előfizetésben?

Közvetlen EA ügyfelek, a Microsoft Azure piactéren díjak jelennek meg a vállalati portál. Ne feledje, hogy minden piactér vásárlások és felhasználási kívüli pénzbeli lekötési számlázva vannak vannak a negyedéves és utólag a számlát kapni. EA ügyfelek kell fizetnie közvetlenül a külső szolgáltatók és teheti a fizetési mód, például hitelkártya azok EA fiókkal engedélyezésével.

Indirekt EA ügyfelek megtalálhatja saját Azure Piactéri előfizetéseit, az **Előfizetések kezelése** lapon, a vállalati portál, de árak el van rejtve. Ügyfeleink forduljanak a LSP piactér díjak olvashat.

Külső szolgáltatások a Microsoft Azure piactéren hozzáférést a EA Azure regisztrációs rendszergazdák által kezelhető. Letilthatja, vagy újra hozzáférés engedélyezése a 3 fél vásárlások a fiókok kezelése és előfizetések áruházból a vállalati portál fiókok csoportban.

## <a name="who-do-i-contact-for-questions-about-my-bill-for-cleardb-services-in-my-ea-subscription"></a>Ki érhető el a kérdésekre ClearDB szolgáltatások számlájának kapcsolatban a EA előfizetésem?

Lépjen kapcsolatba a [Vállalati ügyfélszolgálatával](http://aka.ms/AzureEntSupport) kapcsolatos számlázási azok EA regisztrációs csoportban. A portál EA támogatási csoport választ kérdésére, vagy a a probléma megoldásához.

 



## <a name="more-information"></a>További információ

[Azure piactér – gyakori kérdések](/marketplace/faq/)
