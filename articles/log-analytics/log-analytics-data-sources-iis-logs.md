<properties
   pageTitle="IIS naplózza a napló Analytics |} Microsoft Azure"
   description="Internet Information Services (IIS) felhasználói tevékenység naplója Analytics gyűjthető naplófájlok tárol.  Ez a cikk ismerteti, hogyan állíthatja be IIS naplók gyűjteménye és a rekordok hoznának létre a MOBILE adattárban részleteit."
   services="log-analytics"
   documentationCenter=""
   authors="bwren"
   manager="jwhit"
   editor="tysonn" />
<tags
   ms.service="log-analytics"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/18/2016"
   ms.author="bwren" />

# <a name="iis-logs-in-log-analytics"></a>IIS naplózza a napló Analytics
Internet Information Services (IIS) felhasználói tevékenység naplója Analytics gyűjthető naplófájlok tárol.  

![IIS naplók](media/log-analytics-data-sources-iis-logs/overview.png)

## <a name="configuring-iis-logs"></a>Naplózza az IIS konfigurálása
Log Analytics, először [állítsa be a naplózás az IIS](https://technet.microsoft.com/library/hh831775.aspx), az IIS által létrehozott naplófájlok gyűjti össze a bejegyzéseket.

Log analitikai csak támogatja az IIS-naplófájlokat W3C formátumban tárolja, és nem támogatja az egyéni mezők és az IIS speciális naplózás.  
Log Analytics nem naplógyűjtés NCSA vagy az IIS natív formátumban.

Állítsa be az IIS naplók napló Analytics a [Analytics beállítások az adatok menü](log-analytics-data-sources.md#configuring-data-sources).  Nincs szükség beállításra, ha kijelöli a **W3C összegyűjtése formátum IIS-naplófájlokat**eltérő van.

Azt javasoljuk, hogy az IIS napló webhelycsoport engedélyezésekor konfigurálnia kell az IIS napló átváltási beállítás-kiszolgálókon.


## <a name="data-collection"></a>Adatok gyűjtése

Log Analytics gyűjti össze a IIS naplóbejegyzések minden csatlakoztatott adatforrás körülbelül 15 percenként.  A agent rögzít minden eseménynaplójában talál, amely azt gyűjti össze a elfoglalt helyét.  A agent offline állapotba kerül, ha majd napló Analytics gyűjti össze a eseményeket, ahol az utolsó abbahagyta, még akkor is, ha az események létrehozott kapcsolat nélküli a agent állapotában.


## <a name="iis-log-record-properties"></a>IIS napló rekord tulajdonságai párbeszédpanelen

IIS naplóbejegyzések **W3CIISLog** típusú, és tárolja az a tulajdonságokat az alábbi táblázatban:

| A tulajdonság | Leírás |
|:--|:--|
| Számítógép | Az esemény összegyűjtött a számítógép nevét. |
| cIP | Az ügyfél IP-címe. |
| csMethod | A kérelmet, például kérése vagy KÜLDÉS metódusát. |
| csReferer | A webhely, hogy a felhasználó ezek után pedig egy hivatkozást az aktuális webhelyre. |
| csUserAgent | Az ügyfél típusú böngészőben. |
| csUserName | A hitelesített felhasználó elérhető a kiszolgáló neve. A névtelen felhasználók elválasztójel jelölik. |
| csUriStem | A kérelmet, például az weblapon célját. |
| csUriQuery | A lekérdezés, az ügyfél próbált végrehajtása. |
| ManagementGroupName | A kezelés csoport Operations Manager ügynökök nevét.  Más ügynökök, ez az AOI -\<munkaterület azonosító\> |
| RemoteIPCountry | Az IP-címét az ügyfél országa. |
| RemoteIPLatitude | Az ügyfél IP-címek szélesség. |
| RemoteIPLongitude | Az ügyfél IP-címek hosszúság. |
| scStatus | HTTP-állapotkód. |
| scSubStatus | Részállapot hiba jelenik meg. |
| scWin32Status | A Windows állapotkód. |
| sIP | Az érintett webkiszolgálóra IP-címe. |
| SourceSystem  | OpsMgr |
| sportágak | Az ügyfél-kiszolgáló portjához csatlakozik. |
| sSiteName | A webhely neve; IIS. |
| TimeGenerated | Dátum és idő a naplóbejegyzés. |
| TimeTaken | Eltelt idő ezredmásodpercben az összehívás feldolgozása. |

## <a name="log-searches-with-iis-logs"></a>Log keresések az IIS naplók

Az alábbi táblázat a napló lekérdezések IIS naplóbejegyzések beolvasó különböző példákat.

| Lekérdezés | Leírás |
|:--|:--|
| Típus = IISLog | Az összes IIS naplóbejegyzések. |
| Típus = IISLog EventLevelName = hiba | Az összes Windows események hiba súlyosságát. |
| Típus = W3CIISLog & #124; Mérje le a count() cIP szerint | Ügyfél IP-címe tételek száma az IIS jelentkezzen. |
| Típus = W3CIISLog csHost = "www.contoso.com" & #124; Mérje le a count() csUriStem szerint | Darab az IIS jelentkezzen be a host www.contoso.com URL-cím által bejegyzéseinek. |
| Típus = W3CIISLog & #124; Mérje le a számítógép & #124; Sum(csBytes) 500 000 összeget tetején| Az összes bájt megkapta az egyes IIS szolgáltatást futtató számítógép. |

## <a name="next-steps"></a>Következő lépések

- Állítsa be a napló Analytics más [adatforrásokból](log-analytics-data-sources.md) elemzéshez összegyűjtéséhez.
- [Log keresések](log-analytics-log-searches.md) megismerheti az adatforrások és megoldások gyűjtött adatok elemzése céljából.
- Log Analytics ezzel kapcsolatban beérkező értesítést küld az IIS naplókban található fontos feltételek értesítések konfigurálása
