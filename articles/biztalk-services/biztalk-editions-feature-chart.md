<properties
    pageTitle="BizTalk szolgáltatások kiadásában funkciókról |} Microsoft Azure"
    description="A szolgáltatások BizTalk kiadásai funkcióinak összehasonlítása: szabad, fejlesztőeszközök, Basic, normál és prémium verzióban. MABS, WABS."
    services="biztalk-services"
    documentationCenter=""
    authors="MandiOhlinger"
    manager="erikre"
    editor=""/>

<tags
    ms.service="biztalk-services"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/15/2016"
    ms.author="mandia"/>


# <a name="biztalk-services-editions-chart"></a>BizTalk szolgáltatások: Kiadásai diagram

Azure BizTalk szolgáltatások több kiadásai kínál. Ez a cikk használatával azt állapíthatja meg, mely edition mindig eset és üzleti igényeinek megfelelően.


## <a name="compare-the-editions"></a>Az verziók összehasonlítása

**Ingyenes (előzetes verzió)**

Létrehozhat és hibrid kezelni. Hibrid kapcsolat rendszerhez való csatlakozáshoz egy Azure webhelyet egy helyszíni, például SQL Server ugyanígy.

**Fejlesztőeszközök**

Hibrid kapcsolatok, a EAI és a szerkesztése üzenet feldolgozási egy kereskedelmi partner kezelőportálja segítségével könnyen használható, és a közös szerkesztése a sémák támogatása és a multimédiás szerkesztése feldolgozás tartalmazza X12 és AS2 felett. A felhőben szolgáltatások összekapcsolása a HTTP/S, a többi, FTP, WCF és SFTP jegyzőkönyvek írási és olvasási üzenetek tipikus esetei EAI hozhat létre.  Kapcsolat a helyszíni üzleti rendszerek csatlakozást használatra kész SAP, az Oracle gazdaságot, az Oracle DB, a Siebel és az SQL Server kártyával. Visual Studio eszközök egyszerűen fejlesztés és üzembe egy oldalközpontú fejlesztői környezet használata. Fejlesztés és tesztelése céljából csak a nem szolgáltatási szerződés SZOLGÁLTATÁSISZINT-korlátozva.

**Egyszerű**

A Fejlesztőeszközök funkciók növeli a legtöbb hibrid kapcsolatok, EAI hidakat, rendelkezést szerkesztése és BizTalk kártya csomag kapcsolatok tartalmazza. Magas elérhetőségét, és a lehetőséget, ha át kívánja méretezni, egy szolgáltatási szerződés SZOLGÁLTATÁSISZINT-is kínál.

**Normál**

Hibrid kapcsolatok, EAI hidakat, rendelkezést szerkesztése és BizTalk kártya csomag kapcsolatok növeli az egyszerű összes funkcióját tartalmazza. Magas elérhetőségét, és a lehetőséget, ha át kívánja méretezni, egy szolgáltatási szerződés SZOLGÁLTATÁSISZINT-is kínál.

**Támogatás**

Hibrid kapcsolatokat, EAI hidakat, rendelkezést szerkesztése és BizTalk kártya Pack kapcsolatok növeli a szabványos összes funkcióját tartalmazza. Az archiválás, magas rendelkezésre állásának és a lehetőséget, ha át kívánja méretezni, egy szolgáltatási szerződés SZOLGÁLTATÁSISZINT-is tartalmaz.


## <a name="editions-chart"></a>Kiadásai diagram
Az alábbi táblázat a különbségeket.

<table border="1">
<tr bgcolor="FAF9F9">
        <th></th>
        <th>Ingyenes (előzetes verzió)</th>
        <th>Fejlesztőeszközök</th>
        <th>Egyszerű</th>
        <th>Normál</th>
        <th>Támogatás</th>
</tr>

