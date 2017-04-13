<properties
   pageTitle="Internetes végpontok az Azure biztonság otthon hozzáférés korlátozása |} Microsoft Azure"
   description="A dokumentum bemutatja, hogyan tudnak megvalósítani az Azure biztonság otthon ajánlást **Internet szemben lévő végpont korlátozott hozzáférés**."
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
   ms.date="10/26/2016"
   ms.author="terrylan"/>

# <a name="restrict-access-through-internet-facing-endpoints-in-azure-security-center"></a>Internetes végpontok az Azure biztonság otthon hozzáférés korlátozása

Azure biztonság otthon javasol internetes végpontok hozzáférés korlátozása, ha a hálózati biztonsági csoportok (NSGs) közül egy vagy több bejövő szabályok, amelyek lehetővé teszik az access az "egy" forrás IP-cím. Érheti el"" megnyitása lehetővé teheti a támadók az erőforrások eléréséhez. Biztonság otthon fog javasoljuk, hogy a bejövő szabályok való hozzáférés korlátozása forrás IP-címek, hogy biztosan szükséges az access szerkesztése.

Ennek ellenére bármely nem webes port tartalmazó "bármely" forrásaként jön létre.

> [AZURE.NOTE] A dokumentum egy példa telepítési használatával vezet be a szolgáltatást. Ez a cikk részletesen ismerteti nem.

## <a name="implement-the-recommendation"></a>Az ajánlási megvalósítása

1. A **javaslatok lap**jelölje be a **korlátozott hozzáférés Internet szemben lévő végpontot**.
![Szemben lévő végpont interneten keresztül hozzáférés korlátozása][1]

2. Ekkor megnyílik a **korlátozott hozzáférés Internet szemben lévő végpont**lap. Ez a lap bejövő szabályok egy potenciális biztonsági problémát létrehozott listák a virtuális gépeken futó (VMs). Jelölje ki a virtuális.
![Jelölje ki a virtuális][2]

3. A **NSG** lap a hálózati biztonsági csoport információkat, a bejövő szabályok kapcsolódó és a kapcsolódó virtuális jeleníti meg. Válassza a **Bejövő szabályok szerkesztéséhez** egy bejövő szabály szerkesztése folytatásához.
![Hálózati biztonsági csoport lap][3]

4. Válassza a **Bejövő szabályok** a lap szerkesztése bejövő szabályt. Ebben a példában jelöljük ki **AllowWeb**.
![A bejövő szabályok][4]

  Megjegyzés:, válassza a minden NSGs található alapértelmezett szabályokat a beállítási **alapértelmezett szabályokat** . Az alapértelmezett szabályokat nem lehet törölni, de a költségerőforrások hozzá vannak rendelve alacsonyabb, mert azok felülbírálhatja a létrehozott szabályokat. További információ az [alapértelmezett szabályokat](../virtual-network/virtual-networks-nsg.md#default-rules).
![Alapértelmezett szabályok][5]

5. Kattintson a **AllowWeb** lap a bejövő szabályok tulajdonságainak szerkesztése úgy, hogy a **forrás** -IP-cím vagy az IP-címek blokkolása. Többet szeretne tudni a bejövő szabály tulajdonságait, olvassa el a [NSG szabályok](../virtual-network/virtual-networks-nsg.md#nsg-rules).

  ![Bejövő szabály szerkesztése][6]

## <a name="see-also"></a>Lásd még:

Ez a cikk mutatott megvalósításáról a biztonság otthon ajánlási "Korlátozott hozzáférés Internet szemben lévő végpontot." NSGs és szabályok engedélyezésével kapcsolatos további információért olvassa el az alábbiakat:

- [Mi az, hogy egy hálózati biztonsági csoport (NSG)?](../virtual-network/virtual-networks-nsg.md)
- [Az Azure portálon NSGs kezelése](../virtual-network/virtual-networks-create-nsg-arm-pportal.md)

Többet szeretne tudni a biztonság otthon, olvassa el az alábbiakat:

- [Az Azure biztonság otthon biztonsági házirendek beállítása](security-center-policies.md)– a Azure előfizetések és erőforrás-csoportok biztonsági házirendek konfigurálásának ismertetése.
- [Biztonsági ajánlások az Azure biztonság otthon kezelése](security-center-recommendations.md)– ismerje meg, hogyan javaslatok segítenek az Azure erőforrások védelme.
- [Az Azure biztonság otthon figyelése, biztonsági állapot](security-center-monitoring.md)– útmutató a Lync-állapota az Azure erőforrások.
- [Kezelése és válaszol a biztonsági figyelmeztetések az Azure biztonság otthon](security-center-managing-and-responding-alerts.md)– megtudhatja, hogy miként kezelheti, és a biztonsági figyelmeztetések válaszolni.
- [Figyelés partnermegoldások Azure biztonsági központ](security-center-partner-solutions.md) – megtudhatja, hogy miként figyelheti a partnerek megoldásaival állapotának.
- [Azure biztonsági központja – gyakori kérdések](security-center-faq.md)– keresés gyakran ismételt kérdések a szolgáltatással.
- [Azure biztonsági blog](http://blogs.msdn.com/b/azuresecurity/)– legújabb Azure biztonsági híreket és információkat.

<!--Image references-->
[1]: ./media/security-center-restrict-access-thru-internet-facing-endpoint/restrict-access-thru-internet-facing-endpoint.png
[2]: ./media/security-center-restrict-access-thru-internet-facing-endpoint/select-a-vm.png
[3]: ./media/security-center-restrict-access-thru-internet-facing-endpoint/network-security-group-blade.png
[4]: ./media/security-center-restrict-access-thru-internet-facing-endpoint/inbound-security-rules.png
[5]: ./media/security-center-restrict-access-thru-internet-facing-endpoint/default-rules.png
[6]: ./media/security-center-restrict-access-thru-internet-facing-endpoint/edit-inbound-rule.png
