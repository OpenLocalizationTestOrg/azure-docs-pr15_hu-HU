<properties
   pageTitle="Dolog, amit biztonsággal kapcsolatos gyakorlati tanácsok az interneten |} Microsoft Azure"
   description="A cikk a Microsoft Internet dolog, amit biztonsággal kapcsolatos gyakorlati tanácsok és általános ajánlások curated listájának tartalmaz."
   services="security"
   documentationCenter="na"
   authors="TomShinder"
   manager="StevenPo"
   editor="TomSh"/>

<tags
   ms.service="security"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/25/2016"
   ms.author="yurid"/>

# <a name="internet-of-things-security-best-practices"></a>Dolog, amit biztonsággal kapcsolatos gyakorlati tanácsok az interneten

A kritikus vállalkozás bárki felügyeli IoT megoldások biztonságossá tétele a dolog Internet (IoT) infrastruktúra. Érintett eszközök száma és ezekre az eszközökre elosztott jellegét, mert a hatás IoT eszközök millió behatolhatnak kapcsolatos biztonsági esemény nem trivial, és kiterjedt hatással lehetnek.

Emiatt a IoT biztonsági a biztonsági jellegű megközelítés igényeknek megfelelően. Adatok kell a felhőben kell biztonságos és a személyes és nyilvános hálózatok fölé viszi, az azt. Módszerek kell biztonságosan hozhatók létre saját maguk IoT eszközök helyen. Minden réteg, eszközről hálózathoz háttéradatbázist cloud erős biztonsági biztosítékokat van szüksége.

Ajánlott eljárások a IoT sorolható, az alábbi módon:

- IoT hardver gyártója vagy integrátor
- IoT megoldás fejlesztője
- IoT megoldás deployer
- IoT megoldás operátor

Ez a cikk áttekintheti [A dolog, amit biztonsági gyakorlati tanácsok az internethez](../iot-suite/iot-security-best-practices.md). Olvassa el a cikk további információt.

## <a name="iot-hardware-manufacturer-or-integrator"></a>IoT hardver gyártója vagy integrátor

Ha Ön egy IoT hardver gyártás vagy a hardver integrátor, hajtsa végre az alábbi tanácsokat:

- **Hatókör hardverfrissítés minimális követelmények**: a hardver tervezés tartalmaznia kell a hardver és semmi további művelet elvégzéséhez szükséges minimális funkciók. 
- **Ellenőrizze a hardver vásárlási átállítani**: a hardver, például az eszköz fedőlap, az eszköz, például egy részének eltávolítása megnyitása fizikai illetéktelen hozzáférés feltárása mechanizmusok összeállítása. 
- **Biztonságos hardver körül összeállítása**: Ha [ELÁBÉ](https://en.wikipedia.org/wiki/Cost_of_goods_sold) teszi lehetővé, összeállítása biztonsági szolgáltatások, például a titkosított és a biztonságos tárolása és a modul Platformmegbízhatósági-alapú indítási funkciók.
- **Ellenőrizze a biztonságos frissítések**: elkerülhetetlen során az eszköz élettartama frissítése a belső vezérlőprogram.

## <a name="iot-solution-developer"></a>IoT megoldás fejlesztője

Ha Ön egy IoT megoldás fejlesztője, hajtsa végre az alábbi tanácsokat:

- **Követés biztonságos szoftver fejlesztési módszertan**: biztonság a kezdete a projekt teljes mértékben a végrehajtására, tesztelése és üzembe alapokról telefonos gondolkodni fejlesztett biztonságos szoftver van szükség.
- **Válassza a Megnyitás szoftver körültekintéssel**: gyors lehetőség a megoldások fejlesztésére lehetőséget kínál a Megnyitás szoftvert.
- **Gondosan integrálás**: a szoftver biztonsági hibákat számos megtalálható a tárakban és az API-khoz szegélyét. 

## <a name="iot-solution-deployer"></a>IoT megoldás deployer

Ha Ön egy IoT megoldás deployer, hajtsa végre az alábbi tanácsokat:

- **Hardveres biztonságosan üzembe**: IoT telepítések megkövetelheti hardvereszközök nem biztonságos helyen, például nyilvános szóközöket vagy felügyelet területi beállítások telepíthető.
- **Hitelesítési kulcsokat biztonsága**: a telepítés során az egyes eszköz azonosítóját igényel, és a felhőbeli szolgáltatástól által generált hitelesítési kulcsokat tartozó. Biztonsága billentyűk fizikailag még a telepítés után. Egy meglévő eszközként helyettesítő bármely módosított kulcs használható rosszindulatú eszközzel.

## <a name="iot-solution-operator"></a>IoT megoldás operátor

Ha Ön egy IoT megoldás operátort, hajtsa végre az alábbi tanácsokat:

- **Naprakész állapotban rendszerek**: Győződjön meg róla, eszköz operációs rendszerek és az összes eszközillesztőket frissülnek a legújabb verzióra. 
- **Rosszindulatú tevékenység elleni védelem**: lehetővé teszi az operációs rendszer, ha a legújabb Anti-Virus és a kártevők elleni funkciók elhelyezése minden eszköz operációs rendszert. 
- **Naplózási gyakran**: IoT infrastruktúra naplózása biztonsággal kapcsolatos problémák kulcsfontosságú, amikor válaszol a biztonsági események.
- **A IoT infrastruktúra fizikailag védelme**: a legrosszabb támadások elleni IoT infrastruktúra fizikai eszközök hozzáféréssel indította el.
- **Védelem felhő hitelesítő adatok**: Felhő hitelesítő adatok beállítását, és egy IoT telepítési működő használt is esetleg a legkönnyebben úgy hozzáférhet, és egy IoT rendszer használhatja. 
