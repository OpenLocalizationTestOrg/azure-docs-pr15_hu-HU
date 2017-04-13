<properties 
   pageTitle="StorSimple 8000 frissítés 0,2 kibocsátási megjegyzések |} Microsoft Azure"
   description="Az új szolgáltatások és a javítások, a megnyitott problémák és a rendelkezésre álló megoldásokat alkalmazhatja ismerteti a január 2015 Microsoft Azure StorSimple kiadás (frissítés 0,2)."
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
   ms.date="05/16/2016"
   ms.author="v-sharos" />


# <a name="storsimple-8000-series-update-02-release-notes---january-2015"></a>StorSimple 8000 sorozat frissítés 0,2 kibocsátási megjegyzések - január 2015

## <a name="overview"></a>– Áttekintés

A következő kibocsátási megjegyzések azonosítása: a Microsoft Azure StorSimple a január 2015 kiadásába fontos nyissa meg problémákat. Is tartalmazhatnak a jelen kiadásban helyet kapott StorSimple szoftver- és belső vezérlőprogram frissítések listáját. Ez a második kiadásának, után a StorSimple 8000 sorozat kiadásával történt július 2014-es általában elérhető.
  
A frissítés a október Update nem változtatja meg a fizikai eszközök szoftver verziószáma. Verzió 6.3.9600.17312 továbbra is. A virtuális eszköz kép által használt kép ebben a kiadásban megváltozott. Emiatt 1/20/2015 után létrehozott összes új virtuális eszközök jelennek meg a verzió 6.3.9600.17361 szerint.  

Olvassa el a következő frissítés a január 2015 kibocsátási megjegyzések tárolt adatok számára.

> [AZURE.IMPORTANT]  
>
>- A frissítés nem érhető el a Windows Update és frissítések például nem lehet telepíteni. Az eszköz nem kap a frissítés, még akkor is, ha a számítógépen telepítve a frissítéseket a Azure klasszikus portál használatával. A frissítés után 2015 január 20 létrehozott virtuális eszközök csak érvényesek lesznek. 
> 
>- Január megjelenési időpontjához StorSimple nem tartalmaz valamilyen frissítés a StorSimple fizikai eszközt. Továbbra is alkalmazhat érhető el Windows-frissítések a virtuális eszközt, a legutóbbi biztonsági javításokat, beleértve, de nem jelenik meg a StorSimple fizikai eszközökre készült verziója módosítása.

## <a name="whats-new-in-the-january-release"></a>A január kiadás újdonságai

