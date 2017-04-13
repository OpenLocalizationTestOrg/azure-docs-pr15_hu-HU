
<properties
    pageTitle="Módosítsa az Azure Active Directory-bérlői az Azure RemoteApp |} Microsoft Azure"
    description="Megtudhatja, hogy miként módosíthatja az Azure Active Directory-bérlői Azure RemoteApp társított"
    services="remoteapp"
    documentationCenter=""
    authors="lizap"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="elizapo" />



# <a name="change-the-azure-active-directory-tenant-in-azure-remoteapp"></a>Az Azure Active Directory-bérlői az Azure RemoteApp módosítása

> [AZURE.IMPORTANT]
> Azure RemoteApp megszakad alatt. Olvassa el a további információt a [közlemény](https://go.microsoft.com/fwlink/?linkid=821148) .

Azure RemoteApp Azure Active Directory (Azure Active Directory) használja a hozzáférést a felhasználónak. A csak Azure AD-bérlő, amelyek segítségével használhatja az Azure RemoteApp az Azure-előfizetéséhez társított lesz. A kapcsolódó előfizetés megtekintése a portálon, a **Beállítások** lapon. Az **előfizetések** lapon tekintse meg a **könyvtár** oszlopban.

> [AZURE.NOTE] A módosítás folytatásra, először távolítsa el a meglévő Azure Active Directory-bérlő összes Azure RemoteApp gyűjtemények összes felhasználóra. Ehhez az Azure-portált, nyissa meg az **Azure RemoteApp** lapot, és nyissa meg a minden Azure RemoteApp webhelycsoport. Nyissa meg a **felhasználók** lapon, és távolítsa el a jelenlegi Azure Active Directory-bérlő tartozó felhasználók. Ismételje meg az összes meglévő Azure RemoteApp gyűjtemények. Ha így tesz, nélkül nem hozhat létre és javítás gyűjtemények.

Ha egy másik bérlői webhelyhez használni kívánt, a társítás-előfizetése módosításához használja ezeket a lépéseket:

1. A portálon távolítsa el, amelyhez adtunk access Azure RemoteApp gyűjtemények bármely Azure AD-felhasználók. (Lásd a fenti lépések ennek módjáról a.)


2. A szolgáltatás-rendszergazda állítja be egy Microsoft-fiókkal (korábbi nevén egy Live ID). (Nem tudja, hogy ha már a szolgáltatás-rendszergazda? Láthatja, hogy **a rendszergazdák-beállítások >**gombra kattintva.) Most az alábbiakban olvashatja, amely:
    1. Kattintson arra a felhasználóra, a jobb felső sarokban, és kattintson a **számlájának megtekintése**gombra.
    2. Kattintson az előfizetésére. Az új lapon, majd görgessen le, és **az előfizetés részletei szerkesztése** kattintson a jobb. (A középső név kezdőbetűjét jobb alsó sarokban, ha a rendezés segít, hogy megtalálja.)
    3. Írja be a Microsoft-fiókot a felhasználót, hogy a szolgáltatás-rendszergazdának kell lennie.

3. Most jelentkezzen ki a portálon, és jelentkezzen be a Microsoft-fiókkal az előző lépésben megadott.


4. Kattintson a **Új-alkalmazás > szolgáltatások-aktív > címtár -> címtár -> egyéni létrehozása**.
5. **A címtár**csoportban válassza a **meglévő könyvtár használata**. Kijelentkezés, a portálon letöltése, ha, ezért válassza az **aláírandó most már készen is állok**megyünk.
6. Bejelentkezés a hozzáadni kívánt könyvtár globális rendszergazdaként biztonsági őket a portálra. (Ön már nem globális rendszergazdának, fogja után a kerekítés, majd a Kijelentkezés, és jelentkezzen be.)
7. Meg kell adnia bejelentkezéskor szeretné-e a meglévő AD-bérlő használata az előfizetést. Kattintson a **Tovább**gombra, és kattintson a **Kijelentkezés gombra**.
5. Jelentkezzen be ismét, és térjen vissza- **Beállítások > előfizetések elemre**. Jelölje ki azt az előfizetést, és kattintson a **Címtár szerkesztése**gombra. Jelölje ki a használni kívánt Azure AD-bérlő.



Most az új Azure Active Directory használata bérlői az Azure előfizetés való hozzáférés szabályozása és konfigurálása a felhasználói hozzáférés az Azure RemoteApp is.
