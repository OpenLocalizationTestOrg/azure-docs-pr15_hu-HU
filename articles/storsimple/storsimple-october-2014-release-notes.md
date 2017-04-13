<properties 
    pageTitle="StorSimple 8000 frissítés 0.1-es kibocsátási megjegyzések |} Microsoft Azure"
    description="Új szolgáltatások és a javítások, a Megnyitás problémák és a rendelkezésre álló megoldások ismerteti az október 2014-es Microsoft Azure StorSimple kiadás (frissítés 0,1)."
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
    ms.date="09/21/2016"
    ms.author="alkohli" />

# <a name="storsimple-8000-series-update-01-release-notes--october-2014"></a>StorSimple 8000 sorozat frissítés 0.1-es kibocsátási megjegyzések – október 2014-es  

## <a name="overview"></a>– Áttekintés

A következő kibocsátási megjegyzések StorSimple 8000 sorozat frissítés jelent meg október 2014-es 0,1 azonosítása: a megnyitott fontos problémákat. Is tartalmazhatnak a jelen kiadásban helyet kapott StorSimple szoftver- és belső vezérlőprogram frissítések listáját. Az első megjelenés az után a StorSimple 8000 sorozat kiadásával általában elérhető a július 2014-es és szoftver verziószáma 6.3.9600.17312 felel meg.  

Azt javasoljuk, hogy keresése hivatkozásra, és minden olyan elérhető frissítést alkalmazni, az eszköz telepítése után azonnal. Is bekapcsolhatja az automatikus frissítések letöltéséhez és telepítéséhez fontos frissítéseket a Microsofttól, amint elengedi őket. További tudnivalókért lásd: [az StorSimple eszköz](storsimple-update-device.md)frissítése.  

Tekintse át a kibocsátási megjegyzések mielőtt beállítaná a frissítéseket a StorSimple megoldás tárolt adatok számára.  

>[AZURE.IMPORTANT]
> 
-   Használja a StorSimple kezelő szolgáltatás, és nem a Windows PowerShell-StorSimple az október frissítések telepítéséhez.  
-   A frissítések kell tudni a 3 órát általában befejezéséhez igénybe vehet.  
-   StorSimple október kiadását nem tartalmaz valamilyen frissítés a StorSimple virtuális eszközt. Érhető el Windows-frissítések, többek között a legutóbbi biztonsági javításokat, továbbra is alkalmazhat, de nem jelenik meg a virtuális eszközökre készült verziója változását.  

Győződjön meg arról, hogy az alábbi előfeltételek teljesülése StorSimple eszköze frissítése előtt.  

