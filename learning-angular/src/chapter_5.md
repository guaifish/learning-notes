# 5. 服务和依赖注入简介

* 服务: `@Injectable()` 装饰器

服务范例

```ts
export class Logger {
  log(msg: any)   { console.log(msg); }
  error(msg: any) { console.error(msg); }
  warn(msg: any)  { console.warn(msg); }
}
```

## 依赖注入(dependency injection)

```ts
constructor(private service: HeroService) { }
```

## 提供服务

* 默认注册到根注入器中
* 在特定的 `NgModule` 注册提供者
* 在组件级注册提供者

```ts
@Injectable({
  providedIn: 'root',
})
```

```ts
@NgModule({
  providers: [
   BackendService,
   Logger
 ],
 ...
})
```

```ts
@Component({
  selector:    'app-hero-list',
  templateUrl: './hero-list.component.html',
  providers:  [ HeroService ]
})
```
