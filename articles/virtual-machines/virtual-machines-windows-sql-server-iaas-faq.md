<properties
    pageTitle="SQL Server Azure virtuális gépeken gyakran ismételt kérdések |} Microsoft Azure"
    description="Ebben a cikkben futó SQL Server Azure VMs kapcsolatos gyakori kérdésekre adott válaszok."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="v-shysun"
    manager="felixwu"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="09/13/2016"
    ms.author="v-shysun"/>

# <a name="sql-server-on-azure-virtual-machines-faq"></a>SQL Server Azure virtuális gépeken – gyakori kérdések

Ez a témakör válaszokat ad [A Azure virtuális gépeken futó SQL Server](https://azure.microsoft.com/services/virtual-machines/sql-server/)futtatásával kapcsolatos leggyakoribb kérdésekre.

[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="frequently-asked-questions"></a>Gyakori kérdések

1. **Hogyan hozhatok létre az Azure virtuális gép SQL Server?**

    Ennek két módja van. A legegyszerűbb megoldás, egy virtuális gép, amely tartalmazza az SQL Server létrehozása. Az Azure fizet elő, és egy SQL virtuális létrehozása a portálról oktatóanyagban [rendelkezni az Azure-portálon SQL Server virtuális gép](virtual-machines-windows-portal-sql-server-provision.md)című témakör tartalmaz. Akkor is, hogy manuálisan az SQL Server telepítése egy virtuális és újbóli felhasználása egy helyszíni licenc [licenc](https://azure.microsoft.com/pricing/license-mobility/)mozgásukban Azure a frissítési garancia keresztül.

1. **Mi a különbség az SQL-VMs és az SQL-adatbázis-szolgáltatás között?**

    Ebben az értelemben futó SQL Server Azure virtuális-gépen különbözik nem, hogy az SQL Servert futtató egy távoli adatközpontban. [SQL-adatbázis](../sql-database/sql-database-technical-overview.md) viszont kínál a adatbázis-mint-a-szolgáltatás. SQL-adatbázissal nem rendelkezik a gép, amely az adatbázisok tárolni a hozzáférést. Lásd: a teljes összehasonlítása érdekében [Válassza az SQL Server lehetőség felhő: (PaaS) Azure SQL-adatbázis vagy SQL Server Azure VMs (IaaS)](../sql-database/sql-database-paas-vs-sql-server-iaas.md).

1. **Hogyan tudom áttelepíteni a helyszíni SQL Server-adatbázis a felhőbe?**

    Először létrehozása az Azure virtuális gép SQL Server-példányt. Ezután áttelepítése a helyszíni adatbázisok példányhoz. Adatok áttelepítési stratégiák [SQL Server-Azure virtuális az SQL Server-adatbázis áttelepítése](virtual-machines-windows-migrate-sql.md)című témakör tartalmaz.

2. **A telepített szolgáltatások módosítása vagy a másodfokú SQL Server telepítése a azonos virtuális?**

    igen. Az SQL Server telepítési adathordozóját a **C** meghajtó mappájában található. Futtassa a **Setup.exe** helyről kapja új SQL Server-példányok hozzáadásához vagy módosításához a számítógépen az SQL Server egyéb telepített szolgáltatások.

3. **Hogyan frissíthetem egy új verziójú/kiadását az SQL Server-Azure virtuális?**

    Jelenleg nem helybeni az SQL Server-Azure virtuális futó. Hozzon létre egy új Azure virtuális gép a kívánt SQL Server verzió/kiadásával, és kattintson az adatbázisok áttelepítése az új kiszolgáló szabványos [adatok áttelepítési módszerek](virtual-machines-windows-migrate-sql.md)segítségével.

4. **Hogyan telepíthetem a licencelt SQL Server-Azure virtuális?**

    Az SQL Server telepítési adathordozóját másolása a Windows Server virtuális, és kattintson az SQL Server telepítése a virtuális. Okok licencelésének, [Licenc mobilitás Azure a frissítési garancia keresztül](https://azure.microsoft.com/pricing/license-mobility/)kell rendelkeznie.

5. **Meg kell fizetnie egy virtuális SQL költsége, ha csak használja készenléti/feladatátvételi?**

    A SQL virtuális keresztül a gyűjtemény hoz létre, ha rendelkeznie kell a készenléti SQL virtuális külön licenc, és a árak megegyezik. Ha telepíti az SQL Server manuálisan [Licenc](https://azure.microsoft.com/pricing/license-mobility/)mozgásukban virtuális gépen, lehetősége van egy ingyenes passzív SQL-példány feladatátvételi. Véleményezze a következőt a korlátozásokat és követelményeknek.

6. **Hogyan frissítések és szervizcsomagok alkalmaznak egy SQL Server virtuális?**

    Virtuális gépeken futó segítségével meghatározhatja az állomásgép, beleértve a mikor és hogyan frissítések telepítését. Az operációs rendszer windows-frissítések manuális alkalmazhat, és engedélyezheti az [Automatikus javítási](virtual-machines-windows-classic-sql-automated-patching.md)nevű ütemezési szolgáltatás. Automatikus javítás telepítési helyet frissítéseiről, fontos, köztük az SQL Server-frissítések kategóriákba tartozó megjelölt. Az SQL Server választható frissítések manuálisan kell telepíteni.

7. **Van erre lehetőség nem jelenik meg a virtuális gépek galéria (a példában a Windows 2008 R2 + SQL Server 2012) konfigurációk beállításához?**

    nem. Az SQL Server rendszerből gyűjtemény képek virtuális gép választania kell a megadott képek közül.

9. **Hogyan telepíthetem az SQL Adateszközök az Azure virtuális?**

    Töltse le és telepítse [A Microsoft SQL Server Data Tools – Business Intelligence for Visual Studio 2013 ](https://www.microsoft.com/en-us/download/details.aspx?id=42313)SQL Adateszközök.

## <a name="resources"></a>Erőforrások

Megtekintés az áttekintést kaphat a Azure virtuális gépeken futó SQL Server [Azure virtuális az SQL Server 2016 a legjobb platform](https://channel9.msdn.com/Events/DataDriven/SQLServer2016/Azure-VM-is-the-best-platform-for-SQL-Server-2016)videó. A témakör a [Áttekintése Azure virtuális gépeken futó SQL Server](virtual-machines-windows-sql-server-iaas-overview.md)egy jó – bevezetés is megnyithatja.

Egyéb források a következők:

- [Az SQL Server virtuális gép az Azure-portálon kiépítése](virtual-machines-windows-portal-sql-server-provision.md)
- [Adatbázis áttelepítése az SQL Server-Azure virtuális](virtual-machines-windows-migrate-sql.md)
- [Magas elérhetőségét, és a helyreállítás az Azure virtuális gépeken futó SQL Server](virtual-machines-windows-sql-high-availability-dr.md)
- [Az Azure virtuális gépeken futó SQL Server ajánlott eljárások a teljesítmény](virtual-machines-windows-sql-performance.md)
- [Alkalmazás mintázatok és Azure virtuális gépeken futó SQL Server fejlesztési stratégiák](virtual-machines-windows-sql-server-app-patterns-dev-strategies.md)
