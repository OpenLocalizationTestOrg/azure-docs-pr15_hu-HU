<properties 
   pageTitle="Nyilvános IP-címek használatáról virtuális hálózatban"
   description="Megtudhatja, hogy miként egy virtuális hálózati nyilvános IP-címek konfigurálása"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn" />
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="04/27/2016"
   ms.author="jdial" />

# <a name="public-ip-address-space-in-a-virtual-network-vnet"></a>Nyilvános IP-címterület virtuális hálózatban (VNet)

Virtuális hálózatok (VNets) tartalmazhat nyilvános és magán (RFC 1918 címterületet) IP cím szóközöket. Amikor felvesz egy nyilvános IP-címtartományokat, akkor a magánjellegű VNet IP címterület használatára, amely csak a VNet, egymáshoz kapcsolódó VNets, és a helyszíni helyről elérhető részeként rendszer kezeli.

Az alábbi képen egy nyilvános és titkos IP-cím szóközöket tartalmazó VNet.

![Magyarázó rész nyilvános IP](./media/virtual-networks-public-ip-within-vnet/IC775683.jpg)

## <a name="how-do-i-add-a-public-ip-address-range"></a>Hogyan vehetek fel egy nyilvános IP-címtartományokat?

Egy nyilvános IP-címtartományokat vegyen fel egy személyes IP-címtartományokat; ugyanúgy hozzáadása bármelyik *netcfg* -fájl használatával, vagy az [Azure portált](http://portal.azure.com)a konfigurációs hozzáadásával. Egy nyilvános IP-címtartományokat is hozzáadhat, a VNet, vagy lépjen vissza, és utána vegye fel. Az alábbi példában látható nyilvános és magán IP cím szóközöket az azonos VNet beállítva.

![A portál nyilvános IP-cím](./media/virtual-networks-public-ip-within-vnet/IC775684.png)

## <a name="are-there-any-limitations"></a>Van bármiféle korlátozás vonatkozóan?

Létezik néhány IP-címtartományok, nem használhatók:

- 224.0.0.0/4 (csoportos)

- 255.255.255.255/32 (szórás)

- 127.0.0.0/8 (visszacsatolási)

- 169.254.0.0/16 (kapcsolaton)

- 168.63.129.16/32 (belső DNS)

## <a name="next-steps"></a>Következő lépések

[Egy VNet által használt DNS-kiszolgálók kezelése](virtual-networks-manage-dns-in-vnet.md)