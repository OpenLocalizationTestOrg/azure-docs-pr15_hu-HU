<properties
   pageTitle="Kulcs tárolóra fejlesztői útmutató |}] A Microsoft Azure"
   description="A fejlesztők Azure kulcs tárolóra segítségével kezelheti a titkosítási kulcsokat a Microsoft Azure környezetben. "
   services="key-vault"
   documentationCenter=""
   authors="BrucePerlerMS"
   manager="mbaldwin"
   editor="bruceper" />
<tags
   ms.service="key-vault"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="10/03/2016"
   ms.author="bruceper" />

# <a name="azure-key-vault-developers-guide"></a>Borzas kulcs tárolóra fejlesztői útmutató
Kulcs tárolóra használva lesz biztonságosan hozzáférhetnek a bizalmas adatok belül az alkalmazásokat úgy, hogy:

- Kulcsok és titkok védett anélkül, hogy saját maga írja be a kódot, és nem fogja könnyen tudja használni azokat az alkalmazásokat.
- Láthatja, hogy az ügyfelek saját és a saját kulcsok kezelése, az alapvető szoftver szolgáltatásokat nyújtó összpontosít. Ily módon az alkalmazások lesz nem tulajdonosa a felelősség vagy a vevők bérlő kulcsok és titkok az esetleges felelősségre.
- Az alkalmazás használhatja a billentyűk aláírási és titkosítási még tartja a kulcskezelési külső alkalmazásból úgy, hogy az oldat megfelelő földrajzilag elosztott alkalmazás.

