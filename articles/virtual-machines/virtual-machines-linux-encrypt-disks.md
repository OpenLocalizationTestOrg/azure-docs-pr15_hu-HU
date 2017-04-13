<properties
   pageTitle="Titkosítsa a Linux virtuális a lemezt |} Microsoft Azure"
   description="Egy Linux virtuális az Azure CLI és az erőforrás-kezelő telepítési modell használata a lemezt titkosítása"
   services="virtual-machines-linux"
   documentationCenter=""
   authors="iainfoulds"
   manager="timlt"
   editor=""/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure"
   ms.date="10/11/2016"
   ms.author="iainfou"/>

# <a name="encrypt-disks-on-a-linux-vm-using-the-azure-cli"></a>Egy Linux virtuális az Azure CLI használata a lemezt titkosítása
Továbbfejlesztett virtuális gép (virtuális) és a megfelelőségre, az Azure virtuális lemezt a többi titkosítható. Az Azure kulcs tárolóból elemre a titkosított cryptographic billentyűkkel a lemez titkosított. A titkosítási kulcs vezérlése és használatuk naplózható. Ez a cikk a Linux virtuális az Azure CLI és az erőforrás-kezelő telepítési modell segítségével virtuális lemezről titkosítása részletezi.


## <a name="quick-commands"></a>Gyors parancsok
Ha gyorsan teljesítenie a tevékenységhez, az alábbiakat szakaszt a következő parancsok a virtuális virtuális lemezről titkosításához. Részletesebb információkat és az egyes lépések környezetben a többi a dokumentumot, a [kezdési itt](#overview-of-disk-encryption)találhatók.

Telepítve van, és az erőforrás-kezelő-üzemmód használata az alábbi képlettel történik-e jelentkezve a [Legújabb Azure CLI](../xplat-cli-install.md) lesz szüksége:

```
azure config mode arm
```

Az alábbi példák cserélje ki a kívánt értékét például paraméterek neve. Példa paraméternevek olyan `myResourceGroup`, `myKeyVault`, és `myVM`.

Először engedélyezése az Azure kulcs tárolóból elemre szolgáltató belül Azure-előfizetése, és hozzon létre egy erőforrás csoportot. Az alábbi példa létrehoz egy erőforrás csoportnevet `myResourceGroup` a a `WestUS` helyét:

```bash
azure provider register Microsoft.KeyVault
azure group create myResourceGroup --location WestUS
```

Hozzon létre egy Azure kulcs tárolóból elemre. Az alábbi példa létrehoz egy kulcsot tárolóra nevű `myKeyVault`:

```bash
azure keyvault create --vault-name myKeyVault --resource-group myResourceGroup \
  --location WestUS
```

A titkosítási kulcs létrehozása a kulcs tárolóból elemre, majd engedélyezze lemez titkosításhoz. Az alábbi példa létrehoz egy kulcsot nevű `myKey`:

```bash
azure keyvault key create --vault-name myKeyVault --key-name myKey \
  --destination software
azure keyvault set-policy --vault-name myKeyVault --resource-group myResourceGroup \
  --enabled-for-disk-encryption true
```

Hozzon létre egy végpontot Azure Active Directory használata a hitelesítés kezelését, és cseréje a titkosítási kulcs a kulcs tárolóból elemre. A `--home-page` és `--identifier-uris` nem kell tényleges átirányítható címét. A legmagasabb szintű biztonság, az ügyfél titkos kulcsok használandó jelszavak helyett. Az Azure CLI jelenleg nem hozható létre ügyfél titkos kulcsok. Ügyfél titkos kulcsok csak az Azure-portálon hozható létre. Az alábbi példa létrehoz egy névvel ellátott Azure Active Directory-végpontot `myAADApp` egy jelszót, és `myPassword`. Adja meg a saját jelszó, az alábbi képlettel történik:

```bash
azure ad app create --name myAADApp \
  --home-page http://testencrypt.contoso.com \
  --identifier-uris http://testencrypt.contoso.com \
  --password myPassword
```

Megjegyzés: a `applicationId` az előző parancs kimenetét látható. Ez az alkalmazás azonosítója használja az alábbi lépéseket:

```bash
azure ad sp create --applicationId myApplicationID
azure keyvault set-policy --vault-name myKeyVault --spn myApplicationID \
  --perms-to-keys [\"all\"] --perms-to-secrets [\"all\"]
```

Adatok lemezen hozzá egy meglévő virtuális. A következő példa adatok lemezen ad egy virtuális nevű `myVM`:

```bash
azure vm disk attach-new --resource-group myResourceGroup --vm-name myVM \
  --size-in-gb 5
```

Olvassa el a részleteket a kulcs tárolóból elemre, és a létrehozott billentyűt. A kulcs tárolóból elemre azonosító URI és kulcs van szüksége az utolsó lépésben URL-CÍMÉT. Az alábbi példa a részletek áttekinti a az egy kulcsot tárolóra nevű `myKeyVault` és nevű kulcs `myKey`:

```bash
azure keyvault show myKeyVault
azure keyvault key show myKeyVault myKey
```

Titkosítsa az alábbiak szerint a lemezt saját paraméternevek egész beírása:

```bash
azure vm enable-disk-encryption --resource-group myResourceGroup --vm-name myVM \
  --aad-client-id myApplicationID --aad-client-secret myApplicationPassword \
  --disk-encryption-key-vault-url myKeyVaultVaultURI \
  --disk-encryption-key-vault-id myKeyVaultID \
  --key-encryption-key-url myKeyKID \
  --key-encryption-key-vault-id myKeyVaultID \
  --volume-type Data
```

Az Azure CLI nem nyújt részletes hibák a titkosítási folyamat során. További hibaelhárítási tudnivalókért tekintse át a `/var/log/azure/Microsoft.OSTCExtensions.AzureDiskEncryptionForLinux/0.x.x.x/extension.log`. Az előző parancs a sok változó van, és előfordulhat, hogy nem kap sok feltüntetése, hogy miért nem sikerült a folyamat, a teljes parancs példa erre az alábbi képlettel történik:

```bash
azure vm enable-disk-encryption -g myResourceGroup -n MyVM \
  --aad-client-id 147bc426-595d-4bad-b267-58a7cbd8e0b6 \
  --aad-client-secret P@ssw0rd! \
  --disk-encryption-key-vault-url https://myKeyVault.vault.azure.net/ \ 
  --disk-encryption-key-vault-id /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.KeyVault/vaults/myKeyVault \
  --key-encryption-key-url https://myKeyVault.vault.azure.net/keys/myKey/6f5fe9383f4e42d0a41553ebc6a82dd1 \
  --key-encryption-key-vault-id /subscriptions/guid/resourceGroups/myResoureGroup/providers/Microsoft.KeyVault/vaults/myKeyVault \
  --volume-type Data
```

Végül tekintse át a titkosítási állapota erősítse meg, hogy a virtuális merevlemezeken most már titkosított. Az alábbi példa egy virtuális nevű állapotának ellenőrzése `myVM` a a `myResourceGroup` erőforráscsoport:

```bash
azure vm show-disk-encryption-status --resource-group myResourceGroup --name myVM
```

## <a name="overview-of-disk-encryption"></a>Lemezen titkosítási áttekintése
A titkosított Linux VMs virtuális lemezről a többi [a titkosítási dm](https://wikipedia.org/wiki/Dm-crypt)használatával. Az Azure virtuális lemez titkosítja ingyen nem. Azure kulcs tárolóból elemre, szoftver-védelemmel Cryptographic billentyűkkel tárolja, vagy a tanúsítvánnyal FIPS 140-2 szintet 2 szabványok hardver (HSMs) biztonsági modulban kulcsok létrehozásához vagy importálhat. Tartsa ellenőrzés e titkosítási kulcsok és használatuk naplózható. Titkosítási billentyűk titkosítása és a virtuális csatolni virtuális lemez visszafejtése használják. Azure Active Directory zárólap lehetővé teszi a biztonságos kiadására cryptographic billentyűk, mint VMs vannak kapcsolva a be- és kikapcsolása.

A titkosított egy virtuális történik, az alábbi képlettel történik:

1. A titkosítási kulcs létrehozása az Azure kulcs tárolóból elemre.
2. Állítsa be a titkosítási kulcs is használható a lemez titkosítására.
3. A titkosítási kulcs olvasható a az Azure kulcs tárolóból elemre, a megfelelő engedélyekkel Azure Active Directory-végpont létrehozása
4. Ki a virtuális lemezt, az Azure Active Directory-végpontot, és a használandó megfelelő titkosítási kulcs titkosítása parancsot.
5. Az Azure Active Directory-végpontot a szükséges titkosítási kulcs kér Azure kulcs tárolóból elemre.
6. A megadott titkosítási kulcs a virtuális lemezt titkosított.


## <a name="supporting-services-and-encryption-process"></a>Szolgáltatások és titkosítási folyamat támogatása
A következő további összetevőit támaszkodik, merevlemez-titkosítás:

- **Azure kulcs tárolóra** - használt cryptographic billentyűkkel védelme és titkos kulcsok a lemez titkosítás/visszafejtés folyamat használható. 
  - Ha van ilyen, használhatja a egy meglévő Azure kulcs tárolóból elemre. Nincs lemez titkosítja jelöl ki egy kulcsot tárolóból elemre kattintva.
  - Külön felügyeleti határai és a fő láthatóságát, létrehozhat egy dedikált kulcs tárolóból elemre.
- **Azure Active Directory** - kezeli a szükséges cryptographic billentyűkkel és a hitelesítéshez kért műveletekhez biztonságos cserél. 
  - Egy meglévő Azure Active Directory-példány használható az alkalmazás elhelyezésére szolgáló általában. 
  - Az alkalmazás kérése, és kérjen ki a megfelelő cryptographic billentyűkkel kulcs tárolóból elemre, és a virtuális gép szolgáltatásokra vonatkozó zárólap több. Ha nincs olyan tényleges alkalmazás, amely integrálódik az Azure Active Directory fejlesztéséhez.


## <a name="requirements-and-limitations"></a>Követelmények és korlátai
Támogatott forgatókönyvek és lemez titkosítási követelményei:

- A következő Linux kiszolgáló SKU - Ubuntu, CentOS, SUSE és SUSE Linux vállalati kiszolgáló (SLES) és piros kalap vállalati Linux rendszerhez.
- Az ugyanazon Azure régió és az előfizetés összes erőforrás (például kulcs tárolóból elemre, a tárterület-fiók és a virtuális) kell lennie.
- Szabványos A, D, DS, G és GS sorozat VMs.

Lemezen titkosítási jelenleg nem támogatott az alábbi esetekben:

- Egyszerű réteg VMs.
- A klasszikus telepítési modell használatával létrehozott VMs.
- Operációs rendszer lemez titkosításának Linux VMs letiltása.
- Egy már titkosított Linux virtuális cryptographic billentyűi frissíteni.


## <a name="create-the-azure-key-vault-and-keys"></a>Hozza létre a Azure kulcs tárolóból elemre, és a billentyűk
A többi Ez az útmutató elvégzéséhez szüksége van telepítve, és az erőforrás-kezelő-üzemmód használata az alábbi képlettel történik-e jelentkezve a [Legújabb Azure CLI](../xplat-cli-install.md) :

```
azure config mode arm
```

A parancs példák egész cserélje összes példa paramétert a saját nevét, helyét és kulcs értékeit. Az alábbi példák a szabályt használ `myResourceGroup`, `myKeyVault`, `myAADApp`stb.

Az első lépésként hozzon létre egy Azure kulcs tárolóból elemre a titkosítási kulcs tárolásához. Azure kulcs tárolóból elemre a billentyűk, a titkos kulcsok és a jelszót, amely engedélyezi a biztonságos végrehajtásukhoz-alkalmazások és szolgáltatások tárolhat. Virtuális lemezre titkosításhoz használatával kulcs tárolóból elemre a titkosítási kulcs, amely titkosítani vagy a virtuális merevlemezeken visszafejteni tárolni. 

Az Azure kulcs tárolóra szolgáltató az Azure-előfizetése engedélyezése, majd hozzon létre egy erőforrás csoportot. Az alábbi példa létrehoz egy névvel ellátott erőforráscsoport `myResourceGroup` a a `WestUS` helyét:

```bash
azure provider register Microsoft.KeyVault
azure group create myResourceGroup --location WestUS
```

Az Azure kulcs tárolóból elemre a titkosítási kulcs és a kapcsolódó számítási erőforrások, például a tárhely és a virtuális magát tartalmazó ugyanabban a régióban kell lennie. Az alábbi példa létrehoz egy Azure kulcs tárolóból elemre nevű `myKeyVault`:

```bash
azure keyvault create --vault-name myKeyVault --resource-group myResourceGroup \
  --location WestUS
```

A szoftver vagy a hardver biztonsági modell Hardvermodul védelem cryptographic billentyűkkel tárolhat. Hardvermodult használatához a prémium kulcs tárolóból elemre. Van egy további költség létrehozása egy prémium szabványos kulcs tárolóból elemre szoftverrel védett kulcsok tároló, hanem kulcs tárolóból elemre. Hozzon létre egy prémium kulcs tárolóból elemre, az előző lépésben adja hozzá `--sku Premium` a parancsra. A következő példa szoftverrel védett kulcsokat használ, mert létrehozott egy szabványos kulcs tárolóból elemre. 

Mindkét védelem modellek az Azure platform van szüksége a titkosítási kulcs kérése, a virtuális virtuális lemezt visszafejtése indításakor hozzáférési jogosultsággal. Hozzon létre a titkosítási kulcs belül a kulcs tárolóból elemre, majd a virtuális lemez titkosítással használatának engedélyezése. Az alábbi példa létrehoz egy kulcsot nevű `myKey` , és ezután lehetővé teszi az merevlemez-titkosítás:

```bash
azure keyvault key create --vault-name myKeyVault --key-name myKey \
  --destination software
azure keyvault set-policy --vault-name myKeyVault --resource-group myResourceGroup \
  --enabled-for-disk-encryption true
```


## <a name="create-the-azure-active-directory-application"></a>Az Azure Active Directory-alkalmazás létrehozása
Ha virtuális lemez titkosított vagy visszafejti, zárólap segítségével kezeli a hitelesítés és cseréje a titkosítási kulcs a kulcs tárolóból elemre. A végpontot, az Azure Active Directory-alkalmazások lehetővé teszi, hogy a megfelelő cryptographic billentyűkkel a virtuális nevében kérhet Azure platform. Egy alapértelmezett Azure Active Directory-példány érhető el az előfizetés, hogy hány szervezetek van dedikált Azure Active Directory könyvtárak.

Ne hozzon létre egy teljes Azure Active Directory-alkalmazást, mint a `--home-page` és `--identifier-uris` paraméterek az alábbi példában nem kell tényleges átirányítható címét. Az alábbi példában is itt adhatja meg, az Azure-portálra a létrehozásakor billentyűparancsok, hanem a jelszó-alapú titkos. Ebben az esetben, kulcs generálása nem történik az Azure CLI. 

Az Azure Active Directory-alkalmazás létrehozása Az alábbi példa létrehoz egy alkalmazás nevű `myAADApp` egy jelszót, és `myPassword`. Adja meg a saját jelszó, az alábbi képlettel történik:

```bash
azure ad app create --name myAADApp \
  --home-page http://testencrypt.contoso.com \
  --identifier-uris http://testencrypt.contoso.com \
  --password myPassword
```

Jegyezze le a `applicationId` , amely az eredményben által visszaadott az előző parancsot. Ez az alkalmazás azonosítója használatban van néhány további lépést. Ezután hozzon létre egy egyszerű szolgáltatásnévvel (SPN), hogy az alkalmazás a környezetén belül érhető el. Sikeresen titkosítására, vagy visszafejtése virtuális lemezre, a titkosítási kulcs tárolt kulcs tárolóból elemre vonatkozó engedélyek lehetővé teszi a felolvastatása az Azure Active Directory-alkalmazás meg kell. 

A SPN létrehozása, és beállíthatja a szükséges engedélyeket az alábbi képlettel történik:

```bash
azure ad sp create --applicationId myApplicationID
azure keyvault set-policy --vault-name myKeyVault --spn myApplicationID \
  --perms-to-keys [\"all\"] --perms-to-secrets [\"all\"]
```


## <a name="add-a-virtual-disk-and-review-encryption-status"></a>Adja hozzá a virtuális lemez és titkosítás állapotának ellenőrzése
Néhány virtuális lemez ténylegesen titkosítása lehetővé teszi, hogy a lemez hozzáadása egy meglévő virtuális. Adja hozzá 5Gb adatot lemezen egy meglévő virtuális az alábbi képlettel történik:

```bash
azure vm disk attach-new --resource-group myResourceGroup --vm-name myVM \
  --size-in-gb 5
```

A virtuális lemez jelenleg nem titkosítva. A virtuális aktuális titkosítási állapotának ellenőrzése, az alábbi képlettel történik:

```bash
azure vm show-disk-encryption-status --resource-group myResourceGroup --name myVM
```


## <a name="encrypt-virtual-disks"></a>Virtuális lemez titkosítása
Most már titkosítása virtuális lemezt, akkor eredményét az előző összetevőket:

1. Adja meg az Azure Active Directory-alkalmazás és a jelszavát.
2. Adja meg a titkosított merevlemezeken metaadatait tárolni a kulcs tárolóból elemre.
3. Adja meg, hogy a titkosítási kulcsokat a tényleges titkosítás és visszafejtés használható.
4. Adja meg, hogy szeretné-e az operációs rendszer szabad, az adatok lemezt vagy az összes titkosítása.

Lehetővé teszi, hogy olvassa el a részleteket az Azure kulcs tárolóból elemre, és a kulcs hozta létre, kulcs tárolóból elemre azonosító URI, és kattintson az utolsó lépésben az URL-cím kulcs:

```bash
azure keyvault show myKeyVault
azure keyvault key show myKeyVault myKey
```

Titkosítsa a virtuális merevlemezeken kimenetét használata a `azure keyvault show` és `azure keyvault key show` parancsok az alábbi képlettel történik:

```bash
azure vm enable-disk-encryption --resource-group myResourceGroup --vm-name myVM \
  --aad-client-id myApplicationID --aad-client-secret myApplicationPassword \
  --disk-encryption-key-vault-url myKeyVaultVaultURI \
  --disk-encryption-key-vault-id myKeyVaultID \
  --key-encryption-key-url myKeyKID \
  --key-encryption-key-vault-id myKeyVaultID \
  --volume-type Data
```

Az előző parancs a sok változó van, mint az az alábbi példa a hivatkozás teljes parancs:

```bash
azure vm enable-disk-encryption -g myResourceGroup -n MyVM \
  --aad-client-id 147bc426-595d-4bad-b267-58a7cbd8e0b6 \
  --aad-client-secret P@ssw0rd! \
  --disk-encryption-key-vault-url https://myKeyVault.vault.azure.net/ \ 
  --disk-encryption-key-vault-id /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.KeyVault/vaults/myKeyVault \
  --key-encryption-key-url https://myKeyVault.vault.azure.net/keys/myKey/6f5fe9383f4e42d0a41553ebc6a82dd1 \
  --key-encryption-key-vault-id /subscriptions/guid/resourceGroups/myResoureGroup/providers/Microsoft.KeyVault/vaults/myKeyVault \
  --volume-type Data
```

Az Azure CLI nem nyújt részletes hibák a titkosítási folyamat során. További hibaelhárítási tudnivalókért tekintse át a `/var/log/azure/Microsoft.OSTCExtensions.AzureDiskEncryptionForLinux/0.x.x.x/extension.log` a virtuális, akkor a titkosított meg.

Végezetül lehetővé teszi, hogy erősítse meg, hogy a virtuális merevlemezeken most már titkosított a titkosítási állapotának ellenőrzése:

```bash
azure vm show-disk-encryption-status --resource-group myResourceGroup --name myVM
```


## <a name="add-additional-data-disks"></a>További adatok lemezre hozzáadása
Miután titkosított adatok lemezt, később további virtuális lemez hozzáadása a virtuális, és is titkosítása őket. Ha futtatja a `azure vm enable-disk-encryption` paranccsal, növeli a sorozat verzióját használja a `--sequence-version` paraméter. A sorozat verzió paraméter lehetővé teszi a azonos virtuális ismételt műveletek hajthatók végre.

Ha például lehetővé teszi, hogy virtuális másodikon hozzáadása a virtuális az alábbi képlettel történik:

```bash
azure vm disk attach-new --resource-group myResourceGroup --vm-name myVM \
  --size-in-gb 5
```

Futtassa újra a parancsot a virtuális lemezt titkosítása ez idő hozzáadása a `--sequence-version` paraméterre, és írja be az első futtatásakor látható módon növekvő:

```bash
azure vm enable-disk-encryption --resource-group myResourceGroup --vm-name myVM \
  --aad-client-id myApplicationID --aad-client-secret myApplicationPassword \
  --disk-encryption-key-vault-url myKeyVaultVaultURI \
  --disk-encryption-key-vault-id myKeyVaultID \
  --key-encryption-key-url myKeyKID \
  --key-encryption-key-vault-id myKeyVaultID \
  --volume-type Data
  --sequence-version 2
```


## <a name="next-steps"></a>Következő lépések

- Azure kulcs tárolóból elemre, beleértve a titkosítási kulcs és tárolókban, törlése kezelésével kapcsolatban további tudnivalókat a [kulcs tárolóra kezelése használata CLI](../key-vault/key-vault-manage-with-cli.md)témakörben talál.
- További információt a lemez titkosítás, például egy titkosított egyéni virtuális Azure-feltöltése előkészítése a [Azure lemezre titkosítás](../security/azure-security-disk-encryption.md)témakörben talál.