---
title: Enviar notificações por push para o iOS usando os Hubs de Notificação do Azure e o SDK do iOS versão 3.0.0 versão prévia 1
description: Neste tutorial, você aprenderá a usar os Hubs de Notificação do Azure e o Apple Push Notification Service para enviar notificações por push para dispositivos iOS específicos.
author: sethmanheim
ms.author: sethm
ms.date: 06/19/2020
ms.topic: tutorial
ms.service: notification-hubs
ms.reviewer: thsomasu
ms.lastreviewed: 06/01/2020
ms.openlocfilehash: bec4b771387854c40045f2b57afe7ead6c52f2c7
ms.sourcegitcommit: 2ff0d073607bc746ffc638a84bb026d1705e543e
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87836030"
---
# <a name="tutorial-send-push-notifications-to-ios-apps-using-azure-notification-hubs-version-300-preview1"></a>Tutorial: Enviar notificações por push para aplicativos iOS usando os Hubs de Notificação do Microsoft Azure (versão 3.0.0 versão prévia 1)

Este tutorial mostra como usar os Hubs de Notificação do Azure para enviar notificações por push para um aplicativo iOS usando o SDK dos Hubs de Notificação do Azure versão 2.0.4.

Este tutorial cobre as seguintes etapas:

- Criar um aplicativo iOS de exemplo.
- Conectar seu aplicativo iOS aos Hubs de Notificação do Azure.
- Enviar notificações por push de teste.
- Verificar se seu aplicativo recebe notificações.

