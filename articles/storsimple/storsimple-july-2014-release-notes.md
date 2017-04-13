<properties 
   pageTitle="StorSimple 8000 kiadás verziója kibocsátási megjegyzések |} Microsoft Azure"
   description="Az új szolgáltatások, a Megnyitás problémák és a elérhető megoldásokat alkalmazhatja ismerteti a július 2014-es Microsoft Azure StorSimple megjelenés."
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

# <a name="storsimple-8000-series-release-version-release-notes---july-2014"></a>StorSimple 8000 sorozat kiadásával kibocsátási megjegyzések – július 2014-es 

## <a name="overview"></a>– Áttekintés

A követés kibocsátási megjegyzések a kritikus megnyitott problémák azonosítása StorSimple 8000 adatsor Microsoft Azure StorSimple július 2014-es általános elérhetőség (kiadás) kiadását. Ebben a kiadásban szoftver verziószáma 6.3.9600.17215 felel meg.  

Hiányában, ezek kibocsátási megjegyzések az StorSimple eszköz összes levő alkalmazni. A program folyamatosan frissíti a kibocsátási megjegyzések; megoldás igénylő fontos problémákat megjelenése, mint hozzáadásuk. Mielőtt beállítaná a Microsoft Azure StorSimple megoldás, vegye figyelembe az alábbi információkat.  

## <a name="known-issues-in-this-release"></a>Ebben a kiadásban ismert problémák
Az alábbi táblázat összefoglalja az ebben a kiadásban ismert problémákról.  
 
| nem. | A szolgáltatás | A probléma | Megjegyzések és megoldása | Fizikai eszközök vonatkozik | Virtuális eszköz vonatkozik |
|-----|---------|-------|----------------------------|----------------------------|---------------------------|
| 1 | Gyári alaphelyzetbe állítása | Bizonyos esetekben, amikor elkészíti a gyári alaphelyzetbe állítása az StorSimple eszköz előfordulhat, hogy és az üzenet megjelenítése: **az gyári visszaállítása (fázis 8) folyamatban van**. Ez történik, ha a CTRL + C billentyűkombinációt, miközben folyamatban van a parancsmag. | Nem után nyomja le a CTRL + C kezdeményezése egy gyári alaphelyzetbe állítása. Ha már ez az állapot, lépjen kapcsolatba a Microsoft Support a következő lépéseket. | igen | nem |
| 2 | Lemezen kvórum | Ritkán Ha egy 8600 eszköz EBOD helyiségben lemezt többségét kapcsolata nincs lemez kvórum, így akkor a tárhely kvótáját lesz offline. Kapcsolat nélküli maradnak, még akkor is, ha a lemez vannak újra. | Meg kell indítsa újra az eszközt. Ha a probléma nem szűnik meg, lépjen kapcsolatba a Microsoft Support a következő lépéseket. | igen | nem |
| 3 | Felhőalapú pillanatkép hibák | Ritkán felhőalapú pillanatkép előfordulhat, hogy hiba miatt sikertelen a **maximális biztonsági korlát elérhető**. Ez akkor fordul elő, ha nagyobb, mint 255 online klónok ugyanarra az eszközre, a törlése után ugyanazon eredeti kötet. | | igen | igen |
| 4 | Helytelen vezérlő azonosító | A vezérlő helyettesítő végrehajtásakor vezérlő 0 is megjelennek majd vezérlő 1. Során vezérlő feltölteni a kép a peer csomópont betöltésekor a vezérlő azonosító is megjelennek az eredetileg majd a peer vezérlő azonosítójával. Ritkán a rendszer újraindítása után is a jelenség is látható. | Felhasználói beavatkozás nélkül szükség. Ez a helyzet feloldja, magát a vezérlő helyettesítő befejeződése után. | igen | nem |
| 5 | Eszköz diagramok figyelése | Az ellenőrző eszköz diagramok nem működnek, ha egyszerű vagy NTLM-hitelesítés engedélyezve van a proxy beállítása az eszköz a StorSimple-kezelő szolgáltatás. | Módosítsa az eszközt úgy, hogy a hitelesítés nincs értékre van állítva a StorSimple kezelő szolgáltatás regisztrált webes proxybeállításait. Ehhez futtassa a a Windows PowerShell-StorSimple Set-HcsWebProxy parancsmag. | igen | igen |
| 6 | Tárterület-fiókok | A tároló szolgáltatás törlése a tárterület-fiók használata egy nem támogatott forgatókönyv. Ez a helyzet, amelyben a felhasználói adatok nem lehet beolvasni vezet. | | igen | igen |
| 7 | Visszaállás | Nem támogatott egy visszaállás vészhelyreállítás (DR) 24 órán belül. | | igen | nem |
| 8 | Eszköz feladatátvevő | Több feladatátadás egy mennyiségi tároló forrás ugyanarra az eszközre másik célt eszközök nem támogatott. Egyetlen csendes eszközről áttérni különféle eszközökön láthatóvá válik a mennyiségi tárolók az első fölé eszközt nem sikerült adatok tulajdonjoga elvesznek. Egy feladatátvétel után következő mennyiségi tárolók jelennek meg, vagy viselkedésük eltérő, ha az Azure klasszikus portálon megtekintése. | | igen | nem |
| 9 | Telepítés | Során StorSimple adaptert SharePoint-telepítését meg kell adnia egy eszköz IP a telepítéshez sikeres befejezéséhez. | | igen | nem |
| 10 | Hálózati kapcsolatok | Hálózati kapcsolatok adatok 2 és 3 adatok meg a szoftver voltak cserélni. | Kérjük, forduljon a Microsoft Support, ha módosítani szeretné ezeket a felületek beállítása. | igen | nem |


 
