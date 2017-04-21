<properties
   pageTitle="Hivatkozások létrehozása markdown cikkek" description="Megtudhatja, hogyan crosslinks markdown a kódot." metaKeywords="" services="" solutions="" documentationCenter="" authors="tysonn" videoId="" scriptId="" manager="carolz" />

<tags ms.service="contributor-guide" ms.devlang="" ms.topic="article" ms.tgt_pltfrm="" ms.workload="" ms.date="02/03/2015" ms.author="tysonn" />

# <a name="linking-guidance-for-azure-technical-content"></a>Útmutató a Azure műszaki tartalom összekapcsolása

### <a name="links-from-one-acom-article-to-another"></a>Hivatkozások egy ACOM cikkből a másikra

Hozzon létre egy beágyazott hivatkozás egy ACOM technikai cikk ACOM technikai cikk, a következő szintaxissal hivatkozás:  

- Egy cikk ugyanabban a szolgáltatás könyvtárban szolgáltatás címtár kapcsolatokban-cikk:

  `[link text](article-name.md)`

- Egy cikk hivatkozást tartalmaz egy szolgáltatás alkönyvtárából a legfelső szintű címtárban:

  `[link text](../article-name.md)`

- A legfelső szintű címtár hivatkozást tartalmaz a szolgáltatás alkönyvtáraként cikkben: 

  `[link text](./service-directory/article-name.md)`

- A szolgáltatás alkönyvtár hivatkozást tartalmaz egy másik szolgáltatás alkönyvtárban cikkben:

  `[link text](../service-directory/article-name.md)`
 

## <a name="links-to-anchors"></a>Horgonyok mutató hivatkozások

Nincs horgonyok létrehozása – az automatikusan generált a közzétételi H2 címsorok ideje. Beállítást kell elvégeznie, létrehozhat hivatkozásokat a H2 szakaszokat.

- Hivatkozni szeretne egy címsort, ugyanazt a cikk belül:

  `[link](#the-text-of-the-H2-section-separated-by-hyphens)`  
  `[Create cache](#create-cache)`

- Hivatkozni szeretne egy másik, azonos alkönyvtárban cikkben horgony:

  `[link text](article-name.md#anchor-name)`
  `[Configure your profile](media-services-create-account.md#configure-your-profile)`

- Hivatkozni szeretne egy másik szolgáltatás alkönyvtárban horgony:

  `[link text](../service-directory/article-name.md#anchor-name)`
  `[Configure your profile](../service-directory/media-services-create-account.md#configure-your-profile)`

Automatizálja a hivatkozások létrehozása horgony automatikusan létrehozott hivatkozások a cikkek egyik módja [MarkdownAnchorLinkGenerator - eszköz a megfelelő formátumban ACOM horgony hivatkozások létrehozására](https://github.com/Azure/Azure-CSI-Content-Tools/tree/master/Tools/ACOMMarkdownAnchorLinkGenerator).

## <a name="links-from-includes"></a>Hivatkozásait tartalmazza.

Minthogy olyan fájlt egy másik mappában találhatók, meg kell használni a már relatív elérési utak alább látható módon. Egy cikk beágyazott fájlból mutató hivatkozás létrehozásához használja ezt a formátumot:

    [link text](../articles/service-folder/article-name.md)
    
További tudnivalók az [egyéni markdown bővítmények irányelvek](custom-markdown-extensions.md#includes)egy beágyazott fájl használatáról.

## <a name="links-in-selectors"></a>Hivatkozások választók

Ha egy Belefoglalás ágyazott választók, összekapcsolásának a rendezés használja: 

    > [AZURE. ADATKIJELÖLŐ-lista (Dropdown1 |} Dropdown2)]     -  [(szöveg1 |} Example1)](../articles/service-folder/article-name1.md)
    - [(szöveg1 |} Pelda2)](../articles/service-folder/article-name2.md)
    - [(szöveg2 |} Example3)](../articles/service-folder/article-name3.md)
    - [(szöveg2 |} Example4)](../articles/service-folder/article-name4.md)


## <a name="reference-style-links"></a>Hivatkozás stílusú hivatkozásokat

Stílus mutató hivatkozások találhatók, így könnyebben olvassa el a forrás-tartalom is használhatja. A stílus mutató hivatkozások találhatók, amely lehetővé teszi, a hosszú URL-ek lépjen a cikk végén egyszerűsített képletszintaxisát cserélje le a beágyazott hivatkozás szintaxisa. Az alábbiakban Daring Fireball példa:

Beágyazott szöveg:

    I get 10 times more traffic from [Google][1] than from [Yahoo][2] or [MSN][3].

