<properties
   pageTitle="A szolgáltatás háló fürtre biztonságos |} Microsoft Azure"
   description="Ez a témakör a biztonsági szolgáltatás háló fürt és ezek esetek végrehajtásához használt különböző technológiákkal."
   services="service-fabric"
   documentationCenter=".net"
   authors="ChackDan"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/19/2016"
   ms.author="chackdan"/>

# <a name="service-fabric-cluster-security-scenarios"></a>Szolgáltatás háló fürt biztonsági felhasználási területei

A szolgáltatás háló fürtre egy erőforrást, amely az Ön tulajdonában. Fürt mindig titkosítani kell, hogy az illetéktelen felhasználók a csatlakozást a fürthöz, különösen akkor, ha van rá futó termelési feladatok. Bár lehet létrehozni egy nem védett fürthöz, ezzel így lehetővé bármely névtelen felhasználók is csatlakozni, ha azt közzététele management végpontok nyilvános internetkapcsolat. 

Ez a cikk áttekintést nyújt a biztonsági forgatókönyvek Azure vagy különálló és a különféle, adott esetek végrehajtásához használt technológiák futó fürtre vonatkozóan. Az fürt biztonsági jelenik meg a következők:

- Biztonsági a csomópont-csomópont
- Ügyfél-csomópont biztonsági
- Szerepköralapú hozzáférés-szerepalapú

## <a name="node-to-node-security"></a>Biztonsági a csomópont-csomópont
Védelemmel látja el a VMs vagy a fürt számítógépek közötti kommunikációt. Ezzel biztosíthatja, hogy csak az engedélyezett fürthöz számítógépek létesíthet-alkalmazások és szolgáltatások a fürt szolgáltatója.

![A csomópontok kapcsolati diagram][Node-to-Node]

