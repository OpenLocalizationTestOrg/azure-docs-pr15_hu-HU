<properties 
   pageTitle="StorSimple 8000 sorozat frissítés 3 kibocsátási megjegyzések |} Microsoft Azure"
   description="Az új funkciók, problémák és megoldások StorSimple 8000 sorozat frissítés 3 ismerteti."
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
   ms.date="10/14/2016"
   ms.author="alkohli" />

# <a name="storsimple-8000-series-update-3-release-notes"></a>StorSimple 8000 sorozat frissítés 3 kibocsátási megjegyzések  

## <a name="overview"></a>– Áttekintés

A következő kibocsátási megjegyzések az új szolgáltatásokat ismertetik, és a kritikus megnyitott kapcsolatos problémák azonosítása StorSimple 8000 sorozat frissítés 3. Is tartalmazhatnak a jelen kiadásban helyet kapott StorSimple szoftverfrissítések listáját. 

Frissítés 3 bármely StorSimple eszköz kiadás (kiadás) vagy a frissítés 0,1 átnézett frissítés 2.2 alkalmazhatók. Az eszköz társított frissítés 3-as verziója 6.3.9600.17759.

Tekintse át a kibocsátási megjegyzések mielőtt beállítaná a frissítést a StorSimple megoldás tárolt adatok számára.

>[AZURE.IMPORTANT]
> 
> - Frissítés 3 eszközhöz LSI illesztőprogram és belső vezérlőprogram és Storport van, és Spaceport frissíti. A frissítés körülbelül 1.5-2 órán vesz igénybe. 

> - Az új verziókban, esetleg nem látható frissítések azonnal, mert a frissítéseket a szakaszos bevezetésének mi. Néhány napot várni, és amint majd újra, ezek a frissítések keresése lesz elérhető.


## <a name="whats-new-in-update-3"></a>A frissítés 3 újdonságai

