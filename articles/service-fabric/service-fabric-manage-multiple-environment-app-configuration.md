<properties
   pageTitle="A szolgáltatás háló több környezetekben kezelése |} Microsoft Azure"
   description="Szolgáltatás háló alkalmazások, amelyek mérete egyik számítógépről gépek ezer tartományt fürt futtatását is lehetővé teszi. Egyes esetekben kívánt konfigurálása a másképp használja ezeket változatos környezetben. Ez a cikk bemutatja, hogy miként környezet egy másik alkalmazás paraméterek."
   services="service-fabric"
   documentationCenter=".net"
   authors="seanmck"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotNet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="07/19/2016"
   ms.author="seanmck"/>

# <a name="manage-application-parameters-for-multiple-environments"></a>Több környezetekben alkalmazás paramétereinek kezelése

Tetszőleges használatával egy gépek sok ezer és Azure Service háló fürt hozhat létre. Alkalmazás bináris különböző környezetekben a széles dokumentumhasználat módosítás nélkül is futtatása, miközben be lesz gyakran szeretné állítani az alkalmazás másképp, attól függően, hogy a gép esetén bevezetéshez számát.

Fontolja meg egy egyszerű példa `InstanceCount` állapot nélküli szolgáltatáshoz. Ha futtat alkalmazásokat Azure-ban, általában kívánt értékének-"1" a paraméter értéke. Ezzel biztosíthatja, hogy fut-e a szolgáltatás a fürt minden csomóponton. Jó helyen jár ez a beállítás nem alkalmas egyetlen-gép fürt óta figyel az azonos végpontot egyetlen számítógépre több folyamat nem lehet. Ehelyett, általában az elrendezéstípus `InstanceCount` 1-re.

## <a name="specifying-environment-specific-parameters"></a>Környezetfüggő paraméter megadása

A megoldás a konfigurációs probléma paraméteres alapértelmezett szolgáltatások és alkalmazások paraméter fájlok kitölteni ezeket paraméterértékeket, hogy egy adott környezetben. Alapértelmezett szolgáltatások és az alkalmazás paraméterei van konfigurálva a az alkalmazások és szolgáltatások jegyzékfájlok. A ServiceManifest.xml és ApplicationManifest.xml fájlok sémadefiníciója telepítve van a szolgáltatás háló SDK és a *C:\Program Files\Microsoft SDKs\Service Fabric\schemas\ServiceFabricServiceModel.xsd*eszközök.

### <a name="default-services"></a>Alapértelmezett szolgáltatások

Szolgáltatás háló alkalmazások szolgáltatás példányainak gyűjteménye tevődnek össze. Lehetséges, hogy hozzon létre egy üres alkalmazást, és hozzon létre az összes szolgáltatás példányainak dinamikusan lép, a legtöbb alkalmazással beállított az core services mindig hozható létre, ha az alkalmazás jön létre. Ezek "alapértelmezett szolgáltatások" nevezik. Az alkalmazás jegyzék szögletes zárójelek között szerepel a környezet konfiguráció helyőrzőkkel azokat:

    <DefaultServices>
        <Service Name="Stateful1">
            <StatefulService
                ServiceTypeName="Stateful1Type"
                TargetReplicaSetSize="[Stateful1_TargetReplicaSetSize]"
                MinReplicaSetSize="[Stateful1_MinReplicaSetSize]">

                <UniformInt64Partition
                    PartitionCount="[Stateful1_PartitionCount]"
                    LowKey="-9223372036854775808"
                    HighKey="9223372036854775807"
                />
        </StatefulService>
    </Service>
  </DefaultServices>

Az elnevezett paraméterek mindegyikében belül a paraméterek elem az alkalmazás jegyzék kell megadni:

    <Parameters>
        <Parameter Name="Stateful1_MinReplicaSetSize" DefaultValue="2" />
        <Parameter Name="Stateful1_PartitionCount" DefaultValue="1" />
        <Parameter Name="Stateful1_TargetReplicaSetSize" DefaultValue="3" />
    </Parameters>

A DefaultValue attribútum megadja az egy adott környezetben több-specifikus paraméter hiányában használandó értékét.

>[AZURE.NOTE] Nem minden szolgáltatás példány paraméterek nem alkalmas környezet konfigurációjának. A fenti példában a szolgáltatás particionálási séma LowKey és HighKey értékének kifejezetten határozzák meg a szolgáltatás összes előfordulását óta partíciót tartománya adatok tartomány, nem a környezet függvény.


### <a name="per-environment-service-configuration-settings"></a>Környezet szolgáltatás konfigurációs beállításai

