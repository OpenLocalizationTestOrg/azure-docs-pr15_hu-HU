<properties
    pageTitle="Az Azure CDN él teljesítményelemző |} Microsoft Azure"
    description="Teljesítményelemző él csomópontot a Microsoft Azure CDN. Él teljesítmény Analytics biztosít a CDN részletes információkat forgalmat és sávszélesség használatát."
    services="cdn"
    documentationCenter=""
    authors="camsoper"
    manager="erikre"
    editor=""/>

<tags
    ms.service="cdn"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/28/2016"
    ms.author="casoper"/>

# <a name="analyze-edge-node-performance-in-microsoft-azure-cdn"></a>A Microsoft Azure CDN él csomópont-teljesítmény elemzése

[AZURE.INCLUDE [cdn-premium-feature](../../includes/cdn-premium-feature.md)]

## <a name="overview"></a>– Áttekintés
Él teljesítmény analytics biztosít a CDN részletes információkat forgalmat és sávszélesség használatát. Ez az információ majd trendfigyelésre statisztika, amelyek lehetővé teszik, hogy hogyan eszközeit rendszer éppen ismereteket szerezhet létrehozásához használható gyorsítótárazott, és az ügyfelek sikeresen kézbesítve lett. A Ez lehetővé teszi, hogy egy stratégia űrlap a hogyan optimalizálhatja a a tartalmakat, és határozza meg, milyen problémákkal kell megoldásra jobb emelés az a CDN-t. Eredményt adja nem csak akkor fogja tudni adatokat a kézbesítési teljesítmény javítása érdekében, de is tudják a CDN költségek csökkentése.

> [AZURE.NOTE] Az összes jelentések meghatározása a dátum/idő UTC/GMT jelölés használatával.

## <a name="reports-and-log-collection"></a>Jelentések és a napló gyűjtemény

CDN-tevékenységadatok kell gyűjti össze a szegély teljesítmény Analytics modul előtt, jelentéseket, hozhatnak létre. A webhelycsoport folyamat fordul elő, ha nap, és ismerteti azokat a tevékenységet, amely során az előző napra következett. Ez azt jelenti, hogy egy jelentés statisztika feldolgozása, és nem feltétlenül időben jelenítik meg a nap statisztikai minta a teljes készlete adatokat tartalmazzák az adott napra. Ezeket a jelentéseket elsődleges funkciója mérje fel, hogy a teljesítmény. Azok a számlázási célból vagy a pontos numerikus statisztika nem használható.

> [AZURE.NOTE] A szegély teljesítmény analitikus jelentések létrehozása, amelyhez nyers adatokból legalább 90 napig érhető el.

## <a name="dashboard"></a>Irányítópult

A szegély teljesítmény statisztika irányítópultján nyomon követi a jelenlegi és korábbi CDN forgalom az egy diagram és a statisztikát. Használja az irányítópult feltárása a legutóbbi és a hosszú távú trendek CDN-forgalmat a fiók teljesítményét.

Az irányítópult áll:

* Interaktív diagramot, amely lehetővé teszi a Megjelenítés kulcs mértékek és trendek.
* Ütemterv, a hosszú távú mintázatok értelmezhető nyújt kulcs mértékek és a trendek.
* Fő mértékek és statisztikai adatokat, hogy miként CDN-hálózatba javítja a mért általános teljesítményt, használatát és hatékonyabb webhely forgalmat.

### <a name="accessing-the-edge-performance-dashboard"></a>A szegély teljesítmény irányítópult elérése

1. Az a CDN a profil lap a **kezelés** gombra.

    ![CDN-profil lap kezelése gomb](./media/cdn-edge-performance/cdn-manage-btn.png)

    Ekkor megnyílik a CDN kezelőportálja segítségével.

2. Mutasson a **Analytics** fülre, majd mutasson rá az **Él teljesítmény Analytics** előugró.  Kattintson az **irányítópulton**.

    A szegély csomópont statisztika irányítópultján jelenik meg.

### <a name="chart"></a>Diagram

Az irányítópult olyan diagramot, amely egy mérőszám követi az alatta megjelenő ütemterven kijelölt időszakban tartalmazza.  Ütemterv, hogy sokféle be az utolsó két év CDN tevékenység közvetlenül a diagram alatt jelenik meg.

