<properties
    pageTitle="Jelentkezzen be a Keresés az napló Analytics |} Microsoft Azure"
    description="Log keresések kombinálása, és a környezetén belül több forrásból származó adatokat gépi összehangolására teszi lehetővé."
    services="log-analytics"
    documentationCenter=""
    authors="bandersmsft"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="banders"/>

# <a name="log-searches-in-log-analytics"></a>A napló Analytics napló keresések

A napló Analytics alapvető van a napló keresési funkcióval, amely lehetővé teszi, hogy egyesítésére és gépi a környezetén belül több forrásból származó adatokat összehangolására. Megoldások is hajtott napló keresés használatával jelenítse meg a mértékek átalakítani egy adott probléma terület köré.

A keresés lapon lekérdezést hozhat létre, és ezután Ha keres, az eredmények szerint szűrhető dimenzió vezérlői. Speciális lekérdezések átalakítás, szűrés és a jelentés az eredmények is létrehozhat.

Gyakori napló keresési lekérdezések a legtöbb megoldás lapokon jelennek meg. A MOBILE konzol egész mozaik gombra, vagy más elemekhez, az elem adatainak megjelenítéséhez napló keresőfunkcióval részletezést.

Ebben az oktatóanyagban módszeren végigvezetjük terjed ki az összes alapvető napló keresési használatakor példákat.

Hogy miként elindítása egyszerű, gyakorlati példákat, és ezután létrehozhatja őket, hogy ki kell olvasni a hírcsatornájában, amelyet az adatok az alábbi használatáról a gyakorlati használati eset megértéséhez elérheti.

Után, ha már ismeri a keresési technikák, tekintse át a [napló Analytics jelentkezzen be a keresés hivatkozás](log-analytics-search-reference.md).

## <a name="use-basic-filters"></a>Egyszerű szűrők használata

Tudnivalók a legfontosabb dolog, hogy a keresési első része a lekérdezés, mielőtt bármilyen "|}" függőleges vonás, akkor mindig *szűrő*. Érdemes elképzelnie, mint TSQL – a WHERE záradékban *milyen* ki a MOBILE adatokat tároló csoportosítani adatok részhalmazát határozza meg. Keresés az adatok tárolása az nagymértékben adja meg az adatokat, amelyet ki kell olvasni, így a természetes módon, hogy a lekérdezés a WHERE záradékkal kezdene jellemzői.

A legalapvetőbb szűrőket is használhatja olyan *kulcsszavakat*, például a "hiba" vagy "időtúllépés" vagy a számítógép nevét. Az ilyen típusú lekérdezések egyszerű általában az eredmény mindazon adatait különböző alakzatok adja eredményül. Ennek az oka napló Analytics különböző *típusú* rendelkezik, a rendszer.


### <a name="to-conduct-a-simple-search"></a>Egyszerű keresési elvégzéséhez
1. A MOBILE portálon kattintson a **Log keresés**gombra.  
    ![Keresés csempe](./media/log-analytics-log-searches/oms-overview-log-search.png)
2. Írja be a lekérdezésmező `error` , és kattintson a **Keresés**gombra.  
    ![keresési hiba](./media/log-analytics-log-searches/oms-search-error.png)  
    Ha például a lekérdezés `error` az alábbi képen a visszaadott 100 000 **esemény** records (a napló Management által gyűjtött), a 18 **ConfigurationAlert** rekordokat (konfigurációs értékelési által generált) és 12 **ConfigurationChange** records (a változások követése által rögzített).   
    ![keresési eredmények](./media/log-analytics-log-searches/oms-search-results01.png)  

Ezek a szűrők valójában nem objektum típusa és osztályok. *Írja be* a csak egy címkét, vagy egy tulajdonság, vagy a karakterlánc/név vagy kategória, csatlakoztatott adatok valamilyen. Dokumentumok némelyikét a rendszer **Típusa: ConfigurationAlert** , címkézett és **Típusa: Perf**vagy **Esemény típusa:**címkézett, és így tovább. Minden egyes keresési eredmény, a dokumentumot, a rekordot vagy a bejegyzés jeleníti meg a nyers tulajdonságokat és az értékeket az egyes e csatolt fájllal, és használhatja ezeket a mezőnevek adja meg a szűrőben, ha csak azok a rekordok lekérdezni kívánt azok a mező nem kapott érték.

*Típus* valójában csak, ha egy mezőt, amely az összes rekordot van, nem különbözik bármely más mezőre. Ez a hozta létre a Típus mező értéke alapján. Rekord lesz egy másik alakzatot vagy formában. Mellékesen **típus = Perf**, vagy **típus = esemény** egyben az alábbi lekérdezés teljesítményadatokat vagy események ismerje meg, hogyan kell.

Egy kettőspontot (:) vagy egy egyenlőségjelet (=) után a mező nevét, valamint az érték előtt használható. **Esemény típusa:** és **típus = esemény** vannak azonos értelemben, megadhatja a stílust, inkább.

Igen, ha a típus = Perf rekord tartalmaz egy "CounterName" nevű mezőt, majd a lekérdezés hasonlítanak is írhat `Type=Perf CounterName="% Processor Time"`.

Meg fogalmat ad a teljesítményadatok a teljesítmény számláló neve "processzor %" helyére.

### <a name="to-search-for-processor-time-performance-data"></a>Processzor idő teljesítményadatokat keresése
- Írja be a keresőmezőbe lekérdezés`Type=Perf CounterName="% Processor Time"`

