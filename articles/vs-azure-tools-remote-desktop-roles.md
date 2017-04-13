<properties 
   pageTitle="Távoli asztali változatában Azure szerepkörök |} Microsoft Azure"
   description="Távoli asztali változatában Azure szerepkörök"
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags 
   ms.service="multiple"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="using-remote-desktop-with-azure-roles"></a>Távoli asztali változatában Azure szerepkörök

Az Azure SDK és a távoli asztali szolgáltatásokat használatával Azure szerepkörök és, amelyekhez Azure virtuális gépeken futó érheti el. A Visual Studióban távoli asztali szolgáltatásokat is beállíthatja az Azure projektből. Ahhoz, hogy a távoli asztali szolgáltatásokat, kell, amely tartalmazza az egy vagy több szerepkörök munka projekt létrehozása és közzététele Azure.

>[AZURE.IMPORTANT] Azure szerepet hibaelhárítási vagy csak a fejlesztői kell érheti el. Virtuális gépeken célja egy szerepkör az Azure, nem futó alkalmazás más ügyfélalkalmazásokban futtatásához. Ha használandó Azure virtuális gép célra használható tárolni, olvassa el a elérése Azure virtuális gépeken futó kiszolgáló Intézőből.

## <a name="to-enable-and-use-remote-desktop-for-an-azure-role"></a>Engedélyezheti és Azure szerepet távoli asztali használata

1. A megoldás Intézőben nyissa meg a helyi menü beállítása a projekthez, és válassza a **Közzététel**gombra.

    A **Közzététel Azure** varázsló jelenik meg.

    ![Parancs Felhőszolgáltatásba projekt közzététele](./media/vs-azure-tools-remote-desktop-roles/IC799161.png)

1. **Microsoft Azure-a Publish Settings** lapon, a varázsló alján jelölje be a **Távoli asztal engedélyezése** minden szerepkörök jelölőnégyzetet. 

    A **Távoli asztali beállítása** párbeszédpanel jelenik meg.

1. A **Távoli asztali beállítása** párbeszédpanel alján válassza a **További beállítások** gombra. 
 
    Megjelenik egy legördülő listát, amellyel létrehozása, vagy válasszon egy tanúsítványt, úgy, hogy a távoli asztali keresztül csatlakozáskor titkosíthatja hitelesítő adatok.

1. A legördülő listában válassza a ** &lt;létrehozása >**, vagy válassza ki a listából egy meglévőt. 

    Ha úgy dönt, hogy a meglévő tanúsítvány, hagyja ki az alábbi lépéseket.

    >[AZURE.NOTE] A tanúsítványok van szüksége a távoli asztali kapcsolat különböznek az egyéb Azure műveletek használt tanúsítványokat. A távoli hozzáférés tanúsítványt egy titkos kulcs kell rendelkeznie.

    Megjelenik a **Tanúsítvány létrehozása** párbeszédpanel.

    1. Adja meg az új tanúsítvány egy rövid nevet, és válassza az **OK** gombra. Az új tanúsítvány jelenik meg a legördülő listából.

    1. **Távoli asztali beállítása** párbeszédpanelen adja meg a felhasználónév és jelszó.
    
        Meglévő fiók nem használható. A felhasználó nevét, az új fiók rendszergazdai nem adja meg.

        >[AZURE.NOTE] Ha a jelszó nem felel meg a összetettsége követelményeknek, egy piros ikonja mellett a jelszó szövegmezőbe. A jelszó nagybetűvel, a kisbetűket, és a számok vagy szimbólumok kell tartalmaznia.

    1. Válassza a fiók lejár, és milyen távoli asztali kapcsolat, azzal blokkolhatja után egy dátumot.

    1. Miután útjával a szükséges információkat, válassza az **OK** gombra.
    
        Számos beállítást, amely engedélyezése a távoli az Access Services megjelennek a .cscfg és .csdef fájlokat.

1. A **Microsoft Azure Publish Settings** varázslóban válassza az **OK** gombra, ha készen áll a felhőalapú szolgáltatás közzététele.

    Ha nem szeretne közzétenni kattintson a **Mégse** gombra. A beállítások kerülnek, és, tegye közzé a felhőalapú szolgáltatás.

## <a name="connect-to-an-azure-role-by-using-remote-desktop"></a>Azure szerepet csatlakozás távoli asztali használatával

A felhőalapú szolgáltatás Azure a közzététel után bejelentkezik a virtuális gépeken futó Azure tároló kiszolgáló Explorer segítségével. 

1. A kiszolgáló Intézőben bontsa ki az **Azure** csomópontot, és bontsa ki a egy felhőalapú szolgáltatásba, és a példányok listáját megjelenítendő szerepkörök egyikét kapja a csomópontot.

1. Nyissa meg a helyi menü egy példány csomópontra, és válassza a **Csatlakozás a távoli asztal**.

    ![Csatlakozás távoli asztali keresztül](./media/vs-azure-tools-remote-desktop-roles/IC799162.png)

1. Írja be a felhasználónevét és a korábban létrehozott jelszót. Most már bejelentkezett a távoli munkamenetet.


