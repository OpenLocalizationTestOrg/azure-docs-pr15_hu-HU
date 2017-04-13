<properties
    pageTitle="Azure Papírhalom telepítsen újra |} Microsoft Azure"
    description="Azure Papírhalom újratelepítése."
    services="azure-stack"
    documentationCenter=""
    authors="ErikjeMS"
    manager="byronr"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/29/2016"
    ms.author="erikje"/>

# <a name="redeploy-azure-stack"></a>Azure Papírhalom újratelepítése

Telepítsen újra Azure Papírhalom, el kell indítania keresztül az alapoktól az alábbiaknak.

## <a name="steps-to-redeploy-azure-stack"></a>Azure Papírhalom telepítsen újra lépései

1. Indítsa újra a host (telepítve van a rendszer nélküli) eredeti operációs rendszerbe. Ez nem az alapértelmezett indítási menüben, jelölje ki az újraindításkor kell használnia KVM vagy a helyi console (a telepítés során a "indítási a virtuális merevlemez" OS "AzureStack TP2" nevű; Ez segít azonosítani tudja az operációs rendszer, amely).

    Nem kell távolítsa el a meglévő indítási bejegyzést (az új támogatási parancsfájl "PrepareBootFromVHD.ps1" gondoskodik arra, hogy ki.)

2. Ha nem rendelkezik KVM, vagy válassza az indítási operációs rendszer újraindítása előtt szeretné:
    
    1. Keresse meg a parancsfájlt.\BootMenuNoKVM.ps1. Ez a fájl mellett a Szerkesztés megadott egyéb támogatási parancsfájlok érhető el.
    
    2. Futtassa a parancsfájlt magasabb szintű jogosultságokkal rendelkező. Jelölje ki az eredeti Host-OS nevét. Ez lesz a indítsa el a host az eredeti állomás OS KVM hozzáférést igénylő nélkül.
    
    3. Amikor befejeződött a parancsfájl a rendszer kéri, az újraindítás megerősítéséhez.

    - Ha más felhasználók-e jelentkezve, ez a parancs sikertelen lesz.

    - Kérjük, futtassa a következő parancsot: a számítógép újraindítása-kikényszerítése 
 
3. Törölje a CloudBuilder.vhdx fájlt, amelyet az előző telepítés részeként használt.

    Nem kell a meglévő Tárolókészlethez törlése az előző TP2 telepítési. A telepítési parancsfájlt észleli és törli a köteggel a meglévő, majd új hoz létre.

5. Telepítsen újra másoljanak CloudBuilder.vhdx, indítási azt, például egy új példányát.

## <a name="next-steps"></a>Következő lépések

[Azure Papírhalom csatlakoztatása](azure-stack-connect-azure-stack.md)
