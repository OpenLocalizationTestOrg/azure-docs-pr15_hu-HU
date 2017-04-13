<properties 
   pageTitle="StorSimple 8000 sorozat frissítés 2 kibocsátási megjegyzések |} Microsoft Azure"
   description="Az új funkciók, problémák és megoldások StorSimple 8000 sorozat frissítés 2 ismerteti."
   services="storsimple"
   documentationCenter="NA"
   authors="SharS"
   manager="carmonm"
   editor="" />
 <tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="05/24/2016"
   ms.author="v-sharos" />

# <a name="storsimple-8000-series-update-2-release-notes"></a>StorSimple 8000 sorozat frissítés 2 kibocsátási megjegyzések  

## <a name="overview"></a>– Áttekintés

A következő kibocsátási megjegyzések az új szolgáltatásokat ismertetik, és a kritikus megnyitott kapcsolatos problémák azonosítása StorSimple 8000 sorozat frissítés 2. Is tartalmazhatnak a StorSimple szoftver, a illesztőprogram és a jelen kiadásban helyet kapott lemez belső vezérlőprogram frissítések listáját. 

2 frissítés bármely StorSimple eszköz frissítés 1.2-es kiadás (kiadás) vagy a frissítés 0,1 átnézett alkalmazhatók. Az eszköz frissítés 2 társított verziója 6.3.9600.17673.

Tekintse át a kibocsátási megjegyzések mielőtt beállítaná a frissítést a StorSimple megoldás tárolt adatok számára.

>[AZURE.IMPORTANT]
> 
- A frissítés (beleértve a Windows-frissítések) körülbelül 4-7 óra vesz igénybe. 
- 2 frissítés tartalmaz, szoftver, a LSI illesztőprogram és SSD belső vezérlőprogram frissítéseket.
- Az új verziókban, esetleg nem látható frissítések azonnal, mert a frissítéseket a szakaszos bevezetésének mi. Néhány napot várni, és amint majd újra, ezek a frissítések keresése lesz elérhető.


## <a name="whats-new-in-update-2"></a>A frissítés 2 újdonságai

2 frissítés vezet be az alábbi új szolgáltatásokat.

- A **helyi meghajtóra kiemelt kötet** – a korábbi verziókban StorSimple 8000 adatsor adatblokkok voltak többszintű a felhőbe használatát alapján. Nem tudja biztosítani, hogy megakadályozza a helyi volna kapcsolatának volt. A frissítés 2 kötet, létrehozásakor is kijelölhet egy mennyiségi módon, hogy a kötet helyileg rögzített és elsődleges adatainak nem lehet a felhőbe többszintű. Helyi meghajtóra rögzített mennyiségének pillanatképek fog továbbra is másolható biztonsági mentés a felhőbe, hogy az a felhő adatok mobilitás és katasztrófa helyreállítási célokra használható. Továbbá módosíthatja a kötet típusa (Ez azt jelenti, hogy konvertálás többszintű kötet helyileg rögzített kötet való és az átalakítás helyileg kiemelt kötet többszintű). 

- **StorSimple virtuális eszköz fejlesztések** : az előzőleg, a StorSimple 8000 sorozat elhelyezni a virtuális eszköz megoldásként katasztrófa helyreállítás vagy fejlesztési/próba. Virtuális eszköz (modell 1 100) csak egy mintája volt. 2 frissítés vezet be két virtuális eszköz modellek: 

     - 8010 (korábbi nevén az 1 100) – ne módosítsa; 30 TB kapacitása és Azure szabványos tárhelyek.
     - 8020 – 64 TB kapacitással rendelkezik, és Azure prémium tárterületet használ a nagyobb teljesítmény.

    Egy egyetlen virtuális mindkét virtuális eszköz modellek (8010/8020) nem. A virtuális eszköz első indításakor platform paraméterek észleli, és a megfelelő modell verzió vonatkozik.

- **Hálózati fejlesztések** : Update 2 tartalmazza a következő hálózati fejlesztéseket:

     - Több engedélyezhető az a felhő, így feladatátvevő akkor fordulhat elő, ha nem sikerül egy egy hálózati kártya.
     - Útválasztási továbbfejlesztettük, rögzített mértékek felhő blokkok engedélyezve van.
     - Online újrapróbálkozási sikertelen erőforrások egy átadása előtt.
     - Új riasztásainak szolgáltatás hibák.

