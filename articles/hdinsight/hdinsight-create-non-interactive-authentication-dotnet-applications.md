<properties
    pageTitle="Hozzon létre nem interaktív hitelesítési .NET HDInsight applciations |} Microsoft Azure"
    description="Megtudhatja, hogy miként .NET HDInsight-alkalmazások létrehozása interaktív hitelesítést."
    editor="cgronlun"
    manager="jhubbard"
    services="hdinsight"
    documentationCenter=""
    tags="azure-portal"
    authors="mumian"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/02/2016"
    ms.author="jgao"/>

# <a name="create-non-interactive-authentication-net-hdinsight-applications"></a>Interaktív hitelesítés .NET HDInsight-alkalmazások létrehozása

A .NET Azure hdinsight szolgáltatáshoz alkalmazás alkalmazás saját identitással (nem interaktív), vagy az alkalmazás (interaktív) jelentkezett a felhasználó a hajthat végre. Az interaktív alkalmazás minta [elküldése struktúra/malac/Sqoop feladatok HDInsight .NET SDK használatával](hdinsight-submit-hadoop-jobs-programmatically.md#submit-hivepigsqoop-jobs-using-hdinsight-net-sdk)című témakör tartalmaz. Ez a cikk bemutatja, hogyan hozhat létre, nem interaktív hitelesítési .NET alkalmazást Azure hdinsight szolgáltatáshoz csatlakozhat, és a struktúra feladat elküldése.

A .NET levelezőprogramból lesz szüksége:

- az Azure előfizetés bérlői azonosítója
- az Azure Directory ügyfél-azonosító
- az Azure-címtár-alkalmazás titkos kulcs.  

A fő folyamat a következő lépésekből áll:

2. Azure címtár-alkalmazás létrehozása.
2. Szerepkörök hozzárendelése az Active Directory-alkalmazást.
3. Az ügyfélalkalmazás fejlesztése.


##<a name="prerequisites"></a>Előfeltételek

- Fürt hdinsight szolgáltatásból lehetőségre. Létrehozhat egyet az [első lépések az oktatóprogram](hdinsight-hadoop-linux-tutorial-get-started.md#create-cluster)található utasításokat. 




## <a name="create-azure-directory-application"></a>Azure címtár-alkalmazás létrehozása 
Az Active Directory-alkalmazás létrehozásakor ténylegesen létrehoz az alkalmazás és a szolgáltatás egyszerű. Az alkalmazás az alkalmazás azonosítóját a hajthat végre.

Jelenleg az Active Directory új alkalmazás létrehozása az Azure klasszikus portál kell használnia. Ez a lehetőség, hozzáadódik az Azure-portálra az későbbi kiadásokban. Ezeket a lépéseket, Azure PowerShell vagy Azure CLI keresztül is elvégezheti. A PowerShell alrendszerrel vagy CLI fő szolgáltatással kapcsolatos további tudnivalókért lásd: a [hitelesítés szolgáltatás fő Azure erőforrás-kezelővel](../resource-group-authenticate-service-principal.md).

**Azure címtár-alkalmazás létrehozása**

1.  Jelentkezzen be az [Azure klasszikus portálon]( https://manage.windowsazure.com/).
2.  A bal oldali ablaktáblában válassza ki **Az Active Directory** .

    ![Azure klasszikus portál az active directory](.\media\hdinsight-create-non-interactive-authentication-dotnet-application\active-directory.png)
    
3.  Jelölje be az új alkalmazás létrehozásához használni kívánt könyvtár. A meglévőt kell lennie.
4.  A meglévő alkalmazásokat felsoroló tetején kattintson az **alkalmazások** elemre.
5.  Új alkalmazás hozzáadása lentről felfelé, kattintson a **Hozzáadás** gombra.
6.  Adja meg **nevét**, jelölje be a **webes alkalmazás és/vagy a webes API -val**, és kattintson a **Tovább gombra**.

    ![új azure active directory-alkalmazás](.\media\hdinsight-create-non-interactive-authentication-dotnet-application\hdinsight-add-ad-application.png)

7.  Írja be **bejelentkezési URL** és az **Alkalmazás azonosítója URI**. **SIGN-ON URL-CÍMÉT**adja meg a URI webhelyre, hogy az alkalmazás ismerteti. A webhely megléte érvényesítése nem volt. Az alkalmazás azonosítója URI a URI, amely azonosítja az alkalmazás biztosítanak. És kattintson a **kész**gombra.
Az alkalmazás létrehozása néhány percet vesz igénybe.  Amikor az alkalmazás létrejött, a portálon megtekintheti a rövid Glace lap az új alkalmazás. Ne zárja be a portálon. 

    ![azure active directory-alkalmazás új tulajdonságai](.\media\hdinsight-create-non-interactive-authentication-dotnet-application\hdinsight-add-ad-application-properties.png)

**Az alkalmazás ügyfél megszerezni azonosító és a titkos kulcs**

1.  Az újonnan létrehozott AD alkalmazás lapon kattintson a **beállítás** a felső menüből.
2.  **Ügyfél-azonosító**egy másolatot készíteni. A .NET-alkalmazásban kell.
3.  Kattintson a **billentyűk**, területen **Jelölje be az időtartamot** legördülő menü, és válassza a **1 év** és a **2 éves**. A kulcs értékét nem jelenik meg, amíg nem menti a konfiguráción.
4.  A lap alján kattintson a **Mentés** gombra. Amikor megjelenik a titkos kulcs, másolat készítése a billentyűt. A .NET-alkalmazásban kell.

##<a name="assign-ad-application-to-role"></a>Active Directory-alkalmazás szerepkör hozzárendelése

Az alkalmazás [szerepkör](../active-directory/role-based-access-built-in-roles.md) műveletek elvégzésére vonatkozó engedélyek ad hozzá kell rendelnie. Beállíthatja, hogy a hatókör az előfizetést, az erőforráscsoport vagy az erőforrás szintjén. Az engedélyek alsó szinteket (például egy alkalmazást, az olvasói szerepkörhöz erőforráscsoport azt jelenti, hogy meg tudja olvasni az erőforráscsoport hozzáadása és erőforrások tartalmaz), a hatókör származnak. Ebben az oktatóanyagban hatókör fog beállítása erőforrás csoportszinten.  Az Azure klasszikus portálon nem támogatja az erőforrás csoportok, mivel ebben a részben tartalmaz, az Azure portálról végre kell hajtani. 

**A tulajdonos szerepkör hozzáadása az Active Directory-alkalmazás**

1.  Jelentkezzen be az [Azure-portálon](https://portal.azure.com).
2.  A bal oldali ablaktáblában kattintson a **Erőforráscsoport** elemre.
3.  Kattintson az erőforrás csoportra, amely tartalmazza a HDInsight fürt, ahol futtatható a struktúra lekérdezés később az oktatóprogram. Ha túl sok erőforrást csoportok, a szűrő is használhatja.
4.  Kattintson **az Access** a fürt lap.

    ![felhőalapú és thunderbolt ikon = quickstart útmutató](./media/hdinsight-hadoop-create-linux-cluster-portal/quickstart.png)
5.  A **felhasználók** lap a, kattintson a **Hozzáadás** gombra.
6.  Kövesse a **tulajdonos** szerepkör hozzáadása az Active Directory-alkalmazást az utolsó eljárás létrehozott utasítás. Befejezése után sikeresen, amikor az alkalmazás, a felhasználók lap a tulajdonos szerepkörrel felsorolt kell látni.


##<a name="develop-hdinsight-client-application"></a>HDInsight ügyfélalkalmazás kidolgozása

A [HDInsight elküldése Hadoop feladatok](hdinsight-submit-hadoop-jobs-programmatically.md#submit-hivepigsqoop-jobs-using-hdinsight-net-sdk)található útmutatást követve C# .net console-alkalmazás létrehozása. Majd cserélje ki a GetTokenCloudCredentials módszer a következőket:

    public static TokenCloudCredentials GetTokenCloudCredentials(string tenantId, string clientId, SecureString secretKey)
    {
        var authFactory = new AuthenticationFactory();

        var account = new AzureAccount { Type = AzureAccount.AccountType.ServicePrincipal, Id = clientId };

        var env = AzureEnvironment.PublicEnvironments[EnvironmentName.AzureCloud];

        var accessToken =
            authFactory.Authenticate(account, env, tenantId, secretKey, ShowDialog.Never)
                .AccessToken;

        return new TokenCloudCredentials(accessToken);
    }

A bérlői azonosító Powershellen keresztül beolvasásához:

    Get-AzureRmSubscription

Másik lehetőségként Azure CLI:

    azure account show --json

      
## <a name="see-also"></a>Lásd még:

- [A HDInsight Hadoop-feladatok elküldése](hdinsight-submit-hadoop-jobs-programmatically.md)
- [Az Active Directory-alkalmazás és a portálon szolgáltatás egyszerű létrehozása](../resource-group-create-service-principal-portal.md)
- [Hitelesítő szolgáltatás egyszerű a Azure-kezelő eszközzel](../resource-group-authenticate-service-principal.md)
- [Azure szerepköralapú hozzáférés-vezérlés](../active-directory/role-based-access-control-configure.md)
