<properties
    pageTitle="SQL Server Management Studio használata Azure RemoteApp SQL-adatbázis csatlakoztatása |} Microsoft Azure"
    description="Ebből az oktatóanyagból megtudhatja, hogy miként használható az SQL Server Management Studio Azure RemoteApp a biztonság és a teljesítmény SQL-adatbázishoz való csatlakozáskor használata"
    services="sql-database"
    documentationCenter=""
    authors="adhurwit"
    manager="jhubbard"/>

<tags
    ms.service="sql-database"
    ms.workload="data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/05/2016"
    ms.author="adhurwit"/>

# <a name="use-sql-server-management-studio-in-azure-remoteapp-to-connect-to-sql-database"></a>SQL Server Management Studio használata Azure RemoteApp SQL-adatbázishoz való csatlakozáshoz

## <a name="introduction"></a>– Bevezetés  
Ebből az oktatóanyagból megtudhatja, hogy miként, SQL Server Management Studio (SSMS) használandó Azure RemoteApp SQL-adatbázishoz való csatlakozáshoz. Ez a végigvezeti a folyamaton, állítsa be az SQL Server Management Studio az Azure RemoteApp, előnyökkel jár mutatja be, valamint biztonsági funkciók láthatók, amelyek is használhatja az Azure Active Directory.

**Becsült időt vesz igénybe:** 45 perc

## <a name="ssms-in-azure-remoteapp"></a>Az Azure RemoteApp SSMS

Azure RemoteApp egy távoli asztali szolgáltatás, amely biztosítja az alkalmazások Azure-ban. Itt talál további információt: [RemoteApp Újdonságok?](../remoteapp/remoteapp-whatis.md)

Azure RemoteApp futó SSMS lehetővé teszi a azonos felület SSMS helyileg futó.

![Képernyőkép a területről, SSMS az Azure RemoteApp fut.][1]



## <a name="benefits"></a>Legfontosabb előny

Előnyökkel sok SSMS használata Azure RemoteApp, például:

- Azure SQL Server 1433 portjához nem rendelkezik külső felhasználókkal (Azure) kívüli kezdeményezhető.
- Nincs szükség, megtartása, felvétele és IP-címek eltávolítása a Azure SQL Server-tűzfal.
- Az összes Azure RemoteApp kapcsolatok előfordulását HTTPS 443-as port használatáról titkosított távoli asztali Protocol (protokoll)
- Több felhasználó, és méretezheti.
- A teljesítmény nyereség ugyanabban a régióban, mint az SQL-adatbázis SSMS bejelentkezésen van.
- Azure RemoteApp felhasználása az Azure Active Directory, amelynek felhasználói tevékenység jelentések prémium kiadásának naplózható.
- Többtényezős hitelesítés (MFA) engedélyezheti.
- Access SSMS tetszőleges használatakor a támogatott Azure RemoteApp ügyfelek, amely tartalmazza az iOS, az Android, az Mac, a Windows Phone és a Windows rendszerben közül.


## <a name="create-the-azure-remoteapp-collection"></a>Az Azure RemoteApp webhelycsoport létrehozása

Az Azure RemoteApp webhelycsoport létrehozását SSMS szereplő lépések a következők:


### <a name="1-create-a-new-windows-vm-from-image"></a>1. Hozzon létre egy új Windows virtuális kép
Segítségével a "Windows Server távoli asztali munkamenet Host Windows Server 2012 R2" kép a gyűjteményből az új virtuális.


### <a name="2-install-ssms-from-sql-express"></a>2. SSMS telepíteni az SQL Express