<tr>
<td><strong>Kezdő ár</strong></td>
<td colspan="5"><a HREF="http://go.microsoft.com/fwlink/p/?LinkID=304011"> Árak Azure BizTalk szolgáltatások</a> <br/><br/> <a HREF="http://azure.microsoft.com/pricing/calculator/?scenario=full">Számológép árak Azure</a></td>
</tr>
<tr>
<td><strong>Alapértelmezett minimális konfiguráció</strong></td>
<td>1 ingyenes egység</td>
<td>1 Fejlesztőeszközök egység</td>
<td>1 alapegység</td>
<td>1 szabványos egység</td>
<td>1 prémium egység</td>
</tr>
<tr>
<td><strong>Méretarány</strong></td>
<td>A méretezés nélkül</td>
<td>A méretezés nélkül</td>
<td>Igen, 1 alapegység lépésekben</td>
<td>Igen, 1 szabványos egység lépésekben</td>
<td>Igen, 1 prémium egység lépésekben</td>
</tr>
<tr>
<td><strong>Megengedett legnagyobb skála meg</strong></td>
<td>A méretezés nélkül</td>
<td>A méretezés nélkül</td>
<td>Legfeljebb 8 egység</td>
<td>Legfeljebb 8 egység</td>
<td>Legfeljebb 8 egység</td>
</tr>
<tr>
<td><strong>EAI hidakat egységnyi</strong></td>
<td>Nem része</td>
<td>25</td>
<td>25</td>
<td>125</td>
<td>500</td>
</tr>
<tr>
<td><strong>AS2 SZERKESZTÉSE</strong>
<br/><br/>
TPM rendelkezést tartalmazza.</td>
<td>Nem számít bele</td>
<td>Tartalmazza. 10 rendelkezést egységre.</td>
<td>Tartalmazza. 50 rendelkezést egységre.</td>
<td>Tartalmazza. 250 rendelkezést egységre.</td>
<td>Tartalmazza. 1000 rendelkezést egységre.</td>
</tr>
<tr>
<td><strong>Hibrid kapcsolatok egységnyi</strong></td>
<td>5</td>
<td>5</td>
<td>10</td>
<td>50</td>
<td>100</td>
</tr>
<tr>
<td><strong>Hibrid kapcsolat adatátvitel (GB) egységnyi</strong></td>
<td>5</td>
<td>5</td>
<td>50</td>
<td>250</td>
<td>500</td>
</tr>
<tr>
<td><strong>A helyszíni üzleti rendszerek BizTalk kártya szolgáltatás kapcsolatok</strong></td>
<td>Nem része</td>
<td>1 kapcsolat</td>
<td>2 kapcsolatok</td>
<td>5 kapcsolatok</td>
<td>a kapcsolatok 25</td>
</tr>
<tr>
<td align="left"><strong>Támogatott protokollok rendszerek:</strong>
<ul>
<li>HTTP</li>
<li>HTTPS</li>
<li>FTP</li>
<li>SFTP</li>
<li>WCF</li>
<li>Szolgáltatás Bus (SB)</li>
<li>Azure Blob</li>
<li>REST API-hoz</li>
</ul>
</td>
<td>Nem része</td>
<td>Része</td>
<td>Része</td>
<td>Része</td>
<td>Része</td>
</tr>
<tr>
<td><strong>Magas elérhetősége</strong>
<br/><br/>
A szolgáltatási szerződés SZOLGÁLTATÁSISZINT-, lásd: <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=304011">Azure BizTalk szolgáltatások árak</a>.
</td>
<td>Nem része</td>
<td>Nem számít bele</td>
<td>Része</td>
<td>Része</td>
<td>Része</td>
</tr>
<tr>
<td><strong>Biztonsági mentési és visszaállítási</strong></td>
<td>Nem része</td>
<td>Része</td>
<td>Része</td>
<td>Része</td>
<td>Része</td>
</tr>
<tr>
<td><strong>A nyomon követés</strong></td>
<td>Nem része</td>
<td>Része</td>
<td>Része</td>
<td>Része</td>
<td>Része</td>
</tr>
<tr>
<td><strong>Az Archiválás</strong><br/><br/>
Tartalmazza a nem hitelesítettek nyugta (NRR) és a nyomon követett üzenetek letöltése</td>
<td>Nem része</td>
<td>Része</td>
<td>Nem számít bele</td>
<td>Nem számít bele</td>
<td>Része</td>
</tr>
<tr>
<td><strong>Egyéni kód használata</strong></td>
<td>Nem része</td>
<td>Része</td>
<td>Része</td>
<td>Része</td>
<td>Része</td>
</tr>
<tr>
<td><strong>A átalakítások, beleértve az egyéni XSLT használata</strong></td>
<td>Nem része</td>
<td>Része</td>
<td>Része</td>
<td>Része</td>
<td>Része</td>
</tr>
</table>

