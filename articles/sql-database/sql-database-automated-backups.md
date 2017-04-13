<properties
   pageTitle="SQL-adatbázis biztonsági másolatok - automatikus, geo felesleges |} Microsoft Azure" 
   description="SQL-adatbázis automatikusan létrehozza a helyi adatbázis biztonsági másolatot öt percenként, és Azure-olvasásra geo felesleges tároló (TS-GRS) geo redundancia használatával. "
   services="sql-database"
   documentationCenter=""
   authors="CarlRabeler"
   manager="jhubbard"
   editor="monicar"/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/20/2016"
   ms.author="carlrab;barbkess"/>

<!------------------
This topic is annotated with TEMPLATE guidelines for FEATURE TOPICS.


Metadata guidelines

pageTitle
    60 characters or less. Includes name of the feature - primary benefit. Not the same as H1. Its 60 characters or fewer including all characters between the quotes and the Microsoft Azure site identifier.

description
    115-145 characters. Duplicate of the first sentence in the introduction. This is the abstract of the article that displays under the title when searching in Bing or Google. 

    Example: "SQL Database automatically creates a local database backup every few minutes and uses Azure read-access geo-redundant storage for geo-redundancy."
------------------>

<!----------------

TEMPLATE GUIDELINES for feature topics

The Feature Topic is a one-pager (ok, sometimes longer) that explains a capability of the product or service. It explains what the capability is and characteristics of the capability.  

It is a "learning" topic, not an action topic.

DO explain this:
    • Definition of the feature terminology.  i.e., What is a database backup?
    • Characteristics and capabilities of the feature. (How the feature works)
    • Common uses with links to overview topics that recommend when to use the feature.
    • Reference specifications (Limitations and Restrictions, Permissions, General Remarks, etc.)
    • Next Steps with links to related overviews, features, and tasks.

DON'T explain this:
    • How to steps for using the feature (Tasks)
    • How to solve business problems that incorporate the feature (Overviews)
------------------->

<!------------------
GUIDELINES for the H1 
    
    The H1 should answer the question "What is in this topic?" Write the H1 heading in conversational language and use search key words as much as possible. Since this is a learning topic, make sure the title indicates that and doesn't mislead people to think this will tell them how to do tasks.  
    
    To help people understand this is a learning topic and not an action topic, start the title with "Learn about ... "

    Heading must use an industry standard term. If your feature is a proprietary name like "Elastic database pools", use a synonym. For example:    "Learn about elastic database pools for multi-tenant databases". In this case multi-tenant database is the industry-standard term that will be an anchor for finding the topic.

-------------------->

# <a name="learn-about-sql-database-backups"></a>További tudnivalók: az SQL-adatbázis biztonsági mentése

<!------------------
    GUIDELINES for introduction
    
    The introduction is 1-2 sentences.  It is optimized for search and sets proper expectations about what to expect in the article. It should contain the top key words that you are using throughout the article.The introduction should be brief and to the point of what the feature is, what it is used for, and what's in the article. 

    If the introduction is short enough, your article can pop to the top in Google Instant Answers.

    In this example:
    
 

Sentence #1 Explains what the article will cover, which is what the feature is or does. This is also the metadata description. 
    SQL Database automatically creates a local database backup every five minutes and uses Azure read-access geo-redundant storage (RA-GRS) to provide geo-redundancy. 

Sentence #2 Explains why I should care about this.  
    Database backups are an essential part of any business continuity and disaster recovery strategy because they protect your data from accidental corruption or deletion.

-------------------->

SQL-adatbázis automatikusan létrehozza a helyi adatbázis biztonsági másolatot minden néhány percet, és Azure-olvasásra geo felesleges tárterületet használ a redundancia geo. Adatbázis biztonsági mentése minden üzleti folytonosságot és katasztrófa helyreállítási stratégia alapvető részét képezik, mert azok az adatok védelme biztonsági véletlen sérülése vagy törlését. 

<!-- This image needs work, so not putting it in right now.

This diagram shows SQL Database running in the US East region. It creates a database backup every five minutes, which it stores locally to Azure Read Access Geo-redundant Storage (RA-GRS). Azure uses geo-replication to copy the database backups to a paired data center in the US West region.

![geo-restore](./media/sql-database-geo-restore/geo-restore-1.png)

-->

<!---------------
GUIDELINES for the first ## H2.

    The first ## describes what the feature encompasses and how it is used. It points to related task articles.
    
    For consistency, being the heading with "What is ... "
----------------->

## <a name="what-is-a-sql-database-backup"></a>Mi az SQL-adatbázis biztonsági másolatának?  

