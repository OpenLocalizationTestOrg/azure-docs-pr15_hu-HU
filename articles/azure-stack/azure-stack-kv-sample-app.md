<properties
    pageTitle="Teszik lehetővé revtrieve Azure Papírhalom kulcs tárolóra titkos kulcsok alkalmazás |} Microsoft Azure"
    description="Egy minta alkalmazás használata Azure Papírhalom kulcs tárolóból elemre"
    services="azure-stack"
    documentationCenter=""
    authors="rlfmendes"
    manager="natmack"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/26/2016"
    ms.author="ricardom"/>

# <a name="run-the-sample-application-for-key-vault"></a>A kulcs tárolóból elemre a minta alkalmazásnak a futtatására 

Ebből az útmutatóból kell megadnia a minta alkalmazások titkos kulcsok és a jelszavakat beolvasni a kulcs tárolóból elemre.

## <a name="download-the-samples-and-prepare"></a>Töltse le a minták és előkészítése

Töltse le az [Azure kulcs tárolóra ügyfél minták lap](https://www.microsoft.com/en-us/download/details.aspx?id=45343)ügyfél minták Azure kulcs tárolóból elemre.

Bontsa ki a helyi számítógépre a .zip fájl tartalmát.

Olvassa el a **README.md** fájlt (Ez a szövegfájlból), és kövesse az utasításokat.

## <a name="run-sample-1--hellokeyvault"></a>Minta #1 – HelloKeyVault futtatása
HelloKeyVault egy konzol alkalmazás, amely végigvezeti az esetek kulcs tárolóra által támogatott:

  1. A kulcs (HSM vagy szoftver) létrehozása és importálása
  2. A titkos kulccsal tartalom titkosítása
  3. A tartalom kulcs kulcs tárolóra kulccsal tördelése
  4. A tartalom kulcs tördelésének
  5. A titkos visszafejtése
  6. A titkos beállítása

A New fusson változtatás nélkül, azzal a különbséggel, hogy a megfelelő beállítások App.Config frissülnek szerint az alábbi lépéseket:

1. Az alkalmazás konfigurációs beállítások frissítése az HelloKeyVault\App.config tárolóra URL-CÍMÉT, a fő azonosítója és a titkos. Az adatokat tetszés szerint **scripts\GetAppConfigSettings.ps1**hozhatók létre.
2. Frissítse a kötelező változók GetAppConfigSettings.ps1 az értékeket.
3. Indítsa el a Windows PowerShell-ablakot.
4. Futtassa a GetAppConfigSettings.ps1 a PowerShell ablakon belül.
5. Másolja a parancsfájl eredményét a HelloKeyVault\App.config fájlt.


## <a name="next-steps"></a>Következő lépések

[A virtuális kulcs tárolóra jelszóval terjesztése](azure-stack-kv-deploy-vm-with-secret.md)

[A virtuális kulcs tárolóra tanúsítvánnyal terjesztése](azure-stack-kv-push-secret-into-vm.md)