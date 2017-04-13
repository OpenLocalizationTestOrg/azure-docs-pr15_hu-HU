<properties
   pageTitle="Azure automatizálási biztonsági |} Microsoft Azure"
   description="Ez a cikk áttekintést nyújt automatizálás, biztonság és a rendelkezésre álló különböző hitelesítési módszereket automatizálást fiókok az Azure automatizálás."
   services="automation"
   documentationCenter=""
   authors="MGoedtel"
   manager="jwhit"
   editor="tysonn"
   keywords="automatizálási biztonsági, biztonságos automatizálási" />
<tags
   ms.service="automation"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="07/29/2016"
   ms.author="magoedte" />

# <a name="azure-automation-security"></a>Azure automatizálási biztonsági
Azure automatizálási ellen erőforrások Azure, a helyszíni és más felhő szolgáltatók például Amazon Web Services (AWS) feladatok automatizálása teszi lehetővé.  Ahhoz, hogy a szükséges műveleteket a runbook akkor keresztül biztonságosan hozzáférhet az erőforrások belül az előfizetéshez szükséges minimális jogosultságokkal jogosultsággal kell rendelkeznie.  
Ez a cikk fogják ellátni a különböző hitelesítési forgatókönyvek Azure automatizálási által támogatott, és megtudhatja, hogy miként első lépések a környezet vagy kezeléséhez környezetekben alapján.  

## <a name="automation-account-overview"></a>Automatizálási fiók áttekintése
Azure automatizálási először indítja el, ha legalább egy automatizálási-fiókot kell létrehoznia. Automatizálási fiókok teszi lehetővé az automatizálási erőforrások (runbooks, eszközök, konfigurációk) elkülönítése más automatizálási fiókokban található forrásokból. Automatizálási fiókokba erőforrások bonthatók külön logikai környezetben. Fiókok használható például fejlesztési, egy másik gyártási és egy másik, a helyszíni környezetben.  Azure automatizálási fiók eltér a Microsoft-fiókkal vagy az Azure-előfizetése készített fiókok.

Az automatizálási erőforrások minden automatizálási fiókhoz társított egyetlen Azure terület, de automatizálási fiókok bármelyik tartományban lévő erőforrások kezelheti. A fő oka automatizálási-fiókok létrehozása a különböző régiók akkor is, hogy az adatok és erőforrások kell elkülöníteni egy adott területre házirendek esetén.

>[AZURE.NOTE]Automatizálási partnerek és a bennük, amely erőforrások jönnek létre az Azure-portálon, nem érhető el az Azure klasszikus portálon. Ha szeretné az ezekben a fiókokban vagy a Windows PowerShell a források kezelése, az erőforrás-kezelő Azure modulok kell használnia.

Minden, ellen erőforrások Azure erőforrás-kezelő és az Azure parancsmag használatával az Azure automatizálás végeznek feladatot kell jelentkeznie Azure Azure Active Directory szervezeti azonosítójával hitelesítőadat-alapú hitelesítés használatával.  Tanúsítvány-alapú hitelesítés az eredeti hitelesítési mód Azure Szolgáltatáskezelés üzemmódban, de összetettek ahhoz, hogy a telepítő volt.  Azure AD-felhasználóval való Azure hitelesítése lett nemcsak a 2014-es bevezetett vissza egyszerűbbé-hitelesítés fiókot beállítani, de is támogat, az azt jelenti, hogy az erőforrás-kezelő Azure és a klasszikus erőforrások működő egyetlen felhasználói fiók Azure nem interaktív hitelesíteni.   

Jelenleg, ha az Azure-portálon hoz létre egy új automatizálási fiókot, akkor automatikusan létrehoz:

-  Létrehoz egy új szolgáltatás egyszerű az Azure Active Directory, a tanúsítvány és a közös munka szerepköralapú hozzáférés-szerepalapú, runbooks segítségével erőforrás-kezelő erőforrások kezelésére szolgáló rendeli fiókként fusson.
-  Klasszikus Futtatás mint fiók feltöltésével, Azure Szolgáltatáskezelés vagy klasszikus erőforrásoknak runbooks kezelésére szolgáló felügyeleti tanúsítvány.  

Szerepköralapú hozzáférés-vezérlés érhető el az Azure erőforrás-kezelő adja meg az Azure Active Directory felhasználói fiók és Futtatás mint fiók engedélyezett műveletek, majd a szolgáltatás egyszerű hitelesítést végezni.  Olvassa el a [szerepköralapú hozzáférés-vezérlés Azure automatizálási a cikk](../automation/automation-role-based-access-control.md) további információt a modell automatizálási engedélyek kezelésére szolgáló segíthessenek.  

A adatközpontban vagy szemben a AWS services számítások hibrid Runbook dolgozó futó Runbooks ugyanazt a módszert, amellyel a szokásos runbooks hitelesítése Azure erőforrásokhoz nem használható.  Ennek az oka, hogy ezek az erőforrások Azure-Ön kívüli rendszert futtat, és így a saját hitelesítő adatokat az automatizálási információforrások, amelyek helyileg lesz hozzáférésük hitelesíteni definiált esetén kell.  

## <a name="authentication-methods"></a>Hitelesítési módszerek

Az alábbi táblázat összefoglalja a különböző hitelesítési módszereket Azure automatizálást és a cikk azt mutatja be, a runbooks hitelesítés beállítása által támogatott környezetben.

A módszer  |  Környezet  | A cikk
----------|----------|----------
Azure Active Directory-felhasználói fiók | Azure erőforrás-kezelő és Azure szolgáltatás kezelése | [Azure Active Directory felhasználói fiók Runbooks hitelesítéshez](../automation/automation-sec-configure-aduser-account.md)
Azure Futtatás fiókként | Azure erőforrás-kezelő | [Hitelesítő Runbooks Azure Futtatás mint fiókkal](../automation/automation-sec-configure-azure-runas-account.md)
Azure klasszikus Futtatás fiókként | Azure szolgáltatás kezelése | [Hitelesítő Runbooks Azure Futtatás mint fiókkal](../automation/automation-sec-configure-azure-runas-account.md)
Windows-hitelesítés | A helyszíni adatközponthoz | [Runbooks hitelesíteni a hibrid Runbook dolgozók számára](../automation/automation-hybrid-runbook-worker.md)
AWS hitelesítő adatok | Amazon webszolgáltatások | [Hitelesítő Runbooks Amazon webszolgáltatással (AWS)](../automation/automation-sec-configure-aws-account.md)