Nyissa meg az új virtuális alakzatot, és keresse meg azt a letöltési lapját: [Microsoft® SQL Server® 2014-es Express](https://www.microsoft.com/en-us/download/details.aspx?id=42299)

Csak töltse le a SSMS szolgáló lehetőség van. Letöltés után mappába érkeznek a telepítési könyvtár és SSMS telepítéséhez futtassa telepítőt.

Is kell telepítenie az SQL Server 2014-es Service Pack 1. Itt letöltése: [Microsoft SQL Server 2014-es Service Pack 1 (SP1)](https://www.microsoft.com/en-us/download/details.aspx?id=46694)

Az SQL Server 2014-es Service Pack 1 Azure SQL-adatbázis használata alapvető funkciókat tartalmaz.


### <a name="3-run-validate-script-and-sysprep"></a>3. parancsfájl ellenőrzése és a Sysprep futtatása

A virtuális asztali egy PowerShell-parancsprogramot neve ellenőrzése. Futtassa a duplán kattintva. Ellenőrzi, hogy a virtuális használható alkalmazások távoli szolgáltatója készen áll-e. Ellenőrzés befejeződése után a program kéri a Sysprep-választva indítsa el.

Sysprep befejeztével leállíthatja a virtuális.

Azure RemoteApp képként létrehozásával kapcsolatos további tudnivalókért lásd: [hogyan hozhat létre egy RemoteApp sablonként képe Azure-ban](http://blogs.msdn.com/b/rds/archive/2015/03/17/how-to-create-a-remoteapp-template-image-in-azure.aspx)


### <a name="4-capture-image"></a>4. a kép rögzítéséhez

Ha a virtuális leállt, keresse meg az aktuális portált, és rögzítse.

További információ a képet a rögzítés című témakörben talál [egy Azure Windows virtuális gép a klasszikus telepítési modell készült képet](../virtual-machines/virtual-machines-windows-classic-capture-image.md)


### <a name="5-add-to-azure-remoteapp-template-images"></a>5. Azure RemoteApp a webhelysablonok képét hozzáadása

Az aktuális portál Azure RemoteApp szakaszában nyissa meg a sablon képeket lapot, és kattintson a Hozzáadás gombra. Az előugró mezőben jelölje be a "Kép importálása a virtuális gépeken futó tár", és válassza ki az imént létrehozott képet.



### <a name="6-create-cloud-collection"></a>6. felhő webhelycsoport létrehozása

Az aktuális portálon hozzon létre egy új Azure RemoteApp felhő. Válassza a SSMS telepítve van az imént importált sablonként képe.

![Hozzon létre új felhő gyűjtemény][2]


### <a name="7-publish-ssms"></a>7. SSMS közzététele

A közzététel a lap új felhő webhelycsoport, válassza a Start menüből alkalmazás közzététele, és válassza ki a SSMS a listából.

![Alkalmazás közzététele][5]

### <a name="8-add-users"></a>8. felhasználó hozzáadása

A felhasználói hozzáférés lapon jelölje ki a felhasználókat, hogy hozzáférést kap a Azure RemoteApp gyűjtemény SSMS tartalmazó.

![Felhasználó hozzáadása][6]


### <a name="9-install-the-azure-remoteapp-client-application"></a>9. Telepítse az Azure RemoteApp ügyfélalkalmazás

Tölt le és telepítse az Azure RemoteApp ügyfél itt: [Letöltés |} Azure RemoteApp](https://www.remoteapp.windowsazure.com/en/clients.aspx)



## <a name="configure-azure-sql-server"></a>Azure SQL Server-konfigurálása

A csak szükség beállítás annak érdekében, hogy Azure Services engedélyezve van-e a tűzfalon keresztül. Ha a megoldás, majd szükségtelen hozzáadása a bármely IP-címek nyissa meg a tűzfalat. A hálózati forgalmat, hogy engedélyezve van az SQL Server Azure szolgáltatásokból van.


![Azure engedélyezése][4]



## <a name="multi-factor-authentication-mfa"></a>Többtényezős hitelesítés (MFA)

MFA kifejezetten engedélyezhető az alkalmazáshoz. Nyissa meg az Azure Active Directory az alkalmazások fülre. Egy bejegyzést a Microsoft Azure RemoteApp találja. Kattintson az alkalmazást, és adja meg, ha látni fogja a lapon az alábbi hol engedélyezése a MFA ehhez az alkalmazáshoz.

![MFA engedélyezése][3]



## <a name="audit-user-activity-with-azure-active-directory-premium"></a>Az Azure Active Directory prémium verzió felhasználói műveletek naplózása

Ha nem rendelkezik az Azure Active Directory prémium, majd van kapcsolni a licencek csoportban a könyvtár. Az engedélyezett prémium verzióban a prémium szintre adhatnak a felhasználóknak.

Amikor kattint egy felhasználóhoz, az az Azure Active Directory, majd látogathat el a tevékenység lap Azure RemoteApp a bejelentkezési adatok megjelenítéséhez.



## <a name="next-steps"></a>Következő lépések

Miután elkészült a fenti lépéseket, fogja futtatható az Azure RemoteApp ügyfél és a napló egy tevékenységhez rendelt felhasználóval. Választhat a SSMS az alkalmazások egyikét, és ha Azure SQL Server hozzáféréssel rendelkező számítógépen telepítve volt módon futtathatja.

A kapcsolat SQL-adatbázishoz további tájékoztatást talál [Csatlakozás SQL Server Management Studio az SQL-adatbázishoz, és hajtsa végre a minta T-SQL-lekérdezés](sql-database-connect-query-ssms.md).


Ez a szolgáltatás most. Fogják túlságosan nagyra értékelni!



<!--Image references-->
[1]: ./media/sql-database-ssms-remoteapp/ssms.png
[2]: ./media/sql-database-ssms-remoteapp/newcloudcollection.png
[3]: ./media/sql-database-ssms-remoteapp/mfa.png
[4]: ./media/sql-database-ssms-remoteapp/allowazure.png
[5]: ./media/sql-database-ssms-remoteapp/publish.png
[6]: ./media/sql-database-ssms-remoteapp/user.png
