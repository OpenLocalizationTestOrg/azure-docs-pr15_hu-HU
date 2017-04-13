<properties 
   pageTitle="StorSimple virtuális tömb frissítések kibocsátási megjegyzések |} Microsoft Azure"
   description="A frissítés 0,3 futó StorSimple virtuális tömb kritikus megnyitott hibák és megoldások ismerteti."
   services="storsimple"
   documentationCenter=""
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/15/2016"
   ms.author="alkohli" />

# <a name="storsimple-virtual-array-update-03-release-notes"></a>StorSimple virtuális tömb frissítés 0,3 kibocsátási megjegyzések

## <a name="overview"></a>– Áttekintés

A következő kibocsátási megjegyzések azonosítása: a kritikus megnyitott problémákat és a megoldott a Microsoft Azure StorSimple virtuális tömb frissítések keresése hivatkozásra.

Kibocsátási megjegyzések folyamatosan frissülnek, és megjelenése megoldás igénylő fontos problémákat, ki őket hozzá. Mielőtt beállítaná a StorSimple virtuális tömb, gondosan tekintse át a kibocsátási megjegyzések tárolt adatok számára.

A szoftver verzió **10.0.10288.0**frissítés 0,3 felel meg.

> [AZURE.NOTE] Frissítések zavaró, és indítsa újra az eszközt. Ha I/O folyamatban, az eszköz a legrövidebb leállás vonz.


## <a name="whats-new-in-the-update-03"></a>A frissítés 0,3 újdonságai

Frissítés 0,3 elsősorban a hibajavítás összeállítás. Ebben a verzióban a korábbi verziójában biztonsági hibák így több hibák javított van.

## <a name="issues-fixed-in-the-update-03"></a>A frissítés 0,3 javított

Az alábbi táblázat összefoglalja az ebben a kiadásban rögzített problémák.

| nem.  | A szolgáltatás                              | A probléma                                                                                                                                                                                                                                                                                                                           |
|------|--------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 1    | Biztonsági másolatok                                |Probléma a korábbi verziója, ahol a biztonsági másolatok nem felelnek meg a fájlmegosztás befejezéséhez volt látható. Ha a probléma jelentkezett, nem felelnek meg a biztonsági mentést, és egy fontos figyelmeztetés az StorSimple kezelő szolgáltatása a felhasználó értesítése emelte. Ez a probléma nem volt hatással az adatokat, kattintson a megosztás vagy hozzáférés az adatokhoz. A kiváltóok azonosította, ebben a kiadásban rögzített volt. <br></br> A javítás nem vonatkozik a visszamenőleges megosztások, amely már láthatók a probléma. Azon ügyfelek, akik láthatók a probléma kell először 0,3 frissítést, majd lépjen kapcsolatba a Microsoft Support egy teljes rendszer biztonsági másolat a probléma megoldása. Kapcsolatba a Microsoft Support, hanem ügyfelek is visszaállíthatja egy új megosztás a szóban forgó megosztások megfelelő másolatból.                                                                                                                                                                                 |
| 2    | iSCSI                         | Probléma a hol szeretné eltűnnek a kötet, amikor egy mennyiségi a StorSimple virtuális tömbben lévő adatok másolása a korábbi verzióból volt látható. Ez a probléma rögzítésének ebben a verzióban. <br></br> A javítások érvénybe, hogy csak a kötet az újonnan létrehozott. A javítások kötet, amely már láthatók a probléma visszamenőleges nem vonatkoznak. Ügyfelek jelenítse meg a szóban forgó összekapcsolhatja az Azure klasszikus portálon keresztül, a biztonsági mentés végrehajtása e mennyiségek és majd visszaállíthatja az alábbi kötet új kötet javasolja.                                                               |


## <a name="known-issues-in-the-update-03"></a>A frissítés 0,3 ismert problémák

Az alábbi táblázat összefoglalja az ismert problémák a StorSimple virtuális tömb, és a problémákat, a korábbi verziókban való megjelenési jegyezni tartalmazza. 


