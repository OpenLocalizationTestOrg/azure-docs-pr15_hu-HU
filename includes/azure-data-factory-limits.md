Adatok gyári olyan helyen, hogy az ügyfél előfizetések védi az egymás munkaterhelésekből az alábbi alapértelmezett korlátokat tartalmazó több bérlői szolgáltatás. A korlátok számos is egyszerűen léptethető felfelé a maximális érték előfizetéshez tartozó ügyfélszolgálat megkeresésével. 

**Erőforrás** | **Alapértelmezett korlát** | **Maximális érték**
-------- | ------------- | -------------
adatok gyárak az Azure-előfizetésben | 50 | [Kapcsolatfelvétel az ügyfélszolgálattal](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/)
a folyamatok adatok gyár belül | 2 500 | [Kapcsolatfelvétel az ügyfélszolgálattal](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/)
adatok gyár belül adatkészleteket | 5000 | [Kapcsolatfelvétel az ügyfélszolgálattal](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/)
egy adathalmaz egyidejű szeletek | 10 | 10
egy objektum folyamat objektumok <sup>1</sup> bájt | 200 KB | 2000 KB
egy objektum adatkészlet és csatolt szolgáltatás objektumok <sup>1</sup> bájt | A 100 KB | 2000 KB
HDInsight igény szerinti fürt magmintákat belül egy <sup>2</sup> előfizetés | 48 | [Kapcsolatfelvétel az ügyfélszolgálattal](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/)
Felhőalapú adatok mozgását egység <sup>3</sup> | 8 | [Kapcsolatfelvétel az ügyfélszolgálattal](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/)
Ismételje meg a folyamat tevékenység fut száma | 1000 | MaxInt (32 bites)

<sup>1</sup> folyamat, adatkészlet és csatolt szolgáltatás objektumok jelenítik meg a terhelést logikai csoportja. Korlátozások az objektumok nem kapcsolódnak mennyiségű adat, áthelyezheti és feldolgozása az Azure Data Factory szolgáltatással. Adatok gyári méretezni petabytes adatok kezelésére szolgál.

<sup>2</sup> az igény szerinti HDInsight magmintákat az előfizetést, az adatok gyári tartalmazó ki van rendelve. Emiatt a fenti korlát adatok gyári vannak érvényben az igény szerinti HDInsight magmintákat core korlátját és eltér az Azure-előfizetéséhez társított fő korlátot.

<sup>3</sup> felhő adatok mozgását egység (DMU) használja a felhőbe a felhőbe fájlmásolás. Ez azt méri, amely a power (kombinációi Processzor, a memória és a hálózati erőforrás-hozzárendelés) az adatok Factory egyetlen egység. Újabb példány átviteli érhet el további DMUs-höz használt feljebb helyezése. Keresse meg a [felhőben adatok mozgását egységek](../../articles/data-factory/data-factory-copy-activity-performance.md#cloud-data-movement-units) szakasz részletei.

**Erőforrás** | **Alapértelmezett alsó határ** | **Minimális érték**
-------- | ------------------- | -------------
Ütemezési intervallum | 15 percet | 15 percet
Újrapróbálkozási kísérletek közötti intervallum | 1 másodperc | 1 másodperc
Ismételje meg az időkorlát | 1 másodperc | 1 másodperc


### <a name="web-service-call-limits"></a>Webes szolgáltatás hívás korlátai

Azure erőforrás-kezelő munkalapjai API-hívások. Az [Azure erőforrás-kezelő API korlátai](../azure-subscription-service-limits.md#resource-group-limits)belül sebessége API hívásokat is folytathat. 