Még pontosabban és használható **példánynév = "Összesen" _** a lekérdezés, amely olyan, a Windows teljesítmény számláló. Emellett bejelölheti a dimenzió és egy másik **mezőérték:**. A szűrő automatikusan felkerül a szűrőben, a lekérdezés sávon. Az alábbi képen láthatja. Jelzi, hogy hol szeretné hozzáadásához kattintson ide **példánynév: "_Total"** bármi beírása nélkül a lekérdezésre.

![Keresés dimenzió](./media/log-analytics-log-searches/oms-search-facet.png)

A lekérdezés most válik.`Type=Perf CounterName="% Processor Time" InstanceName="_Total"`

Ebben a példában nem kell megadni **típus = Perf** Ez az eredmény eléréséhez. Mivel a mezők CounterName és példánynév csak léteznek típusú rekord = Perf, a lekérdezés nem jellemző ugyanazt az eredményt, mint a hosszabb, előző vissza:
```
CounterName="% Processor Time" InstanceName="_Total"
```

Ennek oka az, a lekérdezés az összes szűrőt, *és* hogy a többi kiértékelése történik. Hatékony, ha a további mezőket vesz fel a feltételt, kisebb, akkor bizonyos és finomított eredmények.

Ha például a lekérdezés `Type=Event EventLog="Windows PowerShell"` azonos `Type=Event AND EventLog="Windows PowerShell"`. Összes eseményének, amelyeket bejelentkezve, és a Windows PowerShell eseménynaplójának gyűjtött adja vissza. Ha felvesz szűrő többször az azonos dimenzió többször választva, majd a probléma oka tisztán kozmetikai –, előfordulhat, hogy a Keresés sáv rendezetlen elemek, de továbbra is visszaküldi ugyanazt az eredményt, mert az implicit és műveleti jel létezik mindig.

Az implicit és operátorral egy nem operátort kifejezetten használatával egyszerűen visszavonhatja. Példa:

`Type:Event NOT(EventLog:"Windows PowerShell")`vagy azzal egyenértékű `Type=Event EventLog!="Windows PowerShell"` összes eseményének bevétel az egyéb naplók, amelyek nem a Windows PowerShell naplót.

Vagy más logikai operátorok használhatja például: "Vagy". Az alábbi lekérdezés, amelynek az Eseménynapló mindkét alkalmazást vagy a rendszer nem rekordokat adja vissza.

```
EventLog=Application OR EventLog=System
```

A fenti lekérdezésnek használata esetén vissza bejegyzések mindkét naplók az azonos eredményhalmaz.

Jó helyen jár Ha törli a vagy hagyja az implicit és helyen, majd az alábbi lekérdezés nem hoznak létre találatokat, mert nem egy naplóbejegyzés, amely a két naplók tartozik. Minden egyes naplóbejegyzés készült a két naplók egyikére.

```
EventLog=Application EventLog=System
```


## <a name="use-additional-filters"></a>A további szűrők használata

Az alábbi lekérdezés 2 eseménynaplók az adatok küldött összes számítógépre bejegyzéseinek adja eredményül.

```
EventLog=Application OR EventLog=System
```

![keresési eredmények](./media/log-analytics-log-searches/oms-search-results03.png)

A mezők vagy szűrők valamelyikének kiválasztásával szűkítheti a lekérdezés adott számítógépre, kivételével az összes másokat. Az eredményül kapott lekérdezés lenne a következőhöz hasonló:

```
EventLog=Application OR EventLog=System Computer=SERVER1.contoso.com
```

Amely egyenértékű a következő miatt nem az implicit és művelet.

```
EventLog=Application OR EventLog=System AND Computer=SERVER1.contoso.com
```

Minden lekérdezés kiértékeli a következő explicit sorrendben. Megjegyzés: a zárójelet.

```
(EventLog=Application OR EventLog=System) AND Computer=SERVER1.contoso.com
```

Az Eseménynapló mező hasonlóan meghallgathatja csak egy sor olyan számítógépen meghatározott adatokat hozzáadásával vagy. Példa:

```
(EventLog=Application OR EventLog=System) AND (Computer=SERVER1.contoso.com OR Computer=SERVER2.contoso.com OR Computer=SERVER3.contoso.com)
```

Hasonlóképpen Ez az alábbi lekérdezés vissza **% Processzor idő** csak a kijelölt két számítógépek.

```
CounterName="% Processor Time"  AND InstanceName="_Total" AND (Computer=SERVER1.contoso.com OR Computer=SERVER2.contoso.com)
```


### <a name="boolean-operators"></a>Logikai operátorok
A datetime és a számmezők használatával *nagyobb, mint*, értékek is kereshet *kisebb mint*, és *kisebb vagy egyenlő*. Például: Egyszerű operátorok használhatók >, <>, =, < =,! a lekérdezés Keresés sáv =.


Egy adott időszakra vonatkozóan is lekérdezhetők egy adott eseménynaplójában talál. Az elmúlt 24 óra például a következő mnemonikus kifejezéssel van megadva.

```
EventLog=System TimeGenerated>NOW-24HOURS
```


#### <a name="to-search-using-a-boolean-operator"></a>Keresés a logikai operátor használata
- Írja be a keresőmezőbe lekérdezés`EventLog=System TimeGenerated>NOW-24HOURS"`  
    ![a logikai érték keresése](./media/log-analytics-log-searches/oms-search-boolean.png)

