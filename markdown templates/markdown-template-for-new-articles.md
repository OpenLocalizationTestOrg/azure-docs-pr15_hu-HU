<properties
   pageTitle="A böngésző lap és a keresési eredmények között megjelenő lapcím"
   description="A cikk leírást, amely megjelenik a érkezési oldalak és a legtöbb keresési eredmények"
   services="service-name"
   documentationCenter="dev-center-name"
   authors="GitHub-alias-of-only-one-author"
   manager="manager-alias"
   editor=""/>

<tags
   ms.service="required"
   ms.devlang="may be required"
   ms.topic="article"
   ms.tgt_pltfrm="may be required"
   ms.workload="required"
   ms.date="mm/dd/yyyy"
   ms.author="Your MSFT alias or your full email address;semicolon separates two or more"/>

# <a name="markdown-template-article-heading-1-or-h1-heading-at-the-top-of-the-article"></a>Markdown sablon (Címsor 1 vagy H1 cikk): a témakör tetején a címsor

Másolja a markdown ezt a sablont, másolja a vágólapra a cikk a helyi repó, vagy kattintson a felhasználói felület GitHub nyers gombra, és másolja a markdown.

  ![Helyettesítő szöveg; Ne hagyja üresen. Kép ismertetik.][8]

Bekezdés a bevezetőben: Lorem dolor amet, adipiscing elit. Phasellus interdum nulla risus, lacinia porta nisl imperdiet sed. Mauris dolor mauris, tempus sed lacinia nec, euismod nem felis. A nunc semper porta ultrices. Maecenas szöveget nulla, a condimentum szakmai ipsum amet, dignissim aliquet nisi elhelyezkedik.

## <a name="heading-2-h2"></a>A Címsor 2 (H2)

Aenean ülnie amet leo nec purus placerat fermentum ac gravida odio. Aenean tellus lectus, a rhoncus a faucibus sed urna faucibus.  volutpat mi azonosító purus ultrices iaculis nec nem szöveget. [Hivatkozás szövegét hivatkozás azure.microsoft.com kívül](http://weblogs.asp.net/scottgu). Nullam dictum dolor aliquam pharetra elemre. Vivamus ac hendrerit mauris [Példa hivatkozás szövegét hivatkozás egy cikk szolgáltatás mappában](../articles/expressroute/expressroute-bandwidth-upgrade.md).

Nagyobb 10 alkalommal forgalmat jelenik meg a [Google]  [ gog] mint a [Yahoo]  [ yah] - vagy [MSN] [msn].

> [AZURE.NOTE] Tagolt jegyzet szövegét.  "Megjegyzés" szó bekerül a kiadvány során. Elvetése Európai Unió pretium lacus. Nullam purus vállalati ellenőrzési, iaculis sed vállalati ellenőrzési ntenként, euismod vehicula odio. Curabitur lacinia, erat tristique iaculis rutrum, erat sem sodales nisi, az Európai Unió condimentum turpis nisi egy purus.

1. Aenean ülnie amet leo nec **Purus** placerat fermentum ac gravida odio.

2. Aenean tellus lectus, a faucibus sed urna **Rhoncus** a faucius. Suspendisse volutpat mi azonosító purus ultrices iaculis nec nem szöveget.

    ![Helyettesítő szöveg; Ne hagyja üresen. A piros racing autós adatgyűjtő.][5]

3. Nullam dictum dolor aliquam pharetra elemre. Vivamus ac hendrerit mauris. Sed dolor dui, condimentum et egy, a nisl vehicula varius.

    ![Helyettesítő szöveg; Ne hagyja üresen][6]


Suspendisse volutpat mi azonosító purus ultrices iaculis nec nem szöveget. Nullam dictum dolor aliquam pharetra elemre. Vivamus ac hendrerit mauris. Otrus informatus: [hivatkozás egy másik azure.microsoft.com dokumentáció témakör 1](virtual-machines-windows-hero-tutorial.md)

## <a name="heading-h2"></a>Fejléc (H2)

Elvetése Európai Unió pretium lacus. Nullam purus vállalati ellenőrzési, iaculis sed vállalati ellenőrzési ntenként, euismod vehicula odio.

1. Curabitur lacinia, erat tristique iaculis rutrum, erat sem sodales nisi, az Európai Unió condimentum turpis nisi egy purus.

        - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:
        (NSDictionary *)launchOptions
        {
            // Register for remote notifications
            [[UIApplication sharedApplication] registerForRemoteNotificationTypes:
            UIRemoteNotificationTypeAlert | UIRemoteNotificationTypeBadge | UIRemoteNotificationTypeSound];
            return YES;
        }

2. Vestibulum előzetes ipsum primis a faucibus orci luctus et ultrices posuere cubilia.

        // Because toast alerts don't work when the app is running, the app handles them.
        // This uses the userInfo in the payload to display a UIAlertView.
        - (void)application:(UIApplication *)application didReceiveRemoteNotification:
        (NSDictionary *)userInfo {
            NSLog(@"%@", userInfo);
            UIAlertView *alert = [[UIAlertView alloc] initWithTitle:@"Notification" message:
            [userInfo objectForKey:@"inAppMessage"] delegate:nil cancelButtonTitle:
            @"OK" otherButtonTitles:nil, nil];
            [alert show];
        }


    > [AZURE.NOTE] Duis sed diam nem <i>nisl molestie</i> pharetra eget egy vállalati ellenőrzési. [Hivatkozás egy másik azure.microsoft.com dokumentáció témakör 2](web-sites-custom-domain-name.md)


Quisque commodo eros ntenként lectus euismod auctor eget ülnie amet leo. Proin faucibus suscipit tellus dignissim ultrices.

## <a name="heading-2-h2"></a>A Címsor 2 (H2)

1. Maecenas sed condimentum nisi. Suspendisse potenti.

  + Fusce
  + Malesuada
  + Sem

2. A massa Európai Unió tellus tempus hendrerit Nullam.

    ![Helyettesítő szöveg; Ne hagyja üresen. Példa egy csatorna 9 videót.][7]

3. Quisque felis enim, fermentum elvetése aliquam nec, pellentesque pulvináris magna.




<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Következő lépések

Vestibul előzetes ipsum primis a faucibus orci luctus et ultrices posuere cubilia Curae; Nullam ultricies, ipsum szakmai volutpat hendrerit purus diam pretium eros, a szakmai tincidunt nulla lorem sed turpis: [hivatkozás egy másik azure.microsoft.com dokumentáció témakör 3](storage-whatis-account.md).

<!--Image references-->
[5]: ./media/markdown-template-for-new-articles/octocats.png
[6]: ./media/markdown-template-for-new-articles/pretty49.png
[7]: ./media/markdown-template-for-new-articles/channel-9.png
[8]: ./media/markdown-template-for-new-articles/copytemplate.png

<!--Reference style links - using these makes the source content way more readable than using inline links-->
[gog]: http://google.com/        
[yah]: http://search.yahoo.com/  
[msn]: http://search.msn.com/    
