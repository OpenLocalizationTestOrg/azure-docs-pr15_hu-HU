<properties
    pageTitle="Azure AD Connect szinkronizálása: szinkronizálás szolgáltatáskezelő felhasználói felület |} Microsoft Azure"
    description="A műveletek fülre a szinkronizálás szolgáltatáskezelő Azure AD Connect ismertetése."
    services="active-directory"
    documentationCenter=""
    authors="andkjell"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/07/2016"
    ms.author="billmath"/>


# <a name="azure-ad-connect-sync-synchronization-service-manager"></a>Azure AD Connect szinkronizálása: szinkronizálás Service Manager

[Műveletek](active-directory-aadconnectsync-service-manager-ui-operations.md) | [Összekötők](active-directory-aadconnectsync-service-manager-ui-connectors.md) | [Metaverse Designer alkalmazásban](active-directory-aadconnectsync-service-manager-ui-mvdesigner.md) | [Metaverz keresés](active-directory-aadconnectsync-service-manager-ui-mvsearch.md)
--- | --- | --- | ---

![Szinkronizálási Service Manager](./media/active-directory-aadconnectsync-service-manager-ui/operations.png)

A Műveletek lap mutatja az eredményeket, a legutóbbi műveletek. Ezen a lapon, és kapcsolatos problémák megoldása billentyűt.

## <a name="understand-the-information-visible-in-the-operations-tab"></a>A Műveletek lap látható információk megértéséhez
Felső részén minden futó krónikus sorrendben jeleníti meg. Alapértelmezés szerint a műveletek napló továbbra is az utóbbi hét napban adatait, de ez a beállítás használatával módosítható az [ütemezőt](active-directory-aadconnectsync-feature-scheduler.md). Bármelyik run, amelyek nem jelennek állapota success keresni szeretne. A rendezés a fejlécek gombra kattintva módosíthatja.

Az **állapot** oszlopban a legfontosabb adatok, és megjeleníti a legszigorúbb a Futtatás a probléma. A leggyakoribb állapotok vizsgálja meg, hogy az elsőbbségi sorrendben rövid összefoglalása az alábbiakban (ahol * több lehetséges hibaüzenetek jelezheti).

Állapot | Megjegyzés
--- | ---
leállítva-* | A Futtatás nem hajtható végre. Ha például a távoli rendszer kapcsolva, és nem lehet Önnel kapcsolatba lépni.
leállítva hiba-korlát | 5000-nél több hibák történnek. A Futtatás automatikusan leállt a nagyszámú hibák miatt.
Befejezett -\*-hibák | A Futtatás befejeződött, de meg kell vizsgálni hibák (kevesebb mint 5 000).
Befejezett -\*-figyelmeztetések | A Futtatás befejeződött, de néhány adatot nem a várt állapotban van. Ha hibákat, majd ezt az üzenetet az általában csak a jelenség. Hibák van címezve, amíg nem vizsgálja meg figyelmeztetések.
a siker | Problémák.

Amikor kijelöl egy sort, alsó frissíti arra, hogy futtassa a részletek megjelenítéséhez. A képernyő bal alsó az, hogy lehet, hogy **lépés #**mondanak listáját. Ez a lista csak akkor jelenik meg, ha több tartománya van, a erdőben, amelyben minden egyes tartományhoz lépés képviseli. A tartománynév **partíciót**címszó alatt találhatók. A **Szinkronizálási statisztika**talál további információt a módosításokat, amelyeket feldolgozott számát. A hivatkozásokra kattintva a módosított objektumokat listájának kattinthat. Ha a hibát tartalmazó objektumok, ezek a hibák jelennek meg a **Szinkronizálási hibákat**.

## <a name="troubleshoot-errors-in-operations-tab"></a>Hibaelhárítás a Műveletek lap
![Szinkronizálási Service Manager](./media/active-directory-aadconnectsync-service-manager-ui/errorsync.png)  
Ha hibákat, a hiba, és a hiba, magát a mind az objektum olyan hivatkozásokat tartalmaz további tájékoztatást.

A hiba karakterlánc (**szinkronizálás-szabály-hiba-függvény – amelyen** a kép) kattintva indítsa el. Először áttekintheti az objektum mutatnak. Tényleges hibaüzenet jelenik meg, kattintson a következő gombra: **Papírhalom nyomkövetési**. Ez a nyomkövetési információk hibakeresési szintű a hiba.

**Tipp:** Most kattintson a jobb gombbal a **hívás Papírhalom információk** mezőben, választhat a **Jelölje ki az összes**és a **másolatot**. Ezután másolja a vágólapra az objektumhalomban, és tekintse meg a hiba a kedvenc szerkesztő, például a Jegyzettömbben.

- Ha a hiba **SyncRulesEngine**származik, majd a hívás Papírhalom adatokat először foglalja magában: összes attribútum listáját az objektum Görgessen lefelé, amíg meg nem jelenik a címsort **InnerException = >**.  
![Szinkronizálási Service Manager](./media/active-directory-aadconnectsync-service-manager-ui/errorinnerexception.png)  
A sor után jeleníti meg, hogy a hibát. A fenti képen a program a létrehozott egyéni szinkronizálási szabály Fabrikam.

Ha a hiba magát nem nyújt elegendő információt, majd pedig tekintse meg az adatokat, magát. A hivatkozásra a objektumazonosító, és [hajtsa végre az objektum tartalmával együtt – a rendszer](active-directory-aadconnectsync-service-manager-ui-connectors.md#follow-an-object-and-its-data-through-the-system).

## <a name="next-steps"></a>Következő lépések
További tudnivalók az [Azure AD Connect szinkronizálása](active-directory-aadconnectsync-whatis.md) konfigurációt.

További tudnivalók [a helyszíni identitás és Azure Active Directory integrálása](active-directory-aadconnect.md).
