<properties
    pageTitle="Az alkalmazás a virtuális hálózati csatlakozás PowerShell használatával"
    description="Megtudhatja, hogy miként csatlakozhat, és a virtuális hálózatok kezelése a PowerShell használatával"
    services="app-service"
    documentationCenter=""
    authors="ccompy"
    manager="wpickett"
    editor="cephalin"/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/29/2016"
    ms.author="ccompy"/>

# <a name="connect-your-app-to-your-virtual-network-by-using-powershell"></a>Az alkalmazás a virtuális hálózati csatlakozás PowerShell használatával #

## <a name="overview"></a>– Áttekintés ##

Azure App szolgáltatásban lehet csatlakozni az alkalmazás (webes, mobile vagy API-val) Azure virtuális hálózathoz (VNet) az előfizetést. Ez a szolgáltatás neve VNet integráció. A VNet integrációja funkció nem összekeverni az alkalmazás szolgáltatási környezetben funkciót, amely lehetővé teszi, hogy az Azure alkalmazás szolgáltatás egy példánya futtatható a virtuális hálózata.

A VNet integrációja funkció az új portál integrálása a klasszikus telepítési modell vagy az erőforrás-kezelő Azure telepítési modell telepített virtuális hálózatok használó felhasználói felületét tartalmaz. Ha azt szeretné, ha többet szeretne tudni a szolgáltatást, olvassa el a [Integrate Azure virtuális hálózattal az alkalmazás](web-sites-integrate-with-vnet.md).

Ez a témakör az nem használatáról a felhasználói felület, de inkább arról, hogy miként alkalmazással való integráció engedélyezése a PowerShell használatával. Minden telepítési modell szolgáló parancsok eltérőek, mert ez a témakör a szakaszba kerüljön az egyes telepítési modell tartalmazza.  

Ez a cikk a folytatás előtt győződjön meg arról, hogy:

- A legújabb Azure PowerShell SDK telepítve. A webes Platform telepítővel telepítheti.
- Az alkalmazás normál vagy prémium Termékváltozat futó Azure App szolgáltatásban.

## <a name="classic-virtual-networks"></a>Klasszikus virtuális hálózatok ##

Ebből a szakaszból, amelyek a klasszikus telepítési modell virtuális hálózatok három műveletet:

1. Csatlakozás egy korábban létrehozott virtuális hálózati az átjárók be van állítva az csatlakozási pontot a webhely-és az alkalmazás.
1. Az alkalmazás, a virtuális hálózati integrációs adatainak frissítése.
1. A virtuális hálózati kapcsolat bontása az alkalmazást.

### <a name="connect-an-app-to-a-classic-vnet"></a>Csatlakozás egy klasszikus VNet alkalmazás ###

Az alkalmazás egy virtuális hálózathoz csatlakozik, kövesse az alábbi három lépéseket:

1. Az elemeket rekordként a web App alkalmazásban, hogy azt fogja kell kapcsolódok be egy adott virtuális hálózaton. Az alkalmazás hoz létre egy tanúsítványt, a webhely pont-kapcsolatot a virtuális hálózati biztosítandó.
1. Töltse fel a web app-tanúsítványt a virtuális hálózat, és kattintson az a pont-webhely VPN csomag URI beolvasásához.
1. A webhely pont-csomaggal URI a webalkalmazás virtuális hálózati kapcsolat frissítése

Az első és harmadik lépéseket teljesen parancsfájlok segítségével, de a második lépés van szükség egy egyszeri, kézi művelet központon vagy az erőforrás-kezelő Azure virtuális hálózati végpont **ELHELYEZNI** , vagy **a javítás** műveleteket access keresztül. Azure ügyfélszolgálatát, hogy engedélyezve van a beállítás. Mielőtt nekikezdene, győződjön meg arról, hogy van-e klasszikus virtuális hálózatot, amelyen a webhely-pont kapcsolat már be van kapcsolva, és telepítve az átjárók. Az átjáró létrehozásához, és engedélyezze a csatlakozási pont-webhely, akkor használja a portálon a [virtuális Magánhálózati átjáró létrehozása]leírtak szerint[createvpngateway].

A klasszikus virtuális hálózat kell lennie, mint a alkalmazás díjcsomagjától, amely tartalmazza az alkalmazást, amely szolgáltatással való integrálásáról ugyanabban az előfizetésben.

##### <a name="set-up-azure-powershell-sdk"></a>Azure PowerShell SDK beállítása #####

Nyisson meg egy PowerShell-ablakot, és állítsa be az Azure-fiók és az előfizetés használatával:

    Login-AzureRmAccount

Ez a parancs nyílik meg egy kérdés a Azure hitelesítő adatok eléréséhez. Miután bejelentkezett, az alábbi parancsok egyikét használatával jelölje ki a használni kívánt előfizetést. Ellenőrizze, hogy az előfizetést, a virtuális hálózati és az alkalmazás szolgáltatáscsomagja kell használnia.

    Select-AzureRmSubscription –SubscriptionName [WebAppSubscriptionName]

vagy

    Select-AzureRmSubscription –SubscriptionId [WebAppSubscriptionId]

##### <a name="variables-used-in-this-article"></a>A jelen cikkben használt változók #####

Egyszerűsítése érdekében parancsokat, akkor állítja az adott beállításokkal **$Configuration** PowerShell változó.

