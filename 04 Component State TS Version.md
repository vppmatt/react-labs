# Lab 4 - Component State

## Intro

In this lab we will see why state is needed, and learn how to implement it with the useState hook. We will add a button to the greeting component, and when the button is clicked it will change the greeting to say hello to somebody different.

## Pre-requisites

You should have a running "hello world" application with a simple Greeting component.

## 1. Create an element to fire an event

1. **Create a button** within the JSX containing the text "change my name"

2. Edit the button so that **when it is clicked**, it will run a function called "changeName"

3. **Create a function** called changeName. When the function is executed print to the console the phrase "the button was clicked".

### End of section code
At this point your code should look like this:

```
const Greeting = (props: GreetingProps) => {
 
    const changeName = () : void => {
        console.log("button was clicked");
    }

    return (
        <div>
            <p className="greeting_text"> Hello {props.name}. You are {props.age} years old. </p>
            <button onClick={changeName} >change my name</button>
        </div>
    );
}

export default Greeting;

type GreetingProps = {name: string, age: number};
```

## 2. Create a stateful variable

1. Import the `useState` hook from `'react'`

2. In the body of the component function, before the button click method, **set up a stateful variable** called `currentName`, and give it the initial value of the name passed into the component via its properties.

3. In the changeName function, **change the value of the currentName** variable to "James"

4. **Change the JSX** to greet the content of the `currentName` variable.

### End of section code
At this point your code should look like this:

```
const Greeting = (props: GreetingProps) => {

    let [currentName, setCurrentName] = useState<string>(props.name);
 
    const changeName = () : void => {
        setCurrentName("James");
    }

    return (
        <div>
            <p className="greeting_text"> Hello {currentName}. You are {props.age} years old. </p>
            <button onClick={changeName} >change my name</button>
        </div>
    );
}

export default Greeting;

type GreetingProps = {name: string, age: number};
```
