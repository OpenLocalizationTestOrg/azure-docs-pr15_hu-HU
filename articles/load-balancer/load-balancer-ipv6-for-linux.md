<properties
    pageTitle="Linux VMs DHCPv6 beállítása |} Microsoft Azure"
    description="Hogyan lehet a szolgáltatáshoz való konfigurálása DHCPv6 Linux VMs."
    services="load-balancer"
    documentationCenter="na"
    authors="sdwheeler"
    manager="carmonm"
    editor=""
    keywords="az IPv6, azure terheléselosztó, kettős Papírhalom, nyilvános ip, natív ipv6, Mobiltelefonról, iot"
/>
<tags
    ms.service="load-balancer"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="09/14/2016"
    ms.author="sewhee"
/>

# <a name="configuring-dhcpv6-for-linux-vms"></a>Linux VMs DHCPv6 konfigurálása

A Microsoft Azure piactéren Linux virtuális gép képek részét DHCPv6 alapértelmezés szerint nem rendelkezik. Az IPv6 támogatása érdekében DHCPv6 kell beállítania a a használni kívánt Linux OS eloszlás belül. Különböző Linux terjesztését DHCPv6 beállítását, mert azok más csomagok különböző módokon van.

>[AZURE.NOTE] A Microsoft Azure piactéren legutóbbi SUSE Linux és CoreOS képek DHCPv6 az előre beállított volna. Nincsenek további módosításokat szükség, ezekkel a képekkel használata esetén.

A dokumentum DHCPv6 ahhoz, hogy a Linux virtuális gép beolvassa az IPv6-címek ismerteti.

>[AZURE.WARNING] Helytelen a hálózati konfigurációja fájlok szerkesztése a virtuális hálózati hozzáférés elveszíti okozhatják. Ajánlott, hogy a módosításokat nem éles rendszeren vizsgálati. Ez a cikk utasításait teszteltük a Linux képek az Azure piactéren elérhető legújabb verzióját. A részletes útmutatás Linux adott verziójának dokumentációjában.

## <a name="ubuntu"></a>Ubuntu

1. Szerkessze a fájlt `/etc/dhcp/dhclient6.conf` és a következő sor hozzáadása:

        timeout 10;

2. A hálózati konfigurálásról a eth0 felületén a következő beállításokkal szerkesztése:

    * **Ubuntu 12.04 és 14.04**, kattintson a fájl szerkesztése`/etc/network/interfaces.d/eth0.cfg`
    * **Ubuntu 16.04**a fájl szerkesztése`/etc/network/interfaces.d/50-cloud-init.cfg`

    ```bash
    iface eth0 inet6 auto
        up sleep 5
        up dhclient -1 -6 -cf /etc/dhcp/dhclient6.conf -lf /var/lib/dhcp/dhclient6.eth0.leases -v eth0 || true
    ```

3. Megújítás IPv6 address:

    ```bash
    # sudo ifdown eth0 && sudo ifup eth0
    ```

## <a name="debian"></a>Debian

1. Szerkessze a fájlt `/etc/dhcp/dhclient6.conf` és a következő sor hozzáadása:

        timeout 10;

2. Szerkessze a fájlt `/etc/network/interfaces` , és adja hozzá a következő beállításokat:

        iface eth0 inet6 auto
            up sleep 5
            up dhclient -1 -6 -cf /etc/dhcp/dhclient6.conf -lf /var/lib/dhcp/dhclient6.eth0.leases -v eth0 || true

3. Megújítás IPv6 address:

    ```bash
    # sudo ifdown eth0 && sudo ifup eth0
    ```

## <a name="rhel--centos--oracle-linux"></a>RHEL / CentOS / Oracle Linux

1. Szerkessze a fájlt `/etc/sysconfig/network` és a következő paraméter felvétele:

        NETWORKING_IPV6=yes

2. Szerkessze a fájlt `/etc/sysconfig/network-scripts/ifcfg-eth0` és a következő két paraméterek hozzáadása:

        IPV6INIT=yes
        DHCPV6C=yes

3. Megújítás IPv6 address:

    ```bash
    # sudo ifdown eth0 && sudo ifup eth0
    ```

## <a name="sles-11--opensuse-13"></a>SLES 11 & openSUSE 13-mal

Azure-ban a legutóbbi SLES és openSUSE képek DHCPv6 az előre beállított volna. Nincsenek további módosításokat szükség, ezekkel a képekkel használata esetén. Ha egy régebbi vagy egyéni SUSE kép alapján virtuális, kövesse az alábbi lépéseket:

1. Telepítse a `dhcp-client` csomag, szükség esetén:

    ```bash
    # sudo zypper install dhcp-client
    ```

2. Szerkessze a fájlt `/etc/sysconfig/network/ifcfg-eth0` és a következő paraméter felvétele:

        DHCLIENT6_MODE='managed'

3. Az IPv6 address megújításhoz:

    ```bash
    # sudo ifdown eth0 && sudo ifup eth0
    ```

## <a name="sles-12-and-opensuse-leap"></a>SLES 12 és openSUSE termékek

Azure-ban a legutóbbi SLES és openSUSE képek DHCPv6 az előre beállított volna. Nincsenek további módosításokat szükség, ezekkel a képekkel használata esetén. Ha egy régebbi vagy egyéni SUSE kép alapján virtuális, kövesse az alábbi lépéseket:

1. Szerkessze a fájlt `/etc/sysconfig/network/ifcfg-eth0` , és cserélje le a paraméter

        #BOOTPROTO='dhcp4'

    a következő értéket:

        BOOTPROTO='dhcp'

2. Adja hozzá a következő paraméter `/etc/sysconfig/network/ifcfg-eth0`:

        DHCLIENT6_MODE='managed'

3. Az IPv6 address megújításhoz:

    ```bash
    # sudo ifdown eth0 && sudo ifup eth0
    ```

## <a name="coreos"></a>CoreOS

A legutóbbi CoreOS képek Azure-ban DHCPv6 az előre beállított volna. Nincsenek további módosításokat szükség, ezekkel a képekkel használata esetén. Ha egy régebbi vagy egyéni CoreOS kép alapján virtuális, kövesse az alábbi lépéseket:

1. A fájl szerkesztése`/etc/systemd/network/10_dhcp.network`

        [Match]
        eth0

        [Network]
        DHCP=ipv6

2. Az IPv6 address megújításhoz:

    ```bash
    # sudo systemctl restart systemd-networkd
    ```
