# Tour  Of Heroes

Following the [Tour of heroes - angular.io tutorial](https://angular.io/tutorial)

## Checklist

- [x] Introduction
- [x] Create a project
- [x] 1 . The hero editor
- [ ] 2 . Diplay a list
- [ ] 3 . Create a feature component
- [ ] 4 . Add services
- [ ] 5 . Add navigation
- [ ] 6 . Get data from a server

## Useful commands

Create new angular project

```bash
ng new tour-of-heroes
```

Serve the application locally

```bash
ng serve --open
```

Generate new component

```bash
ng generate component heroes
```

## Learning Notes

Project's application wide style is defined in src/style.css.

------

In the component the selector tag defines what name we use when using it with its own tag in parent html.

```typescript
@Component({
  selector: 'app-heroes',
  templateUrl: './heroes.component.html',
  styleUrls: ['./heroes.component.css']
})
```

```html
<app-heroes></app-heroes>
```

------

You can format data directly in the html using pipes, for example one of these pipes which are available in angular is `UppercasePipe` .

```html
<h2>{{hero.name | uppercase}} Details</h2>
```

------

Interfaces are used as data classes, and they sit in their own .ts file

```typescript
export interface Hero {
  id: number;
  name: string;
}
```

------

To use a component's property defined like so:

```typescript
import { Component, OnInit } from '@angular/core';
import { Hero } from '../hero';

@Component({
  selector: 'app-heroes',
  templateUrl: './heroes.component.html',
  styleUrls: ['./heroes.component.css']
})
export class HeroesComponent implements OnInit {
  hero: Hero = {
    id: 1,
    name: 'Windstorm'
  };

  constructor() { }

  ngOnInit(): void {
  }

}
```

Inside the html, you can access it with double curly braces likes so:

```html
<h2>{{hero.name}} Details</h2>
<div><span>id: </span>{{hero.id}}</div>
<div><span>name: </span>{{hero.name}}</div>
```

------

For your input fields angular can do two-way binding using `FormsModule`.

```html
<div>
  <label for="name">Hero name: </label>
  <input id="name" [(ngModel)]="hero.name" placeholder="name">
</div>
```

But you have to import it in your module by adding to app.module.ts the import and adding it to its imports like so:

```typescript
import { FormsModule } from '@angular/forms'; // <-- NgModel lives here
...
imports: [
  BrowserModule,
  FormsModule
],
```

