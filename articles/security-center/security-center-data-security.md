<properties
   pageTitle="Azure biztonság otthon adatok biztonsági |} Microsoft Azure"
   description="Ez a dokumentum adatainak felügyelt és az Azure biztonság otthon őrizni ismerteti."
   services="security-center"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor=""/>

<tags
   ms.service="security-center"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/25/2016"
   ms.author="yurid"/>

# <a name="azure-security-center-data-security"></a>Azure biztonság otthon adatvédelem
Munkában megelőzésére, feltárására és megválaszolása az Azure biztonság otthon gyűjti össze, és az Azure erőforrások, például a konfigurációs adatok, a metaadat-alapú, az eseménynaplók, adatainak feldolgozásával veszélyek, összeomolhat kiírása fájlok, és így tovább. Erős kötelezettségek adatvédelmi és biztonsági az adatok védelme VÁLLALUNK. Microsoft nakhttp://go.microsoft.com/fwlink/?LinkId=311864 megfelelően igazodik szigorú megfelelőség és a biztonsági útmutató – a szolgáltatás működik-ból. 

Ez a cikk ismerteti, adatainak felügyelt és az Azure biztonság otthon őrizni.

## <a name="data-sources"></a>Adatforrások

Azure biztonság otthon elemzi a következő forrásokból származó adatok:

- Azure szolgáltatások: Felolvassa az adott erőforrás szolgáltató kommunikáció telepítette az Azure szolgáltatások konfigurálása információt.
- Hálózati forgalmának engedélyezésére: Mintát olvasás hálózati forgalmat a Microsoft infrastruktúra, például az IP-/ port csomag mérete, forrás metaadatok és protokoll.
- A Partnermegoldások: Biztonsági figyelmeztetések integrált partnermegoldások, például a tűzfalak és antimalware megoldások gyűjti össze. Az adatok Azure biztonság otthon tárolására, jelenleg az Amerikai Egyesült Államokban található vannak tárolva.
- A virtuális gépeken futó: Azure biztonság otthon is konfigurációs információk és biztonsági eseményeket, például a Windows-eseménynaplózás kapcsolatos információk összegyűjtése és naplózási naplók, az IIS naplók, syslog üzenetek és összeomlik kiírása-fájlokat a virtuális gépeken futó adatok gyűjtése ügynökök használatával. Című "Adatgyűjtés kezelése" alatti további információt.  

Ezeken kívül információ kapcsolatos biztonsági riasztások, javaslatok és biztonsági állapot állapot Azure biztonság otthon tárolására, jelenleg az Amerikai Egyesült Államokban található vannak tárolva. Ezt az információt a kapcsolódó konfigurációs információk és biztonsági események a virtuális gépeken futó, hogy biztosítson a biztonsági figyelmeztetés, ajánlási vagy biztonsági állapot állapot szükség szerint begyűjtött tartalmazhat.

## <a name="data-protection"></a>Adatok védelme

**Adatok feladatkörök**: a szolgáltatás teljes egyes összetevőre adatok logikailag külön legyen. Az összes adat egy szervezet címkézett. A címkézés továbbra is fennáll, az adatok életciklus során, és ez a szolgáltatás egyes rétegben vannak érvényben. Ezenkívül a virtuális gépeken futó gyűjtött adatokat a tárterület-fiókok vannak tárolva.