A cikk végén hivatkozásait:

    <!--Reference links in article-->
    [1]: http://google.com/
    [2]: http://search.yahoo.com/  
    [3]: http://search.msn.com/

Győződjön meg arról, írja be a térköz után a kettőspont, mielőtt a hivatkozást. Ha elfelejti, amelyet fel szeretne venni a szóköz csatolása további technikai cikkek, ha a hivatkozás oszlanak közzétett című témakörben. 

## <a name="link-to-acom-pages-that-are-not-part-of-the-technical-documentation-set"></a>Nem vesznek részt a technikai dokumentumkészlethez ACOM lapokra mutató hivatkozás

ACOM (például árak lap, SZOLGÁLTATÁSISZINT-lap vagy bármely más, nem a dokumentáció cikk) lapra, amelyre a hivatkozás, használja az abszolút URL, de kihagyja az nyelvét. Az alábbi célja GitHub, majd a leképezett webhelyet a hivatkozások használható:

    [link text](http://azure.microsoft.com/pricing/details/virtual-machines/)


## <a name="link-to-msdn-or-technet"></a>Hivatkozás az MSDN webhelyen vagy a TechNet webhelyen

Ha csatolása az MSDN webhelyen vagy a TechNet webhelyen kell a teljes hivatkozásra kattintva a témakör, és távolítsa el a en-us nyelv hivatkozást. 

### <a name="use-friendly-link-text-for-all-links"></a>Az összes csatolás rövid hivatkozásszöveget használata

A hivatkozás szerepeltetni kívánt szavakat kell rövid – Ez azt jelenti, legyen a normál angol szavak vagy a hivatkozott lap a cím. Ne használjon "kattintson ide". Keresőmotor-Optimalizálási a hibás, és nem megfelelően cél leírása.

**Javítsa ki:**

- `For more information, see the [contributor guide index](https://github.com/Azure/azure-content/blob/master/contributor-guide/contributor-guide-index.md).`

- `For more details, see the [SET TRANSACTION ISOLATION LEVEL](https://msdn.microsoft.com/library/ms173763.aspx) reference.`

**Helytelen:**

- `For more details, see [https://msdn.microsoft.com/library/ms173763.aspx](https://msdn.microsoft.com/library/ms173763.aspx).`

- `For more information, click [here](https://github.com/Azure/azure-content/blob/master/contributor-guide/contributor-guide-index.md).`


## <a name="fwlinks"></a>A Skype vállalati verzióval

A Skype vállalati verzióval (a átirányítás rendszerünk) ne azure.microsoft.com tartalomban. Azok használandó csak utolsó lehetőségként során létre kell hoznia egy lapot, amelynek URL-címe, még nem ismeri a hivatkozást. Gyakorlatilag soha nem ténylegesen szükségesek. Az ACOM meghatározhatja a fájl nevére, így akkor is, hogy mi legyen időszakokra. A tár közötti még nem közzétett létrehozhat egy hivatkozást, amelyre a témakör globálisan egyedi azonosítója használ, így nem kell használni az átirányítási hivatkozás.

A P paraméter, így azok Állandó átirányítás terjed ki kell használnia az átirányítási hivatkozás a weblapon:

    http://go.microsoft.com/fwlink/p/?LinkId=389595

Amikor az átirányítási hivatkozás eszköz illessze a célhely URL-címet, ne felejtse el, ha a cél-hivatkozás ACOM, vagy az MSDN webhelyen vagy a TechNet könyvtár, távolítsa el a területi beállítások.

## <a name="remember-the-azure-library-chrome"></a>Ne feledje, hogy az Azure tár chrome!
Ha szeretne egy Azure tár témakör csoportban [a csomópont](https://msdn.microsoft.com/library/azure)videókra mutató hivatkozás, ne feledje, hogy az Azure megadhatja a hivatkozás (/ azure /) a Chrome-ban. Az Azure chrome megosztja a ACOM navigációs beállítások, és csak az MSDN-tár Azure tartalmat jeleníti meg. Megfelelően hatókörű hivatkozás néz ki:

    http://msdn.microsoft.com/library/azure/dd163896.aspx

Egyéb esetben a lap jelenik meg az MSDN normál nézetben, a teljes MSDN fa jelenik meg.

### <a name="contributors-guide-links"></a>Munkatársak naptárának útmutató hivatkozások

- [A cikk – áttekintés](./../README.md)
- [Index útmutatást cikkek](./contributor-guide-index.md)

<!--image references-->
[1]: ./media/create-tables-markdown/table-markdown.png
[2]: ./media/create-tables-markdown/break-tables.png
