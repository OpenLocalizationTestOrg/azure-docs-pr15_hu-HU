<properties 
pageTitle="SAP – IDES EHP7 SP3 telepíti az SAP ERP 6.0 a Microsoft Azure |} Microsoft Azure" 
description="SAP – IDES EHP7 SP3 SAP ERP 6.0 a Microsoft Azure üzembe helyezése" 
services="virtual-machines-windows" 
documentationCenter="" 
authors="hermanndms" 
manager="timlt" 
editor="" 
tags="azure-resource-manager" 
keywords=""/> 
<tags 
ms.service="virtual-machines-windows" 
ms.devlang="na" 
ms.topic="article" 
ms.tgt_pltfrm="vm-windows" 
ms.workload="infrastructure-services" 
ms.date="09/16/2016" 
ms.author="hermannd"/> 


# <a name="deploying-sap-ides-ehp7-sp3-for-sap-erp-60-on-microsoft-azure"></a>SAP – IDES EHP7 SP3 SAP ERP 6.0 a Microsoft Azure üzembe helyezése 

Ez a cikk ismerteti, hogyan szeretné telepíteni az SQL Server és a Windows operációs rendszer fut a Microsoft Azure keresztül SAP felhő készülék tár 3.0 SAP IDES. A számítógépen készített képernyőképeket megjelenítése a folyamat lépésenkénti. A lista más megoldást üzembe helyezése a folyamat szemszögéből ugyanígy működik. Csak egy rendelkezik jelöljön ki egy másik megoldást.

Induljon SAP felhő készülék Library (SAP-CAL) válassza [az alábbi](https://cal.sap.com/). Az SAP az új [SAP felhő készülék tár 3.0](http://scn.sap.com/community/cloud-appliance-library/blog/2016/05/27/sap-cloud-appliance-library-30-came-with-a-new-user-experience)kapcsolatos blog van. 


Az alábbi képernyőképek lépésenkénti megjelenítése a Microsoft Azure SAP IDES telepítéséről. A folyamat megoldásait ugyanígy működik.


![](./media/virtual-machines-windows-sap-cal-ides-erp6-ehp7-sp3-sql/ides-pic1.jpg)

Az első képet érhetők el a Microsoft Azure összes megoldásokat mutatja. A kiemelt SAP IDES Windows-alapú megoldást, amely csak a Azure választotta, hogy feldolgozzuk a folyamat.

![](./media/virtual-machines-windows-sap-cal-ides-erp6-ehp7-sp3-sql/ides-pic2.jpg)

SAP-CAL új fiók először fel hozható létre. Jelenleg az Azure - kétféle folytatási normál Azure és Azure meg, hogy a partner 21Vianet üzemelteti Kínában kontinens.

![](./media/virtual-machines-windows-sap-cal-ides-erp6-ehp7-sp3-sql/ides-pic3.jpg)

Kattintson egy rendelkezik írja be az Azure-portálra – lásd még: további lefelé hogyan szerezheti be a program Azure előfizetés Azonosítót. Ezután az Azure management tanúsítványt kell letölteni.

![](./media/virtual-machines-windows-sap-cal-ides-erp6-ehp7-sp3-sql/ides-pic6.jpg)

Az új Azure-ban portál egy keresi meg a "Előfizetések" bal oldalán. Kattintson arra a felhasználói minden aktív előfizetések megjelenítése.

![](./media/virtual-machines-windows-sap-cal-ides-erp6-ehp7-sp3-sql/ides-pic7.jpg)

Az előfizetések valamelyikének kiválasztásával és lehetőséget, majd a "Management tanúsítványok" ebből a cikkből megtudhatja, hogy nincs fogalma új "szolgáltatás rendszerbiztonsági" használata az új Azure erőforrás-kezelő modell.
SAP-CAL még nem módosítani ezt a modellt, és továbbra is megköveteli a "klasszikus" modell és a korábbi Azure portál kezelése tanúsítványok végezhető.

![](./media/virtual-machines-windows-sap-cal-ides-erp6-ehp7-sp3-sql/ides-pic4.jpg)

Itt láthatja egyet az előbbi Azure-portálra. A feltöltés management tanúsítvány SAP CAL hozhat létre virtuális gépeken futó ügyfél-előfizetés belül az engedélyek adja vissza. A "ELŐFIZETÉSEK" a lapon egyszerre megtalálja az előfizetés azonosítója, amelynek az SAP-CAL portálon kell megadni.

![](./media/virtual-machines-windows-sap-cal-ides-erp6-ehp7-sp3-sql/ides-pic5.jpg)

A második lap ajánlatos kattintson a kezelés tanúsítvány előtt az SAP-CAL letöltött feltöltése.

![](./media/virtual-machines-windows-sap-cal-ides-erp6-ehp7-sp3-sql/ides-pic8.jpg)

Jelölje ki a letöltött tanúsítvány-fájlt megjelenik egy kisméretű párbeszédpanel.

![](./media/virtual-machines-windows-sap-cal-ides-erp6-ehp7-sp3-sql/ides-pic9.jpg)

Miután a tanúsítvány feltöltése SAP CAL és Azure előfizetés ügyfél közötti kapcsolat is vizsgálni SAP CAl belül. Kis üzenet kell bukkan fel, amely azt jelenti, hogy a kapcsolat érvényes.

![](./media/virtual-machines-windows-sap-cal-ides-erp6-ehp7-sp3-sql/ides-pic10.jpg)

A fiók beállítása után egy ki kell választania kell telepíthető, és hozzon létre egy példány megoldás.
A "egyszerű" mód valójában trivial. Írja be a példány nevét, válassza az Azure régió, valamint definiálhatók a fő jelszavának a megoldást.

![](./media/virtual-machines-windows-sap-cal-ides-erp6-ehp7-sp3-sql/ides-pic11.jpg)

Egy kis időt, attól függően, hogy méretétől és összetettségétől a megoldás (becslés képlettel SAP CAL) után látható a "aktív" és a használatra kész. Érdemes rendkívül egyszerű.

![](./media/virtual-machines-windows-sap-cal-ides-erp6-ehp7-sp3-sql/ides-pic12.jpg)

Megjeleníti a megoldás egy bizonyos részleteit láthatja, milyen típusú VMs telepítve volt. Ebben az esetben nem egy egyetlen Azure virtuális az SAP-CAL által létrehozott D12 méret.

![](./media/virtual-machines-windows-sap-cal-ides-erp6-ehp7-sp3-sql/ides-pic13.jpg)

Az Azure portálon a virtuális gép azonos nevű példányt az SAP-CAL adott kezdési találhatók.

![](./media/virtual-machines-windows-sap-cal-ides-erp6-ehp7-sp3-sql/ides-pic14.jpg)

Mostantól lehetőség a megoldást a Csatlakozás gombra az SAP-CAL portálon keresztül szeretne csatlakozni. A kis párbeszédpanel az összes az alapértelmezett hitelesítő adatait, és a megoldás használata leíró felhasználói segédvonalhoz hivatkozást tartalmaz.
[Itt](https://caldocs.hana.ondemand.com/caldocs/help/Getting_Started_Guide_IDES607MSSQL.pdf) található a hivatkozás az útmutató a IDES megoldás.

![](./media/virtual-machines-windows-sap-cal-ides-erp6-ehp7-sp3-sql/ides-pic15.jpg)

Egy másik, hogy a Windows virtuális bejelentkezni, és kezdjen például az előre konfigurált SAP-grafikus.





