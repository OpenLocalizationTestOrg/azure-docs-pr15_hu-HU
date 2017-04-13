<properties 
    pageTitle="Adatok Factory - elnevezési szabályai |} Microsoft Azure" 
    description="Adatok gyári szervezetek elnevezési szabályai ismerteti." 
    services="data-factory" 
    documentationCenter="" 
    authors="sharonlo101" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/12/2016" 
    ms.author="shlo"/>

# <a name="azure-data-factory---naming-rules"></a>Azure adatok Factory - elnevezési szabályai 
Az alábbi táblázat a Data Factory eltérések elnevezési szabályai biztosít.



név | Név egyediségét | Adatérvényesítési szempontú vizsgálatok
:--- | :-------------- | :----------------
Adatok gyári | Egyedi Microsoft Azure keresztül. Nevek-és nagybetűk, ez azt jelenti, hogy a MyDF és mydf keresse meg a azonos adatokat gyári. |<ul><li>Minden adat gyári pontosan egy Azure előfizetés területhez tartozik.</li><li>Objektumnevek egy betűt vagy egy számot kell kezdődnie, és csak betűket, számokat és a kötőjel (-) karaktert tartalmazhatnak.</li><li>Minden egyes kötőjel (-) karakter azonnal kell előtte és utána egy betűt vagy egy számot. Egymást követő szaggatott vonal tároló nevek nem engedélyezettek.</li><li>Név 3-63 karakter hosszú lehet.</li></ul>
Szolgáltatások/táblák és folyamatok csatolt | Az adatok gyár az egyedi. Nevek és nagybetűk között. | <ul><li>A táblázatok nevét a karakterek maximális száma: 260.</li><li>Objektumnevek betű szám vagy az aláhúzásjel _ kell kezdődnie.</li><li>A következő karakterek nem használhatók: ".", "+", "?", "/", "<", ">","*", "%", "&", ":","\\"</li></ul>
Erőforráscsoport | Egyedi Microsoft Azure keresztül. Nevek és nagybetűk között. | <ul><li>Karakterek maximális száma: 1000.</li><li>Név tartalmazhat betűk, számjegyek és a következő karakterek: "-", "_",","és".".</li></ul>