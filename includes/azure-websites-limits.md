Erőforrás|Ingyenes|Megosztott (előzetes verzió)|Egyszerű|Normál|Prémium (előzetes verzió)</th>
---|---|---|---|---|---
[Webes mobil vagy az API-alkalmazások](https://azure.microsoft.com/services/app-service/) [alkalmazás szolgáltatáscsomagja](../articles/app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md)<sup>1</sup> /|10|100|Korlátlan<sup>2</sup>|Korlátlan<sup>2</sup>|Korlátlan<sup>2</sup>
[Logika alkalmazások](https://azure.microsoft.com/services/app-service/logic/) egy [alkalmazás szolgáltatáscsomagja](../articles/app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md)</a><sup>1</sup>|10|10|10|alapvető 20|alapvető 20
[Alkalmazás szolgáltatáscsomagja](../articles/app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md)|1 / régió|10 dB / erőforráscsoport|Erőforráscsoport 100|Erőforráscsoport 100|Erőforráscsoport 100
Kiszámítania példány típusa|A megosztott|A megosztott|Saját<sup>3</sup>|Saját<sup>3</sup>|Saját<sup>3</sup></p>
[Méretezési](../articles/app-service-web/web-sites-scale.md) (max példány)|a megosztott 1|a megosztott 1|3 dedikált<sup>3</sup>|10 dedikált<sup>3</sup>|20 dedikált (ASE az 50)<sup>3.4.</sup>
Tárterület-<sup>5</sup>|<sup>5</sup> 1 GB|<sup>5</sup> 1 GB|10 GB-os<sup>5</sup>|50 GB<sup>5</sup>|500 GB<sup>4,5</sup></p>
Processzor idő (rövid)<sup>6</sup>|3 perc|3 perc|Korlátlan, [díjak](https://azure.microsoft.com/pricing/details/app-service/) fizetett</a>|Korlátlan, fizetési szabványos kamatlába mellett.|Korlátlan, fizetési szabványos kamatlába mellett.
Kitöltött Processzor (nap) idő<sup>6</sup>|60 perc|240 perc|Korlátlan, [díjak](https://azure.microsoft.com/pricing/details/app-service/) fizetett</a>|Korlátlan, fizetési szabványos kamatlába mellett.|Korlátlan, fizetési szabványos kamatlába mellett.
A memória (1 óra)|Felhasználónként alkalmazás szolgáltatáscsomagja 1024 MB|Felhasználónként alkalmazás 1024 MB|A #HIÁNYZIK|A #HIÁNYZIK|A #HIÁNYZIK
A sávszélesség|165 MB|Korlátlan, [adatátviteli sebességet](https://azure.microsoft.com/pricing/details/data-transfers/) alkalmazása|Korlátlan, adatátvitel díjak alkalmazása|Korlátlan, adatátvitel díjak alkalmazása|Korlátlan, adatátvitel díjak alkalmazása
Alkalmazás-architektúra|a 32 bites|a 32 bites|32 bites és 64 bites|32 bites és 64 bites|32 bites és 64 bites
Webes Sockets egy példány<sup>7</sup>|5|35-tel|350|Korlátlan|Korlátlan
Egyidejű [debugger kapcsolatok](../articles/app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md) alkalmazás per|1|1|1|5|5
[azurewebsites.NET altartomány FTP/S és a hitelesítésszolgáltató](../articles/app-service-web/web-sites-configure-ssl-certificate.md)|X|X|X|X|X
[Egyéni tartomány](../articles/app-service-web/web-sites-custom-domain-name.md) támogatás||X|X|X|X
[Az SSL támogatja az](../articles/app-service-web/web-sites-configure-ssl-certificate.md) egyéni tartomány|||Korlátlan|Korlátlan, 5 SNI SSL és 1 IP SSL-kapcsolatot része|Korlátlan, 5 SNI SSL és 1 IP SSL-kapcsolatot része
Integrált terheléselosztó||X|X|X|X
[Állandó](../articles/app-service-web/web-sites-configure.md)|||X|X|X
[Ütemezett biztonsági másolatok](../articles/app-service-web/web-sites-backup.md)||||Naponta egyszer|Miután minden 5 perccel<sup>8</sup>
[Automatikus méretezés](../articles/app-service-web/web-sites-scale.md)|||X|X|X
[WebJobs](../articles/app-service-web/web-sites-create-web-jobs.md) <sup>9</sup>|X|X|X|X|X
[Azure ütemező](https://azure.microsoft.com/services/scheduler/) támogatás||X|X|X|X
[Végpont figyelése](../articles/app-service-web/web-sites-monitor.md)|||X|X|X
[Átmeneti helyek (előzetes verzió)](../articles/app-service-web/web-sites-staged-publishing.md)||||5|20
Alkalmazás egy egyéni tartományok</a>||500|500|500|500
SLA||<p>|99,9 %|<sup>10</sup> %-os 99.95|<sup>10</sup> %-os 99.95

<sup>1</sup> Alkalmazások és tárhelykvótáinak szerepelnek egy alkalmazás szolgáltatáscsomagja kivéve, ha egyébként jegyezni  
<sup>2</sup> Ezen a gépen üzemeltetheti alkalmazásai a tényleges száma a tevékenység alkalmazások, a gép példányok és a megfelelő erőforrás-kihasználtság méretének függ.  
<sup>3</sup> Saját példány különböző méretű lehet. Lásd: az [Alkalmazás szolgáltatás árak](https://azure.microsoft.com/pricing/details/data-transfers/pricing/details/app-service/) további információt.  
<sup>4</sup> Prémium réteg lehetővé teszi, hogy legfeljebb 50 kiszámítja (fizetnie elérhetőség) példányok és 500 GB szabad lemezterület környezetek alkalmazás használatakor, és 20 kiszámítania példányok és 250 GB tárhelyet egyébként.  
<sup>5</sup> A tárterületkorlát tartozik a teljes tartalom mérete használt összes alkalmazások alkalmazás szolgáltatás megegyező csomag További tárterület beállítások érhetők el az [Alkalmazás-szolgáltatási környezetben](../articles/app-service-web/app-service-web-configure-an-app-service-environment.md#storage)  
<sup>6</sup> Ezek az erőforrások a dedikált példányok (példány méretét és példányainak száma) a fizikai erőforrások által korlátozott.  
<sup>7</sup> Az alkalmazás méretezni, az egyszerű réteg két példányaiban, esetén az egyes a két példánya 350 egyidejű kapcsolatok.  
<sup>8</sup> Prémium réteg lehetővé teszi, hogy legfeljebb 5 percenként lefelé biztonsági intervallumok környezetek alkalmazás használatakor, és 50 alkalommal napi különben a program.  
<sup>9</sup> Egyéni végrehajtható és/vagy a parancsfájlok futtasson igény, ütemezés vagy folyamatosan háttér feladatként az alkalmazás szolgáltatás példányának belül. Mindig szükség a folyamatos WebJobs végrehajtása. Azure ütemező szabad és normál az ütemezett WebJobs szükség. Nem nincsenek előre meghatározott korlátozást, a megadott WebJobs, hogy egy App szolgáltatás példány futtatását is lehetővé teszi, de mi az alkalmazás kódja partnere kapcsolatba próbál tegye függő gyakorlati korlátozás van érvényben.   
<sup>10</sup> SZOLGÁLTATÁSISZINT-kezelővel több példányon az Azure forgalom telepítésekhez megadott 99.95 %-os feladatátvevő be van állítva.  
