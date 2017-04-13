<properties
    pageTitle="Hogyan hozhat létre, és a billentyűk HSM védett át az Azure kulcs tárolóból elemre |} Microsoft Azure"
    description="Ez a cikk segítségével megtervezése, létrehozása és majd átadásához saját HSM védett billentyűk Azure kulcs tárolóból elemre."
    services="key-vault"
    documentationCenter=""
    authors="cabailey"
    manager="mbaldwin"
    tags="azure-resource-manager"/>

<tags
    ms.service="key-vault"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/24/2016"
    ms.author="cabailey"/>
#<a name="how-to-generate-and-transfer-hsm-protected-keys-for-azure-key-vault"></a>Hogyan hozhat létre és átadása HSM védett Azure kulcs tárolóból elemre billentyűparancsai

##<a name="introduction"></a>– Bevezetés

A felvett garancia, az Azure kulcs tárolóból elemre, ha importálhatja és kulcsok létrehozásához a hardver biztonsági modulokat (HSMs), amely a HSM oszlopazonosító soha ne hagyja. Ebben az esetben gyakran nevezik *jelenítse meg a saját billentyűt*, vagy BYOK. A HSMs 140-2 szintet 2 érvényesített FIPS. Azure kulcs tárolóból elemre a billentyűk védelme Thales nShield termékcsaládban HSMs használja.

Ebben a témakörben szereplő információk segítségével megtervezése, létrehozása és majd átadásához saját HSM védett billentyűk Azure kulcs tárolóból elemre.

Ez a funkció nem érhető el az Azure Kína. 

>[AZURE.NOTE] Azure kulcs tárolóra kapcsolatos további tudnivalókért lásd [Mi az Azure kulcs tárolóra?](key-vault-whatis.md)  
>
>Keresztüli lépések oktatóanyagot, amelyek tartalmazzák még a fő tárolóból elemre a billentyűparancsok HSM védett létrehozása, lásd: [első lépések az Azure kulcs tárolóból elemre](key-vault-get-started.md).

További információ: létrehozása, és egy HSM védett kulcs átvitele az interneten keresztül:

- A kulcs egy offline munkaállomásról, amely csökkenti a támadások felület létre.

- A kulcs az egy kulcsot Exchange kulcs (KEK), amely titkosított marad, amíg meg nem kerül át az Azure kulcs tárolóra HSMs titkosítva van. Csak a titkosított verzióban a termékkulcsot az eredeti munkaállomás elhagyja.

- A toolset tulajdonságainak beállítása a bérlői kulcs, amely összekapcsolja a termékkulcsot az Azure kulcs tárolóra biztonsági világ. Ezt követően az Azure kulcs tárolóra HSMs kap, és visszafejtése a termékkulcsot, csak ezek HSMs használhatja azt. Nem lehet exportálni a termékkulcsot. Ez a kötés kényszeríti a Thales HSMs.

- A kulcs Exchange kulcs (KEK), amely titkosítani a termékkulcsot az Azure kulcs tárolóra HSMs belül jön létre, és nem exportálható. A hivatkozási a HSMs, hogy az a KEK kívül a HSMs törlése változata nem lehet. Ezeken kívül az toolset tartalmazza az, hogy a KEK nem exportálható, és a valódi HSM, amely a Thales lett készítik belül lett létrehozva Thales igazolási.

- A toolset igazolási az, hogy az Azure kulcs tárolóra biztonsági nemzetközi is jött létre a valódi HSM Thales készítik a Thales tartalmazza. A tanúsítvány kiderül, hogy meg, hogy a Microsoft "eredeti" webhelyére hardver használ.

- A Microsoft külön KEKs használ, és külön minden földrajzi régióban biztonsági a világon. A elkülönítésének biztosítja, hogy használható-e a termékkulcsot csak a adatközpontokban, amelyben meg titkosított, akkor a tartományban lévő. Például az Európai ügyfél egy kulcsot a adatközpontokban Észak-amerikai vagy Ázsia nem használhatók.

##<a name="more-information-about-thales-hsms-and-microsoft-services"></a>További információt a Thales HSMs és a Microsoft-szolgáltatások

