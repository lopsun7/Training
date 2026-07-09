# Homework 23 - Angular Employee UI and Front-End Basics

## Code Implementation

Project repository:

- Spring Boot + Angular project: `git@github.com:lopsun7/student-management-system.git`

What I implemented:

- Added an Angular UI under `employee-angular-ui`.
- Added username/password login through the existing Spring Security JWT endpoint.
- Added a demo Gmail login endpoint: `POST /api/v1/auth/gmail-demo`.
- Added employee CRUD endpoints: `GET/POST/PUT/DELETE /api/v1/employees`.
- Added CORS support for Angular dev server `http://localhost:4200`.
- Angular stores the JWT in `localStorage` and sends it as a Bearer token for employee CRUD.
- The employee API maps UI field `department` to the existing backend field `course`, so the homework can be completed without breaking prior student-management assignments.

Run backend:

```bash
cd "/Users/lopsun/Documents/New project 4"
SPRING_PROFILES_ACTIVE=h2 ./mvnw spring-boot:run
```

Run Angular UI:

```bash
cd "/Users/lopsun/Documents/New project 4/employee-angular-ui"
pnpm install
pnpm start
```

Open:

```text
http://localhost:4200
```

Demo login:

```text
username: steven
password: password123
```

Gmail demo login:

```text
steven.demo@gmail.com
```

Validation commands:

```bash
./mvnw test
cd employee-angular-ui
pnpm build
```

Verified results:

```text
Backend tests: 55 passed
JaCoCo instruction coverage: 93.69%
Angular production build: successful
```

## Demo Script

First, I start the Spring Boot backend with the H2 profile. Then I start the Angular application on port 4200. On the login page, I can either use the demo username and password or use a Gmail demo address. After login, Angular stores the JWT token and calls protected employee APIs with the Bearer token.

In the employee dashboard, I can create an employee, refresh the list, edit an existing employee, and delete an employee. The UI calls my Spring Boot employee endpoints, and Spring Security protects those endpoints.

## Question List

### 1. What is HTML and what is its main responsibility in a web page?

HTML stands for HyperText Markup Language. Its main responsibility is to define the structure and meaning of a web page, such as headings, paragraphs, links, images, forms, tables, and buttons.

HTML tells the browser what content exists and how the content is organized. It does not handle styling deeply and it does not handle business logic.

### 2. What is an HTML file essentially made of?

An HTML file is essentially made of elements. Each element is usually built from an opening tag, content, and a closing tag.

Example:

```html
<h1>Employee Dashboard</h1>
```

It can also include attributes, comments, text, metadata, scripts, and links to CSS files.

### 3. What does `<!DOCTYPE html>` do and why is it required?

`<!DOCTYPE html>` tells the browser that the document should be interpreted as modern HTML5. It helps the browser use standards mode instead of older compatibility mode.

It is required because without it, browsers may render the page inconsistently.

### 4. What is an HTML tag and how does it work?

An HTML tag is a marker that tells the browser what kind of element it is creating. For example, `<p>` creates a paragraph and `<button>` creates a button.

Most tags come in pairs:

```html
<p>Hello</p>
```

Some tags are self-contained, such as:

```html
<img src="logo.png" alt="Logo">
```

### 5. What is the difference between the `<head>` and `<body>` sections?

The `<head>` contains metadata and resources for the page, such as the title, CSS links, character set, viewport setting, and SEO metadata.

The `<body>` contains the visible content users interact with, such as text, buttons, images, forms, and application UI.

### 6. What is an HTML attribute and why is it needed?

An HTML attribute provides extra information about an element. It is written inside the opening tag.

Example:

```html
<input type="email" placeholder="name@gmail.com">
```

Attributes are needed to configure behavior, accessibility, styling hooks, links, form input types, and data.

### 7. How does the browser parse HTML and convert it into the DOM?

The browser reads HTML from top to bottom, tokenizes the markup, and builds a tree structure called the DOM, which stands for Document Object Model.

Each HTML element becomes a DOM node. JavaScript and CSS can then interact with those DOM nodes to update content, apply styles, and respond to user actions.

### 8. What is CSS and what problem does it solve?

