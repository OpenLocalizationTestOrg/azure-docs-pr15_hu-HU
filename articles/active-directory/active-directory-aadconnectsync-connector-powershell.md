<properties
   pageTitle="A PowerShell összekötő |} Microsoft Azure"
   description="Ez a cikk ismerteti, hogyan kell konfigurálni a Microsoft Windows PowerShell-összekötő."
   services="active-directory"
   documentationCenter=""
   authors="AndKjell"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.workload="identity"
   ms.tgt_pltfrm="na"
   ms.devlang="na"
   ms.topic="article"
   ms.date="08/30/2016"
   ms.author="billmath"/>

# <a name="windows-powershell-connector-technical-reference"></a>A Windows PowerShell-összekötő technikai útmutató
Ez a cikk ismerteti a Windows PowerShell-összekötő. A cikk az alábbi termékek vonatkozik:

- A Microsoft Identitáskezelő 2016 (MIM2016)
- A Forefront identitáskezelő 2010 R2 (FIM2010R2)
    -   Használjon 4.1.3671.0 változata, illetve az újabb [KB3092178](https://support.microsoft.com/kb/3092178).

MIM2016 és FIM2010R2 az összekötő érhető el a [Microsoft letöltőközpontból](http://go.microsoft.com/fwlink/?LinkId=717495)letöltésként.

## <a name="overview-of-the-powershell-connector"></a>A PowerShell összekötő áttekintése
A PowerShell Connector lehetővé teszi a szinkronizálási szolgáltatás integráció a Windows PowerShell-alapú API-khoz kínáló külső rendszerek. Az összekötő itt hidat között a hívás-alapú bővíthető kapcsolatok kezelése agent lehetőségeit, 2 (ECMA2) keretrendszer és a Windows PowerShell. ECMA keretében kapcsolatos további tudnivalókért lásd: a [Bővíthető kapcsolódási 2.2 Management ügynök hivatkozást](https://msdn.microsoft.com/library/windows/desktop/hh859557.aspx).

### <a name="prerequisites"></a>Előfeltételek
Mielőtt az összekötő használni, győződjön meg arról, hogy a szinkronizálás kiszolgálón a következő:

- 4.5.2 a Microsoft .NET-keretrendszer vagy újabb verzió
- A Windows PowerShell 2.0, 3.0-s vagy 4.0

Az adatvégrehajtás házirendet a szinkronizálási szolgáltatás kiszolgálón kell beállítania, hogy engedélyezze az összekötő futtatásához a Windows PowerShell-parancsfájlokat. Kivéve, ha a parancsfájlok, lefut az összekötő digitálisan aláírt, állítsa be az adatvégrehajtás-házirend Ez a parancs futtatásával:  
`Set-ExecutionPolicy -ExecutionPolicy RemoteSigned`

## <a name="create-a-new-connector"></a>Hozzon létre egy új összekötőt
A szinkronizálási szolgáltatás a Windows PowerShell-összekötő létrehozásához meg kell adnia a szinkronizálási szolgáltatás által igényelt lépések végrehajtása Windows PowerShell-parancsprogramok sorozata. Attól függően, hogy az adatforrás való csatlakozás és funkciókra van szüksége be kell állítani a parancsfájlok változik. Ez a szakasz ismertet egyes a parancsfájlok, amely kell végrehajtani, és mikor szükség.

A Windows PowerShell-összekötő a parancsfájlok belül a szinkronizálási szolgáltatás adatbázis mindegyike tárolására szolgál. A fájlrendszerben tárolt parancsfájlok futtatásának lehetőség, azt is könnyebben beszúrása a szervezet minden parancsfájlok közvetlenül az összekötő beállításait.

Egy PowerShell-összekötő létrehozásához **Szinkronizálási szolgáltatás** válassza a **Kezelés ügynök** és **létrehozása**. Jelölje be a **PowerShell (Microsoft)** összekötő.

![Összekötő létrehozása](./media/active-directory-aadconnectsync-connector-powershell/createconnector.png)

### <a name="connectivity"></a>Kapcsolat
Adja meg a beállítandó történő csatlakozás távoli rendszer. Ezek az értékek biztonságosan a szinkronizálási szolgáltatás által tárolt és rendelkezésére a Windows PowerShell-parancsfájlokat az összekötő futtatásakor.

![Kapcsolat](./media/active-directory-aadconnectsync-connector-powershell/connectivity.png)

A következő kapcsolódási paramétereket adhatja meg:

**Kapcsolat**

Paraméter | Alapérték | Cél
--- | --- | ---
Kiszolgáló | <Blank> | Kiszolgáló neve, amely az összekötő csatlakozzon.
Tartomány | <Blank> | A hitelesítő adatok tárolására használatra, az összekötő futtatásakor tartományát.
Felhasználói | <Blank> | Username a hitelesítő adatok tárolására használatra, az összekötő futtatásakor.
Jelszó | <Blank> | A hitelesítő adatok tárolására használatra, az összekötő futtatásakor jelszavát.
Összekötő fiók megszemélyesítés | Hamis | Ha értéke igaz, a szinkronizálási szolgáltatás fut a Windows PowerShell-parancsfájlokat a megadott hitelesítő adatok környezetében. Ha lehetséges, azt ajánljuk, hogy a **$Credentials** paraméter átadott az egyes parancsfájl megszemélyesítési helyett használja. Ezzel a beállítással, akkor olvassa el a [További konfigurációja megszemélyesítési](#additional-configuration-for-impersonation)szükséges további engedélyekről további információt.
Felhasználói profil betöltése, ha magát | Hamis | Arra utasítja az összekötő hitelesítő adatokat a felhasználói profil betöltését megszemélyesítés során. Ha a megszemélyesített felhasználónak van egy központi profilt, az összekötő nem töltődik be a központi profilt. A paraméter című témakörben olvashat [További konfigurációja megszemélyesítési](#additional-configuration-for-impersonation)szükséges további engedélyekről további információt.
Ha magát a bejelentkezési típus | Nincs lehetőség | Bejelentkezési típusa megszemélyesítés során. További tudnivalókért tekintse át a [dwLogonType] [ dw] dokumentációt.
Csak az aláírt parancsfájlok | Hamis | IGAZ, ha a Windows PowerShell-összekötő azt ellenőrzi, hogy minden parancsfájl van-e a digitális aláírás érvényes. Ha értéke HAMIS, ellenőrizze, hogy a szinkronizálási szolgáltatás kiszolgáló a Windows PowerShell végrehajtás házirend RemoteSigned vagy a nem korlátozott.

**Közös modul**  
Az összekötő lehetővé teszi a megosztott Windows PowerShell-modult tárolni a konfigurációban. Parancsfájl az összekötő futtatásakor a Windows PowerShell-modult kicsomagolta a fájlrendszer, hogy az egyes parancsfájl importálhatók.

A jelszó-szinkronizálás, importálása és exportálása parancsfájlok a közös modul kicsomagolta az összekötő MAData mappájában. A közös modul kicsomagolta séma, adatérvényesítési, hierarchia és partíciót feltárás parancsfájlok a % TEMP % mappát. Mindkét esetben a kibontott közös modul parancsfájl neve szerint a közös modul parancsfájl neve beállítást.

Töltse be a modul FIMPowerShellConnectorModule.psm1 nevű a MAData mappából, használja az alábbi utasítás:`Import-Module (Join-Path -Path [Microsoft.MetadirectoryServices.MAUtils]::MAFolder -ChildPath "FIMPowerShellConnectorModule.psm1")`

Töltse be a % TEMP % mappát FIMPowerShellConnectorModule.psm1 hívását modul, használja az alábbi utasítás:`Import-Module (Join-Path -Path $env:TEMP -ChildPath "FIMPowerShellConnectorModule.psm1")`

**Paraméter érvényesítése**  
Az adatérvényesítési parancsfájl egy nem kötelező a Windows PowerShell-parancsprogramot, győződjön meg arról, hogy a rendszergazda által megadott összekötő konfigurációs paramétereket érvényesek használható. Érvényesítése server, a kapcsolat hitelesítő adatok és a kapcsolat paraméterek olyan gyakori felhasználási lehetőségei az érvényesítési parancsfájl. Az adatérvényesítési parancsfájl neve után a következő lapokon és párbeszédpanelek módosulnak:

- Kapcsolat
- A globális paraméterek
- Partition konfigurálása

Az adatérvényesítési parancsfájl a következő paraméterek kap az összekötő:

név | Adattípus | Leírás
--- | --- | ---
ConfigParameterPage | [ConfigParameterPage][cpp] | A konfiguráció lap vagy párbeszédpanel, amelyen az érvényesítési kérelem.
ConfigParameters | [A KeyedCollection gyűjteményben] [ keyk] [karakterlánc, [ConfigParameter][cp]] | Az összekötő konfigurációs paramétereinek táblázatot.
Hitelesítő adatok | [PSCredential][pscred] | A kapcsolat lapon a rendszergazda által megadott hitelesítő adatokat tartalmazza.

Az adatérvényesítési parancsfájl ParameterValidationResult egyetlen objektum térjen vissza a folyamat.

**Séma feltárás**  
A séma feltárás parancsfájl kötelező. A parancsfájl adja vissza az objektum típusa, attribútumok és attribútum kényszerek, a szinkronizálási szolgáltatást használó attribútum levéltovábbítási szabályok beállításakor. A séma feltárás parancsfájl összekötő létrehozása során fut, és az összekötő séma kitölt. Azt is használják a séma frissítése művelet a szinkronizálás szolgáltatáskezelő.

A séma feltárás parancsfájl a következő paraméterek kap az összekötő:

név | Adattípus | Leírás
--- | --- | ---
ConfigParameters | [A KeyedCollection gyűjteményben] [ keyk] [karakterlánc, [ConfigParameter][cp]] | Az összekötő konfigurációs paramétereinek táblázatot.
Hitelesítő adatok | [PSCredential][pscred] | A kapcsolat lapon a rendszergazda által megadott hitelesítő adatokat tartalmazza.

A parancsfájl kell eredményül adnia [sémát] egyetlen[ schema] a folyamat-objektumot. A Schema objektum tevődik össze [SchemaType] [ schemaT] objektumtípusok megjelenítő objektumok (például: felhasználók és csoportok). A SchemaType objektum [SchemaAttribute] gyűjteményét tartalmazza[ schemaA] a tulajdonságait megjelenítő objektumok (például: utónév vezetéknév és postai címe) típusú.

**További paramétereket**  
A szokásos beállításai mellett további egyéni beállításokat az összekötő példányának jellemző adhatja meg. A partíciók, az összekötő adható meg a paraméterek vagy futtatása lépés szintek, és a megfelelő Windows PowerShell-parancsprogramot webböngészőn keresztül. Egyéni beállításokkal tárolhatók a szinkronizálási szolgáltatás adatbázisban az egyszerű szöveges formátumban, vagy előfordulhat, hogy titkosítja. A szinkronizálási szolgáltatás automatikusan titkosítja és visszafejtése biztonságos konfigurációs beállítások szükség esetén.

Egyéni beállítások megadásához elválaszthatja egymástól vesszővel (,) mindegyik paraméter neve.

Egyéni beállításokkal egy forgatókönyv eléréséhez az aláhúzásjel nevét kell utótag ( \_ ) és a paraméter (globális, partíciók vagy RunStep) körét. A globális fájlnév paraméter elérésére, például a kódtöredék használja:`$ConfigurationParameters["FileName_Global"].Value`

### <a name="capabilities"></a>Funkciók
A kezelés Agent tervező funkciók lapja a viselkedés és a használható funkciók körét, az összekötő határozza meg. Nem lehet módosítani a választott ezen a lapon, az összekötő létrehozásakor. A következő táblázat a képesség beállításait.

![Funkciók](./media/active-directory-aadconnectsync-connector-powershell/capabilities.png)

Lehetőség | Leírás |
--- | --- |
[Megkülönböztető név stílus][dnstyle] | Azt jelzi, ha az összekötő támogatja megkülönböztető név, és ha igen, mik stílus.
[Exportálás típusa][exportT] | Határozza meg, hogy milyen típusú objektumok, amelyek bemutatják az exportálási parancsfájl. <li>AttributeReplace – a skype_for_businesshoz egy többértékű attribútum értékeket tartalmazza, az attribútum megváltozásakor.</li><li>AttributeUpdate – csak egy többértékű attribútum delta tartalmazza, ha az attribútum változik.</li><li>MultivaluedReferenceAttributeUpdate - értékeket nem hivatkozás többértékű attribútum és a hivatkozás többértékű attribútum csak delta teljes készlete tartalmazza.</li><li>ObjectReplace – az objektumok attribútumainak tartalmazza, amikor bármely attribútum módosítások</li>
[Adatok normalizálás][DataNorm] | Arra utasítja, a szinkronizálási szolgáltatás normalizálandó horgony attribútumok, mielőtt a parancsfájlok állnak rendelkezésre.
[Objektum megerősítése][oconf] | Konfigurálja a függőben lévő import viselkedése a szinkronizálás szolgáltatásban. <li>Normál – importálás keresztül kell erősíteni minden exportált módosításai vár, alapértelmezett működés</li><li>NoDeleteConfirmation – objektum törlésekor nem nincs függőben lévő import hozza létre.</li><li>NoAddAndDeleteConfirmation – objektum létrehozásakor vagy törli, nem nincs függőben lévő import hozza létre.</li>
Használja az DN horgony | Ha a megkülönböztető név stílus LDAP van beállítva, a horgony attribútum összekötő térköz is megkülönböztető név.
Több összekötők egyidejű műveletek | Ha be van jelölve, több Windows PowerShell az összekötők egyszerre futtatható.
Partíciót | Ha be van jelölve, az összekötő támogatja a több partíciók és partíciót feltárás.
Hierarchia | Ha be van jelölve, az összekötő támogatja a egy LDAP stílus hierarchikus szerkezete.
Importálás engedélyezése | Ha be van jelölve, az összekötő importálja a adatok importálása parancsfájlok keresztül.
Delta importálása engedélyezése | Ha be van jelölve, az összekötő is kérhet delta az importálás parancsfájlokat.
Exportálás engedélyezése | Ha be van jelölve, a az összekötő exportálja az adatokat használja export-parancsfájlokat.
Teljes Exportálás engedélyezése | Ha be van jelölve, az Exportálás parancsfájlokat támogatja a exportálása a teljes összekötő szóköz. Ez a funkció használatához engedélyezése exportálása is ellenőrizni kell.
Nincs hivatkozás értékeket az első exportálás fázisban | Ha be van jelölve, hivatkozás attribútumok exportálja a program egy második exportálás lépéssel.
Objektum átnevezése engedélyezése | Ha be van jelölve, megkülönböztető név módosítható.
Törlés-adja meg cseréje | Ha be van jelölve, törlés hozzáadása-műveletek exportálja a program egyetlen helyettesítőként.
Jelszó műveletek engedélyezése | Ha be van jelölve, a jelszó-szinkronizálás parancsfájlok támogatottak.
Az első fázisban exportálás jelszó engedélyezése | Ha be van jelölve, a jelszavak beállítása során kiépítési az objektum létrehozásakor exportálja.

### <a name="global-parameters"></a>A globális paraméterek
A globális paraméterek fülre, a kezelés ügynök tervezőben lehetővé teszi, hogy állítsa be az összekötő által futtatott Windows PowerShell-parancsfájlokat. A kapcsolat lapon egyéni konfigurációs beállítások globális értékek is beállítható.

**Partition feltárás**  
Egy partíciót egy megosztott séma belül külön névteret. Ha például az Active Directory minden tartománya partíciót egy erdőn belül. Partíciót a logikai csoportosítása az importálás és exportálás műveleteket. Az importálás és exportálás partíciót rendelkezni, mint a környezetben, és az összes művelet történik a környezetben. Partíciót kellene jelenítik meg az LDAP hierarchiát. A partíciók megkülönböztető név szolgál az importálás győződjön meg róla, hogy az összes visszaadott objektum partíciót hatálya alá. Partition megkülönböztető név is használják során az összekötő területre a metaverse a kiépítési az exportálás során objektum társított kell partíciót határozza meg.

A partíciók feltárás parancsfájl a következő paraméterek kap az összekötő:

név | Adattípus | Leírás
--- | --- | ---
ConfigParameters  | [A KeyedCollection gyűjteményben][keyk][karakterlánc, [ConfigParameter][cp]] | Az összekötő konfigurációs paramétereinek táblázatot.
Hitelesítő adatok | [PSCredential][pscred] | A kapcsolat lapon a rendszergazda által megadott hitelesítő adatokat tartalmazza.

A parancsprogram vissza kell térnie a vagy egy olyan [partíciót] [ part] objektum vagy a folyamat partíciót objektumok [T] listáját.

**Hierarchia feltárás**  
A hierarchia feltárás parancsfájl csak böngészik megkülönböztető név stílus képes LDAP. A parancsprogram lehetővé teszik tallózással keresse meg és jelölje ki a tárolók csoportja, amely számít vagy kicsinyítése alapján való importálás és exportálás műveletek szolgál. A parancsprogram csak biztosítania kell a legfelső szintű csomópontot, adja meg a parancsfájlt közvetlen gyermekeinek csomópontok listáját.

A hierarchia feltárás parancsfájl a következő paraméterek kap az összekötő:

név | Adattípus | Leírás
--- | --- | ---
ConfigParameters | [A KeyedCollection gyűjteményben][keyk][karakterlánc, [ConfigParameter][cp]] | Az összekötő konfigurációs paramétereinek táblázatot.
Hitelesítő adatok | [PSCredential][pscred] | A kapcsolat lapon a rendszergazda által megadott hitelesítő adatokat tartalmazza.
ParentNode | [HierarchyNode][hn] | A hierarchia, amelyek alapján a parancsfájl közvetlen gyermekek térjen vissza a legfelső szintű csomópontot.

A parancsprogram vissza kell térnie a vagy a egyetlen gyermek HierarchyNode objektum vagy a gyermek HierarchyNode objektumok [T] listája a folyamat.

#### <a name="import"></a>Importálás
Összekötők importálási műveletekhez támogató három parancsfájlok kell végrehajtania.

**Importálás megkezdéséhez.**  
A kezdő importálása parancsfájl az importálás futtatása lépésnek elején fut. Ebben a lépésben során a forrás rendszer kapcsolatot létesíteni, és végezze el az előkészítő lépéseket az adatok importálása a csatlakoztatott rendszerből előtt.

A kezdő importálása parancsfájl a következő paraméterek kap az összekötő:

név | Adattípus | Leírás
--- | --- | ---
ConfigParameters | [A KeyedCollection gyűjteményben][keyk][karakterlánc, [ConfigParameter][cp]] | Az összekötő konfigurációs paramétereinek táblázatot.
Hitelesítő adatok | [PSCredential][pscred] | A kapcsolat lapon a rendszergazda által megadott hitelesítő adatokat tartalmazza.
OpenImportConnectionRunStep | [OpenImportConnectionRunStep][oicrs] | A parancsprogram tájékoztatja arról az importálás futtatása (delta vagy teljes) partíciót, a hierarchia, a vízjelet és a várt oldalméret típusú.
Típusai | [Séma][schema] | Az összekötő terület importált sémája.

A parancsprogram vissza kell térnie a egyetlen [OpenImportConnectionResults] [ oicres] objektum a folyamat, például:`Write-Output (New-Object Microsoft.MetadirectoryServices.OpenImportConnectionResults)`

**Adatok importálása**  
Az importálás parancsfájl nevezik az összekötő mindaddig, amíg a parancsfájl, az azt jelzi, hogy nincs-e több adat importálása. A Windows PowerShell-összekötő a 9999-objektumok az oldalméret tartalmaz. Ha a parancsprogram 9999-nél több objektumok importálása ad vissza, támogatja a lapozási. Az összekötő biztosítja, hogy is használhatja a tár vízjel, hogy minden alkalommal, amikor az importálási parancsfájl egyéni tulajdonság neve, a parancsprogram folytatja a importálná az objektumokat, ahol abbahagyta.

Az importálás parancsfájl a következő paraméterek kap az összekötő:

név | Adattípus | Leírás
--- | --- | ---
ConfigParameters | [A KeyedCollection gyűjteményben][keyk][karakterlánc, [ConfigParameter][cp]] | Az összekötő konfigurációs paramétereinek táblázatot.
Hitelesítő adatok | [PSCredential][pscred] | A kapcsolat lapon a rendszergazda által megadott hitelesítő adatokat tartalmazza.
GetImportEntriesRunStep | [ImportRunStep][irs] | A vízjel (CustomData) közben használható lapozható import, és importálja a delta tárolja.
OpenImportConnectionRunStep | [OpenImportConnectionRunStep][oicrs] | A parancsprogram tájékoztatást arról, az importálás futtatása (delta vagy teljes) partíciót, a hierarchiát, a vízjelet és a várt oldalméret típusú.
Típusai | [Séma][schema] | Az összekötő terület importált sémája.

Az importálás parancsfájl kell írnia a lista [[CSEntryChange][csec]] a folyamat-objektumot. Ebben a gyűjteményben, amelyek az egyes objektumokra importált képviselik CSEntryChange attribútumok tevődik össze. Egy teljes importálás futtatása során ebben a gyűjteményben a skype_for_businesshoz CSEntryChange olyan objektumok, amelyek minden objektumhoz tartozó minden attribútumokkal kell rendelkeznie. Alatt, a Delta importálja a CSEntryChange objektum vagy tartalmaznia kell a attribútum szintű delta az egyes objektumokra importálni, és a teljes megadott az objektumokat, amelyek módosultak (csere mód).

**Záró importálása**  
Az importálás futtatása befejezésekor a befejezési importálása parancsfájl fut. Ez a parancsfájl bármely karbantartási műveletek szükséges (például Bezárás kapcsolatok rendszerek) és a válasz a hibák kell végezni.

A befejezési importálása parancsfájl a következő paraméterek kap az összekötő:

név | Adattípus | Leírás
--- | --- | ---
ConfigParameters | [A KeyedCollection gyűjteményben][keyk][karakterlánc, [ConfigParameter][cp]] | Az összekötő konfigurációs paramétereinek táblázatot.
Hitelesítő adatok | [PSCredential][pscred] | A kapcsolat lapon a rendszergazda által megadott hitelesítő adatokat tartalmazza.
OpenImportConnectionRunStep | [OpenImportConnectionRunStep][oicrs] | A parancsprogram tájékoztatja arról az importálás futtatása (delta vagy teljes) partíciót, a hierarchia, a vízjelet és a várt oldalméret típusú.
CloseImportConnectionRunStep | [CloseImportConnectionRunStep][cecrs] | A parancsprogram értesíti importálása befejeződött okát.

A parancsprogram vissza kell térnie a egyetlen [CloseImportConnectionResults] [ cicres] objektum a folyamat, például:`Write-Output (New-Object Microsoft.MetadirectoryServices.CloseImportConnectionResults)`

#### <a name="export"></a>Exportálás
Az összekötő importálása architektúráját azonos, exportálás támogató összekötők kell végrehajtania három parancsfájlokat.

**Exportálási lépések**  
A kezdő exportálási parancsfájl fut az Exportálás futtatása lépésnek elején. Ebben a lépésben során a forrás rendszer kapcsolatot létesíteni, és bármilyen előkészítő lépéseket használata előtt, adatok exportálása a csatlakoztatott rendszer.

A kezdő exportálási parancsfájlt a következő paraméterek kap az összekötő:

név | Adattípus | Leírás
--- | --- | ---
ConfigParameters | [A KeyedCollection gyűjteményben][keyk][karakterlánc, [ConfigParameter][cp]] | Az összekötő konfigurációs paramétereinek táblázatot.
Hitelesítő adatok | [PSCredential][pscred] | A kapcsolat lapon a rendszergazda által megadott hitelesítő adatokat tartalmazza.
OpenExportConnectionRunStep | [OpenExportConnectionRunStep][oecrs] | A parancsprogram tájékoztatja arról Exportálás futtatása (delta vagy teljes) partíciót, a hierarchia és a várt oldalméret típusú.
Típusai | [Séma][schema] | Az összekötő helyet az exportált sémája.

A parancsprogram nem kell minden kimeneti visszatérhet a folyamat.

**Adatok exportálása**  
A szinkronizálási szolgáltatás hívások az adatok exportálása parancsfájl, ahányszor szükséges összes függőben lévő exportálását feldolgozása. Ha az összekötő terület további függőben lévő exportálását, mint az összekötő oldalméret tartalmaz, az Exportálás parancsfájl előfordulhat, hogy hívható többször és esetleg többször ugyanazon objektumhoz.

Az Exportálás parancsfájl a következő paraméterek kap az összekötő:

név | Adattípus | Leírás
--- | --- | ---
ConfigParameters | [A KeyedCollection gyűjteményben][keyk][karakterlánc, [ConfigParameter][cp]] | Az összekötő konfigurációs paramétereinek táblázatot.
Hitelesítő adatok | [PSCredential][pscred] | A kapcsolat lapon a rendszergazda által megadott hitelesítő adatokat tartalmazza.
CSEntries | IList[CSEntryChange][csec] | Az összekötő terület objektumok listáját a függőben lévő exportnak feldolgoztatni a menet közben.
OpenExportConnectionRunStep | [OpenExportConnectionRunStep][oecrs] | A parancsprogram tájékoztatja arról Exportálás futtatása (delta vagy teljes) partíciót, a hierarchia és a várt oldalméret típusú.
Típusai | [Séma][schema] | Az összekötő helyet az exportált sémája.

Az Exportálás parancsfájl vissza kell térnie a [PutExportEntriesResults] [ peeres] a folyamat-objektumot. Az objektum nem szükséges eredménye az alábbi információkat tartalmazzák az egyes exportált összekötő kivéve, ha a hiba és horgony attribútum módosításának fordul elő. Ha például egy PutExportEntriesResults objektum visszatérni a folyamat:`Write-Output (New-Object Microsoft.MetadirectoryServices.PutExportEntriesResults)`

**Exportálás befejezése**  
Az Exportálás futtatása, a befejezési exportálása parancsfájl futtatásához befejezésekor. Ez a parancsfájl bármely karbantartási műveletek szükséges (például Bezárás kapcsolatok rendszerek) és a válasz a hibák kell végezni.

A befejezési exportálási parancsfájl a következő paraméterek kap az összekötő:

név | Adattípus | Leírás
--- | --- | ---
ConfigParameters | [A KeyedCollection gyűjteményben][keyk][karakterlánc, [ConfigParameter][cp]] | Az összekötő konfigurációs paramétereinek táblázatot.
Hitelesítő adatok | [PSCredential][pscred] | A kapcsolat lapon a rendszergazda által megadott hitelesítő adatokat tartalmazza.
OpenExportConnectionRunStep | [OpenExportConnectionRunStep][oecrs] | A parancsprogram tájékoztatja arról Exportálás futtatása (delta vagy teljes) partíciót, a hierarchia és a várt oldalméret típusú.
CloseExportConnectionRunStep | [CloseExportConnectionRunStep][cecrs] | A parancsprogram értesíti az oka, az exportálás befejeződött.

A parancsprogram nem kell minden kimeneti visszatérhet a folyamat.

#### <a name="password-synchronization"></a>A jelszó-szinkronizálás
A Windows PowerShell az összekötők jelszó módosítása vagy alaphelyzetbe állítása cél használható.

A jelszó parancsfájl a következő paraméterek kap az összekötő:

név | Adattípus | Leírás
--- | --- | ---
ConfigParameters | [A KeyedCollection gyűjteményben][keyk][karakterlánc, [ConfigParameter][cp]] | Az összekötő konfigurációs paramétereinek táblázatot.
Hitelesítő adatok | [PSCredential][pscred] | A kapcsolat lapon a rendszergazda által megadott hitelesítő adatokat tartalmazza.
Partition | [Partition][part] | A címtár partíciót, amely a CSEntry.
CSEntry | [CSEntry][cse] | Az objektumra, amely az összekötő hely elérési pontját a jelszó módosítása vagy alaphelyzetbe kapott.
OperationType | Karakterlánc | Azt jelzi, hogy a művelet a alaphelyzetbe állítása (**SetPassword**) vagy egy módosítása (**ChangePassword**).
PasswordOptions | [PasswordOptions][pwdopt] | Jelölők, adja meg a kívánt jelszót viselkedés alaphelyzetbe állítása. A paraméter csak akkor érhető el, ha OperationType **SetPassword**.
Régi_jelszó | Karakterlánc | Az objektum a régi jelszót a jelszó módosítások feltöltve. A paraméter csak akkor érhető el, ha OperationType **ChangePassword**.
ÚjJelszó | Karakterlánc | Az objektum új jelszót a parancsfájl kell beállítania feltöltve.

A jelszó parancsfájl nem várt eredményt visszatérhet a Windows PowerShell-folyamat. Ha hiba történik a jelszó parancsfájl, a parancsfájl throw kell egyet a szinkronizálási szolgáltatás tájékoztatni a problémát a következő kivételekkel:

- [PasswordPolicyViolationException] [ pwdex1] – Ha a jelszó nem felel meg a házirendet a csatlakoztatott rendszer elő.
- [PasswordIllFormedException] [ pwdex2] – elő, ha a jelszó nem fogadhatók el a csatlakoztatott rendszerhez.
- [PasswordExtension] [ pwdex3] – a jelszó parancsfájl egyéb hibáinak elő.

## <a name="sample-connectors"></a>Minta összekötők
A rendelkezésre álló minta összekötők teljes áttekintéséhez, lásd: a [Windows PowerShell összekötő minta összekötő gyűjtemény][samp].

## <a name="other-notes"></a>Más jegyzetek

### <a name="additional-configuration-for-impersonation"></a>Megszemélyesítési további konfigurációja
Adja meg a felhasználót, hogy a program a következő engedélyeket a szinkronizálási szolgáltatás kiszolgálón megszemélyesített:

A következő beállításkulcsokban olvasási hozzáférést:

- HKEY_USERS\\\Software\Microsoft\PowerShell [SynchronizationServiceServiceAccountSID]
- HKEY_USERS\\\Environment [SynchronizationServiceServiceAccountSID]

Határozza meg a biztonsági azonosító (biztonsági AZONOSÍTÓK) a szinkronizálási szolgáltatás szolgáltatásfiók, futtassa az alábbi PowerShell-parancsokat:

```
$account = New-Object System.Security.Principal.NTAccount "<domain>\<username>"
$account.Translate([System.Security.Principal.SecurityIdentifier]).Value
```

A következő rendszer fájlmappák olvasási hozzáférést:

- %ProgramFiles%\Microsoft forefront identitás Manager\2010\Synchronization Service\Extensions
- %ProgramFiles%\Microsoft forefront identitás Manager\2010\Synchronization Service\ExtensionsCache
- %ProgramFiles%\Microsoft forefront identitás Manager\2010\Synchronization Service\MaData\\{ConnectorName}

A Windows PowerShell-összekötő neve helyettesítése a {ConnectorName} helyőrzőt.

## <a name="troubleshooting"></a>Hibaelhárítás

-   Kapcsolatos hibák elhárítása az összekötő naplózás engedélyezése a további tudnivalókért lásd az [annak engedélyezése esemény-nyomkövetés nyomon követése az összekötőkhöz](http://go.microsoft.com/fwlink/?LinkId=335731).

<!--Reference style links - using these makes the source content way more readable than using inline links-->
[cpp]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.configparameterpage.aspx
[keyk]: https://msdn.microsoft.com/library/ms132438.aspx
[cp]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.configparameter.aspx
[pscred]: https://msdn.microsoft.com/library/system.management.automation.pscredential.aspx
[schema]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.schema.aspx
[schemaT]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.schematype.aspx
[schemaA]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.schemaattribute.aspx
[dnstyle]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.madistinguishednamestyle.aspx
[exportT]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.maexporttype.aspx
[DataNorm]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.manormalizations.aspx
[oconf]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.maobjectconfirmation.aspx
[dw]: https://msdn.microsoft.com/library/windows/desktop/aa378184.aspx
[part]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.partition.aspx
[hn]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.hierarchynode.aspx
[oicrs]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.openimportconnectionrunstep.aspx
[cecrs]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.closeexportconnectionrunstep.aspx
[oicres]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.openimportconnectionresults.aspx
[cecrs]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.closeexportconnectionrunstep.aspx
[cicres]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.closeimportconnectionresults.aspx
[oecrs]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.openexportconnectionrunstep.aspx
[irs]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.importrunstep.aspx
[cse]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.csentry.aspx
[csec]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.csentrychange.aspx
[peeres]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.putexportentriesresults.aspx
[pwdopt]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.passwordoptions.aspx
[pwdex1]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.passwordpolicyviolationexception.aspx
[pwdex2]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.passwordillformedexception.aspx
[pwdex3]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.passwordextensionexception.aspx
[samp]: http://go.microsoft.com/fwlink/?LinkId=394291
