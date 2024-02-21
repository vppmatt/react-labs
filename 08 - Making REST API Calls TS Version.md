# Lab 8 - Making REST API calls

## Intro

In this lab you will be seeing how we can communicate with a server over REST.

## 1. Get some data from a back end server

1. Ensure the Payments back-end application is running. 

2. **Create an additional function** in the DataFunctions.js module called `getAllPaymentsRestVersion`. 

3. **Call this function** from the transactions component. For now just call this somewhere near the top of the component (don't try and integrate it yet).

4. In the getAllPaymentsRestVersion function, **use `fetch`** to issue a GET request to the payments server's get all transactions rest end point. (<https://payments.multicode.uk/api/payment>). Note that you need to provide the following header: `accept: application/json`.

5. **Use the `then` method** to get the response and print it out to the console.

6. Run the application and **look at the response object** in the browser's console.  Notice you can't see the data at this point.


### End of section code
This is a sample solution to the requirements - you may have other code present from previous exercises.

```
export const getAllPaymentsRestVersion = () :void  => {
    const responsePromise : Promise<Response> =
        fetch("https://payments.multicode.uk/api/payment",
          { method: "GET", headers : {'Accept': 'application/json'} });
    responsePromise.then(response => console.log(response));
}
```

## 2. Extract the data returned by the server

1. Within the `then` function that has the response object, **call the `json()` function** of the response object. This also returns a promise.

2. When the jsonPromise completes, **print out the data** received to the console.

Hint: you will be creating nested code to make this work!


### End of section code
This is a sample solution to the requirements - you may have other code present from previous exercises.

```
export const getAllPaymentsRestVersion = () :void  => {
    const responsePromise : Promise<Response> =
          fetch("https://payments.multicode.uk/api/payment",
               { method: "GET", headers : {'Accept': 'application/json'} });
    
    responsePromise.then(response => {
        response.json().then (data => {
            console.log(data);
        })
    });
}
```

## 3. Attempt to implement a good user experience while fetching data.

**WARNING: This part of the lab will not work... there is a problem which we'll fix the problem in the next section!**

1. Create a function to replace `getAllPayments` that **returns the Promise<Response>** .

In the transactions component:

2. **Set up a stateful variable** called loading, with an initial value of `true`.

3. Change the return method of the component so that the **table is only displayed if the loading variable has the value `false`**.  Show a friendly message if it's true. Hint: use conditional rendering

4. Change the **payments array to be a stateful object** with an initial value of an empty array.

5. We have a call the getAllPayments function - this now returns a promise, **so store this as a variable**. 

6. When the promise completes, **call its `json()` function**. 

7. When the json function completes, **extract the data** and use it to set the values of the stateful variable payments. Also at this point **set the loading variable to `false`**. 

### End of section code
This is a sample solution to the requirements - you may have other code present from previous exercises.

**DataFunctions.ts**
```
export const getAllPayments = () : Promise<Response> => {
    const transactionsPromise = fetch(`${serverURL}/api/payment`,{ method: "GET", headers : {'Accept': 'application/json'} });
    return transactionsPromise;
}
```

**Transactions.tsx**

```
const Transactions = (): JSX.Element => {

    const [payments, setPayments] = useState<PaymentType[]>([]);
    const [loading, setLoading] = useState<boolean>(false);
    
    getAllPayments()
        .then(response => {
            response.json()
                .then(data  => {
                    setPayments(data);
                    setLoading(false);    
                })
            }
    );

    return (
        <div>
            {loading && <p className="loadingMessage">The data is loading please wait...</p>}
            
            {!loading &&
                <div>
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
                            {payments.map(payment => <PaymentTableRow key={payment.id} {...payment} /> }
                        </tbody>
                    </table>
                </div>
            }
        </div>
    );
}
```

## 4. Use useEffect to control when code is run

1. Move the code that will load the data and set the value of payments + loading into a **separate function** called `loadData`.

2. **Create a useEffect hook** that will call this method. The second parameter should be an empty array so that the method is only called when the component is evaluated the first time, and not on subsequent re-evaluations.

3. Place a `console.log` line at the start of the transactions component and in the fetching data method, so that you can see how many times each runs.

4. Check that this is now working as expected in the browser!

### End of section code
This is a sample solution to the requirements - you may have other code present from previous exercises.

```
const Transactions = (): JSX.Element => {

    console.log("transactions is rendering")

    const [payments, setPayments] = useState<PaymentType[]>([]);
    const [loading, setLoading] = useState<boolean>(false);

    const loadData = () => {
        getAllPayments()
        .then(response => {
            response.json()
                .then(data => {
                    setPayments(data);
                    setLoading(false);    
                })
            }
        );
    }
       
    useEffect(() => loadData(), []);

    return (
        <div>
            {loading && <p className="loadingMessage">The data is loading please wait...</p>}
            
            {!loading &&
                <div>
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
                            {payments.map(payment => <PaymentTableRow key={payment.id} {...payment} /> }
                        </tbody>
                    </table>
                </div>
            }
        </div>
    );
}
```


## 5. Upgrade the code to use axios, and implement error handling

1. **Stop the application** from running

2. **Install axios** by running the following command in the terminal:

```
npm install axios
```

3. **Restart the application**

4. Within DataFunctions.js, **convert the fetch function to an axios function**. You'll need to import axios into this file.

5. Adjust the code in Transactions.js to** use the Axios version** (note that there is no longer a separate promise - you can just access resopnse.data)

6. Enhance the code by adding **error handling**. Use console.log to output if something has gone wrong.

### End of section code
This is a sample solution to the requirements - you may have other code present from previous exercises.

**DataFunctions.ts**
```
export const getAllPaymentsAxiosVersion = () : Promise<AxiosResponse<PaymentType[]>> => {
    const transactionsPromise = axios<PaymentType[]>({ url : `${serverURL}/api/payment`, method: "GET", headers : {'Accept': 'application/json'} });
    return transactionsPromise;
}
```

**Transactions.tsx**
```

const loadData = () => {
        getAllPayments()
           .then(data => {
                    setPayments(data);
                    setLoading(false);    
                })
          );
    }

```
