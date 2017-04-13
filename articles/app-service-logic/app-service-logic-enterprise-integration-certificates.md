
<properties
    pageTitle="Nagyvállalati integrációs Pack tanúsítványokkal |} Microsoft Azure"
    description="A nagyvállalati integrációs csomag és összefüggés-alkalmazások használata a tanúsítványok"
    services="logic-apps"
    documentationCenter=".net,nodejs,java"
    authors="msftman"
    manager="erikre"
    editor="cgronlun"/>

<tags
    ms.service="logic-apps"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/06/2016"
    ms.author="deonhe"/>

# <a name="learn-about-certificates-and-enterprise-integration-pack"></a>Tudnivalók a tanúsítványok és vállalati integrációs csomag

## <a name="overview"></a>– Áttekintés
Nagyvállalati integrációs biztonságos kommunikáció B2B tanúsítványok használja. A nagyvállalati integrációs alkalmazásokban kétféle tanúsítvány használható:

- Nyilvános tanúsítványokról meg kell vásárolni hitelesítésszolgáltatótól (CA).
- A magánjellegű tanúsítványokról állíthatnak saját magának. Ezek a tanúsítványok önaláírt tanúsítványok is nevezik.


## <a name="what-are-certificates"></a>Mik azok a tanúsítványok?
Tanúsítványok digitális dokumentumok, amelyek kívül a résztvevők elektronikus kommunikáció azonosítani és, amely biztonságos kommunikáció elektronikus.

## <a name="why-use-certificates"></a>Miért érdemes használni a tanúsítványok?
Előfordul, hogy B2B kommunikáció kell tartani a bizalmas. Nagyvállalati integrációs biztonságos ezeket kétféleképpen kommunikáció tanúsítványok használja:

- Az üzenetek tartalma titkosítása
- Üzenetek digitális aláírásához szerint  

## <a name="how-do-you-upload-certificates"></a>Hogyan feltöltése tanúsítványok?

### <a name="public-certificates"></a>A nyilvános tanúsítványok
*Nyilvános tanúsítványt* használni a B2B funkciókhoz összefüggés-alkalmazásokat, először töltse fel a tanúsítvány integrációs fiókjába. *Önaláírt tanúsítványt*használatához azonban, meg kell először töltse fel [Azure kulcs tárolóból elemre],(../key-vault/key-vault-get-started.md "megismerheti a kulcs tárolóból elemre").

Miután feltöltött egy tanúsítványt, akkor érhető el segít a B2B üzenetek biztonságossá tétele, tulajdonságaik megadásakor az [rendelkezést](./app-service-logic-enterprise-integration-agreements.md) hoz létre.  

Az alábbiakban a nyilvános tanúsítványok feltölthető integrációs fiókjába, az Azure-portálra bejelentkezés után a részletes lépéseket:

1. Válassza a **Tallózás gombra**.  
    ![Válassza a Tallózás](./media/app-service-logic-enterprise-integration-overview/overview-1.png)  

2. Írja be a **integráció** a szűrés a Keresés mezőbe, és válassza a **Fiókok integráció** az eredménylistából.     
    ![Válassza a fiókok integrációja](./media/app-service-logic-enterprise-integration-overview/overview-2.png)

3. Jelölje ki az integration, amelyre meg a hozzáadni kívánt fióktípust a tanúsítvány.  
    ![Jelölje ki a integrációs fiókot, amelyhez szeretne a tanúsítvány](./media/app-service-logic-enterprise-integration-overview/overview-3.png)  

4.  Kattintson a **tanúsítványok** csempére.  
    ![Jelölje ki a tanúsítványok mozaikot](./media/app-service-logic-enterprise-integration-certificates/certificate-1.png)

5. A megnyíló a **tanúsítványok** lap válassza a **Hozzáadás** gombot.
    ![Válassza a Hozzáadás gombra.](./media/app-service-logic-enterprise-integration-certificates/certificate-2.png)

6. Írja be a tanúsítvány **nevét** , és válassza ki a tanúsítvány típusa. (Ebben a példában azt használja a nyilvános tanúsítvány típusa.) Válassza a mappa ikonja a **tanúsítvány** szövegdoboz jobb oldalán.

7. Amikor megnyílik a fájlválasztó, keresse meg, és jelölje ki a feltöltendő integrációs fiókjára biztonságitanúsítvány-fájl.

8. Jelölje ki a tanúsítványt, és válassza **az OK gombra** a fájl-választó. Ellenőrzi, és a feltölti a tanúsítvány integrációs fiókját.

8. Jelentkezzen be a **tanúsítvány hozzáadása** lap végül válassza az **OK** gombra.  
    ![Az OK gombra](./media/app-service-logic-enterprise-integration-certificates/certificate-3.png)  

