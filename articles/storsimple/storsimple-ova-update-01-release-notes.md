<properties 
   pageTitle="StorSimple virtuális tömb frissítések kibocsátási megjegyzések |} Microsoft Azure"
   description="A frissítés 0,2 és 0,1 futó StorSimple virtuális tömb kritikus megnyitott hibák és megoldások ismerteti."
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
   ms.date="06/16/2016"
   ms.author="alkohli" />

# <a name="storsimple-virtual-array-update-02-and-01-release-notes"></a>StorSimple virtuális tömb 0,2 és frissítés 0,1 kibocsátási megjegyzések

## <a name="overview"></a>– Áttekintés

A következő kibocsátási megjegyzések azonosítása: a kritikus megnyitott problémákat és a megoldott a Microsoft Azure StorSimple virtuális tömb frissítések keresése hivatkozásra. (A Microsoft Azure StorSimple virtuális tömb a más néven a StorSimple helyszíni virtuális vagy a StorSimple virtuális eszköz.) 

Kibocsátási megjegyzések folyamatosan frissülnek, és megjelenése megoldás igénylő fontos problémákat, ki őket hozzá. Mielőtt beállítaná a StorSimple virtuális eszköz, gondosan tekintse át a kibocsátási megjegyzések tárolt adatok számára.

Frissítés 0,2 felel meg a szoftver verzió **10.0.10280.0**; Frissítés 0,1 **10.0.10279.0**verziója. Az alábbi szakaszokban az egyes frissítések a változások felsorolása. 

> [AZURE.NOTE] Frissítések zavaró és lesz, indítsa újra az eszközt. Ha I/O folyamatban, az eszköz merülnek fel, amelyekre legrövidebb leállás.

## <a name="issues-fixed-in-the-update-02"></a>A frissítés 0,2 javított
Frissítés 0,2 a frissítés 0,1 kívül az alábbi táblázatban ismertetett javítása az összes módosítás tartalmazza:

A szolgáltatás                              | A probléma                                                                                                                                                                                                                                                                                                                           |
--------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
Frissítések                                 | Az utolsó kiadásban frissítések nem észlelése automatikusan az Azure klasszikus portálon, így volt szükség helyi webes felületének frissítések telepítéséhez. Ez a probléma megoldódott, ebben a verzióban. 0,2 frissítés telepítése után az Azure klasszikus portálon későbbi frissítések is telepítheti.                       

## <a name="whats-new-in-the-update-01"></a>A frissítés 0,1 újdonságai

Frissítés 0,1 értéket tartalmaz, az alábbi hibajavítások és fejlesztések. 

- **Továbbfejlesztett tűrőképessége az felhő kimaradások**: Ebben a kiadásban számos hibajavítás vészhelyreállítás, biztonsági mentése, visszaállítása és a felhő kapcsolódási zavarok megelőzve tiering körül van. 

- **Továbbfejlesztett teljesítmény visszaállítása**: Ebben a kiadásban hibajavítás, amely rendelkezik jelentősen kivágása lefelé a visszaállítási feladatok befejezési időpontját magában.

- **Az Automatikus térköz visszanyerése optimalizálási**: adatok törlésekor a vékonyan kiépített kötet még nem használt tárterület blokkok nyerhető kell. Ebben a kiadásban megújult a hely visszanyerése folyamat a felhőből, így a nem használt szóköz valamit elérhető gyorsabb, mint a korábbi verziók.

- **Új pillanatkép képek**: új virtuális VHDX és VMDK elérhetők az Azure klasszikus portálon keresztül. Ezeket a képeket, hozhatók létre új frissítés 0,1 eszközök töltheti le.

- **A feladat állapotát a portálon pontosságát javítása**: a szoftver előző verziójában feladatállapot-jelentés a portálon nem volt részletes. Ebben a kiadásban a probléma megoldásához.