Megadhatja, hogy az időszakot grafikusan, és a legtöbb időpontok érdemes lehet megtennie, bár vannak előnyei többek között az idő szűrő közvetlenül a lekérdezést. Például Ez nagyszerű irányítópultok, ahol minden mozaik, függetlenül a *globális* idő sorkijelölőre az irányítópult lapon ideje felülbírálhatja. További tudnivalókért lásd: az [Idő ügyekben az irányítópulton](http://cloudadministrator.wordpress.com/2014/10/19/system-center-advisor-restarted-time-matters-in-dashboard-part-6/).

Ha idő szerinti szűrés, tartsa szem előtt eredmények beszerzése a *Metszet* a két időszakaira: a megadott a MOBILE portálon (S1) és a befogadó (S2) lekérdezésben megadott.

![Metszet](./media/log-analytics-log-searches/oms-search-intersection.png)

Ez azt jelenti, ha az időszakok nem metszi egymást, például a MOBILE portálon, ahol kiválaszthatja, hogy **Ez a hét** és a lekérdezés, ahol megadhatja a **múlt héten**, majd nem nincs metszet és nem fogadhat találatokat.

Összehasonlító operátorok, használja a TimeGenerated mezőhöz is egyéb esetekben lehet hasznos. Ha például a számadatokat tartalmazó mezőket.

Ha például adott, konfigurációs értékelési riasztások rendelkezik-e az alábbi súlyosságát értékeket:

- 0 = információk
- 1 = figyelmeztetés
- 2 kritikus =

A lekérdezés figyelmeztető és a kritikus nyomon követése riasztásokkal parancsát, és is kizárja a következő lekérdezés a tájékoztató lehetőségekből:

```
Type=ConfigurationAlert  Severity>=1
```


Tartomány lekérdezések is használhatja. Ez azt jelenti, hogy az értékek sorrendben elején és végén köre lehet nyújtani. Ha például ha azt szeretné, hogy hol a EventID a nagyobb vagy egyenlő 2100, de nem nagyobb, mint 2199 Operations Manager eseménynaplójában talál eseményeinek, majd a következő lekérdezés adja vissza őket.

```
Type=Event EventLog="Operations Manager" EventID:[2100..2199]
```


>[AZURE.NOTE] Szintaxisa a következő tartományt kell használni a kettőspont (:) mezőérték: elválasztó karaktert, és *nem* az egyenlőségjel (=). A tartomány alsó és felső végén foglaljuk szögletes zárójelek közé, és válassza szét a két fizetési időszak (.).

## <a name="manipulate-search-results"></a>Keresési eredmények kezelése

Ha az adatok keres, célszerű finomíthatja a keresést, és szabályozhatja az eredményeket egy jó szintű. Eredmények olvassa be a program, amikor alkalmazhat parancsok átalakításához őket.

Log Analytics keresések *kell* parancsok kövesse a függőleges vonás (|) után. Szűrő mindig a lekérdezési karakterlánc első része. Azt határozza meg az adatkészlet dolgozik, és ezután "pipák" ezeket az eredményeket egy parancsa. A függőleges vonás használatával adja hozzá a további parancsok majd. Az módszerektől hasonlóan, mint a Windows PowerShell-folyamat.

Az általános a napló Analytics keresési nyelv próbálja hajtsa végre a PowerShell stílus és irányelveket ahhoz hasonlóan, mint az informatikai szakemberek és a tanulási görbe megkönnyítése érdekében.

Parancsok megtalálja a műveletek azoknak, így könnyen megállapíthatja ugyanúgy működnek.  

### <a name="sort"></a>Rendezés

A Rendezés parancs lehetővé teszi, hogy megadhatja a rendezési sorrend szerint egy vagy több mezőt. Akkor is, ha nem használja, alapértelmezés szerint, csökkenő sorrendben egyszerre lép érvénybe. Legutóbbi eredménye mindig a keresési eredmények tetején. Ez azt jelenti, hogy a keresést, futtatásakor `Type=Event EventID=1234` mi valójában hajtanak végre, akkor:

```
Type=Event EventID=1234 **| Sort TimeGenerated desc**
```

Az van, mert a már jól ismert naplókban élmény típusát. Ha például a Windows eseménynaplójának.

Ha meg szeretné változtatni a visszaadott eredmény rendezés is használhatja. Az alábbi példák bemutatják, hogyan működik.

```
Type=Event EventID=1234 | Sort TimeGenerated asc
```

```
Type=Event EventID=1234 | Sort Computer asc
```

```
Type=Event EventID=1234 | Sort Computer asc,TimeGenerated desc
```


A fenti egyszerű példa megjelenítése parancsok működése – ezek az eredmények a szűrő visszaküldött alakzatának módosítása.

### <a name="limit-and-top"></a>Határérték és a felső
Egy másik kevesebb ismert parancs is a KORLÁTOT. A korlát egy PowerShell-szerű igei. A korlát funkcionális azonos a TOP parancsot. Az alábbi lekérdezés ugyanazt az eredményt ad vissza.

```
Type=Event EventID=600 | Limit 1
```

```
Type=Event EventID=600 | Top 1
```


#### <a name="to-search-using-top"></a>Keresés a felső használatával
- Írja be a keresőmezőbe lekérdezés`Type=Event EventID=600 | Top 1`   
    ![Keresés tetején](./media/log-analytics-log-searches/oms-search-top.png)

A fenti képen rekord is létezik 358 ezer EventID az = 600. A mezők, metszettel és a bal oldali szűrők mindig legyen látható a információkat az eredmények visszaadott *szerinti szűrés része* a lekérdezés, amely bármely függőleges vonal karakter elé része. Az **eredmények** ablaktáblája csak adja eredményül legutóbbi 1, mert a fenti parancs alakú, és az eredmények átalakítva.

### <a name="select"></a>Válassza a

A SELECT parancs úgy viselkednek, mint a PowerShell kijelölése-objektumot. Szűrt eredményben, amely nem rendelkezik az összes eredeti tulajdonságaik ad vissza. Ehelyett csak a megadott tulajdonságok kijelöli.

#### <a name="to-run-a-search-using-the-select-command"></a>A select paranccsal keresés futtatása

1. A Keresés mezőbe írja be `Type=Event` , és kattintson a **Keresés**gombra.
2. Kattintson a **+ több megjelenítése** az egyik az eredményeket az eredményeket tartalmazó tulajdonságok megtekintése gombra.
3. Jelölje be azokat egy része explicit módon, és a lekérdezés változik `Type=Event | Select Computer,EventID,RenderedDescription`.  
    ![Válassza a keresés](./media/log-analytics-log-searches/oms-search-select.png)

Ez a parancs különösen hasznos akkor, ha a szabályozhatja a keresési eredményt ad, és válassza a csak az adatokat a feltárási, amely a teljes bejegyzés gyakran nem igazán számít része. Ez akkor is hasznos, ha különböző típusú rekord *bizonyos* közös tulajdonságok van, de nem az *összes* tulajdonságaik a közös. A kimenet, amely a következőképpen néz természetesen több táblázat létrehozása vagy jól használhatók CSV-fájlba exportálja, és a formázott, sertéscsülköt olyan tartalmazó, az Excel programban a majd termék.



## <a name="use-the-measure-command"></a>A mértékegység paranccsal

MÉRTÉK egyike, a legtöbb sokoldalú parancsok napló Analytics kiterjedjen. Lehetővé teszi a statisztikai *függvények* alkalmazása az adatokat, és az összesítő eredményeket egy adott mező szerint csoportosítva. Vannak, amely támogatja a Méretezőeszköz több statisztikai függvények.

### <a name="measure-count"></a>Mérték count()

Az első statisztikai funkció használata, és az egyik a legegyszerűbb megértéséhez, akkor a *count()* függvény.

Például: minden olyan keresési lekérdezés eredményei `Type=Event`, szűrők, más néven metszettel bal oldalán a keresési eredmények megjelenítéséhez. A szűrők nyilvántartása egy értéket az eredményeket egy adott mező által végrehajtott keresés.

![Keresés mérték száma](./media/log-analytics-log-searches/oms-search-measure-count01.png)

Például a fenti képen láthatja a **számítógép** mezőben, és azt azt mutatja, hogy a találatok között, szinte 739 ezer események belül a **számítógép** mező rekordjait 68 egyedi és egyedi értékek vannak. A csempe csak azt mutatja, a felső 5, amely a **számítógép** mezőket írt leggyakoribb 5 értékek), amely tartalmazza az adott adott értéket abban a mezőben lévő dokumentumok száma szerint vannak rendezve. A képen megtekintheti, hogy – – szinte 369 ezer események között 90 ezer beépített részei, és a számítógépről OpsInsights04.contoso.com 83 ezer DB03.contoso.com számítógépről, és így tovább.


