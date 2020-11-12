# 创建项目

## 安装 Angular

```bash
mkdir ng-wyy
cd ng-wyy
npm i @angular/cli@8.3
ng --version
```

## 新建项目

```bash
ng new ng-wyy --directory . --style=less --routing -S
```

## 安装 ng-zorro

```bash
ng add ng-zorro-antd # y y zh_CN blank
```

## 创建全局样式

* `src/styles.less` 移动到 `src/assets/styles/index.less`
* 修改 `angular.json` styles 为 `src/assets/styles/index.less`
* 修改 `index.less`  `@import "~ng-zorro-antd/ng-zorro-antd.less";`
* 替换样式文件 `src/assets/`
* 安装 `minireset.css`: `npm i minireset.css`

### 尝试使用 ng-zorro 组件

```html
<!-- app.component.html -->
<button nz-button nzType="primary">button</button>
```
