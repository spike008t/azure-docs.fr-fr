---
author: conceptdev
ms.author: crdun
ms.service: app-service-mobile
ms.topic: include
ms.date: 08/23/2018
ms.openlocfilehash: a53d2b259bc4ece12c4ccb1cf47409cd2f0af86f
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66140722"
---
**Objective-C**:

1. Dans **QSAppDelegate.m**, importez le Kit de développement logiciel (SDK) iOS ainsi que **QSTodoService.h** :

    ```objc
    #import <MicrosoftAzureMobile/MicrosoftAzureMobile.h>
    #import "QSTodoService.h"
    ```

2. Dans `didFinishLaunchingWithOptions` de **QSAppDelegate.m**, insérez les lignes suivantes juste avant `return YES;` :

    ```objc
    UIUserNotificationSettings* notificationSettings = [UIUserNotificationSettings settingsForTypes:UIUserNotificationTypeAlert | UIUserNotificationTypeBadge | UIUserNotificationTypeSound categories:nil];
    [[UIApplication sharedApplication] registerUserNotificationSettings:notificationSettings];
    [[UIApplication sharedApplication] registerForRemoteNotifications];
    ```

3. Dans **QSAppDelegate.m**, ajoutez les méthodes de gestionnaire suivantes. L'application est mise à jour et prend en charge les notifications Push. 

    ```objc
    // Registration with APNs is successful
    - (void)application:(UIApplication *)application
    didRegisterForRemoteNotificationsWithDeviceToken:(NSData *)deviceToken {

        QSTodoService *todoService = [QSTodoService defaultService];
        MSClient *client = todoService.client;

        [client.push registerDeviceToken:deviceToken completion:^(NSError *error) {
            if (error != nil) {
                NSLog(@"Error registering for notifications: %@", error);
            }
        }];
    }

    // Handle any failure to register
    - (void)application:(UIApplication *)application didFailToRegisterForRemoteNotificationsWithError:
    (NSError *)error {
        NSLog(@"Failed to register for remote notifications: %@", error);
    }

    // Use userInfo in the payload to display an alert.
    - (void)application:(UIApplication *)application
            didReceiveRemoteNotification:(NSDictionary *)userInfo {
        NSLog(@"%@", userInfo);

        NSDictionary *apsPayload = userInfo[@"aps"];
        NSString *alertString = apsPayload[@"alert"];

        // Create alert with notification content.
        UIAlertController *alertController = [UIAlertController
                                        alertControllerWithTitle:@"Notification"
                                        message:alertString
                                        preferredStyle:UIAlertControllerStyleAlert];

        UIAlertAction *cancelAction = [UIAlertAction
                                        actionWithTitle:NSLocalizedString(@"Cancel", @"Cancel")
                                        style:UIAlertActionStyleCancel
                                        handler:^(UIAlertAction *action)
                                        {
                                            NSLog(@"Cancel");
                                        }];

        UIAlertAction *okAction = [UIAlertAction
                                    actionWithTitle:NSLocalizedString(@"OK", @"OK")
                                    style:UIAlertActionStyleDefault
                                    handler:^(UIAlertAction *action)
                                    {
                                        NSLog(@"OK");
                                    }];

        [alertController addAction:cancelAction];
        [alertController addAction:okAction];

        // Get current view controller.
        UIViewController *currentViewController = [[[[UIApplication sharedApplication] delegate] window] rootViewController];
        while (currentViewController.presentedViewController)
        {
            currentViewController = currentViewController.presentedViewController;
        }

        // Display alert.
        [currentViewController presentViewController:alertController animated:YES completion:nil];

    }
    ```

**Swift**:

1. Ajoutez le fichier **ClientManager.swift** avec le contenu suivant. Remplacez *%AppUrl%* par l’URL du serveur principal Azure Mobile App.

    ```swift
    class ClientManager {
        static let sharedClient = MSClient(applicationURLString: "%AppUrl%")
    }
    ```

2. Dans **ToDoTableViewController.swift**, remplacez la ligne `let client` chargée d’initialiser un `MSClient` par cette ligne :

    ```swift
    let client = ClientManager.sharedClient
    ```

3. Dans **AppDelegate.swift**, remplacez le corps de `func application` comme suit :

    ```swift
    func application(application: UIApplication,
        didFinishLaunchingWithOptions launchOptions: [NSObject: AnyObject]?) -> Bool {
        application.registerUserNotificationSettings(
            UIUserNotificationSettings(forTypes: [.Alert, .Badge, .Sound],
                categories: nil))
        application.registerForRemoteNotifications()
        return true
    }
    ```

4. Dans **AppDelegate.swift**, ajoutez les méthodes de gestionnaire suivantes. L'application est mise à jour et prend en charge les notifications Push.

    ```swift
    func application(application: UIApplication,
        didRegisterForRemoteNotificationsWithDeviceToken deviceToken: NSData) {
        ClientManager.sharedClient.push?.registerDeviceToken(deviceToken) { error in
            print("Error registering for notifications: ", error?.description)
        }
    }

    func application(application: UIApplication,
        didFailToRegisterForRemoteNotificationsWithError error: NSError) {
        print("Failed to register for remote notifications: ", error.description)
    }

    func application(application: UIApplication,
        didReceiveRemoteNotification userInfo: [NSObject: AnyObject]) {

        print(userInfo)

        let apsNotification = userInfo["aps"] as? NSDictionary
        let apsString       = apsNotification?["alert"] as? String

        let alert = UIAlertController(title: "Alert", message: apsString, preferredStyle: .Alert)
        let okAction = UIAlertAction(title: "OK", style: .Default) { _ in
            print("OK")
        }
        let cancelAction = UIAlertAction(title: "Cancel", style: .Default) { _ in
            print("Cancel")
        }

        alert.addAction(okAction)
        alert.addAction(cancelAction)

        var currentViewController = self.window?.rootViewController
        while currentViewController?.presentedViewController != nil {
            currentViewController = currentViewController?.presentedViewController
        }

        currentViewController?.presentViewController(alert, animated: true) {}

    }
    ```
