<properties
   pageTitle="A PowerShell létrehozása az Azure szolgáltatás fő |} Microsoft Azure"
   description="Azure PowerShell használata az Active Directory-alkalmazások és -szolgáltatás egyszerű létrehozása, és szerepköralapú hozzáférés-vezérlés keresztül erőforrások hozzáférést adni azt ismerteti. Szemlélteti a jelszó vagy a tanúsítvány alkalmazással hitelesítést végezni."
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
   ms.date="09/12/2016"
   ms.author="tomfitz"/>

# <a name="use-azure-powershell-to-create-a-service-principal-to-access-resources"></a>Hozzon létre egy egyszerű erőforrások eléréséhez a Azure PowerShell használatával

> [AZURE.SELECTOR]
- [A PowerShell](resource-group-authenticate-service-principal.md)
- [Azure CLI](resource-group-authenticate-service-principal-cli.md)
- [Portál](resource-group-create-service-principal-portal.md)

Amikor az adott alkalmazás vagy forrásokat kell, hogy a parancsprogram, akkor valószínűleg nem szeretne futtassa ezt a folyamatot, a saját hitelesítő adatok területén. Előfordulhat, hogy az alkalmazás, amelyet más engedélyeket, és nem szeretné, hogy az alkalmazás továbbra is használni a hitelesítő adatait, ha feladata változnak. Ehelyett hoz létre, amely tartalmazza a szerepkör-hozzárendelések és hitelesítő alkalmazásához identitás. Minden alkalommal, amikor az alkalmazás elindul, akkor hitelesíti magát a hitelesítő adatokat. Ez a témakör bemutatja, hogyan állíthat be minden szükséges az alkalmazás futtatásához a saját hitelesítő adatok és -Identitáskezelés [Azure PowerShell](powershell-install-configure.md) használatával.

Az PowerShell az Active Directory-alkalmazás hitelesítő két lehetőség közül választhat, ha van:

 - jelszó
 - tanúsítvány

