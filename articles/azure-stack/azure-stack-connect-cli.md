<properties
    pageTitle="Csatlakozás CLI az Azure Papírhalom |} Microsoft Azure"
    description="Megtudhatja, hogy miként kezelheti, és a Azure Papírhalom erőforrások üzembe a platformok parancssori kezelőfelületről használatával"
    services="azure-stack"
    documentationCenter=""
    authors="HeathL17"
    manager="byronr"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/19/2016"
    ms.author="helaw"/>

# <a name="install-and-configure-azure-stack-cli"></a>Telepítse és állítsa be az Azure Papírhalom CLI

A jelen dokumentum azt végigvezeti Önt a folyamaton, Azure parancssori felület (CLI) használatával kezelheti az Azure Papírhalom erőforrásokat Linux és a Mac ügyfél platformon futó.  

## <a name="install-azure-stack-cli"></a>Telepítse az Azure Papírhalom CLI

Ha a Mac vagy Linux rendszerhez, a következő parancs segítségével elérheti a CLI:
  
    `npm install -g azure-cli@0.10.4`.


## <a name="connect-to-azure-stack"></a>Azure Papírhalom csatlakoztatása
Az alábbi lépéseket az Azure CLI Azure Papírhalom csatlakozhat konfigurálása. Majd jelentkezzen be, és az előfizetés adatai beolvasásához.

1.  Aktív-címtár-erőforrás-azonosító érték beolvasása végrehajtása a PowerShell szerint:
        
          (Invoke-RestMethod -Uri https://api.azurestack.local/metadata/endpoints?api-version=1.0 -Method Get).authentication.audiences[0]

2.  A következő paranccsal CLI hozzáadása a Papírhalom Azure környezetben, gondoskodhat arról, hogy frissíteni *– aktív-címtár-erőforrás-azonosító* adatokat tartalmazó URL-CÍMÉT az előző lépésben beolvasott:

           azure account env add AzureStack --resource-manager-endpoint-url "https://api.azurestack.local" --management-endpoint-url "https://api.azurestack.local" --active-directory-endpoint-url  "https://login.windows.net" --portal-url "https://portal.azurestack.local" --gallery-endpoint-url "https://portal.azurestack.local" --active-directory-resource-id "https://azurestack.local-api/" --active-directory-graph-resource-id "https://graph.windows.net/"

3.  Jelentkezzen be a következő parancs (csere *felhasználónév* a felhasználó nevével együtt) használatával:

        azure login -e AzureStack -u “<username>”

    >[AZURE.NOTE]Ha tanúsítvány érvényességi problémái érkezik, a következő parancs futtatásával letiltása tanúsítvány érvényességi `set        NODE_TLS_REJECT_UNAUTHORIZED=0`.

4.  Állítsa az Azure konfigurációs mód az Azure erőforrás-kezelő használatával az alábbi parancsot:

        azure config mode arm

5.  Lekérdezi előfizetések listáját.

        azure account list     

## <a name="next-steps"></a>Következő lépések

[Az Azure CLI sablonok telepítése](azure-stack-deploy-template-command-line.md)

[Csatlakozás a PowerShell segítségével](azure-stack-connect-powershell.md)

[Felhasználói engedélyek kezelése](azure-stack-manage-permissions.md)