A [szolgáltatás háló alkalmazásmodell](service-fabric-application-model.md) lehetővé teszi, hogy a szolgáltatásokat tartalmazó egyéni megadott értékekkel, amelyek olvasható futásidőben konfigurációs csomagok felvenni. Ezeket a beállításokat az értékek is is lehet termékenként környezet megadásával egy `ConfigOverride` az alkalmazás jegyzék a.

Tegyük fel, hogy a következő beállítást a Config\Settings.xml fájlban a `Stateful1` szolgáltatás:


    <Section Name="MyConfigSection">
      <Parameter Name="MaxQueueSize" Value="25" />
    </Section>

Ezt az értéket az egy adott alkalmazás-környezet páros felülbírálásához hozzon létre egy `ConfigOverride` a szolgáltatás jegyzék az alkalmazás jegyzék a importálásakor.

    <ConfigOverrides>
     <ConfigOverride Name="Config">
        <Settings>
           <Section Name="MyConfigSection">
              <Parameter Name="MaxQueueSize" Value="[Stateful1_MaxQueueSize]" />
           </Section>
        </Settings>
     </ConfigOverride>
  </ConfigOverrides>

Ez a paraméter majd beállítható környezet felett látható módon. Ehhez deklarálása azt az alkalmazás jegyzék paraméterek szakaszában, és az alkalmazás paraméter fájljait környezet kötött értékek megadása.

>[AZURE.NOTE] Szolgáltatás konfigurációs beállítások, ha vannak olyan három helyen, ahol a kulcs értékét beállítható, hogy: a szolgáltatás konfigurálása csomag, az alkalmazás jegyzék és az alkalmazás paraméter fájlt. Szolgáltatás háló mindig választhat az alkalmazás paraméter fájl első (ha van megadva), majd az alkalmazás jegyzék, végül a konfigurációs csomagot.


### <a name="application-parameter-files"></a>Alkalmazás paraméter fájlok

A szolgáltatás háló alkalmazás projektet is elhelyezhet alkalmazás paraméter egy vagy több fájlt. Mindegyiket határozza meg, hogy az alkalmazás jegyzék határozzák meg a paraméterek egyedi értékeket:

    <!-- ApplicationParameters\Local.xml -->

    <Application Name="fabric:/Application1" xmlns="http://schemas.microsoft.com/2011/01/fabric">
        <Parameters>
            <Parameter Name ="Stateful1_MinReplicaSetSize" Value="2" />
            <Parameter Name="Stateful1_PartitionCount" Value="1" />
            <Parameter Name="Stateful1_TargetReplicaSetSize" Value="3" />
        </Parameters>
    </Application>

Új alkalmazás alapértelmezés szerint a két alkalmazás paraméter fájlokat, Local.xml és Cloud.xml nevű tartalmazza:

![Megoldás Intéző alkalmazást paraméter fájlok][app-parameters-solution-explorer]

Hozzon létre egy új paraméter-fájlt, egyszerűen másolja és illessze be a meglévő és adjon meg egy új nevet.

## <a name="identifying-environment-specific-parameters-during-deployment"></a>Környezetfüggő paraméterek azonosítja a telepítés során

Telepítési időben kell a megfelelő paraméter fájl elemet választva alkalmazza az alkalmazást. Ehhez a közzététel párbeszédpanel a Visual Studióban, vagy PowerShell keresztül.

### <a name="deploy-from-visual-studio"></a>A Visual Studio terjesztése

A rendelkezésre álló paraméter fájlok listája választhat az alkalmazás a Visual Studióban közzétételekor.

![Válassza a paraméter fájl közzététele párbeszédpanel][publishdialog]

### <a name="deploy-from-powershell"></a>A PowerShell telepítése

A `Deploy-FabricApplication.ps1` PowerShell-parancsprogramot, a project-sablon szereplő elfogad paraméterként közzététel profilt, és a PublishProfile a alkalmazás paraméterek fájlra mutató hivatkozást tartalmaz.

  ```PowerShell
    ./Deploy-FabricApplication -ApplicationPackagePath <app_package_path> -PublishProfileFile <publishprofile_path>
  ```

## <a name="next-steps"></a>Következő lépések

Néhány alapvető fogalmak ebben a témakörben tárgyalt kapcsolatos további információért lásd: a [szolgáltatás háló technikai áttekintés](service-fabric-technical-overview.md). Más alkalmazás szolgáltatásairól a Visual Studio használható tudni olvassa el a [Visual Studio a szolgáltatás háló alkalmazások kezelése](service-fabric-manage-application-in-visual-studio.md)című témakört.

<!-- Image references -->

[publishdialog]: ./media/service-fabric-manage-multiple-environment-app-configuration/publish-dialog-choose-app-config.png
[app-parameters-solution-explorer]:./media/service-fabric-manage-multiple-environment-app-configuration/app-parameters-in-solution-explorer.png
