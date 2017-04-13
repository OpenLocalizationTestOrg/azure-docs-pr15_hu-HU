<properties
   pageTitle="Hálózati biztonsági csoportokat az Azure biztonság otthon engedélyezése |} Microsoft Azure"
   description="A dokumentum bemutatja, hogyan tudnak megvalósítani az Azure biztonság otthon ajánlást **Hálózati biztonsági csoportok engedélyezése**."
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

# <a name="enable-network-security-groups-in-azure-security-center"></a>Hálózati biztonsági csoportokat az Azure biztonság otthon engedélyezése

Azure biztonság otthon javasol hálózati biztonsági csoport (NSG) engedélyezése, ha egy nem engedélyezett. NSGs Access vezérlőelem-lista (vezérlés) szabályok, amelyek lehetővé teszik, vagy megtagadja a hálózati forgalmat a virtuális példányaiban virtuális hálózatban listáját tartalmazza. Lehet, hogy NSGs alhálózat vagy a alhálózat egyes virtuális példányokat társított. Ha egy NSG alhálózat társítva, vezérlés szabályokat alkalmazni alhálózat a virtuális példányok. Ezeken kívül az egyes virtuális forgalom korlátozva lehet egy NSG közvetlenül az, hogy virtuális társításával további. További című témakörben találhat további [egy hálózati biztonsági csoport (NSG) tartalma?](../virtual-network/virtual-networks-nsg.md)

Ha nincs engedélyezve NSGs, biztonság otthon két javaslatok jelenítsen meg: hálózati biztonsági csoportok engedélyezése a alhálózat és a hálózati biztonsági csoportok engedélyezése a virtuális gépeken futó. Kiválaszthatja, hogy melyik szint, alhálózat vagy virtuális NSGs alkalmazni.


> [AZURE.NOTE] A dokumentum egy példa telepítési használatával vezet be a szolgáltatást.  Ez a cikk részletesen ismerteti nem.

## <a name="implement-the-recommendation"></a>Az ajánlási megvalósítása

1. A **javaslatok** lap kijelölése **Engedélyezése hálózati biztonsági csoportok** alhálózat vagy a virtuális gépeken futó
![Hálózati biztonsági csoportok engedélyezése][1]

2. Ekkor megnyílik a **Hiányzó hálózati biztonsági csoportok beállítása** a alhálózat vagy virtuális gépeken futó, attól függően, hogy a kijelölt ajánlási lap. Válassza a alhálózat vagy egy virtuális gép egy NSG tartományi.

  ![Állítsa be a NSG alhálózat][2]

  ![Virtuális NSG konfigurálása][3]
3. A **választható hálózati biztonsági csoport** lap a Válasszon egy meglévő NSG vagy hozhat létre egy új NSG.

  ![Válassza a hálózat biztonsági csoport][4]

Ha létrehoz egy új NSG, kövesse a hozzon létre egy NSG és biztonsági szabályok [kezelése az Azure portálon NSGs](../virtual-network/virtual-networks-create-nsg-arm-pportal.md) .

## <a name="see-also"></a>Lásd még:

Ez a cikk a biztonság otthon ajánlási "A hálózati biztonsági csoportok engedélyezése" alhálózat vagy virtuális gépeken futó megvalósításáról mutatott. NSGs engedélyezésével kapcsolatos további információért olvassa el az alábbiakat:

- [Mi az, hogy egy hálózati biztonsági csoport (NSG)?](../virtual-network/virtual-networks-nsg.md)
- [Az Azure portálon NSGs kezelése](../virtual-network/virtual-networks-create-nsg-arm-pportal.md)

Többet szeretne tudni a biztonság otthon, olvassa el az alábbiakat:

- [Az Azure biztonság otthon biztonsági házirendek beállítása](security-center-policies.md) – a Azure előfizetések és erőforrás-csoportok biztonsági házirendek konfigurálásának ismertetése.
- [Kezelése biztonsági ajánlások az Azure biztonság otthon](security-center-recommendations.md) névre hallgat – ismerje meg, hogyan javaslatok segítenek az Azure erőforrások védelme.
- [Biztonsági állapot ellenőrzése Azure biztonság otthon](security-center-monitoring.md) – útmutató a Lync-állapota az Azure erőforrások.
- [Kezelése és válaszol a biztonsági figyelmeztetések az Azure biztonság otthon](security-center-managing-and-responding-alerts.md) – megtudhatja, hogy miként kezelheti, és a biztonsági figyelmeztetések válaszolni.
- [Figyelés partnermegoldások Azure biztonsági központ](security-center-partner-solutions.md) – megtudhatja, hogy miként figyelheti a partnerek megoldásaival állapotának.
- [Azure biztonsági központja – gyakori kérdések](security-center-faq.md) – keresés gyakran ismételt kérdések a szolgáltatással.
- [Azure biztonsági blog](http://blogs.msdn.com/b/azuresecurity/) – legújabb Azure biztonsági híreket és információkat.

<!--Image references-->
[1]: ./media/security-center-enable-nsg/enable-nsg.png
[2]:./media/security-center-enable-nsg/configure-nsg-for-subnet.png
[3]: ./media/security-center-enable-nsg/configure-nsg-for-vm.png
[4]: ./media/security-center-enable-nsg/choose-nsg.png
