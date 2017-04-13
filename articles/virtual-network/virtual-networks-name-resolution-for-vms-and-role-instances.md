<properties 
   pageTitle="Felbontását VMs és szerepkör-példányok"
   description="Megoldás esetek Name Azure IaaS, hibrid megoldások különböző cloud services, az Active Directory és a saját DNS-kiszolgálót használ között "
   services="virtual-network"
   documentationCenter="na"
   authors="GarethBradshawMSFT"
   manager="carmonm"
   editor="tysonn" />
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/31/2016"
   ms.author="telmos" />

# <a name="name-resolution-for-vms-and-role-instances"></a>A névfeloldás VMs és szerepkör-példányok

Attól függően, hogy hogyan használatával Azure IaaS, PaaS és hibrid megoldások előfordulhat, hogy engedélyeznie kell a VMs és szerepkör-példányok létrehozott egymással. Bár ez kommunikációs IP-címek használatával hajtható végre, is sokkal egyszerűbb név, amely jól megjegyezhető, és ne módosítsa. 

Szerepkör-példányok és Azure-ban tárolt VMs kell a tartománynevek belső IP-címek, amikor azok két módszer közül választhat:

- [Azure által biztosított névfeloldás](#azure-provided-name-resolution)

- [A saját DNS-kiszolgálót használ névfeloldás](#name-resolution-using-your-own-dns-server) (amely lehet továbbítsa az Azure által biztosított DNS-kiszolgálók) 

A használt névfeloldás típusú attól függ, hogyan a VMs és szerepkör-példányok kell egymással.

**Az alábbi táblázat azt mutatja be, forgatókönyveket és a megfelelő név felbontás megoldások:**

| **Eset** | **Megoldás** | **Utótag** |
|--------------|--------------|----------|
| Szerepkör-példányok vagy egy felhőalapú szolgáltatásba vagy virtuális hálózati VMs közötti névfeloldás | [Azure által biztosított névfeloldás](#azure-provided-name-resolution)| Hostname (állomásnév) vagy a teljes Tartománynevet |
| Szerepkör-példányok vagy VMs, olvassa el a különböző virtuális hálózatok között névfeloldás | Ügyfél által kezelt DNS-kiszolgálók között vnets feloldáshoz Azure (DNS-proxy) szerint lekérdezések továbbítása.  Lásd: [a saját DNS-kiszolgálót használ névfeloldás](#name-resolution-using-your-own-dns-server)| Csak FQDN |
| A helyszíni számítógép és a szolgáltatás neve szerepkör-példányok vagy VMs Azure-ban felbontásának | Ügyfél által kezelt DNS-kiszolgálók (pl. helyszíni tartományvezérlőnek, helyi csak olvasható tartományvezérlőnek vagy a DNS-másodlagos zónát használatával szinkronizált).  Lásd: [a saját DNS-kiszolgálót használ névfeloldás](#name-resolution-using-your-own-dns-server)|Csak FQDN |
| Azure állomásnevekké helyszíni számítógépekről felbontásának | Ügyfél által kezelt DNS-proxy kiszolgáló a megfelelő vnet előre lekérdezéseket, a proxykiszolgáló továbbítja lekérdezések Azure vonatkozó felbontásban. Lásd: [a saját DNS-kiszolgálót használ névfeloldás](#name-resolution-using-your-own-dns-server)| Csak FQDN |
| Fordított DNS belső IP-címei | [A saját DNS-kiszolgálót használ névfeloldás](#name-resolution-using-your-own-dns-server) | a #hiányzik |
| A névfeloldás VMs vagy más felhőszolgáltatások, nem a virtuális hálózaton található szerepkör-példányok között| Nem alkalmazható. Nem támogatott egy virtuális hálózatán kívülről VMs és más felhőszolgáltatások szerepkör példányok közötti kapcsolatot.| a #hiányzik |



## <a name="azure-provided-name-resolution"></a>Azure által biztosított névfeloldás

Nyilvános DNS-nevek felbontásának együtt Azure nyújt a belső névfeloldás VMs és szerepkör-példányok, amely a virtuális hálózaton vagy a felhőbeli szolgáltatástól találhatók.  Egy felhőalapú szolgáltatásba VMs/példányok osztani ugyanazt a DNS-utótag (önmagában hostname (állomásnév) is elegendő), de klasszikus virtuális hálózatok különböző felhőben a szolgáltatások más DNS-utótag rendelkezik, így a teljesen minősített tartománynév van szükség különböző felhőszolgáltatások közötti névfeloldásra.  Az erőforrás-kezelő telepítési modell virtuális hálózatok a DNS-utótag egységesek a virtuális hálózaton keresztül (tehát nem szükséges a FQDN), és a DNS-neveket is hálózati kártyák és VMs rendelhetők. Azure által biztosított névfeloldás nem szükséges beállításokat, de még nem a megfelelő választás minden telepítési forgatókönyvek, ahogy a fenti táblázatban látható.

> [AZURE.NOTE] Webes és dolgozó szerepkörök, amíg a belső IP-címek alapján szerepkör nevét és példányát a Azure szolgáltatás felügyeleti REST API szerepkör-példányok is elérheti. További tudnivalókért olvassa el a [Szolgáltatás kezelése REST API-útmutató](https://msdn.microsoft.com/library/azure/ee460799.aspx)című témakört.

### <a name="features-and-considerations"></a>Funkciók és szempontok

**Az alábbi szolgáltatások:**

- Egyszerű használat: Azure által biztosított névfeloldás használatához szükséges beállítást.

- Azure által biztosított névfeloldás biztosítása erősen elérhetővé válik, mentése, létrehozása és kezelése a saját DNS-kiszolgálók fürt kell.

- Használható együtt a saját DNS-kiszolgálók helyszíni és is Azure állomásnevekké feloldásához.

- Névfeloldás példányok szerepkör VMs belül ugyanazt a felhőalapú szolgáltatást, anélkül, hogy szüksége van egy teljesen minősített tartománynév között megadva.

- Névfeloldás VMs az erőforrás-kezelő telepítési modell nélkül teljes Tartománynevét használó virtuális hálózatok között megadva. A klasszikus telepítési modell virtuális hálózatok kell a teljes Tartománynevét névfeloldás a különböző felhőszolgáltatások közben. 

- A telepítési környezetének legjobban leíró állomásnevekké használható automatikusan létrehozott nevek használata helyett.

**Szempontok**

- Az Azure létrehozott DNS-utótag nem módosítható.

- Nem lehet manuálisan rögzítheti-rekordok önálló.

- WINS és NetBIOS nem támogatottak. (Nem lásd: a Windows Intézőben VMs.)

- Állomásnevekké kell lennie a DNS-kompatibilis (csak 0 – 9-es, a – z kell használniuk, és "-", és nem kezdődhet vagy végződhet egy "-". Lásd: RFC 3696 szakasz 2.)

- Minden egyes virtuális folyamatban van a DNS-lekérdezést adatforgalom. Ne hatással lehet a legtöbb alkalmazással.  Kérés szabályozásának megfigyelt, győződjön meg arról, hogy engedélyezve van-e az ügyféloldali gyorsítótárazás.  További részletekért olvassa el [a legtöbbet Azure által biztosított névfeloldás](#Getting-the-most-from-Azure-provided-name-resolution).

- Csak az első 180 cloud Services VMs minden virtuális hálózati klasszikus telepítési adatmodell regisztrált. Ez nem vonatkozik a virtuális hálózatokhoz erőforrás-kezelő telepítési modellekben.


### <a name="getting-the-most-from-azure-provided-name-resolution"></a>Ahhoz, hogy a legtöbb Azure által biztosított névfeloldás
**Ügyféloldali gyorsítótárazás:**

Nem minden DNS-lekérdezést kell lehet küldeni a hálózaton keresztül.  Ügyféloldali gyorsítótárazás segítséget nyújt a késés csökkentése és a hálózati blips címtárfrissítések növelheti a helyi gyorsítótárból ismétlődő DNS-lekérdezések megoldása.  DNS-rekordjait a Time-To-Live (TTL), amely lehetővé teszi, hogy a gyorsítótár tárolására, amíg a rekord nélkül érintő a rekord frissessége, így az ügyféloldali gyorsítótárazás alkalmas esetek többségében tartalmazzák.

Az alapértelmezett Windows DNS-ügyfél egy beépített DNS-gyorsítótár tartalmaz.  Néhány Linux distros nem tartalmazza a gyorsítótár-alapértelmezés szerint, javasoljuk, hogy egy adható hozzá minden Linux virtuális (után ellenőrzése, hogy nem egy helyi gyorsítótár már).

Számos különböző DNS gyorsítótárazási csomagok érhető el, például dnsmasq, dnsmasq telepítése a leggyakoribb distros szereplő lépések a következők:

- **Ubuntu (resolvconf használ)**:
    - csak a telepítés a dnsmasq csomag ("sudo apt get-telepítés dnsmasq").
- **SUSE (netconf használ)**:
    - Telepítse a dnsmasq csomagot ("sudo zypper telepítés dnsmasq") 
    - Engedélyezze a dnsmasq szolgáltatást ("systemctl engedélyezése dnsmasq.service") 
    - Indítsa el a dnsmasq szolgáltatást ("systemctl start dnsmasq.service") 
    - Szerkesztés "/ config stb/sysconfig/hálózati", és módosíthatja a NETCONFIG_DNS_FORWARDER = "" a "dnsmasq"
    - a helyi DNS-feloldó frissíti a resolv.conf ("netconfig frissítése") a gyorsítótár beállítása
- **OpenLogic (NetworkManager használ)**:
    - Telepítse a dnsmasq csomagot ("sudo yum telepítés dnsmasq")
    - Engedélyezze a dnsmasq szolgáltatást ("systemctl engedélyezése dnsmasq.service")
    - Indítsa el a dnsmasq szolgáltatást ("systemctl start dnsmasq.service")
    - Adja hozzá a "prepend tartomány-name servers 127.0.0.1;" a "/etc/dhclient-eth0.conf"
    - Indítsa újra a gyorsítótár jelölhető meg a helyi DNS-feloldó hálózati szolgáltatást ("szolgáltatásának hálózati újraindítása")

> [AZURE.NOTE] A "dnsmasq" csomag egyike csak a sok DNS gyorsítótárát Linux érhető el.  Mielőtt használni kezdi, ellenőrizze az adott igényeinek alkalmas, és nincs más gyorsítótár telepítve van.

**Ügyféloldali ismétlés:**

DNS-elsősorban UDP protokoll egy.  Az UDP protokoll nem garantálja az üzenet kézbesítését, mint újrapróbálkozási logika kezelése a DNS-protokoll magát.  Minden DNS-ügyfél (operációs rendszer) is mutathat különböző újrapróbálkozási logika attól függően, hogy a készítői preferencia-sorrend:

 - Windows operációs rendszerek újra 1-es után második, és válassza ismét után egy másik 2, 4-es és egy másik 4 másodperc. 
 - Az alapértelmezett Linux beállítási újrapróbálkozások 5 másodperc után.  Ez az újbóli próbálkozáshoz 5 megszorzása időközönként 1 második ajánlott.  

Az aktuális beállítások a Linux virtuális, "macska /etc/resolv.conf" és a "beállítások" sor figyelmébe pl. ellenőrzése:

    options timeout:1 attempts:5

A resolv.conf fájl van általában automatikusan létrehozott, és nem szerkeszthetők.  Az adott lépések leírását a "beállítások" sor distro függően eltérőek:

- **Ubuntu** (resolvconf használ):
    - a beállítások sor hozzáadása "/ etc/resolveconf/resolv.conf.d/head" 
    - Futtassa a resolvconf -u frissítése
- **SUSE** (netconf használ):
    - "timeout:1 kísérletek: 5" hozzáadása a NETCONFIG_DNS_RESOLVER_OPTIONS = "" a "/ config stb/sysconfig/hálózati" paraméter 
    - Futtassa a "frissítés netconfig" frissítése
- **OpenLogic** (NetworkManager használ):
    - "a"beállítások timeout:1 kísérletek: 5"visszhang" hozzáadása "/ etc/NetworkManager/dispatcher.d/11-dhclient" 
    - Futtassa a "hálózati újraindítás szolgáltatás" frissítése

## <a name="name-resolution-using-your-own-dns-server"></a>A saját DNS-kiszolgálót használ névfeloldás
Számos olyan helyzeteket mutatnak Ha igényeinek megfelelően neve felbontás előfordulhat, hogy lépjen túl az által biztosított szolgáltatások Azure, például amikor használatával Active Directory – tartományok vagy szüksége a DNS-feloldási virtuális hálózatok (vnets) között.  Hogy fedezze fel az alábbiakat Azure lehetővé teszi a segítségével saját DNS-kiszolgálók.  

DNS-kiszolgálók belül egy virtuális hálózati DNS-lekérdezések továbbíthat Azure-féle rekurzív névfeloldók úgy, hogy virtuális hálózaton belül állomásnevekké.  Egy tartomány vezérlő (Adatközpont) Azure-ban futó például a tartományok DNS-lekérdezések megválaszolása és minden más lekérdezés továbbítása a Azure.  Ezzel a helyszíni erőforrások (keresztül az Adatközpont) és az Azure által biztosított állomásnevekké (keresztül a továbbító) VMs.  Azure-féle rekurzív névfeloldók a hozzáférést a virtuális IP 168.63.129.16 keresztül kapja.

DNS-továbbítás is lehetővé teszi, hogy a többek vnet DNS-feloldási, és lehetővé teszi, hogy a helyszíni gépek Azure által biztosított állomásnevekké feloldásához.  Kiküszöbölése érdekében egy virtuális hostname (állomásnév), a DNS-kiszolgáló virtuális kell lennie, virtuális ugyanabba a hálózatba, és beállítania, hogy előre hostname (állomásnév) lekérdezéseket Azure.  A DNS-utótag eltér az egyes vnet, mint a feltételes továbbítási szabályok használatával küld a megfelelő vnet vonatkozó felbontásban DNS-lekérdezéseket.  Az alábbi képen az látható, két vnets és az ezzel a módszerrel többek vnet DNS-feloldási módon helyszíni hálózaton.  Egy példa DNS-továbbító az [Azure quickstart útmutató sablongyűjteményben](https://azure.microsoft.com/documentation/templates/301-dns-forwarder/) és [GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/301-dns-forwarder)érhető el.

![Többek vnet DNS](./media/virtual-networks-name-resolution-for-vms-and-role-instances/inter-vnet-dns.png)

Azure által biztosított névfeloldás, egy belső DNS-utótag használata esetén (*. internal.cloudapp.net) kapja meg az egyes virtuális DHCP használatával.  A hostname (állomásnév) rekordok vannak a internal.cloudapp.net zónában hostname (állomásnév) felbontás lehetővé teszi.  Amikor a saját neve felbontás megoldást, a IDN utótag nem adják át VMs, mert azt zavarja más DNS architektúrákban (például a tartományhoz tartozó esetek).  Csatlakozik kínálunk, amely nem működő helyőrző (reddog.microsoft.com).  

Ha szükséges, a belső DNS-utótag meghatározható PowerShell alrendszerrel vagy a API:

-  Az erőforrás-kezelő telepítési modellekben virtuális hálózatok a képz_jel nem érhető el, a [hálózati kártya](https://msdn.microsoft.com/library/azure/mt163668.aspx) forráson keresztül, vagy a [Get-AzureRmNetworkInterface](https://msdn.microsoft.com/library/mt619434.aspx) parancsmag keresztül.    
-  A klasszikus telepítési modellek, a képz_jel keresztül az [Első telepítési API-](https://msdn.microsoft.com/library/azure/ee460804.aspx) hívás vagy keresztül érhető el a [Get-AzureVM-hibakeresési](https://msdn.microsoft.com/library/azure/dn495236.aspx) parancsmag.


Azure továbbít lekérdezések nem felel meg igényeinek, ha szüksége lesz a saját DNS-megoldást nyújtanak.  A DNS-megoldás lesz szüksége:

-  Adja meg a megfelelő hostname (állomásnév) felbontás, például [DDNS](virtual-networks-name-resolution-ddns.md)keresztül.  Figyelje meg, ha szüksége lehet letiltani a DNS record takarítás Azure-féle DHCP bérletek vannak túl hosszú és törlési DDNS használatával eltávolíthatja a DNS-rekordok idő. 
-  Adja meg a megfelelő rekurziót engedélyezése a külső tartománynevek felbontását.
-  Könnyen kezelhető (TCP- és UDP-port 53) szolgál az ügyfelek és fér hozzá az internethez.
-  Az internetről külső ügynökök által felvetett kockázatok csökkentésében lehet access ellen védett.

> [AZURE.NOTE] A legjobb teljesítmény elérése érdekében Azure VMs, a DNS-kiszolgálók használatakor az IPv6 le lesznek tiltva, és egy [Példány szintű nyilvános IP](virtual-networks-instance-level-public-ip.md) egyes DNS-kiszolgálók virtuális kell rendelni.  Ha úgy dönt, hogy a Windows Server használni a DNS-kiszolgálót, [Ebben](http://blogs.technet.com/b/networking/archive/2015/08/19/name-resolution-performance-of-a-recursive-windows-dns-server-2012-r2.aspx) a cikkben további teljesítmény elemzése és optimalizálásokat.


### <a name="specifying-dns-servers"></a>A DNS-kiszolgálók megadása

Amikor a saját DNS-kiszolgálók, az Azure lehetővé teszi a több DNS-kiszolgálók virtuális hálózati vagy egy hálózati kapcsolat (erőforrás-kezelő), és a felhő szolgáltatás (klasszikus).  DNS-kiszolgálók egy felhőalapú szolgáltatás/hálózati kapcsolat megadott elsőbbséget élveznek a virtuális hálózat megadott beolvasása

> [AZURE.NOTE] A hálózati kapcsolat tulajdonságai parancsra, például a DNS-kiszolgáló IP-címei, érdemes nem lehet szerkeszteni közvetlenül elérhető Windows VMs, azok is első törlődik szolgáltatás során javítandó, amikor a virtuális hálózati adapteren helyettesíti. 


Az erőforrás-kezelő telepítési modell használata esetén a DNS-kiszolgálók a portálon API/sablonok ([vnet](https://msdn.microsoft.com/library/azure/mt163661.aspx), [hálózati kártya](https://msdn.microsoft.com/library/azure/mt163668.aspx)) vagy a PowerShell ([vnet](https://msdn.microsoft.com/library/mt603657.aspx), [hálózati kártya](https://msdn.microsoft.com/library/mt619370.aspx)) adható meg.

A klasszikus telepítési modell használata esetén a virtuális hálózati DNS-kiszolgálóiról a portálon vagy [a *Hálózati konfigurálásról* fájl](https://msdn.microsoft.com/library/azure/jj157100)adható meg.  Cloud Services a DNS-kiszolgálók van megadva, a PowerShellben ([Új-AzureVM](https://msdn.microsoft.com/library/azure/dn495254.aspx)), vagy [a *Szolgáltatás konfigurációs* fájl](https://msdn.microsoft.com/library/azure/ee758710) keresztül.

> [AZURE.NOTE] Ha egy virtuális hálózati/virtuális a számítógépen, amely már telepítve van a DNS-beállítások módosításához meg kell minden érintett virtuális a módosítások érvénybelépéséhez indítsa újra.


## <a name="next-steps"></a>Következő lépések

Erőforrás-kezelő telepítési modellje:

- [A virtuális hálózat létrehozása vagy módosítása](https://msdn.microsoft.com/library/azure/mt163661.aspx)
- [Létrehozása vagy módosítása egy hálózati kártya](https://msdn.microsoft.com/library/azure/mt163668.aspx)
- [Új AzureRmVirtualNetwork](https://msdn.microsoft.com/library/mt603657.aspx)
- [Új AzureRmNetworkInterface](https://msdn.microsoft.com/library/mt619370.aspx)

 
Klasszikus telepítési modellje:

- [Azure szolgáltatás konfigurációs séma](https://msdn.microsoft.com/library/azure/ee758710)
- [Virtuális hálózati konfigurációja séma](https://msdn.microsoft.com/library/azure/jj157100)
- [Virtuális hálózat konfigurálása hálózati konfigurációs fájl használatával](virtual-networks-using-network-configuration-file.md) 

