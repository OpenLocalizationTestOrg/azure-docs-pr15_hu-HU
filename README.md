# <a name="azure-technical-documentation-contributor-guide"></a>Azure műszaki dokumentáció közreműködői útmutató

Megtalálta a GitHub tárházba, amely a forrás, a műszaki dokumentáció [http://azure.microsoft.com/documentation](http://azure.microsoft.com/documentation)Azure dokumentáció középen közzétett tárolja.

Ez a tár is tartalmaz útmutatást segítséget szerkeszthetik azokat a műszaki dokumentációt.  A munkatársak útmutatóban a cikkek listájáért lásd: [az index](https://github.com/Azure/azure-content/blob/master/contributor-guide/contributor-guide-index.md).

## <a name="contribute-to-azure-documentation"></a>Azure dokumentációt közreműködés

Köszönjük az érdeklődését Azure dokumentációjában!

* [Közreműködés módjai](#ways-to-contribute)
* [Kódját lebonyolítása](#code-of-conduct)
* [Tudnivalók az adományok Azure tartalom](#about-your-contributions-to-azure-content)
* [A szervezet tárházba](#repository-organization)
* [GitHub, mely számjegy és a tárházba](#use-github-git-and-this-repository)
* [Hogyan formázhatja a témakör markdown segítségével](#how-to-use-markdown-to-format-your-topic)
* [Visszajelzés, a megjegyzések és támogatás](./contributor-guide/feedback-and-comments.md)
* [További források](#more-resources)
* [Az összes munkatársak útmutató cikkek indexe](./contributor-guide/contributor-guide-index.md) (az új lap nyílik meg)

## <a name="ways-to-contribute"></a>Közreműködés módjai 

[Azure dokumentáció](http://azure.microsoft.com/documentation/) többféleképpen hozzájárulhat:

* [Fórum vitafórum](http://social.msdn.microsoft.com/Forums/windowsazure/home)járulni.
* Cikkek alján Disqus megjegyzéseket küldhetnek.
* Egyszerűen hozzájárulhat a GitHub felhasználói felület technikai cikkeket. A cikkben található ez a tár vagy látogasson el a cikk [http://azure.microsoft.com/documentation](http://azure.microsoft.com/documentation) , és kattintson a hivatkozásra, amely a cikk a GitHub forrás ugrik című témakörben.
* Jelentős módosításokat szeretne egy meglévő cikk készít, ha hozzáadása vagy módosítása a képek vagy járult hozzá az új hozzászólás, akkor a tárházba ágaznak, telepítenie kell mely számjegy Bash, Markdown parancsot, és ismerje meg, bizonyos mely számjegy parancsok.

##<a name="code-of-conduct"></a>Kódját lebonyolítása

Ehhez a projekthez elfogadta a [Microsoft megnyitott forrás viselkedési](https://opensource.microsoft.com/codeofconduct/). Lásd: további információt a [Gyakori lebonyolítása kódot](https://opensource.microsoft.com/codeofconduct/faq/) vagy kapcsolattartó [opencode@microsoft.com](mailto:opencode@microsoft.com) bármilyen további kérdéseiket vagy megjegyzéseiket az.

##<a name="about-your-contributions-to-azure-content"></a>Tudnivalók az adományok Azure tartalom

###<a name="minor-corrections"></a>Kisebb javítások

Az [Azure webhely használati (mezőket)](http://azure.microsoft.com/support/legal/website-terms-of-use/)kisebb javításainak vagy elküldése e repó a dokumentáció és kód példák magyarázata elfednek.


###<a name="larger-submissions"></a>Nagyobb beküldött elemek

Ha Ön kérelem küldése ki az új vagy jelentős módosításokat dokumentáció és kód példák, rendszerünk egy e-megjegyzés GitHub a szólít fel egy online hozzájárulása licenc szerződés (CLA) elküldése, ha ezeket a csoportjai egyikében:

* Nyissa meg a Microsoft-technológiák csoportjának tagjai.
* Munkatársak, akik a Microsoft nem működnek.

Végre kell hajtania az online űrlapot, mielőtt azt is fogadja el a leküldéses kérelem szükséges.

Minden részlet a [http://azure.github.io/guidelines/#cla](http://azure.github.io/guidelines/#cla)érhetők el.

## <a name="repository-organization"></a>A szervezet tárházba

A tartalom az azure-tartalom adattárban dokumentáció a szervezet [Azure.Microsoft.com](http://azure.microsoft.com)következik. Ez a tár két legfelső szintű mappa tartalmazza:

### <a name="articles"></a>\articles

A *\articles* mappát a dokumentáció cikkek *.md* kiterjesztésű markdown fájlként formázott tartalmazza.

Cikkek a legfelső szintű címtárban közzétett Azure.Microsoft.com az elérési út *http://azure.microsoft.com/documentation/articles/ {cikk-neve-nélkül – md} /*.

* **Fájlnevek cikket:** Lásd: [a fájlelnevezési útmutatást](./contributor-guide/file-names-and-locations.md).

A saját szolgáltatás mappába cikkek közzétett Azure.Microsoft.com az elérési út *http://azure.microsoft.com/documentation/articles/service-folder/ {cikk-neve-nélkül – md} /*

* **Media almappák:** A *\articles* mappában található a legfelső szintű címtár cikk médiafájlok *\media* mappa belül, amely az egyes cikk képekkel almappái.  A szolgáltatás mappák tartalmaz egy külön media mappát a cikkek minden szolgáltatás mappán belül. A cikk kép mappákat a cikk fájlt *.md* kiterjesztéssel mínusz azonos neve.

### <a name="includes"></a>\includes

Újból felhasználható tartalom szakaszok bekerülnek az egy vagy több cikkek hozhat létre. [A műszaki tartalom használt egyéni bővítmények](./contributor-guide/custom-markdown-extensions.md)megtekintése

### <a name="markdown-templates"></a>\markdown sablonok

Ez a mappa formázással egyszerű markdown cikk van szüksége a szabványos markdown sablont tartalmaz.

### <a name="contributor-guide"></a>\contributor-Guide

Ez a mappa a munkatársak útmutató részét képező cikkek tartalmazza.  

## <a name="use-github-git-and-this-repository"></a>GitHub, mely számjegy és a tárházba

Hogyan segíthet a GitHub felhasználói felületének használata szeretné küldeni a módosításokat a kis és ágaznak és a fontosabb adományok tárháza klónozhatja kapcsolatos tudnivalókért lásd: [telepítése és beállítása a GitHub létrehozására szolgáló eszközök](./contributor-guide/tools-and-setup.md).

Ha telepíti a GitBash, és válassza a helyi számítógépen, a lépéseket, egy új helyi munkaidő-fiók létrehozása, módosítása és elküldése a módosításokat a fő ág vissza jelennek meg [egy új témakör létrehozása vagy egy meglévő cikk frissítése mely számjegy-parancsok](./contributor-guide/git-commands-for-master.md)

### <a name="branches"></a>Ágak

Azt javasoljuk, hogy hozza létre a helyi munka ágak városrész egy adott hatókörének módosítása. Ágak egy egyetlen fogalma és a cikkben is munkafolyamat egyszerűsítése és az esélye egyesítés ütközések, korlátozott kell lennie.  A következő erőfeszítéseket a megfelelő hatókör egy új ág a következők:

* Egy új referenciacikkünket (és a kapcsolódó képeket)
* Egy cikk helyesírás- és nyelvhelyesség-ellenőrzés módosításokat.
* Alkalmazza a formázási egyetlen módosítás nagyszámú cikkek (pl. új szerzői jogi élőláb).

## <a name="how-to-use-markdown-to-format-your-topic"></a>Hogyan formázhatja a témakör markdown segítségével

Ez a tár összes cikkeket GitHub flavored markdown használja.  Az alábbiakban az erőforrások listáját.

- [Markdown alapjai](https://help.github.com/articles/markdown-basics/)

- [Nyomtatható markdown cheatsheet](./contributor-guide/media/documents/markdown-cheatsheet.pdf?raw=true)

- A markdown szerkesztők listáját olvassa el a az [Eszközök és a telepítő témakör](./contributor-guide/tools-and-setup.md#install-a-markdown-editor)című témakört.

## <a name="article-metadata"></a>A cikk metaadatok

Cikk metaadatok lehetővé teszi, hogy az egyes funkciók a azure.microsoft.com webhelyen, például alapon Szerző, közreműködői alapon, tetejéhez, cikk leírásai és keresőmotor-Optimalizálási optimalizálásokat is jelentéskészítési Microsoft használja tartalom teljesítményét ki szeretné számítani. A metaadatok, fontos! [Az alábbiakban az útmutató módon bizonyosodhat meg arról, hogy a metaadat-alapú jobbra befejeződött](./contributor-guide/article-metadata.md).

### <a name="labels"></a>Címkék

Automatikus címkék kérések segítse a szolgáltatás kezelése a leküldéses kérelem munkafolyamat és megoldása a lehetővé teszik, hogy mi legyen a történésekkel kapcsolatban a leküldéses kérésével csoportosítani oszthatók:

* Kapcsolódó hozzájárulása licencszerződése
    * CLA nem kötelező: A változás viszonylag kisebb, és nem szükséges, hogy bejelentkezik egy CLA.
    * CLA kötelező: hatókörének módosítása viszonylag nagy és igényel, hogy bejelentkezik egy CLA.
    * aláírt cla: A közös munka aláírt a CLA, hogy a leküldéses kérelem is most előrelépés véleményezésre.
* Címkék pillér: például PnP címkék, Modern alkalmazás elemet, és TDC súgó kategorizálása a leküldéses kérelmek a belső, olvassa el a leküldéses kérelem kell, hogy a szervezet által.
* Tartalom-előállítás küldött módosítása: A Szerző van értesült a függőben lévő ki kérelmet.

## <a name="more-resources"></a>További források

Lásd: az [index útmutatójának a közös munka](./contributor-guide/contributor-guide-index.md) az útmutató témakörök tartalmaznak.
