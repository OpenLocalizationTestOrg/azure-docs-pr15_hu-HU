<properties
   pageTitle="A szolgáltatás háló szolgáltatások elérhetősége |} Microsoft Azure"
   description="Hibafa észlelése, feladatátadási és helyreállítási szolgáltatások ismerteti"
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

# <a name="availability-of-service-fabric-services"></a>A szolgáltatás háló szolgáltatások elérhetősége
Lehet, hogy Azure Service háló szolgáltatások állapot-nyilvántartó vagy állapot nélküli. Ez a cikk áttekintést nyújt a hogyan kezeli az szolgáltatás háló a hibák megelőzve a szolgáltatások elérhetőségét.

## <a name="availability-of-service-fabric-stateless-services"></a>A szolgáltatás háló állapot nélküli szolgáltatások elérhetősége
Állapot nélküli szolgáltatás az alkalmazás szolgáltatásainak nincs telepítve a bármely [helyi állandó állapotát](service-fabric-concepts-state.md).

Állapot nélküli szolgáltatás létrehozásához definiálásáról-példányok száma, amely az állapot nélküli szolgáltatás, amely futnia kell a fürt példányainak száma. Az a példányszámot lesz szabad létrehozni a fürt alkalmazás logikájának. A méretezés állapot nélküli szolgáltatás be az ajánlott módszereket példányok számának növelése.

Ha egy hiba lép fel, állapot nélküli szolgáltatás példányán, egy új példányát más jogosult csomópontot a fürt jön létre.

## <a name="availability-of-service-fabric-stateful-services"></a>Állapot-nyilvántartó szolgáltatás háló-szolgáltatások elérhetősége
Állapot-nyilvántartó szolgáltatás van társítva állapot. A szolgáltatás háló állapot-nyilvántartó szolgáltatás modellezni kópiák formájában. Mindegyik kópiában a szolgáltatás állapota másolatát tartalmazó a kód egy példánya. Olvasás és írás műveleteket egy replika (más néven elsődleges) elemre. Adja meg a módosítások írási műveletek *replikált* több más kópiák (más néven aktív formátumú másodlagos zónák). Az elsődleges és az aktív másodlagos kópiák kombinációja a szolgáltatás a kópiakészlet.

Nem lehet csak egy elsődleges replika karbantartási olvasási és írási kérelmek, de több aktív másodlagos replikákat is lehet. Az aktív másodlagos replikákat egy konfigurálható, és kópiák nagyobb számú elviseli egyidejű szoftver és hardver hibák nagyobb számú.

Az esemény (ha az elsődleges replika megszakad) egy hiba, a háló szolgáltatás lehetővé teszi az aktív másodlagos replikákat közül az új elsődleges replika. A aktív másodlagos replika már rendelkezik a frissített verziójának állapotát (keresztül *replikációs*), és továbbra is a feldolgozása további olvasási és írási műveletek.

Ezt a fogalmat – egy elsődleges vagy az aktív replika másodlagos – a replikált szerepkör nevezik.

### <a name="replica-roles"></a>Replika szerepkörök
A szerepkör a kópiák az adott replika felügyelt állam életciklusára kezelésére szolgál. Akinek szerepköre elsődleges szolgáltatások kópia olvasási kérelmek. Állapotába frissítése és esetében, amelyek a változtatásokat az aktív formátumú másodlagos zónák a kópiakészlet írási kérelem is szolgáltatásokat. Az aktív másodlagos feladata, az elsődleges replika eljutott állam módosítások és frissítheti a nézetet az állam.

>[AZURE.NOTE] A [megbízható szereplők keretrendszer](service-fabric-reliable-actors-introduction.md) például magasabb szintű programozási modellek absztrakt nem vagyok a gépnél a fejlesztői a replikált szerepkör fogalmának.

## <a name="next-steps"></a>Következő lépések

Szolgáltatás háló fogalmak a további tudnivalókért lásd: a következő:

- [A szolgáltatás háló szolgáltatások méretezhetőség:](service-fabric-concepts-scalability.md)

- [Szétválasztás szolgáltatás háló szolgáltatások](service-fabric-concepts-partitioning.md)

- [Létrehozásához és kezeléséhez állam](service-fabric-concepts-state.md)
