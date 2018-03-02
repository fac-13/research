# Accessibility Team

**Accessibility = ensuring that all users are able to experience your product in a way that works for them.**

_Note_ General Accessibility Definition:

Inaccessibility is present whenever any user, using any device, is unable to take advantage of the information you are offering.

We should consider all of the following possibilities:

-   User agents without Javascript, CSS, or rich media formats (Flash, Java)
    
    Includes some screen readers, search engine spiders, the Lynx browser, and any modern browser where the user has made this choice.
    
-   Users with difficulty reading text
    
    Can include dyslexic or autistic visitors, sometimes deaf visitors, and, always, blind visitors.
    
-   Users with difficulty seeing low-contrast colors or certain color contrasts
    
    Color blindness causes a number of contrast problems, as do many other factors as the [eye ages](http://www.lighthouse.org/eye-health/the-basics-of-the-eye/the-aging-eye/).
    
-   Users with problems seeing small type
    
    Very common amongst older users or users with degenerative sight syndromes such as [Macular Degeneration](http://en.wikipedia.org/wiki/Macular_degeneration).
    
-   Conflicts with user agent defaults
    
    Use of defined accessibility features such as `tabindex` or `accesskeys`can create conflicts with the normal behavior of a visitor’s browser.


**4 key areas of accessibility**: 
- **keyboard accessible navigation** (navigating navbar without mouse);
- **Touch device capable** (e.g. click VS mouseover --> if you have dropdown menus that are triggered by hovering over an anchor element, many users will never be able to access those secondary menu items.
)
- **Mobile users with smaller screens**
- **Assistive technology friendly**



## How to write an accessible Navigation bar

### 1. Allowing the user to skip to main or desired content quickly

Joe Dolson [suggests](https://www.joedolson.com/2013/07/designing-accessible-navigation/) deploying two page navigations:
1. the first navigation menu (id = "maincontent" in example below) should provide imediate internal access to the main areas of the page itself (Content, Navigation, Footer, e.g.) - and this should be the first entry after the opening body tag. It is then the first set of links that a screen reader will encounter and will allow a screen reader user to quickly access the general areas of the page that they require, including, importantly, the site or main navigation area.


2. the second navigation menu (id = "navigation" in example below) can be the main navigation for the site and, being linked to by the top level page nav, can be placed anywhere the designer likes within the page layout (though obviously the convention is to place the main navigation in the header).

__Example 1.__

```
<body>  
<a href="#maincontent">Skip to main content</a>   
...  
**<main id="maincontent">**  
<h1>Heading</h1>  
<p>This is the first paragraph</p>

```

__Example 2.__

```
<body>
<ul id="skiplinks">  
<li><a href="#content">To Content</a></li>  
<li><a href="#navigation">To Navigation</a></li>  
<li><a href="#footer">To Footer</a></li>  
</ul>
... CONTENT HERE ...
<ul id="navigation">  
<li><a href="/">Home</a></li>  
<li><a href="/news.php">News</a></li>  
<li><a href="/bio.php">Biography</a></li>  
</ul>
... CONTENT HERE ...
</body>
```

Once this double navigation is in place for every page, it would be possible if desired to hide the top level internal page nav from the visual browser user (but not the screen reader user) by setting `#skiplinks`to `{display: none;}` in the stylesheet. But if this strategy were implemented for visual design reasons (e.g. the `#skiplinks` may be hard to style appropriately or may break the designer's preferred layout) the reccomended approach is to ensure that the hidden menu becomes visible as soonas it receives keyboard focus. See the [WebAIM](https://webaim.org/) site.



### 2. Enabling the use of keyboard shortcuts to access different sections of website
The **accesskey attribute** allows web developers to assign keyboard shortcuts to HTML elements, so that the user can jump directly to the desired page/section.

EXAMPLE 

```
`<a href="http://webaim.org/" ****accesskey="w"****>WebAIM.org</a>  
<form action="submit.htm" method="post">  
<label for="name">Name</label>  
<input type="text" id="name" **accesskey="n"**>  
<input type="submit" id="submitform" **accesskey="s"** value="Submit">  
</form>`

```

Although, unfortunately accesskeys are still poorly implemented,as  there are still no common standards the keyboard shortcuts of browsers, operating systems, browser extensions, etc.


### 3. Making the navigation visually accessible 

Now, further styling decisions can be made to account for users with varying degrees of visual ability...

The most important issues in thinking of visual accessiblity for the navigation are:
- Colour Contrast
- Underlining or other visual cues
- Font, Link and Button Size etc.

1. **Colour contrast** should be such that even users with reduced visual acuity can distinguish text and shape etc. adequately.  Moreover, there are colour blind users who will be unable to make required distinctions when certain colour combinations are deployed. As such, colour should not be the exclusive means of conveying important information - brigntess and outline could be used instead, e.g. Colour schemes can be tested for these faults with various online tools - e.g. the [Colourblind Webpage Filter](https://www.toptal.com/designers/colorfilter). For best colour contrast tools see Lea Verou's [Contrast Ratio Test](https://leaverou.github.io/contrast-ratio/) or the [Accessibility DevTools extension](https://chrome.google.com/webstore/detail/accessibility-developer-t/fpkknkljclfencbdbgkenhalefipecmb) for Chrome. Recommended minimum contrast ration is 4.5:1.  
It is essential to remember to set also the `:hover`, `:active`, `:focus`, `:visited` etc. states on the link to an appropriate contrast and colour combination.

2. The convention with **underlining** is that if the menu is sufficiently distinguished from the general content then the underlining cue for nav links is not necessary - but this is a grey area...

3. **Font, Link, Button Size** : it is crucial to use the proper **viewport meta tag** and **media queries** to ensure that all text is easily readable and that links and buttons are easily clickable. **Buttons or links should be large enough, and have enough space around them**, to make them easy to press without accidentally overlapping onto other elements. In addition, the user should be able to adjust their browser settings themselves. As such, we should use `em` or `rem` or other relative sizing method (old versions of IE block user resizing if the font-size is specified in `px` apparrently).

It is a good idea to build in user control to the site style directly, allowing for the user to choose between a range of specially designed and pre-coded font style and size combinations and contrast ranges (dark, light, night, high-contrast, sepia etc.). If none of these designer specified combinations is satisfactory through, the user must be able to choose their own preferences from within the browser.

Example:

see the [City of Manchester](http://www.manchester.gov.uk/) website...


### 4. Making the navbar accessible even without the use of a mouse

It is improtant to make sure to use _device independent_ event handlers for designing menus and that all links are accessible via a click event.

For instance, `onMouseOver`, `onMouseOut`, and `onDblClick` rely upon the use of a mouse. Whereas event handlers such as `onClick` can be triggered by any device.


### 5. Using semantic HTML5 and ARIA labels
Semantic markup conveys the menu structure to users. The menu should be structured using list elements and it should be identified semantically by the use of the HTML5 `<nav>` tag.

Elements should be assigned a `role` and/or an `aria-label` attribute, for example:

```
<nav>
    <ul role="menubar">
        <li role="none">
            <a role="menuitem">Menu Entry 1</a>
        </li>
        <li>
            <a>Menu Entry 2</a>
        </li>
        <li>
            <a>Menu Entry 3</a>
        </li>
    </ul>
<nav>

```


```
<nav aria-labelledby="mainmenulabel">  
    <h2 id="mainmenulabel" class="visuallyhidden">Main Menu</h2>
</nav>
```





## Modal Team!!

(Hint: think about semantic HTML, managing focus, adding key handlers, adding aria attributes)


#### *What is a modal?*
A modal is a dialog box/popup window that is displayed on top of the current page. When the modal is open, the rest of the page is inaccessible. 




key for good modal:
* tab trap (not going behind the modal when it has popped up, lose track of where you are)
* The dialog fills 100% of the screen and the background window is hidden - this helps minimise background movement and enhances readability 
* When the modal is closed focus should return to the previously focused element (e.g. the element that opened the modal)
* Aria is used to express meaning where html can't.
* semantic (add meaning to tags so visually impared users can know what is on the screen)

```
role="dialog"
This identifies the element that serves as the dialog container 

attribute: aria-labelledby="IDREF"
Gives the dialog an acessible name by referring to the element that provides the dialog title 

attribute: aria-modal="true"
Tells assistive technologies that the windows underneath the current dialog are not available for current interaction  
```
```
<li tabindex="0" class="checkbox" checked>promo</li>
```
```
<li tabindex="0" class="checkbox" role="checkbox" checked aria-checked="true">promo</li>
```

another example
```
<a href="#" role="button" aria-label="DeleteItem1">Delete</a>
```
#### *Keyboard support*
Tab - to move the focus to the the next focusable element inside the dialog. It's important that when that last focusable element is reached, pressing tab moves the focus back to the first element (wrap around)

shift tab - Moves the focus to the previous focusable element inside the dialog. Again, it is important there is wrap around.

Escape - closes the dialog box 

#### *Giving focus to the modal*
When an item has keyboard "focus", it can be activated or manipulated with the keyboard e.g. using the tab key to navigate through a page. 


A modal is not one of the natively focusable HTML elements and so it will not receive focus by default. Instead focus must be programatically given to the modal. 

This can be achieved by using: 
* tabindex attribute
    * indicates if an element can be focused, and if/where it participates in sequential keyboard navigation (i.e. the taborder). 
    * can have 3 values - 
       * 0, a postive integer or -1
       * -1 - allows the element to receive focus programatically 
       * The modal should have a tabindex of -1 since we want it to be focusable but only when we decide
       
* .focus()
    * Setting focus will cause the screen reader to recognize the change of context and detect that focus is now inside a dialog
    * Focus will need to be trapped within the modal while its open: 
        * Add key press handlers to the first element and the last element to make them wrap around to the last element and first element respectively 


##### javascript

```
openModalButton.addEventListener('click', openModal); 

function openModal() {
    1. save the element that had focus before modal
    2. Find all the focusable elements in the modal and put these in an array
    3. add a keydown event listener which calls a function to trap tabbing inside the modal
    4. add an event listener which listens for when the close modal button is clicked

}
```


