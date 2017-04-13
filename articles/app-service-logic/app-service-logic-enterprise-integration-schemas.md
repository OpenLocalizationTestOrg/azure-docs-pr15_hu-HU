<properties
    pageTitle="A sémák és a nagyvállalati integrációs csomag áttekintése |} Microsoft Azure"
    description="A sémák használata a nagyvállalati integrációs csomag és logika alkalmazással"
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
    ms.date="07/29/2016"
    ms.author="deonhe"/>

# <a name="learn-about-schemas-and-the-enterprise-integration-pack"></a>A sémák és a nagyvállalati integrációs csomag ismertetése  

## <a name="why-use-a-schema"></a>Miért érdemes használni a séma?
A sémák segítségével győződjön meg arról, hogy kap XML-dokumentumok érvényes, előre megadott formátum várható adataival. A sémák segítségével érvényesítése B2B helyzetben továbbított üzeneteket.

## <a name="add-a-schema"></a>Adja hozzá a séma
Az Azure portálról:  

1. Válassza a **További szolgáltatások**.  
![Képernyőkép az Azure-portálon szolgáltatásokkal"több" kiemelve](./media/app-service-logic-enterprise-integration-overview/overview-11.png)    

2. A szűrő keresés párbeszédpanelen adja meg **integráció**, és **Integráció fiókok** az eredménylistából.     
![Szűrő keresőmező](./media/app-service-logic-enterprise-integration-overview/overview-21.png)  
3. Jelölje ki, amelybe fel a séma **integrációs fiók** .    
![Integráció partnerlistájában képernyőképe](./media/app-service-logic-enterprise-integration-overview/overview-31.png)  

4. Jelölje ki, hogy a **sémák** mozaikot.  
![Képernyőkép a IEP integrációs fiók, kiemelt "sémák"](./media/app-service-logic-enterprise-integration-schemas/schema-11.png)  

### <a name="add-a-schema-file-less-than-2-mb"></a>2 MB-nál kisebb sémafájl hozzáadása  

1. A **sémák** lap megnyíló (az előző lépést) válassza a **Hozzáadás**elemre.  
![Képernyőkép a sémák lap "Hozzáadás" gombja](./media/app-service-logic-enterprise-integration-schemas/schema-21.png)  

2. Adjon meg egy nevet a séma. Töltse fel a sémafájl, válassza a mappa ikonja a **séma** szövegmező mellett. A feltöltés folyamat befejezése után kattintson **az OK gombra**.    
![Képernyőkép a "Séma hozzáadása", "Kis fájllal" kiemelve](./media/app-service-logic-enterprise-integration-schemas/schema-31.png)  

### <a name="add-a-schema-file-larger-than-2-mb-up-to-a-maximum-of-8-mb"></a>2 MB (legfeljebb 8 MB) nagyobb sémafájl hozzáadása  

Ez a folyamat függ, hogy a blob-tárolóhoz hozzáférési szinttel: **nyilvános** vagy **Nincs név nélküli hozzáférés**. Határozza meg a hozzáférési szintet, az **Azure tároló Explorer** **Blob tárolók**, csoportban válassza ki a kívánt blob-tárolóhoz. Válassza a **Biztonság**, és válassza ki a **Hozzáférési szintet** lapot.

1. Ha a blob biztonsági hozzáférési szintet **nyilvános**, kövesse az alábbi lépéseket.  
  ![Képernyőkép az Azure tárolás Explorer, a "Blob tárolók", "Biztonsági" és "Nyilvános" kiemelt](./media/app-service-logic-enterprise-integration-schemas/blob-public.png)  

    egy. Töltse fel a séma tárolás, és másolja a URI.  
    ![Tárterület-fiókkal, a kiemelt URI képernyőképe](./media/app-service-logic-enterprise-integration-schemas/schema-blob.png)  

    b. **Séma hozzáadása**jelölje be a **nagyméretű fájlt**, és küldje el a URI a **Tartalom URI** mezőben.  
    ![A sémák "Hozzáadás" gomb és a kiemelt "nagyméretű fájlt" képernyőképe](./media/app-service-logic-enterprise-integration-schemas/schema-largefile.png)  

2. Ha a blob biztonsági hozzáférési szinttel **nem név nélküli hozzáférés**, kövesse az alábbi lépéseket.  
  ![Képernyőkép Azure tároló Intéző-ablakról, "Blob tárolók", "Biztonsági" és "Nincs a névtelen hozzáférés" kiemelve](./media/app-service-logic-enterprise-integration-schemas/blob-1.png)  

    egy. Töltse fel a séma tárolás.  
    ![Képernyőkép a tárterület-fiók](./media/app-service-logic-enterprise-integration-schemas/blob-3.png)

    b. A séma átengedése aláírást létrehozni.  
    ![Képernyőkép: a tárterület sccount átengedése aláírások lap kiemelt](./media/app-service-logic-enterprise-integration-schemas/blob-2.png)

    c billentyűkombinációt. **Séma hozzáadása**jelölje be a **nagyméretű fájlt**, és küldje el a megosztott access aláírás URI a **Tartalom URI** szövegmezőbe.  
    ![A sémák "Hozzáadás" gomb és a kiemelt "nagyméretű fájlt" képernyőképe](./media/app-service-logic-enterprise-integration-schemas/schema-largefile.png)  

3. A **sémák** lap integrációs fiók EIP-t az újonnan hozzáadott séma most megjelenik.  
![Képernyőkép a EIP-t integrációs fiók "Sémák" és a kiemelt új séma](./media/app-service-logic-enterprise-integration-schemas/schema-41.png)
  

## <a name="edit-schemas"></a>A sémák szerkesztése
1. Jelölje ki, hogy a **sémák** mozaikot.  
2. Jelölje ki a szerkesztendő a megnyíló **sémák** lap séma.
3. A **sémák** lap a válassza a **Szerkesztés**elemre.  
![Képernyőkép a sémák lap](./media/app-service-logic-enterprise-integration-schemas/edit-12.png)    
4. Jelölje ki a szerkesztendő a fájl kiválasztása párbeszédpanel megnyitó használatával sémafájl.
5. Válassza a **Megnyitás** a fájlválasztó.  
![Fájlválasztó képernyőképe](./media/app-service-logic-enterprise-integration-schemas/edit-31.png)  
6. A feltöltés sikerességéről értesítést kap.  

## <a name="delete-schemas"></a>A sémák törlése
1. Jelölje ki, hogy a **sémák** mozaikot.  
2. Jelölje ki a törlendő a **sémák** lap, amely megnyitja a séma.  
3. A **sémák** lap a válassza a **Törlés**elemre.
![Képernyőkép a sémák lap](./media/app-service-logic-enterprise-integration-schemas/delete-12.png)  

4. Erősítse meg, hogy válassza az **Igen gombra**.  
![Képernyőkép a "Delete séma" megerősítést kérő üzenet](./media/app-service-logic-enterprise-integration-schemas/delete-21.png)  
5. Végezetül látható, hogy a **sémák** lap az sémák listáját frissíti, már nem szerepel a törölt sémában.  
![Képernyőkép a EIP-t integrációs fiókot, kiemelt "sémák"](./media/app-service-logic-enterprise-integration-schemas/delete-31.png)    

## <a name="next-steps"></a>Következő lépések

- [További tudnivalók a nagyvállalati integrációs csomag] (./app-service-logic-enterprise-integration-overview.md "Megtudhatja, hogy a nagyvállalati integrációs csomagot").  
