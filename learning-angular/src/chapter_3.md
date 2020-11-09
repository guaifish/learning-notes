# 3. 模块简介

## NgModule 元数据

- declarations(可声明对象表) —— 那些属于本 NgModule 的组件、指令、管道
- exports(导出表) —— 那些能在其它模块的组件模板中使用的可声明对象的子集
- imports(导入表) —— 那些导出了本模块中的组件模板所需的类的其它模块
- providers —— 本模块向全局服务中贡献的那些服务的创建器. 这些服务能被本应用中的任何部分使用
- bootstrap —— 应用的主视图，称为根组件. 它是应用中所有其它视图的宿主. 只有根模块才应该设置这个 bootstrap 属性

一个简单的根 NgModule 定义:

```ts
import { NgModule } from "@angular/core";
import { BrowserModule } from "@angular/platform-browser";
@NgModule({
  imports: [BrowserModule],
  providers: [Logger],
  declarations: [AppComponent],
  exports: [AppComponent],
  bootstrap: [AppComponent],
})
export class AppModule {}
```

## MgModule 和组件

- NgModule 为其中的组件提供了一个编译上下文环境
- 根模块总会有一个根组件, 并在引导期间创建它
- 任何模块都能包含任意数量的其它组件, 这些组件可以通过路由器加载, 也可以通过模板创建
- 那些属于这个 NgModule 的组件会共享同一个编译上下文环境

