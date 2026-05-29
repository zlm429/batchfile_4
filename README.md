# 批量文件夹重命名工具

## 📦 项目信息

- **GitHub**: https://github.com/zlm429/batchfile_new
- **Gitee**: https://gitee.com/zeng-limin00/batchfile_app

---

## ✨ 功能说明

这是一个功能强大的桌面应用程序，提供三种文件管理功能：

### 功能1：批量重命名文件夹 📁
1. **Excel 4列数据组合**：读取Excel前4列（A+B+C+D），自动组合成文件夹名称
2. **选择性重命名**：支持多选需要重命名的文件夹，不是全部处理
3. **智能冲突处理**：如果目标名称已存在，自动添加序号避免覆盖

### 功能2：批量重命名文件夹内的文件 📄
1. **自动重命名**：文件夹内的所有文件会自动重命名为与文件夹相同的名字（保留原扩展名）
2. **使用料号命名**：自动提取文件夹名称第一个"-"前的部分作为文件名
3. **跳过空文件夹**：自动检测并跳过没有文件的文件夹

### 功能3：自动分类文件到对应文件夹 【新功能】
1. **智能匹配**：自动识别文件名是否包含文件夹名
2. **批量移动**：将匹配的文件自动移动到对应文件夹
3. **冲突处理**：如果目标位置已有同名文件，自动添加序号

**使用场景示例：**
```
原始结构：
├── A000/
├── A001/
├── A002/
── A000-1.txt
├── A001-1.txt
└── A002.txt

执行后：
├── A000/
│   └── A000-1.txt
── A001/
│   └── A001-1.txt
└── A002/
    └── A002.txt
```

## 技术栈

- Java 17
- JavaFX 17.0.2（图形界面）
- Apache POI 5.2.3（Excel文件处理）

## 使用方法

### 1. 准备Excel文件
- Excel文件需包含4列数据（A、B、C、D列）
- 程序会自动将4列数据用下划线连接组合成文件夹名称
- 每行对应一个文件夹名称
- 确保名称数量不少于需要重命名的文件夹数量

**Excel示例：**
```
| A列            | B列    | C列           | D列            |
|---------------|-------|--------------|---------------|
| J76000016.3426 | 雾卡其 | 170/92A(M)   | 6942674037033 |
| J76000016.3427 | 雾卡其 | 175/96A(L)   | 6942674037040 |
```

**生成的文件夹名称：**
```
J76000016.3426_雾卡其_170/92A(M)_6942674037033
J76000016.3427_雾卡其_175/96A(L)_6942674037040
```

### 2. 运行程序
```bash
mvn clean javafx:run
```

或者打包后运行：
```bash
mvn clean package
java -jar target/batchfile_app-0.0.1-SNAPSHOT.jar
```

### 3. 操作步骤

#### 功能1：重命名文件夹
1. 点击"📋 从剪贴板获取文件夹"按钮
2. 点击"开始重命名文件夹"按钮执行操作

#### 功能2：重命名文件夹内的文件
1. 在资源管理器中选中需要处理的文件夹（可多选）
2. 按 Ctrl+C 复制
3. 点击"📋 从剪贴板获取文件夹"按钮
4. 点击"开始重命名文件"按钮

#### 功能3：分类文件到文件夹
1. 在资源管理器中同时选中文件夹和文件（可多选）
2. 按 Ctrl+C 复制
3. 点击"📋 从剪贴板获取文件夹"按钮
4. 点击"分类文件到文件夹"按钮
5. 查看日志确认移动结果

---

## 📂 项目结构

```
batchfile_app/
├── .github/
│   └── workflows/
│       └── macos-build.yml          # GitHub Actions 自动打包配置
├── src/main/java/com/rt/batchfile_app/
│   ├── BatchfileAppApplication.java  # 主应用程序入口
│   ├── RenameController.java         # 控制器类，处理业务逻辑
│   └── module-info.java              # Java模块配置
├── src/main/resources/com/rt/batchfile_app/
│   └── hello-view.fxml               # JavaFX界面布局文件
├── mvnw / mvnw.cmd                   # Maven Wrapper 脚本
└── pom.xml                           # Maven 项目配置
```

---

## 🛠️ 构建和运行

### 开发环境运行
```bash
mvn clean javafx:run
```

### 使用 Maven Wrapper（推荐）
```bash
# Windows
.\mvnw.cmd clean javafx:run

# macOS/Linux
./mvnw clean javafx:run
```

### 打包 JAR 文件
```bash
mvn clean package
```

生成的JAR文件位于 `target/batchfile_app-0.0.1-SNAPSHOT.jar`

### macOS 打包（本地）

如果您想在本地打包 macOS 应用：

```bash
# 编译项目
./mvnw clean package -DskipTests

# 打包成 DMG
jpackage --input target \
         --name "BatchRenameTool" \
         --main-jar batchfile_app-0.0.1-SNAPSHOT.jar \
         --main-class com.rt.batchfile_app.BatchfileAppApplication \
         --dest target/dist \
         --type dmg \
         --app-version 1.0.0 \
         --vendor RT \
         --java-options "--add-modules" \
         --java-options "javafx.controls,javafx.fxml"
```

---



## ❓ 故障排除

### 常见问题

1. **无法启动**
   - 确认Java版本为17或更高：`java -version`
   - 如果使用 macOS，确保从 Applications 文件夹运行

2. **Excel读取失败**
   - 检查文件格式是否为 .xlsx 或 .xls
   - 确保文件没有被其他程序占用

3. **重命名失败**
   - 检查文件名是否包含非法字符：\ / : * ? " < > |
   - 确保有足够的磁盘权限进行文件操作
   - 关闭可能占用文件的程序

4. **macOS 应用无法打开**
   - 右键点击应用 → 选择"打开"
   - 在弹出对话框中再次点击"打开"
   - 这是 macOS 的安全机制，只需首次设置

5. **GitHub Actions 没有自动触发**
   - 检查仓库 Settings → Actions → General
   - 确保启用了 Actions 功能
   - 手动触发：点击 Actions → Run workflow

---

## 🤝 贡献

欢迎提交 Issue 和 Pull Request！

## 📝 许可证

MIT License

---

**祝您使用愉快！** 🎉
