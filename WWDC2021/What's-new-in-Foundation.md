# What's new in Foundation

> 🗂WWDC21  | Category : Foundation
> 
> 🔗 [https://developer.apple.com/videos/play/wwdc2021/10109/](https://developer.apple.com/videos/play/wwdc2021/10109/)
>  <br />
>  <br />
> 
> 🔖 Discover how the latest updates to Foundation can help you improve your app's localization and internationalization support. Find out about the new AttributedString, designed specifically for Swift, and learn how you can use Markdown to apply style to your localized strings. Explore the grammar agreement engine, which automatically fixes up localized strings so they match grammatical gender and pluralization. And we'll take you through improvements to date and number formatting that simplify complex requirements while also improving performance.

<br />
<br />  

## AttributedString

- Characters
- Ranges
- Dictinary

Attributed string allow you to associate attributes, which are key-value pairs, to a specific range of a string. The most common attributes are defined by the SDK, but you can also create your own.

![/WWDC2021/images/What's-new-in-Foundation/Untitled.png](/WWDC2021/images/What's-new-in-Foundation/Untitled.png)

![/WWDC2021/images/What's-new-in-Foundation/Untitled%201.png](/WWDC2021/images/What's-new-in-Foundation/Untitled%201.png)

<br />  

### AttributedString

- Value type
- Compatible with String
- same character-counting behavior as Swift String
- Localizable
- Safety and security 
- both compile time safety by using strong typing and also safety during unarchiving using Codable.
- 

![/WWDC2021/images/What's-new-in-Foundation/Untitled%202.png](/WWDC2021/images/What's-new-in-Foundation/Untitled%202.png)

<br />  

### AttributedString Views

- Chracters - which provides addess to the string, and runs, which provides a access to the attributes
- Runs - functions that you are familiar with from types like Array are availble here two.

![/WWDC2021/images/What's-new-in-Foundation/Untitled%203.png](/WWDC2021/images/What's-new-in-Foundation/Untitled%203.png)

![/WWDC2021/images/What's-new-in-Foundation/Untitled%204.png](/WWDC2021/images/What's-new-in-Foundation/Untitled%204.png)

![/WWDC2021/images/What's-new-in-Foundation/Untitled%205.png](/WWDC2021/images/What's-new-in-Foundation/Untitled%205.png)

![/WWDC2021/images/What's-new-in-Foundation/Untitled%206.png](/WWDC2021/images/What's-new-in-Foundation/Untitled%206.png)

Localizable

![/WWDC2021/images/What's-new-in-Foundation/Untitled%207.png](/WWDC2021/images/What's-new-in-Foundation/Untitled%207.png)

![/WWDC2021/images/What's-new-in-Foundation/Untitled%208.png](/WWDC2021/images/What's-new-in-Foundation/Untitled%208.png)

<br />  

### Conversion

- NSAttributedString
- Codable
- Custom attributes

![/WWDC2021/images/What's-new-in-Foundation/Untitled%209.png](/WWDC2021/images/What's-new-in-Foundation/Untitled%209.png)

![/WWDC2021/images/What's-new-in-Foundation/Untitled%2010.png](/WWDC2021/images/What's-new-in-Foundation/Untitled%2010.png)

<br />  

### Attribute Keys

That defines what type of value is requires and a name for archiving

- Declares type of value
- Customizes encoding behavior

![/WWDC2021/images/What's-new-in-Foundation/Untitled%2011.png](/WWDC2021/images/What's-new-in-Foundation/Untitled%2011.png)

![/WWDC2021/images/What's-new-in-Foundation/Untitled%2012.png](/WWDC2021/images/What's-new-in-Foundation/Untitled%2012.png)

![/WWDC2021/images/What's-new-in-Foundation/Untitled%2013.png](/WWDC2021/images/What's-new-in-Foundation/Untitled%2013.png)

<br />  

### Attribute Scopes

- Group of attribute keys
- Defined by each framework or app

![/WWDC2021/images/What's-new-in-Foundation/Untitled%2014.png](/WWDC2021/images/What's-new-in-Foundation/Untitled%2014.png)


<br />  

## Formatters

![/WWDC2021/images/What's-new-in-Foundation/Untitled%2015.png](/WWDC2021/images/What's-new-in-Foundation/Untitled%2015.png)

 

![/WWDC2021/images/What's-new-in-Foundation/Untitled%2016.png](/WWDC2021/images/What's-new-in-Foundation/Untitled%2016.png)

![/WWDC2021/images/What's-new-in-Foundation/Untitled%2017.png](/WWDC2021/images/What's-new-in-Foundation/Untitled%2017.png)

![/WWDC2021/images/What's-new-in-Foundation/Untitled%2018.png](/WWDC2021/images/What's-new-in-Foundation/Untitled%2018.png)

<br />  

### Date formatting

![/WWDC2021/images/What's-new-in-Foundation/Untitled%2019.png](/WWDC2021/images/What's-new-in-Foundation/Untitled%2019.png)

![/WWDC2021/images/What's-new-in-Foundation/Untitled%2020.png](/WWDC2021/images/What's-new-in-Foundation/Untitled%2020.png)

![/WWDC2021/images/What's-new-in-Foundation/Untitled%2021.png](/WWDC2021/images/What's-new-in-Foundation/Untitled%2021.png)

- Fields
- Order does not matter
- Sensible default
- Add fields to customize

![/WWDC2021/images/What's-new-in-Foundation/Untitled%2022.png](/WWDC2021/images/What's-new-in-Foundation/Untitled%2022.png)

![/WWDC2021/images/What's-new-in-Foundation/Untitled%2023.png](/WWDC2021/images/What's-new-in-Foundation/Untitled%2023.png)

![/WWDC2021/images/What's-new-in-Foundation/Untitled%2024.png](/WWDC2021/images/What's-new-in-Foundation/Untitled%2024.png)

<br />  

### Number formatting

![/WWDC2021/images/What's-new-in-Foundation/Untitled%2025.png](/WWDC2021/images/What's-new-in-Foundation/Untitled%2025.png)

![/WWDC2021/images/What's-new-in-Foundation/Untitled%2026.png](/WWDC2021/images/What's-new-in-Foundation/Untitled%2026.png)


<br />  

## Grammar agreement

![/WWDC2021/images/What's-new-in-Foundation/Untitled%2027.png](/WWDC2021/images/What's-new-in-Foundation/Untitled%2027.png)

![/WWDC2021/images/What's-new-in-Foundation/Untitled%2028.png](/WWDC2021/images/What's-new-in-Foundation/Untitled%2028.png)
