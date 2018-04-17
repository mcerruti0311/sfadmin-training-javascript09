# sfadmin-training-javascript09
### Scripting with the DOM

#### What You’ll Learn in This Hour:
* The concept of nodes
* The different types of nodes
* Using nodeName, nodeType, and nodeValue
* Using the childNodes collection
* Selecting elements with getElementsByTagName()
* How to use Mozilla’s DOM Inspector
* How to create new elements
* Ways to add, edit, and remove child nodes
* Dynamically loading JavaScript files
* Changing element attributes

You’ve already learned about the W3C DOM and, in the code
examples of previous hours, you used various DOM objects,
properties, and methods.

In this hour you begin exploring how JavaScript can directly
interact with the DOM. In particular, you learn some new
ways to navigate around the DOM, selecting particular DOM
objects that represent parts of the page’s HTML contents.
You see how to create new elements, how to add, edit, and
remove nodes of the DOM tree, and how to manipulate
elements’ attributes.

### DOM Nodes
You probably recall that the top-level object in the DOM
hierarchy is the window object, and that one of its
children is the document mainly deal with the document
object and its properties and methods.

![09fig02](images\09fig02.jpg)

**FIGURE 9.2 The DOM's tree model of our page.**

The `<html>` element contains all the other markup of the page. It
is the parent element to two immediate child elements, the
`<head>` and `<body>` elements. These two elements are siblings,
as they share a parent. They are also parents themselves; the
`<head>` element has one child element, `<title>`, and the
`<body>`element three children: the `<h1>` heading, the ordered
list `<ol>`, and a paragraph, `<p>`. Of these three siblings, only
`<ol>` has children, those being three `<li>` line item
elements. Various of these elements of the tree contain
text, shown represented in the gray boxes in Figure 9.2.

The DOM is constructed as a hierarchy of such relationships. The
boxes that make up the junctions and terminating points of the
tree diagram are known as nodes.

#### Tip
```
Where they exist, text nodes are always contained within element
nodes. However, not every element node contains a text node.
```
#### Caution
```
Remember, the DOM is not available until the page has finished
loading. Don’t try to execute any statements that use the DOM
until then, or your script is likely to produce errors.
```
The `<html>` element contains all the other markup of the page. It
is the parent element to two immediate child elements, the
`<head>` and `<body>` elements. These two elements are siblings,
as they share a parent. They are also parents themselves; the
`<head>` element has one child element, `<title>`, and the
`<body>` element three children: the `<h1>` heading, the ordered
list `<ol>`, and a paragraph, `<p>`. Of these three siblings,
only `<ol>` has children, those being three `<li>` line item
elements. Various of these elements of the tree contain
text, shown represented in the gray boxes in Figure 9.2.

The DOM is constructed as a hierarchy of such relationships. The
boxes that make up the junctions and terminating points of the
tree diagram are known as nodes.

### Type of Nodes
![09tab01](images\09tab01.jpg)

**TABLE 9.1 nodeType Values**

### The childNodes Property
You’ll likely do most of your work using node types 1, 2,
and 3, as you manipulate page elements, their attributes,
and the text that those elements contain.

A useful DOM property for each node is a collection of its
immediate children. This array-like list is called
`childNodes`, and it enables you to access information about
the children of any DOM node.

The `childNodes` collection is a so-called NodeList, in
which the items are numerically indexed. A collection looks
and (for the most part) behaves like an array—you can
refer to its members like those of an array, and you can
iterate through them like you would for an array.
However, there are a few array methods you can’t use,
such as `push()` and `pop()`. For all the examples
here, you can treat the collection like you would a
regular array.

A node list is a live collection, which means that any
changes to the collection to which it refers are
immediately reflected in the list; you don’t have to fetch
it again when changes occur.

#### Tip
```
You’ll likely do most of your work using node types 1,
2, and 3, as you manipulate page elements, their
attributes, and the text that those elements contain.
```