- **Frissítése fejlesztések** : az 1.2-es és korábbi rendszerekben a StorSimple 8000 sorozat frissítés frissült két csatornákon keresztül: frissítés a Windows rendszerhez fürtözés, iSCSI és egyebekkel, és a Microsoft Update bináris és belső vezérlőprogram.
    2 frissítés a Microsoft Update használja, az összes frissítése csomagok. Ez célszerű kevesebb időt javítása, illetve feladatátadás módon vezethet. 

- **Belső vezérlőprogram frissítések** – a következő belső vezérlőprogram frissítések szerepelnek:
    - LSI: a termék verziója 2.00.72.10 lsi_sas2.sys
    - Csak SSD (nincs merevlemez-frissítések): XMGG, XGEG, KZ50, F6C2 és VR08

- **Megelőző támogatási** – frissítés 2 lehetővé teszi a Microsoft arra, hogy lekérje az eszközről a diagnosztikai tájékozódhat. Ha a tevékenységek csoportunk azonosítja problémák vannak eszközök, vagyunk jobban ellátni a információk összegyűjtése az eszközről és problémáinak diagnosztizálása. A **frissítés 2, elfogadásával engedélyezi a velünk a megelőző támogatása érdekében**.    
 

## <a name="issues-fixed-in-update-2"></a>A frissítés 2 javított

Az alábbi táblázatokban frissítések 2 javított hibák összefoglalását tartalmazza.    

| nem. | A szolgáltatás | A probléma | Fizikai eszközök vonatkozik | Virtuális eszköz vonatkozik |
|-----|---------|-------|--------------------------------|--------------------------------|
| 1 | Hálózati kapcsolatok | Frissítés 1-re frissítés után a StorSimple kezelő szolgáltatás jelenteni, hogy a adat2 és Data3 portokat egyetlen vezérlőn sikertelen volt. Ez a probléma megoldását. | igen | nem |
| 2 | Frissítések | Frissítés 1-re frissítés után hallható riasztások több eszközön az Azure klasszikus portálon történt. Ez a probléma megoldását. | igen | nem |
| 3 | Openstack hitelesítés | A felhő szolgáltatóként Openstack használatakor, hogy a felhőben hitelesítési karakterlánc túl hosszú volt hiba jelenhet meg. A rögzített. | igen | nem |


## <a name="known-issues-in-update-2"></a>Ismert problémák a frissítés 2

Az alábbi táblázat összefoglalja az ebben a kiadásban ismert problémákról.

