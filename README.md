# 实验2 Android界面布局

**福建师范大学 · Android开发实验**

## 一、实验目的

1. 掌握 Android 常用布局：`LinearLayout`、`TableLayout`、`ConstraintLayout`
2. 学会用 XML 描述界面结构
3. 学习 Jetpack Compose 声明式 UI 编程
4. 掌握 Git 与 GitHub 协作，完成代码版本管理

## 二、开发环境

| 项目 | 版本 |
|------|------|
| Android Studio | Quail 4 (2026.1.4) |
| Kotlin | 2.2 |
| Compile SDK | 36 (Android 16) |
| Minimum SDK | 24 (Android 7.0) |
| 操作系统 | Windows 11 |

## 三、实验内容

### 3.1 线性布局 LinearLayout

**目标**：实现 4×4 网格，每格黑底白字，显示 `One,One` 到 `Four,Four`。

**实现要点**：
- 外层垂直 `LinearLayout` 分 4 行
- 每行水平 `LinearLayout`，4 个 `TextView` 用 `layout_weight="1"` 等宽
- 文字颜色 `#FFFFFF`，背景 `#000000`

**关键代码**：

```xml
<LinearLayout
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:orientation="horizontal">

    <TextView
        android:layout_width="0dp"
        android:layout_height="wrap_content"
        android:layout_weight="1"
        android:background="#000000"
        android:gravity="center"
        android:text="@string/one_one"
        android:textColor="#FFFFFF" />
</LinearLayout>
```

**截图**：
<img width="1410" height="416" alt="image" src="https://github.com/user-attachments/assets/4241019f-83c5-4a30-898d-a01c5af41b9a" />


---

### 3.2 表格布局 TableLayout

**目标**：模拟菜单界面 `Hello TableLayout`，包含 Open、Save、Save As、Import、Export、Quit 等项。

**实现要点**：
- `TableLayout` + `TableRow`
- `stretchColumns="0,1"` 让两列都拉伸
- 快捷键右对齐（`gravity="end"`）
- 分隔线用 `<View>` 实现

**关键代码**：

```xml
<TableLayout
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:background="#1E1E1E"
    android:stretchColumns="0,1">

    <TableRow android:padding="10dp">
        <TextView
            android:layout_width="0dp"
            android:layout_weight="1"
            android:text="Open..."
            android:textColor="#FFFFFF" />
        <TextView
            android:layout_width="0dp"
            android:layout_weight="1"
            android:gravity="end"
            android:text="Ctrl-O"
            android:textColor="#FFFFFF" />
    </TableRow>
</TableLayout>
```

**截图**：
<img width="1114" height="934" alt="image" src="https://github.com/user-attachments/assets/bf1fec86-9f24-4c20-a823-9f08a9c8bc68" />


---

### 3.3 约束布局 ConstraintLayout（计算器）

**目标**：实现一个计算器界面，包含输入显示区和 4×4 数字/运算符按钮。

**实现要点**：
- 用 `ConstraintLayout` 的**链式约束**（Chain）让 4 列等宽
- 顶部标题栏绿色 `#009688`
- 显示区黄绿色 `#C5C28C`
- 按钮浅灰 `#DDDDDD`

**关键代码**：

```xml
<TextView
    android:id="@+id/btn7"
    android:layout_width="0dp"
    android:layout_height="48dp"
    android:background="#DDDDDD"
    android:gravity="center"
    android:text="7"
    app:layout_constraintEnd_toStartOf="@id/btn8"
    app:layout_constraintHorizontal_chainStyle="spread"
    app:layout_constraintStart_toStartOf="parent"
    app:layout_constraintTop_toBottomOf="@id/tvDisplay" />
```

**截图**：
<img width="986" height="805" alt="image" src="https://github.com/user-attachments/assets/096276d5-b469-4341-a4f9-7ac61b857032" />


---

### 3.4 约束布局 ConstraintLayout（太空订票）

**目标**：实现 `Space Stations / Flights / Rovers` 三个 Tab 和 DCA ↔ MARS 订票卡片。

**实现要点**：
- 顶部 3 个 Tab 用 `LinearLayout` + `layout_weight="1"` 均分
- 绿色卡片 `#1E5B3E`，中间用白色方块作为"交换"按钮
- One Way 和 1 Traveller 用橙色 `#C2672A` 的窄条
- 底部 DEPART 按钮通栏

**关键代码**：

