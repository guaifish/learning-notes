# 模块化设计

## 创建模块

### 创建 `ShareModule`

```bash
ng g m share
```

移动 `AppModule` 内的

* `NgZorroAntdModule`
* `FormsModule`

到 `ShareModule` 并导出(`exports`)

### 创建 `PagesModule`

```bash
ng g m pages
```

导入 `ShareModule` 到 `PagesModule`

### 创建 `ServicesModule`

```bash
ng g m services
```

### 创建 `CoreModule`

```bash
ng g m core
```

移动 `AppModule` 内的

* `registerLocaleData(zh)`
* `BrowserModule`
* `AppRoutingModule`
* `HttpClientModule`
* `BrowserAnimationsModule`
* `providers: [{ provide: NZ_I18N, useValue: zh_CN }]`

到 `CoreModule`. *`AppRoutingModule` 要放在最后面*

导入

* `ServicesModule`
* `PagesModule`
* `ShareModule`

到 `CoreModule`

导出

* `ShareModule`
* `AppRoutingModule`

导入 `CoreModule` 到 `AppModule`

### 给 `CoreModule` 构造函数添加逻辑

```ts
constructor(@SkipSelf() @Optional() parentModule: CoreModule) {
  if (parentModule) {
    throw new Error("CoreModule 只能被AppModule引入");
  }
}
```
