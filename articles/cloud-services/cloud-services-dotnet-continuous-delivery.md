<properties
    pageTitle="Folytonos kézbesítési felhő services az Azure-ban TFS |} Microsoft Azure"
    description="Megtudhatja, hogy miként állíthatja be az Azure felhő alkalmazások folyamatos kézbesítési. Mintakódok MSBuild parancssori kimutatások és a PowerShell-parancsfájlokat."
    services="cloud-services"
    documentationCenter=""
    authors="TomArcher"
    manager="douge"
    editor=""/>

<tags
    ms.service="cloud-services"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="07/30/2016"
    ms.author="tarcher"/>

# <a name="continuous-delivery-for-cloud-services-in-azure"></a>Folytonos szállítási Cloud Services Azure-ban

A jelen cikkben ismertetett folyamat megtudhatja, hogy miként állíthatja be az Azure felhő alkalmazások folyamatos kézbesítési. Ez a folyamat lehetővé teszi automatikusan a csomagok létrehozása és telepítése az előkészítés Azure után minden kód beadás. A jelen cikkben ismertetett csomag build folyamat egyenértékű a **csomag** parancs a Visual Studióban, és a közzétételi szereplő lépések megegyeznek a **Közzététel** parancs a Visual Studióban.
A cikk bemutatja az összeállítás kiszolgálót hozhat létre, a MSBuild parancssori kimutatásokban és a Windows PowerShell-parancsfájlokat használható módszerek, és azt is bemutatja, hogyan lehet igény szerint állítsa be a Visual Studio a Team Foundation Server - definíciók Csapat összeállítása a MSBuild parancsok és a PowerShell-parancsfájlokat. A folyamat, testre szabható a build-környezet és a cél Azure környezetben.

Visual Studio csapat szolgáltatásait, könnyebben ehhez a Azure-ban tárolt TFS-verziót is használhatja. További tudnivalókért lásd: [Folytonos kézbesítési Azure által Visual Studio csapat szolgáltatások használatával][].

Mielőtt nekikezdene, közzé kell tennie a Visual Studio alkalmazásban.
Ezzel biztosíthatja, hogy minden erőforrás-e a rendelkezésre álló és inicializált amikor megpróbálja automatizálni a kiadvány folyamat.

## <a name="1-configure-the-build-server"></a>1: az összeállítás kiszolgáló konfigurálása

Mielőtt az Azure-csomagok MSBuild használatával hozhat létre, telepítenie kell a szükséges szoftverek és eszközök a build kiszolgálón.

Visual Studio függvény nem telepíthető az összeállítás kiszolgálón szükséges. Ha szeretné az összeállítás kiszolgáló kezelése a Team Foundation összeállítása szolgáltatás használatával, kövesse a [Team Foundation összeállítása szolgáltatás][] dokumentációjában.

