<properties
   pageTitle="A Azure FreeBSD – bevezetés |} Microsoft Azure"
   description="Azure FreeBSD virtuális gépeken futó használatával kapcsolatban további tudnivalók"
   services="virtual-machines-linux"
   documentationCenter=""
   authors="KylieLiang"
   manager="timlt"
   editor=""
   tags="azure-service-management"/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure-services"
   ms.date="08/27/2016"
   ms.author="kyliel"/>

# <a name="introduction-to-freebsd-on-azure"></a>A Azure FreeBSD – bevezetés
Ez a témakör áttekintést nyújt a FreeBSD virtuális gépen futó Azure-ban.

## <a name="overview"></a>– Áttekintés
A Microsoft Azure FreeBSD pedig egy speciális számítógépen futó operációs rendszer használt power modern kiszolgálók, asztali gépre, beágyazott platformokon. A FreeBSD 10.3 kép Microsoft Corporation által biztosított, és az Azure érhető el. A FreeBSD 10.3 megjelenés, és telepítve van a [2.1.4](https://github.com/Azure/WALinuxAgent/releases/tag/v2.1.4) Azure virtuális Vendég ügynök alapul. Az ügynök nem felelős a FreeBSD virtuális és a műveletek, például a virtuális (felhasználónevet, jelszót, a host name, stb.) az első használat kiépítési és szelektív virtuális Extensions funkció engedélyezése az Azure háló közötti kommunikációt.
FreeBSD jövőbeli verzióiban, mint a stratégia, hogy mindig naprakész elérhetővé teheti az Újdonságok azokat a FreeBSD megjelenés gyártástechnológiai csapatának elengedi után hamarosan. Következő [FreeBSD 11](https://www.freebsd.org/releases/11.0R/schedule.html).

## <a name="deploying-a-freebsd-virtual-machine"></a>Virtuális FreeBSD gép üzembe helyezése
A [Microsoft Azure piactéren](https://azure.microsoft.com/marketplace/partners/microsoft/freebsd103/)lévő kép használata egy egyszerű folyamatábra üzembe helyezése a FreeBSD virtuális gép.

## <a name="vm-extensions-for-freebsd"></a>Virtuális kiterjesztéseinek FreeBSD
Az alábbiakban a FreeBSD támogatott virtuális bővítményeket.

### <a name="vmaccess"></a>VMAccess

A [VMAccess](https://github.com/Azure/azure-linux-extensions/tree/master/VMAccess) kiterjesztést is:

- Az eredeti sudo felhasználó jelszavának alaphelyzetbe állítása.
- Hozzon létre egy új sudo felhasználó a megadott jelszót.
- A megadott kulccsal host nyilvános kulcs beállítása.
- Állítsa alaphelyzetbe a nyilvános host kulcs során virtuális kiépítési, ha nem adja meg a host billentyűt.
- Nyissa meg a SSH port (22), és a sshd_config vissza, ha a reset_ssh értéke igaz.
- Távolítsa el a meglévő felhasználót.
- Jelölje be a lemezt.
- Javítsa ki a hozzáadott lemez.

### <a name="customscript"></a>CustomScript

A [CustomScript](https://github.com/Azure/azure-linux-extensions/tree/master/CustomScript) kiterjesztést is:

- Ha megadták, töltse le a testre szabott parancsfájlok Azure-tárhely és a külső nyilvános tárhelyek (például GitHub).
- Futtassa a bejegyzés pontot.
- Támogatja a beágyazott parancsokat.
- Windows-stílus újsor rendszerhéj és Python parancsfájlok automatikusan konvertálja.
- Távolítsa el AJ burkolatok és Python parancsfájlok automatikusan.
- CommandToExecute kényes adatok védelméhez.

## <a name="authentication-user-names-passwords-and-ssh-keys"></a>Hitelesítés: felhasználóneveket, a jelszavakat és SSH kulcsok
Virtuális FreeBSD gép létrehozásakor a Azure portál használatával meg kell adnia felhasználónevét, a jelszó vagy a nyilvános kulcshoz SSH.
Nevével üzembe helyezése a Azure virtuális FreeBSD gép nem egyeznie kell a system fiók nevével (UID < 100) már szerepel a virtuális gép ("root," példa).
Jelenleg csak a RSA SSH kulcs használata támogatott. Egy többsoros SSH billentyűt kell kezdődnie "---KEZDŐ SSH2 nyilvános kulcs---" végén pedig használja "---END SSH2 nyilvános kulcs---".

## <a name="obtaining-superuser-privileges"></a>Rendszeradminisztrátori jogosultságokkal beszerzése
A felhasználó a Azure virtuális gép példány a telepítés során megadott egy olyan jogosultsággal rendelkező fiók. A csomag sudo a közzétett FreeBSD képe lett telepítve.
Ezen a fiókon keresztül van bejelentkezve, miután a parancs szintaxissal parancsai futtathatók legfelső szintű.

    # sudo <COMMAND>

Másik lehetőségként szerezhet be a legfelső szintű rendszerhéj sudo -s használatával.

## <a name="next-steps"></a>Következő lépések
- Nyissa meg a FreeBSD virtuális [Azure piactérről](https://azure.microsoft.com/marketplace/partners/microsoft/freebsd103/) .
- Ha szeretné saját FreeBSD előtérbe Azure, olvassa el az [létrehozása és feltöltése az Azure virtuális FreeBSD merevlemez](../virtual-machines-linux-classic-freebsd-create-upload-vhd.md).
