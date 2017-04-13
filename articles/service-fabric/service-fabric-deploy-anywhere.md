<properties
   pageTitle="Azure Service háló fürt létrehozása a Windows Server és a Linux |} Microsoft Azure"
   description="Futtassa a Windows Server és Linux, ami azt jelenti is üzembe és host szolgáltatás háló tetszőleges alkalmazások szolgáltatás háló fürt futtathatja a Windows Server vagy Linux rendszerhez."
   services="service-fabric"
   documentationCenter=".net"
   authors="Chackdan"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotNet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/22/2016"
   ms.author="chackdan"/>

# <a name="create-service-fabric-clusters-on-windows-server-or-linux"></a>Szolgáltatás háló fürt létrehozása a Windows Server vagy Linux rendszerhez

Azure Service háló lehetővé teszi, hogy minden VMs vagy a Windows Server vagy Linux futtató számítógépeknek szolgáltatás háló fürt létrehozását. Ez azt jelenti, hogy tudja telepíteni és szolgáltatás háló alkalmazások futtatásához minden olyan környezetben, ha vannak olyan a Windows Server vagy Linux számítógépek összekapcsolt, azt a helyszíni Microsoft Azure vagy bármely felhő szolgáltatónál.

##<a name="create-service-fabric-clusters-on-azure"></a>Azure Service háló fürt létrehozása

Fürt létrehozása az Azure a befejeződött, akár egy erőforrás modell sablont, illetve az Azure-portálra. Olvassa el az [erőforrás-kezelő sablonnal szolgáltatás háló fürt létrehozása](service-fabric-cluster-creation-via-arm.md) vagy [létrehozása az Azure portálról szolgáltatás háló fürt](service-fabric-cluster-creation-via-portal.md) további információt.

## <a name="supported-operating-systems-for-clusters-on-azure"></a>Azure fürt támogatott operációs rendszerek

E operációs rendszert futtató VMs fürt létrehozhatnak áll:

* A Windows Server 2012 R2
* A Windows Server 2016 (miután általánosan elérhetővé van bejelentve)
* Linux Ubuntu 16.04 (a nyilvános előzetes verzió) 


##<a name="create-service-fabric-standalone-clusters-on-premise-or-with-any-cloud-provider"></a>Szolgáltatás háló önálló fürt létrehozása a helyszíni, vagy bármely felhő szolgáltatónál

Szolgáltatás háló tartalmaz egy telepítés csomag hozhat létre különálló szolgáltatás háló fürt a helyszíni, vagy bármely felhő szolgáltatónál

A Windows Server különálló szolgáltatás háló fürt beállításával kapcsolatos további tudnivalókért olvassa el a [szolgáltatás háló fürt létrehozása a Windows Server](service-fabric-cluster-creation-for-windows-server.md)

### <a name="any-cloud-deployments-vs-on-premises-deployments"></a>Bármely felhő telepítések és a helyszínen telepített összehasonlítása
A szolgáltatás háló fürt helyszíni létrehozásának folyamata hasonlít fürt létrehozásának bármely felhő VMs beállítási lehetőség. A kezdeti lépéseket a VMs kiépítése a felhőben vagy a helyszíni környezet használni kívánt vonatkoznak. Után egy sor olyan VMs a közöttük lévő engedélyezve van a hálózati kapcsolat majd lépésekkel állíthatja be a szolgáltatás háló csomag fürt beállításainak szerkesztése, és futtassa a fürt létrehozása és kezelése parancsfájlok azonosak. Ez biztosítja, hogy a Tudásbázis és működő és kezelni a szolgáltatás háló fürt élményét átruházható Ha úgy dönt, hogy új üzemeltetési környezet Megcélozhat.

### <a name="benefits-of-creating-standalone-service-fabric-clusters"></a>Különálló szolgáltatás háló fürt létrehozása előnyei
* Szabadon bármely felhő válassza a fürt tárolni.
* Szolgáltatás háló alkalmazásai, egyszer, futtatható több üzemeltetési környezetekben a minimális nem módosítható.
* Szolgáltatás háló alkalmazások létrehozásába ismerete egy üzemeltetési környezet a másikra végzi.
* Műveleti élményét fut, és kezelni a szolgáltatás háló fürtök végzi fölé egy környezetből között.
* Széles ügyfél vannak a működés határolatlan környezet kényszerek szolgáltatója.
* Megbízhatósága és kiterjedt kimaradások elleni védelem egy további réteget létezik, mert áthelyezheti a szolgáltatások egy másik telepítési környezet Ha adatszolgáltató központot vagy a felhőben van egy kiesési.

## <a name="supported-operating-systems-for-standalone-clusters"></a>Támogatott operációs rendszerek önálló fürtre vonatkozóan
Ön fürt létrehozhatnak VMs vagy az alábbi operációs rendszert futtató számítógépeken:

* A Windows Server 2012 R2
* A Windows Server 2016 (miután általánosan elérhetővé van bejelentve)
* Linux (hamarosan)

## <a name="advantages-of-service-fabric-clusters-on-azure-over-standalone-service-fabric-clusters-created-on-premises"></a>Különálló szolgáltatás háló fürtök fölé Azure Service háló fürt előnyei a helyszíni létrehozott

Azure Service háló fürt futó biztosít a helyszíni előnye lehetőséget, ha nincs telepítve az adott van szükség, ha futtatja a fürt, így, akkor javasoljuk, hogy a Azure futtatása. Azure, a biztosítunk integrációs más Azure funkciók és szolgáltatások, amely lehetővé teszi a műveletek és kezelése a fürt egyszerűbbé és megbízhatóbb.

* **Azure portálon:** Azure portál egyszerűen hozhat létre és kezelhet fürt.

* **Azure erőforrás-kezelő:** Az Azure erőforrás-kezelő használata lehetővé teszi, hogy a könnyű kezelés egységként a fürt által használt összes erőforrás, és leegyszerűsíti a költségek nyomon követését és a számlázásra.
* **Szolgáltatás háló fürt Azure erőforrásként** A szolgáltatás háló fürtre egy ARM erőforrás, mint az Azure ARM anyagainak modellezheti azt.
* **Integrálása az Azure-infrastruktúrát** Az operációs rendszer, hálózati és más frissítések elérhetőségéről és az alkalmazás megbízhatóságára javítható az alapul szolgáló Azure infrastruktúra szolgáltatást háló koordináták.  
* **Diagnosztika:** Azure, a biztosítunk integrációs Azure diagnosztikai és napló Analytics.
* **Automatikus méretezés:** Az Azure fürt kínálunk, amely beépített Automatikus méretezés funkciók miatt virtuális gép skála-készletek parancsot. A helyszíni és más felhőalapú környezetben hogy készíthet saját automatikus méretezés szolgáltatás vagy manuálisan a szolgáltatás háló közzététele API-k használata fürt méretezés skála.

## <a name="next-steps"></a>Következő lépések
Hozzon létre egy fürt VMs vagy a Windows Server operációs rendszert futtató számítógépeken: [szolgáltatás háló fürt létrehozása a Windows Server](service-fabric-cluster-creation-for-windows-server.md)

Hozzon létre egy fürt VMs vagy Linux futtató számítógépeknek: [Szolgáltatás háló Linux](service-fabric-linux-overview.md)
