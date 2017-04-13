<properties
    pageTitle="Azure esemény hubok archív |} Microsoft Azure"
    description="Az Azure esemény hubok Archiválás szolgáltatás áttekintése."
    services="event-hubs"
    documentationCenter=""
    authors="djrosanova"
    manager="timlt"
    editor=""/>

<tags
    ms.service="event-hubs"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/13/2016"
    ms.author="darosa;sethm"/>

# <a name="azure-event-hubs-archive"></a>Azure esemény hubok archiválása

Azure esemény hubok archiválás lehetővé teszi, hogy automatikusan előadása a adatfolyam az esemény hubok egy Blob tároló fiókkal lehetőség a rugalmasabb adja meg a kívánt időtartamot, vagy a méretezés időtartományt a választás az. Állítsa be az archiválás gyors, vannak nem futtatható felügyeleti költségek, és az esemény hubok [átviteli egységek](event-hubs-overview.md#capacity-and-security)automatikus méretezés. Esemény hubok archívum adatfolyam betöltése Azure legegyszerűbb módja, és lehetővé teszi, hogy kiemelése rögzítése, hanem adatfeldolgozás.

Azure esemény hubok archiválás lehetővé teszi, hogy az azonos adatfolyam a valós idejű és köteg folyamatok feldolgozása. Ez lehetővé teszi, hogy megoldások, amelyek lehet nagyobb, az adott idő alatt az igényeinek. Köteg alapú rendszerekhez ma felé jövőbeli valós idejű feldolgozás figyelje szeretne összeállítani, vagy egy hatékony hideg elérési út hozzáadása meglévő valós idejű megoldás szeretné, hogy az esemény hubok archív adatfolyam-adatok használata lehetővé teszi.

## <a name="how-event-hubs-archive-works"></a>Hogyan működik a esemény hubok archiválása

Esemény hubok telemetriai bejövő adatok, elosztott naplózási hasonlít az idő-adatmegőrzési tartós puffer. Ha az esemény hubok kulcsa a [particionált fogyasztói modell](event-hubs-overview.md#partition-key). Egyes partíciók-adatok független szakaszához, és önállóan felhasznált. Ki, éves adatok időbeli az konfigurálható az adatmegőrzési időszak alapján. Emiatt egy adott esemény hubhoz soha nem kap "túl teljes."

Esemény hubok archiválás lehetővé teszi, hogy adja meg a saját fiók Azure Blob-tárolóhoz és tároló, hogy tárolja az archivált adatokat használt. Ehhez a fiókhoz lehet ugyanaz az esemény központi régió vagy egy másik régiójában, az a lehetősége az esemény hubok Archiválás szolgáltatás hozzáadása.

Archivált adatok írott [Apache Avro][] formátumba; tömör, gyors, bináris formátum beágyazott séma használata gazdag adatszerkezetek tartalmazza. Ez a formátum elterjedt Hadoop ökológiai és Értékáram-elemzés és Azure Data Factory. A jelen cikk további információt a Avro használata érhető el.

### <a name="archive-windowing"></a>Archiválás ablakok elrendezése

Esemény hubok archiválás lehetővé teszi, hogy kattintva állítsa be az archiválás ablak beállítása. Ebben az ablakban egy minimális méret és az "első wins házirend" azaz amely az első eseményindító ütközött archív művelet időpontja konfigurációs. Ha a 15-perc/100 MB archívum ablakban, és 1 MB/s küldése, a méret ablakban indítja az időkeret előtt. Egyes partíciók független archívumok, és egy befejezett blokk blob írása archívum, időben, mikor történt, az archiválás intervallum alkalommal nevű. Az elnevezésük is egységes segédprogram a következő:

```
<Namespace>/<EventHub>/<Partition>/<YYYY>/<MM>/<DD>/<HH>/<mm>/<ss>
```

### <a name="scaling-to-throughput-units"></a>Átviteli egységek méretezése

Esemény hubok forgalom [átviteli egységek](event-hubs-overview.md#capacity-and-security)szabályozza. Egy átviteli időegység lehetővé teszi, hogy 1 MB/s vagy a be- és kilépési ennyi kétszer másodpercenként 1000 események. Szabványos esemény hubok 1-20 átviteli egységekkel beállítható, és további vásárolhatja kvóta növelése [támogatás kérése][]keresztül. Folyamatban van a megvásárolt átviteli egységek túl használatát. Esemény hubok archív adatait másolja közvetlenül a belső esemény hubok tároló kihagyásával az átvitel egység kilépési kvóták és a kilépési mentése más feldolgozás olvasók, például Értékáram-elemzés vagy külső.

Beállítása után esemény hubok archív automatikusan elindul, amint az első esemény küldi. Futó mindig továbbra is. Meg szeretné könnyíteni a későbbi feldolgozásához tudnivalók a folyamat működését, esemény hubok ír üres fájlokat, ha nincs adat. Ez a kiszámíthatóan cadence és jelölő, amely a köteg processzorok is hírcsatorna biztosít.

## <a name="setting-up-event-hubs-archive"></a>Esemény hubok archív beállítása

Esemény központi létrehozási idejének az portál vagy az erőforrás-kezelő Azure esemény hubok archív is be kell állítani. **A** gombra kattintva engedélyezze archív egyszerűen. A lap **tároló** szakasza gombra kattintva beállíthatja a tárhely és a tároló. Mivel esemény hubok Archiválás szolgáltatás hitelesítést használ a tároló, nem kell adja meg a tárhely kapcsolati karakterlánc. Az erőforrás-választó automatikusan kijelöli az erőforrás URI a tárterület-fiókjához. Ha Azure erőforrás-kezelő használja, meg kell adnia a URI kifejezetten karakterláncként.

Az érték öt perc az alapértelmezett időkeret. A legkisebb értéke 1, a maximális 15. A **méret** ablakban a 10-500 MB-os tartomány rendelkezik.

![][1]

## <a name="adding-archive-to-an-existing-event-hub"></a>Egy meglévő eseményt hubhoz archív hozzáadása

Archívumok a meglévő eseményt hubok szereplő egy esemény hubok névtér beállíthatók. A régebbi üzenetek vagy vegyes típus névtér a funkció nem érhető el. Ahhoz, hogy a egy meglévő eseményt hubhoz archív, vagy módosíthatja az archiválási beállításokat, kattintson a névtér az **alapvető tudnivalók** lap betöltése, majd kattintson az esemény-központban, amelynek meg szeretné engedélyezése, vagy módosítsa az archiválás beállítást. Végül kattintson a Megnyitás a lap **Tulajdonságok** részén az alábbi ábrán látható módon.

![][2]

Esemény hubok archív Azure erőforrás-kezelő sablonok keresztül is beállítható. További tudnivalókat [ismertető](event-hubs-resource-manager-namespace-event-hub-enable-archive.md)témakört is.

## <a name="exploring-the-archive-and-working-with-avro"></a>Az archiválás feltárása és Avro használata

Beállítása után az esemény hubok archív a Azure-tárhely és a tárolóhoz adni a beállított időkeret fájlokat hoz létre. Ezek a fájlok megtekintése bármely eszközben, például az [Azure tároló Explorer][]. A fájlok helyi hozzájuk letöltheti.

Az esemény hubok archív készített fájlokat a következő Avro séma van:

![][3]

Avro fájlok feltárása legegyszerűbben, a [Avro eszközök][] üveg a Apache használatával. A üveg a letöltés után megjelenik a következő parancs futtatásával a séma egy konkrét Avro fájl:

```
java -jar avro-tools-1.8.1.jar getschema \<name of archive file\>
```

Ez a parancs adja eredményül.

```
{

    "type":"record",
    "name":"EventData",
    "namespace":"Microsoft.ServiceBus.Messaging",
    "fields":[
                 {"name":"SequenceNumber","type":"long"},
                 {"name":"Offset","type":"string"},
                 {"name":"EnqueuedTimeUtc","type":"string"},
                 {"name":"SystemProperties","type":{"type":"map","values":["long","double","string","bytes"]}},
                 {"name":"Properties","type":{"type":"map","values":["long","double","string","bytes"]}},
                 {"name":"Body","type":["null","bytes"]}
             ]
}
```

Avro eszközök segítségével JSON formátum konvertálja a fájlt, és hajtsa végre a többi feldolgozása.

Speciális feldolgozás végezhet, töltse le és telepítse a Avro platform választási számára. Az írás időben nincsenek megvalósítás C, C++, C\#, Java, NodeJS, Perl, PHP, Python és fonetikus.

Apache Avro teljes első lépések útmutatóinak [Java][] és [Python][]rendelkezik. Az [Esemény hubok archív – első lépések](event-hubs-archive-python.md) a cikk is olvasható.

## <a name="how-event-hubs-archive-is-charged"></a>Hogyan esemény hubok archív terheli.

Esemény hubok archív van forgalmi hasonlóképpen átviteli egységek, mint az óránkénti ingyenesen. A költség közvetlenül a névtér vásárolt átviteli mennyiségek arányos. Átviteli egységek nagyobb és csökkent esemény hubok archív növeli, és kevesebb egyező teljesítmény elérése érdekében. A méter fordulhat elő, a kapcsolatszolgáltatásokkal. A költség esemény hubok archívumhoz 0,10 $ átviteli egységre, a betekintő időszakban 50 %-os árengedménnyel felajánlott óránként.

Esemény hubok archív valóban legkönnyebben az adatok beolvasása az Azure. Azure adatok tó Azure Data Factory és Azure hdinsight szolgáltatáshoz használ, végezhet parancsfájl és más elemzésének a választás a bármely skála van szüksége a jól ismert eszközök és platformok használja.

## <a name="next-steps"></a>Következő lépések

Akkor talál további információt esemény hubok megtalálhatók az alábbi hivatkozások:

- Egy teljes [minta alkalmazást használó esemény hubok][].
- A [Méretezés meg az esemény hubok feldolgozása esemény][] minta.
- [Esemény hubok – áttekintés][]

[Apache Avro]: http://avro.apache.org/
[támogatási kérelem]: https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade
[1]: ./media/event-hubs-archive-overview/event-hubs-archive1.png
[2]: media/event-hubs-archive-overview/event-hubs-archive2.png
[Azure tároló Explorer]: http://azurestorageexplorer.codeplex.com/
[3]: ./media/event-hubs-archive-overview/event-hubs-archive3.png
[Avro eszközök]: http://www-us.apache.org/dist/avro/avro-1.8.1/java/avro-tools-1.8.1.jar
[Java]: http://avro.apache.org/docs/current/gettingstartedjava.html
[Python]: http://avro.apache.org/docs/current/gettingstartedpython.html
[Esemény hubok – áttekintés]: event-hubs-overview.md
[Esemény hubok használó minta alkalmazás]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097
[Esemény feldolgozása az esemény hubok méretezése]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-45f43fc3