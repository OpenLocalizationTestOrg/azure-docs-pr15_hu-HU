<properties 
   pageTitle="StorSimple 8000 sorozat frissítés 1.2-es kibocsátási megjegyzések |} Microsoft Azure"
   description="Az új funkciók, problémák és megoldások StorSimple 8000 sorozat frissítés 1.2-es ismerteti."
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
   ms.date="08/18/2016"
   ms.author="alkohli" />

# <a name="storsimple-8000-series-update-12-release-notes"></a>StorSimple 8000 sorozat frissítés 1.2-es kibocsátási megjegyzések  

## <a name="overview"></a>– Áttekintés

A következő kibocsátási megjegyzések az új szolgáltatásokat ismertetik, és a kritikus megnyitott kapcsolatos problémák azonosítása a StorSimple 8000 sorozat frissítés 1.2. Is tartalmazhatnak a StorSimple szoftver, vezető és lemez belső vezérlőprogram frissítések jelen kiadásban helyet kapott listáját. 

Frissítés 1.2-es kiadás kiadás, frissítés 0,1 értéket, frissítés 0,2 vagy 0,3 Update szoftver futtató StorSimple készülék alkalmazhatók. Frissítés 1.2-es nem érhető el, ha az eszköz frissítés 1 vagy a frissítés 1.1-es operációs rendszert futtató. Ha eszköze problémamentesen működik megjelenés kiadás, tájékoztassa a [lépjen kapcsolatba a Microsoft terméktámogatási](storsimple-contact-microsoft-support.md) segítséget nyújtanak a frissítés telepítése.

Az alábbi táblázat a megfelelő frissítések 1, 1.1-es és 1.2-es eszköz szoftver-verziók.

| Ha a frissítés futtatása... | Ez az eszköz szoftver verziószáma. |
|---------------------|---------------------------------------|
| Az 1.2 frissítése          | 6.3.9600.17584                        |
| 1.1-es frissítés          | 6.3.9600.17521                        |
| 1.0 frissítése          | 6.3.9600.17491                        |

Tekintse át a kibocsátási megjegyzések mielőtt beállítaná a frissítést a StorSimple megoldás tárolt adatok számára. További tudnivalókért lásd: a [frissítés 1.2-es StorSimple eszközén](storsimple-install-update-1.md)telepítése. 

>[AZURE.IMPORTANT]
> 
- A frissítés (beleértve a Windows-frissítések) körülbelül 5-10 óra vesz igénybe. 
- Frissítés 1.2-es szoftver, a LSI illesztőprogram és a lemez belső vezérlőprogram frissítések tartalmaz. Szeretné telepíteni, kövesse a [frissítés 1.2-es StorSimple eszközén telepítése](storsimple-install-update-1.md).
- Az új verziókban, esetleg nem látható frissítések azonnal, mert a frissítéseket a szakaszos bevezetésének mi. Néhány nap ismét a frissítések keresése, ezek lesz elérhető hamarosan.


## <a name="whats-new-in-update-12"></a>A frissítés 1.2-es újdonságai

Ezek a funkciók először megjelentek frissítés 1 végrehajtott módosítások felhasználó korlátozta rendelkezésére. A frissítés 1.2-es verzió a StorSimple felhasználók többsége szeretné tanulmányozza az alábbi új szolgáltatásokat és a fejlesztések:

- **5000-7000 sorozatból 8000 sorozat eszközökre való áttelepítés** – ebben a kiadásban egy új áttelepítési szolgáltatás, amely lehetővé teszi a StorSimple 5000-7000 sorozat készülék felhasználói adataik áttelepítése a StorSimple 8000 sorozat fizikai készülék vagy egy virtuális készülék vezet be. Két fő érték értékűeknek az áttelepítési szolgáltatás foglalja magában:                                                                  

    - **Üzleti folytonosságot**, mivel az 5000-7000 sorozat berendezések 8000 sorozat készülékek a meglévő adatainak áttelepítési állapotával.
    - **Továbbfejlesztett szolgáltatás által kínált 8000 sorozat készülékek**hatékony központi kezelése több készülékek keresztül StorSimple kezelő szolgáltatás, például a hardver osztály jobb, és frissítse a belső vezérlőprogram, virtuális készülékek, adatok mobilitás és a későbbi – útmutató szolgáltatásait.

    Olvassa el az [áttelepítési útmutató](http://www.microsoft.com/download/details.aspx?id=47322) StorSimple 5000-7000 sorozat áttelepítése egy 8000 sorozat eszközön olvashat. 

- **Az Azure kormányzati portálon elérhetősége** – StorSimple érhető el az Azure kormányzati portálon. Lásd: [az Azure kormányzati portálon StorSimple eszköz](storsimple-deployment-walkthrough-gov.md)telepítése.

- **Egyéb felhő szolgáltatók támogatása** – a többi felhő szolgáltatók támogatott Amazon S3, Amazon S3 Erőforrásrekordok, HP és OpenStack (beta).

- **Frissítés a legújabb tároló API** – az ebben a kiadásban StorSimple frissítette a legújabb Azure tárhelyszolgáltatáshoz API-khoz. StorSimple 8000 sorozat eszközökön futó 1 előtti Update szoftver verziók (verzió, a 0,1 értéket, 0,2 és 0,3) az Azure tároló szolgáltatás API-k július 17, 2009-nél korábbi verzióját használják. A frissített [tartalmat tároló szolgáltatás verzióinak eltávolítása](http://blogs.msdn.com/b/windowsazurestorage/archive/2015/10/19/microsoft-azure-storage-service-version-removal-update-extension-to-2016.aspx)augusztus 1, 2016, által jelzett módon ezek API-khoz fog használata ajánlott. Elengedhetetlen StorSimple 8000 sorozat frissítés 1 előtt 2016 augusztus 1 telepítését. Ha nem sikerül kéri, StorSimple eszközök leáll megfelelően működik-e.

- **Támogatja a zóna felesleges tároló (ZRS)** – a legújabb verzióra a tárhely API-k frissítése után a StorSimple 8000 sorozat támogatni fogja zóna felesleges tároló (ZRS) helyileg felesleges tároló (LRS) és Geo felesleges tároló (GRS) mellett. Ez a [cikk Azure tároló redundancia beállításai](../storage/storage-redundancy.md) ZRS részletekért olvassa el.

- **Kezdeti telepítés és frissítés felület továbbfejlesztett** – ebben a verzióban, a telepítési és a frissítési folyamat van a továbbfejlesztett. A beállítási varázsló segítségével a telepítés visszajelzést a felhasználó számára, ha a hálózati konfigurálásról és tűzfalbeállításokat helytelenek elérését. További diagnosztikai parancsmagok kapott a hálózat az eszköz hibaelhárítási segítséget. Lásd: a [telepítés cikk hibaelhárítási](storsimple-troubleshoot-deployment.md) az új diagnosztikai parancsmag hibaelhárításhoz használt további információt.

## <a name="issues-fixed-in-update-12"></a>Frissítés az 1.2-es javított

Az alábbi táblázat összefoglalja az 1.2-es, 1.1-es és 1 frissítések javított hibák.    


| nem. | A szolgáltatás | A probléma | A rögzített frissítése | Fizikai eszközök vonatkozik | Virtuális eszköz vonatkozik |
|-----|---------|-------|-----------------|---------------------------------|--------------------------------|
| 1 | A Windows PowerShell StorSimple használata | Amikor a felhasználó távolról érhető el az StorSimple eszköz StorSimple a Windows PowerShell használatával, és kattintson a a beállítási varázsló indítása, e az összeomlást, amint az adatok 0 IP vitte történt. Ezt a hibát ettől kezdve a frissítés 1 rögzített. | 1 frissítése | igen | igen |
| 2 | Gyári alaphelyzetbe állítása | Bizonyos esetekben a gyári alaphelyzetbe állítása végrehajtásakor az StorSimple eszköz ragadt vált, és ez az üzenet jelenik meg: **az gyári visszaállítása (fázis 8) folyamatban van**. Ez történt, miközben folyamatban volt a parancsmag lenyomása CTRL + C billentyűkombinációt. Most már megoldódott ezt a hibát.| 1 frissítése | igen | nem |
| 3 | Gyári alaphelyzetbe állítása | Után egy hibás kettős vezérlő gyári új, a program engedélyezte eszköz regisztrációs folytatásához. Nem támogatott a Rendszerkonfiguráció ennek következtében. Frissítés 1 hibaüzenet jelenik meg, és regisztrációs zárolva van egy eszközön, hogy a sikertelen gyár visszaállította. | 1 frissítése | igen | nem |
| 4 | Gyári alaphelyzetbe állítása | Bizonyos esetekben hamis pozitív eltérés riasztások hatványát volt. Helytelen eltéréseket riasztások már nem hozható létre frissítés 1 rendszerű eszközökön. | 1 frissítése | igen | nem |
| 5 | Gyári alaphelyzetbe állítása | A gyár alaphelyzetbe állítása befejezés előtt megszakadt, ha az eszköz megadott helyreállítási mód, és nem engedi meg elérése a Windows PowerShell StorSimple. Most már megoldódott ezt a hibát. | 1 frissítése | igen | nem |
| 6 | Vészhelyreállítás | Egy katasztrófa helyreállítási (DR) hiba rögzítésének Itt megadhatja DR nem felelnek meg a célként megadott eszköz a biztonsági másolatok feltárását során. | 1 frissítése | igen | igen |
| 7 | LED figyelése | Bizonyos esetekben LED figyelése hátulján készülék található nem jelezte megfelelő állapotot. A kék LED kikapcsolt. ADATOK 0 és adatok 1 LED villogó volt, még ha ezek felületek nem állította be. A probléma megoldását és felügyeleti LED most jelzi a helyes állapotát.  | 1 frissítése | igen | nem |
| 8 | LED figyelése | Bizonyos esetekben 1, a frissítés telepítése után az aktív vezérlő kék egyszerűsített ki van kapcsolva tétele az az aktív vezérlő azonosítása nehezen. Ez a probléma megoldását ebben a kiadásban javítást.| Az 1.2 frissítése | igen | nem |
| 9 | Hálózati kapcsolatok | A korábbi verziókban nem átirányítható az átjárók konfigurált StorSimple eszköz sikerült offline módba lépne. Ebben a kiadásban a 0-adatok útválasztási mérőszám már elvégzett a legalacsonyabb; ezért akkor is, ha más hálózati kapcsolatok felhő engedélyezni, az eszközről felhő forgalom továbbítja adatok 0 keresztül. | 1 frissítése | igen | igen | 
| 10 | Biztonsági másolatok | Frissítés 1 24 nap után meghiúsító biztonsági másolatok okozó probléma megoldását javítás frissítés 1.1-es kiadását. | 1.1-es frissítés | igen | igen |
| 11 | Biztonsági másolatok | Korábbi verziók hibáját csökkenhet a teljesítmény alacsony módosítása díjak a felhőben pillanatképek megszakításához. Ebben a kiadásban javítás rögzített ezt a hibát.| Az 1.2 frissítése | igen | igen |
| 12 | Frissítések | Ebben a kiadásban javítás frissítés 1 jelentett sikertelen frissítés és a vezérlők módba helyreállítási, okozott hibáját megoldását.| Az 1.2 frissítése | igen | igen |


## <a name="known-issues-in-update-12"></a>Ismert problémák a frissítés 1.2

Az alábbi táblázat összefoglalja az ebben a kiadásban ismert problémákról.

| nem. | A szolgáltatás | A probléma | Megjegyzések és megoldása | Fizikai eszközök vonatkozik | Virtuális eszköz vonatkozik |
|-----|---------|-------|----------------------------|----------------------------|---------------------------|
| 1 | Lemezen kvórum | Ritkán Ha egy 8600 eszköz EBOD helyiségben lemezt többségét kapcsolata nincs lemez kvórum, így akkor a tárhely kvótáját lesz offline. Kapcsolat nélküli maradnak, még akkor is, ha a lemez vannak újra. | Meg kell indítsa újra az eszközt. Ha a probléma nem szűnik meg, lépjen kapcsolatba a Microsoft Support a következő lépéseket. | igen | nem |
| 2 | Helytelen vezérlő azonosító | A vezérlő helyettesítő történik, amikor vezérlő 0 is megjelennek majd vezérlő 1. Során vezérlő feltölteni a kép a peer csomópont betöltésekor a vezérlő azonosító is megjelennek az eredetileg majd a peer vezérlő azonosítójával. Ritkán a rendszer újraindítása után is a jelenség is látható. | Felhasználói beavatkozás nélkül szükség. Ez a helyzet feloldja, magát a vezérlő helyettesítő befejeződése után. | igen | nem |
| 3 | Tárterület-fiókok | A tároló szolgáltatás törlése a tárterület-fiók használata egy nem támogatott forgatókönyv. Ez a helyzet, amelyben a felhasználói adatok nem lehet beolvasni vezet. | igen | igen |
| 4 | Eszköz feladatátvevő | Több feladatátadás egy mennyiségi tároló forrás ugyanarra az eszközre másik célt eszközök nem támogatott. Eszköz áttérni egyetlen csendes eszközről különféle eszközökön láthatóvá válik a mennyiségi tárolók az első fölé eszközt nem sikerült adatok tulajdonjoga elvesznek. Egy feladatátvétel után következő mennyiségi tárolók jelennek meg, vagy viselkedésük eltérő, ha az Azure klasszikus portálon megtekintése. | | igen | nem |
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

## <a name="physical-device-updates-in-update-12"></a>Frissítés az 1.2-es fizikai eszköz frissítése

Ha a javítás frissítése 1.2-es alkalmazott fizikai eszköz (előtt 1 módosítása verziója fut), a 6.3.9600.17584 változik a szoftver verziószáma.

## <a name="controller-and-firmware-updates-in-update-12"></a>Frissítés az 1.2-es vezérlő és a belső vezérlőprogram frissítések

Ebben a kiadásban frissíti a vezető és a lemez belső vezérlőprogram az eszközön.
 
- Társítások vezérlő frissítésével kapcsolatos további tudnivalókért lásd: [1 frissítése a Microsoft Azure StorSimple készülék LSI Társítások vezérlők](https://support.microsoft.com/kb/3043005). 

- További információt a lemez belső vezérlőprogram frissítése című témakör [lemez belső vezérlőprogram 1 frissítése a Microsoft Azure StorSimple készülék](https://support.microsoft.com/kb/3063416).
 
## <a name="virtual-device-updates-in-update-12"></a>Frissítés az 1.2-es virtuális eszköz frissítése

A frissítés nem alkalmazható a virtuális eszközt. Új virtuális eszközök kell létrehozni. 

## <a name="next-steps"></a>Következő lépések

- [Frissítés 1.2 telepítse az eszközön](storsimple-install-update-1.md).
 
