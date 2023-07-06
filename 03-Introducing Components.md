# Lab 3 - Introducing Components

## Intro

In this lab we will create our first component, and see how to place it on the web page.

## Pre-requisites

You should have a running "hello world" application that shows the dummy data contained when the application was first created.

**Note: From this point on we will not ever edit index.js!**

## 1. Create the component file

Components sit in javascript files, however with a typical application containing many components we need to think about how to structure our files. It is usual to create a folder called components, and then place components either directly in that folder or in subfolders. We use subfolders to create logical groups of components. 

1. **Create a new folder** under the src folder called "components".

2. **Create a new folder** under the components folder called "Greeting".

3. **Create a new file** in the Greeting folder called "Greeting.js". Components always start with an uppercase letter and it is convention to name the file with the same name as the component it contains. 

4. Create the outline of a **regular javascript function** in the Greeting.js file. The function should be called Greeting. You should also make it the default export function for the file.
 
```
const Greeting = () => {};

export default Greeting;
```

5. Make the function **return some valid JSX** containing a paragraph with the word "Hello".

```
const Greeting = () => {
    return (<p>Hello</p>);
};
```

*Note: At this point we have created the greeting component, but we won't yet see it on the web page.*

### End of section code
At this point your code should look like this:

```
const Greeting = () => {
    return (<p>Hello</p>);
};

export default Greeting;
```

## 2. Make the component appear on the screen

1. At the moment, the dummy code we see on screen is coming from App.js. **Remove all the return value** from the App function in the App.js file.

2. Return the greeting component that you created. To do this we must return the value `<Greeting />` (remember that JSX elements must be closed).

```
function App() {
  return (
    <Greeting />
  );
}
```

3. What we have now written is interpreted as "return the value that is obtained by executing the function called greeting" - we know this function produces an HTML paragraph object.  However for the function to be executed **we need to import it**. You can manually type the import line at the top of the file, or you can ask the IDE to import it - to do this place your cursor after the end of the word Greeting and press CTRL+Space. 

```
import Greeting from './components/Greeting/Greeting';
```

Note that the import line contains the file name with the folder structure, but excluding the .js part. The component we are importing does not need to be in { } as it is the default export from this file. 

4. If you view the page in the browser you should now see the greeting is displayed.

### End of section code
At this point your code should look like this:

```
import './App.css';
import Greeting from './components/Greeting/Greeting';

function App() {
  return (
    <Greeting />
  );
}

export default App;
```

## 3. Pass properties into the component

For this part of the lab there is a sample solution below, but attempt the exercise before looking at it!

1. Adjust the definition of the greeting component so that it **takes properties**.

2. Where we have used the greeting component (in App.js), **supply 2 properties**, a name and an age.

3. In the **return value** of the greeting component, change the message so that it says hello with the person's name, and tells them how old they are.

### End of section code
At this point your code should look like this:

Greeting.js
```
const Greeting = (props) => {
    return (<p>Hello {props.name}. You are {props.age} years old.</p>);
};

export default Greeting;
```

App.js
```
import './App.css';
import Greeting from './components/Greeting/Greeting';

function App() {
  return (
    <Greeting name="Matt" age="21" />
  );
}

export default App;
```

## 4. Style the component

1. **Create a file** called "Greeting.css" in the same folder as the Greeting.js file.

2. **Define a css class** within this file that sets the text colour to red.

```
.greeting_text {
    color : #f00;
}
```

3. Import the css file into the Greeting component file. (Hint - see how the App.css file is imported into App.js)

4. Apply the style the paragraph. (Hint - be careful when choosing the attribute name for the element). 

### End of section code
At this point your code should look like this:

Greeting.css
```
.greeting_text {
    color : #f00;
}
```

Greeting.js
```
import './Greeting.css';

const Greeting = ({name,age}) => {

    return (
        <div>
            <p className="greeting_text"> Hello {name}. You are {age} years old. </p>
        </div>
    );
}

export default Greeting;
```


## 5. Re-use the component

1. In the App.js file, re-use the Greeting component to greet 2 other people. (Hint: Remember you can only return 1 top level item!)

### End of section code
At this point your code should look like this:

App.js
```
import './App.css';
import Greeting from './components/Greeting/Greeting'

function App() {
  return (
    <div>
        <Greeting name="Matt" age="21"/>
        <Greeting name="Sally" age="27"/>
        <Greeting name="Mike" age="24"/>
    </div>
  );
}

export default App;
```



## Summary

In this lab we have:

* Created a component
* Seen how to use a component
* Provided properties to a component and understood how to access those properties (bind to them) within the function
* Seen that components can be re-used