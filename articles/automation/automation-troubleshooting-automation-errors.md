<properties
   pageTitle="Azure automatizálási hibakezelés |} Microsoft Azure"
   description="Ez a cikk lépéseit egyszerű hiba kezelése és közös Azure automatizálást hibák elhárítása."
   services="automation"
   documentationCenter=""
   authors="mgoedtel"
   manager="stevenka"
   editor="tysonn"
   tags="top-support-issue"
   keywords="automatizálási hiba, hibakezelés"/>
<tags
   ms.service="automation"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="07/06/2016"
   ms.author="sngun; v-reagie"/>

# <a name="error-handling-tips-for-common-azure-automation-errors"></a>Tippek a gyakori Azure automatizálási hibák hibakezelési

Ez a cikk ismerteti, hogy a néhány gyakori Azure automatizálási hibakódja kaphat, és a javításukhoz javasolt a lehetséges hibakezelésre vonatkozó lépéseket.

## <a name="troubleshoot-authentication-errors-when-working-with-azure-automation-runbooks"></a>Hitelesítés hibaelhárítás Azure automatizálási runbooks való munkavégzés során  

### <a name="scenario-sign-in-to-azure-account-failed"></a>Alkalmazási helyzetek: Jelentkezzen be az Azure-fiók nem sikerült.

**Hiba:** A hibaüzenet "Unknown_user_type: Ismeretlen felhasználói típus" a Hozzáadás-AzureAccount vagy bejelentkezési-AzureRmAccount parancsmagokat használatakor.

**a hiba oka:** Ez a hiba történik, ha a hitelesítő adatok eszköz neve nem érvényes, vagy ha a felhasználónevét és jelszavát, amellyel az automatizálási hitelesítőadat-eszköz a telepítő nem érvényesek.

**Hibaelhárítási tippek:** Annak megállapítása, hogy mi, kövesse az alábbi lépéseket:  

1. Győződjön meg arról, hogy nincs-e olyan speciális karakterek, például a **@** az automatizálási hitelesítő eszköz neve Azure csatlakozhat használni kívánt karaktert.  

2. Ellenőrizze, hogy használható a felhasználónevét és jelszavát, amelyeket az Azure automatizálási hitelesítő adatokhoz a helyi PowerShell ISE szerkesztőben. Ehhez a PowerShell ISE a következő parancsmagok futtatásával:  

        $Cred = Get-Credential  
        #Using Azure Service Management   
        Add-AzureAccount –Credential $Cred  
        #Using Azure Resource Manager  
        Login-AzureRmAccount –Credential $Cred

