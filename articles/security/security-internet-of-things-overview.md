<properties
   pageTitle="Az Internet a dolog, amit biztonsági áttekintése |} Microsoft Azure"
   description=" Azure internetes szolgáltatások dolog (IoT) széles köre lehetőségeket kínálnak. Ez a cikk segít megtudhatja, hogyan biztonságos a IoT megoldások Azure-ban. "
   services="security"
   documentationCenter="na"
   authors="TomShinder"
   manager="MBaldwin"
   editor="TomSh"/>

<tags
   ms.service="security"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/09/2016"
   ms.author="terrylan"/>

# <a name="internet-of-things-security-overview"></a>Az Internet a dolog, amit biztonsági – áttekintés

Azure internetes szolgáltatások dolog (IoT) széles köre lehetőségeket kínálnak. Az alábbi vállalati minőségű szolgáltatások lehetővé teszi:

- Adatgyűjtés eszközökről
- Adatok adatfolyam megjelenítését a – mozgás elemzése
- Tárolhatja és nagy adatkészletek lekérdezés
- Valós idejű és a korábbi adatok ábrázolása
- Integráció az office-háttér rendszerek

Használhat az előadáshoz alábbi funkciók, Azure IoT csomagja csomagok közös egyéni kiterjesztésű több Azure szolgáltatás előre definiált megoldások formájában. Ezekkel az előre beállított megoldásokkal olyan gyakori csökkentheti az időt, hogy a IoT megoldásokat IoT megoldás mintázatok alap példányait. Használja a IoT szoftverfejlesztői, testreszabása és kiterjesztése a következő megoldások saját igényeinek kielégítéséhez. Is használhatja ezeket a megoldások példák vagy sablonok új IoT megoldások fejlesztésekor.

Az Azure IoT csomagot az igényeinek megfelelően IoT hatékony megoldás. Azonban érdemes upmost fontos, hogy a IoT megoldások szem előtt az elejétől biztonsági készült. IoT eszközök puszta száma, mert bármely biztonsági esemény gyorsan előfordulhat, hogy egy kiterjedt esemény jelentős következményeit.

Megtudhatja, hogyan a IoT megoldások biztonságossá tétele érdekében van az alábbi információkat.

## <a name="security-architecture"></a>Biztonsági architektúrája

A rendszer tervezésekor fontos, a potenciális veszélyek, hogy a rendszer, és adja meg megfelelő védelmet megfelelően, a rendszer tervezett és tervezett. Különösen fontos, hogy az elejétől a termék tervezése az biztonsági szem előtt, mert bemutatása, hogyan támadó előfordulhat olyan behatolhatnak segít, győződjön meg arról, hogy megfelelő megoldásokkal kapcsolatban vannak még az elejétől.

IoT biztonsági architektúrája megismerheti [Az Internet a dolog, amit biztonsági architektúrája](../iot-suite/iot-security-architecture.md)elolvasásával.

Ez a cikk azt ismerteti, hogy az alábbi témaköröket:

- [Biztonsági kezdődő veszélyt modell](../iot-suite/iot-security-architecture.md#security-starts-with-a-threat-model)
- [Az IoT biztonsága](../iot-suite/iot-security-architecture.md#security-in-iot)
- [Az Azure IoT hivatkozás architektúra modellezése veszélyt](../iot-suite/iot-security-architecture.md#threat-modeling-the-azure-iot-reference-architecture)

## <a name="security-from-the-ground-up"></a>Biztonsági alapoktól

A IoT egyedi biztonság, adatvédelem és megfelelőség kihívásokkal világszerte vállalkozások jelent. Hagyományos számítógépes technológiát, ahol a jelen hibák a szoftvert, és hogyan megvalósított köré szerveződik, eltérően IoT vonatkozik, mi történik, ha a számítógépes és a tényleges világon szerveződik. IoT megoldások védelme szükséges eszközök, ezekre az eszközökre és a felhőben, és a biztonságos adatvédelem a felhőben feldolgozása, illetve a tárhely közötti biztonságos kapcsolatot biztonságos kiépítési biztosítása. Azonban dolgozik ellen ilyen funkciók, olyan eszközök erőforrás korlátozódik, földrajzi megoszlása telepítések és belüli megoldást eszközök nagyszámú.

Ezekre a területekre biztonsági kezelése az [internetes a dolog, amit biztonsági alapoktól](../iot-suite/securing-iot-ground-up.md)olvasásával talál.

A cikk azt ismerteti, hogy az alábbi témaköröket:

- [Biztonságos infrastruktúra alapoktól](../iot-suite/securing-iot-ground-up.md#secure-infrastructure-from-the-ground-up)
- [Microsoft Azure – a vállalati biztonságos IoT infrastruktúra](../iot-suite/securing-iot-ground-up.md#microsoft-azure---secure-iot-infrastructure-for-your-business)

## <a name="best-practices"></a>Ajánlott eljárások

A szigorú biztonsági-stratégia egy IoT infrastruktúrájának védelme szükséges. Adatok védelme a hálózaton átvitt adatok integritását nyilvános interneten keresztül, és az azt jelenti, hogy biztonságosan kiépítése eszközök, mert a felhőben biztonságossá tétele kezdve a rétegekre nagyobb security garancia általános infrastruktúra hoz létre.

Megismerheti az Internet a dolog, amit biztonsági ajánlott eljárások [az Internet a dolog, amit biztonsággal kapcsolatos gyakorlati tanácsok](../iot-suite/iot-security-best-practices.md)olvasásával.

A cikk azt ismerteti, hogy az alábbi témaköröket:

- [IoT hardver gyártója/integrátor](../iot-suite/iot-security-best-practices.md#iot-hardware-manufacturerintegrator)
- [IoT megoldás fejlesztője](../iot-suite/iot-security-best-practices.md#iot-solution-developer)
- [IoT megoldás deployer](../iot-suite/iot-security-best-practices.md#iot-solution-deployer)
- [IoT megoldás operátor](../iot-suite/iot-security-best-practices.md#iot-solution-operator)
