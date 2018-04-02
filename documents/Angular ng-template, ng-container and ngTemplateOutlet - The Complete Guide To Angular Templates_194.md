# Angular ng-template, ng-container and ngTemplateOutlet - The Complete Guide To Angular Templates

https://blog.angular-university.io/angular-ng-template-ng-container-ngtemplateoutlet/

## ng-template 
this means that the content of this tag will contain part of a template, that can be then be composed together with other templates in order to form the final component template.

```ts
/// nothing diaplayed, because the temolate is defed but not used.
@Component({
  selector: 'app-root',
  template: `      
      <ng-template>
          <button class="tab-button" 
                  (click)="login()">{{loginText}}</button>
          <button class="tab-button" 
                  (click)="signUp()">{{signUpText}}</button>
      </ng-template>
  `})
export class AppComponent {
    loginText = 'Login';
    signUpText = 'Sign Up'; 
    lessons = ['Lesson 1', 'Lessons 2'];

    login() {
        console.log('Login');
    }

    signUp() {
        console.log('Sign Up');
    }
}
```


```html
<div class="lessons-list" *ngIf="lessons else loading">
  ... 
</div>

<ng-template #loading>
    <div>Loading...</div>
</ng-template>


<ng-template [ngIf]="lessons" [ngIfElse]="loading">
   <div class="lessons-list">
     ... 
   </div>
</ng-template>

<ng-template #loading>
    <div>Loading...</div>
</ng-template>

```