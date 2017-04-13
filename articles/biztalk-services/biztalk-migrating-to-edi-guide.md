<properties   
    pageTitle="Áttelepítés BizTalk kiszolgálói szerkesztése megoldások BizTalk szolgáltatások technikai útmutató |} Microsoft Azure"
    description="Szerkesztése áttelepítése MABS; Microsoft Azure BizTalk szolgáltatások"
    services="biztalk-services"
    documentationCenter="na"
    authors="MandiOhlinger"
    manager="erikre"
    editor=""/>

<tags 
    ms.service="biztalk-services"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="mandia"/>


# <a name="migrating-biztalk-server-edi-solutions-to-biztalk-services-technical-guide"></a>Áttelepítés BizTalk kiszolgálói szerkesztése megoldások BizTalk szolgáltatások: technikai útmutató

Szerző: Tim Wieman és Nitin Mehrotra

Véleményező: Karthik Bharthy

Használatával írt: Microsoft Azure BizTalk szolgáltatások – február 2014-es kiadás.

## <a name="introduction"></a>– Bevezetés

Elektronikus adatok Interchange (szerkesztése) az egyik legelterjedtebb azt jelenti, mely vállalkozások exchange adatok elektronikusan, üzleti vállalati verzió vagy B2B tranzakcióként is zónára. BizTalk kiszolgálón keresztül egy évtizede kezdeti BizTalk Server alkalmazás szolgálattól támogatása szerkesztése volt. Az BizTalk szolgáltatásaival Microsoft szerkesztése megoldások támogatása a Microsoft Azure platform továbbra is. B2B tranzakciók főleg-szervezeten kívüli, és így még egyszerűbb, ha egy felhőalapú platformon implementálása történt. Microsoft Azure Ez a képesség BizTalk szolgáltatásokon keresztül biztosít.

Néhány ügyfelek új szerkesztése megoldások "greenfield" platformot, tekintse meg BizTalk szolgáltatások, miközben a vevőknek aktuális BizTalk Server szerkesztése megoldások is szeretnének áttelepítése az Azure van. Mivel BizTalk szolgáltatások szerkesztése van tervezett alapján azonos kulcs szervezetek, mint a BizTalk kiszolgáló szerkesztése architektúra (kereskedési a partnerek, a személyek, a rendelkezést), akkor lehet BizTalk Server szerkesztése eltérések áttelepítése BizTalk szolgáltatások.

