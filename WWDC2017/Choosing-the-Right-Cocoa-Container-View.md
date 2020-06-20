# Choosing the Right Cocoa Container View

> 📅 2020.6.20 (SAT)
> 
> 🗂WWDC 2017 | Session : 218 | Category : App Frameworks
>
> 🔗 [https://developer.apple.com/videos/play/wwdc2017/218/](https://developer.apple.com/videos/play/wwdc2017/218/)
>  <br />
>  <br />
> 🔖 AppKit offers numerous ways to easily present your data. Join our framework engineers for a guided tour of versatile standard view classes you can put to work in your own macOS apps. Hear about NSStackView, NSTableView, NSCollectionView, and other container views in AppKit. Explore the interesting features and benefits of each, and examine real-world use cases to help you choose the most suitable building blocks for your apps' user interfaces.
> <br />
<br />  

![/WWDC2017/images/Choosing-the-Right-Cocoa-Container-View/Untitled.png](/WWDC2017/images/Choosing-the-Right-Cocoa-Container-View/Untitled.png)

![/WWDC2017/images/Choosing-the-Right-Cocoa-Container-View/Untitled%201.png](/WWDC2017/images/Choosing-the-Right-Cocoa-Container-View/Untitled%201.png)

### Agenda

Container view tour

- NSStackView, NSGridView
- NSTableView, NSOutlineView, NSBrowser, NSCollectionView

Choosing the right container view

Case studies

### **What if We Want to Build a More Interactive Grid?**

## Container Views

### NSStackView

- Flows views in a single column or row
- Configurable edge insets
- Configurable alignment
- Optionally fill cross dimension
- Your Auto Layout constraints set heights
- Configurable spacing
- Adapts on resize
- Can automatically drop low-priority views
- Gravity areas (sections)
- Easier UI layout localization

![/WWDC2017/images/Choosing-the-Right-Cocoa-Container-View/Untitled%202.png](/WWDC2017/images/Choosing-the-Right-Cocoa-Container-View/Untitled%202.png)

### NSGridView

![/WWDC2017/images/Choosing-the-Right-Cocoa-Container-View/Untitled%203.png](/WWDC2017/images/Choosing-the-Right-Cocoa-Container-View/Untitled%203.png)

- Coordinated rows and colums
- Align corresponding views
- Simple control over size, alignment, and spacing
- Automatically adapts for right-to-left languages

### NSTableView

![/WWDC2017/images/Choosing-the-Right-Cocoa-Container-View/Untitled%204.png](/WWDC2017/images/Choosing-the-Right-Cocoa-Container-View/Untitled%204.png)

- Interactive, vertical list view
- Can be single-column
- Arbitrary item content
- Type-selection, keyboard navigation
- Group rows
- Variable row height
- Row actions, change animations
- Scalable to large item counts
- Populate using Bindings, or a row-index-based data source
- Versatile, easy way to present tabular data or lists

### **What About Trees? 🌲**

### NSOutlineView

![/WWDC2017/images/Choosing-the-Right-Cocoa-Container-View/Untitled%205.png](/WWDC2017/images/Choosing-the-Right-Cocoa-Container-View/Untitled%205.png)

- All the capabilities of NStableView, plus:
    - A hierarchical data model
    - Tracks model objects and their children
    - Expandable/collapsable container nodes
    - User can explore multiple branches simultaneously

    ### NSBrowser

    ![/WWDC2017/images/Choosing-the-Right-Cocoa-Container-View/Untitled%206.png](/WWDC2017/images/Choosing-the-Right-Cocoa-Container-View/Untitled%206.png)

    ![/WWDC2017/images/Choosing-the-Right-Cocoa-Container-View/Untitled%207.png](/WWDC2017/images/Choosing-the-Right-Cocoa-Container-View/Untitled%207.png)

    - Column-based "drill-down" UI
    - User-sizable columns
    - Optional custom header views
    - Optional custom preview ViewController
    - User can explore one branch at a time

    ### What About Custom Layouts?

    ## NSCollectionView

    > A customizable, scalable data view

    - Arbitrary, developer-defined layouts
    - Easy, wrapped layout
    - Optional sections, header and footer views
    - Item selection, drag and drop
    - Arbitrary item representation (view subtree)
    - Scalable, sparing item instantiation and reuse
    - Can animate inserts/deletes/moves, transitions

![/WWDC2017/images/Choosing-the-Right-Cocoa-Container-View/Untitled%208.png](/WWDC2017/images/Choosing-the-Right-Cocoa-Container-View/Untitled%208.png)

![/WWDC2017/images/Choosing-the-Right-Cocoa-Container-View/Untitled%209.png](/WWDC2017/images/Choosing-the-Right-Cocoa-Container-View/Untitled%209.png)

![/WWDC2017/images/Choosing-the-Right-Cocoa-Container-View/Untitled%2010.png](/WWDC2017/images/Choosing-the-Right-Cocoa-Container-View/Untitled%2010.png)

## Case Studies

### Mail: Message List

![/WWDC2017/images/Choosing-the-Right-Cocoa-Container-View/Untitled%2011.png](/WWDC2017/images/Choosing-the-Right-Cocoa-Container-View/Untitled%2011.png)

- Unbounded umber of emails
→ ~~NSStackView~~ ~~NSGridView~~
- Selectable
- Lots of properties: sender, subject, etc...
→ ~~NSCollectionView~~ ~~NSBrowser~~
- Critique use of space

**→ NSOutlineView ?**

- I can't even view the entire message here without scrolling
- I can just add more space by changing the split view, but then I would subtract space away from the OutlineView and my list of messages

![/WWDC2017/images/Choosing-the-Right-Cocoa-Container-View/Untitled%2012.png](/WWDC2017/images/Choosing-the-Right-Cocoa-Container-View/Untitled%2012.png)

**→ NSTableView**

- Now as the user can read more email message with less scrolling,
- Since we have now a vertical split, no matter how the user changes the size of the split view, I'll always be able to see the same number of emails in the column list

![/WWDC2017/images/Choosing-the-Right-Cocoa-Container-View/Untitled%2013.png](/WWDC2017/images/Choosing-the-Right-Cocoa-Container-View/Untitled%2013.png)

### Preview: Sidebar

![/WWDC2017/images/Choosing-the-Right-Cocoa-Container-View/Untitled%2014.png](/WWDC2017/images/Choosing-the-Right-Cocoa-Container-View/Untitled%2014.png)

- Large number of pages
- Selectable
- Reorderable
- Vertical layout

→ ~~NSStackView~~ ~~NSGridView~~ ~~NSBrowser~~

- Need collapsable categories

![/WWDC2017/images/Choosing-the-Right-Cocoa-Container-View/Untitled%2015.png](/WWDC2017/images/Choosing-the-Right-Cocoa-Container-View/Untitled%2015.png)

Reusable data source → **NSCollectionView**

### Internet Accounts: Account Types

![/WWDC2017/images/Choosing-the-Right-Cocoa-Container-View/Untitled%2016.png](/WWDC2017/images/Choosing-the-Right-Cocoa-Container-View/Untitled%2016.png)

- Small number of account types
- One dimensional, vertical layout

→ ~~NSGridView~~ ~~NSCollectionView~~ ~~NSOutlineView~~ ~~NSBrowser~~

- No selection: each item is a button
→ ~~NSTableView~~

But **NSTableView**

- Historical
- Dynamic data

### Terminal: New Remote connection

![/WWDC2017/images/Choosing-the-Right-Cocoa-Container-View/Untitled%2017.png](/WWDC2017/images/Choosing-the-Right-Cocoa-Container-View/Untitled%2017.png)

- Size and shape of UI
- Vertical space versus horizontal space
- Interactive division

![/WWDC2017/images/Choosing-the-Right-Cocoa-Container-View/Untitled%2018.png](/WWDC2017/images/Choosing-the-Right-Cocoa-Container-View/Untitled%2018.png)

![/WWDC2017/images/Choosing-the-Right-Cocoa-Container-View/Untitled%2019.png](/WWDC2017/images/Choosing-the-Right-Cocoa-Container-View/Untitled%2019.png)

![/WWDC2017/images/Choosing-the-Right-Cocoa-Container-View/Untitled%2020.png](/WWDC2017/images/Choosing-the-Right-Cocoa-Container-View/Untitled%2020.png)

![/WWDC2017/images/Choosing-the-Right-Cocoa-Container-View/Untitled%2021.png](/WWDC2017/images/Choosing-the-Right-Cocoa-Container-View/Untitled%2021.png)
