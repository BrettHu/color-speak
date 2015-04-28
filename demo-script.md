# ColorSpeak demo

* Demo prep:
```
git clone https://github.com/codemonkeychris/color-speak.git color-speak
git checkout demo-start
npm install -g gulp
npm install
gulp
```

This demo starts out with a basic set of div to put together a color selection piece UI.
The goal here is to allow a person to type in a color and narrow down the search 
Let us walk through visually what happens first:
1) You tab (or set focus) to the text box
2) You start to type, as you do the visuals on the screen start to change
  - The list start to get smaller 
3) If you tab to the list you can arrow up/down and see that the color block changes to the color in question
If you start up Narrator (WindowsKey + Enter) and tab to the text box you will notice that when you type Narrator does not read that the list is being refined.  What are the steps we can take to solve this?
    
* Baseline accessibility
    * Open http://localhost:3000
    * Show UX
    * Highlight problems
        * Doesn't read "X of 949 matched"
        * Doesn't read "selected"
        * Doesn't read item position


Have a text area that tells you information that is happening as you type.  This type of setup is common when an app calls for a design that has a notification from something as simple as a "loading..." or "syncing..." type of message that is displayed on the screen to the possibilities of error messages such as "invalid credit card."  For this demo we will put in the number of items. 
To accomplish this we add a property to the text block of:  aria-live="polite" This now forces the text block to be read via a screen reader. 

* Add live region - read "X of 949 matched"
```
<div id='summary' aria-live='polite'></div> 
```

he next change, you now go in the list below and see what is read
The visual expectation is a list… but it sure does not sound like a list. 
hoe to fix this?
We need to add new properties to the divs:
For the list box area we need to add:  aria-role="listbox" 
For each list item we need to add aria-role="list-item" 
Startup Narrator, tab into the list, and listen to the results ... you should now hear that there are list items..  Sweet, progress!!!

Now visually it is clear on a short list where you are in the overall number of list items. 
How can we help a Narrator or screen reader user know the N of M?
The ability to read number N of M is done by using the property aria-posinset (position in set) and aria-setsize (size of the set).  By adding these in now a screen reader user can know where they are in the list.
Let us take a listen --- all should work now.  Great!!

* Read list item position
```js
return "<div " +
    "    class='item' " +
    "    aria-setsize='"+totalCount+"' " +
    "    aria-posinset='"+index + "'>" +
```

We have one major area left.
Visually we know when something is selected – so how can a Narrator user know this?
This can be accomplished by us adding and setting an property on each list of aria-selected=”true” or “false”


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

With all of that work, let us listen through the end-to-end now.
For the final listen --- let us start back at the top of the walkthrough:
  a) Turn on Narrator
  b) Start typeing 
    - notice: you hear the list reduce 
  c) Keep typing until there is nothing 
    - notice: you should hear there is nothing 
  d) Delete back a bit and notice that there are options in the list
   e) Jump down to the list and arrow/up down
    - Notice you hear the N of M, much as you see this on the screen
    d) Now do a selection - notice it now says "selected" 
Awesome work!
Now, how many lines of code was it to get this from nothing to something???
