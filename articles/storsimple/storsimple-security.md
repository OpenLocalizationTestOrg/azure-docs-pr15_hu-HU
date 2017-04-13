<properties 
   pageTitle="StorSimple biztonsági |} Microsoft Azure" 
   description="Az StorSimple szolgáltatás, az eszköztől és a helyszíni és felhőbeli adatok védelme biztonsági és adatvédelmi szolgáltatások ismertetése" 
   services="storsimple" 
   documentationCenter="NA" 
   authors="SharS" 
   manager="carmonm" 
   editor=""/>

<tags
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD" 
   ms.date="05/03/2016"
   ms.author="v-sharos"/>

# <a name="storsimple-security-and-data-protection"></a>StorSimple biztonság és adatvédelem

## <a name="overview"></a>– Áttekintés

Biztonsági fő fontos van egy új technológia elfogadása, különösen akkor, ha a technológiát használja a bizalmas vagy saját adataival mindenki számára. Ahogy felmérése a különböző technológiák, figyelembe kell vennie megnövelt kockázatok és az adatvédelem költségek. Microsoft Azure StorSimple nyújt a biztonsági és adatvédelmi megoldás adatvédelem, annak biztosítására, így: 

- **Bizalmas** – csak az adatok megtekintése engedéllyel rendelkező szervezetek. 
- Csak a hitelesített szervezetek **integritását** – módosíthatja és törölheti az adatokat.

A Microsoft Azure StorSimple megoldás négy fő összetevői, amelyek együttműködnek a többi áll:

- **Microsoft Azure-ban tárolt StorSimple kezelő szolgáltatás** – az alkalmazáskezelési szolgáltatás konfigurálása és a StorSimple eszköz kiépítése használt.
- **StorSimple eszköz** – egy telepítve van a adatközpontban fizikai eszközt. Hosts és a ügyfélprogramokat sorolja készítése adatok csatlakoztatása a StorSimple eszköz, és az eszköz kezeli az adatokat, és áthelyezi az Azure felhő szükség szerint.
- **Ügyfelek/hosts csatlakozik az eszköz** – az ügyfelek a infrastruktúra, amely a StorSimple eszköz csatlakozzon, és készítése a védeni kívánt adatokat.
- **Felhőalapú tárhelyek** – adatok tárolásának helye a Azure felhőben helyét.

Az alábbi szakaszok ismertetik a StorSimple az összetevők, illetve a rajtuk tárolt adatok védelme biztonsági funkciói. Azt is, lehet, hogy a Microsoft Azure StorSimple biztonsági, illetve a megfelelő válaszokkal kapcsolatos kérdések listáját tartalmazza.

## <a name="storsimple-manager-service-protection"></a>StorSimple kezelő szolgáltatás védelme

A StorSimple kezelő szolgáltatás Microsoft Azure-ban tárolt és a szervezet közvetített összes StorSimple eszközök kezelésére szolgáló felügyeleti szolgáltatás. A StorSimple kezelő szolgáltatás elérheti az Azure klasszikus portálra böngészőn keresztül jelentkezzen be a szervezeti hitelesítő adatok segítségével. 

A StorSimple kezelő szolgáltatás használatához a szervezetnél az Azure előfizettek StorSimple. Az előfizetés szabályozza a Funkciók, amelyek az Azure klasszikus portálon érheti el. Ha a szervezet nem az Azure előfizetéssel rendelkezik, és azt szeretné, ha többet szeretne tudni őket, olvassa el a [Regisztrálás az Azure, mint egy szervezet](../active-directory/sign-up-organization.md)című témakört. 

