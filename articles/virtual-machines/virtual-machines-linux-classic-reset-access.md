<properties
        pageTitle="Linux virtuális jelszavának alaphelyzetbe és a CLI SSH kulcs |} Microsoft Azure"
        description="VMAccess kiterjesztése a az Azure parancssori felület (CLI) használatáról virtuális Linux jelszóval vagy SSH kulcs alaphelyzetbe, a SSH konfiguráció javításához és lemez konzisztencia ellenőrzése"
        services="virtual-machines-linux"
        documentationCenter=""
        authors="cynthn"
        manager="timlt"
        editor=""
        tags="azure-service-management"/>

<tags
        ms.service="virtual-machines-linux"
        ms.workload="infrastructure-services"
        ms.tgt_pltfrm="vm-linux"
        ms.devlang="na"
        ms.topic="article"
        ms.date="06/14/2016"
        ms.author="cynthn"/>

# <a name="how-to-reset-a-linux-vm-password-or-ssh-key-fix-the-ssh-configuration-and-check-disk-consistency-using-the-vmaccess-extension"></a>Alaphelyzetbe Linux virtuális jelszóval vagy SSH billentyűt, a SSH konfiguráció javításához és VMAccess bővítményében lemezre konzisztencia ellenőrzése


Ha nem tud csatlakozni a Azure virtuális Linux gép miatt egy elfelejtett jelszót Secure rendszerhéj (SSH) nem megfelelő billentyűt, vagy a SSH konfigurációs probléma használata a VMAccessForLinux bővítmény az Azure CLI alaphelyzetbe a jelszó vagy a SSH billentyűt, a SSH konfiguráció javításához, és a lemez konzisztencia ellenőrzése. 

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Megtudhatja, miként [végezheti el ezeket a lépéseket az erőforrás-kezelő modellt használja](https://github.com/Azure/azure-linux-extensions/tree/master/VMAccess).

Az Azure CLI a használatával az **azure virtuális bővítmény beállítása** parancsot a parancssor (Bash, Terminálszolgáltatások, parancssor) az access-parancsokat. Futtassa a **Súgó azure virtuális bővítmény beállítása** a részletes bővítmény használatát.

Az Azure CLI végezheti el az alábbi műveleteket:

+ [A jelszó alaphelyzetbe állítása](#pwresetcli)
+ [Állítsa alaphelyzetbe a SSH billentyűt](#sshkeyresetcli)
+ [Állítsa alaphelyzetbe a jelszó és a SSH billentyűt](#resetbothcli)
+ [Új sudo felhasználói fiók létrehozása](#createnewsudocli)
+ [A SSH konfiguráció visszaállítása](#sshconfigresetcli)
+ [Felhasználó törlése](#deletecli)
+ [A VMAccess bővítmény állapotának megjelenítése](#statuscli)
+ [A felvett lemezre konzisztencia ellenőrzése](#checkdisk)
+ [A Linux virtuális a hozzáadott lemez javítása](#repairdisk)


## <a name="prerequisites"></a>Előfeltételek

Meg kell tegye a következőket:

- [Telepítse az Azure CLI](../xplat-cli-install.md) , és [az előfizetés való kapcsolódáshoz](../xplat-cli-connect.md) kell a fiókkal társított Azure források.
- A klasszikus telepítési modell megfelelő módjának beállítása a parancssorba írja be a a következőket:
        
        azure config mode asm
        
- Van egy új jelszót vagy SSH hívóbetűk, ha bármelyik alaphelyzetbe. Ha a SSH konfiguráció alaphelyzetbe, ezek nem szükséges.


## <a name="pwresetcli"></a>A jelszó alaphelyzetbe állítása

1. Ezek a sorok PrivateConf.json nevű fájl létrehozása. A szögletes zárójelek csere és a & #60; helyőrző & #62. a saját adatokkal értékeket.

        {
        "username":"<currentusername>",
        "password":"<newpassword>",
        "expiration":"<2016-01-01>"
        }

2. Futtassa a parancsot a virtuális gép #60; virtuális neve & #62; nevét.

        azure vm extension set <vm-name> VMAccessForLinux Microsoft.OSTCExtensions 1.* –-private-config-path PrivateConf.json

## <a name="sshkeyresetcli"></a>Állítsa alaphelyzetbe a SSH billentyűt

1. Hozzon létre egy fájlt, PrivateConf.json nevű tartalmát. A szögletes zárójelek csere és a & #60; helyőrző & #62. a saját adatokkal értékeket.

        {
        "username":"<currentusername>",
        "ssh_key":"<contentofsshkey>"
        }

2. Futtassa a parancsot a virtuális gép #60; virtuális neve & #62; nevét.

        azure vm extension set <vm-name> VMAccessForLinux Microsoft.OSTCExtensions 1.* --private-config-path PrivateConf.json

## <a name="resetbothcli"></a>Állítsa alaphelyzetbe a jelszó és a SSH billentyűt

1. Hozzon létre egy fájlt, PrivateConf.json nevű tartalmát. A szögletes zárójelek csere és a & #60; helyőrző & #62. a saját adatokkal értékeket.

        {
        "username":"<currentusername>",
        "ssh_key":"<contentofsshkey>",
        "password":"<newpassword>"
        }

2. Futtassa a parancsot a virtuális gép #60; virtuális neve & #62; nevét.

        azure vm extension set <vm-name> VMAccessForLinux Microsoft.OSTCExtensions 1.* --private-config-path PrivateConf.json

## <a name="createnewsudocli"></a>Új sudo felhasználói fiók létrehozása

Ha elfelejtette a felhasználónevét, VMAccess hozzon létre egy újat a sudo jogosultságokat is használhatja. Ebben az esetben a meglévő felhasználónevet és jelszót nem módosulnak.

Jelszó hozzáféréssel rendelkező új felhasználó sudo létrehozásához használja a parancsfájl [a jelszó alaphelyzetbe állítása](#pwresetcli) , és adja meg az új felhasználó nevét.

SSH kulcs hozzáféréssel rendelkező új felhasználó sudo létrehozásához használja a parancsfájl [alaphelyzetbe állítása az SSH billentyűt](#sshkeyresetcli) , és adja meg az új felhasználó nevét.

Akkor is használhatja [alaphelyzetbe a jelszó és a SSH kulcs](#resetbothcli) hozhat létre új felhasználót a jelszó és a fő access SSH.

## <a name="sshconfigresetcli"></a>A SSH konfiguráció visszaállítása

Ha a SSH konfiguráció nemkívánatos állapotban van, is előfordulhat, hogy elveszíti hozzáférését a virtuális. A VMAccess bővítmény segítségével a konfiguráció visszaállítása az alapértelmezett állapotába. Ha igen, csak kell beállítania "Igaz" a "reset_ssh" billentyűt. A bővítmény indítsa újra a SSH kiszolgálót, nyissa meg a virtuális SSH portjához, és a SSH konfiguráció visszaállítása az alapértelmezett értékekre. A felhasználói fiók (nevét, jelszavát és SSH kulcsokat) nem lehet módosítani.

> [AZURE.NOTE] A SSH konfigurációs fájl, amely kap alaphelyzetbe /etc/ssh/sshd_config helyezkedik el.

1. A tartalom PrivateConf.json nevű fájl létrehozása.

        {
        "reset_ssh":"True"
        }

2. Futtassa a parancsot a virtuális gép #60; virtuális neve & #62; nevét. 

        azure vm extension set <vm-name> VMAccessForLinux Microsoft.OSTCExtensions 1.* --private-config-path PrivateConf.json

## <a name="deletecli"></a>Felhasználó törlése

Ha egy felhasználói fiókot a naplózás be a virtuális közvetlenül nélkül törölni szeretné a parancsfájl is használhatja.

1. A tartalom, a felhasználó nevét, ha el szeretné távolítani a #60; usernametoremove & #62; helyettesítése PrivateConf.json nevű fájl létrehozása. 

        {
        "remove_user":"<usernametoremove>"
        }

2. Futtassa a parancsot a virtuális gép #60; virtuális neve & #62; nevét. 

        azure vm extension set <vm-name> VMAccessForLinux Microsoft.OSTCExtensions 1.* --private-config-path PrivateConf.json

## <a name="statuscli"></a>A VMAccess bővítmény állapotának megjelenítése

Szeretné megjeleníteni a VMAccess bővítmény állapotát, a művelet végrehajtása

        azure vm extension get

## <a name="a-namecheckdiskacheck-consistency-of-added-disks"></a>< a név = "próbálkozik" <</a>hozzáadott lemezre konzisztencia ellenőrzése

A Linux virtuális gépen összes lemezen fsck futtatásához szüksége lesz tegye a következőket:

1. A tartalom PublicConf.json nevű fájl létrehozása. Jelölje be a lemez logikai érték-e ellenőrzést a lemezt a virtuális gép csatolva, vagy nem vesz igénybe. 

        {   
        "check_disk": "true"
        }

2. Futtassa a végrehajtás, ezzel a paranccsal a virtuális gép #60; virtuális neve & #62; nevét.

        azure vm extension set <vm-name> VMAccessForLinux Microsoft.OSTCExtensions 1.* --public-config-path PublicConf.json 

## <a name='repairdisk'></a>Javítsa ki a lemezen 

Lemezen csatlakoztatása nem tartalmazó vagy csatlakoztatási konfigurációs hibák javítása, a Linux virtuális gépen csatlakoztatási konfigurációjának visszaállítása a VMAccess bővítmény használatával. A nevét a lemez #60; yourdisk & #62; helyettesítése.

1. A tartalom PublicConf.json nevű fájl létrehozása. 

        {
        "repair_disk":"true",
        "disk_name":"<yourdisk>"
        }

2. Futtassa a végrehajtás, ezzel a paranccsal a virtuális gép #60; virtuális neve & #62; nevét.

        azure vm extension set <vm-name> VMAccessForLinux Microsoft.OSTCExtensions 1.* --public-config-path PublicConf.json



## <a name="next-steps"></a>Következő lépések

* Azure PowerShell-parancsmagok vagy az erőforrás-kezelő Azure sablonok használatával állítsa alaphelyzetbe a jelszó vagy a SSH kulcs szeretné, ha a SSH konfiguráció javításához és lemez konzisztencia ellenőrzése, [GitHub VMAccess bővítmény dokumentációt](https://github.com/Azure/azure-linux-extensions/tree/master/VMAccess). 

* A jelszó alaphelyzetbe állítása az [Azure portálon](https://portal.azure.com) is használhatja, vagy egy Linux virtuális SSH kulcsa a klasszikus telepítési modell rendszerbe. Jelenleg nem használható a portál ne Ez egy Linux virtuális rendszerbe az erőforrás-kezelő telepítési modell.

* Lásd: [virtuális gép extensions és-szolgáltatásokról](virtual-machines-linux-extensions-features.md) az Azure virtuális gépeken futó a virtuális bővítmények használatáról további információt.
