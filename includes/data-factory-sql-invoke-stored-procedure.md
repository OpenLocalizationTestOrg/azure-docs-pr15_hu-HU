## <a name="invoking-stored-procedure-for-sql-sink"></a>Az SQL-gyűjtése meghívása a tárolt eljárás

Adatok másolása, áthelyezése SQL Server vagy Azure SQL-vagy SQL Server-adatbázishoz, amikor a felhasználó által megadott tárolt eljárás sikerült konfigurálható és további paraméterrel indított. 

A tárolt eljárás valószínűleg kihasználják, ha a beépített másolás mechanizmusok nem szolgálják célját. Ez akkor kapcsolatos általában károkozásra, el kell végezni, mielőtt a céltábla munkalapon tárolt forrásadatok végleges beillesztése (többtáblás... való beillesztését további érték keresése az oszlopok egyesítése) igényeinek extra feldolgozása közben. 

A tárolt eljárás megválasztott alkalmazhatja. A következő példa bemutatja, hogyan végezze el az adatbázis táblába egy egyszerű kurzor a tárolt eljárás használatával. 

**Kimeneti adatkészlet**

Ebben a példában típusának beállítása: SqlServerTable. Állítsa be az Azure SQL-adatbázis használata AzureSqlTable. 

    {
      "name": "SqlOutput",
      "properties": {
        "type": "SqlServerTable",
        "linkedServiceName": "SqlLinkedService",
        "typeProperties": {
          "tableName": "Marketing"
        },
        "availability": {
          "frequency": "Hour",
          "interval": 1
        }
      }
    }
    
Adja meg a SqlSink szakasz másolása tevékenység JSON az alábbi képlettel történik. Ha fel szeretne hívni a tárolt eljárás közben beszúrni az adatokat, SqlWriterStoredProcedureName és a SqlWriterTableType tulajdonság van szükség.

    "sink":
    {
        "type": "SqlSink",
        "SqlWriterTableType": "MarketingType",
        "SqlWriterStoredProcedureName": "spOverwriteMarketing", 
        "storedProcedureParameters":
                {
                    "stringData": 
                    {
                        "value": "str1"     
                    }
                }
    }

Az adatbázis meghatározásával SqlWriterStoredProcedureName a tárolt eljárás a névvel. A bemeneti adatokat a megadott forrás és a Beszúrás kezeli a kimeneti táblázatba. Figyelje meg, hogy a tárolt eljárás paraméter neve legyen ugyanaz, mint a tábla JSON fájlban definiált táblanév.

    CREATE PROCEDURE spOverwriteMarketing @Marketing [dbo].[MarketingType] READONLY, @stringData varchar(256)
    AS
    BEGIN
        DELETE FROM [dbo].[Marketing] where ProfileID = @stringData
        INSERT [dbo].[Marketing](ProfileID, State)
        SELECT * FROM @Marketing
    END

Az adatbázis meghatározásával SqlWriterTableType azonos nevű táblázat típusát. Figyelje meg, hogy a táblázat típusú séma ugyanaz, mint a bemeneti adatok által visszaadott séma kell lennie.

    CREATE TYPE [dbo].[MarketingType] AS TABLE(
        [ProfileID] [varchar](256) NOT NULL,
        [State] [varchar](256) NOT NULL
    )

A tárolt eljárás funkció [Table-Valued paraméterei](https://msdn.microsoft.com/library/bb675163.aspx)előnyeit.