#### <a name="using-the-chart"></a>A diagram használata

* Alapértelmezés szerint az elmúlt 30 nap gyorsítótár hatékonyság beírásával forrásadatok lesz.
* Ez a diagram jön létre a naponta rendezve adatokból.
* A vonaldiagram a nap mutatva jelzi a dátum- és az érték a mérőszám azon a napon.
* Kattintson a kiemelés hétvégék válthat az egyszerűsített szürke függőleges vonal megjelenítő hétvégék alakzatot a diagram egy további. Átfedő ilyen típusú esetén hasznos forgalmat azonosító hétvégék fölé.
* Kattintson a nézet egy év korábbi Váltás az előző év tevékenység egy további azonos időszakban alakzatot a diagramot. Összehasonlító ilyen típusú hosszú távú CDN szokásai betekintést nyújt. A diagram jobb felső sarokban, amely jelzi az egyes vonaldiagram a színkód jelmagyarázat tartalmazza.

#### <a name="updating-the-chart"></a>A diagram frissítése

* Időtartomány: Hajtsa végre az alábbiak egyikét:
    * Jelölje ki a kívánt terület az ütemterv. A diagram, amely megfelel az adott időszakra adatokkal frissülnek.
    * Kattintson duplán a két év legfeljebb az összes rendelkezésre álló korábbi adatok megjelenítése a diagramon.
* Metrikus: Kattintson a kívánt mérőszám mellett látható diagram ikonra. Az ütemterv és a diagram frissíti a megfelelő mérőszám adatokkal.


### <a name="key-metrics-and-statistics"></a>Fő mértékek és statisztika

#### <a name="efficiency-metrics"></a>Hatékonyság mérőszámok

Ezek a mértékek célja kattintva megtekintheti, hogy gyorsítótár hatékonyság javítható. Gyorsítótár hatékonyság származás fő előnye van:

* A forráskiszolgálóval, amely vezethet csökkentett terheltsége:
    * Jobb webkiszolgáló teljesítményét.
    * Csökkentett működési költségeket.
* Továbbfejlesztett adatok kézbesítési gyorsítás, mivel több kérést szolgáltató közvetlenül az a CDN-t.

A mező | Leírás
------|------------
Gyorsítótár hatékonyság | Százalékot gyorsítótárból kombinációja átvitt adatok mennyiségét mutatja. A metrikus mértékek, amikor a kért tartalom gyorsítótárazott verzióját lett felszolgált közvetlenül az a CDN (peremhálózat-kiszolgálói) igénylő (például webböngésző)
Találati arány | Gyorsítótárból kombinációja kérelmek arányát jelzi. A metrikus mértékek, amikor a kért tartalom gyorsítótárazott verzióját a igénylő (például webböngésző) közvetlenül az a CDN (peremhálózat-kiszolgálói) lett fel.
a távoli bájt - nincs gyorsítótár Config % | Azt jelzi, hogy a forgalmat, a CDN-re (peremhálózat-kiszolgálói), amely nem lesz a figyelmen kívül hagyása gyorsítótár-szolgáltatás (HTTP szabályok motor) eredményeként gyorsítótárba origin kiszolgálókról lett felszolgált százalékát.
a távoli bájt - lejárt gyorsítótár % | Elavult tartalom ismételt érvényesítése eredményeként forgalom kombinációja lett origin kiszolgálókról a CDN-re (peremhálózat-kiszolgálói) arányát jelzi.

#### <a name="usage-metrics"></a>Mértékek használata

Ezek a mértékek célja a következő költség-kivágása intézkedéseket betekintést nyújt:

* A CDN keresztül működési költségek minimalizálása.
* CDN-kiadások gyorsítótár hatékonyság és a tömörítési csökkentése.

> [AZURE.NOTE] Adatforgalom mennyiségi számok arányok és százalékos értékekkel kapcsolatos számításokban használt, és előfordulhat, hogy csak egy részét a teljes forgalom az megjelenítése nagy mennyiségű ügyfelek forgalom jelenítik meg.

