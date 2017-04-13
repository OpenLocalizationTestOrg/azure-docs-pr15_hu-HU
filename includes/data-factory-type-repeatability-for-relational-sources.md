## <a name="repeatability-during-copy"></a>Ismételhetőség másolás során

Ha az adatok másolása, kezdő és záró relációs tárolja, kell ismételhetőség tartsa szem előtt elkerülése érdekében a várt eredményeket. 

Szeletek kell futtathatja automatikusan az Azure Data Factory szerint a megadott újrapróbálkozási házirend. Azt javasoljuk, hogy beállított-e hibák tranziens szembeni védekezés érdekében újrapróbálkozási házirend. Így ismételhetőség az adatok mozgás közben gondoskodik a fontos eleme. 

**Adatforrásként:**

> [AZURE.NOTE] Az alábbi példák az Azure SQL, de alkalmazandó bármely adattár, amely támogatja a téglalap alakú adatkészleteket. Módosítsa a forrás- és a **lekérdezés** tulajdonság **típusa** is (például: lekérdezési sqlReaderQuery helyett) az adatokat tárolja.   

Általában a relációs tárolja olvasásakor volna elolvasni kívánt csak az adott szeletet megfelelő adatokat. Erre lehetőség használatával a rendelkezésre álló WindowStart és WindowEnd változók Azure Data Factory lenne. A változók és a függvények az Azure adatok itt gyári [ütemezése, illetve a végrehajtás](../articles/data-factory/data-factory-scheduling-and-execution.md) című témakörben olvashat. Példa: 
    
      "source": {
        "type": "SqlSource",
        "sqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm\\'', WindowStart, WindowEnd)"
      },

A lekérdezés adatokat olvas "Táblanév", amely szeletek időtartam tartomány esik. Ez a cikk ismétlése is mindig gondoskodjon arról, hogy ez a probléma. 

Egyéb esetben, olvassa el a teljes táblázat érdemes (tegyük fel, hogy az egyszeri áthelyezése csak) és a következőképpen határozhatják meg a sqlReaderQuery:

    
    "source": {
                "type": "SqlSource",
                "sqlReaderQuery": "select * from MyTable"
              },
    
