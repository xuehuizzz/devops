# fnm 安装node
```bash
# 安装并设置默认 LTS
fnm install --lts --use
fnm default "$(fnm current)"

# 验证
node -v
npm -v
fnm current
fnm list

# 常用命令
fnm install --lts --use  # 安装最新LTS并立即使用
fnm use 20  # 切换版本
fnm default 20  # 设置默认版本
fnm current # 查看当前版本
fnm list 或 fnm ls  # 列出已安装版本
fnm list-remote 或 fnm ls-remote  # 列出远程可用版本,加 --lts 只看 LTS
fnm uninstall 20  # 卸载版本
fnm alias 20 my-node  # 创建别名
fnm exec --using=20 node -v  # 执行指定版本

```

> 优先使用npx创建项目, 例如: npx create-next-app@latest report-system --typescript

# <mark>pnpm</mark><sub>管理项目的依赖包</sub>
```bash
npm install -g pnpm
pnpm -v

# 初始化项目
pnpm init   -y 跳过交互, 生成默认package.json

# 删除依赖
pnpm remove xxx
# 安装生产依赖
pnpm add xxx
# 安装开发依赖
pnpm add -D xxx   # -D等同于 npm 的 --save-dev
# 全局安装包
pnpm add -g xxx


前端框架
    React：pnpm create react-app my-app
    Vue：pnpm create vue@latest my-app
    Svelte：pnpm create svelte@latest my-app
全栈框架
    Next.js：pnpm create next-app my-app
    Nuxt.js：pnpm create nuxt@latest my-app
后端框架
    Express：pnpm create express-app my-app
    NestJS：pnpm create @nestjs/cli my-app

# 运行脚本
在 package.json 里写脚本，比如：

{
  "scripts": {
    "dev": "vite",
    "build": "vite build"
  }
}
然后:
pnpm dev
```

# degit
```bash
# 快速下载仓库的当前快照, 不包含git历史, 常用于下载模板项目使用
pnpm install -g degit

# how to use?
degit user/repo my-project
```

# yarn
```bash
# Yarn 和 npm 都是包管理工具, 可以共存, 但在同一个项目中最好只使用其中一个
npm install -g yarn
yarn init  # 初始化项目
yarn add <package>  # 安装依赖
yarn global add <package>  # 全局安装依赖
yarn remove <package>  # 删除依赖
yarn 或 yarn install   # 安装package.json中的所有依赖
yarn upgrade  # 更新所有依赖
yarn upgrade <package>  # 更新指定依赖
yarn list  # 
yarn cache clean  # 清理缓存
yarn start  # 启动项目
```

# npm ci
npm ci主要用于在自动化环境中安装依赖, 与npm install有以下区别:
1. 严格性:
   - npm ci必须有package.json文件存在
   - package.json和package-lock.json必须匹配, 否则会报错
   - 如果已存在node_modules目录, 会先删除再安装
2. 确定性:
   - 始终按照 package-lock.json 的确切版本安装依赖
   - 不会修改 package.json 和 package-lock.json
   - 安装结果在不同环境中完全一致
3. 速度:
   - 在 CI 环境中通常比 npm install 快
   - 因为跳过了一些不必要的计算和操作
4. 安全性:
   - 不会意外更新依赖版本
   - 能确保团队成员使用完全相同的依赖版本
   - 使用场景: CI/CD流程中, Docker镜像构建过程-
