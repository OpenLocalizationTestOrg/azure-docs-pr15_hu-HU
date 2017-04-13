<properties
   pageTitle="Egyszerű portálon szolgáltatás hozzon létre |} Microsoft Azure"
   description="Ismerteti, hogyan hozhat létre egy új Active Directory-alkalmazás és a szolgáltatás egyszerű, a szerepköralapú hozzáférés-vezérlés az Azure erőforrás-kezelő az erőforrásokhoz való hozzáférés kezelése a használható."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="tfitzmac"
   manager="timlt"
   editor="tysonn"/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/07/2016"
   ms.author="tomfitz"/>

# <a name="use-portal-to-create-active-directory-application-and-service-principal-that-can-access-resources"></a>Az Active Directory-alkalmazás és a források hozzáférő szolgáltatás egyszerű létrehozása portal segítségével

> [AZURE.SELECTOR]
- [A PowerShell](resource-group-authenticate-service-principal.md)
- [Azure CLI](resource-group-authenticate-service-principal-cli.md)
- [Portál](resource-group-create-service-principal-portal.md)


Ha eléréséhez, vagy módosítsa az erőforrások kell, hogy egy alkalmazást, az Active Directory (AD) alkalmazás beállítása, és rendelje hozzá a szükséges engedélyekkel. Ez a témakör bemutatja, hogyan végezze el ezeket a lépéseket a portálon keresztül. Jelenleg kell használnia a klasszikus portál hozzon létre egy új Active Directory-alkalmazást, és ezután váltsa a szerepkör hozzárendelése az alkalmazás az Azure-portálra. 

> [AZURE.NOTE] Ebben a témakörben ismertetett lépéseket csak akkor alkalmazható, ha a **Klasszikus portal** segítségével az Active Directory-alkalmazás létrehozása. **Ha az Active Directory-alkalmazás létrehozása az Azure portálon használja, ezeket a lépéseket nem fog sikerülni.** 
>
> Előfordulhat, hogy ez egyszerűbbé teszi állíthatja be az Active Directory-alkalmazás vagy szolgáltatás fő [PowerShell](resource-group-authenticate-service-principal.md) vagy [Azure CLI](resource-group-authenticate-service-principal-cli.md), különösen akkor, ha a hitelesítési tanúsítványt használni szeretne. Ez a témakör a tanúsítványt használatához ne jelenjen meg.

Az Active Directory fogalmak szakaszok egyenként ismertetik olvassa el az [alkalmazás és a szolgáltatás egyszerű objektumok](./active-directory/active-directory-application-objects.md)című témakört. Az Active Directory authentication kapcsolatos további tudnivalókért lásd: [Az Azure Active Directory Authentication esetek](./active-directory/active-directory-authentication-scenarios.md).

A lépések részletes ismertetése a kérelmet integrálása az Azure erőforrások kezelésére szolgáló [fejlesztői](resource-manager-api-authentication.md)útmutatójában engedély a Azure erőforrás-kezelő API-val.

## <a name="create-an-active-directory-application"></a>Az Active Directory-alkalmazás létrehozása

