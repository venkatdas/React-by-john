# Advanced topics of React


## How we setup Vite with React
- ```npm create vite@latest my-vue-app -- --template vue```
- Instead of vue replace it with react like
- ```npm create vite@latest my-react-app -- --template react ```
After that
- ```npm install```
- ```npm run dev```

### Table of content React hooks
- useState Hook.
- useEffect Hook.
- useRef Hook.
- useCallback Hook.
- useMemo Hook.
- useContext Hook.
- useReducer Hook.

## What is React Hooks?
- React Hooks are simple JavaScript functions that we can use to isolate the reusable part from a functional component. Hooks can be stateful and can manage side-effects.
- Hooks are a new addition in React 16.8. They let you use state and other React features without writing a class.

### useState Hook
- return an array with two elements : the current state value , and a funcion that we can use to update the state 
- accepts default value as an argument 
- state updates triggers re-render

![image](https://github.com/venkatdas/React-by-john/assets/43024084/905e56bc-b866-4a11-b8c0-bc9a1f6f0732)

![image](https://github.com/venkatdas/React-by-john/assets/43024084/c33114dc-6dd0-4230-afd2-773ea69c2ddb)

- When ever you click the button it's keep on increasing the count.
### useState Array Example:
![image](https://github.com/venkatdas/React-by-john/assets/43024084/3bb57ac4-df4c-41f6-805a-70796ff3ce20)

- We are importing above data 
```Javascript
const UseStateArray = () => {
  const [people, updatedPeople] = useState(data);
  const RemoveItem = (id) => {
    const newItem = people.filter((person) => person.id !== id);
    updatedPeople(newItem);
  };
  const ClearAllItems = () => {
    updatedPeople([]);
  };
  return (
    <div>
      {people.map((value) => {
        const { name, id } = value;
        return (
          <div key={id}>
            <h4>{name}</h4>
            <button onClick={() => RemoveItem(id)} className="btn">
              remove
            </button>
          </div>
        );
      })}
      <button
        className="btn"
        style={{ marginTop: "2rem" }}
        onClick={ClearAllItems}
      >
        Clear Items
      </button>
    </div>
  );
};
```

- The Output is 
- ![image](https://github.com/venkatdas/React-by-john/assets/43024084/56f25fa2-88aa-4682-9f6d-4827ace330d5)

- Whenever if you click on the remove button you remove it from existing content and as well as if you click on the clear all it clears the entire screen.

### Inatiall Render VS Re-render
- In a React application, the initial render is the first time that the component tree is rendered to the DOM. It happens when the application first loads, or when the root component is first rendered. This is also known as "mounting" the components.
- Re-renders, on the other hand, happen when the component's state or props change and the component needs to be updated in the DOM to reflect these changes. React uses a virtual DOM to optimize the process of updating the actual DOM so that only the necessary changes are made.
#### There are a few ways that you can trigger a re-render in a React component:
- By changing the component's state or props. When the component's state or props change, React will re-render the component to reflect these changes.
- When the parent element re-renders, even if the component's state or props have not changed.
#### General rules of hooks

- starts with "use" (both -react and custom hooks)
- component must be uppercase
- invoke inside function/component body
- don't call hooks conditionally 
- set functions don't update state immediately 

#### Automatic Batching
- In React, "batching" refers to the process of grouping multiple state updates into a single update. This can be useful in certain cases because it allows React to optimize the rendering of your components by minimizing the number of DOM updates that it has to perform.
- By default, React uses a technique called "auto-batching" to group state updates that occur within the same event loop into a single update. This means that if you call the state update function multiple times in a short period of time, React will only perform a single re-render for all of the updates.
- React 18 ensures that state updates invoked from any location will be batched by default. This will batch state updates, including native event handlers, asynchronous operations, timeouts, and intervals.

### useEffect hook

- useEffect is a hook in React that allows you to perform side effects in function components.There is no need for urban dictionary - basically any work outside of the component. Some examples of side effects are: subscriptions, fetching data, directly updating the DOM, event listeners, timers, etc.
- It is basically a hook replacement for the "old-school" lifecycle methods componentDidMount, componentDidUpdate and componentWillUnmount.


- useEffect hook
- accepts two arguments (second optional)
- first argument - cb function
- second argument - dependency array
- by default runs on each render (initial and re-render)
- cb can't return promise (so can't make it async)
- if dependency array empty [] runs only on initial render

- useEffect will only runs inintial render
- ![useeffect](https://github.com/venkatdas/React-by-john/assets/43024084/98e69e93-5b01-48b3-aea4-43f5b66a8667)
![useeffect -output](https://github.com/venkatdas/React-by-john/assets/43024084/14d39eec-17c4-4683-a746-518d257b7424)

- From the above code the main difference between use Effect and normal function is as follows
- If we have just the plan function or we invoke the function inside of the component yes, it's going to run on INTIAL RENDER AND EVERY RE-RENDER.
- However, with useEffect we can start controlling  when this functionality runs  .
- Briefly, with useEffect we provide a callback function which is going to be invoked pretty much after every render, unless we provide here a dependency array. In that case , if we have dependecy array and if it's empty , the functionality inside of the useEffect is going to run once only when the component mounts on the initial render.

### Use Effect another example by fetching data

```javascript
const DataFetch = () => {
  const [users, setUsers] = useState([]);
  useEffect(() => {
    // fetch(url).then((resp) => {
    //   resp.json().then((data) => {
    //     setUsers(data);
    //   });
    // });
    const DataFetching = async () => {
      try {
        const resp = await fetch(url);
        const listOfData = await resp.json();
        setUsers(listOfData);
      } catch (error) {
        console.log(error);
      }
    };
    DataFetching();
  }, []);

  return (
    <section>
      <h2>Github user API</h2>
      <ul className="users">
        {users.map((user) => {
          const { id, login, avatar_url, html_url } = user;
          return (
            <li key={id}>
              <img src={avatar_url} alt={login} />
              <div>
                <h5>{login}</h5>
                <a href={html_url}>Profile</a>
              </div>
            </li>
          );
        })}
      </ul>
    </section>
  );
};
```

![image](https://github.com/venkatdas/React-by-john/assets/43024084/899c545e-efbd-4d55-a848-5ffe58ab5324)


### Conditional Rendering in React

- Your components will often need to display different things depending on different conditions. In React, you can conditionally render JSX using JavaScript syntax like if statements, &&, and ? : operators.
- In React, conditional rendering is the process of displaying different content based on certain conditions or states. It allows you to create dynamic user interfaces that can adapt to changes in data and user interactions

#### Why conditional rendering is necessary in react?

- **Improved User Experience:** Conditional rendering allows you to create dynamic user interfaces that adapt to changes in data and user interactions. By showing and hiding content based on the user's actions or the application state, you can create a more intuitive and engaging user experience.
- **Improved Performance:** By conditionally rendering content, you can avoid rendering unnecessary components and improve the performance of your application. This is particularly important in larger applications where unnecessary rendering can lead to performance issues.
- **Simplified Code:** Conditional rendering can help you simplify your code and make it more readable. By using conditional statements to decide what content should be rendered, you can avoid duplicating code and create more modular components.
- **Flexibility**: Conditional rendering allows you to create more flexible and customizable components. By rendering different content based on the application state, you can create components that can be used in different contexts and adapt to different user interactions.

#### Multiple returns using conditional rendering examples.


```javascript

const MultipleReturnsFetchData = () => {
  const [isLoading, setIsLoading] = useState(true);
  const [isError, setIsError] = useState(false);
  const [data, setData] = useState([]);
  useEffect(() => {
    const fetchUser = async () => {
      try {
        const resp = await fetch(url);
        console.log(resp);
        // if (!resp.ok) {
        //   setIsError(true);
        //   setIsLoading(false);
        //   return;
        // }
        const result = await resp.json();
        setData(result);
        // setIsLoading();
        //console.log(result);
      } catch (error) {
        setIsError(true);
        console.log(error);
      }
      setIsLoading(false);
      // console.log(result);
    };
    fetchUser();
  }, []);

  if (isLoading) {
    return <h2>Loading....</h2>;
  }
  if (isError) {
    return <h2>There was an error....</h2>;
  }

  return (
    <div>
      <img
        style={{ width: "150px", borderRadius: "25px" }}
        src={data.avatar_url}
        alt={data.name}
      />
      <h2>Fetch Data</h2>
      <h2>{data.name}</h2>
      <h4>works at {data.company}</h4>
      <p>{data.bio}</p>
    </div>
  );
};
export default MultipleReturnsFetchData;
```

- From the above code we are getting the 
![image](https://github.com/venkatdas/React-by-john/assets/43024084/6673b082-f730-4cbe-ac97-bcb0bc1a8ff0)
- From 189 line useState has twoparams like data, setData..
- From the fetch value we need to pass the API output to the setData() and for the future purpose call the data wherever it requires.

#### How to handle the Fetch errors 

- Unlike for example Axios, by default, the fetch() API does not consider** HTTP status codes in the 4xx or 5xx range to be errors**. Instead, it considers these status codes to be indicative of a successful request,

#### Example..
- Consider the above example lke if there is an any error in the URL, If you log the **resp** you can inspect the element 

![image](https://github.com/venkatdas/React-by-john/assets/43024084/77e35563-bbda-47a9-9cd1-bcc442a30002)

- From the above image if you observe you can still got the 200 status response but it can ot handle the 4XX or 5XX series errors.That's the reason we have to add the code 

```javascript
if (!resp.ok) {
         setIsError(true);
           setIsLoading(false);
           return;
         }

```
- From that piece of code you can handle the errors in fetch.
 
 
 ### React Hooks Rules
 
- Only Call Hooks at the Top Level:
Hooks should only be called at the top level of a functional component or another custom hook. They should not be called inside loops, conditions, or nested functions. This rule ensures that hooks are called consistently on every render and that the order of hooks is preserved.
- Call Hooks in Functional Components:
Hooks can only be called from functional components or custom hooks. They should not be called from regular JavaScript functions, class components, or event handlers.

- Use Hooks in the Same Order:
If you have multiple hooks in a component, make sure to use them in the same order on every render. This ensures that the React reconciler can correctly associate the state and updates with the corresponding hooks.

- Don't Call Hooks Conditionally:
Hooks should always be called unconditionally and not inside conditional statements. React relies on the order and number of hooks being consistent between renders, so conditional calls can lead to unexpected behavior.

- Use Hooks for Each Related Stateful Logic:
If you have multiple separate stateful logic in a component, use multiple hooks to manage each of them independently. Don't try to combine unrelated stateful logic into a single hook.

- Follow the Naming Convention:
React hooks are conventionally named with a prefix of "use" (e.g., useState, useEffect, useCustomHook). This naming convention helps differentiate hooks from regular functions and helps with readability and understanding.

- Don't Modify Hooks' Dependencies Array:
When using hooks like useEffect, ensure that the dependency array is not modified from within the component. Modifying the dependency array can cause unexpected behavior and should be avoided. Instead, provide a stable array of dependencies.


### Forms in react
- There are two ways to handle the data
- Controlled Components: In this approach, form data is handled by React through the use of hooks such as the useState hook.
- Uncontrolled Components: Form data is handled by the Document Object Model (DOM) rather than by React. The DOM maintains the state of form data and updates it based on user input.

- Let's focus on the **controlled Forms**

- In React, a controlled component is a component where form elements derive their value from a React state.

- When a component is controlled, the value of form elements is stored in a state, and any changes made to the value are immediately reflected in the state.

- To create a controlled component, you need to use the value prop to set the value of form elements and the onChange event to handle changes made to the value.

```javascript
import { useState } from "react";

const ControlledInputs = () => {
  const [name, setName] = useState("");
  const [email, setEmail] = useState("");

  // const handleNameClick = (e) => {
  //   // const name = e.target.name;
  //   // const value = e.target.value
  //   setName(e.target.value);
  // };

  const handleClick = (e) => {
    e.preventDefault();
    console.log(name, email);
  };
  return (
    <form className="form" onSubmit={handleClick}>
      <h4>controlled inputs</h4>
      <div className="form-row">
        <label htmlFor="name" className="form-label">
          name
        </label>
        <input
          type="text"
          className="form-input"
          id="name"
          value={name}
          onChange={(e) => setName(e.target.value)}
        />
      </div>
      <div className="form-row">
        <label htmlFor="email" className="form-label">
          Email
        </label>
        <input
          type="email"
          className="form-input"
          id="email"
          value={email}
          onChange={(e) => setEmail(e.target.value)}
        />
      </div>
      <button type="submit" className="btn btn-block">
        submit
      </button>
    </form>
  );
};
export default ControlledInputs;

```
![image](https://github.com/venkatdas/React-by-john/assets/43024084/8ffdd993-6360-411c-b803-8e7a591618d3)

- Let's understand the code from above example.
- onChange event we can write as different below

```javascript
const  handleChange = (event) => {
		setName(event.target.value);
	};
  onChange={handleChange} // it is same as line no 342
```





