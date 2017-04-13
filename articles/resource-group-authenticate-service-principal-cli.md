<properties
   pageTitle="Szolgáltatás fő létrehozása az Azure CLI |} Microsoft Azure"
   description="Azure CLI használja az Active Directory-alkalmazások és -szolgáltatás egyszerű létrehozása, és szerepköralapú hozzáférés-vezérlés keresztül erőforrások hozzáférést adni azt ismerteti. Szemlélteti a jelszó vagy a tanúsítvány alkalmazással hitelesítést végezni."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="tfitzmac"
   manager="timlt"
   editor="tysonn"/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="multiple"
   ms.workload="na"
   ms.date="09/30/2016"
   ms.author="tomfitz"/>

# <a name="use-azure-cli-to-create-a-service-principal-to-access-resources"></a>Hozzon létre egy egyszerű erőforrások eléréséhez az Azure CLI használatával

> [AZURE.SELECTOR]
- [A PowerShell](resource-group-authenticate-service-principal.md)
- [Azure CLI](resource-group-authenticate-service-principal-cli.md)
- [Portál](resource-group-create-service-principal-portal.md)


Amikor az adott alkalmazás vagy forrásokat kell, hogy a parancsprogram, akkor valószínűleg nem szeretne futtassa ezt a folyamatot, a saját hitelesítő adatok területén. Előfordulhat, hogy az alkalmazás, amelyet más engedélyeket, és nem szeretné, hogy az alkalmazás továbbra is használni a hitelesítő adatait, ha feladata változnak. Ehelyett hoz létre, amely tartalmazza a szerepkör-hozzárendelések és hitelesítő alkalmazásához identitás. Minden alkalommal, amikor az alkalmazás elindul, akkor hitelesíti magát a hitelesítő adatokat. Ez a témakör bemutatja, hogyan állíthatja be a saját hitelesítő adatok és -Identitáskezelés futó alkalmazás [For Mac, Linux és a Windows Azure CLI](xplat-cli-install.md) használatával.

Az Azure CLI az Active Directory-alkalmazás hitelesítő két lehetőség közül választhat, ha van:

 - jelszó
 - tanúsítvány

