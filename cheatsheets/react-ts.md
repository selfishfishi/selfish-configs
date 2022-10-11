## Typescript
- Type script has interfaces. You define them the same way you define objects but with types instead of values
	- There are types too but should use interfaces most of the time. 
	- Type is mostly enum that can have different underlying types
	- The key distinction is that a type cannot be re-opened to add new properties vs an interface which is always extendable.
- You can define a class that implements the interface (implicitly).
- Typescript also has generics
- Genereally typeof and instanceof are not useful since they give you the underlying type at JS runtime (and not TS runtime)
- You can use `varName?: string` in a function to indicate that varName is optional. Because accessing a non existing function returns `undefined`, you need to explicitly check for undefined when accessing an optional function param
- Type Assertion: `const myCanvas = document.getElementById("main_canvas") as HTMLCanvasElement;` or `const myCanvas = <HTMLCanvasElement>document.getElementById("main_canvas");`
- The `as const` suffix acts like const but for the type system, ensuring that all properties are assigned the literal type instead of a more general version like string or number.
```
const req = { url: "https://example.com", method: "GET" } as const;
handleRequest(req.url, req.method);
``` 

In this example Typescript stops complaining on handleRequest that req.method could be any string

- unknown is a similar, but safer alternative to any. You have to try to specifically cast unknown later
- The readonly keyword can prevent arrays from being changed: `const names: readonly string[] = ["Dylan"];`
- Tuples: `let ourTuple: [number, boolean, string];` or `const graph: [x: number, y: number] = [55.2, 41.3];`
- Index signatures can be used for objects without a defined list of properties.`const nameAgeMap: { [index: string]: number } = {}; nameAgeMap.Jack = 25; // no error`. Index signatures like this one can also be expressed with utility types like `Record<string, number>`.

- Interface extension: 
```
interface Rectangle {
  height: number,
  width: number
}

interface ColoredRectangle extends Rectangle {
  color: string
}
```

- Remember that you can type functions too
- TypeScript provides a convenient way to define class members in the constructor, by adding a visibility modifiers to the parameter. public constructor(private name: string) {}
- Similar to arrays, the readonly keyword can prevent class members from being changed.
- Interfaces (covered here) can be used to define the type a class must follow through the implements keyword.
- When *extending* (and not implementing) you can optionally use the override keyword for methods that you are overriding
- Partial changes all the properties in an object to be optional. Required changes all the properties in an object to be required.jj

```
let pointPart: Partial<Point> = {}; // `Partial` allows x and y to be optional
```

- Record is a shortcut to defining an object type with a specific key type and value type.
```
const nameAgeMap: Record<string, number> = {
  'Alice': 21,
  'Bob': 25
};
```
- Omit removes keys from an object type. Pick removes all but the specified keys from an object type.Exclude removes types from a union.
- ReturnType extracts the return type of a function type. Parameters extracts the parameter types of a function type as an array.
- When used on an object type with explicit keys, keyof creates a union type with those keys.
- Optional Chaining is a JavaScript feature that works well with TypeScript's null handling. It allows accessing properties on an object, that may or may not exist, with a compact syntax. It can be used with the ?. operator when accessing properties.

# General JS
- You have to be careful about the meaning of this in JSX callbacks. In JavaScript, class methods are not bound by default. If you forget to bind this.handleClick and pass it to onClick, this will be undefined when the function is actually called.
	- This is not React-specific behavior; it is a part of how functions work in JavaScript. Generally, if you refer to a method without () after it, such as onClick={this.handleClick}, you should bind that method.