Az alábbi főbb változásokkal és hibajavítások esett frissítés 3.

 
- **Automatikus térköz visszanyerése módosításokat** – frissítés 3 kezdve, a hely visszanyerése algoritmusok futtassa a készenléti vezérlő, a rendszer, így gyorsabban végrehajtása. A hely visszanyerése végezhető szükséges portokon további tudnivalókért tekintse át a [hálózati követelmények StorSimple](storsimple-system-requirements.md#networking-requirements-for-your-storsimple-device).

- **Teljesítménnyel kapcsolatos fejlesztések** – frissítés 3 megújult a felhőbe írási és olvasási teljesítményét.

- **Áttelepítési kapcsolatos fejlesztések** : az ebben a kiadásban számos hibajavítás és javítása az befejeződött volna az áttelepítési szolgáltatás 5000/7000 sorozat eszközökről 8000 sorozat eszközöket. További információt az áttelepítési szolgáltatás használatát nyissa meg [az 5000/7000 sorozat eszközről 8000 sorozat eszközre áttelepítési](https://www.microsoft.com/en-us/download/details.aspx?id=47322). 

- A **kapcsolódó javítások figyelése** - ebben a kiadásban, diagramokat, szolgáltatás irányítópult és eszköz irányítópult figyelése kapcsolatos hibák lettek javítva.


## <a name="issues-fixed-in-update-3"></a>A frissítés 3 javított

Az alábbi táblázatban a frissítés 3 javított hibák összefoglalását tartalmazza.    

| nem | A szolgáltatás                                    | A probléma                                                                                                                                                                                                                                                                                        | Fizikai eszközök vonatkozik | Virtuális eszköz vonatkozik |
|----|--------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------|---------------------------|
| 1  | A Host melletti adatok áttelepítése                      | A korábbi verzióban, a StorSimple felhő készülék volt kapcsolat nélküli módba host melletti adatok áttelepítés során. Ez a probléma megoldódott, ebben a verzióban.                                                | nem                        | igen                        |
| 2  | Helyi meghajtóra rögzített kötet                     | A korábbi verzióból voltak I/O hibák, a mennyiségi átalakítási hibáinak és a helyi meghajtóra rögzített kötet datapath hibák kapcsolatos problémákat. Ezeket a problémákat legfelső szintű okozott volt, és ebben a kiadásban rögzített.                | igen                        | nem                       |
| 3  | Figyelése                                    | Jelentési egységek, és hol helytelen adatokat helyileg rögzített kötet megjelent eszköz irányítópult-diagramok és figyelése több problémák volt. Ebben a kiadásban javított ezeket a problémákat. | igen                         | nem                       |
| 4  | Nehéz írások I/O                          | Ha StorSimple munkaterhelésekből érintő nehéz írások használata esetén a felhasználó volna fellépő esetleges egy ritka programhibák hol munkakészlet lett éppen többszintű be a felhőben. Ez a hiba megoldódott, ebben a verzióban. | igen                        | igen                       |
| 5  | Biztonsági másolat                                     | Bizonyos esetekben a ritka, szoftver, a korábbi verzióiban, amikor a felhasználó elvégez egy távoli adatfeliratsor biztonsági másolatot készít, őket kellene problémába felhő hibák, és a művelet volna hiba jelenik meg. Ebben a kiadásban a probléma megoldódott, és a művelet sikeresen befejeződött.             | igen                        | igen                       |
| 6  | Biztonsági másolat házirend                                     | Bizonyos esetekben a ritka a szoftver, a korábbi verziókban történt, a Törlés biztonsági házirend kapcsolatos hiba. Ez a probléma megoldódott, ebben a verzióban.       | igen                        | igen                       |



## <a name="known-issues-in-update-3"></a>Ismert problémák a frissítés 3

Az alábbi táblázat összefoglalja az ebben a kiadásban ismert problémákról.

| nem. | A szolgáltatás | A probléma | Megjegyzések és megoldása | Fizikai eszközök vonatkozik | Virtuális eszköz vonatkozik |
|-----|---------|-------|----------------------------|----------------------------|---------------------------|
| 1 | Lemezen kvórum | Ritkán Ha egy 8600 eszköz EBOD helyiségben lemezt többségét kapcsolata nincs lemez kvórum, így a tárhely kvótáját fog látogassa meg offline. Kapcsolat nélküli maradnak, még akkor is, ha a lemez vannak újra. | Meg kell indítsa újra az eszközt. Ha a probléma nem szűnik meg, lépjen kapcsolatba a Microsoft Support a következő lépéseket. | igen | nem |
| 2 | Helytelen vezérlő azonosító | A vezérlő helyettesítő végrehajtásakor vezérlő 0 is megjelennek majd vezérlő 1. Során vezérlő feltölteni a kép a peer csomópont betöltésekor a vezérlő azonosító is megjelennek az eredetileg majd a peer vezérlő azonosítójával. Ritkán a rendszer újraindítása után is a jelenség is látható. | Felhasználói beavatkozás nélkül szükség. Ez a helyzet feloldja, magát a vezérlő helyettesítő befejeződése után. | igen | nem |
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
| 16 |Helyi meghajtóra rögzített kötet szabad lemezterület | Ha töröl egy helyi rögzített mennyiség, az új kötet rendelkezésre álló hely előfordulhat, hogy nem frissülnek azonnal. A StorSimple kezelő szolgáltatás frissíti a helyi lemezterület óránként körülbelül.| Várja meg egy óra próbál létrehozni az új kötet előtt. | igen | nem |
| 17 | Helyi meghajtóra rögzített kötet | A visszaállítási feladat az ideiglenes biztonságimásolat-pillanatfelvétel a biztonságimásolat-katalógust, de csak a visszaállítási feladat időtartamának közzététele. Emellett az előtag **tmpCollection** **Biztonsági házirendek** lapon, de csak a visszaállítási feladat időtartamának pillanatkép csoport közzététele azt. | Ez akkor fordulhat elő, ha a visszaállítási feladat van csak a helyi számítógépre rögzített, kötet vagy kombinálja a helyi meghajtóra rögzített és többszintű kötet. Ha a visszaállítási feladat csak többszintű kötet tartalmazza, majd a probléma nem történik. Felhasználói beavatkozás nélkül szükség. | igen | nem |
| 18 | Helyi meghajtóra rögzített kötet | Ha megszakítja a visszaállítási feladat és a vezérlő feladatátvevő fordul elő a közvetlenül ezt követően a visszaállítási feladat jelennek meg: **nem sikerült** **Visszavonva**helyett. Ha egy visszaállítási feladat sikertelen, és a vezérlő feladatátvétel azonnal ezután jelennek meg a visszaállítási feladat **megszakad** helyett **sikertelen volt**. | Ez akkor fordulhat elő, ha a visszaállítási feladat van csak a helyi számítógépre rögzített, kötet vagy kombinálja a helyi meghajtóra rögzített és többszintű kötet. Ha a visszaállítási feladat csak többszintű kötet tartalmazza, majd a probléma nem történik. Felhasználói beavatkozás nélkül szükség. | igen | nem |
| 19 |Helyi meghajtóra rögzített kötet | Ha megszakítja a visszaállítási feladat, vagy ha nem sikerül a visszaállítás, majd vezérlő feladatátvétel, egy további visszaállítási feladat megjelenik a **feladatok** lapon. | Ez akkor fordulhat elő, ha a visszaállítási feladat van csak a helyi számítógépre rögzített, kötet vagy kombinálja a helyi meghajtóra rögzített és többszintű kötet. Ha a visszaállítási feladat csak többszintű kötet tartalmazza, majd a probléma nem történik. Felhasználói beavatkozás nélkül szükség. | igen | nem |
| 20 |Helyi meghajtóra rögzített kötet | Ha megpróbál (létrehozott és frissítés 1.2-es verziójával klónozott vagy régebbi) többszintű kötet konvertálása helyi rögzített kötet és az eszköz fogy a hely, vagy egy felhőalapú üzemszünetek van, a clone(s) is sérült.| A probléma csak a kötet, amelyeket létrehozott, és a klónozott előtti Update 2.1 szoftver. Ez egy ritka forgatókönyv kell.|
| 21 | Mennyiségi átalakítás | Nem frissülnek a kötet csatolva, miközben folyamatban van a mennyiségi konvertálás ACRs (helyben kiemelt többszintű vagy fordítva). A ACRs frissítése eredményezhet adatsérülés. | Ha szükséges, frissítse a mennyiségi konvertálás előtt a ACRs és nem minden további ACR frissítések miközben folyamatban van a konvertálás. |
| 22-es hibakód | Frissítések | Frissítés 3 alkalmazásakor az Azure klasszikus portálon a **karbantartása** lap frissítés 2 kapcsolódik, a következő üzenet jelenik meg, – "StorSimple 8000 sorozat 2 frissítés tartalmaz képességét a Microsoft ezzel kapcsolatban beérkező gyűjtendő adatok az eszközről, ha azt észleli, hogy a lehetséges problémák". Ez félrevezető lehet, ahogy azt jelzi, hogy a frissítés 2 frissíteni az eszközt. Miután az eszköz succeesfully frissítés 3 frissíteni, ez az üzenet eltűnnek. | Ez a probléma az elkövetkező kiadásokban kijavítjuk. | igen | nem |


## <a name="controller-and-firmware-updates-in-update-3"></a>A frissítés 3 vezérlő és a belső vezérlőprogram frissítések

Ebben a kiadásban van LSI illesztőprogram és a belső vezérlőprogram. További információt a LSI vezető és a belső vezérlőprogram frissítések telepítési eszközön című témakörben [telepítse a frissítés 3](storsimple-install-update-3.md) StorSimple.

 
## <a name="virtual-device-updates-in-update-3"></a>A frissítés 3 virtuális eszköz frissítése

A frissítés nem alkalmazható a StorSimple felhő készülék (más néven a virtuális eszközt). Új virtuális eszközök kell létrehozni. 


## <a name="next-step"></a>Következő lépés

Ismerje meg, [telepítse a frissítés 3](storsimple-install-update-3.md) StorSimple eszközén.
