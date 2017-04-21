<properties
    title="required"
    pageTitle="A műszaki cikkekben használt egyéni markdown bővítmények"
    description="Egyéni markdown bővítmények engedélyezése, beágyazott videók, jegyzetek és tippek, újból felhasználható tartalom és egyéb elem azure.microsoft.com technikai cikkekben listája."
    services=""
    solutions=""
    documentationCenter=""
    authors="tysonn"
    manager="carolz"
    editor=""/>

<tags
    ms.service="contributor-guide"
    ms.devlang=""
    ms.topic="article"
    ms.tgt_pltfrm=""
    ms.workload=""
    ms.date="01/22/2015"
    ms.author="tysonn"/>

## <a name="markdown-for-azuremicrosoftcom"></a>A Azure.microsoft.com markdown

Általános markdown, című témakör ír részletesen [Markdown alapjai](https://help.github.com/articles/markdown-basics/) és a [markdown cheatsheet](./media/documents/markdown-cheatsheet.pdf?raw=true). Ha szüksége cikk crosslinks markdown, olvassa el a [össze útmutatás] (. / create-links-markdown.md#markdown-syntax-for-acom-relative-links.md/).

Azure.microsoft.com [kapcsolódó kód blokkok](https://help.github.com/articles/github-flavored-markdown/#fenced-code-blocks) , és a [Kiemelés szintaxis](https://help.github.com/articles/github-flavored-markdown/#syntax-highlighting)támogatja. Azonban ACOM támogatja a kiemelés színsémát, függetlenül a megadott nyelven kód blokkjában csak egy szintaxis.

## <a name="custom-markdown-extensions-used-in-our-technical-articles"></a>A műszaki cikkekben használt egyéni markdown bővítmények

A cikkek GitHub flavored markdown használata a legtöbb cikk formázáshoz - bekezdések, hivatkozások, listákat, a címsorok stb. De egyéni markdown bővítmények használjuk, ahol szükséges sokoldalúbb formázás azure.microsoft.com leképezett lapjain. Az alábbiakban a bővítményeket, hogy jelenleg használ:

+ [Megjegyzések és tippek]
+ [Tartalmaz]
+ [Beágyazott videók]
+ [Technológia és a platform választók]

## <a name="notes-and-tips"></a>Megjegyzések és tippek

4 típusa a megjegyzések és tippek közül választhat:

- AZURE. MEGJEGYZÉS:
- AZURE. FIGYELMEZTETÉS
- AZURE. TIPss
- AZURE. FONTOS

###<a name="usage"></a>Használat
Általánosságban elmondható használja a jegyzetek és a tippek a cikkek egész csak ritkán használjunk. Ha használja őket, válassza ki a megfelelő jegyzet vagy tipp típusú:

- AZURE használja. Megjegyzés: Jelölje ki a semleges vagy pozitív hangsúlyozza vagy adatok egészíti ki a szövegtörzs főbb pontjairól. Jegyzet látja el, amely csak bizonyos esetekben alkalmazzák.

  ![](./media/custom-markdown-extensions/Notes-note.PNG)

- AZURE használja. Figyelmeztetés: a felhasználó a felhasználót, hogy egy feltétel, előfordulhat, hogy a jövőben okozó probléma. Például egy bizonyos lehetőséget választva, vagy egy bizonyos választási kezdeményezhet előfordulhat, hogy véglegesen zárolása, egy adott példa.

  ![](./media/custom-markdown-extensions/Notes-warning.PNG)

- AZURE használja. Tipp: segítheti a felhasználókat, és a saját igényeinek szöveg ismertetett eljárások alkalmazni. Előfordulhat, hogy ezt a tippet is ajánlja fel, amely esetleg nem egyértelmű alternatív módszereket. Tippek, azonban nem lényegesek a szöveg egyszerű megértése érdekében.

  ![](./media/custom-markdown-extensions/Notes-tip.PNG)

- AZURE használja. FONTOS adatokat, akkor lényeges, hogy egy tevékenység befejezését.

  ![](./media/custom-markdown-extensions/Notes-important.PNG)

Miközben a jegyzetek és a tippek támogatja a kód megakadályozza, képek, a listák és a hivatkozásokat, próbálja meg a megőrzése a jegyzetek és a tippek, egyszerű és egyértelmű. Ha saját maga összetett feljegyzések létrehozása formázás rengeteg talál, amelyek valószínűleg egy bejelentkezési, akkor csak meg kell egy másik szakasz fő szövegét a cikk a. És egy cikket a túl sok feljegyzések zavaró és nehezen beolvasás, vagy ismerje meg lehet.

###<a name="sample-markdown"></a>Minta markdown

Az összes minták megjelenítése az AZURE. MEGJEGYZÉS:. Tipp:, a figyelmeztetés vagy a fontos használatához cserélje le a "Megjegyzés" a markdown:

    > [AZURE.TIP]

    > [AZURE.WARNING]

    > [AZURE.IMPORTANT]

Ha egyetlen bekezdésből:

    > [AZURE.NOTE] Ebben az oktatóanyagban befejezéséhez az az aktív Microsoft Azure-fiókkal kell rendelkezniük. Nem rendelkeznek fiókkal, ha mindössze néhány perc is létrehozhat ingyenes próba-fiók.

Multiparagraph:

    > [AZURE.NOTE] Ebben az oktatóanyagban befejezéséhez az az aktív Microsoft Azure-fiókkal kell rendelkezniük.
    >
    > Ha nem rendelkeznek fiókkal, is [ingyenes próba-fiók létrehozása](http://www.windowsazure.com/pricing/free-trial/) az mindössze néhány perc.

## <a name="includes"></a>Tartalmaz

A GitHub adattárban újrafelhasználható szöveg található fájlokban, hogy hívása "tartalmazza a". Ha több cikkekben használni kívánt szöveget, meg kell adnia az újból felhasználható információ fájlra mutató hivatkozást. A felvétel magát egy olyan egyszerű markdown (.md)-fájl. Bármely érvényes markdown, beleértve a szöveget, hivatkozásokat és képeket is tartalmazhat. Az összes olyan markdown fájlok kell lennie [a / tartalmaz címtárat](https://github.com/Azure/azure-content/tree/master/includes) a legfelső szintű a tárat. A cikk közzétételekor felvétele szöveg zökkenőmentes integrálni a közzétett témakört.

- Egy adott szintaxis segítségével egy Belefoglalás hivatkozást.

- Médiafájlok helyezi el egy belefoglalása létre kell hozni media mappában a felvétel jellemző. Médiafájlok mappákat is tartalmaz [az azure-tartalom/tartalmaz/media](https://github.com/Azure/azure-content/tree/master/includes/media)mappában tartoznak. A médiafájlok könyvtár nem tartalmazhat a legfelső szintű képet. Ha a felvétel nincs képeket, majd a megfelelő media könyvtárat nincs szükség.

###<a name="usage"></a>Használat

- Használata bárhol szükséges jelenjen meg több cikkekben ugyanazt az szöveget tartalmazza.

- Is van kialakítva, hogy jelentős mennyiségű tartalom – bekezdés vagy két, egy megosztott eljárást, vagy egy megosztott szakasz használható. Ne használja őket az összes kisebb, mint egy mondatot; a **termék neve nem kapcsolódnak**.

- Győződjön meg róla, mind a szöveg egy belefoglalása írott teljes mondatot vagy egy kifejezést, amely nem függő előző szöveget vagy a következő szöveggel, amely a felvétel hivatkozik című témakörben. Figyelmen kívül hagyása az útmutató hoz létre egy untranslatable karakterlánc, amely megszünteti a honosított felület című témakörben. 

- Nem beágyazása másik belül is tartalmaz. Azok a rendszer közzétételi diagnosztikai házirend-szolgáltatás által nem támogatott.

- Multimédia-fájlok közötti nem oszthat meg. Egy külön fájlba használata egy egyedi nevet az egyes felvétele és a cikkben. A multimédiás fájl tárolását a felvétel társított media mappájában.

- Egy felvétel nem használja az csak egy cikk tartalma.  Is van kialakítva, hogy egészíti ki a többi a cikk a tartalmat.

- Mivel minden ugyanerre kell lennie a / magában foglalja, címtár-felvétele egy cikket a elérési útja mindig

    .. / tartalmaz

- A cikk és a felvétel hivatkozás vagy kép filename hivatkozást a ne ismétlődjenek. Hozzáadása "-tartalmazzák" hivatkozás media vagy hivatkozás fájlnév elkerülése a hivatkozás ismétlődő:

 **A hivatkozás**

 Módosítása: A odata.org: odata.org tartalmazza

 **Képhivatkozás**

 Módosítsa a table.png való: táblázat-include.png

###<a name="sample-markdown"></a>Minta markdown
Egy felvétel hozzáadása egy dokumentáció cikk szintaxisa:

    [AZURE.INCLUDE [include-short-name](../includes/include-file-name.md)]

Példa

    [AZURE.INCLUDE [howto-blob-storage](../includes/howto-blob-storage.md)]

A felvétel első része a Belefoglalás nevét, az elérési utat és a .md kiterjesztés nélkül. A második rész a felvétel a relatív elérési útját a / címtár .md kiterjesztésű tartalmazza.

###<a name="rendering"></a>A megjeleníthető

A leképezett GitHub lapon a felvétel fog jeleníti meg az alábbi képlettel történik:

 [AZURE. Útmutató blobtárolóhoz olyan]

A azure.microsoft.com, a HTML-kódot a leképezett HTML-a tartalmaz a dokumentum HTML a többi egyesíteni van. Azonban a HTML-kód fogják tartalmazni a HTML megjegyzés, melyhez az eredeti markdown fájlnév- és a GitHub jóváhagyás kivonat tartalmazza. A megjegyzés megtalálható hibaelhárítási célból, hogy a forrás tartalom egyszerűen tudja azonosítani és GitHub található:

  ![](./media/custom-markdown-extensions/include.png)


## <a name="embedded-videos"></a>Beágyazott videók

A műszaki cikkek embeddeded videók támogatja a technikai cikkek mindaddig, amíg a videók van a Microsoft- [csatorna 9](http://channel9.msdn.com/) webhelyén. A videókat a csatornában 9 kell integrálni [a videó központ azure.microsoft.com](http://azure.microsoft.com/documentation/videos/home/). Jelenleg nem támogatott diagramtípusról beágyazott YouTube-videókat; Ha egy közösségi közreműködői, Ön üdvözli a hivatkozás a YouTube-on, ha meg szeretne jeleníteni a videó van közzétéve-e. A Microsoft munkatársak csatorna 9-es és a videó központot kell használni.

### <a name="usage"></a>Használat

- Győződjön meg róla, hogy a videó a videó központban.

- A videó a csatorna 9 böngészőbarát URL-cím vagy a Azure videó központból, másolja a vágólapra a videó Azonosítót. A videó azonosítója, a videó a [http://azure.microsoft.com/documentation/videos/azure-scheduler-unusual-schedules/](http://azure.microsoft.com/documentation/videos/azure-scheduler-unusual-schedules/) például az **azure-ütemező-szokatlan – ütemtervek**is.

### <a name="syntax"></a>Szintaxis

    > [AZURE.VIDEO video-id-string]

### <a name="rendering"></a>A megjeleníthető

A GitHub: [https://github.com/Azure/azure-content-pr/blob/master/articles/web-sites-backup.md](https://github.com/Azure/azure-content-pr/blob/master/articles/web-sites-backup.md)

A cikk közzétett: [http://azure.microsoft.com/documentation/articles/web-sites-backup/](http://azure.microsoft.com/documentation/articles/web-sites-backup/)


## <a name="technology-and-platform-selectors"></a>Technológia és a platform választók

Használja technológia és a platform switchers technikai cikkek, ha a szerző elkészíti a cím különbségeket végrehajtása során az azonos cikk több változatok technológiák vagy platformon keresztül. Ez a legtöbb általában alkalmazható a mobil platform tartalom fejlesztők számára. Jelenleg választók, [egyszerű választók](#simple-selectors) és [kétirányú választók](#two-way-selectors)két különböző típusú.

Mivel az azonos Adatkijelölő markdown Ugrás az egyes témakörök a kijelölésbe, javasoljuk, hogy a témakörnek a sorkijelölőre elhelyezése egy felvétele, majd arra hivatkozó, hogy az összes a témakörök, amelyek azonos a választó felvétele.

###<a id="simple-selectors"></a>Egyszerű választók

Egyszerű (egyirányú) választók jobbra a cím alatti választógombok formájában jeleníti meg. Ezek a gombok használatával ügyfelek kell az értékhalmaz egyetlen platform vagy technológia, például a .NET rendszerhez, a Node.js és a Java témakörökben.  Az egyéni markdown bővítmény használata bármely választók – ne használja a HTML-választók.  

Az [értesítés hubok – első lépések](http://azure.microsoft.com/documentation/articles/notification-hubs-windows-phone-get-started/) kattintva megtekintheti, hogy a szerző 8 változatának ugyanazon a cikk, de a használt választók navigációs ahhoz, hogy át őket az összes létrehozásának módjától című.

![Példa egyszerű választó](./media/custom-markdown-extensions/selectors.PNG)

####<a name="syntax"></a>Szintaxis

    > [AZURE.SELECTOR]
    - [Hivatkozás: #1 címke](link #1 url)
    - [hivatkozás #2 címke](link #2 url)

Példa:

    > [AZURE.SELECTOR]
    - [Univerzális Windows](../articles/notification-hubs-windows-store-dotnet-get-started/)
    - [Windows Phone](../articles/notification-hubs-windows-phone-get-started/)
    - [iOS](../articles/notification-hubs-ios-get-started/)
    - [Android](../articles/notification-hubs-android-get-started/)
    - [Kindle](../articles/notification-hubs-kindle-get-started/)
    - [Baidu](../articles/notification-hubs-baidu-get-started/)
    - [Xamarin.iOS](../articles/partner-xamarin-notification-hubs-ios-get-started/)
    - [Xamarin.Android](../articles/partner-xamarin-notification-hubs-android-get-started/)

#### <a name="rendering"></a>A megjeleníthető

A fenti képen látható azure.microsoft.com a megjelenítését. A leképezett GitHub lapon a választók hivatkozások listaként jeleníti meg.

###<a id="two-way-selectors"></a>Kétirányú választók

Kétirányú választók lehetővé teszi a felhasználóknak, válassza ki a témakörök a két módon mátrixokban. Ez akkor lényeges, ha a mobil szolgáltatások, például egy Azure technológia több kódmentes platformokon, valamint több ügyfél támogatja. Tartsa szem előtt a következőket:

- Miközben mint készült `(Platform | Backend)`, a dropwdown szöveg most testre szabható.
- Nincs szükség, a listaelemek a mátrix mindegyik pontjának, de csak ha témakör URL-cím létezik, és nem ismétlődő elem van.
- A hivatkozás lehet bármely URL-CÍMÉT, bár általában más GitHub témakört is.

Lásd: a [Mobile szolgáltatások – első lépések](http://azure.microsoft.com/en-us/documentation/articles/mobile-services-ios-get-started/) a szerző 15 változatának ugyanazon cikk (9-es mobil ügyfélprogram platformokon és 2 kódmentes platformokon), de a használt választók navigációs ahhoz, hogy mindet keresztül létrehozásának módjától megtekintéséhez. Ne feledje, hogy 3 cikkek nem kódmentes verziójával is.

![Kétirányú választók példa](./media/custom-markdown-extensions/selector-list.png)

####<a name="syntax"></a>Szintaxis

    > [AZURE. ADATKIJELÖLŐ-lista (Dropdown1 |} Dropdown2)]     -  [(Dropdown1Text1 |} Dropdown2Text1)](../articles/dropdown1-text1-dropdown2-text1.md)
    - [(Dropdown1Text1 |} Dropdown2Text2)](../articles/dropdown1-text1-dropdown2-text1.md)
    - [(Dropdown1Text2 |} Dropdown2Text3)](../articles/dropdown1-text1-dropdown2-text1.md)
    - [(Dropdown1Text3 |} Dropdown2Text4)](../articles/dropdown1-text1-dropdown2-text1.md)

Példa:

    > [AZURE. ADATKIJELÖLŐ-lista (Platform |} Kódmentes)]     -  [(iOS |} .NET)](./mobile-services-dotnet-backend-ios-get-started-push.md)
    - [(iOS |} A JavaScript)](./mobile-services-javascript-backend-ios-get-started-push.md)
    - [(Windows univerzális C# |} .NET)](./mobile-services-dotnet-backend-windows-universal-dotnet-get-started-push.md)
    - [(Windows univerzális C# |} A JavaScript)](./mobile-services-javascript-backend-windows-universal-dotnet-get-started-push.md)
    - [(Windows Phone |} .NET)](./mobile-services-dotnet-backend-windows-phone-get-started-push.md)
    - [(Windows Phone |} A JavaScript)](./mobile-services-javascript-backend-windows-phone-get-started-push.md)
    - [(Android |} .NET)](./mobile-services-dotnet-backend-android-get-started-push.md)
    - [(Android |} A JavaScript)](./mobile-services-javascript-backend-android-get-started-push.md)
    - [(Xamarin iOS |} A JavaScript)](./partner-xamarin-mobile-services-ios-get-started-push.md)
    - [(Xamarin Android |} A JavaScript)](./partner-xamarin-mobile-services-android-get-started-push.md)

#### <a name="rendering"></a>A megjeleníthető

A fenti képen látható azure.microsoft.com a megjelenítését. A leképezett GitHub lapon a választók hivatkozások listaként jeleníti meg.

<!--Anchors-->
[Megjegyzések és tippek]: #notes-and-tips
[Tartalmaz]: #includes
[Beágyazott videók]: #embedded-videos
[Technológia és a platform választók]: #technology-and-platform-selectors

###<a name="contributors-guide-links"></a>Munkatársak naptárának útmutató hivatkozások

- [A cikk – áttekintés](./../README.md)
- [Index útmutatást cikkek](./contributor-guide-index.md)
