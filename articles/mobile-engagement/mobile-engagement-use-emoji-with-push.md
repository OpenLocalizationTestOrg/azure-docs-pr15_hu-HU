<properties 
    pageTitle="Azure Mobile tetszés szerint elmélyedhet belül Emoji Hangulatjelek használata" 
    description="A leküldéses értesítések belül Emoji Hangulatjelek használata"     
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="piyushjo" 
    manager="dwrede" 
    editor="" />

<tags 
    ms.service="mobile-engagement" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="mobile-windows-phone" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/19/2016" 
    ms.author="piyushjo" />

#<a name="use-emoji-emoticon-within-push-notifications"></a>Leküldéses értesítések belül Emoji hangulatjel használata

Emoji is elhelyezhet, a hangulatjelek leküldéses értesítést a néhány egyszerű lépéssel: 

1. Először is meg szeretné tudni, a Emoji el szeretné küldeni az üzenetet a. Győződjön meg arról, hogy a elemre kattintva Emoji támogatja a célként megadott eszköz, az eszköz termékek újonnan engedélyezett Emojis hozzáadása a eszköz platformon némi időbe telik. 

2. A **Windows** - nyissa meg azt a [hivatkozást](http://apps.timwhitlock.info/emoji/tables/unicode) , és másolja a natív"ikonra.

    ![][7] 

3. A **Mac** - található a szótár alkalmazásban a Szerkesztés csoport Emojis -> Emoji és szimbólumok.

    ![][6] 

4. Most, lépjen a **elérje a** lapra a az Azure Mobile tetszés szerint elmélyedhet portálon. Válassza ki a leküldéses értesítést (közleményt, lekérdezi stb). Ez a példa azt egy bejelentése leküldéses közül.

5. Adja meg az értesítés különböző területei, amíg el nem éri az értesítés szövegét. Ez akkor, ha hozzáadja a Emoji hangulatjel. Megadhatja, hogy elhelyezése a cím, az üzenet vagy mindkettőt. Húzza a fenti helyekről ismertetésében Emoji másolja vagy kell. 

    ![][1]

6. Töltse ki az értesítés a mezőket, és mentse. 

7. Egy teszt vagy aktiválása az értesítés jelenik meg a hangulatjel az értesítés, a megadott módon.   

    ![][3] ![][4] ![][5]

<!-- Images. -->
[1]: ./media/mobile-engagement-use-emoji-with-push/notification_input.png
[3]: ./media/mobile-engagement-use-emoji-with-push/iOS_Emoji.png
[4]: ./media/mobile-engagement-use-emoji-with-push/Android_Emoji.png
[5]: ./media/mobile-engagement-use-emoji-with-push/WindowsPhone_Emoji.png
[6]: ./media/mobile-engagement-use-emoji-with-push/Mac_SelectEmoji.png
[7]: ./media/mobile-engagement-use-emoji-with-push/Windows_SelectEmoji.png

