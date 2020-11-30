# Null vs Undefined

---

## Null

> I call it my billion-dollar mistake  
> \- Tony Hoare, inventor of ALGOL W.

--

## Null

- is one of the 6 primitive types<!-- .element: class="fragment" -->
- is an object type ??!!<!-- .element: class="fragment" -->
- it's a bug according to Brendan Eich<!-- .element: class="fragment" -->

--

## Null

```typescript []
const a = null;
typeof a; // "object"
Number(a); // 0
0 == null; // false
```

---

## Undefined

> If null is the billion dollar mistake, undefined only doubles our losses  
> \- Daniel Rosenwasser, Program Manager, TypeScript

- https://devblogs.microsoft.com/typescript/announcing-typescript-2-0/

--

## Undefined

![Image](./NullVsUndefined/Javascript_rating.png)
https://www.lucidchart.com/techblog/2015/08/31/the-worst-mistake-of-computer-science/

--

## Undefined

- is also one of the 6 primitive types<!-- .element: class="fragment" -->
- makes Javascript worse<!-- .element: class="fragment" -->
- unique to Javascript only<!-- .element: class="fragment" -->
- cannot be transmitted using JSON<!-- .element: class="fragment" -->

--

## Undefined

```typescript []
let a; // or const a = undefined;
typeof a; // "undefined"
Number(a); // NaN -> means Not-A-Number
typeof NaN; // "number"
NaN == undefined; // false
```

---

## Javascript

<ul>
  <li>avoid using undefined</li><!-- .element: class="fragment" -->
  <li>DOM APIs return null instead of undefined</li><!-- .element: class="fragment" -->
  <li>https://eslint.org/docs/rules/no-undef-init</li><!-- .element: class="fragment" -->
</ul>

---

## Typescript

<ul>
  <li>use undefined only</li>
  <li>https://palantir.github.io/tslint/rules/no-null-keyword/</li>
  <li>https://github.com/Microsoft/TypeScript/wiki/Coding-guidelines#null-and-undefined</li>
  <li>https://basarat.gitbooks.io/typescript/docs/javascript/null-undefined.html</li>
</ul>

---

## Typescript

```typescript []
interface IProfile {
  name: string;
  age: number;
}
function getProfile(id: undefined | number) {
  return db.getProfile(id);
}
function getProfile2(id?: number) {
  return db.getProfile(id);
}

getProfile(); // ts error
getProfile(undefined); // ok
getProfile(1); // ok
getProfile2(); // ok
getProfile2(undefined); // ok
getProfile2(1); // ok
```

---

## Tab vs Space?

- not really, it's bigger than that<!-- .element: class="fragment" -->
- this is not a styling opinion where there is no right or wrong<!-- .element: class="fragment" -->
- this is programming to solve real world problem<!-- .element: class="fragment" -->

---

## Null vs Undefined

```typescript []
null == undefined; // true
isNil(null); // true
isNil(undefined); // true
null === undefined; // false
isNull(undefined); // false
isUndefined(null); // false
JSON.stringify([null, null]) // "[null,null]"
JSON.stringify([undefined, undefined]) // "[null,null]"
```

--

## Null vs Undefined

<ul>
  <li>when you get `null` value, most likely it's intended by programmer to set something as a value of nothing</li><!-- .element: class="fragment" -->
  <li>when you get `undefined` value, most likely it's not part of the schema or any column in database table</li><!-- .element: class="fragment" -->
  <li>but you can define a field as undefined in typescript😂</li><!-- .element: class="fragment" -->
</ul>

---

## Null only situation

```typescript []
sendToAPI(
  guid: string,
  profile: {address?: string | null}
): Promise<IProfile> {
  return postOrPutRequest({
    guid,
    ...profile,
  });
  // sending with null
  // {
  //   guid: '123',
  //   address: null,
  // }
  // sending with undefined and json is invalid structure
  // {
  //   guid: '123',
  // }
}
```

--

## Null only situation #2

```typescript []
knexUpdateProfile(
  guid: string,
  profile: {address?: string | null}
): Promise<IProfile> {
  return Profile.query()
    .patchAndFetchById(guid, profile);
  // sending with null can set the address to null
  // sending with undefined will not doing anything
}
```

--

## Undefined only situation

- NONE!!<!-- .element: class="fragment" -->
- because undefined means the field doesn't exist<!-- .element: class="fragment" -->

--

## Undefined only situation
```typescript []
interface IProfile {
  name: string;
  address? string;
}
function getProfile(): IProfile {
  return {
    name: 'user',
    // address: 'undefined' // why this?
  };
}
```

---

## GraphQL uses NULL for nullable field

- can't return undefined in JSON
- so to keep the JSON structure complete, null must be returned

--

## GraphQL.js

- accepts **null** and **undefined** for arguments/input

```graphql []
type Mutation {
  test(testString: String): TestStructure
}

type TestStructure {
  test1(testString: String): Boolean
}
```

--

## Resolver

```typescript []
const resolvers: IResolvers = {
  Mutation: {
    test: (_, {testString: string | null | undefined})
    => {
      console.log('test', {testString});
      return {};
    },
  },
  TestStructure: {
    test1: (_, {testString: string | null | undefined})
    => {
      console.log('test1', {testString});
      return true;
    },
  },
};
```

--

## Data from client

<div>

```graphql []
mutation {
  a: test(testString: "123") {
    test1(testString: "123")
  }
  b: test(testString: "") {
    test1(testString: "")
  }
  c: test(testString: null) {
    test1(testString: null)
  }
  d: test {
    test1
  }
}
```

```javascript []
test {testString: '123'}
test1 {testString: '123'}
test {testString: ''}
test1 {testString: ''}
test {testString: null}
test1 {testString: null}
test {testString: undefined}
test1 {testString: undefined}
```

</div><!-- .element: style="margin: 0 auto; height: 75vh; overflow-y: auto;" -->

---

## Why you shouldn't mix null / undefined

- it's like you shouldn't use 0/1 to replace true/false<!-- .element: class="fragment" -->
- many libraries like lodash / knex react differently when you use null/undefined<!-- .element: class="fragment" -->
- because you are javascript programmer, whether you are using typescript or not<!-- .element: class="fragment" -->
