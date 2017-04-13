<properties
    pageTitle="Azure kormányzati szolgáltatások |} Microsoft Azure"
    description="Az előfizetés az Azure kormányzati kezeléséről"
    services="Azure-Government"
    cloud="gov" 
    documentationCenter=""
    authors="zakramer"
    manager="liki"
    editor="" />

<tags
    ms.service="multiple"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="azure-government"
    ms.date="10/21/2016"
    ms.author="zakramer" />


#  <a name="managing-and-connecting-to-your-subscription-in-azure-government"></a>Kezelés és az előfizetés az Azure kormányzati csatlakozás

Azure kormányzati egyedi URL-EK és kezelése a környezettől végpontokat tartalmaz. Fontos a megfelelő kapcsolatot használhat kezelése a környezettől PowerShell vagy a portálon keresztül. Miután csatlakozott az Azure kormányzati környezetbe, a szolgáltatás kezelésére szolgáló a szokásos műveletek működik, ha lett telepítve az összetevőt.

## <a name="connecting-via-the-portal"></a>Csatlakozás a portálon keresztül
A portál módja az elsődleges, amely a legtöbben Azure kormányzati csatlakozni.  Szeretne csatlakozni, nyissa meg a portált a [https://portal.azure.us](https://portal.azure.us).  Az Azure portál régi verzióját [https://manage.windowsazure.us](https://manage.windowsazure.us)keresztül is elérhető.

Az előfizetések [https://account.windowsazure.us](https://account.windowsazure.us)összekapcsolásával hozhat létre a fiókjához.

## <a name="connecting-via-powershell"></a>Csatlakozás a PowerShell használatával

Azure PowerShell e esetén kezelheti a nagy-előfizetés keretében parancsfájlt vagy a hozzáférési funkciók, amelyek nem jelenleg elérhető az Azure-portálon Azure kormányzati helyett Azure nyilvános csatlakozni szeretne.  Ha Powershellt használta az Azure nyilvános, akkor elsősorban azonos.  Azure kormányzati közötti különbségek a következők:

+ A fiókhoz
+ Régió nevek

>[AZURE.NOTE] Ha még nem használt PowerShell, olvassa el az [Azure PowerShell bemutatása](../powershell-install-configure.md).

A PowerShell indításakor van állapítható meg, hogy a Azure PowerShell-környezet paraméter megadásával Azure kormányzati csatlakozni.  A paraméter biztosítja, hogy a megfelelő végpontok PowerShell-hoz csatlakozik.  Bejelentkezés a fiókjába való csatlakozáskor a végpontok gyűjtemény határozza meg.  Különböző API csak a Váltás a környezet különböző verziói:

Kapcsolat típusa | Parancs
---|----
[Szolgáltatás kezeléséhez](https://msdn.microsoft.com/library/dn708504.aspx) szükséges parancsokat | `Add-AzureAccount -Environment AzureUSGovernment`
[Erőforrás-kezelés](https://msdn.microsoft.com/library/mt125356.aspx) parancsok | `Add-AzureRmAccount -EnvironmentName AzureUSGovernment`
[Azure Active Directory](https://msdn.microsoft.com/library/azure/jj151815.aspx) -parancsok | `Connect-MsolService -AzureEnvironment UsGovernment`
[Azure Active Directory parancs v2](https://msdn.microsoft.com/library/azure/mt757189.aspx) | `Connect-AzureAD -AzureEnvironmentName AzureUSGovernment`

Előfordulhat, hogy a kapcsoló használatával az környezet egy új-AzureStorageContext használatával tárolás fiókhoz történő csatlakozáskor, és adja meg a AzureUSGovernment.

### <a name="determining-region"></a>Annak megállapítása, régió

Miután csatlakozott, egy további különbség van – a szolgáltatás célként használt régiók.  Minden Azure felhő különböző régiók tartalmaz.  Ön láthatja őket az szolgáltatás elérhetősége lapon.  A szokásos módon használhatja az a hely paraméter területhez parancs.

Van egy tényleges.  Az Azure kormányzati régiók a formázandó közös nevük eltérően kell:

Közös neve | Parancs
---|----
Amerikai Egyesült Államok Gov Virginia | USGov Virginia
Amerikai Egyesült Államok Gov Iowa | USGov Iowa

>[AZURE.NOTE] A hely paraméter használatával, nincs hely között az Amerikai Egyesült Államokban és Gov.

Ha minden eddiginél érvényesíteni szeretné a rendelkezésre álló régiók az Azure kormányzati, a következő parancsokat, és az aktuális lista nyomtatása:

    Get-AzureLocation

Ha kíváncsi, a rendelkezésre álló környezetben Azure keresztül, akkor futtatását is lehetővé teszi:

    Get-AzureEnvironment

## <a name="next-steps"></a>Következő lépések

Ha a keresett további információért érdemes:

+ [GitHub PowerShell-dokumentumokat](https://github.com/Azure/azure-powershell)
+ [Lépésenkénti utasítások az erőforrás-kezelés való csatlakozással kapcsolatban](https://blogs.msdn.microsoft.com/azuregov/2015/10/08/configuring-arm-on-azure-gc/)
+ [Azure PowerShell-dokumentumokat az MSDN webhelyen](https://msdn.microsoft.com/library/mt619274.aspx)

A kiegészítő információk és frissítések fizessen elő a [Microsoft Azure kormányzati Blog] (https://blogs.msdn.microsoft.com/azuregov/)
