# 机器学习实践平台项目分析报告

202212033024 李煜均

[TOC]

## 1.项目简介

机器学习实践平台是一个用于进行机器学习的测试平台，用户可通过选择不同的数据集及不同的分割方式，机器学习模型，评价指标更客观全面的探寻机器学习的流程和输出。

## 2.项目启动

### 2.1 项目环境

- Window11(X86)
- Anaconda3-Python 3.12

### 2.2 IDE

- Cursor
- Visual Studio Code
- PyCharm 2024.1.7
- WebStorm 2024.1.7

### 2.3 前端启动

```
cd dlframe-front-master

npm install
npm run dev
```

### 2.4 后端启动

```
cd dlframe-back/tests
python Display.py
```



## 3.项目整体架构

### 3.1 前后端分离架构



- 前端: dlframe-front-master (Vue 3 + TypeScript)
- 后端: dlframe-back (Python)
- 通信: WebSocket协议



### 3.2 技术栈

**前端:**

- Vue 3
- TypeScript
- Element Plus
- Vite

**后端:**

- Python
- WebSocket
- NumPy
- Pillow
- Sklearn
- Pytorch



## 4. 后端架构 (dlframe-back)

### 4.1 核心模块

**数据集**



**鸢尾花分类数据集**

> 特征：
>
> 4个数值特征：萼片长度、萼片宽度、花瓣长度、花瓣宽度
>
> 分类：
>
> - Setosa（山鸢尾）
>
> - Versicolor（变色鸢尾）
> - Virginica（维吉尼亚鸢尾）
>
>  参数：
>         无需参数，直接从sklearn加载



**葡萄酒分类数据集**

> 特征：
>
> 13个化学成分指标
>
> 包括酒精度、苹果酸、灰分等
>
> 分类：
>
> 3个不同品种的葡萄酒
>
> 参数：
>         无需参数，直接从sklearn加载
>     



**乳腺癌诊断数据集**

> 特征：
>
> 30个数值特征
>
> 包括细胞核的各种特征测量值
>
> 分类：
>
> - 良性肿瘤
> - 恶性肿瘤
>
> 参数：
>         无需参数，直接从sklearn加载



**天气-活动模型数据集** 

> 特征：
>
> 隐状态：晴天、雨天
>
> 观测状态：散步、购物、清洁
>
> 概率分布：
>
> - 转移概率：天气状态转换概率
> - 发射概率：不同天气下进行各项活动的概率
>
> 参数：
>         sequence_length: 生成序列的长度，默认2000



**高斯混合模型聚类数据集**

> 特征：
>
> 2维特征空间中的点
>
> 来自多个高斯分布的混合
>
> 聚类：
>     - n_components个不同的高斯分布簇
>
> 参数：
>         n_samples: 样本数量，默认300
>         n_components: 高斯分布的数量，默认3
>         random_state: 随机种子，默认42



**癌症基因表达数据集**

> 特征：
>
> 选择2个重要基因的表达水平
>
> 每个基因的表达值范围在0-15之间
>
> 癌症亚型：
>
> - A型：CCND1高表达，CD8A低表达
> - B型：CCND1低表达，CD8A高表达
> - C型：CCND1和CD8A都处于中等水平
>
> 参数:
>         n_samples: 样本数量
>         random_state: 随机种子



**数据分割**

- RandomSplitter
- KFoldSplitter



**机器学习模型**

|           KNN           |    Decision-Tree    |  SVM   | Naive-Bayes | Boosting |
| :---------------------: | :-----------------: | :----: | :---------: | :------: |
| **Logistic Regression** | **Maximum Entropy** | **EM** | **K-MEANS** | **HMM**  |



**评价指标**

- '准确率accuracy'
- '精确率precision'
- '召回率recall'
- 'f1系数'
- 'ROC-AUC曲线'
- '轮廓系数Clustering'
- '天气预报指标'



**数据可视化**

```python
def visualize(self , testDataset)
```



