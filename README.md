# Pnpm + Monorepo项目结构搭建

创建文件夹结构：
```bash
mkdir monorepo-template
cd monorepo-template
pnpm init
mkdir components
mkdir packages
```
添加配置文件`pnpm-workspace.yaml`：
```yaml
packages:
  - 'packages/**'
  - 'components/**'
```

在`components`文件夹设计自定义组件。

```bash
pnpm init
pnpm add vue typescript
mkdir Button
cd Button
touch index.vue
```

```vue
<template>
    <button class="btn">
        <slot></slot>
    </button>
</template>

<style>
.btn {
    width: 100px;
    height: 40px;
    background-color: rgb(216, 172, 172);
    border:none
}
</style>
```

导出组件，在Button同级文件夹下新建`index.ts`。

```bash
touch index.ts
```

```ts
import Button from './Button/index.vue'
export { Button }
```

修改`package.json`：
```json
{
  "name": "@mt/components",
  "version": "1.0.0",
  "private": true,
  "main": "index.ts",
   //...
}
```
确保子包可以被安装，项目根目录下新建`.npmrc`文件。
```text
link-workspace-packages = true
```

在packages文件夹下创建项目：
```bash
cd packages
pnpm create vite
```
安装`components`子包。
```bash
pnpm add @mt/components
```

测试使用`components`的组件。
```vue
<script setup lang="ts">
import { Button } from '@mt/components'
</script>

<template>
  <div>
    <Button>
      我是按钮
    </Button>
  </div>
</template>
```

项目根目录下`package.json`添加script。
```json
{
  "scripts": {
    "dev": "pnpm run -C packages/web dev",
    "build": "pnpm run -C packages/web build"
  },
}
```