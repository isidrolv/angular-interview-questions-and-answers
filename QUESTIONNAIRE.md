# Angular Certification Questionnaire
## Google Angular University Based - 60 Questions

### Section 1: Angular Fundamentals (Questions 1-10)

#### 1. What is Angular?
   a. A JavaScript library for building user interfaces
   b. A TypeScript-based open-source web application framework
   c. A back-end framework for Node.js
   d. A CSS framework for responsive design

**Answer: b**

**Explanation:** Angular is a TypeScript-based open-source web application framework led by the Angular Team at Google and by a community of individuals and corporations. It is a complete rewrite from the same team that built AngularJS and is used for building dynamic single-page applications (SPAs).

#### 2. What is the difference between AngularJS and Angular?
   a. They are the same framework
   b. Angular is written in JavaScript, AngularJS in TypeScript
   c. AngularJS (1.x) uses JavaScript, Angular (2+) uses TypeScript and has a component-based architecture
   d. Angular is only for mobile apps

**Answer: c**

**Explanation:** AngularJS (version 1.x) is the first version of Angular, written in JavaScript with a controller-based architecture. Angular (2+) is a complete rewrite using TypeScript with a component-based architecture, better performance, and mobile support. The newer Angular versions use a component-based structure instead of the MVC pattern.

#### 3. Which of the following is the correct way to bootstrap an Angular application?
   a. angular.bootstrap()
   b. platformBrowserDynamic().bootstrapModule()
   c. app.bootstrap()
   d. ng.start()

**Answer: b**

**Explanation:** The correct way to bootstrap an Angular application is using `platformBrowserDynamic().bootstrapModule(AppModule)` in the main.ts file. This method compiles the application with the JIT (Just-in-Time) compiler and bootstraps the application's root module (AppModule) to run in the browser.

#### 4. What is a decorator in Angular?
   a. A design pattern for styling components
   b. A function that adds metadata to a class, method, property, or parameter
   c. A CSS preprocessor
   d. A testing utility

**Answer: b**

**Explanation:** Decorators are a TypeScript feature that allows you to attach metadata to classes, methods, properties, or parameters. In Angular, decorators like @Component, @Injectable, @NgModule, @Input, and @Output provide configuration metadata that determines how classes should be processed, instantiated, and used at runtime.

#### 5. Which Angular CLI command creates a new Angular project?
   a. ng create project-name
   b. ng init project-name
   c. ng new project-name
   d. ng generate project-name

**Answer: c**

**Explanation:** The command `ng new project-name` is used to create a new Angular workspace and an initial Angular application. It sets up the necessary files, installs npm packages, and configures TypeScript. You can add flags like --routing to include routing or --style to specify the stylesheet format.

#### 6. What is AOT compilation in Angular?
   a. Asynchronous Operation Testing
   b. Ahead-of-Time compilation - compiling during the build process
   c. After Operation Transformation
   d. Angular Optimization Tool

**Answer: b**

**Explanation:** AOT (Ahead-of-Time) compilation converts Angular HTML and TypeScript code into efficient JavaScript code during the build phase before the browser downloads and runs that code. This results in faster rendering, fewer asynchronous requests, smaller Angular framework download size, earlier detection of template errors, and better security.

#### 7. What is the purpose of NgModule in Angular?
   a. To create components
   b. To organize an application into cohesive blocks of functionality
   c. To handle HTTP requests
   d. To manage routing

**Answer: b**

**Explanation:** NgModule is a decorator that marks a class as an Angular module and provides configuration metadata. It organizes an application into cohesive blocks of functionality by consolidating components, directives, pipes, and services. Every Angular application has at least one NgModule class, the root module (AppModule), which bootstraps the application.

#### 8. What is the role of the main.ts file in an Angular application?
   a. It defines the main component
   b. It is the entry point that bootstraps the Angular application
   c. It contains the main routing configuration
   d. It defines global styles

**Answer: b**

**Explanation:** The main.ts file is the entry point of an Angular application. It bootstraps the root module (AppModule) using platformBrowserDynamic().bootstrapModule(AppModule). This file is responsible for starting the Angular application and can also enable production mode, configure error handlers, and set up other application-wide configurations.

