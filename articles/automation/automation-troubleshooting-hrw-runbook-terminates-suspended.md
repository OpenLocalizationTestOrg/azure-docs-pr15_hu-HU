<properties
   pageTitle="Hibrid Runbook dolgozó: Felfüggesztett állapotban megszakítja A runbook feladat |} Microsoft Azure"
   description="A jelenség okok és megoldások hibrid Runbook dolgozó feladat lemondási hiba."
   services="automation"
   documentationCenter=""
   authors="mgoedtel"
   manager="jwhit"
   editor="tysonn" />
<tags
   ms.service="automation"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/17/2016"
   ms.author="magoedte" />

# <a name="hybrid-runbook-worker-a-runbook-job-terminates-with-a-status-of-suspended"></a>Hibrid Runbook dolgozó: Felfüggesztett állapotban megszünteti A runbook feladat

## <a name="summary"></a>Összefoglalás

A runbook hamarosan után háromszor végrehajtásához próbál fel van függesztve. Megszakíthatja a sikeres befejezését runbook feltételeket, és a kapcsolódó hibaüzenet nem tartalmaz az esetleges további információkat, amely jelzi, hogy miért. Ez a cikk hibaelhárítási lépéseit a hibrid Runbook dolgozó runbook végrehajtási hibákat tapasztal, kapcsolatos problémák.

Ha az Azure nem kérdést az Ez a cikk, keresse fel a Azure fórumait [MSDN és a túlcsordulás Papírhalom](https://azure.microsoft.com/support/forums/). A problémát a következő fórumokon, vagy tehetnek [ @AzureSupport a Twitteren](https://twitter.com/AzureSupport). Azure támogatási kérelmet is is fájl kiválasztásával a **technikai támogatás kérése** az [Azure támogatja a](https://azure.microsoft.com/support/options/) webhelyen.

## <a name="symptom"></a>A jelenség

Runbook végrehajtása nem sikerült, és a visszaadott hiba "a"Aktiválás"nem futtatható, mert a folyamat leállítása váratlanul feladat műveletet. A feladat művelet kísérlet történt 3 időpontok."


## <a name="cause"></a>OK

Létezik a hiba több lehetséges okai: 

  1. A hibrid dolgozó van a tűzfal vagy proxy mögött.
  2. A hibrid dolgozó futtató számítógép van kisebb, mint a minimális [követelmények](automation-hybrid-runbook-worker.md#hybrid-runbook-worker-requirements) 
  3. A runbooks nem tudja hitelesíteni helyi erőforrásokkal


## <a name="cause-1-hybrid-runbook-worker-is-behind-proxy-or-firewall"></a>: 1 hibrid Runbook dolgozó oka proxykiszolgáló vagy a tűzfal mögött

A hibrid Runbook dolgozó futtató számítógép tűzfalat vagy proxy mögött van, és kimenő hálózati hozzáférés nem előfordulhat, hogy engedélyezett vagy helyesen beállítva.

### <a name="solution"></a>Megoldás

Ellenőrizze, hogy a számítógép hozzáfér kimenő *. cloudapp.net portokra 443, 9354 és 30000-30199. 

## <a name="cause-2-computer-has-less-than-minimum-hardware-requirements"></a>2 okai lehetnek: A számítógépben kisebb, mint a minimális hardverkövetelményei

A hibrid Runbook dolgozó futtató számítógépeknek előtt, hogy ez a szolgáltatás üzemelteti kijelölő a minimális hardverkövetelményeknek feleljen meg. Ellenkező esetben az attól függően, hogy az erőforrás-kihasználtság más háttérfolyamatok és a végrehajtás során runbooks okozta kérelem, a számítógép feletti használni fog válik, és runbook feladat késések vagy időtúllépései okozhat. 

### <a name="solution"></a>Megoldás 

Először ellenőrizze a hibrid Runbook dolgozó funkció futtatása kijelölt számítógép megfelel-e a minimális.  Ha igen, figyelheti a Processzor- és a kihasználtság között a hibrid Runbook dolgozó folyamat teljesítményét és a Windows bármely korrelációs határozza meg.  Ha memóriaméret vagy Processzor nyomása, jelezheti, hogy szükséges a frissítése további processzorok hozzáadásával vagy erőforrás szűk cím és a hiba megoldásához memória növeléséhez. Azt is megteheti válassza ki a különböző számítási erőforrást, amelyek támogatják a minimális követelmények és arányának amikor terhelést igények jelezheti növekedését szükség.         

## <a name="cause-3-runbooks-cannot-authenticate-with-local-resources"></a>OK 3: Runbooks hitelesítő nem lehet helyi erőforrásokkal

### <a name="solution"></a>Megoldás

Jelölje be a megfelelő esemény *Win32 folyamat kilépett [4294967295] kóddal*leírás a **Microsoft-SMA** eseménynaplójában talál.  Ez a hiba oka, még nem a runbooks hitelesítéshez konfigurált vagy Futtatás mint hitelesítő adatait a hibrid dolgozó csoportban megadott.  Véleményezze a következőt [Runbook engedélyek](automation-hybrid-runbook-worker.md#runbook-permissions) megfelelően hitelesítési beállította a runbooks megerősítéséhez.  


 

