<properties
    pageTitle="Oktatóprogram: Web App alkalmazásban a többszörös bérlői-adatbázisokkal entitás keretrendszer és a sor szintű biztonság"
    description="Megtudhatja, hogy miként kidolgozása egy ASP.NET MVC 5 web App alkalmazásban a többszörös bérlő SQL-adatbázisokkal backent, sor szintű biztonság és a szervezet keretében."
  metaKeywords="azure asp.net mvc entity framework multi tenant row level security rls sql database"
    services="app-service\web"
    documentationCenter=".net"
    manager="jeffreyg"
  authors="tmullaney"/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="04/25/2016"
    ms.author="thmullan"/>

# <a name="tutorial-web-app-with-a-multi-tenant-database-using-entity-framework-and-row-level-security"></a>Oktatóprogram: Web App alkalmazásban a többszörös bérlői-adatbázisokkal entitás keretrendszer és a sor szintű biztonság

Ebből az oktatóanyagból megtudhatja, hogy miként összeállítása és a "[megosztott adatbázishoz, megosztott séma](https://msdn.microsoft.com/library/aa479086.aspx)" bérleti modellt, a szervezet keretrendszer és [Sor szintű biztonsági](https://msdn.microsoft.com/library/dn765131.aspx)több bérlői webalkalmazást. Ebben a modellben egy adatbázis több bérlők adatait tartalmazza, és minden tábla minden egyes sorához tartozik egy "bérlői azonosítót." Sor szintű biztonsági (RLS) egy új szolgáltatást Azure SQL-adatbázis eléréséhez egymás bérlők megakadályozása szolgál. Ehhez csak egyetlen, kis módosításának az alkalmazást. A bérlői access logika magában az adatbázisban összegyűjtő, RLS leegyszerűsíti az alkalmazás kódja és csökkenti a bérlők közötti véletlen adatok szivárgás.

Kezdjük az egyszerű Contact Manager alkalmazás [auth és SQL-adatbázis-ASP.NET MVP-alkalmazás létrehozása, és telepítse az Azure alkalmazás szolgáltatás](web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database.md). Jobb most az alkalmazás lehetővé teszi, hogy minden felhasználó (bérlők) az összes névjegy megtekintéséhez:

![Contact Manager alkalmazás RLS történő engedélyezése előtt:](./media/web-sites-dotnet-entity-framework-row-level-security/ContactManagerApp-Before.png)

Néhány kis módosításokkal hozzáadunk több bérleti, támogatása, hogy a felhasználók csak a hozzájuk tartozó partnereket látja.

## <a name="step-1-add-an-interceptor-class-in-the-application-to-set-the-sessioncontext"></a>Lépés: 1: Hozzáadása egy elfogó osztály az alkalmazásban a SESSION_CONTEXT beállítása

Egy alkalmazás módosítása, hogy szükség van. Az összes alkalmazás felhasználóinak csatlakozni ugyanazt a kapcsolati karakterláncot (vagyis egy SQL-bejelentkezés) az adatbázist, mert jelenleg nincs lehetőség, hogy mely felhasználói szűrő kell egy RLS házirend. Ezt a megközelítést oka gyakori webalkalmazásokban hatékony kapcsolatkészletezés lehetővé teszi, de ez azt jelenti, hogy úgy is az adatbázis belül az aktuális alkalmazás felhasználó azonosítása szükséges. A megoldás, hogy az alkalmazás kulcs-érték két beállítása az aktuális felhasználóazonosító a [SESSION_CONTEXT](https://msdn.microsoft.com/library/mt590806) a közvetlenül a kapcsolat, megnyitása, mielőtt bármilyen lekérdezések végrehajtása után. SESSION_CONTEXT a munkamenet – munkafüzetszintű kulcs-érték áruházból, és RLS szabályzatunk fogja használni a felhasználóazonosító benne tárolt azonosítható az aktuális felhasználó.

Hozzáadunk egy [elfogó](https://msdn.microsoft.com/data/dn469464.aspx) (különösen a [DbConnectionInterceptor](https://msdn.microsoft.com/library/system.data.entity.infrastructure.interception.idbconnectioninterceptor)), egy új szolgáltatást a szervezet keretrendszer (EF) 6 automatikusan beállítani az aktuális felhasználóazonosító a SESSION_CONTEXT a T-SQL utasítás végrehajtása, valahányszor EF megnyit egy kapcsolatot.

1.  Nyissa meg a ContactManager projekt Visual Studio.
2.  Kattintson a jobb gombbal a mappára a megoldást Intéző modellek, és válassza a Hozzáadás > osztály.
3.  Nevezze el az új osztály "SessionContextInterceptor.cs", és kattintson a Hozzáadás gombra.
4.  Cserélje le a SessionContextInterceptor.cs tartalmát a következő kódot.

```
using System;
using System.Collections.Generic;
using System.Linq;
using System.Web;
using System.Data.Common;
using System.Data.Entity;
using System.Data.Entity.Infrastructure.Interception;
using Microsoft.AspNet.Identity;

namespace ContactManager.Models
{
    public class SessionContextInterceptor : IDbConnectionInterceptor
    {
        public void Opened(DbConnection connection, DbConnectionInterceptionContext interceptionContext)
        {
            // Set SESSION_CONTEXT to current UserId whenever EF opens a connection
            try
            {
                var userId = System.Web.HttpContext.Current.User.Identity.GetUserId();
                if (userId != null)
                {
                    DbCommand cmd = connection.CreateCommand();
                    cmd.CommandText = "EXEC sp_set_session_context @key=N'UserId', @value=@UserId";
                    DbParameter param = cmd.CreateParameter();
                    param.ParameterName = "@UserId";
                    param.Value = userId;
                    cmd.Parameters.Add(param);
                    cmd.ExecuteNonQuery();
                }
            }
            catch (System.NullReferenceException)
            {
                // If no user is logged in, leave SESSION_CONTEXT null (all rows will be filtered)
            }
        }
        
        public void Opening(DbConnection connection, DbConnectionInterceptionContext interceptionContext)
        {
        }

        public void BeganTransaction(DbConnection connection, BeginTransactionInterceptionContext interceptionContext)
        {
        }

        public void BeginningTransaction(DbConnection connection, BeginTransactionInterceptionContext interceptionContext)
        {
        }

        public void Closed(DbConnection connection, DbConnectionInterceptionContext interceptionContext)
        {
        }

        public void Closing(DbConnection connection, DbConnectionInterceptionContext interceptionContext)
        {
        }

        public void ConnectionStringGetting(DbConnection connection, DbConnectionInterceptionContext<string> interceptionContext)
        {
        }

        public void ConnectionStringGot(DbConnection connection, DbConnectionInterceptionContext<string> interceptionContext)
        {
        }

        public void ConnectionStringSet(DbConnection connection, DbConnectionPropertyInterceptionContext<string> interceptionContext)
        {
        }

        public void ConnectionStringSetting(DbConnection connection, DbConnectionPropertyInterceptionContext<string> interceptionContext)
        {
        }

        public void ConnectionTimeoutGetting(DbConnection connection, DbConnectionInterceptionContext<int> interceptionContext)
        {
        }

        public void ConnectionTimeoutGot(DbConnection connection, DbConnectionInterceptionContext<int> interceptionContext)
        {
        }

        public void DataSourceGetting(DbConnection connection, DbConnectionInterceptionContext<string> interceptionContext)
        {
        }

        public void DataSourceGot(DbConnection connection, DbConnectionInterceptionContext<string> interceptionContext)
        {
        }

        public void DatabaseGetting(DbConnection connection, DbConnectionInterceptionContext<string> interceptionContext)
        {
        }

        public void DatabaseGot(DbConnection connection, DbConnectionInterceptionContext<string> interceptionContext)
        {
        }

        public void Disposed(DbConnection connection, DbConnectionInterceptionContext interceptionContext)
        {
        }

        public void Disposing(DbConnection connection, DbConnectionInterceptionContext interceptionContext)
        {
        }

        public void EnlistedTransaction(DbConnection connection, EnlistTransactionInterceptionContext interceptionContext)
        {
        }

        public void EnlistingTransaction(DbConnection connection, EnlistTransactionInterceptionContext interceptionContext)
        {
        }

        public void ServerVersionGetting(DbConnection connection, DbConnectionInterceptionContext<string> interceptionContext)
        {
        }

        public void ServerVersionGot(DbConnection connection, DbConnectionInterceptionContext<string> interceptionContext)
        {
        }

        public void StateGetting(DbConnection connection, DbConnectionInterceptionContext<System.Data.ConnectionState> interceptionContext)
        {
        }

        public void StateGot(DbConnection connection, DbConnectionInterceptionContext<System.Data.ConnectionState> interceptionContext)
        {
        }
    }

    public class SessionContextConfiguration : DbConfiguration
    {
        public SessionContextConfiguration()
        {
            AddInterceptor(new SessionContextInterceptor());
        }
    }
}
```

Ez az alkalmazás csak módosítás szükséges. Lépjen tovább és készítése és közzététele az alkalmazás.

## <a name="step-2-add-a-userid-column-to-the-database-schema"></a>Lépés: 2: Az adatbázissémát felhasználóazonosító oszlop hozzáadása

Ezután szükség felhasználóazonosító oszlop hozzáadása a névjegyek táblázat minden egyes sorára társítása egy felhasználói (bérlő esetében). A séma közvetlenül az adatbázisban, azt módosítja, így azt nem kell a EF adatmodellt ebben a mezőben felvenni.

Az adatbázis közvetlenül kapcsolódik, használja az SQL Server Management Studio és a Visual Studióban, és ezután hajtsa végre az alábbi T SQL:

```
ALTER TABLE Contacts ADD UserId nvarchar(128)
    DEFAULT CAST(SESSION_CONTEXT(N'UserId') AS nvarchar(128))
```

Felhasználóazonosító oszlop hozzáadása a névjegyek táblázat. A nvarchar(128) adattípus használjuk a UserIds a AspNetUsers táblában tárolt megfelelően, és hozzunk létre, amely automatikusan beállítja a SESSION_CONTEXT jelenleg tárolt felhasználói azonosítóval kell az újonnan beszúrt sorok felhasználói azonosítóval alapértelmezett kényszer.

Ekkor a táblázatot néz ki:

![SSMS névjegyek táblázat](./media/web-sites-dotnet-entity-framework-row-level-security/SSMS-Contacts.png)

Új kapcsolatok létrehozásakor ezeket fog automatikusan hozzárendeli a megfelelő felhasználói azonosítóval. Bemutató céljára azonban érdemes hozzárendelése ezeket a meglévő névjegyeket néhány meglévő felhasználó.

Ha létrehozta az alkalmazás már néhány felhasználót (például helyi használatával, a Google vagy a Facebook fiókok), a AspNetUsers táblázatban láthatók. Az alábbi képernyőképen van csak egy felhasználó az eddigi.

![SSMS AspNetUsers táblázat](./media/web-sites-dotnet-entity-framework-row-level-security/SSMS-AspNetUsers.png)

Másolja az azonosító user1@contoso.com, és illessze be az alábbi T-SQL-utasítást. A névjegyek közül három társítani a felhasználóazonosító a utasítás végrehajtása.

```
UPDATE Contacts SET UserId = '19bc9b0d-28dd-4510-bd5e-d6b6d445f511'
WHERE ContactId IN (1, 2, 5)
```

## <a name="step-3-create-a-row-level-security-policy-in-the-database"></a>Lépés 3: Sor szintű biztonsági házirend létrehozása az adatbázisban

Az utolsó lépésként hozzon létre egy biztonsági házirendet, amely SESSION_CONTEXT a felhasználóazonosító használatával automatikusan a lekérdezés által visszaadott találatok szűrése.

Miközben továbbra is csatlakozik az adatbázishoz, hajtsa végre az alábbi T SQL:

```
CREATE SCHEMA Security
go

CREATE FUNCTION Security.userAccessPredicate(@UserId nvarchar(128))
    RETURNS TABLE
    WITH SCHEMABINDING
AS
    RETURN SELECT 1 AS accessResult
    WHERE @UserId = CAST(SESSION_CONTEXT(N'UserId') AS nvarchar(128))
go

CREATE SECURITY POLICY Security.userSecurityPolicy
    ADD FILTER PREDICATE Security.userAccessPredicate(UserId) ON dbo.Contacts,
    ADD BLOCK PREDICATE Security.userAccessPredicate(UserId) ON dbo.Contacts
go

```

Ez a kód háromféleképpen tehet tartalmaz. Első lépésként hoz létre egy új séma összegyűjtő és az RLS objektumokhoz való hozzáférés korlátozása a legjobb módszer. Ezután hoz létre, amely visszaadja a "1" egy sor a felhasználóazonosító a felhasználóazonosító SESSION_CONTEXT a megfelelő predikátumalakzat függvény. Végül egy biztonsági házirendet, amely hozzáadja ezt a funkciót, a névjegyek táblázatban a szűrés és a blokk predikátumok hoz létre. A szűrő predikátumok hatására a lekérdezésekben való visszatéréshez csak az aktuális felhasználóhoz tartozó sorokat, és a továbbfejlesztett fájlblokkolás predikátumok működik-e a védelmét, hogy az alkalmazás minden eddiginél véletlenül sorbeszúrás a megfelelő felhasználó számára.

Most, hogy futtassa az alkalmazást, és jelentkezzen be, mint user1@contoso.com. A felhasználó most már láthatja a csak az a partnereket, hogy rendelt a felhasználóazonosító korábbi:

![Contact Manager alkalmazás RLS történő engedélyezése előtt:](./media/web-sites-dotnet-entity-framework-row-level-security/ContactManagerApp-After.png)

A további, próbálkozzon egy új felhasználó regisztrálása érvényesítéséhez. Mivel nincs rájuk feladatot kapott nem partnerek, megjelenik számukra. Ha az azok új névjegyet, rendeli őket hozzá, és csak lesz láthatja.

## <a name="next-steps"></a>Következő lépések

Az egész! Az egyszerű Contact Manager webalkalmazás több elem bérlői webhelyet, ahol minden felhasználó rendelkezik-e saját névjegylista egy lett alakítva. Sor szintű biztonsági használatával azt már kerülni az alkalmazás kódja bérlői access logikája kényszerítése komplexitását. Ez az áttetszőség lehetővé teszi, hogy az alkalmazás a valós üzleti probléma kéznél kiemelése és csökkenthető a kockázat véletlenül httpserversockethandlers adatok, mint az alkalmazás a kód helye a megnő.

Ebben az oktatóanyagban van mondtam az, hogy mi mindenre jó RLS. Például lehetséges van további kifinomult vagy részletes access logika és lehet tárolni a SESSION_CONTEXT nem egyszerűen csak az aktuális felhasználói azonosítóval. Hogy az is lehetséges [a a rugalmas adatbázis eszközök ügyfél tárak RLS](../sql-database/sql-database-elastic-tools-multi-tenant-row-level-security.md) integrálása a több elem bérlői shards támogatási egy méretezési adatok réteg.

Ezek a lehetőségek túl is dolgozunk a probléma, hogy a RLS még jobb. Ha kérdése van, ötletek vagy szeretné látni, hogy elemet tudassa velünk a megjegyzések. Nagyra értékeljük a visszajelzését!
