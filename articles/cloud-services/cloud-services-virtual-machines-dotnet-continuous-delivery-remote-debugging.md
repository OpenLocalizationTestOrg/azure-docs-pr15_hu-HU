<properties
    pageTitle="Engedélyezése a távoli hibakeresés folyamatos kézbesítési |} Microsoft Azure"
    description="Megtudhatja, hogy miként engedélyezése a távoli hibakeresés Azure szeretne telepíteni, folytonos kézbesítési használatakor"
    services="cloud-services"
    documentationCenter=".net"
    authors="TomArcher"
    manager="douge"
    editor=""/>

<tags
    ms.service="cloud-services"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-multiple"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="tarcher"/>

# <a name="enable-remote-debugging-when-using-continuous-delivery-to-publish-to-azure"></a>Engedélyezése a távoli hibakeresés folyamatos kézbesítési Azure közzététele használatakor

Lehetősége van engedélyezni a távoli hibakeresése során az Azure-cloud services vagy a virtuális gépeken futó, ezeket a lépéseket követve Azure közzététele [folyamatos kézbesítési](cloud-services-dotnet-continuous-delivery.md) használatakor.

## <a name="enabling-remote-debugging-for-cloud-services"></a>Távoli hibakeresés cloud Services engedélyezése

1. A Szerkesztés Agent beállítása a kezdeti környezet Azure [Parancssori Azure készítése](http://msdn.microsoft.com/library/hh535755.aspx)című témakörben ismertetett módon.
2. A távoli hibakeresési futtatókörnyezet (msvsmon.exe) szükség a csomagnak, mert a [Távoli Tools for Visual Studio 2015](http://www.microsoft.com/en-us/download/details.aspx?id=48155) (vagy a Visual Studio 2013 használata a [Microsoft Visual Studio 2013 frissítés 5-ös távoli eszközei](https://www.microsoft.com/en-us/download/details.aspx?id=48156) ) telepítése. Alternatív megoldásként másolhatja a távoli hibakeresési bináris, amelyen telepítve van a Visual Studio rendszerből.
3. Tanúsítvány létrehozása [Az Azure Cloud Services a tanúsítványok – áttekintés](cloud-services-certs-create.md)című témakörben ismertetett módon. A .pfx és RDP tanúsítvány ujjlenyomat, és töltse fel a tanúsítvány a cél felhőalapú szolgáltatást.
4. Használja az alábbi lehetőségek a MSBuild parancssorban létre, és a távoli hibakeresési engedélyezett csomag. (Helyette tényleges elérési út a rendszer és a project fájlok szög címsorainak elemek.)

        msbuild /TARGET:PUBLISH /PROPERTY:Configuration=Debug;EnableRemoteDebugger=true;VSX64RemoteDebuggerPath="<remote tools path>";RemoteDebuggerConnectorCertificateThumbprint="<thumbprint of the certificate added to the cloud service>";RemoteDebuggerConnectorVersion="2.7" "<path to your VS solution file>"

    `VSX64RemoteDebuggerPath`van a mappa elérési útját tartalmazó msvsmon.exe a távoli Tools for Visual Studio.
    `RemoteDebuggerConnectorVersion`a felhőszolgáltatásában Azure SDK verziója. A Visual Studio verziója is egyeznie kell.

5. Tegye közzé a cél felhőalapú szolgáltatást az előző lépésben létrehozott csomag és .cscfg-fájl használatával.
6. Importálja a tanúsítványt (.pfx fájl), amelynek az Azure SDK Visual Studio .NET telepítve a számítógépre. Ügyeljen arra, hogy importálja a `CurrentUser\My` tanúsítvány mentése, egyéb esetben csatolása a Visual Studio hibaelhárítóját sikertelen lesz.

## <a name="enabling-remote-debugging-for-virtual-machines"></a>Virtuális gépeken futó távoli hibakeresés engedélyezése

1. Hozzon létre egy Azure virtuális gépen. Lásd: [a Windows Server operációs rendszert futtató virtuális gép létrehozása](../virtual-machines/virtual-machines-windows-hero-tutorial.md) vagy [létrehozása és kezelése a Visual Studióban Azure virtuális gépeken futó](../virtual-machines/virtual-machines-windows-classic-manage-visual-studio.md).
2. [Azure klasszikus portál lapja](http://go.microsoft.com/fwlink/p/?LinkID=269851)a virtuális géphez irányítópult lásd: a virtuális géphez **RDP tanúsítvány UJJLENYOMAT**megtekintése Ez az érték szolgál a `ServerThumbprint` az kiterjesztést beállítás értékét.
3. Ügyfél tanúsítvány létrehozása [Az Azure Cloud Services a tanúsítványok – áttekintés](cloud-services-certs-create.md) című témakörben ismertetett módon (megőrzése a .pfx és RDP tanúsítvány ujjlenyomat).
4. Azure Powershell telepítése (verzió 0.7.4 vagy újabb) [telepítése és konfigurálása az Azure PowerShell](../powershell-install-configure.md)című témakörben ismertetett módon.
5. Futtassa a következő a RemoteDebug bővítmény engedélyezése. Cserélje ki az elérési út és a személyes adatok saját, például az előfizetés nevét, a szolgáltatás neve és a ujjlenyomat.

    >[AZURE.NOTE] Ez a parancsfájl Visual Studio 2015 van beállítva. Ha a Visual Studio 2013 használata esetén módosítsa a `$referenceName` és `$extensionName` hozzárendelések használatához az alábbi `RemoteDebugVS2013` (helyett `RemoteDebugVS2015`).

    <pre>
   AzureAccount hozzáadása

    Jelölje ki-AzureSubscription "Microsoft előfizetésem"

    $vm = get-AzureVM - ServiceName "mytestvm1"-neve "mytestvm1"

    $endpoints = @( ,@{Name="RDConnVS2013"; PublicPort = 30400; PrivatePort = 30398} ,@{Name="RDFwdrVS2013"; PublicPort = 31400; PrivatePort = 31398})  

    foreach (a $endpoints $endpoint) {hozzáadása-AzureEndpoint - virtuális $vm-$endpoint nevet. Name - protokoll tcp - PublicPort $endpoint. PublicPort - Helyi_port $endpoint. PrivatePort}

    $referenceName = "Microsoft.VisualStudio.WindowsAzure.RemoteDebug.RemoteDebugVS2015" $publisher = "Microsoft.VisualStudio.WindowsAzure.RemoteDebug" $extensionName = "RemoteDebugVS2015" $version = "1.*" $publicConfiguration = "<PublicConfig>< Connector.Enabled > igaz < /Connector.Enabled ><ClientThumbprint>56D7D1B25B472268E332F7FC0C87286458BFB6B2</ClientThumbprint><ServerThumbprint>E7DCB00CB916C468CC3228261D6E4EE45C8ED3C6</ServerThumbprint><ConnectorPort>30398</ConnectorPort><ForwarderPort>31398</ForwarderPort></PublicConfig>."

    $vm |} Set-AzureVMExtension `
    -ReferenceName $referenceName ` 
    – Publisher $publisher `
    -ExtensionName $extensionName ` 
    -verzió $version "- PublicConfiguration $publicConfiguration

    foreach (a $vm $extension. VIRTUÁLIS. ResourceExtensionReferences) {Ha (($extension. Hivatkozásnév - eq $referenceName) `
    -and ($extension.Publisher -eq $publisher) ` 
    - és ($extension. Név - eq $extensionName) "- és ($extension. Verzió - eq $version)) {$extension. ResourceExtensionParameterValues [0]. Kulcs = "config.txt fájl" Oldaltörés}}

    $vm |} Frissítés-AzureVM </pre>

6. Importálja a tanúsítványt (.pfx), amelynek az Azure SDK Visual Studio .NET telepítve a számítógépre.
