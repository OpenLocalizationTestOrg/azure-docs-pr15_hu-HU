<properties
   pageTitle="Az Azure parancssori build |} Microsoft Azure"
   description="Az Azure parancssori összeállításához"
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags
   ms.service="multiple"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="command-line-build-for-azure"></a>Az Azure parancssori összeállításához

## <a name="overview"></a>– Áttekintés

A parancssorba MSBuild futtatásával Azure telepítéshez csomag hozhat létre. Állítsa be, és hibakeresési, előkészítése és munkakörnyezeti mellett néhány folyamat automatizálása buildjeiben megadása.


## <a name="microsoft-build-engine-msbuild"></a>Microsoft Build motor (MSBuild)

A Microsoft összeállítása motor (MSBuild) használatával készíthet verziójában termékek labor környezetben, amely nem tartalmaz a Visual Studio. Extensible és teljesen támogatott, a Microsoft project-fájlokat az XML formátum MSBuild használja. Ez a fájlformátum, az elemek kell leírhatja egy vagy több platformokon és a konfigurációk épített.

Futtathatja a MSBuild parancssorba, és ez a témakör leírja, hogy megközelítés. A parancssorba hozhat létre a projekt egyes konfigurációk állítható be. Is meghatározhat Hasonlóképpen, a tárolók gyűjt a MSBuild parancsot. Parancssori paramétereket és MSBuild kapcsolatos további tudnivalókért olvassa el a [MSBuild parancssori hivatkozás](https://msdn.microsoft.com/library/ms164311.aspx)című témakört.

## <a name="installation"></a>Telepítés

Ahogy az alábbi eljárás azt ismerteti, telepítenie kell szoftverek és eszközök build kiszolgálói előtt MSBuild használatával hozhat létre az Azure-csomagok:

1. Telepítse a .NET-keretrendszer 4-es vagy újabb, amely magában foglalja MSBuild.

1. Telepítse az [Azure létrehozáshoz használható eszközök](http://go.microsoft.com/fwlink/?LinkId=394615) (keresse meg a MicrosoftAzureAuthoringTools-x64.msi vagy MicrosoftAzureAuthoringTools-x86.msi.

1. Telepítse az [Azure-tárak a .NET rendszerhez](http://go.microsoft.com/fwlink/?LinkId=394616) (keresse meg a MicrosoftAzureLibsForNet-x64.msi vagy MicrosoftAzureLibs-x86.msi.

1. A Microsoft.WebApplication.targets fájl másolása egy másik számítógépen a Visual Studio példányát.

    A fájl található a címtár-C:\Program Files (x86) \MSBuild\Microsoft\Visual Studio\v12.0\WebApplications (v11.0 for Visual Studio 2012), és másolja azt a build kiszolgálói azonos címtárhoz.

1. Telepítse az [Azure Tools for Visual Studio](http://go.microsoft.com/fwlink/?LinkId=394616).

    Keresse meg a Visual Studio 2013 projektek létrehozásának WindowsAzureTools.vs120.exe.

## <a name="msbuild-parameters"></a>MSBuild paraméterei

A csomag létrehozása legegyszerűbben a MSBuild futtatásához a `/t:Publish` lehetőséget. Alapértelmezés szerint ez a parancs könyvtárat hoz létre a projekthez, például ProjectDir\bin\Configuration\app.publish gyökérmappájából viszonyított\. generál egy Azure projekten, ha két fájlokat, a csomagfájlt, magát, és a kapcsolódó konfigurációs fájl létrehozása:

- Project.cspkg

- ServiceConfiguration.TargetProfile.cscfg

Alapértelmezés szerint minden Azure project tartalmaz olyan egy helyi (Hibakeresés) szolgáltatás-konfigurációs fájl hoz létre, és egy másikat a felhőbe (átmeneti vagy gyártási) buildjeiben, de hozzáadhat vagy eltávolíthat konfigurációjának fájlokat, szükség szerint. Ha egy csomag Visual Studio generál, akkor megkéri melyik szolgáltatás-konfigurációs fájl, amelyet fel szeretne venni a csomag mellett. Ha egy csomag MSBuild használatával generál, a program a helyi szolgáltatás-konfigurációs fájl alapértelmezés szerint látható. Egy másik konfigurációjának fájlt tartalmazza, állítsa be a `TargetProfile` tulajdonság MSBuild parancs (`MSBuild /t:Publish /p:TargetProfile=ProfileName`).

Az tárolt csomag és a konfigurációs fájl másik könyvtár használni szeretné, ha az elérési út segítségével be a `/p:PublishDir=Directory\` beállítást, beleértve a záró fordított perjel elválasztó karaktert.

## <a name="deployment"></a>Telepítési

Miután elkészült a csomagot, Azure telepítheti azt. Az oktatóanyagot, amely bemutatja ezt a folyamatot, az Azure webhelyén talál. Folyamatok automatizálása, akkor olvassa el [Az Azure-ban Cloud Services folyamatos kézbesítési](./cloud-services/cloud-services-dotnet-continuous-delivery.md)olvashat.
