<properties
   pageTitle="Kezelése Access (-szabályozási listák) végpontok PowerShell használatával"
   description="Megtudhatja, hogy miként kezelheti a hozzáférés-vezérlési listák szolgáltatást a PowerShell használatával"
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
   ms.date="03/15/2016"
   ms.author="jdial" />

# <a name="how-to-manage-access-control-lists-acls-for-endpoints-by-using-powershell"></a>Kezelése Access (-szabályozási listák) végpontok PowerShell használatával

Létrehozhat és kezelheti a hálózati hozzáférés (-szabályozási listák) végpontok Azure PowerShell használatával vagy az adatkezelési portálon. Ebben a témakörben megtalálja eljárások vezérlés gyakori műveletek végrehajtása a PowerShell használatával. Azure PowerShell-parancsmagok listája látható az [Azure-kezelő parancsmagok](http://go.microsoft.com/fwlink/?LinkId=317721). A hozzáférés-vezérlési listák kapcsolatos további tudnivalókért lásd: [egy hálózati hozzáférés vezérlő lista (vezérlés) tartalma?](virtual-networks-acl.md). Ha szeretne a hozzáférés-vezérlési listák kezelése az adatkezelési portál használatával, [állítsa be végpontok virtuális géphez](../virtual-machines/virtual-machines-windows-classic-setup-endpoints.md)tájékozódhat.

## <a name="manage-network-acls-by-using-azure-powershell"></a>Hálózati hozzáférés-vezérlési listák kezelése Azure PowerShell használatával

Azure PowerShell-parancsmagok segítségével létrehozása, törlése és beállítása (set) hálózati hozzáférés (-szabályozási listák). Néhány példa néhány módszert-vezérlés PowerShell használatával adhatja azt szerepel.

Történő vezérlés PowerShell-parancsmagok listáját, használhatja a következők valamelyikét:

    Get-Help *AzureACL*
    Get-Command -Noun AzureACLConfig

### <a name="create-a-network-acl-with-rules-that-permit-access-from-a-remote-subnet"></a>Hozzon létre egy hálózati vezérlés engedélyezze a hozzáférést a távoli alhálózat szabályokkal

Az alábbi példán hozzon létre egy új listát tartalmazó szabályok lehetőséget. Ez a vezérlés majd érvényes egy virtuális gép végpontot. Az alábbi példában a vezérlés szabályokat lehetővé teszi a távoli alhálózat hozzáférést. Hozzon létre egy új hálózati vezérlés egy távoli alhálózat engedély szabályokkal, nyissa meg az Azure PowerShell ISE. Másolja és illessze be a parancsfájlt alatt, a parancsfájl konfigurálása a saját értékű, és futtassa a parancsfájl.

1. Az új hálózati vezérlés objektum létrehozása

        $acl1 = New-AzureAclConfig

1. Egy szabályt, amely lehetővé teszi az access egy távoli alhálózat beállítása. Az alábbi példában a szabály *100* (amely a elsőbbséget élveznek szabály 200 vagy újabb) a virtuális gép végpont távoli alhálózat *10.0.0.0/8* hozzáférést beállíthatja. Az értékek lecserélése saját konfigurációt igényeinek. A rövid nevet, hívja fel a szabályt, amelyet a "SharePoint-vezérlés config" nevét kell cserélni.

        Set-AzureAclConfig –AddRule –ACL $acl1 –Order 100 `
            –Action permit –RemoteSubnet "10.0.0.0/8" `
            –Description "SharePoint ACL config"

1. További szabályok ismételje meg az értékek cseréje a saját konfigurációs követelmények parancsmag. Ne felejtse el módosítani a szabály szám sorrendben, amelyben a szabályokat alkalmazza a kívánt sorrendjének megfelelően. A szabály évekként bontásakor a nagyobb szám.

        Set-AzureAclConfig –AddRule –ACL $acl1 –Order 200 `
            –Action permit –RemoteSubnet "157.0.0.0/8" `
            –Description "web frontend ACL config"

1. Ezután hozzon létre egy új végpontot (Hozzáadás) vagy a hozzáférés-Vezérlési meglévő zárólap (Set) beállítása. Ebben a példában azt fogja "web" nevű új virtuális gép végpont hozzáadása és frissítése a virtuális gép végpont vezérlés beállításokkal.

        Get-AzureVM –ServiceName $serviceName –Name $vmName `
        | Add-AzureEndpoint –Name "web" –Protocol tcp –Localport 80 - PublicPort 80 –ACL $acl1 `
        | Update-AzureVM

1. Ezután a parancsmagok kombinálása, és futtassa a. Ebben a példában a kombinált parancsmagok így néz ki:

        $acl1 = New-AzureAclConfig
        Set-AzureAclConfig –AddRule –ACL $acl1 –Order 100 `
            –Action permit –RemoteSubnet "10.0.0.0/8" `
            –Description "Sharepoint ACL config"
        Set-AzureAclConfig –AddRule –ACL $acl1 –Order 200 `
            –Action permit –RemoteSubnet "157.0.0.0/8" `
            –Description "web frontend ACL config"
        Get-AzureVM –ServiceName $serviceName –Name $vmName `
        |Add-AzureEndpoint –Name "web" –Protocol tcp –Localport 80 - PublicPort 80 –ACL $acl1 `
        |Update-AzureVM

### <a name="remove-a-network-acl-rule-that-permits-access-from-a-remote-subnet"></a>Olyan hálózati vezérlés szabályt, amely lehetővé teszi az access egy távoli alhálózat eltávolítása

Az alábbi példán hálózati vezérlés szabály eltávolítása lehetőséget.  A távoli alhálózat engedély szabályokkal hálózati vezérlés szabály kikapcsolásához nyissa meg az Azure PowerShell ISE. Másolja és illessze be a parancsfájlt alatt, a parancsfájl konfigurálása a saját értékű, és futtassa a parancsfájl.

1. Első lépésként a hálózati vezérlés objektum beszerzése a virtuális gép végpontot. Ezután a vezérlés szabály fogja eltávolíthatja. Ebben az esetben azt is eltávolítása a azt szabály azonosítója. A szabály azonosítója 0 csak eltávolítása a vezérlés. A vezérlés objektum nem távolítja el a virtuális gép végpontot.

        Get-AzureVM –ServiceName $serviceName –Name $vmName `
        | Get-AzureAclConfig –EndpointName "web" `
        | Set-AzureAclConfig –RemoveRule –ID 0 –ACL $acl1

1. Ezután kell a hálózati vezérlés objektum alkalmazása a virtuális gép végpontot, és frissítse a virtuális gépen.

        Get-AzureVM –ServiceName $serviceName –Name $vmName `
        | Set-AzureEndpoint –ACL $acl1 –Name "web" `
        | Update-AzureVM

### <a name="remove-a-network-acl-from-a-virtual-machine-endpoint"></a>A hálózati vezérlés eltávolítása egy virtuális gép végpontot

Egyes esetekben célszerű, ha el szeretne távolítani egy hálózati vezérlés objektum egy virtuális gép végpontot. Ehhez nyissa meg az Azure PowerShell ISE. Másolja és illessze be a parancsfájlt alatt, a parancsfájl konfigurálása a saját értékű, és futtassa a parancsfájl.

        Get-AzureVM –ServiceName $serviceName –Name $vmName `
        | Remove-AzureAclConfig –EndpointName "web" `
        | Update-AzureVM

## <a name="next-steps"></a>Következő lépések

[Mi az, hogy egy hálózati hozzáférés vezérlő lista (vezérlés)?](virtual-networks-acl.md)
