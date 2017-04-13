<properties 
   pageTitle="Azure adatok tó Analytics feladatokhoz U-SQL nyelvben a felhasználó által definiált operátorok kidolgozása |} Azure" 
   description="Útmutató: a felhasználó által definiált operátorok használt, és az adatok tó Analytics feladatok újrafelhasználható fejlesztését. " 
   services="data-lake-analytics" 
   documentationCenter="" 
   authors="edmacauley" 
   manager="jhubbard" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-analytics"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="05/16/2016"
   ms.author="edmaca"/>


# <a name="develop-u-sql-user-defined-operators-for-azure-data-lake-analytics-jobs"></a>A felhasználó által definiált U-SQL nyelvben operátorok Azure adatok tó Analytics feladatokhoz kidolgozása

Útmutató: a felhasználó által definiált operátorok használt, és az adatok tó Analytics feladatok újrafelhasználható fejlesztését. Ország nevek konvertálni kívánt egyéni operátort fog fejlesztését.

##<a name="prerequisites"></a>Előfeltételek

- Visual Studio 2015, a Visual Studio 2013 frissítése 4-es és a Visual Studio 2012 vizuális C++ telepítve van a 
- Microsoft Azure SDK a .NET rendszerhez 2.5-ös verziójának, vagy felett.  Telepítse újra a webes platform installer használatával.
- Adatok tó Analytics-fiók.  Lásd: az [Első lépések az Azure adatok tó Analytics Azure-portálon](data-lake-analytics-get-started-portal.md).
- Az [első lépések az Azure adatok tó Analytics U-SQL nyelvben Studio](data-lake-analytics-u-sql-get-started.md) oktatóprogram folyamatát.
- Azure csatlakozni, lásd: az [első lépések az Azure adatok tó Analytics U-SQL nyelvben Studio](data-lake-analytics-u-sql-get-started.md#connect-to-azure). 
- Töltse fel a forrásadatokat, lásd: az [első lépések az Azure adatok tó Analytics U-SQL nyelvben Studio](data-lake-analytics-u-sql-get-started.md#upload-source-data-files). 

## <a name="define-and-use-user-defined-operator-in-u-sql"></a>Határozza meg, és a felhasználó által definiált operátor használata U SQL

**Hozhat létre, és az SQL-U feladat elküldése** 

1. A **fájl** menüben kattintson az **Új**gombra, és kattintson a **Projekt**.
2. Válassza ki a **Projekt U-SQL nyelvben** .

    ![új U-SQL nyelvben Visual Studio-projekt](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-analytics-data-lake-tools-new-project.png)

3. Kattintson az **OK gombra**. A Visual studio megoldást hoz létre Script.usql-fájl segítségével.
4. A **Megoldás Intézőben**bontsa ki a Script.usql, és kattintson duplán a **Script.usql.cs**.
5. A fájl illessze be a következő kódot:

        using Microsoft.Analytics.Interfaces;
        using System.Collections.Generic;
        
        namespace USQL_UDO
        {
            public class CountryName : IProcessor
            {
                private static IDictionary<string, string> CountryTranslation = new Dictionary<string, string>
                {
                    {
                        "Deutschland", "Germany"
                    },
                    {
                        "Schwiiz", "Switzerland"
                    },
                    {
                        "UK", "United Kingdom"
                    },
                    {
                        "USA", "United States of America"
                    },
                    {
                        "中国", "PR China"
                    }
                };
        
                public override IRow Process(IRow input, IUpdatableRow output)
                {
        
                    string UserID = input.Get<string>("UserID");
                    string Name = input.Get<string>("Name");
                    string Address = input.Get<string>("Address");
                    string City = input.Get<string>("City");
                    string State = input.Get<string>("State");
                    string PostalCode = input.Get<string>("PostalCode");
                    string Country = input.Get<string>("Country");
                    string Phone = input.Get<string>("Phone");
        
                    if (CountryTranslation.Keys.Contains(Country))
                    {
                        Country = CountryTranslation[Country];
                    }
                    output.Set<string>(0, UserID);
                    output.Set<string>(1, Name);
                    output.Set<string>(2, Address);
                    output.Set<string>(3, City);
                    output.Set<string>(4, State);
                    output.Set<string>(5, PostalCode);
                    output.Set<string>(6, Country);
                    output.Set<string>(7, Phone);
        
                    return output.AsReadOnly();
                }
            }
        }

5. Nyissa meg a Script.usql, és illessze be az alábbi U-SQL nyelvben parancsprogramot:

        @drivers =
            EXTRACT UserID      string,
                    Name        string,
                    Address     string,
                    City        string,
                    State       string,
                    PostalCode  string,
                    Country     string,
                    Phone       string
            FROM "/Samples/Data/AmbulanceData/Drivers.txt"
            USING Extractors.Tsv(Encoding.Unicode);
        
        @drivers_CountryName =
            PROCESS @drivers
            PRODUCE UserID string,
                    Name string,
                    Address string,
                    City string,
                    State string,
                    PostalCode string,
                    Country string,
                    Phone string
            USING new USQL_UDO.CountryName();    
        
        OUTPUT @drivers_CountryName
            TO "/Samples/Outputs/Drivers.csv"
            USING Outputters.Csv(Encoding.Unicode);

6. A **Megoldás Intézőben**kattintson a jobb gombbal az **Script.usql**, és kattintson a **Szerkesztés parancsfájl**.
6. A **Megoldás Explorer**kattintson a jobb gombbal az **Script.usql**, és válassza a **Parancsprogram elküldése**.
7. Ha még nem csatlakozott az Azure előfizetés, lesz parancssorba írja be az Azure-fiók hitelesítő adatait.
7. Kattintson a **Küldés**gombra. Beküldési eredmények és a feladat hivatkozás érhetők el az eredmények ablakában a beküldött befejeztével.
8. A frissítés gombra kattintva megtekintheti a legújabb feladat állapotát és a képernyő frissítése kell kattintania.

**A feladat kimenet megjelenítéséhez**

1. A **Kiszolgáló Intézőben**bontsa ki az **Azure**, bontsa ki az **Adatok tó Analytics**, bontsa ki az adatok tó Analytics-fiókját, bontsa ki a **Tárterület-fiókok**, kattintson a jobb gombbal az alapértelmezett tárolására és **Explorer**parancsra. 
2. Bontsa ki a minták, bontsa ki a kimeneti értékeket, és kattintson duplán a **Drivers.csv**.


##<a name="see-also"></a>Lásd még:

- [Első lépések az adatok tó Analytics PowerShell használatával](data-lake-analytics-get-started-powershell.md)
- [Első lépések az adatok tó Analytics az Azure portál használatával](data-lake-analytics-get-started-portal.md)
- [Visual Studio U – SQL-alkalmazások fejlesztésével tó Adateszközök használata](data-lake-analytics-data-lake-tools-get-started.md)