#### 9. Which of the following is NOT a core building block of Angular?
   a. Components
   b. Modules
   c. Controllers
   d. Services

**Answer: c**

**Explanation:** Controllers were part of AngularJS (1.x) but are not part of Angular (2+). The core building blocks of modern Angular are: Modules (NgModule), Components, Templates, Metadata (Decorators), Data Binding, Directives, Services, and Dependency Injection. Angular uses a component-based architecture instead of the MVC pattern with controllers.

#### 10. What is the purpose of package.json in an Angular project?
   a. To configure TypeScript
   b. To define project dependencies and scripts
   c. To configure the Angular compiler
   d. To define component metadata

**Answer: b**

**Explanation:** The package.json file is a standard npm configuration file that defines the project's dependencies (both production and development), scripts for common tasks (like build, test, serve), project metadata (name, version, description), and other npm-related configurations. It's essential for managing the Node.js packages that the Angular application depends on.

### Section 2: Components and Templates (Questions 11-20)

#### 11. What is a component in Angular?
   a. A service that provides data
   b. A class with a @Component decorator that controls a view
   c. A module that groups related functionality
   d. A directive that modifies DOM elements

**Answer: b**

**Explanation:** A component in Angular is a TypeScript class decorated with @Component that controls a portion of the screen called a view. It consists of three parts: a TypeScript class with the @Component decorator, an HTML template that defines the view, and optional CSS styles. Components are the fundamental building blocks of Angular applications.

#### 12. Which decorator is used to pass data from a parent component to a child component?
   a. @Output
   b. @Input
   c. @ViewChild
   d. @HostBinding

**Answer: b**

**Explanation:** The @Input decorator is used to pass data from a parent component to a child component. It marks a class property as an input property and allows the parent component to bind to it in the template using property binding syntax [propertyName]="value". This enables one-way data flow from parent to child.

#### 13. How do you emit an event from a child component to a parent component?
   a. Using @Input decorator
   b. Using EventEmitter with @Output decorator
   c. Using @ViewChild
   d. Using ngModel

**Answer: b**

**Explanation:** To emit events from a child to a parent component, you use EventEmitter with the @Output decorator. The child component creates an EventEmitter instance and calls its emit() method to send data to the parent. The parent listens to this event using event binding syntax (eventName)="handler($event)".

#### 14. What is data binding in Angular?
   a. A mechanism to connect component data to the DOM
   b. A way to make HTTP requests
   c. A routing configuration
   d. A testing framework

**Answer: a**

**Explanation:** Data binding is a mechanism that allows communication between the component class (TypeScript) and the template (HTML). Angular supports four types of data binding: Interpolation ({{}}), Property binding ([property]), Event binding ((event)), and Two-way binding ([(ngModel)]). It automatically synchronizes data between the component and the view.

#### 15. What is the difference between structural and attribute directives?
   a. There is no difference
   b. Structural directives change the DOM layout, attribute directives change element appearance
   c. Attribute directives are deprecated
   d. Structural directives only work with forms

**Answer: b**

**Explanation:** Structural directives change the DOM layout by adding, removing, or manipulating elements (e.g., *ngIf, *ngFor, *ngSwitch). They are prefixed with an asterisk (*). Attribute directives change the appearance or behavior of an element, component, or another directive (e.g., ngClass, ngStyle, ngModel) without changing the DOM structure.

#### 16. What is the purpose of the ViewChild decorator?
   a. To create child components
   b. To get a reference to a child component, directive, or DOM element
   c. To pass data to child components
   d. To style child elements

**Answer: b**

**Explanation:** @ViewChild is a property decorator that configures a view query. It provides access to a child component, directive, or DOM element from the parent component class. This is useful when you need to call methods on a child component or access its properties programmatically. ViewChild queries are set before the ngAfterViewInit lifecycle hook.

#### 17. What is interpolation in Angular?
   a. A way to embed expressions in HTML using {{ }}
   b. A method to make HTTP calls
   c. A routing technique
   d. A form validation method

**Answer: a**

**Explanation:** Interpolation is a one-way data binding technique that embeds component property values into the HTML template using double curly braces {{ expression }}. Angular evaluates the expression and converts the result to a string, which is then inserted into the HTML. It's commonly used to display component data in the view.

