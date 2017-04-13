<properties
    pageTitle="A Python táblatároló használata |} Microsoft Azure"
    description="Strukturált adatokat tárolja a felhőben Azure táblatárolóhoz, egy NoSQL adattár használatával."
    services="storage"
    documentationCenter="python"
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="python"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="tamram"/>


# <a name="how-to-use-table-storage-from-python"></a>A Python táblatároló használata

[AZURE.INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-tables](../../includes/storage-try-azure-tools-tables.md)]

## <a name="overview"></a>– Áttekintés

Ez az útmutató megtudhatja, miként végezheti el a gyakori alkalmazási területek az Azure-táblából tárhelyszolgáltatáshoz használatával. A minták Python írt, és használja a [Microsoft Azure tároló SDK Python tartalmaz]. Az érintett esetek létrehozása és törlése a táblázat beszúrása és lekérdezése személyek egy táblázat mellett.

[AZURE.INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-table"></a>Táblázat létrehozása

A **TableService** objektum lehetővé teszi a táblázat szolgáltatások használata. A következő kódot létrehoz egy **TableService** -objektumot. A kód felső részén található összes Python fájlt, amelyben el szeretné érni programozás útján a Azure tárhely hozzáadása:

    from azure.storage.table import TableService, Entity

A következő kódot objektumot hoz létre **TableService** a tárterület-fiók nevét és a fiók gombbal.  Cserélje "saját fiók" és "SajátKulcs" a fiók nevét és billentyűt.

    table_service = TableService(account_name='myaccount', account_key='mykey')

    table_service.create_table('tasktable')

## <a name="add-an-entity-to-a-table"></a>Egy személy hozzáadása táblához

Entitás felvenni, először hozzon létre egy szótár vagy entitás, amely meghatározza a szervezet tulajdonságok neve és az értékeket. Figyelje meg, hogy minden egységek, meg kell adnia **PartitionKey** és **RowKey**. Ezek a szervezetek egyedi azonosítóját. Ezek az értékek sokkal gyorsabban végrehajthatók, mint a más tulajdonságokat is lekérdezhetők használatával is lekérdezhetők. A rendszer **PartitionKey** használ osztja fel automatikusan a táblázat személyek fölé sok tároló csomópontot.
Ugyanazon a csomóponton tárolja az azonos **PartitionKey** rendelkező szervezetek. **RowKey** belül a partíciót, amely tartozik a szervezet egyedi azonosítója.

A sikeres egy szótár objektum entitás hozzáadni a táblázathoz, a **beszúrása\_entitás** módot.

    task = {'PartitionKey': 'tasksSeattle', 'RowKey': '1', 'description' : 'Take out the trash', 'priority' : 200}
    table_service.insert_entity('tasktable', task)

A **szervezet** osztály egy példányának is átadhatja az **beszúrása\_entitás** módot.

    task = Entity()
    task.PartitionKey = 'tasksSeattle'
    task.RowKey = '2'
    task.description = 'Wash the car'
    task.priority = 100
    table_service.insert_entity('tasktable', task)

## <a name="update-an-entity"></a>Egy személy frissítése

Ez a kód megtudhatja, hogy miként cserélje ki a meglévő entitás korábbi verziója nem frissült.

    task = {'PartitionKey': 'tasksSeattle', 'RowKey': '1', 'description' : 'Take out the garbage', 'priority' : 250}
    table_service.update_entity('tasktable', 'tasksSeattle', '1', task, content_type='application/atom+xml')

Ha a szervezet frissítendő nem létezik, majd a frissítés sikertelen lesz. Ha azt szeretné, függetlenül attól, hogy létezik előtt entitás tárolni, használhatja a **beszúrása\_vagy\_replace_entity**.
A következő példában az első hívás lecseréli a meglévő entitás. A második hívás szúrja be új egységek, mivel a megadott **PartitionKey** nincs egyed és **RowKey** létezik a táblában.

    task = {'PartitionKey': 'tasksSeattle', 'RowKey': '1', 'description' : 'Take out the garbage again', 'priority' : 250}
    table_service.insert_or_replace_entity('tasktable', 'tasksSeattle', '1', task, content_type='application/atom+xml')

    task = {'PartitionKey': 'tasksSeattle', 'RowKey': '3', 'description' : 'Buy detergent', 'priority' : 300}
    table_service.insert_or_replace_entity('tasktable', 'tasksSeattle', '1', task, content_type='application/atom+xml')

## <a name="change-a-group-of-entities"></a>Személyek, csoportok

Egyes esetekben célszerű elküldése több művelet egy köteg atomi a kiszolgáló által feldolgozása biztosítása érdekében a közös. Elvégezheti, hogy használja-e a **TableBatch** osztály. Ha szeretné, hogy a köteg küldhetik, hívja **Jóváhagyás\_köteg**. Figyelje meg, hogy minden entitás kell lennie az azonos partíciót annak érdekében, hogy egy köteg megváltozhat. Az alábbi példa összeadja a két entitás kötegben.

    from azure.storage.table import TableBatch
    batch = TableBatch()
    task10 = {'PartitionKey': 'tasksSeattle', 'RowKey': '10', 'description' : 'Go grocery shopping', 'priority' : 400}
    task11 = {'PartitionKey': 'tasksSeattle', 'RowKey': '11', 'description' : 'Clean the bathroom', 'priority' : 100}
    batch.insert_entity(task10)
    batch.insert_entity(task11)
    table_service.commit_batch('tasktable', batch)

Kötegeket is használható a segítő felsorolása manager szintaxis szerint:

    task12 = {'PartitionKey': 'tasksSeattle', 'RowKey': '12', 'description' : 'Go grocery shopping', 'priority' : 400}
    task13 = {'PartitionKey': 'tasksSeattle', 'RowKey': '13', 'description' : 'Clean the bathroom', 'priority' : 100}

    with table_service.batch('tasktable') as batch:
        batch.insert_entity(task12)
        batch.insert_entity(task13)


## <a name="query-for-an-entity"></a>Lekérdezés entitás

A lekérdezés entitás egy táblázatban, használja a **első\_entitás** módszer **PartitionKey** és **RowKey**megkerülhetők.

    task = table_service.get_entity('tasktable', 'tasksSeattle', '1')
    print(task.description)
    print(task.priority)

## <a name="query-a-set-of-entities"></a>A lekérdezés személyek csoportja

Ebben a példában a Seattle az összes tevékenységhez **PartitionKey**alapján találja meg.

    tasks = table_service.query_entities('tasktable', filter="PartitionKey eq 'tasksSeattle'")
    for task in tasks:
        print(task.description)
        print(task.priority)

## <a name="query-a-subset-of-entity-properties"></a>A lekérdezés csak egy részhalmazát entitás tulajdonságai

Táblázat lekérdezés néhány tulajdonságok beolvasható entitás.
Ez a módszer *kivetítés*nevű csökkenti a sávszélesség, és javíthatja a lekérdezési teljesítményt, különösen a nagyobb szervezetek. **Válassza a** paraméter használatával, és az ügyfél előtérbe kívánt tulajdonságainak azoknak a át.

A lekérdezés, a következő kódrészlet csak személyek leírását a táblázat adja eredményül.

[AZURE.NOTE] A következő kódtöredékének működik, csak a felhőalapú tárhelyszolgáltatáshoz szemben. Ez a tárhely irányító által nem támogatott.

    tasks = table_service.query_entities('tasktable', filter="PartitionKey eq 'tasksSeattle'", select='description')
    for task in tasks:
        print(task.description)

## <a name="delete-an-entity"></a>Entitás törlése

A partíciók, és a sor gombbal entitás törölheti.

    table_service.delete_entity('tasktable', 'tasksSeattle', '1')

## <a name="delete-a-table"></a>Táblázat törlése

A következő kódot táblázat törlése a tárterület-fiókból.

    table_service.delete_table('tasktable')

## <a name="next-steps"></a>Következő lépések

Most, hogy megtanulta táblatároló alapjait, kövesse az alábbi hivatkozásokra kattintva további.

- [Python Developer Center](/develop/python/)
- [Azure tárolása REST API-val](http://msdn.microsoft.com/library/azure/dd179355)
- [Azure tároló csapatának blogja]
- [Microsoft Azure tároló Python SDK]

[Azure tároló csapatának blogja]: http://blogs.msdn.com/b/windowsazurestorage/
[Microsoft Azure tároló Python SDK]: https://github.com/Azure/azure-storage-python
