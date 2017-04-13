<properties 
   pageTitle="StorSimple 8000 frissítés 0,3 kibocsátási megjegyzések |} Microsoft Azure"
   description="Az új szolgáltatások és a javítások, a megnyitott problémák és a rendelkezésre álló megoldások ismerteti a február 2015 Microsoft Azure StorSimple kiadás (frissítés 0,3)."
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
   ms.date="04/18/2016"
   ms.author="v-sharos" />

# <a name="storsimple-8000-series-update-03-release-notes---february-2015"></a>StorSimple 8000 sorozat frissítés 0,3 kibocsátási megjegyzések - február 2015

## <a name="overview"></a>– Áttekintés

A következő kibocsátási megjegyzések StorSimple 8000 sorozat frissítés jelent meg február 2015 0,3 azonosítása: a nyitott fontos problémákat. Is tartalmazhatnak a jelen kiadásban helyet kapott StorSimple szoftver- és belső vezérlőprogram frissítések listáját. A harmadik kiadásra az után a StorSimple 8000 sorozat kiadásával történt július 2014-es általában elérhető.
  
A frissítés a január Update nem változtatja meg az eszköz szoftver verziószáma. Verzió 6.3.9600.17312 továbbra is. Győződhet meg, hogy telepítették-e a frissítés, jelölje be az **Utolsó frissítés** dátumot. Ha a dátum 2/10/2015 vagy újabb verzió, majd a frissítés telepítése sikerült.  

Azt javasoljuk, hogy keresése hivatkozásra, és minden olyan elérhető frissítést alkalmazása, a StorSimple eszköz telepítése után azonnal. Is bekapcsolhatja az automatikus frissítések letöltéséhez és telepítéséhez fontos frissítéseket a Microsofttól, amint elengedi őket. További tudnivalókért olvassa el a [StorSimple eszköze frissítése](storsimple-update-device.md)című témakört.  

Tekintse át a kibocsátási megjegyzések mielőtt beállítaná a frissítést a StorSimple megoldás tárolt adatok számára.  

>[AZURE.IMPORTANT]   
>
> - A StorSimple kezelő szolgáltatás, és nem a Windows PowerShell-StorSimple segítségével a februári frissíté telepítése.   
> - A frissítés körülbelül egy óra vesz igénybe. Jó helyen jár Ha telepíti az összesített frissítéseit, a folyamat órát is igénybe vehet körülbelül 3 befejezéséhez.  
> - StorSimple február kiadását nem tartalmaz valamilyen frissítés a StorSimple virtuális eszközt. Továbbra is alkalmazhat érhető el Windows-frissítések a virtuális eszközt, a legutóbbi biztonsági javításokat, beleértve, de nem jelenik meg a virtuális eszközökre készült verziója változását.  

Győződjön meg arról, hogy az alábbi előfeltételek teljesülnek-e a StorSimple eszköz frissítése előtt.  