Változó következőképpen beállítása a PowerShell az alábbi paraméterekkel:

    $Configuration = @{}
    $Configuration.WebAppResourceGroup = "[Your web app resource group]"
    $Configuration.WebAppName = "[Your web app name]"
    $Configuration.VnetSubscriptionId = "[Your vnet subscription id]"
    $Configuration.VnetResourceGroup = "[Your vnet resource group]"
    $Configuration.VnetName = "[Your vnet name]"

Az alkalmazás helyét a hely szóköz használata nélkül kell lennie. Például a nyugati USA-beli westus is.

    $Configuration.WebAppLocation = "[Your web app Location]"

A következő elem, hogy hol kell írni a tanúsítvány. A helyi számítógépen írható elérési kell tenni. Győződjön meg arról, ha meg szeretné jeleníteni a végén .cer.

    $Configuration.GeneratedCertificatePath = "[C:\Path\To\Certificate.cer]"

Ha szeretné látni, beállítása, írja be a **$Configuration**.

    > $Configuration

    Name                           Value
    ----                           -----
    GeneratedCertificatePath       C:\vnetcert.cer
    VnetSubscriptionId             efc239a4-88f9-2c5e-a9a1-3034c21ad496
    WebAppResourceGroup            vnetdemo-rg
    VnetResourceGroup              testase1-rg
    VnetName                       TestNetwork
    WebAppName                     vnetintdemoapp
    WebAppLocation                 centralus

Ebben a szakaszban a többi feltételezi, hogy rendelkezik-e egy változó létrehozott csak leírt módon.

##### <a name="declare-the-virtual-network-to-the-app"></a>Az alkalmazás a virtuális hálózat deklarálása #####

A következő paranccsal megállapítani, hogy az alkalmazást, hogy azt fogja használni a adott virtuális hálózathoz. Ennek hatására az alkalmazás szükséges tanúsítványok létrehozásához:

    $vnet = New-AzureRmResource -Name "$($Configuration.WebAppName)/$($Configuration.VnetName)" -ResourceGroupName $Configuration.WebAppResourceGroup -ResourceType "Microsoft.Web/sites/virtualNetworkConnections" -PropertyObject @{"VnetResourceId" = "/subscriptions/$($Configuration.VnetSubscriptionId)/resourceGroups/$($Configuration.VnetResourceGroup)/providers/Microsoft.ClassicNetwork/virtualNetworks/$($Configuration.VnetName)"} -Location $Configuration.WebAppLocation -ApiVersion 2015-07-01

Ha ezt a parancsot sikerült, **$vnet** benne egy **Tulajdonságok** változó kell tartalmaznia. A **Tulajdonságok** változó egy tanúsítványt ujjlenyomat és a tanúsítvány adatokat tartalmazhat.

##### <a name="upload-the-web-app-certificate-to-the-virtual-network"></a>A virtuális hálózat töltse fel a web app-tanúsítvány #####

A manuális, egyszeri lépés szükség, minden előfizetés és virtuális hálózati kombinációt. Ez azt jelenti, hogy ha A virtuális hálózati alkalmazásainak A előfizetés csatlakozik, meg kell végezze el ezt a lépést, csak egyszer, függetlenül attól, hogy hány alkalmazások beállítása. Ha egy másik virtuális hálózathoz hozzáadni új alkalmazás, kell kövesse az alábbi lépéseket ismét. Ennek az oka, hogy egy sor olyan tanúsítványok előfizetés szinten lévő Azure alkalmazás szolgáltatás jön létre, de megadása egyszer az egyes virtuális hálózatokhoz kapcsolódó alkalmazások jön létre.

A tanúsítványok lesz már már beállított Ha követte ezeket a lépéseket, vagy ha a portálon integrálva virtuális hálózatához.

Első lépésként az .cer fájl létrehozásához. A második lépésként a .cer fájl feltöltése a virtuális hálózathoz. Az előző lépésben API hívás a .cer fájl létrehozásához a következő parancsokat.

    $certBytes = [System.Convert]::FromBase64String($vnet.Properties.certBlob)
    [System.IO.File]::WriteAllBytes("$($Configuration.GeneratedCertificatePath)", $certBytes)

A tanúsítvány lesz található hely adott **$Configuration.GeneratedCertificatePath** Itt adhatja meg.

Manuális töltse fel a tanúsítvány, használja az [Azure portál] [ azureportal] és **Tallózás virtuális hálózati (klasszikus)** > **virtuális Magánhálózati kapcsolatot** > **pont webhely** > **kezelése tanúsítványok**. Innen kiindulva töltse fel a tanúsítvány.

##### <a name="get-the-point-to-site-package"></a>A webhely-pont csomag beszerzése #####

A következő lépés a webalkalmazást virtuális hálózati kapcsolatának beállítása, hogy a webhely-pont csomag első, és adja meg azt a web App alkalmazásban.

A következő sablon GetNetworkPackageUri.json valahol nevű a számítógépen, például C:\Azure\Templates\GetNetworkPackageUri.json fájlként való mentéséhez.

    {
        "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "certData": {
                "type": "string"
            },
            "certThumbprint": {
                "type": "string"
            },
            "networkName": {
                "type": "string"
            }
        },
        "variables": {
            "legacyVnetName": "[concat('Group ', resourceGroup().name, ' ', parameters('networkName'))]"
            },
            "resources": [
            ],
        "outputs" : {
            "PackageUri" :
            {
            "value" : "[listPackage(resourceId('Microsoft.ClassicNetwork/virtualNetworks/gateways/clientRootCertificates', parameters('networkName'), 'primary', parameters('certThumbprint')), '2014-06-01').packageUri]", "type" : "string"
            }
        }
    }


