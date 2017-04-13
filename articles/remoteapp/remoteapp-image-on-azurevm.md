<properties
    pageTitle="Hozzon létre egy Azure virtuális alapján Azure RemoteApp kép |} Microsoft Azure"
    description="Megtudhatja, hogy miként hozzon létre egy képet Azure RemoteApp az Azure virtuális gép kezdve."
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



# <a name="create-a-azure-remoteapp-image-based-on-an-azure-virtual-machine"></a>Létrehozása az Azure virtuális gép alapján Azure RemoteApp képe

> [AZURE.IMPORTANT]
> Azure RemoteApp megszakad alatt. Olvassa el a további információt a [közlemény](https://go.microsoft.com/fwlink/?linkid=821148) .

Azure RemoteApp képek (amely a webhelycsoport a megosztott alkalmazások tartsa) az Azure virtuális számítógépről is létrehozhat. Úgy lehetett is dönt, hogy egy virtuális gép képet az Azure virtuális képgyűjtemény, amely megfelel az összes Azure RemoteApp kép jelöltük – használhatja, hogy virtuális kép kiindulási pontként saját virtuális, ha azt szeretné. Keresse meg a "Windows Server távoli asztali munkamenetgazda" Kép helye a tárban.

Saját kép egy Azure virtuális alapján létrehozása - létrehozása a képet, és töltse fel a Azure virtuális tárból Azure RemoteApp két lépésből áll.

## <a name="create-a-custom-image-based-on-an-azure-vm"></a>Létrehozása az Azure virtuális alapuló egyéni képe

Ezeket a lépéseket használatával hozzon létre egy képet az Azure virtuális alapján.

1. Hozzon létre egy Azure virtuális gépen. A "Windows Server távoli asztali munkamenetgazda" vagy "Windows Server távoli asztali munkamenet Host a Microsoft Office 365 ProPlus" kép használhatja az Azure virtuális gép kép gyűjteményből. Ezen a képen megfelel az összes Azure RemoteApp sablon képe.

    Részletekért olvassa el [a virtuális létrehozása a Windows operációs rendszert futtató](../virtual-machines/virtual-machines-windows-hero-tutorial.md).

2. Csatlakozás a virtuális és telepítése és konfigurálása az alkalmazásokat, hogy meg szeretné osztani RemoteApp keresztül. Ellenőrizze, hogy minden további Windows beállításokat, az alkalmazások által igényelt végrehajtásához.

    A részletekért megtudhatja, [hogy miként jelentkezzen be a virtuális gépen futó Windows Server-be](../virtual-machines/virtual-machines-windows-classic-connect-logon.md).

3. A Windows Server távoli asztali munkamenetgazda képek egyikét használja, ha van egy része érvényességi parancsfájl, amely biztosítja a virtuális megfelel-e a RemoteApp előtti reqs. Parancsfájl futtatásához kattintson duplán az asztalon **ValidateRemoteAppImage** . Győződjön meg arról, hogy a parancsprogram által jelzett összes hibát javított utána folytassa a következő lépéssel.

4. SYSPREP generalize, és készítsen egy képet. Megtudhatja, [hogy miként rögzítése a Windows virtuális gép sablonként használandó](../virtual-machines/virtual-machines-windows-classic-capture-image.md) utasításokat.



## <a name="import-the-image-into-the-azure-remoteapp-image-library"></a>A kép importálása az Azure RemoteApp képtárából

Az új kép importálása az Azure RemoteApp az alábbi lépésekkel:

1. A **Webhelysablonok képét** lapon:
    - Ha nincs már meglévő képek, kattintson a **fel- vagy importálása egy sablonként képe**.
    - Ha már legalább egy képet, kattintson a **+** új beállítása.

2. Jelölje ki **a virtuális gépeken futó képet importálása** tárat, és kattintson a **Tovább**gombra.

3. A következő lapon válassza ki az egyéni képet a listából, és győződjön meg arról, hogy karaktert, majd a kép létrehozásakor lépéseket. Kattintson a **Tovább**gombra.
4. Írjon be egy nevet az új RemoteApp kép válassza ki a helyet, majd kattintson az importálás megkezdéséhez a bejelölést.

> [AZURE.NOTE] Importálhatja a képek Azure virtuális gépeken futó által támogatott Azure RemoteApp által támogatott Azure bárhol Azure bárhonnan. Attól függően, hogy a helyek az importálás legfeljebb 25 percig is eltarthat.

Most már készen áll az új webhelycsoport vagy létrehozása a [felhőben](remoteapp-create-cloud-deployment.md) gyűjtemény vagy [hibrid](remoteapp-create-hybrid-deployment.md)igényeitől függően.
