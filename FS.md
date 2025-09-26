# FS 模块

Node 中用来读取文件的模块。

## readFileSync

同步读取文件。

使用示例：

### 读取 JSON 文件

文件对脚本的相对路径为 `../data/users.json`，文件格式为：

```json
{
    "DZZI": {
        "token": "",
        "password":"031213",
        "email": "zsx314@qq.com"
    },
    "ZHY": {
        "token": "",
        "password": "",
        "email": "Z0207199945@163.com"
    }
}
```

示例代码：

```ts
import fs from 'fs';
import path from 'path';

const path = path.resolve(__dirname, '../data/users.json');
const rawData = fs.readFileSync(filePath, 'utf-8');
const users = JSON.parse(rawData);
```