A bemeneti paramétereinek beállítása:

    $parameters = @{"certData" = $vnet.Properties.certBlob ;
    certThumbprint = $vnet.Properties.certThumbprint ;
    "networkName" = $Configuration.VnetName }

Hívja fel a parancsfájl:

    $output = New-AzureRmResourceGroupDeployment -Name unused -ResourceGroupName $Configuration.VnetResourceGroup -TemplateParameterObject $parameters -TemplateFile C:\PATH\TO\GetNetworkPackageUri.json


A változó **$output. Outputs.packageUri** most fogja tartalmazni a csomag URI kell adni a web App alkalmazásban.

##### <a name="upload-the-point-to-site-package-to-your-app"></a>Az alkalmazás a pont a webhely-csomag feltöltése #####

Az utolsó lépésként küldje el az alkalmazást a csomagban. Egyszerűen futtassa a következő parancsot:

    $vnet = New-AzureRmResource -Name "$($Configuration.WebAppName)/$($Configuration.VnetName)/primary" -ResourceGroupName $Configuration.WebAppResourceGroup -ResourceType "Microsoft.Web/sites/virtualNetworkConnections/gateways" -ApiVersion 2015-07-01 -PropertyObject @{"VnetName" = $Configuration.VnetName ; "VpnPackageUri" = $($output.Outputs.packageUri).Value } -Location $Configuration.WebAppLocation

Ha a rendszer megkérdezi, hogy erősítse meg az, hogy vannak-e felülírása meglévő erőforrás, feltétlenül engedélyezze azt.

Miután létrejött a parancs, az alkalmazás letöltése a virtuális hálózathoz kell csatlakoztatni. Erősítse meg a sikeres, nyissa meg az app konzol és írja be a következőt:

    SET WEBSITE_

Ha egy érték, amely megfelel a cél virtuális hálózat nevét tartalmazó WEBSITE_VNETNAME nevű környezeti változó, az összes konfiguráció sikeres volt.

### <a name="update-classic-vnet-integration-information"></a>Klasszikus VNet integrációs adatainak frissítése ###

Frissíteni, vagy a adatok újraszinkronizálási, egyszerűen ismételje meg a integrálása az elsőként létrehozásakor követett. Ezek a lépések a következők:

1. Adja meg a beállításokat.
1. Az alkalmazás a virtuális hálózat az elemeket rekordként.
1. Ismerkedés a webhely pont-csomagot.
1. Töltse fel az alkalmazást a webhely pont-csomagot.

### <a name="disconnect-your-app-from-a-classic-vnet"></a>Az alkalmazás a klasszikus VNet választani ###

Leválasztja az alkalmazást, információra van szüksége a konfigurációs virtuális hálózati integrációval során beállított. Használja ezeket az információkat, van majd egy parancs a virtuális hálózati kapcsolat bontása az alkalmazást.

    $vnet = Remove-AzureRmResource -Name "$($Configuration.WebAppName)/$($Configuration.VnetName)" -ResourceGroupName $Configuration.WebAppResourceGroup -ResourceType "Microsoft.Web/sites/virtualNetworkConnections" -ApiVersion 2015-07-01

## <a name="resource-manager-virtual-networks"></a>Erőforrás-kezelő virtuális hálózatok ##

Erőforrás-kezelő virtuális hálózatok Azure erőforrás-kezelő API-khoz van, amely egyszerűsítése érdekében az egyes folyamatok klasszikus virtuális hálózatok összehasonlítva. Van még egy parancsprogramot, amely segít a az alábbi feladatok elvégzésére:

- Hozzon létre egy erőforrás-kezelő virtuális hálózatot, és az alkalmazás integrálása.
- Átjáró létrehozása, állítsa be a csatlakozási pont-webhely egy korábban létrehozott erőforrás-kezelő virtuális hálózaton, és az alkalmazás integrálása.
- Az alkalmazás integrálása a korábban létrehozott erőforrás-kezelő virtuális hálózat tartalmaz egy átjáró és a pont-webhely kapcsolat engedélyezve van.
- A virtuális hálózati kapcsolat bontása az alkalmazást.

### <a name="resource-manager-vnet-app-service-integration-script"></a>Erőforrás-kezelő VNet alkalmazás szolgáltatás integrációs parancsfájl ###

