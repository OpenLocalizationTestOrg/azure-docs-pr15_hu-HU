<properties
    pageTitle="Azure fájlt tároló problémák elhárítása |} Microsoft Azure"
    description="Azure fájlt tároló problémák elhárítása"
    services="storage"
    documentationCenter=""
    authors="genlin"
    manager="felixwu"
    editor="na"
    tags="storage"/>

<tags
    ms.service="storage"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/24/2016"
    ms.author="genli"/>

# <a name="troubleshooting-azure-file-storage-problems"></a>Azure fájlt tároló problémák elhárítása

Ez a cikk felsorolja a Windows és Linux ügyfelekről történő csatlakozáskor a Microsoft Azure fájltároló kapcsolódó gyakori problémák. Is tartalmazza a lehetséges okok és megoldások a következő problémákat.

**Általános problémák (történik a Windows és a Linux ügyfélprogramokban)**

- [Kvóta hiba, amikor megkísérli megnyitni egy fájlt](#quotaerror)

- [Gyenge teljesítményt elérésekor Azure tárhely Windows vagy Linux rendszerhez](#slowboth)

**A Windows ügyféllel kapcsolatos problémák**

- [A Windows 8.1 vagy Windows Server 2012 R2 Azure fájltároló elérésekor gyenge teljesítményt](#windowsslow)

- [Egy Azure fájlmegosztás csatlakoztatási kísérlet 53 hiba](#error53)

- [Nettó használata sikeres volt, de nem látható az Azure fájl megosztása a Windows Intézőben csatlakoztatott](#netuse)

- [Tárterület-fiókom tartalmaz "/" és a nettó sikertelen parancs használata](#slashfails)

- [Az alkalmazás vagy szolgáltatás a csatlakoztatott Azure fájlok meghajtó nem tud hozzáférni.](#accessfiledrive)

- [További javaslatok az optimális teljesítmény érdekében](#additional)

**Linux ügyféllel kapcsolatos problémák**

- ["A fájl szeretné átmásolni a cél, amely nem támogatja a titkosítást" hiba feltöltése vagy fájlok másolásakor Azure-fájlok](#encryption)

- [Megosztja a "host lefelé" hibaüzenet a létező fájl vagy a rendszerhéj lefagy a csatlakozási pont a parancslista során](#errorhold)

- [Csatlakoztassa a 115 hiba, amikor megpróbál a Linux virtuális csatlakoztatási Azure fájlok](#error15)

- [Véletlen késések, például "ls" parancsok történnek Linux virtuális](#delayproblem)

## <a name="general-problems"></a>Általános problémák
<a id="quotaerror"></a>
### <a name="quota-error-when-trying-to-open-a-file"></a>Kvóta hiba, amikor megkísérli megnyitni egy fájlt

A Windows rendszerben a következőhöz hasonló hibaüzenet jelenhet meg:

**1816 ERROR_NOT_ENOUGH_QUOTA <> – 0xc0000044**

**STATUS_QUOTA_EXCEEDED**

**Nincs elegendő kvóta nem érhető el ez a parancs feldolgozása**

**Érvénytelen leíró érték GetLastError: 53**

Linux a következőhöz hasonló hibaüzenet jelenhet meg:

**<filename>[hozzáférés megtagadva]**

**Lemezen kvóta kimerítve**

#### <a name="cause"></a>OK

A probléma oka az, hogy elérte a felső határ egyidejű megnyitott fogópontot fájl áll rendelkezésre.

#### <a name="solution"></a>Megoldás

Célszerű kevesebb egyidejű megnyitott fogópontok néhány fogópontok bezárásával, majd próbálkozzon újra. További tudnivalókért lásd: [Microsoft Azure tárhely a teljesítmény és méretezhetőség feladatlista](storage-performance-checklist.md).

<a id="slowboth"></a>
### <a name="slow-performance-when-accessing-file-storage-from-windows-or-linux"></a>Gyenge teljesítményt, amikor tárhely Windows vagy Linux rendszerhez

- Ha nem egy adott I/O minimális méret követelmény, azt javasoljuk, használható 1 MB I/O méretének az optimális teljesítmény eléréséhez.

- Ha tudja, hogy egy fájlt, amely az írások bővíti végleges méretét, és a szoftver nem rendelkezik, ha az még nem írt képviselőknek kapcsolatos kompatibilitási problémák a nullát tartalmazó fájlt, majd állítsa a fájl mérete előre minden írási helyett egy kiterjesztve írási alatt.

## <a name="windows-client-problems"></a>A Windows ügyféllel kapcsolatos problémák
<a id="windowsslow"></a>
### <a name="slow-performance-when-accessing-the-file-storage-from-windows-81-or-windows-server-2012-r2"></a>Amikor a tárhely Windows 8.1 vagy Windows Server 2012 R2 gyenge teljesítményt

Az ügyfelek, akik futtatja a Windows 8.1 vagy Windows Server 2012 R2 győződjön meg arról, hogy a javítás [KB3114025](https://support.microsoft.com/kb/3114025) telepítve van-e. A javítás javítja a létrehozás, majd zárja be a fogópont teljesítményét.

Ellenőrizze, hogy a javítás telepítve van az alábbi parancsfájl futtatását is lehetővé teszi:

`reg query HKLM\SYSTEM\CurrentControlSet\Services\LanmanWorkstation\Parameters\Policies`

Ha telepítve van a gyorsjavítás, a következő kimenet jelenik meg:

**HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\LanmanWorkstation\Parameters\Policies**

**{96c345ef-3cac-477b-8fcd-bea1a564241c} REG_DWORD 0x1**

> [AZURE.NOTE]  A Windows Server 2012 R2 képeket a Microsoft Azure piactéren a javítás KB3114025 indítása a Skype 2015 vállalati December alapértelmezés szerint telepítve van.

<a id="additional"></a>
### <a name="additional-recommendations-to-optimize-performance"></a>További javaslatok az optimális teljesítmény érdekében

Soha ne hozzon létre, vagy nyissa meg a fájl gyorsítótárazott i/o-írási, de nem az olvasási hozzáférést igénylő. Ez azt jelenti, hogy mikor, **CreateFile()')**hívás, soha nem adja meg, hogy csak a **GENERIC_WRITE**, de mindig adja meg a **GENERIC_READ |} GENERIC_WRITE**. A csak olvasásra fogópont nem gyorsítótár helyi meghajtóra, kis írások, akkor is, ha a fájlt a egyetlen megnyitott leíró azt. A szigorú teljesítményét ró kis írások. Figyelje meg, hogy a "egy" módba CRT **fopen()** megnyílik egy csak olvasásra fogópontot.

<a id="error53"></a>
### <a name="error-53-when-you-try-to-mount-or-unmount-an-azure-file-share"></a>"Hiba 53" jelenik meg csatlakoztatni, vagy egy Azure fájlmegosztás leválaszthatja

Ez a probléma az alábbi feltételek okozhatott:

#### <a name="cause-1"></a>OK 1

"A 53 rendszer hiba történt. Hozzáférés megtagadva." Biztonsági okokból kapcsolatok Azure fájlok részvényekkel le vannak tiltva, ha a kommunikációs csatorna nem titkosított, és a kapcsolódási kísérletre nem történik meg a központból azonos adatokat amelyen Azure fájlmegosztások találhatók. Kapcsolati csatorna titkosítási nincs megadva, ha a felhasználó ügyfél OS nem támogatják a kis-és Középvállalatok titkosítást. Ez jelzi a "53 a rendszer hiba történt. Hozzáférés megtagadva"hibaüzenet jelenik meg, ha a felhasználó megpróbál a helyszíni, vagy egy másik adatok központból fájlmegosztás csatlakoztatásához. Windows 8, Windows Server 2012-ben és az egyes egyeztetése kérést, amely tartalmazza a kis-és Középvállalatok 3.0-s, amely támogatja a titkosítást újabb verzióiban.

#### <a name="solution-for-cause-1"></a>Megoldás valamilyen okból 1

Csatlakozás egy ügyfélről, amely megfelel a Windows 8, Windows Server 2012 vagy újabb verzió, vagy, amely egy virtuális gép, amely az azonos adatközpont használt Azure tárterület-fiókként beolvasása a Azure fájlmegosztás.

#### <a name="cause-2"></a>OK 2

"Rendszer hiba 53" Ha csatlakoztatja egy Azure fájlmegosztás akkor fordulhat elő, ha a kimenő kommunikáció Port 445 Azure fájlok adatközpont zárolva van. Kattintson a [Itt](http://social.technet.microsoft.com/wiki/contents/articles/32346.azure-summary-of-isps-that-allow-disallow-access-from-port-445.aspx) engedélyezheti vagy tilthatja le a port 445 hozzáférést internetszolgáltatók összefoglalása megjelenítéséhez.

Comcast és néhány informatikai szervezetek blokkolja a port. Ha meg szeretné érteni, hogy ennek az oka mögött "rendszer 53" hibaüzenet, a Portqry használatával lekérdezheti a TCP:445 végpont. Ha a TCP:445 végpont szerint szűrve, a portot le van tiltva. Íme egy példa a lekérdezésre:

`g:\DataDump\Tools\Portqry>PortQry.exe -n [storage account name].file.core.windows.net -p TCP -e 445`

Ha a TCP 445 blokkolja a hálózati útvonalon szabály, a következő kimenet jelennek meg:

**TCP-port 445 (microsoft-ds szolgáltatás): SZŰRVE**

Portqry használatával kapcsolatos további tudnivalókért lásd: [a Portqry.exe parancssori segédprogram leírását](https://support.microsoft.com/kb/310099).

#### <a name="solution-for-cause-2"></a>Megoldás valamilyen okból 2

Nyissa meg a Port 445 [Azure IP-címtartományai](https://www.microsoft.com/download/details.aspx?id=41653)kimenő informatikai szervezete dolgozhat.

#### <a name="cause-3"></a>OK 3

"Rendszer hiba 53" is lehet fogadni, ha NTLMv1 kommunikációs engedélyezve van az ügyfélgépen. Kevésbé biztonságos ügyfél engedélyezett NTLMv1 problémákat hoz létre. Kapcsolati ezért, azzal blokkolhatja Azure-fájlok. Ellenőrizze, hogy ez a hiba oka, győződjön meg arról, hogy a következő beállításkulcsra a 3-as értékre van állítva:

HKLM\SYSTEM\CurrentControlSet\Control\Lsa > LmCompatibilityLevel.

További információ a [LmCompatibilityLevel](https://technet.microsoft.com/library/cc960646.aspx) témakört a TechNet webhelyen.

#### <a name="solution-for-cause-3"></a>Megoldás valamilyen okból 3

Ez a probléma megoldásához visszaállítása az alapértelmezett érték 3 HKLM\SYSTEM\CurrentControlSet\Control\Lsa beállításkulcs LmCompatibilityLevel értéke.

Azure fájlok csak az NTLMv2 hitelesítést támogatja. Győződjön meg arról, hogy az ügyfelek csoportházirend van hozzárendelve. Ez megakadályozza, hogy ez a hiba fordul elő. Ez akkor is tekintett biztonsági okokból. További tudnivalókért lásd: [beállításáról az ügyfelek számára a NTLMv2 csoportházirend használatával](https://technet.microsoft.com/library/jj852207(v=ws.11).aspx)

A javasolt házirend értéke **csak a válasz küldése NTLMv2**. Ez a 3-as beállításjegyzék értékre felel meg. Ügyfelek csak NTLMv2 hitelesítést, de azok NTLMv2 munkamenet használni, ha a kiszolgáló támogatja. Tartomány vezérlők LM, NTLM és NTLMv2 hitelesítési fogadja el.

<a id="netuse"></a>
### <a name="net-use-was-successful-but-dont-see-the-azure-file-share-mounted-in-windows-explorer"></a>Nettó használata sikeres volt, de nem jelenik meg az Azure fájl megosztása a Windows Intézőben csatlakoztatott

#### <a name="cause"></a>OK

Alapértelmezés szerint a Windows Intézőben nem Futtatás rendszergazdaként. Ha **nettó használja** a rendszergazdai parancssort, a hálózati meghajtóra leképezheti "Rendszergazdaként." Mivel a csatlakoztatott meghajtók felhasználói kötődnek, a felhasználói fiókot, hogy be van jelentkezve nem jeleníti meg a meghajtók ha azokat más felhasználói fiók van csatlakoztatva.

#### <a name="solution"></a>Megoldás

Csatlakoztassa a megosztás rendszergazdai jogokkal nem rendelkező parancssorából. Másik lehetőségként, kövesse a [TechNet témakör](https://technet.microsoft.com/library/ee844140.aspx) konfigurálása a **EnableLinkedConnections** beállításazonosítót.

<a id="slashfails"></a>
### <a name="my-storage-account-contains--and-the-net-use-command-fails"></a>Tárterület-fiókom tartalmaz "/" és a nettó sikertelen parancs használata

#### <a name="cause"></a>OK

A **nettó use** parancs a parancssor (cmd.exe) futtatásakor elemzi azt hozzáadásával "/" parancssori lehetőségként. Ennek hatására a meghajtó hozzárendelése sikertelen lesz.

#### <a name="solution"></a>Megoldás

Is használhatja az alábbi lépéseket a probléma megoldása:

• Használja az alábbi PowerShell-parancsot:

`New-SmbMapping -LocalPath y: -RemotePath \\server\share  -UserName acountName -Password "password can contain / and \ etc"`

A köteg fájlból ezt megteheti szerint

`Echo new-smbMapping ... | powershell -command –`

• Helyezi a dupla idézőjelek közé a probléma megoldásához billentyűt – kivéve ha "/" az első karaktere. Ha igen, az interaktív üzemmód használata és külön-külön adja meg a jelszót, vagy kérjen egy kulcsot, amely nem indul el, a perjel (/) karaktertől kezdve a billentyűk újragenerálása.

<a id="accessfiledrive"></a>
### <a name="my-applicationservice-cannot-access-mounted-azure-files-drive"></a>Az alkalmazás vagy szolgáltatás nem tud hozzáférni a csatlakoztatott Azure fájlok meghajtó

#### <a name="cause"></a>OK

Meghajtók felhasználónként van csatlakoztatva. Ha az alkalmazás vagy szolgáltatás különböző felhasználói fiók alatt fut, a felhasználók nem fogja látni a meghajtóra.

#### <a name="solution"></a>Megoldás

A azonos felhasználói fiókot, amely az alkalmazás van a meghajtót csatlakoztatni. Ezt megteheti az eszközökkel, például psexec.

Másik lehetőségként a hálózati szolgáltatás vagy a system fiók jogosultságaival egyenértékű jogosultságokkal rendelkező új felhasználó létrehozása, és futtassa a **cmdkey** és **nettó használata** az adott fiók. A felhasználó nevét kell a tároló fiók nevét, és a jelszó kell lennie a tárterületet fiókkulcs. **Nettó** használatra alternatíva átadni a tárterület-fiók felhasználónevét és a kulcsot a felhasználó nevét és jelszavát paraméterek a **nettó használata** parancsot.

Ezek a lépések után a következő hibaüzenet jelenhet: "a 1312 rendszer hiba történt. Egy adott munkamenetben nem létezik. Azt is, hogy már befejeződött" **nettó használata** a rendszer/hálózati szolgáltatásfiók futtatásakor. Ez akkor fordulhat elő, ha ügyeljen arra, hogy a felhasználónév **nettó használata** átadott tartomány adatait (például: "[tároló fiók neve]. file.core.windows .net").

## <a name="linux-client-problems"></a>Linux ügyféllel kapcsolatos problémák

<a id="encryption"></a>
### <a name="error-you-are-copying-a-file-to-a-destination-that-does-not-support-encryption"></a>"A fájl szeretné átmásolni a cél, amely nem támogatja a titkosítást" hibaüzenet

#### <a name="cause"></a>OK

Azure fájlok BitLocker titkosított fájlok is másolható. A fájltároló azonban nem támogatja a NTFS titkosított fájlrendszer. Ezért valószínűleg esetén titkosított fájlrendszer ebben az esetben. Ha titkosított fájlrendszer keresztül titkosított fájlok, kivéve, ha a Másolás parancs visszafejti másolt fájl egy másolás művelet szeretné a fájlt tároló sikertelen lesz.

#### <a name="workaround"></a>Megoldás:

Fájl másolása a tárhely, hogy meg kell először visszafejteni. Ehhez az alábbi módszerek egyikével:

• **Másolás /d**használja.

• Állítsa be a következő beállításkulcsot:

- Elérési út = HKLM\Software\Policies\Microsoft\Windows\System
- Típus = Duplaszó (DWORD)
- Név = CopyFileAllowDecryptedRemoteDestination
- Érték = 1

Megjegyzendő, hogy a beállításkulcs beállításakor hatása a hálózati megosztás összes másolása művelet.

<a id="errorhold"></a>
### <a name="host-is-down-error-on-existing-file-shares-or-the-shell-hangs-when-you-run-list-commands-on-the-mount-point"></a>Megosztja a "host lefelé" hibaüzenet a létező fájl vagy a rendszerhéj lefagy a csatlakozási pont parancslista futtatásakor

#### <a name="cause"></a>OK

Ez a hiba a Linux ügyfélszámítógépen fordul elő, ha az ügyfélnek nem dolgozik egy hosszabb ideig. Ez a hiba történik, ha az ügyfél kapcsolata megszakad, és az ügyfél kapcsolati időtúllépés történik.

#### <a name="solution"></a>Megoldás

Ez a probléma most már megoldódott a Linux rendszerhéj [módosítható](https://git.kernel.org/cgit/linux/kernel/git/torvalds/linux.git/commit/fs/cifs?id=4fcd1813e6404dd4420c7d12fb483f9320f0bf93), Linux terjesztési szóló függőben lévő backport részeként.

Működése a probléma megoldásához a kapcsolat fenntartása és elkerüléséhez üresjárati állapotba kerülnek, fájlok megtartása, amelyre írni szeretne rendszeresen Azure fájl megosztása. Azt kell az írást, például címek átírása a létrehozott/módosítás dátuma a dokumentumon. Egyébként is javulhat gyorsítótárazott eredményeket, és a művelet nem lehet elindítani a kapcsolatot.

<a id="error15"></a>
### <a name="mount-error-115-when-you-try-to-mount-azure-files-on-the-linux-vm"></a>"Hiba 115 csatlakoztatása" jelenik meg a Linux virtuális Azure fájlok csatlakoztatása

#### <a name="cause"></a>OK

Linux terjesztését eddig nem támogatták titkosítási szolgáltatás a kis-és Középvállalatok 3.0 programban. Az egyes terjesztését, felhasználói előfordulhat, hogy "115" hibaüzenetet kap, ha a kis-és Középvállalatok 3.0 miatt a hiányzó funkció használatával próbálnak csatlakoztatási Azure fájlokat.

#### <a name="solution"></a>Megoldás

Ha a kis-és Középvállalatok Linux ügyfél, amely nem támogatja a titkosítást, a csatlakozási Azure fájlok egy Linux virtuális a kis-és Középvállalatok 2.1-es meg a fájlt tároló fiókként az azonos Adatközpont használatával.

<a id="delayproblem"></a>
### <a name="linux-vm-experiencing-random-delays-in-commands-like-ls"></a>Véletlen késések, például "ls" parancsok történnek Linux virtuális

#### <a name="cause"></a>OK

Ez akkor fordulhat elő, ha a csatlakozási parancs nem tartalmaz a **serverino** lehetőséget. Nélkül **serverino**a ls parancs futtatja a **stat** valamennyi fájlban.

#### <a name="solution"></a>Megoldás

Jelölje be a "/ stb/fstab" tételben a **serverino** :

a/kezdőlap/sampledir típusú cifs azureuser.file.Core.Windows.NET/WMS/Comer (rw, nodev, relatime, ver = 2.1 sec ntlmssp, gyorsítótár = = szigorú, felhasználónév = xxx, tartomány X file_mode = 0755, dir_mode = 0755, serverino, rsize = 65536, wsize = 65536, actimeo = = 1)

Ha nem szerepel a **serverino** lehetőséget, válassza le, és csatlakoztassa ismét az Azure-fájlok úgy, hogy a **serverino** beállítást.

## <a name="learn-more"></a>tudj meg többet

- [A Windows Azure fájltároló – első lépések](storage-dotnet-how-to-use-files.md)

- [Első lépések az Azure Linux tárhely](storage-how-to-use-files-linux.md)
