<properties
    pageTitle="A helyszíni mindig elérhetősége csoportjain kiterjesztése Azure |} Microsoft Azure"
    description="Ebben az oktatóanyagban erőforrásokat a klasszikus telepítési modell készült használ, és ismerteti, hogyan lehet a replika hozzáadása varázsló segítségével az SQL Server Management Studio (SSMS) hozzáadása a mindig a rendelkezésre állási csoport replikájának Azure-ban."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="MikeRayMSFT"
    manager="jhubbard"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="07/12/2016"
    ms.author="MikeRayMSFT" />

# <a name="extend-on-premises-always-on-availability-groups-to-azure"></a>A helyszíni mindig elérhetősége csoportjain kiterjesztése Azure

Mindig a elérhetősége csoportok nyújtanak nagy elérhetőség az adatbázis-csoportok másodlagos kópiák hozzáadásával. Ezek a replikák engedélyezése hiba esetén adatbázisok keresztül nem működnek. Ezenkívül azok használható kiürítése olvasási munkaterhelésekből vagy a biztonsági másolat feladatokat.

A helyszíni rendelkezésre állási csoportok egy vagy több SQL Server Azure-VMs kiépítési és töltse fel őket, kópiák a helyszíni elérhetősége csoportokba bővítheti a Microsoft Azure.

Ebben az oktatóanyagban feltételezi, hogy a következőkre van:

