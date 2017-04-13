## <a name="repeatability-during-copy"></a>Ismételhetőség másolás során

Ha más adatokból Azure SQL-vagy SQL Server másolása adatokat tárolja egyik kell ismételhetőség tartsa szem előtt elkerülése érdekében a várt eredményeket. 

Adatok másolása Azure SQL-vagy SQL Server-adatbázishoz, amikor másolás tevékenység alapértelmezett HOZZÁFŰZÉSE a gyűjtő táblázat adatkészlet alapértelmezés szerint értékeli. Például az adatok másolása CSV (pontosvesszővel elválasztott értékek) fájlt tartalmazó adatforrásokhoz két rekordot Azure SQL-vagy SQL Server-adatbázishoz, ha ez az a tábla néz ki:
    
    ID  Product     Quantity    ModifiedDate
    ... ...         ...         ...
    6   Flat Washer 3           2015-05-01 00:00:00
    7   Down Tube   2           2015-05-01 00:00:00

Tegyük fel, hogy forrásfájlban található hibákat, és frissítve mennyiségét lefelé csövet 2 4-es a forrásfájlban található. Ha újra futtatja a szeletre adott időszakra, új rekordokat Azure SQL-vagy SQL Server-adatbázis fűzött találhatók. Az alábbiakban azt feltételezi, hogy a táblázat oszlopait egyetlen sem rendelkezik, az elsődleges kulcs korlátozás.
    
    ID  Product     Quantity    ModifiedDate
    ... ...         ...         ...
    6   Flat Washer 3           2015-05-01 00:00:00
    7   Down Tube   2           2015-05-01 00:00:00
    6   Flat Washer 3           2015-05-01 00:00:00
    7   Down Tube   4           2015-05-01 00:00:00

Ennek elkerülése érdekében szüksége lesz határozza meg, hogy UPSERT szemantikáját feljebb helyezése közül a 2 mechanizmusok alatti jelzett alatt.

> [AZURE.NOTE] Szelet újra futtatható automatikusan az Azure Data Factory szerint a megadott újrapróbálkozási házirend.

### <a name="mechanism-1"></a>1 mechanizmusa

Először törlési művelet végrehajtásához szelet futtatásakor **sqlWriterCleanupScript** tulajdonság is élvezheti. 

    "sink":  
    { 
      "type": "SqlSink", 
      "sqlWriterCleanupScript": "$$Text.Format('DELETE FROM table WHERE ModifiedDate >= \\'{0:yyyy-MM-dd HH:mm}\\' AND ModifiedDate < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
    }

A Lemezkarbantartó parancsfájl volna hajtható végre első példányt egy adott szeletet szeretné az adatok törlése a kör közepétől megfelelő SQL-táblázat, amely a során. A tevékenység később szúrja be az adatokat az SQL-táblázatba. 

Ha a szeletet mostantól futtassa újra, akkor megtalálja a mennyiség frissül, mint a kívánt.
    
    ID  Product     Quantity    ModifiedDate
    ... ...         ...         ...
    6   Flat Washer 3           2015-05-01 00:00:00
    7   Down Tube   4           2015-05-01 00:00:00

Tegyük fel, az egyszerű, strukturálatlan ablakmosó rekord törlődik az eredeti csv. Újbóli futtatásával a szeletet volna eredményt a következő: 
    
    ID  Product     Quantity    ModifiedDate
    ... ...         ...         ...
    7   Down Tube   4           2015-05-01 00:00:00

Semmi sem új volt, el kell végezni. A Másolás tevékenység adódott a Lemezkarbantartó parancsfájl törlése a kör közepétől a megfelelő adatokat. A bemeneti beolvassa a csv (amely majd szereplő csak 1 rekordot), majd és beilleszti azt a táblázatba. 

### <a name="mechanism-2"></a>2 mechanizmusa
> [AZURE.IMPORTANT] sliceIdentifierColumnName jelenleg nem támogatott az Azure SQL-adatraktár. 

Egy másik mechanizmusok ismételhetőség elérése érdekében úgy, hogy egy külön oszlopban (**sliceIdentifierColumnName**) a táblázat cél van. Ez az oszlop annak érdekében, hogy a forrás- és kapcsolattartás szinkronizált Azure Data Factory volna használt. Ez a módszer akkor működik, rugalmasság módosítását és a cél SQL tábla séma definiálása esetén. 

Ez az oszlop használhatják Azure adatok gyári ismételhetőség célból, és a folyamat Azure adatok gyári fog nem séma módosításokat az a táblázat. Ezt a megközelítést módja:

1.  A cél SQL tábla típusú oszlopokban bináris (32) megadása Ez az oszlop nem korlátozó kell lennie. Most adjon nevet az ebben az oszlopban mint "ColumnForADFuseOnly" Ebben a példában.
2.  Akkor használja a Másolás tevékenység az alábbi képlettel történik:

        "sink":  
        { 
          "type": "SqlSink", 
          "sliceIdentifierColumnName": "ColumnForADFuseOnly"
        }

Azure Data Factory ebben az oszlopban a segítségre van szüksége annak érdekében, hogy a forrás- és kapcsolattartás szinkronizált szerint fog adataival. Ez az oszlop értékei nem használható a helyi kívül a felhasználó által. 

1 mechanizmusa hasonlóan másolás tevékenység automatikusan első karbantartása: az adott szelet a cél SQL-táblázat adatait, és futtassa újra a szokásos módon a Másolás tevékenység illessze be az adatokat a forrásból származó más rendeltetési helyet az adott szeletet. 
