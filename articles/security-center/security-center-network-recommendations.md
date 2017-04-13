<properties
   pageTitle="A hálózat az Azure biztonság otthon védelme |} Microsoft Azure"
   description="A dokumentum címek javaslatok az Azure biztonság otthon, amelyek segítségével védelme Azure hálózatához, és biztonsági házirendek megfelelően maradjon."
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
   ms.date="08/04/2016"
   ms.author="terrylan"/>

# <a name="protecting-your-network-in-azure-security-center"></a>A hálózat az Azure biztonság otthon védelme

Azure biztonság otthon elemzi a biztonsági állapot Azure erőforrásait. Biztonság otthon azonosítja a potenciális biztonsági rés, amikor javaslatok, amely végigvezeti Önt a folyamaton, konfigurálása a szükséges vezérlőelemeket hoz létre.  Javaslatok Azure erőforrástípus alkalmazása: virtuális gépeken futó VMs hálózatot, és SQL-alkalmazásokat.

Ez a cikk a hálózat vonatkozó javaslatok foglalkozik.  Hálózati javaslatok központ a következő generációs tűzfalak, hálózati biztonsági csoportokat, konfigurálás bejövő szabályok és az egyéb körül.  A hivatkozások segít megérteni a rendelkezésre álló hálózati javaslatok, és mindegyik fog mire alkalmazhat, ha az alábbi táblázatban használják.

## <a name="available-network-recommendations"></a>Rendelkezésre álló hálózati javaslatok

|Javaslat|Leírás|
|-----|-----|
|[A következő generációs tűzfal hozzáadása](security-center-add-next-generation-firewall.md)|Javasolja, hogy felvette a következő generációs tűzfal (NGFW) Microsoft partnertől a biztonsági védelem növelése érdekében.|
|[Útvonal forgalom az NGFW csak](security-center-add-next-generation-firewall.md#route-traffic-through-ngfw-only)|Javasolja úgy beállítani, hogy hálózati biztonsági csoport (NSG) szabályok kényszerítése bejövő forgalmat a virtuális a NGFW keresztül.|
|[Hálózati biztonsági csoportok alhálózat vagy virtuális gépeken futó engedélyezése](security-center-enable-network-security-groups.md)|Alhálózat vagy VMs NSGs engedélyezését javasolja.|
|[Szemben lévő végpont interneten keresztül hozzáférés korlátozása](security-center-restrict-access-through-internet-facing-endpoints.md)|Azt a bejövő szabályok beállítása NSGs javasolja.|

## <a name="see-also"></a>Lásd még:

Szeretne többet megtudni más Azure erőforrástípus vonatkozó javaslatok, olvassa el az alábbiakat:

- [A virtuális gépeken futó az Azure biztonság otthon védelme](security-center-virtual-machine-recommendations.md)
- [Az alkalmazások az Azure biztonság otthon védelme](security-center-application-recommendations.md)
- [Az Azure SQL Azure-biztonság otthon szolgáltatásba védelme](security-center-sql-service-recommendations.md)

Többet szeretne tudni a biztonság otthon, olvassa el az alábbiakat:

- [Az Azure biztonság otthon biztonsági házirendek beállítása](security-center-policies.md) – a Azure előfizetések és erőforrás-csoportok biztonsági házirendek konfigurálásának ismertetése.
- [Kezelése és válaszol a biztonsági figyelmeztetések az Azure biztonság otthon](security-center-managing-and-responding-alerts.md) – megtudhatja, hogy miként kezelheti, és a biztonsági figyelmeztetések válaszolni.
- [Azure biztonsági központja – gyakori kérdések](security-center-faq.md) – keresés gyakran ismételt kérdések a szolgáltatással.
