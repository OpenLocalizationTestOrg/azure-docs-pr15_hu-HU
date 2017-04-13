<properties
    pageTitle="Az Azure AD Connect szinkronizálás jelszó-szinkronizálás végrehajtási |} Microsoft Azure"
    description="Hogyan működik a jelszó-szinkronizálás és ahhoz, hogy miként információt tartalmaz."
    services="active-directory"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>
<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/24/2016"
    ms.author="markusvi;andkjell"/>


# <a name="implementing-password-synchronization-with-azure-ad-connect-sync"></a>Az Azure AD Connect szinkronizálás jelszó-szinkronizálás végrehajtása
Ez a témakör a adatokat kell szinkronizálni a felhasználói jelszavak a helyszíni Active Directoryból (AD) egy felhőalapú Azure Active Directory (Azure Active Directory).

## <a name="what-is-password-synchronization"></a>Mi az, hogy a jelszó-szinkronizálás
Annak valószínűsége, hogy a munka elvégzéséhez elfelejtett jelszó miatt a blokkolt kapcsolódó különböző jelszót, akkor ne feledje, hogy a számát. Ne feledje, hogy minél nagyobb valószínűséggel elfelejt egy kell további jelszavakat. Kérdések és a jelszó alaphelyzetbe állítása és más jelszóval kapcsolatos problémák hívások igény a legtöbb segélyszolgálat erőforrást.

A jelszó-szinkronizálás lehetővé teszi az szinkronizál a felhasználói jelszavak a helyszíni Active Directoryból egy felhőalapú Azure Active Directory (Azure Active Directory).
Ez a szolgáltatás lehetővé teszi, hogy jelentkezzen be az Azure Active Directory-szolgáltatások (például az Office 365-ben, a Microsoft Intune, CRM Online és Azure Active Directory tartományi szolgáltatásokból) segítségével jelentkezzen be a helyszíni Active Directory ugyanazt a jelszót használja.

![Mi az Azure AD Connect](./media/active-directory-aadconnectsync-implement-password-synchronization/arch1.png)

A felhasználók kell csak egy karbantartása jelszavak számát csökkentésével jelszó-szinkronizálás nyújt segítséget:

- A felhasználót a hatékonyság növelése
- Az IT-részleghez költségek csökkentése  