> [AZURE.NOTE] Tűrőképessége hardver hibák ellen nagy elérhetősége azt jelenti, egy BizTalk egységbe belül több VMs problémákat.


## <a name="faqs"></a>Gyakori kérdések

#### <a name="what-is-a-biztalk-unit"></a>Mi az a BizTalk egység?
A "" mértékegysége-Azure BizTalk Services-példányhoz atomi szintjét. Minden egyes edition, amelynek a különböző számítási beosztását, és a memóriában időegység megtalálható. Például egy egyszerű egység Fejlesztőeszközök-nél több számítási rendelkezik, normál még több mint egyszerű számítási, és így tovább. Ha egy BizTalk szolgáltatás méretezéséhez, méretezze át egységek számát tekintve.

#### <a name="what-is-the-difference-between-biztalk-services-and-azure-biztalk-vm"></a>Mi a különbség BizTalk Services és az Azure BizTalk virtuális között?
BizTalk szolgáltatások egy true Platform-mint-a-szolgáltatási (PaaS) architektúrája biztosít a felhőben integrációs megoldások fejlesztésére. A PaaS modellel teljesen koncentráljon az alkalmazás logika, és hagyja az összes infrastruktúra kezelése a Microsoftnak, például:

- Nincs szükség, kezelése és javítás virtuális gépeken futó.
- A Microsoft elérhetősége biztosítja.
- Megadásával szabályozhatja, hogy a méretezés igény szerinti igénylésével egyszerűen több vagy kevesebb kapacitás az Azure portálon keresztül.

BizTalk Server Azure virtuális gépeken futó a infrastruktúra-mint-a-szolgáltatás (IaaS) architektúrája biztosít. A létrehozott virtuális gépeken futó és őket pontosan a helyszíni környezet, például a könnyebb olvashatóság érdekében meglévő alkalmazások futtatásához a felhőben, nincs kód módosításokkal. A IaaS akkor felelősek továbbra is konfigurálása a virtuális gépeken futó kezelése a virtuális gépeken futó (például a szoftverek telepítése és OS javítások) és az alkalmazás magas rendelkezésre állásának architecting.

Ha készíteni, amely az infrastruktúra-kezelés munkamennyiség összezárása új integrációs-megoldások fejlesztésére, BizTalk szolgáltatások használata Ha szeretné, hogy a meglévő BizTalk megoldások gyors áttelepíteni, vagy igény szerinti környezetben, valamint BizTalk kiszolgáló alkalmazások tesztelése keres, használja az Azure virtuális gép BizTalk kiszolgálón.

#### <a name="what-is-the-difference-between-biztalk-adapter-service-and-hybrid-connections"></a>Mi a különbség BizTalk kártya szolgáltatás és a hibrid kapcsolatok között?
A BizTalk kártya szolgáltatás az Azure BizTalk szolgáltatásainak használják. A BizTalk kártya szolgáltatás a BizTalk kártya csomagot használ, egy helyszíni sor az üzleti TEVÉKENYSÉGFOLYAM rendszerhez való csatlakozáshoz. Hibrid kapcsolat erőforráshoz való kapcsolódáshoz Azure alkalmazások, a Web Apps alkalmazások funkció az Azure alkalmazás szolgáltatás és az Azure Mobile szolgáltatások, például egy helyszíni egyszerű és kényelmes lehetőséget nyújt.

#### <a name="what-does-hybrid-connection-data-transfer-gb-per-unit-mean-is-this-per-minutehourdayweekmonth-what-happens-when-the-limit-is-reached"></a>Mit jelent az "Hibrid kapcsolat adatátvitel (GB) egységnyi"? A perc/óra vagy nap/hét/hónap / található ez? Mi történik, ha a letelik?