Mi történik, ha meg szeretné, hogy összes értéket, mivel a csempe csak akkor látható, csak az 5 legfontosabb?

Ez a mérték parancs mire alkalmas a count() függvénnyel. Ez a funkció nem használható paramétereket. Csak adja meg a mezőt, ebben az esetben – a **számítógép** mező alapján csoportosítani szeretné:

`Type=Event | Measure count() by Computer`

![Keresés mérték száma](./media/log-analytics-log-searches/oms-search-measure-count-computer.png)

**Számítógép** azonban csak egy mező használható *az* egyes adatcsoportok – nem relációs adatbázisok érintett vannak, és nem található külön **számítógép** objektum tetszőleges helyre. Csak az értékeket *az* adatok leírhatja mely entitás hozza létre őket, és számos más jellemző és szempontjai az adatok – így a szerződési időszak *dimenzió*. Jó helyen jár csak, valamint csoportosíthatja más mezőket. Mivel vannak adatcsatornán mérték parancsa szinte 739 ezer események eredeti eredményeinek is telepítve van a **EventID**nevű mező, alkalmazhat ugyanezzel a technikával Csoportosítás mező és EventID események száma:

```
Type=Event | Measure count() by EventID
```

Ha Ön nem egy adott értéket tartalmaz a tényleges rekordszám érdekli, de inkább saját maguk az értéklista csak tetszés felveheti a *Select* parancs végén, és csak akkor jelölje be az első oszlop:

```
Type=Event | Measure count() by EventID | Select EventID
```

Ezután első további bonyolult és előre szűrheti a találatokat a lekérdezést, vagy túl egyszerűen kattintson a rács oszlopainak.

```
Type=Event | Measure count() by EventID | Select EventID | Sort EventID asc
```

#### <a name="to-search-using-measure-count"></a>Keresés a használó mérték

- Írja be a keresőmezőbe lekérdezés`Type=Event | Measure count() by EventID`
- Hozzáfűző `| Select EventID` a lekérdezés végére.
- Végül a Hozzáfűzés `| Sort EventID asc` a lekérdezés végére.


Létezik néhány fontos tudnivaló figyelje meg, és ki szeretné emelni:

