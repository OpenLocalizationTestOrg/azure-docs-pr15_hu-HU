
<properties 
    pageTitle="Azure RemoteApp kívül áttelepítésének beállítások |} Microsoft Azure" 
    description="Tudjon meg többet való áttelepítés Azure RemoteApp ki a beállításokat." 
    services="remoteapp" 
    documentationCenter="" 
    authors="ericorman" 
    manager="mbaldwin" />

<tags 
    ms.service="remoteapp" 
    ms.workload="compute" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/06/2016" 
    ms.author="elizapo" />

# <a name="options-for-migrating-out-of-azure-remoteapp"></a>Azure RemoteApp kívül áttelepítésének beállításai

> [AZURE.IMPORTANT]
> Azure RemoteApp megszakad alatt. Olvassa el a további információt a [közlemény](https://go.microsoft.com/fwlink/?linkid=821148) .

Ha leállt Azure RemoteApp használata miatt [elavulása bejelentése](https://go.microsoft.com/fwlink/?linkid=821148) , vagy mert befejezte a kiértékelés, kell egy másik alkalmazás szolgáltatás elhagyja Azure RemoteApp áttelepítése. Kétféleképpen különböző való áttelepítés: egy önálló felügyelt (más néven infrastruktúra szolgáltatásként [IaaS]) üzembe vagy egy teljes körű felügyelt (gyakran hívott Platform szolgáltatásként) vagy a szoftver szolgáltatásként [PaaS/szoftver] kínáló. 

Önkiszolgáló IaaS önálló telepítés kezeli, üzemeltetett, és meg közvetlenül a virtuális gépeken futó (VMs) vagy a fizikai rendszeren telepített tulajdonában. A másik végén egy teljes körű felügyelt PaaS/szoftver kínáló Azure RemoteApp például - partner biztosít fölött kezeli műveleti távelérési megoldás szolgáltatás réteg és karbantartási, közben, mint az ügyfél, végezze el néhány kép és az alkalmazás-kezelés.

További információért és a különböző Office beállítás példák Olvasson tovább.    

## <a name="self-managed-iaas-solutions"></a>Önálló felügyelt (IaaS) megoldások

### <a name="rds-on-iaas"></a>**A IaaS távoli asztali** 
Telepítheti a natív munkamenet-alapú távoli asztali szolgáltatásokat (a Windows Server) alkalmazó-telepítéshez RemoteApp vagy asztali számítógépek helyszíni vagy szolgáltatott környezetben (Azure VMs a like). Távoli asztali IaaS környezetekben a legjobb vevők már ismerős és, amelyek a távoli asztali telepítések meglévő technikai szakértelem. 

> [AZURE.NOTE]
> Mennyiségi licencelési a szoftver garancia (Nyelvű) a távoli asztali ügyfél-hozzáférési licencek való telepítésének beállítás szükséges.

Üzembe helyezése a Azure VMs távoli asztali telepítés és javítása (olvassa el az [Áttekintés](https://blogs.technet.microsoft.com/enterprisemobility/2015/07/13/azure-resource-manager-template-for-rds-deployment/) , majd [válassza el őket](https://aka.ms/rdautomation)) sablonok használata esetén minden eddiginél egyszerűbben. Az [Automatikus méretezés parancsfájl](https://gallery.technet.microsoft.com/scriptcenter/Automatic-Scaling-of-9b4f5e76), használatával erőforrásokkal Azure klasszikus telepítési modell (nem Azure erőforrás modell erőforrások) belül Azure RemoteApp azonos rugalmas méretezési lehetőségek elérheti, bár további testreszabásokat és beállításokat. Azure VMs a távoli asztali telepítésekor nyújtott támogatás keresztül [Azure támogatja](https://azure.microsoft.com/support/plans/), a azonos támogatási szakemberei támogatott, az Azure RemoteApp. Akkor is első költség [Azure ügyfélszolgálat](https://azure.microsoft.com/support/plans/)megkeresésével a meglévő használatát alapján ad becslést vagy végezhet számításokat saját maga használata egy hamarosan kell költség Számológép kiadott.  Emellett az N-sorozat VMs (jelenleg a magánjellegű előzetes verzió) is hozzáadhat vGPU – a Ignite munkamenetben [távoli asztali javítása a Windows Server 2016](https://myignite.microsoft.com/videos/2794) hasznosítására kíváncsivá vált vGPU hozzáadásáról és arról, hogy miként.   

Még a üzembe megkönnyítése [A Windows Server 2012 R2](http://aka.ms/rdsonazure) és [Windows Server 2016](http://aka.ms/rdsonazure2016) telepítési lépésenkénti útmutatót kínálunk. Nézze meg a [távoli asztali blog](https://blogs.technet.microsoft.com/enterprisemobility/?product=windows-server-remote-desktop-services) a legfrissebb híreket.
 
### <a name="citrix-on-iaas"></a>**A IaaS Citrix** 
A natív Citrix a munkamenet-alapú XenApp vagy XenDesktop telepítésének is lehet telepítve a helyszíni vagy szolgáltatott környezetben (például a Azure VMs). 

Nézze meg a részletes telepítési útmutató, [Citrix XA 7.6 a Azure](http://www.citrixandmicrosoft.com/Documents/Citrix-Azure Deployment Guide-v.1.0.docx), további információt. További információ [a Azure Citrix](http://www.citrixandmicrosoft.com/Solutions/AzureCloud.aspx), beleértve a ár Számológép. [Citrix lépjen kapcsolatba](http://citrix.com/English/contact/index.asp) a beállításokat tartalmazó megvitatása is megtalálhatók.

## <a name="fully-managed-paassaas-offerings"></a>Teljes körű felügyelt (PaaS/szoftver) ajánlataiban

### <a name="citrix-cloud"></a>**Citrix felhő** 
[Meglévő megoldás Citrix a felhőben](https://www.citrix.com/products/citrix-cloud/), architecturally azonos Citrix XenApp Express. Citrix [50 % -os árengedmény promóciós](https://www.citrix.com/blogs/2016/10/03/special-promotion-for-microsoft-azure-remoteapp-customers/) kínál a meglévő Azure RemoteApp ügyfeleknek. 

### <a name="citrix-xenapp-express-in-tech-preview"></a>**Citrix XenApp Express (a technikai előzetes verzió)**
[Regisztrálni a technikai előzetes verzió](http://now.citrix.com/remoteapp)és a [munkamenet Ignite](https://myignite.microsoft.com/videos/2792) megtekintés (kezdve 20:30 perc). XenApp Express megegyezik architecturally Citrix felhőbe azzal a különbséggel egyszerűbb kezelés felhasználói felület és más szolgáltatások és funkciók, amelyek hasonlítanak Azure RemoteApp tartalmazza. 

További tudnivalók [A Citrix XenApp Express](http://now.citrix.com/remoteapp).   

### <a name="citrix-service-provider-program"></a>**Citrix Service Provider Program** 
A Citrix Service Provider Program megkönnyíti az egyszerűség kedvéért a számítások SMB, hogy virtuális felhőalapú előadásához szolgáltatók számára kínáló azokat a szolgáltatásokat, a könnyen, befizetések modell vágynak. Citrix szolgáltatók Microsoft SPLA kisvállalatoknak a nagyobb, és bontsa ki a távoli asztali platform-befektetések bármilyen eszközzel, bárhonnan elérheti, a lehető alkalmazás támogatás gazdag élmény, nagyobb biztonság és méretezhetőség. Citrix szolgáltatók viszont további előfizetők vonzhat webhelyére, fokozhatja a felhasználói elégedettséget, és azok működési költségek csökkentése. [Tudjon meg többet](http://www.citrix.com/products/service-providers.html) , vagy [keressen egy partnert](https://www.citrix.com/buy/partnerlocator.html).

### <a name="microsoft-hosted-service-provider"></a>**A Microsoft üzemeltetett szolgáltató** 
Üzemeltető partnerek általában a teljes körű felügyelt üzemeltetett Windows asztalra, és alkalmazás szolgáltatása, amely tartalmazhat, az Azure erőforrások, operációs rendszerek, alkalmazások és a partner segítségével segélyszolgálat kezelése a Microsoft és más szoftver szolgáltatók együtt engedélyezni az előfizetői hozzáférés licenc (SAL) változatlan Service Provider licencszerződést folyamatban van licencvásárlás kínálnak. Az alábbi információk információt nyújt a szolgáltatónak, amely segíti az Azure RemoteApp áttelepítés ügyfelek specializálódott részét kapcsolattartási adatait. Nézze meg a befejeződött a távoli asztali elérési utat és a felmérés tanulási IaaS a [teljes listáját a szolgáltatók is](http://aka.ms/rdsonazurecertified) .  

#### <a name="aspex"></a>**ASPEX**
A felhőben való ISV és a külső specializes [ASPEX](http://www.aspex.be/en) "a jelenlegi felhő beállítások optimalizálása szeretné. ASPEX felajánlja a kezelt szolgáltatások, devops és tanácsadás széles köre.  

Elsődleges helye: Antwerpen, Belgium

A művelet terület: nyugati Európa

A partner állapota: [ezüst](https://partnercenter.microsoft.com/pcv/solution-providers/aspex_9397f5dd-ebdd-405b-b926-19a5bda61f7a/cfe00bac-ea36-4591-a60b-ec001c4c3dff)

Microsoft Cloud-szolgáltató: Igen

Munkamenet RemoteApp és asztali megoldásokat kínálnak: Igen, mindkét

Azure RemoteApp áttelepítési megoldások: Igen, [megtudhatja, hogy](https://www.aspex.be/en/azure-remote-apps)

**Felelős:**

- Telefon: +3232202198
- Levelek:[info@aspex.be](mailto:info@aspex.be)
- Web: [http://cloud.aspex.be/contact-ara-0](http://cloud.aspex.be/contact-ara-0)

#### <a name="conexlink-platform-name-mycloudit"></a>**Conexlink (Platform neve: MyCloudIT)**
[MyCloudIT](http://www.mycloudit.com) egy platform, vagyis az automatizálási informatikai cégek egyszerűsítése, optimalizálása és az áttelepítés és távoli asztali számítógépek, távoli alkalmazások és a Microsoft Azure felhőalapú infrastruktúrájának kézbesítve méretezni. 

A MyCloudIT platform 30 %-kal költség Azure 95 %-kal csökkenti a telepítés idejét, és az ügyfél teljes informatikai infrastruktúrát áthelyezi a frissítését és néhány billentyűparancsra a felhőbe. Partnerek kezelhetők az ügyfelek globális irányítópultról, szolgáltatás végfelhasználók a világon például még soha, anélkül, hogy további általános vagy a teljes körű Azure tanfolyamok a bevételek nagyobb.  

Elsődleges helye: Dallas, TX, USA

A művelet terület: a világban

A partner állapota: [Gold](https://partnercenter.microsoft.com/pcv/solution-providers/conexlink_4298787366/843036_1?k=Conexlink)

Microsoft Cloud-szolgáltató: Igen

Munkamenet RemoteApp és asztali megoldásokat kínálnak: Igen, mindkét

Azure RemoteApp áttelepítési megoldások: Igen, [megtudhatja, hogy](https://mycloudit.com/remote-app-microsoft/)

**Felelős:**
- Péter Garoutte, az üzleti fejlesztés igazgató

   Telefon: 972-218-0741

   E-mailben:[brian.garoutte@conexlink.com](mailto:brian.garoutte@conexlink.com)

#### <a name="acuutech"></a>**Acuutech**
[Acuutech](http://www.acuutech.com) specializes nyújtó szolgáltatott asztali alkalmazások, a teljes asztalát és Szoftverfejlesztői alkalmazások szolgáltatások a Microsoft technológiai beépített alap globális ügyfélnek az Azure és a saját adatközpontokkal.

Elsődleges helye: London, Egyesült Királyság; Szingapúr; Houston, TX

A művelet terület: a világban

A partner állapota: Gold

Microsoft Cloud-szolgáltató: Igen

Munkamenet RemoteApp és asztali megoldásokat kínálnak: Igen, mindkét

Azure RemoteApp áttelepítési megoldások: Igen, [megtudhatja, hogy](http://www.acuutech.com/ara-migration/)

**Felelős:**

- Egyesült Királyság:

  5/6 York Házikó, Langston Road,

  Loughton, Essex IG10 3TQ
  
  Telefon: +44 (0) 20 8502 2155
 
- Szingapúr:

  Utca 100 Cecil #09-02, 
  
  A földgömböt szingapúri 069532
  
  Telefonszám: + 65 6709 4933
 
- Észak-Amerika: 

  A 3601 s Sandman út.
  
  Csomagja 200, Houston, TX 77098
  
  Telefon: +1 713 691 0800

## <a name="need-more-help"></a>További segítségre van szüksége?
Továbbra is szükséges válassza a Súgó és további kérdései vannak? Az alábbi módszerek segítségével kérjen segítséget. 

1.  A kapcsolatfelvételi e-mail [arainfo@microsoft.com](mailto:arainfo@microsoft.com).
2.  Lépjen kapcsolatba a [támogatási Azure](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade). Indítsa el az [Azure támogatja az esetet](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade)megnyitásával.
3.  Hívja fel az us. [Értékesítési helyi szám keresése hivatkozásra](https://azure.microsoft.com/overview/sales-number/).
