<properties
   pageTitle="Bevezetés a Microsoft Azure napló integrációs |} Microsoft Azure"
   description="Tudjon meg többet az Azure napló integrációs főbb funkciói és működéséről."
   services="security"
   documentationCenter="na"
   authors="TomShinder"
   manager="MBaldwin"
   editor="TerryLanfear"/>

<tags
   ms.service="security"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/24/2016"
   ms.author="TomSh"/>

# <a name="introduction-to-microsoft-azure-log-integration-preview"></a>Bevezetés a Microsoft Azure napló integrációs (előzetes verzió)

Tudjon meg többet az Azure napló integrációs főbb funkciói és működéséről.

## <a name="overview"></a>– Áttekintés

Olyan szolgáltatást (PaaS) és a szolgáltatásként (IaaS) Azure-ban tárolt infrastruktúra platform biztonsági naplók létrehozása nagy mennyiségű adatot. Ezek a naplók intelligencia és hatékony háttérismeretek be a szabály megsértése, a belső és külső veszélyek, a szabályozási megfelelőségről problémák és a hálózat, host és felhasználói tevékenység rendellenességeinek tartalmaznak alapvető információkat tartalmaznak.

Azure napló integráció lehetővé teszi az Azure erőforrásoktól nyers naplók integrálása a helyszíni biztonsági információk és esemény-kezelés (SIEM) rendszerek. Azure napló integráció a Windows *(ÜVEGVATTA)* gyűjti össze a diagnosztika Azure virtuális gépeken futó, valamint a partnerek megoldásaival, például a webes alkalmazás tűzfal (WAF) diagnosztika. Integrálás biztosít egy egyesített irányítópult az összes eszközeit, a helyszíni, vagy a felhőben, hogy összesítése, összehangolására, elemezheti, és biztonsági események értesítést.

![Azure napló-integráció][1]

## <a name="what-logs-can-i-integrate"></a>Milyen naplók is integrálni?

Azure minden Azure szolgáltatás teljes körű naplózás eredményez. Ezek a naplók kategóriába két fő típusa szerint:

- A **vezérlő/adatkezelési naplók**, amelyek betekintést kap abba, hogy az Azure erőforrás-kezelő létrehozása, frissítése és törlése a műveletek. Azure naplókat képen az ilyen típusú naplót.
- **Adatok sík naplók**, amely betekintést kap abba, az események, a használatát, az erőforrás-Azure részeként hatványát adja. Az ilyen típusú napló példák a Windows-eseménynaplózás a rendszer biztonsági, és alkalmazás naplózza a virtuális gépen.

Azure napló integrációs jelenleg támogatott integrálása a naplókat Azure virtuális gép naplók és Azure biztonság otthon értesítések.

Ha Azure Log integrációs kérdéseket, küldjön e-mailben [AzSIEMteam@microsoft.com](mailto:AzSIEMteam@microsoft.com)

## <a name="next-steps"></a>Következő lépések

A dokumentumban az Azure napló integrációba hozták be. Többet szeretne tudni az Azure napló integráció és a naplókat támogatott típusú, olvassa el az alábbiakat:

- [Microsoft Azure napló integrálása az Azure naplók (előzetes verzió)](https://www.microsoft.com/download/details.aspx?id=53324) – a letöltőközpontból információ, rendszerkövetelmények és Azure napló integráció a telepítési utasításokat.
- [Azure napló integrációs – első lépések](security-azure-log-integration-get-started.md) – Ez az oktatóanyag végigvezeti az Azure napló integráció és integrálása az Azure tárhely, az Azure naplókat és a biztonság otthon riasztások naplók telepítését.
- [Partner konfigurálási lépéseket](https://blogs.msdn.microsoft.com/azuresecurity/2016/08/23/azure-log-siem-configuration-steps/) – blogbejegyzésben bemutatja, hogyan Azure napló integráció a partnerek megoldásaival Splunk, HP ArcSight és IBM QRadar végezhető.
- [Azure napló integrációs gyakran ismételt kérdések](security-azure-log-integration-faq.md) – a Gyakori kérdésekre ad választ Azure napló integrálása.
- [Az Azure integrálása biztonság otthon riasztások jelentkezzen integrációs](../security-center/security-center-integrating-alerts-with-log-integration.md) – ehhez a dokumentumhoz megtudhatja, hogy miként szinkronizálhatja a biztonság otthon riasztások virtuális gép biztonsági események gyűjti össze a Azure diagnosztikai és a naplókat Azure, SIEM megoldás vagy a napló analytics együtt.
- [Azure diagnosztikai és Azure naplókat új szolgáltatásai](https://azure.microsoft.com/blog/new-features-for-azure-diagnostics-and-azure-audit-logs/) – blogbejegyzésben megismerkedhet Azure naplókat, és más szolgáltatásokat nyújt szerezhet be az Azure erőforrások műveletek az összefüggéseket.

<!--Image references-->
[1]: ./media/security-azure-log-integration-overview/azure-log-integration.png
