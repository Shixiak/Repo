# Vite

## 配置

```ts
import { defineConfig } from 'vite';
import vue from '@vitejs/plugin-vue';
import path from 'path';

// https://vite.dev/config/
export default defineConfig({
  plugins: [vue()],
  resolve: {
    alias: {
      '@': path.resolve(__dirname, './src')
    }
  }
})
```

### 引入依赖

```ts
import { defineConfig } from 'vite';
import vue from '@vitejs/plugin-vue';
import path from 'path';
```

* **`defineConfig`**：Vite 提供的辅助函数，让 TS 对配置对象有类型提示（而不是 `any`）。
* **`@vitejs/plugin-vue`**：Vite 官方 Vue 插件，负责解析和编译 `.vue` 文件（模板、样式、脚本）。
* **`path`**：Node.js 内置路径模块，用于处理文件路径（这里用来做路径别名）。

### plugins

```ts
plugins: [vue()],
```

* 告诉 Vite（也就是 Vitest）在编译 `.vue` 文件时，使用 Vue 插件来正确处理。
* 如果没有这个插件，Vitest 测试 Vue 组件时会因为无法解析 `.vue` 而报错。

## resolve.alias

```ts
resolve: {
  alias: {
    '@': path.resolve(__dirname, './src')
  }
}
```

* **作用**：配置路径别名，把 `@` 映射到项目根目录下的 `src` 文件夹。
* 好处：

  * 避免在导入文件时写长相对路径 `../../../components`
  * 改成 `@/components`
* `path.resolve(__dirname, './src')`：

  * `__dirname` 是当前配置文件所在目录
  * 结合 `path.resolve` 生成绝对路径，保证在不同平台都能正确解析。

## 代理服务器配置

```bash
POST http://localhost:5173/api/getCourtList 404 (Not Found)
```

通过 vite 运行前端页面的时候，直接发送的请求会发送到当前 vite 运行的端口号，但是我们的后端程序通常运行在另一个端口号上，所以需要配置代理服务器。

```ts
import { defineConfig } from 'vite';
import vue from '@vitejs/plugin-vue';

// https://vite.dev/config/
export default defineConfig({
  plugins: [vue()],
  server: {
    port: 5173,  // port 参数指定 vite 开发服务器启动的端口
    proxy: {  // 设置代理的属性
      "/api": {  // 前缀匹配，会匹配这个以这个前缀开头的请求
        target: "http://localhost:5001"  // 要转发到的服务器
      }
    }
  }
})
```

## 报错

### 配置 src 为 @

我们在 tsconfig.app.json 里面配置了 @ 为 src，但是在使用 vite 运行项目的时候，vite 不认识 @，所以在运行项目的时候会报这个错误。

```bash
(!) Failed to run dependency scan. Skipping dependency pre-bundling. Error: The following dependencies are imported but could not be resolved:

  @/views/OrderPage.vue (imported by C:/What/order-badminton/vue/src/App.vue?id=0)
```

要解决这个错误，我们需要单独给 vite 再配置一次。

```ts
import { defineConfig } from 'vite';
import path from 'path';

export default defineConfig({
    resolve: {
        alias: {
            '@': path.resolve(__dirname, './src');
        }
    }
})
```
