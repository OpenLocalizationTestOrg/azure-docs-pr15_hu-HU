<properties
    pageTitle="A Windows virtuális általánosítása |} Microsoft Azure"
    description="Ismerje meg, az erőforrás-kezelő telepítési modell használata a Windows virtuális generalize Sysprep használatával."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor="tysonn"
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/20/2016"
    ms.author="cynthn"/>
    
    
    
    
# <a name="generalize-a-windows-virtual-machine-using-sysprep"></a>A Windows virtuális gép Sysprep használata generalize

Ez a szakasz megtudhatja, hogy hogyan használható képként a Windows virtuális géphez generalize. Sysprep eltávolítja az összes a személyes fiók adatait, többek között, és a gép képként használandó előkészíti. További információ a rendszer-előkészítő eszközről [Sysprep használatát mikéntjéről: Bevezetés az](http://technet.microsoft.com/library/bb457073.aspx).

Győződjön meg arról, hogy a számítógépen futó kiszolgálói szerepkörök Sysprep által támogatott. További tudnivalókért lásd: a [Kiszolgálói szerepkörök Sysprep támogatása](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles)

>[AZURE.IMPORTANT] Ha rendszerben Sysprep Azure először a virtuális feltöltése előtt győződjön meg arról, hogy [a virtuális készített](virtual-machines-windows-prepare-for-upload-vhd-image.md) Sysprep futtatása előtt. 

1. Jelentkezzen be a Windows virtuális gépen.

2. Nyissa meg a parancssorablakot rendszergazdaként. A könyvtár módosítása **%windir%\system32\sysprep**, és futtassa `sysprep.exe`.

3. **Rendszer-előkészítő eszköz** párbeszédpanelen jelölje be a **rendszer adja meg a kész élmény (OOBE)**, és győződjön meg arról, hogy a **Generalize** jelölőnégyzet be van-e jelölve.

4. Válassza a **Leállítás** **Leállítási beállítások**.

5. Kattintson az **OK gombra**.

    ![Indítsa el a Sysprep](./media/virtual-machines-windows-upload-image/sysprepgeneral.png)

6. Sysprep befejeztével a virtuális gép leáll. 

## <a name="next-steps"></a>Következő lépések

- Ha a virtuális a helyszíni, akkor most [Töltse fel a az Azure virtuális](virtual-machines-windows-upload-image.md).
- Ha a virtuális már Azure-ban, [Hozzon létre egy képet a általános virtuális](virtual-machines-windows-capture-image.md)is.