<properties
   pageTitle="A Azure Service háló megbízható szolgáltatás beállításainak áttekintése |} Microsoft Azure"
   description="Tudjon meg többet az állapot-nyilvántartó megbízható Services konfigurálása az Azure Service háló."
   services="Service-Fabric"
   documentationCenter=".net"
   authors="sumukhs"
   manager="timlt"
   editor="vturecek"/>

<tags
   ms.service="Service-Fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/18/2016"
   ms.author="sumukhs"/>

# <a name="configure-stateful-reliable-services"></a>Állapot-nyilvántartó megbízható szolgáltatások beállítása:

Nincsenek két csoportját megbízható szolgáltatások beállításait. Egy az összes megbízható szolgáltatás a fürt globális, míg más megadása egy adott adott megbízható szolgáltatáshoz.

## <a name="global-configuration"></a>A globális konfiguráció

A globális megbízható szolgáltatás beállításai a fürt jegyzék KtlLogger csoportjában a fürt van megadva. Lehetővé teszi a megosztott napló helyre és a méretét, valamint a globális memória korlátozások a naplózó által használt konfigurálása. A fürt jegyzék egy beállításokat, és minden csomópontok és a fürt szolgáltatásra vonatkozó beállításokat tartalmazó egyetlen XML-fájlba. A fájl neve általában ClusterManifest.xml. Megjelenik a fürt nyilvánvalóan a fürt a Get-ServiceFabricClusterManifest powershell paranccsal.

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


## <a name="service-specific-configuration"></a>Adott konfigurációjának
Állapot-nyilvántartó megbízható szolgáltatások alapértelmezett beállításokkal módosíthatja a konfigurációs csomag (beállítások) vagy a szolgáltatás végrehajtása (kód) használatával.

+ **Config** - konfigurációs a config csomag keresztül végezhető el a Settings.xml fájl, amelyet a Microsoft Visual Studio csomag legfelső szintű az egyes szolgáltatásokhoz, az alkalmazás a Config mappán módosítása.
+ **Kód** - kód keresztül konfigurációs: hozzon létre egy ReliableStateManagerConfiguration objektum használatával való a megfelelő beállításokat a ReliableStateManager hajtható végre.

Alapértelmezés szerint az Azure Service háló futtatókörnyezet előre definiált szakaszneveket a Settings.xml fájlban keres, és a konfigurációs értékek fogyasztása az alapul szolgáló futtatókörnyezet összetevők létrehozásakor.

>[AZURE.NOTE] **Törölje az alábbi beállításokat a Settings.xml szakasz azoknak a fájl, amely** nincs a Visual Studio megoldásban létrehozott, kivéve, ha azt tervezi, hogy a kód keresztül szolgáltatás konfigurálása.
Átnevezése a config csomagot vagy szakasz neveket esetén kell kód megváltoztatása, a ReliableStateManager a tartalomkeresési.


### <a name="replicator-security-configuration"></a>Replikációs biztonsági beállításai
A kommunikációs csatorna replikáció során használt biztonságos replikációs biztonsági beállításokat használhatók. Ez azt jelenti, hogy szolgáltatások nem tudnak egymás replikációs forgalmat, amely biztosítja, hogy az adatokat, elérhetővé válik a nagyon is biztonságos megjelenítéséhez. Alapértelmezés szerint egy üres biztonsági konfigurációs szakaszt megakadályozza, hogy a replikáció biztonsági.

### <a name="default-section-name"></a>Alapértelmezett szakasz neve
ReplicatorSecurityConfig

>[AZURE.NOTE] Ha módosítani szeretné a szakasz nevére, felülbírálása a replicatorSecuritySectionName paramétert a ReliableStateManagerConfiguration konstruktor ennek a szolgáltatásnak az ReliableStateManager létrehozásakor.


### <a name="replicator-configuration"></a>Replikációs konfiguráció
Replikációs beállítások konfigurálása a replikációs, amely az állapot-nyilvántartó megbízható szolgáltatás állapota nagymértékben megbízható kezdeményezhet replikálása és az állapot helyileg pályától tartós felelős.
Az alapértelmezett beállítás a Visual Studio sablon által generált és kell-e elegendő. Ebben a szakaszban a replikációs finomhangolása elérhető további beállításokat is bemutat.

### <a name="default-section-name"></a>Alapértelmezett szakasz neve
ReplicatorConfig

>[AZURE.NOTE] Ha módosítani szeretné a szakasz nevére, felülbírálása a replicatorSettingsSectionName paramétert a ReliableStateManagerConfiguration konstruktor ennek a szolgáltatásnak az ReliableStateManager létrehozásakor.


