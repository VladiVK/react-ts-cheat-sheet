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

### - useRef hook

We use `useRef` hook in 2 variants:

1. when we just read value - specify DOM element type like `HTMLInputElement`

2. if we deal with mutable value - specify correct types like `useRef<number | null>(null)`

```
import React, { useRef, useEffect } from 'react';

const DomRef = () => {
  const inputRef = useRef<HTMLInputElement>(null);

  useEffect(() => {
    inputRef.current?.focus();
  }, []);

  return (
    <div>
      <input type='text' ref={inputRef} />
    </div>
  );

};

export default DomRef;
```

If we are sure that our reference is never be null,

we can add after `null` an exclamaition sign `null!`

This let us use `focus() `without optional chaining

```
 const inputRef = useRef<HTMLInputElement>(null!);

useEffect(() => {
    inputRef.current.focus();
  }, []);



```

IMPORTANT !!! Use word `window`:

`window.setInterval` and `window.clearInterval`

```
import { useState, useRef, useEffect } from 'react';

const MutableRef = () => {
  const [timer, setTimer] = useState(0);
  const intervalRef = useRef<number | null>(null);

  const stopTimer = () => {

    // we use `null` so we need check value !!!

    if (intervalRef.current) window.clearInterval(intervalRef.current);
  };

  useEffect(() => {
    intervalRef.current = window.setInterval(() => {
      setTimer((prev) => prev + 1);
    }, 1000);

    return () => {
      stopTimer();
    };
  }, []);
  return (
    <div>
      MutableRef: Timer hook: {timer}
      <button onClick={() => stopTimer()}>stop timer</button>
    </div>
  );
};
```

### - Class component

```
App:

<CounterClass message='Counter value is: '/>

```

```
import React, { Component } from 'react';

type CounterProps = {
  message: string;
};
type CounterState = {
  count: number;
};

export default class CounterClass extends Component<CounterProps, CounterState> {

  state = {
    count: 0,
  };

  handleClick = () => {
    this.setState((prevState) => {
      return { count: prevState.count + 1 };
    });
  };
  render() {
    return (
      <div>
        <button onClick={this.handleClick}>increment</button>
        {this.props.message} {this.state.count}
      </div>
    );
  }
}
```

If we do not have props, we just put an empty object:

```
export default class CounterClass extends Component<{}, CounterState> {
```

If we do not have state, we just miss that:

```
export default class CounterClass extends Component< CounterProps > { }
```

## Component as Prop

If we need to put component only - use `React.ComponentType`

If we need to put component with props - use `React.ComponentType< MyPropsType >`

Details see below:

```
App:

 <Private isLoggedIn={true} component={Profile} />
```

```
import React from 'react';

const Login = () => {
  return <div>Please login to continue! </div>;
};

export default Login;
```

```
import React from 'react';

export type ProfileProps = {
  name: string;
};

const Profile = ({ name }: ProfileProps) => {
  return <div>Private Profile Component. Name is {name}</div>;
};

```

```
import Login from './Login';
import { ProfileProps } from './Profile';

type PrivateProps = {
  isLoggedIn: boolean;
  component: React.ComponentType<ProfileProps>;
};

// just rename with destruction {component: Component}

const Private = ({ isLoggedIn, component: Component }: PrivateProps) => {
  return isLoggedIn ? <Component name='Louis' /> : <Login />;
};

```

## Generics

Let us avoid duplication

```
App:

<List items={['Louis', 'Bob', 'Homer']}
      onClick={(value) => console.log(value)}
/>

<List items={[1, 2, 3]} onClick={(value) => console.log(value)} />

```

```
type ListProps<T> = {
  items: T[];
  onClick: (value: T) => void;
};

const List = <T extends string | number>({ items, onClick }: ListProps<T>) => {
  return (
    <div>
      {items.map((item, index) => (
        <button
          style={{ display: 'block' }}
          key={index}
          onClick={() => onClick(item)}
        >
          {item}
        </button>
      ))}
    </div>
  );
};
```

With nums & string will be correct even with next notation:

```
const List = <T extends {}>({ items, onClick }: ListProps<T>) => {

```

Do more, extend our props:

```
App:

<List
    items={[
          { id: 1, fName: 'Bob' },
          { id: 2, fName: 'Homer' },
        ]}
    onClick={(value) => console.log(value)}
/>

```

```

type ListProps<T> = {
  items: T[];
  onClick: (value: T) => void;
};

const List = <T extends { id: number }>({ items, onClick }: ListProps<T>) => {
  return (
    <div>
      {items.map((item) => (
        <button
          style={{ display: 'block' }}
          key={item.id}
          onClick={() => onClick(item)}
        >

          // in this educational example we have a problem with obj,
          // so just for ouput some value we will stringify them

          {JSON.stringify(item)}

        </button>
      ))}
    </div>
  );
};

```
