<properties
    pageTitle="Távoli asztal egy Linux virtuális |} Microsoft Azure"
    description="Megtudhatja, hogy miként telepítheti és konfigurálása a Microsoft Azure Linux virtuális csatlakozás távoli asztali"
    services="virtual-machines-linux"
    documentationCenter=""
    authors="SuperScottz"
    manager="timlt"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="02/01/2016"
    ms.author="mingzhan"/>


#<a name="using-remote-desktop-to-connect-to-a-microsoft-azure-linux-vm"></a>A Microsoft Azure Linux virtuális csatlakozás távoli asztali használatával

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]


##<a name="overview"></a>– Áttekintés

RDP (távoli asztali Protocol) a Windowshoz használt protokoll. Hogyan lehet használjuk RDP távolról csatlakozhat egy Linux virtuális (virtuális számítógép)?

Ez az útmutató képet ad az answer! Azt, hogy telepítse és a Microsoft Azure Linux virtuális a config xrdp segítségével, és nézheti meg a távoli asztal beolvasása egy Windows számítógépre. Ez az útmutató példaként Ubuntu vagy OpenSUSE operációs rendszert futtató Linux virtuális fogjuk használni.

Xrdp egy megnyitása RDP forráskiszolgáló, amely lehetővé teszi a Linux server kapcsolódni távoli asztali Windows számítógépről. Sokkal nicer, mint (ezek olyan virtuális hálózati számítások) VNC hajtja végre. VNC "JPEG" minőség és a lassú viselkedése a sorozat rendelkezik, mivel a RDP gyors és átlátszó kristálykék.


> [AZURE.NOTE] A Microsoft Azure virtuális Linux operációs rendszert futtató már kell rendelkeznie. Szeretne létrehozni, és állítsa be a Linux virtuális, olvassa el a az [Azure Linux virtuális oktatóprogram](virtual-machines-linux-classic-createportal.md)című témakört.


##<a name="create-endpoint-for-remote-desktop"></a>Távoli asztali végpont létrehozásához
Fogjuk használni az alapértelmezett végpont 3389 távoli asztali a dokumentumban. Úgy beállítani 3389 végpont a Linux virtuális, például alább a távoli asztali:


![kép](./media/virtual-machines-linux-classic-remote-desktop/no1.png)


Ha nem tudja, hogy hogyan állíthatja be a virtuális végpontot, akkor a [útmutatást](virtual-machines-linux-classic-setup-endpoints.md).


##<a name="install-gnome-desktop"></a>Gnome asztali telepítése

Csatlakozás a Linux virtuális gitt keresztül, és telepítse `Gnome Desktop`.

Ubuntu használja:

    #sudo apt-get update
    #sudo apt-get install ubuntu-desktop


OpenSUSE használja:

    #sudo zypper install gnome-session

##<a name="install-xrdp"></a>Xrdp telepítése

Ubuntu használja:

    #sudo apt-get install xrdp

OpenSUSE használja:

> [AZURE.NOTE] Frissítés a OpenSUSE verzióval használja az alábbi parancsot, alatt verziószáma egy példa parancs `OpenSUSE 13.2`.

    #sudo zypper in http://download.opensuse.org/repositories/X11:/RemoteDesktop/openSUSE_13.2/x86_64/xrdp-0.9.0git.1401423964-2.1.x86_64.rpm
    #sudo zypper install tigervnc xorg-x11-Xvnc xterm remmina-plugin-vnc


##<a name="start-xrdp-and-set-xdrp-service-at-boot-up"></a>Indítsa el a xrdp és xdrp szolgáltatás értéken indítási felfelé

OpenSUSE használja:

    #sudo systemctl start xrdp
    #sudo systemctl enable xrdp

A Ubuntu, xrdp fog elindulni és eanbled a állítja automatikusan a telepítés után.

##<a name="using-xfce-if-you-are-using-ubuntu-version-later-than-ubuntu-1204lts"></a>Xfce használata, ha később Ubuntu 12.04LTS Ubuntu verzióját használja

Mivel az aktuális xrdp nem támogatják az Gnome asztali Ubuntu 12.04LTS később Ubuntu verziójáról, fogjuk használni `xfce` asztali helyette.

Telepítse `xfce`, használata:

    #sudo apt-get install xubuntu-desktop

Engedélyezheti `xfce`, használata:

    #echo xfce4-session >~/.xsession

A konfigurációs fájl szerkesztése `/etc/xrdp/startwm.sh`, használata:

    #sudo vi /etc/xrdp/startwm.sh   

Sor hozzáadása `xfce4-session` sor elé `/etc/X11/Xsession`.

Indítsa újra a xrdp szolgáltatás, használhatja:

    #sudo service xrdp restart


##<a name="connect-your-linux-vm-from-a-windows-machine"></a>Csatlakozás a Linux virtuális Windows számítógépről
Egy Windows gépen indítsa el a távoli asztali ügyfél, a Linux virtuális tartománynév bemeneti vagy Ugrás `Dashboard` a virtuális Azure klasszikus portálra, és kattintson a gép `Connect` szeretne csatlakozni a Linux virtuális, látni fogja bejelentkezési ablaka alatt:

![kép](./media/virtual-machines-linux-classic-remote-desktop/no2.png)

Jelentkezzen be a `user`  &  `password` a Linux virtuális a most készített a távoli asztal irányítását a Microsoft Azure Linux virtuális!


##<a name="next"></a>Következő
További információt a xrdp használni, akkor is hivatkozhat [Itt](http://www.xrdp.org/).
