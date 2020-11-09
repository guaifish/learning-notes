# 4. 组件简介

* 组件: `@Component` 装饰器
* 管道: `@Pipe` 装饰器
* 指令: `@Directive()` 装饰器

## 组件的元数据

```ts
@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css'],
  providers: [ ]
})
export class AppComponent {
  title = 'ng-app';
}
```

## 模板语法

```html
<h2>Hero List</h2>

<p><i>Pick a hero from the list</i></p>
<ul>
  <li *ngFor="let hero of heroes" (click)="selectHero(hero)">
    {{hero.name}}
  </li>
</ul>

<app-hero-detail *ngIf="selectedHero" [hero]="selectedHero"></app-hero-detail>
```

* `*ngFor` 指令告诉 Angular 在一个列表上进行迭代
* `{{hero.name}}`、`(click)` 和 `[hero]` 把程序数据绑定到及绑定回 DOM, 以响应用户的输入
* `<app-hero-detail>` 标签是一个代表新组件 HeroDetailComponent 的元素

## 数据绑定

* 插值
* 属性绑定
* 事件绑定
* 双向数据绑定

```html
<li>{{hero.name}}</li>
<app-hero-detail [hero]="selectedHero"></app-hero-detail>
<li (click)="selectHero(hero)"></li>
<input [(ngModel)]="hero.name">
```

## 管道

```html
<!-- Default format: output 'Jun 15, 2015'-->
 <p>Today is {{today | date}}</p>

<!-- fullDate format: output 'Monday, June 15, 2015'-->
<p>The date is {{today | date:'fullDate'}}</p>

 <!-- shortTime format: output '9:43 AM'-->
 <p>The time is {{today | date:'shortTime'}}</p>
```

## 指令

* 结构型指令: 通过添加、移除或替换 DOM 元素来修改布局
* 属性型指令: 修改现有元素的外观或行为

```html
<li *ngFor="let hero of heroes"></li>
<app-hero-detail *ngIf="selectedHero"></app-hero-detail>

<input [(ngModel)]="hero.name">
```