#### 18. What is the purpose of ngOnInit lifecycle hook?
   a. To destroy the component
   b. To initialize the component after Angular sets the input properties
   c. To detect changes
   d. To compile the template

**Answer: b**

**Explanation:** ngOnInit is a lifecycle hook that is called once after the component is initialized and after Angular has set the input properties. It's the ideal place to perform component initialization logic such as fetching data from a server, setting up subscriptions, or initializing component properties. It's called after the constructor and after the first ngOnChanges.

#### 19. What is the purpose of the ng-template directive?
   a. To define a template that is not rendered by default
   b. To create new components
   c. To import external templates
   d. To style elements

**Answer: a**

**Explanation:** ng-template is an Angular element for rendering HTML that is not displayed by default. It defines a template that can be instantiated programmatically or used with structural directives. It's commonly used with structural directives like *ngIf, *ngFor, and for creating reusable template fragments that can be referenced elsewhere in the application.

#### 20. How do you create a two-way data binding in Angular?
   a. Using {{ }}
   b. Using [ ]
   c. Using [( )] with ngModel
   d. Using ( )

**Answer: c**

**Explanation:** Two-way data binding in Angular is achieved using the "banana in a box" syntax [(ngModel)]. This combines property binding [] and event binding () to create a two-way communication between the component and the view. When the view changes, the component is updated, and when the component changes, the view is updated. The FormsModule must be imported to use ngModel.

### Section 3: Services and Dependency Injection (Questions 21-30)

#### 21. What is a service in Angular?
   a. A component with a special decorator
   b. A class with the @Injectable decorator that provides reusable business logic
   c. A directive that modifies behavior
   d. A module that groups components

**Answer: b**

**Explanation:** A service in Angular is a TypeScript class with the @Injectable decorator that encapsulates reusable business logic, data access, or functionality that isn't directly related to views. Services are designed to be injected into components or other services using Angular's Dependency Injection system, promoting code reusability and separation of concerns.

#### 22. What is Dependency Injection (DI) in Angular?
   a. A design pattern where dependencies are injected into a class rather than created by it
   b. A way to import modules
   c. A routing mechanism
   d. A form validation technique

**Answer: a**

**Explanation:** Dependency Injection (DI) is a design pattern and mechanism for creating and delivering dependencies to a class from an external source rather than creating them internally. Angular's DI framework provides declared dependencies to a class when that class is instantiated. This promotes loose coupling, easier testing, and better code organization.

#### 23. What is the @Injectable decorator used for?
   a. To mark a class as a component
   b. To mark a class as available for dependency injection
   c. To create HTTP services
   d. To define routes

**Answer: b**

**Explanation:** The @Injectable decorator marks a class as available to be provided and injected as a dependency. When you add @Injectable() to a service class, it tells Angular that this service might itself have injected dependencies. The providedIn property can be set to 'root' to make the service available application-wide as a singleton.

#### 24. What does providedIn: 'root' mean in a service?
   a. The service is provided only in the root component
   b. The service is available application-wide as a singleton
   c. The service is provided in the root module only
   d. The service cannot be injected

**Answer: b**

**Explanation:** When you specify providedIn: 'root' in the @Injectable decorator, Angular creates a single, shared instance of the service at the application root level. This makes the service available throughout the entire application as a singleton. It's the recommended way to provide services and enables tree-shaking to remove unused services.

#### 25. How do you inject a service into a component?
   a. Using the import statement
   b. Through the component's constructor parameters
   c. Using the @ViewChild decorator
   d. Using the require() function

**Answer: b**

**Explanation:** Services are injected into components through the constructor parameters. When you add a parameter with the service type to the constructor (e.g., constructor(private myService: MyService)), Angular's DI system automatically creates or retrieves an instance of that service and passes it to the component. The private/public access modifier creates a class property automatically.

#### 26. What is the difference between providers array in @NgModule and providedIn in @Injectable?
   a. They are exactly the same
   b. providedIn enables tree-shaking and is preferred; providers creates module-scoped instances
   c. providers is deprecated
   d. providedIn only works with components

**Answer: b**

