<properties
    pageTitle="Hozzon létre egy Azure RemoteApp kép |} Microsoft Azure"
    description="További tudnivalók az Azure RemoteApp képek létrehozását ismertető elérhető beállítások:"
    services="remoteapp"
    documentationCenter=""
    authors="lizap"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="elizapo" />



# <a name="create-an-azure-remoteapp-image"></a>Hozzon létre egy Azure RemoteApp képe

> [AZURE.IMPORTANT]
> Azure RemoteApp megszakad alatt. Olvassa el a további információt a [közlemény](https://go.microsoft.com/fwlink/?linkid=821148) .

Azure RemoteApp képek segítségével az alkalmazásokat, hogy a felhasználók megosztásban tartsa. (Hogy a képe vennie és létrehozásához VMs –, hogy a felhasználók hozzáférés Ha az azok jelentkezzen be az Azure RemoteApp.) Hozzon létre egy Azure RemoteApp a választásának megfelelően az alkalmazások, a felhő vagy hibrid legyen, megkezdése: hozzon létre egy képet az alkalmazások telepítése. Hozzon létre egy webhelycsoport adott képhez használó felhasználók hozzárendelése a webhelycsoport és alkalmazások közzététele azoknak a felhasználóknak.

Létrehozásának és képek használatáról több lehetőség közül választhat. Kép egyszerű [követelmény](remoteapp-imagereqs.md) , hogy futtassa a Windows Server 2012 R2, és a távoli asztali munkamenet Host (RDSH) szerepkör telepítve van. Hogyan el, hogy hol dolog, amit hozzá érdekes.

Amikor képeket az alábbi lehetőség közül választhat:

- Importálás és egy [képet az Azure virtuális gép alapján](remoteapp-image-on-azurevm.md)használható. Ez a helyes, például a vállalati verzió egyéni beállítások szükségesek. A kép az alkalmazás személyre szabhatja.
- [Hozzon létre, és töltse fel az egyéni képet](remoteapp-create-custom-image.md)is. Ez a helyes, ha már rendelkezik a helyszíni távoli asztali szolgáltatásokat üzembe használt kép.
- A [Webhelysablonok képét](remoteapp-images.md) az RemoteApp előfizetésében található használhatja. Ezeket a képeket jönnek létre a RemoteApp csoportja által fenntartott és tartalmaz néhány szabványos alkalmazás (például az Office csomagot), amely legyen elérhető a felhasználóknak. Figyelje meg, hogy csak az Office 365 Pro Plus képet egy gyártási beállítással is használható.

Függetlenül attól, ahová a képet kap, vagy hogyan hoz létre célszerű győződjön meg arról, hogy megismeri a [alkalmazás – követelmények](remoteapp-appreqs.md) győződjön meg arról, hogy az alkalmazás működik jól RemoteApp. Ezután a következő lépésként [felhő](remoteapp-create-cloud-deployment.md) vagy [hibrid](remoteapp-create-hybrid-deployment.md) gyűjtemény létrehozásához.
