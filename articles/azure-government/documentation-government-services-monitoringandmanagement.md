<properties
    pageTitle="Azure kormányzati dokumentáció |} Microsoft Azure"
    description="Ez biztosít, funkciók és útmutatást összehasonlítása Azure kormányzati alkalmazások fejlesztéséhez."
    services="Azure-Government"
    cloud="gov"
    documentationCenter=""
    authors="ryansoc"
    manager="zakramer"
    editor=""/>

<tags
    ms.service="multiple"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="azure-government"
    ms.date="10/25/2016"
    ms.author="ryansoc"/>


#  <a name="azure-government-monitoring-and-management"></a>Azure kormányzati figyelő és felügyeleti

Ez a cikk ismerteti, hogy nyomon követése, és a kezelés szolgáltatások változatok és megfontolások az Azure kormányzati környezetben.

## <a name="automation"></a>Automatizálási

Automatizálási érhető el általában az Azure kormányzati.

### <a name="variations"></a>Változatok

Automatizálási szolgáltatásai nem érhetők el jelenleg az Azure kormányzati.

+ A hitelesítési szolgáltatás elve hitelesítő adatainak létrehozása

További információért dokumentációjában [automatizálási nyilvános](../automation/automation-intro.md).

## <a name="log-analytics"></a>Log Analytics

Log Analytics érhető el általában az Azure kormányzati.

### <a name="variations"></a>Változatok

Log Analytics-szolgáltatásokat és megoldások nem érhetők el jelenleg az Azure kormányzati.

+ Megoldások, amelyek a kép a Microsoft Azure, például:
  - Hálózati figyelés megoldás
  - Alkalmazás figyelése függőség megoldás
  - Az Office 365-megoldás
  - A Windows 10 frissítése Analytics megoldás
  - Alkalmazás háttérismeretek megoldás
  - Azure hálózati Analytics-megoldás
  - Azure automatizálási Analytics-megoldás
  - Kulcs tárolóra Analytics megoldás
+ Megoldások és szolgáltatások igénylő frissítések – a helyszíni szoftver, például:
  - System Center műveletek Manager 2016 (Operations Manager korábbi verzióiban támogatott) integrációja
  - A System Center Configuration Manager számítógépek csoportok
  - Felületi központi megoldás
+ Funkciók, amelyek a kép a nyilvános Azure, például:
  - A Power BI adatok exportálása
+ Azure mértékek és Azure diagnosztika
+ Tevékenységek kezelése csomagja mobilalkalmazások

Az URL-címek napló elemzéséhez Azure kormányzati eltérőek:

| Azure nyilvános | Azure kormányzati | Jegyzetek |
|--------------|------------------|-------|
| MMS.microsoft.com | OMS.microsoft.us | Jelentkezzen be a Analytics-portál |
| *workspaceId*. ods.opinsights.azure.com | *workspaceId*. ods.opinsights.azure.us | [Adatgyűjtő API](../log-analytics/log-analytics-data-collector-api.md) 
| \*. ods.opinsights.azure.com | \*. ods.opinsights.azure.us | Ügynök kommunikációs – a [tűzfal beállításainak konfigurálása](../log-analytics/log-analytics-proxy-firewall.md) |
| \*. oms.opinsights.azure.com | \*. oms.opinsights.azure.us | Ügynök kommunikációs – a [tűzfal beállításainak konfigurálása](../log-analytics/log-analytics-proxy-firewall.md) |
| \*. blob.core.windows.net | \*. blob.core.usgovcloudapi.net | Ügynök kommunikációs – a [tűzfal beállításainak konfigurálása](../log-analytics/log-analytics-proxy-firewall.md) |


A következő napló Analytics verziókban eltérően működő funkciói Azure kormányzati:

+ A Windows-ügynök az Azure kormányzati a [napló Analytics-portálon](https://oms.microsoft.us) lehet letölteni.
+ A rendszer központ Operations Manager management server kapcsolódni napló Analytics, szüksége töltheti le, és importálja a frissített management csomagok.
  1. Töltse le és [frissítése az adatkezelési csomagok](http://go.microsoft.com/fwlink/?LinkId=828749)menteni.
  2. A letöltött fájl Unzip.
  3. Az adatkezelési csomagok Operations Manager importálhat. A felügyeleti csomag importálása a lemezen, akkor olvassa el a [importálásával-műveleteket Manager felügyeleti csomag](http://technet.microsoft.com/library/hh212691.aspx) a Microsoft TechNet webhelyen olvashat.
  4. Log Analytics Operations Manager csatlakoztatásához kövesse a [Operations Manager kapcsolatot a naplóban Analytics](../log-analytics/log-analytics-om-agents.md).


## <a name="frequently-asked-questions"></a>Gyakori kérdések

+ Is lehet áttelepítése adatokat a Microsoft Azure napló Analytics Azure kormányzati?
  - nem. Még nem helyezhetők át adatokat vagy a munkaterület Microsoft Azure Azure kormányzati lehetséges.
+ Válthatok Microsoft Azure és Azure kormányzati között a műveletek Management csomagja napló Analytics portálról munkaterületek?
  - nem. A Microsoft Azure és Azure kormányzati portálokról külön, és ne ossza meg az információkat.

További információért dokumentációjában [napló Analytics nyilvános](../log-analytics/log-analytics-overview.md).

## <a name="next-steps"></a>Következő lépések

Feliratkozás a kiegészítő információk és frissítések, a <a href="https://blogs.msdn.microsoft.com/azuregov/">Microsoft Azure kormányzati blogban.</a>
