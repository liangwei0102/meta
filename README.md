# 代码检索后端服务

## 功能说明

这是一个基于Node.js和Express的代码检索后端服务，用于在指定目录中搜索代码文件的内容和文件名。

## 支持的文件类型

- `.sql` - SQL脚本文件
- `.conf` - 配置文件
- `.sh` - Shell脚本文件
- `.txt` - 文本文件
- `.log` - 日志文件
- `.md` - Markdown文件
- `.json` - JSON文件
- `.xml` - XML文件
- `.yaml/.yml` - YAML文件

## 搜索目录配置

当前配置的搜索目录：
- Windows: `C:\Users\lwmic\D\work\Scripts\resources_20230523`
- Linux: `/home/user/scripts` (示例路径)

可以在 `server.js` 中的 `SEARCH_CONFIG` 对象中修改搜索目录。

## 安装和运行

### 1. 安装依赖
```bash
cd backend
npm install
```

### 2. 启动服务
```bash
# 开发模式 (自动重启)
npm run dev

# 生产模式
npm start
```

### 3. 验证服务
访问 http://localhost:3001/api/search/config 查看搜索配置

## API端点

### GET /api/search/config
获取搜索配置信息

**响应示例:**
```json
{
  "searchDirectory": "C:\\Users\\lwmic\\D\\work\\Scripts\\resources_20230523",
  "allowedExtensions": [".sql", ".conf", ".sh", ".txt", ".log", ".md", ".json", ".xml", ".yaml", ".yml"],
  "platform": "win32"
}
```

### POST /api/search
执行代码搜索

**请求体:**
```json
{
  "query": "搜索词"
}
```

**响应示例:**
```json
{
  "results": [
    {
      "type": "content",
      "file": "完整文件路径",
      "fileName": "文件名",
      "relativePath": "相对路径",
      "matches": [
        {
          "content": "匹配的行内容",
          "lineNumber": 123,
          "matchText": "高亮后的内容"
        }
      ]
    }
  ],
  "searchTerm": "搜索词",
  "searchDirectory": "搜索目录",
  "totalMatches": 1,
  "searchTime": 150,
  "message": "找到 1 个匹配结果"
}
```

## 注意事项

1. 确保搜索目录存在且有读取权限
2. 大文件（>10MB）会被跳过
3. 二进制文件会被自动识别并跳过
4. 隐藏目录（以.开头）会被跳过
5. node_modules和.git目录会被跳过 