Első lépésként a sajtóközlemény sem az eredeti nyers eredmények eltűnt. Ehelyett, azok összesített eredmények – lényegében az eredmények csoportokat. Ez a probléma nem, de ismerje meg, hogy van-e személlyel nagyon különböző alakzatba eltér adatok az eredeti nyers alakzatot az összesítés statisztikai függvény eredményeként menet létrehozott kapja a.

Második **mérték száma** jelenleg csak a legelső 100 eltérő eredményt ad. Ezt a korlátot nem vonatkozik a statisztikai függvények. Igen általában kell szűrővel pontosabb először bizonyos elemek keresése mérték count() alkalmazása előtt.

## <a name="use-the-max-and-min-functions-with-the-measure-command"></a>A min és a max függvény használata a mérték parancs

Vannak olyan, ahol **Mérték Max()** és **Mérték Min()** hasznosak különböző forgatókönyvekben. Jó helyen jár mert ellenkező minden függvény minden más, azt fogja ábrázolják Max(), és nyugodtan kísérletezhet Min() önálló.

Ha a lekérdezés biztonsági események változhat **szint** tulajdonságában rendelkeznek. Példa:

```
Type=SecurityEvent
```

![Keresés mérték darab indítása](./media/log-analytics-log-searches/oms-search-measure-max01.png)

Ha meg szeretné tekinteni a legnagyobb érték az összes a biztonsági megadott közös számítógépe, a Csoportosítás mező, események használhatja

```
Type=ConfigurationAlert | Measure Max(Level) by Computer
```

![Keresés mérték max számítógépen](./media/log-analytics-log-searches/oms-search-measure-max02.png)

Program megjelenítési **szint** rekordot tartalmazott számítógépeknél őket a legtöbb tartalmazó legalább szint 8, sok volna 16 szintet.

```
Type=ConfigurationAlert | Measure Max(Severity) by Computer
```

![keresési mérték maximális időt hozza létre a számítógépen](./media/log-analytics-log-searches/oms-search-measure-max03.png)

Ez a funkció számokkal jól működik, de a DateTime mezőket is működik. Célszerű ellenőrzése minden adatot indexelni minden számítógépet a legutóbbi vagy a legutóbbi időbélyegzőjét. Példa: Ha a legújabb biztonsági esemény jelentett minden számítógép?

```
Type=ConfigurationChange | Measure Max(TimeGenerated) by Computer
```

## <a name="use-the-avg-function-with-the-measure-command"></a>Az ÁTLAG függvény használata a mérték parancs

A használt mérték Avg() statisztikai függvény lehetővé teszi, hogy az egyes mező és az eredmények csoportosítása az ugyanazon vagy másik mező alapján átlagos értékét számítja ki. Az esetek, például a teljesítményadatok számos hasznos.

Lássuk először a teljesítményadatok. Figyelje meg, hogy a MOBILE jelenleg gyűjti össze a Windows és a Linux gépekhez teljesítmény számláló.

Szeretne keresni *az összes* teljesítményadatokat, a legalapvetőbb lekérdezés van:

```
Type=Perf
```

![Keresés avg indítása](./media/log-analytics-log-searches/oms-search-avg01.png)

A legfontosabb dolog észre fogja venni, hogy napló Analytics megtekintheti három perspektívák: listában, amely jelzi, hogy mely jeleníti meg a tényleges rekordok mögött a diagramok; Táblázat, amely jeleníti meg a teljesítményadatok számláló; táblázatos nézet és mértékek, amely a teljesítmény számláló diagramok jeleníti meg.

A fenti képen vannak megjelölt mezőt, amely jelzi a következő két csoportját:

- Az első készlet azonosítja a Windows teljesítmény számláló nevét, objektum neve és példány nevét a lekérdezés szűrő. Ezek azok a mezők, valószínűleg leggyakrabban használni kívánt metszettel/szűrők
- **Ellenértéknek** a tényleges érték a számláló. Ebben a példában az érték *75*.
- **TimeGenerated** 12:51, 24 órás formátumban.

Az alábbiakban a mértékek grafikonon nézetének.

![Keresés avg indítása](./media/log-analytics-log-searches/oms-search-avg02.png)

Miután a Perf rekord alakzatra vonatkozó olvasási és kellene további információ: más keresési módszerek mérték Avg() is használhatja összesítése numerikus adatokat az ilyen típusú.

Íme egy egyszerű példa:

```
Type=Perf  ObjectName:Processor  InstanceName:_Total  CounterName:"% Processor Time" | Measure Avg(CounterValue) by Computer
```

![avg samplevalue keresése](./media/log-analytics-log-searches/oms-search-avg03.png)

Ebben a példában válassza ki a teljes ideje Processzor teljesítmény számláló és az átlagos számítógép. Ha azt szeretné, az utolsó 6 órával a találatok szűkítéséhez, az idő szűrő vezérlőjével, vagy adja meg a lekérdezést az alábbi képlettel történik:

```
Type=Perf  ObjectName:Processor  InstanceName:_Total  CounterName:"% Processor Time" TimeGenerated>NOW-6HOURS | Measure Avg(CounterValue) by Computer
```

### <a name="to-search-using-the-avg-function-with-the-measure-command"></a>Keresés a mérték parancs az ÁTLAG függvény használata
- Írja be a keresőmezőbe lekérdezés `Type=Perf  ObjectName:Processor  InstanceName:_Total  CounterName:"% Processor Time" TimeGenerated>NOW-6HOURS | Measure Avg(CounterValue) by Computer`.