**Explanation:** providedIn: 'root' in @Injectable is the modern approach that makes the service tree-shakeable (unused services are removed from the bundle) and creates application-wide singletons. The providers array in @NgModule creates module-scoped instances and doesn't enable tree-shaking. providedIn is generally preferred except when you need module-specific service instances.

#### 27. What are hierarchical injectors in Angular?
   a. A flat DI system
   b. A tree of injectors that mirrors the component tree
   c. A deprecated feature
   d. A routing configuration

**Answer: b**

**Explanation:** Angular's DI system is hierarchical, creating a tree of injectors that parallels the component tree. When a component requests a dependency, Angular starts with that component's injector and traverses up the tree until it finds a provider. This allows for different instances of a service at different levels and enables service isolation when needed.

#### 28. How do you provide a service only to a specific component and its children?
   a. Use providedIn: 'root'
   b. Add the service to the providers array in the @Component decorator
   c. Services cannot be scoped to components
   d. Use the @Injectable decorator

**Answer: b**

**Explanation:** To provide a service only to a specific component and its children, add the service to the providers array in that component's @Component decorator. This creates a new instance of the service for that component and its children. Each instance of the component gets its own service instance, which is not shared with other components.

#### 29. What is the purpose of the useClass provider?
   a. To create new components
   b. To provide an alternative implementation of a service
   c. To import classes
   d. To define component templates

**Answer: b**

**Explanation:** The useClass provider is used in the providers configuration to substitute one class for another. It's useful for providing alternative implementations, mocking services for testing, or providing different implementations based on conditions. For example: { provide: ServiceA, useClass: ServiceB } makes Angular inject ServiceB whenever ServiceA is requested.

#### 30. What is a singleton service in Angular?
   a. A service that can only be injected once
   b. A service with only one instance shared across the application
   c. A service without dependencies
   d. A deprecated service type

**Answer: b**

