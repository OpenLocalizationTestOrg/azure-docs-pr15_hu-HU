<properties 
pageTitle="Indítási feladatok futtatása az Azure Cloud Services |} Microsoft Azure" 
description="A Súgó indítási feladatok előkészítése az alkalmazást a felhőalapú szolgáltatás környezet. Ez útmutatást ad, akkor indítási feladatok működéséről, és hogyan tenni őket" 
services="cloud-services" 
documentationCenter="" 
authors="Thraka" 
manager="timlt" 
editor=""/>
<tags 
ms.service="cloud-services" 
ms.workload="tbd" 
ms.tgt_pltfrm="na" 
ms.devlang="na" 
ms.topic="article" 
ms.date="09/06/2016" 
ms.author="adegeo"/>



# <a name="how-to-configure-and-run-startup-tasks-for-a-cloud-service"></a>Beállítása és egy felhőalapú szolgáltatás indítási feladatok futtatása

Indítási feladatok műveletek elvégzéséhez szerepkörbe megkezdése előtt is használhatja. Műveletek végrehajtása lehet kívánt összetevő telepítése, COM-összetevők regisztrálása, beállításkulcsainak beállítása vagy egy ideig futó elindítását tartalmazzák.

>[AZURE.NOTE] Indítási feladatok sem felel meg Önnek, csak a felhőalapú szolgáltatás Web és dolgozó szerepkörök virtuális gépeken futó.

## <a name="how-startup-tasks-work"></a>Indítási feladatok működése

Indítási feladatok olyan műveleteket, amelyeket kell venni, mielőtt a szerepkörök kezdődik, és határozzák meg a [ServiceDefinition.csdef] fájl a [feladat] elem az [indítási] elemben használatával. Indítási feladatok gyakran köteg fájljai, de azok is lehet konzol alkalmazások vagy a Köteg indítása PowerShell-parancsfájlokat fájlokat.

Környezet változók át az információkat indítási tevékenységre, és a helyi tároló át információkat indítási tevékenység ki is használható. Egy környezeti változóba adhatja meg a telepíteni kívánt program elérési útja, és a fájlok helyi tárolóba, majd később használatával olvasható a szerepkörök írhatók.

Az indítási tevékenység jelentkezhetnek adatok és a hibák a **TEMP** környezeti változó által megadott könyvtárban. Az indítási feladat alatt a **TEMP** környezeti változó oldja fel a *C:\\erőforrások\\temp\\[globálisan egyedi azonosítója]. [ rolename]\\RoleTemp* címtár futtatásakor a felhőben.

Indítási feladatok is futtatható többször újraindul között. Az indítási feladatot fog futni minden alkalommal, amikor a szerepkör a rendszer akkor hasznosítja újra, és a szerepkör újrahasznosítások mindig nem tartalmazhatnak kell indítani a számítógépet. Indítási feladatok oly módon, amely lehetővé teszi őket többször problémamentesen kell írni.

Indítási feladatok az **if** (vagy kilépés kód) nulla az indítási folyamat befejezéséhez kell végződnie. Ha a indítási tevékenység egy nullától **if**végződik, a szerepkör nem indul el.


## <a name="role-startup-order"></a>Szerepkör indítási sorrendben

Az alábbi lista tartalmazza az szerepkört indítási eljárás Azure-ban:

1. A példány van megjelölve, **indítása** , és nem kapja meg a forgalmat.

2. Az összes indítási feladatok végrehajtása aszerint, hogy azok **taskType** attribútum.
    - Az **egyszerű** műveleteket hajtja végre a rendszer szinkronizáltan, de egyszerre csak egyet.
    - A **háttér** - és **előtérszíneket** feladatokat a indítási tevékenységhez aszinkron, párhuzamosan elindított.  
       
    > [AZURE.WARNING] IIS előfordulhat, hogy nem lehet teljesen konfigurálni az indítási folyamat indítási tevékenység szakaszában, szerepkör-specifikus adatok nem feltétlenül vehető igénybe. Szerepkör-specifikus adatok igénylő indítási feladatok [Microsoft.WindowsAzure.ServiceRuntime.RoleEntryPoint.OnStart](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.onstart.aspx)kell használni.