3. Ha a hitelesítés nem sikerül egy helyi meghajtóra, ez azt jelenti, hogy még nem állította be az Azure Active Directory hitelesítő adatok megfelelően. Olvassa el az Ismerkedés az Azure Active Directory-fiókját beállítva [az Azure Active Directory használatával Azure Authenticating](https://azure.microsoft.com/blog/azure-automation-authenticating-to-azure-using-azure-active-directory/) blogbejegyzés.  


### <a name="scenario-unable-to-find-the-azure-subscription"></a>Alkalmazási helyzetek: Nem található az Azure előfizetés

**Hiba:** Hibaüzenet jelenik meg a "nevű előfizetést ``<subscription name>`` nem található" Select-AzureSubscription vagy a kijelölés-AzureRmSubscription parancsmagokat használatakor.

**a hiba oka:** Ez a hiba történik, ha az előfizetés neve nem érvényes, vagy ha az előfizetés rendszergazdaként az Azure Active Directory-felhasználó, aki az előfizetés részletei kérjen próbál van beállítva.

**Hibaelhárítási tippek:** Annak megállapításához, ha megfelelően hitelesítette az Azure, és az előfizetést, válassza a kívánt hozzáférést, kövesse az alábbi lépéseket:  

1. Győződjön meg arról, hogy futtassa a **Hozzáadás-AzureAccount** a **Select-AzureSubscription** parancsmag futtatása előtt.  

2. Ha még mindig látható ez a hibaüzenet, a kód módosítása a **Get-AzureSubscription** parancsmagot a **Hozzáadás-AzureAccount** parancsmag követő hozzáadásával, és hajthat végre a kódot.  Ha a kimenet: a Get-AzureSubscription tartalmaz-e az előfizetés részletei most ellenőrizheti.  
    * Ha nem látható minden az előfizetés részletei az eredményben, ez azt jelenti, hogy az előfizetés még nem inicializált.  
    * Ha az előfizetés részletei az eredményben látható, akkor győződjön meg arról, hogy a helyes előfizetés nevét, vagy az Azonosítóját a használja a **Kijelölés-AzureSubscription** parancsmag.   


### <a name="scenario-authentication-to-azure-failed-because-multi-factor-authentication-is-enabled"></a>Alkalmazási helyzetek: Azure-hitelesítés nem sikerült, mert többtényezős hitelesítés van engedélyezve

**Hiba:** Hibaüzenet jelenik meg a "hozzáadása-AzureAccount: AADSTS50079: erős hitelesítés regisztrációs (vásárlási felfelé) szükség" Azure felhasználónevével és jelszavával való Azure hitelesítése esetén.

**a hiba oka:** Többtényezős hitelesítés Azure-fiókja van, ha az Azure Active Directory-felhasználók nem használható az Azure hitelesíteni.  Ehelyett akkor használja tanúsítvány vagy fő szolgáltatás az Azure hitelesítést végezni.

**Hibaelhárítási tippek:** Az Azure Service kezelő parancsmagok egy tanúsítványt használja, olvassa el [létrehozása és hozzáadása egy tanúsítványt Azure szolgáltatások kezelése.](http://blogs.technet.com/b/orchestrator/archive/2014/04/11/managing-azure-services-with-the-microsoft-azure-automation-preview-service.aspx) Azure erőforrás-kezelő parancsmagok egy egyszerű használatához olvassa el [a portálon Azure szolgáltatás egyszerű](./resource-group-create-service-principal-portal.md) létrehozása és [egy az Azure erőforrás-kezelő szolgáltatás egyszerű hitelesítése.](./resource-group-authenticate-service-principal.md)


## <a name="troubleshoot-common-errors-when-working-with-runbooks"></a>Gyakori hibák elhárítása runbooks való munkavégzés során

### <a name="scenario-runbook-fails-because-of-deserialized-object"></a>Alkalmazási helyzetek: Runbook deszerializált objektum miatt sikertelen

**Hiba:** A hiba miatt sikertelen a runbook "nem lehet kapcsolódni a paraméter ``<ParameterName>``. Nem tudja átalakítani a ``<ParameterType>`` Deserialized típusú érték ``<ParameterType>`` beírásához ``<ParameterType>``".

**a hiba oka:** Ha a runbook PowerShell munkafolyamat, tárolja az összetett objektumok deszerializált formátumban annak érdekében, hogy továbbra is fennáll a runbook állapotát, ha a munkafolyamat fel van függesztve.  

**Hibaelhárítási tippek:**  
Az alábbi három megoldásokkal bármelyikét lesz a probléma megoldásához:

1. Ha egy parancsmagot a másikra összetett objektumok vannak gázvezetékrendszer, tegye parancsmagok egy InlineScript.  
2. Az összetett objektum helyett a teljes objektum áthaladó átadhatja a nevét vagy a szükséges értéket.  

3. Egy PowerShell runbook használata helyett egy PowerShell-munkafolyamat runbook.  


### <a name="scenario-runbook-job-failed-because-the-allocated-quota-exceeded"></a>Alkalmazási helyzetek: Runbook feladat nem sikerült, mert a kiosztott kvóta kimerítve

**Hiba:** A runbook feladatok a hibával meghiúsul, "a futási idő havi teljes projektre vonatkozóan a kvóta elérte az előfizetéshez tartozó".

**a hiba oka:** Ez akkor fordul elő, ha a feladat végrehajtását meghaladja a fiók 500 perces ingyenes kvóta. Ez a kvóta végrehajtás projektfeladatok például egy feladat tesztelés, a projekt kezdési a portálról, a projekt végrehajtásakor webhooks használatával, és a feladat végrehajtásához, vagy az Azure portál használatával, vagy az ütemezési a adatközpontban diagramtípusokat vonatkozik. További információ az automatizálási árak információ [automatizálási árak](https://azure.microsoft.com/pricing/details/automation/).

**Hibaelhárítási tippek:** Ha havonta feldolgozási 500 percnél hosszabb használni kívánt szüksége lesz az előfizetés módosításának az az ingyenes változatról a egyszerű réteg a réteg. Az egyszerű réteg frissítsen az alábbi lépéseket:  

1. Jelentkezzen be az Azure-előfizetéséhez  
2. Jelölje ki a frissíteni kívánt automatizálási fiókot  
3. Kattintson a **Beállítások**elemre > **árak réteg és használati** > **árak szint**  
4. Jelölje ki a **Válassza ki a árak réteg** lap a **egyszerű**    


### <a name="scenario-cmdlet-not-recognized-when-executing-a-runbook"></a>Alkalmazási helyzetek: Parancsmag nem ismer fel, ha egy runbook végrehajtása

**Hiba:** A hiba miatt sikertelen a runbook feladatok "``<cmdlet name>``: A szerződési időszak ``<cmdlet name>`` szolgáltatáscsomagja nem ismerhető fel egy parancsmag, a függvény, a parancsfájlt vagy a használható program nevét."

**a hiba oka:** Ez a hiba oka, amikor a PowerShell motort nem találja az Ön használja a runbook parancsmag.  Ez akkor fordulhat elő a modult tartalmazó parancsmag nem található meg a fiók, nincs névütközés runbook néven, vagy egy másik modulban is létezik a parancsmag és automatizálási nem találja meg a nevét.

**Hibaelhárítási tippek:** Bármely, az alábbi tippekkel lesz a probléma megoldásához:  

- Ellenőrizze, hogy helyesen írt az parancsmag neve.  

- Ellenőrizze, hogy automatizálási fiókja szerepel-e a parancsmag és, hogy nincsenek-e ütközések. Annak ellenőrzéséhez, ha jelen-e a parancsmag, nyissa meg a runbook szerkesztési módban és keresése az a tár vagy a Futtatás a keresett parancsmag **Get-Command ``<CommandName>`` **.  Van-e érvényesített, amely a parancsmag érhető el a fiókot, és hogy nincs neve ütközés más parancsmagok vagy runbooks, felveheti a vászonra, és győződjön meg arról, hogy érvényes paraméter megadása a runbook a használ.  

- Ha névütközés, és a parancsmagot a két különböző modulok érhető el, a teljesen minősített neve parancsmag használatával megoldhatja a. Ha például **ModuleName\CmdletName**is használhatja.  

- Ha a runbook helyszíni végrehajtása egy hibrid dolgozó csoportban, majd győződjön meg arról, hogy a modul parancsmag telepítve van a számítógépen, amelyen a hibrid dolgozó.


### <a name="scenario-a-long-running-runbook-consistently-fails-with-the-exception-the-job-cannot-continue-running-because-it-was-repeatedly-evicted-from-the-same-checkpoint"></a>Alkalmazási helyzetek: Egy ideig futó runbook egységes nem sikerül a kivétellel: "a feladat nem tudja folytatni fut, mert többször eltávolítani az azonos ellenőrzés a kívánt".

**a hiba oka:** Ez a működés működés miatt "Valós megosztás" figyelemmel kísérése folyamatok belül Azure automatizálás, amely automatikusan felfüggeszti a runbook, ha már 3 óra hajt végre. A visszaadott hibaüzenet nem nyújt "következő" beállítások. Egy runbook is fel kell függeszteni számos oka. Hibák miatt főleg felfüggeszti a történik. Például egy runbook, hálózati hiba vagy e az összeomlást a Runbook dolgozó fut a runbook nem kivétel, az összes okoz a runbook fel kell függeszteni, és indítsa el az utolsó ellenőrzés, amikor folytatódik.

**Hibaelhárítási tippek:** A probléma elkerülése érdekében a dokumentált megoldást is pontjainak használhatók a munkafolyamatokban.  További tételéről [Tanulási PowerShell munkafolyamatok](automation-powershell-workflow.md#Checkpoints).  "Valós megosztása" és az ellenőrzés alaposabb magyarázata [Használatával pontjainak Runbooks a](https://azure.microsoft.com/en-us/blog/azure-automation-reliable-fault-tolerant-runbook-execution-using-checkpoints/)blog cikkben találhatók.


## <a name="troubleshoot-common-errors-when-importing-modules"></a>Amikor modulok importálása a gyakori hibák elhárítása

### <a name="scenario-module-fails-to-import-or-cmdlets-cant-be-executed-after-importing"></a>Alkalmazási helyzetek: Modul importálása nem sikerült, vagy parancsmagok nem hajtható végre, importálás után

**Hiba:** Modul importálása nem sikerült, és sikeresen importálja, de nincs parancsmagok kibontotta.

**a hiba oka:** Modul előfordulhat, hogy nem sikerült importálása az Azure automatizálási oka a következők:  

- A szerkezet nem egyezik meg az automatizálási szüksége van rá szükség a struktúra.  

- A modul az automatizálási fiókjára nem telepített másik modulhoz függ.  

- A modul hiányzik a mappában függőségét.  

- A **New-AzureRmAutomationModule** parancsmag töltse fel a modul használja, és nem adott a teljes tárterület elérési útját, vagy a modul nem betöltött nyilvánosan hozzáférhető URL-cím használatával.  

**Hibaelhárítási tippek:**  
Bármely, az alábbi tippekkel lesz a probléma megoldásához:  

- Győződjön meg arról, hogy a modul követi a következő formátumban:  
ModuleName.Zip **->** modulnév vagy verziószámmal **->** (ModuleName.psm1, ModuleName.psd1)

- Nyissa meg a .psd1 fájlt, és a modul jelentkező függőségek.  Ha igen, töltse fel ezeket a modulokat automatizálási fiókba.  

- Győződjön meg arról, hogy minden olyan hivatkozott .dll megtalálhatók-e a modul mappában.  


## <a name="troubleshoot-common-errors-when-working-with-desired-state-configuration-dsc"></a>A kívánt állapot beállítása (a DSC) használatakor a gyakori hibák elhárítása  

### <a name="scenario-node-is-in-failed-status-with-a-not-found-error"></a>Alkalmazási helyzetek: Az csomópont "Nem található" hibával meghibásodott állapotban van.

**Hiba:** A csomópont rendelkezik **sikertelen** állapot és a hibát tartalmazó jelentés "a művelet beolvasása a kiszolgáló https://``<url>``//accounts/``<account-id>``/Nodes(AgentId=``<agent-id>``)/GetDscAction nem sikerült, mert a érvényes konfiguráció ``<guid>`` nem található."

**a hiba oka:** Ezt a hibát jellemzően fordul elő, ha a csomópont hozzá van rendelve konfigurációs nevét (például ABC) helyett a csomópont konfigurációs nevét (például ABC. Webkiszolgáló).  

**Hibaelhárítási tippek:**  

- Győződjön meg arról, hogy a csomópontot, nem a "konfigurációs neve" és "csomópont konfigurációs neve" rendeli hozzá.  

- A csomópont konfiguráció Azure portálon csomópontra, vagy egy PowerShell-parancsmag a rendelhet.
    - Annak érdekében, hogy a csomópont konfiguráció hozzárendelése egy Azure portálon csomópontot, nyissa meg a **DSC csomópontok** lap, majd jelölje ki a csomópontot, és kattintson a **csomópontra konfigurációs hozzárendelése** gombra.  
    - Annak érdekében, hogy a csomópont konfiguráció rendel egy PowerShell-parancsmag használatával csomópontot, használja a **Set-AzureRmAutomationDscNode** parancsmag


### <a name="scenario--no-node-configurations-mof-files-were-produced-when-a-configuration-is-compiled"></a>Alkalmazási helyzetek: Nincs csomópont konfigurációk (MOF fájlok) lettek mutatni a konfiguráció összeállítása után

**Hiba:** A DSC fordítási feladatok felfüggeszti a hiba: "az összeállítás sikeresen befejeződött, de nincs csomópont konfigurációs .mofs jött létre".

**a hiba oka:** Ha a kifejezés következő kiértékeli a **csomópont** kulcsszót a DSC konfigurációs $null, majd a nincs csomópont konfigurációk gyűjtenek.    

**Hibaelhárítási tippek:**  
Bármely, az alábbi tippekkel lesz a probléma megoldásához:  

- Győződjön meg arról, hogy a **csomópont** kulcsszót a konfigurációs definícióban mellett a kifejezés nem értékel $null szeretne.  
- Akkor haladnak ConfigurationData összeállítása a konfiguráció során, győződjön meg arról, hogy átadása a várt értékek, amelyek a konfiguráció szükséges az [ConfigurationData](automation-dsc-compile.md#configurationdata).


### <a name="scenario--the-dsc-node-report-becomes-stuck-in-progress-state"></a>Alkalmazási helyzetek: A DSC csomópont jelentés beragad "folyamatban" állam

**Hiba:** DSC ügynök exportálja a "Nincs a megadott tulajdonság értékek talált példány."

**a hiba oka:** Frissítette a WMF verzióját, és rendelkezik sérült WMI.  

**Hibaelhárítási tippek:** Kövesse a [DSC ismert hibákat és korlátozásokat](https://msdn.microsoft.com/powershell/wmf/limitation_dsc) blogbejegyzés a probléma megoldásához.


### <a name="scenario--unable-to-use-a-credential-in-a-dsc-configuration"></a>Alkalmazási helyzetek: Nem lehet a hitelesítő adatok a DSC konfiguráció használata

**Hiba:** A DSC fordítási feladatok felfüggesztése a hiba: "System.InvalidOperationException hibakezelés tulajdonság"Hitelesítő"típusú ``<some resource name>``: konvertálása és tárolja a egy titkosított jelszót, egyszerű szöveg csak akkor, ha PSDscAllowPlainTextPassword van-e beállítva engedélyezett igaz".

**a hiba oka:** A hitelesítő adatok konfigurációban használt, de nem adja meg a megfelelő **ConfigurationData** **PSDscAllowPlainTextPassword** beállítása az egyes csomópont konfigurációk igaz.  

**Hibaelhárítási tippek:**  
- Ellenőrizze, hogy a megfelelő **ConfigurationData** **PSDscAllowPlainTextPassword** beállításához igaz, minden egyes csomópont konfigurálása a konfigurációban említett átadni. További tudnivalókért olvassa el az [Azure automatizálási DSC eszközök](automation-dsc-compile.md#assets).


## <a name="next-steps"></a>Következő lépések

Ha elvégezte a hibaelhárítási lépéseket, és ez a cikk bármely pontján további segítségre van szüksége, a következőkre van lehetősége:

- Segítségért felkeresheti az Azure jártas szakértőktől. A probléma elküldése a [MSDN Azure vagy Papírhalom túlcsordulás fórumok.](https://azure.microsoft.com/support/forums/)

- A fájl egy Azure támogatási kérelmeiket. Nyissa meg a [webhely támogatja az Azure](https://azure.microsoft.com/support/options/) , és kattintson a **technikai támogatás kérése** **technikai és számlázási**támogatás.

- Tartalmakat tehet közzé, egy parancsprogramot kérése a [Parancsprogram-központban](https://azure.microsoft.com/documentation/scripts/) Ha keres az Azure automatizálási runbook megoldás vagy -integráció a modul.

- Visszajelzést, vagy a szolgáltatás kérések elküldheti a [Felhasználó hangposta](https://feedback.azure.com/forums/34192--general-feedback)Azure automatizálási.