![组件编译上下文](https://angular.cn/generated/images/guide/architecture/compilation-context.png)

- 组件及其模板共同定义视图
- 组件还可以包含视图层次结构
- 一个视图层次结构中可以混合使用由不同 NgModule 中的组件定义的视图
- 当创建一个组件时, 它直接与一个叫做宿主视图的视图关联起来

![视图层次结构](https://angular.cn/generated/images/guide/architecture/view-hierarchy.png)

## Angular 自带的库

```ts
import { Component } from "@angular/core";
import { BrowserModule } from "@angular/platform-browser";
```

## 显示数据

- `{{ }}`: 插值显示组件的属性
- `*ngFor`: 显示数组
- `*ngIf`: 根据一个布尔表达式有条件地显示一段 HTML
- `ng generate class hero`: 创建类

## 模板语法

### 模板中的 HTML

- HTML 是 Angular 模板的语言
- `<script>` 元素被禁用, 以阻止脚本注入攻击的风险
- 有些合法的 HTML 被用在模板中没有意义, 比如 `<html>`、`<body>` 和 `<base>` 元素
- 可以通过组件和指令来扩展模板中的 HTML 词汇. 它们看上去就是新元素和属性

### 插值与模板表达式

- 插值表达式: `{{...}}`
- 花括号之间的文本通常是组件属性的名字, Angular 会把这个名字替换为响应组件属性的字符串值
- 括号间的素材是一个模板表达式, Angular 先对它求值, 再把它转换成字符串
- 这个表达式可以调用宿主组件的方法
- 如果你想用别的分隔符来代替 `{{` 和 `}}, 可以通过 `Component`元数据中的`interpolation` 选项来配置插值分隔符

```html
<h3>Current customer: {{ currentCustomer }}</h3>

<p>{{title}}</p>
<div><img src="{{itemImageUrl}}" /></div>

<!-- "The sum of 1 + 1 is 2" -->
<p>The sum of 1 + 1 is {{1 + 1}}.</p>

<!-- "The sum of 1 + 1 is not 4" -->
<p>The sum of 1 + 1 is not {{1 + 1 + getVal()}}.</p>
```

### 模板表达式

- 模板表达式会产生一个, 并出现在双花括号 `{{ }}` 中
- Angular 执行这个表达, 并把它赋值给绑定目标的属性, 这个绑定目标可能是 HTML 元素、组件或指令
- 模板表达式与 JavaScript 很像, 除了以下内容
  - 赋值 (`=`, `+=`, `-=`, ...)
  - `new`、`typeof`、`instanceof` 等运算符
  - 使用 `;` 或 `,` 串联起来的表达式
  - 自增和自减运算符: `++` 和 `--`
  - 一些 ES2015+ 版本的运算符
  - 不支持位运算, 比如 `|` 和 `&`
  - 新的模板表达式运算符, 例如 `|`、`?` 和 `!`
- 模板表达式不能引用全局命名空间中的任何东, 比如 `window` 或 `document`
- 不能调用 `console.log` 或 `Math.max`. 它们只能引用表达式上下文中的成员

### 表达式上下文

- 模板变量
- 指令的上下文变量
- 组件的成员

```html
<h4>{{recommended}}</h4>
<img [src]="itemImageUrl2" />

<ul>
  <li *ngFor="let customer of customers">{{customer.name}}</li>
</ul>

<label>Type something: <input #customerInput />{{customerInput.value}} </label>
```

### 模板语句

- 模板语句用来响应由绑定目标(如 HTML 元素、组件或指令)触发的事件
- 不允许的语法:
  - `new` 运算符
  - 自增和自减运算符: `++` 和 `--`
  - 操作并赋值, 例如 `+=` 和 `-=`
  - 位运算符, 例如 `|` 和 `&`
  - 管道运算符
- 语句上下文

```html
<button (click)="deleteHero()">Delete hero</button>

<button (click)="onSave($event)">Save</button>
<button *ngFor="let hero of heroes" (click)="deleteHero(hero)">
  {{hero.name}}
</button>
<form #heroForm (ngSubmit)="onSubmit(heroForm)">...</form>
```

### 绑定语法

| 绑定类型 |                                   语法                                    |           分类           |
|:--------:|:-------------------------------------------------------------------------:|:------------------------:|
| 插值属性 | `{{expression}}`<br>`[target]="expression"`<br>`bind-target="expression"` |    单向从数据源到视图    |
|   事件   |             `(target)="statement"`<br>`on-target="statement"`             | 从视图到数据源的单向绑定 |
|   双向   |         `[(target)]="expression"`<br>`bindon-target="expression"`         |           双向           |

* `attribute` 绑定
* `class` 绑定
* `style` 绑定

```html
<!-- create and set an aria attribute for assistive technology -->
<button [attr.aria-label]="actionName">{{actionName}} with Aria</button>

<!-- Notice the colSpan property is camel case -->
<tr><td [colSpan]="1 + 1">Three-Four</td></tr>

<!-- standard class attribute setting -->
<div class="foo bar">Some text</div>

<!-- standard style attribute setting -->
<div style="color: blue">Some text</div>
```

### 事件绑定

* `$event` 和事件处理语句
* 使用 `EventEmitter` 实现自定义事件

```html
<input [value]="currentItem.name"
       (input)="currentItem.name=$event.target.value" >
```

```html
<img src="{{itemImageUrl}}" [style.display]="displayNone">
<span [style.text-decoration]="lineThrough">{{ item.name }}
</span>
<button (click)="delete()">Delete</button>
```

```ts
// This component makes a request but it can't actually delete a hero.
@Output() deleteRequest = new EventEmitter<Item>();

delete() {
  this.deleteRequest.emit(this.item);
  this.displayNone = this.displayNone ? '' : 'none';
  this.lineThrough = this.lineThrough ? '' : 'line-through';
}
```

### 双向绑定

* 表单中的双向绑定

```ts
import { Component, Input, Output, EventEmitter } from '@angular/core';

@Component({
  selector: 'app-sizer',
  templateUrl: './sizer.component.html',
  styleUrls: ['./sizer.component.css']
})
export class SizerComponent {


  @Input()  size: number | string;
  @Output() sizeChange = new EventEmitter<number>();

  dec() { this.resize(-1); }
  inc() { this.resize(+1); }

  resize(delta: number) {
    this.size = Math.min(40, Math.max(8, +this.size + delta));
    this.sizeChange.emit(this.size);
  }

}
```

```html
<div>
  <button (click)="dec()" title="smaller">-</button>
  <button (click)="inc()" title="bigger">+</button>
  <label [style.font-size.px]="size">FontSize: {{size}}px</label>
</div>
```

```html
<app-sizer [(size)]="fontSizePx"></app-sizer>
<div [style.font-size.px]="fontSizePx">Resizable Text</div>
```

### 内置指令

* 属性型指令
  * `NgClass` —— 添加和删除一组 CSS 类
  * `NgStyle` —— 添加和删除一组 HTML 样式
  * `NgModel` —— 将数据双向绑定添加到 HTML 表单元素
* 结构型指令
  * `NgIf` —— 从模板中创建或销毁子视图
  * `NgFor` —— 为列表中的每个条目重复渲染一个节点
  * `NgSwitch` —— 一组在备用视图之间切换的指令

### 模板引用变量

* 模板引用变量通常是对模板中 DOM 元素的引用
* 可以引用指令(包含组件)、元素、`TemplateRef` 或 `Web Component`
* `#` 或 `ref-` 前缀

### 输入和输出属性

* `@Input()`
* `@Output()`

### 模板表达式中的运算符

* 管道: 简单的函, 它们接受输入值并返回转换后的值
* 安全导航运算符: 对在属性路径中出现 `null` 和 `undefined` 值进行保护
* 非空断言运算符: 有类型的变量默认是不允许 `null` 或 `undefined` 值

```html
<p>Title through uppercase pipe: {{title | uppercase}}</p>
<!-- convert title to uppercase, then to lowercase -->
<p>Title through a pipe chain: {{title | uppercase | lowercase}}</p>
<!-- convert title to uppercase, then to lowercase -->
<p>Title through a pipe chain: {{title | uppercase | lowercase}}</p>
<p>Item json pipe: {{item | json}}</p>

<p>The item name is: {{item?.name}}</p>

<!-- Assert color is defined, even if according to the `Item` type it could be undefined. -->
<p>The item's color is: {{item.color!.toUpperCase()}}</p>
```

### 内置模板函数

* 类型转换函数 `$any()`

```html
<p>The item's undeclared best by date is: {{$any(item).bestByDate}}</p>
<p>The item's undeclared best by date is: {{$any(this).bestByDate}}</p>
```

### 模板中的 SVG

```ts
import { Component } from '@angular/core';

@Component({
  selector: 'app-svg',
  templateUrl: './svg.component.svg',
  styleUrls: ['./svg.component.css']
})
export class SvgComponent {
  fillColor = 'rgb(255, 0, 0)';

  changeColor() {
    const r = Math.floor(Math.random() * 256);
    const g = Math.floor(Math.random() * 256);
    const b = Math.floor(Math.random() * 256);
    this.fillColor = `rgb(${r}, ${g}, ${b})`;
  }
}
```

```html
<svg>
  <g>
    <rect x="0" y="0" width="100" height="100" [attr.fill]="fillColor" (click)="changeColor()" />
    <text x="120" y="50">click the rectangle to change the fill color</text>
  </g>
</svg>
```