- Azure active előfizetés. Azt is megteheti, hogy [Regisztráljon az ingyenes próbaverzióra](https://azure.microsoft.com/pricing/free-trial/).

- Meglévő mindig a elérhetősége csoport helyszíni. Rendelkezésre állási csoportok a további tudnivalókért lásd: [Mindig a elérhetősége csoportok](https://msdn.microsoft.com/library/hh510230.aspx).

- Kapcsolat a helyszíni és az Azure virtuális hálózat között. A virtuális hálózat létrehozásával kapcsolatos további tudnivalókért lásd: a [Configure egy webhely VPN az Azure klasszikus portálon](../vpn-gateway/vpn-gateway-site-to-site-create.md).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

## <a name="add-azure-replica-wizard"></a>Azure kópia varázsló hozzáadása

Ez a szakasz mutatja be a mindig a rendelkezésre állási csoport tartalmazzák az Azure megoldást kiterjesztése az **Azure replika hozzáadása varázsló** használatával.

1. Az SQL Server Management Studio belül bontsa ki a **Mindig a magas elérhetősége** > **Rendelkezésre állási csoportok** > **[az elérhetőség csoport neve]**.

1. Kattintson a jobb gombbal az **Elérhetőség kópiák**, majd kattintson a **Replika hozzáadása**.

1. Alapértelmezés szerint az **Elérhetőség csoport varázsló kópiát hozzáadása** jelenik meg. Kattintson a **Tovább**gombra.  Ha be van jelölve a **ne jelenjen meg ez a lap** lehetőséget a lap alján egy előző indítási során a varázsló, a képernyőn nem jelennek meg.

    ![SQL](./media/virtual-machines-windows-classic-sql-onprem-availability/IC742861.png)

1. El kell minden meglévő másodlagos kópiák csatlakozni. Ha rákattint az **Csatlakozás...** mindegyik kópiában, vagy mellett is kattintson a **Csatlakozás minden...** a képernyő alján. A hitelesítés után kattintson a **Tovább gombra** kattintva jelenítheti meg a következő képernyőn.

1. **Kópiák megadása** lapon több lapokon jelennek meg a nézetválasztóval: **kópiák**, a **végpontokat**, a **Biztonsági beállítások**és a **figyelő**. A **kópiák** lapon kattintson a **... Azure replika hozzáadása** Azure replika hozzáadása varázsló elindításához.

    ![SQL](./media/virtual-machines-windows-classic-sql-onprem-availability/IC742863.png)

1. Jelöljön ki egy meglévő Azure Management tanúsítványt a helyi Windows tanúsítvány áruházból, ha telepítve van egy előtt. Jelölje be, vagy adjon meg egy Azure előfizetés azonosítója, ha a használt elé. Töltse le és telepítse az Azure Management tanúsítványt, és töltse le a választható előfizetések Azure-fiókkal történő letöltési kattinthat.

    ![SQL](./media/virtual-machines-windows-classic-sql-onprem-availability/IC742864.png)

1. Minden mező létrehozása az Azure virtuális gép (virtuális) a replika ugyanis használt értékeket tartalmazó lapon fog adataival.

  	|A beállítás|Leírás|
|---|---|
|**Kép**|Válassza ki a kívánt kombinációt az operációs rendszer és az SQL Server|
|**Virtuális mérete**|Jelölje ki a virtuális igényeinek leginkább megfelelő méretének|
|**Virtuális neve**|Adja meg, hogy egy egyedi nevet az új virtuális. A nevét kell között 3 és 15 karaktereket tartalmaznak, is tartalmaz, csak betűket, számokat és kötőjelet, és kell olyan betűvel kezdődő végén pedig használja egy betűt vagy a számot.|
|**Virtuális felhasználónév**|Adja meg a rendszergazdai fiókkal a virtuális szerkeszthetővé vált felhasználónév|
|**Virtuális rendszergazdai jelszó**|Adjon meg egy jelszót az új fiók|
|**Jelszó megerősítése**|Erősítse meg a jelszót az új fiók|
|**Virtuális hálózati**|Adja meg a Azure virtuális hálózat, amelyet az új virtuális kell használnia. Virtuális hálózatokon további tudnivalókért olvassa el a [Virtuális hálózati – áttekintés](../virtual-network/virtual-networks-overview.md)című témakört.|
|**Virtuális alhálózathoz**|Adja meg a virtuális hálózati alhálózat, amelyet az új virtuális kell használnia.|
|**Tartomány**|Erősítse meg a tartományhoz tartozó előre megadott érték helyes|
|**Tartomány felhasználónév**|Adjon meg egy fiókot, amely a helyi fürtre csomópontok a helyi Rendszergazdák csoport|
|**Jelszó**|Adja meg a jelszót a felhasználó tartománynévhez|

1. Kattintson az **OK gombra** a telepítési beállítások ellenőrzéséhez.

1. Ezután jogi feltételek jelennek meg. Olvassa el, és kattintson az **OK gombra** , ha Ön vállalja, hogy ezeket a kifejezéseket.

1. A **Kópiák adja meg,** oldal ismét megjelenik. Ellenőrizze az új Azure másolata a **kópiák**, a **Végpontok**és a **Biztonsági beállítások** lapon található a beállításait. Módosítsa a beállításokat, hogy az üzleti követelményeknek.  A paramétereket, a következő lapokon lévő kapcsolatos további tudnivalókért olvassa el a [Kópiák lapon adja meg (új elérhetősége csoport varázsló/hozzáadása kópia varázsló)](https://msdn.microsoft.com/library/hh213088.aspx)című témakört. Figyelje meg, hogy hallgatók nem hozható létre az elérhetőség csoportok Azure kópiák tartalmazó figyelő lapján. Ezeken kívül figyelő már létezik a varázsló indítása előtt, ha kap, hogy nem támogatja az Azure jelző üzenet. Megnézi lesz, az hallgatók létrehozása **az elérhetőség csoport figyelő létrehozása** szakaszban.

    ![SQL](./media/virtual-machines-windows-classic-sql-onprem-availability/IC742865.png)

1. Kattintson a **Tovább**gombra.

1. Válassza ki az adatokat a szinkronizálás használata a **Kezdeti adatszinkronizálás kiválasztása** lapon, és kattintson a **Tovább**gombra. Ha a legtöbb esetben válassza a **Teljes adatszinkronizálás**. Adatok szinkronizálási módszerekkel kapcsolatos további tudnivalókért lásd: az [Eredeti adatok szinkronizálás lap kijelölése (mindig a elérhetősége csoport varázslók)](https://msdn.microsoft.com/library/hh231021.aspx).

1. Tekintse át az eredményeket az **ellenőrzés** lapon. Függőben lévő kapcsolatos problémák javítása, illetve szükség esetén futtassa újra az ellenőrzés. Kattintson a **Tovább**gombra.

    ![SQL](./media/virtual-machines-windows-classic-sql-onprem-availability/IC742866.png)

1. Tekintse át az **Összegzés** lapon a beállítások, majd kattintson a **Befejezés gombra**.

1. A kiépítési folyamat kezdődik. A varázsló befejeződik, kattintson a **Bezárás** gombra a varázsló kilép.

>[AZURE.NOTE] Azure replika hozzáadása varázsló naplófájl a Users\User Name\AppData\Local\SQL Server\AddReplicaWizard. A naplófájl kapcsolatos hibák elhárítása sikertelen Azure replika telepítések használható. Ha a varázsló nem sikerül egy olyan művelet végrehajtása, minden korábbi műveletek közzétételének vissza, beleértve a kiépített virtuális törlése.

## <a name="create-an-availability-group-listener"></a>Az elérhetőség csoport figyelő létrehozása

Az elérhetőség csoport létrehozását követően az ügyfelek számára a replikák csatlakoztatása figyelő kell létrehozni. Hallgatók közvetlenül az elsődleges vagy a csak olvasható kópia bejövő kapcsolatokkal. A hallgatók további tudnivalókért lásd: a [Configure egy ILB figyelő mindig a elérhetősége csoportok Azure-ban](virtual-machines-windows-classic-ps-sql-int-listener.md).

## <a name="next-steps"></a>Következő lépések

Mellett az **Azure replika hozzáadása varázsló** segítségével a mindig a rendelkezésre állási csoport kiterjesztése Azure, akkor előfordulhat, hogy is átvihet néhány SQL Server-munkaterhelésekből teljesen Azure. Első lépésként olvassa el a [virtuális gép SQL Server Azure a kiépítési](virtual-machines-windows-portal-sql-server-provision.md)című témakört.

Futó SQL Server Azure VMs kapcsolódó témakörök [A Azure virtuális gépeken futó SQL Server](virtual-machines-windows-sql-server-iaas-overview.md)talál.
