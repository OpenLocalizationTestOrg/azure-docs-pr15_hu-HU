<properties
   pageTitle="Adja hozzá a következő generációs tűzfalat az Azure biztonság otthon |} Microsoft Azure"
   description="A dokumentum bemutatja, hogyan az Azure biztonság otthon ajánlások **hozzáadása a következő generációs tűzfal** és **útvonal traffice keresztül csak NGFW**végrehajtásához."
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

# <a name="add-a-next-generation-firewall-in-azure-security-center"></a>Adja hozzá a következő generációs tűzfalat az Azure biztonság otthon

Azure biztonság otthon előfordulhat, hogy ajánlott felvenni a következő generációs tűzfal (NGFW) Microsoft partnertől a biztonsági védelem növelése érdekében. A dokumentum megismerkedhet ehhez példa keresztül.

> [AZURE.NOTE] A dokumentum egy példa telepítési használatával vezet be a szolgáltatást.  Ez a cikk részletesen ismerteti nem.

## <a name="implement-the-recommendation"></a>Az ajánlási megvalósítása

1. Az a **javaslatok** a lap válassza a **következő generációs tűzfalat hozzáadása**lehetőséget.
![A következő generációs tűzfal hozzáadása][1]

2. Jelölje ki a **következő generációs tűzfalat hozzáadása** lap zárólap.
![Jelölje ki a végpontok][2]

3. A második **generációs következő tűzfalat hozzáadása** lap megnyitása Választhatja a meglévő megoldás használhatja, ha elérhető, vagy hozhat létre egy újat. Ebben a példában érhetők el nem létező megoldásokat, azt egy új NGFW létre fogja hozni.
![Hozzon létre új következő generációs tűzfal][3]

4. Hozzon létre egy új NGFW, jelölje be a megoldást az integrált partnerek listájából. Ez a példa azt választja ki **Pont jelölőnégyzetet**.
![Jelölje be a következő generációs tűzfal megoldás][4]

5. A **Jelölőnégyzet pont** lap megnyitása és, akkor a partner megoldást információt. Jelölje ki az adatok lap **létrehozása** .
![Tűzfal információ lap][5]

6. A **Létrehozás virtuális gép** lap megnyitása Itt adhatja meg egy virtuális gép (virtuális), amely a NGFW fog futni léptetőnyíl vezérlőelem szükséges információkat. Kövesse a lépéseket, és adja meg a szükséges NGFW adatokat. Az OK gombra kattintva alkalmazza.
![Virtuális gép NGFW futtatásához létrehozása][6]

## <a name="route-traffic-through-ngfw-only"></a>Útvonal forgalom az NGFW csak

Vissza a **javaslatok** lap. Új bejegyzés után a hozzáadott egy NGFW keresztül biztonság otthon, a hívott **NGFW csak útvonal áthaladó**hozták létre. Ennek ellenére hoz létre, csak akkor, ha a NGFW biztonsági központon keresztül telepítette. Ha internetes végpontok, a biztonság otthon javasol úgy beállítani, hogy a virtuális keresztül a NGFW bejövő kényszerítése hálózati biztonsági csoport szabályokat.

1. A **javaslatok lap**jelölje be a **csak NGFW útvonal áthaladó**.
![Útvonal forgalom az NGFW csak][7]

2. Ekkor megnyílik a **keresztül NGFW csak a forgalmat** , amely felsorolja a VMs is irányítja a forgalmat, a lap. Jelölje ki a virtuális a listából.
![Jelölje ki a virtuális][8]

3. Megjelenik egy lap a kijelölt virtuális és kapcsolódó a bejövő szabályok rajta. Leírás lehetséges következő lépések további információt biztosít. Válassza a **Bejövő szabályok szerkesztéséhez** egy bejövő szabály szerkesztése folytatásához. Az elvárásoknak, hogy **forrás** értéke nem **minden** az internetes végpontok a NGFW kapcsolódik. Többet szeretne tudni a bejövő szabály tulajdonságait, olvassa el a [NSG szabályok](../virtual-network/virtual-networks-nsg.md#nsg-rules).
![Állítsa be a hozzáférés korlátozása érdekében szabályokat][9]
![bejövő szabály szerkesztése][10]

## <a name="see-also"></a>Lásd még:

A dokumentum mutatott megvalósításáról a biztonság otthon ajánlási "Vegye fel a következő generációs tűzfal." Többet szeretne tudni a NGFWs és az ellenőrzés pont partner megoldás, olvassa el az alábbiakat:

- [Következő generációs tűzfal](https://en.wikipedia.org/wiki/Next-Generation_Firewall)
- [Jelölje be a pont vSEC](https://azure.microsoft.com/marketplace/partners/checkpoint/check-point-r77-10/)

Többet szeretne tudni a biztonság otthon, olvassa el az alábbiakat:

- [Az Azure biztonság otthon biztonsági házirendek beállítása](security-center-policies.md) – megtudhatja, hogy miként biztonsági házirendek beállítása.
- [Kezelése biztonsági ajánlások az Azure biztonság otthon](security-center-recommendations.md) névre hallgat – ismerje meg, hogyan javaslatok segítenek az Azure erőforrások védelme.
- [Biztonsági állapot ellenőrzése Azure biztonság otthon](security-center-monitoring.md) – útmutató a Lync-állapota az Azure erőforrások.
- [Kezelése és válaszol a biztonsági figyelmeztetések az Azure biztonság otthon](security-center-managing-and-responding-alerts.md) – megtudhatja, hogy miként kezelheti, és a biztonsági figyelmeztetések válaszolni.
- [Figyelés partnermegoldások Azure biztonsági központ](security-center-partner-solutions.md) – megtudhatja, hogy miként figyelheti a partnerek megoldásaival állapotának.
- [Azure biztonsági központja – gyakori kérdések](security-center-faq.md) – keresés gyakran ismételt kérdések a szolgáltatással.
- [Azure biztonsági blog](http://blogs.msdn.com/b/azuresecurity/) – keresés blog bejegyzések Azure biztonság és megfelelőség kapcsolatban.

<!--Image references-->
[1]: ./media/security-center-add-next-gen-firewall/add-next-gen-firewall.png
[2]: ./media/security-center-add-next-gen-firewall/select-an-endpoint.png
[3]: ./media/security-center-add-next-gen-firewall/create-new-next-gen-firewall.png
[4]: ./media/security-center-add-next-gen-firewall/select-next-gen-firewall.png
[5]: ./media/security-center-add-next-gen-firewall/firewall-solution-info-blade.png
[6]: ./media/security-center-add-next-gen-firewall/create-virtual-machine.png
[7]: ./media/security-center-add-next-gen-firewall/route-traffic-through-ngfw.png
[8]: ./media/security-center-add-next-gen-firewall/select-vm.png
[9]: ./media/security-center-add-next-gen-firewall/configure-rules-to-limit-access.png
[10]: ./media/security-center-add-next-gen-firewall/edit-inbound-rule.png