Másolja a következő parancsokat, és mentse a fájlt. Ha nem szeretné a parancsfájlt, nyugodtan ismerje meg, megtudhatja, hogy miként beállítási egy erőforrás-kezelő virtuális hálózati belőle.


    function ReadHostWithDefault($message, $default)
    {
        $result = Read-Host "$message [$default]"
        if($result -eq "")
        {
            $result = $default
        }
            return $result
        }

    function PromptCustom($title, $optionValues, $optionDescriptions)
    {
        Write-Host $title
        Write-Host
        $a = @()
        for($i = 0; $i -lt $optionValues.Length; $i++){
            Write-Host "$($i+1))" $optionDescriptions[$i]
        }
        Write-Host

        while($true)
        {
            Write-Host "Choose an option: "
            $option = Read-Host
            $option = $option -as [int]

            if($option -ge 1 -and $option -le $optionValues.Length)
            {
                return $optionValues[$option-1]
            }
        }
    }

    function PromptYesNo($title, $message, $default = 0)
    {
        $yes = New-Object System.Management.Automation.Host.ChoiceDescription "&Yes", ""
        $no = New-Object System.Management.Automation.Host.ChoiceDescription "&No", ""
        $options = [System.Management.Automation.Host.ChoiceDescription[]]($yes, $no)
        $result = $host.ui.PromptForChoice($title, $message, $options, $default)
        return $result
    }

    function CreateVnet($resourceGroupName, $vnetName, $vnetAddressSpace, $vnetGatewayAddressSpace, $location)
    {
        Write-Host "Creating a new VNET"
        $gatewaySubnet = New-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -AddressPrefix $vnetGatewayAddressSpace
        New-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $resourceGroupName -Location $location -AddressPrefix $vnetAddressSpace -Subnet $gatewaySubnet
    }

    function CreateVnetGateway($resourceGroupName, $vnetName, $vnetIpName, $location, $vnetIpConfigName, $vnetGatewayName, $certificateData, $vnetPointToSiteAddressSpace)
    {
        $vnet = Get-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $resourceGroupName
        $subnet=Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet

        Write-Host "Creating a public IP address for this VNET"
        $pip = New-AzureRmPublicIpAddress -Name $vnetIpName -ResourceGroupName $resourceGroupName -Location $location -AllocationMethod Dynamic
        $ipconf = New-AzureRmVirtualNetworkGatewayIpConfig -Name $vnetIpConfigName -Subnet $subnet -PublicIpAddress $pip

        Write-Host "Adding a root certificate to this VNET"
        $root = New-AzureRmVpnClientRootCertificate -Name "AppServiceCertificate.cer" -PublicCertData $certificateData

        Write-Host "Creating Azure VNET Gateway. This may take up to an hour."
        New-AzureRmVirtualNetworkGateway -Name $vnetGatewayName -ResourceGroupName $resourceGroupName -Location $location -IpConfigurations $ipconf -GatewayType Vpn -VpnType RouteBased -EnableBgp $false -GatewaySku Basic -VpnClientAddressPool $vnetPointToSiteAddressSpace -VpnClientRootCertificates $root
    }

    function AddNewVnet($subscriptionId, $webAppResourceGroup, $webAppName)
    {
        Write-Host "Adding a new Vnet"
        Write-Host
        $vnetName = Read-Host "Specify a name for this Virtual Network"

        $vnetGatewayName="$($vnetName)-gateway"
        $vnetIpName="$($vnetName)-ip"
        $vnetIpConfigName="$($vnetName)-ipconf"

        # Virtual Network settings
        $vnetAddressSpace="10.0.0.0/8"
        $vnetGatewayAddressSpace="10.5.0.0/16"
        $vnetPointToSiteAddressSpace="172.16.0.0/16"

        $changeRequested = 0
        $resourceGroupName = $webAppResourceGroup

        while($changeRequested -eq 0)
        {
            Write-Host
            Write-Host "Currently, I will create a VNET with the following settings:"
            Write-Host
            Write-Host "Virtual Network Name: $vnetName"
            Write-Host "Resource Group Name:  $resourceGroupName"
            Write-Host "Gateway Name: $vnetGatewayName"
            Write-Host "Vnet IP name: $vnetIpName"
            Write-Host "Vnet IP config name:  $vnetIpConfigName"
            Write-Host "Address Space:$vnetAddressSpace"
            Write-Host "Gateway Address Space:$vnetGatewayAddressSpace"
            Write-Host "Point-To-Site Address Space:  $vnetPointToSiteAddressSpace"
            Write-Host
            $changeRequested = PromptYesNo "" "Do you wish to change these settings?" 1

            if($changeRequested -eq 0)
            {
                $vnetName = ReadHostWithDefault "Virtual Network Name" $vnetName
                $resourceGroupName = ReadHostWithDefault "Resource Group Name" $resourceGroupName
                $vnetGatewayName = ReadHostWithDefault "Vnet Gateway Name" $vnetGatewayName
                $vnetIpName = ReadHostWithDefault "Vnet IP name" $vnetIpName
                $vnetIpConfigName = ReadHostWithDefault "Vnet IP configuration name" $vnetIpConfigName
                $vnetAddressSpace = ReadHostWithDefault "Vnet Address Space" $vnetAddressSpace
                $vnetGatewayAddressSpace = ReadHostWithDefault "Vnet Gateway Address Space" $vnetGatewayAddressSpace
                $vnetPointToSiteAddressSpace = ReadHostWithDefault "Vnet Point-to-site Address Space" $vnetPointToSiteAddressSpace
            }
        }

        $ErrorActionPreference = "Stop";

        # We create the virtual network and add it here. The way this works is:
        # 1) Add the VNET association to the App. This allows the App to generate certificates, etc. for the VNET.
        # 2) Create the VNET and VNET gateway, add the certificates, create the public IP, etc., required for the gateway
        # 3) Get the VPN package from the gateway and pass it back to the App.

        $webApp = Get-AzureRmResource -ResourceName $webAppName -ResourceType "Microsoft.Web/sites" -ApiVersion 2015-08-01 -ResourceGroupName $webAppResourceGroup
        $location = $webApp.Location

        Write-Host "Creating App association to VNET"
        $propertiesObject = @{
         "vnetResourceId" = "/subscriptions/$($subscriptionId)/resourceGroups/$($resourceGroupName)/providers/Microsoft.Network/virtualNetworks/$($vnetName)"
        }
        $virtualNetwork = New-AzureRmResource -Location $location -Properties $PropertiesObject -ResourceName "$($webAppName)/$($vnetName)" -ResourceType "Microsoft.Web/sites/virtualNetworkConnections" -ApiVersion 2015-08-01 -ResourceGroupName $webAppResourceGroup -Force

        CreateVnet $resourceGroupName $vnetName $vnetAddressSpace $vnetGatewayAddressSpace $location

        CreateVnetGateway $resourceGroupName $vnetName $vnetIpName $location $vnetIpConfigName $vnetGatewayName $virtualNetwork.Properties.CertBlob $vnetPointToSiteAddressSpace

        Write-Host "Retrieving VPN Package and supplying to App"
        $packageUri = Get-AzureRmVpnClientPackage -ResourceGroupName $resourceGroupName -VirtualNetworkGatewayName $vnetGatewayName -ProcessorArchitecture Amd64

        # Put the VPN client configuration package onto the App
        $PropertiesObject = @{
        "vnetName" = $VirtualNetworkName; "vpnPackageUri" = $packageUri
        }

        New-AzureRmResource -Location $location -Properties $PropertiesObject -ResourceName "$($webAppName)/$($vnetName)/primary" -ResourceType "Microsoft.Web/sites/virtualNetworkConnections/gateways" -ApiVersion 2015-08-01 -ResourceGroupName $webAppResourceGroup -Force

        Write-Host "Finished!"
    }

    function AddExistingVnet($subscriptionId, $resourceGroupName, $webAppName)
    {
        $ErrorActionPreference = "Stop";

        # At this point, the gateway should be able to be joined to an App, but may require some minor tweaking. We will declare to the App now to use this VNET
        Write-Host "Getting App information"
        $webApp = Get-AzureRmResource -ResourceName $webAppName -ResourceType "Microsoft.Web/sites" -ApiVersion 2015-08-01 -ResourceGroupName $resourceGroupName
        $location = $webApp.Location

        $webAppConfig = Get-AzureRmResource -ResourceName "$($webAppName)/web" -ResourceType "Microsoft.Web/sites/config" -ApiVersion 2015-08-01 -ResourceGroupName $resourceGroupName
        $currentVnet = $webAppConfig.Properties.VnetName
        if($currentVnet -ne $null -and $currentVnet -ne "")
        {
            Write-Host "Currently connected to VNET $currentVnet"
        }

        # Display existing vnets
        $vnets = Get-AzureRmVirtualNetwork
        $vnetNames = @()
        foreach($vnet in $vnets)
        {
            $vnetNames += $vnet.Name
        }

        Write-Host
        $vnet = PromptCustom "Select a VNET to integrate with" $vnets $vnetNames

        # We need to check if this VNET is able to be joined to a App, based on following criteria
            # If there is no gateway, we can create one.
            # If there is a gateway:
                # It must be of type Vpn
                # It must be of VpnType RouteBased
                # If it doesn't have the right certificate, we will need to add it.
                # If it doesn't have a point-to-site range, we will need to add it.

        $gatewaySubnet = $vnet.Subnets | Where-Object { $_.Name -eq "GatewaySubnet" }

        if($gatewaySubnet -eq $null -or $gatewaySubnet.IpConfigurations -eq $null -or $gatewaySubnet.IpConfigurations.Count -eq 0)
        {
            $ErrorActionPreference = "Continue";
            # There is no gateway. We need to create one.
            Write-Host "This Virtual Network has no gateway. I will need to create one."

            $vnetName = $vnet.Name
            $vnetGatewayName="$($vnetName)-gateway"
            $vnetIpName="$($vnetName)-ip"
            $vnetIpConfigName="$($vnetName)-ipconf"

            # Virtual Network settings
            $vnetAddressSpace="10.0.0.0/8"
            $vnetGatewayAddressSpace="10.5.0.0/16"
            $vnetPointToSiteAddressSpace="172.16.0.0/16"

            $changeRequested = 0

            Write-Host "Your VNET is in the address space $($vnet.AddressSpace.AddressPrefixes), with the following Subnets:"
            foreach($subnet in $vnet.Subnets)
            {
                Write-Host "$($subnet.Name): $($subnet.AddressPrefix)"
            }

            $vnetGatewayAddressSpace = Read-Host "Please choose a GatewaySubnet address space"

            while($changeRequested -eq 0)
            {
                Write-Host
                Write-Host "Currently, I will create a VNET gateway with the following settings:"
                Write-Host
                Write-Host "Virtual Network Name: $vnetName"
                Write-Host "Resource Group Name:  $($vnet.ResourceGroupName)"
                Write-Host "Gateway Name: $vnetGatewayName"
                Write-Host "Vnet IP name: $vnetIpName"
                Write-Host "Vnet IP config name:  $vnetIpConfigName"
                Write-Host "Address Space:$($vnet.AddressSpace.AddressPrefixes)"
                Write-Host "Gateway Address Space:$vnetGatewayAddressSpace"
                Write-Host "Point-To-Site Address Space:  $vnetPointToSiteAddressSpace"
                Write-Host
                $changeRequested = PromptYesNo "" "Do you wish to change these settings?" 1

                if($changeRequested -eq 0)
                {
                    $vnetGatewayName = ReadHostWithDefault "Vnet Gateway Name" $vnetGatewayName
                    $vnetIpName = ReadHostWithDefault "Vnet IP name" $vnetIpName
                    $vnetIpConfigName = ReadHostWithDefault "Vnet IP configuration name" $vnetIpConfigName
                    $vnetGatewayAddressSpace = ReadHostWithDefault "Vnet Gateway Address Space" $vnetGatewayAddressSpace
                    $vnetPointToSiteAddressSpace = ReadHostWithDefault "Vnet Point-to-site Address Space" $vnetPointToSiteAddressSpace
                }
            }

            $ErrorActionPreference = "Stop";

            Write-Host "Creating App association to VNET"
            $propertiesObject = @{
             "vnetResourceId" = "/subscriptions/$($subscriptionId)/resourceGroups/$($vnet.ResourceGroupName)/providers/Microsoft.Network/virtualNetworks/$($vnetName)"
            }

            $virtualNetwork = New-AzureRmResource -Location $location -Properties $PropertiesObject -ResourceName "$($webAppName)/$($vnet.Name)" -ResourceType "Microsoft.Web/sites/virtualNetworkConnections" -ApiVersion 2015-08-01 -ResourceGroupName $resourceGroupName -Force

            # If there is no gateway subnet, we need to create one.
            if($gatewaySubnet -eq $null)
            {
                $gatewaySubnet = New-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -AddressPrefix $vnetGatewayAddressSpace
                $vnet.Subnets.Add($gatewaySubnet);
                Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
            }

            CreateVnetGateway $vnet.ResourceGroupName $vnetName $vnetIpName $location $vnetIpConfigName $vnetGatewayName $virtualNetwork.Properties.CertBlob $vnetPointToSiteAddressSpace

            $gateway = Get-AzureRmVirtualNetworkGateway -ResourceGroupName $vnet.ResourceGroupName -Name $vnetGatewayName
        }
        else
        {
            $uriParts = $gatewaySubnet.IpConfigurations[0].Id.Split('/')
            $gatewayResourceGroup = $uriParts[4]
            $gatewayName = $uriParts[8]

            $gateway = Get-AzureRmVirtualNetworkGateway -ResourceGroupName $vnet.ResourceGroupName -Name $gatewayName

            # validate gateway types, etc.
            if($gateway.GatewayType -ne "Vpn")
            {
                Write-Error "This gateway is not of the Vpn type. It cannot be joined to an App."
                return
            }

            if($gateway.VpnType -ne "RouteBased")
            {
                Write-Error "This gateways Vpn type is not RouteBased. It cannot be joined to an App."
                return
            }

            if($gateway.VpnClientConfiguration -eq $null -or $gateway.VpnClientConfiguration.VpnClientAddressPool -eq $null)
            {
                Write-Host "This gateway does not have a Point-to-site Address Range. Please specify one in CIDR notation, e.g. 10.0.0.0/8"
                $pointToSiteAddress = Read-Host "Point-To-Site Address Space"
                Set-AzureRmVirtualNetworkGatewayVpnClientConfig -VirtualNetworkGateway $gateway.Name -VpnClientAddressPool $pointToSiteAddress
            }

            Write-Host "Creating App association to VNET"
            $propertiesObject = @{
             "vnetResourceId" = "/subscriptions/$($subscriptionId)/resourceGroups/$($vnet.ResourceGroupName)/providers/Microsoft.Network/virtualNetworks/$($vnet.Name)"
            }

            $virtualNetwork = New-AzureRmResource -Location $location -Properties $PropertiesObject -ResourceName "$($webAppName)/$($vnet.Name)" -ResourceType "Microsoft.Web/sites/virtualNetworkConnections" -ApiVersion 2015-08-01 -ResourceGroupName $resourceGroupName -Force

            # We need to check if the certificate here exists in the gateway.
            $certificates = $gateway.VpnClientConfiguration.VpnClientRootCertificates

            $certFound = $false
            foreach($certificate in $certificates)
            {
                if($certificate.PublicCertData -eq $virtualNetwork.Properties.CertBlob)
                {
                    $certFound = $true
                    break
                }
            }

            if(-not $certFound)
            {
                Write-Host "Adding certificate"
                Add-AzureRmVpnClientRootCertificate -VpnClientRootCertificateName "AppServiceCertificate.cer" -PublicCertData $virtualNetwork.Properties.CertBlob -VirtualNetworkGatewayName $gateway.Name
            }
        }

        # Now finish joining by getting the VPN package and giving it to the App
        Write-Host "Retrieving VPN Package and supplying to App"
        $packageUri = Get-AzureRmVpnClientPackage -ResourceGroupName $vnet.ResourceGroupName -VirtualNetworkGatewayName $gateway.Name -ProcessorArchitecture Amd64

        # Put the VPN client configuration package onto the App
        $PropertiesObject = @{
        "vnetName" = $vnet.Name; "vpnPackageUri" = $packageUri
        }

        New-AzureRmResource -Location $location -Properties $PropertiesObject -ResourceName "$($webAppName)/$($vnet.Name)/primary" -ResourceType "Microsoft.Web/sites/virtualNetworkConnections/gateways" -ApiVersion 2015-08-01 -ResourceGroupName $resourceGroupName -Force

        Write-Host "Finished!"
    }

    function RemoveVnet($subscriptionId, $resourceGroupName, $webAppName)
    {
        $webAppConfig = Get-AzureRmResource -ResourceName "$($webAppName)/web" -ResourceType "Microsoft.Web/sites/config" -ApiVersion 2015-08-01 -ResourceGroupName $resourceGroupName
        $currentVnet = $webAppConfig.Properties.VnetName
        if($currentVnet -ne $null -and $currentVnet -ne "")
        {
            Write-Host "Currently connected to VNET $currentVnet"

            Remove-AzureRmResource -ResourceName "$($webAppName)/$($currentVnet)" -ResourceType "Microsoft.Web/sites/virtualNetworkConnections" -ApiVersion 2015-08-01 -ResourceGroupName $resourceGroupName
        }
          else
        {
          Write-Host "Not connected to a VNET."
        }
    }

    Write-Host "Please Login"
    Login-AzureRmAccount

    # Choose subscription. If there's only one we will choose automatically

    $subs = Get-AzureRmSubscription
    $subscriptionId = ""

    if($subs.Length -eq 0)
    {
        Write-Error "No subscriptions bound to this account."
        return
    }

    if($subs.Length -eq 1)
    {
        $subscriptionId = $subs[0].SubscriptionId
    }
    else
    {
        $subscriptionChoices = @()
        $subscriptionValues = @()

        foreach($subscription in $subs){
        $subscriptionChoices += "$($subscription.SubscriptionName) ($($subscription.SubscriptionId))";
        $subscriptionValues += ($subscription.SubscriptionId);
        }

        $subscriptionId = PromptCustom "Choose a subscription" $subscriptionValues $subscriptionChoices
    }

    Select-AzureRmSubscription -SubscriptionId $subscriptionId

    $resourceGroup = Read-Host "Please enter the Resource Group of your App"

    $appName = Read-Host "Please enter the Name of your App"

    $options = @("Add a NEW Virtual Network to an App", "Add an EXISTING Virtual Network to an App", "Remove a Virtual Network from an App");
    $optionValues = @(0, 1, 2)
    $option = PromptCustom "What do you want to do?" $optionValues $options

    if($option -eq 0)
    {
        AddNewVnet $subscriptionId $resourceGroup $appName
    }
    if($option -eq 1)
    {
        AddExistingVnet $subscriptionId $resourceGroup $appName
    }
    if($option -eq 2)
    {
        RemoveVnet $subscriptionId $resourceGroup $appName
    }

