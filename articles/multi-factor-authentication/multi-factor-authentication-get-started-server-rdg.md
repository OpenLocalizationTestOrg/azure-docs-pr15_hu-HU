<properties 
    pageTitle="Távoli asztali átjáró és Azure többtényezős hitelesítést kiszolgáló RADIUS használata"
    description="Ez a az Azure többtényezős hitelesítés oldalt, amely segítséget nyújt az asztali (asztali) átjáró és Azure többtényezős hitelesítést kiszolgáló RADIUS használata üzembe helyezése."
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/15/2016"
    ms.author="kgremban"/>

# <a name="remote-desktop-gateway-and-azure-multi-factor-authentication-server-using-radius"></a>Távoli asztali átjáró és Azure többtényezős hitelesítést kiszolgáló RADIUS használata

Sok esetben távoli asztali átjáró segítségével a helyi házirend felhasználók hitelesítést végezni. A dokumentum leírja, hogy miként RADIUS kérése meg a távoli asztali átjáró (keresztül a helyi házirend) a többtényezős hitelesítést kiszolgálóhoz irányítja.

A többtényezős hitelesítést kiszolgáló külön kiszolgálón, amelyek ezután proxy RADIUS kérésére vissza a házirend a távoli asztali átjáró kiszolgálón kell telepíteni. Miután házirend ellenőrzi a felhasználónevet és jelszót, a többtényezős hitelesítést kiszolgáló, amely a második tényező előtt eredményt térjen vissza az átjáró hitelesítési hajt végre választ ad vissza.





## <a name="configure-the-rd-gateway"></a>A távoli asztali átjáró beállítása

A távoli asztali átjáró RADIUS-hitelesítés küldeni az Azure többtényezős hitelesítést kiszolgáló kell beállítania. Távoli asztali átjáró telepítése után és beállítva munkát, válassza a be a távoli asztali átjáró tulajdonságait. Nyissa meg a távoli asztali házirendjeihez lapot, és egy központi házirend-kiszolgálót futtató helyi házirend-kiszolgálót futtató helyett használandó módosításáról. Adja hozzá egy vagy több Azure többtényezős hitelesítést kiszolgálók RADIUS-kiszolgálók, és adjon meg egy megosztott titkos minden kiszolgáló.





## <a name="configure-nps"></a>Házirend beállítása

A távoli asztali átjáró házirend használja a RADIUS kérelmet küldhet az Azure többtényezős hitelesítés. Időtúllépés módosítani kell, hogy a távoli asztali átjáró az időtúllépés miatt többtényezős hitelesítés nem fejeződött. A következő eljárással házirend beállítása.

1. A házirend bontsa ki a bal oldali oszlopban a RADIUS ügyfelek és kiszolgáló menüt, majd kattintson távoli RADIUS kiszolgálói csoportok. Az ÁTJÁRÓ TS SERVER csoport tulajdonságait ismertetőt találhat. Szerkesztheti a RADIUS kiszolgálói jelenik meg, és nyissa meg a terheléselosztás fülre. A "választ, mielőtt tekinthető kérelem nélkül másodpercek számát kihagyott" módosítása és a "kérelmek, ha a kiszolgáló nyilvánítása között eltelt másodpercek számát" 30-60 másodpercre. Kattintson a a hitelesítési/fiók fülre, és biztosítani, hogy a megadott RADIUS-portok egyezik-e a többtényezős hitelesítést kiszolgáló figyelését portokat.
2. Házirend is kell beállítania, hogy fogadhat RADIUS hitelesítési vissza a kiszolgálóról Azure többtényezős hitelesítést. Kattintson a RADIUS-ügyfelek, a bal oldali menüben. Vegye fel az Azure többtényezős hitelesítést kiszolgáló RADIUS-ügyfél. Válasszon egy rövid nevet, és adjon meg egy megosztott titkos.
3. Bontsa ki a bal oldali navigációs sávon a házirendek szakaszt, és kattintson a csatlakozási kérelem házirendek. A kapcsolat házirend TS ÁTJÁRÓ engedélyezési házirendet, amely a távoli asztali átjáró konfigurálásakor nevű tartalmaznia kell. Ezzel a házirenddel az többtényezős hitelesítést kiszolgáló RADIUS kérések továbbítja.
4. Másolja a vágólapra a házirendet, amely hozzon létre egy újat. Adja meg az új házirendet, amely megfelel a felhasználóbarát név ügyfél rövid nevet feltétel megadása a lépés: 2 feletti az Azure többtényezős hitelesítést kiszolgáló RADIUS ügyfél. Módosítani a hitelesítési helyi számítógépre. Ezzel a házirenddel biztosítja, hogy RADIUS kérelem érkezik, a Azure többtényezős hitelesítést kiszolgálóról, amikor a hitelesítés helyileg helyett RADIUS kérelem küld a az Azure többtényezős hitelesítést kiszolgáló, amely hurkot állapotban. A leállításig feltétel elkerülése érdekében az új házirend kell rendelni, az eredeti házirendet, amely a többtényezős hitelesítést kiszolgálóra továbbítja FELETT.

## <a name="configure-azure-multi-factor-authentication"></a>Azure többtényezős hitelesítés beállítása


--------------------------------------------------------------------------------



Az Azure többtényezős hitelesítést kiszolgáló RADIUS-proxy RD átjáró és házirend között van beállítva.  A távoli asztali átjáró kiszolgálóról különböző tartományhoz kiszolgálón kell telepíteni. Az alábbi eljárás használatával állítsa be az Azure többtényezős hitelesítést kiszolgálót.

1. Nyissa meg az Azure többtényezős hitelesítést kiszolgáló, és kattintson a RADIUS-hitelesítés ikonra. Jelölje be a hitelesítés RADIUS engedélyezése jelölőnégyzetet.
2. Az ügyfelek lapon győződjön meg róla, a portokat felel meg, mi van beállítva a házirend, majd kattintson a Hozzáadás... gombra. Adja meg a távoli asztali átjáró kiszolgáló IP-címét, alkalmazás nevét (nem kötelező), és egy megosztott titkos. A megosztott titkos ugyanazok a Azure többtényezős hitelesítést Server és a távoli asztali átjárón kell.
3. Kattintson a cél fülre, és válassza a RADIUS kiszolgálókat választógomb.
4. Kattintson a Hozzáadás... gombra. Adja meg az IP-címet, megosztott titkos és a házirend-kiszolgáló portjai. Kivéve, ha egy központi házirend használata esetén a RADIUS-ügyfél és a RADIUS-céltartományban azonos lesz. A megosztott titkos egyeznie kell a telepítő a házirend-kiszolgáló RADIUS ügyfél szakaszában.

![RADIUS-hitelesítés](./media/multi-factor-authentication-get-started-server-rdg/radius.png)