A dokumentum vázolja BizTalk szolgáltatások felügyeli áttelepítése BizTalk Server szerkesztése eltérések a különbségeket. A dokumentum tartalma feltételezi, hogy BizTalk Server szerkesztése feldolgozás és a partnerek rendelkezést kereskedési munka ismerete. BizTalk Server szerkesztése a további tudnivalókért lásd: [Partnerek kezelése használatával BizTalk kiszolgáló kereskedési](https://msdn.microsoft.com/library/bb259970.aspx).

## <a name="which-version-of-biztalk-server-edi-artifacts-can-be-migrated-to-biztalk-services"></a>Melyik verziója BizTalk Server szerkesztése eltérések telepíthető át az BizTalk szolgáltatások?

A BizTalk kiszolgáló szerkesztése modul jelentősen szervizcsomagja BizTalk Server 2010, ha volt a partnerek, profilok és rendelkezést újra modellezett. BizTalk szolgáltatásokat kereskedelmi a partnerek és a kereskedelmi partnereknek belül az üzleti részlegek rendszerezése ugyanazt a modellt használja. Emiatt szerkesztése eltérések áttérés BizTalk Server 2010-es és újabb verziók BizTalk szolgáltatásokhoz, az egy sokkal több egyenes előre folyamat. Áttérés előtt BizTalk Server 2010-es verzió társított szerkesztése eltéréseket, frissítenie BizTalk Server 2010-re, és kattintson a szerkesztése eltérések áttelepítése BizTalk szolgáltatások.

## <a name="scenariosmessage-flow"></a>Alkalmazási helyzetek/üzenet továbbításához

Mint BizTalk kiszolgálóval BizTalk szolgáltatások feldolgozás szerkesztése épülnek kereskedési Partner (TPM) megoldást. A TPM megoldást fő összetevője a következő:

- Kereskedelmi partnerek, amely jelenítik meg a szervezet B2B tranzakció.
- Profilok, amelyek a kereskedelmi partner belül részlegek képviseli.
- Kereskedési partner rendelkezést (vagy rendelkezést), amely jelenítik meg a vállalati szerződés két partnerek/profilok közötti.

Az alábbi ábra szemlélteti a hasonlóságokat, valamint a, és közötti különbségek BizTalk Server szerkesztése megoldás BizTalk szolgáltatások szerkesztése megoldást:

![][EDImessageflow]

A főbb különbségek vannak, és hasonlóságokat egy szerkesztése megoldás folyamat BizTalk Server és a BizTalk szolgáltatások között a következők:

- Ugyanúgy, mint a BizTalk kiszolgáló egy EDIReceive folyamat használ, üzenetben szerkesztése és szerkesztése üzenet küldése egy EDISend folyamat, BizTalk támogatási szolgálata-híd szerkesztése fogadása fogadására és a szerkesztése küldése híd szerkesztése üzenetet küldjön. BizTalk Server a folyamatok társulnak olyan megállapodást küldés használatával vagy portok kap. BizTalk szolgáltatások a szerződés magát azt jelzi, a Küldés vagy híd kap.
- A kiszolgáló BizTalk a EDIReceive folyamat folyamatok szerkesztése üzenetben után az üzenet kiírja SQL Server-adatbázishoz. A EdiSend folyamat ezután választja ki az üzenetet az SQL Server-adatbázisból, folyamatok, és visszaküldi a kereskedési partnernek.

    BizTalk szolgáltatások után a szerkesztése fogadása híd szerkesztése üzenet dolgozza fel, azt egy külső folyamat továbbítja az üzenetet. A külső folyamat sikerült futtató Microsoft Azure vagy a helyszíni. A külső folyamat kell az üzenet átirányítása a szerkesztése küldés híd; a Küldés híd eleve nem húzza az üzenetet. A feldolgozás után az üzenetet, a szerkesztése küldés híd továbbítja az üzenetet a kereskedelmi partner.

BizTalk szolgáltatások gyorsan létre, és helyezhetnek üzembe a kereskedelmi partnerek nélkül konfigurálása bármely kiszámításához a Microsoft Azure-példány (webes vagy dolgozó szerepkörök), a Microsoft Azure SQL-adatbázisait vagy a minden Microsoft Azure tárterület-fiókból egy B2B megállapodást könnyen használható konfigurációs szolgáltatásokat nyújt. Összetettebb esetek esetén kell munkafolyamatok vagy más szolgáltatás feldolgozás és "szélei körül" egy Partner kereskedési szerződés Ez azt jelenti, hogy előtti vagy utáni kereskedési Partner szerződés szerkesztése híd feldolgozása. A részletes a következő sorozatok események feldolgozása BizTalk szolgáltatások üzenetben szerkesztése során fordul elő.

1. Egy szerkesztése üzenet jelenik meg a partner, a Fabrikam kereskedési.  Szerkesztése üzenetek fogadására kereskedelmi partnertől, BizTalk szolgáltatásokat támogatja a átviteli protokollok, például az FTP, SFTP, AS2 és a HTTP/s

2. A kereskedelmi partner szerződés fogadása melletti feldolgozás visszafejti az XML formátum szerkesztése üzenetet.  Szolgáltatás Bus végpontokhoz, például egy szolgáltatás Bus továbbítási végpontot, szolgáltatás Bus témakör, szolgáltatás Bus várólista vagy egy BizTalk szolgáltatások azt is átirányítja az szétszerelt szerkesztése üzenet (XML formátumban).

3. Az XML-szétszerelt üzenetek majd kapott további egyéni feldolgozásra végpontot.  Ezeket a végpontokat például dolgozható egy helyszíni összetevő vagy a Microsoft Azure kiszámítására példány további folyamat egy Windows munkafolyamat (a Windows Tűzfal) vagy a Windows Communication Foundation (WCF) szolgáltatás, az üzenet.

4. A "Küldés melletti feldolgozás" a kereskedelmi partner szerződés majd az XML-üzenet összeállítja szerkesztése formátumba, és elküldi kereskedelmi partner, a Contoso.  Kereskedelmi partnerek szerkesztése üzeneteket küld, BizTalk szolgáltatások szerkesztése üzenetek fogadására használtakkal azonos protokollok támogatja.

További a dokumentum elvi útmutatást nyújt az áttelepítés néhányat a különböző BizTalk Server szerkesztése BizTalk szolgáltatások.

## <a name="sendreceive-ports-to-trading-partners"></a>Küldési/fogadási portokat a kereskedelmi partnerek

BizTalk kiszolgálón állítsa be a helyek fogadása és fogadási portok szerkesztése/XML-üzenetek fogadására kereskedelmi partnereket, és szerkesztése/XML üzeneteket küldeni kereskedelmi partner küldés portok beállítása. Ezután kötik egy kereskedelmi partner szerződés, a BizTalk Server felügyeleti konzolban portokhoz be. A BizTalk szolgáltatások, a helyek, ahol kapott üzenetek kereskedelmi partnerek és a hol elküldött üzenetek kereskedelmi partnerek úgy van konfigurálva, a kereskedelmi partner szerződés (átviteli beállításainak részeként) magát a BizTalk Services portál részeként.  Így nem igazán rendelkezik amely a "Küldés portok" és "fogadásához helyek" önmagában, BizTalk szolgáltatások. További tudnivalókért olvassa el a [Rendelkezést létrehozása](https://msdn.microsoft.com/library/windowsazure/hh689908.aspx)című témakört.

## <a name="pipelines-bridges"></a>A folyamatok (hidakat)

BizTalk Server szerkesztése folyamatok, amely az adott feldolgozás lehetőségeit, igény szerint az alkalmazás egyéni logika is használható üzenet feldolgozás személyek is. BizTalk szolgáltatásokhoz az annak megfelelő esetben egy szerkesztése híd. Azonban BizTalk szolgáltatások – fontos, az szerkesztése hidakat "bezárja".  Egy szerkesztése híd Ez azt jelenti, nem adhatja hozzá a saját egyéni tevékenységeket. Minden olyan egyéni feldolgozási kell elvégezni az alkalmazásban a szerkesztése híd kívüli előtt vagy után az üzenet beírja a híd konfigurálva a Partner kereskedési megállapodás keretében. EAI hidakat végezze el az egyéni feldolgozási lehetősége van. Ha azt szeretné, hogy egyéni feldolgozási, a EAI hidakat előtt vagy után az üzenetet a szerkesztése híd által feldolgozott használhatja. További információért megtudhatja, [hogy miként hidakat egyéni kódot tartalmazza](https://msdn.microsoft.com/library/azure/dn232389.aspx).

A közzététel/előfizetés továbbításához egyéni kódot és/vagy az üzenetben a sorok és témakörök előtt a kereskedelmi partner megállapodás az üzenetet kap, vagy a szerződés dolgozza fel az üzenetet, és továbbítja egy szolgáltatás Bus végpontot szolgáltatás Bus használatával is beszúrhat.

Ez a témakör az üzenet továbbításához minta **Esetek/üzenet továbbításához** megtekintése

## <a name="agreements"></a>Rendelkezést

Ha ismeri a BizTalk Server 2010 Trading Partner rendelkezést tartalmazó szerkesztése feldolgozásra használt, partner rendelkezést kereskedési BizTalk szolgáltatások keresése nagyon ismerősnek. A szerződés beállítások a legtöbb megegyeznek, és használja a ugyanazokat a kifejezéseket. Bizonyos esetekben a szerződés beállítások az alábbiak sokkal egyszerűbb ugyanazokat a beállításokat a BizTalk kiszolgálón képest. Microsoft Azure BizTalk szolgáltatásokat támogatja X12 EDIFACT és AS2 átviteli.

Microsoft Azure BizTalk szolgáltatásokat is tartalmaz áttelepítése kereskedelmi partnerek és rendelkezést BizTalk kiszolgáló kereskedési Partner modulból BizTalk Services portál **TPM adatok áttelepítési** eszköz. Érhető el a TPM adatainak áttelepítési eszköz eszközök csomag részeként, amely letölthető a [MABS SDK csomagjában talál](http://go.microsoft.com/fwlink/p/?LinkId=235057). A csomag is tartalmaz egy fontos ismertető eszközt, és az alapvető hibaelhárítási információkat használatáról az eszközhöz.

## <a name="schemas"></a>A sémák

BizTalk szolgáltatások szerkesztése sémák BizTalk szolgáltatások megoldások használt biztosít.  Ezeken kívül BizTalk Server szerkesztése sémák is kínál BizTalk szolgáltatások mivel a legfelső szintű csomópont szerkesztése séma azonos BizTalk kiszolgáló, valamint a BizTalk szolgáltatások keresztül. Fogja így tudja közvetlenül a BizTalk kiszolgáló szerkesztése sémák készíthet, és használhatja őket a szerkesztése megoldásokat fejleszt BizTalk szolgáltatások használatával. A sémák is letöltése a [MABS SDK csomagjában talál](http://go.microsoft.com/fwlink/p/?LinkId=235057).

## <a name="maps-transforms"></a>Térképek (átalakítások)

Térképek BizTalk kiszolgálón átalakítások úgynevezett BizTalk szolgáltatások. BizTalk szolgáltatások BizTalk kiszolgálóról áttelepítése térképek lehet egy összetettebb feladatot (attól függően, hogy térkép összetettsége) eléréséhez. A leképezési eszköz BizTalk szolgáltatásokhoz használt eltér a BizTalk hozzárendelést. Annak ellenére, hogy a hozzárendelést ugyanúgy nézzenek ki főleg, az alapul szolgáló térkép formátuma különböző. A functoids (hívott **Térkép műveletek** BizTalk szolgáltatások) a felhasználók számára elérhető, valamint eltérőek.  Gyakorlatilag azt mondja nem közvetlenül használhatók BizTalk térkép BizTalk szolgáltatások. Nem az összes rendelkezésre álló BizTalk kiszolgálón functoids is térkép műveletként BizTalk szolgáltatásokban elérhető.

### <a name="new-transform-operations"></a>Új átalakítási műveletek

Átalakítás térkép végezhető műveletek listájának igazán eltér a BizTalk kiszolgáló hozzárendelést tűnhet, miközben új módszerek a célra ugyanazok a feladatok BizTalk szolgáltatások átalakítások van. Tegyük fel például BizTalk szolgáltatások átalakítások van rendelkezésre álló **Lista műveletek** . Ez nem volt a BizTalk hozzárendelést érhető el.  A **Lista műveletek** engedélyezése hozhat létre, és a "Lista", működjön, ha egy lista olyan elemeket (más néven "sor"), és ha a minden elem beállíthatja, hogy több tagok (más néven "oszlop").  A lista rendezése, jelölje ki azokat az elemeket alapján a feltétel stb.

Új funkciók a BizTalk szolgáltatások átalakítások egy másik példa olyan **Hurok műveletek**.  Beágyazott hurkok létrehozása a BizTalk kiszolgáló hozzárendelést nehezen.  A leállításig térkép műveletek így az átalakítások BizTalk szolgáltatások kerülnek.

Még egy másik példa a **Ha-akkor-máskülönben** kifejezés térkép műveletet.  Ha-akkor-máskülönben művelet során nem volt a BizTalk hozzárendelést lehetőség, de szükség látszólag egyszerű tevékenység elvégzésére több functoids.

### <a name="migrating-biztalk-server-maps"></a>Maps áttelepítése BizTalk kiszolgálón

A Microsoft Azure BizTalk Services biztosít, BizTalk kiszolgáló áttelepítendő eszköz megfelelteti BizTalk szolgáltatások átalakítások. A **BTMMigrationTool** érhető el a mellékelt [BizTalk szolgáltatások SDK letöltése](http://go.microsoft.com/fwlink/p/?LinkId=235057) **eszközök** csomag részeként. A eszközzel kapcsolatos további tudnivalókért olvassa el a [BizTalk szolgáltatások átalakításához BizTalk térkép konvertálása](https://msdn.microsoft.com/library/windowsazure/hh949812.aspx)című témakört.

Is megnézheti egy minta Sandro Pereira, BizTalk MVP, amelyet áthelyezé [BizTalk szolgáltatások átalakítások BizTalk kiszolgálón van hozzárendelve](http://social.technet.microsoft.com/wiki/contents/articles/23220.migrating-biztalk-server-maps-to-windows-azure-biztalk-services-wabs-maps.aspx).

## <a name="orchestrations"></a>Orchestrations

Ha szeretné áttelepíteni a Microsoft Azure feldolgozás BizTalk kiszolgáló üzletifolyamat-tervező, a orchestrations kellene kell írni, mert a Microsoft Azure nem támogatja a futó BizTalk kiszolgáló orchestrations.  A Windows munkafolyamat Foundation 4.0 (WF4) szolgáltatás üzletifolyamat-tervezési funkciók sikerült újraírása.  Ez akkor is egy teljes átírása jelenleg nincs áttérés BizTalk kiszolgáló orchestrations WF4 nem. Íme néhány Windows-munkafolyamathoz tartozó erőforrások:

- [*Hogyan integrálja a WCF-alapú munkafolyamat szolgáltatás szolgáltatás Bus sorban várakozó és témakörök*](https://msdn.microsoft.com/library/azure/hh709041.aspx) Paolo Salvatori szerint. 

- Az összeállítás 2011 konferencia [ *épület-alkalmazásokat a Windows folyamatkövető alaprendszer és Azure* munkamenetet](http://go.microsoft.com/fwlink/p/?LinkId=237314) .

- [*A Windows folyamatkövető alaprendszer fejlesztői központja*](http://go.microsoft.com/fwlink/p/?LinkId=237315) MSDN webhelyen.

- [*Windows munkafolyamat 4-es Foundation (WF4) dokumentációjában*](https://msdn.microsoft.com/library/dd489441.aspx) találhat az MSDN.

## <a name="other-considerations"></a>Egyéb szempontok

Az alábbiakban néhány dolgot BizTalk szolgáltatások használata során is el kell végeznie.

### <a name="fallback-agreements"></a>Visszaváltási rendelkezést

BizTalk Server szerkesztése feldolgozás "Visszalépés rendelkezést", amely tartalmaz.  BizTalk szolgáltatások jelent **nem** Visszalépés szerződés fogalma, amennyiben van.  Témakörök BizTalk dokumentáció [konfigurálása globális rendszergazdának vagy visszaváltás szerződés tulajdonságok](https://msdn.microsoft.com/library/bb245981.aspx) [Szerkesztése feldolgozása rendelkezést a a szerepkör](http://go.microsoft.com/fwlink/p/?LinkId=237317) és információt a BizTalk Server használatának Visszalépés rendelkezést.

### <a name="routing-to-multiple-destinations"></a>Továbbítás több helyre

BizTalk szolgáltatások hidakat, annak aktuális állapotában nem támogatja a közzététel használatával több helyre útválasztási üzenetek-előfizetés modell. Ehelyett sikerült átirányítja szolgáltatás Bus témakörben, majd az üzenet egynél több végpont több előfizetéssel rendelkező BizTalk szolgáltatások hidat származó üzeneteket.

## <a name="conclusion"></a>Elfogadásáról

Microsoft Azure BizTalk szolgáltatások frissül a normál mérföldkövek hozzáadása több funkcióit és képességeit. Az egyes frissítése megnézi előre a támogatási megnövelt funkciók BizTalk szolgáltatások és más Azure technológiák végpont megoldások megkönnyítése érdekében.

## <a name="see-also"></a>Lásd még:

[Az Azure vállalati alkalmazások fejlesztéséhez](https://msdn.microsoft.com/library/azure/hh674490.aspx)

[EDImessageflow]: ./media/biztalk-migrating-to-edi-guide/IC719455.png