Futó Windows Azure vagy különálló fürt futó fürt használható Windows Server gépekhez vagy a [Biztonsági tanúsítványt](https://msdn.microsoft.com/library/ff649801.aspx) , vagy a [Windows biztonsági](https://msdn.microsoft.com/library/ff649396.aspx) .
### <a name="node-to-node-certificate-security"></a>A csomópontok tanúsítvány biztonság
Szolgáltatás háló X.509 kiszolgálói tanúsítványok megadott a csomópont-típus konfigurációk részeként fürt létrehozásakor használ. Mik azok a tanúsítványok, és hogyan szerezheti be, vagy hozzon létre őket gyors áttekintés biztosítjuk, ez a cikk végén.

Biztonsági tanúsítványt úgy van konfigurálva, a fürt az Azure-portálra, Azure erőforrás-kezelő sablonok vagy egy különálló JSON-sablon létrehozása során. Megadhatja, hogy egy elsődleges tanúsítványt és egy másodlagos választható tanúsítvány, amellyel tanúsítvány előgörgetések. Az elsődleges és másodlagos tanúsítványok, adja meg, hogy a rendszergazda ügyfél és a csak olvasható ügyfél tanúsítványok adja meg, ha az [ügyfél-csomópont biztonsági](#client-to-node-security)eltérő kell lennie.

Azure olvassa el az [egy erőforrás-kezelő Azure-sablon segítségével fürt beállítása](service-fabric-cluster-creation-via-arm.md) című témakörben olvashat a fürtre tanúsítvány biztonság beállítása.

Különálló Windows Server olvassa el a [Windows X.509 tanúsítványokkal önálló fürt biztonságos](service-fabric-windows-cluster-x509-security.md)

### <a name="node-to-node-windows-security"></a>A csomópontok – a windows biztonsági
Különálló Windows Server olvassa el a [Windows biztonsági használata Windows különálló fürt biztonságos](service-fabric-windows-cluster-windows-security.md)

## <a name="client-to-node-security"></a>Ügyfél-csomópont biztonsági
Ügyfelek hitelesíti és védelemmel látja el az ügyfél és az egyes csomópontok a fürt közötti kommunikációt. Az ilyen típusú biztonsági hitelesíti, és védelemmel látja el az ügyfél kommunikáció, amely biztosítja, hogy csak hitelesített felhasználó hozzáférhet, a fürt és az alkalmazások, a fürt rendszerre telepíthető. Ügyfelek egyedileg azonosítja a Windows hitelesítő adatokat, vagy biztonsági adataikat keresztül.

![Az ügyfél-csomópont-kapcsolati diagram][Client-to-Node]

A Windows rendszeren futó Azure vagy különálló fürt futó fürt használható vagy a [Biztonsági tanúsítványt](https://msdn.microsoft.com/library/ff649801.aspx) , vagy a [Windows biztonsági](https://msdn.microsoft.com/library/ff649396.aspx).

### <a name="client-to-node-certificate-security"></a>Ügyfél-csomópont tanúsítvány biztonság
 Ügyfél-csomópont tanúsítvány biztonság van beállítva az Azure portálon, erőforrás-kezelő sablonok vagy egy rendszergazda ügyfél-tanúsítványt és/vagy a felhasználó ügyfél-tanúsítvány megadásával JSON önálló sablonból vagy a csoport létrehozásakor.  A felügyeleti ügyfél és a felhasználó ügyfél tanúsítványok megadhatja az elsődleges és másodlagos tanúsítványok [biztonsági a csomópont-csomópont](#node-to-node-security)a megadott eltérő kell lennie.

A felügyeleti tanúsítvány használatával fürthöz kapcsolódó ügyfeleket szolgáltatásairól teljes hozzáférése van.  A csak olvasható felhasználói ügyfél tanúsítvány használatával fürthöz kapcsolódó ügyfeleket hozzáférést csak olvasható kezelési lehetőségeit. Más szóval tanúsítványok a szerepkör alapjainak access szerepalapú jelen cikkben ismertetett segítségével.

Azure olvassa el az [egy erőforrás-kezelő Azure-sablon segítségével fürt beállítása](service-fabric-cluster-creation-via-arm.md) című témakörben olvashat a fürtre tanúsítvány biztonság beállítása.

Különálló Windows Server olvassa el a [Windows X.509 tanúsítványokkal önálló fürt biztonságos](service-fabric-windows-cluster-x509-security.md)

### <a name="client-to-node-azure-active-directory-aad-security-on-azure"></a>Ügyfél-csomópont Azure Active Directory (AAD) biztonsági Azure
Azure futó fürt is is biztonságos módon elérhetők az Azure Active Directory (AAD) használatával management végpontjait. Lásd: a [beállítása szerint fürt erőforrás-kezelő Azure-sablon segítségével](service-fabric-cluster-creation-via-arm.md) hogyan hozhat létre a szükséges AAD eltéréseket, hogyan fürt létrehozása során feltöltéséhez és ezt követően ezeket fürt keresztüli olvashat.

## <a name="security-recommendations"></a>Javaslatok biztonsági
Azure fürtre, vonatkozóan ajánlott AAD biztonsági használhatja az ügyfelek és biztonsági a csomópont-csomópont-tanúsítványok hitelesítést végezni.

Windows Server különálló fürt azt ajánljuk, hogy használjon Windows biztonsági csoport Active Directory és a Windows Server 2012 R2 felügyelt fiókok (GMA). A Windows biztonsági egyébként is használata Windows fiókokat.

## <a name="role-based-access-control-rbac"></a>Szerepköralapú hozzáférés-szerepalapú
Hozzáférés-vezérlés lehetővé teszi, hogy a fürt rendszergazda korlátozhatja a hozzáférést bizonyos fürt műveletek, felhasználók, a fürt biztonságosabbá tétele különböző csoportokhoz. Két különböző hozzáférési vezérlőtípus fürt kapcsolódás ügyfelek esetén támogatottak: rendszergazdai szerepkörét és a felhasználói szerepkör.

A rendszergazdák kezelése-funkciók (köztük a olvasási/írási funkciók) teljes hozzáférése van. A felhasználók, alapértelmezés szerint rendelkeznek csak olvasási hozzáférést kezelési lehetőségek (például a lekérdezés lehetőségek), és az azt jelenti, hogy feloldása az alkalmazások és szolgáltatások.

Megadhatja a rendszergazdai és felhasználói ügyfél szerepkörök fürt létrehozása időben, mert külön azonosítók (tanúsítványok, AAD stb.) az egyes. Az alapértelmezett hozzáférési beállításai és az alapértelmezett beállítások módosítása a további tudnivalókért lásd: a [szerepköralapú hozzáférés-vezérlés szolgáltatás háló ügyfelek számára](service-fabric-cluster-security-roles.md).


## <a name="x509-certificates-and-service-fabric"></a>X.509 tanúsítványok és szolgáltatás háló
Ügyfél- és kiszolgáló hitelesítést végezni, és az üzenetek titkosítása és digitális aláírása a digitális tanúsítványok X.509 gyakran használják. További információt a tanúsítványok lépjen a [tanúsítványok használata](http://msdn.microsoft.com/library/ms731899.aspx).

Néhány fontos dolgot is célszerű figyelembe venni:

- Gyártási munkaterhelésekből futó fürt használt tanúsítványokat kell egy megfelelően konfigurált Windows Server tanúsítvány szolgáltatás használatával létrehozott vagy egy jóváhagyott [Tanúsítvány hitelesítésszolgáltató](https://en.wikipedia.org/wiki/Certificate_authority)nyert.
- Soha ne használjon bármely ideiglenes vagy gyártási, például MakeCert.exe eszközzel létrehozott tanúsítványok tesztelheti.
- Önaláírt tanúsítvány használható, de csak tegye próba fürtre vonatkozóan, és nem a termelési.

### <a name="server-x509-certificates"></a>Kiszolgálói X.509 tanúsítványok

Kiszolgálói tanúsítványok az elsődleges tevékenység hitelesítése ügyfeleknek kiszolgáló (csomópont) vagy a kiszolgáló (csomópont) (csomópont) kiszolgálón hitelesítő van. Amikor egy ügyfél vagy a csomópont hitelesíti csomópont kezdeti ellenőrzést egyik ellenőrzi az értéket a közös név, a Tárgy mezőbe. A közös neve vagy a tanúsítványok Tárgy alternatív nevek közül az engedélyezett közös nevek listája megtalálható kell lennie.

A következő cikk leírja, hogy miként tanúsítványok tárgy alternatív nevek (SAN) készítése: [hogy miként vehet fel egy biztonságos LDAP tanúsítvány tárgy alternatív nevét](http://support.microsoft.com/kb/931351).

A Tárgy mező több értéket is tartalmazhat, minden előtaggal az érték feltüntetheti egy inicializálni. Leggyakrabban a inicializálni szolgál "CN" közönséges neve; Ha például "CN = www.contoso.com". Lehetőség arra is a Tárgy mező üres legyen. A választható alternatív tulajdonosneve mező nem üres, ha a közös a tanúsítvány és egy alternatív tulajdonosneve tételt kell tartalmaznia. Ezek megadhatók tartománynév értékként.

A tanúsítvány kívánt célok mező értékének tartalmaznia kell egy megfelelő értéket, például "Server" vagy "Ügyfél-hitelesítés használatával".

### <a name="client-x509-certificates"></a>Ügyfél X.509 tanúsítványok

Ügyfél-tanúsítványok kibocsátása nem általában egy külső hitelesítésszolgáltató. Ehelyett az aktuális felhasználói hely a személyes tárolóban általában az ügyfél tanúsítványok legfelső szintű hitelesítésszolgáltató, a "Ügyfél hitelesítési"-rendeltetését helyeztek tartalmazza. Az ügyfél ilyen olyan tanúsítványt használhat, ha kölcsönös hitelesítés szükség.

>[AZURE.NOTE] A szolgáltatás háló fürthöz összes felügyeleti művelet kiszolgálói tanúsítványok szükség. Ügyfél-tanúsítványok kezelésére nem használhatók.

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->


## <a name="next-steps"></a>Következő lépések

Ez a cikk tájékoztatást fürt biztonsági. A következő, [az erőforrás-kezelő sablon használatával Azure fürt létrehozása](service-fabric-cluster-creation-via-arm.md) vagy az [Azure portálon](service-fabric-cluster-creation-via-portal.md)keresztül.

<!--Image references-->
[Node-to-Node]: ./media/service-fabric-cluster-security/node-to-node.png
[Client-to-Node]: ./media/service-fabric-cluster-security/client-to-node.png
