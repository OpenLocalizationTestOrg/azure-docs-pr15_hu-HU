<properties
   pageTitle="Csatlakozás SQL-adatbázis vagy adatraktár SQL Azure Active Directory-hitelesítés használatával |} Microsoft Azure"
   description="Megtudhatja, hogy miként SQL-adatbázis csatlakoztatása az Azure Active Directory-hitelesítés használatával."
   services="sql-database"
   documentationCenter=""
   authors="BYHAM"
   manager="jhubbard"
   editor=""
   tags=""/>

<tags
   ms.service="sql-database"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="data-management"
   ms.date="09/16/2016"
   ms.author="rick.byham@microsoft.com"/>

# <a name="connecting-to-sql-database-or-sql-data-warehouse-by-using-azure-active-directory-authentication"></a>Csatlakozás SQL-adatbázis vagy adatraktár SQL Azure Active Directory-hitelesítés használatával

Azure Active Directory authentication egy mechanizmusa történő csatlakozás Microsoft Azure SQL-adatbázis és [SQL adatraktár](../sql-data-warehouse/sql-data-warehouse-overview-what-is.md) identitások az Azure Active Directory (Azure Active Directory) használatával. Azure Active Directory hitelesítéssel központilag kezelheti az adatbázis-felhasználói identitások és az egyéb Microsoft-szolgáltatásokhoz egy központi helyen. Központi azonosító kezelése az adatbázis-felhasználók kezelése: egyetlen helyet biztosít, és leegyszerűsíti a engedélykezelés. Többek között az alábbi előnyöket nyújtja:

- Az SQL Server-hitelesítés alternatív biztosít.
- Segít adatbázis kiszolgálóin állítsa le a száma korlátok, felhasználói identitások között.
- Lehetővé teszi, hogy a jelszó Forgatás egy helyen
- Ügyfelek külső (AAD) csoportokkal adatbázis-engedélyek is kezelheti.
- Tárolását jelszavak, mivel az integrált Windows-hitelesítés és más Azure Active Directory által támogatott hitelesítéstípusok kizárhatja azt.
- Azure Active Directory authentication felhasználási tárolt adatbázis-felhasználói identitások adatbázis szintre hitelesítést végezni.
- Azure Active Directory jogkivonat-alapú hitelesítés SQL-adatbázis csatlakoztatása alkalmazásokat támogatja.
- Azure Active Directory authentication (tartomány-összevonás) ADFS vagy natív jelszó-hitelesítést támogat a helyi Azure Active Directory tartományi szinkronizálás nélkül.  
- Azure Active Directory támogatja a kapcsolat az SQL Server Management Studio használja az Active Directory egyetemes hitelesítése, amelyek tartalmazzák még a többtényezős hitelesítés (MFA).  MFA tartalmaz egyszerű ellenőrzési beállítások tartományba erős hitelesítés – telefonhívás, a szöveges üzenetet, az intelligens kártya PIN-kód vagy mobilalkalmazásban értesítés. További tudnivalókért olvassa el a [SSMS támogatja az Azure Active Directory többtényezős SQL-adatbázis és SQL adatraktár](sql-database-ssms-mfa-authentication.md)című témakört.

Beállítási lépések az alábbi eljárások beállítása és használata az Azure Active Directory authentication tartalmazza.