- Szeptember 2016 megjelenésével a kulcs tárolóra, az alkalmazások hajtsa végre kulcs tárolóra felhasználása tanúsítványokat. További tudnivalókért lásd: **billentyűk, a titkokat, és a tanúsítványokkal kapcsolatos** cikk a [többi hivatkozás](https://msdn.microsoft.com/library/azure/dn903623.aspx).

Azure kulcs tárolóra kapcsolatos általános tudnivalók bővebben: [kulcs tárolóra](key-vault-whatis.md).

## <a name="videos"></a>Videók
Ez a videó bemutatja, hogyan lehet létrehozni saját kulcs tárolóra és "Helló kulcs tárolóra" minta alkalmazása segítségével.

[AZURE.VIDEO azure-key-vault-developer-quick-start]

A videó az említett erőforrások mutató hivatkozások:
- [Borzas PowerShell](http://go.microsoft.com/fwlink/p/?linkid=320376&clcid=0x409)
- [Borzas kulcs tárolóra mintakód](http://go.microsoft.com/fwlink/?LinkId=521527&clcid=0x409)

További információt több is kövesse a [Kulcs tárolóra Blog](http://aka.ms/kvblog) , és részt vesznek a [Kulcs tárolóra fórum](http://aka.ms/kvforum).

## <a name="creating-and-managing-key-vaults"></a>Létrehozása és kezelése a kulcs elzárása

Előtt dolgozik a kódban Azure kulcs tárolóra, hozzon létre, és elzárása keresztül a többi, az erőforrás-kezelő sablonok, a PowerShell vagy a CLI, kezelése, a következő cikkekben ismertetett módon:

- [Létrehozhatja és kezelheti a többi kulcs elzárása](https://msdn.microsoft.com/library/azure/mt620024.aspx)
- [Létrehozása és kezelése a PowerShell kulcs elzárása](key-vault-get-started.md)
- [Létrehozása és kezelése a CLI kulcs elzárása](key-vault-manage-with-cli.md)
- [Kulcs elzárva létrehozása és hozzáadása egy titkos keresztül egy erőforrás-kezelő Azure sablon](../resource-manager-template-keyvault.md)

>[AZURE.NOTE] Kulcs elzárása műveleteket keresztül HRE hitelesíti és engedélyezi a kulcs tárolóra meg saját hozzáférési házirend, egy tárolóra meghatározás keresztül.

## <a name="coding-with-key-vault"></a>A kulcs tárolóra kódolása

A kulcs tárolóra kezelőrendszer programozóknak áll számos, az alapítvány, mint a többi [Kulcs tárolóra REST API Felületével kapcsolatos hivatkozások](https://msdn.microsoft.com/library/azure/dn903609.aspx).

Sikeres engedélyével, a következőket teheti:

- Titkosítási kulcsok [létrehozása](https://msdn.microsoft.com/library/azure/dn903634.aspx), [importálása](https://msdn.microsoft.com/library/azure/dn903626.aspx), [frissítése](https://msdn.microsoft.com/library/azure/dn903616.aspx), [törlése](https://msdn.microsoft.com/library/azure/dn903611.aspx) és egyéb műveletek segítségével kezelheti.

- [Get](https://msdn.microsoft.com/library/azure/dn903633.aspx), [frissítés](https://msdn.microsoft.com/library/azure/dn986818.aspx), [Törlés](https://msdn.microsoft.com/library/azure/dn903613.aspx) és egyéb műveletek segítségével titkok kezelése

- [Előjellel](https://msdn.microsoft.com/library/azure/dn878096.aspx)használ titkosítási kulcsokat/[ellenőrzése](https://msdn.microsoft.com/library/azure/dn878082.aspx), [WrapKey](https://msdn.microsoft.com/library/azure/dn878066.aspx)/[UnwrapKey](https://msdn.microsoft.com/library/azure/dn878079.aspx) és [titkosítása](https://msdn.microsoft.com/library/azure/dn878060.aspx)/műveletek[visszafejtése](https://msdn.microsoft.com/library/azure/dn878097.aspx)

A következő SDK kulcs tárolóra való munkához érhetők el:

|[![.NET](./media/key-vault-developers-guide/msft.netlogo_purple.png)](https://msdn.microsoft.com/library/mt765854.aspx)|[![NODE.js](./media/key-vault-developers-guide/nodejs.png)](http://azure.github.io/azure-sdk-for-node/azure-arm-keyvault/latest)
|:--:|:--:|
|[.NET SDK dokumentációban](https://msdn.microsoft.com/library/mt765854.aspx)|[NODE.js SDK dokumentációban](http://azure.github.io/azure-sdk-for-node/azure-arm-keyvault/latest)|
|[.NET SDK csomag Nuget](http://www.nuget.org/packages/Microsoft.Azure.KeyVault)|[NODE.js SDK csomag](https://www.npmjs.com/package/azure-keyvault)|

További tájékoztatást a .NET SDK a 2.x verzió [Release notes](key-vault-dotnet2api-release-notes.md)fájlban talál.

## <a name="example-code"></a>Példa kód
Teljes példák tárolóra kulcs segítségével az alkalmazások lásd:

- .NET mintaalkalmazás *HelloKeyVault* és Azure webes szolgáltatás példa. [Borzas kulcs tárolóra kódpéldák](http://www.microsoft.com/download/details.aspx?id=45343)
- Oktatóanyag: segíteni Azure kulcs tárolóra használata az Azure egy webalkalmazás. [Egy webalkalmazás Azure kulcs tárolóra használata](key-vault-use-from-web-application.md)

## <a name="how-tos"></a>How-TOS

A következő cikkekben és forgatókönyvek útmutatást feladat adott Azure kulcs tárolóra való munkához:

- [Kulcs tárolóra bérlő azonosító módosítása előfizetés áthelyezése után](key-vault-subscription-move-fix.md) - Ha a bérlő A bérlő B lépünk Azure előfizetését, a meglévő kulcs elzárása esetben a rendszerbiztonsági tagok (felhasználók és alkalmazások) a bérlő B. javítás Ez az útmutató segítségével nem érhetők el.
- [Kulcs tárolóra elérése a tűzfal mögött](key-vault-access-behind-firewall.md) - hozzáférési kulcs elzárva a kulcs tárolóra ügyfélalkalmazásnak képesnek kell lennie több előfizetéskor, a különböző funkciók eléréséhez.

- [Létrehozása és Azure kulcs tárolóra Transfer HSM-Protected kulcsok](key-vault-hsm-protected-keys.md) - Ez segít a tervezése, készítése és majd át a saját HSM védett kulcsok használata Azure kulcs tárolóra.
- [Továbbítása biztonságos értékeket (például jelszavakat), a telepítés során](../resource-manager-keyvault-parameter.md) - Ha egy biztonságos értéket (például jelszó) átadása paraméterként a telepítés során szüksége, ezt az értéket tárolhatja, a titkos kulcs tárolóra Azure és a referencia értéke más erőforrás-kezelő sablonok.
- [Az SQL Server bővíthető kulcskezelés kulcs tárolóra használata](https://msdn.microsoft.com/library/dn198405.aspx) - kulcs tárolóra Azure az SQL Server csatlakozás lehetővé teszi, hogy SQL Server és az SQL-a-a-VM formátumról az Azure kulcs tárolóra szolgáltatás bővíthető Key Management (EKM) szolgáltatóként védelme érdekében saját titkosító kulcsait alkalmazások hivatkozás; Átlátszó adattitkosítás, a biztonsági másolat titkosítás és oszlop szintű titkosítás.
- [VMs kulcs tárolóra a tanúsítványok telepítéséről](https://blogs.technet.microsoft.com/kv/2015/07/14/deploy-certificates-to-vms-from-customer-managed-key-vault/) - egy felhő alkalmazás fut egy virtuális gép Azure tanúsítvány szükséges. Hogyan kapunk a tanúsítvány a VM be ma?
- [Végpont kulcs elforgatás és naplózás kulcs tárolóra a telepítő hogyan](key-vault-key-rotation-log-monitoring.md) - a otthoni hálózat hogyan kulcs elforgatás és Azure kulcs tárolóra a naplózás beállítása.

További tevékenység-specifikus útmutatást integrációjára és kábelaknák kulcs használata Azure példák [Ryan Jones Azure erőforrás-kezelő sablon a kulcs tárolóra](https://github.com/rjmax/ArmExamples/tree/master/keyvaultexamples).

## <a name="integrated-with-key-vault"></a>Kulcs tárolóra integrálva

Ezek a cikkek más forgatókönyvek és szolgáltatások nekünk, hogy vagy kulcs tárolóra integrálása készül.

- [Azure lemez titkosítás](../security/azure-security-disk-encryption.md) az ipar alapvető [a BitLocker](https://technet.microsoft.com/library/cc732774.aspx) szolgáltatás a Windows és a Linux az operációs rendszer és az adatok lemezek kötet titkosítás megadását a [DM-Crypt](https://en.wikipedia.org/wiki/Dm-crypt) szolgáltatás ötvözi. A megoldás Azure segítséget ellenőrzése és kezelése a lemez titkosító kulcsok és titkok a kulcs tárolóra előfizetését, biztosítva, hogy a virtuális gép lemezek az összes adat a Azure tárolóban nyugalmi titkosított kulcs nélkülinek integrálva van.


## <a name="supporting-libraries"></a>Támogató tárak

- Itt [Microsoft Azure kulcs tárolóra Core Library](http://www.nuget.org/packages/Microsoft.Azure.KeyVault.Core) `IKey` és `IKeyResolver` felületek azonosítóból kulcsok keresése és kulcsokkal műveleteket hajt végre.

- [Microsoft Azure kulcs tárolóra Extensions](http://www.nuget.org/packages/Microsoft.Azure.KeyVault.Extensions) Azure kulcs tárolóra vonatkozó bővített szolgáltatásokat nyújt.

## <a name="other-key-vault-resources"></a>Más kulcs tárolóra erőforrások
- [Kulcs tárolóra Blog](http://aka.ms/kvblog)
- [Kulcs tárolóra fórum](http://aka.ms/kvforum)
