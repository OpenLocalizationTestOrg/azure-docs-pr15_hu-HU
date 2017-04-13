<properties
   pageTitle="Az Azure biztonság otthon virtuális ügynök engedélyezése |} Microsoft Azure"
   description="A dokumentum bemutatja, hogyan tudnak megvalósítani az Azure biztonság otthon ajánlást **Virtuális ügynök engedélyezése**."
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
   ms.date="10/17/2016"
   ms.author="terrylan"/>

# <a name="enable-vm-agent-in-azure-security-center"></a>Az Azure biztonság otthon virtuális ügynök engedélyezése

A virtuális Agent telepíteni kell a virtuális gépeken futó (VMs) ahhoz, hogy [adatgyűjtés](security-center-enable-data-collection.md)sorrendben.  Azure biztonság otthon lehetővé tevő, hogy melyik VMs megkövetelése a virtuális ügynök fog javasoljuk, hogy engedélyezze az adott VMs virtuális ügynök.

A virtuális Agent alapértelmezés szerint a a Microsoft Azure piactéren lévő telepített VMs telepítve van. A [virtuális ügynök és bővítmények – 2 rész](https://azure.microsoft.com/blog/vm-agent-and-extensions-part-2/) a cikkben információt a virtuális Agent telepítési.


> [AZURE.NOTE] A dokumentum egy példa telepítési használatával vezet be a szolgáltatást. Ez a cikk részletesen ismerteti nem.

## <a name="implement-the-recommendation"></a>Az ajánlási megvalósítása

1. A **javaslatok lap**jelölje ki a **Virtuális ügynök engedélyezése**.
![Virtuális ügynök engedélyezése][1]

2. Ekkor megnyílik a **Virtuális ügynök hiányzó vagy nem válaszol**lap. Ez a lap a VMs a virtuális Agent igénylő sorolja fel. A megjelenő útmutatást követve telepítheti a virtuális agent a lap.
![Virtuális ügynök hiányzik.][2]

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
[1]: ./media/security-center-enable-vm-agent/enable-vm-agent.png
[2]: ./media/security-center-enable-vm-agent/vm-agent-is-missing.png
