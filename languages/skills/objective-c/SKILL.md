---
name: "objective-c"
description: "Objective-C开发专家助手。当用户需要进行Objective-C语言开发、遗留iOS代码维护、ARC内存管理、Block/Category/Protocol使用或Swift混编时调用。"
---

# Objective-C 开发技能

你是一位资深 Objective-C 开发工程师。在协助 Objective-C 项目时，请遵循以下规范。

> iOS 应用工程与 UI 开发见 mobile-desktop/skills/ios；Swift 语言见 languages/skills/swift；本技能专注 Objective-C 语言本身（遗留代码维护与混编）。

## 技术栈强制约束

- 所有 Objective-C 代码强制启用 ARC（Automatic Reference Counting），禁止新建 MRC 文件
- Objective-C 主要用于遗留项目维护与 Swift 混编场景，新增模块优先评估 Swift
- 最低部署目标以项目既有设置为准，禁止擅自调整
- 头文件引入使用 #import，避免 #include 的重复导入问题

## 命名规范

- 类名必须带 2~3 个大写字母的项目前缀（如 `YDLoginViewController`），避免与系统类（NS/UI/CL 等前缀）冲突
- 方法名：小驼峰，语义完整可连读成句（`insertObject:atIndex:`）
- 属性名：小驼峰，getter 不加 get 前缀（Apple 惯例）
- 常量：static const 加项目前缀的小驼峰（`static const NSTimeInterval YDLoginTimeout = 30.0`），优先于 #define
- 文件名与主类名一致；Category 文件命名为 `原类名+功能`（`NSString+YDHTML.h`）
- 协议名体现能力或角色：能力用 -able 结尾，回调协议用 -Delegate / -DataSource 结尾
- 命名语义化，禁止拼音、无意义缩写

## 内存管理规范（ARC）

- 全程由 ARC 自动管理引用计数，禁止手写 retain / release / autorelease / retainCount
- 循环引用用 weak 引用打破：代理属性声明为 weak；Block 持有 self 前先声明 `__weak typeof(self) weakSelf = self;`
- Block 长时间执行需要保活时，在 Block 内用 `__strong typeof(self) strongSelf = weakSelf;` 做局部强化，避免执行中对象被释放
- 所有权归类：拥有关系（子视图、数据）用 strong，委托与回调用 weak，字符串属性按需 copy
- Core Foundation 与 Foundation 桥接注意所有权：`__bridge` 仅转换不转移，`__bridge_transfer`（CFBridgingRelease）交由 ARC 接管，`__bridge_retained`（CFBridgingRetain）转出手动管理

## 核心语法规范

- 属性修饰符：strong / weak / copy / assign / readonly / nonatomic；iOS 场景属性统一加 nonatomic
- 字符串属性建议 copy，防止可变字符串赋值后被外部修改
- 集合与字符串优先使用字面量语法：`@""`、`@[...]`、`@{...}`、`@(100)`，禁止过时的 `arrayWithObjects:..., nil` 写法
- Block：回调集中定义与销毁；Block 内引用 self 必须先做 weak 处理，防止循环引用
- Category（分类）：为既有类扩展方法；自定义方法名必须加项目前缀（`- (void)yd_reloadData;`），避免与系统方法或其它分类冲突引发未定义行为；分类不应承载存储属性
- Class Extension（匿名分类，写在 .m 中）：声明私有属性与私有方法，对外隐藏实现细节
- 协议：@protocol 定义接口契约；调用 @optional 方法前必须用 respondsToSelector: 判断
- 枚举统一使用 NS_ENUM / NS_OPTIONS 宏定义
- 对外集合属性使用轻量级泛型标注：`NSArray<YDUser *> *`

## 与 Swift 互操作要点

- Swift 调用 Objective-C：通过桥接头文件（{ProductName}-Bridging-Header.h）暴露头文件
- Objective-C 调用 Swift：引入 Xcode 自动生成的 {ProductName}-Swift.h，Swift 类型需标注 @objc 或 @objcMembers
- 暴露给 Swift 的头文件必须完善 nullability 注解：使用 NS_ASSUME_NONNULL_BEGIN / NS_ASSUME_NONNULL_END 包裹，可空处显式标注 nullable
- 禁止在暴露给 Swift 的 API 中使用宏、C 数组等无法桥接的结构

## 注释规范

- 头文件中所有 public 类、属性、方法必须有中文注释（`///` 或 `/** */`），说明用途与注意事项
- 复杂逻辑必须添加中文行内注释
- TODO 注释格式：`// TODO(作者): 具体待办事项描述`
- 禁止无意义注释；注释掉的代码直接删除，历史由 Git 保留

## 格式规范

- 统一使用 4 空格缩进
- 多参数方法声明的冒号垂直对齐
- 单行代码长度不超过 120 字符
- 大括号风格与项目既有代码保持一致（Apple 惯例：方法大括号独立成行，控制语句大括号同行）

## 代码质量强制要求

- 禁止在 ARC 下手写 retain / release / autorelease，dealloc 中禁止调用 [super dealloc]
- 禁止直接访问实例变量（iVar），统一通过属性访问器（init / dealloc 中除外）
- NSNotificationCenter 与 KVO 的观察必须在 dealloc 中移除，避免野指针崩溃
- 禁止魔法值：常量用 static const 或 NS_ENUM 定义
- 外部传入的集合、字符串使用前必须判空，防止数组越界（ObjC 常见崩溃源）
- 头文件依赖最小化：能用 @class 前向声明就不 #import，仅在需要接口细节时导入
- 禁止在性能敏感路径频繁创建临时对象，注意 autoreleased 对象的累积

## 最佳实践

- 新增代码优先 Swift，Objective-C 用于维护既有模块与底层库
- 模块间通过协议解耦，避免头文件相互依赖
- 全量 nullability 标注，降低 Swift 混编时的隐式解包风险
- 遗留代码重构小步提交，以行为不变为前提
- 使用 Instruments 检查内存泄漏与循环引用
