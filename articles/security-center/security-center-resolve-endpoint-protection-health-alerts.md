<properties
   pageTitle="Endpoint protection állapot Azure biztonsági központ riasztásai elhárítása |} Microsoft Azure"
   description="A dokumentum bemutatja, hogyan az Azure biztonság otthon ajánlást **feloldása az Endpoint Protection állapot riasztások**végrehajtásához."
   services="security-center"
   documentationCenter="na"
   authors="TerryLanfear"
   manager="MBaldwin"
   editor=""/>

<tags
   ms.service="security-center"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="07/29/2016"
   ms.author="terrylan"/>

# <a name="resolve-endpoint-protection-health-alerts-in-azure-security-center"></a>Feloldása endpoint protection állapot Azure biztonsági központ riasztásai

Azure biztonság otthon fog javasoljuk, hogy a észlelt végpont védelem állapot riasztások megoldásához.  Biztonság otthon lehetővé teszi, hogy melyik virtuális gépeken futó (VMs) vannak-e endpoint protection hibák és hány hibáit.

> [AZURE.NOTE] A dokumentum egy példa telepítési használatával vezet be a szolgáltatást. Ez a cikk részletesen ismerteti nem.

## <a name="implement-the-recommendation"></a>Az ajánlási megvalósítása

1. A **javaslatok lap**válassza a **feloldása az Endpoint Protection állapot riasztások**lehetőséget.
![Végpont védelem állapot riasztások feloldása][1]

2. Ekkor megnyílik a listák VMs hibák és a kudarcok száma minden virtuális **Endpoint Protection hiba** lap. Jelölje ki a virtuális a listából.
![Endpoint protection hiba][2]

3. A **Hibák listája** a lap megnyitja a kijelölt virtuális, hibák listáját megjelenítő. További információt a listából válassza ki a hiba. Ekkor megnyílik egy lap a kijelölt hibát talál információt.
![Hibák lista][3]
  ![hiba esemény][4]

## <a name="see-also"></a>Lásd még:

Többet szeretne tudni a biztonság otthon, olvassa el az alábbiakat:

- [Az Azure biztonság otthon biztonsági házirendek beállítása](security-center-policies.md)– a Azure előfizetések és erőforrás-csoportok biztonsági házirendek konfigurálásának ismertetése.
- [Biztonsági ajánlások az Azure biztonság otthon kezelése](security-center-recommendations.md)– ismerje meg, hogyan javaslatok segítenek az Azure erőforrások védelme.
- [Az Azure biztonság otthon figyelése, biztonsági állapot](security-center-monitoring.md)– útmutató a Lync-állapota az Azure erőforrások.
- [Kezelése és válaszol a biztonsági figyelmeztetések az Azure biztonság otthon](security-center-managing-and-responding-alerts.md)– megtudhatja, hogy miként kezelheti, és a biztonsági figyelmeztetések válaszolni.
- [Figyelés partnermegoldások Azure biztonsági központ](security-center-partner-solutions.md) – megtudhatja, hogy miként figyelheti a partnerek megoldásaival állapotának.
- [Azure biztonsági központja – gyakori kérdések](security-center-faq.md)– keresés gyakran ismételt kérdések a szolgáltatással.
- [Azure biztonsági blog](http://blogs.msdn.com/b/azuresecurity/)– legújabb Azure biztonsági híreket és információkat.

<!--Image references-->
[1]: ./media/security-center-resolve-endpoint-protection/resolve-endpoint-protection.png
[2]: ./media/security-center-resolve-endpoint-protection/endpoint-protection-failure.png
[3]: ./media/security-center-resolve-endpoint-protection/failure-list.png
[4]: ./media/security-center-resolve-endpoint-protection/failure-event.png
