<properties
    pageTitle="Adja hozzá a napló Analytics-megoldások a megoldástárból |} Microsoft Azure"
    description="Log Analytics-megoldások is logika, a megjelenítés és a adatbeszerzési gyűjteménye szabályok, amelyek biztosítanak a mértékek átalakítani egy adott probléma terület köré."
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

# <a name="add-log-analytics-solutions-from-the-solutions-gallery"></a>Adja hozzá a napló Analytics-megoldások a megoldástárból

Log Analytics-megoldások is **logika**, **megjelenítési** és **adatok WIA szabályokat** , amelyekkel átalakítani egy adott probléma terület köré mértékek gyűjteménye. Ez a cikk listák megoldások napló Analytics támogatott, és jelzi, hogy miként vehet fel, és eltávolíthatja őket a megoldástár használatával.

Megoldások való mélyebb háttérismeretek engedélyezése:

- vizsgálja meg, és gyorsabban működési problémák megoldásához
- különféle típusú adatokra gépi összehangolására és igénybevétele
- segít a megelőző tevékenységekkel, például a kapacitás tervezés, javítás jelentése és biztonsági naplózás kell.


>[AZURE.NOTE] Log Analytics tartalmazza a napló keresési funkciót is tartalmaz, így Önnek nem kell ahhoz, hogy megoldás telepítése. Azonban elérheti adatábrázolást, a javasolt keresések és az összefüggéseket a megoldástárban megoldások hozzáadásával.

Miután felvette a megoldást, az adatokat az infrastruktúra-kiszolgálókról gyűjti és a MOBILE szolgáltatásnak küldött. A MOBILE feldolgozásának általában szolgáltatásnak néhány perc egy óra. Miután a szolgáltatást az adatokat dolgoz fel, megtekintheti a MOBILE.

Egyszerűen eltávolíthatja a megoldást, ha már nincs szükség. Ha eltávolít egy megoldást, az adatok nem MOBILE, amely a napi kvóta által használt adatok mennyiségét csökkentheti, ha több küldi.


## <a name="solutions-supported-by-the-microsoft-monitoring-agent"></a>A Microsoft figyelése Agent által támogatott megoldások

Ekkor a kiszolgálókat, amelyeket a Microsoft figyelése ügynök használatával MOBILE csatlakoztatott a megoldások, beleértve a legtöbb használható:

- Az Active Directory értékelése
- Riasztási Management (nélkül SCOM riasztások)
- Antimalware
- Változások követése
- Biztonsági
- SQL-értékelése
- Rendszer-frissítések

A következő megoldások azonban *nem* használhatók a a Microsoft figyelése Agent és a System Center műveletek Manager (SCOM) ügynök kötelezővé is.

- Riasztási Management (beleértve a SCOM riasztások)
- Kapacitás kezelése
- Konfigurációs értékelése

Olvassa el [Operations Manager kapcsolatot a naplóban Analytics](log-analytics-om-agents.md) olvashat részletesen a SCOM agent napló Analytics.

### <a name="to-add-a-solution-using-the-solutions-gallery"></a>A megoldástár használatával megoldást hozzáadása

1. MOBILE Áttekintés oldalon kattintson a **Megoldástárból** csempére.    
    ![megoldástár](./media/log-analytics-add-solutions/sol-gallery.png)
2. A megoldástár MOBILE lapon című témakörben olvashat minden elérhető megoldást. Kattintson a nevére a MOBILE hozzáadni kívánt megoldás.
3. A lapon az Ön által választott megoldás jelenik meg a megoldást részletes információt. Kattintson a **hozzáadása**gombra.
4. A megoldást, mely részén jelenjen meg az Áttekintés lapon a MOBILE, és használatba vehetik azt a MOBILE szolgáltatás folyamatok az adatok után felvett új csempéje.

## <a name="to-configure-solutions"></a>Megoldások konfigurálása
1. Néhány megoldások kell. Ha például kell automatizálási Azure webhely helyreállítási és biztonsági másolat is használatuk előtt állítsa be.
2. Bármely azok hatókörét kattintson a csempére az Áttekintés oldalon.  
    ![megoldás konfigurálása](./media/log-analytics-add-solutions/configure-additional.png)
3. Ezután a megoldás konfigurálása a szükséges információkat, és kattintson a **Mentés**gombra.  
    ![megoldás konfigurálása](./media/log-analytics-add-solutions/configure.png)

