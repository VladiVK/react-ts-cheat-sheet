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