Baixe o código completo deste tutorial [no GitHub](https://github.com/Azure/azure-notificationhubs-ios/tree/v3-preview2/Samples).

## <a name="prerequisites"></a>Pré-requisitos

Para concluir este tutorial, você precisará dos seguintes pré-requisitos:

- Um Mac executando [Xcode](https://go.microsoft.com/fwLink/p/?LinkID=266532), juntamente com um certificado de desenvolvedor válido instalado em seu conjunto de chaves.
- Um iPhone ou iPad executando o iOS versão 10 ou posterior.
- Seu dispositivo físico registrado no [portal da Apple](https://developer.apple.com/) e associado ao seu certificado.

Antes de prosseguir, siga o tutorial anterior sobre como começar a usar os [Hubs de Notificação do Azure para aplicativos iOS](ios-sdk-get-started.md) para então preparar e configurar as credenciais de push em seu hub de notificação. Mesmo que você não tenha nenhuma experiência anterior com o desenvolvimento do iOS, você será capaz de seguir estas etapas.

> [!NOTE]
> Devido aos requisitos de configuração das notificações por push, você precisa implantá-las e testá-las em um dispositivo iOS físico (iPhone ou iPad), em vez de usar o emulador de iOS.

## <a name="connect-your-ios-app-to-notification-hubs"></a>Conectar seu aplicativo do iOS aos Hubs de Notificação

1. No Xcode, crie um projeto do iOS e selecione o modelo **Aplicativo de Modo de Exibição Único** .

   :::image type="content" source="media/ios-sdk/image1.png" alt-text="Selecionar modelo":::

2. Ao definir as opções para o novo projeto, lembre-se de usar os mesmos **Nome do Produto** e **Identificador Organizacional** que você usou quando configurou o identificador do pacote no Apple Developer Portal.

3. Em Navegador de Projeto, selecione o nome do projeto em **Destinos**e selecione a guia **Assinatura e Funcionalidades** . Verifique se você selecionou a **Equipe** apropriada para sua conta de Desenvolvedor da Apple. O deverá automaticamente efetuar pull do perfil de provisionamento que você criou anteriormente com base em seu identificador de pacote.

   Se você não vir o novo perfil de provisionamento que você criou no Xcode, tente atualizar os perfis da sua identidade de assinatura. Clique em **Xcode** na barra de menus, em **Preferências**, na guia **Conta** , no botão **Exibir Detalhes** , em sua identidade de assinatura e depois clique no botão Atualizar no canto inferior direito.

   :::image type="content" source="media/ios-sdk/image2.png" alt-text="Exibir detalhes":::

4. Na guia **Assinatura e Funcionalidades** , selecione **+ Funcionalidade**. Clique duas vezes em **Notificações por Push** para habilitá-la.

   :::image type="content" source="media/ios-sdk/image3.png" alt-text="Recurso":::

5. Adicione os módulos do SDK dos Hubs de Notificação do Microsoft Azure.

   É possível integrar o SDK dos Hubs de Notificação do Microsoft Azure ao aplicativo usando [Cocoapods](https://cocoapods.org/) ou adicionando manualmente os binários ao projeto.

   - Integração por meio de Cocoapods: Adicione as seguintes dependências ao seu podfile para incluir o SDK dos Hubs de Notificação do Azure em seu aplicativo:

      ```ruby
      pod 'AzureNotificationHubs-iOS'
      ```

      - Execute pod install para instalar seu pod recém-definido e abrir seu .xcworkspace.

         Se um erro como **Não é possível encontrar uma especificação para AzureNotificationHubs-iOS** for exibido durante a execução de pod install, execute `pod repo update` para obter os pods mais recentes do repositório Cocoapods e execute pod install.

   - Integração copiando os binários em seu projeto:

      Você pode realizar a integração copiando os binários para seu projeto, da seguinte maneira:

        - Faça o download da estrutura do [SDK dos Hubs de Notificação do Azure](https://github.com/Azure/azure-notificationhubs-iOS/releases/) fornecida como um arquivo zip e descompacte-o.

        - No Xcode, clique com o botão direito do mouse no projeto e clique na opção **Adicionar Arquivos a** para adicionar a pasta **WindowsAzureMessaging.framework** ao seu projeto do Xcode. Selecione **Opções** , verifique se a opção **Copiar itens se necessário** está selecionada e clique em **Adicionar**.

          :::image type="content" source="media/ios-sdk/image4.png" alt-text="Adicionar estrutura":::

6. Adicione um novo arquivo de cabeçalho a seu projeto chamado **Constants.h**. Para fazer isso, clique com o botão direito do mouse no nome do projeto e selecione **Novo Arquivo…** . Em seguida, selecione **Arquivo de Cabeçalho**. Esse arquivo conterá as constantes para o hub de notificação. Depois, selecione **Avançar**. Nomeie o arquivo **Constants.h**.

7. Adicione o seguinte código ao arquivo Constants.h:

   ```objc
   #ifndef Constants_h
   #define Constants_h
   extern NSString* const NHInfoConnectionString;
   extern NSString* const NHInfoHubName;
   extern NSString* const NHUserDefaultTags;
   #endif /* Constants_h */
   ```

8. Adicione o arquivo de implementação para Constants.h. Para fazer isso, clique com o botão direito do mouse no nome do projeto e selecione **Novo Arquivo…** . Selecione **Arquivo Objective-C** e clique em **Avançar**. Nomeie o arquivo **Constants.m**.

   :::image type="content" source="media/ios-sdk/image5.png" alt-text="Adicionar arquivo de implementação":::

9. Abra o arquivo **Constants.m** e substitua o conteúdo pelo código a seguir. Substitua os espaços reservados de literal de cadeia de caracteres `NotificationHubConnectionString` e `NotificationHubConnectionString` pelo nome do hub e a **DefaultListenSharedAccessSignature**, respectivamente, como você obteve anteriormente do portal:

   ```objc
   #import <Foundation/Foundation.h>
   #import "Constants.h"

   NSString* const NHInfoConnectionString = @"NotificationHubConnectionString";
   NSString* const NHInfoHubName = @"NotificationHubName";NSString* const NHUserDefaultTags = @"notification_tags";
   ```

10. No arquivo **AppDelegate.h** do projeto, adicione a seguinte instrução `import`:

    ```objc
    #import "Constants.h"
    ```

11. No mesmo arquivo **AppDelegate.m** , substitua todo o código após `didFinishLaunchingWithOptions` pelo seguinte código:

    ```objc
    // Tells the delegate that the app successfully registered with Apple Push Notification service (APNs).


    - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {

    NSString *hubName = [[NSBundle mainBundle] objectForInfoDictionaryKey:NHInfoHubName];
    NSString *connectionString = [[NSBundle mainBundle] objectForInfoDictionaryKey:NHInfoConnectionString];
    [MSNotificationHub initWithConnectionString:connectionString withHubName:hubName];


    NSMutableSet *tags = [[NSMutableSet alloc] init];

    // Load and parse stored tags
    NSString *unparsedTags = [[NSUserDefaults standardUserDefaults] valueForKey:NHUserDefaultTags];
    if (unparsedTags.length > 0) {
        NSArray *tagsArray = [unparsedTags componentsSeparatedByString: @","];

        [MSNotificationHub addTags:tagsArray];
    }

    }
    - (void)showAlert:(NSString *)message withTitle:(NSString *)title {
    if (title == nil) {
        title = @"Alert";
    }
    UIAlertController *alert = [UIAlertController alertControllerWithTitle:title message:message preferredStyle:UIAlertControllerStyleAlert];
    [alert addAction:[UIAlertAction actionWithTitle:@"OK" style:UIAlertActionStyleDefault handler:nil]];
    [[[[UIApplication sharedApplication] keyWindow] rootViewController] presentViewController:alert animated:YES completion:nil];
    }

    - (void)showNotification:(NSDictionary *)userInfo {
    [self logNotificationDetails:userInfo];

    NotificationDetailViewController *notificationDetail = [[NotificationDetailViewController alloc] initWithUserInfo:userInfo];
    [[[[UIApplication sharedApplication] keyWindow] rootViewController] presentViewController:notificationDetail animated:YES completion:nil];
    }

    @end
    ```

    Esse código conecta-se ao hub de notificação usando as informações de conexão que você especificou em **Constants.h**. Esse código fornece o token do dispositivo ao hub de notificação para que o hub possa enviar notificações.

### <a name="create-notificationdetailviewcontroller-header-file"></a>Criar arquivo de cabeçalho de NotificationDetailViewController

1. Assim como as instruções anteriores, adicione outro arquivo de cabeçalho chamado **NotificationDetailViewController.h**. Substitua o conteúdo do novo arquivo de cabeçalho pelo seguinte código:

   ```objc
   #import <UIKit/UIKit.h>

   NS_ASSUME_NONNULL_BEGIN

   @interface NotificationDetailViewController : UIViewController

   @property (strong, nonatomic) IBOutlet UILabel *titleLabel;
   @property (strong, nonatomic) IBOutlet UILabel *bodyLabel;
   @property (strong, nonatomic) IBOutlet UIButton *dismissButton;

   @property (strong, nonatomic) NSDictionary *userInfo;

   - (id)initWithUserInfo:(NSDictionary *)userInfo;

   @end

   NS_ASSUME_NONNULL_END
   ```

2. Adicione o arquivo de implementação **NotificationDetailViewController.m**. Substitua o conteúdo do arquivo pelo seguinte código, que implementa os métodos UIViewController:

   ```objc
   #import "NotificationDetailViewController.h"

   @interface NotificationDetailViewController ()

   @end

   @implementation NotificationDetailViewController

   - (id)initWithUserInfo:(NSDictionary *)userInfo {
    self = [super initWithNibName:@"NotificationDetail" bundle:nil];
    if (self) {
        _userInfo = userInfo;
    }
    return self;
   }

   - (void)viewDidLayoutSubviews {
    [self.titleLabel sizeToFit];
    [self.bodyLabel sizeToFit];
   }

   - (void)viewDidLoad {
    [super viewDidLoad];

    NSString *title = nil;
    NSString *body = nil;

    NSDictionary *aps = [_userInfo valueForKey:@"aps"];
    NSObject *alertObject = [aps valueForKey:@"alert"];
    if (alertObject != nil) {
        if ([alertObject isKindOfClass:[NSDictionary class]]) {
            NSDictionary *alertDict = (NSDictionary *)alertObject;
            title = [alertDict valueForKey:@"title"];
            body = [alertObject valueForKey:@"body"];
        } else if ([alertObject isKindOfClass:[NSString class]]) {
            body = (NSString *)alertObject;
        } else {
            NSLog(@"Unable to parse notification content. Unexpected format: %@", alertObject);
        }
    }

    if (title == nil) {
        title = @"<unset>";
    }

    if (body == nil) {
        body = @"<unset>";
    }

    self.titleLabel.text = title;
    self.bodyLabel.text = body;
   }

   - (IBAction)handleDismiss:(id)sender {
    [self dismissViewControllerAnimated:YES completion:nil];
   }

   @end
   ```

### <a name="viewcontroller"></a>ViewController

1. No arquivo **ViewController.m** do projeto, adicione as seguintes instruções `import` :

   ```objc
   #import <WindowsAzureMessaging/WindowsAzureMessaging.h>
   #import <UserNotifications/UserNotifications.h>
   ```

2. Além disso, em **ViewController.h**, adicione as seguintes declarações de propriedade após a declaração `@interface` :

   ```objc
   @property (strong, nonatomic) IBOutlet UITextField *tagsTextField;
   ```

3. No arquivo de implementação **ViewController.m** do projeto, substitua o conteúdo do arquivo pelo seguinte código:

   ```objc
   #import "ViewController.h"
   #import "Constants.h"
   #import "AppDelegate.h"

   @interface ViewController ()

   @end

   @implementation ViewController

   // UIViewController methods

   - (void)touchesBegan:(NSSet<UITouch *> *)touches withEvent:(UIEvent *)event {
    // Simple method to dismiss keyboard when user taps outside of the UITextField.
    [self.view endEditing:YES];
   }

   - (void)viewDidLoad {
    [super viewDidLoad];

    // Load raw tags text from storage and initialize the text field
    self.tagsTextField.text = [[NSUserDefaults standardUserDefaults] valueForKey:NHUserDefaultTags];
   }

   - (IBAction)handleRegister:(id)sender {
    // Save raw tags text in storage
    [[NSUserDefaults standardUserDefaults] setValue:self.tagsTextField.text forKey:NHUserDefaultTags];

   // Delegate processing the register action to the app delegate.
   [[[UIApplication sharedApplication] delegate] performSelector:@selector(handleRegister)];
   }

   - (IBAction)handleUnregister:(id)sender {
    [[[UIApplication sharedApplication] delegate] performSelector:@selector(handleUnregister)];
   }

   @end
   ```

4. Compile e execute o aplicativo no dispositivo para verificar se não há falhas.

## <a name="send-test-push-notifications"></a>Enviar notificações por push de teste

Você pode testar o recebimento de notificações no aplicativo com a opção **Envio de Teste** no [portal do Azure](https://portal.azure.com/). Isso envia uma notificação por push de teste para seu dispositivo.

:::image type="content" source="media/ios-sdk/image6.png" alt-text="Envio de teste":::

As notificações por push normalmente são enviadas em um serviço de back-end como Aplicativos Móveis ou ASP.NET usando uma biblioteca compatível. Se uma biblioteca não estiver disponível para o seu back-end, você também poderá usar a API REST diretamente para enviar mensagens de notificação.

Aqui está uma lista de alguns outros tutoriais que talvez você queira examinar para o envio de notificações:

- Aplicativos Móveis do Azure: para obter um exemplo de como enviar notificações de um back-end dos Aplicativos Móveis integrado aos Hubs de Notificação, confira [Adicionar notificações por push ao seu aplicativo iOS](/previous-versions/azure/app-service-mobile/app-service-mobile-ios-get-started-push).
- ASP.NET: [use os Hubs de Notificação para enviar notificações por push aos usuários](notification-hubs-aspnet-backend-ios-apple-apns-notification.md).
- SDK do Java dos Hubs de Notificação do Azure: confira [Como usar os Hubs de Notificação por meio do Java](notification-hubs-java-push-notification-tutorial.md) para enviar notificações no Java. Isso foi testado no Eclipse para desenvolvimento no Android.
- PHP: [como usar os Hubs de Notificação no PHP](notification-hubs-php-push-notification-tutorial.md).

## <a name="verify-that-your-app-receives-push-notifications"></a>Verifique se seu aplicativo recebe notificações por push

Para testar as notificações por push no iOS, você deve implantar o aplicativo em um dispositivo iOS físico. Não é possível enviar notificações por push da Apple com o iOS Simulator.

1. Execute o aplicativo, verifique se o registro foi bem-sucedido e pressione **OK**.

   :::image type="content" source="media/ios-sdk/image7.png" alt-text="Registrar":::

2. Em seguida, você pode enviar uma notificação por push de teste do [portal do Azure](https://portal.azure.com/), conforme descrito na seção anterior.

3. A notificação por push é enviada a todos os dispositivos que estão registrados para receber as notificações do hub de notificação fornecido.

   :::image type="content" source="media/ios-sdk/image8.png" alt-text="Enviar teste":::

## <a name="next-steps"></a>Próximas etapas

Neste exemplo simples, você difunde notificações por push a todos os seus dispositivos iOS registrados. Para saber como enviar notificações por push para dispositivos iOS específicos, prossiga para o seguinte tutorial:

[Tutorial: Enviar notificações por push para dispositivos específicos](notification-hubs-ios-xplat-segmented-apns-push-notification.md)

Para obter mais informações, consulte os seguintes artigos:

- [Visão geral dos Hubs de Notificação do Azure](notification-hubs-push-notification-overview.md)
- [APIs REST dos Hubs de Notificação](/rest/api/notificationhubs/)
- [SDK dos Hubs de Notificação para operações de back-end](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/)
- [SDKs dos Hubs de Notificação do GitHub](https://github.com/Azure/azure-notificationhubs)
- [Registrar com o back-end de aplicativo](notification-hubs-ios-aspnet-register-user-from-backend-to-push-notification.md)
- [Gerenciamento de registros](notification-hubs-push-notification-registration-management.md)
- [Como trabalhar com marcas](notification-hubs-tags-segment-push-message.md)
- [Como trabalhar com modelos personalizados](notification-hubs-templates-cross-platform-push-messages.md)
- [Controle de acesso do Barramento de Serviço com assinaturas de acesso compartilhado](../service-bus-messaging/service-bus-sas.md)
- [Gerar tokens SAS programaticamente](/rest/api/eventhub/generate-sas-token)
- [Segurança da Apple: criptografia comum](https://developer.apple.com/security/)
- [Hora da época do UNIX](https://en.wikipedia.org/wiki/Unix_time)
- [HMAC](https://en.wikipedia.org/wiki/HMAC)