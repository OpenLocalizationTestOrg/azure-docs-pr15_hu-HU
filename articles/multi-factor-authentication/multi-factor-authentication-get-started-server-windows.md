<properties 
    pageTitle="Windows-hitelesítés és Azure többtényezős hitelesítés kiszolgáló"
    description="Ez a az Azure többtényezős hitelesítés oldalt, amely segítséget nyújt a Windows-hitelesítés és Azure többtényezős hitelesítést Server telepítése."
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
    ms.date="08/04/2016"
    ms.author="kgremban"/>

# <a name="windows-authentication-and-azure-multi-factor-authentication-server"></a>Windows-hitelesítés és Azure többtényezős hitelesítés kiszolgáló

A Windows-hitelesítés szakasz lehetővé teszi, hogy a rendszergazda engedélyezése és beállítása a Windows-hitelesítés az egy vagy több alkalmazást.  Az alábbiakban dolog, amit érdemes észben tartania, előtt állítsa be a Windows-hitelesítés listáját.

-  Indítsa újra a rendszert van szükség, mielőtt az Azure többtényezős hitelesítés Terminálszolgáltatások érvényben lesz.
-  Ha "Azure többtényezős hitelesítés szükséges felhasználói egyezés" jelölőnégyzet be van jelölve, és Ön nem szerepelnek a felhasználók listáját, nem tud bejelentkezni a a számítógép újraindítása után.
-  Megbízható IP-címei animációtól meg, hogy az alkalmazás az ügyfél IP-hitelesítéssel az tartalmaznak. Jelenleg csak Terminálszolgáltatások használata támogatott.  







>[AZURE.NOTE]Ez a funkció nem támogatott a Windows Server 2012 R2 biztonságos Terminálszolgáltatások.




## <a name="to-secure-an-application-with-windows-authentication-use-the-following-procedure"></a>Windows-hitelesítéssel az alkalmazások biztonságos, használja az alábbi eljárást.

1. Kattintson az Azure többtényezős hitelesítést kiszolgáló a Windows-hitelesítés ikonra.
![Windows-hitelesítés](./media/multi-factor-authentication-get-started-server-windows/windowsauth.png)
2. Jelölje be a engedélyezése a Windows-hitelesítés jelölőnégyzetet. Alapértelmezés szerint a jelölőnégyzet nincs bejelölve.
3. Az alkalmazások fülre a rendszergazda beállítása egy vagy több alkalmazásokat a Windows-hitelesítést.
4. Jelöljön ki egy kiszolgálói vagy alkalmazás – adja meg, hogy engedélyezve van-e a kiszolgáló alkalmazást. Kattintson az OK gombra.
5. Kattintson a Hozzáadás gombra. gombra.
6. A megbízható IP-címei lapon adhatja meg, bizonyos IP-címei származó többtényezős hitelesítést a Windows Azure-munkamenetek kihagyása. Ha például ha alkalmazottak az alkalmazás használatához az office és az otthoni dönthet nem szeretné, hogy az irodában Azure többtényezős hitelesítéshez a telefonjuk. Ezt meg szeretné adja meg az office-alhálózat bejegyzést a megbízható IP-címei.
7. Kattintson a Hozzáadás gombra. gombra.
8. Jelölje be a egyetlen IP, ha szeretné, hogy ugorja át a egyetlen IP-címet.
9. Jelölje be a IP-tartomány, ha szeretné, hogy az egész IP-tartomány ugorja át. Példa 10.63.193.1-10.63.193.100.
10. Jelölje ki a alhálózat, ha szeretne, adjon meg egy tartományt az IP-címei alhálózat jelölés használatával. Írja be a alhálózat kezdő IP, és válassza ki a megfelelő maszk a legördülő listából.
11. Kattintson az OK gombra.
