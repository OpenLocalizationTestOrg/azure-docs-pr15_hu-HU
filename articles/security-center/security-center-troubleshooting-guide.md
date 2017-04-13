<properties
   pageTitle="Azure biztonsági központ hibaelhárítási útmutatója |} Microsoft Azure"
   description="A dokumentum Azure biztonság otthon problémák segítségével."
   services="security-center"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor=""/>

<tags
   ms.service="security-center"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/18/2016"
   ms.author="yurid"/>

# <a name="azure-security-center-troubleshooting-guide"></a>Azure biztonsági központ hibaelhárítási útmutatója
Ez az útmutató olyan információk informatikai szakemberek számára, a adatok biztonsági elemzőknek és a felhő rendszergazdák amelynek szervezetek Azure biztonság otthon használja, és elhárítása biztonság otthon kapcsolatos problémákat.

## <a name="troubleshooting-guide"></a>Hibaelhárítási útmutatója
Ez az útmutató ismerteti, hogyan kapcsolatos hibák elhárítása biztonság otthon kapcsolatos problémákat. A hibaelhárítás biztonság otthon végezhető el a legtöbb kerül sor első megjeleníti a sikertelen összetevő [Napló](https://azure.microsoft.com/updates/audit-logs-in-azure-preview-portal/) rekordjainak. A naplókat, és eldöntheti:

- Milyen műveleteket készítésének helye
- A művelet kezdeményezett ki
- Mikor történt, a művelet
- A művelet állapota
- Más tulajdonságaik vannak, amelyek segítséget jelenthet az értékeket a művelet kutatása

A napló tartalmazza az összes írási műveleteket (helyezése, a BEJEGYZÉST, DELETE) az erőforrások, azonban nem része a olvasási műveletek (GET).

## <a name="troubleshooting-monitoring-agent-installation-in-windows"></a>A Windows felügyeleti ügynök telepítési hibáinak elhárítása

A biztonság otthon ügynök figyelése adatgyűjtés végrehajtásához használják. Miután adatgyűjtés engedélyezve van, és a agent megfelelően telepítve van a cél gépen, ezek a folyamatok kell végrehajtása:

- ASMAgentLauncher.exe - Azure ügynök figyelése 
- ASMMonitoringAgent.exe - bővítmény Azure biztonsági figyelése
- ASMSoftwareScanner.exe – Azure ellenőrzés-kezelő

Az Azure biztonsági figyelése bővítmény keres a különböző biztonsági vonatkozó beállításokat, és a virtuális gép gyűjti össze a biztonsági naplók. Javítás képolvasóval ellenőrzési menedzserének lesz.

Ha sikeresen megtörténik a telepítés meg kell jelennie egy bejegyzés alatt egy, a naplókat, a cél virtuális a hasonló:

![Naplókat](./media/security-center-troubleshooting-guide/security-center-troubleshooting-guide-fig1.png)

További információt a telepítési folyamatot szerezze be a ügynök naplók, *%systemdrive%\windowsazure\logs* található olvasásával (Példa: C:\WindowsAzure\Logs).

> [AZURE.NOTE] Ha az Azure biztonsági központ ügynök van nem megfelelően viselkedő, szüksége lesz indítsa újra a cél virtuális, mivel nincs leállítása és elindítása a agent parancs.

## <a name="troubleshooting-monitoring-agent-installation-in-linux"></a>A Linux felügyeleti ügynök telepítési hibáinak elhárítása
Amikor egy Linux rendszerben a virtuális ügynök telepítési hibáinak elhárítása győződjön meg róla, hogy a bővítmény való/var/tár vagy waagent/letöltött. Futtathatja a ellenőrizze, hogy telepítették-e meg az alábbi parancsot:

`cat /var/log/waagent.log` 

A többi naplófájlok, akkor ellenőrizheti, hogy hibaelhárítási célból a következők: 

- /var/log/mdsd.err
- / var/log/azure /

A működő rendszer látnia kell a mdsd folyamat kapcsolat TCP 29130. A kommunikáció a mdsd folyamat syslog az. Ez a jelenség ellenőrizheti a következő parancs futtatásával:

`netstat -plantu | grep 29130`

## <a name="contacting-microsoft-support"></a>Kapcsolatfelvétel a Microsoft terméktámogatás

Problémák elhárításához azonosíthatók, használja az útmutatásokat, feltéve ebben a cikkben mások is megtalálhatók a biztonság otthon nyilvános [fórum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureSecurityCenter)ismertetett. Jó helyen jár, ha van szüksége a további hibaelhárítási, megnyithatja új támogatási kérelem Azure portál használatával, alább látható módon: 

![Microsoft terméktámogatás](./media/security-center-troubleshooting-guide/security-center-troubleshooting-guide-fig2.png)


## <a name="see-also"></a>Lásd még:

A jelen dokumentum biztonsági házirendek beállítása az Azure biztonság otthon megtanulta azt. Többet szeretne tudni az Azure biztonság otthon, olvassa el az alábbiakat:

- [Azure biztonsági központ tervezése és a műveletek útmutató](security-center-planning-and-operations-guide.md) – megtudhatja, hogy miként tervezése és Azure biztonság otthon elfogadja a tervezés kapcsolatos szempontok ismertetése.
- [Az Azure biztonság otthon figyelése, biztonsági állapot](security-center-monitoring.md) – útmutató a Lync-állapota az Azure erőforrások
- [Kezelése és válaszol a biztonsági figyelmeztetések az Azure biztonság otthon](security-center-managing-and-responding-alerts.md) – megtudhatja, hogy miként kezelheti, és a biztonsági figyelmeztetések megválaszolása
- [Az Azure biztonság otthon partnermegoldások figyelése](security-center-partner-solutions.md) – megtudhatja, hogy miként figyelheti a partnerek megoldásaival állapotának.
- [Azure biztonsági központja – gyakori kérdések](security-center-faq.md) – keresés a szolgáltatással kapcsolatos gyakori kérdések
- [Azure biztonsági Blog](http://blogs.msdn.com/b/azuresecurity/) – Azure biztonság és megfelelőség kapcsolatos blogbejegyzéseket megkeresése
