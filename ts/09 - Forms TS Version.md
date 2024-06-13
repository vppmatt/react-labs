# Lab 9 - Forms

## Intro

In this lab you will be creating a form. 

## Pre-requisites

You should have a running "payments ui" application.

## 1. Create a search feature

We'll start by creating a simple form that contains a single text box into which the user can enter a search term to find only relevant transactions.

We already have a component called Search which contains the input.

1. **Create a stateful variable** called "searchTerm", with an initial value of `""` (an empty string)

2. **Bind** the searchTerm to the value of the input.

3. **Create a function** to be executed when the input's value changes, to update the search term (use the event's .target.value property to find out the data in the input)

4. **Call the function** from the onChange event of the input

5. For now, just to test, **add a function to the onClick event** of the button to print out the search term to the console.

### End of section code
This is a sample solution to the requirements - you may have other code present from previous exercises.

```
const Search = () : JSX.Element => {

    const [searchTerm, setSearchTerm] = useState<string>("");
    
    const doSearch = () : void => {
        console.log(searchTerm);
    }

    const handleChange = (event: ChangeEvent<HTMLInputElement>) : void => {
        setSearchTerm(event.target.value);
    }

    return (
        <div className="searchBox">
            <label htmlFor="orderId">Order Id:</label>
            <input id="orderId" type="text" value={searchTerm} onChange={handleChange} />
            <button type="submit" onClick={doSearch} >Search</button>
        </div>
    );
}
```

## 2. Use an HTML Form element

1. Change the JSX in the search component so that it is now **constructed as an HTML form**.

2. **Change the button** to be of type `submit` and remove its onClick event.

3. **Bind the onSubmit event** of the form to the `doSearch` function.

4. Add the code required to the doSearch function to prevent the form being submitted to the server.

5. Test the form is working as expected!

### End of section code
This is a sample solution to the requirements - you may have other code present from previous exercises.

```
const Search = () : JSX.Element => {

    const [searchTerm, setSearchTerm] = useState<string>("");
    
    const doSearch = (event:  FormEvent<HTMLFormElement>) : void => {
        event.preventDefault();
        console.log(searchTerm);
    }

    const handleChange = (event: ChangeEvent<HTMLInputElement>) : void => {
        setSearchTerm(event.target.value);
    }

    return (
        <div className="searchBox">
            <form onSubmit={doSearch}>
            <label htmlFor="orderId">Order Id:</label>
            <input id="orderId" type="text" value={searchTerm} onChange={handleChange} />
            <button type="submit">Search</button>
            </form>
        </div>
    );
}
```


## 3. Implement Form validation

We'll implement the following validation rules:

* A search term is only valid if it contains 1 or more characters, excluding spaces

* The button should only be clickable if the search term is valid

* We should display a visible sign to the user if the search term is not valid.

1. **Create a stateful variable** called `valid` to store if the search term is valid or not, with an initial value of `false`.

2. **Create a stateful variable** called `touched` to store if the user has attempted to enter a search term or not, with an initial value of `false`.

3. In the handleChange event, **set the touched variable** to true, and **the valid variable** to true only if the length of the entry is > 0 (call .trim() first in case they have only entered spaces).

4. **Create a css class** called `searchBoxError` in the css file which gives a solid red border.

5. **Assign the input a class** of searchBoxError if the value of valid is false and the value of touched is true (Hint - use the ternary operator)

6. **Set the disabled property** of the button to true if valid is not true.

7. Test the form is working as expected!

### End of section code
This is a sample solution to the requirements - you may have other code present from previous exercises.

```
import './Search.css';
import {ChangeEvent, useState, FormEvent} from "react";

const Search = () : JSX.Element => {

    const [searchTerm, setSearchTerm] = useState<string>("");
    const [valid, setValid] = useState<boolean>(false);
    const [touched, setTouched] = useState<boolean>(false);

    const doSearch = (event:  FormEvent<HTMLFormElement>) : void => {
        event.preventDefault();
        console.log(searchTerm);
    }

    const handleChange = (event: ChangeEvent<HTMLInputElement>) : void => {
        setSearchTerm(event.target.value);
        setTouched(true);
        setValid (event.target.value.trim().length > 0);
    }

    return (
        <div className="searchBox">
            <form onSubmit={doSearch}>
            <label htmlFor="orderId">Order Id:</label>
            <input id="orderId" type="text" value={searchTerm} onChange={handleChange} className={touched && !valid ? 'searchBoxError' : ''} />
            <button type="submit" disabled={!valid} >Search</button>
            </form>
        </div>
    );
}
```
