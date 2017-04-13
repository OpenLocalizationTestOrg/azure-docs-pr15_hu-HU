<properties 
    pageTitle="Használja a legfelső szintű jogosultságokkal Linux virtuális gépeken |} Microsoft Azure" 
    description="Megtudhatja, hogy miként használhatja a legfelső szintű jogosultságokkal Azure-ban Linux virtuális gépen." 
    services="virtual-machines-linux" 
    documentationCenter="" 
    authors="szarkos" 
    manager="timlt" 
    editor=""
    tags="azure-service-management,azure-resource-manager" />

<tags 
    ms.service="virtual-machines-linux" 
    ms.workload="infrastructure-services" 
    ms.tgt_pltfrm="vm-linux" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/17/2016" 
    ms.author="szark"/>


# <a name="using-root-privileges-on-linux-virtual-machines-in-azure"></a>Legfelső szintű jogosultságokkal használata Linux virtuális gépeken futó Azure-ban

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

Alapértelmezés szerint a `root` felhasználói le van tiltva, Linux virtuális gépeken Azure-ban. Felhasználók futtatását is lehetővé teszi parancsok magasabb szintű jogosultságokkal rendelkező használatával a `sudo` parancsot. Azonban a felület attól is függhet, hogy a rendszer kiépített.

1. **SSH billentyűt, és a jelszó vagy csak** – a virtuális gép egy tanúsítvánnyal kiépített (`.CER` fájl) vagy SSH kulcsot, valamint a jelszavát, vagy csak egy felhasználónevet és jelszót. Ebben az esetben `sudo` a parancs végrehajtása előtt a felhasználók jelszava kérni fogja.

2. **Csak a SSH kulcs** – a virtuális gép kiépített tanúsítvánnyal (`.cer`, `.pem`, vagy `.pub` fájl) vagy SSH kulcs, de nem jelszót.  Ebben az esetben `sudo` figyelmeztető üzenet **nem lesznek** a felhasználók jelszava a parancs végrehajtása előtt.


## <a name="ssh-key-and-password-or-password-only"></a>SSH billentyűt, és a jelszó vagy a jelszó csak

Jelentkezzen be a Linux virtuális gép SSH billentyűt vagy a jelszó-hitelesítést használ, majd futtassa a parancsokat a `sudo`, például:

    # sudo <command>
    [sudo] password for azureuser:

Ebben az esetben a felhasználó kérni fogja a jelszót. A jelszó beírása után `sudo` futtatható a parancsok `root` jogosultságokkal.

Passwordless sudo szerkesztésével is engedélyezheti a `/etc/sudoers.d/waagent` fájlban, például:

    #/etc/sudoers.d/waagent
    azureuser ALL = (ALL) NOPASSWD: ALL

Ez a változás lehetővé teszi a felhasználó "azureuser" passwordless sudo.

## <a name="ssh-key-only"></a>SSH csak kulcs

Jelentkezzen be a Linux virtuális gép SSH fő hitelesítés használatával, majd futtassa a parancsokat a `sudo`, például:

    # sudo <command>

Ebben az esetben a felhasználó program **nem** kéri a jelszót. Után megnyomásával `<enter>`, `sudo` futtatható a parancsok `root` jogosultságokkal.

 
