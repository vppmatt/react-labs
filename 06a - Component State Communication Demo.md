# Lab 6a

- We need 2 components in our interface that do something based on age.
- Greeting component greets a person and increases their age on clicking 'Happy Birthday'
- CanDrink component tells the user where in the world they can legally consume an alcoholic beverage, based on their age.

## D1 Build the CanDrink component and render it in App

1. Have the component return an unordered list with two statements about drinking law, hard coded for now.

### End of section code

At this point your code should look like this:

```
const CanDrink = (): JSX.Element => {

    return (<ul>
        <li>Name, you can drink in the UK</li>
        <li>Name, you can drink in Texas</li>
    </ul>)

}

export default CanDrink;
```

## D2 Render CanDrink in App

```
import Greeting from "./components/Greeting";
import "./App.css";
import CanDrink from "./components/CanDrink";

function App() {

  return (
    <>
      <Greeting name="Matt" age={19} />
      <Greeting name="Tamsin" age={22} />
      <CanDrink />
    </>
  );
}

export default App;

```
<br>
<br>
<br>
## C2 Add props and conditional logic

1. Make the CanDrink component recieve props. Remember to give the props a type.
2. Render the name property into each of the the li tags, e.g. "Tamsin, you can drink in Texas"
3. Add the logic that will evaluate the age of the user, recieved through props, against the legal drinking ages in the UK and Texas (18 and 21, respectively) and print out whether they can or cannot drink in each of the locations.

<br>
<br>
<br>



### End of section code

At this point your code should look like this:

```

const CanDrink = (props: CanDrinkProps): JSX.Element => {
  return (
    <ul>
      <li>
        {props.name}, you can{props.age >= 18 ? "" : "'t"} drink in the UK
      </li>
      <li>
        {props.name}, you can{props.age >= 21 ? "" : "'t"} drink in Texas
      </li>
    </ul>
  );
};

type CanDrinkProps = {
  name: string;
  age: number;
};

export default CanDrink;


```

## D3 Why doesn't CanDrink component respond to a change in Matt's age?

Matt's age, as defined in App, is not stateful. The starting age is passed to Greeting, and a stateful variable is initialised in there.
The stateful age, which is updated on button click, is held in the state area of Greeting.
Problem: Greeting and CanDrink are sibling components. Greeting holds a stateful age, but cannot pass this info to CanDrink.
Solution: lifting state to shared parent.

## D4 Lift the state to App

1. Move the useState import, useState statement and haveBirthday function up to the App.
2. Send the haveBirthday function, which changes the state, down to Greeting through props.

```
<Greeting name="matt" age={age} ageFunc={haveBirthday} />
<CanDrink name="matt" age={age} />

```

3. Edit Greeting component GreetingProps type. It now recieves name, age, and the haveBirthday function.

### End of section code

At this point your code should look like this...

#### For App:

```
import Greeting from "./components/Greeting";
import "./App.css";
import CanDrink from "./components/CanDrink";
import { useState } from "react";

function App() {
  const [age, setAge] = useState<number>(15);

  const haveBirthday = () => {
    setAge((age) => age + 1);
  };

  return (
    <>
      <Greeting name="matt" age={age} ageFunc={haveBirthday} />
      <CanDrink name="matt" age={age} />
    </>
  );
}


export default App;

```

#### For Greeting

```

const Greeting = (props: GreetingProps): JSX.Element => {
  return (
    <div>
      <p className="greeting_text">
        Hello {props.name}. You are {props.age} years old
      </p>
      <button onClick={props.ageFunc}>Happy Birthday</button>
    </div>
  );
};

type GreetingProps = {
  name: string;
  age: number;
  ageFunc: () => void;
};

export default Greeting;

```
