# <a name="compute"></a>Számítási

Azure lehetővé teszi, hogy a üzembe helyezéséhez és a Microsoft adatközpontjaiban belül futó alkalmazás kód figyelése. Amikor-alkalmazás létrehozása és futtatásával Azure, a kód és a konfigurációs együtt Link-Azure üzemeltetett szolgáltatást. Szolgáltatott szolgáltatások is egyszerűen kezelése, méretezheti felfelé és lefelé, átkonfigurálása, és frissítheti az alkalmazás-kódot az új verzióival. Ez a cikk az Azure üzemeltetett koncentrál szolgáltatás alkalmazásmodell.<a id="compare" name="compare"></a>

## A tartalomjegyzék<a id="_GoBack" name="_GoBack"></a>

-   [Azure alkalmazás modell előnyeit][]
-   [Szolgáltatott szolgáltatás Core – fogalmak][]
-   [Szolgáltatott szolgáltatás tervezési szempontok][]
-   [Az alkalmazás a skála tervezése][]
-   [Definiált szolgáltatott szolgáltatás és konfiguráció][]
-   [A szolgáltatás-definíciós fájl][]
-   [A szolgáltatás konfigurációs fájl][]
-   [Hozhat létre és szolgáltatott szolgáltatás üzembe helyezése][]
-   [Hivatkozások][]

## <a id="benefits"> </a>Azure alkalmazás modell előnyeit

A szolgáltatott szolgáltatásként az alkalmazás telepítésekor Azure hoz létre egy vagy több virtuális gépeken futó (VMs), az alkalmazás kódját tartalmazó, és a VMs betölti az Azure adatközpontokban egyikében tartózkodó fizikai gépeken. Ügyfelek kéréseit az szolgáltatott alkalmazás adja meg az adatközpont, mint egy terheléselosztó ezeket a kérelmeket egyaránt a VMs osztja meg. Az alkalmazás az Azure-ban üzemelteti, miközben kap három fő előnyökkel jár:

-   **Magas elérhető.** Magas elérhetősége azt jelenti, hogy az Azure biztosítja, hogy az alkalmazás szerint működik-e, és tudja ügyfél kérelmekre válaszolni. Ha az alkalmazás megszakítja (miatt nem kezelt kivétel, például), majd Azure észleli a, akkor automatikusan indítsa újra az alkalmazást. Ha az alkalmazás a gépen fut szolgáltatások hardver hiba lépett fel, majd Azure fog is észleli a automatikusan hozzon létre egy új virtuális egy másik számítógépen munka tényleges és onnan futtassa a kód. Megjegyzés: Ahhoz, hogy az alkalmazás a Microsoft szolgáltatási szint Szerződés 99.95 %-os érhető el, rendelkeznie kell legalább két VMs az alkalmazás kód futtatását. Ügyfél kérések feldolgozása, miközben a Azure sorból helyezi át a kódot a sikertelen virtuális egy új, jó virtuális egy virtuális lehetővé teszi.

-   **Méretezhetőség.** Azure segítségével egyszerűen, és kezelheti a tényleges terhelés alatt álló helyezett el az alkalmazást az alkalmazás kód futtatását VMs számának dinamikusan módosítása. Ez a terhelést a ügyfelei közben fizet csak a kellő időben van szüksége a VMs rajta helyez az alkalmazás beállítása teszi lehetővé. Ha módosítani szeretné a számát VMs, Azure válaszol, így dinamikusan módosítani szeretné a számát rendszert futtató, amilyen gyakran kívánt VMs percen belül.

-   **Kezelhetőség.** Mivel a Azure Platform kínáló szolgáltatás (PaaS), kezeli az alábbi operációs rendszert futtató számítógépeken megtartása szükséges infrastruktúra (a hardver magát, elektromos és Hálózatkezelés). Azure is kezeli a platform, a minden a megfelelő javítások és biztonsági frissítések, sőt, például a .NET-keretrendszer és az Internet Information Services összetevő frissítések naprakész operációs rendszer biztosítása. Az összes VMs futtatja a Windows Server 2008, mert Azure például diagnosztikai figyelés, a távoli asztali támogatásának, a tűzfalak és a tanúsítványok tár beállítása további szolgáltatásokat is nyújt. Ezek a funkciók a nem találhatók további költség. Erre valójában készítésekor az alkalmazás Azure-ban a Windows Server 2008 operációs rendszeren licenc megtalálható. Mivel az összes a VMs futtatja a Windows Server 2008, bármilyen kódot, amely a Windows Server 2008 fut csak jól működik fut Azure-ban.

