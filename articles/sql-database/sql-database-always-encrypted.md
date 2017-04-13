<properties
    pageTitle="Mindig titkosított: Adatbázis-titkosítással Azure SQL-adatbázisban bizalmas adatokat védelme |} Microsoft Azure"
    description="Az SQL-adatbázis kényes adatok védelme biztonsági perc alatt."
    keywords="titkosítsa az adatok, a sql-titkosítás, az adatbázis-titkosítás, a bizalmas adatokat, mindig titkosított"
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

# <a name="always-encrypted-protect-sensitive-data-in-sql-database-and-store-your-encryption-keys-in-the-windows-certificate-store"></a>Mindig titkosított: SQL-adatbázisban kényes adatok védelme és tárolni a titkosítási kulcs a Windows-tárolóban található

> [AZURE.SELECTOR]
- [Azure kulcs tárolóból elemre](sql-database-always-encrypted-azure-key-vault.md)
- [A Windows áruház-tanúsítvány](sql-database-always-encrypted.md)


Ez a cikk bemutatja, hogyan bizalmas adatokat egy SQL-adatbázisban, az adatbázis-titkosítás biztonságos [SQL Server Management Studio (SSMS)](https://msdn.microsoft.com/library/hh213248.aspx)a [Mindig a titkosított varázsló](https://msdn.microsoft.com/library/mt459280.aspx) segítségével. Azt is megtudhatja, hogy hogyan tárolja a titkosítási kulcs a tanúsítvány a Windows áruházból.

Mindig titkosított egy adatok új titkosítási technológiára Azure SQL-adatbázisban és SQL Server, amely segít a többi a kiszolgálón bizalmas adatok védelme biztonsági ügyfél és a kiszolgáló közötti mozgás közben, és az adatok használata közben, gondoskodik arról, hogy bizalmas soha nem jelenik meg az adatbázis-rendszer belül szövegként. Után titkosítsa az adatokat, csak ügyfélalkalmazásokban vagy a billentyűk hozzáféréssel rendelkező alkalmazás kiszolgálók érheti el egyszerű szöveges adatokat. Részletes tudnivalókért lásd [Mindig titkosított (adatbázismotor)](https://msdn.microsoft.com/library/mt163865.aspx).

Miután az adatbázisról, hogy mindig titkosított, a C# a Visual Studio a titkosított adatokkal végzett munkához ügyfélalkalmazás hoz létre.

Kövesse a ebből a cikkből megtudhatja, hogy miként állíthat be a mindig titkosított az Azure SQL-adatbázishoz. Ez a cikk megtanulhatja, hogyan végezhetők el az alábbi műveleteket:

- SSMS a mindig titkosított varázsló segítségével [Mindig a titkosított kulcsok](https://msdn.microsoft.com/library/mt163865.aspx#Anchor_3)létrehozása.
    - Hozzon létre egy [Oszlop fő kulcs (CMK)](https://msdn.microsoft.com/library/mt146393.aspx).
    - Hozzon létre egy [oszlop titkosítási kulcs (CEK)](https://msdn.microsoft.com/library/mt146372.aspx).
- Adatbázis-táblázat létrehozása és oszlopok titkosítása.
- Hozzon létre egy alkalmazást, amely illeszt be, kijelöli és a titkosított oszlopában található adatokat jeleníti meg.

## <a name="prerequisites"></a>Előfeltételek

Az ebben az oktatóanyagban lesz szüksége:

- Az Azure-fiók és az előfizetés. Ha nincs telepítve egyik regisztráljon az [ingyenes próbaverziót](https://azure.microsoft.com/pricing/free-trial/).
- [SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) verzió 13.0.700.242 vagy újabb verziójában.
- [.NET-keretrendszer 4.6](https://msdn.microsoft.com/library/w0x726c2.aspx) vagy újabb verzióját (az ügyfélszámítógépen).
- [Visual Studio](https://www.visualstudio.com/downloads/download-visual-studio-vs.aspx).



## <a name="create-a-blank-sql-database"></a>Hozzon létre egy üres SQL-adatbázis
1. Jelentkezzen be az [Azure-portálon](https://portal.azure.com/).
2. Kattintson az **Új** > **adatok + tárhely** > **SQL-adatbázishoz**.
3. Hozzon létre egy **üres** adatbázis **klinikán** nevű új vagy meglévő kiszolgálón. Adatbázis létrehozása az Azure-portálon részletes útmutatásért lásd: [percben SQL-adatbázis létrehozása](sql-database-get-started.md).

    ![Üres adatbázis létrehozása](./media/sql-database-always-encrypted/create-database.png)

Az oktatóprogram a később szüksége lesz a kapcsolati karakterlánc. Az adatbázis létrehozása után nyissa meg az új klinikán adatbázist, és másolja a vágólapra a kapcsolati karakterlánc. A kapcsolati karakterlánc bármikor elérheti, de egyszerűen másolja a vágólapra, ha Ön az Azure-portálon.

1. Kattintson az **SQL-adatbázisait** > **klinikán** > **megjelenítése adatbázis kapcsolati karakterláncot**.
2. Másolja a vágólapra a kapcsolati karakterlánc **ADO.NET**.

    ![Másolja a vágólapra a kapcsolati karakterlánc](./media/sql-database-always-encrypted/connection-strings.png)


## <a name="connect-to-the-database-with-ssms"></a>Az adatbázis SSMS csatlakoztatása

Nyissa meg a SSMS, és csatlakozzon a kiszolgálóhoz a klinikán adatbázissal.


1. Nyissa meg a SSMS. (Kattintson a **Csatlakozás** > **Adatbázismotort** a **Csatlakozás a kiszolgálóhoz** ablak megnyitásához, ha még nem nyitott).
2. Írja be a kiszolgáló neve és a hitelesítő adatait. Az SQL-adatbázis lap találhatók a kiszolgáló nevét, és a kapcsolati karakterláncot az előbb másolt. Írja be a teljes kiszolgáló nevét, például *database.windows.net*.

    ![Másolja a vágólapra a kapcsolati karakterlánc](./media/sql-database-always-encrypted/ssms-connect.png)

Ha az **Új tűzfal szabály** ablak nyílik meg, jelentkezzen be az Azure, és SSMS tűzfal új szabály létrehozása a, hogy.


## <a name="create-a-table"></a>Táblázat létrehozása

Ebben a részben létrehoz egy betegek adatokat tároló tábla. Ez lesz az eredetileg – normál táblázat titkosítás a következő szakaszban fog konfigurálása.

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

SSMS tartalmaz egy varázslót, amely egyszerűen állítsa be a mindig titkosított beállításával a CMK CEK és titkosított oszlopok meg.

1. Bontsa ki az **adatbázisok** > **klinikán** > **táblákat**.
2. Kattintson a jobb gombbal a **páciensek** tábla, és válassza az **Oszlopok titkosítása** nyissa meg a titkosított mindig varázslót:

    ![Titkosítsa az oszlopok](./media/sql-database-always-encrypted/encrypt-columns.png)

A titkosított mindig varázsló is tartalmaz, az alábbi szakaszok: **Oszlopos kijelölés**, **Fő kulcs konfigurációs** (CMK), **adatérvényesítési**és **összesítése**.

### <a name="column-selection"></a>Oszlopos kijelölés ###

**Bevezetés** a weblapon, nyissa meg az **Oszlopok kiválasztása** lapon kattintson a **Tovább gombra** . Ezen a lapon, titkosítani kívánt oszlopok kiválasztott [a típusú titkosítást, és milyen oszlop titkosítási kulcs (CEK)](https://msdn.microsoft.com/library/mt459280.aspx#Anchor_2) használni.

Titkosítsa az egyes betegek **Taj-szám** és **SzületésiDátum mező** adatait. A **Taj-szám** oszlopban mérvadó titkosítás, amely támogatja az egyenlő keresések illesztések és a csoportosítás fogja használni. A **SzületésiDátum** oszlop véletlenszerűen titkosítás, amely nem támogatja a műveletek fogja használni.

Állítsa a **Taj-szám** oszlop **Típusú titkosítást** **Deterministic** és az **Randomized** **SzületésiDátum** oszlop. Kattintson a **Tovább**gombra.

![Titkosítsa az oszlopok](./media/sql-database-always-encrypted/column-selection.png)

### <a name="master-key-configuration"></a>Diaminta kulcs konfigurálása###

A **Diaminta kulcs beállítása** lapon pedig a CMK beállítása jelölje ki a fontosabb tár szolgáltató a CMK tárolási helyére. Egy CMK tárolását jelenleg a tanúsítvány a Windows áruházból, Azure kulcs tárolóból elemre vagy hardveres biztonsági modult (HSM). Ebből az oktatóanyagból megtudhatja, hogy miként tárolni a billentyűparancsok a Windows-tárolóban található.

Ellenőrizze, hogy be van jelölve **a Windows áruház-tanúsítvány** , és kattintson a **Tovább**gombra.

![Diaminta kulcs konfigurálása](./media/sql-database-always-encrypted/master-key-configuration.png)


### <a name="validation"></a>Adatérvényesítési###

Az oszlopok most titkosítása, vagy mentse egy PowerShell-parancsprogramot, később futtatni. Ebben az oktatóanyagban válassza a **Folytatás gombra a befejezéshez** , és kattintson a **Tovább**gombra.

### <a name="summary"></a>Összefoglalás###

Győződjön meg róla, hogy a beállításokat az összes javítása, és kattintson a **Befejezés gombra** az mindig titkosított a beállítási folyamat befejezéséhez.

![Összefoglalás](./media/sql-database-always-encrypted/summary.png)


### <a name="verify-the-wizards-actions"></a>A varázsló műveletek ellenőrzése

Miután a varázsló végzett, az adatbázis van beállítva mindig titkosítva. A varázsló által végzett az alábbi műveleteket:

- A létrehozott egy CMK.
- A létrehozott egy CEK.
- A kijelölt oszlopok titkosításhoz beállítva. A **páciensek** tábla jelenleg nem tartalmaz adatokat, de a meglévő adatok a a kijelölt oszlopok most titkosítva van.

Ismételt megjelenítéséhez **klinikán**ellenőrizheti a hívóbetűi SSMS kibocsátása > **biztonsági** > **Mindig a titkosított kulcsok**. Ekkor megjelenik az új kulcsot, amely a varázsló létrehozza a.


## <a name="create-a-client-application-that-works-with-the-encrypted-data"></a>A titkosított adatokkal működő ügyfél-alkalmazás létrehozása

Most, hogy mindig titkosítva van beállítva, létrehozhat egy alkalmazást, amely hajt végre *illeszt be* , és *kiválasztja* a titkosított oszlopok. A minta alkalmazás sikeres futtatásához futtatnia kell azt ugyanazon a számítógépen Ha futtatta a mindig titkosított varázsló. Futtassa az alkalmazást egy másik számítógépen, telepítenie kell a mindig titkosított tanúsítványok az ügyfél alkalmazást futtató számítógép.  

> [AZURE.IMPORTANT] Az alkalmazás [SqlParameter](https://msdn.microsoft.com/library/system.data.sqlclient.sqlparameter.aspx) objektumok kell használnia, ha a kiszolgáló titkosított mindig oszlopokkal egyszerű szöveges adatok átadása. Kivétel eredményezi konstans érték átadása SqlParameter objektumok használata nélkül.


1. Nyissa meg a Visual Studióban, és hozzon létre egy új C# konzol alkalmazást. Győződjön meg arról, hogy a projekt van állítva, **a .NET-keretrendszer 4.6** vagy újabb verziójára.
2. Nevezze el a projekt **AlwaysEncryptedConsoleApp** , és kattintson az **OK gombra**.

![Új New](./media/sql-database-always-encrypted/console-app.png)



## <a name="modify-your-connection-string-to-enable-always-encrypted"></a>A kapcsolati karakterlánc ahhoz, hogy mindig titkosított módosítása

Ez a szakasz ismerteti, hogyan ahhoz, hogy mindig titkosított az adatbázis-kapcsolati karakterláncában megtalálható. Az imént létrehozott "Mindig minta konzolhoz alkalmazás titkosítva.", a következő szakaszban konzolhoz alkalmazás fog módosítása


Ahhoz, hogy mindig titkosított, meg kell az **Oszlopban a titkosítási beállítás** kulcsszó hozzáadása a kapcsolati karakterláncot, és állítsa be **engedélyezve**.

Beállíthatja, hogy ez közvetlenül a kapcsolati karakterláncban, vagy azt is megadhatja ilyenkor egy [SqlConnectionStringBuilder](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnectionstringbuilder.aspx)használatával. A minta alkalmazás a következő szakaszban megtudhatja, hogy hogyan **SqlConnectionStringBuilder**.

> [AZURE.NOTE] Ez a változás csupán titkosított mindig az adott ügyfélalkalmazás szükséges. Egy meglévő alkalmazásban, a kapcsolati karakterlánc külső felekkel tároló (Ez azt jelenti, hogy a konfigurációs fájl), bizonyára, ahhoz, hogy mindig titkosított kódrészleteket módosítása nélkül.


### <a name="enable-always-encrypted-in-the-connection-string"></a>Engedélyezze a mindig titkosított kapcsolati karakterláncban

A következő kulcsszó hozzáadása a kapcsolati karakterláncot:

    Column Encryption Setting=Enabled


### <a name="enable-always-encrypted-with-a-sqlconnectionstringbuilder"></a>Egy SqlConnectionStringBuilder mindig titkosított engedélyezése

A következő kódrészlet szemlélteti, hogyan lehet ahhoz, hogy mindig titkosított [engedélyezve van](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnectioncolumnencryptionsetting.aspx)a [SqlConnectionStringBuilder.ColumnEncryptionSetting](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnectionstringbuilder.columnencryptionsetting.aspx) megadásával.

    // Instantiate a SqlConnectionStringBuilder.
    SqlConnectionStringBuilder connStringBuilder =
       new SqlConnectionStringBuilder("replace with your connection string");

    // Enable Always Encrypted.
    connStringBuilder.ColumnEncryptionSetting =
       SqlConnectionColumnEncryptionSetting.Enabled;



## <a name="always-encrypted-sample-console-application"></a>Mindig titkosított minta New

Ez a példa bemutatja, hogy miként:

- Módosítsa a kapcsolati karakterlánc ahhoz, hogy mindig titkosítva.
- Adatok beillesztése a titkosított oszlopokat.
- Válasszon ki egy rekordot szerinti szűrés egy adott értéket egy titkosított oszlopban.

Cserélje le a **Program.cs** tartalmát a következő kódot. A globális connectionString változó közvetlenül fölötte a fő módszer a sorban a kapcsolati karakterláncot cserélje le a érvényes kapcsolati karakterláncot az Azure portálról. Ez a változás csupán, hogy a kód van szüksége.

Az app futtatásával mindig titkosított működését.

    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Text;
    using System.Threading.Tasks;
    using System.Data;
    using System.Data.SqlClient;

    namespace AlwaysEncryptedConsoleApp
    {
    class Program
    {
        // Update this line with your Clinic database connection string from the Azure portal.
        static string connectionString = @"Replace with your connection string";

        static void Main(string[] args)
        {
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

            InsertPatient(new Patient() {
                SSN = "999-99-0001", FirstName = "Orlando", LastName = "Gee", BirthDate = DateTime.Parse("01/04/1964") });
            InsertPatient(new Patient() {
                SSN = "999-99-0002", FirstName = "Keith", LastName = "Harris", BirthDate = DateTime.Parse("06/20/1977") });
            InsertPatient(new Patient() {
                SSN = "999-99-0003", FirstName = "Donna", LastName = "Carreras", BirthDate = DateTime.Parse("02/09/1973") });
            InsertPatient(new Patient() {
                SSN = "999-99-0004", FirstName = "Janet", LastName = "Gates", BirthDate = DateTime.Parse("08/31/1985") });
            InsertPatient(new Patient() {
                SSN = "999-99-0005", FirstName = "Lucy", LastName = "Harrington", BirthDate = DateTime.Parse("05/06/1993") });


            // Fetch and display all patients.
            Console.WriteLine(Environment.NewLine + "All the records currently in the Patients table:");

            foreach (Patient patient in SelectAllPatients())
            {
                Console.WriteLine(patient.FirstName + " " + patient.LastName + "\tSSN: " + patient.SSN + "\tBirthdate: " + patient.BirthDate);
            }

            // Get patients by SSN.
            Console.WriteLine(Environment.NewLine + "Now let's locate records by searching the encrypted SSN column.");

            string ssn;

            // This very simple validation only checks that the user entered 11 characters.
            // In production be sure to check all user input and use the best validation for your specific application.
            do
            {
                Console.WriteLine("Please enter a valid SSN (ex. 123-45-6789):");
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

Gyorsan ellenőrizheti, hogy a tényleges adatok a kiszolgáló titkosított SSMS **betegek** adatok lekérdezésével. (Használja a meglévő kapcsolatot hol az oszlop titkosítási beállítás még nincs engedélyezve.)

Futtassa a következő lekérdezés klinikán-adatbázisban.

    SELECT FirstName, LastName, SSN, BirthDate FROM Patients;

Láthatja, hogy a titkosított oszlop nem tartalmaz egyszerű szöveges adatokat.

   ![Új New](./media/sql-database-always-encrypted/ssms-encrypted.png)


Egyszerű szöveges adatok eléréséhez SSMS használatához is adhat hozzá a **oszlopot a titkosítási beállítás = engedélyezett** paraméter nélkül a kapcsolatot.

1. SSMS kattintson a jobb gombbal a kiszolgáló az **Objektum Explorerben**, és válassza a **kapcsolat bontása**.
2. Kattintson a **Csatlakozás** > **Adatbázismotort** nyissa meg a **Csatlakozás a kiszolgálóhoz** ablakot, és kattintson a **Beállítások**gombra.
3. Kattintson a **További kapcsolat paramétereket** és típusa **oszlopot a titkosítási beállítás = engedélyezett**.

    ![Új New](./media/sql-database-always-encrypted/ssms-connection-parameter.png)

4. Futtassa a következő lekérdezés **klinikán** -adatbázisban.

        SELECT FirstName, LastName, SSN, BirthDate FROM Patients;

     Ekkor megjelenik a titkosított oszlop egyszerű szöveges adatait.


    ![Új New](./media/sql-database-always-encrypted/ssms-plaintext.png)



> [AZURE.NOTE] Ha egy másik számítógépről SSMS (vagy bármely ügyfél) kapcsolódni, nem érik el a titkosítási kulcs, és nem tudja visszafejteni az adatokat.



## <a name="next-steps"></a>Következő lépések
Miután létrehozott egy használó mindig titkosított adatbázis, érdemes tegye a következőket:

- Futtassa az alábbi példa egy másik számítógépen. Azt nem hozzáférést a titkosítási kulcs, akkor nem lesz hozzáférésük az egyszerű szöveges adatokat, és nem sikerült fog futni.
- [Elforgatás és a billentyűk karbantartása](https://msdn.microsoft.com/library/mt607048.aspx).
- [Adatok, amelyek már titkosított mindig titkosított áttelepítése](https://msdn.microsoft.com/library/mt621539.aspx).
- [A többi ügyfél gépek üzembe mindig titkosított tanúsítványok](https://msdn.microsoft.com/library/mt723359.aspx#Anchor_1) (lásd a "Kezdeményezhet tanúsítványok elérhető az alkalmazások és a felhasználók").

## <a name="related-information"></a>Kapcsolódó információ

- [Mindig titkosított (ügyfél fejlesztés)](https://msdn.microsoft.com/library/mt147923.aspx)
- [Áttetsző adatok titkosítása](https://msdn.microsoft.com/library/bb934049.aspx)
- [SQL Server-titkosítás](https://msdn.microsoft.com/library/bb510663.aspx)
- [Mindig titkosított varázsló](https://msdn.microsoft.com/library/mt459280.aspx)
- [Mindig titkosított Blog](http://blogs.msdn.com/b/sqlsecurity/archive/tags/always-encrypted/)