**Explanation:** A singleton service has only one instance that is shared across the entire application. When you provide a service at the root level (using providedIn: 'root' or in AppModule's providers), Angular creates a single instance and injects the same instance everywhere it's requested. This is useful for sharing state and data across components.

### Section 4: Routing and Navigation (Questions 31-40)

#### 31. What is the Angular Router?
   a. A service for HTTP requests
   b. A library for navigation and routing in single-page applications
   c. A component lifecycle hook
   d. A form validation tool

**Answer: b**

**Explanation:** The Angular Router is a powerful library that enables navigation and routing in single-page applications. It interprets browser URLs as instructions to navigate to client-generated views, allows navigation between different components, supports guards for protecting routes, lazy loading of modules, and provides a rich API for programmatic navigation.

#### 32. Which module must be imported to use routing in Angular?
   a. HttpClientModule
   b. FormsModule
   c. RouterModule
   d. BrowserModule

**Answer: c**

**Explanation:** RouterModule is the Angular module that provides the necessary routing services and directives. It must be imported in your application module and configured with routes using RouterModule.forRoot(routes) in the root module or RouterModule.forChild(routes) in feature modules. It provides directives like routerLink and router-outlet.

#### 33. What is the purpose of <router-outlet>?
   a. To define route paths
   b. To display the component for the active route
   c. To create navigation links
   d. To guard routes

**Answer: b**

**Explanation:** <router-outlet> is a directive that acts as a placeholder where the router should display the component corresponding to the active route. When you navigate to a route, the router looks up the component associated with that route and renders it inside the router-outlet. Multiple named outlets can be used for complex layouts.

#### 34. How do you create a navigation link in Angular?
   a. Using <a href="">
   b. Using <a routerLink="">
   c. Using <link to="">
   d. Using <navigate to="">

**Answer: b**

**Explanation:** The routerLink directive is used to create navigation links in Angular applications. Unlike regular <a href>, routerLink prevents full page reloads and uses Angular's router for navigation. It can accept a string ('/path') or an array (['/path', param]) and automatically updates the browser's URL without reloading the page.

#### 35. What is a route guard in Angular?
   a. A security feature for HTTP requests
   b. An interface that controls navigation to and from routes
   c. A component decorator
   d. A service for data encryption

**Answer: b**

**Explanation:** Route guards are interfaces that control whether a route can be activated, deactivated, or loaded. Angular provides several guard interfaces: CanActivate (control route activation), CanDeactivate (prevent leaving a route), CanLoad (prevent lazy loading), Resolve (pre-fetch data), and CanActivateChild (control child route activation). Guards return true, false, or a UrlTree.

#### 36. What is the purpose of ActivatedRoute?
   a. To create new routes
   b. To access route parameters and data for the currently activated route
   c. To guard routes
   d. To compile templates

**Answer: b**

**Explanation:** ActivatedRoute is a service that provides access to information about the currently activated route, including route parameters (params), query parameters (queryParams), route data (data), URL segments, and the route configuration. It's commonly injected into components to retrieve route information and react to route changes using observables.

#### 37. How do you implement lazy loading in Angular routing?
   a. Using the import statement
   b. Using loadChildren in the route configuration
   c. Using the @LazyLoad decorator
   d. Lazy loading is automatic

**Answer: b**

**Explanation:** Lazy loading is implemented using the loadChildren property in route configuration. Instead of importing the module directly, you provide a function that returns a dynamic import: loadChildren: () => import('./path/module').then(m => m.ModuleName). This loads the module and its components only when the route is activated, improving initial load time.

#### 38. What is the purpose of the Router service?
   a. To define routes
   b. To programmatically navigate between routes
   c. To create components
   d. To handle HTTP requests

**Answer: b**

**Explanation:** The Router service provides methods for programmatic navigation and route management. Key methods include navigate() and navigateByUrl() for navigation, createUrlTree() for URL creation, and events observable for monitoring navigation events. It's injected into components or services when you need to navigate in response to user actions or application logic.

#### 39. What are route parameters used for?
   a. To style components
   b. To pass dynamic values in the URL path
   c. To configure HTTP requests
   d. To define module dependencies

**Answer: b**

**Explanation:** Route parameters are used to pass dynamic values as part of the URL path. They are defined in routes using a colon syntax (e.g., 'user/:id') and accessed via ActivatedRoute.params or ActivatedRoute.snapshot.params. They're useful for passing identifiers, slugs, or other values that determine what data to display in a component.

#### 40. What is the difference between queryParams and route params?
   a. They are the same
   b. queryParams are optional URL parameters (?key=value), route params are part of the path (/path/:param)
   c. queryParams are deprecated
   d. Route params only work with lazy loading

**Answer: b**

**Explanation:** Route parameters are part of the URL path (e.g., /users/123 where 123 is a route param) and are typically required for the route to match. Query parameters are optional key-value pairs after a ? in the URL (e.g., /users?role=admin&active=true). Both are accessed through ActivatedRoute but serve different purposes in application navigation.

### Section 5: Forms (Questions 41-50)

#### 41. What are the two types of forms in Angular?
   a. Simple and Complex
   b. Template-driven and Reactive
   c. Static and Dynamic
   d. Validated and Non-validated

**Answer: b**

**Explanation:** Angular provides two approaches to handling forms: Template-driven forms (using directives like ngModel in the template) and Reactive forms (using FormControl, FormGroup in the component class). Template-driven forms are simpler and use two-way data binding, while Reactive forms are more powerful, scalable, and provide better testability.

#### 42. Which module is required for template-driven forms?
   a. ReactiveFormsModule
   b. FormsModule
   c. HttpClientModule
   d. RouterModule

**Answer: b**

**Explanation:** FormsModule must be imported to use template-driven forms. It provides directives like ngModel, ngForm, and ngModelGroup that enable two-way data binding and form validation in the template. For reactive forms, you would import ReactiveFormsModule instead, which provides FormControl, FormGroup, and FormBuilder.

#### 43. Which module is required for reactive forms?
   a. FormsModule
   b. BrowserModule
   c. ReactiveFormsModule
   d. CommonModule

**Answer: c**

**Explanation:** ReactiveFormsModule must be imported to use reactive forms. It provides classes and directives like FormControl, FormGroup, FormArray, FormBuilder, and formControlName that enable the creation and management of forms in the component class. Reactive forms offer more control, better testability, and are more suitable for complex scenarios.

#### 44. What is FormControl in Angular?
   a. A directive for templates
   b. A class that tracks the value and validation status of an individual form control
   c. A module for forms
   d. A route guard

**Answer: b**

**Explanation:** FormControl is a class that tracks the value and validation status of an individual form control (input, select, textarea, etc.). It provides properties like value, status, errors, dirty, touched, and methods like setValue(), patchValue(), and reset(). FormControls can be created standalone or as part of a FormGroup.

#### 45. What is the purpose of FormBuilder?
   a. To build components
   b. To provide a convenient API for creating FormGroup and FormControl instances
   c. To validate forms
   d. To route between forms

**Answer: b**

**Explanation:** FormBuilder is a service that provides syntactic sugar and a more concise API for creating FormControl, FormGroup, and FormArray instances. Instead of manually creating new FormControl() instances, you can use FormBuilder's group(), control(), and array() methods to build forms more cleanly and with less boilerplate code.

#### 46. How do you add validation to a reactive form control?
   a. Using HTML attributes only
   b. Passing validators as the second argument when creating FormControl
   c. Validation is automatic
   d. Using the @Validate decorator

**Answer: b**

**Explanation:** In reactive forms, validators are added when creating a FormControl by passing them as the second argument: new FormControl('', Validators.required). You can pass a single validator or an array of validators. Angular provides built-in validators (required, minLength, maxLength, pattern, email) or you can create custom validators as functions.

#### 47. What is the difference between dirty and touched in forms?
   a. They are the same
   b. dirty means value changed, touched means control was focused/blurred
   c. dirty is for reactive forms only
   d. touched is deprecated

**Answer: b**

**Explanation:** dirty becomes true when the user changes the value in the form control (pristine is the opposite). touched becomes true when the user focuses on a control and then blurs/leaves it (untouched is the opposite). These properties are useful for controlling when to display validation messages: typically show errors only after a control is touched or dirty.

#### 48. How do you create a custom validator in Angular?
   a. Using the @Validator decorator
   b. Creating a function that returns a ValidatorFn
   c. Custom validators are not possible
   d. Using the validateCustom() method

**Answer: b**

**Explanation:** A custom validator is a function that returns a ValidatorFn (for synchronous validation) or AsyncValidatorFn (for asynchronous validation). The validator function receives an AbstractControl and returns null if valid or a ValidationErrors object if invalid. Custom validators can be used just like built-in validators when creating FormControls.

#### 49. What is FormArray used for?
   a. To create arrays of components
   b. To manage a collection of FormControl, FormGroup, or FormArray instances
   c. To validate arrays
   d. To route to different forms

**Answer: b**

**Explanation:** FormArray is used to manage a collection of FormControl, FormGroup, or FormArray instances when you need a dynamic number of controls. It's useful for scenarios like adding/removing form fields dynamically (e.g., adding multiple addresses or phone numbers). It provides methods like push(), removeAt(), and insert() to manipulate the array.

#### 50. How do you display validation errors in a template?
   a. Errors are displayed automatically
   b. Access control.errors and check for specific error keys
   c. Using the @Error decorator
   d. Validation errors cannot be displayed

**Answer: b**

**Explanation:** Validation errors are accessed via the control's errors property, which returns an object with error keys if the control is invalid. In templates, you check for specific errors using *ngIf="myControl.errors?.['required']" or myControl.hasError('required'). Typically, errors are displayed only when the control is touched or dirty to avoid showing errors before user interaction.

### Section 6: HTTP Client and Observables (Questions 51-60)

#### 51. Which module must be imported to use HttpClient?
   a. HttpModule
   b. HttpClientModule
   c. BrowserModule
   d. FormsModule

**Answer: b**

**Explanation:** HttpClientModule must be imported in your application module to use Angular's HttpClient service. It provides a simplified API for HTTP functionality based on observables, including features like typed request/response objects, request/response interception, testability features, and streamlined error handling through observables.

#### 52. What does HttpClient.get() return?
   a. A Promise
   b. An Observable
   c. A callback function
   d. A direct value

**Answer: b**

**Explanation:** HttpClient methods (get, post, put, delete, etc.) return Observables from RxJS. Observables are lazy and require subscription to execute. They support operators for transformation, filtering, and combination, can be cancelled, and provide a consistent API for handling asynchronous operations. You must subscribe() to an Observable to trigger the HTTP request.

#### 53. What is an Observable in Angular?
   a. A type of component
   b. A lazy stream of data that can emit multiple values over time
   c. A routing configuration
   d. A form control

**Answer: b**

**Explanation:** An Observable is a lazy stream of data that can emit multiple values over time (or zero values). It's based on the observer pattern and is provided by RxJS. Observables are lazy (they don't execute until subscribed to), can be cancelled, support powerful operators for transformation and composition, and are the foundation for handling asynchronous operations in Angular.

