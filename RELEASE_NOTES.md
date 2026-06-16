# 象棋V1.8 SendInput修改版 - 发布说明

## 最新版本 (2026/06/16 18:46)

### 优化内容
- ⚡ **显著提升快棋响应速度**：扫描间隔从100ms降至50ms（提速2倍）
- 🎯 **限制动画确认次数**：最多确认3次后直接使用识别结果，避免在快棋中等待过久
- ✅ **解决快棋后半段跟不上速度的问题**

### V1.8.1 优化说明 (快棋模式)
在快棋对战中，之前的版本可能在后半段无法及时识别对方走棋。本次优化：
1. 扫描频率提升2倍（100ms → 50ms）
2. 动画确认逻辑优化，最多3次确认后立即响应
3. 减少不必要的等待时间

**测试建议**：快棋模式下，程序现在能够更快地响应对方走棋。

---

## 初始版本 (2026/06/15 23:46)

### 修改内容

- **核心修改**：使用 Windows SendInput API 替代 Java Robot 类的点击操作
- **修改原因**：防止在线平台屏蔽自动走棋功能
- **修改效果**：棋子能够正确落下并完成自动走棋

## 技术细节

### 修改文件
1. `AbstractGraphLinker.java` - 添加 SendInput API 支持
   - 导入 JNA 库和 Windows API
   - 实现 `sendMouseDown()` 和 `sendMouseUp()` 方法
   - 修改 `mouseClickByFront()` 方法使用新的点击机制
   - 添加必要的延迟确保操作准确性

2. `WindowsGraphLinker.java` - 调整窗口句柄访问权限

3. `pom.xml` - 包含 Maven Shade 插件配置

### 技术原理
- SendInput 是操作系统级别的输入注入API
- 比 Robot 类更底层，不易被应用程序检测或屏蔽
- 添加了适当的延迟(50ms和200ms)确保棋子能够正确落下

## 运行程序下载

由于GitHub对单个文件大小有100MB限制，而完整的运行程序包含：
- `app.jar` (125.75 MB) - 修改后的程序
- `tchess.exe` - 启动程序
- `java/` - 完整的Java运行环境
- 其他资源文件

**运行程序文件位置**：
运行程序保存在本地：`C:\Users\z6\Desktop\Kiro\tchess_V1.8_windows_x64\tchess_V1.8\`

## 使用说明

1. 将运行程序目录解压到本地
2. 运行 `tchess.exe` 启动程序
3. 在程序中启用"连线"功能
4. 程序将自动从在线平台获取走棋建议并执行

## 注意事项

- 运行时鼠标会被程序占用（这是技术限制，无法同时满足"正常走棋"和"鼠标独立控制"）
- 确保象棋窗口在前台
- 确保已正确识别棋盘位置

## 源代码

源代码已托管在GitHub：https://github.com/mac958/public-Xiangqi

可以使用Maven编译：
```bash
mvn clean package
```

编译后的JAR文件位于 `target/app.jar`

## 原作者

原始项目来自：https://github.com/ZhangzheBJUT/public-Xiangqi
