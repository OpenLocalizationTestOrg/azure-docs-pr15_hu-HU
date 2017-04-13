<properties 
    pageTitle="Tanúsítvány hozzáadása a Java hitelesítésszolgáltató tároló |} Microsoft Azure" 
    description="Megtudhatja, hogy miként vehet fel egy tanúsítványt hitelesítésszolgáltató tanúsítványát az Java hitelesítésszolgáltató (cacerts) tanúsítvány Store Twilio szolgáltatás vagy Azure Service Bus." 
    services="" 
    documentationCenter="java" 
    authors="rmcmurray" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="multiple" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="Java" 
    ms.topic="article" 
    ms.date="08/11/2016" 
    ms.author="robmcm"/>

# <a name="adding-a-certificate-to-the-java-ca-certificates-store"></a>Tanúsítvány felvétele az Java hitelesítésszolgáltató tanúsítványok áruházból
Az alábbi lépésekkel megjelenítése egy tanúsítványt hitelesítésszolgáltató tanúsítványát hozzáadása a Java hitelesítésszolgáltató (cacerts) tanúsítvány áruházból. A példában használt a Twilio szolgáltatáshoz szükséges hitelesítésszolgáltató tanúsítványát szolgál. A témakör a megadott adatokkal a hitelesítésszolgáltató tanúsítványának telepítése az Azure Service Bus ismerteti. 

Adja hozzá hitelesítésszolgáltató tanúsítványát a JDK tömörítéséről, és a Azure projekt **approot** mappába helyezés előtt keytool is használhatja, vagy az Azure indítási tevékenység hozzáadása a tanúsítvány keytool használó esetleges. Ez a példa feltételezi, hogy egy hitelesítésszolgáltató tanúsítványát az éppen zip JDK előtt szeretne felvenni. Is egy adott hitelesítésszolgáltató tanúsítványt használni fogják a példában, de a másik hitelesítésszolgáltató tanúsítvány beszerzése, és importálja a fájlt a cacerts tárolóba lépésein hasonló lenne.

## <a name="to-add-a-certificate-to-the-cacerts-store"></a>Tanúsítvány hozzáadása a cacerts tárolóhoz

1. Parancssorba, amely a JDK **jdk\jre\lib\security** mappába van állítva futtassa az alábbi módon, hogy milyen tanúsítványaik vannak telepítve:

    `keytool -list -keystore cacerts`

    Kérni fogja a tár jelszót. Az alapértelmezett jelszava **változik**. (Ha szeretné módosítani a jelszót, tekintse át a keytool dokumentációt a <http://docs.oracle.com/javase/7/docs/technotes/tools/windows/keytool.html>.) Ez a példa feltételezi, hogy a tanúsítvány MD5 ujjlenyomat 67:CB:9 D: C0:13:24:8A:82:9B:B2:17:1E:D1:1B:EC:D4 nem szerepel a listában, és azt (ebben a tanúsítványban szükséges Twilio API szolgáltatás) importálni szeretne.
2. A tanúsítvány beszerzése a tanúsítványok [GeoTrust legfelső szintű tanúsítványok](http://www.geotrust.com/resources/root-certificates/)webhelyén állítsa be a listából. Kattintson a jobb gombbal a sorszám 35:DE:F4:CF tanúsítványát a hivatkozására, és mentse a **jdk\jre\lib\security** mappába. Nevű fájl mentve volt ebben a példában alkalmazásában **Equifax\_biztonságos\_tanúsítvány\_Authority.cer**.
3. Importálás a tanúsítvány használatával az alábbi parancsot:

    `keytool -keystore cacerts -importcert -alias equifaxsecureca -file Equifax_Secure_Certificate_Authority.cer`

    Amikor a rendszer kéri a ebben a tanúsítványban meg, ha a tanúsítvány MD5 ujjlenyomat 67:CB:9 D: C0:13:24:8A:82:9B:B2:17:1E:D1:1B:EC:D4, a válasz **y**beírásával.
4. A következő parancsot a hitelesítésszolgáltató tanúsítványát sikeresen importálta biztosítása:

    `keytool -list -keystore cacerts`

5. Zip-a JDK, és vegye fel a Azure projekt **approot** mappát.

Keytool kapcsolatos információkért olvassa el a <http://docs.oracle.com/javase/7/docs/technotes/tools/windows/keytool.html>című témakört.

## <a name="azure-root-certificates"></a>Azure legfelső szintű tanúsítványok

Az Azure szolgáltatások (például Azure Service Bus) használó alkalmazások kell a Egerben CyberTrust legfelső szintű tanúsítvány a megbízható gombra. (Nyitó április 15, 2013-ban Azure meg a legfelső szintű Egerben CyberTrust áttelepítés GTE CyberTrust globális legfelső szintjén. Az áttelepítés tartott hónapokkal befejezéséhez.)

A tanúsítvány előfordulhat, hogy telepítve kell lennie a cacerts áruház Egerben így tartania, hogy futtassa a **keytool-lista** látható, ha már létezik ilyen először parancsot.

Ha, hogy fel kell vennie a Egerben CyberTrust legfelső szintű, rajta időértékét 02:00:00:b9 és SHA1 ujjlenyomat d4:de:20:d0:5e:66:fc:53:fe:1a:50:88:2 c: 78:db:28:52:ca:e4:74. Azt is lehet letölteni <https://cacert.omniroot.com/bc2025.crt>, egy helyi bővítmény **.cer**fájl menti, és importálja a fent látható **keytool** használatával.

## <a name="next-steps"></a>Következő lépések

A legfelső szintű tanúsítványok Azure által használt kapcsolatos további tudnivalókért olvassa el az [Azure legfelső szintű tanúsítvány áttelepítési](http://blogs.msdn.com/b/windowsazure/archive/2013/03/15/windows-azure-root-certificate-migration.aspx)című témakört.

Java kapcsolatos további tudnivalókért lásd: a [Java Developer Center](/develop/java/).