Is [**az Active Directory összevonási szolgáltatások összevonási**](https://channel9.msdn.com/Series/Azure-Active-Directory-Videos-Demos/Configuring-AD-FS-for-user-sign-in-with-Azure-AD-Connect)választásakor tetszés szerint engedélyezheti a jelszó-szinkronizálás másolatként abban az esetben, ha az AD FS-infrastruktúrát sikertelen lesz.

A jelszó-szinkronizálás a szerepelni fog az Azure AD Connect szinkronizálás címtár-szinkronizálási szolgáltatás bővítménye. A jelszó-szinkronizálás használata a környezetben, kell:

- Azure Active Directory telepítése kapcsolódni  
- Konfigurálja a címtár-szinkronizálás között a helyszíni Active Directory és az Azure Active Directory
- Jelszó-szinkronizálás engedélyezése

További részletekért olvassa el [a helyszíni identitás és Azure Active Directory integrálása](active-directory-aadconnect.md)

> [AZURE.NOTE] Többet szeretne tudni az Active Directory tartományi szolgáltatások, amelyek FIPS és a jelszó-szinkronizálás konfigurálva vannak olvassa el a [Jelszó-szinkronizálás és FIPS](#password-synchronization-and-fips)című témakört.

## <a name="how-password-synchronization-works"></a>Hogyan működik a jelszó-szinkronizálás
Az Active Directory tartományi szolgáltatások megjeleníthető a tényleges felhasználó jelszavát kivonat érték ábrázolása a jelszavakat tárolja. Egy ujjlenyomat értéke (a "*algoritmus kivonatolás*") egyirányú matematikai függvény eredménye. Nincs semmilyen módszerrel nem vissza egy jelszót egyszerű szöveges verziója egyirányú függvény eredménye. Jelentkezzen be a helyszíni hálózaton jelszó kivonat nem használható.

A jelszó szinkronizálásához az Azure AD Connect szinkronizálási a jelszó kivonat olvas a helyszíni Active Directory. További biztonsági feldolgozása előtt, a rendszer szinkronizálja az Azure Active Directory authentication szolgáltatás érvényes a jelszó kivonat. A felhasználónkénti alapon és a válaszokat időrendbe jelszavak szinkronizálódnak.

A jelszó-szinkronizálás a tényleges adatok folyamat hasonlít a szinkronizálás a felhasználói adatok, például DisplayName vagy E-mail címét. Azonban jelszavak gyakrabban a szabványos címtár szinkronizálás ablakban más attribútumok vannak szinkronizálva. A jelszó-szinkronizálás 2 percenként fut. Ez a folyamat gyakoriságának nem módosíthatók. Jelszó szinkronizálásakor felülírja a meglévő felhő jelszót.

Az első alkalommal engedélyezheti a jelszó-szinkronizálás funkció, a felhasználók a hatókör jelszavak kezdeti szinkronizálásával végrehajtott. Felhasználói jelszavak szinkronizálni kívánt csak egy részhalmazát explicit módon nem definiálhatók.

Ha módosítja egy helyszíni jelszót, a frissített jelszót szinkronizálja, leggyakrabban a percek.
A jelszó-szinkronizálás funkció automatikusan próbálkozások sikertelen szinkronizálási kísérlet. Ha hiba történik a jelszó szinkronizálni kísérlet során, hiba történt be van jelentkezve az Eseménynapló nevet.

A szinkronizálás jelszó nincs hatással van a bejelentkezett felhasználó.
Az aktuális felhőalapú szolgáltatás munkamenet azonnal nem érinti a szinkronizált jelszó módosítása, amely akkor következik be, miközben be van jelentkezve egy felhőalapú szolgáltatásba. Jó helyen jár Ha a felhőalapú szolgáltatást igényel hitelesítő újra, meg kell adnia az új jelszót.

> [AZURE.NOTE] Jelszó-szinkronizálás csak az objektum típusa felhasználó az Active Directory támogatott. Nem támogatott, az iNetOrgPerson objektumtípus.

### <a name="how-password-synchronization-works-with-azure-ad-domain-services"></a>Hogyan működik a jelszó-szinkronizálás az Azure Active Directory tartományi szolgáltatások
A jelszó-szinkronizálás funkció használatával a helyszíni jelszavak az [Azure Active Directory tartományi szolgáltatások](../active-directory-domain-services/active-directory-ds-overview.md)szinkronizálása. Ebben az esetben lehetővé teszi, hogy a felhasználók a felhőben hitelesíteni a helyszíni érhető el minden módszer Azure Active Directory tartományi szolgáltatások AD. Ebben az esetben, a felület hasonlít egy helyszíni környezetben használja az Active Directory eszköz (Áttelepítési).

### <a name="security-considerations"></a>Biztonsági megfontolások
A jelszavak szinkronizálásakor a jelszót egyszerű szöveges verziója nem érhető el az Azure Active Directory, a jelszó szinkronizálási szolgáltatás vagy a kapcsolódó szolgáltatások közül.

Nincs is, a jelszó mentéséhez éppen formátumban a helyszíni Active Directory nem szükséges. Az Active Directory-jelszó kivonat címzett szolgál a Microsoft között a helyszíni Active Directory és Azure Active Directory. A jelszó kivonat a címzett nem használhatók a helyszíni környezetben erőforrások eléréséhez.

### <a name="password-policy-considerations"></a>Jelszó házirend kapcsolatos szempontok
Jelszó-szinkronizálás engedélyezése által érintett jelszóházirendek két típusa van:

1. Összetettsége jelszóházirend
2. Jelszólejárati házirendjének

**Összetettsége jelszóházirend**  
Jelszó-szinkronizálás engedélyezése, ha a helyszíni Active Directory összetettsége jelszóházirendek bírálja felül az összetettsége házirendek szinkronizált felhasználók a felhőben. A helyszíni Active Directory összes érvényes jelszavak használata Azure AD-szolgáltatások eléréséhez.

> [AZURE.NOTE] Közvetlenül az a felhő létrehozott felhasználó jelszavának továbbra is fizetnie jelszóházirendek meg kell a felhőben.

**Jelszólejárati házirendjének**  
Ha egy felhasználó a hatókör jelszó-szinkronizálás, a felhőben fiók jelszava értéke ", hogy*Sohase járjon le*".
Jelentkezzen be a felhőbeli szolgáltatások, amelyek a helyszíni környezetben lejárt szinkronizált jelszóval is. A felhő jelszavát frissül a következő alkalommal, amikor a helyszíni környezetben a jelszó módosítása.

### <a name="overwriting-synchronized-passwords"></a>Jelszavak felülírása szinkronizálása
A rendszergazda manuálisan új jelszót adhat meg a Windows PowerShell használatával.

Ebben az esetben az új jelszót felülírja a szinkronizált jelszavát, és az új jelszót alkalmazott összes jelszóházirendek definiált a felhőben.

Ha például módosítja a helyszíni jelszót újra, az új jelszót szinkronizálja a felhőbe, és felülírja a manuális frissített jelszót.

## <a name="enabling-password-synchronization"></a>A jelszó-szinkronizálás engedélyezése
Jelszó-szinkronizálás automatikusan engedélyezett, ha telepíti az Azure AD Connect **Express beállításai**. Lásd: [Ismerkedés az Azure AD Connect express beállításokkal](./connect/active-directory-aadconnect-get-started-express.md).

Ha Azure AD Connect telepítésekor egyéni beállításokat használ, lehetősége van engedélyezni a jelszó-szinkronizálás a felhasználó bejelentkezési lapon. További részletekért olvassa el [az Azure AD Connect egyéni telepítés](./connect/active-directory-aadconnect-get-started-custom.md).

![A jelszó-szinkronizálás engedélyezése](./media/active-directory-aadconnectsync-implement-password-synchronization/usersignin.png)

### <a name="password-synchronization-and-fips"></a>A jelszó-szinkronizálás és FIPS
Ha a kiszolgáló zárolva van lefelé szerint Federal információk Processing Standard (FIPS), majd MD5 le van tiltva.

**A jelszó-szinkronizálás MD5 engedélyezéséhez hajtsa végre az alábbi lépéseket:**

1. Nyissa meg az **Active Directory %programfiles%\Azure Sync\Bin**.
2. Nyissa meg a **miiserver.exe.config**.
3. Nyissa meg a **konfigurációs/futtatókörnyezet** csomópont (végén található a fájl).
4. Adja hozzá a következő csomópont:`<enforceFIPSPolicy enabled="false"/>`
5. A módosítások mentéséhez.

Hivatkozások a metszet, hogyan példához hasonló:

```
    <configuration>
        <runtime>
            <enforceFIPSPolicy enabled="false"/>
        </runtime>
    </configuration>
```

Biztonság és FIPS információt talál [AAD jelszó-szinkronizálás, titkosítást és FIPS megfelelőség](https://blogs.technet.microsoft.com/enterprisemobility/2014/06/28/aad-password-sync-encryption-and-fips-compliance/)

## <a name="troubleshooting-password-synchronization"></a>A jelszó-szinkronizálás – hibaelhárítás
Jelszavak nem Szinkronizáló a várt módon működik, ha azt lehet a felhasználók egy részét vagy az összes felhasználó számára.

- Ha egyes objektumok problémát, majd [fellépő, amely nem szinkronizálódik jelszavak egy objektum](#troubleshoot-one-object-that-is-not-synchronizing-passwords)látható.
- Ha problémát hol vannak szinkronizálva a jelszavak nélkül, olvassa el a [Hol vannak szinkronizálva a jelszavak nélkül fellépő problémák](#troubleshoot-issues-where-no-passwords-are-synchronized)című témakört.

### <a name="troubleshoot-one-object-that-is-not-synchronizing-passwords"></a>Egy olyan objektumra, amely nem szinkronizálódik jelszavak – problémamegoldás
Egyszerűen elháríthatja a jelszó-szinkronizálási problémák objektum állapotának megtekintésével.

Indítsa el az **Active Directory-felhasználók és a számítógépen**. Keresse meg a felhasználót, és ellenőrizze, hogy a **felhasználónak meg kell változtatnia jelszavát a következő bejelentkezéskor** nincs bejelölve.
![Az Active Directory hatékonyabban jelszavak](./media/active-directory-aadconnectsync-implement-password-synchronization/adprodpassword.png)  
Ha be van jelölve, kérje meg a felhasználónak bejelentkezés, és a jelszó módosítása. Az Azure Active Directory nem szinkronizálódnak az ideiglenes jelszót.

Ha az eredményt helyes-e az Active Directory, majd a következő lépés, ha követi a felhasználó a sync Engine. Követve a felhasználó a helyszíni Active Directoryból az Azure Active Directory, megtekintheti, ha egy leíró hiba lép az objektumon.

1. Indítsa el a **[Szinkronizálási szolgáltatáskezelő](active-directory-aadconnectsync-service-manager-ui.md)**.
2. Kattintson az **összekötők**.
3. Jelölje ki a felhasználó helyezkedik el az **Active Directory-összekötő** .
4. Jelölje ki a **keresési összekötő területet**.
5. Keresse meg azt a felhasználót, keres.
6. Jelölje ki a **leszármazással** fülre, és győződjön meg arról, hogy legalább egy szinkronizálási szabály jeleníti meg a **Jelszó-szinkronizálás** **true**. Az alapértelmezett beállítás a szinkronizálási szabály neve **az Active Directory - felhasználó AccountEnabled a**.  
    ![Tudnivalók a felhasználó felsorolása](./media/active-directory-aadconnectsync-implement-password-synchronization/cspasswordsync.png)  
7. Ezután [kövesse a felhasználó](active-directory-aadconnectsync-service-manager-ui-connectors.md#follow-an-object-and-its-data-through-the-system) a metaverse az Azure Active Directory-összekötő sorköz keresztül. Az összekötő terület objektum van, hogy a **Jelszó-szinkronizálás** értéke **Igaz**kimenő szabály. Az alapértelmezett beállítás a szinkronizálási szabály neve **kifelé a AAD - felhasználók csatlakozásra**.  
    ![Felhasználó tulajdonságain összekötő terület](./media/active-directory-aadconnectsync-implement-password-synchronization/cspasswordsync2.png)  
8. A múlt héten az objektum jelszó szinkronizálási részleteinek megtekintéséhez kattintson a **napló …**gombra.  
    ![Naplófájl részletes adatai](./media/active-directory-aadconnectsync-implement-password-synchronization/csobjectlog.png)  

Az Állapot oszlopban a következő értékeket lehet:

Állapot | Leírás
---- | -----
A siker | Jelszó szinkronizálása sikertelen volt.
FilteredByTarget | Jelszó beállítása **kell változtatni a következő bejelentkezéskor jelszót**. Nem lett szinkronizálva a jelszót.
NoTargetConnection | Nincs objektum a metaverse vagy az Azure Active Directory-összekötő helyre.
SourceConnectorNotPresent | Nincs objektum a helyszíni Active Directory-összekötő szóköz található.
TargetNotExportedToDirectory | Az objektum az Azure Active Directory-összekötő helyen még nem lett exportálja.
MigratedCheckDetailsForMoreInfo | Naplóbejegyzés előtt build 1.0.9125.0 jött létre, és régebbi állapotában látható.

### <a name="troubleshoot-issues-where-no-passwords-are-synchronized"></a>Ha nincs jelszavak vannak szinkronizálva kapcsolatos problémák megoldása
Kezdje azzal, hogy a parancsprogram futtatása [állapotuk jelszó-szinkronizálás beállításai](#get-the-status-of-password-sync-settings)szakaszában. Azt áttekintést nyújt a jelszó-szinkronizálás konfiguráció.  
![Jelszó-szinkronizálás beállításai PowerShell parancsprogramot kimenete](./media/active-directory-aadconnectsync-implement-password-synchronization/psverifyconfig.png)  
Ha a szolgáltatás nincs engedélyezve a az Azure Active Directory vagy a szinkronizálási csatorna állapota nincs engedélyezve, majd futtassa a csatlakozás telepítés varázslót. Jelölje ki a **Testreszabás szinkronizálási beállítások** , majd a jelszó-szinkronizálás kijelölésének megszüntetése. Ez a változás ideiglenesen letiltja a szolgáltatást. Ezután a varázslót, és újra a jelszó-szinkronizálás engedélyezése. Futtassa a ismét ellenőrizni, hogy helyes-e a konfigurációs.

Ha a parancsfájl azt mutatja, hogy nem nem szívveréséhez, majd futtassa a parancsfájl [hatására az összes jelszavak teljes szinkronizálást](#trigger-a-full-sync-of-all-passwords). Ez a parancsfájl más esetben, ha a konfiguráció helyes, de nem szinkronizálódnak a jelszavak is használható.

#### <a name="get-the-status-of-password-sync-settings"></a>Jelszó-szinkronizálás beállításai állapotuk

```
Import-Module ADSync
$connectors = Get-ADSyncConnector
$aadConnectors = $connectors | Where-Object {$_.SubType -eq "Windows Azure Active Directory (Microsoft)"}
$adConnectors = $connectors | Where-Object {$_.ConnectorTypeName -eq "AD"}
if ($aadConnectors -ne $null -and $adConnectors -ne $null)
{
    if ($aadConnectors.Count -eq 1)
    {
        $features = Get-ADSyncAADCompanyFeature -ConnectorName $aadConnectors[0].Name
        Write-Host
        Write-Host "Password sync feature enabled in your Azure AD directory: "  $features.PasswordHashSync
        foreach ($adConnector in $adConnectors)
        {
            Write-Host
            Write-Host "Password sync channel status BEGIN ------------------------------------------------------- "
            Write-Host
            Get-ADSyncAADPasswordSyncConfiguration -SourceConnector $adConnector.Name
            Write-Host
            $pingEvents =
                Get-EventLog -LogName "Application" -Source "Directory Synchronization" -InstanceId 654  -After (Get-Date).AddHours(-3) |
                    Where-Object { $_.Message.ToUpperInvariant().Contains($adConnector.Identifier.ToString("D").ToUpperInvariant()) } |
                    Sort-Object { $_.Time } -Descending
            if ($pingEvents -ne $null)
            {
                Write-Host "Latest heart beat event (within last 3 hours). Time " $pingEvents[0].TimeWritten
            }
            else
            {
                Write-Warning "No ping event found within last 3 hours."
            }
            Write-Host
            Write-Host "Password sync channel status END ------------------------------------------------------- "
            Write-Host
        }
    }
    else
    {
        Write-Warning "More than one Azure AD Connectors found. Please update the script to use the appropriate Connector."
    }
}
Write-Host
if ($aadConnectors -eq $null)
{
    Write-Warning "No Azure AD Connector was found."
}
if ($adConnectors -eq $null)
{
    Write-Warning "No AD DS Connector was found."
}
Write-Host
```

#### <a name="trigger-a-full-sync-of-all-passwords"></a>Az összes jelszavak teljes szinkronizálást elindítása
A teljes szinkronizálást az összes jelszavak az alábbi parancsfájl használatával válthat:

```
$adConnector = "<CASE SENSITIVE AD CONNECTOR NAME>"
$aadConnector = "<CASE SENSITIVE AAD CONNECTOR NAME>"
Import-Module adsync
$c = Get-ADSyncConnector -Name $adConnector
$p = New-Object Microsoft.IdentityManagement.PowerShell.ObjectModel.ConfigurationParameter "Microsoft.Synchronize.ForceFullPasswordSync", String, ConnectorGlobal, $null, $null, $null
$p.Value = 1
$c.GlobalParameters.Remove($p.Name)
$c.GlobalParameters.Add($p)
$c = Add-ADSyncConnector -Connector $c
Set-ADSyncAADPasswordSyncConfiguration -SourceConnector $adConnector -TargetConnector $aadConnector -Enable $false
Set-ADSyncAADPasswordSyncConfiguration -SourceConnector $adConnector -TargetConnector $aadConnector -Enable $true
```

## <a name="next-steps"></a>Következő lépések

* [Azure Active Directory csatlakozás szinkronizálása: A szinkronizálás beállításainak testreszabása](active-directory-aadconnectsync-whatis.md)
* [A helyszíni identitások integrálása az Azure Active Directory](active-directory-aadconnect.md)
