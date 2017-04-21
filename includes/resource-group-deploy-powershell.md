## <a name="how-to-deploy-with-powershell"></a>A PowerShell telepítése

1. Jelentkezzen be az Azure-fiókjába.

          Add-AzureAccount

   Adja meg a hitelesítő adatokat, miután a parancsot a fiókkal kapcsolatos információk adja eredményül.

          Id                             Type       ...
          --                             ----    
          example@contoso.com            User       ...   

2. Ha több előfizetéssel rendelkezik, adja meg a telepítéshez használni kívánt előfizetés azonosítója. 

          Select-AzureSubscription -SubscriptionID <YourSubscriptionId>

3. Váltson az erőforrás-kezelő Azure modulra.

          Switch-AzureMode AzureResourceManager

4. Ha nem egy meglévő erőforráscsoport, hozzon létre egy új erőforráscsoport. Adja meg a nevét az erőforráscsoport és helyre van szüksége a megoldás.

        New-AzureResourceGroup -Name ExampleResourceGroup -Location "West US"

   Az új erőforráscsoport összefoglalását adja vissza.

        ResourceGroupName : ExampleResourceGroup
        Location          : westus
        ProvisioningState : Succeeded
        Tags              :
        Permissions       :
                    Actions  NotActions
                    =======  ==========
                    *
        ResourceId        : /subscriptions/######/resourceGroups/ExampleResourceGroup

5. Hozzon létre egy új telepítésének a erőforráscsoport, futtassa a **New-AzureResourceGroupDeployment** parancsot, és adja meg a szükséges paramétereket. A paraméterek egy nevet a telepítéshez, az erőforráscsoport, az elérési út vagy URL-CÍMÉT az létrehozott sablonra nevét és az igényektől szükséges paramétereket fogja tartalmazni. 
   
   Paraméterértékeket kezeléséről az alábbi lehetőségekből választhat: 
   
   - Beágyazott paraméterek használata.

            New-AzureResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup -TemplateFile <PathOrLinkToTemplate> -myParameterName "parameterValue"

   - A paraméter objektum használja.

            $parameters = @{"<ParameterName>"="<Parameter Value>"}
            New-AzureResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup -TemplateFile <PathOrLinkToTemplate> -TemplateParameterObject $parameters

   - Paraméter fájl használatával.

            New-AzureResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup -TemplateFile <PathOrLinkToTemplate> -TemplateParameterFile <PathOrLinkToParameterFile>

  Lett telepítve az erőforráscsoport, ha látni fogja a telepítési összefoglalását.

             DeploymentName    : ExampleDeployment
             ResourceGroupName : ExampleResourceGroup
             ProvisioningState : Succeeded
             Timestamp         : 4/14/2015 7:00:27 PM
             Mode              : Incremental
             ...

6. Telepítési hibáinak kapcsolatos tájékozódáshoz.

        Get-AzureResourceGroupLog -ResourceGroup ExampleResourceGroup -Status Failed

7. A telepítési hibák részletes információkat kaphat.

        Get-AzureResourceGroupLog -ResourceGroup ExampleResourceGroup -Status Failed -DetailedOutput
