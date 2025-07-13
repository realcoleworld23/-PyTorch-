# 慧眼识真——植物病虫害与毒蘑菇智能识别平台

## 项目简介

这是一个基于Flask和PyTorch的智能识别Web应用，支持：
- **植物病虫害识别**：使用Grad-CAM可视化技术，展示模型关注区域
- **毒蘑菇识别**：集成大模型API自动生成科普信息
- **现代化界面**：支持拖拽上传、实时预览、无跳转结果展示

## 系统要求

- Python 3.8+
- Windows 10/11 或 Linux/macOS
- 至少4GB内存
- 支持CUDA的显卡（可选，用于加速推理）

## 安装步骤

### 1. 克隆项目
```bash
git clone <项目地址>
cd ICT
```

### 2. 创建虚拟环境（推荐）
```bash
# Windows
python -m venv venv
venv\Scripts\activate

# Linux/macOS
python3 -m venv venv
source venv/bin/activate
```

### 3. 安装依赖
```bash
pip install -r requirements.txt
```

**注意**：如果`requirements.txt`格式有问题，请手动安装以下依赖：
```bash
pip install torch torchvision
pip install flask pillow numpy opencv-python matplotlib
pip install requests websocket-client
pip install scikit-learn tqdm
```
或者是直接
```bash
pip install "torch>=1.12.0" \
            "torchvision>=0.13.0" \
            "flask>=2.0.0" \
            "pillow>=9.0.0" \
            "numpy>=1.21.0" \
            "opencv-python>=4.5.0" \
            "matplotlib>=3.5.0" \
            "requests>=2.25.0" \
            "websocket-client>=1.0.0" \
            "scikit-learn>=1.0.0" \
            "tqdm>=4.64.0" \
            "werkzeug>=2.0.0"
```

## 路径配置
首先您需要下载realease中的web_demo文件夹“ICT”,以及毒蘑菇识别模型的权重pth文件，植物病害识别模型的权重pth文件。
以下的文件都存在于web_demo文件夹“ICT”中。

要使用web_demo必须下载github release中的web_demo文件夹，里面会有demo文件ICT.zip
（github release中也有两个模型权重文件，可直接下载，也可只下载web_demo文件夹中的ICT.zip，里面也包含有两个模型权重文件）

## 文件结构说明

```
ICT/
├── app.py                 # 主应用文件
├── config.py             # API配置文件
├── llm_api.py            # 大模型API模块
├── GRAD_CAM.py           # Grad-CAM可视化模块
├── requirements.txt      # 依赖包列表
├── static/              # 静态文件目录
│    ├── uploads/         # 上传图片存储
│    └── results/         # 结果图片存储
├── templates/           # HTML模板
│    ├── index.html       # 主页模板
│    └── result.html      # 结果页模板
└── mushroom_predict/    # 蘑菇识别模块
|    ├── mushroom.py      # 蘑菇识别核心代码
|    ├── best_se_resnet50_mushroom14.pth  # 蘑菇模型权重
|    └── class.json       # 蘑菇类别映射
|___plantdisease_predict/ #植物病害识别模块
     |__clase_index.json。 #植物病害类别映射
     |__se_resnet50_new.pth  #植物病害模型权重
```

### 必须修改的文件路径

#### 1. `app.py` - 主应用文件
需要修改以下路径（第25-27行和第118-120行）：

```python
# 植物病害模型路径（第25-27行）
MODEL_PATH = "你的项目路径/se_resnet50_new.pth"
CLASS_INDEX_PATH = "你的项目路径/class_index.json"

# 蘑菇识别模型路径（第118-120行）
model_path = "你的项目路径/mushroom_predict/best_se_resnet50_mushroom14.pth"
class_json_path = "你的项目路径/mushroom_predict/class.json"
```

**示例**（假设项目在`C:\Users\YourName\Desktop\ICT`）：
```python
MODEL_PATH = "C:/Users/YourName/Desktop/ICT/se_resnet50_new.pth"
CLASS_INDEX_PATH = "C:/Users/YourName/Desktop/ICT/class_index.json"

model_path = "C:/Users/YourName/Desktop/ICT/mushroom_predict/best_se_resnet50_mushroom14.pth"
class_json_path = "C:/Users/YourName/Desktop/ICT/mushroom_predict/class.json"
```

#### 2. `config.py` - API配置文件
配置大模型API密钥（可选，不配置会使用本地字典）：

```python
# 百度文心一言API配置
BAIDU_API_KEY = "你的百度API Key"
BAIDU_SECRET_KEY = "你的百度Secret Key"

# 其他API配置...
DEFAULT_LLM = "baidu"  # 默认使用百度API
```



## 运行应用

### 1. 启动服务器(在相应的web_demo文件夹中打开终端窗口）
```bash
python app.py
```
成功后请勿关闭终端。

### 2. 访问应用
打开浏览器，访问：`http://localhost:5000`

### 3. 使用说明
- **左侧**：植物病虫害识别
  - 上传植物图片
  - 获得病害识别结果和Grad-CAM热力图
- **右侧**：毒蘑菇识别
  - 上传蘑菇图片
  - 获得识别结果和科普信息

## 功能特性

### 植物病虫害识别
- 支持38种植物病害分类
- Grad-CAM热力图可视化
- 实时识别，无需页面跳转

### 毒蘑菇识别
- 支持156种蘑菇分类
- 大模型API自动生成科普信息
- 本地字典兜底机制

### 用户体验
- 拖拽上传支持
- 图片实时预览
- 加载动画
- 响应式设计

## 常见问题

### Q1: 模型文件找不到
**A**: 确保以下文件存在且路径正确：
- `se_resnet50_new.pth`（植物病害模型）
- `class_index.json`（植物病害类别）
- `mushroom_predict/best_se_resnet50_mushroom14.pth`（蘑菇模型）
- `mushroom_predict/class.json`（蘑菇类别）

### Q2: 依赖包安装失败
**A**: 尝试以下解决方案：
```bash
# 升级pip
python -m pip install --upgrade pip

# 使用国内镜像
pip install -i https://pypi.tuna.tsinghua.edu.cn/simple/ torch torchvision

# 或使用conda
conda install pytorch torchvision -c pytorch
```

### Q3: API调用失败
**A**: 
1. 检查`config.py`中的API密钥配置
2. 确保网络连接正常
3. 系统会自动使用本地字典兜底

### Q4: 端口被占用
**A**: 修改`app.py`最后一行：
```python
app.run(debug=True, port=5001)  # 使用其他端口
```

### Q5: 内存不足
**A**: 
1. 关闭其他程序释放内存
2. 使用CPU模式（默认已配置）
3. 减少同时上传的图片数量

## 开发说明

### 添加新的植物病害类别
1. 修改`class_index.json`
2. 重新训练模型
3. 更新`NUM_CLASSES`变量

### 添加新的蘑菇类别
1. 修改`mushroom_predict/class.json`
2. 重新训练模型
3. 更新`num_classes`参数

### 自定义API配置
参考`README_LLM_API.md`文档，了解如何配置不同的大模型API。

## 技术支持

如遇到问题，请检查：
1. Python版本是否为3.8+
2. 所有依赖包是否正确安装
3. 文件路径是否正确配置
4. 模型文件是否存在

---

**注意**：AI识别结果仅供参考，特别是蘑菇识别，请勿仅凭此结果判断是否可食用，务必咨询专业人士！ 