A StorSimple kezelő szolgáltatás üzemelteti Azure-ban, mert azt az Azure biztonsági funkciók védik. A Microsoft Azure által biztosított biztonsági szolgáltatásaival kapcsolatos további tudnivalókért nyissa meg a [Microsoft Azure az Adatvédelmi központ](https://azure.microsoft.com/support/trust-center/security/).

## <a name="storsimple-device-protection"></a>StorSimple eszköz védelme

StorSimple eszköz egy helyszíni hibrid tárolóeszközre (SSD) és a merevlemez-meghajtók (HDDs), együtt felesleges vezérlők, és automatikusan átveszi lehetőségeket tartalmazó. A vezérlők tiering, úgy, hogy éppen használt (vagy meleg) adatok helyi tárolás (a StorSimple eszköz vagy a helyszíni kiszolgálót), a kisebb a gyakran használt adatok áthelyezése a felhőbe közben tárhely kezelése

Csak a hitelesített StorSimple eszközök használhatók a StorSimple kezelő szolgáltatás az Azure előfizetés létrehozott csatlakozni szeretne. Kattintva engedélyezheti egy eszközt, regisztrálnia kell azt a StorSimple Manager szolgáltatással, mert a szolgáltatás regisztrációs billentyűt. A szolgáltatás regisztrációs billentyűt az Azure klasszikus portálon létrehozott 128 bites véletlen kulcs. 

![Szolgáltatás regisztrációs kulcs](./media/storsimple-security/ServiceRegistrationKey.png)

Megtudhatja, hogyan el a szolgáltatás regisztrációs termékkulccsal, nyissa meg a [2 lépés: a szolgáltatás regisztrációs kulcs első](storsimple-deployment-walkthrough.md#step-2-get-the-service-registration-key).

A szolgáltatás regisztrációs kulcsa egy hosszú kulcs 100 + karaktereket tartalmaz. Másolja a billentyűt, és mentse egy biztonságos helyen szövegfájlban, hogy szükség esetén további eszközök engedélyezése használni. A szolgáltatás regisztrációs kulcs elvész, az első eszköz regisztrálását követően, ha a StorSimple kezelő szolgáltatás hozhat létre új kulcsot. Ez a művelet a meglévő eszközök nem érinti. 

Miután regisztrált egy eszközt, kommunikáció a Microsoft Azure tokenek használ. A szolgáltatás regisztrációs kulcs eszköz regisztráció után nem használható.

> [AZURE.NOTE] Azt javasoljuk, hogy minden használata után a szolgáltatás regisztrációs kulcs újragenerálása.

## <a name="protect-your-storsimple-solution-via-passwords"></a>A StorSimple megoldás keresztül jelszavak védelme

Jelszavak számítógépen biztonsági fontos eleme, és a StorSimple megoldás érdekében győződjön meg arról, hogy az adatok csak az engedélyezett felhasználók számára hozzáférhető öröklődést használt. StorSimple lehetővé teszi a következő jelszavak beállítása:

- StorSimple eszközt rendszergazdai jelszó
- Hitelesítés CHAP Handshake Protocol () kezdeményező- és célwebhelyek jelszavak kéri.
- StorSimple pillanatkép Manager jelszó

### <a name="windows-powershell-for-storsimple-and-the-storsimple-device-administrator-password"></a>A Windows PowerShell StorSimple és a StorSimple eszközt rendszergazdai jelszó

StorSimple a Windows PowerShell a parancssor StorSimple eszközök kezeléséhez használható. A Windows PowerShell-StorSimple funkciók, amelyek lehetővé teszik, hogy regisztráljon az eszközön, a hálózati kapcsolat beállítása mobileszközön, bizonyos típusú frissítések telepítése, az eszköz hibaelhárítása elérése a támogatási munkamenethez és a eszköz állapotának módosítása tartalmaz. Érheti el a Windows PowerShell-StorSimple csatlakoztatása az eszközre a soros konzol vagy távelérési a Windows PowerShell használatával.

A PowerShell távelérési HTTPS vagy HTTP történik. Ha engedélyezve van a Távfelügyelet HTTPS, szüksége lesz a Távfelügyelet tanúsítvány letöltése az eszközre, és telepítse a távoli ügyfél. További információt a PowerShell távelérési megnyitásához [Csatlakozás távoli StorSimple eszközére](storsimple-remote-connect.md).

Miután a Windows PowerShell-StorSimple csatlakoztatni az eszközt, szüksége lesz való bejelentkezéshez az eszközt, a eszközt rendszergazdai jelszó megadása.

![Eszköz rendszergazdai jelszó](./media/storsimple-security/DeviceAdminPW.png)

Az alábbi gyakorlati tanácsokat tartsa szem előtt:

- Távfelügyelet alapértelmezés szerint ki van kapcsolva. A StorSimple kezelő szolgáltatás használatával kapcsolhatja be. Biztonsági okokból távelérési engedélyezni csak időszakban az, hogy valóban szüksége van rá.
- Ha például módosítja a jelszót, feltétlenül összes távelérési a felhasználók értesítése, így azok nem tapasztal az váratlan kapcsolódási veszteség.
- A StorSimple kezelő szolgáltatás nem tudja beolvasni a meglévő jelszavak: azt csak átállíthatja őket. Azt javasoljuk, hogy tárolja az összes jelszavak biztonságos helyen, hogy a jelszó alaphelyzetbe állítása, ha elfelejti, nem rendelkezik. Ha a jelszó alaphelyzetbe állítása van szükség, ügyeljen arra, hogy értesítést küldjön minden felhasználónak, előtt, hozzon létre egy új. 

A Windows PowerShell-felület elérheti az eszközre a soros kapcsolat használatával. Is elérheti, távolról HTTP vagy HTTPS, amely nagyobb biztonságot nyújt. HTTPS itt magasabb szintű biztonság, mint a soros vagy a HTTP-kapcsolatot. Azonban HTTPS használatához előbb telepítenie kell egy tanúsítványt az ügyfélszámítógépen, az eszköz hozzáférő. Az eszköz beállítása lapon az StorSimple kezelő szolgáltatás letöltheti a távelérési tanúsítvány. Ha elveszik a tanúsítvány távoli eléréséhez, töltse le egy újat és propagálása az minden, használja a Távfelügyelet jogosult ügyfélnek.

### <a name="challenge-handshake-authentication-protocol-chap-initiator-and-target-passwords"></a>Hitelesítés CHAP Handshake Protocol () kezdeményező- és célwebhelyek jelszavak kéri.

CHAP egy távoli ügyfelek azonosítása érvényesítéséhez StorSimple eszköz által használt hitelesítési mód. Az igazolás megosztott jelszó alapul. Lehet, hogy CHAP egyirányú (egyirányú) vagy kölcsönös (kétirányú). Egyirányú CHAP, az a cél (StorSimple eszköz) hitelesíti a kezdeményező (állomás). Kölcsönös vagy fordított CHAP és elő kell készítenie, hogy a célhely hitelesíteni a kezdeményező a kezdeményező hitelesítse a cél. A StorSimple beállítható úgy, hogy mindkét módszer használható.

Tartsa szem előtt a következőket a CHAP konfigurálásakor:

- A CHAP felhasználónév 233-nál kevesebb karakterből kell tartalmaznia.
- A CHAP jelszót 12 és 16 karaktert között kell lennie. Kísérel meg hosszabb, felhasználónév vagy jelszóval a Windows állomáson hitelesítési hibát eredményez.
- A CHAP kezdeményező és a CHAP cél is ugyanazt a jelszót nem használható.
- Miután beállította a jelszót, módosítható, de nem tudja visszaszerezni. A jelszava módosul, ne meg, hogy tud csatlakozni tudnak a StorSimple eszközt a összes távelérési felhasználók.

További információt a CHAP, és hogyan kell beállítani a StorSimple megoldás lépjen [Konfigurálása CHAP StorSimple mobileszközére](storsimple-configure-chap.md).

### <a name="storsimple-snapshot-manager-password"></a>StorSimple pillanatkép Manager jelszó

StorSimple pillanatkép felügyelője egy Microsoft Management Console (MMC) beépülő modul, amely a mennyiségi csoportok és a Windows kötet árnyék szolgáltatás használja az alkalmazás egységes biztonsági másolatok készítése. Ezeken kívül biztonsági ütemterveket és adatfeliratsor hozhat létre és távolíthat el kötet StorSimple pillanatkép Manager is használhatja.

Egy eszköz StorSimple pillanatkép-kezelővel konfigurálásakor szükséges a StorSimple pillanatkép Manager jelszó megadását. Erre a jelszóra van először megadása a Windows PowerShellben StorSimple regisztráció során. A jelszó is beállítása és a StorSimple kezelő szolgáltatás változik. Erre a jelszóra hitelesíti a StorSimple pillanatkép kezelő eszközt.

![StorSimple pillanatkép Manager jelszó](./media/storsimple-security/SnapshotMgrPassword.png)

A StorSimple pillanatkép Manager jelszó 14-15 karakterből kell állnia, és legalább 3 nagybetűsre, kisbetűsre, numerikus és speciális karakterek kombinációjából kell tartalmazó. Miután megadta a StorSimple pillanatkép Manager jelszót, módosítható, de nem tudja visszaszerezni. Ha például módosítja a jelszót, ügyeljen arra, hogy értesítést küldjön minden távoli felhasználó.

Az StorSimple pillanatkép Manager kapcsolatos további tudnivalókért kattintson a [StorSimple pillanatkép Manager?](storsimple-what-is-snapshot-manager.md)

### <a name="password-best-practices"></a>Helyszavak helyes használatáról

Azt javasoljuk, hogy biztosítása érdekében, hogy StorSimple jelszavak erős és jól védett érdekében kövesse az alábbi útmutatásokat:

- Minden három hónap jelszavának módosítása A jelszó módosítása kényszerített archiválásra.
- Erős jelszavak használata. További információért lépjen hozhat [létre erősebb jelszavakat, és védelmük](http://blogs.microsoft.com/cybertrust/2014/08/25/create-stronger-passwords-and-protect-them/).
- Mindig másik access szerkezetek; a különböző jelszavak használata a jelszavak, adja meg, hogy minden egyes egyedinek kell lennie.
- Nem osztja meg a jelszavak, akinek nincs engedélyezve a StorSimple eszköz eléréséhez.
- Nem felolvasás elé mások jelszóval kapcsolatban, vagy a jelszó formátumának indexútmutató.
- Ha azt gyanítja, hogy egy fiókot vagy jelszó napvilágra kerülhetett, a eseményről szóló jelentés az adatok biztonsági részleg.
- Az összes jelszavak kezelje bizalmas, a bizalmas információkat. 

## <a name="storsimple-data-protection"></a>StorSimple adatok védelme

Ez a szakasz ismerteti a StorSimple a hálózaton átvitt adatok és a tárolt adatok védelme biztonsági funkciói.

Más szakaszban leírtak jelszavak használata engedélyezheti és hitelesíti a felhasználókat, mielőtt azok is hozzáférhetnek a StorSimple megoldás. Egy másik biztonsági szempontokat védi az adatokat a nem engedélyezett felhasználó tároló rendszerek között, és miközben folyamatban tárolására van továbbított közben. Az alábbi szakaszok ismertetik a mellékelt StorSimple adatok védelmi funkciókkal.

> [AZURE.NOTE] Deduplication tárolt StorSimple az eszközre, és a Microsoft Azure-tárolóban lévő adatok további védelmet nyújt. Adatok deduplicated van, amikor a data objects tárolódnak a térképen, és elérheti őket metaadatok: nincs rendelkezésre álló tárhely szintű környezet szeretné állítania az adatok alapján a mennyiségi szerkezetének, a fájlrendszer és a fájl nevét.

## <a name="protect-data-flowing-through-the-service"></a>Lépés a szolgáltatáson keresztül adatok védelme

Az elsődleges a StorSimple kezelő szolgáltatás célja kezeléséhez, és állítsa be a StorSimple eszközt. A StorSimple kezelő szolgáltatás fut, a Microsoft Azure-ban. Az Azure klasszikus portal segítségével adja meg az eszköz konfigurációs adatokat, és a Microsoft Azure a használja az StorSimple kezelő szolgáltatás elküldi az adatokat az eszközön. StorSimple biztosíthatja, hogy az Azure szolgáltatás egy biztonságos nem lesz a tárolt adatok biztonságos aszimmetrikus kulcs párokká rendszert használja. 

![Adatok titkosítását folyamatban](./media/storsimple-security/DataEncryption.png)

A aszimmetrikus kulcsfontosságú adatokhoz segít az adatokat a szolgáltatáson keresztül átfolyása az alábbiak szerint:

1. Adatok titkosítási tanúsítvány használó aszimmetrikus nyilvános és titkos kulcsok pár jön létre az eszközre, és az adatok védelmére szolgál. A billentyűparancsok akkor jön létre, amikor az első eszközön van regisztrálva. 
2. Az adatok titkosítási tanúsítvány kulcsok a szolgáltatás adatok titkosítókulcs, amely 128 bites kulcs erős, véletlenszerűen hozza létre az első eszközön regisztráció során védett személyes információkat Exchange (.pfx) fájlba exportálja.
3. A nyilvános kulcs a tanúsítvány a StorSimple kezelő szolgáltatás biztonságos elérhetővé válik, és a titkos kulcs továbbra is az eszköz.
4. Adatok bevitele, a szolgáltatás a biztosítása, hogy az Azure szolgáltatás nem tudja visszafejteni az adatokat, az eszköz halad nyilvános kulcs és visszafejtett az eszközön tárolt titkos kulccsal titkosítva.

A szolgáltatás adatok titkosítókulcs csak az első eszközön regisztrálva van a szolgáltatás jön létre. Minden későbbi eszköz regisztrált a szolgáltatással az azonos adatokat titkosítókulcs kell használnia. 

> [AZURE.IMPORTANT] 
> 
> Nagyon fontos, hogy az adatok titkosítókulcs másolatot készíteni, és mentse a biztonságos helyen. Az adatok titkosítókulcs másolatának oly módon, hogy egy hivatalos személy által is elérhető, és az eszköz rendszergazdáját egyszerűen tájékoztatni kell tárolni.
>
> A szolgáltatás adatok titkosítási kulcs elvész, ha a Microsoft támogatási személy segíthetnek beolvasásához, feltéve, hogy legalább egy eszköz online állapotban van. Azt javasoljuk, hogy a szolgáltatás adatok titkosítási kulcs módosítása után azt veszi. További tudnivalókért folytassa [adatok titkosítókulcs módosítása](storsimple-service-dashboard.md#change-the-service-data-encryption-key).

Adatok titkosítókulcs és a megfelelő adatokat titkosítási tanúsítvány módosíthatja a szolgáltatás irányítópulton a **szolgáltatás adatok titkosítási kulcs módosítása** lehetőséget. Győződjön meg arról, hogy az adatok biztonsági nem sérül, fizikai StorSimple eszközt kell használnia a szolgáltatás adatok titkosítási kulcs módosítása. A titkosítási kulcs módosítása, hogy az összes eszközön frissülni az új kulcs használatához szükséges. Emiatt azt javasoljuk, hogy a kulcs módosítsa, ha az összes eszközökön online állapotban. Eszközök offline módban, ha igényelhetnek különböző egyszerre is módosíthatók. Az elévült billentyűkkel eszközök alkalmazásait továbbra is használhatja a biztonsági mentést, de ezek nem lesznek állítsa vissza az adatokat, amíg a kulcs frissül. További tudnivalókért olvassa el az [használata az StorSimple kezelő szolgáltatás irányítópulton](storsimple-service-dashboard.md).

A szolgáltatás adatok titkosítási kulcs és az adatok titkosítási tanúsítvány nem jár le. Jó helyen jár azt javasoljuk, hogy módosítja a szolgáltatás adatok titkosítási kulcs évente kulcs nem biztonságos megelőzése érdekében.

## <a name="protect-data-at-rest"></a>A többi adatok védelme

A StorSimple eszköz kezeli a adatokat tárolja őket meghatározási helyi meghajtóra, és a felhőben, attól függően, hogy a használat gyakorisága. Az összes host gépek, amelyeket az eszköz csatlakoztatott adatok küldése az eszközre, amely az adatokat a felhőben, szükség szerint helyezi át. Adatok átkerül az eszközről a felhőben biztonságosan az interneten keresztül. Az egyes egy iSCSI-tároló, amely megjeleníti az összes megosztott kötet eszközön futó tartalmaz. Az összes adat titkosítva van tároló cloud elküldés előtt. 

![Felhőalapú tárolási titkosítási kulcs](./media/storsimple-security/CloudStorageEncryption.png)

Biztonság és a felhőbe került adatok integritását biztosítása érdekében StorSimple lehetővé teszi a következőképpen adhatja meg a felhőalapú tárolási titkosítási kulcs:

- A felhőalapú tárolási titkosítókulcs mennyiségi tároló létrehozásakor adja meg. A kulcs nem módosítható, vagy később fel. 
- A mennyiségi tárolóban lévő összes kötet ugyanazt a titkosítási kulcs megosztása. Ha azt szeretné, hogy egy másik űrlap egy adott kötet titkosítási, azt javasoljuk, hogy hoz létre egy új mennyiségi tároló tárolni, hogy a kötet.
- A felhőalapú tárolási titkosítókulcs írja be a StorSimple kezelő szolgáltatás, amikor a kulcs titkosítsa az adatok titkosítókulcs nyilvános részének használ, és elküldi az eszközön.
- A felhőalapú tárolási titkosítókulcs nem az van tárolva bárhol a szolgáltatást, és csak az eszközre az ismert.
- Egy felhőalapú tárolási titkosítási kulcs megadása nem kötelező. Az eszköz szolgáltatónál titkosított adatokat is küldhet.

### <a name="additional-security-best-practices"></a>További biztonsággal kapcsolatos gyakorlati tanácsok

- Adatforgalom felosztása: a vállalati helyi felhasználó forgalmat a SAN iSCSI elkülönítése üzembe helyezése a teljes mértékben szétválasztott hálózati és VLAN hol fizikai elkülönítési lehetőség nem. Egy dedikált hálózati tartozó garantálja a biztonság és a teljesítmény fontos üzleti adatát. Tárterület és a felhasználó forgalom keverése a vállalati hálózaton keresztül nem ajánlott is időtartam növelése és hálózati problémákhoz.

- A host melletti hálózati biztonság használja a hálózati kapcsolatok, amelyek támogatják a TCP/IP kiürítése motor (TOE). TOE Processzor betöltés csökkenti a hálózati adapteren a TCP feldolgozása.

## <a name="protect-data-via-storage-accounts"></a>Tárterület-fiókok keresztül adatok védelme

Minden Microsoft Azure-előfizetés egy vagy több tárterület-fiókokat hozhat létre. (Egy tárterület-fiókkal rendelkezik egy egyedi névtér az Azure a felhőben tárolt adatok használatához.) A tárterület-fiók elérésének a előfizetés access billentyűkombinációt, hogy tárterületet fiókkal társított szabályozza. 

Amikor létrehoz egy tárterület-fiókkal, a Microsoft Azure két 512 bites tároló hívóbetűk, amelyek közül hitelesítéshez használt StorSimple eszköz fér hozzá a tárterület-fiók esetén hoz létre. Ne feledje, hogy csak az egyik billentyűk használja. A többi billentyűt a foglalás, amivel a billentyűk rendszeres elforgatása kell tartani. Billentyűk forgatásához aktívvá tenni a másodlagos billentyűt, és törölje az elsődleges kulcs. Ezután létrehozhat egy új kulcs használható a következő forgatás közben. (Biztonsági okokból sok adatközpontokkal szükségesek az kulcs Forgatás.) 

Azt javasoljuk, kövesse az alábbi gyakorlati tanácsok a fő Forgatás:

- Tárterület fiók billentyűk rendszeresen biztosíthatja, hogy a tárterület-fiók nem érhető el nem engedélyezett felhasználók által kell elforgatása.
- Rendszeres időközönként a Azure rendszergazda vagy kell váltania az elsődleges és másodlagos kulcsa újragenerálása az Azure klasszikus portál tároló szakasza így közvetlenül elérheti a tárterület-fiók használatával.


## <a name="protect-data-via-encryption"></a>Titkosítási keresztül adatok védelme

StorSimple használja a következő titkosítási algoritmusok tárolt adatok védelme és a StorSimple megoldás összetevői közötti út közben.

| Algoritmus | Kulcs hossza | Protokoll/alkalmazások/megjegyzések |
| --------- | ---------- | ------------------------------- |
| RSA       | 2048       | RSA PKCS 1 1.5 használja, az Azure klasszikus portál titkosítsa az eszközt küldött konfigurációs adatok: például tárterület fiók hitelesítő adatait, StorSimple eszköz konfigurálása és a felhőbeli tárhelyek titkosítási kulcs. |
| AES       | 256        | A CBC AES titkosítása adatok titkosítókulcs nyilvános része, az Azure klasszikus portálra a StorSimple eszközről elküldése előtt használják. Azt is használják a StorSimple eszköz adatainak titkosítására, felhőalapú tárolási fiókba az adatok elküldése előtt. |


## <a name="storsimple-virtual-device-security"></a>StorSimple virtuális eszköz biztonsági

[AZURE.INCLUDE [storsimple virtual device security](../../includes/storsimple-virtual-device-security.md)]

## <a name="frequently-asked-questions-faq"></a>Gyakori kérdések

Az alábbi táblázat néhány kapcsolatos kérdések és válaszok biztonság és a Microsoft Azure StorSimple.

**Q:** A szolgáltatás sérül. Mi legyen a következő lépések?

**A:** Azonnal adatainak titkosítókulcs és a tárterület-fiók tiering adatokhoz használt tárterület fiók billentyűparancsok módosítani kell. Utasításokért nyissa meg: 

- [A szolgáltatás adatok titkosítási kulcs módosítása](storsimple-service-dashboard.md#change-the-service-data-encryption-key)
- [Kulcs Elforgatás tárterület-fiókok](storsimple-manage-storage-accounts.md#key-rotation-of-storage-accounts)

**Q:** A szolgáltatás regisztrációs kulcs van kérő új StorSimple eszközt van. Hogyan beolvasásához azt?

**A:** A kulcs első létrehozásakor a StorSimple kezelő szolgáltatás hozták létre. A StorSimple kezelő szolgáltatás használatakor csatlakoztatni az eszközt a szolgáltatás első lépések lap segítségével megtekintése vagy a szolgáltatás regisztrációs kulcs újragenerálása. Új szolgáltatás regisztrációs kulcs létrehozása meglévő bejegyzett eszközök nincs hatással. Utasításokért nyissa meg:

- [Megtekintése vagy a szolgáltatás regisztrációs kulcs újragenerálása](storsimple-service-dashboard.md#view-or-regenerate-the-service-registration-key)

**Q:** A szolgáltatás adatok titkosítási kulcs elvesztettem. Mit tegyek?

**A:** Lépjen kapcsolatba a Microsoft-támogatást. Az eszköztől és a Súgó beolvasni a kulcsot (ha legalább egy eszközt az interneten található) a támogatási munkamenetre is bejelentkeznek. Adatok titkosítókulcs beszerzéséhez után azonnal módosítania kell, hogy győződjön meg arról, hogy az új kulcs ismert csak az Ön. Utasításokért nyissa meg:

- [A szolgáltatás adatok titkosítási kulcs módosítása](storsimple-service-dashboard.md#change-the-service-data-encryption-key)

**Q:**  Lehet egy eszközt szolgáltatás adatok titkosítási kulcs módosítására jogosult, de nem indult el a legfontosabb változások folyamat. Mit kell tenni?

**A:** Ha az időtúllépés lejárt, szüksége lesz az eszköz a szolgáltatás adatok titkosítási kulcs változtatások zavartalanul, és indítsa újra a folyamat.

**Q:**  A szolgáltatás adatok titkosítási kulcs módosítása, de lehet nem frissíthetők az egyéb eszközök 4 órán belül. Indítsa el újra kell?

**A:** A 4 óra időszakon belül csak a változás kezdeményezése nem. Miután elindította a frissítési folyamat hivatalos StorSimple az eszközre, az engedély érvénytelen mindaddig, amíg az összes eszközön frissülnek.

**Q:** A StorSimple rendszergazda elhagyta a vállalat. Mit kell tenni?

**A:** Módosíthatja és a jelszót az StorSimple eszközön való hozzáférés engedélyezése, valamint annak érdekében, hogy az új adatok nem ismert jogosulatlan személyek szolgáltatás adatok titkosítókulcs. Utasításokért nyissa meg:

- [A StorSimple kezelő szolgáltatás használatával storsimple jelszavának módosítása](storsimple-change-passwords.md)
- [A szolgáltatás adatok titkosítási kulcs módosítása](storsimple-service-dashboard.md#change-the-service-data-encryption-key)
- [Az StorSimple eszköz CHAP konfigurálása](storsimple-configure-chap.md)

**Q:** Szeretném ahhoz, hogy a StorSimple pillanatkép Manager jelszót a szolgáltató, amely a StorSimple eszköz csatlakozik, de nem érhető el a jelszót. Mit lehet tenni?

**A:** Ha elfelejtette a jelszavát, akkor hozzon létre egy újat. Ezután feltétlenül tájékoztassa minden meglévő, hogy megváltozott-e a jelszót, és, hogy azok frissítenie kell használni az új jelszót az ügyfelek. Utasításokért nyissa meg:

- [StorSimple pillanatkép Manager jelszavának módosítása](storsimple-change-passwords.md#change-the-storsimple-snapshot-manager-password)
- [Hitelesítő eszköz](storsimple-snapshot-manager-manage-devices.md#authenticate-a-device)

**Q:** A tanúsítvány távoli eléréséhez a Windows PowerShell-StorSimple módosítását követően az eszközön. Hogyan módosíthatom az távelérési ügyfeleimet?

**A:** Az új tanúsítvány letöltése a StorSimple kezelő szolgáltatás, és majd meg kell adnia, hogy telepítve van a távelérési ügyfelet tárolóban található. Utasításokért nyissa meg:

- [Tanúsítvány importálása parancsmag](https://technet.microsoft.com/library/hh848630.aspx)

**Q:** Az adatok védett esetén a StorSimple kezelő szolgáltatás sérül van?

**A:** Szolgáltatás konfigurációs adatok mindig titkosítva van a nyilvános kulccsal webböngészőben megtekintésekor. A szolgáltatás nem fér hozzá a titkos kulcs, mert a szolgáltatás nem tudja jelennek meg az adatok. A StorSimple kezelő szolgáltatás sérül meg, ha nincs nincs hatása, mivel nincsenek a StorSimple kezelő szolgáltatás tárolt kulcsok.

**Q:** Ha valaki hozzáfér az adatok titkosítási tanúsítvány, lesz az adatok kell sérül?

**A:** Microsoft Azure a felhasználói adatok titkosítási kulcs (.pfx fájl) tárolja a titkosított formátumban. Mivel a .pfx fájl titkosítva van, és a StorSimple szolgáltatás nem tartozik a .pfx fájl visszafejtése adatok titkosítókulcs, egyszerűen a hozzáférést a .pfx fájl első fog nem jelenítik meg bármely titkos kulcsok.

**Q:** Mi történik, ha egy kormányzati entitás Microsoft kér az adataimat?

**A:** A szolgáltatás titkosítsa az összes adatot, és a titkos kulcs legyen az eszközhöz kapott, mert a kormányzati entitás kérje meg az ügyfél adatokhoz. 

## <a name="next-steps"></a>Következő lépések

A [központi telepítés StorSimple eszközére](storsimple-deployment-walkthrough.md).
 
