一键生成请求方法的工具 —— OpenAPI Typescript Codegen

# 用法

官网：https://github.com/ferdikoomen/openapi-typescript-codegen

首先安装：

```shell
npm install openapi-typescript-codegen --save-dev
```

--input：指定接口文档的路径、url 或字符串内容（必填）

--output：代码生成的目录

--client：生成的代码所需要使用的请求库

~~~shell
openapi --input ./spec.json --output ./generated --client xhr
~~~

![image-20240225133608737](https://cdn.jsdelivr.net/gh/Epiphany6666/Picture/blog/%E4%B8%80%E9%94%AE%E7%94%9F%E6%88%90%E8%AF%B7%E6%B1%82%E6%96%B9%E6%B3%95%E7%9A%84%E5%B7%A5%E5%85%B7%20%E2%80%94%E2%80%94%20OpenAPI%20Typescript%20Codegen/assets/202402251502053.png)

实例代码

~~~shell
openapi --input http://localhost:8121/api//v2/api-docs --output ./generated --client axios
~~~

执行完成后，在项目根目录中会出现下图目录，可以看见，它直接给我们生成了向后端发请求的函数，当需要使用时，只需要直接调用就好了。

![image-20240225144417848](https://cdn.jsdelivr.net/gh/Epiphany6666/Picture/blog/%E4%B8%80%E9%94%AE%E7%94%9F%E6%88%90%E8%AF%B7%E6%B1%82%E6%96%B9%E6%B3%95%E7%9A%84%E5%B7%A5%E5%85%B7%20%E2%80%94%E2%80%94%20OpenAPI%20Typescript%20Codegen/assets/202402251502211.png)

如果接口文档发生了变化，只需要再次执行一次上述指令，就可以重新生成了，非常的方便！

---

# 报错解决

明明已经执行了安装命令，却还是报错

![image-20240225143742140](https://cdn.jsdelivr.net/gh/Epiphany6666/Picture/blog/%E4%B8%80%E9%94%AE%E7%94%9F%E6%88%90%E8%AF%B7%E6%B1%82%E6%96%B9%E6%B3%95%E7%9A%84%E5%B7%A5%E5%85%B7%20%E2%80%94%E2%80%94%20OpenAPI%20Typescript%20Codegen/assets/202402251502624.png)



**解决办法**

法1：执行以下命令

> `npx` 是一个 Node.js 工具，用于执行安装在项目依赖中的可执行文件。它的作用是在不全局安装的情况下，临时运行项目依赖中的命令。
>
> 使用 `npx` 命令可以避免在全局安装可执行文件时可能出现的版本冲突问题，并且可以确保项目依赖的命令始终是最新版本。

~~~shell
npx openapi-typescript-codegen --input http://localhost:8121/api/v2/api-docs --output ./generated --client axios
~~~

法2：全局安装

~~~shell
npm install openapi-typescript-codegen -g
~~~

法3：将目录中的 node_modules/.bin 配置到环境变量中去