Hibrid kapcsolat egységár attól függ, hogy a BizTalk Services Editiont. Költségek függő egyszerűen helyezi el, hogy mennyi adatot a át. Például 10 GB adatot naponta átvitele költségek kisebb, mint a napi átvitele 100 GB. Az [Ár Számológép](https://azure.microsoft.com/pricing/calculator/?scenario=full) BizTalk szolgáltatások segítségével határozza meg a adott költségeket. Általában a vannak korlátozva naponta. Ha egy művelet túllépi a korlátot, bármilyen hozzárendelések alapdíjával díjazott a $ 1 GB /.

#### <a name="when-i-create-an-agreement-in-biztalk-services-why-does-the-number-of-bridges-go-up-by-two-instead-of-just-one"></a>Hozhatok létre olyan megállapodást BizTalk szolgáltatásokban, amikor miért nem hidakat számát a részletezésből által két csak egy helyett?

Két különböző hidakat tárgyból minden szerződés: a Küldés egymás kommunikációs híd és a kommunikáció hidat fogadási egymás.

####  <a name="what-happens-when-i-hit-the-quota-limit-on-the-number-of-bridges-or-agreements"></a>Mi történik, amikor e találati kvóta korlát a hidakat vagy rendelkezést?

Ön nem tudja üzembe minden új hidakat, vagy hozzon létre a minden új rendelkezést. További üzembe helyezéséhez kell további egységek a BizTalk szolgáltatás méretezni, vagy egy újabb kiadására frissítése.

#### <a name="how-do-i-migrate-from-one-tier-of-biztalk-services-to-another"></a>Hogyan tudom áttelepíteni az egy réteg BizTalk szolgáltatások között?

Az ingyenes edition nem lehet áttelepített vagy "méretezni", hogy egy másik réteg, és nem kell a biztonsági mentésben és egy másik vissza. Ha egy másik, hozzon létre egy új BizTalk az új réteg használatával. Minden olyan hibrid kapcsolatok, például az ingyenes edition használatával létrehozott eltérések kell újra létre kell hozni az új BizTalk szolgáltatásban. 

A hátralévő kiadás használata a biztonsági mentés és visszaállítása a eltérések áttérés az egy réteg között. Például biztonsági mentése az eltérések a szabványos réteg, és visszaállításukról a prémium réteg. [BizTalk szolgáltatások: biztonsági mentési és visszaállítási](biztalk-backup-restore.md) a támogatott áttelepítési elérési utak ismerteti, és megjeleníti, hogy milyen eltérések biztonsági másolat készül. Látható, hogy a hibrid kapcsolatok nem készített biztonsági másolatot. Biztonsági mentése és visszaállítása egy új réteget, után, majd hozza létre újból a hibrid kapcsolatokat.  


#### <a name="is-the-biztalk-adapter-service-included-in-the-service-how-do-i-receive-the-software"></a>Az a BizTalk kártya szolgáltatás szolgáltatás? Hogyan jelenik meg a szoftver?

Igen, a BizTalk kártya szolgáltatás a BizTalk kártya csomaggal is megjelennek az Azure BizTalk szolgáltatások SDK [letöltése](http://www.microsoft.com/download/details.aspx?id=39087).

## <a name="next-steps"></a>Következő lépések

Azure BizTalk Services létrehozása az Azure-portálon, használja a [BizTalk szolgáltatások: az Azure portálon kiépítési](biztalk-provision-services.md). Indítsa el az alkalmazások létrehozása, keresse fel az [Azure BizTalk szolgáltatások](http://go.microsoft.com/fwlink/p/?LinkID=235197).

## <a name="additional-resources"></a>További források
- [BizTalk szolgáltatások: Kiépítési az Azure portál használatával](biztalk-provision-services.md)<br/>
- [BizTalk szolgáltatások: Állapot diagram kiépítése](biztalk-service-state-chart.md)<br/>
- [BizTalk szolgáltatások: Irányítópult, a Monitor és a méret lapon](biztalk-dashboard-monitor-scale-tabs.md)<br/>
- [BizTalk szolgáltatások: Biztonsági mentése és visszaállítása](biztalk-backup-restore.md)<br/>
- [BizTalk szolgáltatások: szabályozása](biztalk-throttling-thresholds.md)<br/>
- [BizTalk szolgáltatások: Kibocsátó neve és a kibocsátó billentyűk](biztalk-issuer-name-issuer-key.md)<br/>
- [Hogyan tudom használatával indítsa el az Azure BizTalk szolgáltatások SDK](http://go.microsoft.com/fwlink/p/?LinkID=302335)<br/>