### <a name="to-remove-a-solution-using-the-solutions-gallery"></a>Ha el szeretné távolítani a megoldást használata a megoldástárba

1. MOBILE Áttekintés oldalon kattintson a **Beállítások** csempére.
2. A beállítások lapon az megoldások lapon kattintson az **Eltávolítás** gombra, hogy el szeretné távolítani a megoldáshoz.
3. A megerősítést kérő párbeszédpanelen kattintson az **Igen gombra** kattintva távolítsa el a megoldást.

## <a name="data-collection-details-for-oms-features-and-solutions"></a>A webhelycsoport Adatrészletek MOBILE funkcióit és megoldások

A következő táblázat mutatja az adatgyűjtési módszerek, és hogyan adatgyűjtés MOBILE funkcióit és megoldások más adatait. Közvetlen ügynökök és SCOM ügynökök megegyeznek lényegében, azonban a közvetlen ügynök annak érdekében, hogy a MOBILE munkaterület csatlakozzon, és egy proxyn keresztül irányítása további funkciókat tartalmazza. Ha egy SCOM ügynök használja, MOBILE ügynökeként MOBILE kommunikáció kell célzott. A táblázat SCOM ügynökök MOBILE ügynökök SCOM csatlakozó. Olvashat részletesen a meglévő SCOM környezet MOBILE [Operations Manager csatlakoztatása a napló Analitikájának](log-analytics-om-agents.md) megtekintése

>[AZURE.NOTE] Ügynök használt típusa határozza meg, hogyan adatok együtt küldi el a MOBILE, az alábbi feltételek:

- Használhat a közvetlen agent vagy egy SCOM csatolt MOBILE ügynök.
- SCOM szükség, ha a megoldás SCOM ügynök adatok mindig küldi MOBILE a SCOM kezelése csoport használata. Ezenkívül SCOM szükség, ha csak a SCOM agent használják a megoldást.
- Ha SCOM használata nem kötelező, és a táblázat mutatja, hogy SCOM ügynök adatokat a rendszer elküldi MOBILE használatával, a kezelés csoport, majd SCOM ügynök adatok mindig küldi MOBILE felügyeleti csoportokkal. Közvetlen ügynökök a kezelés csoport átlépése, és küldjön közvetlenül az adataikhoz MOBILE.
- Ha SCOM ügynök adatok nem küldhetők el egy felügyeleti csoportot, majd az adatokat a rendszer elküldi közvetlenül MOBILE – a kezelés csoport kihagyásával.