#### Caution
```
Whitespace (such as the space and tab characters) in HTML
code is generally ignored by the browser when rendering the
page. However, the presence of whitespace within a
page element—for example, within your ordered list
element—will in many browsers create a child node of
type text (nodeType == 3) within the element. This
makes simply using childNodes.length a risky business.
```

### firstChild and lastChild
There is a handy shorthand for selecting the first and last
elements in the `childNodes` array.
`firstChild` is, unsurprisingly, the first element in the
`childNodes` array. Using `firstChild` is equivalent to using
`childNodes[0]`.

To access the last element in the collection, you gain a
big advantage by using `lastChild`. To access this element
you would otherwise have to do something like this:

`var lastChildNode =
myElement.childNodes[myElement.childNodes.length - 1];`

That’s pretty ugly. Instead, you can simply use

`var lastChildNode = myElement.lastChild;`

### The parentNode Property
The `parentNode` property, unsurprisingly, returns the parent
node of the node to which it’s applied. In the previous
example, you used

`var lastChildNode = myElement.lastChild;`

Using `parentNode` you can go one step back up the tree. The line

`var parentElement = lastChildNode.parentNode;`
would return the parent element of `lastChildNode`, which is,
of course, the object `myElement`.

### nextSibling and previousSibling
Sibling nodes are nodes that share a parent node. When
applied to a specified parent node, these read-only
properties return the next and previous sibling nodes,
respectively, or null if there is no such node.

```JavaScript
var olElement = document.getElementById("toDoList");
var firstOne = olElement.firstChild;
var nextOne = firstOne.nextSibling;
```
### Node Value
In addition to `nodeType`, the DOM offers the property
`nodeValue` to return the value stored in a node. You generally
want to use this to return the text stored in a text node.

Let’s suppose that instead of counting the list items in the
previous example, you wanted to extract the text contained in
the `<p>` element of the page. To do this you need to access
the relevant `<p>` node, find the text node that it contains,
and then use `nodeValue` to return the information:

``` JavaScript
var text = '';
var pElement = document.getElementById("toDoNotes");
for (var i=0; i < pElement.childNodes.length; i++) {
    if(pElement.childNodes[i].nodeType == 3) {
        text += pElement.childNodes[i].nodeValue;
    };
}
alert("The paragraph says:\n\n" + text );
```
### Caution
```
Make careful note of the spelling. Elements (plural) is used in
getElementsByTagName(), whereas Element (singular) is used in
getElementById().
```

### Node Name
The `nodeName` property returns the name of the specified node as
a string value. The values returned by the `nodeName` value are
summarized in Table 9.2. The `nodeName` property is
read-only—you can’t change its value.

![09tab02](images\09tab02.jpg)

**TABLE 9.2 Values Returned by the nodeName Property**

Where `nodeName` returns an element name, it does so without the
surrounding `<` and `>` that you would use in HTML source code:

```JavaScript
var pElement = document.getElementById("toDoNotes");
alert( pElement.nodeName);  // alerts 'P'
```

### Selecting Elements with getElementsByTagName()
You already know how to access an individual page element using
the `document` object’s `getElementById()` method. Another
method of the `document` object, `getElementsByTagName()`,
allows you to build an array populated with all of the
occurrences of a particular tag.

Like `getElementById()`, the `getElementsByTagName()` method
accepts a single argument. However, in this case it’s not the
element ID but the required tag name that is passed to the
method as an argument.

As an example, suppose you wanted to work with all of the `<div>`
elements in a particular document. You can populate a variable
with an array-like collection called `myDivs` by using

`var myDivs = document.getElementsByTagName("div");`

You don’t have to use `getElementsByTagName()` on the entire
document. It can be applied to any individual object and return
a collection of elements with the given tag name contained
within that object.

#### Tip
```
Even if there is only one element with the specified tag name,
getElementsByTagName() still returns a collection, although it
will contain only one item.
```