## <a id="concepts"> </a>Üzemeltetett szolgáltatás Core – fogalmak

Amikor az alkalmazás telepítve van az Azure szolgáltatott szolgáltatásként, futása szerint egy vagy több *szerepkörök.* *Szerepkör* egyszerűen alkalmazások fájljai és konfigurációs hivatkozik. Egy vagy több szerepkörök adhatja meg az alkalmazás, a saját alkalmazások fájljai és konfiguráció beállítása mindegyiket. Az alkalmazás minden szerepkörhöz megadhatja VMs vagy *szerepkör-példányok*száma futtatásához. Az alábbi ábrán egy alkalmazást használó szerepkörök és szerepkör-példányok szolgáltatott szolgáltatásként modellezni két egyszerű példa megjelenítése.

##### <a name="figure-1-a-single-role-with-three-instances-vms-running-in-an-azure-data-center"></a>Ábra 1: Egyetlen szerepkör a három példány (VMs), kattintson az Azure adatok központ fut

![kép][0]

##### <a name="figure-2-two-roles-each-with-two-instances-vms-running-in-an-azure-data-center"></a>Ábra 2: Két szerepkörök, mindegyiket VMs két példánya fut az Azure adatközpont

![kép][1]

Szerepkör-példányok általában összehívások internetes ügyfél beírása az Adatközpont *beviteli végpont*úgynevezett keresztül. Egyetlen szerepkör 0 vagy több bemeneti végpontok állhat. Minden végpontra (HTTP, HTTPS vagy TCP) protokoll olyan portot mutatja. Gyakori beállítása, hogy a két bemeneti végpontok szerepkörbe: 80-as és 443-as port figyelését HTTPS figyel HTTP. Az ábrán az alábbi példa az két különböző szerepeket és más beviteli végpontok irányítja őket kéréseket.

![kép][2]

Azure-ban tárolt szolgáltatás létrehozásakor meg van-e hozzárendelve egy nyilvánosan címmel rendelkező IP-címet, amely ügyfélprogramok segítségével elérheti őket. A szolgáltatott szolgáltatás létrehozását követően is választania kell az IP-címet a rendelt URL-cím előtaggal. Ezt az előtagot egyedinek kell lennie, az *előtag*lényegében lefoglalja. cloudapp.net URL-CÍMÉT, így nem is kell, hogy senki ne azt. Ügyfelek az URL-cím használatával kommunikál a szerepkör-példányok. Általában, akkor nem terjesztése, illetve közzététele az Azure *előtag*. cloudapp.net URL-CÍMÉT. Ehelyett fog DNS-tartományregisztrálója megválasztott vásárolja meg a DNS-név, és állítsa be a DNS-név ügyfél kérések átirányítása az Azure URL-címet. További részletekért tanulmányozza a [Azure az egyéni tartománynév beállítása][]című témakört.

## <a id="considerations"> </a>Üzemeltetett szolgáltatás tervezési szempontok

Futó felhőalapú környezetben alkalmazás tervezésekor több dolgot például késés, magas rendelkezésre állás és méretezhetőség megfontolni.

Az alkalmazás-kód elhelyezése esetén fontos tényező egy szolgáltatott szolgáltatást futtató Azure-ban. Általában az ügyfelek késés csökkentése és a legjobb teljesítmény elérése érdekében lehetséges legközelebbi adatközpontokban az alkalmazás telepítéséhez.
Azonban előfordulhat, hogy választhat egy adatközpont közelebb vállalata vagy az adatok közelebb néhány joghatósági vagy jogi kétségei az adatok és a, amely tartalmazza. Nincsenek alkalmas az alkalmazás kódja szolgáltatója körül a földgömb hat adatközpontokban. Az alábbi táblázat mutatja az elérhető helyek:

<table border="2" cellspacing="0" cellpadding="5" style="border: 2px solid #000000;">
<tbody>
<tr>
<td style="width: 100px;">
**Ország/régió**

