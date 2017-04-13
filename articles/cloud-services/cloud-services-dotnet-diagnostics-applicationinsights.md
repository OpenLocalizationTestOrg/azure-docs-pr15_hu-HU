<properties
   pageTitle="Cloud Services alkalmazás mélyebb használatával elhárítása |} Microsoft Azure"
   description="Megtudhatja, hogy miként felhőalapú szolgáltatás kapcsolatos problémák megoldása folyamat adatainak az Azure diagnosztika alkalmazás háttérismeretek segítségével."
   services="cloud-services"
   documentationCenter=".net"
   authors="sbtron"
   manager="timlt"
   editor="tysonn" />
<tags
   ms.service="cloud-services"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="12/15/2015"
   ms.author="saurabh" />


# <a name="troubleshoot-cloud-services-using-application-insights"></a>Cloud Services alkalmazás háttérismeretek használatával – problémamegoldás

[Azure SDK 2,8](https://azure.microsoft.com/downloads/) és Azure diagnosztika kiterjesztésű 1,5 most elküldheti az Azure diagnosztikai adatok a felhőben szolgáltatásbeli közvetlenül az alkalmazás az összefüggéseket. Naplók gyűjti össze Azure diagnosztikai naplók alkalmazás, a windows-eseménynaplók, beleértve a különböző típusú esemény-nyomkövetés jelentkezik, és a teljesítmény számláló most alkalmazás mélyebb küldhető és formájában jelenik meg az alkalmazást az összefüggéseket portálon felhasználói felület. Az alkalmazás az összefüggéseket SDK együtt használva most elérheti a mértékek, és az alkalmazás, valamint a rendszer és infrastruktúra szintű az adatok diagnosztika Azure-ból érkező naplók az összefüggéseket.

## <a name="configure-azure-diagnostics-to-send-data-to-application-insights"></a>Adatok küldése a alkalmazás háttérismeretek Azure diagnosztika konfigurálása

Kövesse az alábbi lépéseket a felhőalapú szolgáltatás projekt Azure diagnosztika adatok küldése a hírcsatornájában alkalmazás beállítása.

1) A Visual Studio megoldás Intézőben kattintson a jobb gombbal egy szerepkört, majd válassza a **Tulajdonságok** kattintva nyissa meg a szerepkör-Tervező

![Megoldás Explorer szerep tulajdonságai][1]

2) Szerepkör designer diagnosztika csoportjában jelölje be a jelölőnégyzetet, **alkalmazás mélyebb diagnosztikai adatok** küldése

![Szerepkör designer diagnosztika adatok küldése a alkalmazás mélyebb][2]

3) Az előugró párbeszédpanel jelölje ki az alkalmazás mélyebb erőforrás, amelyeket szeretne küldeni az Azure diagnosztikai adatok. A párbeszédpanelen jelölje be a meglévő alkalmazás mélyebb erőforrás az előfizetésből, vagy adja meg az alkalmazás mélyebb erőforrás-műszerezettségi kulcs manuálisan teszi lehetővé. Ha nincs telepítve a meglévő alkalmazás mélyebb erőforrás majd létrehozhat meg az **Új erőforrás létrehozása** hivatkozásra, amely megnyílik egy böngészőablakban az Azure klasszikus portált, ahol létrehozhatja az alkalmazás mélyebb erőforrás gombra kattintva. Az alkalmazás mélyebb erőforrás létrehozásával kapcsolatban további információt a [Új alkalmazás mélyebb erőforrás létrehozása](../application-insights/app-insights-create-new-resource.md) című témakör tartalmaz.

![Válassza az alkalmazás mélyebb erőforrás][3]

4) Miután elhelyezte az alkalmazás az összefüggéseket erőforrás, a műszerezettségi billentyűt az adott erőforrás egy szolgáltatás konfigurációs beállítások **APPINSIGHTS_INSTRUMENTATIONKEY**nevű tárolja. Jelöljön ki egy másik konfigurációt a szolgáltatás konfigurálása legördülő listáról módosíthatja az egyes konfigurációjának vagy a környezet konfigurációja beállítás lefelé, és adja meg az adott konfigurációban új műszerezettségi kulcsot.

![Jelölje be a szolgáltatás konfigurálása][4]

Visual Studio a **APPINSIGHTS_INSTRUMENTATIONKEY** konfigurációs beállítások konfigurálása a diagnosztika kiterjesztése a megfelelő információkkal alkalmazás háttérismeretek erőforrás közzététel során használt. A konfigurációs beállítások meghatározása a különböző műszerezettségi kulcsok másik szolgáltatáscsaládba konfigurációk kényelmesen. Visual Studio fordítása ezt a beállítást, és a szúrja be a diagnosztikai bővítmény nyilvános konfigurációja, közzétételekor. Segítségével egyszerűsíthető a diagnosztika kiterjesztése a PowerShell, a csomag kimeneti Visual Studio is tartalmazza a megfelelő alkalmazás háttérismeretek műszerezettségi a nyilvános konfigurációban XML szereplő billentyűt. A nyilvános config fájlok létrehozásakor a bővítmények mappára a, és kövesse a mintázat PaaSDiagnostics. <RoleName>. PubConfig.xml. Bármely PowerShell-alapú telepítés ezt a minta segítségével megfeleltetése egyes kereséskonfigurációs szerepkörbe.