### Reading an Element’s Attributes
HTML elements often contain a number of attributes with
associated values:

`<div id="id1" title="report">Here is some text.</div>`

The attributes are always placed within the opening tag, each
attribute being of the form `attribute=value`. The attributes
themselves are child nodes of the element node in which they
appear, as depicted in Figure 9.4.

![09fig04](images\09fig04.jpg)

** FIGURE 9.4 Attribute nodes **

Having navigated to the element node of interest, you can read
the value of any of its attributes using the `getAttribute()``
method:

``` JavaScript
var myNode = document.getElementById("id1");
alert(myNode.getAttribute("title"));
```
The previous code snippet would display “report” within the
`alert` dialog. If you try to retrieve the value of a nonexistent
attribute, `getAttribute()` will return `null`. You can use this
fact to efficiently test whether an element node has a
particular attribute defined:

``` JavaScript
if(myNode.getAttribute("title")) {
 ... do something ...
}
```

The `if()` condition will only be met if `getAttribute()` returns
a non-null value, since null is interpreted by JavaScript as a
“falsy” value (not Boolean false, but considered as such).

#### Note
```
A further useful method for getting a collection of elements is

document.getElementsByClassName()

As you’ll have worked out from the method name, this method
returns all the page elements having a particular value of the
class attribute. However, this was not supported in Internet
Explorer until IE9.
```

#### Caution
```
There also exists a property simply called attributes that
contains an array of all of a node’s attributes. In theory you
can access the attributes as name=value pairs in the order
they appear in the HTML code, by using a numerical key; so
attributes[0].name would be id and attributes[1].value
would be report. However, its implementation in Internet
Explorer and some versions of Firefox is buggy. It’s safer
to use getAttribute() instead.
```

### Creating New Nodes
Adding new nodes to the DOM tree is a two-stage process:

1. First, you create a new node. Once created, the node is
initially in a kind of “limbo”; it exists, but it’s not actually
located anywhere in the DOM tree, and therefore doesn’t appear
on the visible page in the browser window.


2. Next, you add the new node to the tree, in the desired
location. At this point it becomes part of the visible page.

Let’s look at some of the methods of the `document` object that
are available for creating nodes.

#### createElement()
You can call on the `createElement()` method to create new HTML
elements having any of the standard HTML element
types—paragraphs, spans, tables, lists, and so on.

Let’s suppose you’ve decided to create a new `<div>` element for
your document. To do so, you simply need to pass the relevant
`nodeName` value—in this case "div"—to the `createElement`
method:

`var newDiv = document.createElement("div");`

The new `<div>` element now exists, but currently has no contents,
no attributes, and no location in the DOM tree. You see how to
solve these issues shortly.

#### createTextNode()
Many of the HTML elements in your page need some content in the
form of text. The `createTextNode()` method takes care of that. It
works pretty much like `createElement()`, except that the
argument it accepts is not a nodeName value, but a string
containing the desired text content of the element:

`var newTextNode = document.createTextNode("Here is some text content.");`

As with `createElement()`, the newly created node is not yet
located in the DOM tree; JavaScript has it stored in the
newTextNode variable while it waits for you to place it in its
required position.

#### cloneNode()
There’s no point in reinventing the wheel. If you already have a
node in your document that’s just like the new one you want to
create, you can use `cloneNode()` to do so.

Unlike `createElement()` and `createTextNode()`, `cloneNode()`
 takes a single argument—a Boolean value of true or false.
Passing true to the `cloneNode()` function tells JavaScript that
you want to clone not only the node, but all of its child nodes:

```JavaScript
var myDiv = document.getElementById("id1");
var newDiv = myDiv.cloneNode(true);
```
In this example, I’ve asked JavaScript to clone the element’s
child nodes too; for example, any text that `myDiv` contained
(which would be contained in a child text node of the element)
will be faithfully reproduced in the new `<div>` element.

##### Had I called

`var newDiv = myDiv.cloneNode(false);`

then the new `<div>` element would be identical to the original,
except that it would have no child nodes. It would, for
instance, have any attributes belonging to the original element
(provided that the original node was an element node, of
course).

As with new nodes created by `createElement()` and
`createTextNode()`, the new node created by `cloneNode()` is
initially floating in space; it does not yet have a place in
the DOM tree.

You see how to achieve that next.

#### Tip
```
When you amend the DOM using the methods described in this hour,
you change the way a page appears in the browser. Bear in mind,
though, that you’re not changing the document itself. If you
ask your browser to display the source code of the page, you
won’t see any changes there.

