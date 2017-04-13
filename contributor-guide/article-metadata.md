

#<a name="metadata-for-azure-technical-articles"></a>Azure technikai cikkek metaadatait

Az összes Azure technikai cikkekből két metaadatok szakaszok – a tulajdonságok és a címkék szakaszát. A Tulajdonságok szakasz lehetővé teszi, hogy egyes webhely automatizálási és a keresőmotor-Optimalizálási elemek, során a címkék csoport lehetővé teszi, hogy a tartalom jelentése belső sok. Mindkét szakaszok szükség.

- [Szintaxis]
- [Használat]
- [Attribútumok és a Tulajdonságok szakasz értékek]
- [Attribútumok és a címkék szakasz értékek]

##<a name="syntax"></a>Szintaxis

A Tulajdonságok szakasz szintaxisa a következő:

    <properties
       pageTitle="Page title that displays in search results and the browser tab | Microsoft Azure"
       description="Article description that will be displayed on landing pages and in most search results"
       services="service-name"
       documentationCenter="dev-center-name"
       authors="GitHub-alias-of-only-one-author"
       manager="manager-alias"
       editor=""
       tags="optional"
       keywords="For use by SEO champs only. Separate terms with commas. Check with your SEO champ before you change content in this article containing these terms."/>

A címkék csoport szintaxisa a következő:

    <tags
       ms.service="required"
       ms.devlang="may be required"
       ms.topic="article"
       ms.tgt_pltfrm="may be required"
       ms.workload="na"
       ms.date="mm/dd/yyyy"
       ms.author="Your MSFT alias or your full email address;semicolon separates two or more"/>

##<a name="usage"></a>Használat

- Az elem neve és a attribútum neve és nagybetűk.
- A <properties> szakasz kell lennie a fájl első sorában.
- Egy üres sort hagyjon minden metaadatok szakasz után, és a lap címét ahhoz, hogy az oldal előtt cím helyes alakul HTML a közzétételi folyamat során.

## <a name="attributes-and-values-for-the-properties-section"></a>Attribútumok és a Tulajdonságok szakasz értékek

![](./media/article-metadata/checkmark-small.png)**lap címe**: kötelező, keresőmotor-Optimalizálási fontos. A szöveg az attribútum a böngésző lap és a cím, a keresési eredmény jelenik meg. 55-60 karakterek szóközökkel együtt, és többek között a webhely azonosítót *használata |} Microsoft Azure* (mint beírt: Microsoft Azure függőleges vonás terület szóköz).  A lap címe eltér a H1 kell lennie.

![](./media/article-metadata/checkmark-small.png)**Leírás**: kötelező, Fontos, hogy a keresőmotor-Optimalizálási (relevancia) és a webhely funkcióit. A leírás kell legalább 125 karakter hosszú való 155 karakter (szóközökkel együtt). Írja le a tartalom célját, így a felhasználók fog tudja, hogy a keresési eredmények listájában válassza ki. Az érték:

