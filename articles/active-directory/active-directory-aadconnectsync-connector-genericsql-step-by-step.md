<properties
   pageTitle="Általános SQL-összekötő lépés által lépés |} Microsoft Azure"
   description="Ez a cikk az útmutató, egy egyszerű HR rendszer lépésenkénti használata az általános SQL-összekötő alapján."
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

# <a name="generic-sql-connector-step-by-step"></a>Általános SQL összekötő lépésről lépésre
Ez a témakör részletes útmutatásra. Egy egyszerű példa HR adatbázist hoz létre és a importáló bizonyos felhasználók és a csoporttagság használható.

## <a name="prepare-the-sample-database"></a>A mintavállalati adatbázis előkészítése
Az SQL Servert futtató-kiszolgálón futtatása SQL [mintája](#appendix-a)található. Ez a parancsfájl mintaadatbázis GSQLDEMO nevű hoz létre. A létrehozott adatbázisok az objektummodell néz ki a képet:  
![Objektummodell](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\objectmodel.png)

Az adatbázishoz való csatlakozáshoz használni kívánt felhasználó is létrehozhat. Az útmutató a felhasználó FABRIKAM\SQLUser néven, és a tartományban található.

## <a name="create-the-odbc-connection-file"></a>ODBC-kapcsolatfájl létrehozása
Az általános SQL-összekötő használatával, hogy a távoli kiszolgáló csatlakozik a ODBC. Először szükség hozzon létre egy fájlt az ODBC-kapcsolat adatait.

1. Indítsa el az ODBC-kezelés segédprogramot a kiszolgálón:  
![ODBC](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\odbc.png)
2. Kattintson a **Fájl DSN**fülre. Kattintson a **Hozzáadás**gombra.
![ODBC1](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\odbc1.png)
3. A kimenő kész illesztőprogram dolgozik finom, így jelölje ki azt, és kattintson a **Tovább >**.  
![ODBC2](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\odbc2.png)
4. Nevezze el a fájlt, például **GenericSQL**.  
![ODBC3](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\odbc3.png)
5. Kattintson a **Befejezés gombra**.  
![ODBC4](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\odbc4.png)
6. A kapcsolat konfigurálásához ideje. Írja be az adatforrás megfelelő leírását, és az SQL Server szolgáltatást futtató kiszolgáló neveként.  
![ODBC5](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\odbc5.png)
7. Válassza ki, hogyan SQL hitelesítést végezni. Ebben az esetben a Windows-hitelesítés használjuk.  
![ODBC6](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\odbc6.png)
8. Adja meg a mintaadatbázisban **GSQLDEMO**nevét.  
![ODBC7](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\odbc7.png)
9. Mindent a képernyőn megjelenő alapértelmezett megőrzése. Kattintson a **Befejezés gombra**.  
![ODBC8](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\odbc8.png)
10. Annak ellenőrzéséhez, hogy minden várt módon működik, kattintson a **Próba-adatforráshoz**.  
![ODBC9](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\odbc9.png)
11. Ellenőrizze a vizsgálat sikeres.  
![ODBC10](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\odbc10.png)
12. ODBC-konfigurációs fájl megjelenik a fájl DSN.  
![ODBC11](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\odbc11.png)

Most már van a fájlt, hogy van szükség, és az összekötő kezdeni.

## <a name="create-the-generic-sql-connector"></a>Az általános SQL-összekötő létrehozása

1. A szinkronizálás szolgáltatáskezelő felhasználói felületének kijelölése **összekötők** és **létrehozása** Jelölje ki az **Általános SQL (Microsoft)** , és adjon meg egy jól megjegyezhető nevet.  
![Connector1](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\connector1.png)
2. Az előző részben létrehozott DSN-fájlt, és töltse fel a kiszolgálóra. Adja meg a hitelesítő adatok az adatbázishoz való csatlakozáshoz.  
![Connector2](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\connector2.png)
3. Az útmutató hogy vannak ezzel leegyszerűsítve nekünk, és mondja ki, hogy vannak-e két objektum típusa, a **felhasználók** és **csoportok**.
![Connector3](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\connector3.png)
4. Az attribútumok megkereséséhez szeretnénk e attribútumok feltárása magát a táblát megjeleníti az összekötőt. Mivel a **felhasználók** az SQL foglalt szó, adja meg azt a szögletes zárójelek [] szükség.  
![Connector4](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\connector4.png)
5. Adja meg a horgony attribútum és a DN attribútum időt. A **felhasználók**, ha a két attribútum felhasználónév Alkalmazottkód használjuk. **Csoport**csoportnév használjuk (nem reálisan valós életben, de ez a forgatókönyv működik).
![Connector5](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\connector5.png)
6. Nem az összes attribútum típusok észlelhető SQL-adatbázisban. A hivatkozástípus attribútum különösen nem. Az objektumtípus csoport azt módosítania kell a OwnerID és MemberID hivatkozni szeretne.  
![Connector6](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\connector6.png)
7. A azt ki, mint az előző lépésben hivatkozás attribútumok csak az objektum típusa ezeket az értékeket attribútumok mutató hivatkozás. Ebben az esetben a User objektumban írja be.  
![Connector7](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\connector7.png)
8. A globális paraméterek lapon jelölje be a delta stratégia **vízjel** . A dátum/idő formátum **yyyy-MM-dd óó**is beírhat.
![Connector8](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\connector8.png)
9. A **partíciók konfigurálása és a hierarchiák** lapján jelölje be a két objektumtípusok.
![Connector9](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\connector9.png)
10. **Jelölje be az objektum típusa** és **Attribútumok jelölje ki**jelölje ki az összes attribútumok és a objektumtípusok. A **Horgonyok konfigurálása** lapon kattintson a **Befejezés gombra**.

## <a name="create-run-profiles"></a>Futási profilok létrehozása

1. A szinkronizálás szolgáltatáskezelő felhasználói felületének kijelölése **összekötők**, és **Állítsa be a profilok futtatása** Kattintson az **Új profil**. Akkor indítsa el **a teljes importálás**.  
![Runprofile1](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\runprofile1.png)
2. Válassza ki a **Teljes importálás (csak terület)**.  
![Runprofile2](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\runprofile2.png)
3. Válassza ki a partíciót **objektum = felhasználó**.  
![Runprofile3](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\runprofile3.png)
4. Jelölje ki a **táblát** , és írja be a **[felhasználók]**. Görgessen le a többértékű objektum típusa csoportban, és írja be az adatokat, ahogy az alábbi ábrán. Jelölje ki a mentheti a lépés **befejezése** .
![Runprofile4a](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\runprofile4a.png)  
![Runprofile4b](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\runprofile4b.png)  
5. Jelölje be az **új lépést**. Ebben az esetben válassza **objektum = csoport**. Utolsó lapján ahogy az alábbi képen a konfiguráció használata Kattintson a **Befejezés gombra**.  
![Runprofile5a](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\runprofile5a.png)  
![Runprofile5b](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\runprofile5b.png)  
6. Nem kötelező: Ha azt szeretné, hogy, beállíthatja további futtatása profilok. Ez a forgatókönyv csak a teljes importálás használatos.
7. Kattintson az **OK** futtatása profilok módosításának befejezéséhez.

## <a name="add-some-test-data-and-test-the-import"></a>Adatok vizsgálata és tesztelje az importálás hozzáadása
Töltse ki a mintaadatbázisban vizsgált adatokat. Ha készen áll, jelölje be a **futtatása** és a **teljes importálás**.

Az alábbiakban két telefonszámok rendelkező felhasználó és az egyes tagok csoport.  
![cs1](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\cs1.png)  
![CS2](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\cs2.png)  

## <a name="appendix-a"></a>Mintája
**A mintavállalati adatbázis létrehozása az SQL-parancsprogram**

```SQL
---Creating the Database---------
Create Database GSQLDEMO
Go
-------Using the Database-----------
Use [GSQLDEMO]
Go
-------------------------------------
USE [GSQLDEMO]
GO
/****** Object:  Table [dbo].[GroupMembers]   ******/
SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
CREATE TABLE [dbo].[GroupMembers](
    [MemberID] [int] NOT NULL,
    [Group_ID] [int] NOT NULL
) ON [PRIMARY]

GO
/****** Object:  Table [dbo].[GROUPS]   ******/
SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
CREATE TABLE [dbo].[GROUPS](
    [GroupID] [int] NOT NULL,
    [GROUPNAME] [nvarchar](200) NOT NULL,
    [DESCRIPTION] [nvarchar](200) NULL,
    [WATERMARK] [datetime] NULL,
    [OwnerID] [int] NULL,
PRIMARY KEY CLUSTERED
(
    [GroupID] ASC
)WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON) ON [PRIMARY]
) ON [PRIMARY]

GO
/****** Object:  Table [dbo].[USERPHONE]   ******/
SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
SET ANSI_PADDING ON
GO
CREATE TABLE [dbo].[USERPHONE](
    [USER_ID] [int] NULL,
    [Phone] [varchar](20) NULL
) ON [PRIMARY]

GO
SET ANSI_PADDING OFF
GO
/****** Object:  Table [dbo].[USERS]   ******/
SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
CREATE TABLE [dbo].[USERS](
    [USERID] [int] NOT NULL,
    [USERNAME] [nvarchar](200) NOT NULL,
    [FirstName] [nvarchar](100) NULL,
    [LastName] [nvarchar](100) NULL,
    [DisplayName] [nvarchar](100) NULL,
    [ACCOUNTDISABLED] [bit] NULL,
    [EMPLOYEEID] [int] NOT NULL,
    [WATERMARK] [datetime] NULL,
PRIMARY KEY CLUSTERED
(
    [USERID] ASC
)WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON) ON [PRIMARY]
) ON [PRIMARY]

GO
ALTER TABLE [dbo].[GroupMembers]  WITH CHECK ADD  CONSTRAINT [FK_GroupMembers_GROUPS] FOREIGN KEY([Group_ID])
REFERENCES [dbo].[GROUPS] ([GroupID])
GO
ALTER TABLE [dbo].[GroupMembers] CHECK CONSTRAINT [FK_GroupMembers_GROUPS]
GO
ALTER TABLE [dbo].[GroupMembers]  WITH CHECK ADD  CONSTRAINT [FK_GroupMembers_USERS] FOREIGN KEY([MemberID])
REFERENCES [dbo].[USERS] ([USERID])
GO
ALTER TABLE [dbo].[GroupMembers] CHECK CONSTRAINT [FK_GroupMembers_USERS]
GO
ALTER TABLE [dbo].[GROUPS]  WITH CHECK ADD  CONSTRAINT [FK_GROUPS_USERS] FOREIGN KEY([OwnerID])
REFERENCES [dbo].[USERS] ([USERID])
GO
ALTER TABLE [dbo].[GROUPS] CHECK CONSTRAINT [FK_GROUPS_USERS]
GO
ALTER TABLE [dbo].[USERPHONE]  WITH CHECK ADD  CONSTRAINT [FK_USERPHONE_USER] FOREIGN KEY([USER_ID])
REFERENCES [dbo].[USERS] ([USERID])
GO
ALTER TABLE [dbo].[USERPHONE] CHECK CONSTRAINT [FK_USERPHONE_USER]
GO
```
