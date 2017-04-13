<properties 
    pageTitle="SSH billentyűk használata a Windows Linux VMs |} Microsoft Azure" 
    description="Megtudhatja, hogy miként hozhat létre és SSH billentyűk segítségével Windows rendszerű csatlakozás a Azure virtuális Linux géphez." 
    services="virtual-machines-linux" 
    documentationCenter="" 
    authors="squillace" 
    manager="timlt" 
    editor=""
    tags="azure-service-management,azure-resource-manager" />

<tags 
    ms.service="virtual-machines-linux" 
    ms.workload="infrastructure-services" 
    ms.tgt_pltfrm="vm-linux" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/18/2016" 
    ms.author="rasquill"/>

# <a name="how-to-use-ssh-keys-with-windows-on-azure"></a>Használat SSH kulcsok a Windows Azure-on

> [AZURE.SELECTOR]
- [A Windows](virtual-machines-linux-ssh-from-windows.md)
- [A Mac vagy Linux rendszerhez](virtual-machines-linux-mac-create-ssh-keys.md)

Linux virtuális gépeken futó (VMs) Azure-ban való csatlakozáskor [nyilvános kulcsú](https://wikipedia.org/wiki/Public-key_cryptography) kínál biztonságosabbá jelentkezzen be a Linux virtuális kell használnia. Ez a folyamat magában foglalja a nyilvános és titkos kulcs exchange a biztonságos rendszerhéj (SSH) paranccsal hitelesítést végezni, saját maga, hanem egy felhasználónevet és jelszót. A jelszavak megkülönböztetik próbálkozásos támadások, különösen a internetes VMs például webkiszolgálón érinti. Ez a cikk áttekintést nyújt SSH kulcsok és a Windows rendszerű a megfelelő billentyűk létrehozásának módját.


## <a name="overview-of-ssh-and-keys"></a>SSH és a billentyűk – áttekintés

Biztonságosan jelentkezhet a Linux virtuális nyilvános és titkos kulcsok használatával:

- A **nyilvános kulcshoz** a Linux virtuális vagy bármely más szolgáltatás használata a nyilvános kulcsú használni kívánt kerül.
- A **titkos kulcs** , mit váltást a Linux virtuális amikor bejelentkezik az, hogy igazolja személyazonosságát. A titkos kulcs védelme. Ne ossza meg.

A nyilvános és titkos kulcsokat több VMs és szolgáltatások is használható. Az egyes virtuális vagy el szeretné érni szolgáltatás szükségtelen billentyűk két. A részletes ismertetése című témakörben található [nyilvános kulcsú](https://wikipedia.org/wiki/Public-key_cryptography).

SSH egy titkosított kapcsolat protokoll, amely lehetővé teszi, hogy a biztonságos bejelentkezés nem biztonságos kapcsolaton keresztül. Az alapértelmezett csatlakozási protokoll az Azure-ban tárolt Linux VMs. Bár maga SSH titkosított kapcsolatot lehetővé teszi, a jelszavak használatáról SSH kapcsolatokkal továbbra is elhagyja a virtuális próbálkozásos célú támadásokkal és a jelszavak találgatás kitéve. Egy biztonságosabbá, és előnyben részesített, egy virtuális SSH használatával történő csatlakozás módja a nyilvános és titkos kulcsok, más néven SSH billentyűk segítségével.

Ha nem szeretné használni a SSH kulcsok, akkor is továbbra is jelentkezzen be a jelszóval Linux VMs. Ha a virtuális nem érhető el az internethez, elegendő lehet a jelszavak használatáról. Azonban van szüksége a jelszavak kezelése minden Linux virtuális és és karbantartását megfelelő jelszóházirendek eljárások, például a jelszó minimális hosszát és rendszeres frissítése. SSH kulcsok használatának egyszerűbben egyes hitelesítő adatok kezelése több VMs keresztül.


## <a name="windows-packages-and-ssh-clients"></a>Windows-csomagok és SSH-ügyfélprogramok.

Csatlakozzon, és kezelheti a Linux VMs **ssh** ügyfélprogram Azure-ban. Windows rendszerű számítógépeken általában nem rendelkezik egy **ssh** ügyfélprogrammal. Közös Windows SSH ügyfelek telepítheti az alábbi csomagok szerepelnek:

- [A Windows mely számjegy](https://git-for-windows.github.io/)
- [GITT](http://www.chiark.greenend.org.uk/~sgtatham/putty/)
- [MobaXterm](http://mobaxterm.mobatek.net/)
- [Cygwin](https://cygwin.com/)

> [AZURE.NOTE] A Windows 10 évforduló legfrissebb Bash Windows tartalmazza. Ez a szolgáltatás lehetővé teszi, hogy futtassa a Windows alrendszer Linux és az access segédprogramok, például egy SSH ügyfél számára. Bash Windows fejlesztés alatt, és bétaverziójával számít. Bash Windows kapcsolatos további tudnivalókért olvassa el a [a Windows Ubuntu Bash](https://msdn.microsoft.com/commandline/wsl/about)című témakört.


## <a name="which-key-files-do-you-need-to-create"></a>Milyen fontos fájlok kell létrehozása?

Azure igényel legalább 2048 bites **ssh-rsa** nyilvános és titkos kulcsok formázhatja. Ha a klasszikus telepítési modell használata Azure erőforrások tartománya, is szeretne létrehozni egy PEM (`.pem` fájl).

Az alábbiakban a telepítési forgatókönyveket és használhatja az egyes fájltípusok:

1. **ssh-rsa** kulcsok szükségesek bármely az [Azure portal](https://portal.azure.com)telepítési és erőforrás-kezelő telepítések az [Azure CLI](../xplat-cli-install.md)használatával.
    - Billentyűk rendszerint a legtöbben az összes szükséges.
2. `.pem`a fájl a [Klasszikus portál](https://manage.windowsazure.com)VMs létrehozásához szükséges. A klasszikus környezetekben, amelyek az [Azure CLI](../xplat-cli-install.md)billentyűk is támogatott.
    - Csak kell ezek további billentyűk és tanúsítványok létrehozása, ha a klasszikus telepítési modell használatával létrehozott erőforrások tartománya.


## <a name="install-git-for-windows"></a>Telepítse a Windows mely számjegy

Az előző szakaszban felsorolt több csomagokat, amely tartalmazza a `openssl` eszköz Windows. Ez az eszköz nyilvános és titkos kulcsok létrehozására van szükség. Az alábbi példák részletesen telepítése és használata **Windows mely számjegy**, azonban megadhatja, hogy bármelyik inkább csomagot. **Mely számjegy a Windows** néhány további Megnyitás-forrás szoftver ([OSS](https://en.wikipedia.org/wiki/Open-source_software)) eszközök és segédprogramok, akkor lehet hasznos, Linux VMs használatakor is hozzáférést biztosít.

1. Töltse le és telepítse **A Windows mely számjegy** a következő helyen található: [https://git-for-windows.github.io/](https://git-for-windows.github.io/).

2. Fogadja el az alapértelmezett beállításokat a telepítés során, kivéve, ha kifejezetten módosítania kell őket.

3. **Mely számjegy Bash** futtassa a **Start menü** > **mely számjegy** > **Mely számjegy Bash**. Az alábbi példa a konzol néz:

    ![Mely számjegy-Windows Bash rendszerhéj](./media/virtual-machines-linux-ssh-from-windows/git-bash-window.png)


## <a name="create-a-private-key"></a>Hozzon létre egy titkos kulcs

1. A **Mely számjegy Bash** ablakban, `openssl.exe` titkos kulcs létrehozásához. Az alábbi példa létrehoz egy kulcsot nevű `myPrivateKey` és a tanúsítvány nevű `myCert.pem`:

    ```bash
    openssl.exe req -x509 -nodes -days 365 -newkey rsa:2048 \
        -keyout myPrivateKey.key -out myCert.pem
    ```

    A kimeneti formátumban a következőhöz hasonló:

    ```bash
    Generating a 2048 bit RSA private key
    .......................................+++
    .......................+++
    writing new private key to 'myPrivateKey.key'
    -----
    You are about to be asked to enter information that will be incorporated
    into your certificate request.
    What you are about to enter is what is called a Distinguished Name or a DN.
    There are quite a few fields but you can leave some blank
    For some fields there will be a default value,
    If you enter '.', the field will be left blank.
    -----
    Country Name (2 letter code) [AU]:
    ```

2. Válaszolja meg a képernyőn megjelenő utasításokat ország nevét, a helyet, a szervezet neve, a stb.

3. Az új titkos kulcs és a tanúsítvány az aktuális munkakönyvtár jönnek létre. Biztonsággal kapcsolatos gyakorlati tanácsok, az kell engedélyeket állít be az a titkos kulcs, hogy csak érhető el:

    ```bash
    chmod 0600 myPrivateKey
    ```

4. Meg kell klasszikus erőforrások kezelésére, átalakíthatja a `myCert.pem` való `myCert.cer` (DER kódolású X509 tanúsítvány). Csak akkor, ha kifejezetten erőforrások régebbi klasszikus kell ez nem kötelező lépés végrehajtása 

    Konvertálja a tanúsítvány használatával az alábbi parancsot:

    ```bash
    openssl.exe  x509 -outform der -in myCert.pem -out myCert.cer
    ```

## <a name="create-a-private-key-for-putty"></a>A titkos kulcs gitt létrehozása

GITT a Windows közös SSH ügyfél. Szabadon használni kívánt SSH jegyzetlapon. GITT használatához szükséges hoz létre egy további kulcs – egy gitt titkos kulcs (PPK). Ha nem szeretné használni a gitt, hagyja ki az ebben a szakaszban.

Az alábbi példa létrehoz kifejezetten az gitt használni a további titkos kulcs:

1. **Mely számjegy Bash** segítségével a titkos kulcs konvertálása egy RSA titkos kulcs, amely érthető PuTTYgen. Az alábbi példa létrehoz egy kulcsot nevű `myPrivateKey_rsa` a meglévő kulcs nevű `myPrivateKey`:

    ```bash
    openssl rsa -in ./myPrivateKey.key -out myPrivateKey_rsa
    ```

    Biztonsággal kapcsolatos gyakorlati tanácsok, az kell engedélyeket állít be az a titkos kulcs, hogy csak érhető el:

    ```bash
    chmod 0600 myPrivateKey_rsa
    ```

2. Töltse le és PuTTYgen futtassa a következő helyről: [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)

3. Kattintson a menü: **fájl** > **betöltése a titkos kulcs**

4. Keresse meg a titkos kulcs (`myPrivateKey_rsa` az előző példában). Az alapértelmezett könyvtár **Mely számjegy Bash** indításakor `C:\Users\%username%`. A fájl módosításához megjelenítéséhez **az összes fájl (\*.\*)**:

    ![A meglévő titkos kulcs betöltése PuTTYgen](./media/virtual-machines-linux-ssh-from-windows/load-private-key.png)

5. Kattintson a **Megnyitás**gombra. Egy figyelmeztető üzenet jelzi, hogy a kulcs sikeresen importálva lett-e:

    ![PuTTYgen sikeresen importálva billentyűvel](./media/virtual-machines-linux-ssh-from-windows/successfully-imported-key.png)

6. Kattintson **az OK gombra** kattintva zárja be a kérdés.

7. A nyilvános kulcs a **PuTTYgen** ablak tetején jelenik meg. Másolja és illessze be a nyilvános kulcshoz az Azure-portálra vagy az erőforrás-kezelő Azure-sablon egy Linux virtuális létrehozásakor. A **nyilvános kulcshoz mentése** másolatának mentése a számítógépre is kattinthat:

    ![Mentés gitt nyilvános kulcs](./media/virtual-machines-linux-ssh-from-windows/save-public-key.png)

    A következő példa bemutatja, hogyan meg kellene másolása és beillesztése a nyilvános kulcshoz az Azure portal egy Linux virtuális létrehozásakor. A nyilvános kulcs általában ezt követően tárolni a `~/.ssh/authorized_keys` meg az új virtuális.

    ![Nyilvános kulcs használata egy virtuális az Azure-portálon létrehozásakor](./media/virtual-machines-linux-ssh-from-windows/use-public-key-azure-portal.png)

7. Újra **PuTTYgen**, kattintson a **Mentés titkos kulcs**:

    ![Titkos kulcs gitt fájl mentése](./media/virtual-machines-linux-ssh-from-windows/save-ppk-file.png)

    > [AZURE.WARNING] Egy figyelmeztető üzenet kéri, hogy szeretné-e a jelszó megadása a kulcs nélkül továbbra is. A jelszó olyan, mintha egy jelszót, a titkos kulcs csatolva. Még ha valaki a titkos kulcs juthat, azok továbbra is szeretné nem fognak tudni hitelesítést végezni csak a billentyűvel. A jelszó szükséges is tenné. Anélkül, hogy egy jelszót Ha valaki megkapja a titkos kulcs azokat is jelentkezzen be bármilyen virtuális vagy a szolgáltatás által használt billentyűre. Azt javasoljuk, hogy hoz létre egy jelszót. Ha elfelejtette a jelszavát, van azonban nem tudja helyreállítani.

    Ha ki szeretne adni egy jelszót, kattintson a **nem**, adjon meg egy jelszót a PuTTYgen főablakában, és válassza a **titkos kulcs mentése** ismét. Egyéb esetben kattintson az **Igen** anélkül, hogy a választható jelszó továbbra is.

8. Írja be a nevét és helyét a PPK fájl mentéséhez.


## <a name="use-putty-to-ssh-to-a-linux-machine"></a>A Linux géphez SSH gitt használata

Ismét gitt a Windows közös SSH ügyfél. Szabadon használni kívánt SSH jegyzetlapon. A következő lépések részletesen használatáról a titkos kulcs hitelesíteni a Azure virtuális SSH használatával. A lépések hasonlóak más SSH kulcs az ügyfelek értelmez erre szolgáló betöltése a titkos kulcs a SSH kapcsolat hitelesítést végezni.

1. Töltse le és a következő helyről futtatása gitt: [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)

2. Töltse ki az állomásnév vagy az Azure portálról a virtuális IP-címe:

    ![Nyissa meg az új gitt kapcsolat](./media/virtual-machines-linux-ssh-from-windows/putty-new-connection.png)

3. Korábban **megnyitott**, kattintson a **kapcsolat** > **SSH** > **Auth** fülre. Tallózással keresse meg és jelölje ki a titkos kulcs:

    ![Jelölje ki a gitt titkos kulcs hitelesítéshez](./media/virtual-machines-linux-ssh-from-windows/putty-auth-dialog.png)

4. Kattintson a **Megnyitás** a virtuális gép csatlakoztatása
 

## <a name="next-steps"></a>Következő lépések
[Az OS X és Linux](virtual-machines-linux-mac-create-ssh-keys.md)nyilvános és titkos kulcsok is készítése.

Bash a Windows és a Windows rendszerű számítógépén a könnyen hozzáférhető OSS eszközök problémákat előnyei kapcsolatos további tudnivalókért lásd: [a Windows Ubuntu Bash](https://msdn.microsoft.com/commandline/wsl/about).

Ha problémákat tapasztal a Linux VMs való kapcsolódáshoz SSH használ, olvassa el a [Hibaelhárítás SSH kapcsolatok egy Linux Azure virtuális](virtual-machines-linux-troubleshoot-ssh-connection.md)című témakört.