1. Hozzon létre és Azure Active Directory feltöltéséhez.
2. Ellenőrizze az adatbázis az Azure SQL-adatbázis V12. (Nem szükséges SQL adatraktár.)
3. Nem kötelező: Társítani, vagy módosíthatja az active directory jelenleg társított az Azure-előfizetés.
4. Azure Active Directory-rendszergazda létrehozása az Azure SQL Server vagy [Azure SQL-adatraktár](https://azure.microsoft.com/services/sql-data-warehouse/).
5. Állítsa be az ügyfélszámítógépek.
6. A tárolókban található adatbázis-felhasználók létrehozása az Azure Active Directory azonosítási megfeleltetve adatbázis.
7. Az adatbázis Azure AD-identitást használatával csatlakozni.


## <a name="trust-architecture"></a>A megbízható architektúra

Az alábbi magas szintű diagram összefoglalja a megoldási az Azure SQL-adatbázissal az Azure Active Directory-hitelesítés használatával. Az azonos fogalmak SQL adatraktár vonatkoznak. A támogatási Azure Active Directory natív felhasználó jelszavát, csak a felhőben része és Azure Active Directory és Azure SQL-adatbázis számít. Külső hitelesítés (vagy felhasználók és jelszóval a Windows hitelesítő adatok) ADFS blokk kommunikációt szükség. A nyilak jelzik kapcsolati útjainak.

![aad auth diagram][1]

A következő diagramon azt jelzi, az összevonási adatvédelmi és üzemeltetési kapcsolatok, hogy engedélyezte csatlakozhat adatbázis jogkivonat elküldése az ügyfélnek. A token az Azure Active Directory hitelesíti, és az adatbázisok megbízhatónak. Ügyfél 1 megfelelhet natív felhasználókkal Azure Active Directory vagy a szövetséges felhasználók Azure Active Directory. Ügyfél 2 jelöli megoldást, köztük az importált felhasználók; Ebben a példában a szövetséges Azure Active Directory és az ADFS-alapú az Azure Active Directory címtárral szinkronizált érkező. Fontos, hogy az access-adatbázishoz Azure AD-hitelesítés használatával kell lennie, hogy az Office-előfizetés kapcsolódik az Azure Active Directory ismertetése. Az azonos előfizetés kell használni az SQL Server-az Azure SQL-adatbázis vagy SQL adatraktár létrehozásához.

![előfizetés kapcsolat][2]

## <a name="administrator-structure"></a>Rendszergazdai struktúra

Ha Azure AD-hitelesítést használ, akkor két rendszergazdai fiókkal az SQL-adatbázis-kiszolgáló; az eredeti SQL Server-rendszergazda, és az Azure Active Directory-rendszergazda. Az azonos fogalmak SQL adatraktár vonatkoznak. Csak a rendszergazda Azure AD-fiók alapján hozhat létre az első szereplő Azure AD-adatbázis felhasználó felhasználói adatbázisban. Az Azure Active Directory-rendszergazdai bejelentkezés lehet az Azure Active Directory-felhasználó vagy csoport Azure AD. Ha a rendszergazda a csoportfiókok, csoporttag, engedélyezése az SQL Server-példány több Azure AD-rendszergazdák által használható. Rendszergazdaként csoport fiókkal hatékonyabbá tehető kezelhetőség azáltal, hogy a központi hozzáadása és eltávolítása a csoport tagjai az Azure Active Directory figyelmen kívül hagyásával a felhasználók és engedélyek SQL-adatbázisban. Csak egy Azure AD-rendszergazda (a felhasználó vagy csoport) bármikor beállíthatók.

![rendszergazdai struktúra][3]

## <a name="permissions"></a>Engedélyek

Új felhasználók létrehozásához rendelkeznie kell a `ALTER ANY USER` jogosultsági az adatbázisban. A `ALTER ANY USER` jogosultsági bármilyen adatbázis-felhasználói hozzárendelhető. A `ALTER ANY USER` engedéllyel a kiszolgáló rendszergazdai fiókkal, és az adatbázis-felhasználók is birtokában a `CONTROL ON DATABASE` vagy `ALTER ON DATABASE` engedéllyel arra az adatbázisra, és a tagok által a `db_owner` adatbázis szerepkör.

A tárolókban található adatbázis felhasználó létrehozása az Azure SQL-adatbázis vagy SQL adatraktár, csatlakoznia kell Azure AD-identitás az adatbázist. Az első tárolt adatbázis felhasználó létrehozása, csatlakoznia kell az adatbázis Azure Active Directory-rendszergazdaként (aki az adatbázis tulajdonosának) használatával. Ez az igazolni a 4-es és 5 az alábbi lépéseket. Minden Azure Active Directory-hitelesítés lehetőség csak akkor lehetséges, ha az Azure Active Directory-rendszergazdai készült Azure SQL-adatbázis vagy adatraktár SQL server. Ha az Azure Active Directory-rendszergazdai el lett távolítva a kiszolgálóról, meglévő készült Azure Active Directory-felhasználók korábban belül az SQL Server már nem csatlakozhat az adatbázis az Azure Active Directory hitelesítő adataival.

## <a name="azure-ad-features-and-limitations"></a>Azure Active Directory-szolgáltatások és érvényes korlátozások

A következő tagok az Azure Active Directory az Azure SQL Server- vagy SQL adatraktár kiépítése:

- Belső tagok: készült Azure Active Directory felügyelt a tartományban vagy egy ügyfél tartományban tag. További tudnivalókért lásd: [Azure ad a saját tartománynév hozzáadása](../active-directory/active-directory-add-domain.md).

- Összevont tartományhoz: készült Azure Active Directory és a szövetséges tartományban tag. További tudnivalókért olvassa el a [Microsoft Azure most támogatja a Windows Server Active Directory összevonási](https://azure.microsoft.com/blog/2012/11/28/windows-azure-now-supports-federation-with-windows-server-active-directory/)című témakört.

- Az importált tagokat más Azure Active könyvtárak natív vagy a szövetséges tartományban tagjait.

- Active Directory csoportok biztonsági csoportokat létrehozni.

Microsoft-fiókok (például outlook.com, hotmail.com, live.com) vagy más vendégfiókokat (például a gmail.com, a yahoo.com) nem támogatott. Ha nem lehet bejelentkezni [https://login.live.com](https://login.live.com) a fiókot, és a jelszót használja, majd esetén Microsoft-fiókkal, amelyet nem támogat a Azure SQL-adatbázis vagy az Azure SQL-adatraktár Azure AD-hitelesítést.

### <a name="additional-considerations"></a>További megfontolandó szempontok

- Tökéletesíthetik kezelhetőség, ajánlott, létrehozhatnak egy dedikált Azure Active Directory-csoportot rendszergazdaként.
- Csak egy Azure AD-rendszergazda (a felhasználó vagy csoport) Azure SQL Server vagy Azure SQL-adatraktár beállíthatók bármikor.
- Csak az SQL Server Azure Active Directory rendszergazdák kezdetben csatlakozhat az Azure SQL Server vagy az Azure SQL Azure Active Directory-fiókkal adatraktár. Az Active Directory-rendszergazda konfigurálhatja az Azure Active Directory későbbi adatbázis-felhasználók.
- Azt javasoljuk, hogy a kapcsolat időtúllépési 30 másodperc beállítást.
- SQL Server 2016 Management Studio és az SQL Server Data Tools for Visual Studio 2015 (2016 vagy újabb verzió 14.0.60311.1April) az Azure Active Directory-hitelesítés támogatására. (Azure Active Directory authentication; a **.NET keretrendszer adatszolgáltatója SQL Server**által támogatott legalább verzió .NET-keretrendszer 4.6). Ezért a legújabb verzióra ezen eszközök és az adatok szintű alkalmazások (DAC és .bacpac) az Azure Active Directory authentication használható.
- [ODBC 13.1](https://www.microsoft.com/download/details.aspx?id=53339) verzióval Azure Active Directory authentication azonban `bcp.exe` nem tud csatlakozni az Azure Active Directory-hitelesítés használatával, mert egy régebbi ODBC-szolgáltatóval használja.
- `sqlcmd`támogatja az Azure Active Directory authentication elején érhető el a [Letöltőközpontból](http://go.microsoft.com/fwlink/?LinkID=825643)13.1 verzióval.  
- Az SQL Server Data Tools for Visual Studio 2015 legalább az a (verzió 14.0.60311.1) Adateszközök 2016 áprilisi verziója szükséges. Azure Active Directory-felhasználók jelenleg nem jelennek a SSDT objektum Intézőben. Kerülő megtekintheti a felhasználók [sys.database_principals](https://msdn.microsoft.com/library/ms187328.aspx).
- [A Microsoft JDBC illesztőprogram 6.0 az SQL Server](https://www.microsoft.com/en-us/download/details.aspx?id=11774) Azure Active Directory authentication támogatja. Lásd még [állítsa a kapcsolat tulajdonságai parancsra](https://msdn.microsoft.com/library/ms378988.aspx).
- PolyBase nem tudja hitelesíteni Azure Active Directory-hitelesítés használatával.
- Egyes eszközök, például BI és az Excel nem támogatottak.
- Azure Active Directory authentication Azure portál **Importálása** és **Exportálása adatbázis** rögzítéséhez által támogatott SQL-adatbázis. Az importálás és exportálás Azure Active Directory-hitelesítés használatával is a PowerShell-parancs támogatott.


## <a name="1-create-and-populate-an-azure-ad"></a>1. a létrehozása és feltöltése az Azure Active Directory

Hozzon létre egy Azure Active Directory és a felhasználók és csoportok. Az Azure Active Directory lehet a kezdeti tartomány: Azure AD-felügyelt tartományt. Az Azure Active Directory-a helyszíni Active Directory tartományi szolgáltatások, amelyek identitásszolgáltatóval összevont az Azure Active Directoryval is lehet.

További tudnivalókért lásd: [integrálása az Azure Active Directory címtárral a helyszíni identitások](../active-directory/active-directory-aadconnect.md), [az Azure AD a saját tartománynév](../active-directory/active-directory-add-domain.md), [most már a Microsoft Azure támogatja a Windows Server Active Directory összevonási](https://azure.microsoft.com/blog/2012/11/28/windows-azure-now-supports-federation-with-windows-server-active-directory/), [az Azure Active directory felügyelete](https://msdn.microsoft.com/library/azure/hh967611.aspx)és [kezelése a Windows PowerShell használatá Azure AD](https://msdn.microsoft.com/library/azure/jj151815.aspx).

## <a name="2-ensure-your-sql-database-is-version-12"></a>2. legyen az SQL-adatbázis verzió 12

A legújabb SQL-adatbázis V12 az Azure Active Directory authentication támogatja. Információt az SQL-adatbázis V12 és megtudhatja, hogy elérhető régiójában olvassa el [az SQL-adatbázis frissítése legújabb V12 újdonságai](sql-database-v12-whats-new.md)című témakört. Ez a lépés nem áll Azure SQL-adatraktár szükséges, mivel SQL adatraktár az V12 csak érhető el.

Ha egy meglévő adatbázishoz, győződjön meg arról, hogy helyezkedik el az SQL-adatbázis V12 való csatlakozással az adatbázist (például SQL Server Management Studio) és végrehajtása `SELECT @@VERSION;`. A várt eredményt ad az SQL-adatbázis V12 az adatbázis legalább van **A Microsoft SQL Azure (RTM) - 12.0**. Ha az adatbázis nem helyezkedik el az SQL-adatbázis V12, olvassa el a [megtervezése és a Felkészülés a SQL-adatbázis V12 frissítése](sql-database-v12-plan-prepare-upgrade.md), majd keresse fel az Azure SQL-adatbázis V12 az adatbázis áttelepítése klasszikus portálon.

Azt is megteheti létrehozhat egy új adatbázist az SQL-adatbázis V12 [az első Azure SQL-adatbázis](sql-database-get-started.md)létrehozása lépéseket követve. **Tipp**: mielőtt az új adatbázis jelöljön ki egy előfizetést, olvassa el a következő lépéssel.

## <a name="3-optional-associate-or-change-the-active-directory-that-is-currently-associated-with-your-azure-subscription"></a>3. a nem kötelező: Társítása vagy az active directory jelenleg társított az Azure-előfizetés módosítása

Az adatbázis társítása az Azure Active Directory-címtár a szervezet számára, végezze el a címtár az Azure előfizetés az adatbázist tároló megbízható könyvtárában. További tudnivalókért olvassa el a [hogyan Azure előfizetések társítva Azure Active Directory](https://msdn.microsoft.com/library/azure/dn629581.aspx)című témakört.

**További információ:** Minden Azure előfizetés Azure AD-példányt tartson megbízhatósági kapcsolat tartalmaz. Ez azt jelenti, hogy megbízik-e a címtár hitelesítést végezni a felhasználók, szolgáltatások és eszközök. Több előfizetés megbízhat ugyanabban a könyvtárban, de előfizetés csak egy címtár megbízik. Láthatja, hogy melyik címtár megbízhatónak az előfizetés az [https://manage.windowsazure.com/](https://manage.windowsazure.com/)a **Beállítások** lapon. A meghatalmazás előfizetést tartalmazó könyvtár és a kapcsolatot, előfizetést tartalmazó az Azure (webhelyeket, adatbázisok és stb.), minden más forrásokból további alárendelt erőforrások az előfizetés például amelyek eltérően van. Ha az előfizetés lejár, majd az előfizetéshez tartozó e más források az access leállítja is. De a címtárban marad Azure-ban, és egy másik előfizetés társítása könyvtárra, és továbbra is a címtár-felhasználók kezelése. Erőforrások kapcsolatos további tudnivalókért olvassa el a [ismertetése erőforrás access Azure-ban](https://msdn.microsoft.com/library/azure/dn584083.aspx)című témakört.

Az alábbi eljárások az adott előfizetés társított Directoryban módosításával kapcsolatos lépésenkénti útmutatást.

1. Csatlakozzon az [Azure klasszikus Portal](https://manage.windowsazure.com/) Azure előfizetés rendszergazdaként.
2. A bal oldali transzparens válassza a **Beállítások**lehetőséget.
3. Az előfizetések jelennek meg a beállítások képernyőn. Nem jelenik meg a kívánt előfizetést, kattintson az **előfizetések elemre** a képernyő tetején legördülő lista **Szűrő által könyvtár** mezőbe, és válassza az előfizetések tartalmazó könyvtár, és kattintson az **ALKALMAZ**gombra.

    ![Jelölje ki az előfizetés][4]
4. A **Beállítások** területen kattintson az előfizetés, és kattintson a **SZERKESZTÉS CÍMTÁR** az oldal alján.

    ![Active Directory-beállítások-portál][5]
5. A **CÍMTÁR szerkesztése** párbeszédpanelen jelölje be az Azure Active Directory az SQL Server- vagy SQL adatraktár társított, és kattintson a nyílra a Tovább gombra.

    ![Szerkesztés-címtár-kiválasztása][6]
6. **Megerősítés** könyvtár hozzárendelése párbeszédpanelen győződjön meg arról, hogy "**minden további rendszergazdák törlődnek.**"

    ![Szerkesztés a címtár-megerősítése][7]
7. Kattintson a frissítse a portálon.

> [AZURE.NOTE] Amikor módosítja a címtárban, további rendszergazdák, Azure Active Directory-felhasználók és csoportok, az access és a címtár-biztonsági erőforrás felhasználók törlődik, és már nem férhet hozzá, az előfizetés vagy az erőforrások. Csak, szolgáltatás rendszergazdái is beállíthatja az access a rendszerbiztonsági az új könyvtár alapján. Ez a változás jelentős mértékű propagálása az összes erőforrás időt vehet igénybe. Is a könyvtár módosítása az Azure Active Directory-rendszergazda módosítja az SQL-adatbázis és SQL adatraktár és letiltása az access adatbázis bármelyik meglévő Azure Active Directory-felhasználók. A Azure AD-rendszergazdának kell lennie, visszaállítása (az alábbiaknak) és az új Azure Active Directory-felhasználók létre kell hozni.

## <a name="4-create-an-azure-ad-administrator-for-azure-sql-server"></a>4. a Azure SQL Server Azure AD-rendszergazda létrehozása

Minden egyes Azure SQL Server (ami egy SQL-adatbázis vagy SQL adatraktár tárolja) kezdődik, amely a teljes Azure SQL Server rendszergazdája egyetlen kiszolgáló rendszergazdai fiókkal. A második SQL Server rendszergazdája létre kell hozni, amely az Azure Active Directory-fiók. A fő a fő adatbázisban tárolt adatbázis felhasználóként jön létre. Rendszergazdák a kiszolgáló rendszergazdafiókjához **minden felhasználó adatbázis jelentkeztünk** tartoznak, és írja be a felhasználó adatbázisonként **dbo** felhasználóként. A kiszolgáló rendszergazdája fiókokkal kapcsolatos további tudnivalókért lásd: az [adatbázisok kezelése és Azure SQL-adatbázisban bejelentkezések](sql-database-manage-logins.md) és a **bejelentkezési adatok és a felhasználók** szakasz [Azure SQL-adatbázis biztonsági irányelveket](sql-database-security-guidelines.md)és korlátai.

Azure Active Directory Geo ismétlésekkel használatakor az Azure Active Directory-rendszergazdának kell beállítania, az elsődleges és másodlagos kiszolgálókhoz. Ha egy kiszolgáló nem rendelkezik az Azure Active Directory-rendszergazda, majd bejelentkezések Azure Active Directory és a felhasználók kap egy "nem tud csatlakozni" kiszolgálóhiba.

> [AZURE.NOTE] Azok a felhasználók nem alapuló (beleértve a Azure SQL Server Rendszergazdafiók) Azure AD-fiókkal, az Azure Active Directory-alapú felhasználók nem hozható létre, mert azok nincs engedélye az Azure Active Directory és a javasolt adatbázis felhasználóinak.

### <a name="provision-an-azure-active-directory-administrator-for-your-azure-sql-server-by-using-the-azure-portal"></a>Az Azure SQL Server Azure Active Directory rendszergazdája hozhatók létre az Azure portál használatával

1. Az [Azure portál](https://portal.azure.com/)jobb felső sarokban kattintson a kapcsolat a legördülő lista lehetséges aktív könyvtárak listáját. Válassza ki a helyes-e az Active Directory Azure Active Directory alapértelmezés szerint. Ezt a lépést az előfizetés társítását, az Active Directoryval, SQL Server Azure-gondoskodhat arról, hogy szolgál, hogy az azonos előfizetés mindkét hivatkozások Azure Active Directory és az SQL Server. (Az Azure SQL Server is kell-, Azure SQL-adatbázis vagy a Azure SQL-adatraktár.)

    ![ADFS-kiválasztása][8]
2. A bal oldali transzparens jelölje ki az **SQL-kiszolgálók**, jelölje ki azt az **SQL server**és a az **SQL Server** -lap tetején kattintson a **Beállítások**.

    ![Active Directory-beállítások][9]
3. Kattintson a **Beállítások** lap ** az Active Directory-rendszergazda
4. Az **Active Directory-rendszergazdai** lap kattintson a **rendszergazda az Active Directory**és a képernyő tetején, kattintson a **rendszergazda beállítása**.
5. A **rendszergazda hozzáadása** lap, keressen a felhasználó, jelölje be a felhasználó vagy csoport rendszergazdája lesz, és válassza a **Jelölje ki**. (Az Active Directory a rendszergazda lap jeleníti meg a tagok és a csoportok, az Active Directory. Felhasználók vagy csoportok jelennek meg szürkén, nem választható, mert nem támogatott Azure AD-rendszergazdaként. (Lásd: támogatott rendszergazdák az **Azure Active Directory-szolgáltatások és érvényes korlátozások** a fenti listája.) Szerepköralapú hozzáférés-szerepalapú csak az a portálon, és az SQL Server nem propagálja.
6. Az **Active Directory-rendszergazda** a lap tetején kattintson a **MENTÉS**gombra.
    ![Válassza a rendszergazda][10]

    A rendszergazda módosítása a folyamat eltarthat néhány percig, amíg. Kattintson az új rendszergazdai **Active Directory-rendszergazdai** mezőben látható.

> [AZURE.NOTE] Az Azure Active Directory-rendszergazdai beállításakor az új felügyeleti nevet (felhasználó vagy csoport) már nem lehet a virtuális fő adatbázist szerepel egy SQL Server-hitelesítés felhasználóként. Ha jelen van, az Azure Active Directory felügyeleti telepítés meghiúsul; létezik visszaállítása annak létrehozását, és már jelezve, hogy ilyen rendszergazda (név). Mivel az ilyen egy SQL Server-hitelesítés felhasználó nem része az Azure Active Directory, az Azure Active Directory-hitelesítés használatával kiszolgálóhoz való csatlakozáshoz bármely munkamennyiség sikertelen lesz.

Később eltávolítja a rendszergazda, az **Active Directory-rendszergazda** , a lap tetején kattintson a **rendszergazda eltávolítása**gombra, és kattintson a **Mentés**.

### <a name="provision-an-azure-ad-administrator-for-azure-sql-server-by-using-powershell"></a>Azure SQL Server Azure Active Directory rendszergazdája kiépítése a PowerShell használatával

PowerShell-parancsmagok futtatásához Azure PowerShell telepítése és futtatása van szükség. Információ megtudhatja, [hogy miként telepítheti, állíthatja Azure PowerShell](../powershell-install-configure.md).

Hozhatók létre az Azure Active Directory-rendszergazda, hajtsa végre a következő Azure PowerShell-parancsokkal:

- AzureRmAccount hozzáadása
- Jelölje ki-AzureRmSubscription


A parancsmagok kiépítése, és Azure AD-rendszergazda kezelésére használható:

| Parancsmag neve                                       | Leírás                                                                                                     |
|---------------------------------------------------|-----------------------------------------------------------------------------------------------------------------|
| [Set-AzureRmSqlServerActiveDirectoryAdministrator](https://msdn.microsoft.com/library/azure/mt603544.aspx)    | Azure Active Directory-rendszergazda kiépítése Azure SQL Server vagy Azure SQL-adatraktár. (Kell lennie az aktuális előfizetése.) |
| [Eltávolítás-AzureRmSqlServerActiveDirectoryAdministrator](https://msdn.microsoft.com/library/azure/mt619340.aspx) | Azure SQL Server vagy Azure SQL-adatraktár eltávolítja az Azure Active Directory-rendszergazdaként. |
| [Get-AzureRmSqlServerActiveDirectoryAdministrator](https://msdn.microsoft.com/library/azure/mt603737.aspx)    | Ad egy már konfigurálva vannak az Azure SQL Server vagy Azure SQL-adatraktár Azure Active Directory-rendszergazdának felvilágosítást. |

A PowerShell-parancsot – súgó használatával kapcsolatban további részletekre kíváncsi az egyes ezek a parancsok, például ``get-help Set-AzureRmSqlServerActiveDirectoryAdministrator``.

A következő parancsfájl rendelkezések az Azure Active Directory-rendszergazda csoport nevű **DBA_Group** (objektumazonosító `40b79501-b343-44ed-9ce7-da4c8cc7353f`) a **demo_server** kiszolgáló erőforrás csoport neve **csoport – 23**:

```
Set-AzureRmSqlServerActiveDirectoryAdministrator –ResourceGroupName "Group-23"
–ServerName "demo_server" -DisplayName "DBA_Group"
```

A **DisplayName** bemeneti paraméterre fogadja el az Azure Active Directory megjelenítendő név vagy a felhasználó egyszerű felhasználóneve. Ha például ``DisplayName="John Smith"`` és ``DisplayName="johns@contoso.com"``. Az Azure Active Directory-csoportok csak az Azure Active Directory támogatja a megjelenítendő nevet.

> [AZURE.NOTE] Az Azure PowerShell-parancsot ```Set-AzureRmSqlServerActiveDirectoryAdministrator``` nem akadályozza meg kiépítési Azure AD-rendszergazdák-felhasználóknak nem támogatott. Egy nem támogatott felhasználói kiépítése, de nem tud kapcsolódni a adatbázis. (Lásd: támogatott rendszergazdák az **Azure Active Directory-szolgáltatások és érvényes korlátozások** a fenti listája.)

Az alábbi példában a választható **objektumazonosító**:

```
Set-AzureRmSqlServerActiveDirectoryAdministrator –ResourceGroupName "Group-23"
–ServerName "demo_server" -DisplayName "DBA_Group" -ObjectId "40b79501-b343-44ed-9ce7-da4c8cc7353f"
```

> [AZURE.NOTE] Az Azure Active Directory- **objektumazonosító** szükség, ha a **DisplayName** nem egyedi. **Objektumazonosító** és **DisplayName** értékeinek beolvasásához, használja a klasszikus portál Azure Active Directory című szakaszát, és megtekintheti a felhasználó vagy csoport tulajdonságait.

Az alábbi példa eredménye az aktuális információt az Azure SQL Server Azure AD-rendszergazda:

```
Get-AzureRmSqlServerActiveDirectoryAdministrator –ResourceGroupName "Group-23" –ServerName "demo_server" | Format-List
```

Az alábbi példa eltávolítja az Azure Active Directory-rendszergazda:
```
Remove-AzureRmSqlServerActiveDirectoryAdministrator -ResourceGroupName "Group-23" –ServerName "demo_server"
```

Azure Active Directory rendszergazda is a REST API-k használatával is kiépítése. További tudnivalókért lásd: a [szolgáltatás kezelése REST API-útmutató és SQL-adatbázisok műveletekhez Azure a Azure SQL-adatbázisait műveletek](https://msdn.microsoft.com/library/azure/dn505719.aspx)

## <a name="5-configure-your-client-computers"></a>5. az ügyfélszámítógépek beállítása

Az összes ügyfélszámítógépekre, amelyből az alkalmazások és a felhasználók csatlakozás Azure SQL-adatbázis vagy Azure SQL Azure Active Directory identitást használatával adatraktár telepíteni kell az alábbi szoftvereknek:

- .NET-keretrendszer 4.6-os vagy újabb verzió, a [https://msdn.microsoft.com/library/5a4x27ek.aspx](https://msdn.microsoft.com/library/5a4x27ek.aspx).
- Azure Active Directory Authentication Library az SQL Server (**ADALSQL. DLL**) több nyelven érhető el (x86 és amd64) a [Microsoft Active Directory Authentication Library a Microsoft SQL Server](http://www.microsoft.com/download/details.aspx?id=48742)a letöltőközpontból.

### <a name="tools"></a>Eszközök

- [SQL Server 2016 Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) vagy az [SQL Server Data Tools for Visual Studio 2015](https://msdn.microsoft.com/library/mt204009.aspx) megkövetelt a .NET-keretrendszer 4.6.
- **ADALSQL SSMS telepíti a x86 verziója. DLL**.
- SSDT **ADALSQL amd64 verzióját telepíti. DLL**.
- A [Visual Studio tölthető le](https://www.visualstudio.com/downloads/download-visual-studio-vs) a legújabb Visual Studio .NET-keretrendszer 4.6 megkövetelt a, de nem **ADALSQL szükséges amd64 verziójának telepítése. DLL**.

## <a name="6-create-contained-database-users-in-your-database-mapped-to-azure-ad-identities"></a>6. a tárolókban található adatbázis-felhasználók létrehozása az Azure Active Directory azonosítási megfeleltetve adatbázis

### <a name="about-contained-database-users"></a>A tárolókban található adatbázis felhasználókról

Azure Active Directory-hitelesítést igényel tárolt adatbázis felhasználóként létrejön az adatbázis-felhasználók. A tárolókban található adatbázis felhasználóhoz Azure Active Directory identitás alapján egy adatbázis-felhasználó, nincs telepítve a bejelentkezési a fő-adatbázisban, és amely a társított az adatbázis Azure AD-címtárban identitás megfelelteti. Az Azure Active Directory-identitás lehet egyéni felhasználói fiókot vagy csoportot. A tárolókban található adatbázis-felhasználók kapcsolatos további tudnivalókért témakörben [Szereplő adatbázis-felhasználói-létrehozása az adatbázis hordozható](https://msdn.microsoft.com/library/ff929188.aspx).

> [AZURE.NOTE] Adatbázis-felhasználók (kivételével rendszergazdák) portálon nem hozható létre. RBAC szerepkörök nem vonatkoznak az SQL Server, SQL-adatbázis vagy SQL adatraktár. Azure RBAC szerepkörök Azure erőforrások kezeléséhez használható parancsmagokról, és az adatbázis-engedélyek nem vonatkoznak. Ha például az **SQL Server munkatársi** szerepkörök nem hozzáférést az SQL-adatbázis vagy SQL adatraktár. A hozzáférési engedélyt kell adni közvetlenül a Transact-SQL-utasítások az adatbázist.

### <a name="connect-to-the-user-database-or-data-warehouse-by-using-sql-server-management-studio-or-sql-server-data-tools"></a>Csatlakozás SQL Server Management Studio vagy SQL Server Data Tools segítségével a felhasználók adatbázis vagy raktári

Erősítse meg az Azure Active Directory-rendszergazda megfelelően állított be, akkor a **fő** adatbázist rendszergazdai fiókkal az Azure Active Directory csatlakoztatása van.
Az Azure Active Directory-alapú tárolt adatbázis-felhasználó (kívül a kiszolgáló rendszergazdája, amely az adatbázis tulajdonosa) kiépítése, csatlakoznia kell az adatbázis identitással Azure Active Directory, amelynek hozzáférést az adatbázishoz.

> [AZURE.IMPORTANT] Azure Active Directory-hitelesítés támogatása az [SQL Server 2016 Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) és a Visual Studio 2015 [SQL Server Data Tools](https://msdn.microsoft.com/library/mt204009.aspx) érhető el. A SSMS augusztus 2016 verzióját is támogatja az Active Directory univerzális hitelesítése, amely lehetővé teszi a rendszergazdáknak a telefonhívás, és a szöveges üzenetben, intelligens kártya PIN-kód vagy mobilalkalmazásban értesítés többtényezős hitelesítést igényel.

#### <a name="connect-using-active-directory-integrated-authentication"></a>Csatlakozás Active Directory integrált hitelesítés használatával

Használja ezt a módszert, ha be van jelentkezve a Windows Azure Active Directory hitelesítő adatainak felhasználásával szövetséges tartományokból.

1. Indítsa el a Management Studio vagy Adateszközök, és **Csatlakozás a kiszolgálóhoz** (vagy a **Kapcsolódás adatbázis-vezérlő**) párbeszédpanelen a **hitelesítés** mezőben jelölje ki az **Active Directory beépített hitelesítése**. Nincs jelszó van szükség, és megadható, mert a meglévő hitelesítő adatok formában jelennek meg a kapcsolatot.
    ![Jelölje ki az integrált Active Directory-hitelesítés][11]

2. Kattintson a **Beállítások** gombra, és a **Kapcsolat tulajdonságai** lapon az **adatbázis csatlakoztatása** mezőbe írja be a nevét a felhasználói adatbázis, amelyhez csatlakozni szeretne.
    ![Jelölje ki az adatbázis neve][13]


#### <a name="connect-using-active-directory-password-authentication"></a>Csatlakozás Active Directory jelszó-hitelesítés használatával

Ezt a módszert, ha a tartomány használata az Azure Active Directory-Azure Active Directory egyszerű felhasználónév összekapcsolása felügyelt. Szintén felhasználhatja azt a tartományt, például amikor távolról dolgozik hozzáféréssel nem szövetséges fiók.

Használja ezt a módszert, ha be van jelentkezve a Windows hitelesítő adatok felhasználásával egy tartományt, amelyet a rendszer nem szövetséges szervezetekbe tartozó Azure, vagy ha az eredeti és az ügyfél tartomány használata az Azure Active Directory Azure AD-hitelesítés használatával alapján.

1. Indítsa el a Management Studio vagy Adateszközök, és **Csatlakozás a kiszolgálóhoz** (vagy a **Kapcsolódás adatbázis-vezérlő**) párbeszédpanelen a **hitelesítés** mezőben jelölje ki az **Active Directory jelszó-hitelesítést**.
2. A **felhasználónév** mezőbe írja be a formátumot az Azure Active Directory-felhasználónevét **username@domain.com**. Az Azure Active Directory-fiókot kell lennie, vagy egy tartományból fiók kapcsolt az Azure Active Directoryval.
3. A **jelszó** mezőbe írja be a felhasználó jelszavát az Azure Active Directory-fiók vagy a tartományi fiók összevont.
    ![Jelölje ki az Active Directory-jelszó-hitelesítést][12]

4. Kattintson a **Beállítások** gombra, és a **Kapcsolat tulajdonságai** lapon az **adatbázis csatlakoztatása** mezőbe írja be a nevét a felhasználói adatbázis, amelyhez csatlakozni szeretne. (Lásd az előző beállításra lévő kép).


### <a name="create-an-azure-ad-contained-database-user-in-a-user-database"></a>Egy tárolt Azure AD-adatbázis felhasználó létrehozása felhasználói adatbázisban

Az Azure Active Directory-alapú tárolt adatbázis felhasználó létrehozása (kívül a kiszolgáló rendszergazdája, amely az adatbázis tulajdonosa), csatlakoznia kell az adatbázis Azure Active Directory identitással rendelkező felhasználóként legalább a **Bármely felhasználó ALTER** engedéllyel. Ezután a következő szintaxissal Transact-SQL nyelvben:

    CREATE USER <Azure_AD_principal_name>
    FROM EXTERNAL PROVIDER;


*Azure_AD_principal_name* lehet Azure AD-felhasználó egyszerű felhasználónév vagy a megjelenítendő nevét az Azure Active Directory-csoportot.

**Példák:** A tárolókban található adatbázis létrehozása az Azure Active Directory, amely a felhasználó összevont, illetve kezeli a tartomány felhasználói:

    CREATE USER [bob@contoso.com] FROM EXTERNAL PROVIDER;
    CREATE USER [alice@fabrikam.onmicrosoft.com] FROM EXTERNAL PROVIDER;

A tárolókban található adatbázis-felhasználó, amely az Azure Active Directory vagy a szövetséges tartományban csoport létrehozásához adja meg a biztonsági csoport megjelenítendő neve:

    CREATE USER [ICU Nurses] FROM EXTERNAL PROVIDER;

Egy olyan alkalmazás, amely az Azure Active Directory token használatával csatlakozik, amely a tárolókban található adatbázis-felhasználó létrehozása:

    CREATE USER [appName] FROM EXTERNAL PROVIDER;

Azure Active Directory identitások alapján tárolt adatbázis-felhasználók létrehozásával kapcsolatos további tudnivalókért lásd: [Felhasználó létrehozása (Transact-SQL nyelvben)](http://msdn.microsoft.com/library/ms173463.aspx).


> [AZURE.NOTE] Azure SQL Server Azure Active Directory rendszergazdája eltávolítása megakadályozza, hogy bármelyik Azure Active Directory authentication felhasználó csatlakozás a kiszolgálóhoz. Ha szükséges, használhatatlanná Azure Active Directory-felhasználók manuálisan a SQL-adatbázis rendszergazdájának húzható.

Amikor létrehoz egy adatbázis-felhasználó, a felhasználó a **Csatlakozás** engedélyt kap, és a **nyilvános** szerepkör tagként arra az adatbázisra csatlakozhat. Az eredetileg a csak a felhasználó számára elérhető engedélyek: a **nyilvános** szerepkör bármely engedélyeket vagy bármely engedélyeket, hogy be tagja Windows csoportokat. Miután kiépítése az Azure Active Directory-alapú tárolt adatbázis-felhasználó, akkor a felhasználó további engedélyeket adhat, ugyanúgy, mint bármilyen más típusú felhasználói engedélyt. A szokásos adatbázis szerepkörök engedélyeket, és a felhasználók felvétele az szerepkörök. [Adatbázis motor jogosultsági alapjai](http://social.technet.microsoft.com/wiki/contents/articles/4433.database-engine-permission-basics.aspx)című cikkben további információt. Speciális SQL-adatbázis szerepkörök kapcsolatos további tudnivalókért olvassa el a [adatbázisok kezelése és Azure SQL-adatbázisban bejelentkezések](sql-database-manage-logins.md)című témakört.
A szövetséges tartományban felhasználó, a manage domain importálja a felügyelt tartomány identitás kell használnia.

> [AZURE.NOTE] Azure Active Directory-felhasználók jelöli az adatbázis-metaadatok típusú E (EXTERNAL_USER) és -csoportok típus X (EXTERNAL_GROUPS). További tudnivalókért olvassa el a [sys.database_principals](https://msdn.microsoft.com/library/ms187328.aspx)című témakört.


## <a name="7-connect-by-using-azure-ad-identities"></a>7. összekapcsolása az Azure Active Directory identitások használatáról

Azure Active Directory authentication az alábbi módszerek egyikét Azure AD-identitások használatáról adatbázishoz való kapcsolódás támogatja:

- Integrált Windows-hitelesítés használatával
- Az Azure Active Directory egyszerű felhasználónév és jelszó használatával
- Alkalmazás jogkivonat hitelesítés használatával

### <a name="71-connecting-using-integrated-windows-authentication"></a>7.1. Csatlakozás integrált (Windows)-hitelesítés használatával

Integrált Windows-hitelesítés használatához a tartományt az Active Directory és az Azure Active Directory szövetséges tartozó. A adatbázishoz való csatlakozáskor az ügyfélalkalmazás (vagy egy szolgáltatást) futnia kell a számítógépen a tartományhoz egy felhasználó tartományát hitelesítő adatok területén.

Adatbázishoz szeretne kapcsolódni egy beépített hitelesítés és Azure Active Directory-identitás, az adatbázis-kapcsolati karakterláncában megtalálható a hitelesítési kulcsszó kell beállítani az Active Directory integrált. A következő C# kód minta ADO .NET használja.

    string ConnectionString =
    @"Data Source=n9lxnyuzhv.database.windows.net; Authentication=Active Directory Integrated; Initial Catalog=testdb;";
    SqlConnection conn = new SqlConnection(ConnectionString);
    conn.Open();

Jegyzet, hogy a kapcsolati karakterlánc kulcsszó ``Integrated Security=True`` Azure SQL-adatbázishoz való csatlakozáshoz nem támogatott.
Fontos tudni, hogy amikor ODBC-kapcsolatot, fog szóközök eltávolítása és a "ActiveDirectoryIntegrated" hitelesítés beállítása.

### <a name="72-connecting-with-an-azure-ad-principal-name-and-a-password"></a>7.2. Az Azure Active Directory egyszerű felhasználónév és jelszó összekapcsolása
Adatbázishoz szeretne kapcsolódni egy beépített hitelesítés és Azure Active Directory-identitás használatával, a hitelesítési kulcsszó kell beállítani az Active Directory-jelszavát. A kapcsolati karakterlánc felhasználói azonosító/UID és a jelszó/PWD kulcsszavak és értékeket kell tartalmaznia. A következő C# kód minta ADO .NET használja.

    string ConnectionString =
      @"Data Source=n9lxnyuzhv.database.windows.net; Authentication=Active Directory Password; Initial Catalog=testdb;  UID=bob@contoso.onmicrosoft.com; PWD=MyPassWord!";
    SqlConnection conn = new SqlConnection(ConnectionString);
    conn.Open();

További tudnivalók a bemutató mintakódok használatával [Azure Active Directory Authentication GitHub bemutató](https://github.com/Microsoft/sql-server-samples/tree/master/samples/features/security/azure-active-directory-auth)Azure Active Directory hitelesítési módszereket.


### <a name="73-connecting-with-an-azure-ad-token"></a>7.3. az Azure Active Directory jogkivonat összekapcsolása
Ez a hitelesítési módszer lehetővé teszi, hogy a középszintű szolgáltatások Azure SQL-adatbázis vagy az Azure SQL-adatraktár jogkivonat az Azure Active Directory (AAD) bekérésével csatlakozni. Összetett olyan esetek, beleértve a tanúsítvány-alapú hitelesítés lehetővé teszi. Azure Active Directory jogkivonat hitelesítést négy alapvető lépéseket kell elvégeznie:

1. Az alkalmazás regisztrálása az Azure Active Directory és az ügyfél-azonosító beszerzése a kódot. 
2. Hozzon létre egy adatbázis-felhasználó, amely az alkalmazást. (Elvégezve korábbi a 6).
3. Hozzon létre egy tanúsítványt az ügyfelet a számítógép futtatja a az alkalmazás.
4. A tanúsítvány felvétele az alkalmazás kulcsként.

Minta kapcsolati karakterláncot:

```
string ConnectionString =@"Data Source=n9lxnyuzhv.database.windows.net; Initial Catalog=testdb;"
SqlConnection conn = new SqlConnection(ConnectionString);
connection.AccessToken = "Your JWT token"
conn.Open();
```

További információért tekintse meg [az SQL Server biztonsági](https://blogs.msdn.microsoft.com/sqlsecurity/2016/02/09/token-based-authentication-support-for-azure-sql-db-using-azure-ad-auth/).

### <a name="connecting-with-sqlcmd"></a>Sqlcmd összekapcsolása  
A következő utasítások csatlakozás 13.1 az verziójával sqlcmd, amely érhető el a [Letöltőközpontból](http://go.microsoft.com/fwlink/?LinkID=825643).

```
sqlcmd -S Target_DB_or_DW.testsrv.database.windows.net  -G  
sqlcmd -S Target_DB_or_DW.testsrv.database.windows.net -U bob@contoso.com -P MyAADPassword -G -l 30
```

## <a name="see-also"></a>Lásd még:

[Adatbázisok és bejelentkezés az Azure SQL-adatbázis kezelése](sql-database-manage-logins.md)

[A tárolókban található adatbázis-felhasználók](https://msdn.microsoft.com/library/ff929071.aspx)

[A felhasználó (Transact-SQL nyelvben) létrehozása](http://msdn.microsoft.com/library/ms173463.aspx)


<!--Image references-->

[1]: ./media/sql-database-aad-authentication/1aad-auth-diagram.png
[2]: ./media/sql-database-aad-authentication/2subscription-relationship.png
[3]: ./media/sql-database-aad-authentication/3admin-structure.png
[4]: ./media/sql-database-aad-authentication/4select-subscription.png
[5]: ./media/sql-database-aad-authentication/5ad-settings-portal.png
[6]: ./media/sql-database-aad-authentication/6edit-directory-select.png
[7]: ./media/sql-database-aad-authentication/7edit-directory-confirm.png
[8]: ./media/sql-database-aad-authentication/8choose-ad.png
[9]: ./media/sql-database-aad-authentication/9ad-settings.png
[10]: ./media/sql-database-aad-authentication/10choose-admin.png
[11]: ./media/sql-database-aad-authentication/11connect-using-int-auth.png
[12]: ./media/sql-database-aad-authentication/12connect-using-pw-auth.png
[13]: ./media/sql-database-aad-authentication/13connect-to-db.png

