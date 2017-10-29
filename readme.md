# Up and Running with Angular 4

## Prerequisites
- NPM scripts
- Typescript 
- HTML/CSS

## Getting up and Running with Angular 
### Set up the Development Environment: 
Please refer to the official angular startup guide to start with angular 4:
[link](https://angular.io/guide/quickstart) if you are lost. 

Refer to my guide to create an angular project without any permission issues. 

Install the angular-cli globally 
```
sudo npm install -g @angular/cli
```
Create a new project and skill installation 
```
ng new my-app --skip-install
```
Configure appropriate permission Serve the application
```
sudo chown <username> -R my-app
cd my-app
mkdir node_modules
sudo chown <username> node_modules
chmod a+w node_modules
npm install
ng serve --open
```

### How components work 
An Angular component allows you to define both the logic and the views for your app. A component is structured in three different sections. First we have the imports, which reside at the top of the file. These imports allow you to use a variety of code that your component may require. The component decorator which consists of metadata; this lets Angular know how to process our class. And then we have the component class. This is where you define the logic that's specific to the given component.

### Generating components from the cli
```
ng g component component_name 
```
## Templating Basics
### Interpolation                                                                                                                                     
In an angular template, we use what's called Interpolation to communicate data that's defined within the component class as well as template variables. Interpolation uses double curly braces that are wrapped around a template expression.

Interpolation syntax
```javascript
{{Template Expression}}
```

Inside template: 
```javascript
{{component_property}} 
```

### NgFor
Sometimes you need to loop through an iterable, which is a collection of data like an array, or an array of objects. In these cases, Angular provides you with the ngFor directive. ngFor works by allowing you to repeat a template with each item of an iterable. The basic syntax is to declare an ngFor attribute and inside of the quotes bind each iterable to a local template variable.

ngFor Syntax: 
```html
<div *ngFor="let item of items">{{item}}</div>
```
app.component.html:
```html
<ul>
  <li *ngFor="let team of teams; let i = index; let l = last; let e = even; let o = odd">
    {{i}}: {{ team.baseball }} 
    <strong *ngIf="l">Hey, I'm last!</strong>
    <strong *ngIf="e">Hey, I'm Even!</strong>
    <strong *ngIf="o">Hey, I'm Odd!</strong>
  </li>
</ul>
```
app.component.ts: 
```javascript
export class AppComponent {
  teams = [
    { baseball: 'Indians'},
    { baseball: 'Mets'},
    { baseball: 'Yankees'},
  ]
}
```

### ngIf, else and then 
If, else, and then are all defined within the ngIf directive. We define ngIf inside of an HTML element and inside of the quotes we bind it to a template expression. When we add else, we're able to define a local template variable and, when we add them, we can define a local template variable based on the template expression resulting in true or false.

ngIf, else and then Syntax:
```html
<div *ngIf="property">Content</div>
<div *ngIf="property; else templVar">Content</div>
<div *ngIf="property; then tp1 else tp2">Content</div>  
```

app.component.html:
```html
<h1 *ngIf="title">I'm here!</h1> <!-- as you can see, the h1 element remains because the title property is present in our component class. -->

<h1 *ngIf="!title; else mytemplate">I'm here!</h1> 
<!-- we add a semicolon to else and then the name of our local template variable and we'll make this mytemplate. Next we have to add an ng template element, ng-template, and we reference the local template variable that we created above with a #mytemplate. -->
<ng-template #mytemplate>My Template</ng-template>

<!--  finally, we could define a then statement to call templates that validate to being true.  -->
<div *ngIf="!title; then truthtempl else falsetempl">
    This will not appear.
  </div>

  <ng-template #truthtempl>I'm here</ng-template>
  <ng-template #falsetempl>I'm not here</ng-template>

<!--  So if, then, and else are all ways that you can conditionally show templates based on properties you define in the component class. -->
```
app.component.ts: 
```javascript
export class AppComponent {
  teams = [
    title = 'hello world', 
    { baseball: 'Indians'},
    { baseball: 'Mets'},
    { baseball: 'Yankees'},
  ]
}
```

### ngSwitch 
NgSwitchCase is a directive that allows you to specify a property and based on the value, show one of multiple template elements. This is useful when a given property can equal multiple values. The syntax for NgSwitchCase is to wrap ngSwitch in brackets and then binding it to a property. Then nest it inside of the element that contains the ngSwitch directive you add ngSwitchCase and bind it to any of the possible values that the property could match. If the given SwitchCase is true, it will show the element in the DOM.

ngSwitch syntax: 
```html
<ul [ngSwitch]="property">
  <li *ngSwitchCase="value1">Content</li>
  <li *ngSwitchCase="value2">Content</li>
</ul>
```

app.component.html:
```html
  <div [ngSwitch]="likes">
    <div *ngSwitchCase="'baseball'">// Display baseball content</div>
    <div *ngSwitchCase="'football'">// Display football content</div>
    <div *ngSwitchCase="'basketball'">// Display basketball content</div>
    <div *ngSwitchDefault>No matches found.</div>
  </div>

  <ng-template #truthtempl>I'm here</ng-template>
  <ng-template #falsetempl>I'm not here</ng-template>
```
app.component.ts: 
```javascript
export class AppComponent {
  likes = 'baseball';

  teams = [
    { baseball: 'Indians'},
    { baseball: 'Mets'},
    { baseball: 'Yankees'},
  ]
}
```

## Data Binding
### Local variables 
Local template variables allow you to reference and access HTML elements and their associated properties within other HTML elements in the view (we mean by inside the view, inside the html template!) . Now, defining a local variable is simple. You use the hash sign and the name of the variable. Then, depending on where you want to use that variable, you use interpolation with the local variable name inside.

Local variables syntax:
```html
<p #myVariable>My content</p>
<span>{{myVariable}}</span> 
```
app.component.html:
```html
  <input #count value="my value" (keyup)="0">  <!-- count is the local variable -->
  <p>{{ count.value.length }}</p>

  <button (click)="myFunc(count)">My button</button>
```

app.component.ts: 
```javascript
export class AppComponent {
  likes = 'soccer';

  teams = [
    { baseball: 'Indians'},
    { baseball: 'Mets'},
    { baseball: 'Yankees'},
  ]

  myFunc(count) {
    console.log(count.value);
  }
}
```


### Class binding 
In Angular, class binding allows you to add or remove CSS classes from an elements class attribute based on component logic. The syntax for defining a class binding on an HTML element starts off by wrapping class within optional class name, and binding it to some property. If the template expression evaluates to true, it will display the class.

Class binding synthax: 
```html
<p [class]="property">My Content</p>
<!-- OR -->
<p [class.class-name]="property">My Content</p>

```


app.component.ts: 
```javascript
@Component({

  styles: [ //writing inline css 
    `
    .danger {
      color:red;
      font-weight:bold;
    }
    .safe {
      text-decoration:underline;
    }
    .changed {
      font-style:italic;
    }
    `
  ]
})
export class AppComponent {
  
  property1 = true;
  property2 = true;
  property3 = false;
  myClasses = {
    danger: this.property1,
    safe: this.property2,
    changed: this.property3
  }

}

//And as you can see, we can control which classes are displayed based on whether or not our properties are true or false.

```


app.component.html:
```html
<p [ngClass]="myClasses">Class binding example</p>
```

### Style binding 
Style binding is very similar to class binding, but instead of targeting the class attribute, styles are applied to the style attribute. To define style binding for a single CSS property, you wrap stype in brackets and bind it to an expression. For sending multiple CSS properties, you can use the ngStyle directive and bind the property values to an object.

Syle binding synthax: 
```html
<p [style]="property">My Content</p>
<p [style.css-property]="property">My Content</p>
<p [ngStyle]="property">My Content</p>
```

app.component.ts: 
```javascript
@Component({

})
export class AppComponent {
  private myStyles = { 
    'color': 'red', 
    'font-weight':'bold'
  }
}

```

app.component.html:
```html
<p [ngStyle]="myStyles">Style binding example</p>
```

### Property Binding 
Property binding allows you to set a property of a view element. Property binding is referred to as being one way because while you can set a property, you cannot use it to read a property. So to use property binding, you wrap brackets around a property and bind it to a template expression.

Property Binding Syntax:
```html
<p [attribute-name]="property">My Content</p>
```
  app.component.html:
  ```html
  <img [src]="logoUrl">
  <button [disabled]="buttonState">Purchase</button>
```
app.component.ts: 
```javascript
export class AppComponent {

  buttonState = true;
  logoUrl = 'http://lnked.in/linkedinlogopng';

}
```

### Event binding 
When users interact with your angular app sometimes it's necessary to capture these events and know what's happening and that's where event binding comes in handy. Whether it's a simple mouse click or a given keystroke or a mouse hover, angular provides you with a way to capture data associated with these events and call methods within your component class. Like property binding, event binding is one way except it occurs in the opposite direction which is from the view to the component.

A basic event binding works by wrapping an event in parentheses and binding it to a method that can optionally pass through a target event variable. 

Event binding syntax:
```html
<p (event)="method($target)">My Content</p>
```

app.component.html:
```html
  <img [src]="logoUrl" *ngIf="buttonState">
  <button (mouseenter)="toggleLogo()">Toggle Logo</button>

  <input type="text" (keyup.enter)="keyPress($event.target.value)">
  <p>{{ typed }}</p>
```

app.component.ts: 
```javascript
export class AppComponent {

  buttonState = true;
  logoUrl = 'http://lnked.in/linkedinlogopng';
  typed:string = '';

  toggleLogo() {
    this.buttonState = (this.buttonState ? false : true);
  }

  keyPress(event) {
    this.typed = event;
  }
```

## Animation 
Animation plays a vital role in improving the user experience of your app. Fortunately, Angular provides us with the ability to easily define animations based on your app's logic. These animations are based on the Web Animations API which allow animations to run natively on browsers. So to get started with animation in Angular, we have to import some animation specific functions and also import the Angular animations library. New in Angular Four, the animations are no longer a part of the Angular core library.

So we have to first use npm to install the Angular animations library. 

```
npm install @angular/animations --save
```

In app.module.ts
```javascript
import { BrowserAnimationsModule } from '@angular/platform-browser/animations'; 

@NgModule({
  imports: [
    BrowserAnimationsModule
  ]
})
```
In app.components.ts
```javascript
import { trigger, state, style, transition, animate } from '@angular/animations';

@Component({
  selector: 'app-root',
  template: `

  `,
  styles: [``],
  animations: [

  ]
})
export class AppComponent {

}
;
```

### Triggers and states
The animation-specific Trigger function is the very first function that you must define when creating an animation. The trigger function defines each unique animation in your app. The first argument of this function is the name of the trigger. You will use this name to reference the specific animation in your view as you attach it to various HTML elements. The second argument of the trigger function accepts both a state and transition animation specific function. The state function defines a beginning and end state of a given animation. 

Triggers and states Syntax
```javascript
trigger('animationName', [
  state(name_state, style({}))
])
```
In app.components.ts
```javascript

@Component({
  selector: 'app-root',

  styles: [``],
  animations: [
    trigger('myAnimation', [
      state('inactive', style({
        backgroundColor: '#eee'
      })),
      state('active', style({
        backgroundColor: '#ffcc00'
      }))
    ])
  ]
})
export class AppComponent {

  animationState = 'inactive';

  animate() {
    this.animationState = (this.animationState === 'inactive' ? 'active' : 'inactive');
  }

}
```

In app.component.html
```html
    <p [@myAnimation]="animationState">I am learning to animate.</p>

    <button (click)="animate()">Animate the paragraph.</button>
```
Now, no animation is actually occurring between these two states, because that requires defining the transition animation specific function. We'll handle that next

### Transitions 
The animation-specific function Transition is what allows us to control the actual animation that occurs between two different states. The first argument of the transition function accepts the two different states and the direction between them. The next argument accepts the animate function, which allows you to define the timing and easing of the animation. Continuing on from the previous example, let's add a transition to our animation. 

Transitions syntax:
```javascript
transition('state1' => 'state2', [animate('250ms ease-in'))])
```
```javascript
import { Component } from '@angular/core';
import { trigger, state, style, transition, animate } from '@angular/animations';

@Component({
  selector: 'app-root',
  template: `
    <p [@myAnimation]="animationState">I am learning to animate.</p>

    <button (click)="animate()">Animate the paragraph.</button>
  `,
  styles: [``],
  animations: [
    trigger('myAnimation', [
      state('inactive', style({
        backgroundColor: '#eee'
      })),
      state('active', style({
        backgroundColor: '#ffcc00'
      })),

      transition('active <=> inactive', [
        style({
          transform: 'translateX(40px)'
        }),
        animate('500ms ease-in')
        ])
    ])
  ]
})
export class AppComponent {

  animationState = 'inactive';

  animate() {
    this.animationState = (this.animationState === 'inactive' ? 'active' : 'inactive');
  }

}
```

### Multi-step animations 
Angular animations give you the ability to create multi-step, keyframe-based animations. You can add the animation-specific function keyframes to the second argument of the animate function. The keyframe function accepts multiple style functions that allow you to define animatable properties along with an offset, which marks the beginning and end of each style animation. 

Syntax:
```javascript
animate('250ms ease-in', keyframes([
  style({property:'value', offset: 0})
  style({property:'value', offset: 0})
]))
```

```javascript
import { Component } from '@angular/core';
import { trigger, state, style, transition, animate, keyframes } from '@angular/animations';

@Component({
  selector: 'app-root',
  template: `
    <p [@myAnimation]="animationState">I am learning to animate.</p>

    <button (click)="animate()">Animate the paragraph.</button>
  `,
  styles: [``],
  animations: [
    trigger('myAnimation', [
      state('inactive', style({
        backgroundColor: '#eee'
      })),
      state('active', style({
        backgroundColor: '#ffcc00'
      })),

      transition('active <=> inactive', [
        animate('600ms ease-in-out', keyframes([
          style({transform: 'translateX(0)', backgroundColor: '#eee', offset: 0}),
          style({transform: 'translateX(50px) translateY(-10px)', offset: .5}),
          style({transform: 'translateX(0)', backgroundColor: '#ffcc00', offset: 1})
        ]))
      ])
    ])
  ]
})
export class AppComponent {

  animationState = 'inactive';

  animate() {
    this.animationState = (this.animationState === 'inactive' ? 'active' : 'inactive');
  }

}
```


## Creating routes manually (without cli)
Example: 
app.routes.ts:
```javascript
import { ModuleWithProviders } from '@angular/core';
import { Routes, RouterModule } from '@angular/router';

import { AppComponent } from './app.component';
import { AboutComponent } from './about/about.component';
import { ProductsComponent } from './products/products.component';

export const router: Routes = [
    { path: '', redirectTo: 'products', pathMatch: 'full'},
    { path: 'about', component: AboutComponent },
    { path: 'products', component: ProductsComponent }
];

export const routes: ModuleWithProviders = RouterModule.forRoot(router);
```
app.module.ts
```javascript
import { routes } from './app.routes';

@NgModule({
  declarations: [
    AppComponent,
    HeaderComponent,
    ProductsComponent,
    AboutComponent
  ],
  imports: [
    routes
  ],
```

app.component.html:
```html
<app-header></app-header>
<router-outlet></router-outlet>
```



