<properties
  pageTitle="Azure DNS használatával más Azure szolgáltatásokhoz |} Microsoft Azure"
  description="Azure DNS használatának egyéb Azure szolgáltatás neve feloldásához megértése"
  services="dns"
  documentationCenter="na"
  authors="sdwheeler"
  manager="carmonm"
  editor=""
  tags="azure dns"
/>
<tags
  ms.service="dns"
  ms.devlang="na"
  ms.topic="article"
  ms.tgt_pltfrm="na"
  ms.workload="infrastructure-services"
  ms.date="09/21/2016"
  ms.author="sewhee"
/>

# <a name="using-azure-dns-with-other-azure-services"></a>Azure DNS használatával más Azure szolgáltatásokhoz

Azure DNS egy szolgáltatott DNS-kezelési és neve felbontás szolgáltatás. Lehetővé teszi, hogy az alkalmazások és szolgáltatások telepítette nyilvános DNS-nevek létrehozása az Azure-ban. A szolgáltatás a megfelelő típusú rekord hozzáadásával, egyszerűen létrehozása az Azure szolgáltatás egy nevet az egyéni tartomány.

* Dinamikusan kiosztott IP-címek létre kell hoznia egy CNAME DNS-rekord, amely a tartománynév-a szolgáltatás készült Azure rendeli. DNS-szabványokat megakadályozzák a használja a zóna csúcs egy CNAME rekordot.
* A felosztott statikus IP-címek létrehozhat egy tetszőleges nevet, többek között a zóna csúcs a _nyílt_ tartománynév DNS A rekordot.

Az alábbi táblázat is használható különféle Azure szolgáltatás támogatott bejegyzéstípusok ismertet. Az alábbi táblázatban látható, mint a Azure DNS csak az internetes erőforrásoknak támogatja a DNS-rekordok. Azure DNS-névfeloldás a belső, személyes címek nem használhatók.

| Azure szolgáltatás | Hálózati kapcsolat | Leírás |
|---------------|-------------------|-------------|
| Alkalmazás átjáró | Előtér nyilvános IP | A DNS A vagy CNAME rekordot is létrehozhat. |
| Terheléselosztó | Előtér nyilvános IP | A DNS A vagy CNAME rekordot is létrehozhat. Terheléselosztó is IPv6 nyilvános IP-címet, amely dinamikusan van-e hozzárendelve. Tehát létre kell hoznia az IPv6-címek CNAME rekordot. |
| Adatforgalom Manager | Nyilvános neve | Csak a megadott profilját a kezelő forgalom trafficmanager.net név leképezi CNAME hozhat létre. További tudnivalókért olvassa el a [hogyan forgalom Manager működik](../traffic-manager/traffic-manager-how-traffic-manager-works.md#traffic-manager-example)című témakört. |
| Felhőalapú szolgáltatás | Nyilvános IP | Felosztott statikus IP-címek létrehozhat egy DNS A-rekordot. Dinamikusan kiosztott IP-címek létre kell hoznia egy CNAME rekordot, amely a _cloudapp.net_ nevét. Ez a szabály azokat telepítik egy felhőalapú szolgáltatásba, mert a klasszikus portál létrehozott VMs vonatkozik. További tudnivalókért lásd: a [Configure Cloud Services az egyéni tartománynevet](../cloud-services/cloud-services-custom-domain-name-portal.md). |
| Alkalmazás szolgáltatás | Külső IP | Külső IP-címek létrehozhat egy DNS A-rekordot. Egyéb esetben létre kell hoznia egy CNAME rekordot, amely a azurewebsites.net nevét. További tudnivalókért lásd: a [feleltesse meg egy egyéni tartománynevet az Azure alkalmazásba](../app-service-web/web-sites-custom-domain-name.md) |
| Erőforrás-kezelő VMs | Nyilvános IP | Erőforrás-kezelő VMs beállíthatja, hogy nyilvános IP-címet. Egy nyilvános IP-címmel virtuális mögött egy terheléselosztó is lehet. A kihangosító DNS A vagy CNAME rekordjának létrehozása A saját nevét a virtuális a terheléselosztó a megkerülje használható. |
| Klasszikus VMs | Nyilvános IP | Klasszikus VMs a PowerShell használatával létrehozott vagy CLI konfigurálható (dinamikus vagy statikus fenntartott) virtuális címet. A DNS-CNAME vagy egy rekordot, illetve hozhat létre. |
