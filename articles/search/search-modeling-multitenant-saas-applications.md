<properties
    pageTitle="Azure keresés Multitenancy modellezése |} Microsoft Azure |} A felhőben tárolt keresési szolgáltatás"
    description="Tudjon meg többet közös tervezés mintázatok multitenant szoftver-alkalmazásokhoz a Azure keresés használata közben."
    services="search"
    manager="jhubbard"
    authors="ashmaka"
    documentationCenter=""/>

<tags
    ms.service="search"
    ms.devlang="NA"
    ms.workload="search"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.date="10/26/2016"
    ms.author="ashmaka"/>

# <a name="design-patterns-for-multitenant-saas-applications-and-azure-search"></a>Multitenant szoftver-alkalmazások és Azure keresési mintázatok tervezése

Egy multitenant alkalmazás egyike, amellyel a bérlők, akik nem látható, vagy bármely más bérlői adatok megosztása tetszőleges számú ugyanazt a szolgáltatások és funkciók. A dokumentum azt ismerteti, hogy a bérlői elkülönítési stratégiák az Azure keresés beépített multitenant alkalmazásokat.

## <a name="azure-search-concepts"></a>Azure keresési fogalmak
Keresési-mint-a-szolgáltatás megoldásként az Azure keresési lehetővé teszi a fejlesztők számára gazdag keresési élményt hozzáadása alkalmazások kezelése bármely infrastruktúra és keresési terület szakértője valamit nélkül. Adatok töltenek fel a szolgáltatást, és kattintson a felhőben tárolt. Egyszerű kérelmek az Azure keresési API-t használ, az adatok majd módosíthatók és szeretne keresni. A szolgáltatás áttekintése a [jelen cikkben](http://aka.ms/whatisazsearch)találhatók. Tervező mintázatok tárgyal, mielőtt fontos Azure keresés néhány fogalmak megértéséhez.

### <a name="search-services-indexes-fields-and-documents"></a>Szolgáltatások, index, mezők és dokumentumok keresése
Azure keresési használatakor egy előfizet egy _keresési szolgáltatás_. Adatok feltöltött Azure keresési, mint a keresési szolgáltatás _index_ vannak tárolva. Az indexek egyetlen szolgáltatás számos lehet. A jól ismert fogalmak az adatbázisok használatához a közben az indexek szolgáltatás belül is likened, adatbázis tábláiból az adatbázishoz a keresési szolgáltatás is likened.

Keresési szolgáltatásának belül minden index rendelkezik saját sémát, amelyet testre szabható _mezők_számos határozza meg. Adatok bekerül az egyes _dokumentumokat_formájában Azure keresési index. Minden dokumentum egy adott indexhez fel kell tölteni, és a tárgymutató-séma kell igazítása. Azure keresőfunkcióval adatok keresésekor a teljes szöveges keresési lekérdezések adják, egy adott index szemben.  Az adatbázis e fogalmak össze, mezőket is likened, a táblázat oszlopainak, és dokumentumok is likened, sorok.

### <a name="scalability"></a>Méretezhetőség:
Két dimenzióban a [réteg árak](https://azure.microsoft.com/pricing/details/search/) szabványos bármely Azure keresési szolgáltatásának méretezheti: tárolására és elérhetőségét.
* Keresési szolgáltatásának tárolására növelése _partíciót_ bővíthető.
* A keresési szolgáltatásának kezelhető kapacitásának növeléséhez szolgáltatás _kópiák_ bővíthető.

Hozzáadása és eltávolítása partíciók, és a kópiák lehetővé teszi az adatok mennyiségét a nagyobb, és az alkalmazás igények forgalmat a keresési szolgáltatás kapacitása. Ahhoz, hogy a keresési szolgáltatás [SLA](https://azure.microsoft.com/support/legal/sla/search/v1_0/)olvasási eléréséhez két kópia szükséges. Ahhoz, hogy egy írási és olvasási [SLA](https://azure.microsoft.com/support/legal/sla/search/v1_0/)eléréséhez szolgáltatás három kópiák szükséges.


### <a name="service-and-index-limits-in-azure-search"></a>Szolgáltatás és a tárgymutató-korlátozások az Azure-keresés
Létezik néhány másik [Rétegek árak](https://azure.microsoft.com/pricing/details/search/) Azure keresés, a rétegek mindegyikének különböző [korlátai és kvóták](search-limits-quotas-capacity.md). Ezek a korlátozások némelyike a szolgáltatás szintjén, az index szinten vannak, és a partíciót szinten vannak.


|                                  | Egyszerű     | Standard1   | Standard2   | Standard3   | HD-minőségű Standard3  |
|----------------------------------|-----------|-------------|-------------|-------------|---------------|
| Egy szolgáltatás maximális kópiák     | 3         | 12          | 12          | 12          | 12            |
| Maximális partíciót egy szolgáltatás   | 1         | 12          | 12          | 12          | 1             |
| Keresés maximális mennyiség (kópiák * partíciót) / szolgáltatás | 3         | 36          | 36          | 36          | 36 (legfeljebb 3 partíciót)            |
| Egy szolgáltatás maximális dokumentumok    | 1 millió | 180 millió | 720 millió | 1.4 milliárd | 600 millió   |
| Egy szolgáltatás maximális tárhely      | 2 GB      | 300 GB      | AZ 1.2 TB      | 2.4 TB      | 600 GB        |
| Per Partition maximális dokumentumok  | 1 millió | 15 millió  | 60 millió  | 120 millió | 200 millió   |
| Maximális tárolási partíciót egy    | 2 GB      | 25 GB       | 100 GB      | 200 GB      | 200 GB        |
| Egy szolgáltatás maximális indexek      | 5         | 50          | 200         | 200         | 3000 (legfeljebb 1000 indexek/partíciót)          |


#### <a name="s3-high-density"></a>S3 Magas sűrűségfüggvény eredménye "
Azure keresési S3 árak réteg van a magas sűrűség (HD) mód, amelyeket kifejezetten multitenant esetek beállítást. Sok esetben támogatja az egyszerűség és a költség hatékonyság előnyei eléréséhez egyetlen szolgáltatás a kisebb bérlők nagyszámú szükség.

A sok kis indexek irányuló egyetlen keresési szolgáltatásának kezelése csoportjában, az azt jelenti, hogy az indexek használatával lehetővé teszi a partíciók szeretné egy szolgáltatás további indexek üzemeltetni méretezése kereskedési S3 HD tesz lehetővé.

S3 szolgáltatásainak konkrétan, 1 és 200 indexek tároló közös sikerült legfeljebb 1.4 milliárd dokumentumok között lehetnek. Egy S3 HD azonban lehetővé teszik, hogy az egyes indexek csak küldésre legfeljebb 1 millió dokumentumok, de is képes kezelni 200 millió partíciót a teljes dokumentum számának (felfelé / szolgáltatás 3000) partíciót egy legfeljebb 1000 indexek (akár 600 millió szolgáltatást).



## <a name="considerations-for-multitenant-applications"></a>Multitenant alkalmazások megfontolandó szempontok
Multitenant alkalmazások hatékony kell terjesztése erőforrások a bérlők közötti megőrzése mellett néhány a különböző bérlők közötti adatvédelmi szintjét. Ha dolgot kell néhány az alkalmazáshoz a architektúrájának tervezése:

* _Elkülönítési bérlői:_ Alkalmazás a fejlesztők kell győződjön meg arról, hogy nincs bérlők jogosulatlan vagy az egyéb bérlők az adatokhoz való hozzáférés nemkívánatos megfelelő intézkedéseket. Adatok védelmét a rajtuk kívül a bérlői elkülönítési stratégiák megosztott erőforrásokat és zajos szomszédok elleni védelem hatékony kezelésének szükség.
* _Erőforrásköltség cloud:_ Másik alkalmazás, a szoftver megoldások maradhat költség versenytársak egy multitenant alkalmazás részeként.
* _Műveletek Kezeléstechnikai:_ Egy multitenant architektúra fejlesztésekor az alkalmazás műveletek és összetettsége hatással a fontos tényező. Azure keresés egy [99,9 % SLA](https://azure.microsoft.com/support/legal/sla/search/v1_0/)tartalmaz.
* _Globális helyigénye:_ Multitenant alkalmazások szükséges hatékony a bérlők, amelyek között a földgömb elosztott szolgálnak.
* _Méretezhetőség:_ Alkalmazásfejlesztő figyelembe kell vennie, hogyan azok egyeztetése elég alacsony alkalmazás összetettsége fenntartása és az alkalmazás, ha át kívánja méretezni a bérlők számát és a bérlők adatok és a terhelést méretének tervezése között.

Azure keresési kínál néhány határai bérlők adatok és a terhelést elkülönítése használható.

## <a name="modeling-multitenancy-with-azure-search"></a>Modellezési multitenancy az Azure-keresés
Multitenant esetben, ha az alkalmazás fejlesztője fogyasztása egy vagy több keresési szolgáltatás, és a bérlők között szolgáltatások vagy indexek osztása. Azure keresési van néhány gyakori mintázatok, ha a modellezési multitenant példa:

1. _Bérlőnként Index:_ Egyes bérlői rendelkezik saját index más bérlők megosztott keresési szolgáltatásának belül.
1. _Szolgáltatás bérlőnként:_ Minden bérlői legmagasabb szintű adatok és a terhelést a szétválasztás kínáló saját dedikált Azure keresési szolgáltatás tartalmaz.
1. _Vegyesen is:_ Nagyobb, több aktív bérlők dedikált szolgáltatások vannak rendelve, miközben kisebb bérlők kapnak az egyedi indexeket belül megosztottszolgáltatás.

## <a name="1-index-per-tenant"></a>1. index bérlőnként
![A tárgymutató-használati bérlői modell egy portrayal](./media/search-modeling-multitenant-saas-applications/azure-search-index-per-tenant.png)

A tárgymutató-használati bérlői modell az több bérlők vezérlőelemmel egyetlen Azure keresési szolgáltatásának hol minden bérlői van-e saját indexe.

Bérlők adatok elkülönítési érhető el, mivel minden keresési kérelem és a dokumentum műveletek az Azure keresési index szinten adják. Az alkalmazási réteg van a szükséges tájékoztatás irányítsa át a különböző bérlők-alapú forgalmat a megfelelő indexek összes bérlők között a szolgáltatás szintjén erőforrások is kezelésekor.

A fő a tárgymutató-használati bérlői modell attribútum a lehetőségét, hogy az alkalmazás fejlesztője való oversubscribe az alkalmazás bérlők között keresési szolgáltatásának kapacitása. Ha a bérlők terhelést egy páratlan eloszlása van, bérlők optimális kombinációját terjeszthetők végig a keresési szolgáltatás indexek közben egyidejű felszolgálásához egy kisebb aktív bérlők a képviselőknek erősen aktív, az erőforrás-igényes bérlők számos igazodik. A csökkentés megakadályozhatják a modell minden bérlő esetén egyidejűleg erősen aktív helyzetek kezelésében.

A tárgymutató-használati bérlői modell alapjául szolgáló változó költség modell, ahol egy teljes Azure keresési szolgáltatás vásárolt terveznie és majd ezt követően töltve bérlők. Ez használaton kívüli kapacitása ahhoz, hogy ki kell jelölni a próbaidőszakra vagy ingyenes fiókok tesz lehetővé.

Globális tárhely alkalmazások esetén a tárgymutató-használati bérlői modell nem lehet a lehető leghatékonyabb. Ha egy alkalmazás bérlők között a földgömb elosztott, lehet, hogy egy külön szolgáltatás szükséges az egyes régiókra, amely előfordulhat, hogy ismétlődő költségek mindegyikénél ezek.

Azure keresési az egyedi indexeket, mind az indexek száma beosztásának növekedéséhez tesz lehetővé. Ha egy megfelelő árak réteg választja, és partíciók és kópiák adhatók hozzá a teljes keresési szolgáltatás, ha a szolgáltatás belül egyedi index megnő túl nagy tároló vagy a forgalmat.

Indexek száma növekszik, túl nagy ahhoz, hogy egyetlen szolgáltatás, ha rendelkezik egy másik szolgáltatás kiterjesztve azt az új bérlők kell építenie. Indexek van a keresési szolgáltatás között áthelyezni, az új szolgáltatások kerülnek, ha az adatokat az indexből kézzel kell átmásolni egy index a másikra, Azure keresés nem engedélyezi a tárgymutató-be áthelyezendő rendelkezik.


## <a name="2-service-per-tenant"></a>2. bérlőnként szolgáltatás
![A szolgáltatás a bérlő használati modell egy portrayal](./media/search-modeling-multitenant-saas-applications/azure-search-service-per-tenant.png)

A szolgáltatás a bérlő használati architektúra minden bérlői saját keresési szolgáltatás tartalmaz.

Ebben a modellben az alkalmazás éri el a bérlők elkülönítve maximális szintjét. Egyes szolgáltatásokhoz rendelkezik kitűzött célja, tárterület és a keresési kérelem, valamint a külön API-kulcsok kezelésére szolgáló kapacitása.

Alkalmazások, ahol minden bérlői egy nagy helyigénye vagy a terhelést a kis adattartományról bérlői bérlői tartalmaz a szolgáltatás a bérlő használati modell megegyezik egy hatékony választási lehetőségek különböző bérlők munkaterhelésekből keresztül nem megosztott erőforrásokat.

Egy bérlői modellenként szolgáltatást is kínál kiszámítható, a fix költség modell előnyeit. Egy teljes keresési szolgáltatás nincs terveznie befektetés nem addig, amíg nincs töltse ki, hogy bérlői webhelyre, azonban a költség-használati-bérlő nagyobb, mint az index a bérlő használati modell.

A szolgáltatás a bérlő használati modell esetén a globális tárhely alkalmazások egy hatékony választani. A bérlők földrajzilag elosztott nagyon egyszerűen, hogy az egyes bérlői szolgáltatás a megfelelő régióban.

A méretezés a minta a problémáit merülnek fel, ha egyes bérlők elszaporodó a szolgáltatás. Azure keresési jelenleg nem támogatja az összes adat új szolgáltatás manuálisan kell másolni szeretné, hogy a keresési szolgáltatás a árak réteg frissítésével.

## <a name="3-mixing-both-models"></a>3. a két modellek keverése
Egy másik mintájának multitenancy modellezése van keverése index a bérlő használati és a szolgáltatás a bérlő használati stratégiák.

Által a két mintázatok keverése, az alkalmazások legnagyobb bérlők is vezérlőelemmel saját szolgáltatások, miközben a képviselőknek kevésbé aktív, a kisebb bérlők az indexek foglalni a megosztott szolgáltatás. Ez a modell gondoskodhat arról, hogy a legnagyobb bérlők és a kisebb bérlők védekezésben bármely zajos szomszédok a szolgáltatásból egységes nagy teljesítményű.

Azonban előrejelző előrejelzésére, mely a bérlők esetén kell egy dedikált szolgáltatás egy megosztott szolgáltatás az index és a stratégia végrehajtása támaszkodik. Alkalmazás összetettsége növeli a mindkét alábbi multitenancy modellek kezeléséhez szükséges a.

## <a name="achieving-even-finer-granularity"></a>Akár finomabb granuláltságú elérése
A fenti tervezés mintázatú kiemelésével Azure keresés multitenant esetek modell tegyük fel, ahol minden bérlői webhelye fel az alkalmazás egy teljes példánya egységes hatókör. Alkalmazások azonban néha képes kezelni sok kisebb hatókörét.

Szolgáltatás a bérlő használati és index a bérlő használati modellek nem elég hatókörök állnak, ha ajánlatos modell a tárgymutató-még részletesebben szintű engedélyeknek eléréséhez.

Ahhoz, hogy a korábbi verziókban eltérően működnek, a másik ügyfél végpontokhoz egyetlen index létrehozása, tárgymutató, amely egy adott értéket minden lehetséges ügyfél bővíthető egy mezőt. Minden alkalommal, amikor egy ügyfél felhívja Azure keresési lekérdezés, vagy módosítsa az indexet, a kód ügyfélalkalmazás Itt adhatja meg a lekérdezés időtartama Azure keresési [szűrő](https://msdn.microsoft.com/library/azure/dn798921.aspx) lehetőséget használja ezt a mezőt a megfelelő értéket.

Ez a módszer is használható, külön felhasználói fiókokat, külön engedélyszinteket működésének eléréséhez, és akár teljesen külön alkalmazások.

## <a name="next-steps"></a>Következő lépések
Azure keresési esetén számos alkalmazás, a [További információ a szolgáltatás nagy teljesítményű funkciókkal](http://aka.ms/whatisazsearch)lenyűgöző választani. A különböző tervezés mintázatok multitenant alkalmazások kiértékelésekor fontolja meg a [különböző árak rétegek](https://azure.microsoft.com/pricing/details/search/) és a megfelelő [szolgáltatás korlátai](search-limits-quotas-capacity.md) legjobb személyre szab Azure keresési alkalmazás munkaterhelésének és a különféle méretű architektúrájának igazítása.

Azure keresési és multitenant esetek kapcsolatos kérdéseit is a irányítja azuresearch_contact@microsoft.com.