That’s because the browser is actually displaying the current DOM
representation of the document. Change that, and you change what
appears onscreen.
```
#### Caution
```
Remember that the id of an element is one of its attributes. When
you clone a node, remember to then change the id of your new
element, since id values should be unique within a document.
```
### Manipulating Child Nodes
The new nodes you’ve created aren’t yet of any practical value,
as they don’t yet appear anywhere in the DOM. A few methods of
the `document` object are specifically designed for placing nodes
in the DOM tree, and they are described in the following
sections.

#### appendChild()

Perhaps the simplest way of all to attach a new node to the DOM
is to append it as a child node to a node that already exists
somewhere in the document. Doing so is just a matter of
locating the required parent node and calling the `appendChild()`
method:

```JavaScript
var newText = document.createTextNode("Here is some text content.");
var myDiv = document.getElementById("id1");
myDiv.appendChild(newText);
```

In the preceding code snippet, a new text node has been created
and added as a child node to the currently existing `<div>`
element having an `id` of `id1`.

Of course, `appendChild()` works equally well with all types of
nodes, not just text nodes. Suppose you needed to add another
`<div>` element within the parent <div> element:

```JavaScript
var newDiv = document.createElement("div");
var myDiv = document.getElementById("id1");
myDiv.appendChild(newDiv);
```

Your originally existing `<div>` element now contains a further
`<div>` element as its last child; if the parent `<div>` element
already contained some text content in the form of a child text
node, then the parent `div`(as represented in the newly
modified DOM, not in the source code) would now have the
following form:
```JavaScript
<div id="id1">
    Original text contained in text node
    <div></div>
</div>
```

####  Tip
````
Remember that appendChild() always adds a child node after the
last child node already present, so the newly appended node
becomes the new lastChild of the parent node.
````
### insertBefore()

Whereas `appendChild()` always adds a child element to the end of
the list of children, with `insertBefore()` you can specify a
child element and insert the new node immediately before it.

The method takes two arguments: the new node, and the child
before which it should be placed. Let’s suppose that your page
contains the following HTML snippet:

``` JavaScript
<div id="id1">
    <p id="para1">This paragraph contains some text.</p>
    <p id="para2">Here is some more text.</p>
