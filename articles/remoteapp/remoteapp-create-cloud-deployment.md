<properties 
    pageTitle="Hogyan hozhat létre az Azure RemoteApp felhő gyűjteménye |} Microsoft Azure" 
    description="Megtudhatja, hogy miként hozhat létre, amely menti az adatokat az Azure felhőben Azure RemoteApp álló telepítés." 
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

# <a name="how-to-create-a-cloud-collection-of-azure-remoteapp"></a>Hogyan hozhat létre az Azure RemoteApp felhő gyűjteménye

> [AZURE.IMPORTANT]
> Azure RemoteApp megszakad alatt. Olvassa el a további információt a [közlemény](https://go.microsoft.com/fwlink/?linkid=821148) .

[Azure RemoteApp](remoteapp-collections.md)gyűjtemények két fő típusba sorolhatók: 

- Felhőalapú: teljesen Azure-ban található. Lehetősége van az összes adat mentése a felhőbe (így a felhőben gyűjtemény) vagy való kapcsolódáshoz a webhelycsoport egy VNET, és ott adatokat is menteni.   
- Hibrid: tartalmaz egy virtuális hálózati a helyszíni hozzáférés - ehhez szükséges a Azure Active Directory és a helyszíni Active Directory-környezetben.

Ebben az oktatóanyagban végigvezeti az a felhő webhelycsoport létrehozásának folyamata. Négy lépésből áll: 

1.  Hozzon létre egy Azure RemoteApp.
2.  Igény szerint állítsa be a címtár-szinkronizálás. Ha Azure Active Directory használja az Active Directory, ki kell szinkronizálni a felhasználók, a névjegyalbumhoz és a jelszavakat a helyszíni Active Directoryból a Azure AD-bérlő +.
5.  Alkalmazások közzététele.
6.  Felhasználói hozzáférés konfigurálása


**Első lépések**

Kell a webhelycsoport létrehozása előtt tegye a következőket:

- [Jelentkezzen](https://azure.microsoft.com/services/remoteapp/) az Azure RemoteApp. 
- A felhasználók, ha szeretne engedélyt adni kívánt információkat gyűjthet. Ez lehet a Microsoft-fiók adatainak vagy a fiók adatainak az Active Directory-felhasználóknak.
- Ez az eljárás feltételezi, hogy Ön, vagy nyissa meg a sablon képeket előfizetése részeként egyikének használatára, vagy hogy már feltölteni a használni kívánt sablon képe. Ha szeretne egy másik sablont képet feltölteni, hajtsa végre, amely a webhelysablonok képét lapról. Csak kattintson **egy sablon kép feltöltése** elemre, és kövesse a varázsló lépéseit. 
- Az Office 365 ProPlus kép használata: szeretne? Nézze meg az információ [Itt](remoteapp-officesubscription.md).
- Adja meg az egyéni alkalmazások vagy LOB-programok? Hozzon létre egy új [képet](remoteapp-imageoptions.md) , és a felhő gyűjteményben használni.
- Ábra szükség van-e egy VNET csatlakozni. Ha úgy dönt, hogy egy VNET csatlakozni, ellenőrizze, hogy a [méretező irányelvek](remoteapp-vnetsizing.md) és az, hogy eleget tesz [RemoteApp csatlakozhat](remoteapp-vnet.md). Olvassa el a [tervezési VNET a cikk ](remoteapp-planvnet.md)további információt.
- Ha egy VNET használata esetén eldönteni, hogy a helyi Active Directory tartományi csatlakozni.

## <a name="step-1-create-a-cloud-collection---with-or-without-a-vnet"></a>Lépés: 1: Hozzon létre egy felhőalapú webhelycsoport - vagy egy VNET nélkül##


Használja a **felhőben gyűjtemény**létrehozásához az alábbi lépéseket:

1. Az adatkezelési portálon lépjen a RemoteApp lapra.
2. Kattintson a **Új > gyors létrehozása**.
3. Adja meg a webhelycsoport nevét, és válassza ki a területet.
4. Válassza a use - normál vagy egyszerű használni kívánt csomagot.
5. Válassza a gyűjtemény használható a sablont. 

    **Tipp:** Az előfizetés RemoteApp a [sablon képeket](remoteapp-images.md) tartalmazó Office 365-ben vagy az Office 2013 (kipróbálásra) programok, néhány közzétett (például Word) és mások készen áll a közzététel megtalálható. Hozzon létre egy új [képet](remoteapp-imageoptions.md) is, és használni a felhőben gyűjteményben.


1. Kattintson a **webhelycsoport létrehozása RemoteApp**.
    
    **Fontos:** A webhelycsoport kiépítése akár 30 percig vehet igénybe.

Az Azure RemoteApp webhelycsoport létrehozása után kattintson duplán a webhelycsoport nevét. Az **Első lépések** oldal állapotba kerül – ez az, ha végzett a konfigurálása a webhelycsoport.

Egy **felhőalapú + VNET webhelycsoport**létrehozásához kövesse az alábbi lépéseket:

1. Az adatkezelési portálon lépjen az Azure RemoteApp lapra.
2. Kattintson az **Új** > **VNET létrehozását**.
3. Adja meg a webhelycsoport nevét.
4. Válassza a use - normál vagy egyszerű használni kívánt csomagot.
5. Válassza ki a már létrehozott VNET. Nem tudja, hogy hogyan? Most a lépéseket a [hibrid](remoteapp-create-hybrid-deployment.md) témakörben találhatók.
6. Döntse el, hogy szeretné-e a webhelycsoport csatlakozik a tartományhoz. Ha igen, kell használni a AD Connect integrálása az Azure Active Directory és az Active Directory-környezet. Vonatkozik az alatt a **2**.
6. Kattintson a **webhelycsoport létrehozása RemoteApp**.


## <a name="step-2-configure-active-directory-directory-synchronization-optional"></a>Lépés: 2: Konfigurálása az Active Directory címtár-szinkronizálás (nem kötelező) ##

Ha szeretné használni az Active Directory, a Azure RemoteApp Azure Active Directory és a helyszíni Active Directory szinkronizál a felhasználók, a névjegyalbumhoz és a jelszót az Azure Active Directory-bérlőhöz közötti címtár-szinkronizálás szükséges. Lásd: [Azure RemoteApp Active Directory konfigurálása](remoteapp-ad.md) tervezési információival. Is ugorhat közvetlenül [AD Connect](https://blogs.technet.microsoft.com/enterprisemobility/2014/08/04/connecting-ad-and-azure-ad-only-4-clicks-with-azure-ad-connect/) információt.

## <a name="step-3-publish-apps"></a>3 lépés: Az alkalmazások közzététele ##

Azure RemoteApp alkalmazás az alkalmazás vagy a program a felhasználók által. A sablon képe, a webhelycsoport feltöltött található. Felhasználó fér hozzá egy alkalmazást, az alkalmazás futtatásához helyi környezetükben megjelenik, de nagyon fut az Azure virtuális gépen. 

A felhasználók hozzá tudnak férni az alkalmazások, mielőtt azokat közzé kell, – közzétételi apps lehetővé teszi, hogy a felhasználók elérése az alkalmazások keresztül a távoli asztali ügyfél.
 
Az Azure RemoteApp webhelycsoport több alkalmazások közzéteheti. A közzététel lapon kattintson a **Közzététel** a program hozzáadása elemre. A sablon kép vagy az elérési út megadásával a sablon képen az alkalmazás a **Start** menüből vagy közzéteheti. Ha úgy dönt, hogy a **Start** menüből hozzáadása, válassza az alkalmazás közzététele. Ha úgy dönt, hogy adja meg az App elérési útját, nevezze el az alkalmazást, és elérési útját, ahol már telepítve van a sablon képe.

## <a name="step-4-configure-user-access"></a>Lépés: 4: Felhasználói hozzáférés konfigurálása ##

Most, hogy a webhelycsoport létrehozott, kell adja hozzá a felhasználókat, hogy a távoli erőforrások használni szeretne. Az Active Directory használatakor a felhasználóknak, hogy az előfizetéshez tartozó az Active Directory-ös bérlői léteznek kell elérése hozta létre ebben a gyűjteményben.

1.  Az első lépések lapon kattintson a **konfigurálás felhasználói hozzáférés**elemre. 
2.  Adja meg a hozzáférést a kívánt munkahelyi fiók (az Active Directory) vagy Microsoft-fiókjával.

    **Megjegyzések:** 

    Győződjön meg arról, hogy használja az “user@domain.com” formátum.

    Alkalmazás használatakor az Office 365 ProPlus a gyűjteményben, az Active Directory identitások kell használnia a felhasználók számára. Ezzel az elemzéssel licencelési ellenőrzése. 

3.  A felhasználók érvényesítése, kattintson a **Mentés**gombra.


## <a name="next-steps"></a>Következő lépések ##

Ez - sikeresen létre és az Azure RemoteApp felhő webhelycsoport rendszerbe. A következő lépésben, hogy a felhasználók, töltse le és telepítse a távoli asztali ügyfél. Az URL-címet az ügyfél Azure RemoteApp első lépések lapon talál. Ezt követően a felhasználók log az ügyfél be van, és a hozzáférési közzétett alkalmazások.

### <a name="help-us-help-you"></a>Segítse a szolgáltatás segítségével 
Tudta, hogy mellett ez a cikk minősítés, és a megjegyzések megadására lefelé alatt, módosíthatja a cikkben magát? Valami hiányzó? Valami hiba történt? DID lehet Írjon valamit, amely csak áttekinthetőbb legyen? Görgetés felfelé, és kattintson a **szerkesztése a GitHub** módosításához - e érkezzenek us véleményezésre, és majd, ha azt jelentkezzen, a őket, látni fogja a módosításokat, és itt javítása.