|adattípus| Platform | Közvetlen Agent | SCOM agent | Azure tárhely | Kötelező SCOM? | E-kezelés csoport elküldött SCOM ügynök adatok | a webhelycsoport gyakorisága |
|---|---|---|---|---|---|---|---|
|Active Directory értékelése|A Windows|![igen](./media/log-analytics-add-solutions/oms-bullet-green.png)|![igen](./media/log-analytics-add-solutions/oms-bullet-green.png)|![nem](./media/log-analytics-add-solutions/oms-bullet-red.png)|![nem](./media/log-analytics-add-solutions/oms-bullet-red.png)|![igen](./media/log-analytics-add-solutions/oms-bullet-green.png)|  7 nap|
|Active Directory-replikáció állapota|A Windows|![igen](./media/log-analytics-add-solutions/oms-bullet-green.png)|![igen](./media/log-analytics-add-solutions/oms-bullet-green.png)|![nem](./media/log-analytics-add-solutions/oms-bullet-red.png)|![nem](./media/log-analytics-add-solutions/oms-bullet-red.png)|![nem](./media/log-analytics-add-solutions/oms-bullet-red.png)|5 nap|
|Értesítések (Nagios)|Linux|![igen](./media/log-analytics-add-solutions/oms-bullet-green.png)|![nem](./media/log-analytics-add-solutions/oms-bullet-red.png)|![nem](./media/log-analytics-add-solutions/oms-bullet-red.png)|![nem](./media/log-analytics-add-solutions/oms-bullet-red.png)|![nem](./media/log-analytics-add-solutions/oms-bullet-red.png)|érkezéskor|
|Értesítések (Zabbix)|Linux|![igen](./media/log-analytics-add-solutions/oms-bullet-green.png)|![nem](./media/log-analytics-add-solutions/oms-bullet-red.png)|![nem](./media/log-analytics-add-solutions/oms-bullet-red.png)|![nem](./media/log-analytics-add-solutions/oms-bullet-red.png)|![nem](./media/log-analytics-add-solutions/oms-bullet-red.png)|1 perc|
|Értesítések (Operations Manager)|A Windows|![nem](./media/log-analytics-add-solutions/oms-bullet-red.png)|![igen](./media/log-analytics-add-solutions/oms-bullet-green.png)|![nem](./media/log-analytics-add-solutions/oms-bullet-red.png)|![igen](./media/log-analytics-add-solutions/oms-bullet-green.png)|![igen](./media/log-analytics-add-solutions/oms-bullet-green.png)|3 perc|
|Antimalware|A Windows|![igen](./media/log-analytics-add-solutions/oms-bullet-green.png)|![igen](./media/log-analytics-add-solutions/oms-bullet-green.png)|![nem](./media/log-analytics-add-solutions/oms-bullet-red.png)|![nem](./media/log-analytics-add-solutions/oms-bullet-red.png)|![igen](./media/log-analytics-add-solutions/oms-bullet-green.png)| óránkénti|
|Kapacitás kezelése|A Windows|![nem](./media/log-analytics-add-solutions/oms-bullet-red.png)|![igen](./media/log-analytics-add-solutions/oms-bullet-green.png)|![nem](./media/log-analytics-add-solutions/oms-bullet-red.png)|![igen](./media/log-analytics-add-solutions/oms-bullet-green.png)|![igen](./media/log-analytics-add-solutions/oms-bullet-green.png)| óránkénti|
|Változások követése|A Windows|![igen](./media/log-analytics-add-solutions/oms-bullet-green.png)|![igen](./media/log-analytics-add-solutions/oms-bullet-green.png)|![nem](./media/log-analytics-add-solutions/oms-bullet-red.png)|![nem](./media/log-analytics-add-solutions/oms-bullet-red.png)|![igen](./media/log-analytics-add-solutions/oms-bullet-green.png)| óránkénti|
|Változások követése|Linux|![igen](./media/log-analytics-add-solutions/oms-bullet-green.png)|![nem](./media/log-analytics-add-solutions/oms-bullet-red.png)|![nem](./media/log-analytics-add-solutions/oms-bullet-red.png)|![nem](./media/log-analytics-add-solutions/oms-bullet-red.png)|![nem](./media/log-analytics-add-solutions/oms-bullet-red.png)|óránkénti|
|Konfigurációs értékelési (régi Advisor)|A Windows|![nem](./media/log-analytics-add-solutions/oms-bullet-red.png)|![igen](./media/log-analytics-add-solutions/oms-bullet-green.png)|![nem](./media/log-analytics-add-solutions/oms-bullet-red.png)|![igen](./media/log-analytics-add-solutions/oms-bullet-green.png)|![igen](./media/log-analytics-add-solutions/oms-bullet-green.png)| kétszer naponként|
|ESEMÉNY-NYOMKÖVETÉS|A Windows|![nem](./media/log-analytics-add-solutions/oms-bullet-red.png)|![nem](./media/log-analytics-add-solutions/oms-bullet-red.png)|![igen](./media/log-analytics-add-solutions/oms-bullet-green.png)|![nem](./media/log-analytics-add-solutions/oms-bullet-red.png)|![nem](./media/log-analytics-add-solutions/oms-bullet-red.png)|5 percig tart|
|IIS naplók|A Windows|![igen](./media/log-analytics-add-solutions/oms-bullet-green.png)|![igen](./media/log-analytics-add-solutions/oms-bullet-green.png)|![igen](./media/log-analytics-add-solutions/oms-bullet-green.png)|![nem](./media/log-analytics-add-solutions/oms-bullet-red.png)|![nem](./media/log-analytics-add-solutions/oms-bullet-red.png)|5 percig tart|
|Kulcs tárolókban|A Windows|![nem](./media/log-analytics-add-solutions/oms-bullet-red.png)|![nem](./media/log-analytics-add-solutions/oms-bullet-red.png)|![igen](./media/log-analytics-add-solutions/oms-bullet-green.png)|![nem](./media/log-analytics-add-solutions/oms-bullet-red.png)|![nem](./media/log-analytics-add-solutions/oms-bullet-red.png)|10 perc|
|Hálózati alkalmazás átjárók|A Windows|![nem](./media/log-analytics-add-solutions/oms-bullet-red.png)|![nem](./media/log-analytics-add-solutions/oms-bullet-red.png)|![igen](./media/log-analytics-add-solutions/oms-bullet-green.png)|![nem](./media/log-analytics-add-solutions/oms-bullet-red.png)|![nem](./media/log-analytics-add-solutions/oms-bullet-red.png)|10 perc|
|Hálózati biztonsági csoportok|A Windows|![nem](./media/log-analytics-add-solutions/oms-bullet-red.png)|![nem](./media/log-analytics-add-solutions/oms-bullet-red.png)|![igen](./media/log-analytics-add-solutions/oms-bullet-green.png)|![nem](./media/log-analytics-add-solutions/oms-bullet-red.png)|![nem](./media/log-analytics-add-solutions/oms-bullet-red.png)|10 perc|
|Az Office 365-ben|A Windows|![nem](./media/log-analytics-add-solutions/oms-bullet-red.png)|![nem](./media/log-analytics-add-solutions/oms-bullet-red.png)|![nem](./media/log-analytics-add-solutions/oms-bullet-red.png)|![nem](./media/log-analytics-add-solutions/oms-bullet-red.png)|![nem](./media/log-analytics-add-solutions/oms-bullet-red.png)|szóló értesítés|
|Teljesítmény számláló|A Windows|![igen](./media/log-analytics-add-solutions/oms-bullet-green.png)|![igen](./media/log-analytics-add-solutions/oms-bullet-green.png)|![nem](./media/log-analytics-add-solutions/oms-bullet-red.png)|![nem](./media/log-analytics-add-solutions/oms-bullet-red.png)|![nem](./media/log-analytics-add-solutions/oms-bullet-red.png)|ütemezett, legalább 10 másodpercig|
|Teljesítmény számláló|Linux|![igen](./media/log-analytics-add-solutions/oms-bullet-green.png)|![nem](./media/log-analytics-add-solutions/oms-bullet-red.png)|![nem](./media/log-analytics-add-solutions/oms-bullet-red.png)|![nem](./media/log-analytics-add-solutions/oms-bullet-red.png)|![nem](./media/log-analytics-add-solutions/oms-bullet-red.png)|ütemezett, legalább 10 másodpercig|
|Szolgáltatás háló|A Windows|![nem](./media/log-analytics-add-solutions/oms-bullet-red.png)|![nem](./media/log-analytics-add-solutions/oms-bullet-red.png)|![igen](./media/log-analytics-add-solutions/oms-bullet-green.png)|![nem](./media/log-analytics-add-solutions/oms-bullet-red.png)|![nem](./media/log-analytics-add-solutions/oms-bullet-red.png)|5 percig tart|
|SQL-értékelése|A Windows|![igen](./media/log-analytics-add-solutions/oms-bullet-green.png)|![igen](./media/log-analytics-add-solutions/oms-bullet-green.png)|![nem](./media/log-analytics-add-solutions/oms-bullet-red.png)|![nem](./media/log-analytics-add-solutions/oms-bullet-red.png)|![igen](./media/log-analytics-add-solutions/oms-bullet-green.png)| 7 nap|
|SurfaceHub|A Windows|![igen](./media/log-analytics-add-solutions/oms-bullet-green.png)|![nem](./media/log-analytics-add-solutions/oms-bullet-red.png)|![nem](./media/log-analytics-add-solutions/oms-bullet-red.png)|![nem](./media/log-analytics-add-solutions/oms-bullet-red.png)|![nem](./media/log-analytics-add-solutions/oms-bullet-red.png)|érkezéskor|
|Syslog|Linux|![igen](./media/log-analytics-add-solutions/oms-bullet-green.png)|![nem](./media/log-analytics-add-solutions/oms-bullet-red.png)|![nem](./media/log-analytics-add-solutions/oms-bullet-red.png)|![nem](./media/log-analytics-add-solutions/oms-bullet-red.png)|![nem](./media/log-analytics-add-solutions/oms-bullet-red.png)|Azure tárhelyről: 10 perc; ügynök: érkezéskor|
|Rendszer-frissítések|A Windows|![igen](./media/log-analytics-add-solutions/oms-bullet-green.png)|![igen](./media/log-analytics-add-solutions/oms-bullet-green.png)|![nem](./media/log-analytics-add-solutions/oms-bullet-red.png)|![nem](./media/log-analytics-add-solutions/oms-bullet-red.png)|![igen](./media/log-analytics-add-solutions/oms-bullet-green.png)| legalább 2 és naponta és 15 percet frissítés telepítése után|
|A Windows biztonsági eseménynaplók|A Windows|![igen](./media/log-analytics-add-solutions/oms-bullet-green.png)|![igen](./media/log-analytics-add-solutions/oms-bullet-green.png)|![igen](./media/log-analytics-add-solutions/oms-bullet-green.png)|![nem](./media/log-analytics-add-solutions/oms-bullet-red.png)|![nem](./media/log-analytics-add-solutions/oms-bullet-red.png)| Azure tárolására: 10 perc; a Agent: érkezéskor|
|A Windows tűzfal naplók|A Windows|![igen](./media/log-analytics-add-solutions/oms-bullet-green.png)|![igen](./media/log-analytics-add-solutions/oms-bullet-green.png)|![nem](./media/log-analytics-add-solutions/oms-bullet-red.png)|![nem](./media/log-analytics-add-solutions/oms-bullet-red.png)|![nem](./media/log-analytics-add-solutions/oms-bullet-red.png)| érkezéskor|
|Windows-eseménynaplók|A Windows|![igen](./media/log-analytics-add-solutions/oms-bullet-green.png)|![igen](./media/log-analytics-add-solutions/oms-bullet-green.png)|![igen](./media/log-analytics-add-solutions/oms-bullet-green.png)|![nem](./media/log-analytics-add-solutions/oms-bullet-red.png)|![igen](./media/log-analytics-add-solutions/oms-bullet-green.png)| Azure tárolására: 1 perc; a Agent: érkezéskor|
|Átutalásra vonatkozó adatok|A Windows (2012 R2 / 8.1-es vagy újabb verzió)|![igen](./media/log-analytics-add-solutions/oms-bullet-green.png)|![igen](./media/log-analytics-add-solutions/oms-bullet-green.png)|![nem](./media/log-analytics-add-solutions/oms-bullet-red.png)|![nem](./media/log-analytics-add-solutions/oms-bullet-red.png)|![nem](./media/log-analytics-add-solutions/oms-bullet-red.png)| 1 percenként|