A frissítés tartalmaz egy virtuális az eszközre az offline módra kötet kapcsolódó javítást. (Lásd [az ebben a kiadásban javított](#issues-fixed-in-the-january-release).)  

A frissítés új szolgáltatások és funkciók nem tartalmaz.  

## <a name="issues-fixed-in-the-january-release"></a>Javított a január verzióban

Az alábbi táblázat ismerteti a problémát a frissítés javított.

|nem.|A szolgáltatás|A probléma|Fizikai eszközök vonatkozik|Virtuális eszköz vonatkozik 
|---|-------|-----|--------------------------|-------------------------
|1|Kapcsolat nélküli módba kötet|Ha magas felhő késések továbbra is fennáll, percekig, StorSimple virtuális eszköz kötet kapcsolat nélküli módba a állomásokon. Ez a fix felhő késések, ezáltal minimalizálása az esetek, amikor a hosts a kapcsolat nélküli módba kötet okozhat küszöbértékét megnöveli.|nem|igen  

## <a name="known-issues-in-the-january-release"></a>Január kiadásának ismert problémák

Az alábbi táblázat összefoglalja az ebben a kiadásban ismert problémákról.
 
|nem.|A szolgáltatás|A probléma|Megjegyzések és megoldása|Fizikai eszközök vonatkozik|Virtuális eszköz vonatkozik 
|---|-------|-----|-------------------|--------------------------|-------------------------
|1| Gyári alaphelyzetbe állítása|  Bizonyos esetekben, amikor elkészíti a gyári alaphelyzetbe állítása az StorSimple eszköz előfordulhat, hogy és az üzenet megjelenítése: **az gyári visszaállítása (fázis 8) folyamatban van.** Ez történik, ha a CTRL + C billentyűkombinációt, miközben folyamatban van a parancsmag.| Nem után nyomja le a CTRL + C kezdeményezése egy gyári alaphelyzetbe állítása. Ha már ez az állapot, kérjük, forduljon Microsoft Support a következő lépéseket.|igen|nem|
|2|Lemezen kvórum| Ritkán Ha egy 8600 eszköz EBOD helyiségben lemezt többségét kapcsolata nincs lemez kvórum, így akkor a tárhely kvótáját lesz offline. Kapcsolat nélküli maradnak, még akkor is, ha a lemez vannak újra.|Meg kell indítsa újra az eszközt. Ha a probléma nem szűnik meg, lépjen kapcsolatba a Microsoft Support a következő lépéseket.|igen |nem
|3|Felhőalapú pillanatkép hibák|Ritkán felhőalapú pillanatkép előfordulhat, hogy hiba miatt sikertelen a **maximális biztonsági korlát elérhető**. Ez akkor fordul elő, ha nagyobb, mint 255 online klónok ugyanarra az eszközre, a törlése után ugyanazon eredeti kötet.||igen|igen
|4|Helytelen vezérlő azonosító|A vezérlő helyettesítő történik, amikor vezérlő 0 is megjelennek majd vezérlő 1. Során vezérlő feltölteni a kép a peer csomópont betöltésekor a vezérlő azonosító is megjelennek az eredetileg majd a peer vezérlő azonosítójával.  Ritkán a rendszer újraindítása után is a jelenség is látható.|Felhasználói beavatkozás nélkül szükség. Ez a helyzet feloldja, magát a vezérlő helyettesítő befejeződése után.|igen|nem 
|5| Eszköz diagramok figyelése|Az ellenőrző eszköz diagramok nem működnek, ha egyszerű vagy NTLM-hitelesítés engedélyezve van a proxy beállítása az eszköz a StorSimple-kezelő szolgáltatás.|Módosítsa az eszközt úgy, hogy a hitelesítés nincs értékre van állítva a StorSimple kezelő szolgáltatás regisztrált webes proxybeállításait. Ehhez futtassa a a Windows PowerShell-StorSimple Set-HcsWebProxy parancsmag.|igen|igen
|6| Tárterület-fiókok|A tároló szolgáltatás törlése a tárterület-fiók használata egy nem támogatott forgatókönyv. Ez a helyzet, amelyben a felhasználói adatok nem lehet beolvasni vezet.|| igen |  igen
|7|Eszköz feladatátvevő| Több feladatátadás egy mennyiségi tároló forrás ugyanarra az eszközre másik célt eszközök nem támogatott.| Egyetlen csendes eszközről áttérni különféle eszközökön láthatóvá válik a mennyiségi tárolók az első fölé eszközt nem sikerült adatok tulajdonjoga elvesznek. Egy feladatátvétel után következő mennyiségi tárolók jelennek meg, vagy viselkedésük eltérő, ha az Azure klasszikus portálon megtekintése.|igen|nem
|8| Telepítés|Során StorSimple adaptert SharePoint-telepítését meg kell adnia egy eszköz IP ahhoz, hogy a telepített sikeres befejezéséhez.||igen|nem
|9| Webhely-proxy|Ha a webes proxy konfigurációjának a megadott protokoll HTTPS-t, az eszköz-szolgáltatás kommunikációs hatással lesz, és az eszköz offline módba. A folyamat használata az eszközön jelentős erőforrások más támogatási csomag is jön létre.|Győződjön meg arról, hogy rendelkezik-e a megadott protokolljaként HTTP proxy webes URL-CÍMÉT. További információ [konfigurálása webes](storsimple-configure-web-proxy.md)proxyhoz az eszközön|igen |nem
|10|Webhely-proxy|  Ha konfigurálása és webes proxy regisztrált eszközön engedélyezése, majd meg kell indítsa újra az aktív vezérlő az eszközön.|| igen |nem
|11|Magas felhő időtartama és magas I/O terhelést|StorSimple eszköze nagyon magas felhő késések (másodperc sorrendben) és a magas I/O terhelést észlel, ha az eszköz kötet csökkentett állapotba lépjen, és a műveletekkel együtt során, melyről a "nem áll készen eszközt" hibaüzenet.|Manuálisan indítsa újra az eszközt vezérlők, és végezze el az eszköz áttérni ezt a helyzetet helyreállítása kell.|igen|nem

## <a name="physical-device-updates-in-the-january-release"></a>Engedje fel a fizikai eszközök frissítések januárban

A frissítés nem tartalmaz az StorSimple eszközre egyéb szükséges módosításait.

## <a name="serial-attached-scsi-sas-controller-and-firmware-updates-in-the-january-release"></a>Belső vezérlőprogram frissítéseket, januárban és a soros csatolt SCSI () vezérlő kiadás

Ebben a kiadásban nem tartalmaz valamilyen frissítés a soros csatolt SCSI () vezérlő vagy a belső vezérlőprogram. Az illesztőprogram frissítése az október, 2014 megjelenés volt. 

## <a name="virtual-device-updates-in-the-january-release"></a>Engedje fel az januárban virtuális eszköz frissítése

Ebben a kiadásban a virtuális eszköz frissített képet tartalmazza. A virtuális eszközök 2015 január 20 után létrehozott 6.3.9600.17361 szerint jelennek meg a szoftver verziószáma.

 
