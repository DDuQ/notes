# Definition

A bundler turns project files (JS, TS, JSX, CSS, images) into optimized browser-ready files by resolving imports, transpiling code, and packaging everything efficiently.

## Module Resolution
The bundler analyzes your source code, follows every `import` and `export`, and builds a dependency graph.  

This includes:
- Locating modules (local files, node_modules, etc.)
- Understanding different import formats (ES Modules, CommonJS, etc.)
- Detecting unused modules for optimization

>[!INFO] Goal 
>Understand _what depends on what_ and load files in the correct order.

## Transpilation
Modern JavaScript features and languages like TypeScript or JSX are converted into code that browsers understand.  

This step uses tools like **Babel**, **SWC**, or **esbuild**.

Common transformations:

- TypeScript → JavaScript
- JSX → JavaScript
- ESNext → ES5 for compatibility
- Stripping decorators, private fields, optional chaining, etc.

>[!INFO] Goal
>Ensure your code runs on the widest range of browsers.

## Optimization
Produces the smallest, fastest version of your application by applying a set of techniques:
### Tree shaking
Removes unused code (code that is not being referenced or used by other parts of the project).
### Minification/Uglification
**Minification** -> Takes the code and ==removes any unnecessary data== while keeping functionality (blank spaces, comments, line breaks, unused code, etc.). Ending up in a one-liner file with our code.

**Uglification** -> ==Makes source code unreadable== by *renaming methods*, *flattening code*, *rewriting logic* and *restructuring expressions*.

>[!INFO]
>Uglification is a step further from Minification in order to provide a sense of encryption to our code.

## Bundling
All processed modules are combined into one or more browser-ready files.

Bundling ensures:
- Correct execution order
- Efficient loading
- Stable caching with hashed filenames

> [!INFO] Goal
> Deliver a final set of files the browser can load efficiently.