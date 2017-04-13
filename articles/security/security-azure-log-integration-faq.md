<properties
   pageTitle="Azure napló integrációs gyakran ismételt kérdések |} Microsoft Azure"
   description="Ez a cikk megválaszolja Azure log integrációs kérdéseket."
   services="security"
   documentationCenter="na"
   authors="TomShinder"
   manager="MBaldwin"
   editor="TerryLanfear"/>

<tags
   ms.service="security"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/23/2016"
   ms.author="TomSh"/>

# <a name="azure-log-integration-frequently-asked-questions-faq"></a>Azure napló integráció a gyakori kérdések

Ez a cikk megválaszolja Azure napló integráció, szolgáltatás, amely lehetővé teszi, hogy az Azure erőforrásoktól nyers naplók integrálása a helyszíni biztonsági információk és esemény-kezelés (SIEM) rendszerek kérdéseket. Ez az integráció biztosítja egységesített irányítópult az összes eszközeit, a helyszíni, vagy a felhőben, hogy az összesítő, összehangolására, elemezheti, és a felhasználó-alkalmazásokkal kapcsolatos biztonsági események.

## <a name="how-can-i-see-the-storage-accounts-from-which-azure-log-integration-is-pulling-azure-vm-logs-from"></a>Hogyan jelennek meg a tárhely fiókok, amelyből Azure napló integrálása az Azure virtuális naplók van adatok?

Futtassa a parancs **azlog forrás listában**.

## <a name="how-can-i-update-the-proxy-configuration"></a>Hogyan frissítheti a proxy konfigurációjának?

Ha közvetlenül a proxy beállításai nem engedélyezi a hozzáférést Azure tárhely, nyissa meg a **AZLOG. EXE. CONFIG** **c:\Program Files\Microsoft Azure napló integráció**a fájlt. Frissítse a **defaultProxy** szakaszt a proxycímű a szervezet meg a fájlt. Amikor befejeződik a frissítés, leállítása és a **nettó leállítása azlog** parancsok és a **nettó kezdő azlog**szolgáltatás.

    <?xml version="1.0" encoding="utf-8"?>
    <configuration>
      <system.net>
        <connectionManagement>
          <add address="*" maxconnection="400" />
        </connectionManagement>
        <defaultProxy>
          <proxy usesystemdefault="true"
          proxyaddress=http://127.0.0.1:8888
          bypassonlocal="true" />
        </defaultProxy>
      </system.net>
      <system.diagnostics>
        <performanceCounters filemappingsize="20971520" />
      </system.diagnostics>   

## <a name="how-can-i-see-the-subscription-information-in-windows-events"></a>Hogyan jelennek meg a Windows-események az előfizetési adatokat?

A **subscriptionid** hozzáfűzése a könnyen megjegyezhető nevet a forrás hozzáadása közben.

    Azlog source add <sourcefriendlyname>.<subscription id> <StorageName> <StorageKey>  

Az esemény XML a metaadatokat tartalmaz, lásd lentebb, beleértve az előfizetés azonosítója.

![Esemény XML][1]

## <a name="error-messages"></a>Hibaüzenetek jelennek meg

### <a name="when-running-command-azlog-createazureid-why-do-i-get-the-following-error"></a>Parancs **azlog createazureid**, miért jelenik meg a következő hiba futtatásakor?

Hiba:

  *Nem sikerült létrehozni AAD alkalmazás – bérlői 72f988bf-86f1-41af-91ab-2d7cd011db37-oka = "Tiltott" - üzenet = "Megfelelő jogosultsága a művelet végrehajtásához."*

Hozzon létre egy egyszerű az előfizetések, amelyen az Azure bejelentkezés van hozzáférése az Azure AD-bérlők megpróbálja **Azlog createazureid** . Ha az Azure bejelentkezési csak az adott Azure AD-bérlő Vendég felhasználó, majd a parancs az "Megfelelő jogosultsága a művelet végrehajtásához." A fiók hozzáadása a bérlői webhelyemen felhasználóként bérlői rendszergazdai kérhet.

### <a name="when-running-command-azlog-authorize-why-do-i-get-the-following-error"></a>Futtatásakor parancs **azlog engedélyezheti**, miért jelenik meg a következő hiba?

