# [ARCHIVED] Discovery SDK for iOS (Preview)

> [!IMPORTANT]
> This preview SDK has been deprecated and this repo is no longer being maintained. We recommend that you use [Microsoft Graph](https://graph.microsoft.com/) and the associated [Microsoft Graph SDKs](https://developer.microsoft.com/en-us/graph/code-samples-and-sdks) instead.

> [!NOTE]
> Security vulnerabilities may exist in the project, or its dependencies. If you plan to reuse or run any code from this repo, be sure to perform appropriate security checks on the code or dependencies first. Do not use this project as the starting point of a production Office Add-in. Always start your production code by using the Office/SharePoint development workload in Visual Studio, or the [Yeoman generator for Office Add-ins](https://github.com/OfficeDev/generator-office), and follow security best practices as you develop the add-in.

Easily integrate services and data from Discovery into native iOS apps using this Objective-C library.

---

Information about official Microsoft support is available  [here][support-placeholder].

[support-placeholder]: https://support.microsoft.com/

---

This library is generated from the Discovery API metadata using [Vipr] and [Vipr-T4TemplateWriter] and uses a [shared client stack][orc-for-ios].

[Vipr]: https://github.com/microsoft/vipr
[Vipr-T4TemplateWriter]: https://github.com/msopentech/vipr-t4templatewriter
[orc-for-ios]: https://github.com/msopentech/orc-for-ios

## Quick Start

To use this library in your project, follow these general steps, as described further below:

1. Configure a [Podfile].
2. Set up authentication.
3. Construct an API client.

[Podfile]: https://guides.cocoapods.org/syntax/podfile.html

### Setup

1. Create a new Xcode application project from the Xcode splash screen. In the dialog, choose iOS > Single View Application. Name your application as you wish; we'll assume the name *DiscoveryQuickStart* here.

2. Add a file to the project. Choose iOS > Other > Empty from the dialog and name your file `Podfile`.

3. Add these lines to the Podfile to import the Discovery SDK

 ```ruby
 source 'https://github.com/CocoaPods/Specs.git'
 xcodeproj 'DiscoveryQuickStart'
 pod 'MSOffice365-Discovery-SDK-iOS'
 ```

 > NOTE: For detailed information on Cocoapods and best practices for Podfiles, read the [Using Cocoapods] guide.

4. Close the Xcode project.

5. From the command line, change to your project's directory. Then run `pod install`.

 > NOTE: Install Cocoapods first of course. Instructions [here](https://guides.cocoapods.org/using/getting-started.html).

6. From the same location in the terminal, execute `open DiscoveryQuickStart.xcworkspace` to open a workspace containing your original project together with imported pods in Xcode.

---

### Authenticate and construct client

With your project prepared, the next step is to initialize the dependency manager and an API client.

:exclamation: If you haven't yet registered your app in Azure AD, you'll need to do so before completing this step by following [these instructions][MSDN Add Common Consent].

1. Right-click the DiscoveryQuickStart folder and choose "New File." In the dialog, select *iOS* > *Resource* > *Property List*. Name the file `adal_settings.plist`. Add the following keys to the list and set their values to those from your app registration. **These are just examples; be sure to use your own values.**

 |Key|Value|
 |---|-----|
 |ClientId|Example: e59f95f8-7957-4c2e-8922-c1f27e1f14e0|
 |RedirectUri|Example: https://my.client.app/|
 |ResourceId|Example: https://api.office.com/discovery|
 |AuthorityUrl|https://login.microsoftonline.com/common/|

2. Open ViewController.m from the DiscoveryQuickStart folder. Add the umbrella header for Discovery and ADAL related headers.

 ```objective-c
 #import <MSDiscovery.h>
 #import <impl/ADALDependencyResolver.h>
 #import <ADAuthenticationResult.h>
 ```

3. Add properties for the ADALDependencyResolver and MSDiscoveryClient in the class extension section of ViewController.m.

 ```objective-c
 @interface ViewController ()
 
 @property (strong, nonatomic) ADALDependencyResolver *resolver;
 @property (strong, nonatomic) MSDiscoveryClient *discoveryClient;
 
 @end
 ```

4. Initialize the resolver and client within the viewDidLoad method of the ViewController.m file.

 ```objective-c
 - (void)viewDidLoad {
     [super viewDidLoad];
     
    self.resolver = [[ADALDependencyResolver alloc] initWithPlist];
    
    self.discoveryClient = [[MSDiscoveryClient alloc] initWithUrl:@"https://api.office.com/discovery/v2.0/me" dependencyResolver:self.resolver];
    }
 ```

5. Before using the client, you must ensure the user has been logged on interactively at least once. You can use either `interactiveLogon` or `interactiveLogonWithCallback:` to initiate the logon sequence. In this exercise, add the following to the viewDidLoad method from the last step:

 ```objective-c
 [self.resolver interactiveLogonWithCallback:^(ADAuthenticationResult *result) {
     if (result.status == AD_SUCCEEDED) {
         [self.resolver.logger logMessage:@"Connected." withLevel:LOG_LEVEL_INFO];
     } else {
         [self.resolver.logger logMessage:@"Authentication failed." withLevel:LOG_LEVEL_ERROR];
     }
 }];
 ```

6. Now you can safely use the API client.

[Using Cocoapods]: https://guides.cocoapods.org/using/using-cocoapods.html
[MSDN Add Common Consent]: https://msdn.microsoft.com/en-us/office/office365/howto/add-common-consent-manually

## Samples
- [O365-iOS-Connect] - Getting started and authentication <br />
- [O365-iOS-Snippets] - API requests and responses

[O365-iOS-Connect]: https://github.com/OfficeDev/O365-iOS-Connect
[O365-iOS-Snippets]: https://github.com/OfficeDev/O365-iOS-Snippets

## Contributing
You will need to sign a [Contributor License Agreement](https://cla2.msopentech.com/) before submitting your pull request. To complete the Contributor License Agreement (CLA), you will need to submit a request via the form and then electronically sign the Contributor License Agreement when you receive the email containing the link to the document. This needs to only be done once for any Microsoft Open Technologies OSS project.

This project has adopted the [Microsoft Open Source Code of Conduct](https://opensource.microsoft.com/codeofconduct/). For more information, see the [Code of Conduct FAQ](https://opensource.microsoft.com/codeofconduct/faq/) or contact [opencode@microsoft.com](mailto:opencode@microsoft.com) with any additional questions or comments.

## License
Copyright (c) Microsoft, Inc. All rights reserved. Licensed under the Apache License, Version 2.0.
