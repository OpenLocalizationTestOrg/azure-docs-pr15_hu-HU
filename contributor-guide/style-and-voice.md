<properties title="" pageTitle="Írás Azure dokumentáció - stílus- és hangposta cheat lap" description="Stílus- és hangposta információt találhat arról létrehozása az Azure dokumentáció Center műszaki tartalma." metaKeywords="" services="" solutions="" documentationCenter="" authors="tysonn" videoId="" scriptId="" manager="required" />

<tags ms.service="contributor-guide" ms.devlang="" ms.topic="article" ms.tgt_pltfrm="" ms.workload="" ms.date="12/16/2014" ms.author="glenga" />

#<a name="writing-azure-documentation---style-and-voice-cheat-sheet"></a>Írás Azure dokumentáció - stílus- és hangposta cheat lap

Az alábbiakban csal mutatók arról, hogy miként írja be az Azure-szolgáltatások és -technológiák technikai cikkeket tartalmazó lap. Szabályok érvényesek, hogy hoz létre új dokumentáció vagy frissíteni a meglévő dokumentációt.

Tájékoztassa a bA legalább:

- Helyesírás- és nyelvhelyesség-ellenőrzés jelölje be a témaköreit, még akkor is, ha kivágása és beillesztése a Word adunk rendelkezik.
- Használja a alkalmi és rövid hang – például, amelyeket beszélgetésben egy másik személy one-on-one.
- Egyszerű mondatok használja. Azok könnyebben megérthető, és az emberi és a gép fordítók könnyebben fordítja.

Az alábbi szakaszok tartalmaz egy további részletek:

+ [Egy vevő környezetbarát hangposta használata]
+ [Fontolja meg a honosítási és gépi fordítási]
+ [Nézze meg, hogy más stílus- és hangposta kapcsolatos problémák]


##<a name="use-a-customer-friendly-voice"></a>Egy vevő környezetbarát hangposta használata

Azt szeretné, hajtsa végre az alábbi elvek, ha azt írja be a műszaki tartalmat az Azure aspire. Hogy esetleg nem mindig kapják meg van, de szükség próbálkozni!

- **Használja a mindennapos szavak**: próbálja használni a természetes nyelvi, a szavakat, használja az ügyfelekkel; kevésbé hivatalos, de nem kisebb műszaki; az példákon mutatják be új fogalmak.

- **Tömören írni**: nem kell a válaszra szavakat, és nem feletti ismertetik. Megerősítő, és nem használja a felesleges szavakat vagy rengeteg jelei. Tartsa a mondatok rövid és tömör. A témakör fókuszáló megtartani. Ha egy tevékenységnek van egy minősítő, vigye a mondat vagy bekezdés elejére. Minimális is tartsa a jegyzetek számára. Képernyőkép mentheti, hogy szavak használatával.

- **Szeretne képet beolvasni a könnyen**: a legfontosabb dolog, amit először helyezi. Szakaszok használatával hosszú eljárások darabos kezelhetőbb csoportokba (12-nél több lépésben eljárások valószínűleg túl hosszúak) lépéseket. Képernyőkép használatával áttekinthetővé ad hozzá.

- **Empathy megjelenítése**: egy támogató hangjelzést használni a cikkben, hogy megőrizze jogi nyilatkozatok minimális. Tisztességesen feliratozni lesz bosszantóak vevőknek területen. Ellenőrizze, hogy a cikk koncentrál mi számít az ügyfélnek, csak nem adjon meg egy technikai előadás.

##<a name="consider-localization-and-machine-translation"></a>Fontolja meg a honosítási és gépi fordítási
A műszaki cikkek vannak fordítani több nyelvet, és néhány az adott piacokon módosulnak. Személyek is használhatja gépi fordítási a webes a technikai cikkek olvasható. Igen írja be az alábbi útmutatást szem előtt:

- **Ellenőrizze, hogy a cikk nem tartalmaz nyelvhelyesség-ellenőrzés, a helyesírás-ellenőrzés, vagy írásjelek hibák**: Ez az, amit az általános azt kell tennie. Markdown billentyűivel 2.0-s egyszerű helyesírás-ellenőrzőt tartalmaz, de kell is illessze be a (leképezett HTML-kód) a cikk a Wordbe, amely mellett egy nagyobb teljesítményű a helyesírás és nyelvhelyesség-ellenőrző tartalom.

- **Ellenőrizze a lehető legrövidebb mondatok**: összetett mondatot vagy záradékok lánccal megnehezítik fordítási. Felosztása mondatok, ha túl felesleges vagy hallható furcsa nélkül is megteheti. Nem igazán szeretnénk vagy másik nyelven írt cikkeket.

- **Használata egyszerű és egységes mondat építés**: konzisztencia célszerűbb fordítási. Parentheticals és asides elkerülése érdekében, és a tárgyat, a lehető mondat elejére közelébe kell. Nézze meg néhány közzétett témakörök –, ha a témakör a rövid, olvassa el a stílus, a modellben vele egyszerű tartalmaz.

- **Egységes szövege használata és a nagybetűk**: ismét konzisztencia-e billentyűt. Azure használja a mondat géphez címéhez, így soha ne legyen nagybetű egy word, ha nem egy mondatot vagy egy megfelelő főnévi elején.

- **Belefoglalás "kis szót"**: szavakat, amelyek akkor fontolja meg, a kis- és angolul lényegtelen mert környezetben értelmezni, hogy (például "a", "a", ",", és a "van") roppant fontos a gépifordítás - felejtse el beírni!

##<a name="other-style-and-voice-issues-to-watch-for"></a>Nézze meg, hogy más stílus- és hangposta kapcsolatos problémák

- Nem oldaltörés megjegyzéseivel vagy asides lépések mentése.

- A lépések, amelyek kódtöredék tartalmaznak üzembe további információt a lépés a kódot megjegyzésként. Ez a csökkenti a felhasználóknak ki kell majd olvassa el a szöveg méretét, és a termékkulccsal kapcsolatos adatokat kapja értékeit másolja a program a kód projekt figyelmeztethet másokat a Mi a kód tesz a Ha az azok a hivatkozott később.

- A hivatalos termék neve "Microsoft Azure", de majdnem mindig csak mondani, hogy "Azure", "Azure Mobile Services" ahogy.

- "MA" vagy "Válaszokhoz" kezdetű mozaikszavak nem hoz létre. Csak használata "Azure" a szolgáltatás vagy szolgáltatás neve előtt első hivatkozással elérhető, és engedje el (például "Azure Mobile Services" "Mobile szolgáltatások" után válik első használat). Lehetőleg ne a mozaikszavak általános – csak személyek tévessze.

- Azure mondat géphez címeket használja.

- Használja a "bejelentkezési" és a nem "napló modul."

- A "következő" szavak vagy a "következő" minden mondatot, a program egy lista vagy a kód kódtöredék bele.

- "SQL-adatbázis" található az Azure szolgáltatás. A "SQL-adatbázis" egy adatbázis-példány futó SQL-adatbázis.

- Azure tároló több "management adatszolgáltatások", amely a táblázat szolgáltatás, a Blob-szolgáltatás és a várólista szolgáltatást tartalmaz. (Ez hívása nem a "Azure táblázat tárhelyszolgáltatáshoz".)




###<a name="contributors-guide-links"></a>Munkatársak naptárának útmutató hivatkozások

- [A cikk – áttekintés](./../README.md)
- [Index útmutatást cikkek](./contributor-guide-index.md)



<!--Anchors-->
[Egy vevő környezetbarát hangposta használata]: #use-a-customer-friendly-voice
[Fontolja meg a honosítási és gépi fordítási]: #consider-localization-and-machine-translation
[Nézze meg, hogy más stílus- és hangposta kapcsolatos problémák]: #other-style-and-voice-issues-to-watch-for
