<properties 
   pageTitle="Virtuális tömb StorSimple kibocsátási megjegyzések |} Microsoft Azure"
   description="A StorSimple virtuális tömb kritikus megnyitott hibák és megoldások ismerteti."
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
   ms.date="05/13/2016"
   ms.author="alkohli" />

# <a name="storsimple-virtual-array-release-notes"></a>Virtuális tömb StorSimple kibocsátási megjegyzések

## <a name="overview"></a>– Áttekintés

A következő kibocsátási megjegyzések azonosítása: a Microsoft Azure StorSimple virtuális tömb (más néven a StorSimple helyszíni virtuális eszköz vagy a StorSimple virtuális eszköz) március 2016 általános elérhetőség (kiadás) verzióval fontos nyissa meg problémákat. Ebben a kiadásban szoftver verziószáma 10.0.10271.0 felel meg.

Kibocsátási megjegyzések folyamatosan frissülnek, és megjelenése megoldás igénylő fontos problémákat, ki őket hozzá. Mielőtt beállítaná a StorSimple virtuális eszköz, gondosan tekintse át a kibocsátási megjegyzések tárolt adatok számára. 

Az alábbi táblázat összefoglalja az ebben a kiadásban ismert problémákról.


| nem. | A szolgáltatás | A probléma | Megoldás: / megjegyzések |
|-----|--------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **1.** | Frissítések | A virtuális eszközök előzetes verziójában létrehozott nem lehet frissíteni egy támogatott általános elérhetősége verzióra. | Általános elérhetősége verzióval katasztrófa helyreállítási (DR) munkafolyamattal ezekre az eszközökre virtuális sikertelen fölé. |
| **2.** | Kiépített adatok lemez | Egyszer a megadott méretben adatok lemezen van kiépítve és hozta létre a megfelelő StorSimple virtuális eszközre, kell nem bontsa ki a vagy a Lekicsinyítve, az adatok lemez. Ehhez kísérel meg az eszköz a helyi rétegek adatai mérvű eredményezi. |   |
| **3.** | A csoportházirend | Ha egy eszközt a tartományhoz, egy csoportházirend negatív hatással lehet az eszköz művelet. | Győződjön meg arról, hogy a virtuális tömb saját szervezeti egységre az Active Directory, és nem csoportházirend-objektumok (GPO) vonatkoznak rá.|
| **4.** | Helyi webes felhasználói felület | Ha nagyobb biztonság szolgáltatások az Internet Explorer (IE ESC) engedélyezett, előfordulhat, hogy bizonyos helyi felhasználói felület weblapok például hibaelhárítás és karbantartás nem működik megfelelően. A következő lapokon gombok nem is működnek. | Kapcsolja ki a nagyobb biztonság szolgáltatások az Internet Explorerben.|
| **5.** | Helyi webes felhasználói felület | A Hyper-V virtuális gépen a hálózati csatolófelületet a webhely felhasználói felület összegéhez viszonyítva 10 GB/s felületek. | Ez a probléma a Hyper-v tükröződés. A Hyper-V mindig 10 GB/s virtuális hálózati kártya jeleníti meg. |
| **6.** | Többszintű kötet és megosztás | A többszintű kötet nem támogatott StorSimple verzióval használható alkalmazások zárolása bájt tartományban. Ha bájt tartomány zárolása engedélyezve van, StorSimple tiering nem fog működni. | Ajánlott mértékek a következők: <br></br>Kapcsolja ki az alkalmazás logika a zárolás bájt tartományban.<br></br>Válassza a szeretné elhelyezni az adatokat az alkalmazás nem pedig a többszintű kötet helyileg rögzített mennyiségben.<br></br>*Bővítve*: Ha bájt tartomány zárolása engedélyezve van a kiemelt kötet használ helyben, tartsa szem előtt, hogy a helyi meghajtóra rögzített mennyiségi lehet online még azelőtt, a visszaállítás befejeződött. Ezekben az esetekben ha visszaállítás van folyamatban, majd meg kell várnia a visszaállítás befejezéséhez. |
| **7.** | Többszintű megosztások | Nagyméretű fájlok használata eredményezhet lassú réteg meg. | Ha nagyobb fájlokkal dolgozik, azt javasoljuk, hogy a legnagyobb fájl, a megosztás méretű 3 %-nál kisebb. |
| **8.** | Megosztás kapacitása használt | Megjelenhet felhasználási hiányában a megosztott adatok megosztása. Ennek oka az, a használt kapacitás megosztások metaadatokat tartalmaz. |   |
| **9.** | Vészhelyreállítás | Csak a kívánt fájl kiszolgáló, amely az adatforrás eszköz azonos tartományban vészhelyreállítás végezheti el. Ebben a kiadásban nem támogatott vészhelyreállítás egy másik tartományban cél eszközre. | Ez az későbbi kiadásokban hajtja végre. |
| **10.** | Azure PowerShell | Ebben a kiadásban az Azure Powershellen keresztül nem lehet felügyelni a StorSimple virtuális eszközök. | A virtuális eszközök irányításának az Azure klasszikus portál és a felhasználói felület helyi interneten keresztül kell elvégezni. |
| **11.** | A jelszó módosítása | A virtuális tömb eszköz konzol csak fogadja el a beviteli hu-hu billentyűzet formátumban. |   |
| **12.** | CHAP | Egyszer létrehozott CHAP hitelesítő adatokat nem lehet eltávolítani. Emellett a CHAP hitelesítő adatok módosításakor szüksége lesz a kötet offline állapotba helyezni, és majd füleket online a változtatások csak akkor lépnek érvénybe. | Ezek az későbbi kiadásokban címzettje lesz. |
| **13-mal.** | iSCSI kiszolgáló  | A "használt tárterület"-iSCSI mennyiségi jelenik meg a StorSimple kezelő szolgáltatás és az iSCSI állomás eltérő lehet. | A iSCSI host a fájlrendszer nézet tartozik.<br></br>Az eszköz kiosztva, amikor a mennyiségi volt a maximális méret blokkok látja.|
