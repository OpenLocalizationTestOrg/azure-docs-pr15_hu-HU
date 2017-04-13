<properties
    pageTitle="Mindig titkosított: Adatbázis-titkosítással Azure SQL-adatbázisban bizalmas adatokat védelme |} Microsoft Azure"
    description="Az SQL-adatbázis kényes adatok védelme biztonsági perc alatt."
    keywords="a felhő titkosítási titkosítást, a titkosítási kulcs"
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor="cgronlun"/>


<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/18/2016"
    ms.author="sstein"/>

# <a name="always-encrypted-protect-sensitive-data-in-sql-database-and-store-your-encryption-keys-in-azure-key-vault"></a>Mindig titkosított: SQL-adatbázisban kényes adatok védelme és tárolni a titkosítási kulcs Azure kulcs tárolóból elemre

> [AZURE.SELECTOR]
- [Azure kulcs tárolóból elemre](sql-database-always-encrypted-azure-key-vault.md)
- [A Windows áruház-tanúsítvány](sql-database-always-encrypted.md)


Ez a cikk bemutatja, hogyan biztonságossá tétele bizalmas adatokat egy SQL-adatbázisban adatok titkosítással [Mindig a titkosított varázsló](https://msdn.microsoft.com/library/mt459280.aspx) segítségével [Az SQL Server Management Studio (SSMS)](https://msdn.microsoft.com/library/hh213248.aspx). Bemutatja, hogyan kell tárolni az egyes a titkosítási kulcs az Azure kulcs tárolóból elemre az útmutató is bemutat.

Mindig titkosítva egy adatok új titkosítási technológiára az Azure SQL-adatbázis és az SQL Server, amely segít a többi a kiszolgálón kényes adatok védelme biztonsági során az ügyfél- és kiszolgálóoldali között, és az adatok használata közben mozgását. Mindig a titkosított biztosítja, hogy bizalmas adatokat soha nem jelenik meg az adatbázis-rendszer belül szövegként. Miután beállította az adatok titkosítása, csak ügyfélalkalmazásokban vagy alkalmazás kiszolgálók férnek hozzá a billentyűk érheti el egyszerű szöveges adatokat. Részletes tudnivalókért lásd [Mindig titkosított (adatbázismotor)](https://msdn.microsoft.com/library/mt163865.aspx).


Miután beállította az adatbázisról, hogy mindig titkosított, a C# a Visual Studio a titkosított adatokkal végzett munkához ügyfélalkalmazás létrehoznia.

Kövesse az ebben a cikkben leírt lépéseket, és megtudhatja, hogy miként állíthat be a mindig titkosított az Azure SQL-adatbázishoz. Ez a cikk megtanulhatja, hogyan végezhetők el az alábbi műveleteket:

- SSMS a mindig titkosított varázsló segítségével [mindig titkosított kulcsok](https://msdn.microsoft.com/library/mt163865.aspx#Anchor_3)létrehozása.
    - Hozzon létre egy [oszlop fő kulcs (CMK)](https://msdn.microsoft.com/library/mt146393.aspx).
    - Hozzon létre egy [oszlop titkosítási kulcs (CEK)](https://msdn.microsoft.com/library/mt146372.aspx).
- Adatbázis-táblázat létrehozása és oszlopok titkosítása.
- Hozzon létre egy alkalmazást, amely illeszt be, kijelöli és a titkosított oszlopában található adatokat jeleníti meg.


## <a name="prerequisites"></a>Előfeltételek

Az ebben az oktatóanyagban lesz szüksége:

- Az Azure-fiók és az előfizetés. Ha nincs telepítve egyik regisztráljon az [ingyenes próbaverziót](https://azure.microsoft.com/pricing/free-trial/).
- [SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) verzió 13.0.700.242 vagy újabb verziójában.
- [.NET-keretrendszer 4.6](https://msdn.microsoft.com/library/w0x726c2.aspx) vagy újabb verzióját (az ügyfélszámítógépen).
- [Visual Studio](https://www.visualstudio.com/downloads/download-visual-studio-vs.aspx).
- [Azure PowerShell](../powershell-install-configure.md), 1.0 vagy újabb verziója. Típus **(Get-modul azure - ListAvailable). Verzió** PowerShell melyik verziót futtatja a megjelenítéséhez.



## <a name="enable-your-client-application-to-access-the-sql-database-service"></a>Az SQL-adatbázis szolgáltatás eléréséhez az ügyfélalkalmazás engedélyezése

Engedélyeznie kell az ügyfélalkalmazás az SQL-adatbázis-szolgáltatás eléréséhez állítsa be a szükséges hitelesítés és beszerzése a *ClientId* és a *titkos* szüksége lesz az alkalmazás a következő kódrészlet hitelesítést végezni.

1. Nyissa meg az [Azure klasszikus portálon](http://manage.windowsazure.com).
2. Jelölje ki **Az Active Directory** , majd kattintson az Active Directory-példány, amelyekkel az alkalmazást.
3. Kattintson az **alkalmazások**elemre, és kattintson a **hozzáadása**gombra.
4. Írja be az alkalmazás nevét (például: *myClientApp*), jelölje be a **WEBES alkalmazás**és a nyílra kattintva továbbra is.
5. A **SIGN-ON URL-cím** és az **Alkalmazás azonosítója URI** írjon be egy érvényes URL-CÍMÉT (például a *http://myClientApp*), és továbbra is.
6. Kattintson a **KONFIGURÁLÁS**gombra.
7. Másolja a vágólapra a **ügyfél-azonosító**. (Szüksége lesz ezt az értéket a kódban később.)
8. A **billentyűk** szakaszában jelölje be az **1 év** a legördülő listában **Jelölje ki az időtartam** listában. (, Másolja a kulcs lépésben 14 mentése után.)
11. Görgessen le, és kattintson az **alkalmazás hozzáadása**parancsra.
12. Hagyja bejelölve a **megjelenítése** beállítás **A Microsoft** -alkalmazás, és kattintson a **Microsoft Azure szolgáltatás kezelése**. Kattintson a továbbra is be van jelölve.
13. Jelölje ki a **Hozzáférési Azure szolgáltatás kezelése** a **Meghatalmazott engedélyeit** legördülő listából.
14. Kattintson a **MENTÉS**gombra.
15. A mentés befejezése után a kulcs értékét másolja a **billentyűk** szakaszában. (Szüksége lesz ezt az értéket a kódban később.)



## <a name="create-a-key-vault-to-store-your-keys"></a>Hozzon létre egy fő tárolóból elemre a kulcsok tárolására

Most, hogy be van állítva az ügyfél-alkalmazást, és az ügyfél-azonosító van, érdemes létrehozni egy fő tárolóból elemre, és konfigurálása a saját hozzáférési házirendet, hogy Ön és az alkalmazás férhet hozzá a tárolóra titkos kulcsok (mindig titkosított billentyűk). A *Hozzon létre*, *kérjen*, *lista*, *bejelentkezési*, *Ellenőrizze*, *wrapKey*és *unwrapKey* engedélyekre szükség, hozhat létre egy új oszlop fő billentyűt, és az SQL Server Management Studio titkosítási beállítása.

A következő parancsfájl futtatásával gyorsan létrehozhat egy fő tárolóból elemre. Részletes magyarázatot parancsmagok és további információt az létrehozása és konfigurálása a fő tárolóból elemre olvassa el az [első lépések az Azure kulcs tárolóból elemre](../Key-Vault/key-vault-get-started.md)című témakört.



    $subscriptionName = '<your Azure subscription name>'
    $userPrincipalName = '<username@domain.com>'
    $clientId = '<client ID that you copied in step 7 above>'
    $resourceGroupName = '<resource group name>'
    $location = '<datacenter location>'
    $vaultName = 'AeKeyVault'


    Login-AzureRmAccount
    $subscriptionId = (Get-AzureRmSubscription -SubscriptionName $subscriptionName).SubscriptionId
    Set-AzureRmContext -SubscriptionId $subscriptionId

    New-AzureRmResourceGroup –Name $resourceGroupName –Location $location
    New-AzureRmKeyVault -VaultName $vaultName -ResourceGroupName $resourceGroupName -Location $location

    Set-AzureRmKeyVaultAccessPolicy -VaultName $vaultName -ResourceGroupName $resourceGroupName -PermissionsToKeys create,get,wrapKey,unwrapKey,sign,verify,list -UserPrincipalName $userPrincipalName
    Set-AzureRmKeyVaultAccessPolicy  -VaultName $vaultName  -ResourceGroupName $resourceGroupName -ServicePrincipalName $clientId -PermissionsToKeys get,wrapKey,unwrapKey,sign,verify,list




## <a name="create-a-blank-sql-database"></a>Hozzon létre egy üres SQL-adatbázis
1. Jelentkezzen be az [Azure-portálon](https://portal.azure.com/).
2. Nyissa meg az **Új** > **adatok + tárhely** > **SQL-adatbázishoz**.
3. Hozzon létre egy **üres** adatbázis **klinikán** nevű új vagy meglévő kiszolgálón. Adatbázis létrehozása az Azure-portálon kapcsolatos részletes útmutatást a [perc egy SQL-adatbázis létrehozása](sql-database-get-started.md)című témakör tartalmaz.

    ![Üres adatbázis létrehozása](./media/sql-database-always-encrypted-azure-key-vault/create-database.png)

A kapcsolat szüksége lesz az oktatóprogram a karakterlánc, így után hoz létre az adatbázist, tallózással keresse meg az új klinikán adatbázist, és másolja a vágólapra a kapcsolati karakterlánc. A kapcsolati karakterlánc bármikor elérheti, de egyszerűen másolja a vágólapra az Azure-portálon.

1. Nyissa meg a **SQL-adatbázisait** > **klinikán** > **megjelenítése adatbázis kapcsolati karakterláncot**.
2. Másolja a vágólapra a kapcsolati karakterlánc **ADO.NET**.

    ![Másolja a vágólapra a kapcsolati karakterlánc](./media/sql-database-always-encrypted-azure-key-vault/connection-strings.png)


## <a name="connect-to-the-database-with-ssms"></a>Az adatbázis SSMS csatlakoztatása

Nyissa meg a SSMS, és csatlakozzon a kiszolgálóhoz a klinikán adatbázissal.


1. Nyissa meg a SSMS. (Válassza a **Csatlakozás** > **Adatbázismotort** a **Csatlakozás a kiszolgálóhoz** ablak megnyitásához, ha ez nem nyissa meg.)
2. Írja be a kiszolgáló neve és a hitelesítő adatait. Az SQL-adatbázis lap találhatók a kiszolgáló nevét, és a kapcsolati karakterláncot az előbb másolt. Írja be a teljes kiszolgáló nevét, például *database.windows.net*.

    ![Másolja a vágólapra a kapcsolati karakterlánc](./media/sql-database-always-encrypted-azure-key-vault/ssms-connect.png)

Ha az **Új tűzfal szabály** ablak nyílik meg, jelentkezzen be az Azure, és SSMS tűzfal új szabály létrehozása a, hogy.


## <a name="create-a-table"></a>Táblázat létrehozása

Ebben a részben létrehoz egy betegek adatokat tároló tábla. Még nem kezdetben titkosított – titkosítási fog konfigurálni az alábbi szakasszal.

1. Bontsa ki az **adatbázisokat**.
1. Kattintson a jobb gombbal a **klinikán** adatbázist, majd kattintson az **Új lekérdezést**.
2. Másolja a következő Transact-SQL nyelvben (T-SQL) az új lekérdezés ablak, és a **végrehajtás** azt.


        CREATE TABLE [dbo].[Patients](
         [PatientId] [int] IDENTITY(1,1),
         [SSN] [char](11) NOT NULL,
         [FirstName] [nvarchar](50) NULL,
         [LastName] [nvarchar](50) NULL,
         [MiddleName] [nvarchar](50) NULL,
         [StreetAddress] [nvarchar](50) NULL,
         [City] [nvarchar](50) NULL,
         [ZipCode] [char](5) NULL,
         [State] [char](2) NULL,
         [BirthDate] [date] NOT NULL
         PRIMARY KEY CLUSTERED ([PatientId] ASC) ON [PRIMARY] );
         GO


## <a name="encrypt-columns-configure-always-encrypted"></a>Titkosítsa az oszlopok (mindig titkosított konfigurálása)

SSMS biztosít, amelyek segítségével egyszerűen állítsa be a mindig titkosított beállításával a oszlop fő kulcs, az oszlop titkosítókulcs és a titkosított oszlopban, a varázsló.

1. Bontsa ki az **adatbázisok** > **klinikán** > **táblákat**.
2. Kattintson a jobb gombbal a **páciensek** tábla, és válassza az **Oszlopok titkosítása** nyissa meg a titkosított mindig varázslót:

    ![Titkosítsa az oszlopok](./media/sql-database-always-encrypted-azure-key-vault/encrypt-columns.png)

A titkosított mindig varázsló is tartalmaz, az alábbi szakaszok: **Oszlopos kijelölés**, **Fő kulcs konfigurációs**, **adatérvényesítési**és **összesítése**.

### <a name="column-selection"></a>Oszlopos kijelölés##

**Bevezetés** a weblapon, nyissa meg az **Oszlopok kiválasztása** lapon kattintson a **Tovább gombra** . Ezen a lapon, titkosítani kívánt oszlopok kiválasztott [a típusú titkosítást, és milyen oszlop titkosítási kulcs (CEK)](https://msdn.microsoft.com/library/mt459280.aspx#Anchor_2) használni.

Titkosítsa az egyes betegek **Taj-szám** és **SzületésiDátum mező** adatait. A TAJ-szám oszlopban mérvadó titkosítás, amely támogatja az egyenlő keresések illesztések és a csoportosítás fogja használni. A SzületésiDátum oszlop véletlenszerűen titkosítás, amely nem támogatja a műveletek fogja használni.

Állítsa a TAJ-szám oszlop **Típusú titkosítást** **Deterministic** és az **Randomized**SzületésiDátum oszlop. Kattintson a **Tovább**gombra.

![Titkosítsa az oszlopok](./media/sql-database-always-encrypted-azure-key-vault/column-selection.png)

### <a name="master-key-configuration"></a>Diaminta kulcs konfigurálása###

A **Diaminta kulcs beállítása** lapon pedig a CMK beállítása jelölje ki a fontosabb tár szolgáltató a CMK tárolási helyére. Egy CMK tárolását jelenleg a tanúsítvány a Windows áruházból, Azure kulcs tárolóból elemre vagy hardveres biztonsági modult (HSM).

Ebből az oktatóanyagból megtudhatja, hogy hogyan tárolja a billentyűk Azure kulcs tárolóból elemre.

1.     Jelölje ki az **Azure kulcs tárolóból elemre**.
1.     A legördülő listából, jelölje ki a kívánt fontosabb tárolóból elemre.
1.     Kattintson a **Tovább**gombra.

![Diaminta kulcs konfigurálása](./media/sql-database-always-encrypted-azure-key-vault/master-key-configuration.png)


### <a name="validation"></a>Adatérvényesítési###

Az oszlopok most titkosítása, vagy mentse egy PowerShell-parancsprogramot, később futtatni. Ebben az oktatóanyagban válassza a **Folytatás gombra a befejezéshez** , és kattintson a **Tovább**gombra.

### <a name="summary"></a>Összefoglalás ###

Győződjön meg róla, hogy a beállításokat az összes javítása, és kattintson a **Befejezés gombra** az mindig titkosított a beállítási folyamat befejezéséhez.


![Összefoglalás](./media/sql-database-always-encrypted-azure-key-vault/summary.png)


### <a name="verify-the-wizards-actions"></a>A varázsló műveletek ellenőrzése

Miután a varázsló végzett, az adatbázis van beállítva mindig titkosítva. A varázsló által végzett az alábbi műveleteket:

- Egy oszlop fő kulcs létrehozott és tárolt azt az Azure kulcs tárolóból elemre.
- Egy oszlop titkosítókulcs létrehozott és tárolt azt az Azure kulcs tárolóból elemre.
- A kijelölt oszlopok titkosításhoz beállítva. A páciensek tábla jelenleg nem tartalmaz adatokat, de a meglévő adatok a a kijelölt oszlopok most titkosítva van.

A SSMS hívóbetűi kibocsátása ellenőrzéséhez **klinikán**kibontása > **biztonsági** > **Mindig a titkosított kulcsok**.


## <a name="create-a-client-application-that-works-with-the-encrypted-data"></a>A titkosított adatokkal működő ügyfél-alkalmazás létrehozása

Most, hogy mindig titkosítva van beállítva, létrehozhat egy alkalmazást, amely hajt végre *illeszt be* , és *kiválasztja* a titkosított oszlopok.  

> [AZURE.IMPORTANT] Az alkalmazás [SqlParameter](https://msdn.microsoft.com/library/system.data.sqlclient.sqlparameter.aspx) objektumok kell használnia, ha a kiszolgáló titkosított mindig oszlopokkal egyszerű szöveges adatok átadása. Kivétel eredményezi konstans érték átadása SqlParameter objektumok használata nélkül.

1. Nyissa meg a Visual Studióban, és hozzon létre egy új C# konzol alkalmazást. Győződjön meg arról, hogy a projekt van állítva, **a .NET-keretrendszer 4.6** vagy újabb verziójára.
2. Nevezze el a projekt **AlwaysEncryptedConsoleAKVApp** , és kattintson az **OK gombra**.
![Új New](./media/sql-database-always-encrypted-azure-key-vault/console-app.png)
3. A következő NuGet csomagok telepítése **eszközök**ismételt > **NuGet csomag Manager** > **Csomag kezelője konzol**.

Futtassa a következő kódsorokat két a csomag Manager konzolban.

    Install-Package Microsoft.SqlServer.Management.AlwaysEncrypted.AzureKeyVaultProvider
    Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory



## <a name="modify-your-connection-string-to-enable-always-encrypted"></a>A kapcsolati karakterlánc ahhoz, hogy mindig titkosított módosítása

Ez a szakasz ismerteti, hogyan ahhoz, hogy mindig titkosított az adatbázis-kapcsolati karakterláncában megtalálható.


Ahhoz, hogy mindig titkosított, meg kell az **Oszlopban a titkosítási beállítás** kulcsszó hozzáadása a kapcsolati karakterláncot, és állítsa be **engedélyezve**.

Beállíthatja, hogy ez közvetlenül a kapcsolati karakterláncban, vagy azt is megadhatja ilyenkor [SqlConnectionStringBuilder](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnectionstringbuilder.aspx)használatával. A minta alkalmazás a következő szakaszban megtudhatja, hogy hogyan **SqlConnectionStringBuilder**.



### <a name="enable-always-encrypted-in-the-connection-string"></a>Engedélyezze a mindig titkosított kapcsolati karakterláncban

A következő kulcsszó hozzáadása a kapcsolati karakterlánc.

    Column Encryption Setting=Enabled


### <a name="enable-always-encrypted-with-sqlconnectionstringbuilder"></a>Mindig titkosított SqlConnectionStringBuilder engedélyezése

A következő kódot megtudhatja, hogy miként kapcsolhatja be mindig titkosított [SqlConnectionStringBuilder.ColumnEncryptionSetting](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnectionstringbuilder.columnencryptionsetting.aspx) beállítás [engedélyezve van](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnectioncolumnencryptionsetting.aspx).

    // Instantiate a SqlConnectionStringBuilder.
    SqlConnectionStringBuilder connStringBuilder =
       new SqlConnectionStringBuilder("replace with your connection string");

    // Enable Always Encrypted.
    connStringBuilder.ColumnEncryptionSetting =
       SqlConnectionColumnEncryptionSetting.Enabled;

## <a name="register-the-azure-key-vault-provider"></a>Regisztráljon az Azure kulcs tárolóra szolgáltató

A következő kódot megtudhatja, hogy miként regisztrálhatja a ADO.NET-illesztőprogram az Azure kulcs tárolóra szolgáltató.

    private static ClientCredential _clientCredential;

    static void InitializeAzureKeyVaultProvider()
    {
       _clientCredential = new ClientCredential(clientId, clientSecret);

       SqlColumnEncryptionAzureKeyVaultProvider azureKeyVaultProvider =
          new SqlColumnEncryptionAzureKeyVaultProvider(GetToken);

       Dictionary<string, SqlColumnEncryptionKeyStoreProvider> providers =
          new Dictionary<string, SqlColumnEncryptionKeyStoreProvider>();

       providers.Add(SqlColumnEncryptionAzureKeyVaultProvider.ProviderName, azureKeyVaultProvider);
       SqlConnection.RegisterColumnEncryptionKeyStoreProviders(providers);
    }



## <a name="always-encrypted-sample-console-application"></a>Mindig titkosított minta New

Ez a példa bemutatja, hogy miként:

- Módosítsa a kapcsolati karakterlánc ahhoz, hogy mindig titkosítva.
- Regisztráljon az Azure kulcs tárolóból elemre az alkalmazás fő áruházból szolgáltatóként.  
- Adatok beillesztése a titkosított oszlopokat.
- Válasszon ki egy rekordot szerinti szűrés egy adott értéket egy titkosított oszlopban.

Cserélje le a **Program.cs** tartalmát a következő kódot. Cserélje ki a kívánt szövegsorba, közvetlenül a program a fő mód az érvényes kapcsolattal az Azure portálról a globális connectionString változó a kapcsolati karakterlánc. Ez a változás csupán, hogy a kód van szüksége.

Az app futtatásával mindig titkosított működését.

    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Text;
    using System.Threading.Tasks;
    using System.Data;
    using System.Data.SqlClient;
    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    using Microsoft.SqlServer.Management.AlwaysEncrypted.AzureKeyVaultProvider;

    namespace AlwaysEncryptedConsoleAKVApp
    {
    class Program
    {
        // Update this line with your Clinic database connection string from the Azure portal.
        static string connectionString = @"<connection string from the portal>";
        static string clientId = @"<client id from step 7 above>";
        static string clientSecret = "<key from step 13 above>";


        static void Main(string[] args)
        {
            InitializeAzureKeyVaultProvider();

            Console.WriteLine("Signed in as: " + _clientCredential.ClientId);

            Console.WriteLine("Original connection string copied from the Azure portal:");
            Console.WriteLine(connectionString);

            // Create a SqlConnectionStringBuilder.
            SqlConnectionStringBuilder connStringBuilder =
                new SqlConnectionStringBuilder(connectionString);

            // Enable Always Encrypted for the connection.
            // This is the only change specific to Always Encrypted
            connStringBuilder.ColumnEncryptionSetting =
                SqlConnectionColumnEncryptionSetting.Enabled;

            Console.WriteLine(Environment.NewLine + "Updated connection string with Always Encrypted enabled:");
            Console.WriteLine(connStringBuilder.ConnectionString);

            // Update the connection string with a password supplied at runtime.
            Console.WriteLine(Environment.NewLine + "Enter server password:");
            connStringBuilder.Password = Console.ReadLine();


            // Assign the updated connection string to our global variable.
            connectionString = connStringBuilder.ConnectionString;


            // Delete all records to restart this demo app.
            ResetPatientsTable();

            // Add sample data to the Patients table.
            Console.Write(Environment.NewLine + "Adding sample patient data to the database...");

            InsertPatient(new Patient()
            {
                SSN = "999-99-0001",
                FirstName = "Orlando",
                LastName = "Gee",
                BirthDate = DateTime.Parse("01/04/1964")
            });
            InsertPatient(new Patient()
            {
                SSN = "999-99-0002",
                FirstName = "Keith",
                LastName = "Harris",
                BirthDate = DateTime.Parse("06/20/1977")
            });
            InsertPatient(new Patient()
            {
                SSN = "999-99-0003",
                FirstName = "Donna",
                LastName = "Carreras",
                BirthDate = DateTime.Parse("02/09/1973")
            });
            InsertPatient(new Patient()
            {
                SSN = "999-99-0004",
                FirstName = "Janet",
                LastName = "Gates",
                BirthDate = DateTime.Parse("08/31/1985")
            });
            InsertPatient(new Patient()
            {
                SSN = "999-99-0005",
                FirstName = "Lucy",
                LastName = "Harrington",
                BirthDate = DateTime.Parse("05/06/1993")
            });


            // Fetch and display all patients.
            Console.WriteLine(Environment.NewLine + "All the records currently in the Patients table:");

            foreach (Patient patient in SelectAllPatients())
            {
                Console.WriteLine(patient.FirstName + " " + patient.LastName + "\tSSN: " + patient.SSN + "\tBirthdate: " + patient.BirthDate);
            }

            // Get patients by SSN.
            Console.WriteLine(Environment.NewLine + "Now lets locate records by searching the encrypted SSN column.");

            string ssn;

            // This very simple validation only checks that the user entered 11 characters.
            // In production be sure to check all user input and use the best validation for your specific application.
            do
            {
                Console.WriteLine("Please enter a valid SSN (ex. 999-99-0003):");
                ssn = Console.ReadLine();
            } while (ssn.Length != 11);

            // The example allows duplicate SSN entries so we will return all records
            // that match the provided value and store the results in selectedPatients.
            Patient selectedPatient = SelectPatientBySSN(ssn);

            // Check if any records were returned and display our query results.
            if (selectedPatient != null)
            {
                Console.WriteLine("Patient found with SSN = " + ssn);
                Console.WriteLine(selectedPatient.FirstName + " " + selectedPatient.LastName + "\tSSN: "
                    + selectedPatient.SSN + "\tBirthdate: " + selectedPatient.BirthDate);
            }
            else
            {
                Console.WriteLine("No patients found with SSN = " + ssn);
            }

            Console.WriteLine("Press Enter to exit...");
            Console.ReadLine();
        }


        private static ClientCredential _clientCredential;

        static void InitializeAzureKeyVaultProvider()
        {

            _clientCredential = new ClientCredential(clientId, clientSecret);

            SqlColumnEncryptionAzureKeyVaultProvider azureKeyVaultProvider =
              new SqlColumnEncryptionAzureKeyVaultProvider(GetToken);

            Dictionary<string, SqlColumnEncryptionKeyStoreProvider> providers =
              new Dictionary<string, SqlColumnEncryptionKeyStoreProvider>();

            providers.Add(SqlColumnEncryptionAzureKeyVaultProvider.ProviderName, azureKeyVaultProvider);
            SqlConnection.RegisterColumnEncryptionKeyStoreProviders(providers);
        }

        public async static Task<string> GetToken(string authority, string resource, string scope)
        {
            var authContext = new AuthenticationContext(authority);
            AuthenticationResult result = await authContext.AcquireTokenAsync(resource, _clientCredential);

            if (result == null)
                throw new InvalidOperationException("Failed to obtain the access token");
            return result.AccessToken;
        }

        static int InsertPatient(Patient newPatient)
        {
            int returnValue = 0;

            string sqlCmdText = @"INSERT INTO [dbo].[Patients] ([SSN], [FirstName], [LastName], [BirthDate])
     VALUES (@SSN, @FirstName, @LastName, @BirthDate);";

            SqlCommand sqlCmd = new SqlCommand(sqlCmdText);


            SqlParameter paramSSN = new SqlParameter(@"@SSN", newPatient.SSN);
            paramSSN.DbType = DbType.AnsiStringFixedLength;
            paramSSN.Direction = ParameterDirection.Input;
            paramSSN.Size = 11;

            SqlParameter paramFirstName = new SqlParameter(@"@FirstName", newPatient.FirstName);
            paramFirstName.DbType = DbType.String;
            paramFirstName.Direction = ParameterDirection.Input;

            SqlParameter paramLastName = new SqlParameter(@"@LastName", newPatient.LastName);
            paramLastName.DbType = DbType.String;
            paramLastName.Direction = ParameterDirection.Input;

            SqlParameter paramBirthDate = new SqlParameter(@"@BirthDate", newPatient.BirthDate);
            paramBirthDate.SqlDbType = SqlDbType.Date;
            paramBirthDate.Direction = ParameterDirection.Input;

            sqlCmd.Parameters.Add(paramSSN);
            sqlCmd.Parameters.Add(paramFirstName);
            sqlCmd.Parameters.Add(paramLastName);
            sqlCmd.Parameters.Add(paramBirthDate);

            using (sqlCmd.Connection = new SqlConnection(connectionString))
            {
                try
                {
                    sqlCmd.Connection.Open();
                    sqlCmd.ExecuteNonQuery();
                }
                catch (Exception ex)
                {
                    returnValue = 1;
                    Console.WriteLine("The following error was encountered: ");
                    Console.WriteLine(ex.Message);
                    Console.WriteLine(Environment.NewLine + "Press Enter key to exit");
                    Console.ReadLine();
                    Environment.Exit(0);
                }
            }
            return returnValue;
        }


        static List<Patient> SelectAllPatients()
        {
            List<Patient> patients = new List<Patient>();


            SqlCommand sqlCmd = new SqlCommand(
              "SELECT [SSN], [FirstName], [LastName], [BirthDate] FROM [dbo].[Patients]",
                new SqlConnection(connectionString));


            using (sqlCmd.Connection = new SqlConnection(connectionString))

            using (sqlCmd.Connection = new SqlConnection(connectionString))
            {
                try
                {
                    sqlCmd.Connection.Open();
                    SqlDataReader reader = sqlCmd.ExecuteReader();

                    if (reader.HasRows)
                    {
                        while (reader.Read())
                        {
                            patients.Add(new Patient()
                            {
                                SSN = reader[0].ToString(),
                                FirstName = reader[1].ToString(),
                                LastName = reader["LastName"].ToString(),
                                BirthDate = (DateTime)reader["BirthDate"]
                            });
                        }
                    }
                }
                catch (Exception ex)
                {
                    throw;
                }
            }

            return patients;
        }


        static Patient SelectPatientBySSN(string ssn)
        {
            Patient patient = new Patient();

            SqlCommand sqlCmd = new SqlCommand(
                "SELECT [SSN], [FirstName], [LastName], [BirthDate] FROM [dbo].[Patients] WHERE [SSN]=@SSN",
                new SqlConnection(connectionString));

            SqlParameter paramSSN = new SqlParameter(@"@SSN", ssn);
            paramSSN.DbType = DbType.AnsiStringFixedLength;
            paramSSN.Direction = ParameterDirection.Input;
            paramSSN.Size = 11;

            sqlCmd.Parameters.Add(paramSSN);


            using (sqlCmd.Connection = new SqlConnection(connectionString))
            {
                try
                {
                    sqlCmd.Connection.Open();
                    SqlDataReader reader = sqlCmd.ExecuteReader();

                    if (reader.HasRows)
                    {
                        while (reader.Read())
                        {
                            patient = new Patient()
                            {
                                SSN = reader[0].ToString(),
                                FirstName = reader[1].ToString(),
                                LastName = reader["LastName"].ToString(),
                                BirthDate = (DateTime)reader["BirthDate"]
                            };
                        }
                    }
                    else
                    {
                        patient = null;
                    }
                }
                catch (Exception ex)
                {
                    throw;
                }
            }
            return patient;
        }


        // This method simply deletes all records in the Patients table to reset our demo.
        static int ResetPatientsTable()
        {
            int returnValue = 0;

            SqlCommand sqlCmd = new SqlCommand("DELETE FROM Patients");
            using (sqlCmd.Connection = new SqlConnection(connectionString))
            {
                try
                {
                    sqlCmd.Connection.Open();
                    sqlCmd.ExecuteNonQuery();

                }
                catch (Exception ex)
                {
                    returnValue = 1;
                }
            }
            return returnValue;
        }
    }

    class Patient
    {
        public string SSN { get; set; }
        public string FirstName { get; set; }
        public string LastName { get; set; }
        public DateTime BirthDate { get; set; }
    }
    }



## <a name="verify-that-the-data-is-encrypted"></a>Ellenőrizze, hogy az adatok titkosítva van

Gyorsan ellenőrizheti, hogy a tényleges adatok kiszolgálói titkosítja betegek adatok lekérdezés a SSMS (használatával a meglévő kapcsolatot hol **Oszlop titkosítási beállítás** még nem engedélyezett).

Futtassa a következő lekérdezés klinikán-adatbázisban.

    SELECT FirstName, LastName, SSN, BirthDate FROM Patients;

Láthatja, hogy a titkosított oszlop nem tartalmaz egyszerű szöveges adatokat.

   ![Új New](./media/sql-database-always-encrypted-azure-key-vault/ssms-encrypted.png)


Egyszerű szöveges adatok eléréséhez SSMS használatához is adhat hozzá a *oszlopot a titkosítási beállítás = engedélyezett* paraméter nélkül a kapcsolatot.

1. SSMS kattintson a jobb gombbal a kiszolgáló az **Objektum Explorerben** , és válassza a **kapcsolat bontása**.
2. Kattintson a **Csatlakozás** > **Adatbázismotort** a **Csatlakozás a kiszolgálóhoz** ablak megnyitásához, majd kattintson a **Beállítások**gombra.
3. Kattintson a **További kapcsolat paramétereket** és típusa **oszlopot a titkosítási beállítás = engedélyezett**.

    ![Új New](./media/sql-database-always-encrypted-azure-key-vault/ssms-connection-parameter.png)

4. Futtassa a következő lekérdezés klinikán-adatbázisban.

        SELECT FirstName, LastName, SSN, BirthDate FROM Patients;

     Ekkor megjelenik a titkosított oszlop egyszerű szöveges adatait.


    ![Új New](./media/sql-database-always-encrypted-azure-key-vault/ssms-plaintext.png)


## <a name="next-steps"></a>Következő lépések
Miután létrehozott egy használó mindig titkosított adatbázis, érdemes tegye a következőket:

- [Elforgatás és a billentyűk karbantartása](https://msdn.microsoft.com/library/mt607048.aspx).
- [Adatok, amelyek már titkosított mindig titkosított áttelepítése](https://msdn.microsoft.com/library/mt621539.aspx).


## <a name="related-information"></a>Kapcsolódó információ

- [Mindig titkosított (ügyfél fejlesztés)](https://msdn.microsoft.com/library/mt147923.aspx)
- [Áttetsző adatok titkosítása](https://msdn.microsoft.com/library/bb934049.aspx)
- [Az SQL Server-titkosítás](https://msdn.microsoft.com/library/bb510663.aspx)
- [Mindig a titkosított varázsló](https://msdn.microsoft.com/library/mt459280.aspx)
- [Mindig titkosított blog](http://blogs.msdn.com/b/sqlsecurity/archive/tags/always-encrypted/)
