<properties 
   pageTitle="DNS-beállítások megadása a virtuális hálózati konfigurációs fájl |} Microsoft Azure"
   description="A klasszikus telepítési modell virtuális hálózati konfigurációs fájl használatával virtuális hálózati DNS-kiszolgáló beállításainak módosítása"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn" 
   tags="azure-service-management" />
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/23/2016"
   ms.author="jdial" /> 


# <a name="specifying-dns-settings-in-a-virtual-network-configuration-file"></a>DNS-beállítások megadása a virtuális hálózati konfigurációs fájl

A hálózati konfigurációs fájl van két elemeket, amelyek segítségével adja meg a tartománynévrendszer (DNS) beállításai: **DnsServers** és **DnsServerRef**. IP-címek és a hivatkozás nevét a **DnsServers** elem megadásával felveheti a DNS-kiszolgálók listáját. Egy **DnsServerRef** elem segítségével, majd adja meg, hogy melyik DNS-kiszolgáló bejegyzések DnsServers elemből segítségével különböző hálózati helyek a virtuális hálózaton belül.

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]Ez a cikk bemutatja a klasszikus telepítési modell.

A hálózati konfigurációs fájl tartalmazhat, az alábbi elemeket. Minden elem címére az elem érték beállítások további információt tartalmaz az oldalak van csatolva.

>[AZURE.IMPORTANT] A hálózati konfigurációs fájl konfigurálásáról további tudnivalókért lásd [konfigurálása használatáról virtuális hálózati hálózati konfigurációs fájl](virtual-networks-using-network-configuration-file.md). A hálózati kereséskonfigurációs fájlban lévő elemek tudni olvassa el a [Azure virtuális hálózati konfigurációja séma](https://msdn.microsoft.com/library/azure/jj157100.aspx)című témakört.

[DNS-elem](http://go.microsoft.com/fwlink/?LinkId=248093)

    <Dns>
      <DnsServers>
        <DnsServer name="ID1" IPAddress="IPAddress1" />
        <DnsServer name="ID2" IPAddress="IPAddress2" />
        <DnsServer name="ID3" IPAddress="IPAddress3" />
      </DnsServers>
    </Dns>

>[AZURE.WARNING] A **név** attribútum **Dns_kiszolgáló** eleme csak hivatkozásként szolgál az **DnsServerRef** elemet. A DNS-kiszolgáló állomásneve azt nem jelent meg. Minden egyes **Dns_kiszolgáló** attribútum értéknek egyedinek kell lennie a teljes Microsoft Azure-előfizetésben

[Virtuális hálózati helyek elem](http://go.microsoft.com/fwlink/?LinkId=248093)

    <DnsServersRef>
      <DnsServerRef name="ID1" />
      <DnsServerRef name="ID2" />
      <DnsServerRef name="ID3" />
    </DnsServersRef>

>[AZURE.NOTE] Annak érdekében, hogy adja meg ezt a beállítást, a virtuális hálózati helyek elemhez, meg kell lennie korábban gyökérelemben lesz definiálva DNS. A DnsServerRef a *nevét* a virtuális hálózati helyek elem egy megadott Dns_kiszolgáló *nevét*a DNS elem név oszlopban látható értéket kell hivatkozniuk.

## <a name="next-steps"></a>Következő lépések

- Ismerje meg az [Azure virtuális hálózati konfigurációja séma](http://go.microsoft.com/fwlink/?LinkId=248093).
- Ismerje meg az [Azure szolgáltatás konfigurációs séma](https://msdn.microsoft.com/library/windowsazure/ee758710).
- A [virtuális hálózat konfigurálása hálózati konfigurációs fájl használatával](virtual-networks-using-network-configuration-file.md).
