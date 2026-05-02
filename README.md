# gaps

从<https://github.com/nemanja-m/gaps>抄的，稍微用uv重构了一下，方便uvx或pipx，存档备份  

基于遗传算法的拼图求解器，支持自动检测拼图块大小  

## 安装

克隆仓库：

```bash
git clone https://github.com/YC-CLT/gaps.git
cd gaps
```

在本地安装项目：

```bash
uv tool install -e . 
```

或

```bash
pipx install .
```

完成之后就应该就可以直接使用 `gaps` 命令了  

如果需要安装依赖：

```bash
uv sync
```

## 从图片创建拼图

使用 `gaps create` 命令从图片创建拼图：

```bash
gaps create images/pillars.jpg puzzle.jpg --size=64
```

这将从 `images/pillars.jpg` 创建一个包含 240 个拼图块的拼图，每个拼图块大小为 64x64 像素。

运行 `gaps create --help` 查看详细帮助信息。

**注意**：根据给定的拼图块大小，创建的拼图图像尺寸可能小于原始图像。系统会从原始图像中裁剪最大可能的矩形区域  

## 求解拼图

使用 `gaps run` 命令求解拼图：

```bash
gaps run puzzle.jpg solution.jpg --generations=20 --population=600
```

这将启动遗传算法，初始种群大小为 600，迭代 20 代。

提供以下选项：

选项 | 描述
--- | ---
`--size` | 拼图块大小（像素）
`--generations` | 遗传算法的代数
`--population` | 种群中的个体数量
`--debug` | 每代结束后显示最佳解

运行 `gaps run --help` 查看详细帮助信息。

### 尺寸检测

如果不显式提供 `--size` 参数，系统会自动检测拼图块大小。

当然，你也可以显式指定 `--size` 参数：

```bash
gaps run puzzle.jpg solution.jpg --generations=20 --population=600 --size=48
```

**注意**：尺寸检测功能适用于大多数图像，但在某些边缘情况下可能会检测到错误的拼图块大小。此时可以手动指定拼图块大小。

### 终止条件

遗传算法的终止条件对于决定 GA 运行何时结束非常重要。观察发现，GA 初期进展非常快，每隔几次迭代就会出现更好的解，但在后期阶段改进会变得非常小  

`gaps` 将在以下情况下终止：

- 种群在 `X` 次迭代中没有改进，或者
- 达到指定的代数上限
