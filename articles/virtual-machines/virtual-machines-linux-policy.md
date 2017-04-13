<properties
    pageTitle="Az Azure erőforrás-kezelő virtuális gépeken futó házirendek alkalmazása |} Microsoft Azure"
    description="Hogyan házirendet szeretne egy Azure erőforrás-kezelő Linux virtuális gépen"
    services="virtual-machines-linux"
    documentationCenter=""
    authors="singhkays"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="04/13/2016"
    ms.author="singhkay"/>

# <a name="apply-policies-to-azure-resource-manager-virtual-machines"></a>Az Azure erőforrás-kezelő virtuális gépeken futó házirendek alkalmazása

Házirendek használatával a szervezet különböző szokásai és szabályok vállalatszerte is hivatkozási. A kívánt viselkedés végrehajtását segíthetnek a kockázatok csökkentésében járult hozzá, hogy a szervezet egyik közben. Ebben a cikkben azt bemutatja, hogyan használhatja Azure erőforrás-kezelő házirendeket szervezete virtuális gépeken futó a kívánt viselkedés definiálni.

A Vázlat lépések végrehajtásához megegyezik alatt

1. Azure erőforrás-kezelő házirend 101
2. A virtuális gép szabályzat kialakítása
3. A házirend létrehozása
4. A szabály alkalmazása

## <a name="azure-resource-manager-policy-101"></a>Azure erőforrás-kezelő házirend 101

Az első lépések az erőforrás-kezelő Azure házirendek, azt javasoljuk az alábbi cikkben olvasása, és kattintson a jelen cikkben ismertetett lépések folytatása. Az az alábbi cikkben a Alapfogalmak és szerkezetének egy házirendet, hogyan házirendek első kiértékelt és a házirend-definíciók különböző példákat.

* [Házirend segítségével kezelheti az erőforrásokat, és a hozzáférés szabályozása](../resource-manager-policy.md)

## <a name="define-a-policy-for-your-virtual-machine"></a>A virtuális gép szabályzat kialakítása

Vállalati esetei közül lehet engedélyezése csak a felhasználók az adott operációs rendszerek, amely egy üzleti alkalmazás kompatibilisek legyenek teszteltük virtuális gépeken futó létrehozásához. Az erőforrás-kezelő Azure-házirend használata a feladat elvégezhető néhány lépésben. A házirend példában fogjuk engedélyezése csak Ubuntu 14.04.2-LTS virtuális gépeken futó létrehozni. A házirend-definícióhoz néz alatt

```
"if": {
  "allOf": [
    {
      "field": "type",
      "equals": "Microsoft.Compute/virtualMachines"
    },
    {
      "not": {
        "allOf": [
          {
            "field": "Microsoft.Compute/virtualMachines/imagePublisher",
            "equals": "Canonical"
          },
          {
            "field": "Microsoft.Compute/virtualMachines/imageOffer",
            "equals": "UbuntuServer"
          },
          {
            "field": "Microsoft.Compute/virtualMachines/imageSku",
            "equals": "14.04.2-LTS"
          }
        ]
      }
    }
  ]
},
"then": {
  "effect": "deny"
}
```

A fenti házirend könnyen módosítható, hogy hol lehet engedélyezni szeretné a virtuális gép telepítés használandó bármilyen Ubuntu LTS kép példa a módosítás alatt

```
{
  "field": "Microsoft.Compute/virtualMachines/imageSku",
  "like": "*LTS"
}
```

#### <a name="virtual-machine-property-fields"></a>Virtuális gép tulajdonságmezők

Az alábbi táblázat ismerteti a virtuális gép tulajdonságok házirend definíciójának mezőiként használható. További a házirend mezők alapján olvassa el az alábbi:

* [Mezők és a források](../resource-manager-policy.md#fields-and-sources)


| A mező neve     | Leírás                                        |
|----------------|----------------------------------------------------|
| imagePublisher | Adja meg a kép a publisher               |
| imageOffer     | Adja meg a a kijelölt kép a publisher vonatkozó |
| imageSku       | Adja meg a választott ajánlatra SKU             |
| imageVersion   | Adja meg a kép a kiválasztott Termékváltozat verzió     |

## <a name="create-the-policy"></a>A házirend létrehozása

Egyszerűen létrehozható házirend közvetlenül a REST API- vagy a PowerShell-parancsmagok használata. A házirend létrehozása, olvassa el az alábbi:

* [Házirend létrehozása](../resource-manager-policy.md#creating-a-policy)


## <a name="apply-the-policy"></a>A szabály alkalmazása

A házirend létrehozása után kell alkalmazhatja azt egy definiált webhelyen. A hatókör az előfizetést, erőforráscsoport vagy akár az erőforrás lehet. A házirend alkalmazására, olvassa el az alábbi:

* [Házirend létrehozása](../resource-manager-policy.md#applying-a-policy)