A mező | Leírás
------|------------
Kimenő bájtok mentése | Azt jelzi, hogy az átvitt adatok számára az igénylő (például webböngésző) az a CDN (peremhálózat-kiszolgálói) felszolgált kérések átlagos száma.
Nincs gyorsítótár Config bájt sebessége | Azt jelzi, hogy a forgalmat a figyelmen kívül hagyása gyorsítótár-szolgáltatás miatt nem lehet gyorsítótárba igénylő (például webböngésző) az a CDN (peremhálózat-kiszolgálói) a felszolgált százalékát.
Tömörített bájt ráta | Az a CDN (peremhálózat-kiszolgálói) igénylő (például webböngésző) küldött forgalmat arányát jelzi tömörített formátumban.
Kimenő bájtok | Azt jelzi, hogy a bájtokban, hogy az igénylő (például webböngésző) az a CDN (peremhálózat-kiszolgálói) kézbesítve lettek a adatok mennyiségét.  
Bájt | CDN-re (peremhálózat-kiszolgálói) jelzi a igénylő (például webböngésző) küldött bájtos, az adatok mennyiségét.
Távoli bájt | Azt jelzi, hogy bájtos, a CDN (peremhálózat-kiszolgálói) küldött CDN és ügyfélszolgálatának origin-kiszolgálókról az adatok mennyiségét.

#### <a name="performance-metrics"></a>Teljesítménymutatók

Ezek a mértékek célja a forgalom általános CDN teljesítményének nyomon követéséhez.

A mező | Leírás
------|------------
Ráta átadása | Azt jelzi, hogy az átlagos ráta, amelyen tartalom áthelyezték az a CDN-t egy igénylő.
Időtartam | Azt jelzi, az átlag idő ezredmásodpercben tartott végzi a tárgyi eszköz egy igénylő (például webböngésző).
Tömörített kérelem ráta | Azt jelzi, hogy az igénylő (például webböngésző) az a CDN (peremhálózat-kiszolgálói) kézbesítve lettek a találatok százalékos tömörített formátumban.
4xx hiba ráta | Által generált 4xx állapotkódot találatok arányát jelzi.
5XX hiba ráta | Által generált 5xx állapotkódot találatok arányát jelzi.
A találatok | CDN-tartalom kérelem számát jelzi.

#### <a name="secure-traffic-metrics"></a>Biztonságos forgalom mérőszámok

Ezek a mértékek célja HTTPS-forgalom CDN teljesítményének nyomon követéséhez.

A mező | Leírás
------|------------
Biztonságos gyorsítótár hatékonyság | Azt jelzi, hogy azok felszolgált HTTPS-kérések gyorsítótárból adatforgalmat százalékos. A metrikus mértékek, amikor a kért tartalom gyorsítótárazott verzióját lett fel közvetlenül az a CDN (peremhálózat-kiszolgálói) igénylő (például webböngésző) a HTTPS.
Biztonságos átviteli ráta | HTTPS azt jelzi, az átlag ráta, amelyen tartalom áthelyezték az a CDN (peremhálózat-kiszolgálói) igénylő (például webkiszolgáló).
Átlagos biztonságos időtartam | Azt jelzi, az átlag idő ezredmásodpercben tartott tárgyi eszköz egy igénylő (például webböngésző) végzi a HTTPS.
Biztonságos találatok | CDN-tartalom HTTPS-kérések számát jelzi.
Kimenő biztonságos bájtok | Azt jelzi, hogy HTTPS-forgalom bájtokban, hogy az igénylő (például webböngésző) az a CDN (peremhálózat-kiszolgálói) kézbesítve lettek mennyiségét.

## <a name="reports"></a>Jelentések

Minden alkalommal a modul egy diagram és a sávszélesség és a forgalom használatát a különböző típusú mértékek statisztikák tartalmaz (például HTTP állapot kódokat gyorsítótár állapot kódokat kérés URL-cím stb.). Ez az információ használható mélyreható elmélyedhet hogyan tartalom kiszolgált az ügyfelek számára, és testre szabhatja az adatokat a kézbesítési teljesítmény javítása érdekében CDN viselkedését.

### <a name="accessing-the-edge-performance-reports"></a>A szegély teljesítményjelentéseinek elérése