Hiba:

  *Szerepkör-hozzárendelés - AuthorizationFailed létrehozása figyelmeztetés: az ügyfél janedo@microsoft.com' objektummal azonosító "fe9e03e4-4dad-4328-910f-fd24a9660bd2" nincs "Microsoft.Authorization/roleAssignments/write" művelet végrehajtásához engedélyezési hatóköre "/ előfizetések/70 95299-d689-4-es c d 97-b971-0d8ff0000000" fölé.*

**Azlog engedélyezése** parancs rendel a megadott előfizetéseket szerepe olvasó az Azure Active Directory-szolgáltatás fő ( **Azlog createazureid**készült). Ha az Azure bejelentkezés nem egy közös rendszergazdájától, illetve az előfizetés tulajdonosa, a "Engedély nem sikerült" hibaüzenet meghiúsul. Azure szerepköralapú hozzáférés-szerepalapú közös rendszergazda vagy a tulajdonos, ez a művelet befejezéséhez szükséges.

## <a name="where-can-i-find-the-definition-of-the-properties-in-audit-log"></a>Hol találom meg a Tulajdonságok meghatározása a napló?

Lásd:

- [Erőforrás-kezelő műveleteinek naplózása](../resource-group-audit.md)
- [Az adatkezelési események Azure Monitor REST API-t az előfizetés lista](https://msdn.microsoft.com/library/azure/dn931934.aspx)

## <a name="where-can-i-find-details-on-azure-security-center-alerts"></a>Hol találom meg a részleteket a Azure biztonság otthon értesítéseket?

Lásd: [kezelése és a biztonsági figyelmeztetések az Azure biztonság otthon válaszolni](../security-center/security-center-managing-and-responding-alerts.md).

## <a name="how-can-i-modify-what-is-collected-with-vm-diagnostics"></a>Hogyan módosíthatók meg, hogy mit kell felvenni a virtuális diagnosztika?

[Ahhoz, hogy egy virtuális gépen futó Windows Azure diagnosztika PowerShell](../virtual-machines/virtual-machines-windows-ps-extensions-diagnostics.md) témakörben olvashat beszerzése, módosítása, és az Azure diagnosztika beállítása Windows rendszerben *(ÜVEGVATTA)* konfigurációs. Az alábbiakban minta:

### <a name="get-the-wad-config"></a>A ÜVEGVATTA config beszerzése

    -AzureRmVMDiagnosticsExtension -ResourceGroupName AzLog-Integration -VMName AzlogClient
    $publicsettings = (Get-AzureRmVMDiagnosticsExtension -ResourceGroupName AzLog-Integration -VMName AzlogClient).PublicSettings
    $encodedconfig = (ConvertFrom-Json -InputObject $publicsettings).xmlCfg
    $xmlconfig = [System.Text.Encoding]::UTF8.GetString([System.Convert]::FromBase64String($encodedconfig))
    Write-Host $xmlconfig

    $xmlconfig | Out-File -Encoding utf8 -FilePath "d:\WADConfig.xml"

### <a name="modify-the-wad-config"></a>A ÜVEGVATTA Config módosítása

Az alábbi példa a beállítás egy adott csak EventID 4624 és EventId 4625 gyűjtenek biztonsági eseménynaplójában talál. A rendszer eseménynaplójába Microsoft Antimalware események gyűjtenek. Lásd: [használata más események] (https://msdn.microsoft.com/library/windows/desktop/dd996910 (v=vs.85) XPath-kifejezés használatára vonatkozó részletes.

    <WindowsEventLog scheduledTransferPeriod="PT1M">
        <DataSource name="Security!*[System[(EventID=4624 or EventID=4625)]]" />
        <DataSource name="System!*[System[Provider[@Name='Microsoft Antimalware']]]"/>
    </WindowsEventLog>

### <a name="set-the-wad-configuration"></a>A ÜVEGVATTA konfiguráció beállítása

    $diagnosticsconfig_path = "d:\WADConfig.xml"
    Set-AzureRmVMDiagnosticsExtension -ResourceGroupName AzLog-Integration -VMName AzlogClient -DiagnosticsConfigurationPath $diagnosticsconfig_path -StorageAccountName log3121 -StorageAccountKey <storage key>

Módosítások elvégzése után ellenőrizze a tárterület-fiókot, annak érdekében, hogy a helyes események begyűjtési.

Ha Azure Log integrációs kérdéseket, küldjön e-mailben [AzSIEMteam@microsoft.com](mailto:AzSIEMteam@microsoft.com)

<!--Image references-->
[1]: ./media/security-azure-log-integration-faq/event-xml.png
