---
{"dg-publish":true,"permalink":"/博客/利用反代CDN加速网站/","dgPassFrontmatter":true,"noteIcon":"","created":"2023-12-15T19:50:17.034+08:00","updated":"2023-12-15T19:51:59.452+08:00"}
---

### 通过Deno的cdn加速自己的网站

> 将deno的服务器作为中转相当于给自己的网站加了cdn

### 1.初始条件：

- 注册一个 github 账号并且 github 账号有一个月以上
- 注册一个 deno 账号

### 2.部署服务

- 新建项目并输入如下代码

```
mport { serve } from "https://deno.land/std@0.197.0/http/server.ts";

const targetWSHost = "xx.xxx.xxxx";
async function handler(req: Request): Promise<Response> {
  const url = new URL(req.url);
  const upgrade = req.headers.get("upgrade") || "";
  if (upgrade.toLowerCase() != "websocket") {
    url.host = targetWSHost;
    return await fetch(url, req);
  }

  const socketPromse = [];
  const { socket: downstream, response } = Deno.upgradeWebSocket(req);
  downstream.addEventListener("open", ()=>{
    console.log('open----downstream-------');
  })
  url.host = targetWSHost
  url.protocol = 'wss';
  url.port = '443'
  const upstream = new WebSocket(url);
  socketPromse.push(new Promise((resolve) => upstream.addEventListener("open", resolve)));
  await Promise.all(socketPromse);
  console.log("Both WebSocket connections are open");
  downstream.addEventListener("message", (message: any) => {
    upstream.send(message.data);
  });
  downstream.addEventListener("error", (error: any) => {
    console.log("error", error);
  });

  // Proxy messages from the upstream WebSocket connection to the downstream connection
  upstream.addEventListener("message", (message: any) => {
    downstream.send(message.data);
  });
  upstream.addEventListener('error', (e: any) => {
    console.log("error", e);
  });
  return response;
}

serve(handler);
```

- deno 多账号轮询

```
import { serve } from "https://deno.land/std@0.177.0/http/server.ts";


const targetWSHosts = [
  "hello-world-silent-mode-def6.b2l48k4lr8o.workers.dev",
  "hello-world-quiet-hat-1527.b2l48k4lr8o.workers.dev"
];
async function handler(req: Request): Promise<Response> {
  // 选择一个target host
  const targetHost = targetWSHosts[Math.floor(Math.random() * targetWSHosts.length)];
  const url = new URL(req.url);
  url.host = targetHost;
  const upgrade = req.headers.get("upgrade") || "";
  if (upgrade.toLowerCase() != "websocket") {
    url.host = targetHost;
    return await fetch(url, req);
  }

  const socketPromse = [];
  const { socket: downstream, response } = Deno.upgradeWebSocket(req);
  downstream.addEventListener("open", ()=>{
    console.log('open');
  })
  url.host = targetHost;
  url.protocol = 'wss';
  url.port = '443'
  const upstream = new WebSocket(url);
  socketPromse.push(new Promise((resolve) => upstream.addEventListener("open", resolve)));
  await Promise.all(socketPromse);
  // console.log("Both WebSocket connections are open");
  downstream.addEventListener("message", (message: any) => {
    upstream.send(message.data);
  });
  downstream.addEventListener("error", (error: any) => {
    console.log("error", error);
  });

  // messages from the upstream WebSocket connection to the downstream connection
  upstream.addEventListener("message", (message: any) => {
    downstream.send(message.data);
  });
  upstream.addEventListener('error', (e: any) => {
    console.log("error", e);
  });
  return response;
}

serve(handler);
```

### 通过cloudflare的cdn加速自己的网站

> 将cloudflare的服务器作为中转相当于给自己的网站加了cdn

### 1.CloudFlare Workers 单账户反代代码

```
addEventListener(
    "fetch",event => {
        let url=new URL(event.request.url);
        url.hostname="appname.herokuapp.com";
        let request=new Request(url,event.request);
        event. respondWith(
            fetch(request)
        )
    }
)
```

### 2.CloudFlare Workers 单双日轮换反代代码

```
const SingleDay = 'app0.herokuapp.com'
const DoubleDay = 'app1.herokuapp.com'
addEventListener(
    "fetch",event => {

        let nd = new Date();
        if (nd.getDate()%2) {
            host = SingleDay
        } else {
            host = DoubleDay
        }

        let url=new URL(event.request.url);
        url.hostname=host;
        let request=new Request(url,event.request);
        event. respondWith(
            fetch(request)
        )
    }
)
```

### 3.CloudFlare Workers 每五天轮换一遍式反代代码

```
const Day0 = 'app0.herokuapp.com'
const Day1 = 'app1.herokuapp.com'
const Day2 = 'app2.herokuapp.com'
const Day3 = 'app3.herokuapp.com'
const Day4 = 'app4.herokuapp.com'
addEventListener(
    "fetch",event => {

        let nd = new Date();
        let day = nd.getDate() % 5;
        if (day === 0) {
            host = Day0
        } else if (day === 1) {
            host = Day1
        } else if (day === 2) {
            host = Day2
        } else if (day === 3){
            host = Day3
        } else if (day === 4){
            host = Day4
        } else {
            host = Day1
        }

        let url=new URL(event.request.url);
        url.hostname=host;
        let request=new Request(url,event.request);
        event. respondWith(
            fetch(request)
        )
    }
)
```

#### 4,CloudFlare Workers 一周轮换反代代码