</td>
<td style="width: 200px;">
**Körzetek**

</td>
</tr>
<tr>
<td>
Egyesült Államok

</td>
<td>
Dél-, közép- és Észak-központi

</td>
</tr>
<tr>
<td>
Európa

</td>
<td>
Észak- és a nyugati

</td>
</tr>
<tr>
<td>
Ázsia

</td>
<td>
Délkelet és keleti

</td>
</tr>
</tbody>
</table>
A szolgáltatott szolgáltatás létrehozásakor, amelyben a kód végrehajtani kívánt helyét jelző körzet jelölje ki.

Jó elérhetőség és méretezhetőség eléréséhez fontos kritikus, hogy az alkalmazás adatokat egy központi tárban tárolnak elérhető több szerepkör példányaiban kell tartani. Segíti az adatokkal, Azure több tárterületet lehetőséget kínál például BLOB, táblázatok és SQL-adatbázishoz. Olvassa el az [Adatokat tároló ajánlataiban Azure-ban][] cikkünket e tároló technológiák további információt. Az alábbi ábrán látható, hogyan a terheléselosztó belül az Azure adatközpont elosztja a másik szerepkör-példányok, ami fér hozzá az azonos adattárolás kéréseket.

![kép][3]

Általában azt szeretné, keresse meg az alkalmazás kódját, és az adatok az azonos adatközpontban, így az alacsony időtartama (jobb teljesítményt) amikor az alkalmazás kódja az adatok segítségével érheti el. Ezeken kívül nem az előfizetést terhelő sávszélesség áthelyezésekor adatok az azonos adatközpont belül.

## <a id="scale"> </a>Az alkalmazás a skála tervezése

Időnként előfordulhat érdemes egyetlen alkalmazás (például egy egyszerű webhely) készítése és Azure-ban is. De gyakran, az alkalmazás állhat, több szerepkörök, hogy az összes közös munka. Ha például az alábbi ábrán vannak a webhely szerepkör két példánya, a feldolgozási sorrendben szerepkör három példányok és a jelentéskészítő szerepkör egy példánya. Ezeket a szerepköröket összes dolgozik együtt, és az összeset kódját együttes, és Azure felfelé egységként rendszerbe.

![kép][4]

A fő felosztása egy alkalmazást különféle szerepköröket be minden futó saját (Ez azt jelenti, hogy VMs) szerepkör-példányok csoportja azért független méretezni a szerepköröket. Például a téli ünnepek során vevőknek is kell vásárlásával termékek a vállalaton belüli, érdemes lehet szerepkör példánya fut, a webhely szerepkört, valamint a feldolgozási sorrendben szerepkört futtató szerepkör-példányok száma számának növelése. A téli ünnepek után sok termékek ad vissza, így előfordulhat, hogy továbbra is kell webhely-példányok, de kevesebb rendelések feldolgozása példányok sok jelenhetnek meg. Az évet a többi során csak szükség lehet néhány webhely és a feldolgozási sorrendben példányok. Egész mindezek csak egy jelentéskészítő példány szükség lehet. Szerepköralapú telepítések Azure-ban rugalmasan lehetővé teszi, hogy könnyen alkalmazkodás az üzleti igények az alkalmazást.

Közös belül a szolgáltatott szolgáltatásban példányok egymással szerepköre. Például a webhely szerepkört, fogadja el a vevői rendelés, de majd azt offloads a sorrendben, a feldolgozási sorrendben szerepkör példányaiban feldolgozása. A legjobb módszer a példányok csoportja egy másik szerepkör-példányokat az egyik készlete van a technológiával várólista Azure-által biztosított munka űrlap átadni szolgáltatás Bus sorban várakozó vagy várólista szolgáltatást. Egy várólista használata a szövegegység itt kritikus része. A várakozási sorban található lehetővé teszi, hogy a szolgáltatott szolgáltatást, ha át kívánja méretezni a szerepkörök, egymástól függetlenül lehetővé teszi munkaterhelésének elleni költségét. Ha a várakozási sorban található üzenetek száma növekszik, az idő, majd méretezheti felfelé a feldolgozási sorrendben szerepkör példányainak száma. Ha idővel csökken a várakozási sorban található üzenetek számát, majd méretezheti lefelé a feldolgozási sorrendben szerepkör példányainak száma. Ezzel a módszerrel is csak fizet a tényleges terhelését kezeléséhez szükséges előfordulását.

