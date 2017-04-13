<properties
    pageTitle="Egy virtuális Azure Papírhalom kulcs tárolóra tárolt jelszóval telepítése |} Microsoft Azure"
    description="Megtudhatja, hogy miként egy virtuális Azure Papírhalom kulcs tárolóra tárolt jelszóval terjesztése"
    services="azure-stack"
    documentationCenter=""
    authors="rlfmendes"
    manager="natmack"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/26/2016"
    ms.author="ricardom"/>

# <a name="deploy-a-vm-by-retrieving-the-password-stored-in-key-vault"></a>A virtuális telepítéséhez kulcs tárolóra tárolt jelszó beolvasása

Ha paraméterként biztonságos (például a jelszó) értéket a telepítés során adják át kell ezt az értéket tárolni a titkos az Azure Papírhalom kulcs tárolóból elemre, és más erőforrás-kezelő Azure-sablonok az az érték hivatkozás. Csak a titkos hivatkozást tartalmaz a sablon, a titkos soha nem van téve. Nem kell manuálisan adja meg az érték a titkos minden alkalommal, amikor az erőforrások rendszerbe. Megadhatja, hogy mely felhasználók vagy a szolgáltatás rendszerbiztonsági érheti el a titkos.

## <a name="reference-a-secret-with-static-id"></a>A titkos statikus azonosítójú hivatkozás

A paraméterek fájlok átadja értékeket a sablon belül a titkos kulcs hivatkozik. A titkos referencia a megkerülhetők, a fő tárolóból elemre az erőforrás-azonosító és a titkos nevét. Ebben a példában a fő tárolóra titkos már léteznie kell. Statikus értéknek használható az erőforrás-azonosító.

    "parameters": {
    "adminPassword": {
    "reference": {
    "keyVault": {
    "id": "/subscriptions/{guid}/resourceGroups/{group-name}/providers/Microsoft.KeyVault/vaults/{vault-name}"
    },
    "secretName": "sqlAdminPassword"


>[AZURE.NOTE]Fogadja el a titkos paraméter egy *securestring*kell lennie.

## <a name="next-steps"></a>Következő lépések
[Kulcs tárolóból elemre a minta alkalmazások terjesztése](azure-stack-kv-sample-app.md)

[A virtuális kulcs tárolóra tanúsítvánnyal terjesztése](azure-stack-kv-push-secret-into-vm.md)

