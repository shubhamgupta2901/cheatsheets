* CSS - Cascading Style Sheets.
* CSS is often considered as the *adjective* and HTML is considered as a *noun*.
* Genereal Rule: Every line of CSS that we will write will follow this rule:
```css
selector{
    propery: value;
    anotherProperty: value;
}
```
* Example:
```css
  /*Make all the h1 purple and font size 56px*/
  h1{
      color: purple;
      font-size:56px;
  }
  
  /*Give all the images a 3px red border*/
  img{
      border-color: red;
      border-width: 3px;
  }
 ```

 * **Inline CSS:**

 ```html
    <p style="color:blue;font-size:46px;">
      I'm a big, blue, <strong>strong</strong> paragraph
    </p>
 ```

 * **Style Tag:**
 ```html
 <!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <style type="text/css">
        h1{
            color:purple;
        }
        li{
            color: orange;
        }
    </style>
</head>
<body>
    <h1>Rusty Steele</h1>
    <p>Hi, I'm a dog. Woof!</p>
    <img src="http://i.imgur.com/Zl0A6erm.jpg">
    <h2>Some of my favorite places</h2>
    <ul>
        <li>The beach</li>
        <li>The dog park</li>
        <li>The fire hydrant</li>
    </ul>
    <p>Make sure to follow me on <a href="https://www.google.com">Instagram</a></p>

   
</body>
</html>
 ```

* **Seperate CSS File**: Using the ```<link>``` tag.It writes the CSS in totally seperate file, and then connect it with a ```<link>``` tag inside our head and apply the css mentioned to the html contents of the file.

* Sometimes, inline styles are necessary. If you are building a web page by hand, however, you should avoid them whenever possible. Using a separate CSS file is the most powerful and flexible method.

* HTML is meant for conveying structured information. CSS is built to style that structured information. When inline styles are used, this clear separation between structured information and styling is blurred. By separating the CSS from the HTML, the markup can be semantic, which means that it can convey as much meaning as possible without being muddled by visual effects.

* Also, because inline styles only affect the tag they are written in, it can be hard to make changes. So for example, if you have written the same style 20 times in 20 different ```<div>``` tags, you must edit each of those places whenever you want to make a trivial change. This can be exhausting! By using a single CSS rule in a ```<style>``` tag or a separate CSS file, you would only need to change it in one place.

#### CSS Selectors REF: [30 selectors](https://code.tutsplus.com/tutorials/the-30-css-selectors-you-must-memorize--net-16048)
* Element: selects all the elements with ```element-name```
```css
element-name{
    property:value;
}
```

* Class: selects all the elements with class ```class-name```
```css
#class-name{
    property: value;
}
```

* ID: Selects the html element with id ```id```
```css
.id{
    property: value;
}
```

* Star: Selects every single element in the html.
```css
*{
    property: value;
}
```

* Descendant: It takes two or more tagnames or two or more selectors and tries to find the 
```css
/*This style will be implemented on all the anchor tags which are inside the li tags i.e. anchors whose parent is li tag*/
li a{
    property: value;
}
/*Similar to above selector, Syntactically correct; but redundant because li appears only inside ul*/
ul li a{
    property: value;
}
```

* Adjacent Selector:
```css
/*It will let us select element that come after another element on the same level. i.e. it allows us to select a sibling 
Following will select all the ul that are siblings to h4.*/
h4+ul{
    border: 4px solid red;
}
```

* Attribute selector: 
```css
/*This will select all the anchor tags with the attribute href having value "https://www.google.comx"*/
a[href="https://www.google.com"]{
    background: blue;
}

/*selects all inputs with attribute type is text*/
input[type="text"]{

}
```

* nth of type
```css
/*Selects the nth type of element in a page. This will select the 3rd ul element present in the page.*/
ul:nth-of-type(3){
    background: purple;
}
```