A várakozási sorban található a megbízhatóság is tartalmaz. Rendelések feldolgozása szerepkör-példányok száma lefelé méretezésekor Azure úgy dönt, hogy melyik példányok befejezéséhez. Dönthet, amely egy várólista üzenet feldolgozása közepén egy példány befejezéséhez. Jó helyen jár az üzenetek feldolgozása nem jár teljes sikerrel, mert az üzenet láthatóvá válik a ismét a másik rendelések feldolgozása szerepkör példány felveszi azt, és dolgozza fel. Várólista üzenet láthatóság miatt üzenetek garantált feldolgozott idővel első. A sor egyben egy terheléselosztó azáltal, hogy hatékony az üzeneteket, hogy az üzenetek kérése minden szerepkör példányaiban.

A webhely szerepkör-példányok figyelheti a beillesztést érkező forgalmat és döntse el, ha át kívánja méretezni a számát felfelé vagy lefelé, valamint. A várakozási sorban található lehetővé teszi, hogy a webhely szerepkör példányainak függetlenül a feldolgozási sorrendben szerepkör-példányok száma méretezheti. Ez nagyon nagy teljesítményű és sok rugalmasságot biztosít. Természetesen Ha az alkalmazás további szerepkörök áll, hozzáadhatja további sorok kommunikáció közöttük kihasználhatja az azonos méretezése és a költség előnyökkel jár a továbbítás.

## <a id="defandcfg"> </a>Üzemeltetett definiált szolgáltatás konfigurálása

Azure szolgáltatott szolgáltatás telepítése szükséges, hogy a szolgáltatás-definíciós fájl és a szolgáltatás konfigurációs fájl. Ezek a fájlok mindkettő XML-fájl, és lehetővé teszik, hogy deklaratív adja meg a szolgáltatott szolgáltatásbeli telepítési lehetőségek. A szolgáltatás-definíciós fájl összes a szerepkör a szolgáltatott szolgáltatásban, és hogyan kommunikáció alkotó ismerteti. A szolgáltatás konfigurációs fájl ismerteti az egyes szerepkör és a beállításokat, amelyek minden szerepkör-példány konfigurálása példányok száma.

## <a id="def"> </a>a szolgáltatás-definíciós fájl