A parancsprogram másolatának mentése. Ebben a cikkben V2VnetAllinOne.ps1 ennek neve, de más néven is használhatja. Nincsenek-e parancsfájl kapcsolók. Akkor egyszerűen futtathatja. A legfontosabb dolog a parancsfájl hajt végre, kérdezze meg, hogy jelentkezzen be. Miután bejelentkezett, a parancsfájl kapja a fiókkal kapcsolatos részleteket, és előfizetések listáját adja vissza. Nem számítva a hitelesítő adatokat kér, a kezdeti parancsfájl végrehajtása néz ki:

    PS C:\Users\ccompy\Documents\VNET> .\V2VnetAllInOne.ps1
    Please Login

    Environment           : AzureCloud
    Account               : ccompy@microsoft.com
    TenantId              : 722278f-fef1-499f-91ab-2323d011db47
    SubscriptionId        : af5358e1-acac-2c90-a9eb-722190abf47a
    CurrentStorageAccount :

    Choose a subscription

    1) A bemutató előfizetést (af5358e1-acac-2c90-a9eb-722190abf47a)
    2) MS próba (a5350f55-dd5a-41ec-2ddb-ff7b911bb2ef)
    3) Lila bemutató előfizetést (2d4c99a4-57f9-4d5e-a0a1-0034c52db59d)

    Válasszon egy lehetőséget: 3

    Fiók: ccompy@microsoft.com környezet: AzureCloud előfizetés: 2d4c99a4-57f9-4d5e-a0a1-0034c52db59d bérlői: 722278f-fef1-499f-91ab-2323d011db47

    Adja meg az alkalmazás az erőforráscsoport: hcdemo-rg adja meg az alkalmazás nevére: v2vnetpowershell milyen feladatot szeretne tenni?

    1) Egy új virtuális hálózat az alkalmazás hozzáadása
    2) Egy meglévő virtuális hálózat az alkalmazás hozzáadása
    3) Az alkalmazás virtuális hálózat eltávolítása

