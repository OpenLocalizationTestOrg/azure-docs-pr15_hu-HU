<properties
    pageTitle="Tiltsa le a Linux virtuális SSH jelszavak SSHD konfigurálásával |} Microsoft Azure"
    description="Az Azure virtuális Linux gép biztonságos jelszó bejelentkezések az SSH letiltásával."
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
    ms.topic="article"
    ms.date="08/26/2016"
    ms.author="v-livech"/>

# <a name="disable-ssh-passwords-on-your-linux-vm-by-configuring-sshd"></a>Tiltsa le a Linux virtuális SSH jelszavak SSHD konfigurálásával

Ez a cikk ismerteti a Linux virtuális bejelentkezési biztonsága zárolásához szolgáltatásaival.  Amint a SSH port 22 próbál bejelentkezni, a jelszavak próbálgatással nemzetközi botok kezdő nyitja meg.  Tiltsa le a jelszó bejelentkezések keresztüli SSH azt fogja mire ebben a cikkben.  Teljesen eltávolítja a jelszavak használata azt megóvni a Linux virtuális ilyen típusú támadások ellen.  Előnye, akkor Linux SSHD csak engedélyezni bejelentkezések SSH nyilvános és titkos kulcsok keresztül jelentkezzen be a Linux szerint, amennyiben a legbiztonságosabb úgy konfigurálja.  A lehetséges kombinációinak kitalálhatják a titkos kulcs belevágni, és ezért megelőzi botok páros bothering ellen SSH kulcsok megpróbálja a megosztott lenne szükség.


## <a name="goals"></a>Célok

- Állítsa be a SSHD tiltotta le:
  - Jelszó bejelentkezések
  - Legfelső szintű felhasználói bejelentkezés
  - Kérdés-válasz hitelesítés
- Állítsa be a SSHD engedélyezése:
  - csak a SSH kulcs bejelentkezések
- Indítsa újra a SSHD bejelentkezve továbbra is
- Az új SSHD beállítások tesztelése

## <a name="introduction"></a>– Bevezetés

[Definiált SSH](https://en.wikipedia.org/wiki/Secure_Shell)

SSHD az a SSH kiszolgáló, amely a Linux virtuális fut.  SSH egy rendszerhéj a MacBook vagy Linux számítógépen futó ügyfél.  SSH akkor is használt biztonságos és munkaállomástól és a Linux virtuális közötti kommunikáció titkosításához protokoll.

A nagyon fontos, hogy ez a cikk a Linux virtuális egy bejelentkezési nyissa meg a teljes részletes.  Emiatt azt nyílik két terminálok és SSH a Linux virtuális két őket.  Az első Terminálszolgáltatások SSHDs konfigurációs fájl végezze el a módosításokat, és indítsa újra a SSHD szolgáltatást fogjuk használni.  A második terminált tesztelje a módosításokat, a szolgáltatás újraindítása után fogjuk használni.  Mivel azt SSH jelszavak tiltja le, és használna szigorúan SSH kulcsok, ha a SSH kulcsok nem helyesek, és bezárja a virtuális a kapcsolatot, a virtuális véglegesen zárolt, és senki sem tudják azt, hogy törli, és az ismételt megadásukra igénylő bejelentkezési.

## <a name="prerequisites"></a>Előfeltételek

- [SSH kulcsok létrehozása Linux és Mac Linux VMs Azure-ban](virtual-machines-linux-mac-create-ssh-keys.md)
- Azure-fiók
  - [ingyenes próbaverzió azonosító létrehozása](https://azure.microsoft.com/pricing/free-trial/)
  - [Azure portál](http://portal.azure.com)
- Azure futó Linux virtuális
- SSH nyilvános és titkos kulcs átlagostól való`~/.ssh/`
- SSH nyilvános kulcs a `~/.ssh/authorized_keys` a Linux virtuális a
- A virtuális Sudo jogosultságok
- A port nyitva 22

## <a name="quick-commands"></a>Gyors parancsok

_Tapasztalt projektvezetőknek szánt tanácsok Linux-rendszergazdáknak, akik csak a TLDR verzióját innen kell kiindulni.  Az összes résztvevője, amely csevegést a részletes magyarázatot és részletes átugorja ezt a szakaszt._

```
username@macbook$ sudo vim /etc/ssh/sshd_config

# Change PasswordAuthentication to this:
PasswordAuthentication no

# Change PubkeyAuthentication to this:
PubkeyAuthentication yes

# Change PermitRootLogin to this:
PermitRootLogin no

# Change ChallengeResponseAuthentication to this:
ChallengeResponseAuthentication no

# On the Debian Family
username@macbook$ sudo service ssh restart

# On the RedHat Family
username@macbook$ sudo service sshd restart
```

## <a name="detailed-walk-through"></a>Részletes útmutatást kapni a

Jelentkezzen be a Linux virtuális Terminálszolgáltatások 1 (T1).  Jelentkezzen be a Linux virtuális Terminálszolgáltatások 2 (T2).

A T2 fogjuk a SSHD konfigurációs fájl szerkesztése.  

```
username@macbook$ sudo vim /etc/ssh/sshd_config
```

További lehetőségek azt fogja szerkesztése le szeretne tiltani a jelszavak SSH kulcs bejelentkezések engedélyezése csak a beállításokat.  Vannak, hogy Linux & SSH biztonságban szükség szerint a fájl, amely kell a kutatási és miként módosíthatja a sok beállítások.

#### <a name="disable-password-logins"></a>Jelszó bejelentkezések letiltása

```
username@macbook$ sudo vim /etc/ssh/sshd_config

# Change PasswordAuthentication to this:
PasswordAuthentication no
```

#### <a name="enable-public-key-authentication"></a>Nyilvános kulcs hitelesítés engedélyezése

```
username@macbook$ sudo vim /etc/ssh/sshd_config

# Change PubkeyAuthentication to this:
PubkeyAuthentication yes
```

#### <a name="disable-root-login"></a>Tiltsa le a legfelső szintű bejelentkezést

```
username@macbook$ sudo vim /etc/ssh/sshd_config

# Change PermitRootLogin to this:
PermitRootLogin no
```

#### <a name="disable-challenge-response-authentication"></a>Tiltsa le a kérdés-válasz hitelesítés

```
# Change ChallengeResponseAuthentication to this:
ChallengeResponseAuthentication no
```

### <a name="restart-sshd"></a>Indítsa újra a SSHD

Az T1 rendszerhéj győződjön meg arról, hogy továbbra is jelentkezett be.  Ez a helyzet kritikus, nem első zárolt ki a virtuális, ha a SSH kulcsok nem helyesek el, mivel a jelszavak most le vannak tiltva.  Ha bármely beállítását a Linux virtuális sshd_config javítása, akkor továbbra is naplózza a rendszer és SSH megőrzi a kapcsolat életben a SSHD szolgáltatás során T1 is használhatja a helytelen indítsa újra.

A Futtatás T2:

##### <a name="on-the-debian-family"></a>Kattintson a Debian család

```
username@macbook$ sudo service ssh restart
```

##### <a name="on-the-redhat-family"></a>Kattintson a RedHat család

```
username@macbook$ sudo service sshd restart
```

A virtuális szembeni védelmével kapcsolatban: Ez ellen jelszó bejelentkezések jelszavak most le vannak tiltva.  Csak SSH billentyűkkel engedélyezett a tud bejelentkezni gyorsabban és sokkal több biztonságos lesz.