### 4.2 功能模块

计算节点系统 (CalculationNode.py)

通信管理 (WebManager.py)

日志系统 (Logger.py)

性能监控 (TimeWarpper.py)

递归处理 (RecursiveWarpper.py)



## 5. 前端架构 (dlframe-front-master)

![1](img\3.png)

### 5.1 核心组件

```html
<template>
  <el-container class="main-container">
    <!-- 顶部导航栏 -->
    <el-header>...</el-header>
    
    <!-- 主要内容区 -->
    <el-main>
      <!-- 配置面板 -->
      <!-- 结果展示 -->
    </el-main>
  </el-container>
</template>
```



### 5.2 功能模块

**1.WebSocket通信**

```is
const ws = new WebSocket(`ws://${connectUrl}:${connectPort}`)
```

**结果展示**

文本输出

图像显示



**2.配置实验参数功能**

```
interface DeletedButtonsInterface {
  [key: string]: string[];
}
```

**连接图片展示功能**

提供图像输出的开关控制

允许自定义图像显示的最大高度（200px-800px）

当图像输出被禁用时，高度设置也会被禁用

**删除功能**

增加了删除按钮功能

删除的按钮不会被直接销毁，而是保存在 deletedButtons 中

为每个配置分类单独维护已删除按钮列表

**恢复功能**

添加了"恢复按钮"功能，可以找回之前删除的按钮

通过弹出框形式展示可恢复的按钮列表

支持选择性恢复单个按钮

**界面优化**

只有当存在已删除按钮时才显示"恢复按钮"选项

使用 el-popover 组件展示已删除按钮列表

优化了按钮的展示样式和交互效果

3.**数据导出功能**

新增了实验结果导出功能，支持将实验过程中产生的文本和图片结果导出到本地文件。用户可以选择导出文本、图片或全部内容。

```vue
// 在 script 部分添加导出相关方法
import { Download } from '@element-plus/icons-vue'

// 添加导出处理方法
const handleExport = (type: string) => {
  if (runningOutput.value.length === 0) {
    ElMessage.warning('没有可导出的结果')
    return
  }

  switch (type) {
    case 'text':
      exportText()
      break
    case 'image':
      exportImages()
      break
    case 'all':
      exportAll()
      break
  }
}

// 导出文本结果
const exportText = () => {
  const textContent = runningOutput.value
    .filter(item => item.type === 'string')
    .map(item => item.content)
    .join('\n')

  const blob = new Blob([textContent], { type: 'text/plain;charset=utf-8' })
  const url = URL.createObjectURL(blob)
  const link = document.createElement('a')
  link.href = url
  link.download = `实验结果_${new Date().toLocaleString()}.txt`
  document.body.appendChild(link)
  link.click()
  document.body.removeChild(link)
  URL.revokeObjectURL(url)
}

// 导出图片结果
const exportImages = () => {
  const images = runningOutput.value.filter(item => item.type === 'image')
  if (images.length === 0) {
    ElMessage.warning('没有可导出的图片')
    return
  }

  images.forEach((item, index) => {
    const link = document.createElement('a')
    link.href = `data:image/jpeg;base64,${item.content}`
    link.download = `实验图片_${index + 1}.jpg`
    document.body.appendChild(link)
    link.click()
    document.body.removeChild(link)
  })
}

// 导出所有结果
const exportAll = () => {
  exportText()
  exportImages()
}
```



## 6. 通信流程

**连接建立**

```html
// 前端发起连接
connect() {
  ws = new WebSocket(...)
}
```

**数据交互**

```python
# 后端处理
class CSManager:
    def _forward(self, pkt):
        # 消息转发处理