Ebben a szakaszban a többi ismerteti, hogy minden ezeket a beállításokat.

### <a name="create-a-resource-manager-vnet-and-integrate-with-it"></a>Hozzon létre egy erőforrás-kezelő VNet, és azt integrálása ###

Hozzon létre egy új virtuális hálózatot, az erőforrás-kezelő telepítési modell használó integrálása azt az alkalmazást, jelölje be **1) alkalmazás hozzáadása egy új virtuális hálózati**. Ez kérni fogja a virtuális hálózat nevét. Abban az esetben ahogy az alábbi beállításokat is látható lehet használni a nevét, v2pshell.

A parancsprogram a részletes adatait a virtuális hálózat a létrehozandó adja vissza. Ha szeretné, az értékek módosíthatom. Ez például végrehajtása lehet létre virtuális hálózatot, amely tartalmazza a következő beállításokat:

    Virtual Network Name:         v2pshell
    Resource Group Name:          hcdemo-rg
    Gateway Name:                 v2pshell-gateway
    Vnet IP name:                 v2pshell-ip
    Vnet IP config name:          v2pshell-ipconf
    Address Space:                10.0.0.0/8
    Gateway Address Space:        10.5.0.0/16
    Point-To-Site Address Space:  172.16.0.0/12

    Do you wish to change these settings?
    [Y] Yes  [N] No  [?] Help (default is "N"):

