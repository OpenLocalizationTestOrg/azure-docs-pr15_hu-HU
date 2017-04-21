##<a name="tcp-settings-for-azure-vms"></a>Azure VMs TCP beállításai

Azure VMs [hálózati Címfordítást] használatával kommunikál a nyilvános Internet[ nat] (hálózati címfordítást). Hálózati Címfordítást eszközök hozzárendelése a nyilvános IP-cím és a port az Azure virtuális lehetővé teszi, hogy virtuális kommunikációs szoftvercsatornát más eszközökre való létrehozásához. Csomagok állítani egy megadott idő után, hogy szoftvercsatorna keresztül lépés, ha a hálózati Címfordítást eszköz űrlapokat állítjuk leáll a leképezést, és a szoftvercsatorna ingyenes egyéb VMs való használatra.

Ez a közös hálózati Címfordítást működés, amely kommunikációs problémákat okozhatják, amely túl az időtúllépés fenntartására szoftvercsatornát várt alapú TCP-alkalmazások. Vannak is célszerű figyelembe venni a *kapcsolatot* állapotban folyamatokhoz két üresjárati időkorlát-beállítások:

- **bejövő** keresztül [Azure terheléselosztó][azure-lb-timeout]. Az időtúllépés 4 perc az alapértelmezett, és a 30 percig mentése lehet beállítani.
- **kimenő** [SNAT] használatával[ snat] (forrás hálózati Címfordítást). Az időtúllépés 4 perc van beállítva, és nem állítható be.

Annak érdekében, hogy kapcsolatok nem vész el a időkorlátját túl, győződjön meg arról, hogy az alkalmazás megőrzi a munkamenet életben, vagy beállíthatja, hogy a mögöttes operációs rendszer erre. A beállítások használandó eltérőek Linux és a Windows rendszerhez, alább látható módon.

A [Linux][linux], módosítania kell az alábbi kernelváltozók.
NET.IPv4.tcp_keepalive_time 120 net.ipv4.tcp_keepalive_intvl = 30 net.ipv4.tcp_keepalive_probes = = 8
 
A [Windows][windows], módosítania kell a beállításjegyzék az alábbi értékeket.
KeepAliveInterval = 30 KeepAliveTime 120 TcpMaxDataRetransmissions = = 8


A fenti beállítások megőrzése életben csomagot az üresjárati (120 másodperc) a 2 perc múlva küldi, és kattintson a 30 másodpercenként küldött biztosítása. És 8 azokat a csomagokat sikertelen, ha megszakad-e a munkamenetet.

<!-- links -->
[nat]: http://computer.howstuffworks.com/nat.htm
[snat]: ../load-balancer/load-balancer-overview.md/#source-nat
[linux]: http://tldp.org/HOWTO/TCP-Keepalive-HOWTO/usingkeepalive.html
[windows]: http://blogs.technet.com/b/nettracer/archive/2010/06/03/things-that-you-may-want-to-know-about-tcp-keepalives.aspx
[azure-lb-timeout]: ../load-balancer/load-balancer-tcp-idle-timeout.md