### <a name="configuration-names"></a>Konfigurációs nevek
|név|Egység|Alapérték|Megjegyzések:|
|----|----|-------------|-------|
|BatchAcknowledgementInterval|Másodperc|0.015|Az időszak, amelynek a replikációs fogadása elküldése előtt egy művelet után a másodlagos vár, az elsődleges vissza visszaigazolást. Intervallum feldolgozott műveletekhez küldeni bármely más nyugták egy válasz szövegként kapja meg.|
|ReplicatorEndpoint|A #HIÁNYZIK|Alapértelmezett – szükséges paraméter|IP-cím és az elsődleges és másodlagos replikációs használatával kommunikál a kópia más gyártóitól által használt port beállítása. Ez hivatkoznia kell a service jegyzék a TCP-erőforrás végpontokat. Olvassa el a további információ a [nyilvánvalóan erőforrások szolgáltatás](service-fabric-service-manifest-resources.md) végpontjának erőforrások definiálása az egy szolgáltatás jegyzék. |
|MaxPrimaryReplicationQueueSize|Tevékenységek száma|8192|Az elsődleges várakozási sorban található műveletek maximális számát. Művelet sikerült több után az elsődleges replikációs visszaigazolást fogad a másodlagos gyártóitól. Ez az érték nagyobb, mint 64 és a 2 hatványát kell lennie.|
|MaxSecondaryReplicationQueueSize|Tevékenységek száma|16384|A másodlagos várakozási sorban található műveletek maximális számát. Művelet sikerült több állapotába erősen elérhető adatmegőrzési végrehajtása után. Ez az érték nagyobb, mint 64 és a 2 hatványát kell lennie.|
|CheckpointThresholdInMB|MB|50|Az állapot alkulcsaihoz az naplófájlban lévő hely összege.|
|MaxRecordSizeInKB|KB|1024|Legnagyobb rekord mérete, amely a replikációs írhat a naplóban. Ez az érték a 4-es vagy nagyobb, mint 16 többszöröse kell lennie.|
|MinLogSizeInMB|MB|0 (a rendszer határozzák meg)|Minimális méret a tranzakció alapú napló. A napló nem használható ez a beállítás alatt egy értékre rövidítendő szám. 0 azt jelzi, hogy a replikációs meghatározza, hogy a minimális napló méretét. Ez az érték növelésével megnöveli óta esélye annak csonkolásának arányában megfelelő naplóbejegyzések, hogy a részleges másolatok és növekményes biztonsági ezt a lehetőséget.|
|TruncationThresholdFactor|Tényező|2|Határozza meg, hogy mi a napló méretben csonkított aktiválódik. Csonkított küszöb TruncationThresholdFactor szorozva MinLogSizeInMB határozza meg. TruncationThresholdFactor 1-nél nagyobb kell lennie. MinLogSizeInMB * TruncationThresholdFactor MaxStreamSizeInMB kisebbnek kell lennie.|
|ThrottlingThresholdFactor|Tényező|4|Határozza meg, hogy mi a napló méretét, a kópia kezdi szabályozott alatt. Max határozzák meg szabályozási küszöbértéket (MB) ((MinLogSizeInMB *ThrottlingThresholdFactor),(CheckpointThresholdInMB* ThrottlingThresholdFactor)). Szabályozási küszöbértéket (MB) nagyobb, mint (MB) csonkított küszöbértéket kell lennie. Csonkított küszöbértékét (a MB) MaxStreamSizeInMB kisebbnek kell lennie.|
|MaxAccumulatedBackupLogSizeInMB|MB|800|A maximális mérete (MB) egy adott biztonságimásolat-naplót lánc biztonsági naplók halmozott. Egy növekményes biztonsági kérések sikertelen lesz, ha a a növekményes biztonsági másolatot szeretne készítése a halmozott biztonsági naplók okozhat a lehet nagyobb, mint a méret vonatkozó teljes biztonsági mentés óta biztonságimásolat-naplót. Ebben az esetben a teljes biztonsági másolat készítése a felhasználó szükség van.|
|SharedLogId|GLOBÁLISAN EGYEDI AZONOSÍTÓJA|""|Adja meg egy egyedi globálisan egyedi azonosítója azonosítására használt az ezen a megosztott naplófájl használható. Szolgáltatások általában, ne használja ezt a beállítást. Jó helyen jár Ha SharedLogId van megadva, majd SharedLogPath is meg kell adni.|
|SharedLogPath|Teljes elérési út|""|Itt adhatja meg, ahol hozható létre a megosztott naplófájlban az ezen a teljes elérési útját. Szolgáltatások általában, ne használja ezt a beállítást. Jó helyen jár Ha SharedLogPath van megadva, majd SharedLogId is meg kell adni.|
|SlowApiMonitoringDuration|Másodperc|300|A felügyelt API-hívások nyomon intervallumát akkor állítja be. Példa: a felhasználó megadott biztonsági visszahívási függvény. Az intervallum lejárt, miután egy figyelmeztetés állapotjelentés küld az állapot Manager.|

