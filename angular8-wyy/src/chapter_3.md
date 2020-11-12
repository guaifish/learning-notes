# 首尾布局

## 编写页面

`app.component.html`

```html
<div id="app">
  <nz-layout class="layout">
    <nz-header class="header">
      <div class="wrap">
        <div class="left">
          <h1>Music</h1>
          <ul nz-menu nzTheme="dark" nzMode="horizontal">
            <li nz-menu-item>发现</li>
            <li nz-menu-item>歌单</li>
          </ul>
        </div>
        <div class="right">
          <div class="search">
            <nz-input-group [nzSuffix]="suffixIconSearch">
              <input type="text" nz-input placeholder="歌单/歌手/歌曲">
            </nz-input-group>
            <ng-template #suffixIconSearch>
              <i nz-icon type="search"></i>
            </ng-template>
          </div>
          <div class="member">
            <div class="no-login">
              <ul nz-menu nzTheme="dark" nzMode="horizontal">
                <li nz-submenu>
                  <div title>
                    <span>登录</span>
                    <i nz-icon type="down" nzTheme="outline"></i>
                  </div>
                  <ul>
                    <li nz-menu-item>
                      <i nz-icon type="mobile" nzTheme="outline"></i>
                      手机登录
                    </li>
                    <li nz-menu-item>
                      <i nz-icon type="user-add" nzTheme="outline"></i>
                      注册
                    </li>
                  </ul>
                </li>
              </ul>
            </div>
          </div>
        </div>
      </div>
    </nz-header>
    <nz-content class="content">
      <router-outlet></router-outlet>
    </nz-content>
    <nz-footer class="footer">
      Ant Design ©2019 Implement By Angular
    </nz-footer>
  </nz-layout>
</div>
```

## 编写样式

`app.component.less`

```less
@import '../assets/styles/varibles.less';
@import '../assets/styles/mixins.less';

#app .layout {
  .header {
    .wrap {
      display: flex;
      justify-content: space-between;
      width: 1100px;
      .left {
        display: flex;
        h1 {
          width: 157px;
          color: @border-color-base;
          font-size: @font-size-lgx;
          margin-bottom: 0;
          margin-right: 20px;
          text-align: center;
        }
        ul {
          height: 64px;
          line-height: 64px;
        }
      }
      .right {
        display: flex;
        .member {
          display: flex;
          align-items: center;
          color: @white-color;
        }
      }
    }
  }
  .content {
    background-color: @body-color;
  }
  .footer {
    text-align: center;
  }
}

:host
 ::ng-deep .ant-back-top {
   bottom: 80px;
 }
```

## 创建 `HomeModule`

```bash
ng g m pages/home --routing
```

* 移除 `CommonModule`
* 将 `PagesModule` 的 `ShareModule` 移动到 `HomeModule`
* 导入 `HomeModule` 到 `PagesModule`

## 创建 `HomeComponent`

```bash
ng g c pages/home
```

## 编写 home 路由

`home-routing.module.ts`

```ts
const routes: Routes = [
  { path: 'home', component: HomeComponent, data: { title: '发现' }},
];
```

重定向路由

`app-routing.module.ts`

```ts
const routes: Routes = [
  { path: '', redirectTo: 'home', pathMatch: 'full' },
];
```