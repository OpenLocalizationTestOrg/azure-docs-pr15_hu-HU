<properties
   pageTitle="Létrehozásához és kezeléséhez állam |} Microsoft Azure"
   description="Meghatározása és a szolgáltatás állapota a szolgáltatás háló kezelése"
   services="service-fabric"
   documentationCenter=".net"
   authors="appi101"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/10/2016"
   ms.author="aprameyr"/>

# <a name="service-state"></a>Szolgáltatás állapota
**Szolgáltatás állapota** az adatokat, hogy a szolgáltatás működéséhez hivatkozik. Tartalmazza a adatszerkezet és a változók, amely a szolgáltatás beolvassa, és a munkájához.

Például érdemes egy egyszerű számológép szolgáltatás. Ez a szolgáltatás megnyitja a két szám és a SZUM függvény. Ez a pusztán esztétikai célokból állapot nélküli szolgáltatás, amely nem tartalmaz adatokat társítva.

Tegyük fel, az azonos Számológép, de kívül számítógépes SZUM, azt is adatszolgáltató az utolsó összeget azt kiszámította módszert. Ez a szolgáltatás már csak megbízható, szűrőként működő – tartalmaz néhány (ha, a függvény ki új összeg) ír állapotokat és (ha adja vissza az utolsó kiszámított összeg) az olvasást.

Az Azure Service háló az első szolgáltatás állapot nélküli szolgáltatás neve. A második szolgáltatás állapot-nyilvántartó szolgáltatás neve.

## <a name="storing-service-state"></a>Szolgáltatás állapota tárolása
Megye vagy externalized vagy az állapot van kezelésére szolgáló kóddal közös található. Az állam externalization általában befejeződött egy külső adatbázisban vagy a tár használatával. Számológép példánkban ez lehet az SQL-adatbázishoz, amelyben az aktuális eredmény táblában tárolt. Minden kérés számítja ki az összeget frissítés ebben a sorban hajt végre.

Állapot is lehet a kódot, amely kezeli, ezt a kódot tartalmazó közös található. A szolgáltatás háló állapot-nyilvántartó services kialakításának a modell segítségével. Szolgáltatás háló az infrastruktúra annak érdekében, hogy könnyen hozzáférhető ez az állapot és a hiba esetén alternatív hibafa biztosít.

## <a name="next-steps"></a>Következő lépések

Szolgáltatás háló fogalmak a további tudnivalókért lásd: a következő:

- [A szolgáltatás háló szolgáltatások elérhetősége](service-fabric-availability-services.md)

- [A szolgáltatás háló szolgáltatások méretezhetőség:](service-fabric-concepts-scalability.md)

- [Szétválasztás szolgáltatás háló szolgáltatások](service-fabric-concepts-partitioning.md)
