---
title: "Vue3"
date: 2020-07-28T22:16:55+08:00
tages: ["vue3.x"]
---

## 开始

```
npm install
npm run serve
npm run build
```

vue3.x 结合 typescript 初体验

### 一、Vue3.0 的设计目标

- 更小\更快 - Vue 3.0 大小大概减少一半，只有 10kB
- 加强 TypeScript 支持
- 加强 API 设计一致性 - 易读
- 提高自身可维护性
- 开放更多底层功能

vue3.x 采用 Function-based API 形式组织代码，使其更容易压缩代码且压缩效率也更高,由于 修改了组件的声明方式，以函数组合的方式完成逻辑，天然与 typescript 结合。（vue2.x 中的组件是通过声明的方式传入一系列 options 的，所以在 2.x 下使用 typeScript 需要装饰器的方式`vue-class-component`才行）

```
  // vue2.x 要想使用ts 需要这样处理,详见官方文档 https://cn.vuejs.org/v2/guide/typescript.html
<script lang="ts">
    import Vue from 'vue'
    import Component from 'vue-class-component'
    @Component
    export default class App extends Vue {}
</script>
```

### 二、typescript 有什么优点

#### 1、增加代码的可读性与可维护性

- 大部分函数看类型定义就知道是干嘛的
- 静态类型检查，减少运行时错误

#### 2、社区活跃，大牛都在用

- **在 vue3 热潮下，之后 typescript 要成为主流，快学**

### 三、搭建 vue3.x + typescript + vue-router 环境

#### 1、全局安装 vue-cli

```
npm install -g @vue/cli
```

#### 2、初始化 vue 项目

```
vue create vue-next-project
```

这里在输入命令后，需要选择`Manually select features`, 至少要把 `babel` `typescript` `router` 选上，（`vuex` 看自身情况是否需要）。如下图:

![](https://user-gold-cdn.xitu.io/2020/5/22/1723b9c80913e36d?w=566&h=237&f=png&s=9590)

不清楚 vue-cli 的可参考[文章](https://juejin.im/post/5e69de93f265da570c75453e)

#### 3、升级为 vue3.x 项目

```
cd vue-next-project
vue add vue-next
```

这个命令会自动帮你把 vue2.x 升级到 vue3.x ，包括项目中对应的依赖进行升级与模板代码替换，像：`vue-router` `vuex`

完成以上三步主体环境算搭建完成，看刚才创建的目录里多了个 tsconfig.json 配置文件，可以根据自身与项目需要，进行配置。

接下来需要简单处理一下，使其支持 typescript 形式。（模板 cli 还没完善 typescript 的模板代码）

![](https://user-gold-cdn.xitu.io/2020/5/22/1723bb003f8fc297?w=965&h=520&f=png&s=30033)

- 将`shims-vue.d.ts`文件中的内容修改一下，这步操作应该会少了一些报错。

```
// declare module "*.vue" {
//   import Vue from 'vue';
//   export default Vue;
// }
declare module "*.vue" {
    import {defineComponent} from 'vue'
    const Component: ReturnType<typeof defineComponent>;
    export default Component
}
```

- 组件里使用需声明 `script lang="ts"` ,然后用`defineComponent`进行包裹,即可。

```
<script lang="ts">
import {
  defineComponent,
  ref,
  onMounted,
  onUpdated,
  onUnmounted,
} from "vue";

export default defineComponent({
  name: "hello world",
  props: {},
  setup(props: any) {
    const count = ref(0);
    const increase = (): void => {
      count.value++;
    };
    function test(x: number): string {
      return props.toString();
    }
    test(1);
    // test('number');
    // 生命周期
    onMounted(() => {
      console.log("mounted vue3 typescript");
    });
    onUpdated(() => {
      console.log("updated vue3 typescript");
    });
    onUnmounted(() => {
      console.log("onUnmounted vue3 typescript");
    });
    // 暴露给模板
    return {
      count,
      increase
    };
  },
});

</script>
```

生命周期对应
![](https://user-gold-cdn.xitu.io/2020/5/22/1723bc18105dcd9f?w=488&h=319&f=png&s=15352)

### 四、附上学习 vue-next 与 typescript 的官方秘籍

- [composition-api](https://composition-api.vuejs.org/zh/api.html#setup)
- [typescript](https://www.tslang.cn/)
- [typescript 教程](https://ts.xcatliu.com/introduction/what-is-typescript)

### 五、不想搭建你也可以直接拉去 github demo

[vue3+typescript 环境](xxx)