Összesítése, és adatok *különböző* számítógépeken összehangolására. Tegyük fel, hogy egy állomások a farm, ahol minden csomópont más valamelyik egyenlő, és csak azonos beírják munka, célszerű lehet nagyjából terheléselosztás funkciója. A számláló egyben az összes lépjen a következő lekérdezés és a teljes farm átlagokat beszerzése olvasható. Az alábbi példa a számítógépen kiválasztásával indítható el:

```
Type=Perf AND (Computer="AzureMktg01" OR Computer="AzureMktg02" OR Computer="AzureMktg03")
```

Most, hogy a számítógép, csak is kijelölni kívánt két fő teljesítménymutatók (KPI-k): processzorhasználata és % szabad lemezterület. Igen a lekérdezés adott részének lesz:

```
Type=Perf InstanceName:_Total  ((ObjectName:Processor AND CounterName:"% Processor Time") OR (ObjectName="LogicalDisk" AND CounterName="% Free Space")) AND TimeGenerated>NOW-4HOURS
```

Most már számítógépek és a következő példa a számláló adhat hozzá:

```
Type=Perf InstanceName:_Total  ((ObjectName:Processor AND CounterName:"% Processor Time") OR (ObjectName="LogicalDisk" AND CounterName="% Free Space")) AND TimeGenerated>NOW-4HOURS AND (Computer="AzureMktg01" OR Computer="AzureMktg02" OR Computer="AzureMktg03")
```

Mivel nagyon meghatározott részét van, a **mérték Avg()** parancs úgy térhet vissza az átlag nem a számítógép, de a kiszolgálófarmban végig egyszerűen CounterName szerinti csoportosítás. Példa:

```
Type=Perf  InstanceName:_Total  ((ObjectName:Processor AND CounterName:"% Processor Time") OR (ObjectName="LogicalDisk" AND CounterName="% Free Space")) AND TimeGenerated>NOW-4HOURS AND (Computer="AzureMktg01" OR Computer="AzureMktg02" OR Computer="AzureMktg03") | Measure Avg(CounterValue) by CounterName
```

Ezzel kap a környezet KPI-k, néhány hasznos kompakt nézetének.

![Keresés avg csoportosítása](./media/log-analytics-log-searches/oms-search-avg04.png)


A keresési lekérdezés az irányítópulton könnyen használható. Ha például a keresési lekérdezést menthette és *Webes Farm KPI-k*nevű belőle irányítópult létrehozása. Irányítópultok használatával kapcsolatos további információért olvassa el a [naplót Analytics egyéni irányítópult létrehozása](log-analytics-dashboards.md)című témakört.

![Keresés avg irányítópult](./media/log-analytics-log-searches/oms-search-avg05.png)

### <a name="use-the-sum-function-with-the-measure-command"></a>A SZUM függvény használata a mérték parancs

