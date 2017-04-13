<properties 
   pageTitle="DNS névfeloldás beállításai a Linux VMs Azure-ban"
   description="Név felbontás forgatókönyv a Linux VMs Azure IaaS, beleértve a DNS-szolgáltatások, a hibrid külső DNS és hozása: A saját DNS-kiszolgáló."
   services="virtual-machines"
   documentationCenter="na"
   authors="RicksterCDN"
   manager="timlt"
   editor="tysonn" />
<tags 
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/19/2016"
   ms.author="rclaus" />

# <a name="dns-name-resolution-options-for-linux-vms-in-azure"></a>DNS névfeloldás beállításai a Linux VMs Azure-ban

Azure nyújt a DNS-névfeloldás alapértelmezés szerint egy virtuális hálózaton található összes VMs. Alkalmasak a saját DNS-neve felbontás megoldást végrehajtásához az Azure szolgáltatott VMs a saját DNS-szolgáltatások konfigurálásával. Érdemes elolvasnia az alábbi esetekben válassza ki, melyik jobban működik az adott helyzetéhez.

- [Azure által biztosított névfeloldás](#azure-provided-name-resolution)

- [A saját DNS-kiszolgálót használ névfeloldás](#name-resolution-using-your-own-dns-server) 

A használt névfeloldás típusú attól függ, hogyan a VMs és szerepkör-példányok kell egymással.

**A következő táblázat mutatja az esetek és a megfelelő név felbontás megoldások:**

| **Eset** | **Megoldás** | **Utótag** |
|--------------|--------------|----------|
| Szerepkör-példányok vagy ugyanazon a virtuális hálózaton található VMs közötti névfeloldás | [Azure által biztosított névfeloldás](#azure-provided-name-resolution)| Hostname (állomásnév) vagy a teljes Tartománynevet |
| Szerepkör-példányok vagy VMs, olvassa el a különböző virtuális hálózatok között névfeloldás | Ügyfél által kezelt DNS-kiszolgálók között vnets feloldáshoz Azure (DNS-proxy) szerint lekérdezések továbbítása.  Lásd: [a saját DNS-kiszolgálót használ névfeloldás](#name-resolution-using-your-own-dns-server)| Csak FQDN |
| A helyszíni számítógép és a szolgáltatás neve szerepkör-példányok vagy VMs Azure-ban felbontásának | Ügyfél által kezelt DNS-kiszolgálók (például helyszíni tartományvezérlőnek, helyi csak olvasható tartományvezérlőnek, vagy a DNS-másodlagos zónát használatával szinkronizált).  Lásd: [a saját DNS-kiszolgálót használ névfeloldás](#name-resolution-using-your-own-dns-server)|Csak FQDN |
| Azure állomásnevekké helyszíni számítógépekről felbontásának | Ügyfél által kezelt DNS-proxy kiszolgáló a megfelelő vnet előre lekérdezéseket, a proxykiszolgáló továbbítja lekérdezések Azure vonatkozó felbontásban. Lásd: [a saját DNS-kiszolgálót használ névfeloldás](#name-resolution-using-your-own-dns-server)| Csak FQDN |
| Fordított DNS belső IP-címei | [A saját DNS-kiszolgálót használ névfeloldás](#name-resolution-using-your-own-dns-server) | a #hiányzik |

## <a name="azure-provided-name-resolution"></a>Azure által biztosított névfeloldás

Nyilvános DNS-nevek felbontásának együtt Azure nyújt a belső névfeloldás VMs és szerepkör-példányok, amely a virtuális hálózaton belül találhatók.  ARM-alapú virtuális hálózatok a DNS-utótag egységesek a virtuális hálózaton keresztül (tehát nem szükséges a FQDN), és DNS-neveket is hálózati kártyák és VMs rendelhetők. Azure által biztosított névfeloldás nem szükséges beállításokat, de még nem a megfelelő választás minden telepítési forgatókönyvek, ahogy a fenti táblázatban látható.

### <a name="features-and-considerations"></a>Funkciók és szempontok

**Az alábbi szolgáltatások:**

- Egyszerű használat: nincs konfigurációs Azure által biztosított névfeloldás használatához szükséges.

- Azure által biztosított névfeloldás biztosítása erősen elérhetővé válik, mentése, létrehozása és kezelése a saját DNS-kiszolgálók fürt kell.

- Használható együtt a saját DNS-kiszolgálók helyszíni és is Azure állomásnevekké feloldásához.

- Névfeloldás VMs nélkül a teljesen minősített tartománynév virtuális hálózatok között megadva. 

- A telepítési környezetének legjobban leíró állomásnevekké használható automatikusan létrehozott nevek használata helyett.

**Szempontok**

- Az Azure létrehozott DNS-utótag nem módosítható.

- Nem lehet manuálisan rögzítheti-rekordok önálló.

- WINS és NetBIOS nem támogatottak.

- Állomásnevekké kell lennie a DNS-kompatibilis (csak 0 – 9-es, a – z kell használniuk, és "-", és nem kezdődhet vagy végződhet egy "-". Lásd: RFC 3696 szakasz 2.)

- Minden egyes virtuális folyamatban van a DNS-lekérdezést adatforgalom. Ne hatással lehet a legtöbb alkalmazással.  Kérés szabályozásának megfigyelt, győződjön meg arról, hogy engedélyezve van-e az ügyféloldali gyorsítótárazás.  További tudnivalókért lásd: [Azure által biztosított névfeloldás a legtöbbet](#Getting-the-most-from-Azure-provided-name-resolution).


### <a name="getting-the-most-from-azure-provided-name-resolution"></a>Ahhoz, hogy a legtöbb Azure által biztosított névfeloldás
**Ügyféloldali gyorsítótárazás:**

Nem minden DNS-lekérdezés érkezik, a hálózaton keresztül.  Ügyféloldali gyorsítótárazás segítséget nyújt a késés csökkentése és a hálózati blips címtárfrissítések növelheti a helyi gyorsítótárból ismétlődő DNS-lekérdezések megoldása.  DNS-rekordjait a Time-To-Live (TTL), amely lehetővé teszi, hogy a gyorsítótár tárolására, amíg a rekord nélkül rekord frissessége érintő tartalmazzák.  Emiatt ügyféloldali gyorsítótárazás alkalmas esetek többségében.

Néhány Linux distros gyorsítótárazás alapértelmezés szerint nem tartalmazzák.  Ajánlott hozzáadása egy a minden Linux virtuális (után ellenőrzése, hogy nem egy helyi gyorsítótár már).

Vannak olyan számos különböző DNS gyorsítótárazási csomagok érhető el, például dnsmasq, dnsmasq telepítése a leggyakoribb distros szereplő lépések a következők:

- **Ubuntu (resolvconf használ)**:
    - Telepítse a dnsmasq csomagot ("sudo apt get-telepítés dnsmasq").
- **SUSE (netconf használ)**:
    - Telepítse a dnsmasq csomagot ("sudo zypper telepítés dnsmasq") 
    - Engedélyezze a dnsmasq szolgáltatást ("systemctl engedélyezése dnsmasq.service") 
    - Indítsa el a dnsmasq szolgáltatást ("systemctl start dnsmasq.service") 
    - Szerkesztés "/ config stb/sysconfig/hálózati", és módosíthatja a NETCONFIG_DNS_FORWARDER = "" a "dnsmasq."
    - a helyi DNS-feloldó frissíti a resolv.conf ("netconfig frissítése") a gyorsítótár beállítása
- **OpenLogic (NetworkManager használ)**:
    - Telepítse a dnsmasq csomagot ("sudo yum telepítés dnsmasq")
    - Engedélyezze a dnsmasq szolgáltatást ("systemctl engedélyezése dnsmasq.service")
    - Indítsa el a dnsmasq szolgáltatást ("systemctl-dnsmasq.service indítása")
    - Adja hozzá a "prepend tartomány-name servers 127.0.0.1;" a "/etc/dhclient-eth0.conf"
    - Indítsa újra a gyorsítótár jelölhető meg a helyi DNS-feloldó hálózati szolgáltatást ("szolgáltatásának hálózati újraindítása")

> [AZURE.NOTE]: A "dnsmasq" csomag egyike csak a sok DNS gyorsítótárát Linux érhető el.  Mielőtt használni kezdi, jelölje be az adott igényeinek alkalmas, és nincs más gyorsítótár telepítve van.

**Ügyféloldali újrapróbálkozások:**

DNS-elsősorban UDP protokoll egy.  Az UDP protokoll nem garantálja az üzenet kézbesítését, mint újrapróbálkozási logika kezelése a DNS-protokoll magát.  Minden DNS-ügyfél (operációs rendszer) is mutathat különböző újrapróbálkozási logika attól függően, hogy a készítői preferencia-sorrend:

 - Windows operációs rendszerek újra után egy második, és válassza ismét után egy másik két, négy és egy másik négy másodperc. 
 - Az alapértelmezett Linux beállítási újrapróbálkozások öt másodperc után.  Ez a öt alkalommal egy másodperces időközönként próbálja meg kell módosítása.  

Ha például ellenőrzése Linux virtuális, "macska /etc/resolv.conf" és a "beállítások" sor figyelmébe az aktuális beállítást:

    options timeout:1 attempts:5

A resolv.conf fájl jön létre automatikusan, és nem szerkeszthetők.  Az adott lépések leírását a "beállítások" sor distro függően eltérőek:

- **Ubuntu** (resolvconf használ):
    - a beállítások sor hozzáadása "/ etc/resolveconf/resolv.conf.d/head" 
    - Futtassa a resolvconf -u frissítése
- **SUSE** (netconf használ):
    - "timeout:1 kísérletek: 5" hozzáadása a NETCONFIG_DNS_RESOLVER_OPTIONS = "" a "/ config stb/sysconfig/hálózati" paraméter 
    - Futtassa a "frissítés netconfig" frissítése
- **OpenLogic** (NetworkManager használ):
    - "a"beállítások timeout:1 kísérletek: 5"visszhang" hozzáadása "/ etc/NetworkManager/dispatcher.d/11-dhclient" 
    - Futtassa a 'hálózati újraindításra szolgáltatás' frissítése

## <a name="name-resolution-using-your-own-dns-server"></a>A saját DNS-kiszolgálót használ névfeloldás
Vannak olyan igényeinek megfelelően neve felbontás előfordulhat, hogy hová túl a funkciók feltéve által Azure, például amikor szüksége van DNS-feloldási virtuális hálózatok (vnets) közötti több helyzetek.  Hogy pontosan illeszkedjen ebben az esetben, Azure lehetővé teszi a segítségével saját DNS-kiszolgálók.  

DNS-kiszolgálók belül egy virtuális hálózati DNS-lekérdezések továbbíthat Azure-féle rekurzív névfeloldók úgy, hogy virtuális hálózaton belül állomásnevekké.  DNS-kiszolgáló Azure-ban például saját DNS-zóna fájlok a DNS-lekérdezések megválaszolása és minden más lekérdezés továbbítja az Azure.  Ebben a csoportban adhatja VMs mindkét a bejegyzést, a zóna fájlok és Azure által biztosított állomásnevekké (keresztül a továbbító) megjelenítéséhez.  Azure-féle rekurzív névfeloldók a hozzáférést a virtuális IP 168.63.129.16 keresztül kapja.

DNS-továbbítás is lehetővé teszi, hogy a többek vnet DNS-feloldási, és lehetővé teszi, hogy a helyszíni gépek Azure által biztosított állomásnevekké feloldásához.  Egy virtuális hostname (állomásnév) elhárításához a DNS-kiszolgáló virtuális kell lennie, virtuális ugyanabba a hálózatba, és beállítania, hogy Azure előre hostname (állomásnév) lekérdezéseket.  A DNS-utótag eltér az egyes vnet, mint a feltételes továbbítási szabályok használatával küld a megfelelő vnet vonatkozó felbontásban DNS-lekérdezéseket.  Az alábbi képen az látható, két vnets és egy helyszíni hálózat többek vnet DNS-feloldási ezzel a módszerrel módon:

![Többek vnet DNS](./media/virtual-machines-linux-azure-dns/inter-vnet-dns.png)

Azure által biztosított névfeloldás használata esetén a belső DNS-utótag kapja meg minden virtuális DHCP használatával.  Amikor a saját neve felbontás megoldást, az utótag nem adják át VMs, mert azt zavarja más DNS architektúrákban.  Teljesen minősített tartománynév által gépeken futó hivatkozni, vagy az utótag tartományi a VMs, az utótag meghatározható PowerShell alrendszerrel vagy a API:

-  Erőforrás-kezelés Azure felügyelt vnets, a képz_jel nem érhető el a [hálózati kártya](https://msdn.microsoft.com/library/azure/mt163668.aspx) forráson keresztül, vagy a parancsot futtatja `azure network public-ip show <resource group> <pip name>` a nyilvános IP-, beleértve a adaptert teljes Tartománynevét a részletek megjelenítéséhez    


Ha Azure továbbít lekérdezések nem felel meg igényeinek, meg kell adnia a saját DNS-megoldást.  A DNS-megoldás kell:

-  Adja meg a megfelelő hostname (állomásnév) felbontás, például [DDNS](../virtual-network/virtual-networks-name-resolution-ddns.md)keresztül.  Figyelje meg, ha szüksége lehet letiltani a DNS record takarítás Azure-féle DHCP bérletek vannak túl hosszú és törlési DDNS használatával eltávolíthatja a DNS-rekordok idő. 
-  Adja meg a megfelelő rekurziót engedélyezése a külső tartománynevek felbontását.
-  Könnyen kezelhető (TCP- és UDP-port 53) szolgál az ügyfelek és fér hozzá az internethez.
-  Az internetről külső ügynökök által felvetett kockázatok csökkentésében lehet access ellen védett.

> [AZURE.NOTE] A legjobb teljesítmény elérése érdekében Azure VMs, a DNS-kiszolgálók használatakor az IPv6 le lesznek tiltva, és egy [Példány szintű nyilvános IP](../virtual-network/virtual-networks-instance-level-public-ip.md) egyes DNS-kiszolgálók virtuális kell rendelni.  

