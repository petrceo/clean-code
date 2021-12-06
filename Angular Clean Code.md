# Angular Clean Code

## Table of Contents

  1. [Coding style guide](#coding-style-guide)
  2. [Using Immutability](#using-immutability)
  3. [Prevent Memory Leaks in Angular Observable](#prevent-memory-leaks-in-angular-observable)
  4. [Using index file](#using-index-file)
  5. [Change Detection Optimisations](#change-detection-optimisations)
  6. [Using trackBy in NgFor](#using-trackby-in-ngfor)
  7. [Using Smart - Dumb components](#using-smart---dumb-components)
  8. [Module Organisation and Lazy Loading](#module-organisation-and-lazy-loading)

## Coding style guide

Base recommendations from official style guide: https://angular.io/guide/styleguide

**[⬆ back to top](#table-of-contents)**

## Using Immutability

Objects and arrays are the reference types in javascript. If we want to copy them into another object or an array and to modify them, the best practice is to do that in an immutable way using es6 spread operator(…)

```ts
const article = {
  name: 'test',
  active: false
};

const updatedArticle = {
  ...article,
  active: true
};
```

This way, we are deep copying the user object and then just overriding the status property. Now, the original article is going to be preserved with all of its values.

**[⬆ back to top](#table-of-contents)**

## Prevent Memory Leaks in Angular Observable

Observable memory leaks are very common and found in every programming language, library, or framework. Angular is no exception to that. Observables in Angular are very useful as it streamlines your data, but memory leak is one of the very serious issues that might occur if you are not focused. It can create the worst situation in mid of development. Here’re some of the tips which follow to avoid leaks.

- Using async pipe
- Using take(1)
- Using takeUntil()

**[⬆ back to top](#table-of-contents)**

## Using index file

index.ts helps us to keep all related things together so that we don’t have to be bothered about the source file name. This helps reduce the size of the import statement.

For example, we have core/services/index.ts as

```ts
export * from './api.service';
export * from './token.service';
```

We can import all things by using the source folder name.

```ts
import { ApiService, TokenService } from '@app/core/services';
```

**[⬆ back to top](#table-of-contents)**

## Change Detection Optimisations

- Use NgIf and not CSS - If DOM elements aren’t visible, instead of hiding them with CSS, it's a good practice to remove them from the DOM by using *ngIf.
- Move complex calculations into the ngDoCheck lifecycle hook to make your expressions faster.
- Cache complex calculations as long as possible
- Use the OnPush change detection strategy to tell Angular there have been no changes. This lets you skip the entire change detection step.

**[⬆ back to top](#table-of-contents)**

## Using trackBy in NgFor

When using ngFor to loop over an array in templates, use it with a trackBy function which will return a unique identifier for each DOM item.

When an array changes, Angular re-renders the whole DOM tree. But when you use trackBy, Angular will know which element has changed and will only make DOM changes only for that element.

```html
<ul>
  <li *ngFor="let item of items; trackBy: trackByFn">{{item.id}}</li>
</ul>
```

**[⬆ back to top](#table-of-contents)**

## Using Smart - Dumb components

This pattern helps to use OnPush change detection strategy to tell Angular there have been no changes in the dumb components.

Smart components are used in manipulating data, calling the APIs, focussing more on functionalities, and managing states. While dumb components are all about cosmetics, they focus more on how they look.

**[⬆ back to top](#table-of-contents)**

## Module Organisation and Lazy Loading

Utilizing lazy load the modules can enhance productivity. Lazy Load is a built-in feature in Angular which helps us with loading the things on demand. When have used LazyLoad it helps in reducing the size of the application by abstaining from unnecessary file from loading.

**[⬆ back to top](#table-of-contents)**
