<properties
    pageTitle="A parancssorból Azure egymást fedő sablonok telepítése |} Microsoft Azure"
    description="Útmutató a sablonok a ClientVM belül vagy azt követően, hogy a virtuális Magánhálózati használatával csatlakozhat az Azure Papírhalom platformok parancssori felület (CLI) használatával."
    services="azure-stack"
    documentationCenter=""
    authors="heathl17"
    manager="byronr"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/26/2016"
    ms.author="helaw"/>

# <a name="deploy-templates-in-azure-stack-using-the-command-line"></a>A parancssorból Azure egymást fedő sablonok telepítése

A parancssor használatával telepítse az Azure Papírhalom ez Azure erőforrás-kezelő sablonok. Azure erőforrás-kezelő sablonok üzembe helyezéséhez és az alkalmazás egyetlen, összehangolt műveletben az erőforrások kiépítése.

## <a name="download-template"></a>Sablon letöltése        
A környezet, amelyben a CLI tesztelésére, töltse le a fájlok azuredeploy.json és azuredeploy.parameters.json [tároló fiók példa sablon létrehozása](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/101-create-storage-account).

## <a name="deploy-template"></a>Sablon terjesztése
Nyissa meg azt a mappát, ahová ezeket a fájlokat voltak letöltött és bevezetését tervezi a sablon a következő parancsot:

    azure group create "cliRG" "local" –f azuredeploy.json –d "testDeploy" –e azuredeploy.parameters.json

Ez a parancs az Azure Papírhalom Ez alapértelmezett hely az erőforrás-csoport **cliRG** üzembe helyezése a sablont.

## <a name="validate-template-deployment"></a>A sablon telepítésének ellenőrzése
Lásd: Ez az erőforrás csoport és a tárterület a fiók, az alábbi parancsokat használhatja:

    azure group list

    azure storage account list

## <a name="next-steps"></a>Következő lépések

[Felhasználói engedélyek kezelése](azure-stack-manage-permissions.md)
