# Customized Loading in WKWebView
  
>  📅 2020.5.17 (SUN)
>
> WWDC 2017 | Session :220 | Category : App Frameworks
  
🔗 [Customized Loading in WKWebView - WWDC 2017 - Videos - Apple Developer](https://developer.apple.com/videos/play/wwdc2017/220/)  

![](/WWDC2017/images/Customized-Loading-in-WKWebView/Untitled.png)   


### **SafariViewController**

- in app web browsing experience
- Using just a few lines of code, you can integrate a powerful secure web browser
- Don't even need to worry about UI

![](/WWDC2017/images/Customized-Loading-in-WKWebView/Untitled1.png)  

### WKWebView

**Architecture** 

![](/WWDC2017/images/Customized-Loading-in-WKWebView/Untitled2.png)  

- `WKWebView` isolates your application from complexity as best we know how.

    → Load the web content and render it and execute JavaScript, in a separate process from our application's process. This gives us some fantastic security benefits.

- Protect your application from potentially malicious web content.
- Different bits of web content can each run in their own web process.
→ Protect your trusted web content from other potentially malicious web content
- Performance benefits
→ Web content can be run concurrently with your application because of the security, we can enable the advances JavaScript Just-In-Time compiler

But! because all this is happening in a separate process, the normal steps you would take to configure your process don't apply to the web content. We need explicit APIs to interact with the web content.

![](/WWDC2017/images/Customized-Loading-in-WKWebView/Untitled3.png)  

## Manage cookies

![](/WWDC2017/images/Customized-Loading-in-WKWebView/Untitled4.png)  

When a webpage is rendered in a browser engine, a lot of sub resources come up. (Images, JavaScripts files, style sheets)

And both the requests and the responses include little bits of data called cookies.

This helps track a user's session when using a web application. Things like their log in credentials.

Why you might need to manage cookies which is the opposite of helping the session along.

→ You might need to protect the user from having a session tracked for certain types of application, users

### 🆕 WKHTTPCookieStore

![](/WWDC2017/images/Customized-Loading-in-WKWebView/Untitled5.png)  

**Get to a `WKWebViews` cookie store though its `websiteDataStore`**

```swift
open class WKWebsiteDataStore: NSObject, NSCoding {
	open var httpCookiesStore: WKHTTPCookieStore { get }
}

let cookieStore = webView.configuration.websiteDataStore.httpCookieStore
```

**Add a cookie**

```swift
let cookie = HTTPCookie(properties: [
	HTTPCookiePropertyKey.domain: "canineschool.org", 
	HTTPCookiePropertyKey.path: "/",
	HTTPCookiePropertyKey.secure: true,
	HTTPCookiePropertyKey.name: "LoginSessionID", 
	HTTPCookiePropertyKey.value: "5bd9d8cabc46041579a311230539b8d1"])

cookieStore.setCookie(cookie!) {
	webView.load(loggedInURLRequest)
}	
```

`setCookie` 

→ This process is asynchronous  and it needs to send out to all those processes involved in the `WKWebView` mechanism that are isolated from you application. So you need to wail until `WebKit` decides it's all done and ready to go. 

**Retrieve the set of all cookies in a `WKWebsiteDataStore`**

```swift
cookieStore.getAllCookies() { (cookies) in
	for cookie in cookies {
		// Find the login cookie
	}
}
```

**Delete a cookie**

```swift
cookieStore.delete(cookie!) {
	webView.load(loggedOutURLRequest) 
}
```

## Filter unwanted content

### 🆕 WKContentRuleList

![](/WWDC2017/images/Customized-Loading-in-WKWebView/Untitled6.png)  

**You supply rules in JSON**

```swift
[{ 
	"trigger": {
		"url-filter": ".*" 
	},
	"action": {
		"type": "make-https"
	} 
}]
```

All urls take the action of making the url https. 

→ plain text request to e encrypted ones

**Compile your JSON using `WKContentRuleListStore`**

```swift
let jsonString = loadJSONFromBundle()

WKContentRuleListStore.default().compileContentRuleList (
	forIdentifier: "ContnetBlockingRules",
	encodedContentRuleList: jsonString) { (contentRuleList, error) in
		if let error = error {
			return
		}

	createWebViewWithContentRuleList(ruleList!)
}
```

**Access previously compiled `WKContentRuleList`**

```swift
WKContentRuleListStore.default().lookUpContentRuleList(forIdentifier: "ContentBlockingRules") {
(contentRuleList, error) in
	// User previously compiled content rule list
}	
	
```

**Add compiled `WKWebContentRuleList` to your `WKWebView`'s configuration**

```swift
let configuration = WKWebViewConfiguration()
configuration.userContentController.add(contentRuleList)
```

## Provide custom resources

### 🆕 WKURLSchemeHandler

![](/WWDC2017/images/Customized-Loading-in-WKWebView/Untitled7.png)  

> What is URL scheme?

[https://www.apple.com](https://www.apple.com) ⇒ https

[file://Users/Jinha/demo.txt](file://users/Jinha/demo.txt) =? file

mailto:j.appleseed@icluod.com ⇒ mailto

**Simple protocol you implement**

```swift
class MyCustomSchemeHandler: NSObject, WKURLSchemeHandler {
	func webView(_ webView: WKWebView, start urlSchemeTask: WKURLSchemeTask) {
	}

	func webView(_ webView: WKWebView, stop urlSchemTask: WKURLSchemeTask) {
	}
}
```

**Set scheme handlers on the `WKWebViewConfiguration`**

```swift
let configuration = WKWebViewConfiguration()
configuration.setURLSchemeHandler(MYCustomSchemeHandler(), forURLScheme: "apple-local")
```

**Load something in your view that uses your scheme**

```swift
let configuration = WKWebViewConfiguration()
configuration.setURLSchemeHandler(MYCustomSchemeHandler(), forURLScheme: "apple-local")

let webView = WKWebView(frame: getFrame(), configuration: configuration)
webView.load(URLRequest(url: URL(string: "http://example.com/demoContent")!)
```

**The `WKURLSchemeTask` sent to your handler represents a resource load**

```swift
protocol WKURLSchemeTask: NSObjectProtocol {
	var request: URLRequest

	func didReceive(_ reponse: URLResponse)
	func didReceive(_ data: Data)
	func didFinish()
	func didFailWithError(_ error: Error)
}
```

```swift
func webView(_ webView: WKWebView, start urlSchemeTask: WKURLSchemeTask) {
	// 1
	let resourceData = createHTMLResourceData()

	// 2
	let response = URLResponse (
		url: urlSchemeTask.response.url!,
		mimeType: "text/html",
		expectedContentLenght: resourceData.count,
		textEncodingName: nil)

	// 3
	ulrSchemeTask.didReceive(response)
	ulrSchemeTask.didReceive(resourceData)
	//4
	ulrSchemeTask.didFinish()
}

```

1. **You first prepare a response**
2. The response must include the MIME type
3. Deliver the response
4. Signal completion

**Demo**

```Swift
class MyCustomSchemeHandler: NSObject, WKURLSchemeHandler {
    func webView(_ webView: WKWebView, start urlSchemeTask: WKURLSchemeTask) {
        task = urlSchemeTask
        let picker = UIImagePickerController()
        picker.delegate = self
        picker.sourchType = .photoLibray
        controller!.present(picker, animated: true)
    }

    func webView(_ webView: WKWebView, stop urlSchemTask: WKURLSchemeTask) {
        task = nil
    }
}
```

```swift
func imagePickerController(_ picker: UIImagePickerController, didFinishPickingMediaWithInfo info [String: Any]) {
    controller!.dismiss(animgated: true)
    guard let task = task else { return }
    let image = info[UIImagePickerControllerOriginalImage] as! UIImage
    let data = UIImageJPEGRepresentation(image, 1.0)!

    task.didReceive(URLResponse(url: task.request.url!, mimType: "image/jpeg", 
                                    expectedContentLength: data.count, textEncodingName: nil)
    task.didReceive(data)
    task.didFinish()
    }
}
```

[]()
