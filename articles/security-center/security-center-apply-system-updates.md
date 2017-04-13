<properties
   pageTitle="Azure biztonság otthon a rendszer-frissítések telepítése |} Microsoft Azure"
   description="A dokumentum bemutatja, hogyan tudnak megvalósítani a **frissítéseket a rendszer** és a **rendszer frissítés után indítsa újra**az Azure biztonság otthon javaslatok."
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

# <a name="apply-system-updates-in-azure-security-center"></a>Azure biztonság otthon a rendszer-frissítések telepítése

Azure biztonság otthon figyeli naponta Windows és Linux virtuális gépeken futó (VMs) hiányzó operációs rendszer frissítéseit. Biztonság otthon átveszi elérhető biztonsági és a fontos frissítések a Windows Update vagy Windows Server Update szolgáltatások (szolgáltatással), attól függően, amely a szolgáltatás be van állítva egy Windows virtuális a.  Biztonság otthon is ellenőrzi a legújabb frissítések Linux rendszerben. Ha a virtuális hiányzik a rendszer frissítése, biztonság otthon javasol, hogy a rendszer frissítés alkalmazása

> [AZURE.NOTE] A dokumentum egy példa telepítési használatával vezet be a szolgáltatást.  Ez a cikk részletesen ismerteti nem.

## <a name="implement-the-recommendation"></a>Az ajánlási megvalósítása

1. Jelölje ki a **javaslatok** lap **Alkalmaz rendszer frissítéseit**.
![Rendszer-frissítések telepítése][1]

2. Az **alkalmazás rendszer frissítések** lap megnyitja a hiányzó frissítéseket a rendszer VMs listáját megjelenítő. Jelölje ki a virtuális.
![Jelölje ki a virtuális][2]

3. Egy lap nyílik meg, hogy virtuális hiányzó frissítések listáját megjelenítő. Jelölje be a rendszer frissítése. Ebben a példában jelöljük ki KB3156016.
![Hiányzó biztonsági frissítések][3]

4. Kövesse a hiányzó frissítés **Számú biztonsági frissítés** lap.
![Biztonsági frissítés][4]

## <a name="reboot-after-system-updates"></a>Indítsa újra a rendszer frissítés után

5. Vissza a **javaslatok** lap. Új bejegyzés jött létre, miután beállította a rendszer frissítéseit nevű a **frissítéseket a rendszer után indítsa újra**. Ez a bejegyzés tájékoztatja arról, hogy újra kell indítania a virtuális alkalmazásának rendszerben frissítéseket a folyamat befejezéséhez.
![Indítsa újra a rendszer frissítés után][5]

6. Jelölje be a **frissítéseket a rendszer után indítsa újra**. Ezzel megnyitja a VMs, hogy újra kell indítania az alkalmazás rendszer frissítések befejezéséhez listáját megjelenítő lap **újraindítás függőben elvégzéséhez a frissítéseket a rendszer** .
![Indítsa újra a függőben lévő][6]

Indítsa újra a virtuális az Azure a folyamat befejezéséhez.

## <a name="see-also"></a>Lásd még:

Többet szeretne tudni a biztonság otthon, olvassa el az alábbiakat:

- [Az Azure biztonság otthon biztonsági házirendek beállítása](security-center-policies.md) – a Azure előfizetések és erőforrás-csoportok biztonsági házirendek konfigurálásának ismertetése.
- [Kezelése biztonsági ajánlások az Azure biztonság otthon](security-center-recommendations.md) névre hallgat – ismerje meg, hogyan javaslatok segítenek az Azure erőforrások védelme.
- [Biztonsági állapot ellenőrzése Azure biztonság otthon](security-center-monitoring.md) – útmutató a Lync-állapota az Azure erőforrások.
- [Kezelése és válaszol a biztonsági figyelmeztetések az Azure biztonság otthon](security-center-managing-and-responding-alerts.md) – megtudhatja, hogy miként kezelheti, és a biztonsági figyelmeztetések válaszolni.
- [Figyelés partnermegoldások Azure biztonsági központ](security-center-partner-solutions.md) – megtudhatja, hogy miként figyelheti a partnerek megoldásaival állapotának.
- [Azure biztonsági központja – gyakori kérdések](security-center-faq.md) – keresés gyakran ismételt kérdések a szolgáltatással.
- [Azure biztonsági blog](http://blogs.msdn.com/b/azuresecurity/) – keresés blog bejegyzések Azure biztonság és megfelelőség kapcsolatban.

<!--Image references-->
[1]: ./media/security-center-apply-system-updates/recommendation.png
[2]:./media/security-center-apply-system-updates/select-vm.png
[3]: ./media/security-center-apply-system-updates/missing-security-updates.png
[4]: ./media/security-center-apply-system-updates/security-update.png
[5]: ./media/security-center-apply-system-updates/reboot-after-system-updates.png
[6]: ./media/security-center-apply-system-updates/restart-pending.png
