<properties
    pageTitle="DocumentDB tárhely és a teljesítmény |} Microsoft Azure" 
    description="Tudjon meg többet a adattárolás és a dokumentumok tárolása a DocumentDB, és hogyan méretezheti DocumentDB a kapacitás igényeknek megfelelően az alkalmazás." 
    keywords="dokumentumok tárolása"
    services="documentdb" 
    authors="syamkmsft" 
    manager="jhubbard" 
    editor="cgronlun" 
    documentationCenter=""/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/18/2016" 
    ms.author="syamk"/>

# <a name="learn-about-storage-and-predictable-performance-provisioning-in-documentdb"></a>Tárterület és kiszámíthatóan teljesítmény DocumentDB a kiépítési ismertetése
Azure DocumentDB szolgáltatás teljes körű felügyelt méretezhető dokumentumorientált NoSQL adatbázis szolgáltatás JSON dokumentumok. A DocumentDB nincs virtuális gépeken futó bérbe, szoftvert telepíthet vagy Lync-adatbázisok. DocumentDB üzemeltetett és melyekre Microsoft mérnökök nemzetközi osztály elérhetősége, a teljesítmény és az adatvédelem előadásához.  

DocumentDB: [Hozzon létre egy adatbázis-fiók](documentdb-create-account.md) és az [Azure portálon](https://portal.azure.com/)keresztül [DocumentDB adatbázis](documentdb-create-database.md) használatának megkezdéséhez. DocumentDB adatbázisok kínálják, az erőforrás-mennyiség félvezető meghajtó (SSD) biztonsági tárterület és a teljesítmény. A tárolási egységek kiépített környezetet nyújt [adatbázis webhelycsoportok létrehozása](documentdb-create-collection.md) az adatbázis-fiókját, minden fenntartott átviteli definiálhat felfelé vagy lefelé az alkalmazás igényeit bármikor gyűjtemény belül. 

Ha az alkalmazás meghaladja a egy vagy több webhelycsoportok számára fenntartott átviteli, kérelmek egy webhelycsoport alapon korlátozott. Ez azt jelenti, hogy egyes alkalmazás kérések sikerülhet, míg mások esetleg szabályozott.

Ez a cikk áttekintést nyújt az erőforrások és mérőszámok kapacitásának kezelése és adattárolás megtervezése érhető el. 

## <a name="database-account"></a>Adatbázis-fiók
Az Azure előfizetői, mint egy vagy több DocumentDB adatbázis fiókok kezelése az adatbázis-erőforrások kiépítése van is. Egyes előfizetéshez társítva egyetlen Azure előfizetés. 

Az [Azure portálon](documentdb-create-account.md)keresztül, illetve [egy ARM sablon vagy Azure CLI](documentdb-automation-resource-manager-cli.md)használatával DocumentDB számlákat hozhat létre.

## <a name="databases"></a>Adatbázisok
Egyetlen DocumentDB adatbázis gyakorlatilag korlátlan mennyiségű gyűjtemények csoportosítva dokumentumok tárolására is tartalmazhatnak. Webhelycsoportok adja meg a teljesítmény elkülönítési – minden egyes webhelycsoport együtt más gyűjtemények ugyanazt az adatbázis vagy a nem megosztott átviteli kiépítve is. Egy DocumentDB adatbázisa rugalmas méretű, TBs a SSD biztonsági dokumentumok tárolása és kiépített átviteli GB kezdve. A hagyományos RDBMS adatbázisoktól eltérően DocumentDB az adatbázis adatbázisokban nem egy egyetlen számítógépre, és több gépek vagy fürt is kiterjedhet.  

Az DocumentDB Ha át kívánja méretezni, az alkalmazások, szükség szerint készíthet több gyűjtemények vagy adatbázisok vagy mindkettőt. Az [Azure portálon](documentdb-create-database.md) vagy a [DocumentDB SDK](documentdb-dotnet-samples.md)valamelyik keresztül adatbázisokat hozhat létre.   

## <a name="database-collections"></a>Adatbázis-gyűjtemények
Minden egyes DocumentDB adatbázis egy vagy több webhelycsoport tartalmazhat. Webhelycsoportok használni a kívánt könnyen hozzáférhető adatok partíciót a dokumentumok tárolása és feldolgozása. Minden egyes webhelycsoport eltérő séma használata dokumentumok tárolhatók. Automatikus indexelés DocumentDB meg és a lekérdezés funkciók lehetővé teszi, egyszerűen szűrése, valamint dokumentumok. Egy webhelycsoport végrehajtásának dokumentum tárhely és a lekérdezés hatókörének biztosít. Egy webhelycsoport akkor is tranzakció tartomány minden olyan dokumentumnál, benne. Webhelycsoportok beállíthatja az Azure-portálon vagy a SDK keresztül értéke alapján átviteli van rendelve. 

Webhelycsoportok be egy vagy több fizikai kiszolgáló automatikusan particionálva a DocumentDB szerint. Amikor létrehoz egy webhelycsoport, a második és főbb tulajdonságait partíciót egy kérés egységek számát tekintve kiépített átviteli is megadhat. Ez a tulajdonság értékét DocumentDB használt dokumentumok között partíciók és útvonal kérelmeket, például a lekérdezések terjesztése. A partíciók kulcsmező értékét is végpontként tárolt eljárások és indítók tranzakció határa. Minden egyes webhelycsoport egy átviteli, hogy a webhelycsoportban nincs megosztva más gyűjtemények ugyanazzal a fiókkal az adott fenntartott mennyiségű tartalmaz. Ezért méretezheti meg az alkalmazást, mind a tárhely és a teljesítmény. 

Az [Azure portálon](documentdb-create-collection.md) vagy a [DocumentDB SDK](documentdb-sdk-dotnet.md)valamelyik keresztül gyűjtemények hozhat létre.   
 
## <a name="request-units-and-database-operations"></a>Erőforrás-mennyiség és az adatbázis-műveletek

Egy webhelycsoport létrehozásakor, lefoglalása [kérelem egységek (Licencelési)](documentdb-request-units.md) másodpercenként számát tekintve kapacitása. Helyette gondolat, és kezelni a hardver-erőforrások, mint az erőforrásokhoz tartozó egyetlen intézkedés **kérelem egység** érdemes elképzelnie, végezze el az adatbázis-műveletek és szükséges szolgáltatási kérelem kérelmet. 1 KB dokumentum olvasási fogyasztása az azonos 1 Licencelési függetlenül attól, a webhelycsoport vagy a számát rendszert futtató azonos egyidejű kérelmek tárolt elemek számát. Az összes kérések elleni DocumentDB, többek között az összetett műveleteket, mint az SQL-lekérdezések van fejlesztési időben meghatározható kiszámíthatóan Licencelési értéket. Ha tudja a dokumentumok méretét és az egyes műveletek (olvasása, írást és lekérdezések) az alkalmazás támogatja a gyakoriság, átviteli igényeknek megfelelően az alkalmazás a pontos összegét kiépítése, és méretezni az adatbázis felfelé és lefelé, a teljesítmény igények változásával. 

Minden egyes webhelycsoport foglalható másodpercenként, több száz kérelem erőforrás-mennyiség / szekundum milliónyi legfeljebb 100 kérelem egységeinek blokkokban átviteli együtt. A kiépített átviteli teljes módosítását a feldolgozási igényeihez, és az alkalmazás mintázatok eléréséhez gyűjtemény élettartama módosítható. További tudnivalókért olvassa el a [DocumentDB teljesítményszint](documentdb-performance-levels.md)című témakört. 

>[AZURE.IMPORTANT] Webhelycsoportok számlázható szervezetek. A költség a kérelem erőforrás-mennyiség / szekundum gigabájtban felhasznált tárterületkorlátot együtt mért gyűjtemény a kiépített átviteli határozza meg. 

Hány kérelem egységek felhasználja egy adott művelet, például a Beszúrás, törlés, lekérdezést vagy tárolt eljárás végrehajtási? A kérelem egység normalizált méri-összehívás feldolgozása költség. 1 KB dokumentum olvasási értéke 1 Licencelési, de a kérést, csere és törlése a dokumentumon felhasználja további feldolgozása a szolgáltatást, és egyúttal további kérelem egységek. A szolgáltatás egyes válasza tartalmaz egy egyéni fejlécet (`x-ms-request-charge`), amely jelzi, hogy az elfogyasztott mennyiség a kérelem kérelem egységek. Ezt a fejlécet a [SDK](documentdb-sdk-dotnet.md)keresztül is érhető el. A .NET SDK [RequestCharge](https://msdn.microsoft.com/library/azure/dn933057.aspx#P:Microsoft.Azure.Documents.Client.ResourceResponse`1.RequestCharge) [ResourceResponse](https://msdn.microsoft.com/library/azure/dn799209.aspx) objektum tulajdonság. Ha szeretne átviteli igényeinek megfelelően előtt Telefonhívás kezdeményezése egyetlen becslése, súgó: Ez a becslés a [kapacitás Csapattervező](documentdb-request-units.md#estimating-throughput-needs) segítségével. 

>[AZURE.NOTE] Az alapköltség az Alapterv 1 kérelem egység 1 KB dokumentum a [Munkamenet konzisztencia](documentdb-consistency-levels.md)a dokumentum egy egyszerű GET felel meg. 

Számos tényező hatással lehet a kérelem egységek felhasznált DocumentDB adatbázis-fiókot elleni művelet. Ezek a tényezők az alábbiak:

- Dokumentum mérete. Mivel a dokumentum méretének növelése a olvasása, és írja be az adatokat is nő elfogyasztott mennyiség.
- A tulajdonság száma. Feltételezve alapértelmezett indexelés minden tulajdonság, a dokumentum írni elfogyasztott mennyiség mértékben fog nőni a tulajdonság száma növekszik.
- Adatok konzisztencia. Erős vagy határolt Staleness konzisztencia szintű adatok használata esetén a dokumentumok olvasásához további egységek fogyni fog.
- Az indexelt tulajdonság. A minden egyes webhelycsoport az index házirend azt határozza meg, mely tulajdonságokat indexelve vannak alapértelmezés szerint. A kérés egység felhasználási csökkentheti az indexelt tulajdonságok számának korlátozásával. 
- Az indexelés dokumentumot. Minden dokumentum automatikusan indexelt alapértelmezés szerint kevesebb kérelem egységek, ha úgy dönt, hogy nem index néhány dokumentum felhasználja meg.

További tudnivalókért lásd: a [DocumentDB kérelem egységek](documentdb-request-units.md). 

Ha például az alábbiakban táblázatos, hogy hány kérelem egységek kiépítése három másik dokumentumba (1KB, 4KB és 64KB) méretű BorderArt két különböző teljesítmény szinten (500 olvasás/második írások/második 100 és 500 olvasás/második + 500 írások/második). Az adatok konzisztencia konfigurálásáról a munkamenetet, és a indexelési házirend nincs értékre van állítva.

<table border="0" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td valign="top"><p><strong>Dokumentum mérete</strong></p></td>
            <td valign="top"><p><strong>Olvasás/második</strong></p></td>
            <td valign="top"><p><strong>Írások/második</strong></p></td>
            <td valign="top"><p><strong>Erőforrás-mennyiség kérése</strong></p></td>
        </tr>
        <tr>
            <td valign="top"><p>1 KB</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>100</p></td>
            <td valign="top"><p>(500 *1) + (100* 5) = 1 000 Licencelési/s</p></td>
        </tr>
        <tr>
            <td valign="top"><p>1 KB</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>(500 *5) + (100* 5) = 3000 Licencelési/s</p></td>
        </tr>
        <tr>
            <td valign="top"><p>4 KB</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>100</p></td>
            <td valign="top"><p>(500 *1.3-as) + (100* 7) = 1,350 Licencelési/s</p></td>
        </tr>
        <tr>
            <td valign="top"><p>4 KB</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>(500 *1.3-as) + (500* 7) = 4,150 Licencelési/s</p></td>
        </tr>
        <tr>
            <td valign="top"><p>64 KB</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>100</p></td>
            <td valign="top"><p>(500 *10) + (100* 48) = 9,800 Licencelési/s</p></td>
        </tr>
        <tr>
            <td valign="top"><p>64 KB</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>(500 *10) + (500* 48) = 29,000 Licencelési/s</p></td>
        </tr>
    </tbody>
</table>

Lekérdezések, tárolt eljárások és indítók a folyamatban lévő művelet komplexitását alapján kérelem egységek mobilalkalmazásokban Az alkalmazásokat fejleszt, mint vizsgálja meg a kérés ingyenesen fejléce megértéséhez, hogyan az egyes műveletek használata más kérelem egység kapacitása.  


## <a name="choice-of-consistency-level-and-throughput"></a>Egységesebb szint és átviteli megválasztása
Az alapértelmezett konzisztencia szint megválasztása az átvitel és a késés hatással van. Beállíthatja, hogy az alapértelmezett konzisztencia szint programozás útján és az Azure portálon keresztül. A konzisztencia szint kérés / alapon is bármikor felülbírálva. Alapértelmezés szerint a konzisztencia szint beállítása **munkamenet**, amely magában foglalja a monoton olvasási/írási, és olvassa el a írási garanciákkal. Munkamenet konzisztencia kiválóan alkalmasak felhasználói kötődnek alkalmazások és -ideális egyenlege egységesebb és a teljesítmény kompromisszumok biztosít.    

Az Azure portálon a konzisztencia szintjének módosításával kapcsolatos útmutatásért lásd: [a DocumentDB fiók kezelése](documentdb-manage-account.md#consistency). Vagy konzisztencia szintek a további tudnivalókért lásd: [használata konzisztencia szintek](documentdb-consistency-levels.md).

## <a name="provisioned-document-storage-and-index-overhead"></a>Tárterület és a tárgymutató terhelést kiépített dokumentum
DocumentDB egyetlen-partíciót és is particionált gyűjtemények létrehozását támogatja. Minden partíciót DocumentDB legfeljebb 10 GB-nyi biztonsági SSD tárhelyet támogatja. A 10GB-nyi dokumentumok tárolása a dokumentumok megnövelt tárhely az index tartalmazza. Alapértelmezés szerint egy DocumentDB webhelycsoport van beállítva automatikusan index valamennyi a dokumentum bármely másodlagos indexek vagy séma kifejezetten anélkül. A szokásos index terhelést 2 – 20 % között van DocumentDB használó alkalmazásai alapján. Az indexelési technológia DocumentDB által használt biztosítja, hogy az a tulajdonságokat értékeket, függetlenül a tárgymutató terhelést nem haladja meg a dokumentumok, az alapértelmezett beállítások méretének több, mint 80 %-át. 

Alapértelmezés szerint minden dokumentum indexelve vannak DocumentDB automatikusan. Jó helyen jár Ha azt szeretné, testre szabhatja az index terhelést, megadhatja indexelésből beszúrása vagy cseréje a dokumentumot, a időpontjában [DocumentDB indexelési házirendek](documentdb-indexing-policies.md)leírtak szerint egyes dokumentumok eltávolítása. Beállíthatja, hogy a webhelycsoport összes dokumentuma kizárása indexelésből DocumentDB gyűjteménye. Csak bizonyos tulajdonságok vagy a JSON-dokumentumok helyettesítő karakterek útneveket szelektív indexelendő [gyűjtemény az indexelési házirend beállítása](documentdb-indexing-policies.md#configuring-the-indexing-policy-of-a-collection)ismertetett módon DocumentDB gyűjtemény is beállítható. Is kizárhat tulajdonságok és a dokumentumok javítja a írási átviteli – ami azt jelenti, hogy kevesebb kérelem egységek felhasználja azt.   

## <a name="next-steps"></a>Következő lépések

Továbbra is a DocumentDB működéséről tanulási, olvassa el a [particionálására és Azure DocumentDB a méretezés](documentdb-partition-data.md)című témakört.

Az Azure portálon teljesítményszint figyelése, tanulmányozza [a DocumentDB fiók Monitor](documentdb-monitor-accounts.md). További tájékoztatást a webhelycsoportok teljesítményszint kiválasztása [a DocumentDB teljesítményszint](documentdb-performance-levels.md)megtekintése
 