Ha valamelyikét módosítani szeretné, írja be az **I** , és végezze el a módosításokat. Ha elégedett a virtuális hálózat beállításainak, írja be a **N** , vagy egyszerűen nyomja le az ENTER billentyűt, amikor a rendszer kéri, a beállítások módosításával kapcsolatos további. Itt a befejezéséig parancsfájl a program tájékoztatja, néhány műveletet "i's módon mindaddig, amíg az virtuális hálózati átjáró kezdődik. Ezt a lépést is igénybe vehet egy óra. Ebben a fázisában nem nincs folyamatjelzőt, de a parancsfájl szolgálat tájékoztatja Önt, az átjáró létrehozásakor.

Amikor befejeződött a parancsfájlt, azt fog fel **Befejezett**. Ezen a ponton lehetősége van egy erőforrás-kezelő virtuális hálózat nevét és a kijelölt beállításait tartalmazó. Új virtuális hálózat is integrálódik az alkalmazást.

### <a name="integrate-your-app-with-a-preexisting-resource-manager-vnet"></a>Az alkalmazás integrálása a korábban létrehozott erőforrás-kezelő VNet ###

Egy korábban létrehozott virtuális hálózaton, ha megad egy erőforrás-kezelő virtuális hálózat, amely nem tartozik egy átjáró vagy a csatlakozási pont-webhely esetén integrálása, amikor a parancsfájl fog beállítása, hogy. Ha a VNET már beállítása műveletet, a parancsfájl ugrik egyenes app integrációja. Ez a folyamat elindításához, egyszerűen jelölje ki **2) felvétele egy meglévő virtuális hálózat alkalmazás**.

