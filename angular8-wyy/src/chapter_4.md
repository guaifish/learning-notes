# 获取轮播数据

## 调用网易云音乐 API

克隆项目

```bash
git clone https://github.com/Binaryify/NeteaseCloudMusicApi
```

安装依赖

```bash
cd NeteaseCloudMusicApi
npm i
```

启动服务

```bash
node ./app.js
```

## 服务

### 创建服务

```bash
ng g s services/home
```

```ts
// home.service.ts
import { HttpClient } from '@angular/common/http';
import { Inject, Injectable } from '@angular/core';
import { Observable } from 'rxjs';
import { Banner } from './data-types/common.types';
import { API_CONFIG, ServicesModule } from './services.module';
import { map } from "rxjs/internal/operators";

@Injectable({
  providedIn: ServicesModule
})
export class HomeService {

  constructor(private http: HttpClient, @Inject(API_CONFIG) private uri: string) { }

  getBanners(): Observable<Banner[]> {
    return this.http.get(this.uri + "banner")
      .pipe(map((res: { banners: Banner[] }) => res.banners))
  }
}
```

### 定义数据结构

```ts
// services\data-types\common.types.ts
export type Banner = {
  targetId: number;
  url: string;
  imageUrl: string;
}
```

### 抽取 URL

```ts
import { InjectionToken, NgModule } from '@angular/core';

export const API_CONFIG = new InjectionToken("ApiConfigToken");

@NgModule({
  declarations: [],
  imports: [

  ],
  providers: [
    { provide: API_CONFIG, useValue: "http://localhost:4000/" }
  ]
})
export class ServicesModule { }
```

### 在组件中使用服务

```ts
// home.component.ts
constructor(private homeServe: HomeService) {
  this.homeServe.getBanners().subscribe(banners => {
    console.log("banners: ", banners);
  });
}
```

## 编写轮播组件

* `ng g c pages/home/components/wy-carousel`