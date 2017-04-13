<properties 
   pageTitle="Azure adatok tó tárban tárolt adatok védelme |} Microsoft Azure" 
   description="Megtudhatja, hogy miként védik az adatokat az Azure tó adattár segítségével a csoportok és a hozzáférés-vezérlési listák" 
   services="data-lake-store" 
   documentationCenter="" 
   authors="nitinme" 
   manager="jhubbard" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="09/29/2016"
   ms.author="nitinme"/>

# <a name="securing-data-stored-in-azure-data-lake-store"></a>Azure adatok tó tárban tárolt adatok védelme

Egy háromlépéses megközelítést Azure tó adattár adatainak védelme.

1. Biztonsági csoportok az Azure Active Directory (AAD) létrehozásával kell kezdenie. Ezek a biztonsági csoportok használatával szerepköralapú hozzáférés-szerepalapú végrehajtása az Azure-portálon. További információ [a szerepköralapú hozzáférés-vezérlés Microsoft Azure-ban](../active-directory/role-based-access-control-configure.md).

2. A AAD biztonsági csoportok hozzárendelése az Azure tó adattár fiókhoz. Ez szabályozza a hozzáférést a portál és -kezelés műveletek a portálon vagy API-khoz tó adattár-fiókjába.

3. Rendelje hozzá a AAD biztonsági csoportok, az access vezérlő a tó adattár fájlrendszerben vezérlési listákat.

4. Ezenkívül is beállíthatja, hogy egy IP-címtartományokat tó adattár adatainak hozzáférő ügyfelek számára.

Ez a cikk ismertető használni az Azure portált a fenti feladatok elvégzéséhez. Fiók és az adatok szintre, de biztonsági hogyan tó adattár végrehajtja a részletesebb információkat [az Azure tó adattár biztonsági](data-lake-store-security-overview.md)témakörben talál. A hozzáférés-vezérlési listák Azure tó adattár alkalmaznak hogyan mély-merülési tudnivalókért lásd [Áttekintése a hozzáférés-vezérlés tó adattár](data-lake-store-access-control.md).

## <a name="prerequisites"></a>Előfeltételek

Ebben az oktatóanyagban megkezdése előtt a következőket kell rendelkeznie:

- **Az Azure-előfizetés**. Lásd: [Ismerkedés az Azure ingyenes próbaverziót](https://azure.microsoft.com/pricing/free-trial/).
- **Az Azure tó adattár fiókot**. Hogyan hozhat létre egyet, tanulmányozza [Azure tó adattár – első lépések](data-lake-store-get-started-portal.md)

## <a name="create-security-groups-in-azure-active-directory"></a>Biztonsági csoportok létrehozása az Azure Active Directory

Hogyan AAD biztonsági csoportokat hozhat létre és hogyan vehet fel felhasználókat a csoportba, tanulmányozza [az Azure Active Directory kezelése-biztonsági csoportokat](../active-directory/active-directory-accessmanagement-manage-groups.md).

## <a name="assign-users-or-security-groups-to-azure-data-lake-store-accounts"></a>Azure tó adattár fiókok felhasználókat vagy biztonsági csoportokat hozzárendelése

Amikor a felhasználókat vagy biztonsági csoportokat hozzárendeli Azure tó adattár fiókok, hozzáférés az Azure-portál és -Azure erőforrás-kezelő API-k használata a fiók kezelése műveleteket. 

1. Nyissa meg az Azure tó adattár fiókkal. A bal oldali ablaktábláján kattintson a **Tallózás gombra**kattintson **Tó adattár**, és a tó adattár lap a kattintson a fiók nevére, amelyhez szeretne rendelni egy felhasználó vagy biztonsági csoport.

2. A fiók tó adattár lap kattintson a **Beállítások**gombra. A **Beállítások** lap kattintson a **felhasználók**elemre.

    ![Biztonsági csoport tó adattár Azure-fiókjába hozzárendelése] (./media/data-lake-store-secure-data/adl.select.user.icon.png "Biztonsági csoport tó adattár Azure-fiókjába hozzárendelése")

3. A **felhasználó** lap alapértelmezés szerint a tulajdonos **előfizetés admins** csoportjának jelez. 

    ![Felhasználók hozzáadása és szerepkörök] (./media/data-lake-store-secure-data/adl.add.group.roles.png "Felhasználók hozzáadása és szerepkörök")
 
    Kétféleképpen csoport hozzáadása és a megfelelő szerepköröket rendelhet hozzájuk.

    * Egy felhasználó vagy csoport hozzáadása a fiókhoz, és majd rendeljen egy szerepkört, vagy
    * Szerepkör hozzáadása és a felhasználók és csoportok hozzárendelése szerepkörhöz.

    Ebben a részben megnézi az az első megközelítésre hozzáadása egy csoportot, és kattintson a szerepkörök kiosztása. Először jelölje ki a szerepkört, és hozzárendelheti csoportokhoz szerepkörhöz hasonló lépések végrehajtása
    
4. Kattintson a **felhasználók** lap a **Hozzáadás gombra** kattintva nyissa meg az **access hozzáadása** lap. Kattintson a **Hozzáadás az access** a lap kattintson a **Jelölje ki azt a szerepkört**, és válassza ki a felhasználó vagy csoport szerepkörbe.

     ![Szerepkör a felhasználó hozzáadása] (./media/data-lake-store-secure-data/adl.add.user.1.png "Szerepkör a felhasználó hozzáadása")

    A **tulajdonos** és **munkatársi** szerepkörök felügyeleti függvények számos hozzáférést biztosít az adatok tó ügyfél. Az adatok tó fog használhatja felhasználóknál hozzáadhatja őket az **olvasó **szerepkör. Ezeket a szerepköröket hatókörének az adatkezelési műveletek az Azure tó adattár fiókkal kapcsolatos korlátozódik.

    Adatok műveletekhez egyedi fájlrendszer engedélyei adni, a felhasználók mire alkalmas. Ezért olvasó szerepkörbe rendelkező felhasználói is csak megtekinthetik a fiókkal társított felügyeleti beállításait, de is esetleg adatainak olvasása és írása a hozzájuk rendelt fájlrendszer engedélyei alapján. A [hozzáférés-vezérlési listák az Azure tó adattár fájlrendszerben biztonsági csoportok hozzárendelése](#filepermissions)tó adattár fájlrendszer engedélyei témakörben olvashat.



5. Kattintson a **Hozzáadás az access** lap **felhasználók hozzáadása** , a **felhasználók hozzáadása** lap megnyitásához. Ez a lap keresse meg az Azure Active Directory korábban létrehozott biztonsági csoport. Ha sok csoportok közötti kereséshez, a szövegdoboz tetején szűrése használatával a csoport nevére. Kattintson a **Kijelölés**gombra.

    ![Biztonsági csoport hozzáadása] (./media/data-lake-store-secure-data/adl.add.user.2.png "Biztonsági csoport hozzáadása")

    Ha szeretne, amely nem szerepel a csoport vagy felhasználó hozzáadása, meghívhatja a őket használatával a **Meghívás** ikonra, és adja meg a felhasználó vagy csoport e-mail címét.

6. Kattintson az **OK gombra**. Meg kell jelennie a biztonsági csoport hozzáadása az alább látható módon.

    ![Biztonsági csoport hozzáadása] (./media/data-lake-store-secure-data/adl.add.user.3.png "Biztonsági csoport hozzáadása")

7. A felhasználó vagy biztonsági csoport ekkor megjelenik a tó adattár Azure-fiók elérésének. Ha azt szeretné, az adott felhasználókhoz hozzáférést biztosít, később is hozzáadhatja a biztonsági csoporthoz. Hasonlóképpen ha meg szeretné visszavonni a hozzáférést a felhasználó, eltávolíthatja őket a biztonsági csoport. Több biztonsági csoportokat is rendelhet egy fiókot. 

## <a name="filepermissions"></a>Felhasználó vagy biztonsági csoport mint hozzárendelése hozzáférés-vezérlési listák az Azure tó adattár fájlrendszerben

Felhasználó vagy biztonsági csoportokat rendel az Azure adatok tó fájlrendszer, állítsa a hozzáférés-vezérlés az Azure adatok tó tárban tárolt adatok.

1. Kattintson a tó adattár fiók lap **Adatok Explorer**.

    ![Létrehozás könyvtárak tó adattár fiókban] (./media/data-lake-store-secure-data/adl.start.data.explorer.png "Az adatok tó fiók létrehozása könyvtárak")

2. Az **Adatok Explorer** lap kattintson a fájlra vagy mappára, amelynek be szeretné állítani a vezérlés, és kattintson az **Access**. Vezérlés hozzárendelése egy fájlt, a **Fájl előnézet** lap az **Access** kell kattintania.

    ![Beállítása adatok tó fájlrendszerben hozzáférés-vezérlési listák] (./media/data-lake-store-secure-data/adl.acl.1.png "Beállítása adatok tó fájlrendszerben hozzáférés-vezérlési listák")

3. Az **Access** lap normál access és a legfelső szintű már hozzá van rendelve egyéni hozzáférési sorolja fel. Kattintson a **Hozzáadás** ikonra, egyéni szintű hozzáférés-vezérlési listák hozzáadása.

    ![Normál és egyéni access lista] (./media/data-lake-store-secure-data/adl.acl.2.png "Normál és egyéni access lista")

    * **Normál elérése** a UNIX stílusú access megadhatja a olvasható, írja be, három különböző felhasználói osztályok execute (rwx): tulajdonosa, csoport- és másokkal.
    * **Egyéni access** felel meg a POSIX hozzáférés-vezérlési listák, amely lehetővé teszi, hogy az engedélyek megadása a megadott névvel ellátott felhasználók vagy csoportok, és nem csak a fájl tulajdonosának vagy csoportjának. 
    
    További tudnivalókért olvassa el a [Fájlrendszerhez hozzáférés-vezérlési listák](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsPermissionsGuide.html#ACLs_Access_Control_Lists)című témakört. Hogyan tó adattár a hozzáférés-vezérlési listák szerepelni fog a további tudnivalókért lásd: a [Hozzáférés-vezérlés tó adattár](data-lake-store-access-control.md).

4. Kattintson a **Hozzáadás** ikonra kattintva nyissa meg az **Egyéni Access hozzáadása** lap. Ez a lap **felhasználó vagy csoport kiválasztása**gombra, és a **felhasználó vagy csoport kiválasztása** a lap, keresse meg az Azure Active Directory korábban létrehozott biztonsági csoport. Ha sok csoportok közötti kereséshez, a szövegdoboz tetején szűrése használatával a csoport nevére. Kattintson a hozzáadni, és kattintson a **Jelölje ki**a kívánt csoportot.

    ![A csoport hozzáadása] (./media/data-lake-store-secure-data/adl.acl.3.png "A csoport hozzáadása")

5. Kattintson az **Engedélyek kiválasztása**gombra, jelölje ki az engedélyeket, és szeretné vezérlés alapértelmezés szerint az engedélyeket, hogy elérhetők a vezérlés, vagy mindkettőt. Kattintson az **OK gombra**.

    ![Csoport engedélyek hozzárendelése] (./media/data-lake-store-secure-data/adl.acl.4.png "Csoport engedélyek hozzárendelése")

    Tó adattár és alapértelmezett és a hozzáférés hozzáférés-vezérlési listák engedélyekkel kapcsolatos további tudnivalókért lásd: a [Hozzáférés-vezérlés tó adattár](data-lake-store-access-control.md).


6. Az **Egyéni Access hozzáadása** lap kattintson **az OK gombra**. Az újonnan hozzáadott csoportban, a hozzá tartozó engedélyek, a program az **Access** a lap szerepel.

    ![Csoport engedélyek hozzárendelése] (./media/data-lake-store-secure-data/adl.acl.5.png "Csoport engedélyek hozzárendelése")

    > [AZURE.IMPORTANT] Az aktuális programcsomag **Egyéni Access**a 9-es bejegyzések csak van. Felhasználók 9-nél több felhasználó hozzáadása szeretné, ha hozzunk létre a biztonsági csoportok hozzáadása biztonsági csoportokat, és adja meg az access biztonsági csoportokhoz biztosít a tó adattár fiókhoz.

7. Szükség esetén módosíthatja a hozzáférési engedélyek a csoport hozzáadása után. Törölje a jelet, vagy jelölje be a jelölőnégyzetet minden jogosultsági típus (olvasást, írást végrehajtás) alapján szeretné-e a biztonsági csoport ezt a jogosultságot hozzárendelése vagy eltávolítása. Kattintson a **Mentés** gombra a módosítások mentéséhez, vagy **elvetése** gombra a módosítások visszavonásához.

## <a name="set-ip-address-range-for-data-access"></a>IP-címtartományokat az adatokhoz való hozzáférés beállítása

Azure tó adattár lehetővé teszi a hozzáférést a adattár hálózaton szinten további zárolása. Tűzfal engedélyezése, adja meg az IP-címet, vagy egy IP-címtartományokat meghatározása a megbízható ügyfélalkalmazások. Miután engedélyezte, csak a megadott tartományba az IP-címek ügyfelek csatlakozhat az áruházból.

![Tűzfal beállításainak és IP-hozzáférés] (./media/data-lake-store-secure-data/firewall-ip-access.png "Tűzfal beállításainak és IP-cím")

## <a name="remove-security-groups-for-an-azure-data-lake-store-account"></a>Biztonsági csoportok a következőhöz: Azure tó adattár fiók eltávolítása

Biztonsági csoportok eltávolításakor Azure tó adattár fiókokból csak módosítja az access az adatkezelési műveletek az Azure-portál és -Azure erőforrás-kezelő API-k használata a fiók.

1. A fiók tó adattár lap kattintson a **Beállítások**gombra. A **Beállítások** lap kattintson a **felhasználók**elemre.

    ![Biztonsági csoport Azure adatok tó fiókhoz hozzárendelése] (./media/data-lake-store-secure-data/adl.select.user.icon.png "Biztonsági csoport Azure adatok tó fiókhoz hozzárendelése")

2. Kattintson a **felhasználók** lap a biztonsági csoport el szeretné távolítani.

    ![Biztonsági csoport eltávolítása] (./media/data-lake-store-secure-data/adl.add.user.3.png "Biztonsági csoport eltávolítása")

3. A biztonsági csoport lap kattintson az **Eltávolítás**gombra.

    ![Biztonsági csoport eltávolítása] (./media/data-lake-store-secure-data/adl.remove.group.png "Biztonsági csoport eltávolítása")

## <a name="remove-security-group-acls-from-azure-data-lake-store-file-system"></a>Azure tó adattár fájlrendszer hozzáférés-vezérlési listák biztonsági csoport eltávolítása

Biztonsági csoportok hozzáférés-vezérlési listák Azure tó adattár fájlrendszerből eltávolításakor, módosítani szeretné az adatokat az adatok tó áruházban.

1. Kattintson a tó adattár fiók lap **Adatok Explorer**.

    ![Az adatok tó fiók létrehozása könyvtárak] (./media/data-lake-store-secure-data/adl.start.data.explorer.png "Az adatok tó fiók létrehozása könyvtárak")

2. Az **Adatok Explorer** lap kattintson a fájlra vagy mappára, amelyhez el szeretné távolítani a vezérlés, és a fiók lap, kattintson az **Access** ikonra. Vezérlés fájl eltávolításához a **Fájl előnézet** lap az **Access** kell kattintania.

    ![Beállítása adatok tó fájlrendszerben hozzáférés-vezérlési listák] (./media/data-lake-store-secure-data/adl.acl.1.png "Beállítása adatok tó fájlrendszerben hozzáférés-vezérlési listák")

3. Az **Access** lap az **Egyéni Access** csoportjában kattintson a biztonsági csoport el szeretné távolítani. Az **Egyéni Access** -lap kattintson az **Eltávolítás** gombra, és kattintson **az OK**gombra.

    ![Csoport engedélyek hozzárendelése] (./media/data-lake-store-secure-data/adl.remove.acl.png "Csoport engedélyek hozzárendelése")


## <a name="see-also"></a>Lásd még:

- [Azure tó adattár áttekintése](data-lake-store-overview.md)
- [Adatok másolása Azure BLOB-tároló tó adattárhoz](data-lake-store-copy-data-azure-storage-blob.md)
- [Azure adatok tó Analytics használata tó adattárhoz](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
- [Azure hdinsight szolgáltatáshoz használata tó adattárhoz](data-lake-store-hdinsight-hadoop-use-portal.md)
- [Első lépések a PowerShell használatá tó adattárhoz](data-lake-store-get-started-powershell.md)
- [Első lépések a .NET SDK használatával tó adattárhoz](data-lake-store-get-started-net-sdk.md)
- [Az Access a diagnosztikai naplók adatok tó üzlet](data-lake-store-diagnostic-logs.md)