| nem. | A szolgáltatás | A probléma | Megoldás: / megjegyzések |
|-----|--------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **1.** | Frissítések | A virtuális eszközök előzetes verziójában létrehozott nem lehet frissíteni egy támogatott általános elérhetősége verzióra. | Általános elérhetősége verzióval katasztrófa helyreállítási (DR) munkafolyamattal ezekre az eszközökre virtuális sikertelen fölé. |
| **2.** | Kiépített adatok lemez | Egyszer a megadott méretben adatok lemezen van kiépítve és hozta létre a megfelelő StorSimple virtuális eszközre, kell nem bontsa ki a vagy a Lekicsinyítve, az adatok lemez. Végezze el az eszköz a helyi rétegek adatai adatvesztést eredménye kísérel meg. |   |
| **3.** | A csoportházirend | Ha egy eszközt a tartományhoz, egy csoportházirend negatív hatással lehet az eszköz művelet. | Győződjön meg arról, hogy a virtuális tömb saját szervezeti egységre az Active Directory, és nem csoportházirend-objektumok (GPO) vonatkoznak rá.|
| **4.** | Helyi webes felhasználói felület | Ha nagyobb biztonság szolgáltatások az Internet Explorer (IE ESC) engedélyezett, előfordulhat, hogy bizonyos helyi felhasználói felület weblapok például hibaelhárítás és karbantartás nem működik megfelelően. A következő lapokon gombok nem is működnek. | Kapcsolja ki a nagyobb biztonság szolgáltatások az Internet Explorerben.|
| **5.** | Helyi webes felhasználói felület | A Hyper-V virtuális gépen a hálózati csatolófelületet a webhely felhasználói felület összegéhez viszonyítva 10 GB/s felületek. | Ez a probléma a Hyper-v tükröződés. A Hyper-V mindig 10 GB/s virtuális hálózati kártya jeleníti meg. |
| **6.** | Többszintű kötet és megosztás | A többszintű kötet nem támogatott StorSimple verzióval használható alkalmazások zárolása bájt tartományban. Ha bájt tartomány zárolása engedélyezve van, StorSimple tiering nem működnek. | Ajánlott mértékek a következők: <br></br>Kapcsolja ki az alkalmazás logika a zárolás bájt tartományban.<br></br>Válassza a szeretné elhelyezni az adatokat az alkalmazás nem pedig a többszintű kötet helyileg rögzített mennyiségben.<br></br>*Bővítve*: amikor a kiemelt kötet használ helyben, és bájt tartomány zárolása engedélyezve van, a helyi meghajtóra rögzített mennyiségi lehet online még azelőtt, a visszaállítás elkészült. Ezekben az esetekben ha visszaállítás van folyamatban, majd meg kell várnia a visszaállítás befejezéséhez. |
| **7.** | Többszintű megosztások | Nagyméretű fájlok használata eredményezhet lassú réteg meg. | Ha nagyobb fájlokkal dolgozik, azt javasoljuk, hogy a legnagyobb fájl, a megosztás méretű 3 %-nál kisebb. |
| **8.** | Megosztás kapacitása használt | Megjelenhet felhasználási megoszthatja, ha nincs adat a megosztott. Ennek oka az, a használt kapacitás megosztások metaadatokat tartalmaz. |   |
| **9.** | Vészhelyreállítás | Csak a kívánt fájl kiszolgáló, amely az adatforrás eszköz azonos tartományban vészhelyreállítás végezheti el. Ebben a kiadásban nem támogatott vészhelyreállítás egy másik tartományban cél eszközre. | Ez történik, az későbbi kiadásokban. |
| **10.** | Azure PowerShell | Ebben a kiadásban az Azure Powershellen keresztül nem lehet felügyelni a StorSimple virtuális eszközök. | A virtuális eszközök irányításának az Azure klasszikus portál és a felhasználói felület helyi interneten keresztül kell elvégezni. |
| **11.** | A jelszó módosítása | A virtuális tömb eszköz konzol csak fogadja el a beviteli hu-hu billentyűzet formátumban. |   |
| **12.** | CHAP | Egyszer létrehozott CHAP hitelesítő adatokat nem lehet eltávolítani. Ezenkívül CHAP hitelesítő adatok módosításakor szeretne kötet offline állapotba helyezni, és ezután füleket online a változtatások csak akkor lépnek érvénybe. | Ez a probléma címzettjei az későbbi kiadásokban. |
| **13-mal.** | iSCSI kiszolgáló  | A "használt tárterület"-iSCSI mennyiségi jelenik meg a StorSimple kezelő szolgáltatás és az iSCSI állomás eltérő lehet. | A iSCSI host a fájlrendszer nézet tartozik.<br></br>Az eszköz kiosztva, amikor a mennyiségi volt a maximális méret blokkok látja.|
| **14.** | Fájlkiszolgálóra  | Ha egy mappában található fájlhoz egy alternatív adatok adatfolyam (HIRDETÉSEK) társítva van, a HIRDETÉSEK nem készített biztonsági másolatot vagy keresztül vészhelyreállítás adatfeliratsor és elem szint helyreállítása vissza.| |


## <a name="next-step"></a>Következő lépés

[A frissítés 0,3](storsimple-ova-install-update-01.md) a StorSimple virtuális tömb.

## <a name="references"></a>Hivatkozások

Egy régebbi fontos megjegyzés keres? odamegy: 

- [StorSimple virtuális tömb frissítés 0,1 és 0,2 kibocsátási megjegyzések](storsimple-ova-update-01-release-notes.md)

- [StorSimple virtuális tömb általános elérhetősége – kibocsátási megjegyzések](storsimple-ova-pp-release-notes.md)
 

