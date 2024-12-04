# Activity - Session Storage


## Intro

In this activity you will be storing the theme preference to dark after login in session storage

## Pre-requisites

You should have a running "payments ui" application.

## 1. Store the theme in session storage in the **Login** component using sessionStorage.setItem

Both the key and value have to be strings

```
 const login = () : void => {
        //call to REST would be here
        userContext.login({id:1,name : "Matt", role: "admin"});
        sessionStorage.setItem('theme', 'dark');
        navigate("/");
    }
```

Open dev tools, Application tab, Storage - Session storage and check the value being added there once we click the login button

## 2. Try and retrieve the value of the theme from session storage in PageHeader component and display it

```
import './pageHeader.css';
import Menu from "./Menu";
import { Link } from 'react-router-dom';
import { useContext, useEffect, useState } from 'react';
import { userContextType, UserContext } from '../../context/context';

const PageHeader = () => {
    const userContext = useContext<userContextType>(UserContext);
    const theme = sessionStorage.getItem('theme');

    // const [theme,setTheme] = useContext<string>(sessionStorage.getItem('theme')!==null?sessionStorage.getItem('theme'):'none');

    return (
        <>
        <div className="pageHeader">
            <h1><Link to="/">Payments Application</Link></h1>
            <Menu/>            
        </div>
        { userContext.id !== 0 &&  <p>Current user: {userContext.name} <button onClick={userContext.logout}>logout</button>  </p>}
        { theme && <p>Theme preference is {theme}</p>}
        </>
    );
}

export default PageHeader
```

## 3. Delete the value from session storage when logout button is clicked

**App.tsx**

```
const logout = () => {
      setUser({id: 0, name : "", role : ""});
      sessionStorage.removeItem('theme');
}
```

## 4. Local Storage

Above steps can be repeated for localStorage and when we open another window/tab - the value of theme is still displayed