- Győződjön meg arról, hogy mindkét eszköz vezérlők futnak, mielőtt a frissítések keresése. Ha bármelyik vezérlő nem fut, a beolvasott képet sikertelen lesz. Győződjön meg róla, hogy a vezérlők megfelelő állapotba kerül, lépjen a **karbantartása** lap a **Hardveres állapotát** . Ha összetevők, hogy **a figyelmet segítségre van szüksége**, forduljon a Microsoft Support a folytatás előtt minden.
- Győződjön meg arról, hogy rögzített IP-címei vezérlő 0 és 1 vezérlő átirányítható, is csatlakozik az internethez, ezek a karbantartási a frissítéseket, hogy az eszköz használható. A [kapcsolat tesztelése parancsmag](https://technet.microsoft.com/library/hh849808.aspx) egy ismert cím, a hálózaton, például Outlook.com-fiókkal, ellenőrizze, hogy a vezérlőnek a külső hálózat elérhetőségének kívüli pingelése is használhatja.
- Győződjön meg arról, hogy a 80-as és 443-as elérhetők a kimenő kommunikáció StorSimple készülékén. További tudnivalókért olvassa el a [hálózati StorSimple eszközére vonatkozó követelmények](storsimple-system-requirements.md#networking-requirements-for-your-storsimple-device)című témakört.
- Ha az eszköz szoftver verziószáma 6.3.9600.17312 (október 2014-es frissítés) régebbi, ha engedélyezve van, a frissítés megkezdése előtt tiltsa le az adatok 2 és 3 adatok portokat. Az adatok 2 vagy adatok 3 elhagyása engedélyezett, ha a frissítés portok, előfordulhat, hogy az eszköz vezérlő helyreállítási módba. Felhívjuk a figyelmét arra, hogy a hálózati kapcsolatok letiltásakor összes társított kötet kell tenni kapcsolat nélkül, és a műveletekkel együtt fog hibásan fognak működni, a frissítés időtartamára.  
  
## <a name="whats-new-in-the-february-release"></a>A február kiadás újdonságai

A frissítés tartalmaz egy fix gyári visszaállítása a probléma, hogy mikor történt a Georgia a frissített eszközök engedje fel az október 2014-es verziójára. További tudnivalókért lásd az [Ebben a kiadásban javított](#issues-fixed-in-the-february-release).   

A frissítés új szolgáltatások és funkciók nem tartalmaz.  

## <a name="issues-fixed-in-the-february-release"></a>Javított február verziójában

Az alábbi táblázat ismerteti a problémát a frissítés javított.  
 
| nem. | A szolgáltatás | A probléma | Fizikai eszközök vonatkozik | Virtuális eszköz vonatkozik |
|-----|---------|-------|---------------------------------|-------------------------------|
| 1 | Gyári alaphelyzetbe állítása | Egy eszközön, amelyet eredetileg volt a Georgia kiadás (6.3.9600.17215 verzió), de az október frissült alaphelyzetbe gyár végrehajtásakor kiadásával (6.3.9600.17312). A gyár alaphelyzetbe sikertelen, és az eszköz lesz stabil. | igen | nem |


## <a name="known-issues-in-the-february-release"></a>Február kiadásának ismert problémák

Az alábbi táblázat összefoglalja az ebben a kiadásban ismert problémákról.
 
| nem. | A szolgáltatás | A probléma | Megjegyzések és megoldása | Fizikai eszközök vonatkozik  | Virtuális eszköz vonatkozik |
|-----|---------|-------|----------------------------|-----------------------------|--------------------------|
| 1 | Gyári alaphelyzetbe állítása | Bizonyos esetekben, amikor elkészíti a gyári alaphelyzetbe állítása az StorSimple eszköz előfordulhat, hogy és az üzenet megjelenítése: **az gyári visszaállítása (fázis 8) folyamatban van**. Ez történik, ha a CTRL + C billentyűkombinációt, miközben folyamatban van a parancsmag. | Nem után nyomja le a CTRL + C kezdeményezése egy gyári alaphelyzetbe állítása. Ha már ez az állapot, lépjen kapcsolatba a Microsoft Support a következő lépéseket. | igen | nem |
| 2 | Lemezen kvórum | Ritkán Ha a EBOD ház egy 8600device a lemezt a legtöbb kapcsolata nincs lemez kvórum, így akkor a tárhely kvótáját lesz offline. Kapcsolat nélküli maradnak, még akkor is, ha a lemez vannak újra. | Meg kell indítsa újra az eszközt. Ha a probléma nem szűnik meg, lépjen kapcsolatba a Microsoft Support a következő lépéseket. | igen | nem |
| 3 | Felhőalapú pillanatkép hibák | Ritkán felhőalapú pillanatkép előfordulhat, hogy hiba miatt sikertelen a **maximális biztonsági korlát elérhető**. Ez akkor fordul elő, ha nagyobb, mint 255 online klónok ugyanarra az eszközre, a törlése után ugyanazon eredeti kötet. |  | igen | igen |
| 4 | Helytelen vezérlő azonosító | A vezérlő helyettesítő végrehajtásakor vezérlő 0 is megjelennek majd vezérlő 1. Során vezérlő feltölteni a kép a peer csomópont betöltésekor a vezérlő azonosító is megjelennek az eredetileg majd a peer vezérlő azonosítójával. Ritkán a rendszer újraindítása után is a jelenség is látható. | Felhasználói beavatkozás nélkül szükség. Ez a helyzet feloldja, magát a vezérlő helyettesítő befejeződése után. | igen | nem |
| 5 | Eszköz diagramok figyelése | Az ellenőrző eszköz diagramok nem működnek, ha egyszerű vagy NTLM-hitelesítés engedélyezve van a proxy beállítása az eszköz a StorSimple-kezelő szolgáltatás. | Módosítsa az eszközt úgy, hogy a hitelesítés nincs értékre van állítva a StorSimple kezelő szolgáltatás regisztrált webes proxybeállításait. Ehhez futtassa a a Windows PowerShell-StorSimple Set-HcsWebProxy parancsmag. | igen | igen |
| 6 | Tárterület-fiókok | A tároló szolgáltatás törlése a tárterület-fiók használata egy nem támogatott forgatókönyv. Ez a helyzet, amelyben a felhasználói adatok nem lehet beolvasni vezet. |  | igen | igen |
| 7 | Eszköz feladatátvevő | Több feladatátadás egy mennyiségi tároló forrás ugyanarra az eszközre másik célt eszközök nem támogatott.  Egyetlen csendes eszközről áttérni különféle eszközökön láthatóvá válik a mennyiségi tárolók az első fölé eszközt nem sikerült adatok tulajdonjoga elvesznek. Egy feladatátvétel után következő mennyiségi tárolók jelennek meg, vagy viselkedésük eltérő, ha az Azure klasszikus portálon megtekintése. |   | igen | nem |
| 8 | Telepítés | Során StorSimple adaptert SharePoint-telepítését meg kell adnia egy eszköz IP ahhoz, hogy a telepített sikeres befejezéséhez. |  | igen | nem |
| 9 | Webhely-proxy | Ha a webes proxy konfigurációjának a megadott protokoll HTTPS-t, az eszköz-szolgáltatás kommunikációs hatással lesz, és az eszköz offline módba. A folyamat használata az eszközön jelentős erőforrások más támogatási csomag is jön létre. | Győződjön meg arról, hogy rendelkezik-e a megadott protokolljaként HTTP proxy webes URL-CÍMÉT. További információt [az eszköz konfigurálása webes proxy](storsimple-configure-web-proxy.md)olvashat. | igen | nem |
| 10 | Webhely-proxy | Ha konfigurálása és webes proxy regisztrált eszközön engedélyezése, majd meg kell indítsa újra az aktív vezérlő az eszközön. |  | igen | nem |
| 11 | Magas felhő időtartama és magas I/O terhelést | StorSimple eszköze nagyon magas felhő késések (másodperc sorrendben) és a magas I/O terhelést észlel, ha az eszköz kötet csökkentett állapotba lépjen, és a műveletekkel együtt során, melyről a "nem áll készen eszközt" hibaüzenet. | Manuálisan indítsa újra az eszközt vezérlők, és végezze el az eszköz áttérni ezt a helyzetet helyreállítása kell. | igen | nem |

## <a name="physical-device-updates-in-the-february-release"></a>Engedje fel a fizikai eszközök frissítéseket a februári

A frissítés megoldja a gyári alaphelyzetbe probléma, hogy mikor történt a Georgia a frissített eszközök engedje fel az október 2014-es verziójára. Nem tartalmazó frissítések az StorSimple eszközre.  

## <a name="serial-attached-scsi-sas-controller-and-firmware-updates-in-the-february-release"></a>Belső vezérlőprogram frissítéseket, a februári és a soros csatolt SCSI () vezérlő kiadás

Ebben a kiadásban nem tartalmaz valamilyen frissítés a soros csatolt SCSI () vezérlő vagy a belső vezérlőprogram. Az illesztőprogram frissítése az október, 2014 megjelenés volt.  

## <a name="virtual-device-updates-in-the-february-release"></a>Engedje fel az a februári virtuális eszköz frissítése

Ebben a kiadásban nem tartalmaz a virtuális eszköz e frissítéseket. A frissítés telepítése nem változik a virtuális eszköz szoftver verziószáma.
 
