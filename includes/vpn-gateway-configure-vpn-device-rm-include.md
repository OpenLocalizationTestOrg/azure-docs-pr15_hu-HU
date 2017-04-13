
Állítsa be a VPN eszköz, szüksége lesz a nyilvános IP-címet a virtuális hálózati átjáró a helyszíni VPN eszköz konfigurációs. Bizonyos beállítások adatait a számítógépgyártó működnek, és állítsa be az eszközt. Olvassa el a [Virtuális Magánhálózati eszközök](../articles/vpn-gateway/vpn-gateway-about-vpn-devices.md) jól használható Azure virtuális Magánhálózati eszközök további információt.

A nyilvános IP-címet a PowerShell használatá virtuális hálózati átjáró megkereséséhez használja a következő példa:

    Get-AzureRmPublicIpAddress -Name GW1PublicIP -ResourceGroupName TestRG

Is megtekinthetők a nyilvános IP-címet a virtuális hálózati átjáró a Azure portál használatával Nyissa meg azt **a virtuális hálózati átjárók**, majd kattintson az átjáró nevére.