<properties 
   pageTitle="StorSimple 8000 sorozat frissítés 2.2 kibocsátási megjegyzések |} Microsoft Azure"
   description="Az új funkciók, problémák és megoldások StorSimple 8000 sorozat frissítés 2.2 ismerteti."
   services="storsimple"
   documentationCenter="NA"
   authors="alkohli"
   manager="carmonm"
   editor="" />
 <tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="07/18/2016"
   ms.author="alkohli" />

# <a name="storsimple-8000-series-update-22-release-notes"></a>StorSimple 8000 sorozat frissítés 2.2 kibocsátási megjegyzések  

## <a name="overview"></a>– Áttekintés

A következő kibocsátási megjegyzések az új szolgáltatásokat ismertetik, és a kritikus megnyitott kapcsolatos problémák azonosítása StorSimple 8000 sorozat frissítés 2.2 számára. Is tartalmazhatnak a jelen kiadásban helyet kapott StorSimple szoftverfrissítések listáját. 

Frissítés 2.2 bármely StorSimple eszköz frissítés 2.1-es kiadás (kiadás) vagy a frissítés 0,1 átnézett alkalmazhatók. Az eszköz frissítés 2.2 társított verziója 6.3.9600.17708.

Tekintse át a kibocsátási megjegyzések mielőtt beállítaná a frissítést a StorSimple megoldás tárolt adatok számára.

>[AZURE.IMPORTANT]
> 
> - Frissítés 2.2 csak szoftverfrissítések tartalmaz. A frissítés körülbelül 1.5-2 órán vesz igénybe. 

> - Frissítés 2.1-es operációs rendszert futtató, azt javasoljuk frissítés 2.2 minél korábban beállítást alkalmazni.

> - Az új verziókban, esetleg nem látható frissítések azonnal, mert a frissítéseket a szakaszos bevezetésének mi. Néhány napot várni, és amint majd újra, ezek a frissítések keresése lesz elérhető.


## <a name="whats-new-in-update-22"></a>A frissítés 2.2 újdonságai

Az alábbi főbb változásokkal frissítés 2.2 esett.

 
- **Az Automatikus térköz visszanyerése optimalizálási** – adatok törlésekor a vékonyan kiépített kötet, még nem használt tárterület blokkok kell nyerhető. Ebben a kiadásban megújult a hely visszanyerése folyamat a felhőből, így a nem használt szóköz valamit elérhető gyorsabb, mint a korábbi verziók.


- **Pillanatkép teljesítménnyel kapcsolatos fejlesztések** – frissítés 2.2 megújult az idő pillanatkép bizonyos esetekben, ahol nagy kötet használatban van, és nincsenek adatok tejeskanna a minimális felhő feldolgozása. Ez a bővítés előnyös példa az archiválás kötet lenne.


- A **támogatás hardening csomag összegyűjtése** – javítása a támogatási csomag összegyűjtött és ebben a kiadásban feltöltött módon kaptak. 


- Hibajavítás, amelyeknél a egy frissítés nagyobb megbízhatóság **megbízhatóság javítása frissítése** – a jelen kiadás tartalmaz.

  
 

## <a name="issues-fixed-in-update-22"></a>A frissítés 2.2 javított

Az alábbi táblázatban a frissítések 2.2 és 2.1 javított hibák összefoglalását tartalmazza.    