## <a name="log-analytics-preview-solutions-and-features"></a>Jelentkezzen be a Analitikájának megtekintése megoldások és szolgáltatások

Azt a szolgáltatást futtató és devops eljárásokat követve is tudják az ügyfelek a szolgáltatásokat és a megoldások fejlesztésére partner.

Magánjellegű villámnézetben azt átadhatja az ügyfelek access kisebb csoport a korai végrehajtásának az funkcióval vagy a nyereség visszajelzési és javítása megoldás. A korai példányhoz minimális funkcióit és műveleti képességeit.

Célunk gyorsan próbálkozzon a dolgokat, hogy mi a dolgozik, és mi nem működik megtalálhassuk. Ez a folyamat mindaddig, amíg a visszajelzés a személyes preview ügyfeleinek az tájékoztatja velünk, hogy, hogy készen áll a nyilvános előnézetét azt Végigléptetés.

A nyilvános villámnézetben VÁLLALUNK az funkcióval vagy a megoldást érhető el az összes felhasználói további visszajelzés kérése, és ellenőrizze, hogy a méretezés és hatékonyságát. Ebben a fázisban:

- Előzetes verzió szolgáltatások jelennek meg a beállítások lapon, és bármelyik felhasználó engedélyezése
- A gyűjtemény segítségével előzetes megoldások is hozzáadhat vagy közzétett parancsfájl használatával

