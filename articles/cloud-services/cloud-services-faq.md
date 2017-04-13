<properties
    pageTitle="Felhőalapú szolgáltatások – gyakori kérdések |} Microsoft Azure"
    description="Gyakori kérdések a a Cloud Services."
    services="cloud-services"
    documentationCenter=""
    authors="Thraka"
    manager="timlt"
    editor=""/>

<tags
    ms.service="cloud-services"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/19/2016"
    ms.author="adegeo"/>

# <a name="cloud-services-faq"></a>Cloud Services – gyakori kérdések
Ez a cikk a Microsoft Azure Cloud Services kapcsolatos gyakori kérdésekre ad választ. Látogasson el a az [Azure támogatja a gyakori kérdések](http://go.microsoft.com/fwlink/?LinkID=185083) általános Azure árak és támogatási információkat is. A [felhőalapú szolgáltatások virtuális méret lap](cloud-services-sizes-specs.md) is kérjen méret információkat.

## <a name="certificates"></a>Tanúsítványok

### <a name="where-should-i-install-my-certificate"></a>Hol kell telepíteni a tanúsítvány?

- **A**  
Alkalmazás tanúsítvány és a titkos kulcs (\*.pfx, \*.p12).

- **HITELESÍTÉSSZOLGÁLTATÓ**  
Köztes tanúsítványok léphet az üzlet (házirend és Sub hitelesítésszolgáltatók).

- **LEGFELSŐ SZINTŰ**  
A legfelső szintű hitelesítésszolgáltató tárolására, így a fő legfelső szintű hitelesítésszolgáltató tanúsítványának helyezzem.

### <a name="i-cant-remove-expired-certificate"></a>Nem lehet eltávolítani lejárt tanúsítvány

Azure megakadályozza, hogy Ön tanúsítvány eltávolítása, a használatát. Törölje a környezetben, amely a tanúsítványt használja, vagy a telepítés frissítése egy másik vagy új tanúsítvánnyal kell.

### <a name="delete-an-expired-certificate"></a>A lejárt tanúsítvány törlése

Mindaddig, amíg a tanúsítvány nem lesz használatban a [Eltávolítása-AzureCertificate](https://msdn.microsoft.com/library/azure/mt589145.aspx) PowerShell-parancsmag használatával tanúsítvány eltávolítása.

### <a name="i-have-expired-certificates-named-windows-azure-service-management-for-extensions"></a>A Windows Azure Szolgáltatáskezelés nevű Extensions tanúsítványok tudom lejárt

Tanúsítványok jönnek létre, amikor kiterjesztése bekerül a felhőalapú szolgáltatásba, például a távoli asztali bővítmény. Ezek a tanúsítványok csak titkosítása és visszafejtése személyes beállításait a bővítmény segítségével. Nem számít, hogy ezek a tanúsítványok lejár. A lejárati dátum nincs bejelölve.

### <a name="certificates-i-have-deleted-keep-reappearing"></a>A tanúsítványok tudom törölt töröltem

Ezek töröltem valószínűleg egy eszközt használ, például a Visual Studio miatt. Amikor újra kapcsolódik egy eszközt, amelyet egy tanúsítványt használja, akkor fogja újra tölthető fel Azure.

### <a name="my-certificates-keep-disappearing"></a>Saját tanúsítványok ne megtartása tűnhessenek

A virtuális gép példány rendszer akkor hasznosítja újra, ha az összes helyi módosítások elvesznek. [Indítási tevékenység](cloud-services-startup-tasks.md) segítségével tanúsítványok telepítse a virtuális géphez a szerepkör indításakor.

### <a name="i-cannot-find-my-management-certificates-in-the-portal"></a>A felügyeleti tanúsítványok a portálon nem található

[Adatkezelési tanúsítványok](..\azure-api-management-certs.md) az Azure klasszikus portálon csak érhetők el. Az aktuális Azure portálon nem használja az adatkezelési tanúsítványok. 

### <a name="how-can-i-disable-a-management-certificate"></a>Hogyan tilthatom management tanúsítvány?

[Adatkezelési tanúsítványok](..\azure-api-management-certs.md) nem tiltható le. Törli őket az Azure klasszikus portálon keresztül, ha azt szeretné, hogy többé használni.

### <a name="how-do-i-create-an-ssl-certificate-for-a-specific-ip-address"></a>Hogyan hozhatok létre SSL-tanúsítvány, egy adott IP-cím?

Kövesse a [Hozzon létre egy tanúsítványt oktatóprogram](cloud-services-certs-create.md). Az IP-cím használata a DNS-nevét.

## <a name="security"></a>Biztonsági

### <a name="disable-ssl-30"></a>SSL 3.0 letiltása

SSL 3.0 letiltása és TLS biztonsági használatához blogbejegyzésben amely dokumentált indítási feladat létrehozása: https://azure.microsoft.com/en-us/blog/how-to-disable-ssl-3-0-in-azure-websites-roles-and-virtual-machines/

## <a name="scale-a-cloud-service"></a>Egy felhőalapú szolgáltatásba méretezése

### <a name="i-cannot-scale-beyond-x-instances"></a>E nem méretezheti túl az X példányok

Azure előfizetéséhez korlátozott a használható magmintákat számára. Méretezés nem fog működni, ha használta az összes rendelkezésre álló mag. Például ha legfeljebb 100 magmintákat, ez azt jelenti, 100 méretű A1 virtuális gép példányok a felhőben szolgáltatásbeli is rendelkezik, vagy 50 A2 méretű virtuális gép példányok.

## <a name="troubleshooting"></a>Hibaelhárítás

### <a name="i-cant-reserve-an-ip-in-a-multi-vip-cloud-service"></a>E nem lefoglalása IP-címre a többszörös-virtuális felhőszolgáltatásában

Először győződjön meg arról, hogy be van-e kapcsolva a virtuális gépen szeretné fenntartani a IP-példányt. Második győződjön meg arról, hogy használata fenntartott IP-címei többet jelzi a átmeneti és üzemi telepítések. **Nem** módosítsa a beállításokat, frissíti a telepítés során.