```xml
<TextView
    android:id="@+id/tvDCA"
    android:layout_width="0dp"
    android:layout_height="100dp"
    android:background="#1E5B3E"
    android:gravity="center"
    android:text="DCA"
    android:textColor="#FFFFFF"
    app:layout_constraintEnd_toStartOf="@id/tvMARS"
    app:layout_constraintStart_toStartOf="parent"
    app:layout_constraintTop_toBottomOf="@id/tabDivider" />
```

**截图**：
<img width="1090" height="928" alt="image" src="https://github.com/user-attachments/assets/dcd1a845-3e18-48b9-82b2-4a2b4311d096" />


---

### 3.5 Compose 任务列表

**目标**：用 **Jetpack Compose** 实现"课程学习任务"App：
- 初始 3 项任务，完成 1 项
- 添加任务（输入"复习 LazyColumn"）
- 勾选任务后，完成数与删除线同步更新

**实现要点**：
- 使用 `mutableStateListOf<Task>` 保存任务列表
- 用 `LazyColumn` 展示列表
- 勾选改变时，通过 `copy(isCompleted = checked)` 更新状态
- 删除线用 `TextDecoration.LineThrough`
- 主题色 `#C62828`（红色）

**关键代码**：

```kotlin
val tasks = remember {
    mutableStateListOf(
        Task(1, "学习 Column 和 Row", true),
        Task(2, "学习状态管理", false),
        Task(3, "完成 Compose 实验", false)
    )
}

val completedCount = tasks.count { it.isCompleted }

LazyColumn {
    items(tasks, key = { it.id }) { task ->
        TaskItem(
            task = task,
            onCheckedChange = { checked ->
                val index = tasks.indexOfFirst { it.id == task.id }
                tasks[index] = tasks[index].copy(isCompleted = checked)
            },
            onDelete = { tasks.remove(task) }
        )
    }
}
```

**截图**：
<img width="720" height="1195" alt="image" src="https://github.com/user-attachments/assets/5b3a1987-4fc4-4f76-8087-f6fe5fc641e7" />
<img width="579" height="1027" alt="image" src="https://github.com/user-attachments/assets/fac6cfbf-4305-4a01-a610-f057d2e05c23" />
<img width="580" height="1144" alt="image" src="https://github.com/user-attachments/assets/9e03fc16-1bf4-4426-b5dd-0883abc7164a" />




---

## 四、项目结构

```
task02/                              GitHub 仓库
├── LayoutTask/                      实验2 XML布局代码
│   ├── app/
│   │   └── src/main/
│   │       ├── java/cse/fjnu/test02/
│   │       │   └── MainActivity.kt
│   │       ├── res/
│   │       │   ├── drawable/
│   │       │   └── layout/
│   │       │       ├── activity_main.xml
│   │       │       ├── activity_table.xml
│   │       │       ├── activity_constraint1.xml
│   │       │       └── activity_constraint2.xml
│   │       └── AndroidManifest.xml
│   ├── build.gradle.kts
│   └── settings.gradle.kts
│
└── ComposeTask/                     实验2 Compose代码
    ├── app/
    │   └── src/main/java/cse/fjnu/composetask/
    │       └── MainActivity.kt
    └── build.gradle.kts
```

## 五、遇到的问题与解决

| 问题 | 解决方法 |
|------|----------|
| Gradle 依赖下载缓慢 | 在 `settings.gradle.kts` 配置阿里云镜像 |
| GitHub 推送认证失败 | 生成 Personal Access Token 代替密码 |
| 硬编码文本警告 | 使用 `strings.xml` 管理所有文本 |
| 颜色对比度不足 | 深色背景使用 `#FFFFFF` 文字 |
| `edgeToEdge` 导致界面黑屏 | 删除 `enableEdgeToEdge()` |

## 六、实验总结

通过本次实验，我掌握了：

1. **三种经典布局**：`LinearLayout` 适合线性排列，`TableLayout` 适合表格，`ConstraintLayout` 性能最好、最灵活
2. **链式约束**：用 `ChainStyle` 让多个控件均匀分布
3. **Jetpack Compose**：声明式 UI 编程，状态管理自动驱动 UI 更新
4. **Git 版本控制**：本地提交、远程推送、Token 认证

---

## 七、提交信息

- **仓库地址**：https://github.com/cxb051209/task02
- **提交人**：cxb051209
- **完成日期**：2026-09-27