**Adathozzáférés**: Adja meg a biztonsági javaslatok, és vizsgálja meg a potenciális biztonsági kockázatok, a Microsofton hozzáférés összegyűjtött vagy Azure szolgáltatást, többek között a összeomlik kiírása fájlok elemezheti. Összeomlik kiírása fájlokat és a folyamat létrehozása események felhasználói adatok és személyes adatait a virtuális gépeken futó véletlenül tartalmazhat. A [Microsoft Online Services kifejezéseket](http://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=31) és [Adatvédelmi nyilatkozata](https://www.microsoft.com/privacystatement/en-us/OnlineServices/Default.aspx), mely állapot, hogy a Microsoft nem használja az ügyfél adatait, vagy adatait, hirdetési vagy hasonló kereskedelmi célú származhat mérvadónak. Csak használjuk vevő adatait szükség szerint biztosítja az Azure szolgáltatásaival, beleértve azokat a szolgáltatásokat nyújtó kompatibilis céljából. Megőrzi az összes jogosultságot ügyfél adatait.

**Adatok használata**: a Microsoft használja minták és több bérlők között látható veszélyt üzletiintelligencia tartalmasabbá teszi a megelőzése és a kimutatás funkciók; azt a műveletet az adatvédelmi nyilatkozatát el az [Adatvédelmi nyilatkozat](https://www.microsoft.com/privacystatement/en-us/OnlineServices/Default.aspx)leírtak szerint.

**Adatok helye**: A tárterület az egyes régiókra virtuális gépeken futó futtató megadva. Ez lehetővé teszi, hogy ugyanabban a régióban, mint a virtuális gép, amelyből gyűjtött adatokat tárolja az adatokat. Ezeket az adatokat, többek között az összeomlást kiírása a fájlok folyamatosan kíván tárolni a tárterület-fiókjában. A szolgáltatás is tárol a biztonsági figyelmeztetések, többek között integrált partnermegoldások, javaslatok és biztonsági állapot állapot érkező riasztások az Azure biztonság otthon tárolási jelenleg az Amerikai Egyesült Államokban található információkat.

## <a name="managing-data-collection-from-virtual-machines"></a>Adatgyűjtés virtuális gépeken futó kezelése

Ha úgy dönt, hogy engedélyezze az Azure biztonság otthon, adatgyűjtés be van kapcsolva az egyes az előfizetések. Adatgyűjtés az Azure biztonsági központ Irányítópultjára "Biztonsági házirend" részében kikapcsolhatja. Adatgyűjtés bekapcsolt Azure biztonság otthon rendelkezések az Azure figyelése ügynök minden meglévő és támogatja a virtuális gépeken futó létrehozott új faladatokat. Az Azure biztonsági figyelése bővítmény keres a kapcsolódó különböző biztonsági beállításokat és események azt (esemény-nyomkövetés) [Eseményvezérelt Tracing a Windows](https://msdn.microsoft.com/library/windows/desktop/bb968803.aspx) rendszerbe követi nyomon. Az operációs rendszer ezenkívül előléptetése a eseménynaplójának események a gép futtatása során. Ezek az adatok példák: operációs rendszer típusa és verzióban operációs rendszer naplók (Windows eseménynaplók), futó folyamatok, számítógépnév, IP-címek bejelentkezett felhasználó és a bérlői azonosítójával. Az Azure figyelése ügynök felolvassa eseménynaplójának bejegyzései, és esemény-nyomkövetés követi nyomon, és másolja át az őket tároló analysis fiókjára. 

A tárterület-fiók van megadva, az egyes régiókra virtuális gépeken futó operációs rendszert futtató, ahol gyűjtött adatok a modul, amely a virtuális gépeken futó ugyanazon régió tárolt lehetősége van. Ezzel megkönnyíti az meg szeretné tartani az adatokat az azonos földrajzi terület adatvédelmi és adatok felségterületéhez céljából. Beállíthatja az egyes régiókra vonatkozó tároló fiókok az Azure biztonsági központ Irányítópultjára "Biztonsági házirend" szakaszában.

Az Azure figyelése ügynök összeomlik kiírása fájlokat is másolja a tárterület-fiókjába.  Azure biztonság otthon gyűjti össze a összeomlik kiírása fájlok rövid életű másolatának és elemzi őket rosszindulatú kísérletek és sikeres kompromisszumok bizonyítékokra vonatkozóan.  Azure biztonság otthon hajt végre az elemzés ugyanabban a földrajzi régióban a tárterület-fiókként belül, és törli a rövid életű példányokat, analysis befejeződésekor.

Adatok gyűjtése virtuális gépeken futó bármikor, amely távolítsa el az Azure biztonság otthon által korábban telepített figyelése ügynökök tilthatja le.


## <a name="see-also"></a>Lásd még:

A jelen dokumentum szüksége a tanultakhoz adatainak felügyelt és az Azure biztonság otthon védelmét. Azure biztonság otthon kapcsolatos további tudnivalókért lásd:

- [Azure biztonsági központ tervezése és a műveletek útmutató](security-center-planning-and-operations-guide.md) – megtudhatja, hogy miként tervezése és Azure biztonság otthon elfogadja a tervezés kapcsolatos szempontok ismertetése.
- [Az Azure biztonság otthon figyelése, biztonsági állapot](security-center-monitoring.md) – útmutató a Lync-állapota az Azure erőforrások
- [Kezelése és válaszol a biztonsági figyelmeztetések az Azure biztonság otthon](security-center-managing-and-responding-alerts.md) – megtudhatja, hogy miként kezelheti, és a biztonsági figyelmeztetések megválaszolása
- [Az Azure biztonság otthon partnermegoldások figyelése](security-center-partner-solutions.md) – megtudhatja, hogy miként figyelheti a partnerek megoldásaival állapotának.
- [Azure biztonsági központja – gyakori kérdések](security-center-faq.md) – keresés a szolgáltatással kapcsolatos gyakori kérdések
- [Azure biztonsági Blog](http://blogs.msdn.com/b/azuresecurity/) – Azure biztonság és megfelelőség kapcsolatos blogbejegyzéseket megkeresése