Thales az e-biztonsági egy vezető globális szolgáltató az adatok titkosítása és a pénzügyi szolgáltatások, technológiájú, gyártási, kormányzati és informatikai ágazatok számítógépes biztonsági megoldásokat. Egy 40 éves követése rekord védelmének vállalati és kormányzati információk Thales megoldások négy öt legnagyobb energy és űrtechnikai vállalatok által használt. Megoldásukat 22 NATO országok is használják, és a biztonságos több, mint 80 százalékát világszerte fizetési tranzakciók.

A Microsoft a kép állapotának tökéletesíthetik az HSMs Thales közvetlenül az rendelkezik. Ezek a bővítmények lehetővé teszi azok nélkül lemondana szabályozhatja a billentyűk szolgáltatott szolgáltatások tipikus előnyei első. Ezek a bővítmények engedélyezése a konkrétan a HSMs kezelése az, hogy nem rendelkezik a Microsoft. Egy felhőalapú szolgáltatásba, mint Azure kulcs tárolóból elemre méretezés során a rövid értesítés felel meg szervezete használatát kiugrásainak megfelelő. A termékkulcsot, egy időben belül a Microsoft HSMs védett:, tartsa ellenőrzés alatt, a fő életciklusáról, mert a kulcs generálása, és nem ruházhatja át a Microsoft HSMs.

##<a name="implementing-bring-your-own-key-byok-for-azure-key-vault"></a>Végrehajtási jelenítse meg a saját kulcs (BYOK) az Azure kulcs tárolóból elemre

Használja a az alábbi információkat és eljárásokat, ha használni saját HSM védett kulcs generálása és Azure kulcs tárolóra átteheti – az Előrehozás forgatókönyvet az kulcs (BYOK).


##<a name="prerequisites-for-byok"></a>BYOK előfeltételei

Lásd: az alábbi táblázat előfeltételei listáját a saját kulcs (BYOK) hozása az Azure kulcs tárolóból elemre.

