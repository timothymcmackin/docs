---

copyright:
 years: 2015, 2016

---

# Notifiche interattive
{: #interactive-notifications}
Ultimo aggiornamento: 16 agosto 2016
{: .last-updated}

Le notifiche interattive consentono agli utenti di agire quando viene ricevuta una notifica senza aprire l'applicazione. Quando viene ricevuta una notifica, il dispositivo visualizza i pulsanti di azione insieme al messaggio di notifica. Le notifiche interattive sono supportare su dispositivi iOS con versione 8 o successiva. Se viene inviata una notifica interattiva a dispositivi iOS con una versione inferiore alla 8, le azioni di notifica non vengono visualizzate.

##Invio di {{site.data.keyword.mobilepushshort}} interattive


La notifica interattiva può essere inviata utilizzando il dashboard Push o utilizzando l'API REST (vedi la documentazione API REST).

Dalla console push: 



1. Nella scheda delle notifiche nel dashboard push, fai clic su **Invia notifica**. 
2. Scegli i tuoi destinatari per la notifica e fai clic su **Avanti**. 
3. Nella pagina di composizione della notifica, può essere inviato un push interattivo impostando il tipo su predefinito o misto e specificando il valore della categoria nella scheda del delle opzioni avanzate. Per configurare il valore della categoria sul client, controlla **Gestione {{site.data.keyword.mobilepushshort}}** interattiva nella sezione dell'applicazione iOS nativa.

## Gestione {{site.data.keyword.mobilepushshort}} interattive in un'applicazione iOS

Completa la seguente procedura per ricevere le notifiche interattive:

1. Abilita la funzionalità dell'applicazione ad eseguire attività in background per la ricezione di notifiche remote. Questo passo è obbligatorio se alcune delle azioni sono abilitate in background.
1. In AppDelegate (application: didRegisterForRemoteNotificationsWithDeviceTokenapplication:), configura le categorie prima di impostare `deviceToken` su `WLPush Object`.

```
	if([application respondsToSelector:@selector(registerUserNotificationSettings:)]){
	 UIUserNotificationType userNotificationTypes = UIUserNotificationTypeNone | UIUserNotificationTypeSound | UIUserNotificationTypeAlert | UIUserNotificationTypeBadge;
	      
	 UIMutableUserNotificationAction *acceptAction = [[UIMutableUserNotificationAction alloc] init];
	 acceptAction.identifier = @"OK";
	 acceptAction.title = @"OK";
	      
	 UIMutableUserNotificationAction *rejetAction = [[UIMutableUserNotificationAction alloc] init];
	 rejetAction.identifier = @"NOK";
	 rejetAction.title = @"NOK";
	      
	 UIMutableUserNotificationCategory *cateogory = [[UIMutableUserNotificationCategory alloc] init];
	 cateogory.identifier = @"poll";
	 [cateogory setActions:@[acceptAction,rejetAction] forContext:UIUserNotificationActionContextDefault];
	 [cateogory setActions:@[acceptAction,rejetAction] forContext:UIUserNotificationActionContextMinimal];
	      
	 NSSet *catgories = [NSSet setWithObject:cateogory];
	 [application registerUserNotificationSettings:[UIUserNotificationSettings settingsForTypes:userNotificationTypes categories:catgories]];
}
```

1. Implementa il nuovo metodo di callback su AppDelegate:

```
	-(void)application:(UIApplication *)application handleActionWithIdentifier:(NSString *)identifier forRemoteNotification:(NSDictionary *)userInfo completionHandler:(void (ˆ)())completionHandler
``` 

5. Questo nuovo metodo di callback viene richiamato quando l'utente fa clic sul pulsante azione. L'implementazione di questo metodo deve eseguire le azioni associate con l'identificativo specificato ed eseguire il blocco nel parametro `completionHandler`.
