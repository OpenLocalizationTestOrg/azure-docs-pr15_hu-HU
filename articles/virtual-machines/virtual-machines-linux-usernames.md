<properties 
    pageTitle="Kijelölés Linux nevével |} Microsoft Azure" 
    description="Megtudhatja, hogy miként jelölje ki a felhasználóneveket Linux virtuális géphez Azure-ban." 
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



#<a name="selecting-user-names-for-linux-on-azure"></a>Jelölje ki a Azure Linux nevével#

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

Akkor kiépítése a Azure virtuális Linux gép kell megadnia, amelyeket később használhat jelentkezzen be a virtuális nem legfelső szintű felhasználó nevére. Kiválaszthatja, hogy az új felhasználó nevét, vagy ha az Azure klasszikus portálon keresztül kiépítési elfogadhatja az alapértelmezett neve "azureuser".

A legtöbb esetben a felhasználó nem létezik a viszonyítási képre, és a kiépítési folyamat során jön létre. Ha a felhasználó létezik a viszonyítási virtuális képre, majd az Azure Linux ügynök egyszerűen konfigurálja a jelszó és/vagy a felhasználó számára a virtuális létrehozásakor megadott adatok alapján SSH billentyűt.

Linux **azonban**nem lehet használni felhasználóneveket halmazának határozza meg. A kiépítési folyamat fogja, hogy **sikertelen** , ha egy meglévő rendszer-felhasználó, amely 0 – 99 UID rendelkező felhasználóként van meghatározva használatával Linux virtuális kiépítése próbál. Egy tipikus példája a `root` felhasználó, aki rendelkezik UID 0.

 - Lásd még: [csak jelszóval módosítható tartományok Linux Standard alap - felhasználói azonosító](http://refspecs.linuxfoundation.org/LSB_4.1.0/LSB-Core-generic/LSB-Core-generic/uidrange.html)

A Felhasználólista közös beépített CentOS és Ubuntu, hogy lehetőleg ne használja a Azure virtuális Linux gép kiépítésekor a következő: Ez a lista példája csak, olvassa el a terjesztési annak érdekében, hogy a felhasználónév, kiválaszthatja, hogy nem ütközik rendszer meglévő felhasználó dokumentációját.


## <a name="centos"></a>CentOS
- abrt
- ADM
- hang
- mappa
- CD-ROM
- cgred
- démon
- dbus
- Kitárcsázás
- DIP
- merevlemez
- hajlékonylemezen
- FTP
- FTP
- játék
- Gopher
- haldaemon
- Megállítás
- kmem
- zárolása
- LP
- levelek
- egy férfi
- mem
- nfsnobody
- senki sem
- NTP
- műveleti jel
- oprofile
- postdrop
- utólagos javítás
- qpidd
- legfelső szintű
- távoli eljáráshívás
- rpcuser
- saslauth
- Leállítás
- slocate
- sshd
- stapdev
- stapusr
- szinkronizálás
- sys
- szalag
- teszt
- tcpdump
- TTY
- felhasználók
- utempter
- utmp
- uucp
- vcsa
- a videó
- ikon


## <a name="ubuntu"></a>Ubuntu
- ADM
- rendszergazda
- hang
- biztonsági másolat
- mappa
- CD-ROM
- crontab
- démon
- Kitárcsázás
- DIP
- merevlemez
- Faxszolgáltatás
- hajlékonylemezen
- biztosító
- játék
- gnats
- IRC
- kmem
- fekvő
- libuuid
- lista
- LP
- levelek
- egy férfi
- MessageBus
- mlocate
- netdev
- hírek
- senki sem
- nogroup
- műveleti jel
- plugdev
- proxykiszolgáló
- legfelső szintű
- SASL
- Árnyék
- forrás
- ssh
- sshd
- az oktatói
- sudo
- szinkronizálás
- sys
- Syslog
- szalag
- TTY
- felhasználók
- utmp
- uucp
- a videó
- Hangposta
- whoopsie
- webszolgáltatás-adatok

 