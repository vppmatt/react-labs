# Lab 11 - Routing

## Intro

In this lab you will be implementing routing in the Payments UI application.

## Pre-requisites

You should have a running "payments ui" application.

## 1. Install React routing

1. Ensure the applicaiton is **not currently running**

2. **Install react routing** by entering the following command in the terminal: required

```
npm install react-router-dom
```

3. **Restart the application** with `npm run dev`. required

## 2. Create a FindTransactionPage component 
The goal of this file is to combine the Search and Transactions components into a single page. We then will need a prop that is passed to both components.


1. Create a new file called FindTransactionPage.tsx 

2. The HTML should display both Search and Transactions components.



### End of section code
FindTransactionPage.tsx should now look like this:
```
const FindTransactionPage = () => {


    return (
        <div>
            <Search />
            <Transactions />
        </div>
    );
}

```


## 3. Set up routes for 2 pages - required

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

## components/staticPages/HomePage.tsx

```
const HomePage = () : JSX.Element => {
    return (<div><h3>Welcome to the Payments application.</h3></div>);
}
```

export default HomePage;

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
            <li><Link to="/add">New transaction</Link></li>
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

##components/staticPages/PageNotFound.tsx
```
const PageNotFound = () : JSX.Element => {
    return(<h3>Sorry - that page doesn't exist</h3>);
}

export default PageNotFound;
```

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
                <Route path="*" element={<PageNotFound />} />
            </Routes>
        </BrowserRouter>
    );
```

## 6. Create a URL parameter

1. When the user enters an orderId in the search order id input - the URL should change 
to /find/xxx where xxx is the entered orderId. This orderId needs to be extracted in Transactions component using useParams - to display payments only for the entered orderId.  

App 
-- FindTransactionPage
    --- Search
    --- Transactions
    
     Now App Component displays the FindTransactionPage which in turn displays the Search Component - the orderId is entered in the search component - and when the Search Order button is clicked - we need to inform FindTransactionPage component -  so that we can change the URL and also pass that url to Transactions component to display only payments for entered orderid 

**Search.tsx**
```
export type SearchProps = { initialSearchTerm : string, setSearchTerm : ( value : string) => void;}


const Search = (props: SearchProps): JSX.Element => {

    const [searchTerm, setSearchTerm] = useState<string>(props.initialSearchTerm);

    // all other code
    // invoke the setSearchTerm method passed to change the searchTerm in  FindTransactionsPage
    const doSearch = (event: FormEvent<HTMLFormElement>):void => {
        event.preventDefault();
        console.log(searchTerm);
        if (valid){
            props.setSearchTerm(searchTerm);
        }

    }
```

FindTransactionsPage.tsx 
Add the following code to existing code

```
import { useState } from "react";
import Search from "../Search/Search";
import Transactions from "./Transactions";
import { useNavigate } from "react-router";

const FindTransactionPage = () : JSX.Element => {
    const [searchTerm,setSearchTerm] = useState<string>('')

    const navigate = useNavigate();

    const applySearchTerm = (searchTerm : string) : void => {
        setSearchTerm(searchTerm);
        navigate(`/find/${searchTerm}`);
    }

    return (<div>
                <Search initialSearchTerm={searchTerm} setSearchTerm={applySearchTerm}/>
                <Transactions />
            </div>);
}

export default FindTransactionPage;

```

**App.tsx** (Add a new route to /find/:orderId)
```
<Route path="/find/:orderId" element={<FindTransactionPage/>} />
```

**Transactions.tsx** (Extract the orderId from the url and execute function to get data for that orderId)

```
const [selectedOrder, setSelectedOrder] = useState<string>('');


    const params = useParams();
    const desiredOrder : string = params.orderId != null ? params.orderId : '';

    if (desiredOrder !== selectedOrder){
        setSelectedOrder(desiredOrder);
    }
    
    
     if(loading && selectedOrder === "") {
    loadTransactionsForSelectedCountry();
  }

  const loadTransactionsForSelectedOrder = () : void => {
        setPayments([]);
  
         getAllPaymentsForOrderId(selectedOrder)
            .then(response => {
                console.log(response.data);
                setLoading(false);
                setPayments(response.data);
            })
            .catch(error => {
                console.log("something went wrong", error);
            });
    };
    

    useEffect(() => {
    
        if (selectedOrder !== "" ) {
            setPayments([]);
            setLoading(true);
            loadTransactionsForSelectedOrder();
        }else {
            setPayments([]);
            loadCountries();
         }
    }, [selectedOrder]);

     return (
    <>    
     {loading  && (
        <p className="loadingMessage">The data is loading please wait...</p>
      )}

      {countryOptions.length > 0 && (
        <div className="transactionsCountrySelector">
          Select country: {countrySelector}
        </div>
      )}

      <!-- {loading && selectedCountry !== "xxx" && (
        <p className="loadingMessage">The data is loading please wait...</p>
      )}

      {loading && selectedCountry === "xxx" && (
        <p className="loadingMessage">Select a Country...</p>
      )} -->

      {!loading && (
        <table className="transactionsTable">
          <thead>
            <tr>
              <th>Id</th>
              <th>orderId</th>
              <th>Date</th>
              <th>Country</th>
              <th>Currency</th>
              <th>Amount</th>
            </tr>
          </thead>
          <tbody>
        
            {selectedOrder!=="" && payments
                .map((payment) => (
                <PaymentTableRow key={payment.id} {...payment} />
              ))}
            {selectedOrder==="" && payments
              .filter((payment) => payment.country === selectedCountry)
              .map((payment) => (
                <PaymentTableRow key={payment.id} {...payment} />
              ))}
          </tbody>
        </table>
      )}
    </>
  );
};
```

**DataFunctions.ts**

```
export const getAllPaymentsForOrderId = (orderId: string) : Promise<AxiosResponse<PaymentType[]>> => {
    const transactionsPromise = axios<PaymentType[]>({url : `${serverURL}/api/payment?order=${orderId}`, method: "GET", headers : {'Accept': 'application/json'} });
    return transactionsPromise;
}

```
** End of step 6**

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

4. Now when the Transactions component is loaded, we need to **check if there is a country in the URL** (e.g. if the user has bookmarked the page so gone straight to this URL) - if so we need to set the selected country. Passing urlCountry! to confirm that it will not be null.

without this step - it will not extract the country from the query parameter

```
const urlCountry = searchParams.get("country");
if (urlCountry!==null && urlCountry !== selectedCountry) {
    setLoading(true);
    setSelectedCountry(urlCountry!);
}
```

To change the drop down to reflect the country in the url add value attribute 
```
const countrySelector: JSX.Element = (
    <select id="countrySelector" onChange={changeCountry} value={selectedCountry!==''?selectedCountry:'usa'}>
      {countryOptions}
    </select>
  );
```