- `console.log(`I am ${age} years old.`); // Template literal`
- The Symbol type is often used to create unique identifiers. Every symbol created with the Symbol() function is guaranteed to be unique
- const declarations only prevent reassignments — they don't prevent mutations of the variable's value, if it's an object.
- let and const declared variables still occupy the entire scope they are defined in, and are in a region known as the temporal dead zone before the actual line of declaration.
- For equality, the double-equals operator performs type coercion if you give it different types, with sometimes interesting results. On the other hand, the triple-equals operator does not attempt type coercion, and is usually preferred.
- The && and || operators use short-circuit logic, which means whether they will execute their second operand is dependent on the first. This is useful for checking for null objects before accessing their attributes: `const name = o && o.getName();` Or for caching values (when falsy values are invalid): `const name = cachedName || (cachedName = getName());`
- JavaScript also contains two other prominent for loops: for...of, which iterates over iterables, most notably arrays, and for...in, which visits all enumerable properties of an object.
- Objects are passed by reference. This also means two separately created objects will never be equal (!==), because they are different references. If you hold two references of the same object, mutating one would be observable through the other.
- Out-of-bounds indexing doesn't throw. If you query a non-existent array index, you'll get a value of undefined in return:
- Functions can be called with more or fewer parameters than it specifies. If you call a function without passing the parameters it expects, they will be set to undefined. If you pass more parameters than it expects, the function will ignore the extra parameters.ou
- There are a number of other parameter syntaxes available. For example, the rest parameter syntax allows collecting all the extra parameters passed by the caller into an array, similar to Python's *arg: `function avg(...args)`. If a function accepts a list of arguments and you already hold an array, you can use the spread syntax in the function call to spread the array as a list of elements. For instance: avg(...numbers).
- 
```
const obj = { a: 1, b: 2 };
const { a, b } = obj;
// is equivalent to:
// const a = obj.a;
// const b = obj.b;
```
- There are differences between arrow functions and traditional functions, as well as some limitations:
	- Arrow functions don't have their own bindings to this, arguments or super, and should not be used as methods.
	- Arrow functions don't have access to the new.target keyword.
	- Arrow functions aren't suitable for call, apply and bind methods, which generally rely on establishing a scope.
	- Arrow functions cannot be used as constructors.
	- Arrow functions cannot use yield, within its body.

- Static properties are created by prepending static. Private properties are created by prepending a hash # (not private). The hash is an integral part of the property name. (Think about # as _ in Python.)
-async/await, which is a syntactic sugar for Promises
```
async function readFile(filename) {
  const content = await fs.readFile(filename);
  console.log(content);
}
```

- Modules: 
```
import { foo } from "./foo.js";

// Unexported variables are local to the module
const b = 2;

export const a = 1;
```
- We sometimes use => to define "arrow functions". They're like regular functions, but shorter. For example, x => x * 2 is roughly equivalent to function(x) { return x * 2; }. Importantly, arrow functions don't have their own this value so they're handy when you want to preserve the this value from an outer method definition.
- by using x = {[name] : value } you are doing compound property name an
- With arrow functions, the this keyword always represents the object that defined the arrow function. In regular functions the this keyword represented the object that called the function, which could be the window, the document, a button or whatever

## React
- Don’t put quotes around curly braces when embedding a JavaScript expression in an attribute. You should either use quotes (for string values) or curly braces (for expressions), but not both in the same attribute.
- Always start component names with a capital letter.
- We recommend naming props from the component’s own point of view rather than the context in which it is being used.
- All React components must act like pure functions with respect to their props.
- Class components should always call the base constructor with props.
- Three things about setState: 
	- Do not modify State Directly and instead use setState function
	- setState() may be async and batched with a bunch of other components changes. To sequence things, use a form of setState that takes a function with previous value and new value. This function will get called when the state is actually getting updated
	- multiple setState() calls will be merged

- Keys help React identify which items have changed, are added, or are removed. Keys should be given to the elements inside the array to give the elements a stable identity:
- We don’t recommend using indexes for keys if the order of items may change. This can negatively impact performance and may cause issues with component state.
- A good rule of thumb is that elements inside the map() call need keys.
- An input form element whose value is controlled by React in this way is called a “controlled component”. To do this, bind the value element of forms to react state and make sure you prevent the defaults by calling e.preventDefault() 
- setState() automatically merges a partial state into the current state,
- Often, several components need to reflect the same changing data. We recommend lifting the shared state up to their closest common ancestor. Let’s see how this works in action
- In React, sharing state is accomplished by moving it up to the closest common ancestor of the components that need it. You then pass a function down to be called when the prop changes
- React has a powerful composition model, and we recommend using composition instead of inheritance to reuse code between components.
- Props.children give you all the html components passed to a component
- One such technique is the single responsibility principle, that is, a component should ideally only do one thing. If it ends up growing, it should be decomposed into smaller subcomponents.
- Always start your react code with static components (hardcoded props) and then add dynamicis. Don't use state at all
- Using memo will cause React to skip rendering a component if its props have not changed. `export default memo(Todos);` . If you do this in the component, it will not render again if the parent re-renders unless the subcomponent (todos) itself changes

## Hooks
- They let you use state and other React features without writing a class. Hooks are functions that let you “hook into” React state and lifecycle features from function components.
- Hooks rules: 
	- Only call Hooks at the top level. Don’t call Hooks inside loops, conditions, or nested functions.
	- Only call Hooks from React function components. Don’t call Hooks from regular JavaScript functions. (There is just one other valid place to call Hooks — your own custom Hooks. We’ll learn about them in a moment.)ll
