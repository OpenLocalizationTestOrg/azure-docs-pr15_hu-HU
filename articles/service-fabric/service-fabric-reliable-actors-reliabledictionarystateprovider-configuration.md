<properties
   pageTitle="Az Azure Service háló megbízható szereplők ReliableDictionaryActorStateProvider konfiguráció áttekintése |} Microsoft Azure"
   description="Azure Service háló állapot-nyilvántartó szereplők ReliableDictionaryActorStateProvider típusú beállításának ismertetése."
   services="Service-Fabric"
   documentationCenter=".net"
   authors="sumukhs"
   manager="timlt"
   editor=""/>

<tags
   ms.service="Service-Fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="07/18/2016"
   ms.author="sumukhs"/>

# <a name="configuring-reliable-actors--reliabledictionaryactorstateprovider"></a>Megbízható szereplők – ReliableDictionaryActorStateProvider konfigurálása
Az alapértelmezett beállítása ReliableDictionaryActorStateProvider hoz létre a Visual Studio csomag legfelső szintű a Config mappában találja a megadott szereplő settings.xml fájl módosításával módosíthatók.

A Azure Service háló futtatókörnyezet előre definiált szakaszneveket a settings.xml fájlban keres, és a konfigurációs értékek fogyasztása az alapul szolgáló futtatókörnyezet összetevők létrehozásakor.

>[AZURE.NOTE] Végezze el **nem** törölheti vagy módosíthatja a következő konfigurációk jön létre a Visual Studio megoldás settings.xml fájlban szakaszneveket.

Is ReliableDictionaryActorStateProvider konfigurációja befolyásoló globális beállításokat.

## <a name="global-configuration"></a>A globális konfiguráció

A globális konfiguráció a fürt jegyzék KtlLogger csoportjában a fürt van megadva. Lehetővé teszi a megosztott napló helyre és a méretét, valamint a globális memória korlátozások a naplózó által használt konfigurálása. Figyelje meg, hogy a fürt jegyzék hatással vannak az összes ReliableDictionaryActorStateProvider használó és megbízható állapot-nyilvántartó szolgáltatásokat.

A fürt jegyzék egy beállításokat, és minden csomópontok és a fürt szolgáltatásra vonatkozó beállításokat tartalmazó egyetlen XML-fájlba. A fájl neve általában ClusterManifest.xml. Megjelenik a fürt nyilvánvalóan a fürt a Get-ServiceFabricClusterManifest powershell paranccsal.

### <a name="configuration-names"></a>Konfigurációs nevek

|név|Egység|Alapérték|Megjegyzések:|
|----|----|-------------|-------|
|WriteBufferMemoryPoolMinimumInKB|KB|8388608|Az naplózó írási pufferelési memória készlet kernelüzemmódban lefoglalásához KB minimális száma. A memória készlet állapot adatai, mielőtt lemezre írása gyorsítótárazás használják.|
|WriteBufferMemoryPoolMaximumInKB|KB|Nincs korlát|A nagyobb, amelyhez a naplózó írása pufferelési memória készlet maximális mérete is.|
|SharedLogId|GLOBÁLISAN EGYEDI AZONOSÍTÓJA|""|Adja meg egy egyedi azonosító az alapértelmezett megosztott naplófájl minden csomóponton a fürt, amely nem adja meg a SharedLogId az adott konfigurációjának minden megbízható szolgáltatás által használt használandó globálisan egyedi azonosítója. Ha SharedLogId van megadva, majd SharedLogPath is meg kell adni.|
|SharedLogPath|Teljes elérési út|""|Itt adhatja meg, ahol a megosztott naplófájljának használnak az összes megbízható szolgáltatásai minden csomóponton a fürt, amely nem adja meg a SharedLogPath az adott konfigurációjának teljes elérési útját. Jó helyen jár Ha SharedLogPath van megadva, majd SharedLogId is meg kell adni.|
|SharedLogSizeInMB|Megabájtban|8192|A megosztott napló statikus lefoglalásához MB szabad lemezterület számát adja meg. Az értéknek 2048 vagy nagyobb kell lennie.|

### <a name="sample-cluster-manifest-section"></a>Minta fürt nyilvánvalóan szakasz
```xml
   <Section Name="KtlLogger">
     <Parameter Name="WriteBufferMemoryPoolMinimumInKB" Value="8192" />
     <Parameter Name="WriteBufferMemoryPoolMaximumInKB" Value="8192" />
     <Parameter Name="SharedLogId" Value="{7668BB54-FE9C-48ed-81AC-FF89E60ED2EF}"/>
     <Parameter Name="SharedLogPath" Value="f:\SharedLog.Log"/>
     <Parameter Name="SharedLogSizeInMB" Value="16383"/>
   </Section>
```

