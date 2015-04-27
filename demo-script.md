# ColorSpeak demo

* Demo prep:
```
git clone https://github.com/codemonkeychris/color-speak.git color-speak
git checkout demo-start
npm install -g gulp
npm install
gulp
```
    
* Baseline accessibility
    * Open http://localhost:3000
    * Show UX
    * Highlight problems
        * Doesn't read "X of 949 matched"
        * Doesn't read "selected"
        * Doesn't read item position

* Add live region - read "X of 949 matched"
```
<div id='summary' aria-live='polite'></div> 
```

* Read list item position
```js
return "<div " +
    "    class='item' " +
    "    aria-setsize='"+totalCount+"' " +
    "    aria-posinset='"+index + "'>" +
```

* Read selection status
    * Add listbox
    ```html
    <div id='colorSelect' role='listbox'></div> 
    ```
    * Add listitem
    ```js
    return "<div " +
        "    class='item' " +
        "    aria-setsize='"+totalCount+"' " +
        "    aria-posinset='"+index + "'" +
        "    role='listitem'>" +
    ```
    * Add selection notification
    ```js
    if (!element.classList.contains("selected")) {  
        findAllListItems().forEach(function (elem) {  
            elem.classList.remove("selected");  
            elem.setAttribute("aria-selected", "false");
        });  
        element.classList.add("selected");  
        element.setAttribute("aria-selected", "true");  
        onselected && onselected(element);  
    }  
    ```
