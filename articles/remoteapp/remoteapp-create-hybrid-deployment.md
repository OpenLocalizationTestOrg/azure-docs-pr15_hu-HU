<properties
    pageTitle="A hibrid webhelycsoport létrehozása az Azure RemoteApp |} Microsoft Azure"
    description="Megtudhatja, hogyan hozhat létre egy belső hálózatához kapcsolódó RemoteApp telepítését."
    services="remoteapp"
    documentationCenter=""
    authors="lizap"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="elizapo"/>

# <a name="how-to-create-a-hybrid-collection-for-azure-remoteapp"></a>A hibrid webhelycsoport létrehozása az Azure RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp megszakad alatt. Olvassa el a további információt a [közlemény](https://go.microsoft.com/fwlink/?linkid=821148) .

Azure RemoteApp gyűjtemények két fő típusba sorolhatók:

- Felhőalapú: teljesen Azure-ban található. Lehetősége van az összes adat mentése a felhőbe (így a felhőben gyűjtemény) vagy való kapcsolódáshoz a webhelycsoport egy VNET, és ott adatokat is menteni.   
- Hibrid: tartalmaz egy virtuális hálózati a helyszíni hozzáférés - ehhez szükséges a Azure Active Directory és a helyszíni Active Directory-környezetben.

Nem, hogy melyik van szüksége? Nézze meg, hogy [milyen típusú webhelycsoport szükséges az Azure RemoteApp](remoteapp-collections.md).

Ebben az oktatóanyagban végigvezeti a hibrid webhelycsoport létrehozásának folyamata. Nyolc lépésből áll:

1.  Döntse el, a webhelycsoporthoz használandó [képet](remoteapp-imageoptions.md) . Hozzon létre egy egyéni képe, vagy használja a Microsoft képek az előfizetéshez tartozó egyikét.
2. A virtuális hálózat beállítása. Nézze meg a [VNET tervezése](remoteapp-planvnet.md) és [átméretezésre](remoteapp-vnetsizing.md) információkat.
2.  Hozzon létre egy.
2.  A webhelycsoport vegye fel a helyi tartományba.
3.  Sablon kép elhelyezése a webhelycsoport.
4.  Konfigurálja a címtár-szinkronizálás. Azure RemoteApp szükséges integrálása az Azure Active Directory vagy 1) konfigurálása Azure Active Directory Sync jelszó-szinkronizálás lehetőséggel és 2) konfigurálása Azure Active Directory Sync a jelszó-szinkronizálás beállítást, de egy tartományt, amelyet összevont legyen-e az Active Directory összevonási szolgáltatások használata nélkül. Nézze meg az [Active Directory RemoteApp konfigurációs adatok](remoteapp-ad.md).
5.  Távoli alkalmazás formájában használva alkalmazások közzététele.
6.  Felhasználói hozzáférés konfigurálása

**Első lépések**

Kell a webhelycsoport létrehozása előtt tegye a következőket:

