<properties 
    pageTitle="Egy rugalmas Adatbáziseszközök használatával shard hozzáadása |} Microsoft Azure" 
    description="Hogyan lehet új shards hozzáadása egy shard API-skála rugalmas hoz segítségével beállítása." 
    services="sql-database" 
    documentationCenter="" 
    manager="jhubbard" 
    authors="ddove" 
    editor=""/>

<tags 
    ms.service="sql-database" 
    ms.workload="sql-database" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="05/27/2016" 
    ms.author="ddove"/>

# <a name="adding-a-shard-using-elastic-database-tools"></a>Egy rugalmas Adatbáziseszközök használatával shard hozzáadása

## <a name="to-add-a-shard-for-a-new-range-or-key"></a>Egy új tartományban vagy kulcs shard hozzáadása  

Alkalmazások gyakran kell egyszerűen hozzáadhatja az új shards, amely várhatóan új kulcsok vagy kulcs tartományokat, amely már létezik shard térkép adatok kezeléséhez. Például egy bérlői azonosító szerint sharded alkalmazás szükség lehet egy új shard új bérlői webhely kiépítése, vagy adatok sharded havi egy új shard kiépítve minden új hónap megkezdése előtt szükséges. 

Az új helyzetű kulcs értékeit még nem egy meglévő hozzárendelés részét, érdemes az új shard hozzáadása és hozzárendelése az új kulcs vagy az adott shard tartomány rendkívül egyszerű. 

### <a name="example--adding-a-shard-and-its-range-to-an-existing-shard-map"></a>Példa: egy shard és a saját tartomány hozzáadása egy meglévő shard térképre
Ez a példa a [TryGetShard](https://msdn.microsoft.com/library/azure/dn823929.aspx) [CreateShard](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.createshard.aspx), [CreateRangeMapping](https://msdn.microsoft.com/library/azure/dn807221.aspx#M:Microsoft.Azure.SqlDatabase.ElasticScale.ShardManagement.RangeShardMap`1.CreateRangeMapping(Microsoft.Azure.SqlDatabase.ElasticScale.ShardManagement.RangeMappingCreationInfo{`0})) módszerek használ, és létrehoz egy példányát a [ShardLocation](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardlocation.shardlocation.aspx#M:Microsoft.Azure.SqlDatabase.ElasticScale.ShardManagement.ShardLocation.) osztály. Az alábbi példában **sample_shard_2** és belül, akkor az összes szükséges séma objektumok nevű adatbázis már létrehozott tartása, tartomány [300, 400).  

    // sm is a RangeShardMap object.
    // Add a new shard to hold the range being added. 
    Shard shard2 = null; 

    if (!sm.TryGetShard(new ShardLocation(shardServer, "sample_shard_2"),out shard2)) 
    { 
        shard2 = sm.CreateShard(new ShardLocation(shardServer, "sample_shard_2"));  
    } 

    // Create the mapping and associate it with the new shard 
    sm.CreateRangeMapping(new RangeMappingCreationInfo<long> 
                            (new Range<long>(300, 400), shard2, MappingStatus.Online)); 


Alternatívájaként a Powershell használatával hozzon létre egy új Shard térkép kezelő. Példa érhető el [az alábbi](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-DB-Elastic-731883db).
## <a name="to-add-a-shard-for-an-empty-part-of-an-existing-range"></a>Egy-egy üres részére egy meglévő tartomány shard hozzáadása  

Bizonyos körülmények között előfordulhat, hogy már tartomány rendelve egy shard és részben kitöltött az adatokkal, de szeretné ekkor közelgő adatait a egy másik shard irányítja. Például, hogy shard nap tartomány és a már kiosztott 50 napok kell egy shard, de a napon 24 szeretné először egy másik shard a jövőbeli adatait. A rugalmas adatbázis-kezelő [eszköz felosztása és egyesítése](sql-database-elastic-scale-overview-split-and-merge.md) e művelet végrehajtása, de ha adatok mozgását nem szükséges (például a tartomány a napok [25, 50 adatok), azaz, nap 25 végpontokat is beleértve – 50-kizáró, még nem létezik) végezheti el a teljes egészében közvetlenül az Shard térkép kiszolgálói API-ja használatával.

### <a name="example-splitting-a-range-and-assigning-the-empty-portion-to-a-newly-added-shard"></a>Példa: egy tartományban felosztása, és az üres rész hozzárendelése egy újonnan hozzáadott shard

"Sample_shard_2" és az összes szükséges séma objektumok belül azt nevű adatbázis lettek létrehozva.  

 
    // sm is a RangeShardMap object.
    // Add a new shard to hold the range we will move 
    Shard shard2 = null; 

    if (!sm.TryGetShard(new ShardLocation(shardServer, "sample_shard_2"),out shard2)) 
    { 
    
        shard2 = sm.CreateShard(new ShardLocation(shardServer, "sample_shard_2"));  
    } 

    // Split the Range holding Key 25 

    sm.SplitMapping(sm.GetMappingForKey(25), 25); 

    // Map new range holding [25-50) to different shard: 
    // first take existing mapping offline 
    sm.MarkMappingOffline(sm.GetMappingForKey(25)); 
    // now map while offline to a different shard and take online 
    RangeMappingUpdate upd = new RangeMappingUpdate(); 
    upd.Shard = shard2; 
    sm.MarkMappingOnline(sm.UpdateMapping(sm.GetMappingForKey(25), upd)); 

**Fontos**: Ez a módszer használja, csak ha biztos abban, hogy tartományát a frissített hozzárendelés üres.  A fenti módszerek ne jelölje be a tartomány helyezi át, adatok, célszerű az ellenőrzések szerepeltetni a kódot.  Ha a sorok szerepel a tartomány helyezi át, a a tényleges adatok eloszlás nem egyezik a frissített shard térkép. A művelet elvégzéséhez inkább ebben az esetben a [felosztása és egyesítése eszköz](sql-database-elastic-scale-overview-split-and-merge.md) használata  


[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]
 
