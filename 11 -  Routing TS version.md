# Lab 11 - Routing

## Intro

In this lab you will be implementing routing in the Payments UI application.

## Pre-requisites

You should have a running "payments ui" application.

## 1. Install React routing

1. Ensure the applicaiton is **not currently running**

2. **Install react routing** by entering the following command in the terminal:

```
npm install react-router-dom
```

3. **Restart the application** with `npm start`.

## 2. Set up routes for 2 pages

We will now split our 2 pages in the Payments UI into separate URLs:
We'll make the home page for the application the FindTransaction feature, and we'll use a url of /add for  the Add Transaction Page.

1. Replace the return statement in App.tsx with:

```
return (
        <BrowserRouter>
            <Routes>
                <Route path="/add" element={<AddTransactionPage/>} />
                <Route path="/" element={<FindTransactionPage/>} />
            </Routes>
        </BrowserRouter>
    );
```    

2. Check in the browser that **visiting the URLs** shows the correct page

## 3. Restructure the application

Now that we have implemented routing it makes sense to restructure the applicaiton.

1. The page header should appear on every page, so **move the PageHeader** out of the page components and place it in App, above the Routes. (It should be inside the BrowserRouter as we will be putting links into this component soon).

2. **Create a new component** that can act as the home page for the application. This can just return the words "Welcome to the payments application".

3. **Adjust the routes** so that the home page is the new component, and the Find page is accessed from the url /find.

4. Check in the browser that **visiting the URLs** shows the correct page

### End of section code
App.tsx should now look like this:

```
return (
        <BrowserRouter>
            <PageHeader/>
            <Routes>
                <Route path="/add" element={<AddTransactionPage/>} />
                <Route path="/find" element={<FindTransactionPage/>} />
                <Route path = "/" element={<HomePage />} />
            </Routes>
        </BrowserRouter>
    );
```

## 4. Add page navigation

1. **Add links** to the PageHeader and Menu components to set up the navigation.

2. Links generate a regular HTML anchor tag – so you can **apply styling** to links by creating css rules for the anchor tag.

3. Now you should be able to **click on the menu items** on the page and see the URLs change and the relevant components be reloaded.

### End of section code
The following samples are extracts from the different files


**Menu.tsx**
```
const Menu = () : JSX.Element  => {
    return (
        <ul className="nav">
            <li><Link to="/find">Find a transaction</Link></li>
        </ul>
    );
}
```

**PageHeader.tsx**
```
    return (
        <div className="pageHeader">
            <h1><Link to="/">Payments Application</Link></h1>
            <Menu/>
        </div>
    );
```

**PageHeader.css**
```
a {
    text-decoration: none;
}
```

## 5. Add a 404 page

1. **Create a new component** that can display a message like "Sorry, this page doesn't exist".

2. **Create a route** in App.js, with no path to display this component. This must be the final route in the routes list as the first matching route will be used.

### End of section code
App.tsx should now look like this:

```
    return (
        <BrowserRouter>
            <PageHeader/>
            <Routes>
                <Route path="/find" element={<FindTransactionPage/>} />
                <Route path = "/" element={<HomePage />} />
                <Route element={<PageNotFound />} />
            </Routes>
        </BrowserRouter>
    );
```

## 6. Create a URL parameter

1. In the FindTransactionPage component, when the searchTerm is changed, **use the useNavigate hook** to change the URL to /find/xxx

2. In App.tsx, **add a new route** to match /find/xxx - here we use a : in the route path to indicate a parameter name. For example /find/:searchTerm will match any urls in the format /find/abc, and the value provided (abc) will be accessible as a parameter called searchTerm.

3. In the Transactions component, when this component renders, we'll need to **load in the parameter from the URL**, compare it to the search term and if the two are not the same, then set the searchTerm to the parameter. However the search term is part of props, so we can't change that directly – we'll need a new local state variable, which we'll call selectedOrder...

### End of section code
These code samples are excerpts from each file.

**App.tsx**
```
<Route path="/find/:orderId" element={<FindTransactionPage/>} />
```

**FindTransactionPage.tsx**
```
    const navigate = useNavigate();

    const applySearchTerm = (searchTerm : string) : void => {
        setSearchTerm(searchTerm);
        navigate(`/find/${searchTerm}`);
    }
```

**Transactions.tsx**
```
    const [selectedOrder, setSelectedOrder] = useState<string>("");

    const params = useParams();
    const desiredOrder : string = params.orderId != null ? params.orderId : props.searchTerm;

    if (desiredOrder !== selectedOrder) {
        setSelectedOrder(desiredOrder);
    }
```

## 7. Work with a query string

In the Transactions component:

1. Set up a variable to access the query string using the **useSearchParams hook**. The syntax for this is:

```
const [searchParams, setSearchParams] = useSearchParams();
```

2. In the changeCountry function, **use the setSearchParams function** to set the query string for the URL.

```
setSearchParams({country: uniqueCountries[option]});
```

3. In the browser, change the selected country and **check that the URL is changed** as the country is changed. Note that the query string does not affect the page matching for routing, so /find?... will match against the route "/find"

4. Now when the Transactions component is loaded, we need to **check if there is a country in the URL** (e.g. if the user has bookmarked the page so gone straight to this URL) - if so we need to set the selected country.

```
const urlCountry = searchParams.get("country");
if (urlCountry !== selectedCountry) {
    setLoading(true);
    setSelectedCountry(urlCountry);
}
```

