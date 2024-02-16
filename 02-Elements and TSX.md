# Lab 2 - Elements and TSX

## Intro

In this lab we will understand what the Element is - the basic building block of every React applicaitons, and we'll see how to create them using regular Typescript and the JSX extension.

## Pre-requisites

You should have a running "hello world" application.

## 1. Create an element

For this lab we will work in the **index.tsx** file. Normally you would never edit this file but it's a good place for us to explore what an element is. 

1. We will be putting the original code back at the end, so for now, **comment out all the code in this file**, except for the import statements at the top of the page. 

2. **Create an element** called p1, which will be an HTML paragraph, containing the text "This is paragraph 1". It won't have any properties. 
 
```
const p1: React.ReactElement = React.createElement("p", null, "This is paragraph 1");
```

3. **Render the paragraph** in the virtual dom.

```
const root: ReactDOM.Root = ReactDOM.createRoot(document.getElementById('root') as HTMLElement);
root.render(p1);
```

4. **Start the web server** if it is not already running. You can issue this command within a terminal window of the IDE.

```
npm start
```

5. **View the browser page** and check that the text appears on the screen. View the **page source** and then look a the browser's DOM by **inspecting** the page.

### End of section code
At this point your code should look like this:

```
const p1: React.ReactElement = React.createElement("p", null, "This is paragraph 1");
const root : ReactDOM.Root = ReactDOM.createRoot(document.getElementById('root') as HTMLElement);
root.render(p1);
```

## 2. Create child elements, with properties

1. **Create another paragraph** called p2, which will be an HTML paragraph, containing the text "this is paragraph 2". It won't have any properties.

```
const p2: React.ReactElement = React.createElement("p", null, "This is paragraph 2");
```

2. **Create a button**, give it a class of "myButton" and the content of "this is the button". 

```
const button: React.ReactElement = React.createElement("button",{class: "myButton"} , "this is the button");
```

3. **Create a div** called myDiv, with no properties. The content of hte div will be an array containing the two paragraphs and the button.

```
const myDiv: React.ReactElement = React.createElement("div", null, [p1,p2,button]);
```

4. Adjust the render line to **render the myDiv div** rather than the paragraph.

```
root.render(myDiv);
```
5. **View the HTML page** in the browser to check it has worked

### End of section code
At this point your code should look like this:

```
const p1: React.ReactElement = React.createElement("p", null, "This is paragraph 1");
const p2: React.ReactElement = React.createElement("p", null, "This is paragraph 2");
const button: React.ReactElement = React.createElement("button",{class: "myButton"} , "this is the button");
const myDiv: React.ReactElement = React.createElement("div", null, [p1,p2,button]);
const root : ReactDOM.Root = ReactDOM.createRoot(document.getElementById('root') as HTMLElement);
root.render(myDiv);
```

## 3. Create a more complex HTML object

1. **Create a ul** object containing 3 li items, with the content of "first", "second" and "third".  Try and create this as a single line of code (the suggested solution is below but try it first before looking at this!). Then render the ul on the page after the button.

### End of section code
At this point your code should look like this:

```
const p1: React.ReactElement = React.createElement("p", null, "This is paragraph 1");
const p2: React.ReactElement = React.createElement("p", null, "This is paragraph 2");
const button: React.ReactElement = React.createElement("button",{class: "myButton"} , "this is the button");
const myList: React.ReactElement = React.createElement( "ul", null, [
    React.createElement("li", null, "first"),
    React.createElement("li", null, "second"),
    React.createElement("li", null, "third")
]);
const myDiv: React.ReactElement = React.createElement("div", null, [p1,p2,button, myList]);
const root : ReactDOM.Root = ReactDOM.createRoot(document.getElementById('root') as HTMLElement);
root.render(myDiv);
```

## 4. Convert the code to JSX

1. **Comment out all the code** you created excpet for the final two lines (the ones that set the root object and then calls its render method).

2. **Re-write the code** that creates the 2 paragraphs, the button, the ul and the div, **using JSX** (the suggested solution is below but try it first before looking at this!). 

3. Be careful when creating the class for the button - you'll need to use the **className** attribute.

4. Check that the content is displayed correctly in the browser.

### End of section code
At this point your code should look like this:

```
const p1: React.ReactElement = <p>this is paragraph 1</p>;
const p2: React.ReactElement = <p>this is paragraph 2</p>;
const button: React.ReactElement = <button className="myFirstButton">this is the button</button>;
const myList: React.ReactElement = <ul><li>first</li><li>second</li><li>third</li></ul>;
const myDiv: React.ReactElement = <div>{p1}{p2}{button}{myList}</div>;

const root : ReactDOM.Root = ReactDOM.createRoot(document.getElementById('root') as HTMLElement);
root.render(myDiv);
```

## 5. Clean up

To be ready for the next section, we want to put our index.tsx file back to the original state. Remove or comment all the code you wrote, and uncomment the original code in this file. Check that you can see the original dummy content in the browser again.


## Summary

In this lab we have:

* Created various HTML elements using javascript.
* We first created them using a React function called createElement.
* Then we saw the shortcut way to create them using JSX.