#### 54. What is the purpose of the subscribe() method?
   a. To create an Observable
   b. To execute an Observable and receive emitted values
   c. To cancel an Observable
   d. To transform Observable data

**Answer: b**

**Explanation:** The subscribe() method is called on an Observable to execute it and receive the emitted values. It takes up to three callbacks: next (for emitted values), error (for errors), and complete (when the Observable completes). Calling subscribe() returns a Subscription object that can be used to unsubscribe and cancel the Observable execution.

#### 55. How do you handle HTTP errors in Angular?
   a. Using try-catch blocks
   b. Using the catchError RxJS operator in a pipe
   c. Errors are handled automatically
   d. Using the @ErrorHandler decorator

**Answer: b**

**Explanation:** HTTP errors are handled using the catchError operator from RxJS in a pipe. The catchError operator intercepts errors from the Observable chain and allows you to handle them gracefully, log them, or return a fallback value. It receives an error object and must return an Observable (often using throwError() or of() to emit a default value).

#### 56. What is the purpose of RxJS operators?
   a. To create components
   b. To transform, filter, and combine Observable streams
   c. To handle routing
   d. To validate forms

**Answer: b**

**Explanation:** RxJS operators are functions that transform, filter, combine, and manipulate Observable streams. Common operators include map (transform values), filter (filter values), mergeMap/switchMap (flatten nested Observables), tap (side effects), catchError (error handling), and debounceTime (delay emissions). Operators are used in the pipe() method to create data transformation pipelines.

