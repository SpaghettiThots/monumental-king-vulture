# Encylopedia Entries

## tbody
`<tbody>` is an HTML element that is employed in the following scenario:
1. As a child of a `<table>` element...
2. after any `<caption>`, `<colgroup>`, and `<thead>` elements...

Appropriate content for `<tbody>` includes zero or more `<tr>` elements and script-supporting elements.

*But*... **it cannot be used when**...
no `<tr>` elements are children of the `<table>` element.

Interestingly, [*a `<tbody>` element's **start tag can be omitted** if the first thing inside the `<tbody>` element is a `<tr>` element,*](spec URL) and if the element is not immediately preceded by a `<tbody>` , `<thead>` , or `<tfoot`> element whose end tag has been omitted. 

It is important to note that the start tag cannot be omitted if the element is empty.

Similarly, a `<tbody>` element's end tag can be omitted if the `<tbody>` element is immediately followed by a `<tbody>` or `<tfoot>` element, or if there is no more content in the parent element.

The `<tbody>` element represents a block of rows that consist of a body of data for the parent table element, if the `<tbody>` element has a parent and it is a table.
-----

## Specificity
A developer often has to specify what stylesheet (*or stylesheets* apply) applies to a document. This can be easy or a more thoughtful process depending on the project. Once this is settled though, conflicts may still be present. **This means that the cascade continues at the rule level.**

When two rules in a single stylesheet conflict, the **type of selector used** determines which rule is applied. The basic principle is: ***the more specific a selector is, the greater its weight in overriding conflicitng declarations.***

Short and simple.

CSS specificity is probably best illustrated.

Let's begin with a rather general guide for calculating *selector specificity* considering that the actual CSS system is a bit complicated:
 - **ID selectors** are more specific than (*and will override*)
 - **Class selectors**, which are more specific (*and will override*)
 - **Contextual selectors**, which are more specific (*and will override*)
 - **Individual element selectors**

Then, if we have a `button` element in our document with the following styles:
```css
button{
	color: red;
}

div button{
	color: grey;
}
```

Notice, we have two button element selectors: the *individual element selector* `button`, and the *contextual selector* `div button`. Which will win? Think about it for a moment (refer to the guide above!).

The individual element selector will color every button element on the page `red` including those we want to be `blue`. However, if there is any `button contained within the context of a `div`, greater specificity applies to it, and thus it is coloured `blue`. No points for guessing!

Why you say? The contextual selector `div button` is more specific and has more weight as a result than the element selector `button`.

Understanding specificity will help you avoid conflicts in your projects considering that HTML and CSS stay silent when it comes to errors.

Specificity can also be applied to keep markup minimal resulting in stylesheets made for humans, that is, *that are easy to parse*.

Consider these `p` rules:
```css
p{
	font-size: 1.2em;
}

blockquote p{
	font-size: 1em;
}

p#intro{
	font-size: 2em;
}
```

Here's what will happen in this situation:
1. Every `p` element will by default have a `font-size` of `1.2em`.
2. Some `p` elements are contained within a `blockquote`. They should have more weight being contextual selectors! Their `font-size` becomes `1em`.
3. Wait! There is a particular `p` with `id="intro"`. The `font-size` is specified as `2em`. It dares to be diferent from the rest. After all, *id selectors are more specific*  than others according to our guide above.

One cannot emphasise enough how specificity is vital in CSS mastery.

Now, that's pretty much you need to know to use CSS specificity, but there is more if you plan to manipulate this even more. Turn on your Math brain!

To calculate selector specificity:
1. count 1 if the declaration is from a `style` attribute and not a stylesheet rule with a selector. If it's not a `style`attribute, count 0. This is `a`.
2. count the number of `id` attributes in the selector. This is `b`.
3. count the number of other attributes and pseudo-classes in the selector. This is `c`.
4. count the number of element names and pseudo-elements in the selector. This is `d`.

Slowly digest that. Style? ID. Others. Elements.

Examples clarify concepts, so we will use these directly from the CSS2.1 spec:
`*{}`: *A selector applying to every HTML element within this document.*
`a=0`: *From the above, we see it is a stylesheet rule and not an attribute. So it scores `0`.*
`b=0`: *There is no `id` attribute specified in this rule. Score = `0`.*
`c=0`: *Are there any other attributes (apart from `id`s) specified? No. It scores a fat `0`.*
`d=0`: *Finally, is any element name or pseudo-element referenced? No here. Another big, fat `0`.*

Putting it all together,
`specificity = 0, 0, 0, 0`
-----
`li`: *A selector applying to only the `li` element within this document.*
`a=0`: *From the above, we see it is a stylesheet rule and not an attribute. So it scores `0`.*
`b=0`: *There is no `id` attribute specified in this rule. Score = `0`.*
`c=0`: *Are there any other attributes (apart from `id`s) specified? No. It scores a fat `0`.*
`d=1`: *Finally, is any element name or pseudo-element referenced? **YES**, `li`. A slender `1`.*

Putting it all together,
`specificity = 0, 0, 0, 1`
-----
`li:first-line`: *A selector applying to the first line after every `li`.*
`a=0`: *From the above, we see it is a stylesheet rule and not an attribute. So it scores `0`.*
`b=0`: *There is no `id` attribute specified in this rule. Score = `0`.*
`c=0`: *Are there any other attributes (apart from `id`s) specified? No. It scores a fat `0`.*
`d=2`: *Finally, is any element name or pseudo-element referenced? **YES** again. One element `li` and a pseudo-element `:first-line`. That makes `2`.*

Putting it all together,
`specificity = 0, 0, 0, 2`
-----
`ul li`: *A selector applying to every `li` element within a `ul` in this document.*
`a=0`: *From the above, we see it is a stylesheet rule and not an attribute. So it scores `0`.*
`b=0`: *There is no `id` attribute specified in this rule. Score = `0`.*
`c=0`: *Are there any other attributes (apart from `id`s) specified? No. It scores a fat `0`.*
`d=0`: *Finally, is any element name or pseudo-element referenced? Yes. Two elements in fact: `ul` and `li`. Another `2`.*

Putting it all together,
`specificity = 0, 0, 0, 2`
-----
`ul li+li`: *A selector applying to an `li` that is adjacent sibling to another `li`that is a descendant of a `ul`.*
`a=0`: *From the above, we see it is a stylesheet rule and not an attribute. So it scores `0`.*
`b=0`: *There is no `id` attribute specified in this rule. Score = `0`.*
`c=0`: *Are there any other attributes (apart from `id`s) specified? No. It scores a fat `0`.*
`d=3`: *Finally, is any element name or pseudo-element referenced? `3` are: a `ul`, the `li` it might contain and another `li` that might be an adjacent sibling to this particular `li`.*

Putting it all together,
`specificity = 0, 0, 0, 3`
-----
`h1 + *[rel=up]`: *A selector applying to a `h1` that is adjacent sibling to any element `*` with one attribute `rel=up`.*
`a=0`: *From the above, we see it is a stylesheet rule and not an attribute. So it scores `0`.*
`b=0`: *There is no `id` attribute specified in this rule. Score = `0`.*
`c=1`: *Are there any other attributes (apart from `id`s) specified? Yes. It scores a slender `1`.*
`d=1`: *Finally, is any element name or pseudo-element referenced? Just one. A slender `1`.*

Putting it all together,
`specificity = 0, 0, 1, 1`
-----
`ul ol li.red`: *A selector applying to an `li` with a class of `red` contained in an `ol` that's in a `ul` element.*
`a=0`: *From the above, we see it is a stylesheet rule and not an attribute. So it scores `0`.*
`b=0`: *There is no `id` attribute specified in this rule. Score = `0`.*
`c=1`: *Are there any other attributes (apart from `id`s) specified? One class attribute is mentioned.*
`d=3`: *Finally, is any element name or pseudo-element referenced? Three are referenced.*

Putting it all together,
`specificity = 0, 0, 1, 3`
-----
`li.red.level`: *A selector applying to every HTML element within this document.*
`a=0`: *From the above, we see it is a stylesheet rule and not an attribute. So it scores `0`.*
`b=0`: *There is no `id` attribute specified in this rule. Score = `0`.*
`c=2`: *Are there any other attributes (apart from `id`s) specified? `2` class attribute selectors are mentioned.*
`d=1`: *Finally, is any element name or pseudo-element referenced? `li` is the only element mentioned here.*

Putting it all together,
`specificity = 0, 0, 2, 1`
-----
Watch these carefully:
`#text`: *Applies to an element, only one, within this element with `id=text`.*
`a=0`: *A stylesheet rule only scores `0`.*
`b=1`: *An `id` attribute. Score = `1`.*
`c=0`: *No other attributes specified.*
`d=0`: *No element specified here.*