- Ezt a szöveget a leírás vagy a keresési eredmények között a Google absztrakt bekezdés jelennek meg.
- Ez a szöveg megjelenik [a cikk index](https://azure.microsoft.com/documentation/articles/)között.

![](./media/article-metadata/checkmark-small.png)**szolgáltatások**: szükséges projektvezetési cikkeket talál a szolgáltatás foglalkozik. Ez az érték ofter "servce Infó" néven. A megfelelő szolgáltatások, vesszővel elválasztott listája. Az első szolgáltatás, amely a listára a navigációs tájékozódás az oldal és a bal oldali navigációs, amely tulajdonságlapján a lapon látható lesz elindításához.

Adja meg a szolgáltatások érték és a documentationCenter érték cikkekben szolgáltatások értékét a webhely-navigációs fog meghajtó. További értékeket, akkor listában megjelenő címkékként közzétett című témakörben. Értékek:

- Active Directoryval
- aktív-címtár-b2c
- aktív-címtár-ds
- alkalmazás-service\api
- API-kezelés
- alkalmazás-szolgáltatás
- alkalmazás-servic\mobile
- alkalmazás-service\web
- alkalmazás-service\logic
- alkalmazás-átjáró
- alkalmazás-Hírcsatornájában
- automatizálási
- Azure-portál
- erőforrás-Azure-kezelő
- Azure-Papírhalom
- biztonsági másolat
- Köteg
- legjobb – gyakorlás
- BizTalk-szolgáltatások
- gyorsítótár
- CDN
- felhőalapú-szolgáltatások
- adatkatalógus
- adatok gyári
- adatok-tó-elemzés
- tó-adattárhoz
- devtest-labor
- DNS
- documentdb
- készült expressroute
- esemény-hubok
- hdinsight szolgáltatáshoz
- IOT-központban
- billentyű-tárolóból elemre
- terheléselosztó
- gépi tanulási
- Piactér
- multimédia-szolgáltatások
- mobil-előjegyzéssel
- mobil-szolgáltatások
- a multi-factor-authentication
- értesítés-hubok
- műveleti Hírcsatornájában
- műveletek-kezelés – programcsomagban
- powerapps
- helyreállítási-kezelő
- vgx.dll gyorsítótár
- RemoteApp
- tartalomvédelmi
- a Feladatütemező
- Keresés
- biztonsági-központ
- szolgáltatás-bus
- szolgáltatás-háló
- webhely-helyreállítás
- SQL-adatbázis
- SQL-adatraktár
- SQL-jelentés
- tárhely
- tár
- storsimple
- Értékáram-elemzés
- adatforgalom-kezelő
- virtuális gépeken futó
- virtuális hálózati
- Visual – studio online
- VPN-átjáró
- webhelyek –

![](./media/article-metadata/checkmark-small.png)**documentationCenter**: fejlesztők kötődnek cikkek legjobb kiemelt segítségével a fejlesztői központ szükséges. Adja meg a egyetlen fejlesztői központ vagy a nyelvet, a cikk vonatkozik. Az érték, a lista lap navigációs tetejéhez fog elindításához. Adja meg a szolgáltatások érték és a documentationCenter érték cikkekben szolgáltatások értékét a webhely-navigációs fog meghajtó. Értékek:

- **.NET**
- **nodejs**
- **Java**
- **php**
- **Python**
- **fonetikus**
- **mobil**: kiadásában megszűnt. Adott mobil platform cserélje.
- **IOS**: Ez az új érték Verifing
- **Android**: Ez az új érték ellenőrzése
- **a Windows**: Ez az új érték ellenőrzése
- **xamarin**: Ez az új érték ellenőrzése

![](./media/article-metadata/checkmark-small.png)**Szerző**: kötelező, csak egy érték. Az elsődleges szerző vagy a tárgy KKV GitHub fiókjában sorolja fel. Az attribútum a szerző meghajtók, a közzétett olvashatók. A lista csak egyet, annak ellenére, az attribútum többes számú nevét.

![](./media/article-metadata/checkmark-small.png)**kezelő**: kötelező, ha Ön egy Microsoft közös munka. A közzétételi tartalomkezelő a technológia terület az e-mail alias sorolja fel. Ha egy közösségi közreműködői, az attribútum, de üresen hagyja, hogy töltheti.

![](./media/article-metadata/checkmark-small.png)**szerkesztő**: nem használja. Ne használja, más célra.

![](./media/article-metadata/checkmark-small.png)**címkék**: nem kötelező. Csak akkor, ha csoportban a cikk index lapot (http://azure.microsoft.com/documentation/articles/) a cikk navigációs hivatkozás engedélyezni szeretné tartalmazza, amelyek megegyeznek a jóváhagyott értékek közül a cikkek prefiltered listájára. Ezek az értékek van a kialakítva, hogy a tartalom egy csoportba, amikor a tartalom csoportosítás nem szolgáltatásspecifikus ad lehetőséget. A címkéket is megadhatja az parancsot, amely azt jelzi, a technológia Papírhalom a cikk vonatkozik. A érték **nem** támogatja a szabadkézi címkék vagy hashtags; a címkék engedélyeznie kell a webhelyen. Megadhat egy cikk, vesszővel elválasztott értékek több címkék. Az engedélyezett értékei a következők:

  - architektúra
  - erőforrás-Azure-kezelő
  - Azure Szolgáltatáskezelés
  - Számlázás
  - MySQL

![](./media/article-metadata/checkmark-small.png)**kulcsszavak**: nem kötelező. Csak a keresőmotor-Optimalizálási champs használhatják. Külön kifejezések vesszővel válassza el. **Kérdezze meg a keresőmotor-Optimalizálási champ előtt módosítása vagy törlése a tartalom a jelen cikkben ezeket a kifejezéseket tartalmazó.** Az attribútum rekordok kulcsszavak a keresőmotor-Optimalizálási champ van-e irányítva és külön követi nyomon az rangját keresés javítása érdekében. A kulcsszavak nem jelenik meg a közzétett HTML formátumban. Adatérvényesítési nincs szükség az attribútum.

## <a name="attributes-and-values-for-the-tags-section"></a>Attribútumok és a címkék szakasz értékek

![](./media/article-metadata/checkmark-small.png)**MS.Service**: szükséges. Megadja az Azure szolgáltatás, eszközt vagy funkciót, a cikk vonatkozik. Laponként egy értéket.

 Ha egy lapon több szolgáltatás vonatkozik, válassza a szolgáltatás, amely a legtöbb közvetlenül vonatkozik; egy cikk használó üzemeltetett alkalmazás webhelyeken keresztül mutatjuk szolgáltatás Bus funkciókat, például **- webhelyek**helyett a **szolgáltatás-bus** értéket kell rendelkeznie. Ha egy lapon egyaránt több szolgáltatásra vonatkozik, válassza a **több**lehetőséget. Ha egy lapon nem vonatkozik a más szolgáltatások (Ez lesz ritka), válassza a **hiányzik**.

 - **Active Directoryval**
 - **aktív-címtár-b2c**
 - **aktív-címtár-ds**
 - **API-kezelés**
 - **alkalmazás-szolgáltatás**: csak érvényes általános elvi anyag alkalmazás szolgáltatása
 - **alkalmazás-szolgáltatás-api**
 - **alkalmazás-szolgáltatás – logikája**
 - **a mobile service alkalmazás**
 - **a webes szolgáltatás alkalmazás**
 - **alkalmazás-Hírcsatornájában**
 - **alkalmazás-átjáró**
 - **automatizálási**
 - **erőforrás-Azure-kezelő**
 - **Azure-biztonság**
 - **Azure-Papírhalom**
 - **biztonsági másolat**
 - **Köteg**
 - **legjobb – gyakorlás**
 - **BizTalk-szolgáltatások**
 - **Számlázás**
 - **gyorsítótár**
 - **CDN**
 - **felhőalapú-szolgáltatások**
 - **adatkatalógus**
 - **tó-adattárhoz**
 - **adatok-tó-elemzés**
 - **devtest-labor**
 - **készült expressroute**
 - **hdinsight szolgáltatáshoz**
 - **az Internet a dolog:**
 - **IOT-központban**
 - **billentyű-tárolóból elemre**
 - **gépi tanulási**
 - **piactér**: cikkeket talál az Azure piactérről
 - **multimédia-szolgáltatások**
 - **mobil-előjegyzéssel**
 - **mobil-szolgáltatások**
 - **a multi-factor-authentication**
 - **több**: az oldal egyaránt vonatkozik több szolgáltatás
 - **név**: az oldal nem vonatkozik a más szolgáltatások (ritka)
 - **értesítés-hubok**
 - **műveleti Hírcsatornájában**
 - **powerapps**
 - **helyreállítási-kezelő**
 - **vgx.dll gyorsítótár**
 - **RemoteApp**
 - **tartalomvédelmi**
 - **a Feladatütemező**
 - **biztonsági-központ**
 - **szolgáltatás-bus**
 - **szolgáltatás-háló**
 - **webhely-helyreállítás**: korábban helyreállítási szolgáltatások
 - **SQL-adatbázis**
 - **SQL-adatraktár**
 - **SQL-jelentés**
 - **tárhely**
 - **tárolása**: cikkeket talál a Azure áruházból elérhető szolgáltatások
 - **storsimple**
 - **adatforgalom-kezelő**
 - **virtuális gépeken futó**
 - **virtuális hálózati**
 - **Visual – studio online**
 - **VPN-átjáró**
 - **webhelyek –**

![](./media/article-metadata/checkmark-small.png)**MS.devlang**: szükséges. Adja meg a programnyelv, amely a cikk vonatkozik. Egyetlen érték oldalanként.

 Ha egy lapon egyaránt két programozási nyelven vonatkozik, válassza a **több**. Ha egy lapon elsősorban elvi pedig a tartalmát általában alkalmazandó több programnyelv, válassza a **több**. Ha az weblapon nem alkalmazásindítójukban fejlesztők és a programozási nyelven alkalmazási terület nem megfelelő, válassza a **hiányzik**. **Többi-api** segítségével azonosítani a REST API-referenciaanyagokat.

 - **cpp**
 - **DotNet**
 - **Java**
 - **a JavaScript**
 - **több**: az oldal egyaránt vonatkozik több programozási nyelven.
 - **név**: az oldal nem van kiválasztásával fejlesztőknek, és nem jellemző bármely programozási nyelven.
 - **nodejs**
 - **Cél-c**
 - **php**
 - **Python**
 - **REST API-felületről**
 - **fonetikus**


![](./media/article-metadata/checkmark-small.png)**MS.topic**: szükséges. Adja meg a témakör adatai. A legtöbb, a munkatársai által létrehozott új lapok cikk vagy a hivatkozás lesz.

 - **a cikk**: A elvi tematikus, oktatóanyag, szolgáltatásai – útmutató vagy más nem valhttps://technet.microsoft.com/library/dn741250.aspx foglalkozó referenciacikkünket

 - **marketingkampány-lap**: csak a Azure.com.  Egy lapot, amely szerint a céloldal külső kampányok kifejezetten, és nem az elsődleges webhely vonatkozó része  Nem használható dokumentáció cikkek vagy érkezési oldalak normál dokumentumot.  Példa: azure.microsoft.com/develop/net/aspnet/; Azure.microsoft.com/develop/Mobile/IOS/

 - **a fejlesztői-központ – kezdőlap**: csak a Azure.com.  A fejlesztők pl. center kezdőlapja, / / nettó/kidolgozása

 - **első lépések cikk**: hozzárendelése cikkekre mutató vannak kiemelve a bal oldali navigációs egy szolgáltatás a kezdéshez vagy – áttekintés című szakaszát.

 - **a fő-cikk**: "fő" oktatóanyagot, amely célja, hogy adja meg a szolgáltatás bemutatása, vagy a szolgáltatás, amely a látogatók a szolgáltatással gyorsan lépések kap, és ingyenes próbaverziót címlistasablon és MSDN aktiválásra meghajtók. Ez az érték hozzárendelése csak cikkekre mutató felső részén a szolgáltatás dokumentációja céloldal kiemelt vannak.

 - **a kezdőlap**: legfelső szintű dokumentáció kezdőlapján. Csak felkínálunk két: azure.microsoft.com/documentation/ és msdn.microsoft.com/library/azure/

 - **Tárgymutató-lap**: második szintű érkezési oldalak programozási nyelven, szolgáltatások és funkciók. Ezek helyezkednek Azure.com és a tárban, és hatókörű szűkebb információk belépési pontról használják. Példa: http://azure.microsoft.com/develop/mobile/resources-wp8/, http://msdn.microsoft.com/library/azure/jj673460.aspx, http://msdn.microsoft.com/library/azure/hh689864.aspx

 - **hivatkozás az oldalon infographic**: csak a Azure.com. Egy olyan böngészhető infographic vagy plakát, például http://azure.microsoft.com/documentation/infographics/windows-azure/ szolgáltatások lap

 - **hivatkozás**: egy API hivatkozás (beleértve a REST API-val) vagy PowerShell parancsmaggal hivatkozás lapon

 - **szolgáltatás-kezdőlap**: csak a Azure.com.  A dokumentum szolgáltatás kezdőlapján pl. /documentation/services/virtual-machines /

 - **webhely-szakasz-kezdőlapja**: csak a Azure.com. Egy adott típusú tartalomhoz azure.com példák "kezdőlapját": http://azure.microsoft.com/documentation/infographics/, http://azure.microsoft.com/documentation/scripts/, http://azure.microsoft.com/documentation/videos/home/, http://azure.microsoft.com/downloads/

 - **a videó az oldalon**: csak a Azure.com.  Videó, például http://azure.microsoft.com/documentation/videos/azure-webjobs-hosting-testing-net/ szolgáltatások lap

![](./media/article-metadata/checkmark-small.png)**MS.tgt_pltfrm**: szükséges. Adja meg a cél platform, például Windows, Linux, Windows Phone, iOS, Android és speciális gyorsítótár platformokon. Laponként egy értéket. Ez az érték lesz **hiányzik** a legtöbb kivételével mobile témakörök és virtuális gépeken futó.

 - **gyorsítótár-a-szerepkörök**
 - **gyorsítótár-több**
 - **gyorsítótár vgx.dll**
 - **gyorsítótár-szolgáltatás**
 - **gyorsítótár megosztott**
 - **parancssor line felület**
 - **Ibizai**: a Ibizai portál használó tartalom. Ezzel csak abban az esetben, ha a tárgyalt funkció a Ibizai portál és az aktuális portálon is.
 - **mobil Android rendszerű**: Azure.com csak most
 - **a html-Mobile**: Azure.com csak most
 - **az ios Mobile**: Azure.com csak most
 - **mobil-kindle**: Azure.com csak most
 - **mobil-több**
 - **mobil-nokia-x**: Azure.com csak most
 - **mobil-phonegap**: Azure.com csak most
 - **mobil-sencha**: Azure.com csak most
 - **Mobile windows**: Azure.com csak most; A Windows univerzális
 - **windows-mobiltelefonon**
 - **mobil – a windows-tár**
 - **mobil-xamarin**: Azure.com csak most; Xamarin minden platform
 - **mobil xamarin Android rendszerű**: Azure.com csak most
 - **mobil-xamarin-ios**: Azure.com csak most
 - **több**: az oldal egyaránt vonatkozik több platformok
 - **hiányzik**: egy platform módosulnak, akkor nem használható a lapon
 - **a PowerShell**
 - **virtuális linux**
 - **virtuális-több**
 - **windows-virtuális**
 - **a sharepoint windows virtuális**
 - **virtuális-windows-sql server**
 - **viewben-– bevezetés**: azonosítja a VIEWBEN első lépések lap csoportot. A címke 12/1/14 hozzáadott.
 - **viewben – Mi-történt**: azonosítja a VIEWBEN az első lépések Mi történt a lapot. A címke 12/1/14 hozzáadott.

![](./media/article-metadata/checkmark-small.png)**MS.workload**: szükséges. Itt adhatja meg az oldal alkalmazott Azure terhelését. Egy érték, csak egy cikk.

**8/6/15 frissítése** A ms.workload érték egy xls, nem a .md-fájlban szereplő érték szerint leképezése. A ms.workload értéke az érvényességi továbbra is szükséges mindaddig, amíg a szolgáltatás frissíthető. Most már van ütemezve, hogy a munkát.  Használjon **"név"** értékkel most.

![](./media/article-metadata/checkmark-small.png)**MS.Date**: szükséges. Találati pontosság, pontosság, megfelelő képernyőképek és működő hivatkozások a cikk utolsó lett felül dátumát adja meg. Adja meg a dátumot hh/nn/éééé formátumban. Ez a dátum is megjelenik a közzétett cikk utolsó frissített dátumként.

![](./media/article-metadata/checkmark-small.png)**MS.Author**: szükséges. Adja meg a szerző(k) a témához kapcsolódó. Belső jelentések (például frissessége) ezt az értéket a cikk a megfelelő Szerző(k) társíthatja használni. Több értéket is meg kell pontosvesszővel kell őket. Elfogadhatók Microsoft alias vagy a teljes e-mail címét. Az időtartam legfeljebb 200 karakter hosszúságú lehet.


###<a name="contributors-guide-links"></a>Munkatársak naptárának útmutató hivatkozások

- [A cikk – áttekintés](./../README.md)
- [Index útmutatást cikkek](./contributor-guide-index.md)


<!--Anchors-->
[Szintaxis]: #syntax
[Használat]: #usage
[Attribútumok és a Tulajdonságok szakasz értékek]: #attributes-and-values-for-the-properties-section
[Attribútumok és a címkék szakasz értékek]: #attributes-and-values-for-the-tags-section