- **Tartomány illesztés tapasztalható**: hibajavítás kapcsolódó tartomány való csatlakozás és átnevezése eszköz.


## <a name="issues-fixed-in-the-update-01"></a>A frissítés 0,1 javított

Az alábbi táblázat összefoglalja az ebben a kiadásban rögzített problémák.

| nem.  | A szolgáltatás                              | A probléma                                                                                                                                                                                                                                                                                                                           |
|------|--------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 1    | VMDK                                 | Egyes VMware verzióiban az operációs rendszer lemez lett tekinteni a ritka értesítések és okozó normál művelet megszakítása. Ez ebben a kiadásban lett rögzíteni.                                                                                                                                                                                    |
| 2    | iSCSI kiszolgáló                         | Az utolsó kiadásban a felhasználó volt szükség adjon meg egy átjáró engedélyezett hálózati StorSimple virtuális eszköz. Ez a probléma, hogy a felhasználó összes engedélyezett hálózati kapcsolat esetén, ha legalább egy átjáró beállítása ebben a kiadásban módosul.                                                                              |
| 3    | Támogatás csomag                      | A szoftver előző verziójában sikertelen, amikor 1 GB-nál nagyobb méretű csomag csomag webhelycsoport támogatja. Ez a probléma megoldódott, ebben a verzióban.                                                                                                                                                                               |
| 4    | Felhő elérése                         |  Az utolsó kiadásban Ha a StorSimple virtuális tömb nem rendelkezik a hálózati kapcsolat és indította, a helyi felhasználói felületének szeretné, hogy csatlakozási problémákat tapasztal. Ez a probléma megoldódott, ebben a verzióban.                                                                                                                            |
| 5    | Diagramok figyelése                    | A korábbi verzióból, a következő eszköz feladatátvevő, az a felhő kapacitás kihasználtsági diagramok az Azure klasszikus portálon a nem a megfelelő értékek jelenik meg. Ez az aktuális programcsomag rögzített.                                                                                                                          |



## <a name="known-issues-in-the-update-01"></a>Ismert problémák a 0,1 frissítés

Az alábbi táblázat összefoglalja az ismert problémák a StorSimple virtuális tömb, és a problémákat, a korábbi verziókban való megjelenési jegyezni tartalmazza. **Ebben a kiadásban jegyezni problémák megjelenési csillag jelöli. A listában szereplő problémák szinte minden StorSimple virtuális tömb Georgia kiadását átvitt van.**


