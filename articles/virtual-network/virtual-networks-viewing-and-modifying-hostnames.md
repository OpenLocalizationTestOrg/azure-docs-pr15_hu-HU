<properties 
   pageTitle="Megtekintése és módosítása a állomásnevekké |} Microsoft Azure"
   description="Hogyan megtekintése és módosítása az Azure virtuális gépeken futó állomásnevekké, webes és névfeloldás dolgozói szerepkörök"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn" />
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="04/27/2016"
   ms.author="jdial" />

# <a name="viewing-and-modifying-hostnames"></a>Megtekintése és állomásnevekké módosítása

Ha engedélyezni szeretné a szerepkör-példányok állomásnév, amelyre hivatkozni kíván, kell értéke az állomásnév a szolgáltatás konfigurációs fájl minden szerepkörhöz. Akkor ezt megteheti a kívánt állomásnév hozzáadása a **szerepkör** elem **vmName** attribútumának választógombját. A **vmName** attribútum értékének alapjául szolgál az egyes szerepkör-példány állomásnév. Például **vmName** *webrole* és az adott szerepkör három esetben, ha a állomásnevek példányok lesz *webrole0*, *webrole1*és *webrole2*. Szükségtelen virtuális gépeken futó host nevét a konfigurációs fájl adhat meg, mert az állomásnév virtuális géphez van töltve a virtuális gép neve alapján. A Microsoft Azure szolgáltatás beállításáról további információt a lásd [Azure szolgáltatás konfigurációs séma (.cscfg fájl)](https://msdn.microsoft.com/library/azure/ee758710.aspx)

## <a name="viewing-hostnames"></a>Állomásnevekké megtekintése

Egy felhőalapú szolgáltatásba a host azoknak a virtuális gépeken futó és szerepkör-példányok bármely, az alábbi eszközök segítségével tekintheti meg.

### <a name="azure-portal"></a>Azure portálon

A host nevek virtuális gépeken futó meg az Áttekintés lap virtuális gép az [Azure portálon](http://portal.azure.com) is használhatja. Ne feledje, hogy a lap **név** és **A Host Name**érték látható. Bár ezek megegyeznek az eredetileg, az állomásnév módosítása nem változik a virtuális gép vagy szerepkör-példány nevét.

Szerepkör-példányok az Azure-portálon is megtekinthetők, de a példányokat egy felhőalapú szolgáltatásba a listában nem jelenik a állomásnév. Láthatja, hogy minden példány nevét, de a név nem jelent meg az állomásnév.

### <a name="service-configuration-file"></a>Szolgáltatás konfigurációs fájl

A szolgáltatás konfigurációs fájl egy telepített szolgáltatás letölthető a **Configure** lap a szolgáltatás az Azure-portálon. Az állomásnév megtekintéséhez **szerepkör neve** elemhez a **vmName** attribútum majd keressen. Tartsa szem előtt a állomásnév alapjául szolgál az egyes szerepkör-példány állomásnév. Például **vmName** *webrole* és az adott szerepkör három esetben, ha a állomásnevek példányok lesz *webrole0*, *webrole1*és *webrole2*.

### <a name="remote-desktop"></a>Távoli asztal

Miután engedélyezte a távoli asztali (Windows), a Windows PowerShell távelérési (Windows) vagy SSH (Linux és Windows) kapcsolaton, virtuális gépeken futó vagy szerepkör-példányok, tekintheti meg az aktív távoli asztali kapcsolaton állomásneve többféle módon:

- Írja be a hostname (állomásnév) SSH Terminálszolgáltatások vagy a parancssor parancsot.

- Írja be: ipconfig/minden parancsot a parancssorba (csak Windows).

- Megtekintheti a számítógép neve a rendszerbeállításoktól (csak Windows).

### <a name="azure-service-management-rest-api"></a>Azure Szolgáltatáskezelés REST API-val

A többi ügyfélben kövesse az alábbi lépéseket:

1. Győződjön meg arról, hogy van-e csatlakozni az Azure-portálra ügyfél-tanúsítványt. Szerezze be az ügyfél-tanúsítvány, hajtsa végre a található [hogyan: Töltse le és közzététele beállítások importálása és az előfizetés adatai](https://msdn.microsoft.com/library/dn385850.aspx). 

1. Egy x-ms-verzió 2013-11-01 érték nevű élőfej bejegyzés beállítása.

1. Összehívásokkal a következő formátumban: https://management.core.windows.net/\<subscrition-azonosító\>/szolgáltatások/hostedservices/\<szolgáltatás neve\>?embed részletez = true

1. Keresse meg a **Hostname (állomásnév)** elem minden **RoleInstance** elemet.

>[AZURE.WARNING] Megtekintheti a belső tartománynév utótag a felhőben szolgáltatásbeli a a többi hívás választ, jelölje be a **InternalDnsSuffix** elem vagy futtatásával ipconfig/összes a parancssorból távoli asztali kapcsolat (Windows), vagy egy SSH Terminálszolgáltatások (Linux rendszerhez) származó futó macska /etc/resolv.conf.

## <a name="modifying-a-hostname"></a>A hostname (állomásnév) módosítása

Az állomásnév bármely virtuális gép vagy szerepkör-példány módosult szolgáltatási konfigurációs fájl feltöltésekor, vagy a számítógép egy távoli asztali munkamenetből átnevezése módosíthatók.

## <a name="next-steps"></a>Következő lépések

[A névfeloldás (DNS)](virtual-networks-name-resolution-for-vms-and-role-instances.md)

[Azure szolgáltatás konfigurációs séma (.cscfg)](https://msdn.microsoft.com/library/windowsazure/ee758710.aspx)

[Azure virtuális hálózati konfigurációja séma](http://go.microsoft.com/fwlink/?LinkId=248093)

[Adja meg a DNS-beállítások hálózati konfigurációja fájlok használata](virtual-networks-specifying-a-dns-settings-in-a-virtual-network-configuration-file.md)