5) **Diagnosztikai adatok mélyebb alkalmazás küldése** engedélyezése automatikusan konfigurálja a küldött összes teljesítményét számláló és szintű hibanaplók éppen által gyűjtött az Azure diagnosztika ügynök alkalmazás mélyebb Azure diagnosztika. Ha szeretne további az adatok mélyebb alkalmazás küldi el, majd a *diagnostics.wadcfgx* fájl minden szerepkörhöz manuális szerkesztése kell beállítása. Lásd: [Azure diagnosztika konfigurálása alkalmazás mélyebb adatok küldése](../azure-diagnostics-configure-applicationinsights.md) a konfigurációs manuális frissítésről további információt.

Miután a Felhőbeli szolgáltatástól Azure diagnosztikai adatok küldése a alkalmazás háttérismeretek telepítheti azt Azure ugyanúgy kell a szokásos módon van beállítva gondoskodhat arról, hogy az Azure diagnosztika bővítmény engedélyezve van. Lásd: a [közzétételi egy felhőalapú szolgáltatásba, Visual Studio segítségével](../vs-azure-tools-publishing-a-cloud-service.md).  

## <a name="viewing-azure-diagnostics-data-in-application-insights"></a>Alkalmazás mélyebb Azure diagnosztika adatainak megtekintése
Az Azure diagnosztikai telemetriai jelennek meg az alkalmazás mélyebb erőforrás a felhőalapú szolgáltatás be van állítva a.

Az alábbiakban a különböző Azure diagnosztika hogyan típusok térkép jelentkezzen alkalmazás háttérismeretek fogalmak:  

-  Teljesítmény számláló jelennek meg az alkalmazást az összefüggéseket egyéni mértékek
-  Windows-eseménynaplók halad és az alkalmazás az összefüggéseket egyéni események ábrázolva
-  Alkalmazás naplók, esemény-nyomkövetés naplók és bármely infrastruktúra a diagnosztikai naplók oszlopcímkeként jelennek meg az alkalmazást az összefüggéseket a nyomkövetési naplók.

Azure diagnosztikai adatok megjelenítése az alkalmazás Hírcsatornájában:

- [Mértékek](../application-insights/app-insights-metrics-explorer.md) Intézővel ábrázolása egyéni teljesítmény számláló vagy a windows eseménynaplójának események különböző típusú számát.

![A mértékek Intézőben egyéni mérőszámok][5]

- Használja a [keresőt](../application-insights/app-insights-diagnostic-search.md) végig a különböző nyomkövetési naplók Azure diagnosztika által küldött kereséséhez. A példa, ha egy szerepkört, ami miatt összeomlását és Lomtár ezeket az információkat a szerepkör esetén nem kezelt kivételt volna jelenik meg a *Windows eseménynaplójának* *alkalmazás* történt. A keresés funkció tekintse meg a Windows eseménynaplójának hiba és a teljes Papírhalom követés az a kivétel, keresse meg a legfelső szintű a probléma okát úgy is használhatja.

![Keresés halad][6]

## <a name="next-steps"></a>Következő lépések

- [Az alkalmazás az összefüggéseket SDK a felhőalapú szolgáltatás hozzáadása](../application-insights/app-insights-cloudservices.md) kérések, kivételeket, függőségek és minden egyéni telemetriai adatokat küldeni a levelezőprogramból. Egyesíti az Azure diagnosztika szerezheti be az alkalmazás és a rendszer átfogó képet az összes alkalmazás betekintést ugyanaz az erőforrás adatainak.  


<!--Image references-->
[1]: ./media/cloud-services-dotnet-diagnostics-applicationinsights/solution-explorer-properties.png
[2]: ./media/cloud-services-dotnet-diagnostics-applicationinsights/role-designer-sendtoappinsights.png
[3]: ./media/cloud-services-dotnet-diagnostics-applicationinsights/select-appinsights-resource.png
[4]: ./media/cloud-services-dotnet-diagnostics-applicationinsights/role-designer-appinsights-serviceconfig.png
[5]: ./media/cloud-services-dotnet-diagnostics-applicationinsights/metrics-explorer-custom-metrics.png
[6]: ./media/cloud-services-dotnet-diagnostics-applicationinsights/search-windowseventlog-error.png