| nem | A szolgáltatás                                    | A probléma                                                                                                                                                                                                                                                                                        | Fizikai eszközök vonatkozik | Virtuális eszköz vonatkozik |
|----|--------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------|---------------------------|
| 1  | A Host teljesítmény                      | A korábbi verzióban host melletti teljesítménnyel kapcsolatos problémák megfigyelt helyileg rögzített kötet létrehozását és többszintű kötet átalakítása helyi rögzített kötet alatt. Ebben a kiadásban során a mennyiségi létrehozását és átalakítás eljárások host teljesítményének javítása egyúttal így javított ezeket a problémákat.                                                                        | igen                        | nem                        |
| 2  | Helyi meghajtóra rögzített kötet                     | Ritkán a rendszer volna összeomolhat, amikor a helyi meghajtóra rögzített kötet létrehozása. Ebben a kiadásban rögzített ezt a hibát.                                                                                                                                                               | igen                        | nem                        |
| 3  | Tiering                                    | Ha a StorSimple felhő készülékekre (8010 és 8020) metaadatait többszintű a felhőbe volt-kapcsolata időről-időre összeomlik. Ez a probléma megoldódott, ebben a verzióban.                                                                                                                              | nem                         | igen                       |
| 4  | Pillanatkép létrehozása                          | A sok esetben növekményes pillanatképek létrehozásának kapcsolódnak, és nincsenek adatok tejeskanna a minimális volt-problémák. Ebben a kiadásban javított ezeket a problémákat.                                                                                                                 | igen                        | igen                       |
| 5  | Openstack hitelesítés                   | A felhőben szolgáltatóként Openstack használatakor a felhasználó volna fellépő esetleges egy, a hitelesítés kapcsolatos ritka programhibák hol e az összeomlást a JSON-elemzővel eredményeként. Ez a hiba megoldódott, ebben a verzióban.                                                                                                                              | igen                        | nem                        |
| 6  | A Host melletti másolás                             | Korábbi verzióiban a szoftvert egy kapcsolódó ODX időpontjának ritka programhibák volt látható, ha egy mennyiségi az adatok másolása egy másik mennyiségi. Ez egy vezérlő című témakörében, és ha a rendszer sikerült esetleg módba helyreállítási. Ez a hiba megoldódott, ebben a verzióban. | igen                        | nem       |
| 7  | A Windows Management műszerezettségi (WMI) | Szoftver korábbi verzióiban, hiba történt több példányát a kivétel web proxy hiba "<ManagementException> nem sikerült a betöltés". Ezt a hibát WMI fejlesztő rendelt volt, és most már megoldódott.                                                               | igen                        | nem                        |
| 8  | Frissítés                                     | Bizonyos esetekben a ritka, a szoftver, a korábbi verziók a felhasználónak meg az ellenőrzési vagy a frissítések telepítése kapott "CisPowershellHcsscripterror". Ez a probléma megoldódott, ebben a verzióban.                                                                                        | igen                        | igen                       |
| 9  | Támogatás csomag                            | Ebben a kiadásban eddig javítása a támogatási csomag összegyűjtött és feltölteni a módját.                                                                                                                                                                                                      | igen                        | igen                                    |


## <a name="known-issues-in-update-22"></a>Ismert problémák a frissítés 2.2

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
| 20 |Helyi meghajtóra rögzített kötet | Ha megpróbál (létrehozott és frissítés 1.2-es verziójával klónozott vagy régebbi) többszintű kötet konvertálása helyi rögzített kötet és az eszköz fogy a hely, vagy egy felhőalapú üzemszünetek van, a clone(s) is sérült.| A probléma csak a kötet, amelyeket létrehozott, és a klónozott előtti Update 2.1 szoftver. Ez egy ritka forgatókönyv kell.|
| 21 | Mennyiségi átalakítás | Nem frissülnek a kötet csatolva, miközben folyamatban van a mennyiségi konvertálás ACRs (helyben kiemelt többszintű vagy fordítva). A ACRs frissítése eredményezhet adatsérülés. | Ha szükséges, frissítse a mennyiségi konvertálás előtt a ACRs és nem minden további ACR frissítések miközben folyamatban van a konvertálás. |

## <a name="controller-and-firmware-updates-in-update-22"></a>A frissítés 2.2 vezérlő és a belső vezérlőprogram frissítések

Ebben a kiadásban frissítései csak szoftverrel. Jó helyen jár, ha 2 frissítés előtti verziójáról frissít, akkor telepíteni kell-illesztőprogram Storport, Spaceport, és (egyes esetekben) lemezre belső vezérlőprogram frissítések az eszközön.
 
További információt a illesztőprogram, Storport, Spaceport és lemez belső vezérlőprogram frissítések telepítési eszközön című témakörben [telepítse a frissítés 2.2](storsimple-install-update-21.md) StorSimple.

 
## <a name="virtual-device-updates-in-update-22"></a>A frissítés 2.2 virtuális eszköz frissítése

A frissítés nem alkalmazható a virtuális eszközt. Új virtuális eszközök kell létrehozni. 

## <a name="next-step"></a>Következő lépés

Ismerje meg, [telepítse a frissítés 2.2](storsimple-install-update-21.md) StorSimple eszközén.