1. Jelentkezzen be az Azure-fiókjába a [Klasszikus portálon](https://manage.windowsazure.com/)keresztül. 

2. Győződjön meg arról, hogy tudja, hogy az alapértelmezett Active Directory-előfizetéséhez. Access-alkalmazásokhoz az előfizetés ugyanabban a mappában csak adhat. Válassza a **Beállítások** , és keresse meg a könyvtár nevét az előfizetéséhez társított.  További tudnivalókért olvassa el a [hogyan Azure előfizetések társítva Azure Active Directory](./active-directory/active-directory-how-subscriptions-associated-directory.md)című témakört.
   
     ![alapértelmezett könyvtár megkeresése](./media/resource-group-create-service-principal-portal/show-default-directory.png)

2. A bal oldali ablaktáblában válassza ki **Az Active Directory** .

     ![Jelölje ki az Active Directoryban](./media/resource-group-create-service-principal-portal/active-directory.png)
     
3. Jelölje ki az Active Directory, az alkalmazás létrehozásához használni kívánt. Ha egynél több Active Directory, az alkalmazás létrehozása az előfizetéshez tartozó alapértelmezett címtárban.   

     ![Válassza a címtár](./media/resource-group-create-service-principal-portal/active-directory-details.png)
     
3. Az alkalmazások címtárában megtekintéséhez válassza ki azokat az **alkalmazásokat**.

     ![alkalmazások megtekintése](./media/resource-group-create-service-principal-portal/view-applications.png)

4. Ha egy alkalmazás előtt, hogy címtárban eddig nem hozott létre, meg kell jelennie alábbi képhez hasonló. Jelölje be az **alkalmazás hozzáadása**

     ![alkalmazás hozzáadása](./media/resource-group-create-service-principal-portal/create-application.png)

     Vagy az alsó ablaktáblában kattintson a **Hozzáadás** gombra.

     ![hozzáadása](./media/resource-group-create-service-principal-portal/add-icon.png)

5. Válassza ki a létrehozni kívánt alkalmazást. Jelölje ki a **szervezeten elkészítésének az alkalmazás hozzáadása**ebben az oktatóanyagban. 

     ![Új alkalmazás](./media/resource-group-create-service-principal-portal/what-do-you-want-to-do.png)

6. Nevezze el az alkalmazást, és válassza ki a létrehozni kívánt alkalmazást. Ebben az oktatóprogramban egy **WEBES API WEB APPLICATION Programming és/vagy** hozzon létre, és kattintson a Tovább gombra. **Natív ÜGYFÉLALKALMAZÁS**lehetőséget választja, ha a hátralévő lépéseket a jelen cikk nem felel meg a felhasználói felület.

     ![név alkalmazás](./media/resource-group-create-service-principal-portal/tell-us-about-your-application.png)

7. Adja meg az alkalmazás tulajdonságait. **SIGN-ON URL-CÍMÉT**adja meg a URI fel egy olyan webhelyet, amely leírja az alkalmazás. A webhely megléte érvényesítése nem volt. Az **Alkalmazás azonosítója URI**adja meg a URI, amely azonosítja az alkalmazást.

     ![szolgáltatásalkalmazás tulajdonságainak](./media/resource-group-create-service-principal-portal/app-properties.png)

Az alkalmazás hozott létre.

## <a name="get-client-id-and-authentication-key"></a>Ügyfél-azonosító és a hitelesítéshez kulcs beszerzése

Programozás útján jelentkezik, amikor az alkalmazás azonosítója szükséges. Az alkalmazás fut, a saját hitelesítő adatok területén, is szükség van egy hitelesítési kulcsot.

1. Jelölje be a **beállítás** lapon állítsa be a jelszót az alkalmazás.

     ![alkalmazások konfigurálása](./media/resource-group-create-service-principal-portal/application-configure.png)

2. Másolja az **ügyfél-azonosító**.
  
     ![ügyfél-azonosító](./media/resource-group-create-service-principal-portal/client-id.png)

3. Ha az alkalmazás fut, a saját hitelesítő adatok területén, görgessen le a **billentyűk** szakasz és mennyi ideig szeretné legyenek érvényesek a jelszavát.

     ![billentyűk](./media/resource-group-create-service-principal-portal/create-key.png)

4. Válassza a **Mentés** hozhat létre a termékkulcsot.

     ![Mentés](./media/resource-group-create-service-principal-portal/save-icon.png)

     A mentett kulcs jelenik meg, és másolhatja azt. Ön nem tudja később beolvasni a kulcs így másolja a vágólapra gombra.

     ![mentett kulcs](./media/resource-group-create-service-principal-portal/save-key.png)

## <a name="get-tenant-id"></a>Bérlői ID azonosító beszerzése

Bejelentkezéskor programozás útján, kell adják át a hitelesítési kérésével bérlői azonosítója. Web Apps alkalmazások és webes API-alkalmazások esetén a bérlői azonosító meghallgathatja jelölje ki a **Végpontok nézetben** a képernyő alján, és lekérdezése az azonosító, az alábbi képen látható módon.  

   ![Bérlői azonosító](./media/resource-group-create-service-principal-portal/save-tenant.png)

A bérlői azonosító Powershellen keresztül is meghallgathatja:

    Get-AzureRmSubscription

Másik lehetőségként Azure CLI:

    azure account show --json

## <a name="set-delegated-permissions"></a>Engedélyek beállítása a meghatalmazott

Ha az alkalmazás nevében bejelentkezett felhasználó fér hozzá az erőforrásokat, engedélyt kell adni az alkalmazást a meghatalmazott más alkalmazások eléréséhez. Akkor hozzáférést ez a **beállítás** lapon **más alkalmazásokba engedélyek** szakaszában. Alapértelmezés szerint a meghatalmazott jogosultsági már engedélyezve van az Azure Active Directory. Kilépés a meghatalmazott jogosultsági változatlan marad.

1. Jelölje ki azt a **Hozzáadás alkalmazást**.

2. A listából jelölje be a **Windows Azure szolgáltatás felügyeleti API**. Válassza a kész ikonra.

      ![Jelölje be az alkalmazás](./media/resource-group-create-service-principal-portal/select-app.png)

3. Meghatalmazott engedélyeit legördülő listájában jelölje ki az **Access Azure Szolgáltatáskezelés, szervezeti**.

      ![Jelölje ki az engedélyek](./media/resource-group-create-service-principal-portal/select-permissions.png)

4. A módosítás mentéséhez.

## <a name="assign-application-to-role"></a>Alkalmazás szerepkör hozzárendelése

Ha az alkalmazás a saját hitelesítő adatok területén fut, az alkalmazás szerepkörbe hozzá kell rendelnie. Döntse el, hogy milyen szerepkör jelöli a megfelelő engedélyekkel az alkalmazáshoz. A rendelkezésre álló szerepkörök kapcsolatos további tudnivalókért lásd: [RBAC: beépített szerepkörök](./active-directory/role-based-access-built-in-roles.md). 

Rendeljen egy szerepkört az alkalmazás, rendelkeznie a megfelelő engedélyekkel. Pontosabban kell `Microsoft.Authorization/*/Write` hozzáférést, amely a [tulajdonos](./active-directory/role-based-access-built-in-roles.md#owner) szerepkör vagy a [Felhasználói hozzáférés rendszergazdai](./active-directory/role-based-access-built-in-roles.md#user-access-administrator) szerepkör biztosítják. A munkatársi szerepkörök nem rendelkezik a megfelelő hozzáférési.

Beállíthatja, hogy a hatókör az előfizetést, az erőforráscsoport vagy az erőforrás szintjén. Hatókör alsóbb szintre engedélyeit örökli. Például a olvasót szerepkörhöz erőforráscsoport-alkalmazás hozzáadása azt jelenti, hogy meg tudja olvasni az erőforráscsoport és erőforrások tartalmaz.

1. Az alkalmazás a szerepkör hozzárendelése, váltás a klasszikus portálról az [Azure-portálon](https://portal.azure.com).

1. Jelölje be az engedélyek, hogy a szolgáltatás egyszerű hozzárendelése szerepkörhöz. Jelölje ki **az engedélyeket** a fiókjához.

    ![Jelölje ki az engedélyeket.](./media/resource-group-create-service-principal-portal/my-permissions.png)

1. A fiók hozzárendelt engedélyeinek megtekintése. Korábban amint a tulajdonos vagy a felhasználói hozzáférés rendszergazdai szerepkörök tartoznak, vagy az írási hozzáférést biztosít a Microsoft.Authorization a testre szabott szerepet rendelkezik. Az alábbi képen látható az előfizetés, amely nem megfelelő engedélyekkel szerepkörbe alkalmazás hozzárendelése vonatkozó munkatársi szerepkörök rendelt fiók.

    ![az engedélyek megjelenítése](./media/resource-group-create-service-principal-portal/show-permissions.png)

     Ha nem rendelkezik a megfelelő engedélyekkel való hozzáférést egy alkalmazást, be kell bármelyik kérést, hogy az előfizetés rendszergazda felveszi Önt a felhasználói hozzáférés rendszergazdai szerepkör, vagy kéri, hogy a rendszergazda az alkalmazás hozzáférést biztosít.

1. Nyissa meg a hatókör szeretne hozzárendelni, hogy az alkalmazás szintjét. Az előfizetés hatókör a szerepkör hozzárendelése, válassza az **előfizetések**.

     ![Jelölje ki az előfizetés](./media/resource-group-create-service-principal-portal/select-subscription.png)

     Jelölje ki az adott előfizetés hozzárendelése az alkalmazást.

     ![Jelölje ki azt az előfizetést hozzárendelés](./media/resource-group-create-service-principal-portal/select-one-subscription.png)

     Kattintson a jobb felső sarokban az **Access** ikonra.

     ![Jelölje be a hozzáférés](./media/resource-group-create-service-principal-portal/select-access.png)
     
     Vagy erőforrás csoport hatóköre a szerepkör hozzárendelése, nyissa meg azt a erőforráscsoport. Válassza az erőforrás a csoport lap, a **hozzáférés-vezérlés**.

     ![Válassza a felhasználók](./media/resource-group-create-service-principal-portal/select-users.png)

     Az alábbi lépéseket minden olyan hatókör megegyeznek.

2. Jelölje ki a **hozzáadni**.

     ![Válassza a felvétel](./media/resource-group-create-service-principal-portal/select-add.png)

3. Jelölje ki a **olvasó** szerepkört (vagy bármely szerepkört szeretne hozzárendelni az alkalmazás).

     ![szerepkör kiválasztása](./media/resource-group-create-service-principal-portal/select-role.png)

4. Amikor először tekintse át a is hozzáadhat a szerepkör a felhasználók listáját, alkalmazások nem jelenik meg. Csak akkor láthatja, csoport és a felhasználók.

     ![felhasználók megjelenítése](./media/resource-group-create-service-principal-portal/show-users.png)

5. Ha az alkalmazás, meg kell keresnie azt. Kezdje el beírni az alkalmazás nevét, és a rendelkezésre álló lehetőségek listáját változik. Jelölje ki az alkalmazást, megjelenő a listában.

     ![szerepkör hozzárendelése](./media/resource-group-create-service-principal-portal/assign-to-role.png)

6. Válassza a **rendben** a szerepkör hozzárendelése befejezéséhez. Ekkor megjelennek az alkalmazás a listában a cél hozzárendelt erőforrás-csoport szerepkörbe.


Hozzárendelése a felhasználók és szerepkörök a portálon keresztül alkalmazások kapcsolatos további tudnivalókért olvassa el a [szerepkör-hozzárendelések az Azure előfizetés erőforrások való hozzáférés kezelése](role-based-access-control-configure.md#manage-access-using-the-azure-management-portal)című témakört.

## <a name="sample-applications"></a>Minta alkalmazások

A következő példa alkalmazások jelentkezzen be a szolgáltatás egyszerű módját mutatják.

**.NET**

- [Egy SSH telepítését engedélyezve van a .NET sablonnal virtuális](https://azure.microsoft.com/documentation/samples/resource-manager-dotnet-template-deployment/)
- [Azure erőforrások és a .NET erőforrás-csoportok kezelése](https://azure.microsoft.com/documentation/samples/resource-manager-dotnet-resources-and-groups/)

**Java**

- [Java az első lépések – Azure erőforrás-kezelő sablonnal telepítése – források](https://azure.microsoft.com/documentation/samples/resources-java-deploy-using-arm-template/)
- [Java az első lépések – erőforráscsoport kezelése – források](https://azure.microsoft.com/documentation/samples/resources-java-manage-resource-group//)

**Python**

- [Egy SSH telepítését engedélyezve van a virtuális Python lévő sablonok](https://azure.microsoft.com/documentation/samples/resource-manager-python-template-deployment/)
- [Azure erőforrás- és erőforrás-Python csoportok kezelése](https://azure.microsoft.com/documentation/samples/resource-manager-python-resources-and-groups/)

**NODE.js**

- [Egy SSH telepítését engedélyezve van a virtuális Node.js lévő sablonok](https://azure.microsoft.com/documentation/samples/resource-manager-node-template-deployment/)
- [Azure erőforrások és Node.js erőforrás-csoportok kezelése](https://azure.microsoft.com/documentation/samples/resource-manager-node-resources-and-groups/)

**Fonetikus**

- [Egy SSH telepítését engedélyezve van a fonetikus sablonnal virtuális](https://azure.microsoft.com/documentation/samples/resource-manager-ruby-template-deployment/)
- [Azure erőforrás- és erőforrás fonetikus-csoportok kezelése](https://azure.microsoft.com/documentation/samples/resource-manager-ruby-resources-and-groups/)


## <a name="next-steps"></a>Következő lépések

- Eszközbiztonsági házirendek megadásával kapcsolatos további tudnivalókért olvassa el az [Azure szerepköralapú hozzáférés-vezérlés](./active-directory/role-based-access-control-configure.md)című témakört.  
- Szeretné az alábbi lépéseket olvassa el [Az Azure-erőforrás-és az Azure Active Directory-programozott kezelés engedélyezése](https://channel9.msdn.com/Series/Azure-Active-Directory-Videos-Demos/Enabling-Programmatic-Management-of-an-Azure-Resource-with-Azure-Active-Directory).

