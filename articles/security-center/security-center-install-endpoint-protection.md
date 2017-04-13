<properties
   pageTitle="Az Endpoint Protection telepítse az Azure biztonság otthon |} Microsoft Azure"
   description="A dokumentum bemutatja, hogyan tudnak megvalósítani az Azure biztonság otthon ajánlást **Endpoint Protection telepítése**."
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
   ms.date="08/16/2016"
   ms.author="terrylan"/>

# <a name="install-endpoint-protection-in-azure-security-center"></a>Az Endpoint Protection telepítse az Azure biztonság otthon

Azure biztonság otthon javasol kiépítése antimalware programon való az Azure virtuális gépeken futó (VMs) Ha antimalware már nem engedélyezett. A javaslat csak Windows VMs vonatkozik.

> [AZURE.NOTE] A dokumentum egy példa telepítési használatával vezet be a szolgáltatást.  Ez a cikk részletesen ismerteti nem.

## <a name="implement-the-recommendation"></a>Az ajánlási megvalósítása

1. Jelölje ki a **javaslatok** lap **Endpoint Protection telepítése**.
![Válassza a telepítés Endpoint Protection][1]

2. Az **Endpoint Protection telepítése** lap megnyitása nélkül engedélyezett antimalware VMs listáját megjelenítő. Jelölje ki a listából a kívánt antimalware telepítése, és kattintson a **VMs telepítése**VMs.
![Jelölje ki a VMs antimalware telepítése][2]

3. Az **Endpoint Protection jelölje be** a lap megnyitása lehetővé teszi a jelölje ki a használni kívánt antimalware megoldást. Ebben a példában jelöljük ki **Microsoft Antimalware**.
![Jelölje ki az Endpoint Protection][3]

4. További információ a antimalware megoldást jelenik meg. Jelölje ki a **létrehozása**.
![Antimalware megoldás létrehozása][4]

5. Adja meg a szükséges konfigurációs beállítások a **Bővítmény hozzáadása** lap, és kattintson **az OK**gombra. Többet szeretne tudni a beállítások, lásd: az [alapértelmezett és egyéni Antimalware beállítás](../security/azure-security-antimalware.md#default-and-custom-antimalware-configuration).

[A Microsoft Antimalware](../azure-security-antimalware.md) már aktív, kattintson a kijelölt VMs.

## <a name="see-also"></a>Lásd még:

Ez a cikk mutatott megvalósításáról a biztonság otthon ajánlási "Telepítés Endpoint Protection." Azure-ban antimalware programon engedélyezésével kapcsolatos további információért olvassa el az alábbiakat:

- [A Felhőszolgáltatások és a virtuális gépeken futó Microsoft Antimalware](../azure-security-antimalware.md) – Microsoft antimalware telepítése.

Többet szeretne tudni a biztonság otthon, olvassa el az alábbiakat:

- [Az Azure biztonság otthon biztonsági házirendek beállítása](security-center-policies.md) – megtudhatja, hogy miként biztonsági házirendek beállítása.
- [Kezelése biztonsági ajánlások az Azure biztonság otthon](security-center-recommendations.md) névre hallgat – ismerje meg, hogyan javaslatok segítenek az Azure erőforrások védelme.
- [Biztonsági állapot ellenőrzése Azure biztonság otthon](security-center-monitoring.md) – útmutató a Lync-állapota az Azure erőforrások.
- [Kezelése és válaszol a biztonsági figyelmeztetések az Azure biztonság otthon](security-center-managing-and-responding-alerts.md) – megtudhatja, hogy miként kezelheti, és a biztonsági figyelmeztetések válaszolni.
- [Figyelés partnermegoldások Azure biztonsági központ](security-center-partner-solutions.md) – megtudhatja, hogy miként figyelheti a partnerek megoldásaival állapotának.
- [Azure biztonsági központja – gyakori kérdések](security-center-faq.md) – keresés gyakran ismételt kérdések a szolgáltatással.
- [Azure biztonsági blog](http://blogs.msdn.com/b/azuresecurity/) – keresés blog bejegyzések Azure biztonság és megfelelőség kapcsolatban.

<!--Image references-->
[1]:./media/security-center-install-endpoint-protection/select-install-endpoint-protection.png
[2]:./media/security-center-install-endpoint-protection/install-endpoint-protection-blade.png
[3]:./media/security-center-install-endpoint-protection/select-endpoint-protection.png
[4]:./media/security-center-install-endpoint-protection/create-antimalware-solution.png
