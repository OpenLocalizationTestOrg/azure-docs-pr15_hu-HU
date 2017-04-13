<properties
    pageTitle="A fő tárolóra bérlői Azonosítót módosítani, előfizetés áthelyezése után |} Microsoft Azure"
    description="Megtudhatja, hogy miként válthat a fő tárolóból elemre a bérlői Azonosítóját, után előfizetés helyez át egy másik bérlői"
    services="key-vault"
    documentationCenter=""
    authors="amitbapat"
    manager="mbaldwin"
    tags="azure-resource-manager"/>

<tags
    ms.service="key-vault"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="09/13/2016"
    ms.author="ambapat"/>

# <a name="change-a-key-vault-tenant-id-after-a-subscription-move"></a>A fő tárolóra bérlői azonosító módosítása, előfizetés áthelyezése után
### <a name="q-my-subscription-was-moved-from-tenant-a-to-tenant-b-how-do-i-change-the-tenant-id-for-my-existing-key-vault-and-set-correct-acls-for-principals-in-tenant-b"></a>Probléma: az előfizetés helyezett az bérlőből egy bérlői b Hogyan módosítása a meglévő kulcs tárolóból elemre a bérlői azonosító és megfelelő hozzáférés-vezérlési listák megadása rendszerbiztonsági B-ös bérlői?

Az előfizetés hoz létre egy új kulcs tárolóból elemre, ha automatikusan tartozik az alapértelmezett Azure Active Directory bérlői azonosító, hogy az előfizetés. Hozzáférés az összes házirend-bejegyzéseket is tartozik a bérlői azonosító A bérlői az Azure-előfizetése át a bérlői B, a meglévő kulcs tárolókban (a felhasználók és az alkalmazások) bérlőjének b rendszerbiztonsági nem érhetők el Ez a probléma megoldásához kell:

- A bérlői azonosító módosítása társított összes meglévő kulcs tárolókban található az előfizetés bérlőjére b
- Távolítsa el a meglévő hozzáférési házirend bejegyzések.
- Új hozzáférési házirend bejegyzések bérlői b társított hozzáadása

Például az áthelyezett előfizetés kulcs tárolóra "myvault" Ha bérlői A bérlő B, ide kattintva módosíthatja a bérlői Azonosítóját a fő tárolóból elemre, és távolítsa el a régebbi access-házirendek.

<pre>
$vaultResourceId = (get-AzureRmKeyVault - VaultName myvault). ResourceId $vault = Get-AzureRmResource – ResourceId $vaultResourceId - ExpandProperties $vault. Properties.TenantId = (Get-AzureRmContext). Tenant.TenantId $vault. Properties.AccessPolicies = @() Set-AzureRmResource - ResourceId $vaultResourceId-tulajdonságok $vault. Tulajdonságok
</pre>

Mivel a tárolóból elemre az áthelyezés, az eredeti értéket **$vault előtt A bérlői webhelyemen volt. Properties.TenantId** bérlői a közben **(Get-AzureRmContext). Tenant.TenantId** van bérlői b

Most, hogy a tárolóból elemre társítva a megfelelő bérlői azonosító, és a régebbi access házirend bejegyzések törlődnek, új hozzáférés beállítása a [Set-AzureRmKeyVaultAccessPolicy](https://msdn.microsoft.com/library/mt603625.aspx)házirend bejegyzéseinek.

## <a name="next-steps"></a>Következő lépések

Ha kérdései vannak kapcsolatban Azure kulcs tárolóból elemre, látogasson el a [Azure kulcs tárolóra fórumok](https://social.msdn.microsoft.com/forums/azure/home?forum=AzureKeyVault).
