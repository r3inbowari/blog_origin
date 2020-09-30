title: VSCode中调试TypeScript
date: 2020-05-23 00:35:58
tags:
---
参考: [ts-node](https://www.npmjs.com/package/ts-node)

1. 安装npm模块
```cmd
npm install --save-dev typescript ts-node tsconfig-paths
```

2. 生成工作区ts的配置文件
```cmd
tsc -init
```

3. 编译, 按住键盘 `Ctrl` + `Shift` + `B`, 其中监视是当.ts文件有变化时编译为js文件

4. 在工作区根目录随便新建一个demo.js, 并在 `launch.json` 中的 `configuration` 中添加一段配置, 注意 `program` 的目录要对上你要运行的目标js文件
```json
{
      "type": "node",
      "request": "launch",
      "name": "run",
      "skipFiles": ["<node_internals>/**"],
      "program": "${workspaceFolder}\\demo.js",
      "outFiles": ["${workspaceFolder}/**/*.js"]
},
```

5. 在上述方法中, 打断点只能对 `javascript` 文件生效, 也就是说无法对TS文件进行直接断点调试, 调试起来很麻烦. 这里用到dev依赖ts-node, 一个为nodejs提供的一个可交互的执行器, 通过它, 我们可以直接对ts文件进行直接断点调试(ts的版本要大于2.7哦)

6. 还是在 `configuration` 中新增一段配置
```json
{
      "name": "run ts",
      "type": "node",
      "request": "launch",
      "runtimeArgs": [
        "-r",
        "ts-node/register",
        "-r",
        "tsconfig-paths/register"
      ],
      "args": ["${workspaceFolder}/demo.ts"]
 }
 ```
 `args` 的第一个参数就是ts的入口位置  
 还有就是 `runtimeArgs` 注册两个参数  
 `ts-node` 用于处理ts文件  
 `tsconfig-paths` 用于处理 `tsconfig.json` 中 `paths` 的映射
 
7. 此时点击运行 `run ts` 即可正常响应断点.