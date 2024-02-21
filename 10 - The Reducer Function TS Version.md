# Lab 10 - Reducer Functions

## Intro

In this lab you will be creating a reducer function to work with a larger form. 

## Pre-requisites

You should have a running "payments ui" application, and have created the New Payment form.

## 1. Create the reducer function, useReducer and bind the form

1. First **create the standard reducer function** to update data.

2. Define a variable called `initialNewTransactionState` to store the data which will be used the first time the component is loaded.

```
  const initialNewTransactionState: PaymentType = {
    id : null,
    orderId: "",
    date: new Date().toISOString().slice(0, 10),
    amount: 0,
    country: "USA",
    currency: "USD",
    taxCode: 0,
    taxRate: 0.21,
    type: "SALE",
  };
```

Note for the id to be null, we need to alter the PaymentType's id field to allow null values.

2. **Create a stateful variable using useReducer**, setting the initial value to the initialNewTransactionState variable, and referencing the reducer function.  The response objects should be called "newTransaction" and "dispatch".

3. **Bind** the newTransaction to the form elements' value property

4. **Create a handleChange function** which can be called by each of the form's elements. Use the `event.target.id` to find out which field is being changed, and call the dispatch function to update the data. 

5. **Create a submit function** which will print out to the console the value of the newTransaction. Don't forget the code needed to prevent the form being submitted to the server.

### End of section code
This is a sample solution to the requirements

```
import './AddTransaction.css';
import {useReducer} from "react";

const AddTransaction = (): JSX.Element => {
    
  const initialNewTransactionState: PaymentType = {
    id : null,
    orderId: "",
    date: new Date().toISOString().slice(0, 10),
    amount: 0,
    country: "USA",
    currency: "USD",
    taxCode: 0,
    taxRate: 0.21,
    type: "SALE",
  };

  const formReducer = (state: PaymentType, data: { field: string; value: any }): PaymentType => {
    return {...state, [data.field]: data.value};
  };

     const [newTransaction, dispatch] = useReducer(
      formReducer,
      initialNewTransactionState
    );

    const handleSubmit = (event: FormEvent<HTMLFormElement>): void => {
        event.preventDefault();
        console.log(newTransaction);
    }

    const handleChange = (event: ChangeEvent<HTMLInputElement>): void => {
        dispatch( {field : event.target.id, value : event.target.value});
    }

    return (
        <form className="addTransactionsForm" onSubmit={handleSubmit}>
            <h2>New transaction</h2>
            <label htmlFor="orderId">Order Id</label>
            <input type="text" id="orderId"  value={newTransaction.orderId} onChange={handleChange} />
            <br/>
            <label htmlFor="date">Date</label>
            <input type="date" id="date" value={newTransaction.date} onChange={handleChange} />
            <br/>
            <label htmlFor="country">Country</label>
            <input type="text"  id="country" value={newTransaction.country} onChange={handleChange} />
            <br/>
            <label htmlFor="currency">Currency</label>
            <input type="text"  id="currency" value={newTransaction.currency} onChange={handleChange} />
            <br/>
            <label htmlFor="amount">Amount</label>
            <input type="text"  id="amount" value={newTransaction.amount} onChange={handleChange} />
            <br/>
            <label htmlFor="taxCode">Tax Code</label>
            <input type="text"  id="taxCode" value={newTransaction.taxCode} onChange={handleChange} />
            <br/>
            <label htmlFor="amount">Tax Rate</label>
            <input type="text"  id="taxRate" value={newTransaction.taxRate} onChange={handleChange} />
            <br/>
            <label htmlFor="type">Type</label>
            <input type="text"  id="type" value={newTransaction.type} onChange={handleChange} />
            <br/>
            <button type="submit">Save</button>
        </form>
    );
}

export default AddTransaction;
```


## 2. Submit the data to the server!

Note that you could optionally add in some form validation, although we are not going to do that in this lab!

1. In the DataFunctions.js file create a new function to **send the json data to the server**. This function will be a POST, and must provide a header to say that the data is in JSON format.

```
export const addNewTransaction = (payment: PaymentType) : Promise<AxiosResponse<PaymentType>> => {
    return axios<PaymentType>({url : `${serverURL}/api/payment`,
        method: "POST",
        headers : {'Accept': 'application/json', 'Content-Type' : 'application/json'},
        data: payment});
}
```

2. Within the handleSubmit method of the AddTransaction component, call thsi function and display a message on the screen to indicate if it was successful or not. 

### End of section code
This is a sample solution to the requirements. Note in the sample below the newTransaction object has been destructed to make the form a little easier to work with.

```
import "./AddTransaction.css";
import { ChangeEvent, FormEvent, useReducer, useState } from "react";
import { PaymentType, addNewTransaction } from "../../data/DataFunctions";

const AddTransaction = (): JSX.Element => {

  const initialNewTransactionState: PaymentType = {
    id : null,
    orderId: "",
    date: new Date().toISOString().slice(0, 10),
    amount: 0,
    country: "USA",
    currency: "USD",
    taxCode: 0,
    taxRate: 0.21,
    type: "SALE",
  };

  const [message, setMessage] = useState<string>("");

  const formReducer = (state: PaymentType, data: { field: string; value: any }): PaymentType => {
    return {...state, [data.field]: data.value};
  };

  const [newTransaction, dispatch] = useReducer(
    formReducer,
    initialNewTransactionState
  );

  const handleSubmit = (event: FormEvent<HTMLFormElement>): void => {
    event.preventDefault();
    setMessage("saving");
    console.log(newTransaction);
    const response = addNewTransaction(newTransaction);
    response
      .then((result) => {
        if (result.status === 200) {
          setMessage("new transaction added");
        } else {
          setMessage("something went wrong - error code was " + result.status);
        }
      })
      .catch((error) => console.log("something went wrong ", error));
  };

  const handleChange = (event: ChangeEvent<HTMLInputElement>): void => {
    dispatch({ field: event.target.id, value: event.target.value });
  };

  const { orderId, date, amount, country, currency, taxCode, taxRate, type } =
    newTransaction;

  return (
    <form className="addTransactionsForm" onSubmit={handleSubmit}>
      <h2>New transaction</h2>
      <label htmlFor="orderId">Order Id</label>
      <input type="text" id="orderId" value={orderId} onChange={handleChange} />
      <br />
      <label htmlFor="date">Date</label>
      <input type="date" id="date" value={date} onChange={handleChange} />
      <br />
      <label htmlFor="country">Country</label>
      <input type="text" id="country" value={country} onChange={handleChange} />
      <br />
      <label htmlFor="currency">Currency</label>
      <input type="text" id="currency" value={currency} onChange={handleChange} />
      <br />
      <label htmlFor="amount">Amount</label>
      <input type="text" id="amount" value={amount} onChange={handleChange} />
      <br />
      <label htmlFor="taxCode">Tax Code</label>
      <input type="text" id="taxCode" value={taxCode} onChange={handleChange} />
      <br />
      <label htmlFor="amount">Tax Rate</label>
      <input type="text" id="taxRate" value={taxRate} onChange={handleChange} />
      <br />
      <label htmlFor="type">Type</label>
      <input type="text" id="type" value={type} onChange={handleChange} />
      <br />
      <button type="submit">Save</button>
      <div className="addTransactionMessage">{message}</div>
    </form>
  );
};

export default AddTransaction;
```