1.  A Szerkesztés kiszolgálón telepítse a [.NET-keretrendszer 4.5.2.][], amelyek tartalmazzák még a MSBuild.
2.  Telepítse a legújabb [Azure létrehozáshoz használható eszközök a .NET rendszerhez](https://azure.microsoft.com/develop/net/).
3.  Telepítse az [Azure tárakat a .NET rendszerhez](http://go.microsoft.com/fwlink/?LinkId=623519).
4.  Másolja a Microsoft.WebApplication.targets fájlt a Visual Studio példányát build kiszolgálóra.

    Visual Studio telepítve van a számítógépén a fájl található a címtárban C:\\Program Files(x86)\\MSBuild\\Microsoft\\VisualStudio\\v14.0\\WebApplications. Az összeállítás kiszolgáló ugyanabban a könyvtárában kell másolja.
5.  Telepítse az [Azure Tools for Visual Studio](https://www.visualstudio.com/features/azure-tools-vs.aspx).

## <a name="2-build-a-package-using-msbuild-commands"></a>2: MSBuild parancsaival csomag készítése

Ez a szakasz ismerteti, hogyan összeállításához egy MSBuild parancs, amely az Azure-csomagok hoz létre. Ellenőrizze, hogy minden helyesen van beállítva, hogy a MSBuild parancsot tartalmaz, mit szeretne végezze el az összeállítás kiszolgálón futó ezt a lépést. Vagy a parancssor felveheti meglévő összeállítás parancsfájlok build kiszolgálói, vagy használhatja a parancssorban az TFS összeállítása Definition, a következő szakaszban leírt módon. Parancssori paramétereket és MSBuild kapcsolatos további tudnivalókért olvassa el a [MSBuild parancssori hivatkozás](https://msdn.microsoft.com/library/ms164311%28v=vs.140%29.aspx)című témakört.

1.  Visual Studio build kiszolgálói települ, ha keresse meg, és válassza a **Visual Studio fájl törlése kérdés** Windows **Visual Studio Tools** mappájában.

    Ha nincs telepítve a Visual Studio build kiszolgálói, nyisson meg egy parancssort, és győződjön meg arról, hogy MSBuild.exe érhető el az elérési utat a. MSBuild telepíti, az elérési utat % WINDIR a .NET-keretrendszer az\\Microsoft.NET\\keretrendszer\\*verziója*. Ha például MSBuild.exe hozzá a PATH környezeti változó, amikor a .NET-keretrendszer 4 telepítve van, írja be a következő parancsot a parancssorablakban:

        set PATH=%PATH%;"C:\Windows\Microsoft.NET\Framework\v4.0.30319"

2.  A parancssorba nyissa meg a kívánt összeállítása Azure projektfájl tartalmazó mappát.

3.  Futtassa a/TARGET MSBuild: közzététele lehetőséget a következő példának megfelelően:

        MSBuild /target:Publish

    Ez a beállítás is rövidítéssel /t: közzététele. A MSBuild a /t:Publish parancs nem kell összekeverni és a közzététel-parancsok állnak rendelkezésre a Visual Studióban, amikor az Azure SDK telepítve van. A /t: a beállítás csak buildjeiben az Azure-csomagok közzététele. A csomagok nem azt telepítése, mint a közzététel parancsok a Visual Studióban.

    Tetszés szerint megadhatja a projekt nevét, MSBuild paraméterként. Ha nincs megadva, az aktuális könyvtár használják. MSBuild parancssori kapcsolói kapcsolatos további tudnivalókért olvassa el a [MSBuild parancssori hivatkozás](https://msdn.microsoft.com/library/ms164311%28v=vs.140%29.aspx)című témakört.

4.  Keresse meg a kimenet. Alapértelmezés szerint ez a parancs könyvtárat hoz létre a legfelső szintű mappa a projekthez, például *ProjectDir*viszonyított\\bin\\*konfigurációs*\\app.publish\\. Ha generál egy Azure projekten, létre két fájlokat, a csomagfájlt, magát, és a kapcsolódó konfigurációs fájl:

    -   Project.cspkg
    -   ServiceConfiguration. *TargetProfile*.cscfg

    Alapértelmezés szerint minden Azure project tartalmaz olyan egy szolgáltatás konfigurációs fájl (.cscfg) (hibakeresési) buildjeiben helyi és felhőbeli (átmeneti vagy gyártási) buildjeiben egy másik, de felvehet és eltávolíthat onnan szolgáltatás konfigurációs fájl. Ha egy csomag Visual Studio generál, megkéri melyik szolgáltatás konfigurációs fájl mentését a csomag mellett.

5.  Adja meg a szolgáltatás konfigurációs fájl. Ha egy csomag MSBuild használatával generál, a program a helyi szolgáltatás konfigurációs fájl alapértelmezés szerint látható. Egy másik szolgáltatáscsaládba konfigurációs fájl felvenni a tulajdonság TargetProfile MSBuild parancs, a következő példának megfelelően:

        MSBuild /t:Publish /p:TargetProfile=Cloud

6.  Adja meg a kimenet helyét. Állítsa az elérési utat a /p:PublishDir használatával =*könyvtár* \\ beállítást, beleértve a záró fordított perjel elválasztó karaktert, ahogy a következő példában:

        MSBuild /target:Publish /p:PublishDir=\\myserver\drops\

    Miután kialakítani, és tesztelje a projektek létrehozását, és az Azure-csomagok egyesítése őket egy megfelelő MSBuild parancsot, felveheti a parancssorban az összeállítás parancsfájlok. Ha az összeállítás kiszolgáló egyéni parancsfájlokat használ, ez az eljárás részletei határozzák meg, az egyéni build folyamat függ. TFS használatakor build-környezet, majd, útmutatónkat a következő lépésben Azure csomag összeállítása hozzáadása az összeállítás folyamatot.

## <a name="3-build-a-package-using-tfs-team-build"></a>3: TFS Csapat összeállítása használata egy csomag készítése

Ha egy TFS build számítógépre beállítani egy összeállítás vezérlő, valamint az összeállítás kiszolgáló beállítása Team Foundation Server (TFS), majd tetszés szerint állíthat be egy automatizált build az Azure-csomag. Beállítása és használata a Team Foundation server-összeállítás rendszerű című témakörből [skála ki a Szerkesztés rendszerhez][]. Különösen az alábbi eljárás feltételezi, hogy beállított az összeállítás kiszolgáló ismertetett módon [Deploy kiszolgáló fejlesztése és beállítása][], és létrehozott egy csoportwebhely projektet létrehozni egy felhőalapú szolgáltatás project a csapata project.

Azure csomagok össze TFS beállításához hajtsa végre az alábbi lépéseket:

1.  A Visual Studio számítógépen fejlesztését, a Nézet menüben válassza a **Csapat Explorer**, vagy válassza a Ctrl +\\, Ctrl + M billentyűkombinációt. A csapat-kezelő ablakban a bontsa ki a **professzionális** csomópontot vagy a a **professzionális** lapot, és válassza a **Új összeállítása definíció**parancsra.

    ![Új összeállítása Definition beállítás][0]

2.  Kattintson az **eseményindító** fülre, és adja meg a kívánt feltételeit, ha azt szeretné, hogy a csomag létrehozása. Például adja meg a **Folyamatos integrációs** össze a csomagot, bekövetkezik egy adatforrás vezérlő beadás.

3.  Válassza az **Adatforrás-beállítások** lapot, és győződjön meg arról, hogy a projekt mappa az **Adatforrás-vezérlő mappa** oszlop szerepel, és az Állapot oszlop értéke **az aktív**.

4.  Válassza a **Szerkesztés lesz az alapértelmezett** lapot, és csoportban Build vezérlő, ellenőrizze az összeállítás-kiszolgáló nevének.  Is válassza a **Másolás összeállítása eredménye a következő legördülő mappába** lehetőséget, és adja meg a kívánt legördülő helyét.

5.  Kattintson a **folyamat** fülre. A folyamat lap választhat az alapértelmezett sablon **készítése**alatt, a projekt Ha nem az az aktív, és a rács a **Szerkesztés** szakaszban a **Speciális** szakasz kibontásához.

6.  Válassza a **MSBuild argumentumot**, és a megfelelő MSBuild parancssori argumentumok megadása, ahogy a fenti 2. Például adja meg **/t: /p:PublishDir közzététele =\\\\Sajátkiszolgáló\\esik\\ ** létre csomagot, és azt a helyet, a csomag másolást \\ \\Sajátkiszolgáló\\esik\\:

    ![MSBuild argumentumok][2]

    **Megjegyzés:** A fájlok másolása egy nyilvános megosztásra megkönnyíti a csomagok fejlesztési számítógépről manuális telepítése.

5.  A Szerkesztés lépés sikerességének tesztelése, jelölje be a változik, hogy a projekt, vagy be egy új build sorba. Be egy új összeállítása, a csapat Explorer sorba kattintson a jobb gombbal az **Összes összeállítása definíciókat** , és válassza ki a **Várólista új összeállítása**.

## <a name="4-publish-a-package-using-a-powershell-script"></a>4: használata egy PowerShell-parancsprogramot csomag közzététele

Ez a szakasz egy közzétehető a Cloud app csomag kimeneti Azure választható paramétereket alkalmaz a Windows PowerShell-parancsprogramot Egyenletszerkesztővel ismerteti. Ez a parancsfájl hívható után az egyéni build automatizálás lépés összeállítása. Is hívható a munkafolyamat-tevékenységek folyamatsablon a Visual Studio TFS Csapat összeállítása.

1.  Telepítse az [Azure PowerShell-parancsmagok][] (v0.6.1 vagy újabb).
    A parancsmaggal a telepítő fázisban, válassza a beépülő modul telepítése. Figyelje meg, hogy a hivatalos támogatott lecseréli a régebbi verziójú felajánlott keresztül CodePlex, bár a korábbi verziók voltak számozott 2.x.x.

2.  Azure PowerShell a Start menüből vagy lap indul. Ha ezzel a módszerrel kezd, az Azure PowerShell-parancsmagok töltődnek.

3.  A PowerShell-parancssorában, ellenőrizze, hogy a PowerShell-parancsmagok töltődnek be a részleges parancs megadásával `Get-Azure` majd lenyomja a Tab billentyű utasítás feltételei.

    A tabulátorbillentyű többszöri lenyomásával, ha meg kell jelennie különböző Azure PowerShell-parancsait.

4.  Győződjön meg arról, hogy csatlakozhat az Azure előfizetést az előfizetési adatokat a .publishsettings fájlból importálásával.

    `Import-AzurePublishSettingsFile c:\scripts\WindowsAzure\default.publishsettings`

    Írja be a parancs

    `Get-AzureSubscription`

    Ez az előfizetése vonatkozó információkat jelenít meg. Győződjön meg arról, hogy minden adat helyes-e.

4.  C: a parancsfájlok mappába Ez a cikk végén található parancsfájl sablon mentése\\parancsfájlok\\WindowsAzure\\**PublishCloudService.ps1**.

5.  Tekintse át a parancsfájl a paraméterek című szakaszát. Hozzáadása vagy módosítása az alapértelmezett értékeket. Ezek az értékek mindig felülbírálhatja explicit paraméterek átadása.

6.  Győződjön meg róla nincs érvényes felhőalapú szolgáltatás, tárterület fiókok az hoz létre, amely állítható be a közzététel parancsfájl célként előfizetését. A tárterület-fiókot (blob-tárolóhoz) feltöltése és ideiglenesen a telepítési csomag és a konfigurációs fájl létrehozása a telepítés közben használatával.

    -   Hozzon létre egy új, felhőalapú szolgáltatásba, hívja fel a parancsfájl, vagy használja az [Azure klasszikus portálon](http://go.microsoft.com/fwlink/?LinkID=213885). A felhőalapú szolgáltatás neve lesz a teljes tartománynévre előtaggal, és így egyedinek kell lennie.

            New-AzureService -ServiceName "mytestcloudservice" -Location "North Central US" -Label "mytestcloudservice"

    -   Hozzon létre egy új tárterület-fiókot, hívja fel a parancsfájlt, vagy az [Azure klasszikus portálon](http://go.microsoft.com/fwlink/?LinkID=213885)használja. A tároló fióknév lesz a teljes tartománynévre előtaggal, és így egyedinek kell lennie. Megpróbálhatja ugyanazt a nevet a felhőalapú szolgáltatást használ.

            New-AzureStorageAccount -ServiceName "mytestcloudservice" -Location "North Central US" -Label "mytestcloudservice"

7.  Hívja fel a parancsfájl közvetlenül az Azure PowerShell, vagy a vezeték-e parancsfájl csomag összeállítása után végrehajtandó a host build az automatizálás.

    >[AZURE.IMPORTANT] A parancsprogram mindig törlése, és cserélje le a meglévő telepítések alapértelmezés szerint, ha állapotúak. Ez a folyamatos kézbesítési automatizálási az ahhoz szükséges, hogy hol nincs felhasználói Rákérdezés lehetséges.

    **Példa forgatókönyv 1:** folyamatos telepítési szolgáltatás a fejlesztői környezet:

        PowerShell c:\scripts\windowsazure\PublishCloudService.ps1 -environment Staging -serviceName mycloudservice -storageAccountName mystoragesaccount -packageLocation c:\drops\app.publish\ContactManager.Azure.cspkg -cloudConfigLocation c:\drops\app.publish\ServiceConfiguration.Cloud.cscfg -subscriptionDataFile c:\scripts\default.publishsettings

    Ez általában követi útszakasz ellenőrzése és a virtuális felcserélése. A virtuális felcserélése az [Azure klasszikus portálon](http://go.microsoft.com/fwlink/?LinkID=213885) keresztül, vagy az áthelyezés-Deployment parancsmag használatával történik.

    **2 példa eset:** folyamatos telepítést az éles üzemi környezetben, egy dedikált próba-szolgáltatás

        PowerShell c:\scripts\windowsazure\PublishCloudService.ps1 -environment Production -enableDeploymentUpgrade 1 -serviceName mycloudservice -storageAccountName mystorageaccount -packageLocation c:\drops\app.publish\ContactManager.Azure.cspkg -cloudConfigLocation c:\drops\app.publish\ServiceConfiguration.Cloud.cscfg -subscriptionDataFile c:\scripts\default.publishsettings

    **Távoli asztali:**

    Ha a távoli asztali engedélyezve van a Azure projektben szüksége lesz, végezze el annak biztosítására, hogy a megfelelő felhőalapú szolgáltatás tanúsítvány feltöltött jelen parancsfájl célzott összes felhőszolgáltatások további egyszeri lépéseket.

    Keresse meg a szerepkör által elvárt tanúsítvány ujjlenyomatot értékeket. Az ujjlenyomatot értékeket a tanúsítványok a felhőben konfigurációs fájl (azaz ServiceConfiguration.Cloud.cscfg) csoportban jelennek meg. Érdemes emellett látható a távoli asztali beállítása párbeszédpanelen a Visual Studióban, amikor a megjelenítés beállításai és megjelenítése a kijelölt tanúsítvány.

        <Certificates>
              <Certificate name="Microsoft.WindowsAzure.Plugins.RemoteAccess.PasswordEncryption" thumbprint="C33B6C432C25581601B84C80F86EC2809DC224E8" thumbprintAlgorithm="sha1" />
        </Certificates>

    Töltse fel a távoli asztali tanúsítványok egyszeri beállításához lépésként az alábbi parancsmag parancsfájl használatával:

        Add-AzureCertificate -serviceName <CLOUDSERVICENAME> -certToDeploy (get-item cert:\CurrentUser\MY\<THUMBPRINT>)

    Példa:

        Add-AzureCertificate -serviceName 'mytestcloudservice' -certToDeploy (get-item cert:\CurrentUser\MY\C33B6C432C25581601B84C80F86EC2809DC224E8

    Másik lehetőségként a tanúsítvány-fájl exportálása PFX és a titkos kulcs, és minden cél felhőszolgáltatásba az [Azure klasszikus portálon](http://go.microsoft.com/fwlink/?LinkID=213885)feltöltése a tanúsítványok. 
    Olvassa el a további információt a következő cikk: [http://msdn.microsoft.com/library/windowsazure/gg443832.aspx], [].

    **Telepítési és törlés telepítés – frissítése\> új telepítési**

    A parancsfájl alapértelmezés szerint elvégez egy frissítési telepítési ($enableDeploymentUpgrade = 1) Ha nincs paramétert a vagy explicit módon átadott érték 1. Egyetlen előfordulását azt az előnye, hogy egy teljes telepítési-nál kevesebb időt vesz igénybe. A magas elérhetősége is magában az előnye, hogy bizonyos esetekben, míg mások futó elhagyása igénylő példányok frissítése (frissítés tartománya kiállniuk), valamint a virtuális nem lehet törölni.

    Frissítési telepítési lehet letiltani a parancsprogram ($enableDeploymentUpgrade = 0) vagy a *enableDeploymentUpgrade 0* megkerülhetők paraméterként, amely megváltoztatja törölnie kell bármelyik meglévő telepítéshez, és hozzon létre egy új telepítési parancsfájlt viselkedését.

    >[AZURE.IMPORTANT] A parancsprogram mindig törlése, és cserélje le a meglévő telepítések alapértelmezés szerint, ha állapotúak. Ez a folyamatos kézbesítési automatizálás az ahhoz szükséges, hogy hol nincs felhasználói/operátor Rákérdezés lehetséges.

## <a name="5-publish-a-package-using-tfs-team-build"></a>5: segítségével a TFS Csapat összeállítása csomag közzététele

Ez nem kötelező lépés csatlakozik TFS Csapat összeállítása a 4 lépésben létrehozott parancsfájl, amely az Azure csomag készítése közzétételét kezeli. A nagy vonalakban a folyamatsablon, így fog futni egy közzétételi tevékenységet a munkafolyamat végén, a Szerkesztés definíciójának által használt módosítása. A közzététel tevékenység hajtja végre a összeállítása a paraméterek átadása PowerShell-parancsot. A MSBuild kimenetét észlelő, és tegye közzé a parancsprogram lesz a szabványos build kimenet be kell adatcsatornán.

1.  Felelős összeállítása definíciójának szerkesztése folyamatos telepítése.

2.  Kattintson a **folyamat** fülre.

3.  Kövesse [ezeket az utasításokat](http://msdn.microsoft.com/library/dd647551.aspx) egy tevékenység projekthez a build folyamatábra sablon hozzáadása, töltse le az alapértelmezett sablont, adja hozzá a projekthez, és be. Írja be egy új nevet, például AzureBuildProcessTemplate a build folyamatábra sablon.

3.  Térjen vissza a **folyamat** lap, és használja a **Részletek megjelenítése** elérhető build folyamat sablonok listájának megjelenítéséhez. Válassza az **Új...** gombra, és kattintson az imént hozzáadott és beadása a projekt. Keresse meg a sablon az imént létrehozott, és kattintson **az OK gombra**.

4.  Nyissa meg szerkesztésre a kiválasztott folyamatábra sablonra. Közvetlenül a munkafolyamat-tervezőben vagy az XAML dolgozhat az XML-szerkesztő megnyitása

5.  Az új argumentumai az alábbi lista hozzáadása a munkafolyamat-tervezőben argumentumokat lapján külön sorként. Az összes argumentum irányba kell lennie, és írja be az = = karakterlánc. Ezek használandó adatfolyam paramétereit be a munkafolyamat-összeállítás definícióról használt majd első hívja fel a közzététel parancsfájl.

        SubscriptionName
        StorageAccountName
        CloudConfigLocation
        PackageLocation
        Environment
        SubscriptionDataFileLocation
        PublishScriptLocation
        ServiceName

    ![Az argumentumai között][3]

    A megfelelő XAML néz ki:

        <Activity  _ />
          <x:Members>
            <x:Property Name="BuildSettings" Type="InArgument(mtbwa:BuildSettings)" />
            <x:Property Name="TestSpecs" Type="InArgument(mtbwa:TestSpecList)" />
            <x:Property Name="BuildNumberFormat" Type="InArgument(x:String)" />
            <x:Property Name="CleanWorkspace" Type="InArgument(mtbwa:CleanWorkspaceOption)" />
            <x:Property Name="RunCodeAnalysis" Type="InArgument(mtbwa:CodeAnalysisOption)" />
            <x:Property Name="SourceAndSymbolServerSettings" Type="InArgument(mtbwa:SourceAndSymbolServerSettings)" />
            <x:Property Name="AgentSettings" Type="InArgument(mtbwa:AgentSettings)" />
            <x:Property Name="AssociateChangesetsAndWorkItems" Type="InArgument(x:Boolean)" />
            <x:Property Name="CreateWorkItem" Type="InArgument(x:Boolean)" />
            <x:Property Name="DropBuild" Type="InArgument(x:Boolean)" />
            <x:Property Name="MSBuildArguments" Type="InArgument(x:String)" />
            <x:Property Name="MSBuildPlatform" Type="InArgument(mtbwa:ToolPlatform)" />
            <x:Property Name="PerformTestImpactAnalysis" Type="InArgument(x:Boolean)" />
            <x:Property Name="CreateLabel" Type="InArgument(x:Boolean)" />
            <x:Property Name="DisableTests" Type="InArgument(x:Boolean)" />
            <x:Property Name="GetVersion" Type="InArgument(x:String)" />
            <x:Property Name="PrivateDropLocation" Type="InArgument(x:String)" />
            <x:Property Name="Verbosity" Type="InArgument(mtbw:BuildVerbosity)" />
            <x:Property Name="Metadata" Type="mtbw:ProcessParameterMetadataCollection" />
            <x:Property Name="SupportedReasons" Type="mtbc:BuildReason" />
            <x:Property Name="SubscriptionName" Type="InArgument(x:String)" />
            <x:Property Name="StorageAccountName" Type="InArgument(x:String)" />
            <x:Property Name="CloudConfigLocation" Type="InArgument(x:String)" />
            <x:Property Name="PackageLocation" Type="InArgument(x:String)" />
            <x:Property Name="Environment" Type="InArgument(x:String)" />
            <x:Property Name="SubscriptionDataFileLocation" Type="InArgument(x:String)" />
            <x:Property Name="PublishScriptLocation" Type="InArgument(x:String)" />
            <x:Property Name="ServiceName" Type="InArgument(x:String)" />
          </x:Members>

          <this:Process.MSBuildArguments>

6.  Egy új művelet hozzáadása futtassa a ügynök végén:

    1.  Kezdje azzal, ha a kimutatás tevékenység keressen egy érvényes parancsprogram hozzáadása. Állítsa ezt az értéket a feltétel:

            Not String.IsNullOrEmpty(PublishScriptLocation)

    2.  Az azután esetben, ha a kimutatás hozzáadása a sorozat új tevékenységet. A megjelenítendő név kezdő közzététele beállítása

    3.  Az Indítás közzététele a sorozat legyen kijelölve, és adja meg az alábbi lista új változót a munkafolyamat-tervezőben változók lapján külön sorként. Összes változó kell rendelkeznie a változó típus = karakterlánc és a hatókör = kezdés közzététele. Ezek használandó továbbításához paraméterek be a munkafolyamat-összeállítás definícióról használt majd első hívja fel a közzététel parancsfájl.

        -   SubscriptionDataFilePath karakterlánc típusú

        -   PublishScriptFilePath karakterlánc típusú

            ![Új változók][4]

    4.  Ha TFS 2012 vagy korábbi verzió használata esetén a ConvertWorkspaceItem tevékenység hozzáadása a az új sorrend elején. 2013-at vagy újabb TFS használja, ha egy GetLocalPath tevékenység hozzáadása a az új sorrend elején. Egy ConvertWorkspaceItem, állítsa az tulajdonságokat az alábbi képlettel történik: irányba ServerToLocal, DisplayName = = "Konvertálás közzétételéhez parancsfájl fájlnév" beviteli = "PublishScriptLocation", az eredmény "PublishScriptFilePath" munkaterület = = "Munkaterület". Egy GetLocalPath tevékenységét, állítsa be a tulajdonságot, a "PublishScriptLocation" IncomingPath, és az eredmény "PublishScriptFilePath". Ez a tevékenység a közzététel parancsfájl helyekről TFS server (ha alkalmazható) szabványos helyi lemez elérési út az elérési út konvertál.

    5.  TFS 2012 vagy korábbi verzióját használja, ha egy másik ConvertWorkspaceItem tevékenység hozzáadása a végén található az új sorozat. Irányba ServerToLocal, DisplayName = = "Konvertálásához előfizetés fájlnév" beviteli = "SubscriptionDataFileLocation", az eredmény "SubscriptionDataFilePath" munkaterület = = "Munkaterület". Ha TFS, 2013-at vagy újabb verzióját használja, adja meg a másik GetLocalPath. IncomingPath = "SubscriptionDataFileLocation", és az eredmény = "SubscriptionDataFilePath."

    6.  InvokeProcess tevékenység hozzáadása az új művelet végén.
        Ez a tevékenység a összeállítása meghatározás átadott argumentumok PowerShell.exe hívások.

        1.  Argumentumok = String.Format ("-fájl""{0}" "- serviceName {1} - storageAccountName \{2\} - packageLocation""{3}" "- cloudConfigLocation""{4}" "- subscriptionDataFile""{5}" "- selectedSubscription {6}-környezet""{7}" "", PublishScriptFilePath, ServiceName, StorageAccountName, PackageLocation, CloudConfigLocation, SubscriptionDataFilePath, SubscriptionName, környezetre)

        2.  DisplayName végrehajtás = parancsfájl közzététele

        3.  Fájlnév = "PowerShell" (az idézőjelekkel együtt)

        4.  OutputEncoding = System.Text.Encoding.GetEncoding(System.Globalization.CultureInfo.InstalledUICulture.TextInfo.OEMCodePage)

    7.  A InvokeProcess **Szabványos kimeneti kezelése** szakaszban rendelés_részletei.mennyiség állítsa a szövegdoboz "adatok". Ez a változó tárolásához szabványos kimeneti adatai.

    8.  Adja hozzá a WriteBuildMessage tevékenységének közvetlenül a **Normál kimenő kezelje a** csoport alatt. A fontosság beállítása = "Microsoft.TeamFoundation.Build.Client.BuildMessageImportance.High" és az üzenet = "adatok". Ezzel biztosíthatja, hogy a parancsprogram a szabványos eredménye az összeállítás kimeneti kérjen ír.

    9.  A InvokeProcess **Hiba kimeneti kezelése** szakaszban rendelés_részletei.mennyiség állítsa a szövegdoboz "adatok". Ez a változó a standard hiba adatokat tárolja.

    10. Adja hozzá a WriteBuildError tevékenységének közvetlenül a **Hiba kimeneti kezelje a** csoport alatt. Az üzenet megadása = "adatok". Ezzel biztosíthatja, hogy a parancsprogram a standard hiba az összeállítás hiba kimeneti kérjen ír.

    11. Javítsa ki a hibák, kék felkiáltójel jelek jelölt. Mutasson a felkiáltójel jelek egy emlékeztetőt a hibáról eléréséhez. Mentse a munkafolyamatot, törölje a hibákat.

    A közzététel munkafolyamat-tevékenységek a végeredmény fog kinézni a tervezőben:

    ![Munkafolyamat-tevékenységek][5]

    A közzététel munkafolyamat-tevékenységek a végeredmény XAML így jelenik meg:

        <If Condition="[Not String.IsNullOrEmpty(PublishScriptLocation)]" sap2010:WorkflowViewState.IdRef="If_1">
            <If.Then>
              <Sequence DisplayName="Start Publish" sap2010:WorkflowViewState.IdRef="Sequence_4">
                <Sequence.Variables>
                  <Variable x:TypeArguments="x:String" Name="SubscriptionDataFilePath" />
                  <Variable x:TypeArguments="x:String" Name="PublishScriptFilePath" />
                </Sequence.Variables>
                <mtbwa:ConvertWorkspaceItem DisplayName="Convert publish script filename" sap2010:WorkflowViewState.IdRef="ConvertWorkspaceItem_1" Input="[PublishScriptLocation]" Result="[PublishScriptFilePath]" Workspace="[Workspace]" />
                <mtbwa:ConvertWorkspaceItem DisplayName="Convert subscription filename" sap2010:WorkflowViewState.IdRef="ConvertWorkspaceItem_2" Input="[SubscriptionDataFileLocation]" Result="[SubscriptionDataFilePath]" Workspace="[Workspace]" />
                <mtbwa:InvokeProcess Arguments="[String.Format(&quot; -File &quot;&quot;{0}&quot;&quot; -serviceName {1}&#xD;&#xA;            -storageAccountName {2} -packageLocation &quot;&quot;{3}&quot;&quot;&#xD;&#xA;            -cloudConfigLocation &quot;&quot;{4}&quot;&quot; -subscriptionDataFile &quot;&quot;{5}&quot;&quot;&#xD;&#xA;            -selectedSubscription {6} -environment &quot;&quot;{7}&quot;&quot;&quot;,&#xD;&#xA;            PublishScriptFilePath, ServiceName, StorageAccountName,&#xD;&#xA;            PackageLocation, CloudConfigLocation,&#xD;&#xA;            SubscriptionDataFilePath, SubscriptionName, Environment)]" DisplayName="'Execute Publish Script'" FileName="[PowerShell]" sap2010:WorkflowViewState.IdRef="InvokeProcess_1">
                  <mtbwa:InvokeProcess.ErrorDataReceived>
                    <ActivityAction x:TypeArguments="x:String">
                      <ActivityAction.Argument>
                        <DelegateInArgument x:TypeArguments="x:String" Name="data" />
                      </ActivityAction.Argument>
                      <mtbwa:WriteBuildError Message="{x:Null}" sap2010:WorkflowViewState.IdRef="WriteBuildError_1" />
                    </ActivityAction>
                  </mtbwa:InvokeProcess.ErrorDataReceived>
                  <mtbwa:InvokeProcess.OutputDataReceived>
                    <ActivityAction x:TypeArguments="x:String">
                      <ActivityAction.Argument>
                        <DelegateInArgument x:TypeArguments="x:String" Name="data" />
                      </ActivityAction.Argument>
                      <mtbwa:WriteBuildMessage sap2010:WorkflowViewState.IdRef="WriteBuildMessage_2" Importance="[Microsoft.TeamFoundation.Build.Client.BuildMessageImportance.High]" Message="[data]" mva:VisualBasic.Settings="Assembly references and imported namespaces serialized as XML namespaces" />
                    </ActivityAction>
                  </mtbwa:InvokeProcess.OutputDataReceived>
                </mtbwa:InvokeProcess>
              </Sequence>
            </If.Then>
          </If>
        </Sequence>


7.  Az összeállítás munkafolyamat-sablon és a beadás a fájlt menteni.

8.  Az összeállítás definíciójának szerkesztése (zárja be, ha már meg nyitva), és jelölje be az **Új** gombra, ha még nem látható az új sablon a folyamat sablonok listájában.

9.  A paraméterek megadása tulajdonságértékeket egyéb szakaszában az alábbi képlettel történik:

    1.  CloudConfigLocation = "c:\\esik\\app.publish\\ServiceConfiguration.Cloud.cscfg" *ezt az értéket származik: ($PublishDir)ServiceConfiguration.Cloud.cscfg*

    2.  PackageLocation = "c:\\esik\\app.publish\\ContactManager.Azure.cspkg" *ezt az értéket származik: ($PublishDir)($ProjectName) .cspkg*

    3.  PublishScriptLocation = "c:\\parancsfájlok\\WindowsAzure\\PublishCloudService.ps1"

    4.  ServiceName = "mycloudservicename" *a megfelelő felhőalapú szolgáltatás neve itt használata*

    5.  Környezet = "Átmeneti"

    6.  StorageAccountName = "mystorageaccountname" *Itt megfelelő tárolási fióknevét használata*

    7.  SubscriptionDataFileLocation = "c:\\parancsfájlok\\WindowsAzure\\Subscription.xml"

    8.  SubscriptionName = "alapértelmezés"

    ![Paraméterértékeket tulajdonság][6]

10. A módosítások mentéséhez összeállítása-definíciós verziójának.

11. Várólista építés végrehajtása mindkét csomag összeállítása és közzétételéhez. Ha folytonos integrációs beállítása az eseményindító, a minden beadás fog hajtsa végre a a jelenség.

### <a name="publishcloudserviceps1-script-template"></a>PublishCloudService.ps1 parancsprogram-sablon

```
Param(  $serviceName = "",
        $storageAccountName = "",
        $packageLocation = "",
        $cloudConfigLocation = "",
        $environment = "Staging",
        $deploymentLabel = "ContinuousDeploy to $servicename",
        $timeStampFormat = "g",
        $alwaysDeleteExistingDeployments = 1,
        $enableDeploymentUpgrade = 1,
        $selectedsubscription = "default",
        $subscriptionDataFile = ""
     )


function Publish()
{
    $deployment = Get-AzureDeployment -ServiceName $serviceName -Slot $slot -ErrorVariable a -ErrorAction silentlycontinue
    if ($a[0] -ne $null)
    {
        Write-Output "$(Get-Date -f $timeStampFormat) - No deployment is detected. Creating a new deployment. "
    }
    #check for existing deployment and then either upgrade, delete + deploy, or cancel according to $alwaysDeleteExistingDeployments and $enableDeploymentUpgrade boolean variables
    if ($deployment.Name -ne $null)
    {
        switch ($alwaysDeleteExistingDeployments)
        {
            1
            {
                switch ($enableDeploymentUpgrade)
                {
                    1  #Update deployment inplace (usually faster, cheaper, won't destroy VIP)
                    {
                        Write-Output "$(Get-Date -f $timeStampFormat) - Deployment exists in $servicename.  Upgrading deployment."
                        UpgradeDeployment
                    }
                    0  #Delete then create new deployment
                    {
                        Write-Output "$(Get-Date -f $timeStampFormat) - Deployment exists in $servicename.  Deleting deployment."
                        DeleteDeployment
                        CreateNewDeployment

                    }
                } # switch ($enableDeploymentUpgrade)
            }
            0
            {
                Write-Output "$(Get-Date -f $timeStampFormat) - ERROR: Deployment exists in $servicename.  Script execution cancelled."
                exit
            }
        } #switch ($alwaysDeleteExistingDeployments)
    } else {
            CreateNewDeployment
    }
}

function CreateNewDeployment()
{
    write-progress -id 3 -activity "Creating New Deployment" -Status "In progress"
    Write-Output "$(Get-Date -f $timeStampFormat) - Creating New Deployment: In progress"

    $opstat = New-AzureDeployment -Slot $slot -Package $packageLocation -Configuration $cloudConfigLocation -label $deploymentLabel -ServiceName $serviceName

    $completeDeployment = Get-AzureDeployment -ServiceName $serviceName -Slot $slot
    $completeDeploymentID = $completeDeployment.deploymentid

    write-progress -id 3 -activity "Creating New Deployment" -completed -Status "Complete"
    Write-Output "$(Get-Date -f $timeStampFormat) - Creating New Deployment: Complete, Deployment ID: $completeDeploymentID"

    StartInstances
}

function UpgradeDeployment()
{
    write-progress -id 3 -activity "Upgrading Deployment" -Status "In progress"
    Write-Output "$(Get-Date -f $timeStampFormat) - Upgrading Deployment: In progress"

    # perform Update-Deployment
    $setdeployment = Set-AzureDeployment -Upgrade -Slot $slot -Package $packageLocation -Configuration $cloudConfigLocation -label $deploymentLabel -ServiceName $serviceName -Force

    $completeDeployment = Get-AzureDeployment -ServiceName $serviceName -Slot $slot
    $completeDeploymentID = $completeDeployment.deploymentid

    write-progress -id 3 -activity "Upgrading Deployment" -completed -Status "Complete"
    Write-Output "$(Get-Date -f $timeStampFormat) - Upgrading Deployment: Complete, Deployment ID: $completeDeploymentID"
}

function DeleteDeployment()
{

    write-progress -id 2 -activity "Deleting Deployment" -Status "In progress"
    Write-Output "$(Get-Date -f $timeStampFormat) - Deleting Deployment: In progress"

    #WARNING - always deletes with force
    $removeDeployment = Remove-AzureDeployment -Slot $slot -ServiceName $serviceName -Force

    write-progress -id 2 -activity "Deleting Deployment: Complete" -completed -Status $removeDeployment
    Write-Output "$(Get-Date -f $timeStampFormat) - Deleting Deployment: Complete"

}

function StartInstances()
{
    write-progress -id 4 -activity "Starting Instances" -status "In progress"
    Write-Output "$(Get-Date -f $timeStampFormat) - Starting Instances: In progress"

    $deployment = Get-AzureDeployment -ServiceName $serviceName -Slot $slot
    $runstatus = $deployment.Status

    if ($runstatus -ne 'Running')
    {
        $run = Set-AzureDeployment -Slot $slot -ServiceName $serviceName -Status Running
    }
    $deployment = Get-AzureDeployment -ServiceName $serviceName -Slot $slot
    $oldStatusStr = @("") * $deployment.RoleInstanceList.Count

    while (-not(AllInstancesRunning($deployment.RoleInstanceList)))
    {
        $i = 1
        foreach ($roleInstance in $deployment.RoleInstanceList)
        {
            $instanceName = $roleInstance.InstanceName
            $instanceStatus = $roleInstance.InstanceStatus

            if ($oldStatusStr[$i - 1] -ne $roleInstance.InstanceStatus)
            {
                $oldStatusStr[$i - 1] = $roleInstance.InstanceStatus
                Write-Output "$(Get-Date -f $timeStampFormat) - Starting Instance '$instanceName': $instanceStatus"
            }

            write-progress -id (4 + $i) -activity "Starting Instance '$instanceName'" -status "$instanceStatus"
            $i = $i + 1
        }

        sleep -Seconds 1

        $deployment = Get-AzureDeployment -ServiceName $serviceName -Slot $slot
    }

    $i = 1
    foreach ($roleInstance in $deployment.RoleInstanceList)
    {
        $instanceName = $roleInstance.InstanceName
        $instanceStatus = $roleInstance.InstanceStatus

        if ($oldStatusStr[$i - 1] -ne $roleInstance.InstanceStatus)
        {
            $oldStatusStr[$i - 1] = $roleInstance.InstanceStatus
            Write-Output "$(Get-Date -f $timeStampFormat) - Starting Instance '$instanceName': $instanceStatus"
        }

        $i = $i + 1
    }

    $deployment = Get-AzureDeployment -ServiceName $serviceName -Slot $slot
    $opstat = $deployment.Status

    write-progress -id 4 -activity "Starting Instances" -completed -status $opstat
    Write-Output "$(Get-Date -f $timeStampFormat) - Starting Instances: $opstat"
}

function AllInstancesRunning($roleInstanceList)
{
    foreach ($roleInstance in $roleInstanceList)
    {
        if ($roleInstance.InstanceStatus -ne "ReadyRole")
        {
            return $false
        }
    }

    return $true
}

#configure powershell with Azure 1.7 modules
Import-Module Azure

#configure powershell with publishsettings for your subscription
$pubsettings = $subscriptionDataFile
Import-AzurePublishSettingsFile $pubsettings
Set-AzureSubscription -CurrentStorageAccountName $storageAccountName -SubscriptionName $selectedsubscription
Select-AzureSubscription $selectedsubscription

#set remaining environment variables for Azure cmdlets
$subscription = Get-AzureSubscription $selectedsubscription
$subscriptionname = $subscription.subscriptionname
$subscriptionid = $subscription.subscriptionid
$slot = $environment

#main driver - publish & write progress to activity log
Write-Output "$(Get-Date -f $timeStampFormat) - Azure Cloud Service deploy script started."
Write-Output "$(Get-Date -f $timeStampFormat) - Preparing deployment of $deploymentLabel for $subscriptionname with Subscription ID $subscriptionid."

Publish

$deployment = Get-AzureDeployment -slot $slot -serviceName $servicename
$deploymentUrl = $deployment.Url

Write-Output "$(Get-Date -f $timeStampFormat) - Created Cloud Service with URL $deploymentUrl."
Write-Output "$(Get-Date -f $timeStampFormat) - Azure Cloud Service deploy script finished."
```

## <a name="next-steps"></a>Következő lépések

Ahhoz, hogy a távoli hibakereséshez folyamatos kézbesítési használatakor, olvassa el a [engedélyezése a távoli hibakereséshez Azure közzététele folyamatos kézbesítési használatakor](cloud-services-virtual-machines-dotnet-continuous-delivery-remote-debugging.md)című témakört.

  [Folytonos kézbesítési Azure Visual Studio Team Services használatával]: cloud-services-continuous-delivery-use-vso.md  
  [Team Foundation Build szolgáltatás]: https://msdn.microsoft.com/library/ee259687.aspx
  [.NET Framework 4]: https://www.microsoft.com/download/details.aspx?id=17851
  [.NET Framework 4.5]: https://www.microsoft.com/download/details.aspx?id=30653
  [.NET-keretrendszer 4.5.2.]: https://www.microsoft.com/download/details.aspx?id=42643
  [A Szerkesztés rendszer méretezése]: https://msdn.microsoft.com/library/dd793166.aspx
  [Üzembe helyezéséhez és Szerkesztés kiszolgáló konfigurálása]: https://msdn.microsoft.com/library/ms181712.aspx
  [Azure PowerShell-parancsmagok]: powershell-install-configure.md
  [the .publishsettings file]: https://manage.windowsazure.com/download/publishprofile.aspx?wa=wsignin1.0
  [0]: ./media/cloud-services-dotnet-continuous-delivery/tfs-01bc.png
  [2]: ./media/cloud-services-dotnet-continuous-delivery/tfs-02.png
  [3]: ./media/cloud-services-dotnet-continuous-delivery/common-task-tfs-03.png
  [4]: ./media/cloud-services-dotnet-continuous-delivery/common-task-tfs-04.png
  [5]: ./media/cloud-services-dotnet-continuous-delivery/common-task-tfs-05.png
  [6]: ./media/cloud-services-dotnet-continuous-delivery/common-task-tfs-06.png
