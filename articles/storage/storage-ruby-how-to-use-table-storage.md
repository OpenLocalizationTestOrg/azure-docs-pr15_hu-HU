<properties
    pageTitle="A fonetikus Azure Táblatárolóhoz használata |} Microsoft Azure"
    description="Strukturált adatokat tárolja a felhőben Azure táblatárolóhoz, egy NoSQL adattár használatával."
    services="storage"
    documentationCenter="ruby"
    authors="tamram"
    manager="carmonm"
    editor=""/>
<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="ruby"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="tamram"/>


# <a name="how-to-use-azure-table-storage-from-ruby"></a>A fonetikus Azure Táblatárolóhoz használata

[AZURE.INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-tables.md)]

## <a name="overview"></a>– Áttekintés

Ez az útmutató megtudhatja, miként végezheti el a gyakori alkalmazási területek, az Azure-táblából szolgáltatással. A minták kerülnek a fonetikus API. Érintett esetek **hozhat létre és töröl egy táblát, beszúrása és lekérdezése személyek egy táblázatban**.

[AZURE.INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-ruby-application"></a>Fonetikus-alkalmazás létrehozása

Utasításokért fonetikus-alkalmazás létrehozása [az Azure virtuális sínek webalkalmazás a fonetikus](../virtual-machines/linux/classic/virtual-machines-linux-classic-ruby-rails-web-app.md)tudnivalókat.


## <a name="configure-your-application-to-access-storage"></a>A tároló eléréséhez konfigurálása

Azure tároló használata esetén szüksége letöltéséről és használatáról a fonetikus azure csomagot tartalmaz, amely a többi tároló szolgáltatások kommunikálni kényelmesebbé tárak.

### <a name="use-rubygems-to-obtain-the-package"></a>A csomag beszerzése RubyGems használatával

1. Használja a parancssor, például **PowerShell** (Windows), a **terminált** (Mac) vagy a **Bash** (Unix).

2. Írja be a **gem telepítse az azure** a parancssorablakban telepítheti a gem és függőségek.

### <a name="import-the-package"></a>A csomag importálása

A kedvenc szövegszerkesztővel, és adja meg a fonetikus fájlt, ahol tároló használni kívánt tetején a következő:

    require "azure"

## <a name="set-up-an-azure-storage-connection"></a>Tárterület Azure-alapú beállítása

Az azure modul felirat jelenik meg a környezeti változók **AZURE\_tároló\_fiók** és **AZURE\_tároló\_ACCESS\_kulcs** az Azure tároló fiókhoz való csatlakozáshoz szükséges információkat. Ha ezek a környezeti változók nincsenek beállítva, meg kell adnia a fiók adatait, mielőtt **Azure::TableService** használata a következő kódot:

    Azure.config.storage_account_name = "<your azure storage account>"
    Azure.config.storage_access_key = "<your azure storage access key>"

Ezek az értékek beolvasása a klasszikus vagy az erőforrás-kezelő tároló fiókot az Azure-portálon:

1. Jelentkezzen be az [Azure-portálon](https://portal.azure.com).
2. Nyissa meg a használni kívánt tárterület-fiókjába.
3. Kattintson a jobb oldalon a beállítások lap, a **Hívóbillentyűk**.
4. Az Access billentyűk lap látható láthatja, a hívóbetű 1 és 2 hívóbetű. Bármelyikéhez használhatja. 
5. Kattintson a Másolás ikonra a kulcs másolja a vágólapra. 

Ezek az értékek beszerzése a klasszikus Azure portálon klasszikus tárterület-fiókból:

1. Jelentkezzen be a [Klasszikus Azure portálon](https://manage.windowsazure.com).
2. Nyissa meg a használni kívánt tárterület-fiókjába.
3. Kattintson a **HÍVÓBETŰK kezelése** a navigációs ablak alján.
4. Az előugró párbeszédpanel látni fogja a tárhely fiók nevét, az access elsődleges kulcs és másodlagos hívóbetű. Az access-billentyűt vagy az elsődleges adott, vagy a másodlagos is is használhatja. 
5. Kattintson a Másolás ikonra a kulcs másolja a vágólapra.

## <a name="create-a-table"></a>Táblázat létrehozása

A **Azure::TableService** objektum lehetővé teszi a táblák és a szervezetek. Egy táblázat létrehozásához használja a **létrehozása\_table()** módot. Az alábbi példa létrehoz egy tábla vagy nyomtassa ki a hibát, ha bármelyik.

    azure_table_service = Azure::TableService.new
    begin
      azure_table_service.create_table("testtable")
    rescue
      puts $!
    end

## <a name="add-an-entity-to-a-table"></a>Egy személy hozzáadása táblához

Egy személy felvételéhez hozzon létre, amely meghatározza a szervezet tulajdonságainak ujjlenyomat-objektumot. Figyelje meg, hogy minden entitás meg kell adnia egy **PartitionKey** és **RowKey**. Ezek a egyedi azonosítókat, a személyek, és értékek sokkal gyorsabb, mint a többi tulajdonságok lekérdezett. Azure tároló **PartitionKey** használja osztja fel automatikusan a táblázat szervezetek fölé sok tároló csomópontot. Ugyanazon a csomóponton tárolja az azonos **PartitionKey** rendelkező szervezetek. A **RowKey** belül a partíciót tartozik a szervezet egyedi azonosítója.

    entity = { "content" => "test entity",
      :PartitionKey => "test-partition-key", :RowKey => "1" }
    azure_table_service.insert_entity("testtable", entity)

## <a name="update-an-entity"></a>Egy személy frissítése

Több rendelkezésre állnak módszerek a meglévő entitás frissítése:

* **frissítése\_entity():** Frissítse a meglévő entitás C2: C7 alakú azt.
* **Egyesítés\_entity():** Új tulajdonság értékek egyesítése a meglévő entitás frissíti a meglévő entitás.
* **beszúrása\_vagy\_Egyesítés\_entity():** Meglévő entitás frissíti a C2: C7 alakú azt. Ha nincs entitás létezik, a program beszúrja egy újat:
* **beszúrása\_vagy\_cseréje\_entity():** Új tulajdonság értékek egyesítése a meglévő entitás frissíti a meglévő entitás. Ha nincs entitás létezik, a program beszúrja egy újat.

Az alábbi példa bemutatja, hogy egy egyed használatával frissítése **frissítése\_entity()**:

    entity = { "content" => "test entity with updated content",
      :PartitionKey => "test-partition-key", :RowKey => "1" }
    azure_table_service.update_entity("testtable", entity)

A **frissítése\_entity()** és **Egyesítés\_entity()**, ha a frissíteni kívánt személy nem létezik, majd a frissítés sikertelen lesz. Ezért szeretne tárolni, függetlenül attól, hogy már létezik ilyen entitás, ha helyett használjon **beszúrása\_vagy\_cseréje\_entity()** vagy **beszúrása\_vagy\_Egyesítés\_entity()**.

## <a name="work-with-groups-of-entities"></a>Személyek, csoportok kezelése

Egyes esetekben célszerű elküldése több művelet egy köteg atomi a kiszolgáló által feldolgozása biztosítása érdekében a közös. Elvégezheti, hogy, először hozzon létre egy **Köteg** objektumot, és alkalmazza a **végrehajtása\_batch()** **TableService**módszerrel. A következő példa bemutatja, hogyan elküldése két entitás RowKey 2 és 3 kötegben. Figyelje meg, hogy csak az ugyanazt a PartitionKey rendelkező szervezetek működik.

    azure_table_service = Azure::TableService.new
    batch = Azure::Storage::Table::Batch.new("testtable",
      "test-partition-key") do
      insert "2", { "content" => "new content 2" }
      insert "3", { "content" => "new content 3" }
    end
    results = azure_table_service.execute_batch(batch)

## <a name="query-for-an-entity"></a>Lekérdezés entitás

A lekérdezés entitás egy táblázatban, használja a **első\_entity()** módszer a táblanév **PartitionKey** és **RowKey**megkerülhetők.

    result = azure_table_service.get_entity("testtable", "test-partition-key",
      "1")

## <a name="query-a-set-of-entities"></a>A lekérdezés személyek csoportja

A lekérdezés egy sor olyan személyek egy táblázatban, hozzon létre egy lekérdezés kivonat objektumot, és használja a **lekérdezés\_entities()** módot. Az alábbi példa bemutatja, hogy az azonos **PartitionKey**az összes entitás első:

    query = { :filter => "PartitionKey eq 'test-partition-key'" }
    result, token = azure_table_service.query_entities("testtable", query)

> [AZURE.NOTE] Ha az eredményhalmaz van egyetlen lekérdezés vissza, egy folytatását jelző token visszatér beolvasni a későbbi lapok is használhatja, amely túl nagy.

## <a name="query-a-subset-of-entity-properties"></a>A lekérdezés csak egy részhalmazát entitás tulajdonságai

Táblázat lekérdezés néhány tulajdonságok beolvasható entitás. Ez a módszer, úgynevezett "kivetítés", csökkenti a sávszélesség, és javíthatja a lekérdezési teljesítményt, különösen a nagyobb szervezetek. A select záradék használata, és szeretné, hogy az ügyfél előtérbe tulajdonságainak azoknak a át.

    query = { :filter => "PartitionKey eq 'test-partition-key'",
      :select => ["content"] }
    result, token = azure_table_service.query_entities("testtable", query)

## <a name="delete-an-entity"></a>Entitás törlése

Egy személy törléséhez használja a **törlése\_entity()** módot. Meg kell adják át a táblában, amely tartalmazza a szervezet a PartitionKey és RowKey a személy neve.

        azure_table_service.delete_entity("testtable", "test-partition-key", "1")

## <a name="delete-a-table"></a>Táblázat törlése

Táblázat törléséhez használja a **törlése\_table()** módot és pass a törölni kívánt tábla neve.

        azure_table_service.delete_table("testtable")

## <a name="next-steps"></a>Következő lépések

Összetettebb tároló feladatok kapcsolatos további tudnivalókért kövesse ezeket a hivatkozásokat:

- [Azure tároló csapatának blogja](http://blogs.msdn.com/b/windowsazurestorage/)
- [Azure SDK fonetikus](http://github.com/WindowsAzure/azure-sdk-for-ruby) tárházából GitHub
