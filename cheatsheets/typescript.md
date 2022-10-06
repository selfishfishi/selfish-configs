## Basics
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
