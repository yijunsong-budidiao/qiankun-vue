# ð°ï¸ð°ï¸é¡¹ç®è¯´æ qiankun-vueâï¸âï¸

ç¨`qiankun`æ¥å®ç°`vue`ææ¯æ çåç«¯å¾®æå¡

`main`æ¯ä¸»é¡¹ç®ï¼`app-vue-hash`æ¯`hash`æ¨¡å¼è·¯ç±çå­é¡¹ç®ï¼`app-vue-history`æ¯`history`æ¨¡å¼è·¯ç±çå­é¡¹ç®

`qiankun`çå¼ååæååæ­£å¸¸æ¨¡å¼ä¸æ¨¡ä¸æ ·ï¼å®ä½¿ç¨ä¸ä¸ªå¨å±åé`__POWERED_BY_QIANKUN__`æ¥åºåå¾®åç«¯æ¨¡å¼åæ­£å¸¸æ¨¡å¼ï¼ä¸éè¦é¢å¤çéç½®åä»£ç ã

## è¿è¡åæå

å¨æ ¹ç®å½ä¸ï¼

åå®è£ä¾èµï¼ `npm install`ï¼åæ§è¡`npm run install-all`ä¸ºææé¡¹ç®å®è£ä¾èµï¼æåæ§è¡`npm run start-all`å³å¯å¯å¨ææçé¡¹ç®ã

`npm run build-all`å¯ä»¥æåææ`vue`é¡¹ç®ã

## é¡¹ç®ä¸»è¦éç½®ä»ç»

```js
// main/main.js æ³¨åææè¿æ¥çå­é¡¹ç®ï¼æå ä¸ªå°±å¯ä»¥éç½®å ä¸ªï¼åªè¦ç«¯å£ä¸éå¤å°±è¡

registerMicroApps([
  {
    name: "app-vue-hash",
    entry: "http://localhost:1111",
    container: "#appContainer",
    activeRule: "/app-vue-hash",
    props: { data: { store, router, textMsg: "æµè¯æ¶æ¯" } },
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
// app-vue-hash/main.js å­é¡¹ç®æ¸²æéç½®

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
//æµè¯å¨å±åéæ±¡æ
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

## æ»ç»
qiankunå¾®åç«¯çå®ç°æ¹å¼å°±æ¯ä¸»é¡¹ç®åå­é¡¹ç®åå«é½è½èªä¸»å¯å¨ï¼åæ¡ä¾éé¢çé£æ ·ï¼mainå¯å¨ç«¯å£8080ï¼ä¸¤ä¸ªå­é¡¹ç®åå«ä¸º1111ã2222ï¼é½æ¯è½ç¬ç«å¯å¨çï¼ä¿è¯äºå¼åäºç¸ç¬ç«ï¼è¾¾å°ä½è¦åçææãç¶åæå­é¡¹ç®å½æè·¯ç±å°åæ¥çå°±è¡äºãè¯´åå¤é½æ²¡æï¼èªå·±é¢ææéè¦ã
>**é¡¹ç®å¹¶ä¸å¤æï¼å¤ççä¸»é¡¹ç®åå­é¡¹ç®çmian.jsåæ ¹ç»ä»¶App.vue,çè§£ä¸ä¸è·¯ç±,åºæ¬å°±æç½äº**