| nem. | A szolgáltatás | A probléma | Megjegyzések és megoldása | Fizikai eszközök vonatkozik | Virtuális eszköz vonatkozik |
|-----|---------|-------|----------------------------|----------------------------|---------------------------|
| 1 | Lemezen kvórum | Ritkán Ha egy 8600 eszköz EBOD helyiségben lemezt többségét kapcsolata nincs lemez kvórum, így a tárhely kvótáját fog látogassa meg offline. Kapcsolat nélküli maradnak, még akkor is, ha a lemez vannak újra. | Meg kell indítsa újra az eszközt. Ha a probléma nem szűnik meg, lépjen kapcsolatba a Microsoft Support a következő lépéseket. | igen | nem |
| 2 | Helytelen vezérlő azonosító | A vezérlő helyettesítő történik, amikor vezérlő 0 is megjelennek majd vezérlő 1. Során vezérlő feltölteni a kép a peer csomópont betöltésekor a vezérlő azonosító is megjelennek az eredetileg majd a peer vezérlő azonosítójával. Ritkán a rendszer újraindítása után is a jelenség is látható. | Felhasználói beavatkozás nélkül szükség. Ez a helyzet feloldja, magát a vezérlő helyettesítő befejeződése után. | igen | nem |
| 3 | Tárterület-fiókok | A tároló szolgáltatás törlése a tárterület-fiók használata egy nem támogatott forgatókönyv. Ez a helyzet, amelyben a felhasználói adatok nem lehet beolvasni vezet.|  | igen | igen |
| 4 | Eszköz feladatátvevő | Több feladatátadás egy mennyiségi tároló forrás ugyanarra az eszközre másik célt eszközök nem támogatott. Egyetlen csendes eszközről áttérni különféle eszközökön láthatóvá válik a mennyiségi tárolók az első fölé eszközt nem sikerült adatok tulajdonjoga elvesznek. Egy feladatátvétel után következő mennyiségi tárolók jelennek meg, vagy viselkedésük eltérő, ha az Azure klasszikus portálon megtekintése. | | igen | nem |
| 5 | Telepítés | Során StorSimple adaptert SharePoint-telepítését meg kell adnia egy eszköz IP ahhoz, hogy a telepített sikeres befejezéséhez.    | | igen | nem |
| 6 | Webhely-proxy | Ha a webes proxy konfigurációjának a megadott protokoll HTTPS-t, az eszköz-szolgáltatás kommunikációs hatással lesz, és az eszköz offline módba. A folyamat használata az eszközön jelentős erőforrások más támogatási csomag is jön létre. | Győződjön meg arról, hogy rendelkezik-e a megadott protokolljaként HTTP proxy webes URL-CÍMÉT. További információért lépjen [konfigurálása webes proxy az eszközön](storsimple-configure-web-proxy.md). | igen | nem |
| 7 | Webhely-proxy | Ha konfigurálása és webes proxy regisztrált eszközön engedélyezése, majd meg kell indítsa újra az aktív vezérlő az eszközön. | | igen | nem |
| 8 | Magas felhő időtartama és magas I/O terhelést | StorSimple eszköze nagyon magas felhő késések (másodperc sorrendben) és a magas I/O terhelést észlel, ha az eszköz kötet csökkentett állapotba lépjen, és a műveletekkel együtt során, melyről a "nem áll készen eszközt" hibaüzenet. | Manuálisan indítsa újra az eszközt vezérlők, és végezze el az eszköz áttérni ezt a helyzetet helyreállítása kell. | igen | nem |
| 9 | Azure PowerShell | A StorSimple parancsmag **Get-AzureStorSimpleStorageAccountCredential & #124; használata esetén Jelölje ki-objektum - először 1 – várakozás** az első objektumra, hogy hozhat létre egy új **VolumeContainer** objektum kijelöléséhez a parancsmag objektumait adja eredményül. | A parancsmaggal tördelése a zárójelek között az alábbi képlettel történik: **(Get-Azure-StorSimpleStorageAccountCredential) & #124; Jelölje ki-objektum - először 1 – várakozás** | igen | igen |
| 10| Áttelepítési | Több mennyiségi tárolók az áttelepítés át, a legújabb biztonsági másolatának Európai esetén csak az első mennyiségi tároló pontos. Az első mennyiségi tároló az első 4 biztonsági másolatok áttelepítése után a párhuzamos áttelepítési fog elindulni. | Azt javasoljuk, hogy egyszerre egy mennyiségi tároló át. | igen | nem |
| 11| Áttelepítési | A visszaállítás, után kötet nem a biztonsági másolat házirend vagy adott a virtuális lemez csoport. | Szüksége lesz a következő kötet hozzáadása egy biztonsági házirendhez biztonsági másolatok létrehozásához. | igen | igen |
| 12| Áttelepítési | Az áttelepítés befejeződése után az 5000/7000 sorozat eszköz nem kell az áttelepített adatokat tárolók elérni. | Azt javasoljuk, hogy az áttelepített adatokat tárolók törlése után az áttelepítés befejeződött és lekötött. | igen | nem |
| 13-mal| Adatfeliratsor és DR | Frissítés 1 futó StorSimple eszköz nem klónozhatja, és végezze el a frissítés előtti 1 szoftvert futtat eszközre vészhelyreállítás. | Meg kell frissítés engedélyezése az 1 ezeket a műveleteket a célként megadott eszköz frissítése | igen | igen |
| 14 | Áttelepítési | Konfigurációs biztonsági másolatot az áttelepítés 5000-7000 sorozat eszközön meghiúsulhat, ha vannak a hangerő-csoportok nem társított kötet. | Nem társított kötet az üres mennyiségi csoportok törlése, majd próbálkozzon újra a a konfigurációs biztonsági másolatot.| igen | nem |
| 15 | Azure PowerShell-parancsmagok és a helyi meghajtóra rögzített kötet | A helyi meghajtóra rögzített mennyiségi Azure PowerShell-parancsmagok keresztül nem hozható létre. (Tetszőleges Azure PowerShell keresztül létrehozhat mennyiségi fog többszintű.) |Mindig a StorSimple kezelő szolgáltatás használatával állítsa be a helyben rögzített kötet.| igen | nem |
| 16 |Helyi meghajtóra rögzített kötet lemezterület | Ha töröl egy helyi rögzített mennyiség, az új kötet rendelkezésre álló hely előfordulhat, hogy nem frissülnek azonnal. A StorSimple kezelő szolgáltatás frissíti a helyi lemezterület óránként körülbelül.| Várja meg egy óra próbál létrehozni az új kötet előtt. | igen | nem |
| 17 | Helyi meghajtóra rögzített kötet | A visszaállítási feladat az ideiglenes biztonságimásolat-pillanatfelvétel a biztonságimásolat-katalógust, de csak a visszaállítási feladat időtartamának közzététele. Emellett az előtag **tmpCollection** **Biztonsági házirendek** lapon, de csak a visszaállítási feladat időtartamának pillanatkép csoport közzététele azt. | Ez akkor fordulhat elő, ha a visszaállítási feladat van csak a helyi számítógépre rögzített, kötet vagy kombinálja a helyi meghajtóra rögzített és többszintű kötet. Ha a visszaállítási feladat csak többszintű kötet tartalmazza, majd a probléma nem történik. Felhasználói beavatkozás nélkül szükség. | igen | nem |
| 18 | Helyi meghajtóra rögzített kötet | Ha megszakítja a visszaállítási feladat és a vezérlő feladatátvevő fordul elő a közvetlenül ezt követően a visszaállítási feladat jelennek meg: **nem sikerült** **Visszavonva**helyett. Ha egy visszaállítási feladat sikertelen, és a vezérlő feladatátvétel azonnal ezután jelennek meg a visszaállítási feladat **megszakad** helyett **sikertelen volt**. | Ez akkor fordulhat elő, ha a visszaállítási feladat van csak a helyi számítógépre rögzített, kötet vagy kombinálja a helyi meghajtóra rögzített és többszintű kötet. Ha a visszaállítási feladat csak többszintű kötet tartalmazza, majd a probléma nem történik. Felhasználói beavatkozás nélkül szükség. | igen | nem |
| 19 |Helyi meghajtóra rögzített kötet | Ha megszakítja a visszaállítási feladat, vagy ha nem sikerül a visszaállítás, majd vezérlő feladatátvétel, egy további visszaállítási feladat megjelenik a **feladatok** lapon. | Ez akkor fordulhat elő, ha a visszaállítási feladat van csak a helyi számítógépre rögzített, kötet vagy kombinálja a helyi meghajtóra rögzített és többszintű kötet. Ha a visszaállítási feladat csak többszintű kötet tartalmazza, majd a probléma nem történik. Felhasználói beavatkozás nélkül szükség. | igen | nem |
| 20 |Helyi meghajtóra rögzített kötet | Ha megpróbál (létrehozott és frissítés 1.2-es verziójával klónozott vagy régebbi) többszintű kötet konvertálása helyi rögzített kötet és az eszköz fogy a hely, vagy egy felhőalapú üzemszünetek van, a clone(s) is sérült.| A probléma csak a kötet, amelyeket létrehozott, és a klónozott előtti Update 2 szoftver. Ez egy ritka forgatókönyv kell.|
| 21 | Mennyiségi átalakítás | Nem frissülnek a kötet csatolva, miközben folyamatban van a mennyiségi konvertálás ACRs (helyben kiemelt többszintű vagy fordítva). A ACRs frissítése eredményezhet adatsérülés. | Ha szükséges, frissítse a mennyiségi konvertálás előtt a ACRs és nem minden további ACR frissítések miközben folyamatban van a konvertálás. |

## <a name="controller-and-firmware-updates-in-update-2"></a>A frissítés 2 vezérlő és a belső vezérlőprogram frissítések

Ebben a kiadásban frissíti a vezető és a lemez belső vezérlőprogram az eszközön.
 
- További információt a LSI belső vezérlőprogram frissítéséhez olvassa el a Microsoft Tudásbázis 3121900. 
- További információt a lemez belső vezérlőprogram frissítéséhez olvassa el a Microsoft Tudásbázis 3121899.
 
## <a name="virtual-device-updates-in-update-2"></a>A frissítés 2 virtuális eszköz frissítése

A frissítés nem alkalmazható a virtuális eszközt. Új virtuális eszközök kell létrehozni. 

## <a name="next-step"></a>Következő lépés

Ismerje meg, [telepítse a frissítés 2](storsimple-install-update-2.md) StorSimple eszközén.