#### 57. What is the difference between map and switchMap operators?
   a. They are the same
   b. map transforms values, switchMap transforms to new Observables and cancels previous ones
   c. map is deprecated
   d. switchMap only works with HTTP requests

**Answer: b**

**Explanation:** map transforms each emitted value using a projection function and emits the result (Observable<T> -> Observable<U>). switchMap transforms each value into a new Observable and subscribes to it, cancelling any previous inner Observable (useful for HTTP requests). switchMap is a flattening operator that prevents memory leaks from abandoned requests.

#### 58. How do you make multiple HTTP requests in parallel?
   a. Using nested subscribe calls
   b. Using the forkJoin operator
   c. Multiple parallel requests are not possible
   d. Using the merge() function

**Answer: b**

**Explanation:** forkJoin is an RxJS operator that takes an array or object of Observables, executes them in parallel, and emits a single value when all Observables complete. It's similar to Promise.all() and returns an array or object with the results. Other operators like combineLatest or zip can also be used depending on the requirements.

#### 59. What is the async pipe used for?
   a. To create asynchronous functions
   b. To automatically subscribe and unsubscribe from Observables in templates
   c. To handle HTTP errors
   d. To validate forms

**Answer: b**

**Explanation:** The async pipe is a built-in Angular pipe that subscribes to an Observable or Promise and returns the latest value it has emitted. It automatically unsubscribes when the component is destroyed, preventing memory leaks. It also marks the component for change detection when a new value is emitted, making it a clean way to handle Observables in templates.

#### 60. How do you prevent memory leaks when subscribing to Observables?
   a. Memory leaks are not possible with Observables
   b. Unsubscribe in ngOnDestroy or use the async pipe
   c. Use the @NoLeaks decorator
   d. Restart the application

**Answer: b**

**Explanation:** To prevent memory leaks, you must unsubscribe from Observables when a component is destroyed. Common approaches include: 1) Storing subscriptions and calling unsubscribe() in ngOnDestroy, 2) Using the async pipe which auto-unsubscribes, 3) Using takeUntil with a Subject that emits in ngOnDestroy, or 4) Using operators like take or first that automatically complete.

