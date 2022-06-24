# 项目说明 qiankun-vue

用`qiankun`来实现`vue`技术栈的前端微服务

`main`是主项目，`app-vue-hash`是`hash`模式路由的子项目，`app-vue-history`是`history`模式路由的子项目

`qiankun`的开发和打包和正常模式一模一样，它使用一个全局变量`__POWERED_BY_QIANKUN__`来区分微前端模式和正常模式，不需要额外的配置和代码。

## 运行和打包

在根目录下：

先安装依赖： `npm install`，再执行`npm run install-all`为所有项目安装依赖，最后执行`npm run start-all`即可启动所有的项目。

`npm run build-all`可以打包所有`vue`项目。

## 项目主要配置介绍

```js
// main/main.js 注册所有连接的子项目，有几个就可以配置几个，只要端口不重复就行
registerMicroApps([
  {
    name: "app-vue-hash",
    entry: "http://localhost:1111",
    container: "#appContainer",
    activeRule: "/app-vue-hash",
    props: { data: { store, router, textMsg: "测试消息" } },
  },
  {
    name: "app-vue-history",
    entry: "http://localhost:2222",
    container: "#appContainer",
    activeRule: "/app-vue-history",
    props: { data: store },
  },
]);
```

```js
// app-vue-hash/main.js 子项目渲染配置
function render({ data = {}, container } = {}) {
  router = new VueRouter({
    routes,
  });
  instance = new Vue({
    router,
    store,
    data() {
      return {
        parentRouter: data.router,
        parentVuex: data.store,
        textMsg: data.textMsg,
      };
    },
    render: (h) => h(App),
  }).$mount(container ? container.querySelector("#appVueHash") : "#appVueHash");
}

if (!window.__POWERED_BY_QIANKUN__) {
  render();
}
//测试全局变量污染
console.log("window.a", window.a);

export async function bootstrap() {
  console.log("vue app bootstraped");
}

export async function mount(props) {
  console.log("props from main framework", props.data);
  render(props);
}

export async function unmount() {
  instance.$destroy();
  instance.$el.innerHTML = "";
  instance = null;
  router = null;
}
```

## 总结
qiankun微前端的实现方式就是主项目和子项目分别都能自主启动，像案例里面的那样，main启动端口8080，两个子项目分别为1111、2222，都是能独立启动的，保证了开发互相独立，达到低耦合的效果。然后把子项目当成路由地址来看就行了。说再多都没有，自己领悟最重要。
>**项目并不复杂，多看看主项目和子项目的mian.js和根组件App.vue,基本就明白了**