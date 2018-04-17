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

### Type of Nodes
|nodeType|Type of Node|
|--------|------------|
|1|element|
|2|attribute|
|3|text (including whitespace)|
|4|CDATA section|
|5|entity reference|
|6|entity|
|7|processing instruction|
|8|HTML comment|
|9|document|
|10|document type (DTD)
|11|document fragment|
|12|notation|

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
 elements in the childNodes array.

firstChild is, unsurprisingly, the first element in the
 childNodes array. Using firstChild is equivalent to using
  childNodes[0].

To access the last element in the collection, you gain a
 big advantage by using lastChild. To access this element
  you would otherwise have to do something like this:

var lastChildNode =
 myElement.childNodes[myElement.childNodes.length - 1];

That’s pretty ugly. Instead, you can simply use

var lastChildNode = myElement.lastChild;

### The parentNode Property
The parentNode property, unsurprisingly, returns the parent
 node of the node to which it’s applied. In the previous
  example, you used

var lastChildNode = myElement.lastChild;

Using parentNode you can go one step back up the tree. The
 line

var parentElement = lastChildNode.parentNode;

would return the parent element of lastChildNode, which is,
 of course, the object myElement.

### nextSibling and previousSibling
Sibling nodes are nodes that share a parent node. When
 applied to a specified parent node, these read-only
  properties return the next and previous sibling nodes,
   respectively, or null if there is no such node.

var olElement = document.getElementById("toDoList");
var firstOne = olElement.firstChild;
var nextOne = firstOne.nextSibling;

#### Exercises
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
