# Tour  Of Heroes

Following the [Tour of heroes - angular.io tutorial](https://angular.io/tutorial)

## Checklist

- [x] Introduction
- [x] Create a project
- [x] 1 . The hero editor
- [x] 2 . Display a list
- [x] 3 . Create a feature component
- [x] 4 . Add services
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

### 1. The hero editor

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

### 2. Display a list

The `*ngFor` is Angular's **repeater** directive. It repeats the host element for each element in a collection. Host element is one it is placed on.

```html
<li *ngFor="let hero of heroes">
    <button type="button" (click)="onSelect(hero)" [class.selected]="hero === selectedHero">
      <span class="badge">{{hero.id}}</span>
      <span class="name">{{hero.name}}</span>
    </button>
  </li>
```

-------

Event binding is used when we want to bind html event to function in our component.ts file. For example a button click event binding would look like this:

```html
<button type="button" (click)="onSelect(hero)">
```

-------

You add or remove css style classes dynamically with Angular's class binding for example:

```html
<button [class.selected]="hero === selectedHero" type="button" (click)="onSelect(hero)">
```

### 3. Create a feature component

It is better to divide our application into subcomponents each focused on a specific task or workflow

-----

The `Input` symbol from `@angular/core` library, can be used to add an input property to a subcomponent like so:

```typescript
@Input() hero?: Hero;
```

When using the component in another component you can add a  **property binding** which is one way like so:

```html
<app-hero-detail [hero]="selectedHero"></app-hero-detail>
```

This binds the input property 'hero' to the parent property 'selectedHero'

### 4. Add services

Services are created by default with `Injectable` symbol, which allows them to be injected into components/other services automatically using Angular's dependency injection.

```typescript
@Injectable({
  providedIn: 'root',
})
export class HeroService {

  constructor() { }

}
```

`providedIn: 'root'` means it will be a singleton provided to every component which injects it.

------

To inject a service into a component you put it in its constructor like so:

```typescript
constructor(private heroService: HeroService) {}
```

------

Load any data a component needs in it `ngOnInit()` function. The component has to implement `OnInit` interface and implement its method.

```typescript
import { Component, OnInit } from '@angular/core';
	...
    
@Component({
  selector: 'app-heroes',
  templateUrl: './heroes.component.html',
  styleUrls: ['./heroes.component.css']
})
export class HeroesComponent implements OnInit {
	...
  ngOnInit(): void {
      this.getHeroes();
  }
  	...
}
```

-----

When you load data from a service you should always load it asynchronously as to not block the browser while data is fetched and loaded.

To make this possible services should return `Observable<T>` and when you load data you should use `subscribe()` method.

```typ
getHeroes(): Observable<Hero[]> {
  const heroes = of(HEROES);
  return heroes;
}
```

```typ
getHeroes(): void {
  this.heroService.getHeroes()
      .subscribe(heroes => this.heroes = heroes);
}
```

