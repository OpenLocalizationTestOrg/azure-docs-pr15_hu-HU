<properties
   pageTitle="Az Azure Service háló megbízható szereplők KVSActorStateProvider konfiguráció áttekintése |} Microsoft Azure"
   description="Azure Service háló állapot-nyilvántartó szereplők KVSActorStateProvider típusú beállításának ismertetése."
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
   ms.date="09/20/2016"
   ms.author="sumukhs"/>

# <a name="configuring-reliable-actors--kvsactorstateprovider"></a>Megbízható szereplők – KVSActorStateProvider konfigurálása
Az alapértelmezett beállítása KVSActorStateProvider a settings.xml fájlt, amely a Microsoft Visual Studio csomag legfelső szintű a Config mappában jön létre a megadott szereplő megváltoztatásával módosíthatja.

A Azure Service háló futtatókörnyezet előre definiált szakaszneveket a settings.xml fájlban keres, és a konfigurációs értékek fogyasztása az alapul szolgáló futtatókörnyezet összetevők létrehozásakor.

>[AZURE.NOTE] Végezze el **nem** törölheti vagy módosíthatja a következő konfigurációk jön létre a Visual Studio megoldás settings.xml fájlban szakaszneveket.

## <a name="replicator-security-configuration"></a>Replikációs biztonsági beállításai
A kommunikációs csatorna replikáció során használt biztonságos replikációs biztonsági beállításokat használhatók. Ez azt jelenti, hogy a szolgáltatások nem látható egymás replikációs forgalmat, amely biztosítja, hogy az adatokat, elérhetővé válik a nagyon is biztonságos.
Alapértelmezés szerint egy üres biztonsági konfigurációs szakaszt megakadályozza, hogy a replikáció biztonsági.

### <a name="section-name"></a>Szakasz neve
&lt;ActorName&gt;ServiceReplicatorSecurityConfig

## <a name="replicator-configuration"></a>Replikációs konfiguráció
Replikációs beállítások konfigurálása a felelős szereplő állam szolgáltató állam nagymértékben megbízható kezdeményezhet replikációs.
Az alapértelmezett beállítás a Visual Studio sablon által generált és kell-e elegendő. Ebben a szakaszban a replikációs finomhangolása elérhető további beállításokat is bemutat.

### <a name="section-name"></a>Szakasz neve
&lt;ActorName&gt;ServiceReplicatorConfig

### <a name="configuration-names"></a>Konfigurációs nevek

|név|Egység|Alapérték|Megjegyzések:|
|----|----|-------------|-------|
|BatchAcknowledgementInterval|Másodperc|0.015|Az időszak, amelynek a replikációs fogadása elküldése előtt egy művelet után a másodlagos vár, az elsődleges vissza visszaigazolást. Intervallum feldolgozott műveletekhez küldeni bármely más nyugták egy válasz szövegként kapja meg.|
|ReplicatorEndpoint|A #HIÁNYZIK|Alapértelmezett – szükséges paraméter|IP-cím és az elsődleges és másodlagos replikációs használatával kommunikál a kópia más gyártóitól által használt port beállítása. Ez hivatkoznia kell a service jegyzék a TCP-erőforrás végpontokat. Olvassa el a további információ a [nyilvánvalóan erőforrások szolgáltatás](service-fabric-service-manifest-resources.md) végpontjának erőforrások definiálása a szolgáltatás jegyzék a. |
|RetryInterval|Másodperc|5|Az időszak, amely után a replikációs újra továbbítja egy üzenetet, ha nem kapja meg egy művelet visszaigazolást.|
|MaxReplicationMessageSize|Bájt|50 MB|Adott üzenet átvihető replikációs adatok maximális fájlmérete.|
|MaxPrimaryReplicationQueueSize|Tevékenységek száma|1024|Az elsődleges várakozási sorban található műveletek maximális számát. Művelet sikerült több után az elsődleges replikációs visszaigazolást fogad a másodlagos gyártóitól. Ez az érték nagyobb, mint 64 és a 2 hatványát kell lennie.|
|MaxSecondaryReplicationQueueSize|Tevékenységek száma|2048|A másodlagos várakozási sorban található műveletek maximális számát. Művelet sikerült több állapotába erősen elérhető adatmegőrzési végrehajtása után. Ez az érték nagyobb, mint 64 és a 2 hatványát kell lennie.|

## <a name="store-configuration"></a>Tár beállítása
Tár beállítások konfigurálása a helyi tárolóba, amely az állapot replikált megőrzésére szolgálnak.
Az alapértelmezett beállítás a Visual Studio sablon által generált és kell-e elegendő. Ebben a szakaszban a helyi tárolóba finomhangolása elérhető további beállításokat is bemutat.

### <a name="section-name"></a>Szakasz neve
&lt;ActorName&gt;ServiceLocalStoreConfig

### <a name="configuration-names"></a>Konfigurációs nevek

|név|Egység|Alapérték|Megjegyzések:|
|----|----|-------------|-------|
|MaxAsyncCommitDelayInMilliseconds|Ezredmásodpercben|200|A maximális tartós helyi tárolóba véglegesítése intervallumát kötegelés akkor állítja be.|
|MaxVerPages|Oldalak száma|16384|A helyi verzió lapok maximális száma adatbázis tárolni. Azt határozza meg, hogy a nyitott tranzakciók maximális száma.|

## <a name="sample-configuration-file"></a>Minta konfigurációs fájl

```xml
<?xml version="1.0" encoding="utf-8"?>
<Settings xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <Section Name="MyActorServiceReplicatorConfig">
      <Parameter Name="ReplicatorEndpoint" Value="MyActorServiceReplicatorEndpoint" />
      <Parameter Name="BatchAcknowledgementInterval" Value="0.05"/>
   </Section>
   <Section Name="MyActorServiceLocalStoreConfig">
      <Parameter Name="MaxVerPages" Value="8192" />
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
