# Lab 4 - Component State

## Intro

In this lab we will see why state is needed, and learn how to implement it with the useState hook. We will add a button to the greeting component, and when the button is clicked it will change the greeting to say hello to somebody different.

## Pre-requisites

You should have a running "hello world" application with a simple Greeting component.

## 1. Create an element to fire an event

1. **Create a button** within the JSX containing the text "Happy Birthday"

2. Edit the button so that **when it is clicked**, it will run a function called "haveBirthday"

3. **Create a function** called haveBirthday. When the function is executed print to the console the phrase "Happy Birthday".

4. In App, render just one Greeting to the page.

5. Practise clicking the birthday button and check something is getting logged to the console.

6. NOTE THE DIFFERENCE IN ONCLICK

### End of section code, Greeting.tsx

At this point your code should look like this:

```
const Greeting = (props: GreetingProps): JSX.Element => {
  const haveBirthday = () => {
    console.log("Happy Birthday");
  };

  return (
    <div>
      <p className="greeting_text">
        Hello {props.name}. You are {props.age} years old
      </p>
      <button onClick={haveBirthday}>Happy Birthday</button>
    </div>
  );
};

type GreetingProps = {
  name: string;
  age: number;
};

export default Greeting;

```

### End of section code, App.tsx

```
import Greeting from "./components/Greeting";
import "./App.css";

function App() {
  return (
    <>
      <Greeting name="Matt" age={19} />
    </>
  );
}

export default App;
```



##1.5 

Why doesnt this work?

```
const Greeting = (props: GreetingProps): JSX.Element => {
  let age: number = props.age;

  const haveBirthday = () => {
    age = age + 1;
    console.log(age);
  };

  return (
    <div>
      <p>
        Hello {props.name}. You are {age} years old.
      </p>
      <button onClick={haveBirthday}></button>
    </div>
  );
};

export default Greeting;

type GreetingProps = {
  name: string;
  age: number;
};




```



## 2. Create a stateful variable

1. Import the `useState` hook from `'react'`

2. In the body of the component function, before the button click method, **set up a stateful variable** called `age`, and give it the initial value of the age passed into the component via its properties.

3. In the changeName function, use the setAge setter function to increment age.

4. **Change the JSX** to greet the content of the `age` variable. Note that `age` and `props.age` are different things now! `age` is stateful, `props.age` is not.

### End of section code

At this point your code should look like this:

```
import { useState } from "react";

const Greeting = (props: GreetingProps): JSX.Element => {
  const [age, setAge] = useState<number>(props.age);

  const haveBirthday = () => {
    console.log("Happy Birthday");
    setAge(age + 1); //this will work but is not best practise
  };

  return (
    <div>
      <p className="greeting_text">
        Hello {props.name}. You are {age} years old
      </p>
      <button onClick={haveBirthday}>Happy Birthday</button>
    </div>
  );
};

type GreetingProps = {
  name: string;
  age: number;
};

export default Greeting;

```

## 3. Implement better practise

In React, it's better to use a function within setState when the new state depends on the previous state. This is because setState is asynchronous, and using the previous state directly can lead to unexpected results if multiple setState calls are made in quick succession.

```
import { useState } from "react";

const Greeting = (props: GreetingProps):JSX.Element => {
  const [age, setAge] = useState<number>(props.age);
  const haveBirthday = () => {
    setAge((age) => age + 1);
  };
  return (
    <div>
      <p className="greeting_text">
        Hello {props.name}. You are {age} years old
      </p>
      <button onClick={haveBirthday}>Happy Birthday</button>
    </div>
  );
};

type GreetingProps = {
  name: string;
  age: number;
};

export default Greeting;
```

## 4. Render another greeting component

1. Do you think that the Happy Birthday button will work for both components?
   Answer: yes, because each component has its own stateful variable. One component definition, 2 instances.

2. Where is state held?

```
import Greeting from "./components/Greeting";
import "./App.css";

function App() {
  return (
    <>
      <Greeting name="Matt" age={19} />
      <Greeting name="Tamsin" age={22} />
    </>
  );
}

export default App;


```