</div>
```

To insert a new paragraph between the two that are currently in
place, first create the new paragraph:

`var newPara = document.createElement("p");`

Identify the parent node, and the child node before which you
want to make the insertion:
```JavaScript
var myDiv = document.getElementById("id1");
var para2 = document.getElementById("para2");
```
Then pass these two as arguments to `insertBefore()`:

`myDiv.insertBefore(newPara, para2);`

### replaceChild()
You can use `replaceChild()` when you want to replace a current
child node of a specific parent element with another node. The
method takes two arguments—a reference to the new child element
followed by a reference to the old one.

### removeChild()
There is a DOM method specifically provided for removing child
 nodes from the DOM tree.

Referring once more to Listing 9.3, if you wanted to remove the
`<p>` element with `id="para2"` you can just use

```JavaScript
var myDiv = document.getElementById("id1");
var myPara = document.getElementById("para2");
myDiv.removeChild(myPara);
```
The return value from the `removeChild()` method contains a
reference to the removed node. If you need to, you can use this
to further process the child node that has just been removed:

```JavaScript
var removedItem = myDiv.removeChild(myPara);
alert('Item with id ' + removedItem.getAttribute("id") + 'has been removed.');
```
#### Tip
```
If you don’t have a handy reference to the element’s parent, just
use the parentNode property:
myPara.parentNode.removeChild(myPara);
```
### Editing Element Attributes
In the previous hour you saw how to read element attributes using
the `getAttribute()` method.

There is a corresponding method named `setAttribute()` to allow
you to create attributes for element nodes and assign values to
those attributes. The method takes two arguments;
unsurprisingly, these are the attribute to be added and the
value it should have.

In the following example, the title attribute is added to a `<p>`
element and assigned the value “Opening Paragraph”:
```JavaScript
var myPara = document.getElementById("para1");
myPara.setAttribute("title", "Opening paragraph");
```
Setting the value of an attribute that already exists effectively
overwrites the value of that attribute. You can use that
knowledge to effectively edit existing attribute values:
```JavaScript
var myPara = document.getElementById("para1");
myPara.setAttribute("title", "Opening paragraph"); // set 'title'
attribute
myPara.setAttribute("title", "New title");  // overwrite 'title'
attribute
```

#### Dynamically Loading JavaScript Files
On occasion you’ll want to load JavaScript code on the fly to a
page that’s already loaded in the browser. You can use
`createElement()` to dynamically create a new `<script>` element
containing the required code, and then add this element to the
page’s DOM:
```JavaScript
var scr = document.createElement("script");
scr.setAttribute("src", "newScript.js");
document.head.appendChild(scr);
```

Remember that the `appendChild()` method places the new child node
after the last child currently present, so the new `<script>`
element will go right at the end of the `<head>` section of the
page.

Take note, though, that if you dynamically load JavaScript source
files using this method, the JavaScript code contained in those
files will not be available to your page until the external
file has finished loading.

You would be well advised to have your program check that this is
so before attempting to use the additional code.

Nearly all modern browsers implement an `onload` event when the
script has downloaded. This works just like the `window.onload`
event you’ve already met, but instead of firing when the main
page has finished loading, it does so when the external
resource (in this case a JavaScript source file) is fully
downloaded and available for use:
```JavaScript
src.onload = function() {
    ... things to do when new source code is downloaded ...
}
```
#### Caution
```
This won’t work in older versions of Internet Explorer, but
onload has been supported for script elements since IE8. To be
sure, you may prefer to use object detection of the resources
contained in your newly loaded file instead.
```

### Summary
In this hour you learned about DOM nodes and how to navigate the
DOM using a variety of node-related methods. You also learned
about using Mozilla’s DOM Inspector to examine the DOM of your
page.

In addition, you learned how to create new nodes to add to the
DOM, and how to edit page content dynamically by adding,
editing, and removing DOM nodes.

### Exercises
* Using the nodeType information listed in Table 9.1, write
a function to find all the HTML comments in the body
section of a page, and concatenate them into a single
string. Add some comments to the code listed in Listing
9.2; then introduce and test your new function.
* If you have Firefox, download and install the DOM
Inspector and familiarize yourself with its interface. Use
the program to investigate the DOM of some of your
favorite web pages.
* Having used the insertBefore() method, you might
reasonably expect that there would be an insertAfter()
method available. Unfortunately,  that’s not so. Can you
write an insertAfter() function to do this task? Use
similar arguments to insertBefore(); that is,
insertAfter(newNode, targetNode). (Hint: Use insertBefore()
and the nextSibling property.)
* When you click on a menu item generated by the code in
Listing 9.6, the page scrolls to the relevant item. To
return to the menu, you have to manually scroll back up.

Can you modify the script such that, as well as inserting an
anchor, it inserts a Back to Top link before each H2
element? (Hint: You don’t need to add a new link, just add
an href and some link text to each anchor.)
