<properties
    pageTitle="Hozzon létre SSH kulcsok Linux és Mac |} Microsoft Azure"
    description="Készítése, és Linux és Mac az erőforrás-kezelő és Azure klasszikus telepítési modell SSH billentyűk használata."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="vlivech"
    manager="timlt"
    editor=""
    tags="" />

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/25/2016"
    ms.author="v-livech"/>

# <a name="create-ssh-keys-on-linux-and-mac-for-linux-vms-in-azure"></a>SSH kulcsok létrehozása Linux és Mac Linux VMs Azure-ban

Egy SSH kulcspár a feleslegessé jelszavak való bejelentkezéshez, amely alapértelmezés szerint SSH kulcsokkal hitelesítéshez Azure virtuális gépeken futó hozhat létre.  Jelszavak is lehet kitalálhatják, és nyissa meg a VMs folyamatos ellen megkísérli a jelszó felfelé. Azure-sablon segítségével létrehozott VMs vagy a `azure-cli` is elhelyezhet a SSH nyilvános kulcs a telepítési eltávolítása a bejegyzés telepítési konfiguráció részeként.  Ha egy Linux virtuális csatlakozik a Windows, olvassa el a [Ehhez a dokumentumhoz.](virtual-machines-linux-ssh-from-windows.md)

A cikk van szükség:

- az Azure-fiók (az[első ingyenes próbaverziót](https://azure.microsoft.com/pricing/free-trial/)).

- bejelentkezett az [Azure CLI](../xplat-cli-install.md)`azure login`

- az Azure CLI _kell lennie az_ erőforrás-kezelő Azure mód`azure config mode arm`

## <a name="quick-commands"></a>Gyors parancsok

Az alábbi parancsokat cserélje le a példákat saját lehetőséggel.

SSH kulcsok alapértelmezés szerint ki legyenek tartani a `.ssh` címtár.  

```bash
cd ~/.ssh/
```

Ha nem rendelkezik egy `~/.ssh` címtár a `ssh-keygen` parancs, hozza létre a megfelelő engedélyekkel.

```bash
ssh-keygen -t rsa -b 2048 -C "myusername@myserver"
```

Adja meg a mentett fájl nevét a `~/.ssh/` címtár:

```bash
id_rsa
```

Adja meg a jelszó id_rsa:

```bash
correct horse battery staple
```

Most már van egy `id_rsa` és `id_rsa.pub` SSH kulcs átlagostól való a `~/.ssh` címtár.

```bash
ls -al ~/.ssh
```

Adja hozzá az újonnan létrehozott billentyű lenyomásával `ssh-agent` Linux és Mac (OSX kulcslánc is megjelenik):

```bash
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_rsa
```

Másolja be a SSH nyilvános kulcs Linux kiszolgálója:

```bash
ssh-copy-id -i ~/.ssh/id_rsa.pub myusername@myserver
```

Tesztelje a bejelentkezési használata billentyűk jelszó helyett:

```bash
ssh -o PreferredAuthentications=publickey -o PubkeyAuthentication=yes -i ~/.ssh/id_rsa myusername@myserver
Last login: Tue April 12 07:07:09 2016 from 66.215.22.201
$
```

## <a name="detailed-walkthrough"></a>Részletes útmutató

Nyilvános és titkos kulcsok SSH használata a legkönnyebben úgy, hogy jelentkezzen be az Linux-kiszolgálókon. [Nyilvános kulcsú](https://en.wikipedia.org/wiki/Public-key_cryptography) biztosítja sokkal több biztonságos bejelentkezés mint jelszavak, amely lehet találgatásos kényszerített sokkal egyszerűen BSD virtuális Azure-ban vagy Linux rendszerhez. A nyilvános kulcs megosztható bárkivel; de csak Ön (vagy a helyi biztonsági infrastruktúra) kell rendelkeznie a titkos kulcs.  A SSH titkos kulcs van, hogy [nagyon biztonságos jelszó](https://www.xkcd.com/936/) (forrás:[xkcd.com](https://xkcd.com)) megóvása azt.  Erre a jelszóra csak eléréséhez a személyes SSH kulcsot, és **nem** a felhasználói fiók jelszava.  Amikor felvesz egy jelszót a SSH használatával, hogy titkosítja a titkos kulcs úgy, hogy a titkos kulcs használhatatlan oldhatja fel a jelszó nélkül.  Ha a támadók ellopott a titkos kulcs és billentyűre nem volt egy jelszót, azok lenne használhatja az adott titkos kulcs azokat a kiszolgálókat, amely rendelkezik a megfelelő nyilvános kulccsal bejelentkezhetne.  Ha egy titkos kulcs a jelszóval védett azt, hogy támadó biztonsági egy további rétege a infrastruktúrára vonatkozó Azure kezeléséről által nem használhatók.

Ez a cikk *ssh-rsa* formázott kulcs fájlokat, amelyeket javasoltak telepítések az erőforrás-kezelő a hoz létre.  a [portál](https://portal.azure.com) klasszikus, mind az erőforrás-kezelő telepítésekhez *ssh-rsa* kulcsok van szükség.

## <a name="create-the-ssh-keys"></a>A SSH kulcsok létrehozása

Azure igényel legalább 2048 bites ssh-rsa nyilvános és titkos kulcsok formázhatja. Kulcsok használatának létrehozása `ssh-keygen`, amelyek kérdések sorozata kéri, és írja a egy titkos kulcs és a hozzá tartozó nyilvános kulcs. Az Azure virtuális létrehozásakor a nyilvános kulcshoz másolandó `~/.ssh/authorized_keys`.  SSH hívóbetűi `~/.ssh/authorized_keys` az ügyfél, a megfelelő bejelentkezési SSH kapcsolaton megfelelő titkos kulcs arra használják.

## <a name="using-ssh-keygen"></a>Ssh-keygen használatával

Ez a parancs a jelszóval védett (titkosított) SSH kulcspár 2048 bites RSA használatával hoz létre, és segítségével egyszerűen azonosíthatók megjegyzéssel van ellátva.  

Kezdeni, könyvtárak, módosíthatja, hogy az összes a ssh kulcsok létrehozásakor a könyvtárra.

```bash
cd ~/.ssh
```

Ha nem rendelkezik egy `~/.ssh` címtár a `ssh-keygen` parancs, hozza létre a megfelelő engedélyekkel.

```bash
ssh-keygen -t rsa -b 2048 -C "myusername@myserver"
```

_Magyarázata parancs_

`ssh-keygen`a program a billentyűk létrehozásához használt =

`-t rsa`= típusú kulcsot hozhat létre, amely a [RSA formátum](https://en.wikipedia.org/wiki/RSA_(cryptosystem)

`-b 2048`a kulcs olyan =

`-C "myusername@myserver"`a nyilvános-fájl segítségével egyszerűen azonosíthatók vége fűzött megjegyzés =.  A szokásos módon e-mailben szolgál a megjegyzést, de függetlenül működik a legjobban a infrastruktúra is használhatja.

### <a name="using-pem-keys"></a>PEM billentyűk segítségével

Ha használja a klasszikus üzembe modell (Azure klasszikus portál vagy az Azure szolgáltatás felügyeleti CLI `asm`), PEM használni kell formázott SSH kulcsok a Linux VMs eléréséhez.  A következőképpen PEM kulcs létrehozása egy meglévő SSH nyilvános kulcs és egy meglévő x509 tanúsítvány.

Hozzon létre egy PEM formázott SSH meglévő nyilvános kulcs kulcs:

```bash
ssh-keygen -f ~/.ssh/id_rsa.pub -e > ~/.ssh/id_ssh2.pem
```

## <a name="example-of-ssh-keygen"></a>Példa a ssh-keygen

```bash
ssh-keygen -t rsa -b 2048 -C "myusername@myserver"
Generating public/private rsa key pair.
Enter file in which to save the key (/home/myusername/.ssh/id_rsa): id_rsa
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in id_rsa.
Your public key has been saved in id_rsa.pub.
The key fingerprint is:
14:a3:cb:3e:78:ad:25:cc:55:e9:0c:08:e5:d1:a9:08 myusername@myserver
The key's randomart image is:
+--[ RSA 2048]----+
|        o o. .   |
|      E. = .o    |
|      ..o...     |
|     . o....     |
|      o S =      |
|     . + O       |
|      + = =      |
|       o +       |
|        .        |
+-----------------+
```

Mentett kulcs fájlok:

`Enter file in which to save the key (/home/myusername/.ssh/id_rsa): id_rsa`

Ez a cikk a fő pár nevét.  Két fő **id_rsa** nevű, akkor az alapértelmezett, és néhány eszköz gondolná **id_rsa** titkos kulcs fájlnevét, így több jó ötlet. A könyvtár `~/.ssh/` SSH kulcs párban és a SSH konfigurációs fájl alapértelmezett helye.

```bash
ls -al ~/.ssh
-rw------- 1 myusername staff  1675 Aug 25 18:04 id_rsa
-rw-r--r-- 1 myusername staff   410 Aug 25 18:04 rsa.pub
```
Listája a `~/.ssh` címtár. `ssh-keygen`hoz létre a `~/.ssh` címtár lehetőséget, ha nem szerepel, és a is állítja be a megfelelő tulajdonjogot és a fájl megnyitása különböző módokban.

Kulcs jelszó:

`Enter passphrase (empty for no passphrase):`

`ssh-keygen`jelszó utal, mint "a hozzáférési kódot."  *Erősen* javasolt a jelszóval védheti a főbb párokká. Anélkül, hogy a kulcsfontosságú pár védelme jelszóval a titkos kulcs fájlt bárki vele bármely kiszolgáló, amely rendelkezik a megfelelő nyilvános kulccsal bejelentkezni. Idő módosítása a billentyűk használt hozzáadása a jelszó nagyobb védelmet nyújt, abban az esetben, ha valaki éppen tud hozzáférni a titkos kulcs fájl meghatalmazta hitelesítést végezni, akkor.

## <a name="using-ssh-agent-to-store-your-private-key-password"></a>A titkos kulcs jelszava tárolásához ssh-ügynök használatával

A titkos kulcs fájl jelszót minden SSH bejelentkezési gépelési elkerülése érdekében használható `ssh-agent` gyorsítótárban titkos kulcs fájl jelszavát. Ha Mac használata esetén a OSX Kulcsláncból biztonságosan tárolja a titkos kulcs jelszavakat, meghívásakor `ssh-agent`.

Először győződjön meg arról, hogy `ssh-agent` fut

```bash
eval "$(ssh-agent -s)"
```

Most pedig adja hozzá a titkos kulcs `ssh-agent` paranccsal `ssh-add`.

```bash
ssh-add ~/.ssh/id_rsa
```

A titkos kulcs jelszava most tárolt `ssh-agent`.

## <a name="create-and-configure-an-ssh-config-file"></a>Létrehozása és konfigurálása az SSH konfigurációs fájl

Ajánlott létrehozása és konfigurálása a legjobb módszer egy `~/.ssh/config` gyorsításához fájl bejelentkezések és optimalizálása a SSH ügyfél tevékenysége számára.

A következő példa bemutatja a szabványos konfiguráció.

### <a name="create-the-file"></a>A fájl létrehozása

```bash
touch ~/.ssh/config
```

### <a name="edit-the-file-to-add-the-new-ssh-configuration"></a>Szerkessze a fájlt az új SSH konfiguráció hozzáadása:

```bash
vim ~/.ssh/config
```

### <a name="example-sshconfig-file"></a>Példa `~/.ssh/config` fájl:

```bash
# Azure Keys
Host fedora22
  Hostname 102.160.203.241
  User myusername
# ./Azure Keys
# Default Settings
Host *
  PubkeyAuthentication=yes
  IdentitiesOnly=yes
  ServerAliveInterval=60
  ServerAliveCountMax=30
  ControlMaster auto
  ControlPath ~/.ssh/SSHConnections/ssh-%r@%h:%p
  ControlPersist 4h
  IdentityFile ~/.ssh/id_rsa
```

A SSH config lépve szakaszok minden kiszolgáló, amely lehetővé teszi, hogy a saját dedikált kulcs pár. Az alapértelmezett beállítások (`Host *`) vannak, amelyek nem felelnek meg a konfigurációs fájl feljebb adott hosts állomások.


### <a name="config-file-explained"></a>Konfigurációs fájl magyarázata

`Host`= a hívott meg a terminált állomás nevét.  `ssh fedora22`Ez az információ `SSH` értékeket szeretné használni a beállítások blokk címkézett `Host fedora22` Megjegyzés: Ez lehet, hogy minden olyan felirat, amely a használati logikai, és nem jelent meg bármely kiszolgáló tényleges hostname (állomásnév).

`Hostname 102.160.203.241`= az IP-cím vagy a DNS-használatban a kiszolgáló nevét.

`User myusername`a távoli felhasználói fiók használatához a kiszolgálóra történő bejelentkezéskor =.

`PubKeyAuthentication yes`= Jelentkezzen be egy SSH billentyűvel szeretne SSH alapján.

`IdentityFile /home/myusername/.ssh/id_id_rsa`a titkos kulcs SSH és a hitelesítéshez használt megfelelő nyilvános kulccsal =.


## <a name="ssh-into-linux-without-a-password"></a>A jelszó nélküli Linux SSH

Most, hogy van egy SSH kulcs pár és a beállított SSH konfigurációs fájl, akkor is tudják jelentkezzen be a Linux virtuális egyszerűen és biztonságos módon. Az első alkalommal bejelentkezik az olyan kiszolgáló, amely egy SSH kulcsot a parancsot a képernyőn megjelenő utasításokat, az adott kulcs fájl a jelszó.

```bash
ssh fedora22
```

### <a name="command-explained"></a>Magyarázata parancs

Ha `ssh fedora22` megy végbe SSH először a megkeresése, és betölti azt az beállításaitól a `Host fedora22` letiltása, majd kattintson a terhelést a hátralévő beállításokat az utolsó tiltása a `Host *`.

## <a name="next-steps"></a>Következő lépések

Következő fel az új SSH nyilvános kulccsal Azure Linux VMs létrehozására.  Azure VMs, mint a bejelentkezési SSH nyilvános kulccsal létrehozott olyan jobban biztonságos, mint az alapértelmezett bejelentkezési módszer, a jelszavak készült VMs.  A SSH kulcsok készült Azure VMs alapértelmezés szerint ki legyenek le van tiltva, a jelszavak konfigurált rendszerének kísérletek találgatásos kényszerített elkerülése.

- [Hozzon létre egy biztonságos Linux virtuális Azure-sablon használatával](virtual-machines-linux-create-ssh-secured-vm-from-template.md)
- [Hozzon létre egy biztonságos Linux virtuális az Azure portál használatával](virtual-machines-linux-quick-create-portal.md)
- [Hozzon létre egy biztonságos Linux virtuális az Azure CLI használatával](virtual-machines-linux-quick-create-cli.md)
