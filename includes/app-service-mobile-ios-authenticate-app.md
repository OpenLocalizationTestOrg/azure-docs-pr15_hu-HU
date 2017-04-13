**Cél-C**: 

1. Azon a Macen nyissa meg a _QSTodoListViewController.m_ Xcode, és adja hozzá a következő módszerrel. _A google_ módosítása _microsoftaccountja_, _twitter_, _facebook_vagy _windowsazureactivedirectory_ nem használata Google az identitás-konferenciaszolgáltatóként. Ha a Facebook, [az alkalmazás whitelist Facebook tartományokat kell](https://developers.facebook.com/docs/ios/ios9#whitelist)használnia.

            - (void) loginAndGetData
            {
                MSClient *client = self.todoService.client;
                if (client.currentUser != nil) {
                    return;
                }
            
                [client loginWithProvider:@"google" controller:self animated:YES completion:^(MSUser *user, NSError *error) {
                    [self refresh];
                }];
            }


2. Csere `[self refresh]` a `viewDidLoad` a _QSTodoListViewController.m_ a következőre:

            [self loginAndGetData];

3. Nyomja le a _futtatásához_ nyissa meg az alkalmazást, és jelentkezzen be. Ha be van jelentkezve, megtekintheti a teendőlista, és végezze el a módosításokat kell lennie.

**Swift**:

1. Azon a Macen nyissa meg a _ToDoTableViewController.swift_ Xcode, és adja hozzá a következő módszerrel. _A google_ módosítása _microsoftaccountja_, _twitter_, _facebook_vagy _windowsazureactivedirectory_ nem használata Google az identitás-konferenciaszolgáltatóként. Ha a Facebook, [az alkalmazás whitelist Facebookkal tartományokat kell](https://developers.facebook.com/docs/ios/ios9#whitelist)használnia.
        
            func loginAndGetData() {
                
                guard let client = self.table?.client where client.currentUser == nil else {
                    return
                }
                
                client.loginWithProvider("google", controller: self, animated: true) { (user, error) in
                    self.refreshControl?.beginRefreshing()
                    self.onRefresh(self.refreshControl)
                }
            }


2. A sorok eltávolítása `self.refreshControl?.beginRefreshing()` és `self.onRefresh(self.refreshControl)` végén `viewDidLoad()` a _ToDoTableViewController.swift_. Adja hozzá a hívást kezdeményez `loginAndGetData()` helyükre a:

            loginAndGetData()

3. Nyomja le a _futtatásához_ nyissa meg az alkalmazást, és jelentkezzen be. Ha be van jelentkezve, megtekintheti a teendőlista, és végezze el a módosításokat kell lennie.
