- A virtuális hálózatok lehet ugyanazon vagy másik Azure régióban (helyek).

- Egy felhőalapú szolgáltatásba vagy egy terheléselosztási végpont virtuális hálózatokon nem lehet akkor is, ha a közös csatlakozik.

- Azure virtuális hálózatok összevonása összekapcsolása nincs szükség bármely helyszíni VPN átjárók, kivéve, ha határokon helyszíni kapcsolat szükséges.

- VNet-VNet összekötő virtuális hálózatok támogatja. Összekötő virtuális gépeken futó támogatja, vagy nem felhőszolgáltatásokba nem a virtuális hálózat.

- VNet-VNet igényel Azure virtuális Magánhálózati átjárók RouteBased (korábbi nevén dinamikus útválasztás) a VPN-típusokat. 

- Virtuális a hálózati kapcsolat kínál egyszerre több webhelyen VPN adatai és legfeljebb 10 (alapértelmezett/Standard átjárók), illetve 30 (nagy teljesítmény átjárók) VPN bújtatja vagy más virtuális hálózatok kapcsolódás virtuális hálózati VPN átjáró a helyszíni webhelyek.

- A cím szóközöket virtuális hálózatok és a helyszíni helyi hálózati helyek a kell nem áll átfedésben. Egymást átfedő cím szóközöket okoz meghiúsító VNet-VNet kapcsolatok létrehozását.

- Felesleges alagutak két virtuális hálózatok között nem támogatottak.

- A virtuális hálózat minden VPN bújtatás ossza meg az elérhető sávszélesség az Azure virtuális Magánhálózati átjáró és ugyanazon VPN átjáró felmérést SLA Azure-ban.

- VNet-VNet forgalmat a Microsoft Network, nem az interneten keresztül létesített.

- Az azonos régión belüli VNet-VNet forgalom ingyenesen mindkét irányba; tartományok régió VNet-VNet kilépési forgalom terhelve a forrás régiók alapján kimenő többek VNet adatátviteli. Nézze meg az [oldal árak](https://azure.microsoft.com/pricing/details/vpn-gateway/) további információt.