### <a name="remarks"></a>Megjegyzések:
A naplózási tartalmaz az elérhető az összes megbízható szolgáltatáshoz csomóponton gyorsítótárazás állapot adatai, a megbízható szolgáltatás replika a dedikált naplófájl írt társított előtt nem lapozható kernelmemória lefoglalt memória globális erőforráskészlethez tartozik. A készlet mérete szabályozza a WriteBufferMemoryPoolMinimumInKB és WriteBufferMemoryPoolMaximumInKB beállításait. WriteBufferMemoryPoolMinimumInKB Itt adhatja meg, mind a kezdeti memória készlet és a legalacsonyabb mérete, amelyhez a memória készlet kisebb lehet. WriteBufferMemoryPoolMaximumInKB, amelyhez a memória készlet nagyobb előfordulhat, hogy a legnagyobb mérete. Minden megbízható szolgáltatás replika megnyitott előfordulhat, hogy a memória-készlet méretének növelése egy meghatározott rendszer összeg WriteBufferMemoryPoolMaximumInKB felfelé. A memória készletből, mint rendelkezésre álló memória további igény esetén memória kérelem mindaddig, amíg a memória fog késik. Ezért a írási pufferelési memória készlet túl kicsi ahhoz, hogy egy adott konfiguráció esetén majd teljesítmény kárt.

A SharedLogId és SharedLogPath beállításokat mindig használják együtt a fürt határozhat meg a globálisan egyedi azonosítója és az alapértelmezett csomópontjait megosztott naplót helyét. Az alapértelmezett megosztott napló megbízható szolgáltatások nem adja meg a beállításokat az adott szolgáltatás a settings.xml szolgál. A legjobb teljesítmény elérése érdekében megosztott naplófájlok kizárólag használják a megosztott naplófájl csökkentheti a kérelem lemezen szeretné elhelyezni.

SharedLogSizeInMB az alapértelmezett csomópontjait megosztott bejelentkezési lefoglalandó lemezterületet összegét adja meg.  SharedLogId és SharedLogPath nem kell megadni annak érdekében, hogy SharedLogSizeInMB kell megadni.

## <a name="replicator-security-configuration"></a>Replikációs biztonsági beállításai
A kommunikációs csatorna replikáció során használt biztonságos replikációs biztonsági beállításokat használhatók. Ez azt jelenti, hogy a szolgáltatások nem látható egymás replikációs forgalmat, amely biztosítja az adatok, elérhetővé válik a nagyon is biztonságos.
Alapértelmezés szerint egy üres biztonsági konfigurációs szakaszt megakadályozza, hogy a replikáció biztonsági.

### <a name="section-name"></a>Szakasz neve
&lt;ActorName&gt;ServiceReplicatorSecurityConfig

## <a name="replicator-configuration"></a>Replikációs konfiguráció
Replikációs konfigurációk használatával állítsa be a felelős szereplő állam szolgáltató állam nagymértékben megbízható kezdeményezhet replikálása és az állapot helyileg pályától tartós replikációs.
Az alapértelmezett beállítás a Visual Studio sablon által generált és kell-e elegendő. Ebben a szakaszban a replikációs finomhangolása elérhető további beállításokat is bemutat.

### <a name="section-name"></a>Szakasz neve
&lt;ActorName&gt;ServiceReplicatorConfig

### <a name="configuration-names"></a>Konfigurációs nevek

|név|Egység|Alapérték|Megjegyzések:|
|----|----|-------------|-------|
|BatchAcknowledgementInterval|Másodperc|0.015|Az időszak, amelynek a replikációs fogadása elküldése előtt egy művelet után a másodlagos vár, az elsődleges vissza visszaigazolást. Intervallum feldolgozott műveletekhez küldeni bármely más nyugták egy válasz szövegként kapja meg.||
|ReplicatorEndpoint|A #HIÁNYZIK|Alapértelmezett – szükséges paraméter|IP-cím és az elsődleges és másodlagos replikációs használatával kommunikál a kópia más gyártóitól által használt port beállítása. Ez hivatkoznia kell a service jegyzék a TCP-erőforrás végpontokat. Olvassa el a további információ a [nyilvánvalóan erőforrások szolgáltatás](service-fabric-service-manifest-resources.md) végpontjának erőforrások definiálása a szolgáltatás jegyzék. |
|MaxReplicationMessageSize|Bájt|50 MB|Adott üzenet átvihető replikációs adatok maximális fájlmérete.|
|MaxPrimaryReplicationQueueSize|Tevékenységek száma|8192|Az elsődleges várakozási sorban található műveletek maximális számát. Művelet sikerült több után az elsődleges replikációs visszaigazolást fogad a másodlagos gyártóitól. Ez az érték nagyobb, mint 64 és a 2 hatványát kell lennie.|
|MaxSecondaryReplicationQueueSize|Tevékenységek száma|16384|A másodlagos várakozási sorban található műveletek maximális számát. Művelet sikerült több állapotába erősen elérhető adatmegőrzési végrehajtása után. Ez az érték nagyobb, mint 64 és a 2 hatványát kell lennie.|
|CheckpointThresholdInMB|MB|200|Az állapot alkulcsaihoz az naplófájlban lévő hely összege.|
|MaxRecordSizeInKB|KB|1024|Legnagyobb rekord mérete, amely a replikációs írhat a naplóban. Ez az érték a 4-es vagy nagyobb, mint 16 többszöröse kell lennie.|
|OptimizeLogForLowerDiskUsage|Logikai érték|Igaz|IGAZ, ha a napló van konfigurálva, hogy a replika dedikált naplófájl jön létre, a ritka NTFS-fájl használatával. Ez csökkenti a tényleges használt lemezterület a fájlt. Hamis, ha a fájlt a rögzített hozzárendelések, adja meg, írja be a legjobb teljesítmény jön létre.|
|SharedLogId|globálisan egyedi azonosítója|""|Adja meg egy egyedi globálisan egyedi azonosítója azonosítására használt az ezen a megosztott naplófájl használható. Szolgáltatások általában, ne használja ezt a beállítást. Jó helyen jár Ha SharedLogId van megadva, majd SharedLogPath is meg kell adni.|
|SharedLogPath|Teljes elérési út|""|Itt adhatja meg, ahol hozható létre a megosztott naplófájlban az ezen a teljes elérési útját. Szolgáltatások általában, ne használja ezt a beállítást. Jó helyen jár Ha SharedLogPath van megadva, majd SharedLogId is meg kell adni.|


