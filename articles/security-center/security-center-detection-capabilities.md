<properties
   pageTitle="Az Azure biztonság otthon észlelés |} Microsoft Azure"
   description="A dokumentum segítségével Azure biztonság otthon észlelés működésének megértése."
   services="security-center"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor=""/>

<tags
   ms.service="security-center"
   ms.topic="hero-article"
   ms.devlang="na"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/22/2016"
   ms.author="yurid"/>

# <a name="azure-security-center-detection-capabilities"></a>Azure biztonság otthon észlelés
A dokumentum azt ismerteti, hogy az Azure biztonság otthon speciális észlelés, amely segít a Microsoft Azure erőforrások kiválasztásával aktív kockázatok azonosítása és biztosít a hírcsatornájában, gyorsan reagálni szükséges.

> [AZURE.NOTE] A szokásos réteg az Azure biztonság otthon speciális észlelési érhetők el. A 90 napig ingyenes próbaverziója érhető el. A [Yammer biztonsági házirendje](security-center-policies.md)árak réteg kijelölésből frissíthet. Keresse fel a [Biztonság otthon lap](https://azure.microsoft.com/pricing/details/security-center/) árak olvashat. 


## <a name="responding-to-todays-threats"></a>Válasz az aktuális veszélyek
Eddig jelentős módosításokat a kockázatot fekvő a az utolsó 20 évek során. Az elmúlt cégek általában csak kellett webhely felülírása egyes támadók, akik főleg megnéz "sikerült hatása olvasható" érdekli aggódnia. Az aktuális támadók sokkal több kifinomult és rendszerezheti. Gyakran rendelkeznek az adott pénzügyi és stratégiai célok. Azok is lehet őket, további források, előfordulhat, hogy nemzeti államokból vagy szervezett bűnözés által támogatott őket.

Ezt a megközelítést vezetett egy szakmai jelleg erősítése a támadó az orderby_expression a eddiginél szintjét. Már nem azok érdekli a webes felülírása. Most már vannak iránt érdeklődik ellopásának megakadályozására az információkat, a pénzügyi fiókok és a személyes adatok –, ami használhatják a nyílt piacon pénzbeli létrehozásához vagy egy adott üzleti, a politikai vagy katonai pozíció használhatja. Még több vonatkozó mint ezeket a támadók, azzal a céllal, pénzügyi a támadók, akik infrastruktúra és a személyek sértheti hálózatok megsértése.

Válasz, a szervezetek gyakran koncentráljon védelme a szervezet határain vagy a végpontok által ismert támadások aláírások keres, amelyek különböző pont megoldások üzembe helyezése. Ezek a megoldások általában egy biztonsági elemző-mailjei, és azt vizsgálja, hogy csak alacsony funkcióvesztést riasztások nagy mennyiségű létrehozásához. A legtöbb szervezet az időt és a szükséges alábbi szóló jelzés kezelésével – szakértelmét sok Ugrás címzés nélküli helyét.  Eközben a támadók a módszereket, amelyekkel számos aláírás-alapú védelmet és [felhőalapú környezetben alkalmazkodik](https://azure.microsoft.com/blog/detecting-threats-with-azure-security-center/)subvert lett hatékonyabb. Új módszerek szükségesek gyorsabban felmerülő kockázatok azonosítása és felgyorsításához észlelése és választ. 

## <a name="how-azure-security-center-detects-and-responds-to-threats"></a>Hogyan Azure biztonság otthon észleli és veszélyek válaszol

A Microsoft biztonsági elemzők folyamatosan veszélyek a figyelni a vannak. A Microsoft globális jelenléti a felhőben, és a helyszíni nyert telemetriai egy kiterjedtnek halmazára hozzáféréssel rendelkeznek. A wide elérése és különböző gyűjtemény adatkészletek lehetővé teszi a Microsoft fel új homonimaszerű webcímmel szabályszerűségeket és trendeket helyszíni fogyasztói és vállalati termékeit, valamint az online szolgáltatások keresztül. Emiatt biztonság otthon is gyorsan az észlelési algoritmus szerint frissítse a támadók, engedje fel az új és egyre összetett hasznos. Ezt a megközelítést segít gyors mozgó veszélyt környezettel rendelkező lépést tartani. 

Biztonság otthon veszélyt észlelése automatikusan biztonsági információk összegyűjtése az Azure erőforrások, a hálózati és a csatlakoztatott partnermegoldások működik. Megvizsgálja ezt az információt, gyakran használatával történik a kockázatok azonosítása, több forrásból származó információkat. Biztonsági figyelmeztetések a biztonsági központban vannak rangsorolt arra vonatkozó javaslatokat a veszélyt ismételt együtt.

![Biztonság otthon adatok gyűjtése és bemutatása](./media/security-center-detection-capabilities/security-center-detection-capabilities-fig1.png)

Biztonság otthon speciális biztonsági analytics, amely távol meghaladják az aláírás-alapú megközelítés alkalmaz. A nagy adatok és -technológiák [gépi tanulási](https://azure.microsoft.com/blog/machine-learning-in-azure-security-center/) eredményeket kapcsolatos vannak károkozásra át a teljes felhő háló – észlelését veszélyek, amely nem lehetséges kézi az alábbi módszerek használ, és támadások alakulása előrejelzésére azonosítani a események ki szeretné számítani. A biztonsági analytics a következők: 

- **Integrált veszélyt intelligencia**: globális veszélyt intelligenciával kapcsolatos funkciók a Microsoft-termékek és szolgáltatások, a Microsoft digitális bűncselekmények egység (DCU), a Microsoft biztonsági válasz központ (Kialakításában), és külső hírcsatornák vizsgál által ismert – hibás szereplők keres.
- **Viselkedési analytics**: ismert mintázatok rosszindulatú viselkedés felderítésére vonatkozik. 
- **Rendellenességet észlelési**: alapértéknek historikus létrehozásához a statisztikai adatainak összegyűjtése használ. Riasztást jelenít meg, amelyek megfelelnek a potenciális homonimaszerű webcímmel vektornak létrehozott alaptervek eltéréseket.


### <a name="threat-intelligence"></a>Veszélyt intelligenciával kapcsolatos funkciók
Microsoft tartalmaz a globális veszélyt üzletiintelligencia-belevágni összege. Telemetriai folyik át a több forrásból származó, például Azure, Office 365-ben, a Microsoft CRM online, a Microsoft Dynamics AX, outlook.com, MSN.com, a Microsoft digitális bűncselekmények egység (DCU) és a Microsoft biztonsági válasz központ (Kialakításában). Kutatók is tartalmaz, amelyek a fő felhő szolgáltatók között van osztva, és a külső féltől származó veszélyt üzletiintelligencia-hírcsatornák előfizetője veszélyt üzletiintelligencia-adatok. Azure biztonság otthon ezen információk segítségével figyelmeztet az ismert hibás szereplők kockázatok. Néhány példa:

- **Kimenő kommunikáció rosszindulatú IP-címre**: ismert botnet vagy valószínűleg darknet kimenő forgalmának azt jelzi, hogy az erőforrás napvilágra kerülhetett támadó azt kísérel meg végrehajtani parancs a rendszer vagy exfiltrate adatokat. Azure biztonság otthon hasonlítja össze a hálózati forgalmat a Microsoft globális veszélyt adatbázishoz, és figyelmezteti, ha azt észleli, hogy kapcsolatba lépni a kártékony IP-címet.

## <a name="behavioral-analytics"></a>Viselkedési analytics

Viselkedési analytics a elemzi, és miben más az adatok ismert mintázatok gyűjteményének technika. Ezek a minták használata azonban nem egyszerű aláírások. A meghatározásuk összetett gépi tanulási nagy adathalmazok alkalmazott algoritmusok keresztül. Azok is határozzák keresztül rosszindulatú viselkedés ügyeljen elemzésének szakértői elemzők. Azure biztonság otthon viselkedési analytics segítségével virtuális gép naplók, virtuális hálózati eszköz naplók, háló naplók, összeomlik kiírása és más forrásokból elemzésének alapján biztonságos erőforrások azonosítása. 

Ezenkívül van más jeleket keressen egy kiterjedt marketingkampány igazolása támogatása a korrelációs. A korrelációs segít azonosítása követik a biztonságos létrehozott jelölők eseményeket. Néhány példa:

- **Suspicious folyamat végrehajtása**: támadók végrehajtása nélkül észlelési rosszindulatú szoftverek számos módszert alkalmazhat. Például támadó előfordulhat, hogy kártevőt jogos rendszerfájlok megegyező nevű ad, de ezek a fájlok elhelyezése egy másik helyre, adja meg, amely egy jó fájl nagyon hasonlít vagy igaz fájlkiterjesztés maszk. Biztonság otthon modellek folyamatok viselkedése és monitorok feltárása az alábbiakhoz kiugró végrehajtások feldolgozása.  
- **Rejtett kártevőt és kihasználására alkalmas megpróbálja**: összetett kártevőt alkalmas foganatosításával hagyományos antimalware termékek soha nem lemezre írása vagy lemezen tárolt szoftver összetevők titkosítja.  Azonban ilyen kártevőt észlelhető memória elemzésével, mint a kártevőt kell hagynia halad a memóriában ahhoz, hogy működik. Szoftver összeomlik, amikor egy összeomlik kiírása rögzíti memória egy részét a összeomlik idején.  A memória az összeomlást kiírása elemzésével Azure biztonság otthon észlelését technikákat kihasználhatja a szoftver biztonsági, bizalmas adatokat érhet el és rejtett továbbra is fennáll a beépülő modul segítségével egy biztonságos számítógépre anélkül, hogy a gép teljesítményét érintő.
- **Mozgás és belső reconnaissance oldalsó**: továbbra is fennáll, a biztonságos hálózati, és keresse meg és betakarítás értékes adatokat, hogy támadók gyakran próbálnak áthelyezése oldalirányban a biztonságos számítógépről másokkal ugyanazon a hálózaton belül. Biztonság otthon figyeli a folyamat, és jelentkezzen be a tevékenységek kattintva bontsa ki a támadó foothold belül a hálózat távoli parancs végrehajtása hálózati számlálása, például és a fiók számbavétel kísérletek felfedezése érdekében.
- **Rosszindulatú PowerShell-parancsfájlokat**: PowerShell végrehajtani a kártékony kódok a cél virtuális gépeken futó célra sokféle támadók használja. Biztonság otthon megvizsgálja PowerShell tevékenység gyanús tevékenység bizonyítékokra vonatkozóan. 
- **A kimenő támadások**: támadók gyakran Megcélozhat felhő erőforrások azokat az erőforrásokat használó további támadások csatlakoztatása a célt. Biztonságos virtuális gépeken futó, például tapasztalhatott ellen támadások elleni más virtuális gépeken futó indítása, a LEVÉLSZEMÉT küldése vagy a képet beolvasni a nyitott portokat és az internet más eszközökön. Gépi tanulási hálózati forgalmának engedélyezésére alkalmazásával biztonság otthon képes észlelni a hálózati kimenő kommunikáció-nál nagyobb használ. LEVÉLSZEMÉT, ha a biztonság otthon is hibához szokatlan e-mail forgalom az Office 365-ben állapítsa meg, hogy a levelezési valószínűleg intelligenciával nefarious vagy egy érvényes e-mail marketingkampány eredményét.  

### <a name="anomaly-detection"></a>Rendellenességet kimutatására

Azure biztonság otthon a kockázatok azonosítása rendellenességet észlelési is használja. A kontraszt viselkedési analytics (Ez attól függ, hogy a nagyméretű adatkészletek származás ismert mintázatok) rendellenességet észlelési további "személyre szabhatja" és a telepítések jellemző alaptervek koncentrál. Gépi tanulási megállapítása normál tevékenységet a telepítési alkalmazni, és kattintson a szabályok jelenthet biztonsági esemény kiugró feltételek megadásához jönnek létre. Lássunk egy példát:

- **Bejövő RDP/SSH biztonságosabb kényszerítése támadások**: A telepítések lehetnek elfoglalt virtuális gépeken futó bejelentkezések nagyszámú tartalmazó minden nap és más virtuális gépeken futó nagyon kevés rendelkező vagy bármely bejelentkezések. Azure biztonság otthon bejelentkezési tevékenység eredeti az alábbi virtuális gépeken futó meghatározására és gépi tanulási meghatározásához, hogy mi az szokásos bejelentkezési tevékenység kívüli használható. Ha bejelentkezések számát vagy a bejelentkezések, illetve a helyre, ahonnan a bejelentkezések kérnek idejét, vagy az egyéb bejelentkezési kapcsolatos tulajdonságokat lényegesen eltér az eredeti, egy figyelmeztetés előfordulhat, hogy létre. Ismét gépi tanulási határozza meg, hogy mi fontos.

## <a name="continuous-threat-intelligence-monitoring"></a>Folyamatos veszélyt jelent üzletiintelligencia-figyelése

Azure biztonság otthon biztonsági kutatási és adatok tudományos csapatok, amely folyamatosan figyelheti a kockázatot fekvő az megváltozott működik. Ide tartoznak a következő kezdeményezések:

- **Veszélyt üzletiintelligencia-ellenőrzés**: veszélyt intelligencia mechanizmusok, a jelölőket, a következmények és a meglévő és újonnan megjelenő veszélyek értekezletekre útmutatást tartalmaz. Ezt az információt a biztonsági Közösség meg van osztva, és a Microsoft folyamatosan figyeli veszélyt üzletiintelligencia-hírcsatornák belső és külső forrásból származó.
- **Jel megosztása**: a felhő és a helyszíni szolgáltatások, kiszolgálók és ügyfelek végpont eszközökön keresztül széles körű portfolió a Microsoft biztonsági csoportokból háttérismeretek osztva, és elemezheti. 
- **A Microsoft biztonsági szakértő**: csoportok között Microsoft speciális biztonsági mezők, például forensics dolgozik, és a webes támadások észlelése a folyamatban lévő részvételét.
- **Észlelési finombeállítása**: algoritmusok futnak valós ügyfél adatkészletek ellen, és biztonsági elemzők használata az ügyfelek számára az eredmények ellenőrzése. IGAZ és hamis pozitív finomíthatja a gépi tanulási algoritmusok használják.

Új és továbbfejlesztett észlelési, amely előnyeivel azonnali követi a kombinált erőfeszítéseket – az, hogy semmi nem.

## <a name="see-also"></a>Lásd még:
A jelen dokumentum megtanulta azt az Azure biztonság otthon észlelési funkciók működését. Többet szeretne tudni a biztonság otthon, olvassa el az alábbiakat:

- [Azure biztonság otthon tervezése és a műveletek útmutatója](security-center-planning-and-operations-guide.md)
- [Kezelés és válaszol a biztonsági figyelmeztetések az Azure biztonság otthon](security-center-managing-and-responding-alerts.md)
- [Biztonsági figyelmeztetések az Azure biztonság otthon típus szerint](security-center-alerts-type.md)
- [Az Azure biztonság otthon figyelése, biztonsági állapot](security-center-monitoring.md) – útmutató a Lync-állapota az Azure erőforrások.
- [Az Azure biztonság otthon partnermegoldások figyelése](security-center-partner-solutions.md) – megtudhatja, hogy miként figyelheti a partnerek megoldásaival állapotának.
- [Azure biztonsági központja – gyakori kérdések](security-center-faq.md) – gyakori kérdések a szolgáltatással a keresés.
- [Azure biztonsági blog](http://blogs.msdn.com/b/azuresecurity/) – Azure biztonság és megfelelőség kapcsolatos blogbejegyzéseket találja.
