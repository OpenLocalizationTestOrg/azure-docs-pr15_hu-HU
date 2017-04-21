## <a name="how-to-deploy-with-azure-cli"></a>Azure CLI telepítéséről

1. Jelentkezzen be az Azure-fiókjába.

        azure login

  Adja meg a hitelesítő adatokat, miután a parancs a bejelentkezési eredményét adja vissza.

        ...
        info:    login command OK

2. Ha több előfizetéssel rendelkezik, adja meg a telepítéshez használni kívánt előfizetés azonosítója.

        azure account set <YourSubscriptionNameOrId>

3. Váltás a Azure erőforrás-kezelő modul

        azure config mode arm

   Az új üzemmód igazolása kap.

        info:     New mode is arm

4. Ha nem egy meglévő erőforráscsoport, hozzon létre egy új erőforráscsoport. Adja meg a nevét az erőforráscsoport és helyre van szüksége a megoldás.

        azure group create -n ExampleResourceGroup -l "West US"

   Az új erőforráscsoport összefoglalását adja vissza.

        info:    Executing command group create
        + Getting resource group ExampleResourceGroup
        + Creating resource group ExampleResourceGroup
        info:    Created resource group ExampleResourceGroup
        data:    Id:                  /subscriptions/####/resourceGroups/ExampleResourceGroup
        data:    Name:                ExampleResourceGroup
        data:    Location:            westus
        data:    Provisioning State:  Succeeded
        data:    Tags:
        data:
        info:    group create command OK

5. Hozzon létre egy új telepítésének a erőforráscsoport, futtassa a következő parancsot, és adja meg a szükséges paramétereket. A paraméterek egy nevet a telepítéshez, az erőforráscsoport, az elérési út vagy URL-CÍMÉT az létrehozott sablonra nevét és az igényektől szükséges paramétereket fogja tartalmazni.

   Paraméterértékeket kezeléséről az alábbi lehetőségekből választhat:

   - Beágyazott paramétereket és a helyi sablon használata

             azure group deployment create -f <PathToTemplate> {"ParameterName":"ParameterValue"} -g ExampleResourceGroup -n ExampleDeployment

   - Használja a beágyazott paramétereket és a sablon mutató hivatkozást.

             azure group deployment create --template-uri <LinkToTemplate> {"ParameterName":"ParameterValue"} -g ExampleResourceGroup -n ExampleDeployment

   - Használjon paraméter fájlt.

             azure group deployment create -f <PathToTemplate> -e <PathToParameterFile> -g ExampleResourceGroup -n ExampleDeployment

  Lett telepítve az erőforráscsoport, ha látni fogja a telepítési összefoglalását.

         info:    Executing command group deployment create
         + Initializing template configurations and parameters
         + Creating a deployment
         ...
         info:    group deployment create command OK


6. Ha a legújabb üzembe információkra.

         azure group log show -l ExampleResourceGroup

7. A telepítési hibák részletes információkat kaphat.

         azure group log show -l -v ExampleResourceGroup