## <a name="sample-configuration-file"></a>Minta konfigurációs fájl

```xml
<?xml version="1.0" encoding="utf-8"?>
<Settings xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <Section Name="MyActorServiceReplicatorConfig">
      <Parameter Name="ReplicatorEndpoint" Value="MyActorServiceReplicatorEndpoint" />
      <Parameter Name="BatchAcknowledgementInterval" Value="0.05"/>
      <Parameter Name="CheckpointThresholdInMB" Value="180" />
   </Section>
   <Section Name="MyActorServiceReplicatorSecurityConfig">
      <Parameter Name="CredentialType" Value="X509" />
      <Parameter Name="FindType" Value="FindByThumbprint" />
      <Parameter Name="FindValue" Value="9d c9 06 b1 69 dc 4f af fd 16 97 ac 78 1e 80 67 90 74 9d 2f" />
      <Parameter Name="StoreLocation" Value="LocalMachine" />
      <Parameter Name="StoreName" Value="My" />
      <Parameter Name="ProtectionLevel" Value="EncryptAndSign" />
      <Parameter Name="AllowedCommonNames" Value="My-Test-SAN1-Alice,My-Test-SAN1-Bob" />
   </Section>
</Settings>
```

## <a name="remarks"></a>Megjegyzések:
A BatchAcknowledgementInterval paraméter a replikáció időtartama határozza meg. "0" értéket eredményez a legalacsonyabb esetleges késés, de átviteli (mint több visszaigazoló üzenetet küldött legyen, és kevesebb nyugták tartalmazó feldolgozott).
BatchAcknowledgementInterval minél nagyobb érték, annál nagyobb a teljes replikációs adatátvitel, de magasabb művelet késés. Ez közvetlenül a tranzakciók véglegesítése késése megfelelője.

A CheckpointThresholdInMB paraméter szabályozza a lemezterületet, amelyet a replikációs állapotinformációkat tárolni a replika dedikált naplófájl segítségével. Gyorsabb konfigurálás alkalommal, amikor új kópiát hozzáadódik az eredményezhet növelni addig, amíg ez az alapértelmezett-nél nagyobb értéket. Ennek oka, hogy a részleges állam továbbított, hogy mi történjen a további műveletek a naplóban előzményeit elérhetőségét miatt. A potenciálisan növeli replika helyreállítási időt, e az összeomlást után.

OptimizeForLowerDiskUsage értéke igaz, ha a naplózási fájl hely lesz túlságosan kiépített, hogy az aktív replikákat tárolhatja további állapot adatai a naplófájlok közben inaktív kópiák kevesebb lemezterületet fogja használni. Ez lehetővé teszi a csomóponton további kópiák tárolni. Ha OptimizeForLowerDiskUsage hamis értékre állítja, az állapot adatai gyorsan írt a naplófájlok.

A MaxRecordSizeInKB beállítás határozza meg, hogy egy rekord, amely a naplófájl be a replikációs szerint írható maximális méretét. A legtöbb esetben az alapértelmezés szerinti 1024-KB rekord mérete optimális. Jó helyen jár Ha a szolgáltatás okoz nagyobb adatelemeket részének az állapot adatai, majd ezt az értéket lehet szükség növelni kell. Kis kedvezménye érdekében, hogy MaxRecordSizeInKB 1024, kisebb mint kisebb rekordok használata csak a kisebb rekord szükséges hely van. Megakadályozási, hogy ezt az értéket szeretné módosítani kell csak abban az esetben ritka.

A SharedLogId és SharedLogPath beállítások mindig közös létrehozására használhatók az alapértelmezett megosztott naplófájlból megosztott külön naplót használja a csomópont szolgáltatás. A hatékonyság növelése érdekében a lehető legtöbb szolgáltatások meg kell határoznia a azonos megosztott naplót. Megosztott fájlok, amelyeket kizárólag a megosztott naplófájl csökkentheti a központi mozgását kérelem lemezen szeretné elhelyezni. Megakadályozási, hogy ezeket az értékeket szeretne módosítani kell csak abban az esetben ritka.
