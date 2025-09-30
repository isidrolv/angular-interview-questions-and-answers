# Angular Coding Challenges
## 10 Essential Programming Exercises for Angular Interviews

### Challenge 1: Create a Custom Counter Component

**Difficulty:** Easy

**Description:**
Create a counter component that displays a number and has buttons to increment, decrement, and reset the counter. The component should use Angular's component architecture and event handling.

**Requirements:**
- Display the current count
- Three buttons: Increment (+1), Decrement (-1), and Reset (back to 0)
- Use proper data binding
- Style the component to be visually appealing

**Solution:**

```typescript
// counter.component.ts
import { Component } from '@angular/core';

@Component({
  selector: 'app-counter',
  template: `
    <div class="counter-container">
      <h2>Counter: {{ count }}</h2>
      <div class="button-group">
        <button (click)="increment()" class="btn btn-success">+</button>
        <button (click)="decrement()" class="btn btn-danger">-</button>
        <button (click)="reset()" class="btn btn-secondary">Reset</button>
      </div>
    </div>
  `,
  styles: [`
    .counter-container {
      text-align: center;
      padding: 20px;
      border: 2px solid #ccc;
      border-radius: 8px;
      max-width: 300px;
      margin: 0 auto;
    }
    .button-group {
      display: flex;
      gap: 10px;
      justify-content: center;
      margin-top: 20px;
    }
    .btn {
      padding: 10px 20px;
      cursor: pointer;
      border: none;
      border-radius: 4px;
      font-size: 16px;
    }
    .btn-success { background-color: #28a745; color: white; }
    .btn-danger { background-color: #dc3545; color: white; }
    .btn-secondary { background-color: #6c757d; color: white; }
  `]
})
export class CounterComponent {
  count: number = 0;

  increment(): void {
    this.count++;
  }

  decrement(): void {
    this.count--;
  }

  reset(): void {
    this.count = 0;
  }
}
```

**Key Concepts:**
- Component creation with inline template and styles
- Event binding with (click)
- Interpolation with {{ }}
- Method implementation in component class

---

### Challenge 2: Parent-Child Component Communication

**Difficulty:** Medium

**Description:**
Create a parent component that passes data to a child component using @Input, and the child component emits events back to the parent using @Output and EventEmitter.

**Requirements:**
- Parent component should have a list of items
- Child component displays an item and has a delete button
- When delete is clicked, emit an event to the parent
- Parent removes the item from the list

**Solution:**

```typescript
// item.component.ts (Child)
import { Component, Input, Output, EventEmitter } from '@angular/core';

export interface Item {
  id: number;
  name: string;
}

@Component({
  selector: 'app-item',
  template: `
    <div class="item-card">
      <span>{{ item.name }}</span>
      <button (click)="onDelete()" class="delete-btn">Delete</button>
    </div>
  `,
  styles: [`
    .item-card {
      display: flex;
      justify-content: space-between;
      align-items: center;
      padding: 10px;
      margin: 5px 0;
      border: 1px solid #ddd;
      border-radius: 4px;
    }
    .delete-btn {
      background-color: #dc3545;
      color: white;
      border: none;
      padding: 5px 10px;
      cursor: pointer;
      border-radius: 3px;
    }
  `]
})
export class ItemComponent {
  @Input() item!: Item;
  @Output() delete = new EventEmitter<number>();

  onDelete(): void {
    this.delete.emit(this.item.id);
  }
}

// item-list.component.ts (Parent)
import { Component } from '@angular/core';
import { Item } from './item.component';

@Component({
  selector: 'app-item-list',
  template: `
    <div class="list-container">
      <h2>Item List</h2>
      <app-item 
        *ngFor="let item of items" 
        [item]="item"
        (delete)="deleteItem($event)">
      </app-item>
      <p *ngIf="items.length === 0">No items available</p>
    </div>
  `,
  styles: [`
    .list-container {
      max-width: 500px;
      margin: 20px auto;
      padding: 20px;
    }
  `]
})
export class ItemListComponent {
  items: Item[] = [
    { id: 1, name: 'Item 1' },
    { id: 2, name: 'Item 2' },
    { id: 3, name: 'Item 3' }
  ];

  deleteItem(id: number): void {
    this.items = this.items.filter(item => item.id !== id);
  }
}
```

**Key Concepts:**
- @Input decorator for passing data to child
- @Output decorator with EventEmitter for emitting events
- *ngFor structural directive
- Event binding with $event
- Array filtering

---

### Challenge 3: Create a Reusable Service with Dependency Injection

**Difficulty:** Medium

**Description:**
Create a data service that manages a list of users and can be injected into multiple components. Implement CRUD operations (Create, Read, Update, Delete).

**Requirements:**
- Service should be injectable application-wide
- Methods for adding, getting, updating, and deleting users
- Use BehaviorSubject for reactive data updates
- Components should be able to subscribe to user changes

**Solution:**

```typescript
// user.model.ts
export interface User {
  id: number;
  name: string;
  email: string;
}

// user.service.ts
import { Injectable } from '@angular/core';
import { BehaviorSubject, Observable } from 'rxjs';
import { User } from './user.model';

@Injectable({
  providedIn: 'root'
})
export class UserService {
  private users: User[] = [
    { id: 1, name: 'John Doe', email: 'john@example.com' },
    { id: 2, name: 'Jane Smith', email: 'jane@example.com' }
  ];
  
  private usersSubject = new BehaviorSubject<User[]>(this.users);
  public users$: Observable<User[]> = this.usersSubject.asObservable();

  private nextId = 3;

  getUsers(): User[] {
    return [...this.users];
  }

  getUserById(id: number): User | undefined {
    return this.users.find(user => user.id === id);
  }

  addUser(name: string, email: string): void {
    const newUser: User = {
      id: this.nextId++,
      name,
      email
    };
    this.users.push(newUser);
    this.usersSubject.next([...this.users]);
  }

  updateUser(id: number, name: string, email: string): void {
    const index = this.users.findIndex(user => user.id === id);
    if (index !== -1) {
      this.users[index] = { id, name, email };
      this.usersSubject.next([...this.users]);
    }
  }

  deleteUser(id: number): void {
    this.users = this.users.filter(user => user.id !== id);
    this.usersSubject.next([...this.users]);
  }
}

// user-list.component.ts
import { Component, OnInit } from '@angular/core';
import { UserService } from './user.service';
import { User } from './user.model';

@Component({
  selector: 'app-user-list',
  template: `
    <div class="user-list">
      <h2>Users</h2>
      <div *ngFor="let user of users$ | async" class="user-card">
        <p><strong>{{ user.name }}</strong></p>
        <p>{{ user.email }}</p>
        <button (click)="deleteUser(user.id)">Delete</button>
      </div>
    </div>
  `
})
export class UserListComponent implements OnInit {
  users$ = this.userService.users$;

  constructor(private userService: UserService) {}

  ngOnInit(): void {
    // Component automatically receives updates via the Observable
  }

  deleteUser(id: number): void {
    this.userService.deleteUser(id);
  }
}
```

**Key Concepts:**
- @Injectable with providedIn: 'root'
- BehaviorSubject for reactive state management
- Observable streams
- Dependency injection via constructor
- async pipe for automatic subscription
- CRUD operations

---

### Challenge 4: Implement Routing with Parameters

**Difficulty:** Medium

**Description:**
Create a simple application with routing that includes a list view and a detail view. The detail view should receive an ID parameter from the route.

**Requirements:**
- Home page with a list of products
- Clicking a product navigates to a detail page
- Detail page displays product information based on route parameter
- Include a "Back" button to return to the list

**Solution:**

```typescript
// product.model.ts
export interface Product {
  id: number;
  name: string;
  description: string;
  price: number;
}

// product.service.ts
import { Injectable } from '@angular/core';
import { Product } from './product.model';

@Injectable({
  providedIn: 'root'
})
export class ProductService {
  private products: Product[] = [
    { id: 1, name: 'Laptop', description: 'High-performance laptop', price: 999 },
    { id: 2, name: 'Mouse', description: 'Wireless mouse', price: 29 },
    { id: 3, name: 'Keyboard', description: 'Mechanical keyboard', price: 79 }
  ];

  getProducts(): Product[] {
    return this.products;
  }

  getProductById(id: number): Product | undefined {
    return this.products.find(p => p.id === id);
  }
}

// product-list.component.ts
import { Component, OnInit } from '@angular/core';
import { Router } from '@angular/router';
import { ProductService } from './product.service';
import { Product } from './product.model';

@Component({
  selector: 'app-product-list',
  template: `
    <div class="product-list">
      <h2>Products</h2>
      <div *ngFor="let product of products" 
           class="product-item"
           (click)="viewDetails(product.id)">
        <h3>{{ product.name }}</h3>
        <p>{{ product.description }}</p>
        <p class="price">\${{ product.price }}</p>
      </div>
    </div>
  `,
  styles: [`
    .product-item {
      padding: 15px;
      margin: 10px 0;
      border: 1px solid #ddd;
      border-radius: 4px;
      cursor: pointer;
      transition: background-color 0.3s;
    }
    .product-item:hover {
      background-color: #f0f0f0;
    }
    .price {
      color: #28a745;
      font-weight: bold;
    }
  `]
})
export class ProductListComponent implements OnInit {
  products: Product[] = [];

  constructor(
    private productService: ProductService,
    private router: Router
  ) {}

  ngOnInit(): void {
    this.products = this.productService.getProducts();
  }

  viewDetails(id: number): void {
    this.router.navigate(['/product', id]);
  }
}

// product-detail.component.ts
import { Component, OnInit } from '@angular/core';
import { ActivatedRoute, Router } from '@angular/router';
import { ProductService } from './product.service';
import { Product } from './product.model';

@Component({
  selector: 'app-product-detail',
  template: `
    <div class="product-detail" *ngIf="product">
      <h2>{{ product.name }}</h2>
      <p class="description">{{ product.description }}</p>
      <p class="price">Price: \${{ product.price }}</p>
      <button (click)="goBack()">Back to List</button>
    </div>
    <div *ngIf="!product">
      <p>Product not found</p>
      <button (click)="goBack()">Back to List</button>
    </div>
  `,
  styles: [`
    .product-detail {
      padding: 20px;
      max-width: 600px;
      margin: 0 auto;
    }
    .description {
      font-size: 16px;
      margin: 20px 0;
    }
    .price {
      font-size: 24px;
      color: #28a745;
      font-weight: bold;
    }
    button {
      padding: 10px 20px;
      background-color: #007bff;
      color: white;
      border: none;
      border-radius: 4px;
      cursor: pointer;
    }
  `]
})
export class ProductDetailComponent implements OnInit {
  product?: Product;

  constructor(
    private route: ActivatedRoute,
    private router: Router,
    private productService: ProductService
  ) {}

  ngOnInit(): void {
    const id = Number(this.route.snapshot.paramMap.get('id'));
    this.product = this.productService.getProductById(id);
  }

  goBack(): void {
    this.router.navigate(['/products']);
  }
}

// app-routing.module.ts
import { NgModule } from '@angular/core';
import { RouterModule, Routes } from '@angular/router';
import { ProductListComponent } from './product-list.component';
import { ProductDetailComponent } from './product-detail.component';

const routes: Routes = [
  { path: '', redirectTo: '/products', pathMatch: 'full' },
  { path: 'products', component: ProductListComponent },
  { path: 'product/:id', component: ProductDetailComponent }
];

@NgModule({
  imports: [RouterModule.forRoot(routes)],
  exports: [RouterModule]
})
export class AppRoutingModule { }
```

**Key Concepts:**
- RouterModule configuration
- Route parameters with :id
- ActivatedRoute for accessing route parameters
- Router service for programmatic navigation
- snapshot.paramMap for reading route params
- Redirect routes

---

### Challenge 5: Reactive Form with Validation

**Difficulty:** Medium

**Description:**
Create a user registration form using Reactive Forms with custom validation. Include real-time validation feedback and form submission handling.

**Requirements:**
- Form fields: username, email, password, confirm password
- Validation: required fields, email format, password strength, passwords match
- Display validation errors
- Disable submit button when form is invalid

**Solution:**

```typescript
// registration.component.ts
import { Component, OnInit } from '@angular/core';
import { FormBuilder, FormGroup, Validators, AbstractControl, ValidationErrors } from '@angular/forms';

@Component({
  selector: 'app-registration',
  template: `
    <div class="registration-form">
      <h2>User Registration</h2>
      <form [formGroup]="registrationForm" (ngSubmit)="onSubmit()">
        
        <div class="form-group">
          <label>Username:</label>
          <input type="text" formControlName="username" class="form-control">
          <div class="error" *ngIf="username?.invalid && username?.touched">
            <small *ngIf="username?.errors?.['required']">Username is required</small>
            <small *ngIf="username?.errors?.['minlength']">Username must be at least 3 characters</small>
          </div>
        </div>

        <div class="form-group">
          <label>Email:</label>
          <input type="email" formControlName="email" class="form-control">
          <div class="error" *ngIf="email?.invalid && email?.touched">
            <small *ngIf="email?.errors?.['required']">Email is required</small>
            <small *ngIf="email?.errors?.['email']">Invalid email format</small>
          </div>
        </div>

        <div class="form-group">
          <label>Password:</label>
          <input type="password" formControlName="password" class="form-control">
          <div class="error" *ngIf="password?.invalid && password?.touched">
            <small *ngIf="password?.errors?.['required']">Password is required</small>
            <small *ngIf="password?.errors?.['minlength']">Password must be at least 8 characters</small>
            <small *ngIf="password?.errors?.['weakPassword']">Password must contain uppercase, lowercase, number, and special character</small>
          </div>
        </div>

        <div class="form-group">
          <label>Confirm Password:</label>
          <input type="password" formControlName="confirmPassword" class="form-control">
          <div class="error" *ngIf="confirmPassword?.invalid && confirmPassword?.touched">
            <small *ngIf="confirmPassword?.errors?.['required']">Please confirm password</small>
          </div>
          <div class="error" *ngIf="registrationForm.errors?.['passwordMismatch'] && confirmPassword?.touched">
            <small>Passwords do not match</small>
          </div>
        </div>

        <button type="submit" [disabled]="registrationForm.invalid" class="submit-btn">
          Register
        </button>
      </form>

      <div class="success" *ngIf="submitted">
        Registration successful!
      </div>
    </div>
  `,
  styles: [`
    .registration-form {
      max-width: 500px;
      margin: 20px auto;
      padding: 20px;
      border: 1px solid #ddd;
      border-radius: 8px;
    }
    .form-group {
      margin-bottom: 15px;
    }
    label {
      display: block;
      margin-bottom: 5px;
      font-weight: bold;
    }
    .form-control {
      width: 100%;
      padding: 8px;
      border: 1px solid #ddd;
      border-radius: 4px;
      box-sizing: border-box;
    }
    .form-control.ng-invalid.ng-touched {
      border-color: #dc3545;
    }
    .error {
      color: #dc3545;
      margin-top: 5px;
    }
    .error small {
      display: block;
    }
    .submit-btn {
      width: 100%;
      padding: 10px;
      background-color: #28a745;
      color: white;
      border: none;
      border-radius: 4px;
      cursor: pointer;
      font-size: 16px;
    }
    .submit-btn:disabled {
      background-color: #6c757d;
      cursor: not-allowed;
    }
    .success {
      margin-top: 20px;
      padding: 10px;
      background-color: #d4edda;
      color: #155724;
      border-radius: 4px;
    }
  `]
})
export class RegistrationComponent implements OnInit {
  registrationForm!: FormGroup;
  submitted = false;

  constructor(private fb: FormBuilder) {}

  ngOnInit(): void {
    this.registrationForm = this.fb.group({
      username: ['', [Validators.required, Validators.minLength(3)]],
      email: ['', [Validators.required, Validators.email]],
      password: ['', [Validators.required, Validators.minLength(8), this.passwordStrengthValidator]],
      confirmPassword: ['', Validators.required]
    }, { validators: this.passwordMatchValidator });
  }

  // Custom validator for password strength
  passwordStrengthValidator(control: AbstractControl): ValidationErrors | null {
    const value = control.value;
    if (!value) {
      return null;
    }

    const hasUpperCase = /[A-Z]/.test(value);
    const hasLowerCase = /[a-z]/.test(value);
    const hasNumeric = /[0-9]/.test(value);
    const hasSpecialChar = /[!@#$%^&*(),.?":{}|<>]/.test(value);

    const passwordValid = hasUpperCase && hasLowerCase && hasNumeric && hasSpecialChar;

    return !passwordValid ? { weakPassword: true } : null;
  }

  // Custom validator for password match
  passwordMatchValidator(group: AbstractControl): ValidationErrors | null {
    const password = group.get('password')?.value;
    const confirmPassword = group.get('confirmPassword')?.value;

    return password === confirmPassword ? null : { passwordMismatch: true };
  }

  get username() {
    return this.registrationForm.get('username');
  }

  get email() {
    return this.registrationForm.get('email');
  }

  get password() {
    return this.registrationForm.get('password');
  }

  get confirmPassword() {
    return this.registrationForm.get('confirmPassword');
  }

  onSubmit(): void {
    if (this.registrationForm.valid) {
      console.log('Form submitted:', this.registrationForm.value);
      this.submitted = true;
      this.registrationForm.reset();
    }
  }
}
```

**Key Concepts:**
- FormBuilder for creating forms
- Built-in validators (required, email, minLength)
- Custom validators (password strength)
- Form-level validators (password match)
- FormGroup and FormControl
- Reactive form validation
- Conditional error display
- Form state management (invalid, touched, dirty)

---

### Challenge 6: HTTP Service with Error Handling

**Difficulty:** Medium

**Description:**
Create a service that makes HTTP requests to a REST API with proper error handling, loading states, and response transformation.

**Requirements:**
- Service to fetch data from JSONPlaceholder API
- Loading indicator while fetching
- Error handling with user-friendly messages
- Retry logic for failed requests
- Response transformation using RxJS operators

**Solution:**

```typescript
// post.model.ts
export interface Post {
  id: number;
  userId: number;
  title: string;
  body: string;
}

// post.service.ts
import { Injectable } from '@angular/core';
import { HttpClient, HttpErrorResponse } from '@angular/common/http';
import { Observable, throwError, BehaviorSubject } from 'rxjs';
import { catchError, retry, map, tap, finalize } from 'rxjs/operators';
import { Post } from './post.model';

@Injectable({
  providedIn: 'root'
})
export class PostService {
  private apiUrl = 'https://jsonplaceholder.typicode.com/posts';
  private loadingSubject = new BehaviorSubject<boolean>(false);
  public loading$ = this.loadingSubject.asObservable();

  constructor(private http: HttpClient) {}

  getPosts(): Observable<Post[]> {
    this.loadingSubject.next(true);
    
    return this.http.get<Post[]>(this.apiUrl).pipe(
      retry(2), // Retry failed requests up to 2 times
      map(posts => posts.slice(0, 10)), // Get only first 10 posts
      tap(posts => console.log('Fetched posts:', posts.length)),
      catchError(this.handleError),
      finalize(() => this.loadingSubject.next(false))
    );
  }

  getPostById(id: number): Observable<Post> {
    this.loadingSubject.next(true);
    
    return this.http.get<Post>(`${this.apiUrl}/${id}`).pipe(
      retry(2),
      catchError(this.handleError),
      finalize(() => this.loadingSubject.next(false))
    );
  }

  createPost(post: Omit<Post, 'id'>): Observable<Post> {
    this.loadingSubject.next(true);
    
    return this.http.post<Post>(this.apiUrl, post).pipe(
      catchError(this.handleError),
      finalize(() => this.loadingSubject.next(false))
    );
  }

  private handleError(error: HttpErrorResponse): Observable<never> {
    let errorMessage = 'An unknown error occurred';
    
    if (error.error instanceof ErrorEvent) {
      // Client-side error
      errorMessage = `Error: ${error.error.message}`;
    } else {
      // Server-side error
      errorMessage = `Error Code: ${error.status}\nMessage: ${error.message}`;
    }
    
    console.error(errorMessage);
    return throwError(() => new Error(errorMessage));
  }
}

// post-list.component.ts
import { Component, OnInit } from '@angular/core';
import { PostService } from './post.service';
import { Post } from './post.model';

@Component({
  selector: 'app-post-list',
  template: `
    <div class="post-list">
      <h2>Posts</h2>
      
      <div *ngIf="loading$ | async" class="loading">
        Loading posts...
      </div>

      <div *ngIf="error" class="error">
        {{ error }}
        <button (click)="loadPosts()">Retry</button>
      </div>

      <div *ngIf="!(loading$ | async) && !error">
        <div *ngFor="let post of posts" class="post-card">
          <h3>{{ post.title }}</h3>
          <p>{{ post.body }}</p>
          <small>User ID: {{ post.userId }} | Post ID: {{ post.id }}</small>
        </div>
      </div>
    </div>
  `,
  styles: [`
    .post-list {
      max-width: 800px;
      margin: 20px auto;
      padding: 20px;
    }
    .loading {
      text-align: center;
      padding: 20px;
      font-size: 18px;
      color: #007bff;
    }
    .error {
      background-color: #f8d7da;
      color: #721c24;
      padding: 15px;
      border-radius: 4px;
      margin-bottom: 20px;
    }
    .error button {
      margin-top: 10px;
      padding: 5px 10px;
      background-color: #721c24;
      color: white;
      border: none;
      border-radius: 3px;
      cursor: pointer;
    }
    .post-card {
      border: 1px solid #ddd;
      padding: 15px;
      margin-bottom: 15px;
      border-radius: 4px;
    }
    .post-card h3 {
      margin-top: 0;
      color: #007bff;
    }
    .post-card small {
      color: #6c757d;
    }
  `]
})
export class PostListComponent implements OnInit {
  posts: Post[] = [];
  loading$ = this.postService.loading$;
  error: string | null = null;

  constructor(private postService: PostService) {}

  ngOnInit(): void {
    this.loadPosts();
  }

  loadPosts(): void {
    this.error = null;
    this.postService.getPosts().subscribe({
      next: (posts) => {
        this.posts = posts;
      },
      error: (error) => {
        this.error = error.message;
      }
    });
  }
}
```

**Key Concepts:**
- HttpClient service
- HttpClientModule
- RxJS operators (retry, map, tap, catchError, finalize)
- Error handling with HttpErrorResponse
- Loading state management with BehaviorSubject
- Observable subscription with error callback
- async pipe for Observable unwrapping

---

### Challenge 7: Custom Pipe for Data Transformation

**Difficulty:** Easy-Medium

**Description:**
Create custom pipes to transform data in templates. Implement a pipe to filter arrays and a pipe to format phone numbers.

**Requirements:**
- FilterPipe to filter an array based on a search term
- PhoneFormatPipe to format phone numbers (e.g., 1234567890 → (123) 456-7890)
- Use pipes in a component template
- Make pipes reusable

**Solution:**

```typescript
// filter.pipe.ts
import { Pipe, PipeTransform } from '@angular/core';

@Pipe({
  name: 'filter'
})
export class FilterPipe implements PipeTransform {
  transform(items: any[], searchText: string, property: string): any[] {
    if (!items || !searchText) {
      return items;
    }

    searchText = searchText.toLowerCase();

    return items.filter(item => {
      const value = property ? item[property] : item;
      return value.toString().toLowerCase().includes(searchText);
    });
  }
}

// phone-format.pipe.ts
import { Pipe, PipeTransform } from '@angular/core';

@Pipe({
  name: 'phoneFormat'
})
export class PhoneFormatPipe implements PipeTransform {
  transform(value: string | number): string {
    if (!value) return '';

    const phoneStr = value.toString().replace(/\D/g, '');

    if (phoneStr.length === 10) {
      return `(${phoneStr.substring(0, 3)}) ${phoneStr.substring(3, 6)}-${phoneStr.substring(6)}`;
    } else if (phoneStr.length === 11) {
      return `+${phoneStr.substring(0, 1)} (${phoneStr.substring(1, 4)}) ${phoneStr.substring(4, 7)}-${phoneStr.substring(7)}`;
    }

    return value.toString();
  }
}

// contact-list.component.ts
import { Component } from '@angular/core';

interface Contact {
  id: number;
  name: string;
  phone: string;
  email: string;
}

@Component({
  selector: 'app-contact-list',
  template: `
    <div class="contact-list">
      <h2>Contact List</h2>
      
      <div class="search-box">
        <input 
          type="text" 
          [(ngModel)]="searchText" 
          placeholder="Search contacts..."
          class="search-input">
      </div>

      <div class="contacts">
        <div *ngFor="let contact of contacts | filter:searchText:'name'" class="contact-card">
          <h3>{{ contact.name }}</h3>
          <p><strong>Phone:</strong> {{ contact.phone | phoneFormat }}</p>
          <p><strong>Email:</strong> {{ contact.email }}</p>
        </div>
        
        <p *ngIf="(contacts | filter:searchText:'name').length === 0" class="no-results">
          No contacts found
        </p>
      </div>
    </div>
  `,
  styles: [`
    .contact-list {
      max-width: 600px;
      margin: 20px auto;
      padding: 20px;
    }
    .search-box {
      margin-bottom: 20px;
    }
    .search-input {
      width: 100%;
      padding: 10px;
      border: 1px solid #ddd;
      border-radius: 4px;
      font-size: 16px;
      box-sizing: border-box;
    }
    .contact-card {
      border: 1px solid #ddd;
      padding: 15px;
      margin-bottom: 10px;
      border-radius: 4px;
    }
    .contact-card h3 {
      margin-top: 0;
      color: #007bff;
    }
    .no-results {
      text-align: center;
      color: #6c757d;
      padding: 20px;
    }
  `]
})
export class ContactListComponent {
  searchText = '';
  
  contacts: Contact[] = [
    { id: 1, name: 'John Doe', phone: '1234567890', email: 'john@example.com' },
    { id: 2, name: 'Jane Smith', phone: '9876543210', email: 'jane@example.com' },
    { id: 3, name: 'Bob Johnson', phone: '5551234567', email: 'bob@example.com' },
    { id: 4, name: 'Alice Williams', phone: '5559876543', email: 'alice@example.com' }
  ];
}
```

**Key Concepts:**
- Custom pipe creation with @Pipe decorator
- PipeTransform interface
- Pure vs impure pipes
- Pipe parameters
- Chaining pipes
- String manipulation
- Array filtering

---

### Challenge 8: Implement a Guard for Route Protection

**Difficulty:** Medium

**Description:**
Create an authentication guard that protects routes and redirects unauthorized users. Implement a simple authentication service and login component.

**Requirements:**
- AuthGuard to protect routes
- AuthService to manage authentication state
- Login component
- Protected dashboard route
- Redirect to login if not authenticated

**Solution:**

```typescript
// auth.service.ts
import { Injectable } from '@angular/core';
import { BehaviorSubject, Observable } from 'rxjs';
import { Router } from '@angular/router';

@Injectable({
  providedIn: 'root'
})
export class AuthService {
  private isAuthenticatedSubject = new BehaviorSubject<boolean>(false);
  public isAuthenticated$ = this.isAuthenticatedSubject.asObservable();

  private currentUserSubject = new BehaviorSubject<string | null>(null);
  public currentUser$ = this.currentUserSubject.asObservable();

  constructor(private router: Router) {
    // Check if user is logged in from localStorage
    const user = localStorage.getItem('currentUser');
    if (user) {
      this.isAuthenticatedSubject.next(true);
      this.currentUserSubject.next(user);
    }
  }

  login(username: string, password: string): boolean {
    // Simple mock authentication
    if (username === 'admin' && password === 'admin123') {
      this.isAuthenticatedSubject.next(true);
      this.currentUserSubject.next(username);
      localStorage.setItem('currentUser', username);
      return true;
    }
    return false;
  }

  logout(): void {
    this.isAuthenticatedSubject.next(false);
    this.currentUserSubject.next(null);
    localStorage.removeItem('currentUser');
    this.router.navigate(['/login']);
  }

  isAuthenticated(): boolean {
    return this.isAuthenticatedSubject.value;
  }
}

// auth.guard.ts
import { Injectable } from '@angular/core';
import { CanActivate, ActivatedRouteSnapshot, RouterStateSnapshot, Router, UrlTree } from '@angular/router';
import { Observable } from 'rxjs';
import { AuthService } from './auth.service';

@Injectable({
  providedIn: 'root'
})
export class AuthGuard implements CanActivate {
  constructor(
    private authService: AuthService,
    private router: Router
  ) {}

  canActivate(
    route: ActivatedRouteSnapshot,
    state: RouterStateSnapshot
  ): Observable<boolean | UrlTree> | Promise<boolean | UrlTree> | boolean | UrlTree {
    
    if (this.authService.isAuthenticated()) {
      return true;
    }

    // Redirect to login page with return url
    return this.router.createUrlTree(['/login'], { 
      queryParams: { returnUrl: state.url }
    });
  }
}

// login.component.ts
import { Component } from '@angular/core';
import { Router, ActivatedRoute } from '@angular/router';
import { AuthService } from './auth.service';

@Component({
  selector: 'app-login',
  template: `
    <div class="login-container">
      <h2>Login</h2>
      <form (ngSubmit)="onSubmit()">
        <div class="form-group">
          <label>Username:</label>
          <input type="text" [(ngModel)]="username" name="username" required>
        </div>
        <div class="form-group">
          <label>Password:</label>
          <input type="password" [(ngModel)]="password" name="password" required>
        </div>
        <div class="error" *ngIf="error">{{ error }}</div>
        <button type="submit">Login</button>
      </form>
      <p class="hint">Hint: username: admin, password: admin123</p>
    </div>
  `,
  styles: [`
    .login-container {
      max-width: 400px;
      margin: 50px auto;
      padding: 30px;
      border: 1px solid #ddd;
      border-radius: 8px;
      box-shadow: 0 2px 4px rgba(0,0,0,0.1);
    }
    .form-group {
      margin-bottom: 15px;
    }
    label {
      display: block;
      margin-bottom: 5px;
      font-weight: bold;
    }
    input {
      width: 100%;
      padding: 8px;
      border: 1px solid #ddd;
      border-radius: 4px;
      box-sizing: border-box;
    }
    button {
      width: 100%;
      padding: 10px;
      background-color: #007bff;
      color: white;
      border: none;
      border-radius: 4px;
      cursor: pointer;
      font-size: 16px;
    }
    .error {
      color: #dc3545;
      margin: 10px 0;
    }
    .hint {
      text-align: center;
      color: #6c757d;
      font-size: 14px;
      margin-top: 15px;
    }
  `]
})
export class LoginComponent {
  username = '';
  password = '';
  error = '';

  constructor(
    private authService: AuthService,
    private router: Router,
    private route: ActivatedRoute
  ) {}

  onSubmit(): void {
    if (this.authService.login(this.username, this.password)) {
      const returnUrl = this.route.snapshot.queryParams['returnUrl'] || '/dashboard';
      this.router.navigate([returnUrl]);
    } else {
      this.error = 'Invalid username or password';
    }
  }
}

// dashboard.component.ts
import { Component, OnInit } from '@angular/core';
import { AuthService } from './auth.service';

@Component({
  selector: 'app-dashboard',
  template: `
    <div class="dashboard">
      <h2>Dashboard</h2>
      <p>Welcome, {{ currentUser$ | async }}!</p>
      <p>This is a protected route. Only authenticated users can see this.</p>
      <button (click)="logout()">Logout</button>
    </div>
  `,
  styles: [`
    .dashboard {
      max-width: 800px;
      margin: 20px auto;
      padding: 30px;
    }
    button {
      padding: 10px 20px;
      background-color: #dc3545;
      color: white;
      border: none;
      border-radius: 4px;
      cursor: pointer;
    }
  `]
})
export class DashboardComponent implements OnInit {
  currentUser$ = this.authService.currentUser$;

  constructor(private authService: AuthService) {}

  ngOnInit(): void {}

  logout(): void {
    this.authService.logout();
  }
}

// app-routing.module.ts (relevant routes)
const routes: Routes = [
  { path: 'login', component: LoginComponent },
  { 
    path: 'dashboard', 
    component: DashboardComponent,
    canActivate: [AuthGuard]
  },
  { path: '', redirectTo: '/dashboard', pathMatch: 'full' }
];
```

**Key Concepts:**
- CanActivate guard interface
- Route protection
- Authentication service
- localStorage for persistence
- BehaviorSubject for state management
- Query parameters for return URLs
- UrlTree for redirects
- Router navigation

---

### Challenge 9: Implement Lazy Loading with Feature Modules

**Difficulty:** Medium-Hard

**Description:**
Create a feature module that is lazy loaded to improve application performance. Implement separate modules for different features.

**Requirements:**
- Create a feature module (e.g., Admin module)
- Configure lazy loading for the feature module
- Feature module should have its own routing
- Demonstrate performance improvement with lazy loading

**Solution:**

```typescript
// admin/admin.module.ts
import { NgModule } from '@angular/core';
import { CommonModule } from '@angular/common';
import { AdminRoutingModule } from './admin-routing.module';
import { AdminDashboardComponent } from './admin-dashboard/admin-dashboard.component';
import { UserManagementComponent } from './user-management/user-management.component';
import { SettingsComponent } from './settings/settings.component';

@NgModule({
  declarations: [
    AdminDashboardComponent,
    UserManagementComponent,
    SettingsComponent
  ],
  imports: [
    CommonModule,
    AdminRoutingModule
  ]
})
export class AdminModule { }

// admin/admin-routing.module.ts
import { NgModule } from '@angular/core';
import { RouterModule, Routes } from '@angular/router';
import { AdminDashboardComponent } from './admin-dashboard/admin-dashboard.component';
import { UserManagementComponent } from './user-management/user-management.component';
import { SettingsComponent } from './settings/settings.component';

const routes: Routes = [
  {
    path: '',
    component: AdminDashboardComponent,
    children: [
      { path: '', redirectTo: 'users', pathMatch: 'full' },
      { path: 'users', component: UserManagementComponent },
      { path: 'settings', component: SettingsComponent }
    ]
  }
];

@NgModule({
  imports: [RouterModule.forChild(routes)],
  exports: [RouterModule]
})
export class AdminRoutingModule { }

// admin/admin-dashboard/admin-dashboard.component.ts
import { Component } from '@angular/core';

@Component({
  selector: 'app-admin-dashboard',
  template: `
    <div class="admin-dashboard">
      <nav class="admin-nav">
        <h2>Admin Panel</h2>
        <ul>
          <li><a routerLink="users" routerLinkActive="active">User Management</a></li>
          <li><a routerLink="settings" routerLinkActive="active">Settings</a></li>
        </ul>
      </nav>
      <div class="admin-content">
        <router-outlet></router-outlet>
      </div>
    </div>
  `,
  styles: [`
    .admin-dashboard {
      display: flex;
      min-height: 100vh;
    }
    .admin-nav {
      width: 250px;
      background-color: #343a40;
      color: white;
      padding: 20px;
    }
    .admin-nav ul {
      list-style: none;
      padding: 0;
    }
    .admin-nav li {
      margin: 10px 0;
    }
    .admin-nav a {
      color: white;
      text-decoration: none;
      padding: 10px;
      display: block;
      border-radius: 4px;
    }
    .admin-nav a:hover, .admin-nav a.active {
      background-color: #007bff;
    }
    .admin-content {
      flex: 1;
      padding: 20px;
    }
  `]
})
export class AdminDashboardComponent {}

// admin/user-management/user-management.component.ts
import { Component } from '@angular/core';

@Component({
  selector: 'app-user-management',
  template: `
    <div class="user-management">
      <h3>User Management</h3>
      <p>This component is part of the lazy-loaded Admin module.</p>
      <table>
        <thead>
          <tr>
            <th>ID</th>
            <th>Name</th>
            <th>Email</th>
            <th>Role</th>
          </tr>
        </thead>
        <tbody>
          <tr *ngFor="let user of users">
            <td>{{ user.id }}</td>
            <td>{{ user.name }}</td>
            <td>{{ user.email }}</td>
            <td>{{ user.role }}</td>
          </tr>
        </tbody>
      </table>
    </div>
  `,
  styles: [`
    .user-management {
      padding: 20px;
    }
    table {
      width: 100%;
      border-collapse: collapse;
    }
    th, td {
      padding: 12px;
      text-align: left;
      border-bottom: 1px solid #ddd;
    }
    th {
      background-color: #007bff;
      color: white;
    }
  `]
})
export class UserManagementComponent {
  users = [
    { id: 1, name: 'John Doe', email: 'john@example.com', role: 'Admin' },
    { id: 2, name: 'Jane Smith', email: 'jane@example.com', role: 'User' },
    { id: 3, name: 'Bob Johnson', email: 'bob@example.com', role: 'User' }
  ];
}

// admin/settings/settings.component.ts
import { Component } from '@angular/core';

@Component({
  selector: 'app-settings',
  template: `
    <div class="settings">
      <h3>Settings</h3>
      <p>Configure application settings here.</p>
      <div class="setting-item">
        <label>Email Notifications:</label>
        <input type="checkbox" [(ngModel)]="emailNotifications">
      </div>
      <div class="setting-item">
        <label>Auto Logout (minutes):</label>
        <input type="number" [(ngModel)]="autoLogoutTime">
      </div>
      <button (click)="saveSettings()">Save Settings</button>
    </div>
  `,
  styles: [`
    .settings {
      padding: 20px;
    }
    .setting-item {
      margin: 15px 0;
    }
    label {
      display: inline-block;
      width: 200px;
      font-weight: bold;
    }
    button {
      margin-top: 20px;
      padding: 10px 20px;
      background-color: #28a745;
      color: white;
      border: none;
      border-radius: 4px;
      cursor: pointer;
    }
  `]
})
export class SettingsComponent {
  emailNotifications = true;
  autoLogoutTime = 30;

  saveSettings(): void {
    console.log('Settings saved', {
      emailNotifications: this.emailNotifications,
      autoLogoutTime: this.autoLogoutTime
    });
    alert('Settings saved successfully!');
  }
}

// app-routing.module.ts (main routing with lazy loading)
import { NgModule } from '@angular/core';
import { RouterModule, Routes } from '@angular/router';
import { HomeComponent } from './home/home.component';

const routes: Routes = [
  { path: '', component: HomeComponent },
  {
    path: 'admin',
    loadChildren: () => import('./admin/admin.module').then(m => m.AdminModule),
    // canLoad: [AuthGuard] // Optional: protect lazy loading
  }
];

@NgModule({
  imports: [RouterModule.forRoot(routes)],
  exports: [RouterModule]
})
export class AppRoutingModule { }
```

**Key Concepts:**
- Feature modules
- Lazy loading with loadChildren
- Dynamic imports
- RouterModule.forChild()
- Nested routing
- Child routes
- Code splitting
- Performance optimization

---

### Challenge 10: Implement Change Detection Strategy and Performance Optimization

**Difficulty:** Hard

**Description:**
Create a component that demonstrates performance optimization using OnPush change detection strategy, trackBy function, and pure pipes.

**Requirements:**
- Use OnPush change detection strategy
- Implement trackBy for *ngFor
- Create a pure pipe for expensive operations
- Demonstrate performance improvement
- Use immutable data patterns

**Solution:**

```typescript
// item.model.ts
export interface Item {
  id: number;
  name: string;
  price: number;
  quantity: number;
}

// expensive-calculation.pipe.ts
import { Pipe, PipeTransform } from '@angular/core';

@Pipe({
  name: 'expensiveCalculation',
  pure: true // Pure pipe - only recalculates when input changes
})
export class ExpensiveCalculationPipe implements PipeTransform {
  transform(items: Item[]): number {
    console.log('ExpensiveCalculationPipe - transform called');
    
    // Simulate expensive calculation
    let total = 0;
    for (let i = 0; i < 1000; i++) {
      total = items.reduce((sum, item) => sum + (item.price * item.quantity), 0);
    }
    
    return total;
  }
}

// optimized-list.component.ts
import { Component, ChangeDetectionStrategy, ChangeDetectorRef } from '@angular/core';
import { Item } from './item.model';

@Component({
  selector: 'app-optimized-list',
  changeDetection: ChangeDetectionStrategy.OnPush,
  template: `
    <div class="optimized-list">
      <h2>Performance Optimized List</h2>
      
      <div class="controls">
        <button (click)="addItem()">Add Item</button>
        <button (click)="updateFirstItem()">Update First Item</button>
        <button (click)="removeLastItem()">Remove Last Item</button>
        <button (click)="triggerChangeDetection()">Force Change Detection</button>
      </div>

      <div class="stats">
        <p><strong>Total Items:</strong> {{ items.length }}</p>
        <p><strong>Total Value:</strong> \${{ items | expensiveCalculation | number:'1.2-2' }}</p>
        <p><strong>Change Detection Count:</strong> {{ changeDetectionCount }}</p>
      </div>

      <div class="items">
        <div 
          *ngFor="let item of items; trackBy: trackByItemId" 
          class="item-card">
          <h3>{{ item.name }}</h3>
          <p>Price: \${{ item.price }} | Quantity: {{ item.quantity }}</p>
          <p>Subtotal: \${{ item.price * item.quantity }}</p>
        </div>
      </div>

      <div class="info">
        <h3>Optimization Techniques:</h3>
        <ul>
          <li>OnPush Change Detection Strategy</li>
          <li>trackBy function for *ngFor</li>
          <li>Pure pipe for expensive calculations</li>
          <li>Immutable data patterns</li>
        </ul>
      </div>
    </div>
  `,
  styles: [`
    .optimized-list {
      max-width: 800px;
      margin: 20px auto;
      padding: 20px;
    }
    .controls {
      display: flex;
      gap: 10px;
      margin-bottom: 20px;
      flex-wrap: wrap;
    }
    .controls button {
      padding: 10px 15px;
      background-color: #007bff;
      color: white;
      border: none;
      border-radius: 4px;
      cursor: pointer;
    }
    .stats {
      background-color: #f8f9fa;
      padding: 15px;
      border-radius: 4px;
      margin-bottom: 20px;
    }
    .stats p {
      margin: 5px 0;
    }
    .item-card {
      border: 1px solid #ddd;
      padding: 15px;
      margin-bottom: 10px;
      border-radius: 4px;
    }
    .item-card h3 {
      margin-top: 0;
      color: #007bff;
    }
    .info {
      margin-top: 30px;
      background-color: #d4edda;
      padding: 15px;
      border-radius: 4px;
    }
    .info ul {
      margin: 10px 0;
    }
  `]
})
export class OptimizedListComponent {
  items: Item[] = [
    { id: 1, name: 'Laptop', price: 999, quantity: 2 },
    { id: 2, name: 'Mouse', price: 29, quantity: 5 },
    { id: 3, name: 'Keyboard', price: 79, quantity: 3 }
  ];

  changeDetectionCount = 0;
  private nextId = 4;

  constructor(private cdr: ChangeDetectorRef) {
    // Count change detection cycles
    setInterval(() => {
      this.changeDetectionCount++;
    }, 1000);
  }

  // TrackBy function to optimize *ngFor
  trackByItemId(index: number, item: Item): number {
    return item.id;
  }

  addItem(): void {
    // Create new array (immutable pattern)
    this.items = [
      ...this.items,
      {
        id: this.nextId++,
        name: `Item ${this.nextId}`,
        price: Math.floor(Math.random() * 100) + 10,
        quantity: Math.floor(Math.random() * 5) + 1
      }
    ];
  }

  updateFirstItem(): void {
    if (this.items.length > 0) {
      // Create new array with updated first item (immutable pattern)
      this.items = [
        {
          ...this.items[0],
          price: Math.floor(Math.random() * 100) + 10
        },
        ...this.items.slice(1)
      ];
    }
  }

  removeLastItem(): void {
    if (this.items.length > 0) {
      // Create new array without last item (immutable pattern)
      this.items = this.items.slice(0, -1);
    }
  }

  triggerChangeDetection(): void {
    // Manually trigger change detection
    this.cdr.markForCheck();
  }
}

// comparison.component.ts (for demonstration)
import { Component } from '@angular/core';
import { Item } from './item.model';

@Component({
  selector: 'app-comparison',
  // Note: Using Default change detection strategy
  template: `
    <div class="comparison">
      <h2>Default Change Detection (Not Optimized)</h2>
      
      <div class="controls">
        <button (click)="addItem()">Add Item</button>
        <button (click)="updateFirstItem()">Update First Item</button>
      </div>

      <div class="stats">
        <p><strong>Total Items:</strong> {{ items.length }}</p>
        <p><strong>Total Value:</strong> \${{ calculateTotal() | number:'1.2-2' }}</p>
      </div>

      <div class="items">
        <div *ngFor="let item of items" class="item-card">
          <h3>{{ item.name }}</h3>
          <p>Price: \${{ item.price }} | Quantity: {{ item.quantity }}</p>
        </div>
      </div>

      <div class="warning">
        <p><strong>Warning:</strong> calculateTotal() is called on every change detection cycle!</p>
        <p>Open console to see how many times it's called.</p>
      </div>
    </div>
  `,
  styles: [`
    .comparison {
      max-width: 800px;
      margin: 20px auto;
      padding: 20px;
      border: 2px solid #ffc107;
    }
    .warning {
      background-color: #fff3cd;
      color: #856404;
      padding: 15px;
      border-radius: 4px;
      margin-top: 20px;
    }
  `]
})
export class ComparisonComponent {
  items: Item[] = [
    { id: 1, name: 'Laptop', price: 999, quantity: 2 },
    { id: 2, name: 'Mouse', price: 29, quantity: 5 }
  ];

  private nextId = 3;

  calculateTotal(): number {
    console.log('calculateTotal() called - This is inefficient!');
    return this.items.reduce((sum, item) => sum + (item.price * item.quantity), 0);
  }

  addItem(): void {
    this.items.push({
      id: this.nextId++,
      name: `Item ${this.nextId}`,
      price: Math.floor(Math.random() * 100) + 10,
      quantity: Math.floor(Math.random() * 5) + 1
    });
  }

  updateFirstItem(): void {
    if (this.items.length > 0) {
      this.items[0].price = Math.floor(Math.random() * 100) + 10;
    }
  }
}
```

**Key Concepts:**
- ChangeDetectionStrategy.OnPush
- trackBy function for *ngFor optimization
- Pure pipes for expensive calculations
- Immutable data patterns (spread operator, slice)
- ChangeDetectorRef for manual change detection
- Performance optimization techniques
- Avoiding function calls in templates
- Proper use of RxJS observables with OnPush

---

## Summary

These 10 coding challenges cover the essential Angular concepts that are commonly tested in technical interviews:

1. **Component Basics** - Creating components with data binding and event handling
2. **Component Communication** - @Input, @Output, and EventEmitter
3. **Services and DI** - Creating injectable services with reactive state management
4. **Routing** - Navigation, route parameters, and programmatic routing
5. **Reactive Forms** - Form validation and custom validators
6. **HTTP and RxJS** - HTTP requests, error handling, and Observable operators
7. **Custom Pipes** - Creating reusable data transformation pipes
8. **Route Guards** - Authentication and route protection
9. **Lazy Loading** - Feature modules and performance optimization
10. **Performance** - OnPush change detection, trackBy, and optimization techniques

Each challenge includes:
- Clear requirements
- Complete working code
- Key concepts explanation
- Best practices demonstration

Practice these challenges to strengthen your Angular skills and prepare for technical interviews!
