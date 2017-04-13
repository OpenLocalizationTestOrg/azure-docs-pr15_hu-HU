<properties 
   pageTitle="A Windows-eseménynaplózás bejelentkezik a napló Analytics |} Microsoft Azure"
   description="Windows-eseménynaplók a leggyakoribb napló Analytics által használt adatforrások közül.  Ez a cikk ismerteti, hogyan állíthatja be Windows-eseménynaplók gyűjteménye és a rekordok hoznának létre a MOBILE adattárban részleteit."
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

# <a name="windows-event-log-data-sources-in-log-analytics"></a>A Windows eseménynaplójának adatforrások napló Analytics

Windows-eseménynaplók a leggyakoribb [adatforrások](log-analytics-data-sources.md) , mivel ez a módszer a legtöbb alkalmazások segítségével jelentkezzen be az adatok és a hibák a Windows-ügynökök használt.  Események szabványos naplók, például a rendszer és az alkalmazás bármely kell figyelni alkalmazások által létrehozott egyéni naplók megadása mellett összegyűjtheti.

![Windows-események](media/log-analytics-data-sources-windows-events/overview.png)     

## <a name="configuring-windows-event-logs"></a>A Windows eseménynaplók konfigurálása

Állítsa be a Windows-eseménynaplók az [adatok Analytics beállítások menüben](log-analytics-data-sources.md#configuring-data-sources).

Log Analytics csak a Windows-eseménynaplók beállításaiban megadott fog eseményeinek összegyűjtése.  A naplófájl neve beírásával, és kattintson egy új napló is hozzáadhat **+**.  Rögzít csak a kijelölt severities események lesznek összegyűjtve.  Jelölje be az adott napló összegyűjtéséhez, amelyet a severities.  További feltételeket eseményeinek szűrése nem adhat meg.

![Windows-események beállítása](media/log-analytics-data-sources-windows-events/configure.png)


## <a name="data-collection"></a>Adatok gyűjtése

Log Analytics összegyűjti az egyes események, amely megfelel egy felügyelt eseménynaplójának a a kijelölt súlyosságát, az esemény létrehozása.  A agent rögzíteni fogja minden eseménynaplójában talál, amely azt gyűjti össze a elfoglalt helyét.  A agent időbeli offline állapotba kerül, ha majd napló Analytics fog eseményeinek összegyűjtése, ahol az utolsó abbahagyta, még akkor is, ha az események létrehozott kapcsolat nélküli a agent állapotában.


## <a name="windows-event-records-properties"></a>A Windows-eseménynaplózás rekordok tulajdonságai

A Windows esemény rekordok **esemény** típusú, és tárolja az a tulajdonságokat az alábbi táblázatban.

| A tulajdonság | Leírás |
|:--|:--|
| Számítógép            | Az esemény összegyűjtött a számítógép nevét. |
| EventCategory       | Az esemény kategóriát. |
| EventData           | Az összes eseményadatok nyers formátumban. |
| EventID             | Az esemény száma. |
| EventLevel          | Az esemény numerikus formátumban súlyosságát. |
| EventLevelName      | Az esemény szöveges formátumú súlyosságát. |
| Eseménynapló            | Az Eseménynapló az összegyűjtött az esemény nevét. |
| ParameterXml        | Esemény paraméterértékeket XML formátumban. |
| ManagementGroupName | A kezelés csoport SCOM ügynökök nevét.  Más ügynökök Ez az AOI-<workspace ID> |
| RenderedDescription | Paraméterértékeket az esemény leírása |
| Forrás              | Az esemény forrása. |
| SourceSystem  | Az esemény amelyet gyűjtött ügynök típusát. <br> Csatlakozás OpsManager – Windows agent, vagy közvetlen vagy SCOM <br> Linux – az összes Linux ügynökök beállítása  <br> AzureStorage – Azure diagnosztika |
| TimeGenerated       | Dátum és idő az esemény Windows hozták létre. |
| Felhasználónév            | A fiókot, amely az esemény bejelentkezett felhasználó neve. |



## <a name="log-searches-with-windows-events"></a>A Windows eseményekkel napló keresések

Az alábbi táblázat, amely a Windows-eseménynaplózás rekordjával napló keresések különböző példákat.

| Lekérdezés | Leírás |
|:--|:--|
| Típus = esemény | Az összes Windows esemény. |
| Típus = esemény EventLevelName = hiba | Az összes Windows események hiba súlyosságát. |
| Típus = esemény & #124; Forrás mérték count() | Forrás események száma a Windows. |
| Típus = esemény EventLevelName = hiba & #124; Forrás mérték count() | Forrás hiba események száma a Windows. |

## <a name="next-steps"></a>Következő lépések

- Állítsa be a napló Analytics más [adatforrásokból](log-analytics-data-sources.md) elemzéshez összegyűjtéséhez.
- [Log keresések](log-analytics-log-searches.md) megismerheti az adatforrások és megoldások gyűjtött adatok elemzése céljából.  
- [Egyéni mezők](log-analytics-custom-fields.md) használatával az esemény rekordok értelmezhető egyes mezőket.
- Állítsa be a Windows-ügynökök [Teljesítmény számláló gyűjteménye](log-analytics-data-sources-performance-counters.md) .