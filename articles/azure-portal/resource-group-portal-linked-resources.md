<properties 
    pageTitle="Kapcsolódó és csatolt erőforrások, a mozaik elrendezés gyűjteményben" 
    description="A mozaik gyűjtemény a Azure előzetes portál megjelenített kapcsolódó és csatolt forrásokat is találhat." 
    services="azure-portal" 
    documentationCenter="" 
    authors="adamabdelhamed" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="azure-portal" 
    ms.workload="multiple" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="07/16/2015" 
    ms.author="adamab"/>

# <a name="related-and-linked-resources-in-the-tile-gallery"></a>A mozaik elrendezés gyűjteményben kapcsolódó és csatolt erőforrások

A mozaik gyűjtemény lehetővé teszi, hogy a csempék keresse meg az adott erőforrás, és húzza őket az aktuális lap. A mozaik gyűjtemény használatával, erőforrások átterjedő management nézeteket hozhat létre. Minden megadott erőforrás a kapcsolódó források az összes erőforrás által megosztott erőforrásként az azonos erőforráscsoport és hivatkozni szeretne, vagy az erőforrás az erőforrások tartalmazza.

## <a name="linked-resources-in-azure-resource-manager"></a>Csatolt erőforrások az Azure erőforrás-kezelő

A csatolás, akkor egy, az Azure erőforrás-kezelő szolgáltatást.  Lehetővé teszi a deklarálása erőforrások közötti kapcsolatokat, még akkor is, ha nem az azonos erőforráscsoport tárolnak. Az erőforrások, nincs hatással a számlázás és szerepköralapú hozzáférés-nincs hatással a futtatókörnyezet csatolása nincs hatással van.  Érdemes egyszerűen egy mechanizmusa kapcsolatok jelenítik meg, hogy az eszközök, például a csempe gyűjtemény biztosíthat a multimédiás kezelési tapasztalható is használhatja.  Az eszközök a hivatkozások a API hivatkozásokat használva vizsgálata és egyéni Ügyfélkapcsolat-kezelés találkozik, valamint a szükséges. 

## <a name="how-do-i-link-my-resources"></a>Hogyan tudom hivatkozás az erőforrások?

Források a portálon keresztül, vagy telepíti az Azure PowerShell vagy Azure CLI keresztül sablon létrehozásakor hivatkozásokat automatikusan létrejönnek néhány függő erőforrások. Források a [Csatolt erőforrások REST API -t](https://msdn.microsoft.com/library/azure/mt238499.aspx) vagy a kapcsolatok, a sablon a deklarálása programozás útján is csatolhat. Teljes vitafórum csatolt erőforrások való használatáról olvassa el a [Linking erőforrások az Azure erőforrás-kezelő](../resource-group-link-resources.md)című témakört.

## <a name="next-steps"></a>Következő lépések

- Ha a Azure erőforrás-kezelő sablonok írása bemutatása van szüksége, olvassa el a [sablonok létrehozása](../resource-group-authoring-templates.md).
- Kördiagramjaira be részletesebben erőforrások közötti kapcsolatok létrehozásával kapcsolatban, tanulmányozza a [Linking információk az Azure erőforrás-kezelő](../resource-group-link-resources.md)című témakört.
- Szeretne többet szeretne tudni az előnézeti portálon keresztül erőforrás csoportjaival végezhető, olvassa el a [az Azure előzetes portálon kezelheti az Azure erőforrások](resource-group-portal.md)című témakört.