3. A szerepkör host folyamat elindul, és a webhely létrehozása az IIS.

4. A [Microsoft.WindowsAzure.ServiceRuntime.RoleEntryPoint.OnStart](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.onstart.aspx) módszer neve.

5. A példány van megjelölve **készen áll** , és példány továbbítás forgalmat.

6. A [Microsoft.WindowsAzure.ServiceRuntime.RoleEntryPoint.Run](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) módszer neve.


## <a name="example-of-a-startup-task"></a>Példa egy indítási tevékenység

Indítási feladatok [ServiceDefinition.csdef] fájl a **feladat** elem definiálva. A **parancssor** attribútum meghatározza, hogy a név és a paraméterek az indítási parancsfájl konzol parancsa, a **executionContext** attribútum Itt adhatja meg az indítási tevékenység a jogosultsági szintet, illetve a **taskType** attribútum határozza meg, hogyan lehet végrehajtani a tevékenység.

Ebben a példában egy környezeti változóba, **MyVersionNumber**, a indítási tevékenység létrejön, és "**1.0.0.0**" értékre állítja.

**ServiceDefinition.csdef**:

```xml
<Startup>
    <Task commandLine="Startup.cmd" executionContext="limited" taskType="simple" >
        <Environment>
            <Variable name="MyVersionNumber" value="1.0.0.0" />
        </Environment>
    </Task>
</Startup>
```

A következő példában az a **Startup.cmd** parancsfájl ír a sor "jelenlegi verziójával is 1.0.0.0" a StartupLog.txt fájlt a TEMP környezeti változó által megadott könyvtárban. A `EXIT /B 0` sor biztosítja, hogy az indítási tevékenység végződése **if** nulla.

```cmd
ECHO The current version is %MyVersionNumber% >> "%TEMP%\StartupLog.txt" 2>&1
EXIT /B 0
```

> [AZURE.NOTE] A Visual Studióban, a **kimeneti könyvtár másolása** az indítási parancsfájl kell kell tulajdonsága **Mindig másolása** , hogy meggyőződhessen róla, hogy a projekthez Azure a megfelelő telepítését az indítási parancsfájl (**approot\\bin** Web, és **approot** dolgozó szerepkörök).

## <a name="description-of-task-attributes"></a>Tevékenység attribútumok leírása

A következő [ServiceDefinition.csdef] fájl a **feladat** elem tulajdonságainak ismerteti:

**parancssori** – Itt adhatja meg a parancssorban az indítási tevékenység:

- A parancs, választható parancssori kapcsolók, amely az indítási tevékenység kezdődik.
- A gyakran az egy .cmd vagy .bat köteg fájl nevét.
- A tevékenység van a AppRoot viszonyított\\a telepítéshez Bin mappát. A környezeti változók kibontva meghatározásában, az elérési utat és a tevékenység nem jelennek meg. Ha környezet kiterjesztett szükség, létrehozhat egy kis .cmd parancsfájlt, amely felhívja a indítási feladat.
- A New és a parancsfájlt, amely egy [PowerShell-parancsprogramot](cloud-services-startup-tasks-common.md#create-a-powershell-startup-task)is lehet.

**executionContext** – Itt adhatja meg a jogosultsági szintet a indítási tevékenység. A jogosultsági szint korlátozva lehet vagy magasabb szintű:

- **korlátozott**  
Az indítási tevékenység fut, a szerep jogosultságaival egyenértékű jogosultságokkal. A [futtatókörnyezet] elem **executionContext** attribútum is **korlátozott**, ha felhasználóként használják.

- **jogú**  
Az indítási feladat rendszergazdai jogosultságokkal fut. Így a Programok telepítése és az IIS beállításainak módosítása, végezze el a beállításjegyzéket indítási feladatokat és egyéb rendszergazdai szintű feladatokat, magát a szerepkör a jogosultsági szint növelése nélkül.  

> [AZURE.NOTE] A jogosultsági szintet indítási tevékenység nem szükséges lehet ugyanaz, mint a szerepkört, magát.

**taskType** – Itt adhatja meg az indítási tevékenység végrehajtása módja.

- **egyszerű**  
Feladatok végrehajtása szinkronizáltan, a [ServiceDefinition.csdef] fájl megadott egyesével, abban a sorrendben. Egy **egyszerű** indítási tevékenység befejeződésekor az **if** nulla a következő **egyszerű** indítási-tevékenység végrehajtása. Ha nincs több **egyszerű** indítási feladatokat végrehajtani, a szerepkör magát fog elindulni.   

    > [AZURE.NOTE] Ha az **egyszerű** tevékenység egy nullától **if**végződik, azzal blokkolhatja a példány. További **egyszerű** indítási műveleteket, és a szerepkört, magát, nem indul el.

    Győződjön meg arról, hogy a köteg fájl végződése **if** 0, a parancs végrehajtása `EXIT /B 0` végén található a köteg fájl folyamat.

- **háttér**  
Tevékenységek és a szerepkör a indítása párhuzamosan aszinkron, végrehajtása.

- **előtér**  
Tevékenységek és a szerepkör a indítása párhuzamosan aszinkron, végrehajtása. Fő **előtér** és a **háttér** tevékenység közötti különbség, hogy **előtérben** tevékenység megakadályozza, hogy a szerepkör újrafelhasználás vagy leáll, amíg a tevékenység befejeződött. Ezt a korlátozást nem rendelkezik a **háttérben futó** feladatokat.

## <a name="environment-variables"></a>A környezeti változók

Környezet változót is indítási tevékenységhez át az információkat. Például az elérési út elhelyezheti blob, amely tartalmazza a program szeretné telepíteni, vagy a szerepkör által használt portszámokat vagy vezérlő funkcióinak az indítási tevékenység beállításai.

Vannak olyan kétféle indítási feladatok; környezet változói statikus környezet és a [RoleEnvironment] osztály tagjai alapján környezet változók. A [környezet] szakaszt [ServiceDefinition.csdef] fájl vannak, és mindkét használja a [változó] elemet, valamint **neve** attribútum.

Statikus környezet változók a **value** attribútum [változó] elem használja. A fenti példa a környezeti változó "**1.0.0.0**" statikus értéke **MyVersionNumber** hoz létre. Egy másik példa létrehozása manuálisan beállíthatja, hogy "**átmeneti tárolására**" vagy "**gyártási**" értékre a **StagingOrProduction** környezeti változó értéke alapján különböző indítási műveleteket, amelyek **StagingOrProduction** környezet változó lehet.

A RoleEnvironment osztály tagjai alapján környezeti változók ne használja a **value** attribútum [változó] elem. Ehelyett [RoleInstanceValue] gyermekelem a megfelelő **XPath** attribútum értékkel megadott tag [RoleEnvironment] osztály alapján környezeti változó létrehozásához használt. Különböző [RoleEnvironment] értékek eléréséhez **XPath** attribútumok értékeit megtalálható [Itt](cloud-services-role-config-xpath.md).



Hozzon létre egy környezeti változó, amely "**Igaz**" futtatásakor a példány a számítási irányító és a "**false**" fut a felhőben, például használja a következő [változó] és [RoleInstanceValue] elemeket:

```xml
<Startup>
    <Task commandLine="Startup.cmd" executionContext="limited" taskType="simple">
        <Environment>
    
            <!-- Create the environment variable that informs the startup task whether it is running
                in the Compute Emulator or in the cloud. "%ComputeEmulatorRunning%"=="true" when
                running in the Compute Emulator, "%ComputeEmulatorRunning%"=="false" when running
                in the cloud. -->
    
            <Variable name="ComputeEmulatorRunning">
                <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated" />
            </Variable>
    
        </Environment>
    </Task>
</Startup>
```

## <a name="next-steps"></a>Következő lépések
Útmutató: a felhőalapú szolgáltatás néhány [Gyakori indítási műveletek](cloud-services-startup-tasks-common.md) elvégzéséhez.

A [csomag](cloud-services-model-and-package.md) a felhőalapú szolgáltatás.  


[ServiceDefinition.csdef]: cloud-services-model-and-package.md#csdef
[Tevékenység]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Task
[Indítási]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Startup
[Futtatókörnyezet]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Runtime
[Környezet]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Environment
[Változó]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Variable
[RoleInstanceValue]: https://msdn.microsoft.com/library/azure/gg557552.aspx#RoleInstanceValue
[RoleEnvironment]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.aspx