CSS stands for Cascading Style Sheets. It controls how HTML looks: colors, fonts, spacing, layout, borders, animations, and responsive behavior.

CSS solves the problem of separating page structure from visual presentation. HTML defines what the content is, while CSS defines how it should look.

### 9. Why is CSS not considered a programming language?

CSS is not usually considered a programming language because it does not contain normal programming control flow like loops, conditions, functions, and data structures in the same way Java or JavaScript do.

CSS is a style sheet language. It declares rules that the browser applies to matching elements.

### 10. What are the three ways to apply CSS to an HTML page?

The three ways are inline CSS, internal CSS, and external CSS.

Inline CSS:

```html
<p style="color: red;">Hello</p>
```

Internal CSS:

```html
<style>
  p {
    color: red;
  }
</style>
```

External CSS:

```html
<link rel="stylesheet" href="styles.css">
```

### 11. Why are inline styles generally not recommended?

Inline styles are not recommended because they mix structure and styling in the same place. They are harder to reuse, harder to maintain, and harder to override.

In real projects, external CSS or component styles are cleaner because the style rules are organized in one place.

### 12. What is the difference between id selectors and class selectors in CSS?

An id selector targets one unique element:

```css
#loginForm {
  display: grid;
}
```

A class selector targets one or many elements:

```css
.button {
  border-radius: 8px;
}
```

In practice, classes are used more often for styling because they are reusable. IDs are unique and often used for anchors or specific DOM references.

### 13. What is the difference between margin and padding?

Padding is the space inside an element, between the content and the border.

Margin is the space outside an element, between that element and other elements.

Example:

```css
.card {
  padding: 16px;
  margin: 24px;
}
```

### 14. What is the difference between `position: relative` and `position: absolute`?

`position: relative` keeps the element in the normal document flow, but allows it to be moved relative to its original position.

`position: absolute` removes the element from the normal document flow and positions it relative to the nearest positioned ancestor.

Relative positioning is often used as an anchor for absolute children.

### 15. What is JavaScript used for in a web application?

JavaScript is used to make web pages interactive and dynamic. It can handle clicks, submit forms, validate inputs, call backend APIs, update the DOM, manage state, and build full single-page applications like Angular apps.

In my homework, Angular uses TypeScript, which compiles to JavaScript, to handle login, store JWT tokens, and perform employee CRUD operations.

### 16. What are the two ways to include JavaScript in an HTML file?

JavaScript can be written inline inside a `<script>` tag:

```html
<script>
  console.log('Hello');
</script>
```

Or loaded from an external file:

```html
<script src="app.js"></script>
```

In Angular, the build system bundles the application JavaScript and injects it into the page.

### 17. What is a variable in JavaScript and what types of values can it store?

A variable is a named container for a value. In modern JavaScript, we usually declare variables with `let` or `const`.

It can store strings, numbers, booleans, arrays, objects, functions, `null`, `undefined`, symbols, and big integers.

Example:

```javascript
const employeeName = 'Steven';
let count = 3;
```

### 18. How does JavaScript interact with HTML elements through the DOM?

JavaScript can select DOM elements, read their values, change their text, update styles, add or remove classes, create elements, and attach event listeners.

Example:

```javascript
document.querySelector('button').addEventListener('click', () => {
  document.querySelector('h1').textContent = 'Clicked';
});
```

In Angular, we usually do not manually call `document.querySelector`. Angular binds data to the template and updates the DOM for us.

### 19. What are loops used for in JavaScript, and can you give an example use case?

Loops repeat work. They are useful when processing arrays, rendering lists, validating multiple records, or calculating totals.

Example:

```javascript
for (const employee of employees) {
  console.log(employee.email);
}
```

In Angular templates, I used `@for` to render the employee table.

### 20. What is an event listener and why is it important in front-end development?

An event listener is code that waits for a user or browser event, such as click, input, submit, keydown, or page load.

It is important because user interaction is event-driven. When a user clicks a login button or submits an employee form, event listeners let the application respond.

Example:

```javascript
button.addEventListener('click', login);
```

In Angular, event binding looks like this:

```html
<button (click)="logout()">Logout</button>
```