```

**消息类型**

overview: 配置信息

print: 文本输出

imshow: 图像显示



## 7.项目难点

### 7.1 HMM隐马尔可夫模型训练

hmm模型构建初期，存在预测准确率过低的问题，通过修改及调试矩阵参数，更换数据集，优化数据预处理等多种尝试，最终将准确率提升至80%+。

```python
class HMMModel:
    def __init__(self, n_states, n_observations):
        self.n_states = n_states
        self.n_observations = n_observations
        self.logger = Logger.get_logger('HMMModel')
        
        # 初始化模型参数
        self._initialize_parameters()
        
    def _initialize_parameters(self):
        """初始化模型参数"""
        # 使用更好的初始化策略
        self.A = np.random.dirichlet(np.ones(self.n_states) * 10, size=self.n_states)
        self.B = np.random.dirichlet(np.ones(self.n_observations) * 10, size=self.n_states)
        self.pi = np.random.dirichlet(np.ones(self.n_states) * 10)
        
    def _normalize(self, matrix):
        """归一化矩阵，避免数值下溢"""
        return matrix / (np.sum(matrix, axis=1, keepdims=True) + 1e-12)
    
    def forward(self, observations):
        """前向算法的向量化实现"""
        T = len(observations)
        alpha = np.zeros((T, self.n_states))
        scaling_factors = np.zeros(T)
        
        # 初始化
        alpha[0] = self.pi * self.B[:, observations[0]]
        scaling_factors[0] = np.sum(alpha[0])
        alpha[0] /= scaling_factors[0]
        
        # 递归计算
        for t in range(1, T):
            alpha[t] = np.dot(alpha[t-1], self.A) * self.B[:, observations[t]]
            scaling_factors[t] = np.sum(alpha[t])
            alpha[t] /= scaling_factors[t]
            
        return alpha, scaling_factors
    
    def backward(self, observations, scaling_factors):
        """后向算法的向量化实现"""
        T = len(observations)
        beta = np.zeros((T, self.n_states))
        
        # 初始化
        beta[-1] = 1 / scaling_factors[-1]
        
        # 递归计算
        for t in range(T-2, -1, -1):
            beta[t] = np.dot(self.A, (self.B[:, observations[t+1]] * beta[t+1]))
            beta[t] /= scaling_factors[t]
            
        return beta
    
    def train(self, trainDataset):
        """训练模型的包装方法"""
        try:
            # 提取观测序列
            observations = np.array([x[0] for x in trainDataset])
            
            # 训练型
            return self.train_internal(observations)
            
        except Exception as e:
            self.logger.print(f"训练过程中出现错误: {str(e)}")
            import traceback
            self.logger.print(traceback.format_exc())
            
    
    def _compute_xi(self, observations, alpha, beta):
        """计算xi矩阵"""
        T = len(observations)
        xi = np.zeros((T-1, self.n_states, self.n_states))
        
        for t in range(T-1):
            numerator = (alpha[t].reshape(-1, 1) * self.A * 
                        self.B[:, observations[t+1]].reshape(1, -1) * 
                        beta[t+1].reshape(1, -1))
            xi[t] = numerator / np.sum(numerator)
            
        return xi
        
    def _update_parameters(self, observations, gamma, xi):
        """更新模型参数"""
        # 更新初始概率
        self.pi = gamma[0]
        
        # 更新转移概率
        self.A = np.sum(xi, axis=0) / np.sum(gamma[:-1], axis=0).reshape(-1, 1)
        
        # 更新发射概率
        for j in range(self.n_states):
            for k in range(self.n_observations):
                self.B[j,k] = np.sum(gamma[observations == k, j]) / np.sum(gamma[:, j])
                
        # 归一化参数
        self.A = self._normalize(self.A)
        self.B = self._normalize(self.B)
        
    def predict(self, observations):
        """使用维特比算法进行预测"""
        T = len(observations)
        delta = np.zeros((T, self.n_states))
        psi = np.zeros((T, self.n_states), dtype=int)
        
        # 初始化
        delta[0] = np.log(self.pi) + np.log(self.B[:, observations[0]])
        
        # 递归
        for t in range(1, T):
            for j in range(self.n_states):
                temp = delta[t-1] + np.log(self.A[:, j])
                delta[t,j] = np.max(temp) + np.log(self.B[j, observations[t]])
                psi[t,j] = np.argmax(temp)
        
        # 回溯
        states = np.zeros(T, dtype=int)
        states[-1] = np.argmax(delta[-1])
        for t in range(T-2, -1, -1):
            states[t] = psi[t+1, states[t+1]]
            
        return states
    
