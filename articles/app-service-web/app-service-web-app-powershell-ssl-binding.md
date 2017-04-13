<properties
    pageTitle="Az SSL-tanúsítványok kötés PowerShell használatával"
    description="Megtudhatja, hogy miként kötést létrehozni az SSL-tanúsítványok az PowerShell használatával webalkalmazást."
    services="app-service\web"
    documentationCenter=""
    authors="ahmedelnably"
    manager="stefsch"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="01/13/2016"
    ms.author="ahmedelnably"/>

# <a name="azure-app-service-ssl-certificate-binding-using-powershell"></a>Azure alkalmazás szolgáltatás SSL tanúsítvány kötelező PowerShell használatával #

Az verziójának megjelenése után a Microsoft Azure PowerShell 1.1.0 verzió új parancsmag kerül, amely szeretne adni, a felhasználó kötést létrehozni az SSL-tanúsítványok meglévő vagy új egy meglévő Web App.

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 

Megismerheti az erőforrás-kezelő Azure-alapú Azure PowerShell-parancsmagok a Web Apps alkalmazások kezelése jelölje be a jelölőnégyzetet [erőforrás-kezelő Azure-alapú PowerShell-parancsok az Azure Web App](app-service-web-app-azure-resource-manager-powershell.md)

## <a name="uploading-and-binding-a-new-ssl-certificate"></a>Tölt fel és a kötelező új SSL-tanúsítvány ##

Alkalmazási helyzetek: A felhasználó szeretné kötni a web Apps alkalmazások egyikét SSL-tanúsítvány.

Az erőforrás-csoport nevét, amely tartalmazza a web App alkalmazásban, a web app nevét, a tanúsítvány .pfx fájl elérési útját a felhasználó gép, a tanúsítványt és az egyéni hostname (állomásnév) jelszavának ismeretében is használjuk az alábbi PowerShell-parancsot, hogy az SSL kötést létrehozni:

    New-AzureRmWebAppSSLBinding -ResourceGroupName myresourcegroup -WebAppName mytestapp -CertificateFilePath PathToPfxFile -CertificatePassword PlainTextPwd -Name www.contoso.com

Ne feledje, hogy mielőtt felvenne egy SSL kötés megfelelő web App alkalmazásban, meg kell már be van állítva állomásnév (az egyéni tartomány). Ha nincs beállítva az állomásnév, majd a hiba lép fel új-AzureRmWebAppSSLBinding futtatása közben nem létezik 'hostname (állomásnév)'. A hostname (állomásnév) közvetlenül a portálon vagy Azure PowerShell használatával is hozzáadhat. A hostname (állomásnév) beállítása a New-AzureRmWebAppSSLBinding futtatása előtt az alábbi PowerShell kódtöredékének lehet.   
  
    $webApp = Get-AzureRmWebApp -Name mytestapp -ResourceGroupName myresourcegroup  
    $hostNames = $webApp.HostNames  
    $HostNames.Add("www.contoso.com")  
    Set-AzureRmWebApp -Name mytestapp -ResourceGroupName myresourcegroup -HostNames $HostNames   
  
Ha meg szeretné érteni, hogy a Set-AzureRmWebApp parancsmag felülírja a webalkalmazás az állomásnevekké fontos. Ezért a fenti PowerShell kódtöredékének van hozzáfűzése a meglévő lista a a webalkalmazás állomásnevek.  

## <a name="uploading-and-binding-an-existing-ssl-certificate"></a>Tölt fel, és egy meglévő SSL-tanúsítványt kötése ##

Alkalmazási helyzetek: A felhasználó szeretné kötni a web Apps alkalmazások közül egy korábban feltöltött SSL-tanúsítvány.

Azt érheti el a következő parancs használatával egy adott erőforráscsoport már feltöltött tanúsítványok listája

    Get-AzureRmWebAppCertificate -ResourceGroupName myresourcegroup

Figyelje meg, hogy a tanúsítványok helyi, hogy egy adott pontjára, majd az erőforrás, a felhasználónak kell újra töltse fel a tanúsítvány, ha a beállított web app egy másik helyre és erőforrás csoport más, amely a szükséges tanúsítvány 

Arra az erőforrás-csoport nevét, amely tartalmazza a web App alkalmazásban, a web app nevét, a tanúsítvány ujjlenyomat és az egyéni hostname (állomásnév), az alábbi PowerShell-parancsot, hogy az SSL kötés létrehozásához használható azt:

    New-AzureRmWebAppSSLBinding -ResourceGroupName myresourcegroup -WebAppName mytestapp -Thumbprint <certificate thumbprint> -Name www.contoso.com

## <a name="deleting-an-existing-ssl-binding"></a>Egy meglévő SSL kötés törlése  ##

Alkalmazási helyzetek: A felhasználó szeretné törölni egy meglévő SSL kötés.

Az erőforrás-csoport nevét, amely tartalmazza a web App alkalmazásban, a web app nevét és az egyéni hostname (állomásnév) ismeretében azt segítségével az alábbi PowerShell-parancsot, hogy az SSL kötés eltávolítása:

    Remove-AzureRmWebAppSSLBinding -ResourceGroupName myresourcegroup -WebAppName mytestapp -Name www.contoso.com

Ne feledje, ha az eltávolított SSL kötés volt az utolsó kötés, hogy tanúsítvány használatával, hogy a helyet, a tanúsítvány alapértelmezés szerint is törli, ha a felhasználó szeretné tartani a tanúsítvány őt a DeleteCertificate lehetőség használatával a tanúsítvány megtartása

    Remove-AzureRmWebAppSSLBinding -ResourceGroupName myresourcegroup -WebAppName mytestapp -Name www.contoso.com -DeleteCertificate $false

### <a name="references"></a>Hivatkozások ###
- [Azure erőforrás-kezelő alapú PowerShell-parancsait Azure-webalkalmazások számára](app-service-web-app-azure-resource-manager-powershell.md)
- [Alkalmazás-szolgáltatási környezetben – bevezetés](app-service-app-service-environment-intro.md)
- [Azure PowerShell használatával a Azure-kezelő eszközzel](../powershell-azure-resource-manager.md)
