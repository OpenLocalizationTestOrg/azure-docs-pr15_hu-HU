<properties
    pageTitle="Egy Windows Azure virtuális képet |} Microsoft Azure"
    description="Készítsen egy képet, és a klasszikus telepítési modellt létrehozott Azure Windows virtuális gépet."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor="tysonn"
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="cynthn"/>

#<a name="capture-an-image-of-an-azure-windows-virtual-machine-created-with-the-classic-deployment-model"></a>Készítsen egy képet, és a klasszikus telepítési modellt létrehozott Azure Windows virtuális gépet.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Erőforrás-kezelő modell információt című témakörben talál [Legyen másolat virtuális Windows Azure-ban futó](virtual-machines-windows-vhd-copy.md).


Ez a cikk bemutatja, hogyan rögzítheti egy Azure virtuális gép, hogy használni tudja azt képként létrehozása más virtuális gépeken futó Windows operációs rendszert futtató. Ezen a képen az operációs rendszer lemezen és a bármilyen adatok lemezt a virtuális gép csatolt tartalmazza. Hálózati beállítások, akkor nem tartalmazza, úgy kell konfigurálni azokat a többi virtuális gépeken futó, amely a képhez létrehozásakor.

Azure a képet a **Saját képeket**tárolja. Ez a képet feltöltötte tároló ugyanazon a helyen. [Képek kapcsolatos részletekért kapcsolatos tudnivalók](virtual-machines-linux-classic-about-images.md)című részben virtuális gépeken futó képe.

##<a name="before-you-begin"></a>Első lépések##

Ezek a lépések feltételezik, hogy korábban már létrehozott egy Azure virtuális gép és konfigurálni az operációs rendszer, beleértve a bármely adatok lemezt csatolása. Ha ez még nem kész, tanulmányozza az alábbi útmutatókat:

- [Virtuális gép létrehozása képből](virtual-machines-windows-classic-createportal.md)
- [Hogyan csatolhat adatok lemezen virtuális gépen](virtual-machines-windows-classic-attach-disk.md)
- Győződjön meg arról, hogy a kiszolgálói szerepkörök használhatók a Sysprep. További tudnivalókért olvassa el a [Kiszolgálói szerepkörök Sysprep támogatása](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles)című témakört.

> [AZURE.WARNING] Ez a folyamat törli az eredeti virtuális gép, miután a rögzítés. 

Előtt caputuring az Azure virtuális gép képét javasolt a cél virtuális gép biztonsági másolatot. Azure virtuális gépeken futó Azure biztonsági mentéssel készíthető. A részletekért olvassa el, [készítsen biztonsági másolatot az Azure virtuális gépeken futó](../backup/backup-azure-vms.md). Egyéb megoldások tanúsított partnertől érhetők el. Keressen az Azure piactéren elérhető, hogy jelenleg elérhető.


##<a name="capture-the-virtual-machine"></a>A virtuális gép rögzítése

1. Az [Azure klasszikus portál](http://manage.windowsazure.com) **Kapcsolódás** virtuális gépen. Útmutatásért lásd: [Bejelentkezés az Windows Server operációs rendszert futtató virtuális géphez] [].

2.  Nyisson meg egy parancssorablakot rendszergazdaként.

3.  A könyvtár módosítása `%windir%\system32\sysprep`, majd futtassa a sysprep.exe.

4.  A **Rendszer előkészítése eszköz** párbeszédpanel jelenik meg. Tegye a következőket:

    - A **Rendszer a törlési művelet**jelölje ki a **Írja be a rendszer Out kész élmény (OOBE)** , és győződjön meg arról, hogy be van-e jelölve **Generalize** . Sysprep használatával kapcsolatos további tudnivalókért lásd: [Sysprep használatát mikéntjéről: Bevezetés az][].

    - Válassza a **Leállítás** **Leállítási beállítások**.

    - Kattintson az **OK gombra**.

    ![A Sysprep](./media/virtual-machines-windows-classic-capture-image/SysprepGeneral.png)

7.  Sysprep leállítja a virtuális, a számítógépre, amelyen a állapotot módosítja a virtuális gép az Azure klasszikus portálon **leállt**.

8.  Az Azure klasszikus portálon kattintson a **virtuális gépeken futó** , és válassza ki a rögzíteni kívánt virtuális gépet.

9.  A parancssávon kattintson a **Rögzítés**gombra.

    ![Virtuális gép rögzítése](./media/virtual-machines-windows-classic-capture-image/CaptureVM.png)

    A **Rögzítés a virtuális gép** párbeszédpanel jelenik meg.

10. A **Kép neve**mezőbe írja be egy nevet az új kép.

11. Mielőtt Windows Server kép elhelyezése a kívánt egyéni képek, az előző lépésekben utasításai Sysprep futtatásával kell általános. Kattintson a elvégzett, amely jelzi **a virtuális gépen Sysprep kell futtatni** .

12. Kattintson a pipára készítsen egy képet. Az új kép a **képek**letöltése érhető el.

    ![A sikeres rögzítési képe](./media/virtual-machines-windows-classic-capture-image/VMCapturedImageAvailable.png)

##<a name="next-steps"></a>Következő lépések

A kép készen áll a virtuális gépeken futó létrehozására szolgál. Ehhez kell létrehoznia egy virtuális gépet használ a **Gyűjtemény a** menüpont, és válassza az imént létrehozott képe. Című cikkben olvashat [létrehozása képből virtuális géphez](virtual-machines-windows-classic-createportal.md).



[Bejelentkezés Windows Server operációs rendszert futtató virtuális gép]: virtual-machines-windows-classic-connect-logon.md
[Sysprep használatáról: Bevezetés]: http://technet.microsoft.com/library/bb457073.aspx
[Run Sysprep.exe]: ./media/virtual-machines-capture-image-windows-server/SysprepCommand.png
[Enter Sysprep.exe options]: ./media/virtual-machines-windows-classic-capture-image/SysprepGeneral.png
[The virtual machine is stopped]: ./media/virtual-machines-capture-image-windows-server/SysprepStopped.png
[Capture an image of the virtual machine]: ./media/virtual-machines-windows-classic-capture-image/CaptureVM.png
[Enter the image name]: ./media/virtual-machines-capture-image-windows-server/Capture.png
[Image capture successful]: ./media/virtual-machines-capture-image-windows-server/CaptureSuccess.png
[Use the captured image]: ./media/virtual-machines-capture-image-windows-server/MyImagesWindows.png