```

### 7.2 数据可视化的传输

在创建数据可视化的过程中，由于self.logger.imshow()不支持matplotlib图像的直接传入，造成了无法在网页显示图像的情况，后续通过将matplotlib图像转换为np.array()数组解决了这一情况（下图以ROC-AUC曲线绘制为例）

```python
        # 绘制对角线
        plt.plot([0, 1], [0, 1], color='navy', lw=2, linestyle='--')
        plt.xlim([0.0, 1.0])
        plt.ylim([0.0, 1.05])
        plt.xlabel('假正率 (False Positive Rate)')
        plt.ylabel('真正率 (True Positive Rate)')
        plt.title('接收者操作特征(ROC)曲线')
        plt.legend(loc="lower right")
        
        # 将图像转换为数组格式
        buf = BytesIO()
        plt.savefig(buf, format='png', dpi=100, bbox_inches='tight')
        buf.seek(0)
        
        img = Image.open(buf)
        img = img.convert('RGB')
        img_array = np.array(img)
        
        plt.close()
        buf.close()
        
        # 使用logger显示图像数组
        self.logger.imshow(img_array)
```

### 7.3 前后端通信实现

通过WebSocket，实现了前后端通信的互联，将前端展示在http://localhost:3000/同时监听http://localhost:8765/的后端端口

```vue
const connect = () => {
  ws = new WebSocket('ws://' + connectUrl.value + ':' + connectPort.value)
  ws.onopen = () => {
    isConnectedToServer.value = true
    showConnectInfoWindow.value = false
    ElMessage({
      message: '连接成功',
      type: 'success',
    })
    ws?.send(JSON.stringify({
      'type': 'overview', 
      'params': {}
    }))
  }
```

```python
class WebManager(CalculationNodeManager):
    def __init__(self, host='0.0.0.0', port=8765, parallel=False) -> None:
        super().__init__(parallel=parallel)
        self.host = host
        self.port = port
```

### 7.4 前端界面优化

| 原前端界面     | 优化后前端界面 | 二次优化前端界面 |
| -------------- | -------------- | ---------------- |
| ![](img\2.png) | ![](img\1.png) | ![](img\3.png)   |

**组件结构**：

- 使用了 `<template>` 标签包裹所有的模板代码，定义了组件的结构和布局。

**顶部导航栏**：

- `<el-header>`：这是 Element Plus 组件库中的一个组件，用于创建页面的头部。
- `<div class="header-content">`：这个 div 包含了logo和连接状态信息。
- `<img src="/logo.ico" alt="Logo" class="logo">`：显示了一个图标，作为平台的标识。
- `<h1>机器学习实践平台</h1>`：页面的主标题，描述了这个平台的功能。
- `<el-tag>`：在这里用来显示当前的连接状态，包含连接的URL和端口。

**连接状态**：

- 连接状态通过 `{{connectUrl}}:{{connectPort}}` 动态绑定到数据模型，实时更新连接信息。
- `<el-icon><Connection /></el-icon>`：提供一个图标显示连接状态，`Connection` 是一个从 Element Plus 图标库引入的图标组件。

**左侧配置面板**：

- `<el-col :span="8">`：该列占据总宽度的 8 个单位（在 24 格布局中），用于显示实验配置的内容。
- `<el-card>`：卡片组件，提供了一个可用来显示实验配置的区域。

**卡片头部**：

- 使用了带有插槽的 `<template #header>` 定义了卡片的头部内容。包含了一些有关实验配置的标题和工具提示。
- `<el-tooltip>`：为设置图标提供了一个工具提示，描述了图标的功能（配置实验参数）。

### 7.5 解决异步循环出错

```
runtimeerror: task <task pending name='task-9' coro=<sendsocket.sendworker() running at c:\programdata\anaconda3\lib\site-packages\dlframe\webmanager.py:26> cb=[_run_until_complete_cb() at c:\programdata\anaconda3\lib\asyncio\base_events.py:182]> got future <future pending> attached to a different loop
```

当我们使用异步编程时，每个asyncio任务都会绑定到一个事件循环（Event Loop），始终在相同的事件循环上运行。当一个Future对象（例如一个awaitable函数）在一个事件循环上创建，然后在另一个事件循环上被执行时，就会出现异常 “got Future attached to a different loop”。

**1. 线程分离**

新增了 run_in_new_thread 函数，将所有模型训练和评估的代码独立出来

使用 threading.Thread 在单独的线程中运行计算密集型任务

主线程保留用于处理 WebManager 的异步操作

**2. 错误处理优化**

在 run_in_new_thread 函数中添加了异常捕获

提供更清晰的错误信息输出

**3. 线程同步**

使用 training_thread.join() 确保训练线程完成后再关闭事件循环

保证了资源的正确释放

```python
if __name__ == '__main__':
    # 创建新的事件循环
    loop = asyncio.new_event_loop()
    asyncio.set_event_loop(loop)
    
    try:
        with WebManager(host='0.0.0.0', port=8765) as manager:
            # 在新线程中运行模型训练和评估
            training_thread = threading.Thread(target=run_in_new_thread, args=(manager,))
            training_thread.start()
            
            # 等待训练线程完成
            training_thread.join()
    finally:
        loop.close()
```



## 8. 特色功能

### 8.1 实时数据可视化

支持文本和图像实时显示

动态更新实验结果

### 8.2 灵活的配置系统

动态加载实验参数

实时验证配置有效性

### 8.3 多种数据集支持

标准机器学习数据集

### 8.4 前端界面的个性化操作

自由选择删除无关参数

自由配置可视化



## 9. 项目优势

### 9.1 架构设计

- 清晰的模块划分
- 良好的扩展性
- 松耦合的组件设计

### 9.2 用户体验

- 直观的界面设计

- 实时反馈

- 错误提示

- 功能完整性

- 支持多种实验场景

- 完整的数据处理流程

- 数据可视化的展现

### 9.3 线程分离

- 解决了事件循环冲突问题
- 提高了代码的可维护性
- 保证了 WebManager 的正常运行
- 避免了模型训练阻塞 WebSocket 通信



## 10. 改进要求

### 10.1 后端改进

- 添加数据持久化
- 实现可视化展示与否选择
- 增加并发处理能力
- 完善错误处理机制
- 隐马尔可夫试验进一步优化

### 10.2 前端改进

- 优化数据展示
- 实现自定义数据集上传
- 实现数据分割系数自定义
- 增强响应式设计

### 10.3 整体改进

- 添加用户认证系统
- 支持更多数据格式
- 优化通信协议
- 优化异步运行

## 附录

### 工程实现时间线

| 时间          | 完成工作                                                     |
| ------------- | ------------------------------------------------------------ |
| 11周（11.14） | 实现基本机器学习模型的训练测试功能                           |
| 12周（11.21） | 完善HMM和EM、K-means模型；添加评价指标；数据可视化功能实现   |
| 13周（11.28） | 继续优化HMM模型，提高准确度；前端界面的优化；前后端互联功能实现 |
| 14周（12.05） | 数据可视化功能优化；尝试将可视化功能独立成单独模块（未实现）；继续优化前端界面，添加动画效果 |
| 15周（12.12） | 解决异步循环导致软件运行出错；添加前端实验配置功能；添加前端数据导出功能 |
| 16周（12.19） | 撰写工程报告；完善项目细节                                   |