Ez a témakör bemutatja, hogyan Azure CLI mindkettőt használni. Ha azt szeretné, jelentkezhet be az Azure a programozási keretet (például a Python, fonetikus vagy Node.js), a jelszó-hitelesítést valószínűleg a legjobb megoldást. Annak eldöntésében, hogy a jelszó vagy a tanúsítvány, mielőtt a példák a különböző keretek a hitelesítő a [minta alkalmazások](#sample-applications) szakaszban olvashat.

## <a name="active-directory-concepts"></a>Az Active Directory fogalmak

Ebben a cikkben két objektumok – az Active Directory (AD) alkalmazás és a fő szolgáltatás hozzon létre. Az Active Directory-alkalmazás az alkalmazás globális ábrázolása áll. A hitelesítő adatok (alkalmazás azonosítóját és jelszavát vagy tanúsítvány) tartalmaz. A szolgáltatás fő az Active Directory-alkalmazás helyi ábrázolása. Szerepkör-hozzárendelés tartalmaz. Ez a témakör koncentrál egy egyetlen-bérlői alkalmazást, ha az alkalmazás célja, hogy csak egy szervezeten belül futnak. A szokásos egyetlen-bérlői alkalmazások-et futó a vállalati verziós alkalmazásokban a szervezeten belül. Egy egyetlen-bérlői alkalmazásban Ha egy Active Directory-alkalmazást, és egy egyszerű.

Akkor is lehet tudni szeretné, hogy - miért van szükségem mindkét objektumok? Ezt a megközelítést több értelme amikor fontolja meg a több elem bérlői alkalmazásokat. A szokásos több bérlői alkalmazások-et szoftver-mint-a-(szoftver) szolgáltatásalkalmazások, ahol számos különböző előfizetések az alkalmazás fut. Több elem bérlői-alkalmazásokhoz Ha egy Active Directory-alkalmazást, és több szolgáltatás-alapelvei (egy minden, az alkalmazás hozzáférést biztosít az Active Directoryban). A több elem bérlői alkalmazások beállításához [fejlesztői](resource-manager-api-authentication.md)útmutatójában engedély a Azure erőforrás-kezelő API-val.

## <a name="required-permissions"></a>Szükséges engedélyek

Ez a témakör befejezéséhez szükséges engedélyekkel kell rendelkeznie az Azure Active Directory és Azure előfizetését. Pontosabban kell lennie az Active Directory-alkalmazás létrehozása, és a szolgáltatás egyszerű hozzárendelése szerepkörhöz. 

Az Active Directoryban a fiók (például a **Globális rendszergazdának** vagy a **Felhasználó rendszergazda**) rendszergazdának kell lennie. A fiók a **felhasználói** szerepkör van rendelve, ha a rendszergazda a jogosultságszint van szükség.

Az előfizetését, rendelkeznie kell a fiók `Microsoft.Authorization/*/Write` hozzáférés, amely a [tulajdonos](./active-directory/role-based-access-built-in-roles.md#owner) szerepkör vagy a [Felhasználói hozzáférés rendszergazdai](./active-directory/role-based-access-built-in-roles.md#user-access-administrator) szerepkör biztosítják. Ha a fiók vonatkozó **munkatársi** szerepkörök van rendelve, hibaüzenetet kap amikor megpróbál a szolgáltatás egyszerű hozzárendelése szerepkörhöz. Ismét a előfizetés rendszergazdához kell hozzáférést, elegendő.

Ezután folytassa a szakasz a [jelszó](#create-service-principal-with-password) vagy a [tanúsítvány](#create-service-principal-with-certificate) -hitelesítést.

## <a name="create-service-principal-with-password"></a>Hozzon létre szolgáltatás fő jelszóval

Ebben a részben hajtsa végre a lépéseket az Active Directory-alkalmazás létrehozása egy jelszót, és a olvasó szerepkör hozzárendelése a legfontosabb szolgáltatás.

Ezek a lépések megkönnyítésére vegyük sorra lépjen.

1. Bejelentkezés a fiókjába.

        azure login

1. Az Active Directory-alkalmazás készítéséhez két lehetőség közül választhat. Az Active Directory-alkalmazás és a szolgáltatás egy lépésben egyszerű létrehozása, vagy hozzon létre őket külön-külön. Hozzon létre őket egy lépésben Ha nincs szükség, adja meg a Kezdőlap lap és az alkalmazás azonosítója URL-címe. Ha hozzon létre őket külön-külön kell beállítania egy webalkalmazás ezeket az értékeket. Mindkét lehetőségek ebben a lépésben.

     - Az Active Directory-alkalmazás vagy szolgáltatás hozzon létre egy lépésben egyszerű, adja meg a nevét az alkalmazás és a jelszavát, ahogy az alábbi parancsot:
     
            azure ad sp create -n exampleapp -p {your-password}     
     
     - Külön-külön létrehozása az Active Directory-alkalmazást, adja meg a nevét az alkalmazás, kezdőlap URI, azonosító URL-címe és a jelszavát, ahogy az alábbi parancsot:
     
            azure ad app create -n exampleapp --home-page http://www.contoso.org --identifier-uris https://www.contoso.org/example -p <Your_Password>

         Az előző parancs AppId értéket adja eredményül. A szolgáltatás rendszerbiztonsági tag létrehozásához adja meg ezt az értéket a következő parancsot a paraméterként:
     
            azure ad sp create -a <AppId>
     
     Ha a fiókja nem rendelkezik a [szükséges engedélyekkel](#required-permissions) az Active Directory, egy jelző "Authentication_Unauthorized" hibaüzenet jelenik meg, vagy a "Nincs előfizetés keretében található".
    
     Mindkét kérdésre az új szolgáltatás egyszerű adja vissza. A **Objektumazonosító** van szükség, ha a engedélyek megadása. Megjelenik a **Szolgáltatás egyszerű nevek** és guid van szükség, ha a naplózás a. A globálisan egyedi azonosítója érték megegyezik az alkalmazás azonosítója. A minta alkalmazásban ez az érték hivatkozik az **Ügyfél-azonosító**. 
    
        info:    Executing command ad sp create
        + Creating application exampleapp
        / Creating service principal for application 7132aca4-1bdb-4238-ad81-996ff91d8db+
        data:    Object Id:               ff863613-e5e2-4a6b-af07-fff6f2de3f4e
        data:    Display Name:            exampleapp
        data:    Service Principal Names:
        data:                             7132aca4-1bdb-4238-ad81-996ff91d8db4
        data:                             https://www.contoso.org/example
        info:    ad sp create command OK

2. Az előfizetés meg a szolgáltatás fő engedélyek kiosztása. Ebben a példában vesz fel a szolgáltatás egyszerű az **olvasó** szerepkör az előfizetés összes erőforrás olvasási engedélyt. Más szerepkörök, című [RBAC: beépített szerepkörök](./active-directory/role-based-access-built-in-roles.md). A **ServicePrincipalName** paraméter adja meg az alkalmazás létrehozásakor használt **objektumazonosító** . 

        azure role assignment create --objectId ff863613-e5e2-4a6b-af07-fff6f2de3f4e -o Reader -c /subscriptions/{subscriptionId}/

     Ha a fiókja nem rendelkezik engedélyekkel rendeljen egy szerepkört, hibaüzenet jelenik meg. Az üzenet arról, hogy a fiókja **nem rendelkezik engedélyezési fölé hatóköre "/ előfizetések / {guid}" a "Microsoft.Authorization/roleAssignments/write" művelet végrehajtásához**. 

Az egész! Az Active Directory-alkalmazás és a szolgáltatás egyszerű állítsa be. A következő szakaszban megtudhatja, hogy hogyan jelentkezzen be a hitelesítő adatok Azure CLI keresztül. Ha szeretne a hitelesítő adatok használata az kód alkalmazásban, szükségtelen ez a témakör folytatásához. Példák a naplózás be az alkalmazás azonosítójával és jelszavával [minta alkalmazások](#sample-applications) ugorhat. 

### <a name="provide-credentials-through-azure-cli"></a>Adja meg a hitelesítő adatok Azure CLI keresztül

Most kell jelentkezzen be az alkalmazás műveletek elvégzéséhez.

1. Amikor bejelentkezik szerint egy egyszerű, meg kell adnia a címtárban bérlői azonosítója az Active Directory számára. A bérlői webhelye fel az Active Directory-példány. A bérlői azonosító jelenleg hitelesített előfizetéshez tartozó lekérdezni használja:

        azure account show

     Amely adja eredményül:

        info:    Executing command account show
        data:    Name                        : Windows Azure MSDN - Visual Studio Ultimate
        data:    ID                          : {guid}
        data:    State                       : Enabled
        data:    Tenant ID                   : {guid}
        data:    Is Default                  : true
        ...

     Ha egy másik előfizetés a bérlői ID azonosító beszerzése van szükség, használja az alábbi parancsot:

        azure account show -s {subscription-id}

2. Ha az ügyfél-azonosító naplózás a használandó beolvasásához, használja:

        azure ad sp show -c exampleapp --json

     Naplózás a használandó értéke a szerepelnek a szolgáltatásnevek globálisan egyedi azonosítója.

        [
          {
            "objectId": "ff863613-e5e2-4a6b-af07-fff6f2de3f4e",
            "objectType": "ServicePrincipal",
            "displayName": "exampleapp",
            "appId": "7132aca4-1bdb-4238-ad81-996ff91d8db4",
            "servicePrincipalNames": [
              "https://www.contoso.org/example",
              "7132aca4-1bdb-4238-ad81-996ff91d8db4"
            ]
          }
        ]

3. Jelentkezzen be a szolgáltatás egyszerű.

        azure login -u 7132aca4-1bdb-4238-ad81-996ff91d8db4 --service-principal --tenant {tenant-id}

    A jelszó megadását kéri. Adja meg a jelszót, az Active Directory-alkalmazás létrehozásakor meghatározott.

        info:    Executing command login
        Password: ********

Most már hitelesített, a szolgáltatás tőketörlesztés az Ön által létrehozott szolgáltatás egyszerű.

## <a name="create-service-principal-with-certificate"></a>Tanúsítvány szolgáltatás fő létrehozása

Ebben a részben hajtsa végre a lépéseket követve:

- önaláírt tanúsítvány létrehozása
- a tanúsítvány és a szolgáltatás egyszerű az Active Directory-alkalmazás létrehozása
- a szolgáltatás egyszerű az Olvasó szerepkör hozzárendelése

A lépések elvégzéséhez [OpenSSL](http://www.openssl.org/) telepítve kell rendelkeznie.

1. Önaláírt tanúsítvány létrehozása.

        openssl req -x509 -days 3650 -newkey rsa:2048 -out cert.pem -nodes -subj '/CN=exampleapp'

2. A nyilvános és titkos kulcsokat össze.

        cat privkey.pem cert.pem > examplecert.pem

3. Nyissa meg a **examplecert.pem** fájlt, és keresse meg a hosszú sorozat **---KEZDŐ tanúsítvány---** és **---BEFEJEZHETI a tanúsítvány---**között. A tanúsítvány adatok másolása Az adatok sikeres paraméterként létrehozásakor a szolgáltatás fő.

1. Bejelentkezés a fiókjába.

        azure login

1. Az Active Directory-alkalmazás készítéséhez két lehetőség közül választhat. Az Active Directory-alkalmazás és a szolgáltatás egy lépésben egyszerű létrehozása, vagy hozzon létre őket külön-külön. Hozzon létre őket egy lépésben Ha nincs szükség, adja meg a Kezdőlap lap és az alkalmazás azonosítója URL-címe. Ha hozzon létre őket külön-külön kell beállítania egy webalkalmazás ezeket az értékeket. Mindkét lehetőségek ebben a lépésben.

     - Az Active Directory-alkalmazás vagy szolgáltatás hozzon létre egy lépésben egyszerű, adja meg a nevét az alkalmazás és a tanúsítvány-adatok, ahogy az alábbi parancsot:
     
            azure ad sp create -n exampleapp --cert-value <certificate data>
     
     - Az Active Directory-alkalmazás külön-külön létrehozásához adja meg nevét az alkalmazás, Kezdőlap lap URI, azonosító URL-címe és a tanúsítvány adatai, ahogy az alábbi parancsot:
     
            azure ad app create -n exampleapp --home-page http://www.contoso.org --identifier-uris https://www.contoso.org/example --cert-value <certificate data>

         Az előző parancs AppId értéket adja eredményül. A szolgáltatás rendszerbiztonsági tag létrehozásához adja meg ezt az értéket a következő parancsot a paraméterként:
     
            azure ad sp create -a <AppId>
  
     Ha a fiókja nem rendelkezik a [szükséges engedélyekkel](#required-permissions) az Active Directory, egy jelző "Authentication_Unauthorized" hibaüzenet jelenik meg, vagy a "Nincs előfizetés keretében található".
    
     Mindkét kérdésre az új szolgáltatás egyszerű adja vissza. Az objektum azonosító van szükség, ha a engedélyek megadása. Megjelenik a **Szolgáltatás egyszerű nevek** és guid van szükség, ha a naplózás a. A globálisan egyedi azonosítója érték megegyezik az alkalmazás azonosítója. A minta alkalmazásban ez az érték hivatkozik az **Ügyfél-azonosító**. 
    
        info:    Executing command ad sp create
        - Creating service principal for application 4fd39843-c338-417d-b549-a545f584a74+
        data:    Object Id:        7dbc8265-51ed-4038-8e13-31948c7f4ce7
        data:    Display Name:     exampleapp
        data:    Service Principal Names:
        data:                      4fd39843-c338-417d-b549-a545f584a745
        data:                      https://www.contoso.org/example
        info:    ad sp create command OK
        
2. Az előfizetés meg a szolgáltatás fő engedélyek kiosztása. Ebben a példában vesz fel a szolgáltatás egyszerű az **olvasó** szerepkör az előfizetés összes erőforrás olvasási engedélyt. Más szerepkörök, című [RBAC: beépített szerepkörök](./active-directory/role-based-access-built-in-roles.md). A **ServicePrincipalName** paraméter adja meg az alkalmazás létrehozásakor használt **objektumazonosító** . 

        azure role assignment create --objectId 7dbc8265-51ed-4038-8e13-31948c7f4ce7 -o Reader -c /subscriptions/{subscriptionId}/

     Ha a fiókja nem rendelkezik engedélyekkel rendeljen egy szerepkört, hibaüzenet jelenik meg. Az üzenet arról, hogy a fiókja **nem rendelkezik engedélyezési fölé hatóköre "/ előfizetések / {guid}" a "Microsoft.Authorization/roleAssignments/write" művelet végrehajtásához**. 

### <a name="provide-certificate-through-automated-azure-cli-script"></a>Adja meg a tanúsítvány keresztül automatizált Azure CLI parancsfájl

Most kell jelentkezzen be az alkalmazás műveletek elvégzéséhez.

1. Amikor bejelentkezik szerint egy egyszerű, meg kell adnia a címtárban bérlői azonosítója az Active Directory számára. A bérlői webhelye fel az Active Directory-példány. A bérlői azonosító jelenleg hitelesített előfizetéshez tartozó lekérdezni használja:

        azure account show

     Amely adja eredményül:

        info:    Executing command account show
        data:    Name                        : Windows Azure MSDN - Visual Studio Ultimate
        data:    ID                          : {guid}
        data:    State                       : Enabled
        data:    Tenant ID                   : {guid}
        data:    Is Default                  : true
        ...

     Ha egy másik előfizetés a bérlői ID azonosító beszerzése van szükség, használja az alábbi parancsot:

        azure account show -s {subscription-id}

1. A tanúsítvány ujjlenyomat beolvasásához és távolítsa el a felesleges karaktereket használhatja:

        openssl x509 -in "C:\certificates\examplecert.pem" -fingerprint -noout | sed 's/SHA1 Fingerprint=//g'  | sed 's/://g'
    
     Amely értéket ad eredményül ujjlenyomat hasonló:

        30996D9CE48A0B6E0CD49DBB9A48059BF9355851

2. Ha az ügyfél-azonosító naplózás a használandó beolvasásához, használja:

        azure ad sp show -c exampleapp

     Naplózás a használandó értéke a szerepelnek a szolgáltatásnevek globálisan egyedi azonosítója.

        [
          {
            "objectId": "7dbc8265-51ed-4038-8e13-31948c7f4ce7",
            "objectType": "ServicePrincipal",
            "displayName": "exampleapp",
            "appId": "4fd39843-c338-417d-b549-a545f584a745",
            "servicePrincipalNames": [
              "https://www.contoso.org/example",
              "4fd39843-c338-417d-b549-a545f584a745"
            ]
          }
        ]

1. Jelentkezzen be a szolgáltatás egyszerű.

        azure login --service-principal --tenant {tenant-id} -u 4fd39843-c338-417d-b549-a545f584a745 --certificate-file C:\certificates\examplecert.pem --thumbprint {thumbprint}

Most már hitelesített az Active Directory alkalmazáshoz létrehozott fő szolgáltatást.

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
  
- A lépések részletes ismertetése a kérelmet integrálása az Azure erőforrások kezelésére szolgáló [fejlesztői](resource-manager-api-authentication.md)útmutatójában engedély a Azure erőforrás-kezelő API-val.
- További információt a tanúsítványok és Azure CLI használatával című témakörben kaphat [az Azure Service rendszerbiztonsági Linux parancssorból tanúsítvány-alapú hitelesítés](http://blogs.msdn.com/b/arsen/archive/2015/09/18/certificate-based-auth-with-azure-service-principals-from-linux-command-line.aspx). 
