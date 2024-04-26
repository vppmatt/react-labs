# Activity - useContext

## Intro

In this activity you will be implementing a user login context in the Payments UI application.

## Pre-requisites

You should have a running "payments ui" application.

## 1. Create and Export a UserContext object  

1. **Create a new folder** under the src folder called "context".

2. **Create a new file** in the context folder called "context.ts". 

3. Add the following to context.ts

```
import {createContext} from "react";

export type userType = {id: number, name : string, role : string};

export type userContextType = userType & { login : (user : userType) => void, logout : () => void};

export const UserContext : React.Context<userContextType> = 
    createContext<userContextType>({id: 0, name : "", role : "", login : () => {}, logout: () => {}});
```

## 2. Modify App Component to use this UserContext object

 1. Create a stateful variable in **App.tsx** to store this data and define the login and logout functions. 

```
const [user,setUser] = useState<userType>({id: 0, name : "", role : ""});

const login = (user : userType) => {
      setUser(user);
}

const logout = () => {
      setUser({id: 0, name : "", role : ""});
}
```

## 3. Provide the UserContext object at the top of the App component

1. Wrap the JSX returned from App in UserContext.Provider 

```
return (
    <UserContext.Provider value={{...user, login : login, logout : logout}}>
    <BrowserRouter>
      <PageHeader />
      <Routes>
        <Route path="/find/:orderId" element={<FindTransactionPage />} />
        <Route path="/find" element={<FindTransactionPage />} />
        <Route path="/add" element={<AddTransactionPage />} />
        <Route path="/" element={<HomePage />} />
        <Route element={<PageNotFound />} />
      </Routes>
    </BrowserRouter>
    </UserContext.Provider>
```
 
## 4. Create a Login component under staticPages folder to simulate a user login. 

```
import {useContext} from "react";
import {useNavigate} from "react-router-dom";
import { UserContext, userContextType } from "../../context/context";

const Login = () : JSX.Element => {

    const userContext = useContext<userContextType>(UserContext);
    const navigate = useNavigate();

    const login = () : void => {
        //call to REST would be here
        userContext.login({id:1,name : "Matt", role: "admin"});
        navigate("/");
    }

    return (<p>
        This is where the login form would go
        <button onClick={login}>login</button>
    </p>)
}

export default Login;
```


## 5. Add the login page to the routes in App.tsx

```
<Route path="/login" element={<Login />} />
```

## 6. In the Menu component, display a login link if the user id is 0.

Menu.tsx

```
import { Link } from "react-router-dom";
import { UserContext, userContextType } from "../../context/context";
import { useContext } from "react";


const Menu = () : JSX.Element => {

    const userContext = useContext<userContextType>(UserContext);

    return (
        <ul className="nav">
            <li><Link to="/find">Find a transaction</Link></li>
            <li><Link to="/add">New transaction</Link></li>
            {userContext.id === 0 && <li><Link to="/login">Log in</Link></li>}
        </ul>
    );
}

export default Menu;
```

## 7. In the PageHeader display the current logged in user’s name + a logout button if the user id is not 0.

PageHeader.tsx

```
import './pageHeader.css';
import Menu from "./Menu";
import { Link } from 'react-router-dom';
import { useContext } from 'react';
import { UserContext, userContextType } from '../../context/context';

const PageHeader = () : JSX.Element => {

    const userContext = useContext<userContextType>(UserContext);

    return (
        <div className="pageHeader">
            <h1><Link to="/">Payments Application</Link></h1>
            <Menu/>
            { userContext.id !== 0 &&  <p>Current user: {userContext.name} <button onClick={userContext.logout}>logout</button>  </p>}
        </div>
    );
}

export default PageHeader
```
 