### <a name="what-should-i-know-about-preview-features-and-solutions"></a>Mit kell még figyelembe venni előzetes funkciókról és a megoldások?

Új szolgáltatásairól érdeklik, hogy, és a megoldások és közkedvelt fejlesztését őket használata.

Előzetes verzió szolgáltatásai és megoldások nem mindenki számára megfelelő azonban, mielőtt kérő be szeretne kapcsolódni egy privát előzetes verzió és a nyilvános előnézetét engedélyezése győződjön meg arról, OK dolgozik, amit fejlesztés alatt áll.

Ha engedélyezi a megtekintése funkció révén a portálon lesz figyelmeztetést emlékezteti, hogy a funkció előzetes verzióban.

#### <a name="for-both-private-and-public-preview"></a>A *személyes* és *nyilvános* előzetes verzió

Nyilvános és magán előnézeteket a következő vonatkozik:

- Dolog, amit nem mindig működnek helyesen.
  - Problémák tartomány kisebb zajterhelést keresztül szeretne valamit, amit nem működik a egyáltalán nem.
- Nincs negatív hatással van a rendszeren előzetes potenciális / környezet
  - Azt lehetőleg ne negatív dolog, amit történik a rendszer használata esetén MOBILE, de néha váratlan dolog történik
- Az adatok elvesztését / sérült fordulhat.
- Előfordulhat, hogy kérjük, hogy a diagnosztikai naplók vagy egyéb problémák megoldására adatok összegyűjtése
- A szolgáltatás vagy megoldás el lehet távolítani (ideiglenes vagy végleges)
  - Azt nem engedje fel az funkcióval vagy a megoldást dönthet a villámnézetben a learnings alapján
