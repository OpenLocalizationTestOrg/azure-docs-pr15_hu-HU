<properties
    pageTitle="Azure diagnosztika – hibaelhárítás"
    description="Diagnosztika Azure virtuális gépeken futó Azure Felhőszolgáltatások használatakor kapcsolatos hibák elhárítása és "
    services="multiple"
    documentationCenter=".net"
    authors="rboucher"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/04/2016"
    ms.author="robb"/>


# <a name="azure-diagnostics-troubleshooting"></a>Azure diagnosztika – hibaelhárítás
Hibaelhárítási információk Azure diagnosztika segítségével. További információt a Azure diagnosztika [Azure diagnosztika áttekintése](azure-diagnostics.md#cloud-services)című témakörben találhat.

## <a name="azure-diagnostics-is-not-starting"></a>Azure diagnosztika nem indul

Diagnosztikai tevődik két összetevője: Vendég ügynök beépülő modul és az ellenőrzési ügynök. Ellenőrizheti, hogy a naplófájlok **DiagnosticsPluginLauncher.log** és **DiagnosticsPlugin.log** diagnosztika miért nem indul el olvashat.  
  
Egy felhőalapú szolgáltatásba szerepkört vendégként való bekapcsolódáshoz ügynök beépülő modul naplófájlok találhatók: 

``` 
C:\Logs\Plugins\Microsoft.Azure.Diagnostics.PaaSDiagnostics\1.6.3.0\ 
``` 

Az Azure virtuális gép vendég ügynök beépülő modul naplófájlok találhatók: 

``` 
C:\Packages\Plugins\Microsoft.Azure.Diagnostics.IaaSDiagnostics\1.6.3.0\Logs\ 
``` 
 
A naplófájlok utolsó sora fogja tartalmazni a kilépési kódot.  

``` 
DiagnosticsPluginLauncher.exe Information: 0 : [4/16/2016 6:24:15 AM] DiagnosticPlugin exited with code 0 
``` 

A beépülő modul adja eredményül a következő kilépési kódok:

Kilépés a kódot.|Leírás
---|---
0|A siker.
-1|Általános hiba.
-2|Nem lehet a rcf fájl betöltése.<p>Ez a kell csak fordulhat elő, ha a vendégként való bekapcsolódáshoz ügynök beépülő modul megnyitó manuálisan, helytelenül hívják, kattintson a virtuális belső hiba történt.
-3|A diagnosztikai konfigurációs fájl nem töltődik be.<p><p>Megoldás: Az eredmény a konfigurációs fájl nem áthaladó érvényesítése a séma. A megoldás, ha küldje el a konfigurációs fájl, amely megfelel a sémában.
-4|Az ellenőrző ügynök diagnosztika egy másik példányára már használja a helyi erőforrás könyvtárat.<p><p>Megoldás: Adja meg az eltérő **LocalResourceDirectory**.
-6|Indítsa el a diagnosztikai egy érvénytelen parancssori próbált a vendégként való bekapcsolódáshoz ügynök beépülő modul megnyitó ikonra.<p><p>Ez a kell csak fordulhat elő, ha a vendégként való bekapcsolódáshoz ügynök beépülő modul megnyitó manuálisan, helytelenül hívják, a virtuális a belső hiba történt.
-10|A diagnosztikai beépülő modul kilépett esetén nem kezelt kivétellel leállhat.
-11|A vendégként való bekapcsolódáshoz ügynök nem tudott folyamat indítása, valamint az ellenőrző ügynök figyelése felelős létrehozása.<p><p>Megoldás: Győződjön meg arról, hogy elérhetők-e elegendő rendszer-erőforrásokat új folyamatok elindításához.<p>
-101|A diagnosztikai beépülő modul hívásakor érvénytelen argumentumokat.<p><p>Ez a kell csak fordulhat elő, ha a vendégként való bekapcsolódáshoz ügynök beépülő modul megnyitó manuálisan, helytelenül hívják, a virtuális a belső hiba történt.
-102|A beépülő modul folyamat nem tudja inicializálni magát.<p><p>Megoldás: Győződjön meg arról, hogy elérhetők-e elegendő rendszer-erőforrásokat új folyamatok elindításához.
-103|A beépülő modul folyamat nem tudja inicializálni magát. Kifejezetten azt nem tudja létrehozni az naplózó objektumot.<p><p>Megoldás: Győződjön meg arról, hogy elérhetők-e elegendő rendszer-erőforrásokat új folyamatok elindításához.
-104|A vendégként való bekapcsolódáshoz agent által biztosított rcf fájl betöltése nem.<p><p>Ez a kell csak fordulhat elő, ha a vendégként való bekapcsolódáshoz ügynök beépülő modul megnyitó manuálisan, helytelenül hívják, kattintson a virtuális belső hiba történt.
-105|A diagnosztikai beépülő modul nem tudja megnyitni a diagnosztika konfigurációs fájl.<p><p>Ez a kell csak fordulhat elő, ha a beépülő modul diagnosztika manuálisan, helytelenül hívják, a virtuális a belső hiba történt.
-106|A diagnosztikai konfigurációs fájl nem tudja olvasni.<p><p>Megoldás: Az eredmény a konfigurációs fájl nem áthaladó érvényesítése a séma. Ezért a megoldás, küldje el a konfigurációs fájl, amely megfelel a sémában. Az XML-fájl, amelyek a rendszer a a virtuális mappa *%SystemDrive%\WindowsAzure\Config* a diagnosztika kiterjesztést is megkeresheti. Nyissa meg a megfelelő XML-fájlt, és a Keresés **Microsoft.Azure.Diagnostics**, majd a **xmlCfg** mező. Az adatok kódolva, így kell [azt dekódolását](http://www.bing.com/search?q=base64+decoder) kattintva megtekintheti az XML diagnosztika által betöltött base64.<p>
-107|Az erőforrás könyvtár fázis a megfigyeléssel ügynökének érvénytelen.<p><p>Ez a kell csak fordulhat elő, ha az ellenőrzési ügynök manuálisan, helytelenül hívják, kattintson a virtuális belső hiba történt.</p>
-108    |Nem lehet a diagnosztika konfigurációs fájl konvertálása a megfigyeléssel ügynök konfigurációs fájl.<p><p>Ez a kell csak fordulhat elő, ha a beépülő modul diagnosztika manuálisan meghívott egy érvénytelen fájl a belső hiba történt.
-110|Általános diagnosztika konfigurációs hiba.<p><p>Ez a kell csak fordulhat elő, ha a beépülő modul diagnosztika manuálisan meghívott egy érvénytelen fájl a belső hiba történt.
-111|Nem indítható el a megfigyeléssel agent.<p><p>Megoldás: Győződjön meg arról, hogy elérhetők-e a megfelelő rendszer-erőforrásokat.
-112|Általános hiba


## <a name="diagnostics-data-is-not-logged-to-azure-storage"></a>Diagnosztikai adatok, akkor nincs bejelentkezve az Azure-tárolóhoz
Azure diagnosztika az Azure-tárolóban lévő minden adatokat tárolja.

Hiányzó adatok esemény legvalószínűbb oka helytelenül definiált tároló fiókadatok.

Megoldás: A diagnosztikai konfigurációs fájl javítása, illetve diagnosztika újra kell telepítenie.
Ha a probléma nem szűnik meg, újratelepítése a diagnosztika kiterjesztés, akkor előfordulhat, hogy szeretné hibakeresése után további megjeleníti a megfigyeléssel ügynök hibák keresztül. Mielőtt eseményadatok töltenek fel a tárterület-fiókjába a LocalResourceDirectory vannak tárolva.

Felhőalapú szolgáltatás szerepkör a LocalResourceDirectory van:

    C:\Resources\Directory\<CloudServiceDeploymentID>.<RoleName>.DiagnosticStore\WAD<DiagnosticsMajorandMinorVersion>\Tables

Virtuális gépeken futó a LocalResourceDirectory esetén:

    C:\WindowsAzure\Logs\Plugins\Microsoft.Azure.Diagnostics.IaaSDiagnostics\<DiagnosticsVersion>\WAD<DiagnosticsMajorandMinorVersion>\Tables

Ha nincsenek fájlok LocalResourceDirectory mappában, a felügyeleti agent nem tudja elindítani. Ennek oka általában egy érvénytelen fájl, egy eseményt, a CommandExecution.log kell megadni.

Ha a figyelése ügynök sikeresen gyűjt eseményadatok, látni fogja az egyes események a konfigurációs fájl definiált .tsf fájlokat. A figyelés ügynök hibáinak naplózza a MaEventTable.tsf a fájlban. Nézze meg a fájl tartalmát a a .tsf fájlt egy vesszőt szétválasztott values(.csv) fájllá konvertálja a tabel2csv alkalmazás használhatja:

A felhőalapú szolgáltatás szerepet:

    %SystemDrive%\Packages\Plugins\Microsoft.Azure.Diagnostics.PaaSDiagnostics\<DiagnosticsVersion>\Monitor\x64\table2csv maeventtable.tsf

D: a szokásos *rendszermeghajtón* egy felhőalapú szolgáltatás szerepkörei

Virtuális gépen:

    C:\Packages\Plugins\Microsoft.Azure.Diagnostics.IaaSDiagnostics\<DiagnosticsVersion>\Monitor\x64\table2csv maeventtable.tsf

A fenti parancsok a napló fájl *maeventtable.csv*, amelyet megnyit, és nézze meg az hibaüzenetek hoz létre.    


## <a name="diagnostics-data-tables-not-found"></a>Diagnosztikai adatok táblák nem található
Az Azure diagnosztikai adatok tartó Azure-tárolóban lévő táblák neve az alábbi kód használatával:

        if (String.IsNullOrEmpty(eventDestination)) {
            if (e == "DefaultEvents")
                tableName = "WADDefault" + MD5(provider);
            else
                tableName = "WADEvent" + MD5(provider) + eventId;
        }
        else
            tableName = "WAD" + eventDestination;

Lássunk egy példát:

        <EtwEventSourceProviderConfiguration provider=”prov1”>
          <Event id=”1” />
          <Event id=”2” eventDestination=”dest1” />
          <DefaultEvents />
        </EtwEventSourceProviderConfiguration>
        <EtwEventSourceProviderConfiguration provider=”prov2”>
          <DefaultEvents eventDestination=”dest2” />
        </EtwEventSourceProviderConfiguration>

Amely hoz létre a táblák 4:

Esemény|Táblázat neve
---|---
szolgáltató = "prov1" &lt;azonosítójú "1" = /&gt;|WADEvent + MD5("prov1") + "1"
szolgáltató = "prov1" &lt;azonosítójú = "2" eventDestination = "dest1" /&gt;|WADdest1
szolgáltató = "prov1" &lt;DefaultEvents /&gt;|WADDefault+MD5("prov1")
szolgáltató = "prov2" &lt;DefaultEvents eventDestination = "dest2" /&gt;|WADdest2
