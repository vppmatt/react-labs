# Lab 12 - Testing

## Intro

In this lab you will be implementing testing in the Payments UI application.

## Pre-requisites

You should have a running "payments ui" application.

## 1. Comment out the default test and start npm test

1. Ensure the applicaiton is **not currently running**

2. **Edit the file App.test.js** - right now this test will not pass, as our screen no longer says "learn react". However there are other issues with this test (it will fail to load the App component), and we will fix these later. We will exclude the test from running by **changing the method** from `test` to `test.skip`. You should also **comment out** the line that imports the App component

3. Now start the testing service by running:

```
npm test
```

## 2. Create a simple test

For our first test, we will check that when the search box is first rendered, in the Search component, it does not have the `searchBoxError` class applied to it. This should only be applied when the user has attemtped to enter an invalid search term.

1. **Create a file** in the Search folder called `Search.test.js`

2. **Add the following imports** to the top of this file:

```
import {render, screen} from "@testing-library/react";
```

3. Create a test method that will:
    - render the Search component (you will need to import it too)
    - find the input - you can do this with the `getByLabelText` fuction, providing the text of the label
    - check that the item found does not have the class name assigned to it of `searchBoxError`

When complete, the testing code should look like this:

```
import {render, screen} from "@testing-library/react";
import Search from "./Search";

test('check search initially has no class applied to it', () => {
    render(<Search/>);
    const input = screen.getByLabelText('Order Id:');
    expect(input).not.toHaveClass('searchBoxError');
});
```

4. **Check that the test is passing** by viewing the console running the `npm test` command.

5. You may wish to explore using the `screen.logTestingPlaygroundURL();` and `screen.debug();` commands. 

## 3. Render child components / a BrowserRouter

In this test we will check that the menu contains an entry called "find", which when clicked, would direct to the url `/find`. We cannot simply render the menu - if we try and do this we will get an error, because the menu contains Link elements, which can only exist inside a BrowserRouter. So we will render both a BrowserRouter and a menu to complete the test.

1. **Create a file** in the PageHeader folder called `Menu.test.js`

2. **Add the following imports** to the top of this file:

```
import {render, screen} from "@testing-library/react";
```

3. Create a test method that will:
    - render the Menu component inside a BrowserRouter (you will need to import both of these)
    - find the link that contains the word "find" - you can do this with the `queryByText` fuction - check the documentation to see how to provide a part of the matching text only
    - check that the item has been found (use `toBeInTheDocument`)
    - check that the item contains an `href` attribute pointing to the `/find` url. (use `toHaveAttribute`)

When complete, the testing code should look like this:

```
import {render, screen} from "@testing-library/react";
import {BrowserRouter} from "react-router-dom";
import Menu from "./Menu";

test('menu contains the link to the search page', () => {
    render(<BrowserRouter>
        <Menu />
    </BrowserRouter>);
    const firstLink = screen.queryByText("Find", {exact: false} );
    expect(firstLink).toBeInTheDocument();
    expect(firstLink).toHaveAttribute('href','/find');
});
```

4. **Check that the test is passing** by viewing the console running the `npm test` command.


## 4. Testing with user interaction

In this test we will check that when a user types just spaces into the search box, the css class is applied that indicates an error.

1. **Open the file** called `Search.test.js`

2. **Add the following imports** to the top of this file:

```
import userEvent from "@testing-library/user-event";
```

3. Create a test method that will:
    - render the Search component
    - find the input (as in the earlier test)
    - simulate a user typing spaces into the input
    - check that the item found does have the class name assigned to it of `searchBoxError`

When complete, the testing code should look like this:

```
test('Invalid entry in input results in a search error', async () => {
    render(<Search/>);
    const input = screen.getByLabelText('Order Id:');
    await userEvent.click(input);
    await userEvent.keyboard('  ');
    expect(input).toHaveClass('searchBoxError');
})
```

4. **Check that the test is passing** by viewing the console running the `npm test` command.

5. We now have 2 related tests, so **wrap these into a describe function** . 

The code should now look like this:

```
import {render, screen} from "@testing-library/react";
import Search from "./Search";
import userEvent from "@testing-library/user-event";


describe('css applied correctly for search box validation', () => {

    test('check search initially has no class applied to it', () => {
        render(<Search/>);
        const input = screen.getByLabelText('Order Id:');
        expect(input).not.toHaveClass('searchBoxError');
    })

    test('Invalid entry in input results in a search error', async () => {
        render(<Search/>);
        const input = screen.getByLabelText('Order Id:');
        await userEvent.click(input);
        await userEvent.keyboard('  ');
        expect(input).toHaveClass('searchBoxError');
    })

});
```

## 5. Using promises & Mocking 1

In our Search component, when the user clicks on the search button, we expect the parent component's setSearchTerm function to be called. We will now test that the method is called, and that the correct data is passed to the parent component.

1. **Open the file** Search.test.js

2. **Create a test method"" that will:
  - create a mock function for the parent's setSearchTerm function
  - render the Search component using the mock in its properties
  - simulate the user typing a search term into the box, and clicking the button.
  - check that the mock function was called with the search term as a parameter.
  
Note that this is testing different functionality to our earlier tests so it should sit outside the describe function we created earlier.

The completed test function should look like this:

```
test('search data is correctly sent to parent component', async () => {
    const mockSetSearchTerm = jest.fn();
    render(<Search setSearchTerm={mockSetSearchTerm} />);

    const input = screen.getByRole('textbox', { name: /order id:/i });
    const button = screen.getByRole('button', { name: /search/i });

    await userEvent.click(input);
    await userEvent.keyboard('123');
    await userEvent.click(button);

    expect(mockSetSearchTerm).toHaveBeenCalledWith('123');
});
```


## 6. Using promises & Mocking 2

We will now test that the countrySelector component appears on the screen within 2 seconds. Then we will expand this test to check that it contains the expected data.

1. **Create a file** in the Transactions folder called `Transaction.test.js`

2. **Add the following imports** to the top of this file:

```
import {render, screen} from "@testing-library/react";
```

3. **Create a test method** that will:
    - render the Transactions component - this will need to be inside a BrowserRouter
    - find the element by role `combobox` - use the `findByRole` fuction. Wait up to 2 seconds for the item to be found.You can use an empty javascript object for the second parameter.
    - check that the item has been found.

At this point, the testing code should look like this:

```
import {render, screen} from "@testing-library/react";
import Transactions from "./Transactions";
import {BrowserRouter} from "react-router-dom";

test('countries are displayed when loaded', async () => {
    render(<BrowserRouter>
        <Transactions/>
    </BrowserRouter>);

    const countrySelector = await screen.findByRole('combobox', {}, 2000);   //note could find by ID but this is a chance to use findByRole!
    expect(countrySelector).toBeInTheDocument();
});
```

4. **Check the status of the test** by viewing the console running the `npm test` command. You will find that the test is failing with an error that axios cannot be imported.

5. **Add a mock** for the `DataFunctions.js` file, providing an override for the `getCountries` method. This method should return a promise of an array of strings. Use any 3 strings.

The testing code should look like this:

```
import {render, screen} from "@testing-library/react";
import Transactions from "./Transactions";
import {BrowserRouter} from "react-router-dom";

jest.mock('../../../data/DataFunctions', () => {
    return {
        getCountries: () => Promise.resolve({data: ['a', 'b', 'c']})
    };
});

test('countries are displayed when loaded', async () => {
    render(<BrowserRouter>
        <Transactions/>
    </BrowserRouter>);

    const countrySelector = await screen.findByRole('combobox', {}, 2000);  
    expect(countrySelector).toBeInTheDocument();
});
```

6. **Check that the test is passing** by viewing the console running the `npm test` command.

7. Now add in an addition check that the country selector contains 4 options (the items we returned from the promise and the `--select--`)

The final testing code should look like this:

```
import {render, screen} from "@testing-library/react";
import Transactions from "./Transactions";
import {BrowserRouter} from "react-router-dom";

jest.mock('../../../data/DataFunctions', () => {
    return {
        getCountries: () => Promise.resolve({data: ['a', 'b', 'c']})
    };
});

test('countries are displayed when loaded', async () => {
    render(<BrowserRouter>
        <Transactions/>
    </BrowserRouter>);

    const countrySelector = await screen.findByRole('combobox', {}, 2000);   //note could find by ID but this is a chance to use findByRole!
    expect(countrySelector).toBeInTheDocument();
    expect(countrySelector.options).toHaveLength(4);
});
```