- You can use custom react hooks to reuse stateful logic between components
- Custom Hooks are more of a convention than a feature. If a function’s name starts with ”use” and it calls other Hooks, we say it is a custom Hook. The useSomething naming convention is how our linter plugin is able to find bugs in the code using Hook
- If you’re familiar with React class lifecycle methods, you can think of useEffect Hook as componentDidMount, componentDidUpdate, and componentWillUnmount combined.
- Does useEffect run after every render? Yes! By default, it runs both after the first render and after every update.
- So how does React know which state corresponds to which useState call? The answer is that React relies on the order in which Hooks are called
- usually you’ll want to declare functions needed by an effect inside of it
- When state is updated, the entire state gets overwritten. Because we need the current value of state, we pass a function into our setCar function. This function receives the previous value```
  const updateColor = () => {
    setCar(previousState => {
      return { ...previousState, color: "blue" }
    });
  }
```.
- For effect hooks, the dependency `[]` runs only on the first render. Make sure you pass dependencies here
- Use context to pass global variables like User or dependency injections without prop drilling
- The useRef Hook allows you to persist values between renders. It is a state that doesn't get called at each render
- The React useCallback Hook returns a memoized callback function.
- The useCallback Hook only runs when one of its dependencies update. The useMemo and useCallback Hooks are similar. The main difference is that useMemo returns a memoized value and useCallback returns a memoized function. You can learn more about useCallback in the useCallback chapter.


## CSS
- Select for class with `.` for children with `<space>` for id with `#` , state with `:`
- You can also style a state `a:hover` or `a:visited`
- You can also use simple functions in CSS: `width: calc(90%-30px) //90% of the parent box minus 30px`. `transform` property also takes functions
- `@` is called a rule and instructs CSS how it should perform. e.g: `@media (min-width: 30em)`
- CSS provides five special universal property values for controlling inheritance. Every CSS property accepts these values: 
	- `inherit`: CSS provides five special universal property values for controlling inheritance. Every CSS property accepts these values.
	- `initial`: Initial value
	- `revert`: Default provided by the browser
	- `revert-layer`: default provided by the layer above
- Attribute selectors gives you different ways to select elements based on the presence of a certain attribute on an element: `a[href="https://example.com"]
- It also includes pseudo-elements, which select a certain part of an element rather than the element itself. `p::first-line`
- The final group of selectors combine other selectors in order to target elements within our documents. `article > p` 
- The adjacent sibling selector (+) is placed between two CSS selectors. It matches only those elements matched by the second selector that are the next sibling element of the first selector. For example, to select all <img> elements that are immediately preceded by a <p> element: `p + img`
- if you want to select siblings of an element even if they are not directly adjacent, then you can use the general sibling combinator (~). To select all <img> elements that come anywhere after <p> elements, we'd do this:
-  Changing the value of the display property can change whether the outer display type of a box is block or inline. This changes the way it displays alongside other elements in the layout.
- If a box has an outer display type of block, then: The box will break onto a new line. The width and height properties are respected. Padding, margin and border will cause other elements to be pushed away from the box. The box will extend in the inline direction to fill the space available in its container. In most cases, the box will become as wide as its container, filling up 100% of the space available.
- If a box has an outer display type of inline, then: The box will not break onto a new line. The width and height properties will not apply. Vertical padding, margins, and borders will apply but will not cause other inline boxes to move away from the box. Horizontal padding, margins, and borders will apply and will cause other inline boxes to move away from the box.
- In the alternative box model, any width is the width of the visible box on the page. The content area width is that width minus the width for the padding and border (see image below). No need to add up the border and padding to get the real size of the `box-sizing: border-box`
- display: inline-block is a special value of display, which provides a middle ground between inline and block. Use it if you do not want an item to break onto a new line, but do want it to respect width and height and avoid the overlapping seen above. It does not, however, break onto a new line, and will only become larger than its content if you explicitly add width and height properties.
- To crop content when it overflows, you can set overflow: hidden. To scroll: `overflow: scroll`
- When you use margin and padding set in percentages, the value is calculated from the inline size of the containing block — therefore the width when working in a horizontal language. 
- As we learned in our previous lesson, a common technique is to make the max-width of an image 100%. This will enable the image to become smaller in size than the box but not larger.
- The object-fit property can help you here. When using object-fit the replaced element can be sized to fit a box in a variety of ways.
- Just keep in mind that replaced elements, when they become part of a grid or flex layout, have different default behaviors, essentially to avoid them being stretched strangely by the layout.
- Custom properties (sometimes referred to as CSS variables or cascading variables) are entities defined by CSS authors that contain specific values to be reused throughout a document. They are set using custom property notation (e.g., --main-color: black;) and are accessed using the var() function (e.g., color: var(--main-color);).
