<properties
   pageTitle="Az Azure virtuális Magánhálózati átjáró visszaállítása |} Microsoft Azure"
   description="Ez a cikk végigvezeti az Azure virtuális Magánhálózati átjáró alaphelyzetbe állítása. A cikk a klasszikus és az erőforrás-kezelő telepítési modelljei VPN átjárók vonatkozik."
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager,azure-service-management"/>

<tags
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/23/2016"
   ms.author="cherylmc"/>

# <a name="reset-an-azure-vpn-gateway-using-powershell"></a>Állítsa alaphelyzetbe az Azure virtuális Magánhálózati átjáró PowerShell használatával


Ez a cikk végigvezeti alaphelyzetbe állítása az Azure virtuális Magánhálózati átjáró PowerShell-parancsmagok használata. Ezeket az utasításokat a klasszikus telepítési modell, mind az erőforrás-kezelő telepítési modell tartalmazzák.

Alaphelyzetbe állítása az Azure virtuális Magánhálózati átjáró akkor hasznos, ha határokon helyszíni virtuális Magánhálózati kapcsolatot egy vagy több S2S VPN bújtatás a elvesztése. Ebben az esetben a helyszíni virtuális Magánhálózati eszközök minden megfelelően működik, de nem jelennek meg az Azure virtuális Magánhálózati átjáró IPsec alagutak létrehozására. 

Minden egyes Azure virtuális Magánhálózati átjáró két virtuális példánya fut az aktív készenléti konfigurációban tevődik össze. A PowerShell-parancsmag használatakor az átjáró visszaállítása az átjáró újraindul, és ezután újra alkalmazza az idegen helyszíni konfigurációk azt. Az átjáró továbbra is a nyilvános IP-címet is. Ez azt jelenti, hogy módosítania kell a virtuális Magánhálózati útválasztó konfigurációja Azure virtuális Magánhálózati átjáró új nyilvános IP-cím nem.  

Miután a parancs kibocsátott, az aktuális aktív példány az Azure virtuális Magánhálózati átjáró azonnal újra. Lesz egy rövid távolság során az készenléti példány áttérni (éppen újraindítása után), az aktív példányból. A térköz kisebb, mint egy perc kell lennie.

Ha az első újraindítása után nem visszaállítani a kapcsolatot, ki ismét a virtuális másodfokú (az új aktív átjáró) indítsa újra a parancs ugyanaz. Ha a két újraindítása elejétől van szükség, lesznek kissé hosszabb ideig hol vannak éppen indítani mindkét virtuális példányok (aktív és készenléti). Ennek hatására a hosszabb távolság a virtuális Magánhálózati kapcsolatot, legfeljebb 2 és 4 perc VMs a újraindítása befejezéséhez az.

Után két újraindul Ha még mindig határokon helyszíni csatlakozási problémákat tapasztal, nyissa meg egy támogatási kérelmet az Azure portálról.

## <a name="before-you-begin"></a>Első lépések

Az átjáró új, mielőtt ellenőrizze az egyes IPsec-webhelyek (S2S) VPN-csatorna alábbi kulcsfontosságú elemeket. Bármely az elemeket nem egyezik a bontása az S2S VPN bújtatás eredményezi. Ellenőrzése és javítása a helyszíni és Azure virtuális Magánhálózati átjárók konfigurációit takaríthat meg a szükségtelen újraindítása és az átjárók a többi munka kapcsolatok megszakadása.

Ellenőrizze az alábbiakat előtt, hogy az átjáró visszaállítása:

- Az Internet IP-címei (VIP) az Azure virtuális Magánhálózati átjáró és a helyszíni VPN átjáró megfelelően beállítva az Azure és a helyszíni VPN házirendeket.
- Az előre megosztott kulcsnak kell lennie az Azure és a helyszíni VPN átjárók azonos.
- Ha két adott IPsec/IKE konfigurációja, például titkosítás, algoritmusok, és a beállítás (teljes továbbítási titkosítása), a kivonatolás gondoskodhat arról az Azure és a helyszíni VPN átjárók az azonos konfigurációk.

## <a name="reset-a-vpn-gateway-using-the-resource-management-deployment-model"></a>Az erőforrás-kezelés telepítési modell használatáról virtuális Magánhálózati átjáró visszaállítása

A PowerShell erőforrás-kezelő parancsmagok az átjáró visszaállítása `Reset-AzureRmVirtualNetworkGateway`. Az alábbi példa az Azure virtuális Magánhálózati átjáró, "VNet1GW", "TestRG1" erőforráscsoport alaphelyzetbe állítása.

    $gw = Get-AzureRmVirtualNetworkGateway -Name VNet1GW -ResourceGroup TestRG1
    Reset-AzureRmVirtualNetworkGateway -VirtualNetworkGateway $gw

## <a name="reset-a-vpn-gateway-using-the-classic-deployment-model"></a>A klasszikus telepítési modell segítségével virtuális Magánhálózati átjáró visszaállítása

A PowerShell-parancsmag alaphelyzetbe állítása az Azure virtuális Magánhálózati átjáró van `Reset-AzureVNetGateway`. Az alábbi példa az Azure virtuális Magánhálózati átjáró neve "ContosoVNet" virtuális hálózatok alaphelyzetbe állítása.
 
    Reset-AzureVNetGateway –VnetName “ContosoVNet” 

Eredmény:

    Error          :
    HttpStatusCode : OK
    Id             : f1600632-c819-4b2f-ac0e-f4126bec1ff8
    Status         : Successful
    RequestId      : 9ca273de2c4d01e986480ce1ffa4d6d9
    StatusCode     : OK


## <a name="next-steps"></a>Következő lépések
    
Lásd: a [PowerShell Szolgáltatáskezelés parancsmagjai – referencia](https://msdn.microsoft.com/library/azure/mt617104.aspx) és a [PowerShell erőforrás-kezelő parancsmagjai – referencia](http://go.microsoft.com/fwlink/?LinkId=828732) további információt.