1. Az a CDN a profil lap a **kezelés** gombra.

    ![CDN-profil lap kezelése gomb](./media/cdn-edge-performance/cdn-manage-btn.png)

    Ekkor megnyílik a CDN kezelőportálja segítségével.

2. Mutasson a **Analytics** fülre, majd mutasson rá az **Él teljesítmény Analytics** előugró.  Kattintson a **nagy HTTP-objektumot**.

    A szegély csomópont analytics jelentések képernyő jelenik meg.

Jelentés | Leírás
-------|------------
Napi összegzése | Napi forgalom trendek megtekintése a megadott időszakban teszi lehetővé. A grafikonon mindegyik sáv az adott dátumhoz. A sáv méretének azt jelzi, hogy mikor történt dátum találatok száma.
Óránkénti összegzése | Lehetővé teszi a megadott időszakban óránkénti forgalom trendjeinek megtekintése. A grafikonon minden sáv egy adott napon egyetlen órán jelöli. A sáv méretének azt jelzi, hogy óra előfordult találatok száma.
Protokollok | Megjeleníti a részletezzük, hogy a forgalmat a HTTP- és HTTPS protokollt között. Gyűrű diagram azt jelzi, hogy minden egyes protokoll esetében előfordult találatok százalékát.
HTTP módszerek | Ismerkedjen meg a HTTP-módszerek az adatok kérése használják teszi lehetővé. A leggyakoribb HTTP kérések általában kérése, a fej és a BEJEGYZÉST. Gyűrű diagramot, hogy mikor történt az egyes HTTP kérelem módszer: a találatok arányát jelzi.
URL-címei | A felső 10 kért URL-címek megjelenítő diagram tartalmazza. A sáv egyes URL-cím jelenik meg. A sáv magasságát azt jelzi, hogy hány találatok által generált adott URL-CÍMÉT az időszak alatt jelentés tárgyát. A legelső 100 statisztikai kért URL-címek közvetlenül a diagram alatt jelennek meg.
CNAME | Használható eszközök kérni az idő múlásával 10 CNAME időtartomány egy jelentés felső megjelenítő diagram tartalmazza. A legelső 100 statisztikai kért CNAME közvetlenül a diagram alatt jelennek meg.
Forrásokból | A felső 10 CDN megjelenítő diagram vagy a vevő origin kiszolgálók, amelyhez egy adott időszakra vetített állapotával eszközök felkérték tartalmaz. A legelső 100 statisztikai kért CDN vagy a vevő origin kiszolgálók közvetlenül a diagram alatt jelennek meg. Ügyfél origin kiszolgálókat azonosítja a mappanév beállítás definiált nevével.
GEO POP | Megtudhatja, hogy mennyi a forgalom az érkezik egy adott pont-a-jelenléti keresztül (POP). A rövidítésként a POP, az a CDN-hálózatba jelöli.
Ügyfelek | A felső 10 ügyfél által kért eszközök egy adott időszakra vetített állapotával megjelenítő diagram tartalmazza. Ez a jelentés alkalmazásában ugyanazt a címet származik összes kérelem minősülnek a azonos ügyfélprogramból. A legelső 100 ügyfélalkalmazások statisztika közvetlenül a diagram alatt jelennek meg. Ez a jelentés akkor lehet hasznos letöltési tevékenység mintázatok meghatározásakor a felső ügyfelei számára.
Gyorsítótár állapotok | Előfordulhat, hogy jelenítse meg az általános végfelhasználói élmény javítására az alábbi módszerek gyorsítótár viselkedés részletes kifejtése biztosít. Mivel a leggyorsabb teljesítményt gyorsítótár-találatok származik, optimalizálhatja az adatok kézbesítési sebesség gyorsítótár mért sikertelen találatok minimalizálása és a lejárt gyorsítótár-találatok.
Nincs adatai | A felső 10 URL-címeit, amelynek gyorsítótár tartalom frissessége nincs bejelölve egy adott időszakra vetített állapotával eszközök megjelenítő diagram tartalmazza. A legelső 100 URL-címek az ilyen eszközök statisztika közvetlenül a diagram alatt jelennek meg.
CONFIG_NOCACHE részletei | A 10 legjobb URL-ek a elemeket, amelyeket az ügyfél CDN-konfigurációja miatt nem gyorsítótárazott megjelenítő diagram tartalmazza. Az ilyen típusú eszközök közvetlenül a forráskiszolgálóval a voltak fel. A legelső 100 URL-címek az ilyen eszközök statisztika közvetlenül a diagram alatt jelennek meg.
UNCACHEABLE részletei | A felső 10 URL-ek a elemeket, amelyeket nem gyorsítótárba miatt kérelem élőfej adatokat megjelenítő diagram tartalmazza. A legelső 100 URL-címek az ilyen eszközök statisztika közvetlenül a diagram alatt jelennek meg.
TCP_HIT részletei | A felső 10 URL-ek a elemeket, amelyeket közvetlenül a gyorsítótárból felszolgált megjelenítő diagram tartalmazza. A legelső 100 URL-címek az ilyen eszközök statisztika közvetlenül a diagram alatt jelennek meg.
TCP_MISS részletei | A felső 10 URL-ek a elemeket, amelyeket TCP_MISS gyorsítótár állapota megjelenítő diagram tartalmazza. A legelső 100 URL-címek az ilyen eszközök statisztika közvetlenül a diagram alatt jelennek meg.
TCP_EXPIRED_HIT részletei | A POP-ről kombinációja elavult eszközök felső 10 URL-címeit megjelenítő diagram tartalmazza. A legelső 100 URL-címek az ilyen eszközök statisztika közvetlenül a diagram alatt jelennek meg.
TCP_EXPIRED_MISS részletei | A felső 10 URL-címeit, amelyhez új verzió kellett lehet beolvasni a forráskiszolgálóval elavult eszközök megjelenítő diagram tartalmazza. A legelső 100 URL-címek az ilyen eszközök statisztika közvetlenül a diagram alatt jelennek meg.
TCP_CLIENT_REFRESH_MISS részletei | A sávdiagram, amely megjeleníti a 10 legjobb URL-címét, az eszközök beolvasott miatt nem-gyorsítótár kérelmének az ügyféltől origin Server tartalmazza. A legelső 100 URL-címek az ilyen kérések statisztika közvetlenül a diagram alatt jelennek meg.
Lekérdezésiügyfél-típusok kérés | HTTP-ügyfelek (például böngészőkben) által végzett kérések típusáról. Ez a jelentés, amely tartalmaz egy ilyesmire lehetőség, hogy hogyan kérések kezelt gyűrű diagramot tartalmaz. Az egyes kérelem: sávszélessége és a forgalom adatai megjelennek a diagram alatt.
Felhasználói ügynököt | A felső 10 felhasználói ügynökök kérni az a CDN-től a tartalom megjelenítése oszlopdiagramon tartalmazza. Egy felhasználói ügynököt általában egy webböngészőben, media player vagy mobiltelefonon böngészőben. A legelső 100 felhasználói ügynökök statisztikai közvetlenül a diagram alatt jelennek meg.
Ajánlók | A 10 legjobb ajánlók a CDN keresztül érhető el a tartalom megjelenítése oszlopdiagramon tartalmazza. Egy referrer általában a weblap vagy erőforrás, a tartalom mutató URL-CÍMÉT. Részletes információkat a legelső 100 ajánlók hiányzik a diagram alatt.
A tömörítési típusai | E tömörített azokat meg él kiszolgáló által kért eszközök lefelé töréspontok gyűrű diagramot tartalmaz. A százalékos tömörített eszközök használt tömörítés típusú van bontásban. Részletes információkat minden tömörítés típus és az állapot hiányzik a diagram alatt.
Fájltípusok | A felső 10 fájltípusok keresztül a CDN fiókjának kérő megjelenítő oszlopdiagramon tartalmazza. Ez a jelentés alkalmazásában, a fájl típusa határozza meg az eszköz kiterjesztésű és Internet médiatípus (például .html \[szöveg vagy html\], .htm \[szöveg vagy html\], .aspx \[szöveg vagy html\]stb.). Részletes információkat a legfelső 100 fájltípusok hiányzik a diagram alatt.
Egyedi fájlok | Diagram, amely a teljes oldalszám egyedi eszközök, amelyek egy adott napon egy adott időszakra vetített állapotával felkérték rajzolja tartalmazza.
Jogkivonat Auth összegzése | Ez a témakör gyors áttekintést olvashat arról e védett jogkivonat-alapú hitelesítés által kért eszközök tortadiagramhoz tartalmazza. Védett eszközök jelennek meg a diagram a kísérlet hitelesítési eredményének megfelelően.
Jogkivonat Auth megtagadja részletei | Olyan oszlopdiagram, amely lehetővé teszi, hogy a felső 10 kérelmeket, amely miatt jogkivonat-alapú hitelesítés voltak megtagadva megtekintése tartalmazza.
HTTP-válasz kódok | Részletezzük, hogy a HTTP-állapot kódokat tartalmaz (például 200 az OK gombra, 403 mozgást, 404 nem található, stb.), amely kézbesítve lettek a HTTP-ügyfelek meg él-kiszolgálók. Kördiagram lehetővé teszi, hogy gyorsan mérje fel, hogy hogyan eszközeit felszolgált volt. Részletes statisztikai adatai minden egyes válasza kódra alatt a diagram.
404-es hibák | Tartalmaz, amely lehetővé teszi, hogy a felső 10 kérelmeket, nem található válasz 404-es kódot eredményező megjelenítése oszlopdiagramon.
403 hibák | Olyan oszlopdiagram, amely lehetővé teszi, hogy a felső 10 kérelmek 403 Tiltott válasz kódot eredményező megtekintése tartalmazza. Tiltott 403 válasz kódot fordul elő, ha a kérést egy ügyfél forráskiszolgálóval, vagy kattintson a POP-biztonsági kiszolgálójának megtagadva.
4xx hibák | Tartalmaz, amely lehetővé teszi, hogy a felső 10 kérelmek egy válasz kódot 400 tartomány eredményező megjelenítése oszlopdiagramon. A jelentés tartoznak 403 nem található, és a tiltott-válasz 404-es kódot. Általában 4xx válasz kódot történik, amikor egy kérelmet megtagadva eredményeként egy ügyfél hiba.
504 hibák | Olyan oszlopdiagram, amely lehetővé teszi, hogy a felső 10 kéréseket 504 átjáró időtúllépése válasz kódot eredményező megtekintése tartalmazza. Egy átjáró időtúllépése 504 válasz kódot fordul elő, ha az időtúllépés történik, ha a HTTP-proxy partnere kapcsolatba próbál lépni egy másik kiszolgálóval. Az a CDN, esetében 504 átjáró időtúllépése válasz kódot általában fordul elő, ha egy biztonsági kiszolgálójának nem tudja létrehozni egy ügyfél forráskiszolgálóval való kommunikáció.
502 hibák | Olyan oszlopdiagram, amely lehetővé teszi, hogy a felső 10 kérelmek 502 Hibás átjáró válasz kódot eredményező megtekintése tartalmazza. 502 Hibás átjáró válasz kódot fordul elő, ha a HTTP-protokoll hiba fordul elő, a kiszolgáló és a HTTP-proxy közötti. Az a CDN, esetében egy 502 Hibás átjáró válasz kódot általában fordul elő, ha egy ügyfél forráskiszolgálóval egy biztonsági kiszolgálójának érvénytelen válasz adja eredményül. A válasz nem érvényes, ha nem elemezhető, vagy ha még nem teljes.
5XX hibák | Tartalmaz, amely lehetővé teszi, hogy a felső 10 kérelmek egy válasz kódot 500 tartomány eredményező megjelenítése oszlopdiagramon.  A jelentés tartoznak 502 – Hibás átjáró és 504 átjáró időtúllépése válasz kódok.

## <a name="see-also"></a>Lásd még:
* [Azure CDN – áttekintés](cdn-overview.md)
* [A Microsoft Azure CDN valós idejű stat](cdn-real-time-stats.md)
* [A szabályok motor használata alapértelmezett HTTP működés felülbírálása](cdn-rules-engine.md)
* [Speciális HTTP-jelentések](cdn-advanced-http-reports.md)