|Követelmények|További információ|
|---|---|
|Azure-előfizetést|Hozzon létre egy Azure kulcs tárolóból elemre, szüksége van az Azure előfizetéssel: [ingyenes próbaverzióra feliratkozás](https://azure.microsoft.com/pricing/free-trial/)|
|Az Azure kulcs tárolóra prémium szolgáltatási réteg HSM védett kulcsok támogatásához|Azure kulcs tárolóból elemre a szolgáltatási rétegek és lehetőségeivel kapcsolatos további tudnivalókért lásd: az [Azure kulcs tárolóra árak](https://azure.microsoft.com/pricing/details/key-vault/) webhely.|
|A szoftvert, Thales HSM és kártyák|Rendelkeznie kell egy Thales hardveres biztonsági modult és műveleti alapismeretei Thales HSMs való hozzáférést. A lista kompatibilis modellek, illetve Hardvermodult megvásárolni, ha nincs telepítve egyik [Thales hardveres biztonsági modult](https://www.thales-esecurity.com/msrms/buy) talál.|
|Az alábbi hardveres és szoftveres:<ol><li>Az offline x64 munkaállomáson, legalább Windows 7 és a Thales nShield szoftver minimális Windows operációs rendszer verziója 11.50.<br/><br/>Ha a munkaállomás futtatja a Windows 7, [telepítse a Microsoft .NET-keretrendszer 4.5](http://download.microsoft.com/download/b/a/4/ba4a7e71-2906-4b2d-a0e1-80cf16844f5f/dotnetfx45_full_x86_x64.exe)kell.</li><li>Csatlakozik az internethez, és a Windows 7 minimális Windows operációs rendszer van munkaállomás.</li><li>USB-meghajtóra vagy más hordozható tárolóeszközre, amelynek legalább 16 MB szabad lemezterület.</li></ol>|Biztonsági okokból javasoljuk, hogy az első munkaállomás nem hálózathoz csatlakozik. Azonban ennek ellenére nem programozás útján lép életbe.<br/><br/>Figyelje meg, hogy a kövesse az utasításokat a munkaállomás nevezik a leválasztott munkaállomás.</p></blockquote><br/>Ezenkívül ha a bérlői kulcs hálózat, azt javasoljuk használható a második, külön munkaállomás letöltése a toolset, és töltse fel a bérlői billentyűt. De teszteléshez is használhatja ugyanazt a munkaállomás első szakasz.<br/><br/>Figyelje meg, hogy a kövesse az utasításokat, a második munkaállomás nevezik a internetkapcsolattal rendelkező munkaállomás.</p></blockquote><br/>|

##<a name="generate-and-transfer-your-key-to-azure-key-vault-hsm"></a>Készíthet, és nem ruházhatja át a kulcs Azure kulcs tárolóra HSM

Az alábbi öt lépéseket hozhat létre, és nem ruházhatja át a termékkulcsot az Azure kulcs tárolóra HSM fog használni:

- [Lépés: 1: Felkészülés a internetkapcsolattal rendelkező munkaállomástól](#step-1-prepare-your-internet-connected-workstation)
- [Lépés: 2: Felkészülés a leválasztott munkaállomástól](#step-2-prepare-your-disconnected-workstation)
- [3 lépés: A kulcs generálása](#step-3-generate-your-key)
- [Lépés: 4: Felkészülés a termékkulcsot az átadás](#step-4-prepare-your-key-for-transfer)
- [5 lépés: A kulcs átvitele Azure kulcs tárolóból elemre](#step-5-transfer-your-key-to-azure-key-vault)

## <a name="step-1-prepare-your-internet-connected-workstation"></a>Lépés: 1: Felkészülés a internetkapcsolattal rendelkező munkaállomástól
Első lépésként hajtsa végre az alábbi eljárások valamelyikét a számítógépen, amelyhez csatlakozik az internethez.


###<a name="step-11-install-azure-powershell"></a>Lépés 1.1: Azure PowerShell telepítése

Internetkapcsolattal rendelkező munkaállomásról töltse le és telepítse az Azure PowerShell-modult, amely tartalmazza a parancsmagok kezelése Azure kulcs tárolóból elemre. Ehhez a 0.8.13 a legkisebb verziószáma.

A telepítési utasításokat megtudhatja, [hogy miként telepítse és állítsa be a Azure PowerShell](../powershell-install-configure.md).

###<a name="step-12-get-your-azure-subscription-id"></a>Az 1.2 lépés: Az Azure előfizetés ID azonosító beszerzése

Indítsa el a az Azure PowerShell-munkamenetet, és jelentkezzen be az Azure-fiók használatával az alábbi parancsot:

        Add-AzureAccount
Az előugró böngészőablakához írja be az Azure-fiók felhasználónevét és jelszavát. Ezután használja a [Get-AzureSubscription](https://msdn.microsoft.com/library/azure/dn790366.aspx) parancsot:

        Get-AzureSubscription
A kimenet keresse meg az azonosító az előfizetés használhatja az Azure kulcs tárolóból elemre. Szüksége lesz a előfizetés azonosítója később.

Ne zárja be a Azure PowerShell ablakában.

###<a name="step-13-download-the-byok-toolset-for-azure-key-vault"></a>Lépés az 1.3-as: Töltse le a BYOK toolset Azure kulcs tárolóból elemre

Nyissa meg a Microsoft Download Center, és [Töltse le az Azure kulcs tárolóra BYOK toolset](http://www.microsoft.com/download/details.aspx?id=45345) földrajzi régióban vagy Azure példányát. A csomag nevére letöltése és a megfelelő SHA-256 csomag kivonat azonosítása használja az alábbi információkat:

---

**Észak-Amerika:**

KeyVault – BYOK-eszközök-Egyesült States.zip

305F44A78FEB750D1D478F6A0C345B097CD5551003302FA465C73D9497AB4A03

---

**Európa:**

KeyVault-BYOK-eszközök-Europe.zip

C73BB0628B91471CA7F9ADFCE247561C6016A5103EF1A315D49C3EA23AFC0B9C

---

**Ázsia:**

KeyVault-BYOK-eszközök-AsiaPacific.zip

BE9A84B6C76661929F9FDAD627005D892B3B8F9F19F351220BB4F9C356694192

---

**Latin-Amerika:**

KeyVault-BYOK-eszközök-LatinAmerica.zip
    
9E8EE11972DECE8F05CD898AF64C070C375B387CED716FDCB788544AE27D3D23

---

**Japán:**

KeyVault-BYOK-eszközök-Japan.zip

E6B88C111D972A02ABA3325F8969C4E36FD7565C467E9D7107635E3DDA11A8B2

---

**Ausztrália:**

KeyVault-BYOK-eszközök-Australia.zip

7660D7A675506737857B14F527232BE51DC269746590A4E5AB7D50EDD220675D

---

[**Azure kormányzati:**](https://azure.microsoft.com/features/gov/)

KeyVault-BYOK-eszközök-USGovCloud.zip

53801A3043B0F8B4A50E8DC01A935C2BFE61F94EE027445B65C52C1ACC2B5E80

---

**Kanada:**

KeyVault-BYOK-eszközök-Canada.zip

A42D9407B490E97693F8A5FA6B60DC1B06B1D1516EDAE7C9A71AA13E12CF1345

---

**Németország:**

KeyVault-BYOK-eszközök-Germany.zip

4795DA855E027B2CA8A2FF1E7AE6F03F772836C7255AFC68E576410BDD28B48E

---
**India:**

KeyVault-BYOK-eszközök-India.zip

26853511EB767A33CF6CD880E78588E9BBE04E619B17FBC77A6B00A5111E800C

---

A [Get-FileHash](https://technet.microsoft.com/library/dn520872.aspx) parancsmag használatával a letöltött BYOK toolset az Azure-PowerShell-munkamenetet a integritását ellenőrzése.

    Get-FileHash KeyVault-BYOK-Tools-*.zip

A toolset az alábbiakat tartalmazza:

- Egy kulcsot Exchange kulcs (KEK) csomag kezdődő neveket is tartalmazó **BYOK-KEK - pkg-.**
- Biztonsági nemzetközi csomagolása, amelynek kezdődő neveket **BYOK-SecurityWorld - pkg-.**
- V nevű python parancsfájl**erifykeypackage.py.**
- Egy parancssori végrehajtható fájl elnevezett **KeyTransferRemote.exe** és a kapcsolódó DLL-ek.
- A vizuális C++ terjeszthető, nevű **vcredist_x64.exe.**

Másolja a csomag USB-meghajtóra vagy más hordozható tároló.

##<a name="step-2-prepare-your-disconnected-workstation"></a>Lépés: 2: Felkészülés a leválasztott munkaállomástól

Ez a második lépésben hajtsa végre az alábbi eljárások munkaállomás nem (az interneten vagy belső hálózatához) hálózathoz csatlakozik.


###<a name="step-21-prepare-the-disconnected-workstation-with-thales-hsm"></a>Lépés 2.1: Felkészülés a leválasztott munkaállomáson, Thales HSM

A nCipher (Thales) támogatási szoftver telepítése a Windows rendszerű számítógépen, és csatolja a Thales HSM arra a számítógépre.

Győződjön meg arról, hogy vannak-e a Thales eszközök a Path (**%nfast_home%\bin** és **%nfast_home%\python\bin**). Írja be például a következőket:

        set PATH=%PATH%;”%nfast_home%\bin”;”%nfast_home%\python\bin”

További tudnivalókért lásd: a felhasználói útmutatóban Thales működnek a beépített.

###<a name="step-22-install-the-byok-toolset-on-the-disconnected-workstation"></a>Lépés 2.2: Telepítse a BYOK toolset a leválasztott munkaállomáson

Másolja a BYOK toolset csomag az USB-meghajtóra vagy más hordozható tárolására, és végezze el az alábbi lépéseket:

1. Bontsa ki a fájlokat a letöltött csomagból bármely mappájában.
2. A mappából vcredist_x64.exe futtatni.
3. Kövesse az utasításokat a telepítés a Visual C++ futtatókörnyezet összetevőket a Visual Studio 2013.

##<a name="step-3-generate-your-key"></a>3 lépés: A kulcs generálása

Ebben a harmadik lépésben hajtsa végre az alábbi eljárások, a leválasztott munkaállomáson.

###<a name="step-31-create-a-security-world"></a>Lépés 3.1: Hozzon létre egy biztonsági világ

Nyisson meg egy parancssorablakot, és futtassa a Thales új nemzetközi programot.

    new-world.exe --initialize --cipher-suite=DLf1024s160mRijndael --module=1 --acs-quorum=2/3

Ez a program létrehoz egy **Biztonsági nemzetközi** fájlt %-os méretben NFAST_KMDATA%\local\world, amely megfelel a C:\ProgramData\nCipher\Key Management Settings\User mappába. A kvórum különböző értékek is használhatja, de ebben a példában, meg kell adnia adja meg a három üres kártyák és PIN-kódok egyenként. Ezt követően bármely két kártyák teljes hozzáférést biztosít a biztonsági világ. Ezek a kártyák válik a **Rendszergazda kártya meg** az új biztonsági nemzetközi.

Ezután tegye az alábbiakat:

- Készítsen biztonsági másolatot a világ fájlt. Biztonságos és védelme a világ fájlt, a rendszergazda kártyák és a PIN-kódok, és győződjön meg arról, hogy nincs egyetlen személynek egynél több kártya hozzáférése van-e.

###<a name="step-32-validate-the-downloaded-package"></a>A 3,2 lépés: Ellenőrizze a letöltött csomag

Ez a lépés nem kötelező de javasolt, hogy ellenőrizheti, hogy a következőket:

- A fő Exchange kulcs, amelyben szerepel a toolset a egy valódi Thales HSM hozott létre.
- A biztonsági világ, amelyben szerepel a toolset a kivonat az egy valódi Thales HSM létrehozva.
- A fő Exchange kulcs nem exportálható.

>[AZURE.NOTE]A letöltött csomag érvényesítéséhez működnek kell csatlakoznia, kapcsolva, és rendelkeznie kell a biztonsági világon rajta (például az imént létrehozott egy).

A letöltött csomag ellenőrzéséhez:

1.  Futtassa a verifykeypackage.py által és attól függően, hogy a földrajzi régióban vagy Azure-példányt tartson, az alábbiak egyikét:
    - Az Észak-Amerika:

            python verifykeypackage.py -k BYOK-KEK-pkg-NA-1 -w BYOK-SecurityWorld-pkg-NA-1
    - A Europe:

            python verifykeypackage.py -k BYOK-KEK-pkg-EU-1 -w BYOK-SecurityWorld-pkg-EU-1
    - Az ázsiai:

            python verifykeypackage.py -k BYOK-KEK-pkg-AP-1 -w BYOK-SecurityWorld-pkg-AP-1
    - Ha a Latin amerikai:

            python verifykeypackage.py -k BYOK-KEK-pkg-LATAM-1 -w BYOK-SecurityWorld-pkg-LATAM-1
    - Japánban:

            python verifykeypackage.py -k BYOK-KEK-pkg-JPN-1 -w BYOK-SecurityWorld-pkg-JPN-1
    - Ausztrália esetén:

            python verifykeypackage.py -k BYOK-KEK-pkg-AUS-1 -w BYOK-SecurityWorld-pkg-AUS-1
    - [Azure kormányzati](https://azure.microsoft.com/features/gov/), amely az Amerikai Egyesült Államok kormányzati példánya Azure használja:

            python verifykeypackage.py -k BYOK-KEK-pkg-USGOV-1 -w BYOK-SecurityWorld-pkg-USGOV-1
    - Kanada esetében:

            python verifykeypackage.py -k BYOK-KEK-pkg-CANADA-1 -w BYOK-SecurityWorld-pkg-CANADA-1
    - Németország esetében:

            python verifykeypackage.py -k BYOK-KEK-pkg-GERMANY-1 -w BYOK-SecurityWorld-pkg-GERMANY-1
    - Az indiai:

            python verifykeypackage.py -k BYOK-KEK-pkg-INDIA-1 -w BYOK-SecurityWorld-pkg-INDIA-1
    >[AZURE.TIP]A Thales ezek közé tartozik a %NFAST_HOME%\python\bin python

2.  Győződjön meg arról, hogy megjelenik a következő, amely jelzi a sikeres érvényességi: **eredmény: a siker**

A parancsfájl ellenőrzi az aláíró lánc felfelé a Thales legfelső szintű billentyűt. A kivonat a legfelső szintű kulcs van beágyazva, a parancsfájl, és az értéket kell **59178a47 de508c3f 291277ee 184f46c4 f1d9c639**. Ez az érték külön-külön is ellenőrizze a [Thales webhely](http://www.thalesesec.com/)felkeresésével.

Most már készen áll új kulcs létrehozásához.

###<a name="step-33-create-a-new-key"></a>Lépés 3.3: Hozzon létre egy új kulcs

Kulcs generálása Thales **generatekey** program használatával.

A következő parancsot a kulcs létrehozása:

    generatekey --generate simple type=RSA size=2048 protect=module ident=contosokey plainname=contosokey nvram=no pubexp=

Ez a parancs futtatásakor használja ezeket az utasításokat:

- A paraméter *védelme* meg kell érték **modul**látható módon. Ezzel létrehoz egy modul védett billentyűt. A BYOK toolset nem támogatja a billentyűparancsok OCS védett.

- *Contosokey* **ident** és **plainname** értékének cserélje tetszőleges karakterlánc. Felügyeleti költségek kisméretűvé alakítása és hibák a kockázat csökkentése, azt javasoljuk, hogy az azonos értékkel-et is. A **ident** értéket kell tartalmaznia, csak a számokat, a szaggatott vonal és a nagybetűk.

- A pubexp üres (alapértelmezett) ebben a példában marad, de megadhatja, hogy adott értékek. További információ a Thales dokumentációjában olvasható.

Ez a parancs hoz létre, **key_simple_**, a parancs megadott **ident** követ kezdődő nevű fájl tokenekre bontott billentyűt a %NFAST_KMDATA%\local mappában. Példa: **key_simple_contosokey**. A fájl tartalmaz egy titkosított kulcsot.

Készítsen biztonsági másolatot a tokenekre bontott kulcs fájlt egy megbízható helyre.

>[AZURE.IMPORTANT] Ha később átvitele Azure kulcs tárolóból elemre a termékkulcsot, a Microsoft nem exportálásával a kulcs vissza szeretné rendkívül fontos, hogy készítsen biztonsági másolatot a kulcs és biztonsági nemzetközi biztonságosan válik. Lépjen kapcsolatba a Thales útmutatást és a gyakorlati tanácsok a biztonsági mentése a termékkulcsot.

Most már készen áll a kulcs küldjenek Azure kulcs tárolóból elemre.

##<a name="step-4-prepare-your-key-for-transfer"></a>Lépés: 4: Felkészülés a termékkulcsot az átadás

Hajtsa végre az alábbi eljárások, a leválasztott munkaállomáson a negyedik lépése.

###<a name="step-41-create-a-copy-of-your-key-with-reduced-permissions"></a>4.1-es lépés: A kulcs másolatának létrehozása korlátozott engedélyekkel rendelkező

Az engedélyeket a termékkulcsot, a parancssorból csökkentéséhez futtatása, attól függően, hogy a földrajzi régióban vagy Azure-példányt tartson a következők valamelyikét:

- Az Észak-Amerika:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-NA-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-NA-1
- A Europe:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-EU-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-EU-1
- Az ázsiai:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-AP-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-AP-1
- Ha a Latin amerikai:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-LATAM-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-LATAM-1
- Japánban:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-JPN-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-JPN-1
- Ausztrália esetén:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-AUS-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-AUS-1
- [Azure kormányzati](https://azure.microsoft.com/features/gov/), amely az Amerikai Egyesült Államok kormányzati példánya Azure használja:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-USGOV-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-USGOV-1
- Kanada esetében:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-CANADA-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-CANADA-1
- Németország esetében:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-GERMANY-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-GERMANY-1
- Az indiai:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-INDIA-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-INDIA-1


Ez a parancs futtatásakor cseréje *contosokey* megadott azonos értékkel rendelkező **lépés 3.3: hozzon létre egy új kulcsot** a [a kulcs generálása](#step-3-generate-your-key) lépés.

Dugja be a biztonsági nemzetközi felügyeleti kártyák kérni.

Ha a parancs befejeződik, megjelenik **eredmény: sikeres** , és a termékkulcsot, korlátozott engedélyekkel rendelkező másolatát a key_xferacId_ nevű fájlt<contosokey>.

###<a name="step-42-inspect-the-new-copy-of-the-key"></a>Lépés 4.2:, Nézze meg az új példányát a billentyűt

Tetszés szerint futtassa a Thales kattintva erősítse meg az új kulcs minimális engedélyeinek segédprogramok:

- aclprint.PY:

        "%nfast_home%\bin\preload.exe" -m 1 -A xferacld -K contosokey "%nfast_home%\python\bin\python" "%nfast_home%\python\examples\aclprint.py"
- kmfile-dump.exe:

        "%nfast_home%\bin\kmfile-dump.exe" "%NFAST_KMDATA%\local\key_xferacld_contosokey"
Ezek a parancsok futtatásakor cseréje contosokey megadott azonos értékkel rendelkező **lépés 3.3: hozzon létre egy új kulcsot** a [a kulcs generálása](#step-3-generate-your-key) lépés.

###<a name="step-43-encrypt-your-key-by-using-microsofts-key-exchange-key"></a>A 4,3 lépés: Titkosítása a termékkulcsot a Microsoft fő Exchange kulcs használatával

Futtassa a következő parancsokat, attól függően, hogy a földrajzi régióban vagy Azure-példányt tartson egyikét:

- Az Észak-Amerika:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-NA-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-NA-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
- A Europe:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-EU-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-EU-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
- Az ázsiai:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-AP-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-AP-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
- Ha a Latin amerikai:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-LATAM-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-LATAM-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
- Japánban:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-JPN-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-JPN-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
- Ausztrália esetén:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-AUS-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-AUS-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
- [Azure kormányzati](https://azure.microsoft.com/features/gov/), amely az Amerikai Egyesült Államok kormányzati példánya Azure használja:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-USGOV-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-USGOV-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
- Kanada esetében:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-CANADA-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-CANADA-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
- Németország esetében:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-GERMANY-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-GERMANY-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
- Az indiai:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-INDIA-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-INDIA-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey


Ez a parancs futtatásakor használja ezeket az utasításokat:

- *Contosokey* cserélje ki az azonosítója, amellyel a kulcs generálása **lépés 3.3: hozzon létre egy új kulcsot** a [a kulcs generálása](#step-3-generate-your-key) lépés.

- *SubscriptionID* cserélje ki az azonosító az Azure előfizetés, amely tartalmazza a fő tárolóból elemre. Ezt az értéket lekért korábban, az **lépés 1.2-es: Ismerkedés az Azure előfizetés azonosítója** az [Előkészítés internetkapcsolattal rendelkező munkaállomástól](#step-1-prepare-your-internet-connected-workstation) lépés.

- *ContosoFirstHSMKey* cserélje le a címke, amellyel a kimeneti fájl neve.

Ezzel sikeresen befejeződött, amikor megjeleníti **eredmény: a siker** és az aktuális mappa, amely tartalmazza a következő nevet az új fájl: TransferPackage -*ContosoFirstHSMkey*.byok

###<a name="step-44-copy-your-key-transfer-package-to-the-internet-connected-workstation"></a>4.4 lépés: A fő átadás csomag másolása a internetkapcsolattal rendelkező munkaállomás

USB-meghajtóra vagy más hordozható tároló használja a kimeneti fájl másolása az előző lépésben (KeyTransferPackage-ContosoFirstHSMkey.byok) internetkapcsolattal rendelkező munkaállomástól.

##<a name="step-5-transfer-your-key-to-azure-key-vault"></a>5 lépés: A kulcs átvitele Azure kulcs tárolóból elemre

Utolsó lépésként internetkapcsolattal rendelkező munkaállomáson használja a [Hozzáadás-AzureKeyVaultKey](https://msdn.microsoft.com/library/azure/dn868048\(v=azure.300\).aspx) parancsmag töltse fel a fő átadás csomag másolt leválasztott munkaállomásról Azure kulcs tárolóra működnek:

    Add-AzureKeyVaultKey -VaultName 'ContosoKeyVaultHSM' -Name 'ContosoFirstHSMkey' -KeyFilePath 'c:\TransferPackage-ContosoFirstHSMkey.byok' -Destination 'HSM'

Ha a feltöltés befejeződött, akkor jelenik meg a kulcsot, közvetlenül a felvétele után tulajdonságainak.


##<a name="next-steps"></a>Következő lépések

Most már a HSM védett kulcs használhatja a fő tárolóból elemre. További információ című **hardveres biztonsági modult (HSM) használni kívánt** az [Azure kulcs tárolóból elemre az első lépések](key-vault-get-started.md) oktatóprogram során.
