# <a name="quality-criteria-for-pull-request-review"></a>Tekintse át a minőség feltétel ki kérés

Ezek a feltétel szolgálnak, létrehozása és kezelése a technikai cikkek szerzőkkel és ki kérelem véleményezők ki tekintse át a tartalom ki kérések. Ha a leküldéses kérelem nem felel meg [az automatikus egyesítése](contributor-guide-pull-request-etiquette.md#in-a-hurry-submit-prs-that-can-be-accepted-automatically), felülvizsgálatra kerül egy emberi ki kérelem véleményező. Húzza a véleményezők általában áttekintése csak mi az új vagy módosított kérelmet. Húzza a kérelem véleményezőinek neve értékeli ki összehívásban megfelelően a blokkolási és nem blokkolja minőségű Véleményezés cikkek a jelen cikkben felsorolt a módosításokat.

## <a name="blocking-content-quality-items"></a>Tartalom minőségű cikkek letiltása

A frissítéseket a leküldéses kérelem meg kell felelnie a következő feltétellel egyesítendő. Leküldéses kérelem véleményezők visszajelzés küldése a leküldéses kérelem megjegyzések ezen elemek, és írja be `#hold-off` a leküldéses kérésben való visszatéréshez Önnek (a szerző) visszajelzést.

| Kategória | Hangminőség Véleményezés elemet |
|----------|---------------------|
|Előfeltételek| A "kész-a-egyesítés" és a "érvényesítése sikerült" címkéket a PR. van rendelve|
|Előfeltételek| Egyesítés ütközés nem blokkolja a leküldéses kérelmet.|
|Repó integritás|    Leküldéses kérelem nem egyértelmű tartalom hátrányait tartalmazza.|
|Repó integritás|    Leküldéses összehívás nem tartalmaz egy beágyazott repó vagy szokatlan, fölösleges fájlok.|
|Repó integritás |Leküldéses kérelem 100-nál kevesebb módosított fájlokat is tartalmaz, kivéve, ha az ár szándékosan frissíti a Megjelenés ág mesteralakzatból. (Valójában, egy ár tartalmazhat távol annál kevesebb, mint amit azonban 100 megváltozott fájlok, miután GitHub nem jelenik meg a diffs).|
|Elnevezése |Új fájlt a fájlnév hajtsa végre a [fájl elnevezési irányelveket](file-names-and-locations.md).|
|Elnevezése |A repó kövesse a [mappa elnevezési irányelvek](file-names-and-locations.md#folder-names-in-the-repo)területére új mappákat.|
|Tartalom    |A témakör az technikai dokumentumot, és ezért a megfelelő tartalom csatornájában. Lásd: a [Mi kerül, ahol útmutatást](content-channel-guidance.md).|
|Tartalom    |A műszaki dokumentumban tárgyára technikai a cikk megfelelő. Lásd: a [Mi kerül, ahol útmutatást](content-channel-guidance.md).|
|Tartalom    |A cikk tartalmazza az egy bevezető szintű bekezdésre, és a tartalom lépésről vagy elvi törzsében. A cikk kell állni saját egy cikk, elegendő, a teljes tartalmat. Nem kell egy kis kódrészletet információkat. (Kivétel: A "Korlátozások" tematikus esetén egy nagy cikk, amely tartalmazza az összes szolgáltatás határain környezetében.)|
|Tartalom| Elemeket, amelyek kell lennie a számozott listák számozásának, elemeket, amelyek kell lennie rendezés nélkül listák listajeles. Ez az egyszerű használhatósági.|
|Tartalom| Szokatlan vagy új grafikus elemekkel, az információs architektúra vagy a struktúrák vagy a nyilvánvalóan szabványos látványtervek kell kell vetted és az átvezetési ár véleményező monogramjára cseréli. Csoportok alkotják vannak kísérletezzen új dolgot kell egy probléma/megoldás vászonra vagy terv kísérletek értékeléséhez helyen.|
|Funkciók Webhelytervezés| Switchers csak a Váltás több verziója ugyanazon cikk keresztül használják.|
|Funkciók Webhelytervezés| A címek switchered cikkekben switchered megadása a további cikkek a különbséget tesz a minden cikk információkat tartalmaznak.|
|Funkciók Webhelytervezés| Kézzel létrehozott tartalomjegyzék nem engedélyezett. A cikk a saját oldalon tartalomjegyzék H2s kell támaszkodhat.|
|Funkciók Webhelytervezés| Ha H2 címsorok, a cikkben legalább két H2 címsorok. Egy elemű cikk TOC egy H2 címsor használatával hoz létre. H2 címsorok annak érdekében, hogy a tartalomjegyzék létrehozása előtt H3 címsorok kell használni.|
|Markdown| HTML-: Tartalmat a forrás HTML szintjén blokkolása HTML engedélyezett (permitted) – például a felső és alsó, speciális karakterek és markdown nem kisebb egyebek kisebb beágyazott nem tartalmaz. A HTML tábla engedélyezett csak akkor, ha a tábla tartalmazza a listajeles és számozott listák, de ez általában a tartalom kell kell egyszerűsített, így a forrás van kódolva, a markdown jelzése.|
|Markdown   |Egyéni markdown elemek szolgálnak, ahol csak lehetséges. Ex: Jegyzetek kódolt vannak az AZURE használatával. Megjegyzés: a bővítmény nem egyszerű szövegként.|
|KERESŐMOTOR-OPTIMALIZÁLÁSI    |A "& #124; Microsoft Azure"webhely azonosító szükség|
|KERESŐMOTOR-OPTIMALIZÁLÁSI    |A H1 cím elegendő információt tartalmaz ahhoz, hogy elkülönüljön a többi Azure cikkek és valószínűleg ügyfél kulcsszavak hozzárendelése a cikk tartalma. Ha például a "Áttekintése" H1 címeként egy fail: az.|
|Kifejezések| ARM betűszó, V1 vagy V2 klasszikus és erőforrás-kezelő telepítési modellek hivatkozásként használata egy blokkoló terminológia elemre.|
|Képek |Animált GIF nem fogad a repó be.|
|Képek | Képek törlése felbontása, ingyenes hibásan írt szavakat, és nincs személyes adatokat tartalmaznak | 
|Előkészítése|A cikk előzetes verzióban a fejlesztői TISZTÍT kell lennie. Nem tartalmazhat egyértelmű formázási problémák megoldásához: <br><br>-A bekezdés megjelenő számozott és listajeles lista <br> -Részben kód Tiltás és részben kívüli kapott kód blokkjában kód <br>-Hibás behúzás miatt nem megfelelően számozva számozott lépések|

## <a name="non-blocking-content-quality-items"></a>Nem blokkolja minőségű cikkek tartalom

Ezeket az elemeket ki kérelem véleményezők visszajelzési és utasításait egy újabb ki összehívásba javítások nyomon követésére a szerző adja meg. A visszajelzést azonban nem blokkolja a döntési egyesíteni szeretne. Szerzők a javítások 3 üzleti napon belül kell lett beállítva.

| Kategória | Hangminőség Véleményezés elemet |
|----------|---------------------|
|Tartalom|Cikkek az 1-3-as releváns és látványos további lépések végén "következő lépések" kell tartalmaznia. Rövid szöveg szerepeljenek, amely segít a vevő megértéséhez, hogy miért fontosak a következő lépésekkel. (Új cikkek csak). Példa: <https://azure.microsoft.com/en-us/documentation/articles/xplat-cli-install/><br>![](media/contributor-guide-pr-criteria/nextstepsexample.PNG)|
|Tartalom|Helyesírás, nyelvhelyesség-ellenőrzés és egyéb írási problémák - ki kérelem véleményezők előfordulhat, hogy visszajelzést az néhány kisebb problémák, nem blokkoló visszajelzést. Több választási Szerkesztői problémák lépnek fel, ha a véleményezők jelentkezzen a cikk a utáni kiadvány módosítás Szerkesztés felkérés.|
|Képek|Képek használata a megfelelő felirat stílusát és színét, és képernyőképek használja a megfelelő szegély és a helyőrző stílusát. [A kép útmutatást talál](https://github.com/Azure/azure-content/blob/master/contributor-guide/create-images-markdown.md).|
|Képek|Képek helyettesítő szöveget tartalmazza. [A kép útmutatást talál](https://github.com/Azure/azure-content/blob/master/contributor-guide/create-images-markdown.md).|
|Funkciók Webhelytervezés|Legfeljebb 2 sorok ideális sortörés H2 fejléceket, amikor az oldalon, a tartalomjegyzék való megjelenítését. Hosszabb a címsorok segítenek a TOC olvashatók nehezebb szeretne képet beolvasni a.|
|Stílus szabályok|Címek és a fejlécek olyan Mondatkezdő, egy Azure stílust.|
|Folyamatábra|A leküldéses kérelem sikerült van egyszerűen lett újrakonfigurálni PRmerger automatizálási összekapcsolhatók, ha ki kérelem véleményezők visszajelzést a szerző ágak használatáról, így a módosításokat a sikerült automatikusan egyesíteni szeretne. [Az ár etikett](https://github.com/Azure/azure-content/blob/master/contributor-guide/contributor-guide-pull-request-etiquette.md#in-a-hurry-submit-prs-that-can-be-accepted-automatically)témakört.|
|Folyamatábra|Törlése és átnevezése egy cikk, győződjön meg arról, hogy a godaddytől. Húzza a véleményezők kell adja hozzá a következő megjegyzés és hivatkozás egy megjegyzésben kérelmet:<br><br>*Ellenőrizze a cikkek törlésére szolgáló követte a munkatársak útmutatóban a folyamat: <https://github.com/Azure/azure-content/blob/master/contributor-guide/retire-or-rename-an-article.md> .*|

## <a name="related"></a>Kapcsolódó

- [Leküldéses kérelem etikett és a gyakorlati tanácsok Microsoft munkatársak](contributor-guide-pull-request-etiquette.md)

- [Húzza a kérelem Megjegyzés automatizálási](contributor-guide-pull-request-comments.md)