<!-- 
    Explains what a SQL Database backup is and answers an important question that people want to know.
-->

SQL-adatbázis biztonsági másolatának helyi adatbázis biztonsági mentése és geo felesleges biztonsági másolatok is tartalmaz. A biztonsági mentése automatikusan és külön költség nélkül jönnek létre. Nem kell tennie semmit sem, fordulhat elő, hogy.

<!----------------- 
    Explains first component of the backup feature
------------------>

Helyi biztonsági másolatok SQL-adatbázis használja az SQL Server-technológia [teljes](https://msdn.microsoft.com/library/ms186289.aspx) [megkülönböztető](https://msdn.microsoft.com/library/ms175526.aspx )és [tranzakciónaplója](https://msdn.microsoft.com/library/ms191429.aspx) biztonsági másolatok létrehozására. Biztonsági másolatok tranzakció napló fordulhat elő, öt percenként, amely lehetővé teszi, hogy végezze el a pont és az idő visszaállítása az adatbázist tároló ugyanarra a kiszolgálóra. Amikor visszaállít egy adatbázist, a szolgáltatás számadatok, melyik biztonsági másolatok visszaállítandó teljes differenciál és tranzakció naplót meg.

<!--------------- 
    Explicit list of what to do with a local backup. "Use a ..." helps people to scan the topic and find the uses quickly.
---------------->

A helyi adatbázis biztonsági mentése használja:

- Adatbázis visszaállítása az adatmegőrzési időszak belül pont-a-időt. Az adatbázis biztonsági másolatának visszaállítása az adatbázis pont-a-időt, törölt adatbázis visszaállítása az időhöz törléséből eredő hibák, vagy visszaállít egy adatbázist, hogy egy másik földrajzi területhez tartozik. A visszaállítás végrehajtásához, olvassa el a [egy adatbázis biztonsági mentése az adatbázis visszaállítása](sql-database-recovery-using-backups.md)című témakört.

- Másolja a ugyanazon vagy másik tartományban lévő SQL server adatbázis. A Másolás tranzakción keresztül egységesek a jelenlegi SQL-adatbázissal. Végezze el a másolás, olvassa el az [adatbázis-példányt](sql-database-copy.md).

- Adatbázis biztonsági másolatának túl a biztonsági másolat az adatmegőrzési időszak archiválni. Végrehajtásához-archívumba, a fájl [exportálása egy BACPAC SQL-adatbázishoz](sql-database-export.md) . A hosszú távú tárolóhoz BACPAC archiválása, majd az adatmegőrzési időszak túl tárolja azt. Vagy a BACPAC segítségével átadása egy másolatot az adatbázisról, vagy a helyszíni SQL Server vagy az Azure virtuális gépen (virtuális).

<!----------------- 
    Explains first component of the backup feature
------------------>

Geo felesleges biztonsági másolatok az SQL-adatbázis [Azure tároló replikációs](../storage/storage-redundancy.md)használja. SQL-adatbázis helyi adatbázis biztonsági másolatok tárolja az [Olvasásra Geo felesleges tároló (TS-GRS)](../storage/storage-redundancy.md#read-access-geo-redundant-storage) -fiókjában. Azure [párosított adatközpont](../best-practices-availability-paired-regions.md)replikálja a biztonságimásolat-fájlokat. 

<!--------------- 
    Explicit list of what to do with a geo-redundant backup. "Use a ..." helps people to scan the topic and find the uses quickly.
---------------->

A geo felesleges biztonsági másolatot szeretne használni:

- Adatbázis visszaállítása egy másik földrajzi területhez tartozik, abban az esetben, ha nem tud hozzáférni az adatbázis biztonsági mentése az elsődleges adatbázis területről. 

>[AZURE.NOTE] Azure-tárolóban lévő a szerződési időszak *replikációs* fájlok másolása egyik helyről a másikra hivatkozik. SQL- *adatbázis-replikáció* szinkronizálja az elsődleges adatbázis több másodlagos adatbázisokhoz való megőrzési hivatkozik. 

<!----------------
    The next ## H2's discuss key characteristics of how the feature works. The title is in conversational language and asks the question that will be answered.
------------------->
## <a name="how-much-backup-storage-is-included-at-no-cost"></a>Biztonsági másolat tárhely megtalálható áttérhet?

SQL-adatbázis a maximális kiépített adatbázist tároló 200 %-os biztosít a biztonsági másolat tárolására külön költség nélkül. Ha például van egy normál DB példánya kiépített DB méretű 250 GB, esetén 500 GB-nyi biztonságimásolat külön költség nélkül. Ha az adatbázis meghaladja a megadott biztonsági másolat tárolási, megadhatja az adatmegőrzési időszak csökkentése Azure ügyfélszolgálat megkeresésével. Másik lehetőségként a fizetés fölösleges biztonságimásolat díjazott az olvasásra – földrajzilag felesleges tároló (TS-GRS) számlázott. 

## <a name="how-often-do-backups-happen"></a>Milyen gyakran történhet a biztonsági másolatok?

Helyi adatbázis biztonsági másolatok teljes adatbázis biztonsági mentése fordulhat elő, heti, megkülönböztető adatbázis biztonsági mentése fordulhat elő, óránkénti, és a biztonsági másolatok fordulhat elő, öt percenként tranzakciónaplója. Az első teljes biztonsági másolat adatbázis létrehozása után közvetlenül van ütemezve. 30 percen belüli általában befejeződik, de tarthat, ha az adatbázis jelentős méretű. Ha például a kezdeti biztonsági mentés több időt vesz igénybe a visszaállított adatbázist, vagy az adatbázis biztonsági másolatának. Az első teljes biztonsági mentés után minden további másolatok automatikusan ütemezett és felügyelt meg automatikusan a háttérben. A teljes és [eltérő](https://msdn.microsoft.com/library/ms175526.aspx) adatbázis biztonsági mentése pontos időzítésének azt egyenlege az általános munkaterhelés határozza meg. 

Geo felesleges biztonsági másolatok a teljes és megkülönböztető biztonsági másolatok Azure tároló az ütemezésben megfelelően kerülnek.

## <a name="how-long-do-you-keep-my-backups"></a>Mennyi ideig megtartja a biztonsági másolatok?

Minden egyes SQL-adatbázis biztonsági mentése az adatbázis a [szolgáltatási-réteg](sql-database-service-tiers.md) alapuló adatmegőrzési időtartam tartozik. Az adatmegőrzési időszak az adatbázis a:

<!------------------

    Using a list so the information is easy to find when scanning.
------------------->

- Réteg egyszerű szolgáltatás hét napig tart.
- Szokásos szolgáltatás réteg 35 napig tart.
- Prémium szolgáltatási réteg 35 napig tart.


Az adatbázist a normál vagy prémium szolgáltatási rétegek egyszerű vissza léptetheti akkor, ha a biztonsági másolatok hét napig. 7 napnál régebbi meglévő másolatok már nem érhető el. 

Ha az adatbázis rendszerről az egyszerű szolgáltatási réteg normál vagy prémium, a SQL-adatbázis meglévő biztonsági másolatok továbbra is mindaddig, amíg a 35 napnál régebbi. Új biztonsági másolatok megjelenésükkor 35 napig tart.
 
Ha töröl egy adatbázist, SQL-adatbázis online adatbázisok tenné ugyanúgy továbbra is a biztonsági másolatok. Tegyük fel, hogy törli az adatmegőrzési időszak hét napig tartalmazó egyszerű adatbázis. A biztonsági másolatot, amely négy napnál régebbi három több napig menti a program.

>[AZURE.IMPORTANT]
    Ha törli az Azure SQL-kiszolgálót, hogy tárolja az SQL-adatbázisok, adatbázisokra, amelyek a kiszolgáló tartoznak is törlődik, és nem állíthatók. A Törölt kiszolgáló nem tudja visszaállítani.

<!-------------------
OPTIONAL section
## Best practices 
--------------------->

<!-------------------
OPTIONAL section
## General remarks
--------------------->

<!-------------------
OPTIONAL section
## Limitations and restrictions
--------------------->

<!-------------------
OPTIONAL section
## Metadata
--------------------->

<!-------------------
OPTIONAL section
## Performance
--------------------->

<!-------------------
OPTIONAL section
## Permissions
--------------------->

<!-------------------
OPTIONAL section
## Security
--------------------->

<!-------------------
GUIDELINES for Next Steps

    The last section is Next Steps. Give a next step that would be relevant to the customer after they have learned about the feature and the tasks associated with it.  Perhaps point them to one or two key scenarios that use this feature.

    You don't need to repeat links you have already given them.
--------------------->

## <a name="next-steps"></a>Következő lépések

Adatbázis biztonsági mentése minden üzleti folytonosságot és katasztrófa helyreállítási stratégia alapvető részét képezik, mert azok az adatok védelme biztonsági véletlen sérülése vagy törlését. Hogy hogyan adatbázis biztonsági másolatok be egy szélesebb stratégia [áttekintése](sql-database-business-continuity.md)című témakörben találhat üzleti folytonosságot.


