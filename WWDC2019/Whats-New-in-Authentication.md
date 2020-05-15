# What's New in Authentication

>  📅 2020.5.15 (FRI)
> 
> WWDC2019 | Session : 516 | Category : Privacy and Security  

🔗 [What's New in Authentication - WWDC 2019 - Videos - Apple Developer](https://developer.apple.com/videos/play/wwdc2019/516/)  

## Sign In with Apple

![](/WWDC2019/images/Introducing-Sign-In-with-Apple/Untitled.png)  


### Sign In with Apple accounts are secure, verified accounts

- Sign in With Apple has strong two-factor authentication that's already used to protect their Apple ID. This two-factor authentication involves the use's circle of trusted devices and strong biometrics.

![](/WWDC2019/images/Introducing-Sign-In-with-Apple/Untitled1.png)

- It is across platform experience, across all of the user's devices.

![](/WWDC2019/images/Introducing-Sign-In-with-Apple/Untitled2.png)

- On app launch, your app can check for an existing password-based account account saved to the iCloud Keychain even before you show your standard login interface so that users do the right thing.

## Password-based authentication

![](/WWDC2019/images/Introducing-Sign-In-with-Apple/Untitled3.png)

Password AutoFill, which makes it quick and easy for users to sign in to your apps on the existing login screens that you already have.

![](/WWDC2019/images/Introducing-Sign-In-with-Apple/Untitled4.png)

🆕 Password AutoFill is available for iPad apps for Mac with an interface that is specifically tailored to the mac.

Once you've brought your app to the Mac, it'll have a new app ID, and that app ID has to be listed on your server in order to tie your pap and your website together. 

- **Web credintials**

![](/WWDC2019/images/Introducing-Sign-In-with-Apple/Untitled5.png)

All you've going to do is add the app ID to this app's array.

- Universal Links

![](/WWDC2019/images/Introducing-Sign-In-with-Apple/Untitled6.png)

🆕 If you're using universal links, you'll add the new app ID here as part of the new app ID's key that can take a list. 

![](/WWDC2019/images/Introducing-Sign-In-with-Apple/Untitled7.png)

The same sign-in experience I showed you earlier in the context of Sign In with Apple, is available to all apps that user password. The same API and functionality is also available on macOS Catalina.

![](/WWDC2019/images/Introducing-Sign-In-with-Apple/Untitled8.png)  

![](/WWDC2019/images/Introducing-Sign-In-with-Apple/Untitled9.png)  

![](/WWDC2019/images/Introducing-Sign-In-with-Apple/Untitled10.png)

You can request multiple accounts at the same time using the `AutorizationController`. And it is super handy if you want to check for both a password-based account and Sign In with Apple right around app launch.

![](/WWDC2019/images/Introducing-Sign-In-with-Apple/Untitled11.png)  

If you do this, you're going to want to make sure that you handle both types of credentials in the `didCompleteWith Authorization` delegate method.

## Warnings for weak passwords

Automatic strong password

![](/WWDC2019/images/Introducing-Sign-In-with-Apple/Untitled12.png)  

![](/WWDC2019/images/Introducing-Sign-In-with-Apple/Untitled13.png)  

![](/WWDC2019/images/Introducing-Sign-In-with-Apple/Untitled14.png)  

![](/WWDC2019/images/Introducing-Sign-In-with-Apple/Untitled15.png)  

🆕 Now, in Safari 13 and in iOS 13, when user signs into a website and Safari notices that the password they just used was weak, Safari will prompt the user to go and visit the website to change the password.

![](/WWDC2019/images/Introducing-Sign-In-with-Apple/Untitled16.png)

(Twitter, Github, [WordPress.com](http://wordpress.com) already adopted it)

All you have to do is put a redirect at this path on your server that takes the user to your Change Password webpage.

> wicg.github.io/change-pawword-url

## OAuth sign-in

![](/WWDC2019/images/Introducing-Sign-In-with-Apple/Untitled17.png)

Once a user confirms that they're OK with signing in, the AuthenticationSession will use existing signed-in accounts through Safari's cookies and data in order to let the user sign-in even faster.

Sometimes the user will already be signed in to the identity provider, and all they have to do is agree to signing into your app. And once they do that, they're in.

![](/WWDC2019/images/Introducing-Sign-In-with-Apple/Untitled18.png)

🆕 This API is now available on macOS Catalina, this API uses the user's preferred web browser for signing in if that web browser supports it. This means that all of your users will have heir browser's password manager or password manager extension available to them in order to help het them signed in. 

![](/WWDC2019/images/Introducing-Sign-In-with-Apple/Untitled19.png)
If your using the AuthenticationSession API, it has a few new features for you to use this year.

**More private sign-in**

Because the AuthenticatioNSession shares website data with Safari, ASWebAuthenticationSession enables a single sign-on experience.

🆕 New to iOS 13, your app can choose to deliver a more private experience, and experience that won't leave users logged into the identity provider in their web browser after they've signed into your app.

🙅🏻‍♀️

![](/WWDC2019/images/Introducing-Sign-In-with-Apple/Untitled20.png)

(they'll be taken directly to the identity provider in order to get signed in)

![](/WWDC2019/images/Introducing-Sign-In-with-Apple/Untitled21.png)  

![](/WWDC2019/images/Introducing-Sign-In-with-Apple/Untitled22.png)

To do this, set the `prefersEphemeralWebBrowserSession` property on the session to true before starting the session. By doing this, you give your users more privacy and avoid that confirmation dialog, which overall might be a better experience for your app.

**Supporting multiple windows**

In iOS 12, the AuthenticationSession API didn't need any information about view or windows from you in order to display its interface. Because the API was iOS only, and almost all apps drew into a single window. 

But now with iPadOS and macOS support.

![](/WWDC2019/images/Introducing-Sign-In-with-Apple/Untitled23.png)

First, you'll give the session a `presentationContextProvider`, and that `presentationContextProvider` will provide a window via the `presentationAnchor` method. 

**Migrating from SFAuthenticationSession**

One more thing about OAuth `ASWebAuthenticationSession` has a deprecated predecessor called `SFAuthenticationSession`. It''s from the Safari Services framework. 

`ASWebAuthenticationSession` has the new features that we just talked about, and it's available on the Mac.

## USB security keys on macOS

This year, macOS supports USB security keys in Safari through the `WebAuthentication` standard.

![](/WWDC2019/images/Introducing-Sign-In-with-Apple/Untitled24.png)

Safari 13 supports USB-based FIDO2-compliant devices with the WebAuthentication standard. It's available as an experimental feature in Seed 1 of macOS Catalina, and it'll be on by default in Seed 2.

→ Safari Technology Preview

![](/WWDC2019/images/Introducing-Sign-In-with-Apple/Untitled25.png)