- Előnézetek használatakor nem menthetők, és előfordulhat, hogy nem teszteltük az összes konfigurációja, és azt korlátozhatják:
  - Az operációs rendszerek használható (például egy funkció előfordulhat, hogy csak alkalmazása Linux az előzetes verzió)
  - Milyen típusú ügynök (MMA, SCOM) használható (például egy funkció nem működnek a SCOM az előzetes verzió)  
- Előzetes megoldások és szolgáltatások nem vonatkozik a szint szolgáltatási szerződés
- Előzetes verzió szolgáltatások használatát merülnek fel, amelyekre használati költségek
- Szolgáltatások vagy funkciók, hogy a szolgáltatás van szüksége, és a megoldás lehet hasznos lehet a hiányzó vagy hiányos
- Funkciók és megoldások nem feltétlenül vehető igénybe az összes terület
- Funkciók és megoldások nincsenek honosítva
- Funkciók és megoldások lehetnek korlátozott számú ügyfelei vagy használható eszközök
- Előfordulhat, hogy konfigurálása és a megoldás szolgáltatást engedélyezni szeretné a parancsfájlok használata
- A felhasználói felület () hiányos lesz, és a napi változhat
- Nyilvános előnézetek nem lehet a gyártási megfelelő / kritikus rendszerek

#### <a name="for-private-preview"></a>*A magánjellegű* előzetes verzió

A fenti mellett az alábbiakra személyes előnézetek jellemző:

- Megakadályozási, hogy eljuttatjuk kérését a szolgáltatás megoldást jobb küldje el nekünk visszajelzést a felhasználói felület
- Azt kapcsolatba léphet a felmérést, telefonhívások vagy e-mail visszajelzést
- Mi hol mindig nem működik megfelelően
- Azt egy nem-közzététel szerződés (titoktartási) megkövetelheti részvételért vagy bizalmas tartalmak tartalmazhatnak
  - Blogírás, tweeting vagy harmadik fél egyébként kommunikáció előtt ellenőrizze a Program felelős vezető az előnézet megértéséhez közzétételi korlátozások
- Nem működnek a termelési / kritikus rendszerek


### <a name="how-do-i-get-access-to-private-preview-features-and-solutions"></a>Hogyan szerezhető be személyes előzetes verzió szolgáltatásai és megoldások való hozzáférést?

Az ügyfelek számára keresztül többféle módszert attól függően, hogy a kép a magánjellegű előnézetek meghívása azt.

- Válaszoljon a havi ügyfél felmérés, és e-mailben elintézésre Önnel engedély megadása us javítja a meghívott magánjellegű előnézetét a csomagváltás.
- A Microsoft-fiók csoport is hozzászólásokra meg.
- Jelentkezzen a részletek, a twitteren [msopsmgmt](https://twitter.com/msopsmgmt) közzétett alapján
- Jelentkezzen a részletek megosztott közösségi események – alapján tekintse meg az Amerikai értekezlet ups, konferenciák és az online Közösségek.


## <a name="next-steps"></a>Következő lépések

- [Keresés naplók](log-analytics-log-searches.md) megoldások által gyűjtött részletes adatok megjelenítése.
