<properties
   pageTitle="Adatok adatok SQL Azure-esemény hubra |} Microsoft Azure"
   description="Az esemény hubok áttekintése SQL minta importálása"
   services="event-hubs"
   documentationCenter="na"
   authors="spyrossak"
   manager="timlt"
   editor=""/>

<tags 
   ms.service="event-hubs"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/25/2016"
   ms.author="spyros;sethm" />

# <a name="pulling-data-from-sql-into-an-azure-event-hub"></a>Adatok az adatokat az SQL Azure-esemény elosztóhoz

Egy tipikus-architektúrára vonatkozó kérelmet valós idejű adatok feldolgozása magában foglalja a először Azure esemény hubhoz terjesztése azt. Lehet, hogy egy IoT esetben figyelése a forgalmat a különböző pályaszakaszain egy közúti, például vagy egy a frenzied contestants, ha végzett műveletek figyelése játék példa vagy egy vállalati esetben, például épület használati figyelése. Ezekben az esetekben az adatok gyártók idősort – adatok összegyűjtése és elemezni, tárolásához szükséges előállító általában külső objektumok, és törvény alapján, és előfordulhat, hogy amelyekbe munkamennyiség számos, az alábbi folyamatok infrastruktúra épület be. Mi lehet hajtsa végre, ha azt szeretné, hogy adatok valami adatfolyam forrást, hanem egy adatbázis, és a többi adatfolyam adatokkal együtt használja? Fontolja meg az esetet, amelyhez használandó Azure Értékáram-elemzés, távoli adatok Explorer (RDX) vagy egy másik eszközzel elemzése és működjön a Microsoft Dynamics AX lassan módosítása adatainak vagy egyéni létesítménygazdálkodás rendszert? A probléma megoldásához azt már írt és Megnyitás kifejezéskészletébe kis felhő minta módosítása, és telepítését, amely egy SQL-táblából az adatok és az Azure esemény-hubon keresztül csatlakozott egy bemeneteként a későbbi analitikai alkalmazásokban használandó leküldéses azt. Látja, hogy ez a példa ritka, és általában mire a egy esemény hubhoz ellentéte. Ha talál saját maga a hol található ez esetben mit kell tennie, az Azure minták gyűjtemény [Itt](https://azure.microsoft.com/documentation/samples/event-hubs-dotnet-import-from-sql/)a kódot is talál.  

Ne feledje, hogy a kód, a következő példában az imént, hogy egy minta. Még **nem** szánt egy gyártási alkalmazást, és nem a kísérletek nem történtek legyen alkalmas olyan környezetben – stricly egy DIY, például fejlesztő fókuszáló. Tekintse át a különféle biztonság, a teljesítmény, a használható funkciók körét, és a költség tényezők előtt rendezése egy végpont architektúra szüksége.

## <a name="application-structure"></a>Alkalmazás-struktúra

Az alkalmazás írott C#, és a minta readme.md fájl szeretne módosítani, összeállítása és közzététele az alkalmazás minden információt tartalmaz. A következő szakaszokban a Mit jelent az alkalmazás magas szintű áttekintése.

Feltételezve, hogy van hozzáférése egy SQL Azure-táblából kezdje azt. Is kell egy Azure esemény hubhoz be van állítva, és a kapcsolat, hogy hozzáférhessenek szükséges karakterlánc.

A SqlToEventHub megoldást indításakor konfigurációs fájl (App.config) dolgot, számot kap a readme.md fájlban bemutatottak olvassa be. Nagyjából értetődő, például a nevet, és a adattábla ezek, és nincs szükség a magyarázatokat itt rehash nem. 

Az alkalmazás a konfigurációs fájl olvasási, miután azt mutat be hurkot, az SQL-táblázat olvasási rekordok terjesztése az esemény-hubon keresztül csatlakozott, majd előtt végezze el az összes feletti ismét a felhasználó által definiált meddig alhatnak intervallumban várakozás. Néhány dolog, amit megjegyezni vannak:

1. Az alkalmazás feltételezi, hogy az SQL-táblázat néhány külső folyamat frissül, és el szeretné küldeni az összes, és csak a frissítéseket, esemény-hubon keresztül csatlakozott alapul.
2. Az SQL-táblázat egy mezőt, amely számos egyedi és növekvő, például egy rekord sorozata gyártási kell szerepelnie. Ez lehet egyszerűen, egy "Azonosító" nevű mező, vagy bármely más, mint bármilyen frissíti az adatbázis minden ad hozzá a rekordok, például "Creation_time" vagy "Sequence_number". Az alkalmazás a jegyzetek és törzsének tárolja az adott mező értékét. Minden egyes további lépéssel keresztül le az alkalmazás lényegében lekérdezi a tábla minden rekordhoz, ahol az adott mező értéke meghaladja a az érték a leállításig keresztül legutóbb bemutattuk. Hogy a hívott az utolsó érték az "eltolási".
3. Az alkalmazás létrehoz egy táblázatot "TableOffsets", az eltolás tárolásához indításkor. A táblázat "CreateOffsetTableQuery" a konfigurációs fájl lekérdezését jön létre. 
4. Vannak olyan telt el a konfigurációs fájl meghatározott "OffsetQuery", "UpdateOffsetQuery" és "InsertOffsetQuery", az eltolás táblával több lekérdezés. Ne változtassa ezek.
5. Végül a lekérdezés "az adatlekérdezés" definiált a konfigurációs fájl a lekérdezést, amely arra, hogy lekérje a rekordokat, az SQL-táblából fog futni. Ez jelenleg korlátozódik minden át a leállításig - optimalizálási célokra felső 1000 rekordjaihoz, ha például, ami 25 000 rekordok van hozzáadva az adatbázishoz óta az utolsó lekérdezést, akkor is igénybe vehet végrehajtja a lekérdezést. A lekérdezés 1000 rekordok minden alkalommal, amikor korlátozásával a Lekérdezések gyorsítására. Egyszerű 1000 felső kijelölése verembe küldi 1000 rekordok egymást követő tételek az esemény-hubon keresztül csatlakozott.    

## <a name="next-steps"></a>Következő lépések

A megoldást üzembe helyezéséhez klónozhatja vagy a SqlToEventHub alkalmazás letöltése, a App.config fájl szerkesztése, létrehozása, és végül a projekt közzététele a. Az alkalmazás közzétételt követően jelenik meg az Azure klasszikus portálon a felhőalapú szolgáltatások fut, és figyelemmel kísérheti az eseményeket, az esemény központi érkező. Figyelje meg, hogy két dolog határozza meg az ismétlődési: az SQL-táblát, és az alkalmazás a konfigurációs fájl a megadott meddig alhatnak intervallum a frissítések gyakoriságának.