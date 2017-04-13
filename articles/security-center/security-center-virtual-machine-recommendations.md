<properties
   pageTitle="A virtuális gépeken futó az Azure biztonság otthon védelme |} Microsoft Azure"
   description="A dokumentum címét, javaslatok az Azure biztonság otthon, amelyek segítségével védelme a virtuális gépeken futó, és biztonsági házirendek megfelelően maradjon."
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
   ms.date="09/25/2016"
   ms.author="terrylan"/>

# <a name="protecting-your-virtual-machines-in-azure-security-center"></a>A virtuális gépeken futó az Azure biztonság otthon védelme

Azure biztonság otthon elemzi a biztonsági állapot Azure erőforrásait. Biztonság otthon azonosítja a potenciális biztonsági rés, amikor javaslatok, amely végigvezeti Önt a folyamaton, konfigurálása a szükséges vezérlőelemeket hoz létre.  Javaslatok Azure erőforrástípus alkalmazása: virtuális gépeken futó VMs hálózatot, és SQL-alkalmazásokat.

Ez a cikk VMs vonatkozó javaslatok foglalkozik.  Virtuális javaslatok központ körül adatgyűjtés rendszer frissítések, antimalware, kiépítési alkalmazásával titkosítja a virtuális merevlemezeken, és így tovább.  A hivatkozások segít megérteni a rendelkezésre álló virtuális javaslatok, és mindegyik fog mire alkalmazhat, ha az alábbi táblázatban használják.

## <a name="available-vm-recommendations"></a>Rendelkezésre álló virtuális javaslatok

|Javaslat|Leírás|
|-----|-----|
|[Engedélyezése előfizetésben adatgyűjtés](security-center-enable-data-collection.md)|Javasolja, hogy bekapcsolja az egyes az előfizetések és az összes virtuális gépeken futó (VMs) a biztonsági házirendben adatgyűjtés az előfizetések.|
|[Operációs rendszer biztonsági ismételt](security-center-remediate-os-vulnerabilities.md)|Ajánlja igazíthatja a az operációs rendszer konfigurációk ajánlott konfiguráció szabályokkal, például nem engedélyezik a jelszavak szeretné menteni.|
|[Rendszer-frissítések telepítése](security-center-apply-system-updates.md)|Javasolja, hogy beállítaná hiányzó rendszer biztonsági és a fontos frissítések VMs.|
|[Indítsa újra a rendszer frissítés után](security-center-apply-system-updates.md#reboot-after-system-updates)|Javasolja, indítsa újra a virtuális alkalmazásának rendszerben frissítéseket a folyamat befejezéséhez.|
|[Az Endpoint Protection telepítése](security-center-install-endpoint-protection.md)|Javasolja, hogy a VMs (csak a Windows VMs) antimalware programok kiépítése.|
|[Az Endpoint Protection állapot riasztások feloldása](security-center-resolve-endpoint-protection-health-alerts.md)|Javasolja, hogy a endpoint protection hibák megoldásához.|
|[Virtuális ügynök engedélyezése](security-center-enable-vm-agent.md)|Lehetővé teszi, hogy mely VMs megkövetelése a virtuális Agent. A virtuális Agent kell telepíteni kell VMs kiépítése javítás eredeti a beolvasás és antimalware programok végzett vizsgálatot. A virtuális Agent alapértelmezés szerint a a Microsoft Azure piactéren lévő telepített VMs telepítve van. A [virtuális ügynök és bővítmények – 2 rész](http://azure.microsoft.com/blog/2014/04/15/vm-agent-and-extensions-part-2/) a cikkben információt a virtuális Agent telepítési.|
| [Lemezen titkosítási alkalmazása](security-center-apply-disk-encryption.md) |Javasolja, hogy titkosítsa-e a virtuális merevlemezeken Azure lemez titkosítással (Windows és Linux VMs). Az operációs rendszer és az adatok kötet a virtuális a titkosítási ajánlott.|
| [Operációs rendszer frissítése](security-center-update-os-version.md) | Javasolja, hogy az operációs rendszeren verziók frissítése a Felhőbeli szolgáltatás elérhető legújabb verzióját az operációs rendszer család számára.  Felhőbeli szolgáltatásokkal kapcsolatos további információért olvassa el a a [Cloud Services – áttekintés](../cloud-services/cloud-services-choose-me.md)című témakört. |
| [Nincs telepítve a biztonsági értékelése](security-center-vulnerability-assessment-recommendations.md) | Javasolja, rés értékelési megoldás telepítése a virtuális. |
| [Biztonsági ismételt](security-center-vulnerability-assessment-recommendations.md#review-recommendation) | Lásd: a biztonsági értékelési telepítve van a virtuális a megoldás által talált rendszer és az alkalmazások biztonsági teszi lehetővé. |

## <a name="see-also"></a>Lásd még:

Szeretne többet megtudni más Azure erőforrástípus vonatkozó javaslatok, olvassa el az alábbiakat:

- [Az alkalmazások az Azure biztonság otthon védelme](security-center-application-recommendations.md)
- [A hálózat az Azure biztonság otthon védelme](security-center-network-recommendations.md)
- [Az Azure SQL Azure-biztonság otthon szolgáltatásba védelme](security-center-sql-service-recommendations.md)

Többet szeretne tudni a biztonság otthon, olvassa el az alábbiakat:

- [Az Azure biztonság otthon biztonsági házirendek beállítása](security-center-policies.md) – a Azure előfizetések és erőforrás-csoportok biztonsági házirendek konfigurálásának ismertetése.
- [Kezelése és válaszol a biztonsági figyelmeztetések az Azure biztonság otthon](security-center-managing-and-responding-alerts.md) – megtudhatja, hogy miként kezelheti, és a biztonsági figyelmeztetések válaszolni.
- [Azure biztonsági központja – gyakori kérdések](security-center-faq.md) – keresés gyakran ismételt kérdések a szolgáltatással.
