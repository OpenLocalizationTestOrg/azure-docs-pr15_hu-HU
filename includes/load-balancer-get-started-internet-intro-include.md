Az Azure terheléselosztó egy réteg-4 (TCP, UDP) terheléselosztó. A terheléselosztó magas színvonalú elérhetőséget biztosít azáltal, hogy bejövő forgalmat a felhő szolgáltatások egészséges szolgáltatás példányainak vagy load balancer meg virtuális gépek között. Borzas terheléselosztó is tudja mutatni azokat a szolgáltatásokat több porton vagy több IP-címet.

Hogy egy terheléselosztó állíthatja be:

* Egyenleg bejövő internetes forgalmat a virtuális gépek (VMs) betöltése. Ebben az esetben egy terheléselosztó megnevezése, az [internetre terheléselosztó](../articles/load-balancer/load-balancer-internet-overview.md).
* Terhelés egyenleg forgalom VMs (VNet) virtuális hálózatok közötti, VMs a felhő szolgáltatások vagy helyszíni számítógépek és a VMS-rendszer több helyi virtuális hálózat között. Egy [belső terheléselosztó (ILB)](../articles/load-balancer/load-balancer-internal-overview.md), ebben az esetben a terheléselosztóhoz irányítjuk.
* Egy adott példányra VM külső forgalom továbbítására.
