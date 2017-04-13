<properties 
   pageTitle="Ismerje meg, adatok tó Analytics és U SQL Azure-portálon interaktív oktatóanyagok segítségével |} Azure" 
   description="Rövid útmutató az adatok tó Analytics és U-SQL nyelvben. " 
   services="data-lake-analytics" 
   documentationCenter="" 
   authors="edmacauley" 
   manager="jhubbard" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-analytics"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="05/16/2016"
   ms.author="edmaca"/>


# <a name="use-azure-data-lake-analytics-interactive-tutorials"></a>Azure adatok tó Analytics interaktív oktatóanyagok használata

Az Azure portál egy interaktív oktatóanyag, hogy az első lépések az adatok tó Analytics. Ez a cikk bemutatja, hogyan az oktatóprogram webhely naplók elemzéséhez folyamatát.


>[AZURE.NOTE]Az azonos oktatóprogram Visual Studio segítségével folyamatát, című témakörben olvashat [elemzés webhely naplók adatok tó Analytics használatával](data-lake-analytics-analyze-weblogs.md).
>További interaktív oktatóanyagok megjelenjen a portálra.


Más oktatóanyagokkal talál:

- [Első lépések az adatok tó Analytics Azure portál használatával](data-lake-analytics-get-started-portal.md)
- [Első lépések az adatok tó Analytics Azure PowerShell használatával](data-lake-analytics-get-started-powershell.md)
- [Első lépések az adatok tó Analytics .NET SDK használatával](data-lake-analytics-get-started-net-sdk.md)
- [Az SQL-U parancsfájlok tó Adateszközök for Visual Studio kidolgozása](data-lake-analytics-data-lake-tools-get-started.md) 

**Előfeltételek**

Ebben az oktatóanyagban megkezdése előtt a következőket kell rendelkeznie:

- **A adatok tó Analytics-fiókot**.  Lásd: az [Első lépések az Azure adatok tó Analytics Azure-portálon](data-lake-analytics-get-started-portal.md).

##<a name="create-data-lake-analytics-account"></a>Adatok tó Analytics-fiók létrehozása 

Adatok tó Analytics-fiókkal kell rendelkeznie, bármilyen feladat futtatása előtt.

Minden adatok tó Analytics- [Azure tó adattár](../data-lake-store/data-lake-store-overview.md) fiók függőség rendelkezik.  Ehhez a fiókhoz tó adattár alapértelmezett fiókként nevezik.  A tó adattár fiók hozhat létre, előre vagy az adatok tó Analytics-fiók létrehozásakor. Ebben az oktatóanyagban a Analytics-fióknak hoz létre a tó adattár fiók

**Adatok tó Analytics-fiók létrehozása**

