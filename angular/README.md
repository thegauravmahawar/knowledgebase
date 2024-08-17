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

**Components**

Components provide structure for organizing your project into easy-to-understand parts with clear responsibilities so that your code is maintainable and scalable.

**Defining a component**

Every component has the following core properties:

- A `@Component` decorator that contains some configuration
- An HTML template that controls what renders into the DOM
- A CSS selector that defines how the component is used in HTML
- A TypeScript class with behaviors such as managing state, handling user input, or fetching data from a server.

```typescript
@Component({
  selector: "todo-list-item",
  template: ` <li>(TODO) Read Angular Essentials Guide</li> `,
})
export class TodoListItem {}
```

Other common metadata that you'll also see in components include:

- `standalone: true` - The recommended approach of streamlining the authoring experience of components
- `styles` - A string or array of strings that contains any CSS styles you want applied to the component

```typescript
// todo-list-item.component.ts
@Component({
  standalone: true,
  selector: "todo-list-item",
  template: ` <li>(TODO) Read Angular Essentials Guide</li> `,
  styles: `
    li {
      color: red;
      font-weight: 300;
    }
  `,
})
export class TodoListItem {
  /* Component behavior is defined in here */
}
```

**Using a component**

One advantage of component architecture is that your application is modular. In other words, components can be used in other components.

To use a component, you need to:

- Import the component into the file
- Add it to the component's `imports` array
- Use the component's selector in the `template`

```typescript
// todo-list.component.ts
import { TodoListItem } from "./todo-list-item.component.ts";
@Component({
  standalone: true,
  imports: [TodoListItem],
  template: `
    <ul>
      <todo-list-item></todo-list-item>
    </ul>
  `,
})
export class TodoList {}
```
