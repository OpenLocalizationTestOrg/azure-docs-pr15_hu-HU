<properties
   pageTitle="Adatforrás-kapcsolatok |} Microsoft Azure"
   description="Adatforrás-kapcsolatok adatmodellekben az Azure Analysis Services ismerteti."
   services="analysis-services"
   documentationCenter=""
   authors="minewiskan"
   manager="erikre"
   editor=""
   tags=""/>
<tags
   ms.service="analysis-services"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="na"
   ms.date="10/25/2016"
   ms.author="owend"/>

# <a name="datasource-connections"></a>Adatforrás-kapcsolatok

Adatmodellek az Azure Analysis Services különböző adatszolgáltatók megkövetelheti, bizonyos adatforrások csatlakozáskor. Egyes esetekben csatlakozás adatforrásokhoz, például SQL Server natív ügyfele (SQLNCLI11) natív szolgáltatókkal táblázatos modellekben esetleg hibát jelez.

Példa Ha egy a memóriában vagy közvetlen adatmodellezési, amely csatlakozott Azure SQL-adatbázissal, például a felhőben adatforráshoz eltérő SQLOLEDB natív szolgáltatók használatakor, megjelenhet hibaüzenet jelenik meg: **"A"SQLNCLI11.1"nem regisztrált provider"**.

Vagy, ha rendelkezik a helyszíni adatforrások csatlakoztatása DirectQuery modell natív szolgáltatók használatakor hibaüzenet jelenhet: **"hiba történt az OLE DB-sor készlet létrehozása. Nem a megfelelő szintaxis "Határérték" közeli "**.

## <a name="data-source-providers"></a>Forrás adatszolgáltatók

A következő adatforrás szolgáltatók a memóriában támogatott, vagy irányítsa át a lekérdezést adatmodellek csatlakozáskor a helyszíni, vagy felhőbeli adatforrások:

|               | **Adatforrás**                     | **A memóriában**                            |  **Irányítsa át a lekérdezést**                                           |
|---------------------------|-------------------------------|---------------------------------------------|---------------------------------------------|
| **Felhőalapú**                     | Az SQL Azure-adatraktár      | .NET keretrendszer adatszolgáltatója az SQL Server | .NET keretrendszer adatszolgáltatója az SQL Server |
|                           | Azure SQL-adatbázis            | .NET keretrendszer adatszolgáltatója az SQL Server | .NET keretrendszer adatszolgáltatója az SQL Server |
| **A helyszíni** (keresztül átjáró) | Az SQL Server                    | Az SQL Server natív 11.0 ügyfele               | .NET keretrendszer adatszolgáltatója az SQL Server |
|                           |  Az SQL Server                             | Microsoft OLE DB-szolgáltató az SQL Server    |   .NET keretrendszer adatszolgáltatója az SQL Server                                          |
|                           |  Az SQL Server                             | .NET keretrendszer adatszolgáltatója az SQL Server |  .NET keretrendszer adatszolgáltatója az SQL Server                                           |
|                           | Az Oracle                        | Az Oracle Microsoft OLE DB-szolgáltató        | Oracle-adatszolgáltató a .NET rendszerhez               |
|                           |  Az Oracle                             | Oracle-adatszolgáltató a .NET rendszerhez               | Oracle-adatszolgáltató a .NET rendszerhez                                            |
|                           | Teradata                      | OLE DB-szolgáltató Teradata                | Teradata-adatszolgáltató a .NET rendszerhez             |
|                           |  Teradata                             | Teradata-adatszolgáltató a .NET rendszerhez             |  Teradata-adatszolgáltató a .NET rendszerhez                                            |
|                           | Elemzési rendszer-Platform | .NET keretrendszer adatszolgáltatója az SQL Server | .NET keretrendszer adatszolgáltatója az SQL Server |


> [AZURE.NOTE] Gondoskodjon arról, hogy telepítve vannak a 64 bites szolgáltatók, a helyszíni átjáró használata esetén.

Egy helyszíni SQL Server Analysis Services táblázatos modell áttelepítéséhez Azure Analysis Services rendszerhez, a szolgáltató módosításához szükség lehet.

**Adatforrás-szolgáltatója megadása**

1. A SSDT > **Táblázatos modell Explorer** > **Adatforrásokhoz**, kattintson a jobb gombbal egy adatforrás-kapcsolatot, és kattintson az **Adatforrás szerkesztése**.

2. A **Kapcsolat szerkesztése**párbeszédpanelen kattintson a **Speciális fülre** , a Speciális tulajdonságok ablak megnyitásához.

3. A **Speciális tulajdonságok** > **szolgáltatók**, majd válassza ki a megfelelő szolgáltató.

## <a name="impersonation"></a>Megszemélyesítési
Egyes esetekben lehet szükséges adjon meg egy másik megszemélyesítési fiókot. Megszemélyesítési fiók SSDT vagy SSMS kell megadni.

A helyszíni adatforrások:

- Ha az SQL-hitelesítést használ, a megszemélyesítési szolgáltatásfiók kell lennie.
- Ha Windows-hitelesítéssel, adja meg a Windows felhasználói és jelszavával. Az SQL Server egy adott megszemélyesítési fiókkal a Windows-hitelesítés esetén támogatott csak a memóriában adatmodellek.

Felhőbeli adatforrások:

- Ha az SQL-hitelesítést használ, a megszemélyesítési szolgáltatásfiók kell lennie.


## <a name="next-steps"></a>Következő lépések

Ha a helyszíni adatforrások, feltétlenül telepítse az [átjáró a helyszíni](analysis-services-gateway.md). A kiszolgáló SSDT vagy SSMS kezelésével kapcsolatos további tudnivalókért lásd: [a kiszolgáló kezelése](analysis-services-manage.md).