### <a name="sample-configuration-via-code"></a>Minta konfigurációs kód használatával
```csharp
class Program
{
    /// <summary>
    /// This is the entry point of the service host process.
    /// </summary>
    static void Main()
    {
        ServiceRuntime.RegisterServiceAsync("HelloWorldStatefulType",
            context => new HelloWorldStateful(context, 
                new ReliableStateManager(context, 
        new ReliableStateManagerConfiguration(
                        new ReliableStateManagerReplicatorSettings()
            {
                RetryInterval = TimeSpan.FromSeconds(3)
                        }
            )))).GetAwaiter().GetResult();
    }
}    
```
```csharp
class MyStatefulService : StatefulService
{
    public MyStatefulService(StatefulServiceContext context, IReliableStateManagerReplica stateManager)
        : base(context, stateManager)
    { }
    ...
}
```


### <a name="sample-configuration-file"></a>Minta konfigurációs fájl
```xml
<?xml version="1.0" encoding="utf-8"?>
<Settings xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <Section Name="ReplicatorConfig">
      <Parameter Name="ReplicatorEndpoint" Value="ReplicatorEndpoint" />
      <Parameter Name="BatchAcknowledgementInterval" Value="0.05"/>
      <Parameter Name="CheckpointThresholdInMB" Value="512" />
   </Section>
   <Section Name="ReplicatorSecurityConfig">
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


### <a name="remarks"></a>Megjegyzések:
BatchAcknowledgementInterval replikációs időtartama határozza meg. "0" értéket eredményez a legalacsonyabb esetleges késés, de átviteli (mint több visszaigazoló üzenetet küldött legyen, és kevesebb nyugták tartalmazó feldolgozott).
BatchAcknowledgementInterval minél nagyobb érték, annál nagyobb a teljes replikációs adatátvitel, de magasabb művelet késés. Ez közvetlenül a tranzakciók véglegesítése késése megfelelője.

CheckpointThresholdInMB érték, amely a replikációs segítségével állapotinformációkat tárolni a replika dedikált naplófájl lemezterületet szabályozza. Gyorsabb konfigurálás alkalommal, amikor új kópiát hozzáadódik az eredményezhet növelni addig, amíg ez az alapértelmezett-nél nagyobb értéket. Ennek oka, hogy a részleges állam továbbított, hogy mi történjen a további műveletek a naplóban előzményeit elérhetőségét miatt. A potenciálisan növeli replika helyreállítási időt, e az összeomlást után.

A MaxRecordSizeInKB beállítás határozza meg, hogy egy rekord, amely a naplófájl be a replikációs szerint írható maximális méretét. A legtöbb esetben az alapértelmezés szerinti 1024-KB rekord mérete optimális. Jó helyen jár Ha a szolgáltatás okoz nagyobb adatelemeket részének az állapot adatai, majd ezt az értéket lehet szükség növelni kell. Kis kedvezménye érdekében, hogy MaxRecordSizeInKB 1024, kisebb mint kisebb rekordok használata csak a kisebb rekord szükséges hely van. Megakadályozási, hogy ezt az értéket szeretné módosítani kell csak ritkán.

A SharedLogId és SharedLogPath beállítások mindig közös létrehozására használhatók az alapértelmezett megosztott naplófájlból megosztott külön naplót használja a csomópont szolgáltatás. A hatékonyság növelése érdekében a lehető legtöbb szolgáltatások meg kell határoznia a azonos megosztott naplót. Megosztott fájlok, amelyeket kizárólag a megosztott naplófájl csökkentheti a központi mozgását kérelem lemezen szeretné elhelyezni. Megakadályozási, hogy ezt az értéket szeretné módosítani kell csak ritkán.

## <a name="next-steps"></a>Következő lépések
 - [A szolgáltatás háló alkalmazás a Visual Studióban hibakeresése](service-fabric-debugging-your-application.md)
 - [Fejlesztői segédlet megbízható szolgáltatások](https://msdn.microsoft.com/library/azure/dn706529.aspx)