| nem. | A szolgáltatás | A probléma | Megoldás: / megjegyzések |
|-----|--------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **1.** | Frissítések | A virtuális eszközök előzetes verziójában létrehozott nem lehet frissíteni egy támogatott általános elérhetősége verzióra. | Általános elérhetősége verzióval katasztrófa helyreállítási (DR) munkafolyamattal ezekre az eszközökre virtuális sikertelen fölé. |
| **2.** | Kiépített adatok lemez | Egyszer a megadott méretben adatok lemezen van kiépítve és hozta létre a megfelelő StorSimple virtuális eszközre, kell nem bontsa ki a vagy a Lekicsinyítve, az adatok lemez. Ehhez kísérel meg az eszköz a helyi rétegek adatai mérvű eredményezi. |   |
| **3.** | A csoportházirend | Ha egy eszközt a tartományhoz, egy csoportházirend negatív hatással lehet az eszköz művelet. | Győződjön meg arról, hogy a virtuális tömb saját szervezeti egységre az Active Directory, és nem csoportházirend-objektumok (GPO) vonatkoznak rá.|
| **4.** | Helyi webes felhasználói felület | Ha nagyobb biztonság szolgáltatások az Internet Explorer (IE ESC) engedélyezett, előfordulhat, hogy bizonyos helyi felhasználói felület weblapok például hibaelhárítás és karbantartás nem működik megfelelően. A következő lapokon gombok nem is működnek. | Kapcsolja ki a nagyobb biztonság szolgáltatások az Internet Explorerben.|
| **5.** | Helyi webes felhasználói felület | A Hyper-V virtuális gépen a hálózati csatolófelületet a webhely felhasználói felület összegéhez viszonyítva 10 GB/s felületek. | Ez a probléma a Hyper-v tükröződés. A Hyper-V mindig 10 GB/s virtuális hálózati kártya jeleníti meg. |
| **6.** | Többszintű kötet és megosztás | A többszintű kötet nem támogatott StorSimple verzióval használható alkalmazások zárolása bájt tartományban. Ha bájt tartomány zárolása engedélyezve van, StorSimple tiering nem fog működni. | Ajánlott mértékek a következők: <br></br>Kapcsolja ki az alkalmazás logika a zárolás bájt tartományban.<br></br>Válassza a szeretné elhelyezni az adatokat az alkalmazás nem pedig a többszintű kötet helyileg rögzített mennyiségben.<br></br>*Bővítve*: Ha bájt tartomány zárolása engedélyezve van a kiemelt kötet használ helyben, tartsa szem előtt, hogy a helyi meghajtóra rögzített mennyiségi lehet online még azelőtt, a visszaállítás befejeződött. Ezekben az esetekben ha visszaállítás már folyamatban van, majd meg kell várnia a visszaállítás befejezéséhez. |
| **7.** | Többszintű megosztások | Nagyméretű fájlok használata eredményezhet lassú réteg meg. | Ha nagyobb fájlokkal dolgozik, azt javasoljuk, hogy a legnagyobb fájl, a megosztás méretű 3 %-nál kisebb. |
| **8.** | Megosztás kapacitása használt | Megjelenhet felhasználási hiányában a megosztott adatok megosztása. Ennek oka az, a használt kapacitás megosztások metaadatokat tartalmaz. |   |
| **9.** | Vészhelyreállítás | Csak a kívánt fájl kiszolgáló, amely az adatforrás eszköz azonos tartományban vészhelyreállítás végezheti el. Ebben a kiadásban nem támogatott vészhelyreállítás egy másik tartományban cél eszközre. | Ez az későbbi kiadásokban hajtja végre. |
| **10.** | Azure PowerShell | Ebben a kiadásban az Azure Powershellen keresztül nem lehet felügyelni a StorSimple virtuális eszközök. | A virtuális eszközök irányításának az Azure klasszikus portál és a felhasználói felület helyi interneten keresztül kell elvégezni. |
| **11.** | A jelszó módosítása | A virtuális tömb eszköz konzol csak fogadja el a beviteli hu-hu billentyűzet formátumban. |   |
| **12.** | CHAP | Egyszer létrehozott CHAP hitelesítő adatokat nem lehet eltávolítani. Emellett a CHAP hitelesítő adatok módosításakor szüksége lesz a kötet offline állapotba helyezni, és majd füleket online a változtatások csak akkor lépnek érvénybe. | Ezek az későbbi kiadásokban címzettje lesz. |
| **13-mal.** | iSCSI kiszolgáló  | A "használt tárterület"-iSCSI mennyiségi jelenik meg a StorSimple kezelő szolgáltatás és az iSCSI állomás eltérő lehet. | A iSCSI host a fájlrendszer nézet tartozik.<br></br>Az eszköz kiosztva, amikor a mennyiségi volt a maximális méret blokkok látja.|
| **14.** | Fájl kiszolgálói *  | Ha egy mappában található fájlhoz egy alternatív adatok adatfolyam (HIRDETÉSEK) társítva van, a HIRDETÉSEK nem készített biztonsági másolatot vagy keresztül vészhelyreállítás adatfeliratsor és elem szint helyreállítása vissza.| |


## <a name="next-step"></a>Következő lépés

[Frissítések telepítése](storsimple-ova-install-update-01.md) a StorSimple virtuális tömb.