- Győződjön meg arról, hogy mindkét eszköz vezérlők futnak, mielőtt a frissítések keresése. Ha bármelyik vezérlő nem fut, a beolvasott képet sikertelen lesz. Győződjön meg róla, hogy a vezérlők megfelelő állapotba kerül, lépjen a **karbantartása** lap a **Hardveres állapotát** . Ha összetevők, hogy **a figyelmet segítségre van szüksége**, forduljon a Microsoft Support a folytatás előtt minden.  
- Győződjön meg arról, hogy rögzített IP-címei mindkét vezérlő 0 és 1 vezérlő átirányítható, is csatlakozik az internethez, ezek a frissítéseket, hogy az eszköz karbantartási használja. A [kapcsolat tesztelése parancsmag](https://technet.microsoft.com/library/hh849808.aspx) egy ismert cím, a hálózaton, például Outlook.com-fiókkal, ellenőrizze, hogy a vezérlőnek a külső hálózat elérhetőségének kívüli pingelése is használhatja.  
- Győződjön meg arról, hogy a szükséges kimenő legyen elérhető a kimenő kommunikáció StorSimple készülékén. További tudnivalókért olvassa el a [hálózati StorSimple eszközére vonatkozó követelmények](storsimple-system-requirements.md#networking-requirements-for-your-storsimple-device)című témakört.  
- Ha az eszköz szoftver verziószáma 6.3.9600.17312 (október 2014-es frissítés) régebbi, ha engedélyezve van, a frissítés megkezdése előtt tiltsa le az adatok 2 és 3 adatok portokat. Ha hagyja az adatok 2 vagy adatok 3 portok engedélyezett, ha a frissítés alkalmazása, akkor azt a eszköz vezérlő helyreállítási módba jelenhet meg. Felhívjuk a figyelmét arra, hogy a hálózati kapcsolatok letiltásakor összes társított kötet kell tenni kapcsolat nélkül, és az I/O fog hibásan fognak működni, a frissítés időtartamára.  

## <a name="whats-new-in-the-october-release"></a>Az október kiadás újdonságai

A frissítés az alábbi javításokat tartalmazza:

- A felhasználói felület-StorSimple kezelő szolgáltatás kezelése a eszköz vezérlők most már használhatja. Az adatkezelési műveletek felvétele a vezérlőn be- és indítsa újra a Leállítás. További információért lépjen [StorSimple kezelése eszköz vezérlők](storsimple-manage-device-controller.md).  
- WAN sávszélesség felosztás szerint--a-hét napja és az idő nap kombinációk ütemezheti. Ez lehetővé teszi, hogy közben essen WAN-sávszélesség hatékonyabb használatát. Különböző sávszélesség-sablonok különböző mennyiségi tárolók kijelölésének engedélyezése További információért lépjen [a StorSimple sávszélesség-sablonok kezelése](storsimple-manage-bandwidth-templates.md).  
- Ezzel kapcsolatban beérkező értesíteni kell a helyi bejelentkezést és a meglévő, illetve esetleg a közelgő problémák másokat az értesítő e-mailek úgy is beállíthatja. További információért lépjen [értesítési beállítások konfigurálása](storsimple-manage-alerts.md#configure-alert-settings).  

## <a name="issues-fixed-in-the-october-release"></a>Az október kiadásban javított


Az alábbi táblázat összefoglalja a frissítés javított hibák.  

| nem. | A szolgáltatás | A probléma | Fizikai eszközök vonatkozik | Virtuális eszköz vonatkozik |
|-----|---------|-------|---------------------------------|--------------------------------|
| 1 | Hálózati kapcsolatok | A korábbi verzióban a hálózati kapcsolatok adatok 2 és 3 adatok meg a szoftver voltak cserélni. Ez a frissítés megoldását. Törölje a beállításokat, és tiltsa le a hálózati kapcsolatok, a frissítés telepítése előtt. A frissítés telepítése után be kell újrakonfigurálni az alábbi felületek. | igen | nem |
| 2 | Támogatás csomag | A korábbi verzióban, ha futtatta a **Export-HcsSupportPackage** a Windows PowerShell-parancsmag a Baseboard Management vezérlő (BMC) naplók beolvasásához a művelet nem sikerült a következő hibaüzenettel: "a művelet sikeresen befejeződött a vezérlőn, de a peer vezérlőn, a következő hibák miatt sikertelen volt. Ellenőrizze megfelelő-e a partner, és hogy az aktuális csomópont a peer csatlakozhat." Most már megoldódott a probléma. | igen | nem |
| 3 | Eszköz feladatátvevő | A korábbi verzióból volt a névjegyadatok adatok eltérést tapasztal, ha a **biztonsági másolat észleljék** feladat eszköz meghibásodáskor sikertelen volt. Most már megoldódott a probléma. | igen | nem |
| 4 | Eszköz feladatátvevő | A korábbi verzióból, egy eszköz feladatátvétel után a biztonsági másolatok volt látható, de a hozzárendelt mennyiség tároló nem található meg a célként megadott eszköz. Most már megoldódott a probléma. | igen | nem |
| 5 | Eszköz feladatátvevő | A korábbi verzióból volt a hiba az a felhő biztonsági másolatok felsorolása, amely adatok ellentmondás okozhat, ha volt a felhőben csatlakozási problémákat tapasztal a beállításjegyzék-visszaállítási művelet során. | igen | nem |
| 6 | Belső vezérlőprogram frissítése | A korábbi verzióból az eszköz belső vezérlőprogram frissítési feladat nem sikerült, és jelenik meg, hogy a parancsmagok nem ismerhető fel, és azzal, hogy a frissítés sikertelen eredményt hiba történt. A vezérlő majd találja a hiba helyreállítási módba. Most már megoldódott a probléma. | igen | nem |
| 7 | Telepítés | Az eszköz nem készül telepíteni megfelelően a telepítés során okozta hibát most rögzített rendelkeznek. | igen | nem |
| 8 | Gyári alaphelyzetbe állítása | Most már kihagyhatja tetszés szerint gyári alaphelyzetbe belső vezérlőprogram ellenőrzése. Ez a változás az előző verzióból származó. | igen | nem |
| 9 | Gyári alaphelyzetbe állítása | A korábbi verzióból a gyári alaphelyzetbe parancsmag futtatásakor belső vezérlőprogram verzió szempontú vizsgálatok végzett csak az egyes hardver összetevői. További belső vezérlőprogram ellenőrzések az első újraindítás a folyamat, amelyek miatt sikertelen lesz a alaphelyzetbe állítása után történtek. Ez a fix összes a belső vezérlőprogram ellenőrzéseket gyári alaphelyzetbe parancsmag fut, és indítsa újra a mielőtt az első rendszer ellenőrzi. | igen | nem |
| 10 | Tárterület fiók kulcs elforgatása | A most elforgatása a tárhely fiók billentyűk segítségével **Invoke-HcsmServiceDataEncryptionKeyChange** parancsmagot a felhasználónak meg kell adnia az adatok titkosítókulcs kéri. Ez egy szövegközi paraméterként átadott adatok titkosítókulcs, amelyben a korábbi verzióból származó módosítás. | igen | nem |
| 11 | Visszaállás 24 órán belül | Során vészhelyreállítás a karbantartás forrás az eszközre did nem fordulhat elő, tisztán, okozó visszaállás sikertelen lesz. A probléma javítását ebben a verzióban. | igen | nem |

## <a name="known-issues-in-the-october-release"></a>Az október kiadásának ismert problémák

Az alábbi táblázat összefoglalja az ebben a kiadásban ismert problémákról.

| nem. | A szolgáltatás | A probléma | Megjegyzések és megoldása | Fizikai eszközök vonatkozik | Virtuális eszköz vonatkozik |
|-----|---------|-------|----------------------------|----------------------------|---------------------------|
| 1 | Gyári alaphelyzetbe állítása | Bizonyos esetekben, amikor elkészíti a gyári alaphelyzetbe állítása az StorSimple eszköz előfordulhat, hogy és az üzenet megjelenítése: **az gyári visszaállítása (fázis 8) folyamatban van**. Ez történik, ha a CTRL + C billentyűkombinációt, miközben folyamatban van a parancsmag. | Nem után nyomja le a CTRL + C kezdeményezése egy gyári alaphelyzetbe állítása. Ha már ez az állapot, kérjük, forduljon Microsoft Support a következő lépéseket. | igen | nem |
| 2 | Gyári alaphelyzetbe állítása | Végezze el a nem gyári alaphelyzetbe október 2014-es kiadás a frissül StorSimple eszközt engedje fel. | Ez a művelet csak akkor fog működni, ha telepítve van a javítást. Lépjen kapcsolatba a Microsoft Support megszerezni a szükséges javítást. | igen | nem | 
| 3 | Lemezen kvórum | Ritkán Ha egy 8600 eszköz EBOD helyiségben lemezt többségét kapcsolata nincs lemez kvórum, így akkor a tárhely kvótáját lesz offline. Kapcsolat nélküli maradnak, még akkor is, ha a lemez vannak újra. | Meg kell indítsa újra az eszközt. Ha a probléma nem szűnik meg, lépjen kapcsolatba a Microsoft Support a következő lépéseket. | igen | nem |
| 4 | Felhőalapú pillanatkép hibák | Ritkán felhőalapú pillanatkép előfordulhat, hogy hiba miatt sikertelen a **maximális biztonsági korlát elérhető**. Ez akkor fordul elő, ha nagyobb, mint 255 online klónok ugyanarra az eszközre, a törlése után ugyanazon eredeti kötet. | | igen | igen |
| 5 | Helytelen vezérlő azonosító | A vezérlő helyettesítő történik, amikor vezérlő 0 is megjelennek majd vezérlő 1. Során vezérlő feltölteni a kép a peer csomópont betöltésekor a vezérlő azonosító is megjelennek az eredetileg majd a peer vezérlő azonosítójával. Ritkán a rendszer újraindítása után is a jelenség is látható. |Felhasználói beavatkozás nélkül szükség. Ez a helyzet feloldja, magát a vezérlő helyettesítő befejeződése után. | igen | nem |
| 6 | Eszköz diagramok figyelése | Az ellenőrző eszköz diagramok nem működnek, ha egyszerű vagy NTLM-hitelesítés engedélyezve van a proxy beállítása az eszköz a StorSimple-kezelő szolgáltatás. | Módosítsa az eszközt úgy, hogy a hitelesítés nincs értékre van állítva a StorSimple kezelő szolgáltatás regisztrált webes proxybeállításait. Ehhez futtassa a a Windows PowerShell-StorSimple Set-HcsWebProxy parancsmag. | igen | igen |
| 7 | Tárterület-fiókok | A tároló szolgáltatás törlése a tárterület-fiók használata egy nem támogatott forgatókönyv. Ez a helyzet, amelyben a felhasználói adatok nem lehet beolvasni vezet. | | igen | igen |
| 8 | Eszköz feladatátvevő | Több feladatátadás egy mennyiségi tároló forrás ugyanarra az eszközre másik célt eszközök nem támogatott. | Egyetlen csendes eszközről áttérni különféle eszközökön láthatóvá válik a mennyiségi tárolók az első fölé eszközt nem sikerült adatok tulajdonjoga elvesznek. Egy feladatátvétel után következő mennyiségi tárolók jelennek meg, vagy viselkedésük eltérő, ha az Azure klasszikus portálon megtekintése. | igen | nem |
| 9 | Telepítés | Során StorSimple adaptert SharePoint-telepítését meg kell adnia egy eszköz IP ahhoz, hogy a telepített sikeres befejezéséhez.    | | igen | nem |
| 10 | Webhely-proxy | Ha a webes proxy konfigurációjának a megadott protokoll HTTPS-t, az eszköz-szolgáltatás kommunikációs hatással lesz, és az eszköz offline módba. A folyamat használata az eszközön jelentős erőforrások más támogatási csomag is jön létre. | Győződjön meg arról, hogy rendelkezik-e a megadott protokolljaként HTTP proxy webes URL-CÍMÉT. További információt [az eszköz konfigurálása webes proxyjának](storsimple-configure-web-proxy.md)olvashat. | igen | nem |
| 11 | Webhely-proxy | Ha konfigurálása és webes proxy regisztrált eszközön engedélyezése, majd meg kell indítsa újra az aktív vezérlő az eszközön. | | igen | nem |
| 12 | Magas felhő időtartama és magas I/O terhelést | StorSimple eszköze nagyon magas felhő késések (másodperc sorrendben) és a magas I/O terhelést észlel, ha az eszköz kötet csökkentett állapotba lépjen, és a műveletekkel együtt során, melyről a "nem áll készen eszközt" hibaüzenet. | Manuálisan indítsa újra az eszközt vezérlők, és végezze el az eszköz áttérni ezt a helyzetet helyreállítása kell. | igen | nem |

## <a name="physical-device-updates-in-the-october-release"></a>Engedje fel a fizikai eszköz frissítése az október

Ha a frissítések fizikai eszközök alkalmaznak, a szoftver verziószáma 6.3.9600.17312 változik. Hiányában, ezek kibocsátási megjegyzések az StorSimple eszköz összes levő alkalmazni. Az alábbi frissítéseket kapcsolatos további tudnivalókért lásd: [frissítés a Microsoft Azure StorSimple készülék október 2014-es fizikai készülék szoftver](http://support.microsoft.com/kb/2986997).  

## <a name="serial-attached-scsi-sas-controller-and-firmware-updates-in-the-october-release"></a>Belső vezérlőprogram frissítéseket, az október és a soros csatolt SCSI () vezérlő kiadás

Ebben a kiadásban a vezető és a vezérlő a fizikai eszköz a belső vezérlőprogram frissíti. További információt a Társítások vezérlő frissítése című témakör [a Microsoft Azure StorSimple készülék LSI Társítások vezérlők frissítése október 2014-es](http://support.microsoft.com/kb/2987020).   

Ebben a kiadásban is érinti: a göngyölt vezérlőprogram-frissítést, amely megszünteti a eszköz hardver-összetevő kapcsolatos problémák. A belső vezérlőprogram frissítésével kapcsolatos további tudnivalókért olvassa el a [frissítés a Microsoft Azure StorSimple készülék október 2014-es belső vezérlőprogram](http://support.microsoft.com/kb/2987015)című témakört.  

## <a name="virtual-device-updates-in-the-october-release"></a>Engedje fel az az október virtuális eszköz frissítése

Ebben a kiadásban nem tartalmaz a virtuális eszköz e frissítéseket. A frissítés telepítése nem változik a virtuális eszköz szoftver verziószáma.
 
