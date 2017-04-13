
<properties
   pageTitle="Azure útmutatást |} minták és eljárások |} Microsoft Azure"
   description="Azure hivatkozás architektúrákban"
   services=""
   documentationCenter="na"
   authors="bennage"
   manager="marksou"
   editor=""
   tags=""/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/24/2016"
   ms.author="christb"/>

# <a name="azure-reference-architectures"></a>Azure hivatkozás architektúrákban

[AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

_A tartalom aktív fejlesztés szerepel. Célszerű ma, így azt készít, előzetes elérhető. A visszajelzés érték azt._

A hivatkozás architektúrákban esetben szerint vannak rendezve, a több kapcsolódó architektúrákban csoportosítva.
Minden egyes architektúra ajánlott eljárások és elfogadott lépéseket, valamint egy végrehajtható összetevő, amely a javaslatok, úgy kínál.
A architektúrákban közül számos csak a progresszív; felépíteni fölött előző architektúrákban, hogy kevesebb követelményeik vannak.

## <a name="designing-your-infrastructure-for-resiliency"></a>A infrastruktúra tűrőképessége kialakítása

Sorozat ajánlott eljárások a optimális virtuális konfigurációt kezdődik, és a feladatátvételi több területre környezetben culminates.

- [Virtuális Windows Azure fut](guidance-compute-single-vm.md)
- [Egy Linux virtuális futó Azure](guidance-compute-single-vm-linux.md)
- [Több VMs méretezhetőség és elérhetősége fut](guidance-compute-multi-vm.md)
- [A Windows VMs-N szintű architektúrájának fut](guidance-compute-n-tier-vm.md)
- [Az N szintű architektúrájának Linux VMs fut](guidance-compute-n-tier-vm-linux.md)
- [Futó Windows VMs több régióban magas elérhetősége](guidance-compute-multiple-datacenters.md)
- [Több területre magas elérhetőség Linux VMs fut](guidance-compute-multiple-datacenters-linux.md)

## <a name="connecting-your-on-premises-network-to-azure"></a>A helyszíni hálózaton csatlakoztatása az Azure

A meglévő hálózat csatlakoztatása Azure módjai bemutató indítása a sorozat. Ezután azt kibővíti az elérhetőség és biztonsági terjed ki.

- [A hibrid hálózat architektúrája az Azure végrehajtása és a helyszíni VPN](guidance-hybrid-network-vpn.md)
- [Végrehajtási egy hibrid hálózat architektúrája és a készült Azure ExpressRoute](guidance-hybrid-network-expressroute.md)
- [Egy könnyen hozzáférhető hibrid hálózat architektúrája végrehajtása](guidance-hybrid-network-expressroute-vpn-failover.md)

## <a name="securing-your-hybrid-network"></a>A hibrid hálózat biztonságossá tétele

Sorozat Azure-ban, a helyszíni adatközponthoz és az internetről származó biztonságos kapcsolatok létrehozásával DMZ bevált eljárások foglalkozik.

- [A DMZ Azure és a helyszíni adatközponthoz között végrehajtása](guidance-iaas-ra-secure-vnet-hybrid.md)
- [A DMZ Azure és az Internet között végrehajtása](guidance-iaas-ra-secure-vnet-dmz.md)

## <a name="providing-identity-services"></a>Identitás szolgáltatásokat nyújtó

Sorozat indítása bemutatja, hogyan lehet Azure Active Directory használatával adja meg a felhasználói hitelesítés Azure-ban. Azt kibontja az összetett esetek kiterjesztése a ÖSSZEADJA infrastruktúra Azure, és használja az ADFS-meghatalmazás terjed ki.

- [Azure Active Directory végrehajtása](./guidance-identity-aad.md)
- [Azure Active Directory szolgáltatásaihoz (ÖSSZEADJA) kiterjesztés](./guidance-identity-adds-extend-domain.md)
- [Azure Active Directory címtár szolgáltatások (ÖSSZEADJA) erőforráserdők létrehozása](./guidance-identity-adds-resource-forest.md)
- [Azure Active Directory összevonási szolgáltatások (ADFS) végrehajtása](./guidance-identity-adfs.md)

## <a name="architecting-scalable-web-application-using-azure-paas"></a>Azure PaaS használatával architecting méretezhető webalkalmazáshoz

Sorozat javaslatok méretezhető és könnyen hozzáférhető web Apps alkalmazások megépítése foglalkozik. 

- [Egyszerű webalkalmazáshoz](guidance-web-apps-basic.md)
- [A webes alkalmazásokban méretezhetőség javításához](guidance-web-apps-scalability.md)
- [A magas rendelkezésre webalkalmazáshoz](guidance-web-apps-multi-region.md)
