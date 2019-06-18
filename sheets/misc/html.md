### Contents


* Hypertext Markup Language.
* HTML is the structure and content of the page. 
* Every html document we create starts with a boilerplate template which looks like this: 
* 
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>
<body>
   <!-- Our content goes here--> 
</body>
</html>
```
* Note ```<!DOCTYPE html>``` does not have a closing tag like others. It indicates that your HTML content uses HTML 5. Doing so will cause even browsers that don't presently support HTML5 to enter into standards mode, which means that they'll interpret the long-established parts of HTML in an HTML5-compliant way while ignoring the new features of HTML5 they don't support.

* This is much simpler than the former doctypes, and shorter, making it easier to remember and reducing the amount of bytes that must be downloaded.

* ```<html>``` element represents the root (top-level element) of an HTML document, so it is also referred to as the root element. All other elements must be descendants of this element. The permitted content inside is one ```<head>``` element and one ```<body>``` element.

* ```<head>``` element contains metadata about the document like title, scripts, and style sheets.

* ```<title>``` defines the document's title that is shown in a browser's title bar or a page's tab. It only contains text and tags within the element are ignored. It is also important for search engines indexing.

* ```<body>``` represents the content of an HTML document. There can be only one ```<body>``` element in a document.

* Comments in HTML can be written like ```<!-- Comment here-->```