```
const Day0 = 'app0.herokuapp.com'
const Day1 = 'app1.herokuapp.com'
const Day2 = 'app2.herokuapp.com'
const Day3 = 'app3.herokuapp.com'
const Day4 = 'app4.herokuapp.com'
const Day5 = 'app5.herokuapp.com'
const Day6 = 'app6.herokuapp.com'
addEventListener(
    "fetch",event => {

        let nd = new Date();
        let day = nd.getDay();
        if (day === 0) {
            host = Day0
        } else if (day === 1) {
            host = Day1
        } else if (day === 2) {
            host = Day2
        } else if (day === 3){
            host = Day3
        } else if (day === 4) {
            host = Day4
        } else if (day === 5) {
            host = Day5
        } else if (day === 6) {
            host = Day6
        } else {
            host = Day1
        }

        let url=new URL(event.request.url);
        url.hostname=host;
        let request=new Request(url,event.request);
        event. respondWith(
            fetch(request)
        )
    }
)
```

<p align="center">
    <h2 align="center">Cloudflare Pages多账户反代代码</h2>
    <div align="center">将cloudflare的服务器作为中转相当于给自己的网站加了cdn</div>

### 1,操作注意点：

- 无论使用“项目代码拉取模式”、“文件夹目录上传模式”还是“ZIP 压缩上传模式”，请把文件名称一律改为\_worker.js(js 格式)，视频教程中已演示

- 修改\_worker.js 中的 url.hostname="app.example.com"中的 app.example.com 为你要反代的网址

- 使用本地上传模式时，你可能犹豫是先改 js 文件名，还是先改文件内的代码？无所谓先后，随意

- 以下三个多账户代码取自网络上 workers 反代的多账户写法，并参考 Pages 单账户反代代码项目

> 建议：比如 heroku 双账号，一般选择单双日轮换，可分别用两个账号的应用程序名（代理协议、PATH 路径、UUID 保持一致），单双号天分别执行，那一个月就有 550+550 小时，heroku 一个账户默认每月最多免费使用 550 小时

- 以下代码你也可以放置在 Github 上，只要在 Github 上更改网址，最终的 pages 反代内容会自动同步你的更改

### 2.Pages 单账户反代代码

```
export default {
    async fetch(request, env) {
      let url = new URL(request.url);
      if (url.pathname.startsWith('/')) {
        url.hostname="example.com";
        let new_request=new Request(url,request);
        return fetch(new_request);
      }
      // Otherwise, serve the static assets.
      return env.ASSETS.fetch(request);
    }
  };
```

### 3.CloudFlare Pages 单双日轮换反代代码

```
export default {
    async fetch(request, env) {
      const SingleDay = 'app0.example.com'
      const DoubleDay = 'app1.example.com'
      let host = ''
      let nd = new Date();
      if (nd.getDate()%2) {
          host = SingleDay
      } else {
          host = DoubleDay
      }
      let url = new URL(request.url);
      if (url.pathname.startsWith('/')) {
        url.hostname=host;
        let new_request=new Request(url,request);
        return fetch(new_request);
      }
      // Otherwise, serve the static assets.
      return env.ASSETS.fetch(request);
    }
  };
```

### 4.CloudFlare Pages 每五天轮换一遍式反代代码

```
export default {
    async fetch(request, env) {
      const Day0 = 'app0.example.com'
      const Day1 = 'app1.example.com'
      const Day2 = 'app2.example.com'
      const Day3 = 'app3.example.com'
      const Day4 = 'app4.example.com'
      let host = ''
      let nd = new Date();
        let day = nd.getDate() % 5;
        if (day === 0) {
            host = Day0
        } else if (day === 1) {
            host = Day1
        } else if (day === 2) {
            host = Day2
        } else if (day === 3){
            host = Day3
        } else if (day === 4){
            host = Day4
        } else {
            host = Day1
        }

      let url = new URL(request.url);
      if (url.pathname.startsWith('/')) {
        url.hostname=host;
        let new_request=new Request(url,request);
        return fetch(new_request);
      }
      // Otherwise, serve the static assets.
      return env.ASSETS.fetch(request);
    }
  };
```

### 5.CloudFlare Pages 一周轮换反代代码

```
export default {
    async fetch(request, env) {
      const Day0 = 'app0.example.com'
      const Day1 = 'app1.example.com'
      const Day2 = 'app2.example.com'
      const Day3 = 'app3.example.com'
      const Day4 = 'app4.example.com'
      const Day5 = 'app5.example.com'
      const Day6 = 'app6.example.com'
      let host = ''
      let nd = new Date();
        let day = nd.getDay();
        if (day === 0) {
            host = Day0
        } else if (day === 1) {
            host = Day1
        } else if (day === 2) {
            host = Day2
        } else if (day === 3){
            host = Day3
        } else if (day === 4) {
            host = Day4
        } else if (day === 5) {
            host = Day5
        } else if (day === 6) {
            host = Day6
        } else {
            host = Day1
        }

      let url = new URL(request.url);
      if (url.pathname.startsWith('/')) {
        url.hostname=host;
        let new_request=new Request(url,request);
        return fetch(new_request);
      }
      // Otherwise, serve the static assets.
      return env.ASSETS.fetch(request);
    }
  };
```

### 6.任意账号代码

```
export default {
async fetch(request, env) {
const cars = [
"app1.example.com",
"app2.example.com",
"app3.example.com",
"app4.example.com",
"app5.example.com"
];
let host = cars[Math.floor(Math.random() * cars.length)]; //随机选择VPS
//let host = cars[new Date().getDate() % cars.length]; //每天自动更换VPS

let url = new URL(request.url);
if (url.pathname.startsWith('/')) {
url.hostname = host;
let new_request = new Request(url,request);
return fetch(new_request);
}
return env.ASSETS.fetch(request);
}
};
```
