---
title: 自建Docker Hub加速镜像
date: 2024-11-26T17:45:18+0800
tags:
  - docker
categories:
  - Web
---
# 自建 Docker Hub 加速镜像

在国内当个码农就像西天取经一样，想躺都不行，非得让你练点技能出来。

## 使用 CloudFlare Worker 搭建加速镜像

### 事先准备
- 一个在 Cloudflare 管理 DNS 的域名（不用在 Cloudflare 注册）。

---

### 1. 登录 Cloudflare
访问 [Cloudflare 仪表盘](https://dash.cloudflare.com/)。

---

### 2. 选择 Workers 和 Pages 功能
![选择 Workers 和 Pages](https://img.jbface.com/2024-11-26/1732616313-893334-20241126181004.png)

---

### 3. 点击创建
![点击创建](https://img.jbface.com/2024-11-26/1732616308-681244-27cdbc647ebcacfa58a29c6970c7605.png)

---

### 4. 创建 Worker
点击 **创建 Worker**：
![创建 Worker](https://img.jbface.com/2024-11-26/1732616305-624971-7b1c9fe965a576e5301b91318989ca5.png)

---

### 5. 部署 Worker
- 随便起一个名字。
![部署 Worker](https://img.jbface.com/2024-11-26/1732616304-551109-07d323a2a8fefcc2e0c5d4c574f642c.png)

---

### 6. 编辑代码
点击 **编辑代码**，然后复制下面 `_worker.js` 的内容，将 `DOMAIN.com` 修改为你的域名。

![编辑代码](https://img.jbface.com/2024-11-26/1732616311-623102-507e8c785be343e2e004791c6df57f1.png)

---

### 7. 点击部署
代码黏贴好后，点击 **部署**：
![点击部署](https://img.jbface.com/2024-11-26/1732617064-427454-20241126183016.png)

---

### 8. 设置自定义域
返回上级，进入 **设置**，点击 **添加**，选择 **自定义域**。

![选择自定义域](https://img.jbface.com/2024-11-26/1732616309-956077-71cb0fd43afda2ea5141a3770393454.png)
![选择自定义域](https://img.jbface.com/2024-11-26/1732616306-247210-7b84ebd6be2b131df06538e4ed9ae3e.png)
![选择自定义域](https://img.jbface.com/2024-11-26/1732617334-198524-20241126183016.png)

添加完成后，Cloudflare 会自动生成一条类型为 **Worker** 的 DNS 记录。等待 DNS 解析完成即可。

![DNS 解析](https://img.jbface.com/2024-11-26/1732616303-280866-3f91da8970ea67a82bbc14a856025c3.png)

---

### 9. 配置加速镜像
在需要使用加速镜像的地方，填入你在 **自定义域** 中配置的域名即可。

![配置域名](https://img.jbface.com/2024-11-26/1732617520-508038-20241126183016.png)

---

### 最后步骤
别忘了给我们的赛博佛祖 Cloudflare 磕一个！🙇🙇🙇

## _worker.js
```
// _worker.js

  

// Docker镜像仓库主机地址

let hub_host = 'registry-1.docker.io';

// Docker认证服务器地址

const auth_url = 'https://auth.docker.io';

// 自定义的工作服务器地址

let workers_url = 'https://DOMAIN.com/';

  

let 屏蔽爬虫UA = ['netcraft'];

  

// 根据主机名选择对应的上游地址

function routeByHosts(host) {

    // 定义路由表

    const routes = {

        // 生产环境

        "quay": "quay.io",

        "gcr": "gcr.io",

        "k8s-gcr": "k8s.gcr.io",

        "k8s": "registry.k8s.io",

        "ghcr": "ghcr.io",

        "cloudsmith": "docker.cloudsmith.io",

        "nvcr": "nvcr.io",

        // 测试环境

        "test": "registry-1.docker.io",

    };

  

    if (host in routes) return [ routes[host], false ];

    else return [ hub_host, true ];

}

  

/** @type {RequestInit} */

const PREFLIGHT_INIT = {

    // 预检请求配置

    headers: new Headers({

        'access-control-allow-origin': '*', // 允许所有来源

        'access-control-allow-methods': 'GET,POST,PUT,PATCH,TRACE,DELETE,HEAD,OPTIONS', // 允许的HTTP方法

        'access-control-max-age': '1728000', // 预检请求的缓存时间

    }),

}

  

/**

 * 构造响应

 * @param {any} body 响应体

 * @param {number} status 响应状态码

 * @param {Object<string, string>} headers 响应头

 */

function makeRes(body, status = 200, headers = {}) {

    headers['access-control-allow-origin'] = '*' // 允许所有来源

    return new Response(body, { status, headers }) // 返回新构造的响应

}

  

/**

 * 构造新的URL对象

 * @param {string} urlStr URL字符串

 */

function newUrl(urlStr) {

    try {

        return new URL(urlStr) // 尝试构造新的URL对象

    } catch (err) {

        return null // 构造失败返回null

    }

}

  

function isUUID(uuid) {

    // 定义一个正则表达式来匹配 UUID 格式

    const uuidRegex = /^[0-9a-f]{8}-[0-9a-f]{4}-[4][0-9a-f]{3}-[89ab][0-9a-f]{3}-[0-9a-f]{12}$/i;

    // 使用正则表达式测试 UUID 字符串

    return uuidRegex.test(uuid);

}

  

async function nginx() {

    const text = `

    <!DOCTYPE html>

    <html>

    <head>

    <title>Welcome to nginx!</title>

    <style>

        body {

            width: 35em;

            margin: 0 auto;

            font-family: Tahoma, Verdana, Arial, sans-serif;

        }

    </style>

    </head>

    <body>

    <h1>Welcome to nginx!</h1>

    <p>If you see this page, the nginx web server is successfully installed and

    working. Further configuration is required.</p>

    <p>For online documentation and support please refer to

    <a href="http://nginx.org/">nginx.org</a>.<br/>

    Commercial support is available at

    <a href="http://nginx.com/">nginx.com</a>.</p>

    <p><em>Thank you for using nginx.</em></p>

    </body>

    </html>

    `

    return text;

}

  

async function searchInterface() {

    const text = `

    <!DOCTYPE html>

    <html>

    <head>

        <title>Docker Hub Search</title>

        <style>

        body {

            font-family: Arial, sans-serif;

            display: flex;

            flex-direction: column;

            align-items: center;

            justify-content: center;

            height: 100vh;

            margin: 0;

            background: linear-gradient(to right, rgb(28, 143, 237), rgb(29, 99, 237));

        }

        .logo {

            margin-bottom: 20px;

        }

        .search-container {

            display: flex;

            align-items: center;

        }

        #search-input {

            padding: 10px;

            font-size: 16px;

            border: 1px solid #ddd;

            border-radius: 4px;

            width: 300px;

            margin-right: 10px;

        }

        #search-button {

            padding: 10px;

            background-color: rgba(255, 255, 255, 0.2); /* 设置白色，透明度为10% */

            border: none;

            border-radius: 4px;

            cursor: pointer;

            width: 44px;

            height: 44px;

            display: flex;

            align-items: center;

            justify-content: center;

        }          

        #search-button svg {

            width: 24px;

            height: 24px;

        }

        </style>

    </head>

    <body>

        <div class="logo">

        <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 18" fill="#ffffff" width="100" height="75">

            <path d="M23.763 6.886c-.065-.053-.673-.512-1.954-.512-.32 0-.659.03-1.01.087-.248-1.703-1.651-2.533-1.716-2.57l-.345-.2-.227.328a4.596 4.596 0 0 0-.611 1.433c-.23.972-.09 1.884.403 2.666-.596.331-1.546.418-1.744.42H.752a.753.753 0 0 0-.75.749c-.007 1.456.233 2.864.692 4.07.545 1.43 1.355 2.483 2.409 3.13 1.181.725 3.104 1.14 5.276 1.14 1.016 0 2.03-.092 2.93-.266 1.417-.273 2.705-.742 3.826-1.391a10.497 10.497 0 0 0 2.61-2.14c1.252-1.42 1.998-3.005 2.553-4.408.075.003.148.005.221.005 1.371 0 2.215-.55 2.68-1.01.505-.5.685-.998.704-1.053L24 7.076l-.237-.19Z"></path>

            <path d="M2.216 8.075h2.119a.186.186 0 0 0 .185-.186V6a.186.186 0 0 0-.185-.186H2.216A.186.186 0 0 0 2.031 6v1.89c0 .103.083.186.185.186Zm2.92 0h2.118a.185.185 0 0 0 .185-.186V6a.185.185 0 0 0-.185-.186H5.136A.185.185 0 0 0 4.95 6v1.89c0 .103.083.186.186.186Zm2.964 0h2.118a.186.186 0 0 0 .185-.186V6a.186.186 0 0 0-.185-.186H8.1A.185.185 0 0 0 7.914 6v1.89c0 .103.083.186.186.186Zm2.928 0h2.119a.185.185 0 0 0 .185-.186V6a.185.185 0 0 0-.185-.186h-2.119a.186.186 0 0 0-.185.186v1.89c0 .103.083.186.185.186Zm-5.892-2.72h2.118a.185.185 0 0 0 .185-.186V3.28a.186.186 0 0 0-.185-.186H5.136a.186.186 0 0 0-.186.186v1.89c0 .103.083.186.186.186Zm2.964 0h2.118a.186.186 0 0 0 .185-.186V3.28a.186.186 0 0 0-.185-.186H8.1a.186.186 0 0 0-.186.186v1.89c0 .103.083.186.186.186Zm2.928 0h2.119a.185.185 0 0 0 .185-.186V3.28a.186.186 0 0 0-.185-.186h-2.119a.186.186 0 0 0-.185.186v1.89c0 .103.083.186.185.186Zm0-2.72h2.119a.186.186 0 0 0 .185-.186V.56a.185.185 0 0 0-.185-.186h-2.119a.186.186 0 0 0-.185.186v1.89c0 .103.083.186.185.186Zm2.955 5.44h2.118a.185.185 0 0 0 .186-.186V6a.185.185 0 0 0-.186-.186h-2.118a.185.185 0 0 0-.185.186v1.89c0 .103.083.186.185.186Z"></path>

        </svg>

        </div>

        <div class="search-container">

        <input type="text" id="search-input" placeholder="Search Docker Hub">

        <button id="search-button">

            <svg focusable="false" aria-hidden="true" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">

            <path d="M21 21L16.65 16.65M19 11C19 15.4183 15.4183 19 11 19C6.58172 19 3 15.4183 3 11C3 6.58172 6.58172 3 11 3C15.4183 3 19 6.58172 19 11Z" stroke="white" fill="none" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"></path>

            </svg>

        </button>

        </div>

        <script>

        function performSearch() {

            const query = document.getElementById('search-input').value;

            if (query) {

            window.location.href = '/search?q=' + encodeURIComponent(query);

            }

        }

        document.getElementById('search-button').addEventListener('click', performSearch);

        document.getElementById('search-input').addEventListener('keypress', function(event) {

            if (event.key === 'Enter') {

            performSearch();

            }

        });

        </script>

    </body>

    </html>

    `;

    return text;

}

  

export default {

    async fetch(request, env, ctx) {

        const getReqHeader = (key) => request.headers.get(key); // 获取请求头

  

        let url = new URL(request.url); // 解析请求URL

        const userAgentHeader = request.headers.get('User-Agent');

        const userAgent = userAgentHeader ? userAgentHeader.toLowerCase() : "null";

        if (env.UA) 屏蔽爬虫UA = 屏蔽爬虫UA.concat(await ADD(env.UA));

        workers_url = `https://${url.hostname}`;

        const pathname = url.pathname;

  

        // 获取请求参数中的 ns

        const ns = url.searchParams.get('ns');

        const hostname = url.searchParams.get('hubhost') || url.hostname;

        const hostTop = hostname.split('.')[0]; // 获取主机名的第一部分

  

        let checkHost; // 在这里定义 checkHost 变量

        // 如果存在 ns 参数，优先使用它来确定 hub_host

        if (ns) {

            if (ns === 'docker.io') {

                hub_host = 'registry-1.docker.io'; // 设置上游地址为 registry-1.docker.io

            } else {

                hub_host = ns; // 直接使用 ns 作为 hub_host

            }

        } else {

            checkHost = routeByHosts(hostTop);

            hub_host = checkHost[0]; // 获取上游地址

        }

  

        const fakePage = checkHost ? checkHost[1] : false; // 确保 fakePage 不为 undefined

        console.log(`域名头部: ${hostTop}\n反代地址: ${hub_host}\n伪装首页: ${fakePage}`);

        const isUuid = isUUID(pathname.split('/')[1].split('/')[0]);

  

        if (屏蔽爬虫UA.some(fxxk => userAgent.includes(fxxk)) && 屏蔽爬虫UA.length > 0) {

            // 首页改成一个nginx伪装页

            return new Response(await nginx(), {

                headers: {

                    'Content-Type': 'text/html; charset=UTF-8',

                },

            });

        }

  

        const conditions = [

            isUuid,

            pathname.includes('/_'),

            pathname.includes('/r/'),

            pathname.includes('/v2/repositories'),

            pathname.includes('/v2/user'),

            pathname.includes('/v2/orgs'),

            pathname.includes('/v2/_catalog'),

            pathname.includes('/v2/categories'),

            pathname.includes('/v2/feature-flags'),

            pathname.includes('search'),

            pathname.includes('source'),

            pathname == '/',

            pathname == '/favicon.ico',

            pathname == '/auth/profile',

        ];

  

        if (conditions.some(condition => condition) && (fakePage === true || hostTop == 'docker')) {

            if (env.URL302) {

                return Response.redirect(env.URL302, 302);

            } else if (env.URL) {

                if (env.URL.toLowerCase() == 'nginx') {

                    //首页改成一个nginx伪装页

                    return new Response(await nginx(), {

                        headers: {

                            'Content-Type': 'text/html; charset=UTF-8',

                        },

                    });

                } else return fetch(new Request(env.URL, request));

            } else if (url.pathname == '/'){

                return new Response(await searchInterface(), {

                    headers: {

                      'Content-Type': 'text/html; charset=UTF-8',

                    },

                });

            }

            const newUrl = new URL("https://registry.hub.docker.com" + pathname + url.search);

  

            // 复制原始请求的标头

            const headers = new Headers(request.headers);

  

            // 确保 Host 头部被替换为 hub.docker.com

            headers.set('Host', 'registry.hub.docker.com');

  

            const newRequest = new Request(newUrl, {

                    method: request.method,

                    headers: headers,

                    body: request.method !== 'GET' && request.method !== 'HEAD' ? await request.blob() : null,

                    redirect: 'follow'

            });

  

            return fetch(newRequest);

        }

  

        // 修改包含 %2F 和 %3A 的请求

        if (!/%2F/.test(url.search) && /%3A/.test(url.toString())) {

            let modifiedUrl = url.toString().replace(/%3A(?=.*?&)/, '%3Alibrary%2F');

            url = new URL(modifiedUrl);

            console.log(`handle_url: ${url}`);

        }

  

        // 处理token请求

        if (url.pathname.includes('/token')) {

            let token_parameter = {

                headers: {

                    'Host': 'auth.docker.io',

                    'User-Agent': getReqHeader("User-Agent"),

                    'Accept': getReqHeader("Accept"),

                    'Accept-Language': getReqHeader("Accept-Language"),

                    'Accept-Encoding': getReqHeader("Accept-Encoding"),

                    'Connection': 'keep-alive',

                    'Cache-Control': 'max-age=0'

                }

            };

            let token_url = auth_url + url.pathname + url.search;

            return fetch(new Request(token_url, request), token_parameter);

        }

  

        // 修改 /v2/ 请求路径

        if ( hub_host == 'registry-1.docker.io' && /^\/v2\/[^/]+\/[^/]+\/[^/]+$/.test(url.pathname) && !/^\/v2\/library/.test(url.pathname)) {

            //url.pathname = url.pathname.replace(/\/v2\//, '/v2/library/');

            url.pathname = '/v2/library/' + url.pathname.split('/v2/')[1];

            console.log(`modified_url: ${url.pathname}`);

        }

  

        // 更改请求的主机名

        url.hostname = hub_host;

  

        // 构造请求参数

        let parameter = {

            headers: {

                'Host': hub_host,

                'User-Agent': getReqHeader("User-Agent"),

                'Accept': getReqHeader("Accept"),

                'Accept-Language': getReqHeader("Accept-Language"),

                'Accept-Encoding': getReqHeader("Accept-Encoding"),

                'Connection': 'keep-alive',

                'Cache-Control': 'max-age=0'

            },

            cacheTtl: 3600 // 缓存时间

        };

  

        // 添加Authorization头

        if (request.headers.has("Authorization")) {

            parameter.headers.Authorization = getReqHeader("Authorization");

        }

  

        // 发起请求并处理响应

        let original_response = await fetch(new Request(url, request), parameter);

        let original_response_clone = original_response.clone();

        let original_text = original_response_clone.body;

        let response_headers = original_response.headers;

        let new_response_headers = new Headers(response_headers);

        let status = original_response.status;

  

        // 修改 Www-Authenticate 头

        if (new_response_headers.get("Www-Authenticate")) {

            let auth = new_response_headers.get("Www-Authenticate");

            let re = new RegExp(auth_url, 'g');

            new_response_headers.set("Www-Authenticate", response_headers.get("Www-Authenticate").replace(re, workers_url));

        }

  

        // 处理重定向

        if (new_response_headers.get("Location")) {

            return httpHandler(request, new_response_headers.get("Location"));

        }

  

        // 返回修改后的响应

        let response = new Response(original_text, {

            status,

            headers: new_response_headers

        });

        return response;

    }

};

  

/**

 * 处理HTTP请求

 * @param {Request} req 请求对象

 * @param {string} pathname 请求路径

 */

function httpHandler(req, pathname) {

    const reqHdrRaw = req.headers;

  

    // 处理预检请求

    if (req.method === 'OPTIONS' &&

        reqHdrRaw.has('access-control-request-headers')

    ) {

        return new Response(null, PREFLIGHT_INIT);

    }

  

    let rawLen = '';

  

    const reqHdrNew = new Headers(reqHdrRaw);

  

    const refer = reqHdrNew.get('referer');

  

    let urlStr = pathname;

  

    const urlObj = newUrl(urlStr);

  

    /** @type {RequestInit} */

    const reqInit = {

        method: req.method,

        headers: reqHdrNew,

        redirect: 'follow',

        body: req.body

    };

    return proxy(urlObj, reqInit, rawLen);

}

  

/**

 * 代理请求

 * @param {URL} urlObj URL对象

 * @param {RequestInit} reqInit 请求初始化对象

 * @param {string} rawLen 原始长度

 */

async function proxy(urlObj, reqInit, rawLen) {

    const res = await fetch(urlObj.href, reqInit);

    const resHdrOld = res.headers;

    const resHdrNew = new Headers(resHdrOld);

  

    // 验证长度

    if (rawLen) {

        const newLen = resHdrOld.get('content-length') || '';

        const badLen = (rawLen !== newLen);

  

        if (badLen) {

            return makeRes(res.body, 400, {

                '--error': `bad len: ${newLen}, except: ${rawLen}`,

                'access-control-expose-headers': '--error',

            });

        }

    }

    const status = res.status;

    resHdrNew.set('access-control-expose-headers', '*');

    resHdrNew.set('access-control-allow-origin', '*');

    resHdrNew.set('Cache-Control', 'max-age=1500');

  

    // 删除不必要的头

    resHdrNew.delete('content-security-policy');

    resHdrNew.delete('content-security-policy-report-only');

    resHdrNew.delete('clear-site-data');

  

    return new Response(res.body, {

        status,

        headers: resHdrNew

    });

}

  

async function ADD(envadd) {

    var addtext = envadd.replace(/[  |"'\r\n]+/g, ',').replace(/,+/g, ','); // 将空格、双引号、单引号和换行符替换为逗号

    if (addtext.charAt(0) == ',') addtext = addtext.slice(1);

    if (addtext.charAt(addtext.length - 1) == ',') addtext = addtext.slice(0, addtext.length - 1);

    const add = addtext.split(',');

    return add;

}
```
### 

## References

* [【域名被墙 已更换】2024自建Docker镜像代理加速 教程来了 3分钟部署完毕](https://linux.do/t/topic/114345?filter=summary)