Ez a beállítás csak akkor, ha van egy korábban létrehozott erőforrás-kezelő virtuális hálózathoz ugyanabban az előfizetésben, mint az alkalmazás működik. Miután a beállítást választja, bemutatni az erőforrás-kezelő virtuális hálózatok listáját.   

    Select a VNET to integrate with

    1) v2demonetwork
    2) v2pshell
    3) v2vnetintdemo
    4) v2asenetwork
    5) v2pshell2

    Válasszon egy lehetőséget: 5

Egyszerűen jelölje ki a virtuális hálózat, integráció a kívánt. Ha már van egy átjárót, amelyen a webhely-pont kapcsolat engedélyezve, a parancsfájl egyszerűen integrálása az alkalmazás a virtuális hálózat. Ha nem rendelkezik az átjárók, szüksége lesz az átjáró alhálózat adja meg. A virtuális hálózati címterület átjáró alhálózathoz kell lennie, és a más alhálózathoz nem lehet. Ha anélkül, hogy az átjáró virtuális hálózatot használ, és ezt a lépést futtatásához dolog, amit a következőhöz hasonlóan:

    This Virtual Network has no gateway. I will need to create one.
    Your VNET is in the address space 172.16.0.0/16, with the following Subnets:
    default: 172.16.0.0/24
    Please choose a GatewaySubnet address space: 172.16.1.0/26

Ebben a példában a létrehozott egy virtuális hálózati átjáró, amely tartalmazza a következő beállításokat:

    Virtual Network Name:         v2pshell2
    Resource Group Name:          vnetdemo-rg
    Gateway Name:                 v2pshell2-gateway
    Vnet IP name:                 v2pshell2-ip
    Vnet IP config name:          v2pshell2-ipconf
    Address Space:                172.16.0.0/16
    Gateway Address Space:        172.16.1.0/26
    Point-To-Site Address Space:  172.16.0.0/12

    Do you wish to change these settings?
    [Y] Yes  [N] No  [?] Help (default is "N"):
    Creating App association to VNET

Ha szeretné ezeket a beállításokat módosíthatja, megteheti. Vagy nyomja le az ENTER billentyűt, és a parancsfájl lesz az átjáró létrehozása és az alkalmazás csatolni virtuális hálózathoz. Az átjáró létrehozási idejének még egy óra, bár, ezért ügyeljen, tartsa szem előtt. Minden befejezésekor, a parancsfájl fog tegyük fel **Befejezett**.

### <a name="disconnect-your-app-from-a-resource-manager-vnet"></a>Az alkalmazás egy erőforrás-kezelő VNet választani ###

A virtuális hálózati kapcsolat bontása az alkalmazás nem lefelé az átjáró venni vagy letiltása a csatlakozási pont-webhely. Lehet, ha minden használni azt valami mást. Is nem szakítja meg bármely más alkalmazásokból az egyik megadott eltérő. Ez a művelet végrehajtásához jelölje be a **3) virtuális hálózat eltávolítása alkalmazás**. Ha így tesz, láthatja az alábbihoz hasonló:

    Currently connected to VNET v2pshell

    Confirm
    Are you sure you want to delete the following resource:
    /subscriptions/edcc99a4-b7f9-4b5e-a9a1-3034c51db496/resourceGroups/hcdemo-rg/providers/Microsoft.Web/sites/v2vnetpowers
    hell/virtualNetworkConnections/v2pshell
    [Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"):

Bár a parancsfájl törlés azt mondja, nem törli a virtuális hálózat. Azt integrációja csak eltávolítja. Követően, győződjön meg arról, hogy ez a Mit szeretne tenni, a parancs igazán gyors feldolgozása, és ez az **Igaz,** Ha az áttelepítés.

<!--Links-->
[createvpngateway]: http://azure.microsoft.com/documentation/articles/vpn-gateway-point-to-site-create/
[azureportal]: http://portal.azure.com
