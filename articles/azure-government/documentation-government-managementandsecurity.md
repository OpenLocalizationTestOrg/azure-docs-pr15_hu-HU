<properties
    pageTitle="Azure kormányzati dokumentáció |} Microsoft Azure"
    description="Ez ez a témakör a szolgáltatást, és útmutatást összehasonlítás Azure kormányzati alkalmazások fejlesztéséhez"
    services="Azure-Government"
    cloud="gov" 
    documentationCenter=""
    authors="scooxl"
    manager="zakramer"
    editor=""/>
<tags
    ms.service="multiple"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="azure-government"
    ms.date="10/25/2016"
    ms.author="scooxl"/>
#  <a name="azure-government-management-and-security"></a>Azure kormányzati kezelési és biztonság

## <a name="automation"></a>Automatizálási

Automatizálási érhető el általában az Azure kormányzati.

### <a name="variations"></a>Változatok

Automatizálási szolgáltatásai nem érhetők el jelenleg az Azure kormányzati.

+ A hitelesítési szolgáltatás elve hitelesítő adatainak létrehozása

További információért dokumentációjában [automatizálási nyilvános](../automation/automation-intro.md).


##  <a name="key-vault"></a>Fő tárolóból elemre
Ez a szolgáltatás és használatához a részletekért olvassa el a <a href="https://azure.microsoft.com/documentation/services/key-vault">Azure kulcs tárolóra nyilvános dokumentációt.</a>
### <a name="data-considerations"></a>Adatok kapcsolatos szempontok
Az alábbi adatokat az Azure kulcs tárolóból elemre az Azure kormányzati oszlopazonosító sorolja fel:

| Megengedett szabályozott/ellenőrzött adatok | Szabályozni/ellenőrzött adatok nem engedélyezett. |
|--------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Az összes adat-Azure kulcs tárolóra kulccsal titkosított Regulated/szabályozott adatokat is tartalmazhatnak. | Azure kulcs tárolóra metaadatokat tartalmazó ellenőrzött exportálása nem engedélyezett. A metaadatok megadni, amikor létrehozása és fenntartása a kulcs tárolóból elemre az összes konfigurációs adatokat tartalmazza.  A következő mezőkben Regulated/ellenőrzött adatok nem kell megadni: erőforrások csoport nevét, a kulcs tárolóból elemre nevét, az előfizetés neve |

Kulcs tárolóra érhető el általában az Azure kormányzati. Nyilvános, ahogy nem nincs kiterjesztése, így kulcs tárolóra csak akkor érhető el a PowerShell és CLI keresztül.
## <a name="log-analytics"></a>Log Analytics
Log Analytics érhető el általában az Azure kormányzati. 

### <a name="variations"></a>Változatok

Log Analytics-szolgáltatásokat és megoldások nem érhetők el jelenleg az Azure kormányzati. A lista frissül, amikor szolgáltatásai állapotát / megoldások változik.

+ Megoldások, amelyek a kép a nyilvános Azure, például:
  - Hálózati figyelés megoldás
  - Alkalmazások függőség figyelése
  - Az Office 365-megoldás
  - A Windows 10 frissítése Analytics megoldás
  - Alkalmazás Hírcsatornájában
  - Azure hálózati Analytics-megoldás
  - Azure automatizálást Analytics
  - Fő tárolóra Analytics
+ Megoldások és szolgáltatások igénylő frissítések – a helyszíni szoftver, többek között
  - System Center műveletek Manager 2016 (Operations Manager korábbi verzióiban támogatott) integrációja
  - System Center Configuration Manager számítógépek csoportok
  - Felületi központi megoldás
+ Funkciók, amelyek a kép a nyilvános Azure, többek között
  - A PowerBI adatok exportálása
+ Azure mértékek és Azure diagnosztika
+ MOBILE Mobile-alkalmazások

Az URL-címek napló elemzéséhez Azure kormányzati eltérőek:

| Azure nyilvános | Azure kormányzati | Jegyzetek |
|--------------|------------------|-------|
| MMS.microsoft.com | OMS.microsoft.us | Jelentkezzen be a Analytics-portál |
| *workspaceId*. ods.opinsights.azure.com | *workspaceId*. ods.opinsights.azure.us | [Adatgyűjtő API](../log-analytics/log-analytics-data-collector-api.md) 
| \*. ods.opinsights.azure.com | \*. ods.opinsights.azure.us | Ügynök kommunikációs – a [tűzfal beállításainak konfigurálása](../log-analytics/log-analytics-proxy-firewall.md) |
| \*. oms.opinsights.azure.com | \*. oms.opinsights.azure.us | Ügynök kommunikációs – a [tűzfal beállításainak konfigurálása](../log-analytics/log-analytics-proxy-firewall.md) |
| \*. blob.core.windows.net | \*. blob.core.usgovcloudapi.net | Ügynök kommunikációs – a [tűzfal beállításainak konfigurálása](../log-analytics/log-analytics-proxy-firewall.md) |


Az alábbi napló Analytics-szolgáltatások különböző viselkedés Azure kormányzati rendelkeznek:

+ A Windows-ügynök az Azure kormányzati a [napló Analytics-portálon](https://oms.microsoft.us) lehet letölteni.
+ A rendszer központ Operations Manager management server kapcsolódni napló Analytics, szüksége töltheti le, és importálja a frissített Management csomagok.
  1. Töltse le és [frissítése az adatkezelési csomagok](http://go.microsoft.com/fwlink/?LinkId=828749) mentése
  2. A letöltött fájl kibontása
  3. Az adatkezelési csomagok Operations Manager importálhat. A felügyeleti csomag importálása lemezen, akkor olvassa el a [importálásával-műveleteket Manager felügyeleti csomag](http://technet.microsoft.com/library/hh212691.aspx) témakör a Microsoft TechNet webhelyen olvashat.
  4. Log Analytics Operations Manager csatlakoztatásához kövesse a [Operations Manager kapcsolatot a naplóban Analytics](../log-analytics/log-analytics-om-agents.md) 



### <a name="frequently-asked-questions"></a>Gyakori kérdések

+ Is adatokat is áttelepítheti a napló Analytics a nyilvános Azure az Azure kormányzati?
  - nem. Még nem helyezhetők át adatokat vagy a munkaterület nyilvános Azure az Azure kormányzati lehetséges.
+ Tudok átváltani az MOBILE napló Analytics portálról nyilvános Azure és Azure kormányzati munkaterületek között?
  - nem. A nyilvános Azure és Azure kormányzati portálokról külön, és nem osztja meg információkat. 

További információért dokumentációjában [napló Analytics nyilvános](../log-analytics/log-analytics-overview.md).

## <a name="next-steps"></a>Következő lépések

Feliratkozás a kiegészítő információk és frissítések, a <a href="https://blogs.msdn.microsoft.com/azuregov/">Microsoft Azure kormányzati blogban.</a>
 