Amint lehet már említettük, a szolgáltatás definition (CSDEF) a fájl XML-fájlban a különféle szerepköröket, a teljes alkalmazást alkotó leíró. A teljes sémát az XML-fájl megtalálható itt: [http://msdn.microsoft.com/library/windowsazure/ee758711.aspx], [].
A CSDEF fájlt, amelyet az alkalmazás minden szerepkörhöz WebRole vagy WorkerRole elemet tartalmaz. Üzembe helyezése a szerepkör a webes szerep (a WebRole elem segítségével), az azt jelenti, hogy a kód egy szerepkör-példányt, amelyben a Windows Server 2008 és Internet Information Server (IIS) fog futni.
Szerepkör telepítése, mint (a WorkerRole elem segítségével) dolgozó szerepkörbe azt jelenti, hogy a szerepkör-példány lesz a Windows Server 2008 rajta (IIS nem lehetett telepíteni).

Bizonyára létrehozhat és üzembe figyelni a bejövő webes kérések néhány egyéb mechanizmusa használó dolgozó szerepkörbe (például a kód sikerült létrehozása és használata a .NET HttpListener). Mivel a szerepkör példánya fut az összes Windows Server 2008, a kód műveleteket végezheti el minden a szokásos módon használható az alkalmazás Windows Server rendszeren futó
2008.

Minden szerepkörhöz akkor ezzel azt jelzi, hogy az adott szerepkör példányok kell használni kívánt virtuális méretét. Az alábbi táblázatban látható kapható különböző virtuális méretű és az egyes attribútumait:

<table border="2" cellspacing="0" cellpadding="5" style="border: 2px solid #000000;">
<tbody>
<tr align="left" valign="top">
<td>
**Virtuális mérete**

</td>
<td>
**PROCESSZOR**

</td>
<td>
**RAM**

</td>
<td>
**Merevlemez**

</td>
<td>
**Csúcs hálózati I/O**

</td>
</tr>
<tr align="left" valign="top">
<td>
**Kis extra**

</td>
<td>
1 x 1.0 GHz-es

</td>
<td>
768 MB

</td>
<td>
20GB

</td>
<td>
\~5 MB

</td>
</tr>
<tr align="left" valign="top">
<td>
**Kis**

</td>
<td>
1 x 1,6 GHz-es

</td>
<td>
1.75 GB

</td>
<td>
225GB

</td>
<td>
\~100 MB

</td>
</tr>
<tr align="left" valign="top">
<td>
**Közepes**

</td>
<td>
2 x 1,6 GHz-es

</td>
<td>
3,5 GB

</td>
<td>
490GB

</td>
<td>
\~200 MB

</td>
</tr>
<tr align="left" valign="top">
<td>
**Nagy**

</td>
<td>
4 x 1,6 GHz-es

</td>
<td>
7 GB

</td>
<td>
1TB

</td>
<td>
\~400 MB

</td>
</tr>
<tr align="left" valign="top">
<td>
**Nagyon nagy**

</td>
<td>
8 x 1,6 GHz-es

</td>
<td>
14 GB

</td>
<td>
2TB

</td>
<td>
\~800 MB

</td>
</tr>
</tbody>
</table>
Az előfizetést terhelő óránként minden virtuális szerepkör példányt használja, és is az előfizetést terhelő adatokról, a szerepkör-példányok küldése a kívül az Adatközpont. Nem fel az adatok beírása az Adatközpont. További tudnivalókért lásd: [[Azure ár]]. Az általános célszerű használni a sok kis szerepkör-példány nem néhány nagy példányok pedig úgy, hogy az alkalmazás rugalmasabb sikertelen az. Ha minden az kevesebb szerepkör-példányok van, a hiba az egyiket a további katasztrofális az általános alkalmazás. Is említett előtt, telepítenie kell legalább két példánya minden szerepkörhöz ahhoz, hogy a 99.95 % szolgáltatásiszint-szerződés a Microsoft biztosít.

A szolgáltatás definition (CSDEF) fájl is tenné megadhatja a sok attribútumok egyes szerepkörök az alkalmazásban. Íme néhány további hasznos elemek elérhető:

-   **Tanúsítványok**. Tanúsítványok az adatokat, vagy a webszolgáltatás támogatja-e az SSL titkosítása használhatja. Bármely tanúsítványokat kell Azure fel kell tölteni. További tudnivalókért olvassa el a [Tanúsítványok kezelése az Azure-ban][]című témakört. XML-beállítás és a telepítést korábban feltöltött tanúsítványok az szerepkör-példányt tanúsítvány tárolóba úgy, hogy az alkalmazás kóddal használható legyenek.

-   **Konfigurációs beállítás nevét**. Az értékek, amelyet az alkalmazásokat, a szerepkör-példány futtatása közben olvasható. A tényleges érték a konfigurációs beállítások értéke a szolgáltatás (CSCFG) konfigurációs fájl, amely anélkül, hogy a kód telepítsen újra bármikor frissíthetők. Erre valójában is kód oly módon, a módosított konfigurációs értékek feltárása bármely állásidőt nélkül az alkalmazások számára.

-   A **beviteli a végpontok**. Itt adhatja meg bármely HTTP, HTTPS vagy TCP végpontokat (és portokat) kattintva jelenítse meg a külső világ keresztül az *előtagot*, amelyet. cloadapp.net URL-CÍMÉT. Azure üzembe helyezése a szerepkört, amikor konfigurálja a tűzfal szerepkör példányon automatikusan.

-   **Belső végpontok**. Itt adhatja meg bármely HTTP vagy TCP-végpontok, amelyet csak akkor érhető el a többi szerepkör példányát, melyek az alkalmazás részeként. Belső végpontok lehetővé teszi a egymással beszélgetni alkalmazáson belül szerepkör-példányok, de nem érhetők el minden szerepkör példányaiban, amely kívül esik az alkalmazás.

-   **Importálás modulokat**. Ezek tetszés szerint a szerepkör-példányok hasznos összetevők telepítése. Összetevők léteznek diagnosztikai felügyeletet igényel, távoli asztali és Azure csatlakozás (amely lehetővé teszi, hogy a helyszíni erőforrások eléréséhez egy biztonságos csatornán keresztül szerepkör-példány).

-   **Helyi tároló**. Ez lefoglalja egy alkönyvtárába az szerepkör-példányt a használni kívánt alkalmazást. Részletesebb [Adatokat tároló ajánlataiban Azure-ban][] című témakörben leírtak.

-   **Indítási feladatokat**. Indítási feladatok ad kapcsolatban előzetesen szükséges összetevők telepítése egy szerepkör-példányt, mint azt feljebb betölti lehetőséget. A feladatok jogú rendszergazdaként, ha szükséges is futtathatók lesznek.

## <a id="cfg"> </a>a szolgáltatás konfigurációs fájl

Service (CSCFG) konfigurációs fájl ismerteti a beállításokat, amelyekkel módosítható, anélkül, hogy az alkalmazás újbóli XML-fájlt. A teljes sémát az XML-fájl megtalálható itt: [http://msdn.microsoft.com/library/windowsazure/ee758710.aspx][].
A CSCFG fájlt az alkalmazás minden szerepkörhöz szerepkör elemet tartalmaz. Íme néhány az elemeket, megadhatja, hogy a CSCFG fájl:

-   **Operációs rendszer verziója**. Az attribútum jelölje ki a operációs rendszeren verziót használja, az alkalmazás kód futtatását szerepkör előfordulását teszi lehetővé. A *vendégként való bekapcsolódáshoz OS*terület az operációs rendszer, és minden új verziója, a legújabb biztonsági javításokat és kezdésének az operációs rendszer vendégként megjelent az elérhető frissítések tartalmazza. Ha a osVersion attribútumérték "\*", majd az Azure automatikusan frissíti a vendégként való bekapcsolódáshoz OS egyes a szerepkör-példányok, amint elérhetővé válnak a új Vendég-OS verzió. Azonban választhatja ki az automatikus frissítések operációs rendszer adott vendégként kiválasztásával. Például a értékre állítja, a osVersion attribútum "Postafiók-VENDÉG-OS-2,8\_201109-01" okoz a szerepkör példányait megszerezni, ez az weblapon leírtaktól: [http://msdn.microsoft.com/library/hh560567.aspx][]. Vendég-OS verzió kapcsolatos további tudnivalókért olvassa el a [Frissítéseit kezelni az Azure vendégek OS]című témakört.

-   **Példányok**. Ez az elem érték azt jelzi, hogy kiépített az adott jogosultság a kódot futtató kívánt szerepkör-példányok száma. Új CSCFG fájl (nélkül, az alkalmazás újbóli) Azure is feltölthet, mivel található trivially egyszerűen módosítsa az értéket, az elemhez, és töltse fel egy új CSCFG fájlt dinamikusan növelheti vagy csökkentheti az alkalmazás kód futtatását szerepkör-példányok száma. Ez lehetővé teszi, hogy az alkalmazás egyszerűen méretezni, felfelé vagy lefelé közben is szabályozása mennyi az előfizetést terhelő a szerepkör példánya fut találkozik tényleges terhelést igényeivel.

-   **Konfigurációs beállítás értékét**. Ez az elem beállítások értékeit azt jelzi, (a CSDEF fájlban meghatározott). A szerepkör a futása közben is olvashatja ezeket az értékeket. Ezeket a konfigurációs beállítások értékeket érték jellemzően a kapcsolati karakterláncot vagy tárolóhoz Azure SQL-adatbázishoz, de minden kívánt célra használható.

## <a id="hostedservices"> </a>Hozhat létre és szolgáltatott szolgáltatás üzembe helyezése

A szolgáltatott szolgáltatás létrehozásához, hogy először az [Azure Kezelőportálja] és szolgáltatott szolgáltatás kiépítése DNS előtaggal megadásával és az Adatközpont végül szeretné, a kód futtatása. Kattintson a fejlesztői környezet, a szolgáltatás definition (CSDEF) fájl létrehozása, az alkalmazás kódja és csomag (zip), mind a fájlok összeállítása szolgáltatás csomag (CSPKG) fájlba. A szolgáltatás beállítása (CSCFG) fájl kell készítenie is. A szerepkör telepítéséhez töltse fel a CSPKG és CSCFG fájlokat a Azure szolgáltatás felügyeleti API-val. Rendszerbe, Azure, miután fog rendelkezni szerepkör példányok az Adatközpont (konfigurációs adatok alapján), bontsa ki az alkalmazás kódja a csomagból, bemásolhatja a szerepkör-példányok, és indítsa el a példányok. A kód ezt követően lépéseket.

Az alábbi ábrán a CSPKG és CSCFG fájlokat hoz létre, fejlesztési a számítógépen. A CSPKG fájlt tartalmazza a CSDEF fájlt, és két szerepkörök kódját. Miután fájlfeltöltéskor a CSPKG és CSCFG az Azure szolgáltatás felügyeleti API-val, Azure a szerepkör-példányok az Adatközpont hoz létre. Ebben a példában a CSCFG fájl jelzi, hogy Azure példányokat kell létrehoznia, három szerepkör \#szerepkör 1 és két példánya \#2.

![kép][5]

Telepítésével kapcsolatos további tudnivalókért verziójáról vált, és a szerepkörök, újbóli témakörben [üzembe helyezése és Azure-alkalmazások frissítése][] .<a id="Ref" name="Ref"></a>

## <a id="references"> </a>Hivatkozások

-   [Az Azure szolgáltatott szolgáltatás létrehozása][]

-   [Azure-ban megosztott szolgáltatások kezelése][]

-   [Azure áttelepítése alkalmazások][]

-   [Azure-alkalmazások konfigurálása][]

<div style="width: 700px; border-top: solid; margin-top: 5px; padding-top: 5px; border-top-width: 1px;">

<p>Írt Jeffrey Richter (Wintellect)</p>

</div>

  [Azure alkalmazás modell előnyeit]: #benefits
  [Szolgáltatott szolgáltatás Core – fogalmak]: #concepts
  [Szolgáltatott szolgáltatás tervezési szempontok]: #considerations
  [Az alkalmazás a skála tervezése]: #scale
  [Definiált szolgáltatott szolgáltatás és konfiguráció]: #defandcfg
  [A szolgáltatás-definíciós fájl]: #def
  [A szolgáltatás konfigurációs fájl]: #cfg
  [Hozhat létre és szolgáltatott szolgáltatás üzembe helyezése]: #hostedservices
  [Hivatkozások]: #references
  [0]: ./media/application-model/application-model-3.jpg
  [1]: ./media/application-model/application-model-4.jpg
  [2]: ./media/application-model/application-model-5.jpg
  [A saját tartománynév beállítása az Azure-ban]: http://www.windowsazure.com/develop/net/common-tasks/custom-dns/
  [Adatokat tároló ajánlataiban Azure-ban]: http://www.windowsazure.com/develop/net/fundamentals/cloud-storage/
  [3]: ./media/application-model/application-model-6.jpg
  [4]: ./media/application-model/application-model-7.jpg
  
  [Azure Pricing]: http://www.windowsazure.com/pricing/calculator/
  [Tanúsítványok az Azure kezelése]: http://msdn.microsoft.com/library/windowsazure/gg981929.aspx
  [http://msdn.microsoft.com/library/windowsazure/ee758710.aspx]: http://msdn.microsoft.com/library/windowsazure/ee758710.aspx
  [http://msdn.microsoft.com/library/hh560567.aspx]: http://msdn.microsoft.com/library/hh560567.aspx
  [Az Azure vendégek OS frissítéseit kezelése]: http://msdn.microsoft.com/library/ee924680.aspx
  [Azure Kezelőportálja segítségével]: http://manage.windowsazure.com/
  [5]: ./media/application-model/application-model-8.jpg
  [Telepítésével és Azure alkalmazások frissítése]: http://www.windowsazure.com/develop/net/fundamentals/deploying-applications/
  [Az Azure szolgáltatott szolgáltatás létrehozása]: http://msdn.microsoft.com/library/gg432967.aspx
  [Azure-ban megosztott szolgáltatások kezelése]: http://msdn.microsoft.com/library/gg433038.aspx
  [Azure áttelepítése alkalmazások]: http://msdn.microsoft.com/library/gg186051.aspx
  [Azure-alkalmazások konfigurálása]: http://msdn.microsoft.com/library/windowsazure/ee405486.aspx
