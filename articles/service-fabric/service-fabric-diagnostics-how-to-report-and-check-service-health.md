<properties
   pageTitle="Jelentés és Azure Service háló a állapotának ellenőrzése |} Microsoft Azure"
   description="Útmutató: a szolgáltatáskód küldése állapotjelentések és a szolgáltatás állapotának ellenőrzése eszközeivel állapot nyomon Azure Service háló tartalmazza."
   services="service-fabric"
   documentationCenter=".net"
   authors="toddabel"
   manager="mfussell"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/06/2016"
   ms.author="toddabel"/>

# <a name="report-and-check-service-health"></a>Jelentést, és ellenőrizze a szolgáltatás állapota
A szolgáltatások problémák lépnek fel, ha az Ön számára megválaszolása és javítás eseményekre és kimaradások függ, hogy gyorsan a hibák feltárása az Ön számára. Ha, jelentéssel problémák és hibák az Azure Service háló állapot felettes a szolgáltatáskód a szabványos rendszerállapot figyelése eszközök, amelyek szolgáltatás háló jelenít meg, hogy az állapot állapotának ellenőrzése is használhatja.

Két módon, hogy jelentést küldhet a szolgáltatás állapota:

- [Partition](https://msdn.microsoft.com/library/system.fabric.istatefulservicepartition.aspx) vagy [CodePackageActivationContext](https://msdn.microsoft.com/library/system.fabric.codepackageactivationcontext.aspx) -objektumok használata.  
Használhatja a `Partition` és `CodePackageActivationContext` elemeket, amelyek az aktuális környezetben részeként állapotának jelentése az objektumok. Például kópia részeként lefutó kód nyilvántartástól állapot csak adott replika, a partíciót tartozik, és az alkalmazás, amely egy része.

- Használat `FabricClient`.   
Használható `FabricClient` a szolgáltatáskód, ha a fürt nem [biztonságos](service-fabric-cluster-security.md) , vagy ha a szolgáltatás fut-e rendszergazdai jogosultságokkal a jelentés állapota. Ez nem, igaz, a való életben találatokkal. A `FabricClient`, állapot nyilvántartástól bármely személy, amely a fürt része. Ideális esetben azonban szolgáltatáskód csak küldje el saját állapota kapcsolódó jelentéseket.

Ez a cikk végigvezeti egy példa, hogy a szolgáltatáskód az állapot jelenti. A példa azt is bemutatja, hogyan a szolgáltatás háló nyújt eszközöket is használható állapot állapotának ellenőrzéséhez. Ebből a cikkből kell a rendszerállapot figyelése a szolgáltatás háló funkcióinak rövid útmutatást talál. További információ a cikksorozat meg szeretné vizsgálni a állapota a hivatkozást, ez a cikk végén kezdődő erről.

## <a name="prerequisites"></a>Előfeltételek
A következő telepítve kell rendelkeznie:

   * Visual Studio 2015
   * Szolgáltatás háló SDK

## <a name="to-create-a-local-secure-dev-cluster"></a>Helyi biztonságos fejlesztők fürt létrehozása
- Nyissa meg a rendszergazda jogosultságokkal rendelkező Powershellt, és futtassa az alábbi parancsokat.

![A biztonságos fejlesztők fürtre létrehozásának módját mutatják parancsok](./media/service-fabric-diagnostics-how-to-report-and-check-service-health/create-secure-dev-cluster.png)

## <a name="to-deploy-an-application-and-check-its-health"></a>Alkalmazások telepítése, és annak állapotának ellenőrzése

1. Nyissa meg a Visual Studio rendszergazdaként.

2. Projekt létrehozása a **Megbízható, szűrőként működő szolgáltatás** sablon használatával.

    ![Megbízható, szűrőként működő szolgáltatás szolgáltatás háló-alkalmazás létrehozása](./media/service-fabric-diagnostics-how-to-report-and-check-service-health/create-stateful-service-application-dialog.png)

3. Nyomja le az **F5 billentyűparancs hatására** az alkalmazás futtatásához hibakeresési módban. A helyi fürtre telepítik az alkalmazást.

4. Az alkalmazás futása után kattintson a jobb gombbal a helyi felülete ikonjára az értesítési területen, és nyissa meg a szolgáltatás háló Intézőt a helyi menüből válassza a **Helyi fürtre kezelése** .

    ![Nyissa meg a szolgáltatás háló Intézőt az értesítési területen](./media/service-fabric-diagnostics-how-to-report-and-check-service-health/LaunchSFX.png)

5. Ahogy a képen az alkalmazás állapot jelenjen meg. Ekkor az alkalmazás megfelelő hibátlan kell lennie.

    ![A szolgáltatás háló Intézőben megfelelő alkalmazás](./media/service-fabric-diagnostics-how-to-report-and-check-service-health/sfx-healthy-app.png)

6. Az állapot is ellenőrizheti a PowerShell használatával. Használható ```Get-ServiceFabricApplicationHealth``` állapot egy alkalmazást, és segítségével ```Get-ServiceFabricServiceHealth``` ellenőrizni a szolgáltatás állapota. Az állapotjelentés az azonos PowerShell-alkalmazás az ezen a képen szerepel.

    ![Megfelelő PowerShell-alkalmazás](./media/service-fabric-diagnostics-how-to-report-and-check-service-health/ps-healthy-app-report.png)

## <a name="to-add-custom-health-events-to-your-service-code"></a>Egyéni állapot események hozzáadása a szolgáltatáskód
A szolgáltatás háló projektsablonok a Visual Studióban példakódot tartalmazzák. A következő lépések bemutatják, hogyan jelentést a egyéni állapot események a szolgáltatáskód a. Ezeket a jelentéseket automatikusan megjelenik az állapot ellenőrzése, hogy szolgáltatás háló biztosít, például szolgáltatás háló Explorer, a Azure portál állapot megtekintése és a PowerShell a szabványos eszközeit.

1. Nyissa meg az alkalmazás, amely a korábban létrehozott a Visual Studióban, vagy hozzon létre egy új alkalmazást a **Megbízható, szűrőként működő szolgáltatás** Visual Studio sablon használatával.

2. Nyissa meg a Stateful1.cs fájlt, és keresse meg a `myDictionary.TryGetValueAsync` betárcsázni a `RunAsync` módot. Láthatja, hogy ez a módszer adja eredményül egy `result` , amely az aktuális értékét a számláló rendelkezik, mivel az alkalmazás fő logika a operációs rendszert futtató száma. Ha ez egy valós alkalmazást, és ha az eredmény hiánya jeleníti meg a hiba, célszerű az esemény ikonra.

3. Jelentés egy állapot eseményt, ha az eredmény hiánya jelöl egy hiba, vegye fel az alábbi lépéseket.

    egy. Adja hozzá a `System.Fabric.Health` névtér a Stateful1.cs fájlt.

    ```csharp
    using System.Fabric.Health;
    ```

    b. Adja hozzá a következő kód után a `myDictionary.TryGetValueAsync` hívása

    ```csharp
    if (!result.HasValue)
    {
        HealthInformation healthInformation = new HealthInformation("ServiceCode", "StateDictionary", HealthState.Error);
        this.Partition.ReportReplicaHealth(healthInformation);
    }
    ```
    Replika állapot azt jelenteni, mert az állapot-nyilvántartó szolgáltatásból van jelentve. A `HealthInformation` paraméter jelentve állapot problémához adatait tárolja.

    Ha egy állapot nélküli szolgáltatást is létrehozott, használja a következő

    ```csharp
    if (!result.HasValue)
    {
        HealthInformation healthInformation = new HealthInformation("ServiceCode", "StateDictionary", HealthState.Error);
        this.Partition.ReportInstanceHealth(healthInformation);
    }
    ```

4. Ha a szolgáltatás fut rendszergazdai engedélyekkel, vagy ha a fürt nem [biztonságos](service-fabric-cluster-security.md), is használhatja `FabricClient` jelentés állapota, ahogy az alábbi lépéseket.  

    egy. Hozzon létre a `FabricClient` példány után a `var myDictionary` nyilatkozat.

    ```csharp
    var fabricClient = new FabricClient(new FabricClientSettings() { HealthReportSendInterval = TimeSpan.FromSeconds(0) });
    ```

    b. Adja hozzá a következő kód után a `myDictionary.TryGetValueAsync` hívja.

    ```csharp
    if (!result.HasValue)
    {
       var replicaHealthReport = new StatefulServiceReplicaHealthReport(
            this.ServiceInitializationParameters.PartitionId,
            this.ServiceInitializationParameters.ReplicaId,
            new HealthInformation("ServiceCode", "StateDictionary", HealthState.Error));
        fabricClient.HealthManager.ReportHealth(replicaHealthReport);
    }
    ```

5. Nézzük szimulálhatja ezt a hibát, és megjelennek az állapot ellenőrző eszközök a látható. A hibát, ki az első sorban a rendszerállapot jelentéskészítési kódot, amely a korábban hozzáadott a Megjegyzés hasonlóan. Megjegyzések hozzáfűzése ki az első vonalra, miután a kódot a következő példa fog megjelenni.

    ```csharp
    //if(!result.HasValue)
    {
        HealthInformation healthInformation = new HealthInformation("ServiceCode", "StateDictionary", HealthState.Error);
        this.Partition.ReportReplicaHealth(healthInformation);
    }
    ```
 Ez a kód most fire e állapotjelentés, minden alkalommal, amikor `RunAsync` hajt végre. Miután elvégezte a módosítást, nyomja le az **F5 billentyűparancs hatására** az alkalmazás futtatásához.

6. Az alkalmazás futása után nyissa meg a szolgáltatás háló Intézőt az alkalmazás állapotának ellenőrzéséhez. Ebben az esetben szolgáltatás háló Explorer jelennek meg, hogy az alkalmazás sérült-e. Ez a jelentett kódja, akkor a korábban hozzáadott a hiba miatt.

    ![Sérült szolgáltatás háló Intéző-alkalmazás](./media/service-fabric-diagnostics-how-to-report-and-check-service-health/sfx-unhealthy-app.png)

7. Ha az elsődleges replika szolgáltatás háló Explorer, a fastruktúrájú nézetben válassza ki, látni fogja, hogy **Rendszerállapot** hibára utal, is. Szolgáltatás háló Explorer is megjelenik a rendszerállapot jelentés részleteit, amelyeket felvettek a `HealthInformation` paraméter be a kódot. Láthatja, hogy az azonos állapotjelentések PowerShell és az Azure-portálra.

    ![A szolgáltatás háló Intézőben replika állapota](./media/service-fabric-diagnostics-how-to-report-and-check-service-health/replica-health-error-report-sfx.png)

Ez a jelentés az állapot-kezelőben marad, amíg a másik kimutatást, illetve amíg ki nem törli az ezen helyettesíti. Mivel azt nem állít be `TimeToLive` az állapot jelentéshez a `HealthInformation` objektum, a jelentés nem jár.

Azt javasoljuk, hogy állapota a legtöbb alapszinten, amely ebben az esetben a replika kell jelenteni. Akkor is jelenthet állapota a `Partition`.

```csharp
HealthInformation healthInformation = new HealthInformation("ServiceCode", "StateDictionary", HealthState.Error);
this.Partition.ReportPartitionHealth(healthInformation);
```

A jelentés állapota `Application`, `DeployedApplication`, és `DeployedServicePackage`, használjon `CodePackageActivationContext`.

```csharp
HealthInformation healthInformation = new HealthInformation("ServiceCode", "StateDictionary", HealthState.Error);
var activationContext = FabricRuntime.GetActivationContext();
activationContext.ReportApplicationHealth(healthInformation);
```

## <a name="next-steps"></a>Következő lépések
[A szolgáltatás háló állapot mély merülési](service-fabric-health-introduction.md)
