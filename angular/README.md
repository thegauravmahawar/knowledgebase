# Angular

Angular is a web framework that empowers developers to build fast, reliable applications. Angular provides a broad suite of tools, APIs, and libraries to simplify and streamline your development workflow.

## Installation

**Prerequisites**

- Node.js - v.18 or newer

**Install Angular CLI**

```shell
npm install -g @angular/cli
```

**Create a new project**

```shell
ng new <project-name>
```

**Running the project locally**

```shell
cd angular-app
npm start
```

## Essentials

### Components

Components are the main building blocks of Angular applications. Each component represents a part of a larger web page. Organizing an application into components helps provide structure to your project, clearly separating code into specific parts that are easy to maintain and grow over time.

**Defining a component**

Every component has the following core properties:

- A `@Component` decorator that contains some configuration used by Angular.
- An HTML template that controls what renders into the DOM.
- A CSS selector that defines how the component is used in HTML.
- A TypeScript class with behaviors, such as handling user input or making requests to a server.

```typescript
@Component({
  selector: "user-profile",
  template: `
    <h1>User profile</h1>
    <p>This is the user profile page</p>
  `,
})
export class UserProfile {}
```

The `@Component` decorator also optionally accepts a styles property for any CSS you want to apply to your template:

```typescript
// user-profile.ts
@Component({
  selector: "user-profile",
  template: `
    <h1>User profile</h1>
    </p>This is the user profile page</p>
  `,
  styles: `
    h1 {
      font-size: 3em;
    }
  `,
})
export class UserProfile {
  /* Your component code goes here */
}
```

**Separating HTML and CSS into separate files**

You can define a component's HTML and CSS in separate files using `templateUrl` and `styleUrl`:

```typescript
// user-profile.ts
@Component({
  selector: "user-profile",
  templateUrl: "user-profile.html",
  styleUrl: "user-profile.css",
})
export class UserProfile {}
```

```html
<!-- user-profile.html -->
<h1>Use profile</h1>
<p>This is the user profile page</p>
```

```css
/* user-profile.css */
h1 {
  font-size: 3em;
}
```

**Using components**

You build an application by composing multiple components together. For example, if you are building a user profile page, you might break the page up into several components like this:

![Angular Component Example](../assets/angular_component_example.png)

Here, the `UserProfile` component uses several other components to produce the final page.

To import and use a component, you need to:

1. In your component's TypeScript file, add an import statement for the component you want to use.
2. In your @Component decorator, add an entry to the imports array for the component you want to use.
3. In your component's template, add an element that matches the selector of the component you want to use.

Here's an example of a `UserProfile` component importing a `ProfilePhoto` component:

```typescript
// user-profile.ts
import { ProfilePhoto } from "profile-photo.ts";
@Component({
  selector: "user-profile",
  imports: [ProfilePhoto],
  template: `
    <h1>User profile</h1>
    <profile-photo />
    <p>This is the user profile page</p>
  `,
})
export class UserProfile {
  // Component behavior is defined in here
}
```

### Signals

In Angular, you use _signals_ to create and manage state. A signal is a lightweight wrapper around a value.

Use the `signal` function to create a signal for holding local state:

```typescript
import { signal } from "@angular/core";
// Create a signal with the `signal` function.
const firstName = signal("Morgan");
// Read a signal value by calling it— signals are functions.
console.log(firstName());
// Change the value of this signal by calling its `set` method with a new value.
firstName.set("Jaime");
// You can also use the `update` method to change the value
// based on the previous value.
firstName.update((name) => name.toUpperCase());
```

Angular tracks where signals are read and when they're updated. The framework uses this information to do additional work, such as updating the DOM with new state. This ability to respond to changing signal values over time is known as reactivity.

**Computed expressions**

A `computed` is a signal that produces its value based on other signals.

```typescript
import { signal, computed } from "@angular/core";
const firstName = signal("Morgan");
const firstNameCapitalized = computed(() => firstName().toUpperCase());
console.log(firstNameCapitalized()); // MORGAN
```

A `computed` signal is read-only; it does not have a `set` or an `update` method. Instead, the value of the `computed` signal automatically changes when any of the signals it reads change:

```typescript
import { signal, computed } from "@angular/core";
const firstName = signal("Morgan");
const firstNameCapitalized = computed(() => firstName().toUpperCase());
console.log(firstNameCapitalized()); // MORGAN
firstName.set("Jaime");
console.log(firstNameCapitalized()); // JAIME
```

**Using signals in components**

Use `signal` and `computed` inside your components to create and manage state:

```typescript
@Component({
  /* ... */
})
export class UserProfile {
  isTrial = signal(false);
  isTrialExpired = signal(false);
  showTrialDuration = computed(() => this.isTrial() && !this.isTrialExpired());

  activateTrial() {
    this.isTrial.set(true);
  }
}
```

### Dyanmic interfaces with templates

When you need to display dynamic content in your template, Angular uses the double curly brace syntax in order to distinguish between static and dynamic content.

```typescript
@Component({
  selector: "todo-list-item",
  template: ` <p>Title: {{ taskTitle }}</p> `,
})
export class TodoListItem {
  taskTitle = "Read cup of coffee";
}
```

When Angular renders the component you'll see the output:

```html
<p>Title: Read cup of coffee</p>
```

This syntax declares an interpolation between the dynamic data property inside of the HTML. As a result, whenever the data changes, Angular will automatically update the DOM reflecting the new value of the property.

**Dynamic Properties**

When you need to dynamically set the value of standard DOM properties on an HTML element, the property is wrapped in square brackets to inform Angular that the declared value should be interpreted as a JavaScript-like statement instead of a plain string.

For example, a common example of dynamically updating properties in your HTML is determining whether the form submit button should be disabled based on whether the form is valid or not.

```typescript
@Component({
  selector: "sign-up-form",
  template: `
    <button type="submit" [disabled]="formIsInvalid">Submit</button>
  `,
})
export class SignUpForm {
  formIsInvalid = true;
}
```

In this example, because `formIsInvalid` is true, the rendered HTML would be:

```html
<button type="submit" disabled>Submit</button>
```

### Conditionals & Loops

**Conditional Rendering**

`@if` block

```typescript
@Component({
  standalone: true,
  selector: "user-controls",
  template: `
    @if (isAdmin) {
    <button>Erase database</button>
    }
  `,
})
export class UserControls {
  isAdmin = true;
}
```

`@else` block

```typescript
@Component({
  standalone: true,
  selector: "user-controls",
  template: `
    @if (isAdmin) {
    <button>Erase database</button>
    } @else {
    <p>You are not authorized.</p>
    }
  `,
})
export class UserControls {
  isAdmin = true;
}
```

**Rendering a list**

`@for` block

```html
<ul>
  @for (ingredient of ingredientList; track ingredient.name) {
  <li>{{ ingredient.quantity }} - {{ ingredient.name }}</li>
  }
</ul>
```

```typescript
@Component({
  standalone: true,
  selector: "ingredient-list",
  templateUrl: "./ingredient-list.component.html",
})
export class IngredientList {
  ingredientList = [
    { name: "noodles", quantity: 1 },
    { name: "miso broth", quantity: 1 },
    { name: "egg", quantity: 2 },
  ];
}
```

`track` property

When Angular renders a list of elements with `@for`, those items can later change or move. Angular needs to track each element through any reordering, usually by treating a property of the item as a unique identifier or key.

This ensures any updates to the list are reflected correctly in the UI and tracked properly within Angular, especially in the case of stateful elements or animations.
