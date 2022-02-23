# React TypeScript Reference

[FM Introduction link:](https://stevekinney.github.io/react-and-typescript/)

```
https://stevekinney.github.io/react-and-typescript/
```

## 1. Props

Помним, что пропсы - это объект!

```
type Greetprops = {
  name: string;
};
const Greet = (props: Greetprops) => {
  return (
    <div>
      <h2>Wellcome, {props.name}</h2>
    </div>
  );
};

```

Теперь передаем точно по типу пропсы из главного компонента!

```

App:
 <Greet name={'Louis-Ferdinand'} />

```

Расширим!

```
App:

<Greet name={'Louis-Ferdinand'} messageCount={10} isLoggedIn={true} />
```

```
type Greetprops = {
  name: string;
  messageCount?: number;
  isLoggedIn: boolean;
};
const Greet = (props: Greetprops) => {
  const { messageCount = 0 } = props;
  return (
    <div>
      {props.isLoggedIn ? (
        <h2>
          Wellcome, {props.name}. You have {messageCount} unread messages!
        </h2>
      ) : (
        <h2>Wellcome, guest!</h2>
      )}
    </div>
  );
};
```

### Props as object

```
App:

const personName = {
    firstName: 'Louis-Ferdinand',
    lastName: 'Celine',
  };

<Person name={personName} />
```

```
type PersonProps = {
  name: { firstName: string; lastName: string };
};
const Person = (props: PersonProps) => {
  return (
    <div>
      {props.name.firstName} {props.name.lastName}
    </div>
  );
};

```

### Props as array

```
App:

const personsList = [
    { firstName: 'Louis-Ferdinand', lastName: 'Celine' },
    { firstName: 'Bob', lastName: 'Marley' },
    { firstName: 'Homer', lastName: 'Simpson' },
];

<PersonsList personsList={personsList} />
```

```
type PersonsListProps = {
  personsList: { firstName: string; lastName: string }[];
};

const PersonsList = (props: PersonsListProps) => {
```

### Advanced Props

```
App:

<Status status='success' />

```

```
type StatusProps = {
  status: 'success' | 'loading' | 'error';
};

type StatusesKit = {
  success: string;
  error: string;
  loading: string;
};
const statuses: StatusesKit  = {
  success: 'Data fetched successfully',
  error: 'Error fetching data',
  loading: 'Loading...',
};

const getStatus = (kit: StatusesKit, status: keyof StatusesKit) => {
  return kit[status];
};
const Status = ({ status }: StatusProps) => {
  let message = getStatus(statuses, status);
  return (
    <div>
      <h2>Status - {message}</h2>
    </div>
  );
};

```

### Children as props

1. Simple example!

```
App:

<Heading>
  Hello, Children as Props
</Heading>

```

```
type HeadingProps = {
  children: string;
};
const Heading = (props: HeadingProps) => <h2>{props.children}</h2>;
```

2. Children Props as ReactNode

```
App:

  <Oscar>
    <Heading> Hello, Children as ReactNode Props </Heading>
  </Oscar>

```

```
type OscarProps = {
  children: ReactNode;
};
const Oscar = (props: OscarProps) => {
  return <div>{props.children}</div>;
};
```

### Event Props

```
App:

<Button handleClick={() => console.log('handleClick')} />

```

```
type ButtonProps = {
  handleClick: () => void;
}

const Button = (props: ButtonProps) => {
  return <button onClick={props.handleClick}>Button</button>;
}
```

Example with Event

```
App:

<Button handleClick={(e) => console.log('handleClick', e.target)} />
```

```
type ButtonProps = {
  handleClick: (e: React.MouseEvent<HTMLButtonElement>) => void;
};
const Button = (props: ButtonProps) => {
  return <button onClick={props.handleClick}>Button</button>;
};
```

Example with Event & id

```
App:

<Button handleClick={(e, id) => console.log('handleClick', e.target, id)} />


```

```
type ButtonProps = {
  handleClick: (e: React.MouseEvent<HTMLButtonElement>, id: number) => void;
};
const Button = (props: ButtonProps) => {
  return <button onClick={(e) => props.handleClick(e, 1)}>Button</button>;
};
```

onChange Event:

```
App:

<Input value='' handleChange={(e) => console.log(e)} />

```

Variant with props as function

```
type InputProps = {
  value: string;
  handleChange: (e: React.ChangeEvent<HTMLInputElement>) => void;
};
const Input = (props: InputProps) => {
  return (
  <input type='text' value={props.value} onChange={props.handleChange} />

  );
};

```

Varian with function inside component

```
type InputProps = {
  value: string;
};
const Input = (props: InputProps) => {
  const handleOnchangeEvent = (e: React.ChangeEvent<HTMLInputElement>) => {
    console.log(e);
  };

  return (

    <input type='text' value={props.value} onChange={handleOnchangeEvent} />
  );
};
```

### Styles as Props

```
App:

<ContainerStyles styles={{ border: '1px solid coral', padding: '1rem' }} />

```

```
type ContainerStylesProps = {
  styles: React.CSSProperties;
};
const ContainerStyles = (props: ContainerStylesProps) => {
  return <div style={props.styles}>Container wit styles as props</div>;
};
```

### Props Tips

1. Destructing props: ( { name, age } : PersonProps)

2. Export/Import

```
Person.types.ts

export type PersonProps = {
  name: string;
  age: number;
}

import {PersonProps} from /....

```

3. Reusing types

```
export type Name = {
  firstName: string;
  lastName: string
}

export PersonProps = {
  name: Name;
  age: number;
}
```

## Typing Hooks

---

## Typing useState hook

- remember: if we have an object `useState< Person | null >(null)`

we should use optional chaining operator `person?.name`

because our state can be also `null` !!!

```
type UserAuth = {
  name: string;
  email: string;
};
const User = () => {
  const [user, setUser] = useState< UserAuth | null >(null);
  const handleLogin = () => {
    setUser({
      name: 'Bob',
      email: 'bob@gmail.com',
    });
  };
  const handleLogout = () => setUser(null);
  return (
    <div>
      <button onClick={handleLogin}>Log In</button>
      <button onClick={handleLogout}>Log Out</button>

      <h3>User name is - {user?.name}</h3>
      <h3>User email is - {user?.email}</h3>
    </div>
  );
};

```

### - useState Type Assertion

If we are 100% sure that our data will be correct

we can use `{} as UserAuth`

and do not check object with optional chaining

```
const [user, setUser] = useState< UserAuth >( {} as UserAuth );
  const handleLogin = () => {
    setUser({
      name: 'Bob',
      email: 'bob@gmail.com',
    });
  };

  return (
    <div>
      <button onClick={handleLogin}>Log In</button>
      <h3>User name is - {user.name}</h3>
      <h3>User email is - {user.email}</h3>
    </div>
  );
```

### - useReducer Type Assertion

- we can restrict types by interconection between types

```
App:
<Counter />
```

```
type CounterState = {
  count: number;
};
type UpdateAction = {
  type: 'increment' | 'decrement';
  payload: number;
};
type ResetAction = {
  type: 'reset';
};
type CounterAction = UpdateAction | ResetAction;

const initialSate = { count: 0 };

function reducer(state: CounterState, action: CounterAction) {
  switch (action.type) {
    case 'increment':
      return { count: state.count + action.payload };
    case 'decrement':
      return { count: state.count - action.payload };
    case 'reset':
      return initialSate;

    default:
      return state;
  }
}

const Counter = () => {
  const [state, dispatch] = useReducer(reducer, initialSate);
  return (
    <>
      <h2>Count: {state.count}</h2>
      <button onClick={() => dispatch({ type: 'increment', payload: 10 })}>
        increment
      </button>
      <button onClick={() => dispatch({ type: 'decrement', payload: 10 })}>
        decrement
      </button>
      <button onClick={() => dispatch({ type: 'reset' })}>reset</button>
    </>
  );
};

```

### - Context API

```
App:

<ThemeContextprovider>
  <Box />
</ThemeContextprovider>

```

```
theme.ts

export const theme = {
  primary: {
    main: '#3f51b5',
    text: '#fff',
  },
  secondary: {
    main: '#f50057',
    text: '#fff',
  },
};

```

```
ThemeContext.tsx

import { createContext } from 'react';
import { theme } from './theme';

type ThemeContextProviderprops = {
  children: React.ReactNode;
};
export const ThemeContext = createContext(theme);

export const ThemeContextprovider = (props: ThemeContextProviderprops) => {
  return (
    <ThemeContext.Provider value={theme}>
      {props.children}
    </ThemeContext.Provider>
  );
};

```

```
<Box />

import React, { useContext } from 'react';
import { ThemeContext } from './ThemeContext';

const Box = () => {
  const theme = useContext(ThemeContext);
  return (
    <div
      style={{
        backgroundColor: theme.secondary.main,
        color: theme.secondary.text,
      }}
    >
      Box
    </div>
  );
};


```

- Variation with uknown state

```
App:

  <UserContextProvider>
    <User />
  </UserContextProvider>
```

```
import React, { useState, createContext, ReactNode } from 'react';

export type UserAuth = {
  name: string;
  email: string;
};

type UserContextType = {
  user: UserAuth | null;
  setuser: React.Dispatch<React.SetStateAction<UserAuth | null>>;
};

type UserContextProviderProps = {
  children: ReactNode;
};

// if do not know the initial value we set "null"
export const UserContext = createContext<UserContextType | null>(null);

export const UserContextProvider = ({ children }: UserContextProviderProps) => {
  const [user, setuser] = useState<UserAuth | null>(null);
  return (
    <UserContext.Provider value={{ user, setuser }}>
      {children}
    </UserContext.Provider>
  );
};

```

```
import React, { useContext } from 'react';
import { UserContext } from './UserContext';

const User = () => {
  const userContext = useContext(UserContext);
  const handleLogin = () => {
    if (userContext) {
      userContext.setuser({
        name: 'Bob',
        email: 'bob@gmail.com',
      });
    }
  };
  const handleLogout = () => {
    if (userContext) {
      userContext.setuser(null);
    }
  };
  return (
    <div>
      <button onClick={handleLogin}>login</button>
      <button onClick={handleLogout}>logout</button>
      <h3>User name is {userContext?.user?.name}</h3>
      <h3>User email is {userContext?.user?.email}</h3>
    </div>
  );
};
```

We always check: if (userContext) ...

And check: {userContext?.user?.name}

All problems are because we specify null as initial value !!!

But we can (very often pattern !!!) use next variant !!!

```
UserContext.tsx

export const UserContext = createContext({} as UserContextType);

```

```
const User = () => {
  const { user, setuser } = useContext(UserContext);
  const handleLogin = () => {
    setuser({
      name: 'Bob',
      email: 'bob@gmail.com',
    });
  };
  const handleLogout = () => setuser(null);

  return (
    <div>
      <button onClick={handleLogin}>login</button>
      <button onClick={handleLogout}>logout</button>
      <h3>User name is {user?.name}</h3>
      <h3>User email is {user?.email}</h3>
    </div>
  );
};
```