- [Jelentkezzen](https://azure.microsoft.com/services/remoteapp/) az Azure RemoteApp.
- Felhasználói fiók létrehozása az Azure RemoteApp szolgáltatásfiók legyen az Active Directoryban. Ehhez a fiókhoz az engedélyek korlátozása, úgy, hogy a gép csak csatlakozhatnak a tartomány.
- A helyszíni hálózaton információkat gyűjthet: IP-cím VPN adatai és az információk.
- Telepítse az [Azure PowerShell](../powershell-install-configure.md) -modult.
- A felhasználók, ha szeretne engedélyt adni kívánt információkat gyűjthet. Szüksége lesz az Azure Active Directory egyszerű felhasználónév (például name@contoso.com) minden felhasználó számára. Győződjön meg arról, hogy megfelel-e (UPN) közötti Azure Active Directory és az Active Directory.
- Válassza a sablon képe. Azure RemoteApp sablon képet tartalmazza, az alkalmazások és a program, amelyet közzé szeretné tenni a felhasználók számára. [Azure RemoteApp Képbeállítások](remoteapp-imageoptions.md) talál további információt.
- Az Office 365 ProPlus kép használata: szeretne? Nézze meg az információ [Itt](remoteapp-officesubscription.md).
- [Konfigurálása az Active Directory RemoteApp](remoteapp-ad.md).



## <a name="step-1-set-up-your-virtual-network"></a>Lépés: 1: A virtuális hálózat beállítása
Telepítheti a meglévő Azure virtuális hálózat használó hibrid gyűjteménye, vagy létrehozhat egy új virtuális hálózathoz. Virtuális hálózat lehetővé teszi, hogy a felhasználók access-adatok a helyi hálózaton keresztül RemoteApp távoli erőforrások. Azure virtuális hálózat a webhelycsoport közvetlen hálózati hozzáférést biztosít más Azure szolgáltatások és a virtuális gépeken futó virtuális a hálózaton rendszerbe.

Győződjön meg arról, hogy a VNET létrehozása előtt tekintse át a [VNET tervezése](remoteapp-planvnet.md) és [VNET méret](remoteapp-vnetsizing.md) információk.

### <a name="create-an-azure-vnet-and-join-it-to-your-active-directory-deployment"></a>Hozzon létre egy Azure VNET, és hozzá az Active Directory-példányhoz

[Virtuális hálózati](../virtual-network/virtual-networks-create-vnet-arm-pportal.md)létrehozásával kell kezdenie. Ez történik, a **hálózati** lapon az Azure-portálon. Csatlakoznia kell a virtuális hálózat az Active Directory-példányhoz szinkronizált az Azure Active Directory-bérlőhöz.

[Hozzon létre egy virtuális hálózati az Azure portálon](../virtual-network/virtual-networks-create-vnet-arm-pportal.md) talál további információt.

### <a name="make-sure-your-virtual-network-is-ready-for-azure-remoteapp"></a>A virtuális hálózat az Azure RemoteApp használatra kész állapotának ellenőrzése
Előtt hoz létre, a webhelycsoport, nézzük győződjön meg arról, hogy készen áll-e az új virtuális hálózat. Az alábbi lépésekkel ellenőrizheti:

1. Hozzon létre egy Azure virtuális gép belül a alhálózat RemoteApp az imént létrehozott virtuális hálózat.
2. A virtuális géphez kapcsolatfelvétel távoli asztali használatával. (Kattintson a **Csatlakozás**.)
3. Bekapcsolódás a ugyanazt az Active Directory-példányhoz RemoteApp használni kívánt azt.

Hogy működött? A virtuális hálózati és alhálózat készen állnak a Azure RemoteApp!

Azure virtuális gépeken futó létrehozása és a távoli asztali [Itt](https://msdn.microsoft.com/library/azure/jj156003.aspx)csatlakozás további információt talál.

## <a name="step-2-create-an-azure-remoteapp-collection"></a>Lépés: 2: Hozzon létre egy Azure RemoteApp ##



1. Az [Azure portálon](http://manage.windowsazure.com)lépjen az Azure RemoteApp lapra.
2. Kattintson a **Új > VNET létrehozását**.
3. Adja meg a webhelycsoport nevét.
4. Válassza a use - normál vagy egyszerű használni kívánt csomagot.
5. A VNET válassza a legördülő listára, majd az alhálózathoz.
6. Válassza a tartomány csatlakozni.
5. Kattintson a **webhelycsoport létrehozása RemoteApp**.

Az Azure RemoteApp webhelycsoport létrehozása után kattintson duplán a webhelycsoport nevét. Az **Első lépések** oldal állapotba kerül – ez az, ha végzett a konfigurálása a webhelycsoport.

DID valami hiba lép fel? Nézze meg a [hibrid webhelycsoport hibaelhárítási információkat](remoteapp-hybridtrouble.md).

## <a name="step-3-link-your-collection-to-the-local-domain"></a>3 lépés: A webhelycsoport csatolása a helyi tartomány ##


1. Az **Első lépések** lapon kattintson a **Csatlakozás helyi tartomány**elemre.
2. Az Azure RemoteApp szolgáltatási fiók felvétele a helyi Active Directory tartományi. Szüksége lesz a tartománynevét, szervezeti egységre, szolgáltatási fiók felhasználónevét és jelszavát.

    Ez a az információkat, összegyűjtötte, ha követte [Azure RemoteApp Active Directory beállítása](remoteapp-ad.md)című témakör lépéseit.


## <a name="step-4-link-to-an-azure-remoteapp-image"></a>Lépés: 4: Azure RemoteApp kép hivatkozás ##

Azure RemoteApp sablon képet a felhasználóival megosztani kívánt programokat tartalmazza. Hozzon létre egy új [sablon képe](remoteapp-imageoptions.md) , vagy hivatkozni szeretne egy meglévő kép (egy már importált vagy töltenek fel Azure RemoteApp). Is össze lehet kapcsolni az egyik Office 365-ben vagy az (a kipróbálás) Office 2013 alkalmazásainak tartalmazó Azure RemoteApp [a webhelysablonok képét](remoteapp-images.md) .

Ha az új kép feltöltése, akkor írja be a nevét, és válassza ki azt a helyet, hogy a kép szüksége. A varázsló következő lapján fogja beállítási PowerShell-parancsmagok - példányt, és egy magasabb szintű, a megadott kép feltöltése a Windows PowerShell-parancssorában futtassa a parancsmagok.

Ha csatol egy meglévő sablonként képe, egyszerűen adja meg, a kép neve nevet, helyét és kapcsolódó Azure előfizetés.



## <a name="step-5-configure-active-directory-directory-synchronization"></a>Lépés az 5: Konfigurálása az Active Directory-címtár-szinkronizálás ##

Azure RemoteApp szükséges integrálása az Azure Active Directory vagy 1) konfigurálása Azure Active Directory Sync jelszó-szinkronizálás lehetőséggel és 2) konfigurálása Azure Active Directory Sync a jelszó-szinkronizálás beállítást, de egy tartományt, amelyet összevont legyen-e az Active Directory összevonási szolgáltatások használata nélkül.

Csatlakozás [Active Directory](https://blogs.technet.microsoft.com/enterprisemobility/2014/08/04/connecting-ad-and-azure-ad-only-4-clicks-with-azure-ad-connect/) – Ez a cikk segít beállítani a címtár-integrációs 4 lépésben.

[Címtár-szinkronizálás menete](http://msdn.microsoft.com//library/azure/hh967642.aspx) lásd: tervezési információ és a lépések részletes leírását.

## <a name="step-6-publish-apps"></a>Lépés a 6: Alkalmazások közzététele ##

Azure RemoteApp alkalmazás az alkalmazás vagy a program a felhasználók által. A sablon képe, a webhelycsoport feltöltött található. Ha egy felhasználó fér hozzá az alkalmazás, úgy tűnik, hogy futtassa a helyi környezetben, de valójában fut Azure-ban.

Ahhoz, hogy a felhasználók hozzá tudnak férni a alkalmazások, őket közzé kell – ezzel a lehetőséggel a felhasználók access-az alkalmazások között a távoli asztali ügyfél.

A webhelycsoport több alkalmazások közzéteheti. A közzététel lapon kattintson a **Közzététel** , az alkalmazás hozzáadása elemre. A sablon kép vagy az elérési út megadásával a sablon képen az alkalmazás a **Start** menüből vagy közzéteheti. Ha úgy dönt, hogy a **Start** menüből hozzáadása, a kívánt program kiválasztása a hozzáadása. Ha úgy dönt, hogy adja meg az App elérési útját, nevezze el az alkalmazást, és elérési útját, ahol már telepítve van a sablon képe.

## <a name="step-7-configure-user-access"></a>7 lépés: Felhasználói hozzáférés konfigurálása ##

Most, hogy a webhelycsoport létrehozott, kell adja hozzá a felhasználókat, hogy a távoli erőforrások használni szeretne. Ebben a gyűjteményben Azure RemoteApp létrehozásához használt a felhasználóknak, hogy Ön megadja-e kell az előfizetéshez tartozó az Active Directory-ös bérlői létezik a hozzáférést.

1.  Az első lépések lapon kattintson a **konfigurálás felhasználói hozzáférés**elemre.
2.  Adja meg a hozzáférést a kívánt munkahelyi fiók (az Active Directory) vagy Microsoft-fiókjával.

    **Megjegyzések:**

    Győződjön meg arról, hogy használja az “user@domain.com” formátum.

    Alkalmazás használatakor az Office 365 ProPlus a gyűjteményben, az Active Directory identitások kell használnia a felhasználók számára. Ezzel az elemzéssel licencelési ellenőrzése.


3.  Amikor a felhasználók érvényesítése, kattintson a **Mentés**gombra.


## <a name="next-steps"></a>Következő lépések ##
Ez - sikeresen létre és az Azure RemoteApp hibrid webhelycsoport rendszerbe. A következő lépésben, hogy a felhasználók, töltse le és telepítse a távoli asztali ügyfél. Az URL-címet az ügyfél Azure RemoteApp első lépések lapon talál. Ezt követően a felhasználók log az ügyfél be van, és a hozzáférési közzétett alkalmazások.



### <a name="help-us-help-you"></a>Segítse a szolgáltatás segítségével
Tudta, hogy mellett ez a cikk minősítés, és a megjegyzések megadására lefelé alatt, módosíthatja a cikkben magát? Valami hiányzó? Valami hiba történt? DID lehet Írjon valamit, amely csak áttekinthetőbb legyen? Görgetés felfelé, és kattintson a **szerkesztése a GitHub** módosításához - e érkezzenek us véleményezésre, és majd, ha azt jelentkezzen, a őket, látni fogja a módosításokat, és itt javítása.