9. Az körülbelül egy percig, jelenít meg értesítést, amely jelzi, hogy a tanúsítvány feltöltés befejeződött.

10. Kattintson a **tanúsítványok** csempére. Meg kell jelennie a újonnan hozzáadott tanúsítványt.  
    ![Lásd: az újat.](./media/app-service-logic-enterprise-integration-certificates/certificate-4.png)  

### <a name="private-certificates"></a>Saját tanúsítványok
Is feltölthet a saját tanúsítványok integrációs fiókjába a következő lépéseket:  

1. [Töltse fel a titkos kulcs kulcs tárolóból elemre.] (../key-vault/key-vault-get-started.md "Tudnivalók a fő tárolóból elemre").  

    > [AZURE.TIP] Engedélyeznie kell a logika alkalmazások funkció Azure alkalmazás szolgáltatás műveleteket a kulcs tárolóból elemre. Az access az alábbi PowerShell-parancs használatával biztosíthat a logika alkalmazások szolgáltatás egyszerű:`Set-AzureRmKeyVaultAccessPolicy -VaultName 'TestcertKeyVault' -ServicePrincipalName '7cd684f4-8a78-49b0-91ec-6a35d38739ba' -PermissionsToKeys decrypt, sign, get, list`  

2. Személyes tanúsítvány létrehozása.  

3. Töltse fel a személyes tanúsítvány integrációs fiókját.

Miután az előző lépéseket, a személyes tanúsítvány létrehozása rendelkezést is használhatja.

Az alábbiakban a saját tanúsítványok feltölthető integrációs fiókjába, az Azure-portálra bejelentkezés után a részletes lépéseket:  

1. Válassza a **Tallózás gombra**.  
    ![Töltse fel a saját tanúsítványok integrációs fiókba](./media/app-service-logic-enterprise-integration-overview/overview-1.png)    

2. Írja be a **integráció** a szűrés a Keresés mezőbe, és válassza a **Fiókok integráció** az eredménylistából.     
    ![Válassza a fiókok integrációja](./media/app-service-logic-enterprise-integration-overview/overview-2.png)  

3. Jelölje ki az integration, amelyre meg a hozzáadni kívánt fióktípust a tanúsítvány.  
    ![Jelölje ki a integrációs fiókot, amelyhez szeretne a tanúsítvány](./media/app-service-logic-enterprise-integration-overview/overview-3.png)  

4. Kattintson a **tanúsítványok** csempére.  
    ![Jelölje ki a tanúsítványok mozaikot](./media/app-service-logic-enterprise-integration-certificates/certificate-1.png)  

5. A megnyíló a **tanúsítványok** lap válassza a **Hozzáadás** gombot.
    ![Válassza a Hozzáadás gombra.](./media/app-service-logic-enterprise-integration-certificates/certificate-2.png)

6. Írja be a tanúsítvány **nevét** , és válassza ki a tanúsítvány típusa. (Ebben a példában azt használja a nyilvános tanúsítvány típusa.) Válassza a mappa ikonja a **tanúsítvány** szövegdoboz jobb oldalán.

7. Amikor megnyílik a fájlválasztó, keresse meg, és jelölje ki a feltöltendő integrációs fiókjára biztonságitanúsítvány-fájl.

8. A tanúsítvány kiválasztása után kattintson **az OK gombra** a fájl-választó található. Ez a művelet ellenőrzi a tanúsítvány és feltöltések megjelenítése az integration fiókjára.

9. Jelentkezzen be a **tanúsítvány hozzáadása** lap végül válassza az **OK** gombra.  
    ![Tanúsítvány felvétele](./media/app-service-logic-enterprise-integration-certificates/privatecertificate-1.png)  

10. Az körülbelül egy percig, jelenít meg értesítést, amely jelzi, hogy a tanúsítvány feltöltés befejeződött.

11. Kattintson a **tanúsítványok** csempére. Meg kell jelennie a újonnan hozzáadott tanúsítványt.
    ![Lásd: az újat.](./media/app-service-logic-enterprise-integration-certificates/privatecertificate-2.png)  

Miután feltöltött egy tanúsítványt, akkor érhető el segít a B2B üzenetek biztonságossá tétele, [rendelkezést](./app-service-logic-enterprise-integration-agreements.md)tulajdonságaik definiálásakor.  

## <a name="next-steps"></a>Következő lépések
- [B2B szolgáltatásokat használó logika alkalmazás létrehozása](./app-service-logic-enterprise-integration-b2b.md)  
- [Hozzon létre egy B2B szerződés](./app-service-logic-enterprise-integration-agreements.md)  
- [További tudnivalók a kulcs tárolóból elemre.] (../key-vault/key-vault-get-started.md "Tudnivalók a fő tárolóból elemre")  
