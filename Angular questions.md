# Angular Q&A

## Table of Contents

1. [Latest Introductions & Paradigms](#latest-introductions--paradigms)
   - Angular 16+ changes, new control flow (`@if`, `@for`, `@switch`), Deferrable Views, Standalone API
2. [Dependency Injection (DI)](#dependency-injection-di)
   - DI in Angular, provider levels, `inject()`, provider types, `InjectionToken`, `@Optional`/`@Self`
3. [Services](#services)
   - What is a Service, sharing data between components, `providedIn: 'root'` vs `'any'`, HTTP Interceptors
4. [Routing](#routing)
   - Angular Router, Route Guards, Lazy Loading, Route Parameters, Router State, Redirects
5. [Directives](#directives)
   - Structural vs Attribute directives, built-in directives, custom directives, `@HostBinding`/`@HostListener`, `ng-template`, `ng-container`, `ngTemplateOutlet`, `*` syntax, lifecycle hooks
6. [Signals](#signals)
   - `signal()`, `computed()`, `effect()`, Signals vs Observables, `toSignal()`/`toObservable()`, Signal-based components, Signal Inputs/Outputs, Resource API, Linked Signals
7. [Forms](#forms)
   - Template-driven vs Reactive, `FormControl`/`FormGroup`/`FormArray`, Custom Validators, Typed Forms, Signal-based Forms, `FormBuilder`
8. [Docker with Angular](#docker-with-angular)
   - Containerizing Angular, Docker Compose, environment variables, best practices
9. [Additional Angular Concepts](#additional-angular-concepts)
   - Change Detection, Lifecycle Hooks, Content Projection, `ViewChild`/`ContentChild`, Pipes
10. [Angular Fundamentals](#angular-fundamentals)
    - Angular vs AngularJS, NgModules, Data Binding (4 types), Angular CLI
11. [RxJS & Observables](#rxjs--observables)
    - `switchMap`, `concatMap`, `mergeMap`, `exhaustMap`, `combineLatest`, `shareReplay`, Subject types
12. [State Management](#state-management)
    - Service + BehaviorSubject, Service + Signals, NgRx (Redux), NgRx Component Store, when to use what

---

## Latest Introductions & Paradigms

### What are the major changes introduced in Angular 16+?

**Angular 16 (May 2023):**

- **Signals** - A new reactivity primitive for fine-grained change detection
- **Required Inputs** - `@Input({ required: true })`
- **DestroyRef** - A new way to handle component cleanup
- **Improved Server-Side Rendering (SSR)** with hydration
- **Standalone APIs** promoted to stable

**Angular 17 (November 2023):**

- **New Control Flow Syntax** - `@if`, `@for`, `@switch` (replacing *ngIf, *ngFor)
- **Deferrable Views** - `@defer` for lazy loading templates
- **Built-in Control Flow** - Better performance and type safety
- **New Application Builder (esbuild)** - Faster builds
- **View Transitions API** support

**Angular 18 (May 2024):**

- **Zoneless change detection** (experimental)
- **Material 3** components
- **Improved Signals APIs**
- **Route-level `@defer`**

**Angular 19 (November 2024):**

- **Signals fully stable**
- **Incremental hydration** for SSR
- **Enhanced zoneless support**
- **Linked signals** and **resource API**

### What is the new control flow syntax?

The new control flow syntax replaces structural directives with built-in template syntax:

**Old Way:**

```typescript
<div *ngIf="user">{{ user.name }}</div>

<ul>
  <li *ngFor="let item of items; track item.id">{{ item.name }}</li>
</ul>

<div [ngSwitch]="status">
  <p *ngSwitchCase="'active'">Active</p>
  <p *ngSwitchCase="'inactive'">Inactive</p>
  <p *ngSwitchDefault>Unknown</p>
</div>
```

**New Way (Angular 17+):**

```typescript
@if (user) {
  <div>{{ user.name }}</div>
} @else {
  <div>No user</div>
}

<ul>
  @for (item of items; track item.id) {
    <li>{{ item.name }}</li>
  } @empty {
    <li>No items</li>
  }
</ul>

@switch (status) {
  @case ('active') {
    <p>Active</p>
  }
  @case ('inactive') {
    <p>Inactive</p>
  }
  @default {
    <p>Unknown</p>
  }
}
```

**Benefits:**

- Better type checking
- Required track function (prevents common bugs)
- Built-in `@empty` block for collections
- No need to import CommonModule
- Better performance

### What are Deferrable Views?

Deferrable views allow lazy loading of template sections:

```typescript
@defer (on viewport) {
  <heavy-component />
} @placeholder {
  <div>Loading...</div>
} @loading (minimum 2s) {
  <spinner />
} @error {
  <div>Failed to load</div>
}
```

**Triggers:**

- `on idle` - When browser is idle
- `on viewport` - When element enters viewport
- `on interaction` - On click/focus
- `on hover` - On mouse hover
- `on immediate` - Immediately (but async)
- `on timer(3s)` - After specified time
- `when condition` - When expression is truthy

**Prefetching:**

```typescript
@defer (on interaction; prefetch on idle) {
  <heavy-component />
}
```

### What is the Standalone API?

Standalone components don't need to be declared in NgModules:

```typescript
import { Component } from '@angular/core';
import { CommonModule } from '@angular/common';
import { RouterOutlet } from '@angular/router';

@Component({
  selector: 'app-root',
  standalone: true,
  imports: [CommonModule, RouterOutlet, OtherStandaloneComponent],
  template: `<h1>Hello</h1>`
})
export class AppComponent {}
```

**Bootstrapping:**

```typescript
// main.ts
import { bootstrapApplication } from '@angular/platform-browser';
import { AppComponent } from './app/app.component';
import { provideRouter } from '@angular/router';
import { provideHttpClient } from '@angular/common/http';

bootstrapApplication(AppComponent, {
  providers: [
    provideRouter(routes),
    provideHttpClient(),
    // other providers
  ]
});
```

**Benefits:**

- Simpler mental model
- Better tree-shaking
- Easier testing
- Clear dependencies

---

## How to Organize an Angular Project

### Recommended Structure (feature-first)

The **feature-first** structure (not layer-first) groups code by business feature/domain rather than by file type. This is the approach recommended by the Angular community for medium-to-large applications.

```sh
src/
├── app/
│   ├── core/                        # Singleton services, guards, interceptors
│   │   ├── guards/
│   │   │   └── auth.guard.ts
│   │   ├── interceptors/
│   │   │   └── auth.interceptor.ts
│   │   └── services/
│   │       └── auth.service.ts
│   │
│   ├── shared/                      # Reusable UI: components, pipes, directives
│   │   ├── components/
│   │   │   └── spinner/
│   │   ├── directives/
│   │   └── pipes/
│   │
│   ├── features/                    # Features: each folder is lazy-loaded
│   │   ├── auth/
│   │   │   ├── login/
│   │   │   │   └── login.component.ts
│   │   │   ├── auth.routes.ts
│   │   │   └── auth.service.ts
│   │   ├── dashboard/
│   │   │   ├── dashboard.component.ts
│   │   │   └── dashboard.routes.ts
│   │   └── users/
│   │       ├── user-list/
│   │       ├── user-detail/
│   │       ├── users.routes.ts
│   │       └── users.service.ts
│   │
│   ├── app.component.ts
│   ├── app.routes.ts               # Root routes with lazy loading
│   └── app.config.ts               # provideRouter, provideHttpClient, etc.
│
├── environments/
│   ├── environment.ts              # Dev: { production: false, apiUrl: '...' }
│   └── environment.prod.ts
└── assets/
```

### Key Principles

1. **Feature-first:** group everything belonging to a feature (component, service, routes, models) in the same folder.
2. **Lazy loading per feature:** each feature is loaded only when the user navigates to that route.
3. **`core/`** contains only app-wide singletons (auth, interceptors, guards). Never import `core/` from within `features/`.
4. **`shared/`** contains *presentational* and reusable components, pipes, and directives. No business logic.
5. **Standalone components (Angular 15+):** NgModules are gone — each component declares its own dependencies via `imports: []`.
6. **Barrel files (`index.ts`):** export the public API of each folder, hiding internal details.

### Lazy loading con standalone (Angular 15+)

```typescript
// app.routes.ts
export const appRoutes: Routes = [
  { path: '', redirectTo: 'dashboard', pathMatch: 'full' },
  {
    path: 'dashboard',
    loadComponent: () =>
      import('./features/dashboard/dashboard.component').then(m => m.DashboardComponent),
  },
  {
    path: 'users',
    loadChildren: () =>
      import('./features/users/users.routes').then(m => m.usersRoutes),
  },
];

// features/users/users.routes.ts
export const usersRoutes: Routes = [
  { path: '', component: UserListComponent },
  { path: ':id', component: UserDetailComponent },
];
```

### Practical Differences: Component vs Service vs Module

|                        | Component                 | Service                     | Module (NgModule)          |
|------------------------|---------------------------|-----------------------------|----------------------------|
| **Purpose**            | View + user interaction   | Business logic, data access | Grouping (legacy)          |
| **Decorator**          | `@Component`              | `@Injectable`               | `@NgModule`                |
| **Has template?**      | ✅ Yes                     | ❌ No                        | ❌ No                       |
| **Singleton?**         | ❌ No (instance per use)  | ✅ With `providedIn: 'root'` | -                          |
| **Recommended today?** | ✅ Standalone              | ✅                           | ⚠️ Superseded by Standalone |

```typescript
// Standalone component (Angular 15+)
@Component({
  selector: 'app-user-card',
  standalone: true,
  imports: [CommonModule, RouterLink],
  template: `<div>{{ user().name }}</div>`,
})
export class UserCardComponent {
  user = input.required<User>();  // Signal Input (Angular 17+)
}

// Singleton service
@Injectable({ providedIn: 'root' })
export class UserService {
  private readonly http = inject(HttpClient);
  private readonly users = signal<User[]>([]);

  readonly userCount = computed(() => this.users().length);

  loadUsers() {
    return this.http.get<User[]>('/api/users').pipe(
      tap(users => this.users.set(users))
    );
  }
}
```

---

## Dependency Injection (DI)

### What is Dependency Injection in Angular?

Dependency Injection is a design pattern where a class receives its dependencies from external sources rather than creating them itself.

```typescript
// Without DI (bad)
export class UserComponent {
  private userService = new UserService(); // tight coupling
}

// With DI (good)
export class UserComponent {
  constructor(private userService: UserService) {} // injected
}
```

**Benefits:**

- Loose coupling
- Easier testing (can inject mocks)
- Better code reusability
- Centralized configuration

### What are the different levels of providers?

**1. Root Level (Application-wide):**

```typescript
@Injectable({
  providedIn: 'root' // Single instance for entire app
})
export class DataService {}
```

**2. Module Level:**

```typescript
@NgModule({
  providers: [DataService] // Instance per module
})
export class FeatureModule {}
```

**3. Component Level:**

```typescript
@Component({
  selector: 'app-user',
  providers: [DataService] // New instance per component
})
export class UserComponent {}
```

**4. Route Level:**

```typescript
const routes: Routes = [
  {
    path: 'admin',
    providers: [AdminService],
    loadComponent: () => import('./admin.component')
  }
];
```

**5. Element Injector (with standalone):**

```typescript
@Component({
  selector: 'app-child',
  standalone: true,
  providers: [ChildService],
  viewProviders: [ViewService] // Only for view children, not content children
})
export class ChildComponent {}
```

### What is the inject() function?

The `inject()` function allows dependency injection outside of constructors (Angular 14+):

```typescript
import { inject } from '@angular/core';

export class UserComponent {
  private userService = inject(UserService);
  private router = inject(Router);
  
  // Can use in property initializers
  private userId = inject(ActivatedRoute).snapshot.params['id'];
}
```

**Benefits:**

- Cleaner syntax
- Can be used in functions
- Easier to create composable logic

**Usage in functions:**

```typescript
// Custom injection function
export function injectUser() {
  const route = inject(ActivatedRoute);
  const userService = inject(UserService);
  const userId = route.snapshot.params['id'];
  return userService.getUser(userId);
}

// In component
export class ProfileComponent {
  user$ = injectUser();
}
```

### What are different provider types?

**1. Class Provider:**

```typescript
providers: [
  { provide: UserService, useClass: UserService }
  // Shorthand: UserService
]
```

**2. Value Provider:**

```typescript
providers: [
  { provide: 'API_URL', useValue: 'https://api.example.com' }
]
```

**3. Factory Provider:**

```typescript
providers: [
  {
    provide: UserService,
    useFactory: (http: HttpClient) => {
      return environment.production 
        ? new UserService(http)
        : new MockUserService();
    },
    deps: [HttpClient]
  }
]
```

**4. Existing Provider (Alias):**

```typescript
providers: [
  UserService,
  { provide: 'USER_API', useExisting: UserService }
]
```

### What is the InjectionToken?

Used to create tokens for non-class dependencies:

```typescript
import { InjectionToken } from '@angular/core';

export interface AppConfig {
  apiUrl: string;
  timeout: number;
}

export const APP_CONFIG = new InjectionToken<AppConfig>('app.config', {
  providedIn: 'root',
  factory: () => ({
    apiUrl: 'https://api.example.com',
    timeout: 5000
  })
});

// Usage
export class ApiService {
  private config = inject(APP_CONFIG);
  
  constructor() {
    console.log(this.config.apiUrl);
  }
}
```

### What are Optional and Self decorators?

**@Optional()** - Allows null if dependency not found:

```typescript
constructor(@Optional() private service: OptionalService) {
  if (this.service) {
    this.service.doSomething();
  }
}
```

**@Self()** - Only look in the current injector:

```typescript
constructor(@Self() private service: LocalService) {}
```

**@SkipSelf()** - Skip current injector, look in parent:

```typescript
constructor(@SkipSelf() private service: ParentService) {}
```

**@Host()** - Look up to host component only:

```typescript
constructor(@Host() private parentForm: NgForm) {}
```

---

## Services

### What is a Service in Angular?

A service is a class with a specific purpose, typically used for:

- Data sharing between components
- Business logic
- API calls
- State management

```typescript
import { Injectable } from '@angular/core';
import { HttpClient } from '@angular/common/http';
import { Observable } from 'rxjs';

@Injectable({
  providedIn: 'root'
})
export class UserService {
  private apiUrl = 'https://api.example.com/users';

  constructor(private http: HttpClient) {}

  getUsers(): Observable<User[]> {
    return this.http.get<User[]>(this.apiUrl);
  }

  getUser(id: number): Observable<User> {
    return this.http.get<User>(`${this.apiUrl}/${id}`);
  }

  createUser(user: User): Observable<User> {
    return this.http.post<User>(this.apiUrl, user);
  }
}
```

### How do you share data between components using services?

**Using BehaviorSubject:**

```typescript
@Injectable({ providedIn: 'root' })
export class DataService {
  private dataSubject = new BehaviorSubject<string>('initial');
  data$ = this.dataSubject.asObservable();

  updateData(data: string) {
    this.dataSubject.next(data);
  }
}

// Component A
export class ComponentA {
  constructor(private dataService: DataService) {}
  
  update() {
    this.dataService.updateData('new value');
  }
}

// Component B
export class ComponentB {
  data$ = inject(DataService).data$;
}
```

**Using Signals (Angular 16+):**

```typescript
@Injectable({ providedIn: 'root' })
export class DataService {
  private dataSignal = signal('initial');
  readonly data = this.dataSignal.asReadonly();

  updateData(data: string) {
    this.dataSignal.set(data);
  }
}

// Component
export class Component {
  dataService = inject(DataService);
  data = this.dataService.data; // Signal
}
```

### What is the difference between providedIn: 'root' vs providedIn: 'any'?

**providedIn: 'root':**

```typescript
@Injectable({ providedIn: 'root' })
export class SingletonService {
  // One instance for entire application
}
```

- Single instance across the app
- Tree-shakable (removed if not used)
- Most common choice

**providedIn: 'any':**

```typescript
@Injectable({ providedIn: 'any' })
export class MultiInstanceService {
  // One instance per lazy-loaded module
}
```

- New instance for each lazy-loaded module
- Useful when you need isolation between modules

**providedIn: 'platform':**

```typescript
@Injectable({ providedIn: 'platform' })
export class PlatformService {
  // Shared across multiple Angular apps on same page
}
```

### How do you handle HTTP interceptors?

HTTP Interceptors intercept and modify HTTP requests/responses:

```typescript
import { HttpInterceptorFn } from '@angular/common/http';

// Functional Interceptor (Angular 15+)
export const authInterceptor: HttpInterceptorFn = (req, next) => {
  const authToken = inject(AuthService).getToken();
  
  const authReq = req.clone({
    headers: req.headers.set('Authorization', `Bearer ${authToken}`)
  });
  
  return next(authReq);
};

// Error handling interceptor
export const errorInterceptor: HttpInterceptorFn = (req, next) => {
  return next(req).pipe(
    catchError((error: HttpErrorResponse) => {
      if (error.status === 401) {
        inject(Router).navigate(['/login']);
      }
      return throwError(() => error);
    })
  );
};

// Logging interceptor
export const loggingInterceptor: HttpInterceptorFn = (req, next) => {
  console.log('Request:', req.url);
  const started = Date.now();
  
  return next(req).pipe(
    tap(event => {
      if (event.type === HttpEventType.Response) {
        console.log(`Response: ${req.url} in ${Date.now() - started}ms`);
      }
    })
  );
};
```

**Providing Interceptors:**

```typescript
// main.ts
bootstrapApplication(AppComponent, {
  providers: [
    provideHttpClient(
      withInterceptors([authInterceptor, errorInterceptor, loggingInterceptor])
    )
  ]
});
```

**Class-based Interceptor (legacy):**

```typescript
@Injectable()
export class AuthInterceptor implements HttpInterceptor {
  intercept(req: HttpRequest<any>, next: HttpHandler): Observable<HttpEvent<any>> {
    const authToken = inject(AuthService).getToken();
    const authReq = req.clone({
      setHeaders: { Authorization: `Bearer ${authToken}` }
    });
    return next.handle(authReq);
  }
}
```

---

## Routing

### What is Angular Router?

Angular Router enables navigation between views/components:

```typescript
import { Routes } from '@angular/router';

export const routes: Routes = [
  { path: '', component: HomeComponent },
  { path: 'about', component: AboutComponent },
  { path: 'users/:id', component: UserDetailComponent },
  { path: '**', component: NotFoundComponent } // Wildcard
];
```

**Setup (Standalone):**

```typescript
// main.ts
import { provideRouter } from '@angular/router';
import { routes } from './app/app.routes';

bootstrapApplication(AppComponent, {
  providers: [provideRouter(routes)]
});
```

**Template:**

```html
<nav>
  <a routerLink="/">Home</a>
  <a routerLink="/about">About</a>
  <a [routerLink]="['/users', userId]">User</a>
</nav>

<router-outlet />
```

### What are Route Guards?

Guards control access to routes:

**1. CanActivate (Functional Guard):**

```typescript
import { inject } from '@angular/core';
import { Router } from '@angular/router';

export const authGuard = () => {
  const authService = inject(AuthService);
  const router = inject(Router);
  
  if (authService.isLoggedIn()) {
    return true;
  }
  
  return router.createUrlTree(['/login']);
};

// Usage
const routes: Routes = [
  {
    path: 'admin',
    component: AdminComponent,
    canActivate: [authGuard]
  }
];
```

**2. CanActivateChild:**

```typescript
export const parentGuard = () => {
  return inject(AuthService).hasParentAccess();
};

const routes: Routes = [
  {
    path: 'parent',
    canActivateChild: [parentGuard],
    children: [
      { path: 'child1', component: Child1Component },
      { path: 'child2', component: Child2Component }
    ]
  }
];
```

**3. CanDeactivate:**

```typescript
export interface CanComponentDeactivate {
  canDeactivate: () => boolean | Observable<boolean>;
}

export const unsavedChangesGuard = (component: CanComponentDeactivate) => {
  return component.canDeactivate 
    ? component.canDeactivate() 
    : true;
};

@Component({...})
export class FormComponent implements CanComponentDeactivate {
  hasUnsavedChanges = false;
  
  canDeactivate(): boolean {
    if (this.hasUnsavedChanges) {
      return confirm('You have unsaved changes. Leave anyway?');
    }
    return true;
  }
}

const routes: Routes = [
  {
    path: 'form',
    component: FormComponent,
    canDeactivate: [unsavedChangesGuard]
  }
];
```

**4. CanMatch (for lazy loading):**

```typescript
export const featureFlagGuard = (route: Route) => {
  const features = inject(FeatureService);
  return features.isEnabled(route.data?.['feature']);
};

const routes: Routes = [
  {
    path: 'new-feature',
    canMatch: [featureFlagGuard],
    data: { feature: 'newFeature' },
    loadComponent: () => import('./new-feature.component')
  }
];
```

**5. Resolve (data fetching):**

```typescript
export const userResolver = (route: ActivatedRouteSnapshot) => {
  const userService = inject(UserService);
  return userService.getUser(route.params['id']);
};

const routes: Routes = [
  {
    path: 'users/:id',
    component: UserComponent,
    resolve: { user: userResolver }
  }
];

// Component
export class UserComponent {
  route = inject(ActivatedRoute);
  user = this.route.snapshot.data['user'];
}
```

### What is lazy loading in routing?

Lazy loading loads feature modules on-demand:

```typescript
const routes: Routes = [
  {
    path: 'admin',
    loadChildren: () => import('./admin/admin.routes')
      .then(m => m.ADMIN_ROUTES)
  },
  {
    path: 'dashboard',
    loadComponent: () => import('./dashboard/dashboard.component')
      .then(m => m.DashboardComponent)
  }
];
```

**Benefits:**

- Faster initial load
- Smaller bundle size
- Better performance

**Preloading Strategy:**

```typescript
import { PreloadAllModules } from '@angular/router';

bootstrapApplication(AppComponent, {
  providers: [
    provideRouter(routes, 
      withPreloading(PreloadAllModules) // Preload in background
    )
  ]
});
```

**Custom Preloading:**

```typescript
export class CustomPreloadingStrategy implements PreloadingStrategy {
  preload(route: Route, load: () => Observable<any>): Observable<any> {
    return route.data?.['preload'] ? load() : of(null);
  }
}

const routes: Routes = [
  {
    path: 'important',
    data: { preload: true },
    loadChildren: () => import('./important/routes')
  }
];
```

### How do you handle route parameters?

**Route Parameters:**

```typescript
const routes: Routes = [
  { path: 'users/:id', component: UserComponent },
  { path: 'posts/:postId/comments/:commentId', component: CommentComponent }
];
```

**Accessing in Component:**

```typescript
export class UserComponent {
  route = inject(ActivatedRoute);
  
  // Snapshot (one-time read)
  userId = this.route.snapshot.params['id'];
  
  // Observable (for parameter changes)
  userId$ = this.route.params.pipe(
    map(params => params['id'])
  );
  
  // Using paramMap (recommended)
  userId$ = this.route.paramMap.pipe(
    map(params => params.get('id'))
  );
}
```

**Query Parameters:**

```typescript
// Navigate with query params
constructor(private router: Router) {}

navigate() {
  this.router.navigate(['/users'], { 
    queryParams: { page: 2, sort: 'name' } 
  });
  // URL: /users?page=2&sort=name
}

// Read query params
export class UserListComponent {
  route = inject(ActivatedRoute);
  
  page$ = this.route.queryParamMap.pipe(
    map(params => params.get('page'))
  );
}
```

**Route Data:**

```typescript
const routes: Routes = [
  {
    path: 'admin',
    component: AdminComponent,
    data: { title: 'Admin Panel', roles: ['admin'] }
  }
];

// Access data
export class AdminComponent {
  route = inject(ActivatedRoute);
  title = this.route.snapshot.data['title'];
}
```

### What is Router State and how to use it?

Router state contains navigation information:

```typescript
export class NavigationService {
  router = inject(Router);
  
  navigateWithState() {
    this.router.navigate(['/details'], {
      state: { data: 'some data', returnUrl: '/previous' }
    });
  }
}

// Access state
export class DetailsComponent {
  router = inject(Router);
  
  ngOnInit() {
    const navigation = this.router.getCurrentNavigation();
    const state = navigation?.extras.state;
    console.log(state?.['data']); // 'some data'
    
    // Or from location
    const currentState = window.history.state;
    console.log(currentState);
  }
}
```

### What are Route Redirects?

```typescript
const routes: Routes = [
  { path: '', redirectTo: '/home', pathMatch: 'full' },
  { path: 'home', component: HomeComponent },
  { path: 'old-path', redirectTo: '/new-path' },
  { path: 'new-path', component: NewComponent }
];
```

**pathMatch options:**

- `'full'` - Entire URL must match
- `'prefix'` - URL starts with the path (default)

---

## Directives

### What are Directives in Angular?

Directives are classes that add behavior to elements in your Angular applications. There are three types:

1. **Component Directives** - Directives with a template (most common)
2. **Structural Directives** - Change the DOM layout by adding/removing elements (`*ngIf`, `*ngFor`)
3. **Attribute Directives** - Change the appearance or behavior of an element (`ngClass`, `ngStyle`)

**Key Points:**

- Directives use the `@Directive` decorator
- They can manipulate the DOM, handle events, and modify element properties
- Custom directives are reusable across components

---

### What is the difference between Structural and Attribute Directives?

| Aspect                  | Structural Directives                       | Attribute Directives                    |
|-------------------------|---------------------------------------------|-----------------------------------------|
| **Purpose**             | Modify DOM structure (add/remove elements)  | Modify element appearance/behavior      |
| **Syntax**              | Use `*` prefix (e.g., `*ngIf`, `*ngFor`)    | No prefix (e.g., `ngClass`, `ngStyle`)  |
| **DOM Changes**         | Add or remove elements from DOM             | Modify existing elements                |
| **Multiple on Element** | ❌ Only one structural directive per element | ✅ Multiple attribute directives allowed |
| **Examples**            | `*ngIf`, `*ngFor`, `*ngSwitch`              | `ngClass`, `ngStyle`, `ngModel`         |

**Example - Structural Directive:**

```typescript
<div *ngIf="isVisible">Content</div>
<!-- Adds/removes entire div from DOM -->
```

**Example - Attribute Directive:**

```typescript
<div [ngClass]="{ active: isActive }">Content</div>
<!-- Modifies class attribute of existing div -->
```

---

### What are the built-in Structural Directives?

**1. *ngIf** - Conditionally includes/removes elements:

```typescript
<!-- Basic usage -->
<div *ngIf="isLoggedIn">Welcome back!</div>

<!-- With else block -->
<div *ngIf="isLoggedIn; else loggedOut">
  Welcome back!
</div>
<ng-template #loggedOut>
  Please log in
</ng-template>

<!-- With then/else -->
<div *ngIf="user; then userBlock; else guestBlock"></div>
<ng-template #userBlock>Hello {{ user.name }}</ng-template>
<ng-template #guestBlock>Hello Guest</ng-template>

<!-- Modern syntax (Angular 17+) -->
@if (isLoggedIn) {
  <div>Welcome back!</div>
} @else {
  <div>Please log in</div>
}
```

**2. *ngFor** - Repeats elements for each item in a collection:

```typescript
<!-- Basic usage -->
<div *ngFor="let item of items">{{ item.name }}</div>

<!-- With index -->
<div *ngFor="let item of items; let i = index">
  {{ i + 1 }}. {{ item.name }}
</div>

<!-- With trackBy for performance -->
<div *ngFor="let item of items; trackBy: trackByFn">
  {{ item.name }}
</div>

// Component
trackByFn(index: number, item: any): number {
  return item.id; // or index
}

<!-- All available local variables -->
<div *ngFor="let item of items; 
             let i = index; 
             let first = first; 
             let last = last;
             let even = even;
             let odd = odd">
  {{ i }}: {{ item.name }}
</div>

<!-- Modern syntax (Angular 17+) -->
@for (item of items; track item.id) {
  <div>{{ item.name }}</div>
} @empty {
  <div>No items found</div>
}
```

**3. *ngSwitch** - Conditionally displays one element from several:

```typescript
<!-- Traditional syntax -->
<div [ngSwitch]="status">
  <p *ngSwitchCase="'active'">Status: Active</p>
  <p *ngSwitchCase="'inactive'">Status: Inactive</p>
  <p *ngSwitchCase="'pending'">Status: Pending</p>
  <p *ngSwitchDefault>Status: Unknown</p>
</div>

<!-- Modern syntax (Angular 17+) -->
@switch (status) {
  @case ('active') {
    <p>Status: Active</p>
  }
  @case ('inactive') {
    <p>Status: Inactive</p>
  }
  @case ('pending') {
    <p>Status: Pending</p>
  }
  @default {
    <p>Status: Unknown</p>
  }
}
```

---

### What are the built-in Attribute Directives?

**1. ngClass** - Dynamically add/remove CSS classes:

```typescript
<!-- Object syntax -->
<div [ngClass]="{ 
  'active': isActive, 
  'disabled': isDisabled,
  'highlight': isHighlighted 
}">
  Content
</div>

<!-- String syntax -->
<div [ngClass]="'class1 class2 class3'">Content</div>

<!-- Array syntax -->
<div [ngClass]="['class1', 'class2', isActive ? 'active' : '']">
  Content
</div>

<!-- Expression -->
<div [ngClass]="getClasses()">Content</div>

// Component
getClasses() {
  return {
    'active': this.isActive,
    'error': this.hasError
  };
}
```

**2. ngStyle** - Dynamically set inline styles:

```typescript
<!-- Object syntax -->
<div [ngStyle]="{ 
  'color': textColor, 
  'font-size': fontSize + 'px',
  'background-color': isActive ? 'blue' : 'gray'
}">
  Content
</div>

<!-- Expression -->
<div [ngStyle]="getStyles()">Content</div>

// Component
getStyles() {
  return {
    'color': this.textColor,
    'font-size': this.fontSize + 'px',
    'font-weight': this.isBold ? 'bold' : 'normal'
  };
}
```

**3. ngModel** - Two-way data binding for form inputs:

```typescript
<!-- Two-way binding -->
<input [(ngModel)]="username" />

<!-- One-way binding with event -->
<input [ngModel]="username" (ngModelChange)="onUsernameChange($event)" />

// Component
username: string = '';

onUsernameChange(value: string) {
  this.username = value;
  console.log('Username changed:', value);
}

// Note: Requires FormsModule import
import { FormsModule } from '@angular/forms';

@Component({
  imports: [FormsModule], // Standalone
  // or add to module imports array
})
```

**4. ngModelGroup** - Groups form controls:

```typescript
<form #userForm="ngForm">
  <div ngModelGroup="address">
    <input name="street" ngModel />
    <input name="city" ngModel />
  </div>
</form>
```

---

### How do you create a custom Attribute Directive?

Create a directive that changes background color on hover:

```typescript
import { Directive, ElementRef, HostListener, Input } from '@angular/core';

@Directive({
  selector: '[appHighlight]',
  standalone: true
})
export class HighlightDirective {
  @Input() appHighlight: string = 'yellow'; // Default color
  @Input() defaultColor: string = 'transparent';

  constructor(private el: ElementRef) {}

  @HostListener('mouseenter') onMouseEnter() {
    this.highlight(this.appHighlight);
  }

  @HostListener('mouseleave') onMouseLeave() {
    this.highlight(this.defaultColor);
  }

  private highlight(color: string) {
    this.el.nativeElement.style.backgroundColor = color;
  }
}

// Usage
<p appHighlight="lightblue">Hover over me!</p>
<p [appHighlight]="'lightgreen'" [defaultColor]="'white'">
  Custom colors
</p>
```

**More Examples:**

```typescript
// Click Outside Directive
@Directive({
  selector: '[appClickOutside]',
  standalone: true
})
export class ClickOutsideDirective {
  @Output() clickOutside = new EventEmitter<void>();

  @HostListener('document:click', ['$event.target'])
  onClick(target: HTMLElement) {
    const clickedInside = this.el.nativeElement.contains(target);
    if (!clickedInside) {
      this.clickOutside.emit();
    }
  }

  constructor(private el: ElementRef) {}
}

// Usage
<div appClickOutside (clickOutside)="closeDropdown()">
  Dropdown content
</div>
```

```typescript
// Auto-focus Directive
@Directive({
  selector: '[appAutoFocus]',
  standalone: true
})
export class AutoFocusDirective implements OnInit {
  constructor(private el: ElementRef) {}

  ngOnInit() {
    setTimeout(() => {
      this.el.nativeElement.focus();
    }, 0);
  }
}

// Usage
<input appAutoFocus type="text" />
```

---

### How do you create a custom Structural Directive?

Create a custom `*appUnless` directive (opposite of `*ngIf`):

```typescript
import { Directive, Input, TemplateRef, ViewContainerRef } from '@angular/core';

@Directive({
  selector: '[appUnless]',
  standalone: true
})
export class UnlessDirective {
  private hasView = false;

  @Input() set appUnless(condition: boolean) {
    if (!condition && !this.hasView) {
      // Create the view if condition is false
      this.viewContainer.createEmbeddedView(this.templateRef);
      this.hasView = true;
    } else if (condition && this.hasView) {
      // Remove the view if condition is true
      this.viewContainer.clear();
      this.hasView = false;
    }
  }

  constructor(
    private templateRef: TemplateRef<any>,
    private viewContainer: ViewContainerRef
  ) {}
}

// Usage
<div *appUnless="isLoggedIn">
  Please log in to continue
</div>
```

**Understanding the pieces:**

- **TemplateRef** - Reference to the `<ng-template>` (the content to render)
- **ViewContainerRef** - Container where the template will be inserted
- **createEmbeddedView()** - Instantiates the template
- **clear()** - Removes the view from the container

**More Complex Example - Repeat with Delay:**

```typescript
@Directive({
  selector: '[appRepeat]',
  standalone: true
})
export class RepeatDirective implements OnInit {
  @Input() appRepeat: number = 1;

  constructor(
    private templateRef: TemplateRef<any>,
    private viewContainer: ViewContainerRef
  ) {}

  ngOnInit() {
    for (let i = 0; i < this.appRepeat; i++) {
      this.viewContainer.createEmbeddedView(this.templateRef, {
        $implicit: i,
        index: i
      });
    }
  }
}

// Usage
<div *appRepeat="3; let i = index">
  Item {{ i }}
</div>
```

---

### What is the difference between @HostBinding and @HostListener?

Both decorators allow directives to interact with their host element:

**@HostListener** - Listen to events on the host element:

```typescript
@Directive({
  selector: '[appTrackClicks]',
  standalone: true
})
export class TrackClicksDirective {
  clickCount = 0;

  @HostListener('click', ['$event'])
  onClick(event: MouseEvent) {
    this.clickCount++;
    console.log('Clicked', this.clickCount, 'times');
  }

  @HostListener('mouseenter')
  onMouseEnter() {
    console.log('Mouse entered');
  }

  @HostListener('window:resize', ['$event'])
  onWindowResize(event: Event) {
    console.log('Window resized');
  }
}
```

**@HostBinding** - Bind to properties of the host element:

```typescript
@Directive({
  selector: '[appRainbow]',
  standalone: true
})
export class RainbowDirective {
  @HostBinding('style.color') color: string = 'black';
  @HostBinding('class.rainbow') isRainbow = true;
  @HostBinding('attr.data-custom') customAttr = 'value';

  @HostListener('mouseenter')
  onMouseEnter() {
    this.color = this.getRandomColor();
  }

  private getRandomColor(): string {
    const colors = ['red', 'blue', 'green', 'purple', 'orange'];
    return colors[Math.floor(Math.random() * colors.length)];
  }
}

// Usage
<p appRainbow>Hover to change color!</p>
```

**Comparison:**

| Feature   | @HostListener                  | @HostBinding                    |
|-----------|--------------------------------|---------------------------------|
| Purpose   | Listen to host events          | Bind to host properties         |
| Direction | Input (receiving events)       | Output (setting properties)     |
| Use Case  | Handle clicks, keyboard, mouse | Set classes, styles, attributes |
| Example   | `@HostListener('click')`       | `@HostBinding('class.active')`  |

---

### What is ng-template and ng-container?

**ng-template** - Defines a template that is not rendered by default:

```typescript
<!-- Basic usage -->
<ng-template #myTemplate>
  <p>This content is not rendered until explicitly told to</p>
</ng-template>

<!-- Used with *ngIf else -->
<div *ngIf="user; else noUser">
  Hello {{ user.name }}
</div>
<ng-template #noUser>
  <p>No user logged in</p>
</ng-template>

<!-- Rendered programmatically -->
@Component({
  template: `
    <ng-template #dynamicTemplate>
      <p>Dynamic content</p>
    </ng-template>
    <ng-container *ngTemplateOutlet="dynamicTemplate"></ng-container>
  `
})
```

**ng-container** - Grouping element that doesn't create a DOM node:

```typescript
<!-- Problem: Can't use multiple structural directives -->
<!-- ❌ This doesn't work -->
<div *ngIf="condition" *ngFor="let item of items">
  {{ item }}
</div>

<!-- ✅ Solution: Use ng-container -->
<ng-container *ngIf="condition">
  <div *ngFor="let item of items">
    {{ item }}
  </div>
</ng-container>

<!-- Use case: Grouping without extra DOM elements -->
<ng-container>
  <h1>Title</h1>
  <p>Description</p>
</ng-container>
<!-- Renders the h1 and p without a wrapper element -->

<!-- Use case: ngTemplateOutlet -->
<ng-container *ngTemplateOutlet="myTemplate; context: myContext">
</ng-container>
```

**Key Differences:**

| Feature        | ng-template                     | ng-container                     |
|----------------|---------------------------------|----------------------------------|
| **Rendered**   | Not rendered by default         | Rendered but no DOM node created |
| **Purpose**    | Define reusable templates       | Group elements without wrapper   |
| **Use Case**   | Template references, conditions | Multiple directives, grouping    |
| **DOM Impact** | None until instantiated         | None (no wrapper created)        |

---

### What is ngTemplateOutlet?

`ngTemplateOutlet` is a directive that inserts an embedded view from a template:

```typescript
@Component({
  template: `
    <!-- Define template -->
    <ng-template #greetingTemplate let-name="name" let-age="age">
      <p>Hello {{ name }}, you are {{ age }} years old</p>
    </ng-template>

    <!-- Use template with context -->
    <ng-container *ngTemplateOutlet="greetingTemplate; context: { 
      name: 'John', 
      age: 30 
    }">
    </ng-container>

    <!-- Reuse the same template -->
    <ng-container *ngTemplateOutlet="greetingTemplate; context: { 
      name: 'Jane', 
      age: 25 
    }">
    </ng-container>
  `
})
export class MyComponent {}
```

**Practical Example - Reusable Card Template:**

```typescript
@Component({
  template: `
    <!-- Card Template -->
    <ng-template #cardTemplate let-title="title" let-content="content">
      <div class="card">
        <h3>{{ title }}</h3>
        <p>{{ content }}</p>
      </div>
    </ng-template>

    <!-- Use template multiple times -->
    <ng-container *ngTemplateOutlet="cardTemplate; context: {
      title: 'Card 1',
      content: 'First card content'
    }"></ng-container>

    <ng-container *ngTemplateOutlet="cardTemplate; context: {
      title: 'Card 2',
      content: 'Second card content'
    }"></ng-container>
  `
})
export class CardsComponent {}
```

**Dynamic Template Selection:**

```typescript
@Component({
  template: `
    <ng-template #loadingTemplate>
      <p>Loading...</p>
    </ng-template>

    <ng-template #dataTemplate>
      <p>Data loaded!</p>
    </ng-template>

    <ng-template #errorTemplate>
      <p>Error occurred</p>
    </ng-template>

    <ng-container *ngTemplateOutlet="getCurrentTemplate()">
    </ng-container>
  `
})
export class DynamicComponent {
  @ViewChild('loadingTemplate') loadingTemplate!: TemplateRef<any>;
  @ViewChild('dataTemplate') dataTemplate!: TemplateRef<any>;
  @ViewChild('errorTemplate') errorTemplate!: TemplateRef<any>;

  state: 'loading' | 'success' | 'error' = 'loading';

  getCurrentTemplate(): TemplateRef<any> {
    switch(this.state) {
      case 'loading': return this.loadingTemplate;
      case 'success': return this.dataTemplate;
      case 'error': return this.errorTemplate;
    }
  }
}
```

---

### What is the * (asterisk) syntax in structural directives?

The `*` is syntactic sugar that Angular expands into a more verbose form.

**What you write:**

```typescript
<div *ngIf="condition">Content</div>
```

**What Angular generates:**

```typescript
<ng-template [ngIf]="condition">
  <div>Content</div>
</ng-template>
```

**More examples:**

```typescript
// What you write
<div *ngFor="let item of items">{{ item }}</div>

// What Angular generates
<ng-template ngFor let-item [ngForOf]="items">
  <div>{{ item }}</div>
</ng-template>
```

**Custom directive with * syntax:**

```typescript
// Directive
@Directive({
  selector: '[appDelay]'
})
export class DelayDirective {
  @Input() set appDelay(time: number) {
    setTimeout(() => {
      this.viewContainer.createEmbeddedView(this.templateRef);
    }, time);
  }

  constructor(
    private templateRef: TemplateRef<any>,
    private viewContainer: ViewContainerRef
  ) {}
}

// Usage with *
<div *appDelay="2000">
  This appears after 2 seconds
</div>

// Expanded form (what Angular sees)
<ng-template [appDelay]="2000">
  <div>This appears after 2 seconds</div>
</ng-template>
```

---

### How do you pass data to a structural directive?

Use the microsyntax with `let` variables:

```typescript
@Directive({
  selector: '[appRange]',
  standalone: true
})
export class RangeDirective implements OnInit {
  @Input() appRangeFrom: number = 0;
  @Input() appRangeTo: number = 10;

  constructor(
    private templateRef: TemplateRef<any>,
    private viewContainer: ViewContainerRef
  ) {}

  ngOnInit() {
    for (let i = this.appRangeFrom; i < this.appRangeTo; i++) {
      this.viewContainer.createEmbeddedView(this.templateRef, {
        $implicit: i,      // Used with 'let-variable'
        index: i,          // Used with 'let-index="index"'
        even: i % 2 === 0, // Used with 'let-even="even"'
        odd: i % 2 !== 0   // Used with 'let-odd="odd"'
      });
    }
  }
}

// Usage
<div *appRange="let num from 1 to 5">
  Number: {{ num }}
</div>

// With named variables
<div *appRange="let num; from: 1; to: 10; let idx = index; let isEven = even">
  {{ idx }}: {{ num }} ({{ isEven ? 'Even' : 'Odd' }})
</div>
```

**Context Object:**

- `$implicit` - Default value (used with just `let variable`)
- Any other properties can be accessed with `let alias = propertyName`

---

### What are Directive Lifecycle Hooks?

Directives have lifecycle hooks similar to components:

```typescript
@Directive({
  selector: '[appLifecycle]',
  standalone: true
})
export class LifecycleDirective implements 
  OnInit, 
  OnChanges, 
  AfterViewInit, 
  OnDestroy {

  @Input() appLifecycle: any;

  ngOnChanges(changes: SimpleChanges) {
    console.log('Input changed', changes);
  }

  ngOnInit() {
    console.log('Directive initialized');
  }

  ngAfterViewInit() {
    console.log('View initialized');
  }

  ngOnDestroy() {
    console.log('Directive destroyed');
  }
}
```

**Common Lifecycle Hooks for Directives:**

| Hook              | When Called                   | Use Case                        |
|-------------------|-------------------------------|---------------------------------|
| `ngOnChanges`     | When input properties change  | React to input changes          |
| `ngOnInit`        | After first ngOnChanges       | Initialize directive            |
| `ngDoCheck`       | During every change detection | Custom change detection         |
| `ngAfterViewInit` | After view initialization     | Access/manipulate DOM           |
| `ngOnDestroy`     | Before directive destruction  | Cleanup (subscriptions, timers) |

---

### Best Practices for Directives

1. **Use standalone directives (Angular 15+):**

```typescript
@Directive({
  selector: '[appMyDirective]',
  standalone: true  // ✅ Makes it easier to import
})
```

2. **Keep directives focused and reusable:**

```typescript
// ✅ Good - Single responsibility
@Directive({ selector: '[appTooltip]' })
export class TooltipDirective { }

// ❌ Bad - Too many responsibilities
@Directive({ selector: '[appManyThings]' })
export class ManyThingsDirective { }
```

3. **Use descriptive selector names with app prefix:**

```typescript
// ✅ Good
selector: '[appHighlight]'
selector: '[appClickOutside]'

// ❌ Bad - Could conflict with native attributes
selector: '[highlight]'
selector: '[click]'
```

4. **Clean up in ngOnDestroy:**

```typescript
@Directive({ selector: '[appAutoSave]' })
export class AutoSaveDirective implements OnDestroy {
  private saveInterval: any;

  ngOnInit() {
    this.saveInterval = setInterval(() => this.save(), 5000);
  }

  ngOnDestroy() {
    if (this.saveInterval) {
      clearInterval(this.saveInterval); // ✅ Clean up
    }
  }
}
```

5. **Use Renderer2 instead of direct DOM manipulation:**

```typescript
// ✅ Good - Safe and SSR-compatible
constructor(private el: ElementRef, private renderer: Renderer2) {}

ngOnInit() {
  this.renderer.setStyle(this.el.nativeElement, 'color', 'red');
  this.renderer.addClass(this.el.nativeElement, 'highlight');
}

// ❌ Bad - Direct DOM access
constructor(private el: ElementRef) {}

ngOnInit() {
  this.el.nativeElement.style.color = 'red'; // Not SSR-safe
}
```

6. **Type your template references:**

```typescript
@Directive({ selector: '[appTyped]' })
export class TypedDirective {
  constructor(
    private templateRef: TemplateRef<{ $implicit: number, index: number }>,
    private viewContainer: ViewContainerRef
  ) {}
}
```

---

## Signals

### What are Signals in Angular?

Signals are a new reactive primitive (Angular 16+) for managing state with automatic change detection:

```typescript
import { signal, computed, effect } from '@angular/core';

@Component({
  selector: 'app-counter',
  template: `
    <h1>Count: {{ count() }}</h1>
    <h2>Double: {{ double() }}</h2>
    <button (click)="increment()">+</button>
  `
})
export class CounterComponent {
  // Writable signal
  count = signal(0);
  
  // Computed signal (derived state)
  double = computed(() => this.count() * 2);
  
  constructor() {
    // Effect (side effect when signals change)
    effect(() => {
      console.log('Count changed:', this.count());
    });
  }
  
  increment() {
    this.count.update(value => value + 1);
    // or: this.count.set(this.count() + 1);
  }
}
```

**Key Features:**

- Fine-grained reactivity
- Automatic dependency tracking
- Glitch-free (computed values always consistent)
- Better performance than Zone.js

### What is the difference between signal(), computed(), and effect()?

**signal()** - Writable reactive value:

```typescript
const count = signal(0);
count.set(5);           // Set value
count.update(v => v + 1); // Update based on current
console.log(count());   // Read value: 6
```

**computed()** - Read-only derived value:

```typescript
const count = signal(10);
const doubled = computed(() => count() * 2);
const quadrupled = computed(() => doubled() * 2);

console.log(doubled());     // 20
console.log(quadrupled());  // 40

count.set(20);
console.log(doubled());     // 40 (automatically updated)
```

**effect()** - Side effects when dependencies change:

```typescript
const firstName = signal('John');
const lastName = signal('Doe');

effect(() => {
  console.log(`Name: ${firstName()} ${lastName()}`);
  // Runs when firstName or lastName changes
});

firstName.set('Jane'); // Logs: "Name: Jane Doe"
```

**Effect cleanup:**

```typescript
effect((onCleanup) => {
  const subscription = interval(1000).subscribe(console.log);
  
  onCleanup(() => {
    subscription.unsubscribe();
  });
});
```

### How do Signals compare to Observables?

| Feature              | Signals           | Observables (RxJS)                |
|----------------------|-------------------|-----------------------------------|
| **Synchronous**      | Yes               | No (async by default)             |
| **Always has value** | Yes               | No (cold observables)             |
| **Glitch-free**      | Yes               | No (can have intermediate states) |
| **Subscription**     | Automatic         | Manual (subscribe/unsubscribe)    |
| **Operators**        | Limited           | Rich ecosystem                    |
| **Use case**         | Synchronous state | Async operations, streams         |

**When to use each:**

**Signals:**

- Component state
- Form state
- Derived/computed values
- Simple synchronous state management

**Observables:**

- HTTP requests
- WebSocket streams
- Event streams
- Complex async operations
- Need operators (map, filter, debounce, etc.)

**Together:**

```typescript
export class UserComponent {
  private userService = inject(UserService);
  
  userId = signal(1);
  
  // Convert Observable to Signal
  user = toSignal(
    toObservable(this.userId).pipe(
      switchMap(id => this.userService.getUser(id))
    )
  );
}
```

### What are toSignal() and toObservable()?

**toSignal()** - Convert Observable to Signal:

```typescript
import { toSignal } from '@angular/core/rxjs-interop';

export class Component {
  http = inject(HttpClient);
  
  // With initial value
  users = toSignal(
    this.http.get<User[]>('/api/users'),
    { initialValue: [] }
  );
  
  // Without initial value (returns Signal<User[] | undefined>)
  users2 = toSignal(this.http.get<User[]>('/api/users'));
  
  // With requireSync (throws if no sync value)
  count = toSignal(
    of(1),
    { requireSync: true }
  );
}
```

**toObservable()** - Convert Signal to Observable:

```typescript
import { toObservable } from '@angular/core/rxjs-interop';

export class Component {
  searchTerm = signal('');
  
  // Convert to Observable for RxJS operators
  searchResults$ = toObservable(this.searchTerm).pipe(
    debounceTime(300),
    distinctUntilChanged(),
    switchMap(term => this.searchService.search(term))
  );
  
  results = toSignal(this.searchResults$, { initialValue: [] });
}
```

### What are Signal-based Components?

Components using signals throughout:

```typescript
@Component({
  selector: 'app-user-profile',
  standalone: true,
  template: `
    <div>
      <h1>{{ user().name }}</h1>
      <p>{{ user().email }}</p>
      <p>Full name: {{ fullName() }}</p>
      <button (click)="updateName()">Update</button>
    </div>
  `
})
export class UserProfileComponent {
  // Signal input (Angular 17.1+)
  userId = input.required<number>();
  
  // Signal state
  user = signal<User>({ name: 'John', email: 'john@example.com' });
  
  // Computed
  fullName = computed(() => {
    const u = this.user();
    return `${u.name} (${u.email})`;
  });
  
  // Signal output (Angular 17.1+)
  userUpdated = output<User>();
  
  updateName() {
    this.user.update(u => ({ ...u, name: 'Jane' }));
    this.userUpdated.emit(this.user());
  }
}
```

### What are Signal Inputs and Outputs?

**Signal Inputs (Angular 17.1+):**

```typescript
export class ChildComponent {
  // Required input
  name = input.required<string>();
  
  // Optional input with default
  age = input(0);
  
  // With transform
  count = input(0, {
    transform: (value: string | number) => Number(value)
  });
  
  // Computed from input
  greeting = computed(() => `Hello, ${this.name()}!`);
}

// Parent template
<app-child [name]="'John'" [age]="25" />
```

**Signal Outputs (Angular 17.1+):**

```typescript
export class ChildComponent {
  // Output event
  clicked = output<string>();
  
  // Output with alias
  selected = output<number>({ alias: 'itemSelected' });
  
  handleClick() {
    this.clicked.emit('Button clicked');
  }
}

// Parent template
<app-child (clicked)="onClicked($event)" />
```

### What is the Resource API (Angular 19+)?

Resource API simplifies async data loading with signals:

```typescript
import { resource } from '@angular/core';

export class UserComponent {
  userId = signal(1);
  
  // Resource automatically refetches when userId changes
  userResource = resource({
    request: () => ({ id: this.userId() }),
    loader: async ({ request }) => {
      const response = await fetch(`/api/users/${request.id}`);
      return response.json();
    }
  });
  
  // Access in template
  // userResource.value() - the loaded value
  // userResource.isLoading() - loading state
  // userResource.error() - error if any
}
```

**Template:**

```html
@if (userResource.isLoading()) {
  <div>Loading...</div>
} @else if (userResource.error()) {
  <div>Error: {{ userResource.error() }}</div>
} @else {
  <div>{{ userResource.value()?.name }}</div>
}
```

### What are Linked Signals (Angular 19+)?

Linked signals create bidirectional signal connections:

```typescript
import { linkedSignal } from '@angular/core';

export class Component {
  // Source signal
  externalValue = signal(10);
  
  // Linked signal that syncs with source
  internalValue = linkedSignal(() => this.externalValue());
  
  // When externalValue changes, internalValue updates
  // But internalValue can also be set independently
  updateInternal() {
    this.internalValue.set(20); // Doesn't affect externalValue
  }
}
```

**Use case - Form with external data:**

```typescript
export class EditFormComponent {
  // External data
  userData = input.required<User>();
  
  // Form state linked to input
  formData = linkedSignal(() => ({ ...this.userData() }));
  
  // Detect changes
  hasChanges = computed(() => 
    JSON.stringify(this.formData()) !== JSON.stringify(this.userData())
  );
  
  reset() {
    this.formData.set({ ...this.userData() });
  }
}
```

---

## Forms

### What are the types of forms in Angular?

**1. Template-Driven Forms:**

- Uses directives in template
- Automatic with NgModel
- Good for simple forms
- Less control

**2. Reactive Forms:**

- Created in component class
- More control and testing
- Better for complex forms
- Reactive programming

### How do Template-Driven Forms work?

```typescript
import { Component } from '@angular/core';
import { FormsModule } from '@angular/forms';

@Component({
  selector: 'app-user-form',
  standalone: true,
  imports: [FormsModule],
  template: `
    <form #userForm="ngForm" (ngSubmit)="onSubmit(userForm)">
      <div>
        <label>Name:</label>
        <input 
          type="text" 
          name="name" 
          [(ngModel)]="user.name"
          required
          minlength="3"
          #nameInput="ngModel"
        />
        @if (nameInput.invalid && nameInput.touched) {
          <div class="error">
            @if (nameInput.errors?.['required']) {
              <span>Name is required</span>
            }
            @if (nameInput.errors?.['minlength']) {
              <span>Minimum 3 characters</span>
            }
          </div>
        }
      </div>
      
      <div>
        <label>Email:</label>
        <input 
          type="email" 
          name="email" 
          [(ngModel)]="user.email"
          required
          email
        />
      </div>
      
      <button type="submit" [disabled]="userForm.invalid">Submit</button>
    </form>
    
    <pre>{{ user | json }}</pre>
    <pre>Valid: {{ userForm.valid }}</pre>
  `
})
export class UserFormComponent {
  user = {
    name: '',
    email: ''
  };
  
  onSubmit(form: NgForm) {
    if (form.valid) {
      console.log('Submitted:', this.user);
    }
  }
}
```

### How do Reactive Forms work?

```typescript
import { Component } from '@angular/core';
import { 
  FormBuilder, 
  FormGroup, 
  Validators,
  ReactiveFormsModule 
} from '@angular/forms';

@Component({
  selector: 'app-user-form',
  standalone: true,
  imports: [ReactiveFormsModule],
  template: `
    <form [formGroup]="userForm" (ngSubmit)="onSubmit()">
      <div>
        <label>Name:</label>
        <input type="text" formControlName="name" />
        @if (userForm.get('name')?.invalid && userForm.get('name')?.touched) {
          <div class="error">
            @if (userForm.get('name')?.errors?.['required']) {
              <span>Name is required</span>
            }
            @if (userForm.get('name')?.errors?.['minlength']) {
              <span>Min 3 characters</span>
            }
          </div>
        }
      </div>
      
      <div>
        <label>Email:</label>
        <input type="email" formControlName="email" />
        @if (email.invalid && email.touched) {
          <div class="error">{{ email.errors | json }}</div>
        }
      </div>
      
      <div formGroupName="address">
        <label>Street:</label>
        <input formControlName="street" />
        
        <label>City:</label>
        <input formControlName="city" />
      </div>
      
      <button type="submit" [disabled]="userForm.invalid">Submit</button>
    </form>
  `
})
export class UserFormComponent {
  fb = inject(FormBuilder);
  
  userForm = this.fb.group({
    name: ['', [Validators.required, Validators.minLength(3)]],
    email: ['', [Validators.required, Validators.email]],
    address: this.fb.group({
      street: [''],
      city: ['']
    })
  });
  
  // Shorthand access
  get email() {
    return this.userForm.get('email')!;
  }
  
  onSubmit() {
    if (this.userForm.valid) {
      console.log(this.userForm.value);
    }
  }
}
```

### What are FormControl, FormGroup, and FormArray?

**FormControl** - Single form field:

```typescript
import { FormControl, Validators } from '@angular/forms';

const nameControl = new FormControl('initial value', [
  Validators.required,
  Validators.minLength(3)
]);

nameControl.setValue('John');
nameControl.patchValue('Jane');
console.log(nameControl.value); // 'Jane'
console.log(nameControl.valid);
console.log(nameControl.errors);

// Status
nameControl.markAsTouched();
nameControl.markAsDirty();
nameControl.disable();
nameControl.enable();
```

**FormGroup** - Group of controls:

```typescript
const userForm = new FormGroup({
  name: new FormControl('', Validators.required),
  email: new FormControl('', [Validators.required, Validators.email]),
  address: new FormGroup({
    street: new FormControl(''),
    city: new FormControl('')
  })
});

// Set all values
userForm.setValue({
  name: 'John',
  email: 'john@example.com',
  address: { street: '123 Main', city: 'NYC' }
});

// Patch some values
userForm.patchValue({ name: 'Jane' });

// Get values
console.log(userForm.value);
console.log(userForm.get('address.city')?.value);
```

**FormArray** - Dynamic array of controls:

```typescript
import { FormArray } from '@angular/forms';

export class Component {
  fb = inject(FormBuilder);
  
  form = this.fb.group({
    name: [''],
    phones: this.fb.array([
      this.fb.control(''),
    ])
  });
  
  get phones() {
    return this.form.get('phones') as FormArray;
  }
  
  addPhone() {
    this.phones.push(this.fb.control(''));
  }
  
  removePhone(index: number) {
    this.phones.removeAt(index);
  }
}
```

**Template:**

```html
<form [formGroup]="form">
  <input formControlName="name" />
  
  <div formArrayName="phones">
    @for (phone of phones.controls; track $index) {
      <div>
        <input [formControlName]="$index" />
        <button (click)="removePhone($index)">Remove</button>
      </div>
    }
  </div>
  
  <button (click)="addPhone()">Add Phone</button>
</form>
```

### What are Custom Validators?

**Synchronous Validator:**

```typescript
import { AbstractControl, ValidationErrors, ValidatorFn } from '@angular/forms';

// Validator function
export function forbiddenNameValidator(forbidden: RegExp): ValidatorFn {
  return (control: AbstractControl): ValidationErrors | null => {
    const isForbidden = forbidden.test(control.value);
    return isForbidden ? { forbiddenName: { value: control.value } } : null;
  };
}

// Usage
const nameControl = new FormControl('', [
  Validators.required,
  forbiddenNameValidator(/admin/i)
]);

// Check error
if (nameControl.errors?.['forbiddenName']) {
  console.log('Name contains forbidden word');
}
```

**Cross-field Validator:**

```typescript
export const passwordMatchValidator: ValidatorFn = (
  control: AbstractControl
): ValidationErrors | null => {
  const password = control.get('password');
  const confirmPassword = control.get('confirmPassword');
  
  if (!password || !confirmPassword) {
    return null;
  }
  
  return password.value === confirmPassword.value 
    ? null 
    : { passwordMismatch: true };
};

// Usage
const form = this.fb.group({
  password: ['', Validators.required],
  confirmPassword: ['', Validators.required]
}, { validators: passwordMatchValidator });
```

**Async Validator:**

```typescript
import { AsyncValidatorFn } from '@angular/forms';
import { map, catchError } from 'rxjs/operators';
import { of } from 'rxjs';

export function uniqueEmailValidator(
  userService: UserService
): AsyncValidatorFn {
  return (control: AbstractControl): Observable<ValidationErrors | null> => {
    if (!control.value) {
      return of(null);
    }
    
    return userService.checkEmailExists(control.value).pipe(
      map(exists => exists ? { emailTaken: true } : null),
      catchError(() => of(null))
    );
  };
}

// Usage
const emailControl = new FormControl('', 
  [Validators.required, Validators.email],
  [uniqueEmailValidator(this.userService)]
);
```

### What are Typed Forms (Angular 14+)?

Strongly typed forms with automatic type inference:

```typescript
interface UserForm {
  name: string;
  email: string;
  age: number;
  address: {
    street: string;
    city: string;
  };
}

export class Component {
  fb = inject(FormBuilder);
  
  // Typed form
  userForm = this.fb.group<UserForm>({
    name: ['', Validators.required],
    email: ['', [Validators.required, Validators.email]],
    age: [0, [Validators.required, Validators.min(18)]],
    address: this.fb.group({
      street: [''],
      city: ['']
    })
  });
  
  onSubmit() {
    // Type-safe value access
    const value = this.userForm.value; // Partial<UserForm>
    const rawValue = this.userForm.getRawValue(); // UserForm
    
    if (this.userForm.valid) {
      // TypeScript knows the shape
      console.log(value.name); // string | null | undefined
      console.log(rawValue.name); // string
    }
  }
  
  // Type-safe control access
  get nameControl() {
    return this.userForm.controls.name; // FormControl<string>
  }
}
```

### What are Signal-based Forms?

While not officially part of Angular yet, signals can enhance forms:

```typescript
export class SignalFormComponent {
  fb = inject(FormBuilder);
  
  form = this.fb.group({
    username: ['', Validators.required],
    email: ['', [Validators.required, Validators.email]]
  });
  
  // Convert form value to signal
  formValue = toSignal(this.form.valueChanges, { 
    initialValue: this.form.value 
  });
  
  // Convert status to signal
  formValid = toSignal(this.form.statusChanges.pipe(
    map(() => this.form.valid)
  ), { initialValue: this.form.valid });
  
  // Computed validations
  canSubmit = computed(() => this.formValid() && !this.isSubmitting());
  isSubmitting = signal(false);
  
  async onSubmit() {
    if (!this.canSubmit()) return;
    
    this.isSubmitting.set(true);
    try {
      await this.submitForm(this.formValue());
    } finally {
      this.isSubmitting.set(false);
    }
  }
}
```

### What is FormBuilder and why use it?

FormBuilder provides a cleaner syntax for creating forms:

**Without FormBuilder:**

```typescript
form = new FormGroup({
  name: new FormControl('', Validators.required),
  email: new FormControl('', [Validators.required, Validators.email]),
  address: new FormGroup({
    street: new FormControl(''),
    city: new FormControl('')
  })
});
```

**With FormBuilder:**

```typescript
fb = inject(FormBuilder);

form = this.fb.group({
  name: ['', Validators.required],
  email: ['', [Validators.required, Validators.email]],
  address: this.fb.group({
    street: [''],
    city: ['']
  })
});
```

**NonNullableFormBuilder (Angular 14+):**

```typescript
fb = inject(NonNullableFormBuilder);

// Controls default to non-nullable
form = this.fb.group({
  name: [''], // FormControl<string> instead of FormControl<string | null>
  age: [0]    // FormControl<number>
});
```

---

## Docker with Angular

### How do you containerize an Angular application?

**Dockerfile (Multi-stage build):**

```dockerfile
# Stage 1: Build
FROM node:20-alpine AS build

WORKDIR /app

# Copy package files
COPY package*.json ./

# Install dependencies
RUN npm ci

# Copy source code
COPY . .

# Build the app
RUN npm run build -- --configuration production

# Stage 2: Serve with Nginx
FROM nginx:alpine

# Copy built app to Nginx
COPY --from=build /app/dist/your-app-name/browser /usr/share/nginx/html

# Copy custom nginx config (optional)
COPY nginx.conf /etc/nginx/conf.d/default.conf

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]
```

**nginx.conf:**

```nginx
server {
    listen 80;
    server_name localhost;
    root /usr/share/nginx/html;
    index index.html;

    location / {
        try_files $uri $uri/ /index.html;
    }

    # Enable gzip compression
    gzip on;
    gzip_types text/plain text/css application/json application/javascript text/xml application/xml application/xml+rss text/javascript;
}
```

**Build and run:**

```bash
# Build image
docker build -t my-angular-app .

# Run container
docker run -p 8080:80 my-angular-app

# Access at http://localhost:8080
```

### How do you use Docker Compose with Angular?

**docker-compose.yml:**

```yaml
version: '3.8'

services:
  angular-app:
    build:
      context: .
      dockerfile: Dockerfile
    ports:
      - "4200:80"
    environment:
      - NODE_ENV=production
    volumes:
      # Optional: for development with hot reload
      - ./src:/app/src
    networks:
      - app-network

  api:
    image: node:20-alpine
    working_dir: /app
    command: npm start
    ports:
      - "3000:3000"
    volumes:
      - ./api:/app
    environment:
      - DATABASE_URL=postgresql://user:pass@db:5432/mydb
    networks:
      - app-network
    depends_on:
      - db

  db:
    image: postgres:15-alpine
    environment:
      - POSTGRES_USER=user
      - POSTGRES_PASSWORD=pass
      - POSTGRES_DB=mydb
    volumes:
      - postgres-data:/var/lib/postgresql/data
    networks:
      - app-network

networks:
  app-network:
    driver: bridge

volumes:
  postgres-data:
```

**Run:**

```bash
docker-compose up -d
docker-compose down
docker-compose logs -f angular-app
```

### How do you use environment variables in Dockerized Angular?

**Environment replacement approach:**

**Dockerfile:**

```dockerfile
FROM node:20-alpine AS build
WORKDIR /app
COPY package*.json ./
RUN npm ci
COPY . .
RUN npm run build -- --configuration production

FROM nginx:alpine
COPY --from=build /app/dist/app/browser /usr/share/nginx/html
COPY nginx.conf /etc/nginx/conf.d/default.conf

# Add envsubst for environment variable replacement
COPY env.template.js /usr/share/nginx/html/assets/
COPY docker-entrypoint.sh /docker-entrypoint.sh
RUN chmod +x /docker-entrypoint.sh

EXPOSE 80
ENTRYPOINT ["/docker-entrypoint.sh"]
CMD ["nginx", "-g", "daemon off;"]
```

**env.template.js:**

```javascript
window.ENV = {
  API_URL: '${API_URL}',
  ENVIRONMENT: '${ENVIRONMENT}'
};
```

**docker-entrypoint.sh:**

```bash
#!/bin/sh
envsubst < /usr/share/nginx/html/assets/env.template.js > /usr/share/nginx/html/assets/env.js
exec "$@"
```

**Use in Angular:**

```typescript
// environment.service.ts
@Injectable({ providedIn: 'root' })
export class EnvironmentService {
  get apiUrl(): string {
    return (window as any).ENV?.API_URL || 'http://localhost:3000';
  }
  
  get environment(): string {
    return (window as any).ENV?.ENVIRONMENT || 'development';
  }
}
```

**Run with environment variables:**

```bash
docker run -p 8080:80 \
  -e API_URL=https://api.production.com \
  -e ENVIRONMENT=production \
  my-angular-app
```

### What are best practices for Angular Docker images?

1. **Use multi-stage builds** - Smaller final image
2. **Use .dockerignore:**

```
node_modules
dist
.git
.angular
.vscode
*.md
```

3. **Layer caching optimization:**

```dockerfile
# Copy package files first (changes less often)
COPY package*.json ./
RUN npm ci

# Then copy source (changes more often)
COPY . .
```

4. **Security:**

```dockerfile
# Use non-root user
FROM nginx:alpine
RUN addgroup -g 1001 -S appuser && \
    adduser -u 1001 -S appuser -G appuser
USER appuser
```

5. **Health checks:**

```dockerfile
HEALTHCHECK --interval=30s --timeout=3s \
  CMD wget --quiet --tries=1 --spider http://localhost:80 || exit 1
```

6. **Optimize Nginx:**

```nginx
# Enable caching
location ~* \.(js|css|png|jpg|jpeg|gif|ico|svg)$ {
    expires 1y;
    add_header Cache-Control "public, immutable";
}
```

---

## Additional Angular Concepts

### What is Change Detection in Angular?

Change detection is how Angular tracks changes and updates the view:

**Default Change Detection:**

```typescript
@Component({
  selector: 'app-user',
  changeDetection: ChangeDetectionStrategy.Default, // checks entire tree
  template: `<div>{{ user.name }}</div>`
})
export class UserComponent {
  @Input() user!: User;
}
```

**OnPush Change Detection:**

```typescript
@Component({
  selector: 'app-user',
  changeDetection: ChangeDetectionStrategy.OnPush, // only checks when inputs change
  template: `<div>{{ user.name }}</div>`
})
export class UserComponent {
  @Input() user!: User;
  
  // Trigger change detection manually
  cdr = inject(ChangeDetectorRef);
  
  updateManually() {
    this.cdr.markForCheck();
  }
}
```

**With Signals (automatic):**

```typescript
@Component({
  changeDetection: ChangeDetectionStrategy.OnPush,
  template: `<div>{{ count() }}</div>` // automatically updates
})
export class CounterComponent {
  count = signal(0);
  
  increment() {
    this.count.update(v => v + 1); // triggers change detection
  }
}
```

### What are Lifecycle Hooks?

```typescript
export class Component implements OnInit, OnDestroy, AfterViewInit {
  constructor() {
    // Called when component is created
  }
  
  ngOnInit() {
    // Called after component initialization
    // Good for: API calls, setup
  }
  
  ngOnChanges(changes: SimpleChanges) {
    // Called when @Input() properties change
  }
  
  ngDoCheck() {
    // Called during every change detection run
  }
  
  ngAfterContentInit() {
    // Called after content (ng-content) initialized
  }
  
  ngAfterContentChecked() {
    // Called after content checked
  }
  
  ngAfterViewInit() {
    // Called after view initialized
    // Good for: DOM manipulation, ViewChild access
  }
  
  ngAfterViewChecked() {
    // Called after view checked
  }
  
  ngOnDestroy() {
    // Called before component is destroyed
    // Good for: cleanup, unsubscribe
  }
}
```

**With DestroyRef (Angular 16+):**

```typescript
export class Component {
  destroyRef = inject(DestroyRef);
  
  ngOnInit() {
    const subscription = interval(1000).subscribe(console.log);
    
    this.destroyRef.onDestroy(() => {
      subscription.unsubscribe();
    });
  }
}
```

### What is Content Projection (ng-content)?

Pass content from parent to child component:

**Single-slot projection:**

```typescript
// Child component
@Component({
  selector: 'app-card',
  template: `
    <div class="card">
      <ng-content></ng-content>
    </div>
  `
})
export class CardComponent {}

// Parent template
<app-card>
  <h1>Title</h1>
  <p>Content here</p>
</app-card>
```

**Multi-slot projection:**

```typescript
@Component({
  selector: 'app-card',
  template: `
    <div class="card">
      <div class="header">
        <ng-content select="[header]"></ng-content>
      </div>
      <div class="body">
        <ng-content select="[body]"></ng-content>
      </div>
      <div class="footer">
        <ng-content select="[footer]"></ng-content>
      </div>
    </div>
  `
})
export class CardComponent {}

// Usage
<app-card>
  <h1 header>Card Title</h1>
  <p body>Card content</p>
  <button footer>Action</button>
</app-card>
```

### What are ViewChild and ContentChild?

**ViewChild** - Query view elements:

```typescript
@Component({
  selector: 'app-parent',
  template: `
    <input #nameInput type="text" />
    <app-child />
  `
})
export class ParentComponent {
  // Query element reference
  @ViewChild('nameInput') nameInput!: ElementRef;
  
  // Query component
  @ViewChild(ChildComponent) child!: ChildComponent;
  
  ngAfterViewInit() {
    this.nameInput.nativeElement.focus();
    this.child.doSomething();
  }
}
```

**ContentChild** - Query projected content:

```typescript
@Component({
  selector: 'app-wrapper',
  template: `<ng-content></ng-content>`
})
export class WrapperComponent {
  @ContentChild(ChildComponent) child!: ChildComponent;
  
  ngAfterContentInit() {
    this.child.doSomething();
  }
}
```

### What are Pipes?

Transform data in templates:

**Built-in pipes:**

```html
{{ value | uppercase }}
{{ value | lowercase }}
{{ value | titlecase }}
{{ date | date:'short' }}
{{ number | number:'1.2-2' }}
{{ amount | currency:'USD' }}
{{ data | json }}
{{ items | slice:0:5 }}
{{ value | async }}
```

**Custom pipe:**

```typescript
import { Pipe, PipeTransform } from '@angular/core';

@Pipe({
  name: 'exponential',
  standalone: true
})
export class ExponentialPipe implements PipeTransform {
  transform(value: number, exponent = 1): number {
    return Math.pow(value, exponent);
  }
}

// Usage
{{ 2 | exponential:3 }} // 8
```

**Pure vs Impure pipes:**

```typescript
@Pipe({
  name: 'impureFilter',
  standalone: true,
  pure: false // Called on every change detection
})
export class ImpureFilterPipe implements PipeTransform {
  transform(items: any[], filter: string): any[] {
    return items.filter(item => item.includes(filter));
  }
}
```

---

This Q&A covers the latest Angular features with a focus on modern paradigms like signals, standalone components, and the new control flow syntax, along with comprehensive coverage of DI, services, routing, and forms. The Docker section provides practical containerization examples for Angular applications.

---

## Angular Fundamentals

### What is Angular and how does it differ from AngularJS?

Angular is a **TypeScript-based** platform and framework for building single-page applications (SPAs). It provides a comprehensive solution including: a component-based architecture, a powerful template system, dependency injection, routing, forms handling, HTTP client, and a CLI.

**Key differences from AngularJS:**

| Feature                  | AngularJS (1.x)          | Angular (2+)                           |
|--------------------------|--------------------------|----------------------------------------|
| **Language**             | JavaScript               | TypeScript                             |
| **Architecture**         | MVC / Scope-based        | Component-based                        |
| **Binding**              | Two-way (digest cycle)   | One-way by default + explicit two-way  |
| **Change Detection**     | Dirty checking ($digest) | Zone.js + change detection tree        |
| **Mobile Support**       | None                     | Built-in (PWA, responsive)             |
| **Performance**          | Slower (watchers)        | Faster (AOT compilation, tree-shaking) |
| **Dependency Injection** | String-based             | Hierarchical, type-based               |
| **Modules**              | angular.module()         | NgModules / Standalone components      |
| **CLI**                  | None                     | Angular CLI                            |

**Current versions:** Angular follows semantic versioning with major releases every 6 months. Angular 15+ introduced standalone APIs as stable, Angular 16+ introduced Signals, Angular 17+ introduced the new control flow syntax (`@if`, `@for`, `@switch`), and Angular 19 made signals fully stable.

### Explain the concept of Angular modules

Angular modules (**NgModules**) are containers that group related components, directives, pipes, and services into cohesive blocks of functionality. They are decorated with `@NgModule`.

```typescript
@NgModule({
  declarations: [UserComponent, UserListComponent],  // Components, directives, pipes
  imports: [CommonModule, FormsModule, SharedModule], // Other modules
  providers: [UserService],                          // Services (module-level)
  exports: [UserComponent]                           // Public API
})
export class UserModule {}
```

**Key module types:**

- **Root Module (AppModule):** The entry point, bootstraps the app
- **Feature Modules:** Encapsulate features (e.g., `UserModule`, `AdminModule`)
- **Shared Modules:** Reusable components/pipes/directives shared across features
- **Core Module:** Singleton services used app-wide (guards, interceptors)

**NgModules vs Standalone (Angular 15+):**

Since Angular 15, **standalone components** are the recommended approach. They don't need NgModules and manage their own dependencies:

```typescript
@Component({
  selector: 'app-user',
  standalone: true,
  imports: [CommonModule, RouterOutlet, UserCardComponent],
  template: `<app-user-card />`
})
export class UserComponent {}
```

**Bootstrapping without NgModules:**

```typescript
// main.ts
bootstrapApplication(AppComponent, {
  providers: [
    provideRouter(routes),
    provideHttpClient(withInterceptors([authInterceptor])),
  ]
});
```

### How does Angular handle data binding?

Angular provides **four types of data binding:**

**1. Interpolation (one-way, component → template):**

```html
<h1>{{ title }}</h1>
<p>{{ user.name | uppercase }}</p>
```

**2. Property Binding (one-way, component → template):**

```html
<img [src]="imageUrl" />
<button [disabled]="isLoading">Submit</button>
<app-child [user]="currentUser"></app-child>
```

**3. Event Binding (one-way, template → component):**

```html
<button (click)="onSubmit()">Submit</button>
<input (input)="onSearch($event)" />
<app-child (userSelected)="handleSelection($event)"></app-child>
```

**4. Two-Way Binding (bidirectional):**

```html
<input [(ngModel)]="username" />
<!-- Equivalent to: -->
<input [ngModel]="username" (ngModelChange)="username = $event" />
```

**Important:** Angular uses **unidirectional data flow** by default (parent → child). Two-way binding is syntactic sugar and should be used sparingly (mainly for form inputs).

### What is the purpose of Angular CLI?

The **Angular CLI** is a command-line tool that automates the entire development workflow:

```bash
ng new my-app                  # Create new project (standalone by default since v17)
ng generate component user     # Generate component (--standalone flag auto since v17)
ng generate service auth       # Generate service
ng generate pipe format-date   # Generate pipe
ng generate directive highlight # Generate directive
ng generate guard auth         # Generate guard
ng serve                       # Dev server with hot reload (port 4200)
ng build --configuration production  # Production build (AOT + tree-shaking)
ng test                        # Run unit tests (Karma/Jest)
ng e2e                         # Run e2e tests
ng update                      # Update Angular + dependencies
ng add @angular/material       # Add libraries
```

**Key features:**

- **AOT Compilation**: Compiles templates at build time (faster startup)
- **Tree-shaking**: Removes unused code from bundles
- **Code splitting**: Lazy-loaded routes get separate chunks
- **Differential loading**: Generates ES5 + ES2015 bundles
- **esbuild (Angular 17+)**: New build system, 2-4x faster builds


---

## RxJS & Observables

RxJS is fundamental to Angular for handling asynchronous operations.

### Key Operators for Interviews

```typescript
import { map, filter, switchMap, mergeMap, concatMap, exhaustMap,
         debounceTime, distinctUntilChanged, catchError, retry,
         tap, take, takeUntil, shareReplay, combineLatest } from 'rxjs';

// switchMap — Cancels previous inner Observable (HTTP search)
searchTerm$.pipe(
  debounceTime(300),
  distinctUntilChanged(),
  switchMap(term => this.http.get(`/api/search?q=${term}`))
);

// concatMap — Queues inner Observables (order matters)
saveActions$.pipe(
  concatMap(action => this.http.post('/api/save', action))
);

// mergeMap — Runs inner Observables in parallel
ids$.pipe(
  mergeMap(id => this.http.get(`/api/items/${id}`))
);

// exhaustMap — Ignores new emissions while inner is active (prevent double submit)
submitClick$.pipe(
  exhaustMap(() => this.http.post('/api/submit', data))
);

// combineLatest — Combines latest values from multiple Observables
combineLatest([user$, settings$]).pipe(
  map(([user, settings]) => ({ user, settings }))
);

// shareReplay — Multicast + cache last N emissions
getData$ = this.http.get('/api/data').pipe(
  shareReplay(1)  // Cache 1 emission for late subscribers
);
```

#### Subject Types

| Type              | Initial Value  | Replays          | Use Case          |
|-------------------|----------------|------------------|-------------------|
| `Subject`         | No             | No               | Event bus         |
| `BehaviorSubject` | Yes (required) | Last value       | Current state     |
| `ReplaySubject`   | No             | N values         | Cache history     |
| `AsyncSubject`    | No             | Last on complete | Final result only |

---

## State Management

State management is one of the most important Angular topics for mid-to-senior interviews. It covers how data is shared, persisted, and updated across components.

### What is Application State?

**State** is any data that affects what your app renders. There are four kinds:

| Type             | Description                   | Example               |
|------------------|-------------------------------|-----------------------|
| **Local state**  | Belongs to one component      | form input, toggle    |
| **Shared state** | Needed by multiple components | current user, cart    |
| **Server state** | Data fetched from an API      | user list, products   |
| **URL state**    | Reflected in the browser URL  | current page, filters |

### Approaches to State Management in Angular

| Approach                     | Complexity | Best For                   |
|------------------------------|------------|----------------------------|
| Component `@Input`/`@Output` | Low        | Parent-child communication |
| Service + `BehaviorSubject`  | Low-Medium | Small-medium apps          |
| Service + Signals            | Low-Medium | Angular 16+ apps           |
| NgRx (Redux pattern)         | High       | Large, complex apps        |
| NgRx Component Store         | Medium     | Feature-level state        |
| Akita / NGXS                 | Medium     | Alternatives to NgRx       |

---

### Option 1: Service + BehaviorSubject (Classic Pattern)

The most common pattern before Signals. A service holds a `BehaviorSubject` as the single source of truth.

```typescript
// auth.state.service.ts
export interface AuthState {
  user: User | null;
  isLoading: boolean;
  error: string | null;
}

@Injectable({ providedIn: 'root' })
export class AuthStateService {
  // Private BehaviorSubject — only this service can modify state
  private state = new BehaviorSubject<AuthState>({
    user: null,
    isLoading: false,
    error: null
  });

  // Public Observable — components subscribe to this
  readonly state$ = this.state.asObservable();

  // Derived selectors (avoid computing in templates)
  readonly user$ = this.state$.pipe(map(s => s.user));
  readonly isLoggedIn$ = this.user$.pipe(map(u => u !== null));
  readonly isLoading$ = this.state$.pipe(map(s => s.isLoading));

  // Actions — the only way to update state
  login(credentials: LoginCredentials): Observable<User> {
    this.setState({ isLoading: true, error: null });

    return this.authService.login(credentials).pipe(
      tap(user => this.setState({ user, isLoading: false })),
      catchError(err => {
        this.setState({ isLoading: false, error: err.message });
        return throwError(() => err);
      })
    );
  }

  logout() {
    this.setState({ user: null, error: null });
  }

  // Private helper to merge partial state
  private setState(partial: Partial<AuthState>) {
    this.state.next({ ...this.state.getValue(), ...partial });
  }
}
```

```typescript
// Component consuming the state
@Component({
  standalone: true,
  imports: [AsyncPipe, NgIf],
  template: `
    @if (authState.isLoading$ | async) {
      <app-spinner />
    } @else if (authState.isLoggedIn$ | async) {
      <p>Welcome, {{ (authState.user$ | async)?.name }}</p>
      <button (click)="logout()">Logout</button>
    } @else {
      <app-login-form (submit)="login($event)" />
    }
  `
})
export class AppComponent {
  protected authState = inject(AuthStateService);

  login(credentials: LoginCredentials) {
    this.authState.login(credentials).subscribe();
  }

  logout() {
    this.authState.logout();
  }
}
```

---

### Option 2: Service + Signals (Modern Angular Pattern)

The recommended approach for Angular 16+. Signals provide fine-grained reactivity with simpler, synchronous code.

```typescript
// cart.state.service.ts
export interface CartItem {
  productId: number;
  name: string;
  price: number;
  quantity: number;
}

@Injectable({ providedIn: 'root' })
export class CartStateService {
  // Private writable signals
  private readonly _items = signal<CartItem[]>([]);
  private readonly _isOpen = signal(false);

  // Public read-only signals (exposed to components)
  readonly items = this._items.asReadonly();
  readonly isOpen = this._isOpen.asReadonly();

  // Computed state — automatically recalculated when _items changes
  readonly totalItems = computed(() =>
    this._items().reduce((sum, item) => sum + item.quantity, 0)
  );

  readonly totalPrice = computed(() =>
    this._items().reduce((sum, item) => sum + item.price * item.quantity, 0)
  );

  readonly isEmpty = computed(() => this._items().length === 0);

  // Actions
  addItem(product: Product) {
    this._items.update(items => {
      const existing = items.find(i => i.productId === product.id);
      if (existing) {
        return items.map(i =>
          i.productId === product.id
            ? { ...i, quantity: i.quantity + 1 }
            : i
        );
      }
      return [...items, { productId: product.id, name: product.name, price: product.price, quantity: 1 }];
    });
  }

  removeItem(productId: number) {
    this._items.update(items => items.filter(i => i.productId !== productId));
  }

  clearCart() {
    this._items.set([]);
  }

  toggleCart() {
    this._isOpen.update(open => !open);
  }
}
```

```typescript
// Component with Signals — no subscriptions, no async pipe needed
@Component({
  standalone: true,
  template: `
    <button (click)="cart.toggleCart()">
      Cart ({{ cart.totalItems() }})
    </button>

    @if (cart.isOpen()) {
      <div class="cart-panel">
        @for (item of cart.items(); track item.productId) {
          <div class="cart-item">
            <span>{{ item.name }} × {{ item.quantity }}</span>
            <span>{{ item.price * item.quantity | currency }}</span>
            <button (click)="cart.removeItem(item.productId)">Remove</button>
          </div>
        } @empty {
          <p>Your cart is empty</p>
        }
        <strong>Total: {{ cart.totalPrice() | currency }}</strong>
      </div>
    }
  `
})
export class CartComponent {
  protected cart = inject(CartStateService);
}
```

**Benefits of Signals over BehaviorSubject:**

- No subscriptions to manage or unsubscribe
- Synchronous reads — no need for `async` pipe
- Fine-grained updates — only affected components re-render
- Built-in `computed()` for derived state
- Better TypeScript inference

---

### Option 3: NgRx — Redux Pattern for Large Apps

NgRx implements the **Redux pattern** in Angular: a single immutable store, actions describe what happened, reducers compute new state, effects handle side effects.

#### Core Concepts

```
         Component
        /         \
   Dispatch       Select
   (Action)      (Selector)
       |               |
     Store ←-- Reducer ← Action
       |
    Effects (side effects: HTTP, routing, etc.)
```

#### Quick Setup

```bash
ng add @ngrx/store @ngrx/effects @ngrx/entity @ngrx/store-devtools
```

#### 1. Define State and Actions

```typescript
// users/users.actions.ts
import { createActionGroup, emptyProps, props } from '@ngrx/store';

export const UsersActions = createActionGroup({
  source: 'Users',
  events: {
    'Load Users': emptyProps(),
    'Load Users Success': props<{ users: User[] }>(),
    'Load Users Failure': props<{ error: string }>(),
    'Add User': props<{ user: User }>(),
    'Delete User': props<{ userId: number }>(),
  }
});
```

#### 2. Create the Reducer

```typescript
// users/users.reducer.ts
export interface UsersState {
  users: User[];
  isLoading: boolean;
  error: string | null;
}

const initialState: UsersState = {
  users: [],
  isLoading: false,
  error: null
};

export const usersReducer = createReducer(
  initialState,

  on(UsersActions.loadUsers, state => ({
    ...state, isLoading: true, error: null
  })),

  on(UsersActions.loadUsersSuccess, (state, { users }) => ({
    ...state, users, isLoading: false
  })),

  on(UsersActions.loadUsersFailure, (state, { error }) => ({
    ...state, error, isLoading: false
  })),

  on(UsersActions.addUser, (state, { user }) => ({
    ...state, users: [...state.users, user]
  })),

  on(UsersActions.deleteUser, (state, { userId }) => ({
    ...state, users: state.users.filter(u => u.id !== userId)
  }))
);
```

#### 3. Create Selectors

```typescript
// users/users.selectors.ts
export const selectUsersState = createFeatureSelector<UsersState>('users');

export const selectAllUsers = createSelector(selectUsersState, s => s.users);
export const selectIsLoading = createSelector(selectUsersState, s => s.isLoading);
export const selectError = createSelector(selectUsersState, s => s.error);

// Derived selector
export const selectAdminUsers = createSelector(
  selectAllUsers,
  users => users.filter(u => u.role === 'ADMIN')
);
```

#### 4. Create Effects (Side Effects)

```typescript
// users/users.effects.ts
@Injectable()
export class UsersEffects {
  private actions$ = inject(Actions);
  private usersService = inject(UsersService);

  loadUsers$ = createEffect(() =>
    this.actions$.pipe(
      ofType(UsersActions.loadUsers),
      switchMap(() =>
        this.usersService.getUsers().pipe(
          map(users => UsersActions.loadUsersSuccess({ users })),
          catchError(err => of(UsersActions.loadUsersFailure({ error: err.message })))
        )
      )
    )
  );
}
```

#### 5. Use in Component

```typescript
@Component({
  standalone: true,
  imports: [AsyncPipe, NgFor],
  template: `
    @if (isLoading$ | async) {
      <app-spinner />
    }
    @for (user of users$ | async; track user.id) {
      <app-user-card [user]="user" (delete)="deleteUser(user.id)" />
    }
  `
})
export class UsersPageComponent implements OnInit {
  private store = inject(Store);

  users$ = this.store.select(selectAllUsers);
  isLoading$ = this.store.select(selectIsLoading);

  ngOnInit() {
    this.store.dispatch(UsersActions.loadUsers());
  }

  deleteUser(userId: number) {
    this.store.dispatch(UsersActions.deleteUser({ userId }));
  }
}
```

#### 6. Register in App Config

```typescript
// app.config.ts
export const appConfig: ApplicationConfig = {
  providers: [
    provideStore({ users: usersReducer }),
    provideEffects([UsersEffects]),
    provideStoreDevtools({ maxAge: 25, logOnly: !isDevMode() })
  ]
};
```

---

### NgRx Component Store (Feature-Level State)

For state scoped to a single feature or component tree — simpler than full NgRx.

```typescript
export interface ProductsState {
  products: Product[];
  filter: string;
  isLoading: boolean;
}

@Injectable()
export class ProductsStore extends ComponentStore<ProductsState> {
  constructor(private productsService: ProductsService) {
    super({ products: [], filter: '', isLoading: false });
  }

  // Selectors
  readonly products$ = this.select(s => s.products);
  readonly filter$ = this.select(s => s.filter);
  readonly filteredProducts$ = this.select(
    this.products$,
    this.filter$,
    (products, filter) =>
      products.filter(p => p.name.toLowerCase().includes(filter.toLowerCase()))
  );

  // Updaters (synchronous state updates)
  readonly setFilter = this.updater((state, filter: string) => ({
    ...state, filter
  }));

  // Effects (async operations)
  readonly loadProducts = this.effect((trigger$: Observable<void>) =>
    trigger$.pipe(
      tap(() => this.patchState({ isLoading: true })),
      switchMap(() =>
        this.productsService.getAll().pipe(
          tapResponse(
            products => this.patchState({ products, isLoading: false }),
            () => this.patchState({ isLoading: false })
          )
        )
      )
    )
  );
}

// Provide at component level (not root!)
@Component({
  providers: [ProductsStore],
  template: `...`
})
export class ProductsPageComponent implements OnInit {
  store = inject(ProductsStore);

  ngOnInit() {
    this.store.loadProducts();
  }
}
```

---

### When to Use What?

| Scenario                                  | Recommended Approach      |
|-------------------------------------------|---------------------------|
| Simple parent-child communication         | `@Input` / `@Output`      |
| Small app (< 5 features)                  | Service + Signals         |
| Medium app, team familiar with RxJS       | Service + BehaviorSubject |
| Large app, many developers, complex flows | NgRx                      |
| Feature-level state isolation             | NgRx Component Store      |
| Need time-travel debugging                | NgRx + DevTools           |

**Interview tip:** Being able to explain the trade-offs between these approaches is more impressive than just knowing one. A common question is: *"How would you manage state in an Angular app?"* — mention the options, explain the trade-offs, and say you'd choose based on team size and complexity.

---

### State Management Best Practices

```typescript
// ✅ 1. Single source of truth — state lives in one place
// ❌ Don't keep copies of the same data in multiple services

// ✅ 2. Immutable updates — never mutate state directly
this._items.update(items => [...items, newItem]);  // ✅ new array
items.push(newItem);  // ❌ mutating in place

// ✅ 3. Derived state via computed/selectors — don't store computed values
readonly totalPrice = computed(() => this._items().reduce(...));  // ✅
this.totalPrice = this.items.reduce(...);  // ❌ stored, can get out of sync

// ✅ 4. Keep UI state separate from domain state
// UI state: isLoading, selectedTab, isModalOpen
// Domain state: users, products, orders

// ✅ 5. Effects/side effects stay outside reducers
// Reducers must be pure functions — no HTTP calls, no console.log

// ✅ 6. Use OnPush change detection with immutable state
@Component({
  changeDetection: ChangeDetectionStrategy.OnPush,
  ...
})
```