A SZUM függvényt más függvényekkel mérték parancs hasonlít. Példa a SZUM függvény használatáról a [W3C IIS naplók keresés a Microsoft Azure műveleti Hírcsatornájában](http://blogs.msdn.com/b/dmuscett/archive/2014/09/20/w3c-iis-logs-search-in-system-center-advisor-limited-preview.aspx)láthatja.

A számok, időpontok és a szöveges karakterláncok Max() és Min() használható. A szöveges karakterláncok betűrend szerint rendezett és először az első és utolsó.

Azonban nem használhat Sum() semmi nem numerikus mezők. Ez a Avg() is vonatkozik.

### <a name="use-the-percentile-function-with-the-measure-command"></a>A PERCENTILIS függvény használata a mérték parancs

A PERCENTILIS függvény hasonlít Avg() és Sum() abban, hogy a numerikus mezők csak használható. Numerikus mező 1 és 99 közötti bármely PERCENTILIS is használhatja. **PERCENTILIS** és a **százalékban** parancsok is használhatja. Íme néhány példa:  

```
Type:Perf CounterName:"DiskTransers/sec" |measure percentile95(CurrentValue) by Computer
```
```
Type:Perf ObjectName=LogicalDisk CounterName="Current Disk Queue Length" Computer="MyComputerName" | measure pct65(CurrentValue) by InstanceName
```

## <a name="use-the-where-command"></a>Használja a where parancs

Az adott parancs működik, mint a szűrő, de a további szűrést összesített mérték parancs – által készített során alkalmazható nem nyers eredménye pedig, amely szűri a lekérdezés elejére.

Példa:

```
Type=Perf  CounterName="% Processor Time"  InstanceName="_Total" | Measure Avg(CounterValue) as AVGCPU by Computer
```

Hozzáadhat egy másik függőleges vonás "|}" karaktert és a hol, csak a számítógépek, amelynek átlagos Processzor van, az alábbi példa a 80 %-nál első parancsot:

```
Type=Perf  CounterName="% Processor Time"  InstanceName="_Total" | Measure Avg(CounterValue) as AVGCPU by Computer | Where AVGCPU>80
```

Ha jártas a Microsoft System Center - Operations Manager, érdemes elképzelnie a where parancs a felügyeleti csomag feltételek. Ha a példa egy szabályt, a lekérdezés első része lenne az adatforrás és a hol parancs a feltétel észlelési lenne.

Is használhatja a lekérdezés egy mozaikon a **Saját irányítópult**, mint egy monitorként rendezi, a megjelenítéséhez, amikor számítógép processzorok túlterheltek. Irányítópultok kapcsolatos további információért olvassa el a [napló Analytics egyéni irányítópult létrehozása](log-analytics-dashboards.md)című témakört. Is létrehozhat és irányítópultok a mobilalkalmazás használatával. További tudnivalókért olvassa el a [MOBILE Mobile alkalmazásban ](http://www.windowsphone.com/en-us/store/app/operational-insights/4823b935-83ce-466c-82bb-bd0a3f58d865)című témakört. Az alábbi képen az utolsó két csempék, akkor láthatja a monitor listában megjelenített szám. Alapvetően mindig kívánt a szám nulla és a listában, és üres lesz. Egyéb esetben azt jelzi, az értesítési feltételt. Ha szükséges, akkor vele nézze meg, mely gépek nyomása alatt találhatók.

![mobil irányítópult](./media/log-analytics-log-searches/oms-search-mobile.png)

## <a name="use-the-in-operator"></a>Az az operátor használata

Az *az* operátor, *NOT IN* együtt subsearches, amely argumentumként újabb keresést tartalmazó keresések használatát teszi lehetővé. Más *elsődleges* vagy *külső* keresési belül a kapcsos zárójeleket {} találhatók. Egy subsearch gyakran egyedi eredmény érdekében listáját eredményének az elsődleges keresés argumentumaként használja.

Az adatok, hogy nem adja meg közvetlenül a keresési létrehozható, de a keresési kifejezés részhalmazának megfelelően subsearches is használhatja. Például ha érdekli egy keresése szolgáltatással az összes eseményének számítógépekről *hiányzik a frissítések*keresése, majd kell megtervezni a subsearch először azonosító, hogy a *hiányzó biztonsági frissítések számítógépek* , mielőtt megtalálja ezeket a hosts tartozó események.

Igen sikerült express, az alábbi lekérdezés *számítógépek jelenleg hiányzó szükséges biztonsági frissítések* :

```
Type:Update UpdateState=Needed Optional=false Classification="Security Updates" TimeGenerated>NOW-24HOURS | measure count() by Computer
```    

![Keresési példa](./media/log-analytics-log-searches/oms-search-in01-revised.png)

Ha befejezte a listát, egy belső keresést, a keresés is használhatja a hírcsatorna egy külső (elsődleges) keresést, az események ezekről a gépekről úgy gondolja, hogy a számítógépek listáját. Ehhez a belső keresés mellékelve kapcsos zárójelek és az eredmények, keresése külső az IN operátor használata/szűrőmező lehetséges értékei. A lekérdezés hasonló lesz:

```
Type=Event Computer IN {Type:Update UpdateState=Needed Optional=false Classification="Security Updates" TimeGenerated>NOW-24HOURS | measure count() by Computer}
```
![Keresési példa](./media/log-analytics-log-searches/oms-search-in02-revised.png)


Is értesítés az idő szűrő értékét a belső keresés használatos, mert a rendszer frissítés felmérés pillanatfelvételt minden számítógép 24 óránként. Teheti a belső lekérdezés hozza létre, és pontos csak a nap keres. A külső keresés helyett használja az idő kijelölés a felhasználói felület események lekérése az utóbbi 7 napból. Lásd: a [logikai operátorokat](#boolean-operators) idő operátorok kapcsolatban további tudnivalókat.

Mivel az Ön valójában csak a szűrőként belső keresési eredmények értéke a külső egy, a külső Keresés parancs továbbra is alkalmazhat. Ha például továbbra is csoportosíthatja a másik mérték parancs a fenti események:

```
Type=Event Computer IN {Type:Update UpdateState=Needed Optional=false Classification="Security Updates" TimeGenerated>NOW-24HOURS | measure count() by Computer} | measure count() by Source
```

![Keresési példa](./media/log-analytics-log-searches/oms-search-in03-revised.png)


Általánosságban elmondható érdemes a belső lekérdezés gyorsan végrehajtani, mert napló Analytics időtúllépései szolgáltatás mellett, és való visszatéréshez eredmények kis mennyiségű. A belső lekérdezés több eredményt ad vissza, ha a találatlista kap csak, esetleg okozhat, amely a külső találatok helytelen eredményt.

Egy másik szabály, hogy a belső keresés jelenleg meg kell *összesített* eredmények javításához. Más szóval kell tartalmaznia egy *mértéket* parancs; egy külső keresés jelenleg nem hírcsatorna az nyers eredmények.

Is csak egy IN operátor is lehet, és az utolsó szűrő a lekérdezésben, akkor kell lennie. IN operátor nem lehet vagy volna – Ez lényegében megakadályozza, hogy több subsearches fut: a fontos pont, hogy csak egy sub/belső a keresés minden külső keresési lehetőség.

Ezek a korlátok mellett is a lehetővé teszi, hogy a korrelációs keresések számos típusú, és lehetővé teszi, hogy a hasonló számítógépek, például a csoportok meghatározása felhasználók vagy fájl – az adatok tekintet nélkül a mezőket tartalmazó. Az alábbiakban további példákat:

**Hiányoznak a számítógépek, ahol az automatikus frissítés le van tiltva beállítás az összes frissítés**

```
Type=Update UpdateState=Needed Optional=false Computer IN {Type=UpdateSummary WindowsUpdateSetting=Manual | measure count() by Computer} | measure count() by KBID
```

**Az összes hibát eseményeinek (ahol SQL értékelési futtatása =) az SQL Servert futtató számítógépeken**

```
Type=Event EventLevelName=error Computer IN {Type=SQLAssessmentRecommendation | measure count() by Computer}
```

**Az összes biztonsági események számítógépekről, amelyek a tartomány vezérlők (=, ahol AD értékelési futtatása)**

```
Type=SecurityEvent Computer IN { Type=ADAssessmentRecommendation | measure count() by Computer }
```

**Amely más fiókokhoz a azonos számítógépeket, ahol BACONLAND\jochan fiókkal jelentkezett be kell bejelentkezni?**

```
Type=SecurityEvent EventID=4624   Account!="BACONLAND\\jochan" Computer IN { Type=SecurityEvent EventID=4624   Account="BACONLAND\\jochan" | measure count() by Computer } | measure count() by Account
```

## <a name="use-the-distinct-command"></a>A különböző paranccsal

Javaslatokat tesz a nevét, ez a parancs mező biztosít különböző értékek listája. Érdemes meglepő egyszerű, de igazán hasznos. Azonos sikerült érhető el a mérték count() paranccsal, valamint, alább látható módon.

```
Type=Event | Measure count() by Computer
```

![Példa a különböző keresési parancs](./media/log-analytics-log-searches/oms-search-distinct01-revised.png)

Ha az összes, amely érdekli viszont csak a különböző értékek és nem az értékeket, majd a DISTINCT biztosíthat rendelkező dokumentumok száma lista tisztább és könnyebben olvashatók, a kibocsátás és az alábbi ábrán látható módon, rövidebb szintaxist.

```
Type=Event | Distinct Computer
```
![Példa a különböző keresési parancs](./media/log-analytics-log-searches/oms-search-distinct02-revised.png)

## <a name="use-the-countdistinct-function-with-the-measure-command"></a>A countdistinct függvény használata a mérték parancs
A countdistinct függvény összeszámolja azokat a különböző értékek minden egyes csoporton belül. Ha például azt felhasználható minden típusa jelentési egyedi számítógépek számának:

```
* | measure countdistinct(Computer) by Type
```

![MOBILE-countdistinct](./media/log-analytics-log-searches/oms-countdistinct.png)

## <a name="use-the-measure-interval-command"></a>A mértékegység intervallum paranccsal
A teljesítmény valós idejű adatok gyűjtése, közelében is összegyűjtése és bármely teljesítmény számláló napló Analytics ábrázolása. Számláló és a napló Analytics környezetben kiszolgálók száma alapján egyszerűen megadása a lekérdezés **Típusa: Perf** metrikus grafikonok ezer ad eredményül. Az igény szerinti metrikus összesítés megnézheti a teljes mérési módja miatt a magas szintű, és be kell finomabb adatokat mély merülési környezetben.

Tegyük fel, hogy mi az átlagos Processzor összes a számítógépei között használni kívánt. Megjeleníti az átlag Processzor minden számítógép az nem lehet hasznos eredmények előfordulhat, hogy első Görbített mert. Vizsgálja meg, a további információra kíváncsi, hogy is összesítheti az eredményt kisebb egyszerre ablak szövegadattömb, és keresse meg időt sorozatává különböző méretek keresztül. Például végezheti el az óránkénti átlagát processzorhasználata összes a számítógépei között az alábbi képlettel történik:

```
Type:Perf CounterName="% Processor Time" InstanceName="_Total" | measure avg(CounterValue) by Computer Interval 1HOUR
```

![átlagos intervallum mérték](./media/log-analytics-log-searches/oms-measure-avg-interval.png)

Alapértelmezés szerint az eredmények a több adatsort interaktív vonaldiagram fog megjelenni.  Ez a diagram támogatja a sorozat választás (ha az y tengelyt metszi rescaling), a nagyítás és megjelenítése rámutatáskor.  A tábla megjelenítése beállítás érhető el továbbra is a nyers adatokból megtekintésre, ha szükséges.

Más mezőket is csoportosítás. Ebben a példában a számítógépen, egy adott % számláló keresek, és szeretném, hogy mi minden számláló az óránkénti 70 percentilisek:

```
Type:Perf Computer=beefpatty4 CounterName=%* InstanceName=_Total | measure percentile70(CounterValue) by CounterName Interval 1HOUR
```
Megjegyzés egyvalamihez található, hogy ezek a lekérdezések nem teljesítmény számláló korlátozódik. Alkalmazhat rájuk tetszőleges metrikus. Ebben a példában a W3C IIS naplók keresek. Szeretnék tudni, hogy mi az a maximális időt átveszi az 5 perc intervallumát minden kérés végrehajtása:

```
Type:W3CIISLog | measure max(TimeTaken) by csMethod Interval 5MINUTES
```

### <a name="use-multiple-aggregates-in-one-query"></a>Több összegzések használni egy lekérdezésben
Több összesítő záradékok mérték parancs adhatja meg.  Mindegyik aliassal egymástól függetlenül is lehet.  Ha nem történik meg egy aliast a létrejött a mező neve lesz, az összesít függvény, amely használja (azaz "avg(CounterValue)" avg(CounterValue)) számára.

 ```
Type=WireData | measure avg(ReceivedBytes), avg(SentBytes) by Direction interval 1hour
```
![MOBILE-multiaggregates1](./media/log-analytics-log-searches/oms-multiaggregates1.png)

Íme egy másik példa:
 ```
* | measure countdistinct(Computer) as Computers, count() as TotalRecords by Type
```


## <a name="next-steps"></a>Következő lépések

Log keresések kapcsolatos további információkért lásd:

- Log keresés kiterjesztése a [napló Analytics egyéni mezők](log-analytics-custom-fields.md) használatával.
- Tekintse át a [napló Analytics jelentkezzen be a keresés hivatkozás](log-analytics-search-reference.md) megjelenítése minden kereséshez használható mezők és metszettel napló Analytics érhető el.
