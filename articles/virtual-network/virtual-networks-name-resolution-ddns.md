<properties
   pageTitle="A dinamikus DNS állomásnevekké rögzítése"
   description="Ezen az oldalon állomásnevekké regisztrálhatja a saját DNS-kiszolgálók dinamikus DNS-beállításával kapcsolatos részletezi."
   services="dns"
   documentationCenter="na"
   authors="GarethBradshawMSFT"
   manager="carmonm"
   editor="" />
<tags
   ms.service="dns"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/31/2016"
   ms.author="sewhee" />

# <a name="using-dynamic-dns-to-register-hostnames-in-your-own-dns-server"></a>Dinamikus DNS segítségével saját DNS-kiszolgáló állomásnevekké rögzítéséhez

[Azure biztosít névfeloldás](virtual-networks-name-resolution-for-vms-and-role-instances.md) virtuális gépeken futó (VMs) és a szerepkör-példányok. Amikor a névfeloldás túl az Azure által biztosított szüksége van, megadhatja saját DNS-kiszolgálók. Ez a kínálja szabja testre a DNS-megoldás a saját egyedi módon való. Előfordulhat például, helyszíni erőforrások via az Active Directory tartományi vezérlő eléréséhez.

Az egyéni DNS-kiszolgálók irányítópultokkal Azure VMs, amikor az azonos vnet hostname (állomásnév) lekérdezések továbbíthat Azure állomásnevekké feloldásához. Ha nem szeretné használni, ez az útvonal, a DNS-kiszolgáló dinamikus DNS segítségével regisztrálhatja a virtuális állomásnevekké.  Azure nincs lehetősége (például a hitelesítő adatok) közvetlenül létrehozására, rekordokat a DNS-kiszolgálók, az alternatív megoldások gyakran van szükség. Az alábbiakban néhány gyakori alkalmazási területek lehetőségeket.

## <a name="windows-clients"></a>Windows-ügyfelek

Windows-ügyfelek nem tartományhoz tartozó nem biztonságos dinamikus DNS (DDNS) frissítések próbálja meg, ha indítsa el őket, vagy ha az azok IP-cím módosítása. A hostname (állomásnév), valamint az elsődleges DNS-utótag kell DNS nevét. Azure az elsődleges DNS-utótag üresen hagyja, de ez beállíthatja a virtuális az [felhasználói felület](https://technet.microsoft.com/library/cc794784.aspx) vagy a [automatizálási használatával](https://social.technet.microsoft.com/forums/windowsserver/3720415a-6a9a-4bca-aa2a-6df58a1a47d7/change-primary-dns-suffix).

Windows-ügyfelek tartományhoz regisztrálhatja a IP-címek a tartományvezérlőnek biztonságos dinamikus DNS használatával. A tartomány illesztés folyamat állítja be az elsődleges DNS-utótag az ügyfélszámítógépen, és hozza létre, és a meghatalmazás kezeli.

## <a name="linux-clients"></a>Linux ügyfelek

Linux ügyfelek általában nem regisztrálása magukat a DNS-kiszolgáló indításkor, azok tegyük fel, hogy a útja jelent az. Azure-féle Serveren nem rendelkezik regisztrálni a rekordokat a DNS-kiszolgálót az a lehetősége vagy a hitelesítő adatait.  *Nsupdate*, amely szerepel a kötés csomag, nevű eszköz használatával dinamikus DNS módosításai küldése. A dinamikus DNS protokoll szabványos van, mert *nsupdate* használhatja, akkor is, ha nem használ kötés a DNS-kiszolgálón.

A horgok az IDHCP létrehozása és kezelése a hostname (állomásnév) bejegyzést a DNS-kiszolgáló által biztosított is használhatja. A DHCP ciklus során az ügyfél a parancsfájlok */etc/dhcp/dhclient-exit-hooks.d/*hajtja végre. Ez az új IP-cím regisztrálásához *nsupdate*használatával is használható. Példa:

        #!/bin/sh
        requireddomain=mydomain.local

        # only execute on the primary nic
        if [ "$interface" != "eth0" ]
        then
            return
        fi

        # when we have a new IP, perform nsupdate
        if [ "$reason" = BOUND ] || [ "$reason" = RENEW ] ||
           [ "$reason" = REBIND ] || [ "$reason" = REBOOT ]
        then
            host=`hostname`
            nsupdatecmds=/var/tmp/nsupdatecmds
              echo "update delete $host.$requireddomain a" > $nsupdatecmds
              echo "update add $host.$requireddomain 3600 a $new_ip_address" >> $nsupdatecmds
              echo "send" >> $nsupdatecmds

              nsupdate $nsupdatecmds
        fi

        #done
        exit 0;

A *nsupdate* parancs használatával biztonságos dinamikus DNS-frissítések végrehajtása. Ha például kötést létrehozni a DNS-kiszolgáló használata esetén, nyilvános-titkos kulcs két jön [létre](http://linux.yyz.us/nsupdate/).  A DNS-kiszolgáló [konfigurálva](http://linux.yyz.us/dns/ddns-server.html) a kulcs nyilvános része, hogy ellenőrizheti, hogy az aláírás a kérést. A *-k* beállítás megadásához a billentyű-két *nsupdate* ahhoz, hogy a dinamikus DNS frissítése az aláírandó kérelem kell használnia.

Ha egy Windows DNS-kiszolgáló használata esetén a *nsupdate* ( *nsupdate*Windows verziójában nem érhető el) *-g* paraméterrel Kerberos-hitelesítés is használhatja. Ehhez a *kinit* segítségével töltse be a hitelesítő adatait (például a [keytab fájl](http://www.itadmintools.com/2011/07/creating-kerberos-keytab-files.html)). *Nsupdate -g* majd a hitelesítő adatokat a gyorsítótárból felvétele.

Ha szükséges, a DNS-keresés utótag hozzáadhatja a VMs. A DNS-utótag a */etc/resolv.conf* fájlban van megadva. A legtöbb Linux distros automatikusan kezelheti a tartalmat a fájl, így általában nem szerkeszthető. Az utótag azonban bármikor felülbírálva paranccsal az IDHCP *váltja fel* . Ehhez */etc/dhcp/dhclient.conf*hozzáadása:

        supersede domain-name <required-dns-suffix>;