Ez a témakör bemutatja a PowerShell haszná mindkét kérdésre. Ha azt szeretné, jelentkezhet be az Azure a programozási keretet (például a Python, fonetikus vagy Node.js), a jelszó-hitelesítést valószínűleg a legjobb megoldást. Annak eldöntésében, hogy a jelszó vagy a tanúsítvány, mielőtt a példák a különböző keretek a hitelesítő a [minta alkalmazások](#sample-applications) szakaszban olvashat.

## <a name="active-directory-concepts"></a>Az Active Directory fogalmak

Ebben a cikkben két objektumok – az Active Directory (AD) alkalmazás és a fő szolgáltatás hozzon létre. Az Active Directory-alkalmazás az alkalmazás globális ábrázolása áll. A hitelesítő adatok (alkalmazás azonosítóját és jelszavát vagy tanúsítvány) tartalmaz. A szolgáltatás fő az Active Directory-alkalmazás helyi ábrázolása. Szerepkör-hozzárendelés tartalmaz. Ez a témakör koncentrál egy egyetlen-bérlői alkalmazást, ha az alkalmazás célja, hogy csak egy szervezeten belül futnak. A szokásos egyetlen-bérlői alkalmazások-et futó a vállalati verziós alkalmazásokban a szervezeten belül. Egy egyetlen-bérlői alkalmazásban Ha egy Active Directory-alkalmazást, és egy egyszerű.

Akkor is lehet tudni szeretné, hogy - miért van szükségem mindkét objektumok? Ezt a megközelítést több értelme amikor fontolja meg a több elem bérlői alkalmazásokat. A szokásos több bérlői alkalmazások-et szoftver-mint-a-(szoftver) szolgáltatásalkalmazások, ahol számos különböző előfizetések az alkalmazás fut. Több elem bérlői-alkalmazásokhoz Ha egy Active Directory-alkalmazást, és több szolgáltatás-alapelvei (egy minden, az alkalmazás hozzáférést biztosít az Active Directoryban). A több elem bérlői alkalmazások beállításához [fejlesztői](resource-manager-api-authentication.md)útmutatójában engedély a Azure erőforrás-kezelő API-val.

## <a name="required-permissions"></a>Szükséges engedélyek

Ez a témakör befejezéséhez szükséges engedélyekkel kell rendelkeznie az Azure Active Directory és Azure előfizetését. Pontosabban kell lennie az Active Directory-alkalmazás létrehozása, és a szolgáltatás egyszerű hozzárendelése szerepkörhöz. 

Az Active Directoryban a fiók (például a **Globális rendszergazdának** vagy a **Felhasználó rendszergazda**) rendszergazdának kell lennie. A fiók a **felhasználói** szerepkör van rendelve, ha a rendszergazda a jogosultságszint van szükség.

Az előfizetését, rendelkeznie kell a fiók `Microsoft.Authorization/*/Write` hozzáférés, amely a [tulajdonos](./active-directory/role-based-access-built-in-roles.md#owner) szerepkör vagy a [Felhasználói hozzáférés rendszergazdai](./active-directory/role-based-access-built-in-roles.md#user-access-administrator) szerepkör biztosítják. Ha a fiók vonatkozó **munkatársi** szerepkörök van rendelve, hibaüzenetet kap amikor megpróbál a szolgáltatás egyszerű hozzárendelése szerepkörhöz. Ismét a előfizetés rendszergazdához kell hozzáférést, elegendő.

Ezután folytassa a szakasz a [jelszó](#create-service-principal-with-password) vagy a [tanúsítvány](#create-service-principal-with-certificate) -hitelesítést.

## <a name="create-service-principal-with-password"></a>Hozzon létre szolgáltatás fő jelszóval

Ebben a részben hajtsa végre a lépéseket követve:

- az Active Directory-alkalmazás létrehozása jelszó használatával
- a szolgáltatás egyszerű létrehozása
- a szolgáltatás egyszerű az Olvasó szerepkör hozzárendelése

Gyorsan hajthat végre ezeket a lépéseket, olvassa el a következő három parancsmagok. 

     $app = New-AzureRmADApplication -DisplayName "{app-name}" -HomePage "https://{your-domain}/{app-name}" -IdentifierUris "https://{your-domain}/{app-name}" -Password "{your-password}"
     New-AzureRmADServicePrincipal -ApplicationId $app.ApplicationId
     New-AzureRmRoleAssignment -RoleDefinitionName Reader -ServicePrincipalName $app.ApplicationId.Guid

Most hajtsa végre ezeket a lépéseket még figyelmesen győződjön meg arról, hogy megismeri a folyamat.

1. Bejelentkezés a fiókjába.

        Add-AzureRmAccount

1. Hozzon létre egy új Active Directory-alkalmazást, mert a megjelenítendő név, az alkalmazás leíró URI, az URL-címe, amely azonosítja az alkalmazás és a jelszót az alkalmazás identitáshoz.

        $app = New-AzureRmADApplication -DisplayName "exampleapp" -HomePage "https://www.contoso.org/exampleapp" -IdentifierUris "https://www.contoso.org/exampleapp" -Password "<Your_Password>"

     Az URL-címe egyetlen-bérlői alkalmazások esetén nem érvényesíteni.
     
     Ha a fiókja nem rendelkezik a [szükséges engedélyekkel](#required-permissions) az Active Directory, egy jelző "Authentication_Unauthorized" hibaüzenet jelenik meg, vagy a "Nincs előfizetés keretében található".

1. Tekintse meg az új alkalmazás objektumot. 

        $app
        
     Megjegyzés: különösen a **ApplicationId** tulajdonság, amely a szolgáltatás rendszerbiztonsági, szerepkör-hozzárendelések létrehozásának és a hozzáférési jogkivonat beszerzésekor van szükség.

        DisplayName             : exampleapp
        ObjectId                : c95e67a3-403c-40ac-9377-115fa48f8f39
        IdentifierUris          : {https://www.contoso.org/example}
        HomePage                : https://www.contoso.org
        Type                    : Application
        ApplicationId           : 8bc80782-a916-47c8-a47e-4d76ed755275
        AvailableToOtherTenants : False
        AppPermissions          : 
        ReplyUrls               : {}

2. Létrehozhat egy egyszerű az alkalmazás átadása a az Active Directory-alkalmazás azonosítója.

        New-AzureRmADServicePrincipal -ApplicationId $app.ApplicationId

3. Az előfizetés meg a szolgáltatás fő engedélyek kiosztása. Ebben a példában vesz fel a szolgáltatás egyszerű az **olvasó** szerepkör az előfizetés összes erőforrás olvasási engedélyt. Más szerepkörök, című [RBAC: beépített szerepkörök](./active-directory/role-based-access-built-in-roles.md). A **ServicePrincipalName** paraméter adja meg az alkalmazás létrehozásakor használt **ApplicationId** . 

        New-AzureRmRoleAssignment -RoleDefinitionName Reader -ServicePrincipalName $app.ApplicationId.Guid

    Ha a fiókja nem rendelkezik engedélyekkel rendeljen egy szerepkört, hibaüzenet jelenik meg. Az üzenet arról, hogy a fiókja **nem rendelkezik engedélyezési fölé hatóköre "/ előfizetések / {guid}" a "Microsoft.Authorization/roleAssignments/write" művelet végrehajtásához**. 

Az egész! Az Active Directory-alkalmazás és a szolgáltatás egyszerű állítsa be. A következő szakaszban megtudhatja, hogy hogyan jelentkezzen be a hitelesítő adatok Powershellen keresztül. Ha szeretne a hitelesítő adatok használata az kód alkalmazásban, a [minta alkalmazások](#sample-applications)ugorhat. 

### <a name="provide-credentials-through-powershell"></a>Adja meg a hitelesítő adatok Powershellen keresztül

Most kell jelentkezzen be az alkalmazás műveletek elvégzéséhez.

1. Hozzon létre egy **PSCredential** objektumot, amely tartalmazza a hitelesítő adatait a **Get-hitelesítő** parancs futtatásával. Ez a parancs úgy győződjön meg arról, hogy rendelkezésre álló be szeretné illeszteni futtatása előtt a **ApplicationId** van szüksége.

        $creds = Get-Credential

2. Amikor a rendszer kéri, hogy írja be hitelesítő adatait. A felhasználó nevét használja az alkalmazás létrehozásakor használt **ApplicationId** . A jelszót használja a megadott a fiók létrehozásakor.

     ![Adja meg a hitelesítő adatok](./media/resource-group-authenticate-service-principal/arm-get-credential.png)

2. Amikor bejelentkezik szerint egy egyszerű, meg kell adnia a címtárban bérlői azonosítója az Active Directory számára. A bérlői webhelye fel az Active Directory-példány. Ha csak egy előfizetéssel, használhatja:

        $tenant = (Get-AzureRmSubscription).TenantId
    
     Ha egynél több előfizetése van, adja meg az előfizetést, az Active Directory helye. További tudnivalókért olvassa el a [hogyan Azure előfizetések társítva Azure Active Directory](./active-directory/active-directory-how-subscriptions-associated-directory.md)című témakört.

        $tenant = (Get-AzureRmSubscription -SubscriptionName "Contoso Default").TenantId

4. Jelentkezzen a szolgáltatás egyszerű megadásával, hogy a fiók-e egy egyszerű és a hitelesítő adatok objektum megadásával. 

        Add-AzureRmAccount -Credential $creds -ServicePrincipal -TenantId $tenant
        
     Most már hitelesített az Active Directory alkalmazáshoz létrehozott fő szolgáltatást.

### <a name="save-access-token-to-simplify-log-in"></a>A napló egyszerűsítése érdekében jogkivonat mentése

Adja meg a szolgáltatás fő hitelesítő adatokat, minden alkalommal, amikor a bejelentkezés szükséges elkerülése érdekében a hozzáférési jogkivonat mentheti.

1. Az aktuális jogkivonat egy újabb munkamenetben használatához mentse a profilt.

        Save-AzureRmProfile -Path c:\Users\exampleuser\profile\exampleSP.json
        
     Nyissa meg a profilt, és vizsgálja meg a tartalmát. Figyelje meg, hogy egy hozzáférési jogkivonat tartalmazza. 
        
2. Manuális naplózás az újra, hanem egyszerűen betöltése a profil.

        Select-AzureRmProfile -Path c:\Users\exampleuser\profile\exampleSP.json
        
> [AZURE.NOTE] A hozzáférési jogkivonat jár le, így mentett profillal csak akkor működik, az mindaddig, amíg a token érvényes.
        
## <a name="create-service-principal-with-certificate"></a>Tanúsítvány szolgáltatás fő létrehozása

Ebben a részben hajtsa végre a lépéseket követve:

- önaláírt tanúsítvány létrehozása
- a tanúsítvány az Active Directory-alkalmazás létrehozása
- a szolgáltatás egyszerű létrehozása
- a szolgáltatás egyszerű az Olvasó szerepkör hozzárendelése

Gyorsan hajthat végre ezeket a lépéseket az Azure PowerShell 2.0-s verziója a Windows 10-es vagy Windows Server 2016 technikai előzetes verzió, a következő parancsmagok látható. 

    $cert = New-SelfSignedCertificate -CertStoreLocation "cert:\CurrentUser\My" -Subject "CN=exampleapp" -KeySpec KeyExchange
    $keyValue = [System.Convert]::ToBase64String($cert.GetRawCertData())
    $app = New-AzureRmADApplication -DisplayName "exampleapp" -HomePage "https://www.contoso.org" -IdentifierUris "https://www.contoso.org/example" -CertValue $keyValue -EndDate $cert.NotAfter -StartDate $cert.NotBefore
    New-AzureRmADServicePrincipal -ApplicationId $app.ApplicationId
    New-AzureRmRoleAssignment -RoleDefinitionName Reader -ServicePrincipalName $app.ApplicationId.Guid

Most hajtsa végre ezeket a lépéseket még figyelmesen győződjön meg arról, hogy megismeri a folyamat. Ez a cikk azt is megtudhatja, hogy miként feladatokat operációs rendszerek és Azure PowerShell korábbi verzióiban.

### <a name="create-the-self-signed-certificate"></a>Az önaláírt tanúsítvány létrehozása

A PowerShell érhető el a Windows 10-es és a Windows Server 2016 technikai előzetes verziója az önaláírt tanúsítvány létrehozása egy frissített **Új-SelfSignedCertificate** parancsmag tartalmaz. Korábbi operációs rendszerek a New-SelfSignedCertificate parancsmag van, de nem ajánlja fel a paramétereket, ez a témakör szükséges. Ehelyett kell egy modul kattintva állítsa elő a tanúsítvány importálása. Ez a témakör bemutatja mindkét módszer van az operációs rendszer alapján tanúsítvány létrehozásához. 

- Ha **Windows 10-es vagy Windows Server 2016 technikai előzetes verzió**a következő parancsot önaláírt tanúsítvány létrehozása: 

        $cert = New-SelfSignedCertificate -CertStoreLocation "cert:\CurrentUser\My" -Subject "CN=exampleapp" -KeySpec KeyExchange
       
- Ha **nem rendelkezik a Windows 10 vagy Windows Server 2016 technikai előzetes verziót**, akkor kell Microsoft Script Center [önaláírt tanúsítvány nyilvántartás-készítő alkalmazás](https://gallery.technet.microsoft.com/scriptcenter/Self-signed-certificate-5920a7c6/) letöltése. Bontsa ki a tartalmát, és importálja a parancsmag van szüksége.
     
        # Only run if you could not use New-SelfSignedCertificate
        Import-Module -Name c:\ExtractedModule\New-SelfSignedCertificateEx.ps1
    
     Ezután készítése a tanúsítvány.
    
        $cert = New-SelfSignedCertificateEx -Subject "CN=exampleapp" -KeySpec "Exchange" -FriendlyName "exampleapp"

A tanúsítvány van, és folytathatja az Active Directory-alkalmazás létrehozása.

### <a name="create-the-active-directory-app-and-service-principal"></a>Az Active Directory-alkalmazás és a szolgáltatás fő létrehozása

1. A fő érték beolvasása a tanúsítvány.

        $keyValue = [System.Convert]::ToBase64String($cert.GetRawCertData())

2. Jelentkezzen be az Azure-fiókjába.

        Add-AzureRmAccount

3. Hozzon létre egy új Active Directory-alkalmazást, mert a megjelenítendő név, az alkalmazás leíró URI, az URL-címe, amely azonosítja az alkalmazás és a jelszót az alkalmazás identitáshoz.

     Ha rendelkezik az Azure PowerShell 2.0-s (augusztus 2016 vagy újabb), használja az alábbi parancsmagot:

        $app = New-AzureRmADApplication -DisplayName "exampleapp" -HomePage "https://www.contoso.org" -IdentifierUris "https://www.contoso.org/example" -CertValue $keyValue -EndDate $cert.NotAfter -StartDate $cert.NotBefore      
    
    Ha Azure PowerShell 1.0, használja a következő parancsmagot:
    
        $app = New-AzureRmADApplication -DisplayName "exampleapp" -HomePage "https://www.contoso.org" -IdentifierUris "https://www.contoso.org/example" -KeyValue $keyValue -KeyType AsymmetricX509Cert  -EndDate $cert.NotAfter -StartDate $cert.NotBefore      
    
    Az URL-címe egyetlen-bérlői alkalmazások esetén nem érvényesíteni.
    
    Ha a fiókja nem rendelkezik a [szükséges engedélyekkel](#required-permissions) az Active Directory, egy jelző "Authentication_Unauthorized" hibaüzenet jelenik meg, vagy a "Nincs előfizetés keretében található".
        
    Tekintse meg az új alkalmazás objektumot. 

        $app

    Figyelje meg a **ApplicationId** tulajdonság, amely a szolgáltatás rendszerbiztonsági, szerepkör-hozzárendelések létrehozásának és hozzáférési jogkivonat beszerzésekor van szükség.

        DisplayName             : exampleapp
        ObjectId                : c95e67a3-403c-40ac-9377-115fa48f8f39
        IdentifierUris          : {https://www.contoso.org/example}
        HomePage                : https://www.contoso.org
        Type                    : Application
        ApplicationId           : 8bc80782-a916-47c8-a47e-4d76ed755275
        AvailableToOtherTenants : False
        AppPermissions          : 
        ReplyUrls               : {}


5. Létrehozhat egy egyszerű az alkalmazás átadása a az Active Directory-alkalmazás azonosítója.

        New-AzureRmADServicePrincipal -ApplicationId $app.ApplicationId

6. Az előfizetés meg a szolgáltatás fő engedélyek kiosztása. Ebben a példában vesz fel a szolgáltatás egyszerű az **olvasó** szerepkör az előfizetés összes erőforrás olvasási engedélyt. Más szerepkörök, című [RBAC: beépített szerepkörök](./active-directory/role-based-access-built-in-roles.md). A **ServicePrincipalName** paraméter adja meg az alkalmazás létrehozásakor használt **ApplicationId** .

        New-AzureRmRoleAssignment -RoleDefinitionName Reader -ServicePrincipalName $app.ApplicationId.Guid

    Ha a fiókja nem rendelkezik engedélyekkel rendeljen egy szerepkört, hibaüzenet jelenik meg. Az üzenet arról, hogy a fiókja **nem rendelkezik engedélyezési fölé hatóköre "/ előfizetések / {guid}" a "Microsoft.Authorization/roleAssignments/write" művelet végrehajtásához**.

Az egész! Az Active Directory-alkalmazás és a szolgáltatás egyszerű állítsa be. A következő szakaszban megtudhatja, hogy hogyan jelentkezzen be a tanúsítvány Powershellen keresztül.

### <a name="provide-certificate-through-automated-powershell-script"></a>Adja meg a tanúsítvány automatikus PowerShell-parancsprogramot keresztül

Amikor bejelentkezik szerint egy egyszerű, meg kell adnia a címtárban bérlői azonosítója az Active Directory számára. A bérlői webhelye fel az Active Directory-példány. Ha csak egy előfizetéssel, használhatja:

    $tenant = (Get-AzureRmSubscription).TenantId
    
Ha egynél több előfizetése van, adja meg az előfizetést, az Active Directory helye. További tudnivalókért olvassa el a [rendszergazda az Azure Active directory](./active-directory/active-directory-administer.md)című témakört.

    $tenant = (Get-AzureRmSubscription -SubscriptionName "Contoso Default").TenantId

Hitelesítést végezni a parancsfájl, adja meg a fiók egy egyszerű szolgáltatás, és adja meg, a tanúsítvány ujjlenyomat azonosítója, és az bérlői azonosítója. Automatizálható a parancsfájlt, ezeket az értékeket a környezeti változók tárolhatja, és beolvasni a végrehajtás során, vagy azokat is beleveheti a parancsprogram.

    Add-AzureRmAccount -ServicePrincipal -CertificateThumbprint $cert.Thumbprint -ApplicationId $app.ApplicationId -TenantId $tenant

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
- Az alkalmazások és a szolgáltatás rendszerbiztonsági részletes leírást lásd: az [alkalmazás és a szolgáltatás egyszerű objektumok](./active-directory/active-directory-application-objects.md). 
- Az Active Directory authentication kapcsolatos további tudnivalókért lásd: [Az Azure Active Directory Authentication esetek](./active-directory/active-directory-authentication-scenarios.md).