Putting it all together,
`specificity = 0, 0, 1, 0`
-----
`style=""`: *A style attribute at last..*
`a=0`: *Scores `1`. It is not an `id`.*
`b=1`: *No `id` attribute. Score = `0`.*
`c=0`: *No other attributes or pseudoclasses specified.*
`d=0`: *No element specified here.*

Putting it all together,
`specificity = 0, 1, 0, 0`

Using what we now know, think what happens to the following document:
```html
<head>
    <style type="text/css">
        #text{
            color: red
        }
    </style>
</head>
<body>
    <p id="text" style="color: green">
        Colored text
    </p>
</body>
```

---

Here, the color of the `<p>` element would be green. The `<style>` attribute of the `<p>` element will override the one in the **head** `<style>` element because by rule 3 above, it has higher specificity. Of course, we keep styles out of our HTML documents as much as possible to separate structure from presentation.
---

## String.prototype.toUpperCase()
##### What is it?
This refers to a method of the `String` object in JavaScript.

##### What does it do?
It converts a string to uppercase.

##### Syntax
`string.toUpperCase()`, where `string` is the sequence of characters to be turned to uppercase.

##### What does it return?
A copy of the string, with each lowercase letter converted to its uppercase equivalent [if it exists].

**Note that it returns a copy of the string and not the string itself turned to uppercase. This is due to the intrinsic *immutability* property of strings.**

##### An Example
```js
//Define a string
var test-string = "Eat tHat frog.";

//Declare another variable to hold the output of string operation
var test-result = test-string.toUpperCase();

//Log test-result to console
console.log(test-result);  //"EAT THAT FROG."
```

##### Browser Support
`String.prototype.toUpperCase()` is well-supported across all major browsers (desktop and mobile) with no reported quirks.
