
<!--
includes/sql-database-include-connection-string-40-config.md

Latest Freshness check:  2015-09-04 , GeneMi.

## Connection string
-->


### <a name="example-config-file-for-connection-string-security"></a>A kapcsolati karakterlánc biztonság példa konfigurációs fájl


A kapcsolati karakterláncot helyezze a C# kód szövegkonstans eredményezné. Célszerű elhelyezni egy konfigurációs fájl a kapcsolati karakterlánc. Nem szerkesztheti a karakterlánc bármikor nélkül szeretné fordítani.

Tegyük fel, hogy a lefordított C# program nevű **ConsoleApplication1.exe**és, hogy a .exe található, a **bin\debug\* * címtár.

Ebben a példában a kapcsolati karakterlánc a legtöbb részei pontosan **ConsoleApplication1.exe.config**nevű config fájlt tárolja. A konfigurációs fájl kell lennie is **bin\debug\**.

A következő konfigurációs fájl XML-a kapcsolati karakterlánc **ConnectionString4NoUserIDNoPassword**nevű láthatók. A C# kód ezt a karakterláncot keres.

A helyőrzők a tényleges nevét kell szerkesztése:

- {your_serverName_here}
- {your_databaseName_here}



        <?xml version="1.0" encoding="utf-8" ?>
        <configuration>
            <startup> 
                <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.5" />
            </startup>
        
            <connectionStrings>
                <clear />
                <add name="ConnectionString4NoUserIDNoPassword"
                providerName="System.Data.ProviderName"
        
                connectionString=
                "Server=tcp:{your_serverName_here}.database.windows.net,1433;
                Database={your_databaseName_here};
                Connection Timeout=30;
                Encrypt=True;
                TrustServerCertificate=False;" />
            </connectionStrings>
        </configuration>



Az ábrán választottunk két paramétert elhagyja:

- Felhasználói azonosító = {your_userName_here};
- Jelszó = {your_password_here};


Is felvehet, de néha célszerűbb szeretné, hogy a program a felhasználó által a billentyűzet beviteli beolvasása a ezeket az értékeket. Attól függ.



<!--
These three includes/ files are a sequenced set, but you can pick and choose:

includes/sql-database-include-connection-string-20-portalshots.md
includes/sql-database-include-connection-string-30-compare.md
includes/sql-database-include-connection-string-40-config.md
-->
