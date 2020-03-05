# Introducing Sign In with Apple

>  📅 2020.3.4 (THU)
> 
> WWDC2019 | Session : 706 | Category : Privacy and Security  
  
🔗 [Introducing Sign In with Apple - WWDC 2019 - Videos - Apple Developer](https://developer.apple.com/videos/play/wwdc2019/706/)  

![](/WWDC2019/images/Introducing-Sign-In-with-Apple/Untitled.png)

### Fast, easy account setup and sign in

It's secure and it's private, both for your users and for your privacy.

![](/WWDC2019/images/Introducing-Sign-In-with-Apple/Untitled1.png) 

- The user can make a choice on which email they want to share.
- Your app gets a unique, stable ID, a name and a verified email address.
- Secure two-factor authenticated account

## Sign In with Apple

- Streamlined a count setup
Users downloaded your app from the App Store using Apple ID already. And Sign In with Apple helps them engage fully with just a tap in your app.
- Verified email addresses

    hide my email options enable these users to share a hidden email address that routes to  to their verified email inbox.

    > jinhapark@gmail.com → [r45N934br1@privaterelay.appleid.com](mailto:r45N934br1@privaterelay.appleid.com)

    - Hide My Email
        - Linked to verified email
        - Two-way relay
        - Any email communication
        - Apple does not retain messages
- Built-in security
No password, two-factor authentication, no cost to you and no added friction to the user
- Anti-fraud
    - Privacy friendly
    - On-device intelligence
    - Account information
    - Abstracted to a single bit
- Cross-platform

![](/WWDC2019/images/Introducing-Sign-In-with-Apple/Untitled2.png)

## Integrating with Your App

![](/WWDC2019/images/Introducing-Sign-In-with-Apple/Untitled3.png)

### Button

``` Swift
    func setUpProviderLoginView() {
    	let button = ASAutoriationAppleIDButton() 
    	button.addTarget(self, action #selector(handleAuthorizationAppleIDButtonPress, for: .touchUpInside)
    	self.loginProviderStackView.addArrangedSubview(button)
    }
```

### Authorization
``` Swift
    @objc func handleAuthorizationButtonPress() {
    	let request = ASAuthorizationAppleIDProvider().createRequest()
    	request.requestedScopes = [.fullName, .email]
    
    	let controller = ASAuthorizationController(authorizationRequests: [request])
    
    	controller.delegate = self
    	controller.presentationContextProvider = self
    
    	controller.performRequests()
    }
```  
Optionally, if your ares this for the best user experience, you can set requestedScopes for full name and email. You should only request this information if it's truly required for your app and err on the side of minimum amount information.
``` Swift
    func autorizationController(controller _: ASAutorizationController, didCompleteWithAuthorization authorization: ASAutorization) {
    	if let credential = authorization.credential as? ASAuthorizationAppleIDCredential {
    		let userIdentifier = crendtial.user
    		let identityToken = credintial.identifyToken
    		let authCode = crendtial.authorizationCode
    		let realUserStatus = crential.realUserStatus
    
    		// Create account in your system
    		}
    }
    
    func authorizationController(_: ASAuthorizationController, didCompletedWithError error: Error) {
    	// Handle error
    } 
```
**Both of these delegate callbacks are guaranteed to be made on your app's main queue**.

## Sign In

> Response

**User ID**

- Unique, stable, team-scoped user ID

    → You can use it to retrieve information from your user systems across different platforms, different systems, the web, Android. It remains stable across all of them.

**Verification data**

- Identity token, code

    → A short-lived token that you can use with Apple ID servers to exchanges for a refresh token. 

**Account information**

- Name, verified email

**Real user indicator**

- High confidence indicator that likely real user

### Handle Session Changes

![](/WWDC2019/images/Introducing-Sign-In-with-Apple/Untitled4.png)

``` Swift
    let provider = ASAuthorizationAppleIDProvider() 
    
    provider.getCredentialState(forUserID: "currnetUserIdentifier") { (credentialState, error) in
    	switch credentialState {
    	case .authorized:
    		// Apple ID Credential is valid
    	case .revoked:
    		// Apple ID Credintial revokec, handle unlink
    	case .notFound:
    		// Credential not found, show login UI
    	defualt: break
    	}
    }
```

This API is very fast. You should call it on your app's launch to make sure that you provide a tailored experience for each of these states.

![](/WWDC2019/images/Introducing-Sign-In-with-Apple/Untitled5.png)

We expose a notification through NotificationCenter to let you know when this credential sate changed to revoked.

![](/WWDC2019/images/Introducing-Sign-In-with-Apple/Untitled6.png)

```Swift
    // Prompts the user if an existing iCloud Keychain credential or Apple ID credential exists
    func performExistingAccountSetupFlows() {
    	// Prepare requests for both Apple ID and password providers/
    	let request = [ASAuthorizationAppleIDProvider().createRequest(),
    							  ASAuthorizationPasswordProvider().createRequest()]
    
    	// Create an authorization controller iwth the given request/
    	let authorizationController = ASAuthorizationController(authorizationRequests: requests)
    	authorizationController.delegate = self
    	authorizationController.presentationContextProvider = self
    	authorizationController.performRequests()
    }
```
    	
```Swift
    func authorizationController(controller: _: ASAuthorizationController, didCompleteWithAuthorization authorization: ASAuthorization) {
    	switch autorization.credential {
    	case let credential as ASAuthorizationAppleIDCredential:
    		let userIdentifier = crendtial.user
    		// Sign the user in using the Apple ID credential
    	case let credential as ASPasswordCredential:
    		// Sign the user in using their exisiting password credential
    	default: break
    	}
    }
```
  
## Cross-Platform

![](/WWDC2019/images/Introducing-Sign-In-with-Apple/Untitled7.png)

![](/WWDC2019/images/Introducing-Sign-In-with-Apple/Untitled8.png)

### JavaScript SDK

- Include
```
    <script src="https://appleid.cdn-apple.com/appleauth/static/ jsapi/appleid/1/en_US/appleid.auth.js">
```
- Button
```
    <div id="appleid-signin"></div>
```
- Configure
```
    AppleID.auth.init({
    	clientId : 'com.example.webapp',
    	scope : 'name email',
    	redirectURI: 'https://example.com/redirectUri', state : 'state'
    });
```
- Result
```
    POST /redirectUri
```

![](/WWDC2019/images/Introducing-Sign-In-with-Apple/Untitled9.png)

![](/WWDC2019/images/Introducing-Sign-In-with-Apple/Untitled10.png)

[Implementing User Authentication with Sign in with Apple](https://developer.apple.com/documentation/authenticationservices/implementing_user_authentication_with_sign_in_with_apple)
