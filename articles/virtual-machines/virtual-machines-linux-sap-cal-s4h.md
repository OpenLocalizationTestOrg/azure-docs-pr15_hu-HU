<properties 
pageTitle="S/4 HANA vagy FF/4 HANA a az Azure virtuális telepítése |} Microsoft Azure" 
description="S/4 HANA vagy egy Azure virtuális a Sávszélesség/4 HANA üzembe helyezése" 
services="virtual-machines-linux" 
documentationCenter="" 
authors="hermanndms" 
manager="timlt" 
editor="" 
tags="azure-resource-manager" 
  keywords=""/> 
<tags 
  ms.service="virtual-machines-linux" 
  ms.devlang="na" 
  ms.topic="article" 
  ms.tgt_pltfrm="vm-linux" 
  ms.workload="infrastructure-services" 
  ms.date="09/15/2016" 
  ms.author="hermannd"/> 


# <a name="deploying-s4-hana-or-bw4-hana-on-microsoft-azure"></a>S/4 HANA vagy a Microsoft Azure FF/4 HANA üzembe helyezése 

Ez a cikk a Microsoft Azure keresztül SAP felhő készülék tár 3.0 S/4 HANA üzembe ismerteti.
A számítógépen készített képernyőképeket megjelenítése a folyamat lépésenkénti. Más SAP HANA-alapú megoldást üzembe helyezné FF/4 HANA a folyamat szemszögéből ugyanígy működik, mint. Csak egy rendelkezik jelöljön ki egy másik megoldást.

Induljon SAP felhő készülék Library (SAP-CAL) válassza [az alábbi](https://cal.sap.com/). Az SAP az új [SAP felhő készülék tár 3.0](http://scn.sap.com/community/cloud-appliance-library/blog/2016/05/27/sap-cloud-appliance-library-30-came-with-a-new-user-experience)kapcsolatos blog van. 


Az alábbi képernyőképek lépésenkénti megjelenítése a Microsoft Azure S/4 HANA telepítéséről. A folyamat az egyéb megoldások likeBW/4 HANA ugyanígy működik.


![](./media/virtual-machines-linux-sap-cal-s4h/s4h-pic-1b.jpg)

Az első képen az látható, a Microsoft Azure elérhető összes SAP CAL HANA épülő megoldásokat.
Exemplarily a "SAP S/4 HANA helyszíni edition" a folyamat folyamatát választott (megoldást a képernyő alján kattintson a kép).

![](./media/virtual-machines-linux-sap-cal-s4h/s4h-pic-2.jpg)

SAP-CAL új fiók először fel hozható létre. Jelenleg az Azure - kétféle folytatási normál Azure és Azure meg, hogy a partner 21Vianet üzemelteti Kínában kontinens.

![](./media/virtual-machines-linux-sap-cal-s4h/s4h-pic3b.jpg)

Kattintson egy rendelkezik írja be az Azure-portálra – lásd még: további lefelé hogyan szerezheti be a program Azure előfizetés Azonosítót. Ezután az Azure management tanúsítványt kell letölteni.

![](./media/virtual-machines-linux-sap-cal-s4h/s4h-pic6b.jpg)

Az új Azure-ban portál egy keresi meg a "Előfizetések" bal oldalán. Kattintson arra a felhasználói minden aktív előfizetések megjelenítése.

![](./media/virtual-machines-linux-sap-cal-s4h/s4h-pic7b.jpg)

Az előfizetések valamelyikének kiválasztásával és lehetőséget, majd a "Management tanúsítványok" ebből a cikkből megtudhatja, hogy nincs fogalma új "szolgáltatás rendszerbiztonsági" használata az új Azure erőforrás-kezelő modell.
SAP-CAL még nem módosítani ezt a modellt, és továbbra is megköveteli a "klasszikus" modell és a korábbi Azure portál kezelése tanúsítványok végezhető.

![](./media/virtual-machines-linux-sap-cal-s4h/s4h-pic4b.jpg)

Itt láthatja egyet az előbbi Azure-portálra. A feltöltés management tanúsítvány SAP CAL hozhat létre virtuális gépeken futó ügyfél-előfizetés belül az engedélyek adja vissza. A "ELŐFIZETÉSEK" a lapon egyszerre megtalálja az előfizetés azonosítója, amelynek az SAP-CAL portálon kell megadni.

![](./media/virtual-machines-linux-sap-cal-s4h/s4h-pic5.jpg)

A második lap ajánlatos kattintson a kezelés tanúsítvány előtt az SAP-CAL letöltött feltöltése.

![](./media/virtual-machines-linux-sap-cal-s4h/s4h-pic8.jpg)

Jelölje ki a letöltött tanúsítvány-fájlt megjelenik egy kisméretű párbeszédpanel.

![](./media/virtual-machines-linux-sap-cal-s4h/s4h-pic9.jpg)

Miután a tanúsítvány feltöltése SAP CAL és Azure előfizetés ügyfél közötti kapcsolat is vizsgálni SAP CAl belül. Kis üzenet kell bukkan fel, amely azt jelenti, hogy a kapcsolat érvényes.

![](./media/virtual-machines-linux-sap-cal-s4h/s4h-pic10.jpg)

A fiók beállítása után egy ki kell választania kell telepíthető, és hozzon létre egy példány megoldás.
A "egyszerű" mód valójában trivial. Írja be a példány nevét, válassza az Azure régió, valamint definiálhatók a fő jelszavának a megoldást.

![](./media/virtual-machines-linux-sap-cal-s4h/s4h-pic11.jpg)

Egy kis időt, attól függően, hogy méretétől és összetettségétől a megoldás (becslés képlettel SAP CAL) után látható a "aktív" és a használatra kész. Érdemes rendkívül egyszerű.

![](./media/virtual-machines-linux-sap-cal-s4h/s4h-pic12.jpg)

Megjeleníti a megoldás egy bizonyos részleteit láthatja, milyen típusú VMs telepítve volt. Ebben az esetben a különböző méretű és célját, három Azure VMs lettek létrehozva.

![](./media/virtual-machines-linux-sap-cal-s4h/s4h-pic13.jpg)

Az Azure portálon a virtuális gépeken futó azonos nevű példányt az SAP-CAL adott kezdési találhatók.

![](./media/virtual-machines-linux-sap-cal-s4h/s4h-pic14b.jpg)

Mostantól lehetőség a megoldást a Csatlakozás gombra az SAP-CAL portálon keresztül szeretne csatlakozni. A kis párbeszédpanel az összes az alapértelmezett hitelesítő adatait, és a megoldás használata leíró felhasználói segédvonalhoz hivatkozást tartalmaz.

![](./media/virtual-machines-linux-sap-cal-s4h/s4h-pic15.jpg)

Egy másik, hogy az ügyfél Windows virtuális bejelentkezni, és kezdjen például az előre konfigurált SAP-grafikus.