1. Jelentkezzen az [Azure portálon](https://portal.azure.com/signin/index/?Microsoft_Azure_Kona=true&Microsoft_Azure_DataLake=true&hubsExtension_ItemHideKey=AzureDataLake_BigStorage%2cAzureKona_BigCompute).
2. Nyissa meg a StartBoard bal felső sarokban kattintson **A Microsoft Azure** .
3. Kattintson a **piactér** csempére.  
3. Írja be az **Azure adatok tó Analytics** a Keresés mezőbe, kattintson a **minden** lap, és nyomja le az **ENTER BILLENTYŰT**. **Azure adatok tó Analytics** kell látni a listában.
4. Válassza az **Azure adatok tó analitika** a listából.
5. Kattintson a **Create** a lap alján.
6. Írja be vagy válassza ki a következőket:

    ![Azure adatok tó Analytics portál lap](./media/data-lake-analytics-get-started-portal/data-lake-analytics-portal-create-adla.png)

    - **Név**: a Analytics-fiók neve.
    - **Tó adattár**: minden adat tó Analytics-fiók függő tó adattár fiókkal rendelkezik. Az adatok tó Analytics és a függő tó adattár számlát az azonos Azure adatközpontban kell lennie. Hajtsa végre az utasítás tó adattár új fiók létrehozása, vagy jelöljön ki egy meglévőt.
    - **Előfizetés**: válassza ki a a Analytics-fiókhoz használt Azure előfizetést.
    - **Erőforráscsoport**. Jelölje be a meglévő Azure erőforráscsoport, vagy hozzon létre egy újat. Alkalmazások általában épülnek fel sok összetevőjét, például egy webalkalmazás, adatbázis, adatbázis-kiszolgáló, tárolási és 3 külső szolgáltatásokra. Azure erőforrás-kezelő (ARM) lehetővé teszi az erőforrások csoportként az Azure erőforráscsoport néven az alkalmazás használata. Telepítése, frissítése, figyelésére vagy törlése az összes erőforrás egyetlen, összehangolt műveletben az alkalmazás. Telepítés olyan sablont használ, és a sablonon használhatja például vizsgálat, átmeneti és üzemi különböző környezetekben. Egyértelművé teheti számlázási a szervezet megtekintésével, a teljes csoporton az összegzett költségét. További információ az [Azure erőforrás-kezelő áttekintése](azure-resource-manager/resource-group-overview.md)című témakörben találhat. 
    - **Helyét**. Jelölje ki az adatok tó Analytics-fiók a Azure adatközpont. 
7. Válassza a **Rögzítés a Startboard**. Az ebben az oktatóanyagban számított szükséges.
8. Kattintson a **létrehozása**gombra. Ez megjeleníti a portálon StartBoard. A Kezdőlap lap új csempe bekerül a "Telepíti az Azure adatok tó Analytics" megjelenítő címkével ellátott. Adatok tó Analytics-fiók létrehozása néhány percet vesz igénybe. A fiók létrehozásakor a a portálon a fiók egy új lap nyílik meg.

    ![Azure adatok tó Analytics portál lap](./media/data-lake-analytics-get-started-portal/data-lake-analytics-portal-blade.png)

##<a name="run-website-log-analysis-interactive-tutorial"></a>Webhely napló Analysis interaktív oktatóanyag futtatása

**A webhely napló Analytics interaktív oktatóanyag megnyitása**

1. A portálon kattintson a **Microsoft Azure** kattintva nyissa meg a StartBoard a bal oldali menüben.
2. Kattintson a csatolt adatok tó Analytics-fiókját csempére.
3. Kattintson a **Tallózás interaktív oktatóanyagok** az **Essentials** sávról.

    ![Adatok tó Analytics interaktív oktatóanyagok](./media/data-lake-analytics-use-interactive-tutorials/data-lake-analytics-explore-interactive-tutorials.png)

4. Ha megjelenik egy figyelmeztetés narancssárga jelzi "minták nem beállítása, kattintson a …", másolja a mintaadatokat az alapértelmezett adattár tó fiók **Minta adatok másolása** gombra. Az interaktív oktatóanyag kell az adatokat a futtatására.
5. Kattintson az **Interaktív oktatóanyagok** lap **Webhely napló Analytics**. A portál az oktatóprogram egy új portál lap nyílik meg.
5. Kattintson az **1 – bevezetés** és a képernyőn megjelenő utasításokat

##<a name="see-also"></a>Lásd még:

- [Microsoft Azure adatok tó Analytics áttekintése](data-lake-analytics-overview.md)
- [Első lépések az adatok tó Analytics Azure portál használatával](data-lake-analytics-get-started-portal.md)
- [Első lépések az adatok tó Analytics Azure PowerShell használatával](data-lake-analytics-get-started-powershell.md)
- [Az SQL-U parancsfájlok tó Adateszközök for Visual Studio kidolgozása](data-lake-analytics-data-lake-tools-get-started.md)
- [Azure adatok tó Analytics használatával webhely naplók elemzése](data-lake-analytics-analyze-weblogs.md)
