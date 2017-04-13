<properties 
   pageTitle="DNS-beállítások megadása a szolgáltatás konfigurációs fájl |} Microsoft Azure"
   description="szolgáltatás konfigurációs fájl segítségével virtuális hálózat egyéni DNS-beállítások megadása"
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
   ms.date="02/24/2016"
   ms.author="jdial" />

# <a name="specifying-dns-settings-in-a-service-configuration-file"></a>DNS-beállítások megadása a szolgáltatás konfigurációs fájl

## <a name="dns-elements"></a>DNS-elemek

Szolgáltatás konfigurációs fájl is tartalmaz, amely a szolgáltatás által használt tartománynév (DNS) kiszolgálók IPv4-címei és a DnsServers elemet. A szolgáltatás konfigurációs fájl beállításai élveznek a hálózati konfigurációs fájl beállításait. További tudnivalókért lásd: [Azure szolgáltatás konfigurációs séma (.cscfg fájl)](https://msdn.microsoft.com/library/azure/ee758710.aspx).

**NetworkConfiguration elem**

      <DnsServers>
        <DnsServer name="ID1" IPAddress="IPAddress1" />
        <DnsServer name="ID2" IPAddress="IPAddress2" />
        <DnsServer name="ID3" IPAddress="IPAddress3" />
      </DnsServers>

>[AZURE.WARNING] A **név** attribútum **Dns_kiszolgáló** eleme csak egy hivatkozás neveként használják. A DNS-kiszolgáló állomásneve azt nem jelent meg. Minden egyes **Dns_kiszolgáló** attribútumérték egyedinek kell lennie a teljes Microsoft Azure-előfizetésben.

## <a name="see-also"></a>Lásd még:

[Azure szolgáltatás konfigurációs séma (.cscfg)](https://msdn.microsoft.com/library/windowsazure/ee758710)

[Azure virtuális hálózati konfigurációja séma](http://go.microsoft.com/fwlink/?LinkId=248093)

[Hálózati konfigurációja fájlok használatáról virtuális hálózat konfigurálása](http://go.microsoft.com/fwlink/?LinkId=248094)

[A virtuális hálózati beállításai az adatkezelési portálon](http://go.microsoft.com/fwlink/?LinkId=248092)

