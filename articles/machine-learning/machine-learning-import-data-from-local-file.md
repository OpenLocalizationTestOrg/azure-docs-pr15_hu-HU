<properties
    pageTitle="Adatok importálása az gépi tanulási Studio helyi fájlból |} Microsoft Azure"
    description="Hogyan lehet az oktatóanyag adatok Azure gépi tanulási Studio importálása egy helyi fájl."
    keywords="Importálás adatok, adatformátum, adattípusok, adatforrásokhoz, oktatóanyag adatok"
    services="machine-learning"
    documentationCenter=""
    authors="garyericson"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="garye;bradsev" />


# <a name="import-your-training-data-into-azure-machine-learning-studio-from-a-local-file"></a>Az oktatóanyag adatok importálása az Azure gépi tanulási Studio helyi fájlból

[AZURE.INCLUDE [import-data-into-aml-studio-selector](../../includes/machine-learning-import-data-into-aml-studio.md)]


Gépi tanulási Studio használni a saját adatain, töltse fel egy adatfájlt időszakokra merevlemezről helyi adatkészlet modul létrehozása a munkaterületen. 


## <a name="import-data-from-a-local-file"></a>Adatok importálása egy helyi fájl

Helyi merevlemez-meghajtó adatokat importálhatja az alábbiak szerint:

1. Válassza a **+ Új** a gépi tanulási Studio ablak alján.
2. Jelölje ki az **ADATKÉSZLET** és a **Feladó helyi fájl**.
3. **Töltse fel egy új adatkészletet** párbeszédpanelen tallózással keresse meg a feltöltendő fájlt
4. Írjon be egy nevet, azonosítják az adatokat, és tetszés szerint adjon meg egy leírást. Leírás ajánlott – lehetővé teszi, hogy rögzítse minden jellemző kapcsolatban is szükség van a jövőben az adatok használatakor ne feledje, hogy adatokat.
5. A jelölőnégyzet, **az új verziója, amelyet egy meglévő adatkészlet** lehetővé teszi, hogy egy meglévő adatkészlet frissíteni az új adatokat kell. Egyszerűen csak kattintson az ezt a jelölőnégyzetet, majd egy meglévő adatkészlet nevét.

Alatt a feltöltési ekkor megjelenik egy üzenet, hogy a fájl feltöltése folyamatban van. Töltse fel ideje méretét az adatok és a kapcsolat sebességét a szolgáltatás függ.
Ha tudja, hogy a fájl hosszú időt vesz igénybe, műveleteket végezheti el más belül gépi tanulási Studio várakozás közben. Azonban a böngésző bezárása okoz a adatok feltöltés sikertelen lesz.

Az adatok töltenek fel, miután adatkészlet modulban található, és a munkaterületen kísérletek számára érhető el.
Ha egy kísérlet szerkeszt, az adatkészleteket, a **Mentett adatkészleteket** listában a modul paletta **Saját adatkészleteket** listában létrehozott is megkeresheti. Húzza, és húzza az alakzatot a kísérlet vászon adatkészlet, ha meg szeretné használni az adatkészlet számára a további analitikai és gépi tanulási.



