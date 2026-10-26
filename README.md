深度学习框架YOLOV11火灾检测系统智能预警，守护安全
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/5d21772990594180855828fae5349f54.png)

**技术栈：
YOLOv11深度学习框架
Pycharm + Anaconda开发工具
Python 3.11
opencv-python
torch==2.7.1
pyqt5前端界面
✅️测试无bug 稳定运行
功能特点：
多赛道适用：适用于人工智能+赛道、智能制造赛道、消防安全赛道、工业互联网赛道、智慧城市赛道
先进技术：基于YOLOv11深度学习框架的火灾检测系统
全面检测能力：支持图片、视频和摄像头实时检测，界面友好，检测精度高，运行速度快
灵活扩展：支持自定义模型更换，可轻松扩展其他安全检测任务
完善日志系统：带有日志终端输出功能方便调试，日志自动保存，便于问题追踪和系统维护
精准定位：能记录目标对象像素位置，为后续分析提供精确数据支持
智能预警：能自动显示检测到的火灾风险，及时发出警报
适用场景：
工厂仓库监控：实时监测生产区域火灾风险
商业场所安全：商场、写字楼等公共场所防火监控
住宅小区防护：社区消防安全智能化管理
森林防火预警：大面积区域火灾早期发现**

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/d0b49934558e406ab9562e25f3e67208.png)
1
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/c5d6e5bc2da04a44913a835554a7d6e7.png)
# YOLO火灾检测系统 - 完整实现代码

基于YOLOv11的完整火灾检测系统实现代码。以下是系统的模块化实现：

## 1. 项目结构
```
YOLO火灾检测系统/
├── assets/                 # 资源文件
├── config/                 # 配置文件
├── src/                    # 源代码
├── requirements.txt        # 依赖库
├── install.py             # 安装脚本
└── main.py                # 主程序
```

## 2. requirements.txt
```txt
# 深度学习框架
torch==2.7.1
torchvision==0.22.1
ultralytics==8.2.0

# 图像处理
opencv-python==4.9.0.80
Pillow==10.2.0
numpy==1.24.4

# GUI界面
PyQt5==5.15.10
PyQt5-Qt5==5.15.2
PyQt5-sip==12.13.0

# 其他工具
matplotlib==3.8.0
pandas==2.1.4
seaborn==0.13.0
tqdm==4.66.1
loguru==0.7.2
```

## 3. install.py (一键安装脚本)
```python
#!/usr/bin/env python3
# -*- coding: utf-8 -*-
"""
YOLO火灾检测系统 - 一键安装脚本
"""

import subprocess
import sys
import os
from pathlib import Path
import platform

def check_python_version():
    """检查Python版本"""
    if sys.version_info < (3, 8):
        print("❌ 需要Python 3.8或更高版本")
        sys.exit(1)
    print(f"✅ Python版本: {sys.version_info.major}.{sys.version_info.minor}.{sys.version_info.micro}")

def install_requirements():
    """安装依赖包"""
    print("🔧 正在安装依赖包...")
    
    # 升级pip
    subprocess.check_call([sys.executable, "-m", "pip", "install", "--upgrade", "pip"])
    
    # 根据requirements.txt安装
    requirements_file = Path("requirements.txt")
    if requirements_file.exists():
        subprocess.check_call([sys.executable, "-m", "pip", "install", "-r", str(requirements_file)])
    else:
        # 如果文件不存在，直接安装核心依赖
        core_packages = [
            "torch==2.7.1",
            "torchvision==0.22.1",
            "opencv-python==4.9.0.80",
            "PyQt5==5.15.10",
            "ultralytics==8.2.0"
        ]
        for package in core_packages:
            subprocess.check_call([sys.executable, "-m", "pip", "install", package])
    
    print("✅ 依赖包安装完成")

def setup_directories():
    """创建必要的目录结构"""
    directories = [
        "assets",
        "config",
        "models",
        "logs",
        "data/images",
        "data/videos",
        "outputs/detections",
        "outputs/logs"
    ]
    
    for dir_path in directories:
        os.makedirs(dir_path, exist_ok=True)
        print(f"📁 创建目录: {dir_path}")
    
    print("✅ 目录结构创建完成")

def download_model():
    """下载预训练模型（如果可用）"""
    import requests
    from tqdm import tqdm
    
    model_url = "https://github.com/ultralytics/assets/releases/download/v0.0.0/yolo11n.pt"
    model_path = Path("models/yolo11n.pt")
    
    if not model_path.exists():
        print("📥 正在下载YOLOv11模型...")
        try:
            response = requests.get(model_url, stream=True)
            total_size = int(response.headers.get('content-length', 0))
            
            with open(model_path, 'wb') as file, tqdm(
                desc="下载进度",
                total=total_size,
                unit='iB',
                unit_scale=True
            ) as bar:
                for data in response.iter_content(chunk_size=1024):
                    size = file.write(data)
                    bar.update(size)
            print("✅ 模型下载完成")
        except Exception as e:
            print(f"⚠️ 模型下载失败: {e}")
            print("请手动下载模型并放入models/目录")

def main():
    """主安装函数"""
    print("=" * 50)
    print("YOLO火灾检测系统 - 安装程序")
    print("=" * 50)
    
    try:
        # 检查Python版本
        check_python_version()
        
        # 创建目录
        setup_directories()
        
        # 安装依赖
        install_requirements()
        
        # 下载模型
        download_model()
        
        print("\n" + "=" * 50)
        print("✅ 安装完成！")
        print("运行以下命令启动系统：")
        print("python main.py")
        print("=" * 50)
        
    except Exception as e:
        print(f"\n❌ 安装过程中出错: {e}")
        sys.exit(1)

if __name__ == "__main__":
    main()
```

## 4. config/config.yaml (配置文件)
```yaml
# YOLO火灾检测系统配置文件

# 模型配置
model:
  # 模型文件路径
  path: "models/yolo11n.pt"
  # 输入图像尺寸
  imgsz: 640
  # 置信度阈值
  conf: 0.25
  # IOU阈值
  iou: 0.45
  # 设备 (cuda:0, cpu)
  device: "cuda:0"

# 检测类别
classes:
  fire: 0
  smoke: 1

# 界面配置
ui:
  window_size: [1200, 800]
  theme: "dark"  # dark, light
  language: "zh"  # zh, en

# 日志配置
logging:
  level: "INFO"
  file_path: "logs/system.log"
  max_size: "10MB"  # 日志文件最大大小
  retention: "30 days"  # 日志保留时间

# 报警配置
alert:
  enabled: true
  min_confidence: 0.5
  cooldown_time: 10  # 报警冷却时间（秒）
  # 报警方式
  methods:
    sound: true
    notification: true
    email: false
  # 邮件报警配置
  email:
    smtp_server: ""
    smtp_port: 587
    sender: ""
    password: ""
    receivers: []

# 保存配置
output:
  save_detections: true
  save_path: "outputs/detections/"
  image_quality: 95
  save_format: "jpg"  # jpg, png
  save_logs: true
  log_path: "outputs/logs/"

# 实时检测配置
realtime:
  fps: 30
  buffer_size: 10
  show_fps: true
  show_confidence: true
  show_class: true
```

## 5. src/detector.py (核心检测模块)
```python
#!/usr/bin/env python3
# -*- coding: utf-8 -*-
"""
YOLOv11火灾检测器核心模块
"""

import cv2
import torch
import numpy as np
from pathlib import Path
from typing import List, Tuple, Dict, Optional, Union
from dataclasses import dataclass
from enum import Enum
import time
from loguru import logger
from ultralytics import YOLO
from ultralytics.engine.results import Results

class DetectionClass(Enum):
    """检测类别枚举"""
    FIRE = 0
    SMOKE = 1

@dataclass
class Detection:
    """检测结果数据结构"""
    class_id: int
    class_name: str
    confidence: float
    bbox: List[int]  # [x1, y1, x2, y2]
    center: Tuple[int, int]  # 中心点坐标
    timestamp: float

@dataclass
class DetectionResult:
    """检测结果包装类"""
    image: np.ndarray
    detections: List[Detection]
    stats: Dict[str, any]
    processing_time: float

class FireSmokeDetector:
    """火灾烟雾检测器"""
    
    def __init__(self, model_path: str, conf_threshold: float = 0.25, iou_threshold: float = 0.45):
        """
        初始化检测器
        
        Args:
            model_path: 模型文件路径
            conf_threshold: 置信度阈值
            iou_threshold: IOU阈值
        """
        self.model_path = model_path
        self.conf_threshold = conf_threshold
        self.iou_threshold = iou_threshold
        self.device = self._get_device()
        self.model = None
        self.class_names = {0: "fire", 1: "smoke"}
        
        # 统计信息
        self.stats = {
            "total_frames": 0,
            "total_fire": 0,
            "total_smoke": 0,
            "avg_processing_time": 0.0
        }
        
        self._load_model()
        
    def _get_device(self) -> str:
        """获取可用设备"""
        if torch.cuda.is_available():
            device = "cuda:0"
            logger.info(f"使用GPU: {torch.cuda.get_device_name(0)}")
        else:
            device = "cpu"
            logger.info("使用CPU")
        return device
    
    def _load_model(self):
        """加载YOLO模型"""
        try:
            logger.info(f"加载模型: {self.model_path}")
            self.model = YOLO(self.model_path)
            logger.success("模型加载成功")
        except Exception as e:
            logger.error(f"模型加载失败: {e}")
            raise
    
    def detect_image(self, image_path: Union[str, Path]) -> DetectionResult:
        """
        检测单张图片
        
        Args:
            image_path: 图片路径
            
        Returns:
            DetectionResult: 检测结果
        """
        start_time = time.time()
        
        # 读取图片
        if isinstance(image_path, str):
            image_path = Path(image_path)
        
        if not image_path.exists():
            raise FileNotFoundError(f"图片不存在: {image_path}")
        
        image = cv2.imread(str(image_path))
        if image is None:
            raise ValueError(f"无法读取图片: {image_path}")
        
        # 运行检测
        results = self.model(
            image,
            conf=self.conf_threshold,
            iou=self.iou_threshold,
            device=self.device,
            verbose=False
        )
        
        # 解析结果
        detections = self._parse_results(results[0], image.shape)
        processing_time = time.time() - start_time
        
        # 更新统计
        self._update_stats(detections, processing_time)
        
        return DetectionResult(
            image=image,
            detections=detections,
            stats=self._get_current_stats(),
            processing_time=processing_time
        )
    
    def detect_video(self, video_path: Union[str, Path], 
                    callback: Optional[callable] = None) -> None:
        """
        检测视频
        
        Args:
            video_path: 视频路径
            callback: 每帧回调函数
        """
        if isinstance(video_path, str):
            video_path = Path(video_path)
        
        if not video_path.exists():
            raise FileNotFoundError(f"视频不存在: {video_path}")
        
        cap = cv2.VideoCapture(str(video_path))
        
        if not cap.isOpened():
            raise ValueError(f"无法打开视频: {video_path}")
        
        fps = int(cap.get(cv2.CAP_PROP_FPS))
        width = int(cap.get(cv2.CAP_PROP_FRAME_WIDTH))
        height = int(cap.get(cv2.CAP_PROP_FRAME_HEIGHT))
        
        logger.info(f"视频信息: {width}x{height}, {fps}FPS")
        
        frame_count = 0
        start_time = time.time()
        
        while True:
            ret, frame = cap.read()
            if not ret:
                break
            
            frame_count += 1
            frame_start = time.time()
            
            # 运行检测
            results = self.model(
                frame,
                conf=self.conf_threshold,
                iou=self.iou_threshold,
                device=self.device,
                verbose=False
            )
            
            # 解析结果
            detections = self._parse_results(results[0], frame.shape)
            processing_time = time.time() - frame_start
            
            # 更新统计
            self._update_stats(detections, processing_time)
            
            # 如果有回调函数，传递结果
            if callback:
                result = DetectionResult(
                    image=frame,
                    detections=detections,
                    stats=self._get_current_stats(),
                    processing_time=processing_time
                )
                callback(result)
            
            # 计算FPS
            elapsed_time = time.time() - start_time
            current_fps = frame_count / elapsed_time
            
            if frame_count % 30 == 0:
                logger.debug(f"处理帧数: {frame_count}, FPS: {current_fps:.2f}")
        
        cap.release()
        logger.info(f"视频检测完成，总帧数: {frame_count}")
    
    def detect_realtime(self, camera_id: int = 0, callback: Optional[callable] = None) -> None:
        """
        实时摄像头检测
        
        Args:
            camera_id: 摄像头ID
            callback: 每帧回调函数
        """
        cap = cv2.VideoCapture(camera_id)
        
        if not cap.isOpened():
            raise ValueError(f"无法打开摄像头: {camera_id}")
        
        # 设置摄像头参数
        cap.set(cv2.CAP_PROP_FRAME_WIDTH, 640)
        cap.set(cv2.CAP_PROP_FRAME_HEIGHT, 480)
        cap.set(cv2.CAP_PROP_FPS, 30)
        
        logger.info("开始实时检测...")
        
        frame_count = 0
        start_time = time.time()
        
        try:
            while True:
                ret, frame = cap.read()
                if not ret:
                    logger.warning("无法读取摄像头帧")
                    break
                
                frame_count += 1
                frame_start = time.time()
                
                # 运行检测
                results = self.model(
                    frame,
                    conf=self.conf_threshold,
                    iou=self.iou_threshold,
                    device=self.device,
                    verbose=False
                )
                
                # 解析结果
                detections = self._parse_results(results[0], frame.shape)
                processing_time = time.time() - frame_start
                
                # 更新统计
                self._update_stats(detections, processing_time)
                
                # 如果有回调函数，传递结果
                if callback:
                    result = DetectionResult(
                        image=frame,
                        detections=detections,
                        stats=self._get_current_stats(),
                        processing_time=processing_time
                    )
                    callback(result)
                
                # 计算FPS
                elapsed_time = time.time() - start_time
                current_fps = frame_count / elapsed_time
                
                # 在图像上显示FPS
                cv2.putText(frame, f"FPS: {current_fps:.1f}", (10, 30),
                           cv2.FONT_HERSHEY_SIMPLEX, 1, (0, 255, 0), 2)
                
                # 显示检测结果
                for det in detections:
                    label = f"{det.class_name} {det.confidence:.2f}"
                    x1, y1, x2, y2 = det.bbox
                    
                    # 绘制边界框
                    color = (0, 0, 255) if det.class_name == "fire" else (255, 0, 0)
                    cv2.rectangle(frame, (x1, y1), (x2, y2), color, 2)
                    
                    # 绘制标签
                    cv2.rectangle(frame, (x1, y1-30), (x1+len(label)*10, y1), color, -1)
                    cv2.putText(frame, label, (x1, y1-10),
                               cv2.FONT_HERSHEY_SIMPLEX, 0.5, (255, 255, 255), 2)
                
                # 显示统计信息
                stats_text = f"Fire: {sum(1 for d in detections if d.class_name == 'fire')} | "
                stats_text += f"Smoke: {sum(1 for d in detections if d.class_name == 'smoke')}"
                cv2.putText(frame, stats_text, (10, 60),
                           cv2.FONT_HERSHEY_SIMPLEX, 0.7, (255, 255, 255), 2)
                
                # 显示图像
                cv2.imshow("YOLO火灾检测系统 - 实时检测", frame)
                
                # 按'q'退出
                if cv2.waitKey(1) & 0xFF == ord('q'):
                    break
                    
        except KeyboardInterrupt:
            logger.info("用户中断检测")
        finally:
            cap.release()
            cv2.destroyAllWindows()
            logger.info("实时检测结束")
    
    def _parse_results(self, results: Results, image_shape: Tuple) -> List[Detection]:
        """解析YOLO检测结果"""
        detections = []
        
        if results.boxes is not None:
            boxes = results.boxes.cpu().numpy()
            
            for box in boxes:
                if len(box.xyxy) > 0:
                    x1, y1, x2, y2 = box.xyxy[0].astype(int)
                    confidence = box.conf[0]
                    class_id = int(box.cls[0])
                    
                    # 只处理火灾和烟雾类别
                    if class_id in self.class_names:
                        class_name = self.class_names[class_id]
                        center_x = (x1 + x2) // 2
                        center_y = (y1 + y2) // 2
                        
                        detection = Detection(
                            class_id=class_id,
                            class_name=class_name,
                            confidence=float(confidence),
                            bbox=[int(x1), int(y1), int(x2), int(y2)],
                            center=(center_x, center_y),
                            timestamp=time.time()
                        )
                        detections.append(detection)
        
        return detections
    
    def _update_stats(self, detections: List[Detection], processing_time: float):
        """更新统计信息"""
        self.stats["total_frames"] += 1
        
        for det in detections:
            if det.class_name == "fire":
                self.stats["total_fire"] += 1
            elif det.class_name == "smoke":
                self.stats["total_smoke"] += 1
        
        # 更新平均处理时间
        avg_time = self.stats["avg_processing_time"]
        n = self.stats["total_frames"]
        self.stats["avg_processing_time"] = (avg_time * (n-1) + processing_time) / n if n > 1 else processing_time
    
    def _get_current_stats(self) -> Dict:
        """获取当前统计信息"""
        return self.stats.copy()
    
    def set_confidence_threshold(self, conf: float):
        """设置置信度阈值"""
        self.conf_threshold = max(0.0, min(1.0, conf))
        logger.info(f"置信度阈值设置为: {self.conf_threshold}")
    
    def set_iou_threshold(self, iou: float):
        """设置IOU阈值"""
        self.iou_threshold = max(0.0, min(1.0, iou))
        logger.info(f"IOU阈值设置为: {self.iou_threshold}")
    
    def save_detection_result(self, result: DetectionResult, output_path: Union[str, Path]):
        """保存检测结果"""
        output_path = Path(output_path)
        output_path.parent.mkdir(parents=True, exist_ok=True)
        
        # 绘制检测框
        image_with_boxes = result.image.copy()
        
        for det in result.detections:
            label = f"{det.class_name} {det.confidence:.2f}"
            x1, y1, x2, y2 = det.bbox
            
            # 设置颜色
            color = (0, 0, 255) if det.class_name == "fire" else (255, 0, 0)
            
            # 绘制边界框
            cv2.rectangle(image_with_boxes, (x1, y1), (x2, y2), color, 2)
            
            # 绘制标签背景
            (label_width, label_height), baseline = cv2.getTextSize(
                label, cv2.FONT_HERSHEY_SIMPLEX, 0.5, 2
            )
            cv2.rectangle(image_with_boxes, 
                         (x1, y1 - label_height - baseline - 5),
                         (x1 + label_width, y1),
                         color, -1)
            
            # 绘制标签文字
            cv2.putText(image_with_boxes, label, (x1, y1 - 5),
                       cv2.FONT_HERSHEY_SIMPLEX, 0.5, (255, 255, 255), 2)
        
        # 添加统计信息
        stats_text = [
            f"检测时间: {result.processing_time:.3f}s",
            f"火焰数量: {sum(1 for d in result.detections if d.class_name == 'fire')}",
            f"烟雾数量: {sum(1 for d in result.detections if d.class_name == 'smoke')}",
            f"总检测数: {len(result.detections)}"
        ]
        
        for j, text in enumerate(stats_text):
            cv2.putText(image_with_boxes, text, (10, 30 + j*25),
                       cv2.FONT_HERSHEY_SIMPLEX, 0.6, (0, 255, 0), 2)
        
        # 保存图片
        cv2.imwrite(str(output_path), image_with_boxes)
        logger.info(f"检测结果已保存: {output_path}")
        
        # 保存检测信息到文本文件
        txt_path = output_path.with_suffix('.txt')
        with open(txt_path, 'w', encoding='utf-8') as f:
            f.write("检测结果统计:\n")
            f.write(f"图片尺寸: {result.image.shape[1]}x{result.image.shape[0]}\n")
            f.write(f"处理时间: {result.processing_time:.3f}秒\n")
            f.write(f"检测数量: {len(result.detections)}\n")
            f.write("\n详细检测信息:\n")
            
            for i, det in enumerate(result.detections, 1):
                f.write(f"\n目标 {i}:\n")
                f.write(f"  类别: {det.class_name}\n")
                f.write(f"  置信度: {det.confidence:.3f}\n")
                f.write(f"  边界框: [{det.bbox[0]}, {det.bbox[1]}, {det.bbox[2]}, {det.bbox[3]}]\n")
                f.write(f"  中心点: ({det.center[0]}, {det.center[1]})\n")
                f.write(f"  时间戳: {det.timestamp}\n")
```

## 6. src/gui.py (PyQt5界面)
```python
#!/usr/bin/env python3
# -*- coding: utf-8 -*-
"""
火灾检测系统 - PyQt5 GUI界面
"""

import sys
import os
from pathlib import Path
from typing import Optional, List, Dict
from datetime import datetime

import cv2
import numpy as np
from PyQt5.QtWidgets import (QApplication, QMainWindow, QWidget, QVBoxLayout, 
                             QHBoxLayout, QLabel, QPushButton, QComboBox,
                             QSlider, QSpinBox, QDoubleSpinBox, QFileDialog,
                             QMessageBox, QGroupBox, QFrame, QTextEdit,
                             QProgressBar, QTableWidget, QTableWidgetItem,
                             QHeaderView, QTabWidget, QSplitter, QListWidget,
                             QListWidgetItem, QCheckBox, QRadioButton, QButtonGroup)
from PyQt5.QtCore import Qt, QTimer, QThread, pyqtSignal, pyqtSlot, QSize
from PyQt5.QtGui import QImage, QPixmap, QFont, QIcon, QColor, QPainter

from loguru import logger
from src.detector import FireSmokeDetector, DetectionResult
from src.utils import setup_logging, AlertSystem

class DetectionThread(QThread):
    """检测线程"""
    detection_completed = pyqtSignal(object)  # 检测完成信号
    frame_processed = pyqtSignal(np.ndarray)  # 帧处理信号
    error_occurred = pyqtSignal(str)  # 错误信号
    
    def __init__(self, detector: FireSmokeDetector):
        super().__init__()
        self.detector = detector
        self.source_type = None  # 'image', 'video', 'camera'
        self.source_path = None
        self.camera_id = 0
        self.running = False
        
    def set_source(self, source_type: str, source_path: Optional[str] = None, camera_id: int = 0):
        """设置检测源"""
        self.source_type = source_type
        self.source_path = source_path
        self.camera_id = camera_id
        
    def run(self):
        """线程运行函数"""
        try:
            if self.source_type == 'image' and self.source_path:
                result = self.detector.detect_image(self.source_path)
                self.detection_completed.emit(result)
                
            elif self.source_type == 'video' and self.source_path:
                self.running = True
                self.detector.detect_video(self.source_path, self._video_callback)
                
            elif self.source_type == 'camera':
                self.running = True
                self.detector.detect_realtime(self.camera_id, self._video_callback)
                
        except Exception as e:
            self.error_occurred.emit(str(e))
            
    def stop(self):
        """停止检测"""
        self.running = False
        
    def _video_callback(self, result: DetectionResult):
        """视频检测回调函数"""
        if self.running:
            self.detection_completed.emit(result)
            self.frame_processed.emit(result.image)

class MainWindow(QMainWindow):
    """主窗口"""
    
    def __init__(self):
        super().__init__()
        self.detector = None
        self.detection_thread = None
        self.current_result = None
        self.history = []
        
        # 初始化UI
        self.init_ui()
        self.setup_connections()
        
        # 加载默认模型
        self.load_default_model()
        
    def init_ui(self):
        """初始化UI界面"""
        self.setWindowTitle("YOLO火灾检测系统")
        self.setGeometry(100, 100, 1400, 900)
        
        # 设置窗口图标
        self.setWindowIcon(QIcon("assets/icon.png") if Path("assets/icon.png").exists() else QIcon())
        
        # 创建中心部件
        central_widget = QWidget()
        self.setCentralWidget(central_widget)
        
        # 主布局
        main_layout = QHBoxLayout(central_widget)
        
        # 左侧面板
        left_panel = self.create_left_panel()
        main_layout.addWidget(left_panel, 1)
        
        # 右侧面板
        right_panel = self.create_right_panel()
        main_layout.addWidget(right_panel, 2)
        
        # 状态栏
        self.statusBar().showMessage("就绪")
        
    def create_left_panel(self) -> QWidget:
        """创建左侧面板"""
        left_widget = QWidget()
        left_layout = QVBoxLayout(left_widget)
        
        # 标题
        title_label = QLabel("火灾识别系统")
        title_label.setFont(QFont("Arial", 20, QFont.Bold))
        title_label.setAlignment(Qt.AlignCenter)
        title_label.setStyleSheet("color: #FF6B6B; margin: 10px;")
        left_layout.addWidget(title_label)
        
        # 实时检测统计
        stats_group = QGroupBox("实时检测统计")
        stats_layout = QVBoxLayout()
        
        # 火焰统计
        fire_layout = QHBoxLayout()
        fire_label = QLabel("火焰:")
        fire_label.setFont(QFont("Arial", 12, QFont.Bold))
        fire_label.setStyleSheet("color: #FF6B6B;")
        self.fire_count_label = QLabel("0")
        self.fire_count_label.setFont(QFont("Arial", 16, QFont.Bold))
        self.fire_count_label.setStyleSheet("color: #FF6B6B;")
        fire_layout.addWidget(fire_label)
        fire_layout.addStretch()
        fire_layout.addWidget(self.fire_count_label)
        stats_layout.addLayout(fire_layout)
        
        # 烟雾统计
        smoke_layout = QHBoxLayout()
        smoke_label = QLabel("烟雾:")
        smoke_label.setFont(QFont("Arial", 12, QFont.Bold))
        smoke_label.setStyleSheet("color: #4ECDC4;")
        self.smoke_count_label = QLabel("0")
        self.smoke_count_label.setFont(QFont("Arial", 16, QFont.Bold))
        self.smoke_count_label.setStyleSheet("color: #4ECDC4;")
        smoke_layout.addWidget(smoke_label)
        smoke_layout.addStretch()
        smoke_layout.addWidget(self.smoke_count_label)
        stats_layout.addLayout(smoke_layout)
        
        stats_group.setLayout(stats_layout)
        left_layout.addWidget(stats_group)
        
        # 文件操作
        file_group = QGroupBox("文件操作")
        file_layout = QVBoxLayout()
        
        # 模型选择
        model_layout = QHBoxLayout()
        model_label = QLabel("选择模型:")
        self.model_combo = QComboBox()
        self.model_combo.addItem("YOLOv11n (默认)")
        self.model_combo.addItem("YOLOv11s")
        self.model_combo.addItem("YOLOv11m")
        self.model_combo.addItem("YOLOv11l")
        self.model_combo.addItem("YOLOv11x")
        model_layout.addWidget(model_label)
        model_layout.addWidget(self.model_combo)
        file_layout.addLayout(model_layout)
        
        # 文件选择按钮
        self.select_image_btn = QPushButton("选择图片")
        self.select_video_btn = QPushButton("选择视频")
        self.select_camera_btn = QPushButton("摄像头")
        self.select_folder_btn = QPushButton("选择文件夹")
        
        self.select_image_btn.setMinimumHeight(40)
        self.select_video_btn.setMinimumHeight(40)
        self.select_camera_btn.setMinimumHeight(40)
        self.select_folder_btn.setMinimumHeight(40)
        
        file_layout.addWidget(self.select_image_btn)
        file_layout.addWidget(self.select_video_btn)
        file_layout.addWidget(self.select_camera_btn)
        file_layout.addWidget(self.select_folder_btn)
        
        # 模型加载状态
        self.model_status_label = QLabel("模型状态: 未加载")
        self.model_status_label.setStyleSheet("color: #888888;")
        file_layout.addWidget(self.model_status_label)
        
        file_group.setLayout(file_layout)
        left_layout.addWidget(file_group)
        
        # 检测控制
        control_group = QGroupBox("检测控制")
        control_layout = QVBoxLayout()
        
        # 开始/停止按钮
        button_layout = QHBoxLayout()
        self.start_btn = QPushButton("开始检测")
        self.stop_btn = QPushButton("停止检测")
        self.stop_btn.setEnabled(False)
        
        self.start_btn.setMinimumHeight(50)
        self.stop_btn.setMinimumHeight(50)
        
        self.start_btn.setStyleSheet("""
            QPushButton {
                background-color: #4CAF50;
                color: white;
                font-weight: bold;
                border-radius: 5px;
            }
            QPushButton:hover {
                background-color: #45a049;
            }
        """)
        
        self.stop_btn.setStyleSheet("""
            QPushButton {
                background-color: #f44336;
                color: white;
                font-weight: bold;
                border-radius: 5px;
            }
            QPushButton:hover {
                background-color: #da190b;
            }
        """)
        
        button_layout.addWidget(self.start_btn)
        button_layout.addWidget(self.stop_btn)
        control_layout.addLayout(button_layout)
        
        # 进度条
        self.progress_bar = QProgressBar()
        self.progress_bar.setTextVisible(False)
        control_layout.addWidget(self.progress_bar)
        
        control_group.setLayout(control_layout)
        left_layout.addWidget(control_group)
        
        # 检测参数
        params_group = QGroupBox("检测参数")
        params_layout = QVBoxLayout()
        
        # 置信度阈值
        conf_layout = QHBoxLayout()
        conf_label = QLabel("置信度阈值:")
        self.conf_slider = QSlider(Qt.Horizontal)
        self.conf_slider.setRange(0, 100)
        self.conf_slider.setValue(25)
        self.conf_value_label = QLabel("0.25")
        conf_layout.addWidget(conf_label)
        conf_layout.addWidget(self.conf_slider)
        conf_layout.addWidget(self.conf_value_label)
        params_layout.addLayout(conf_layout)
        
        # IOU阈值
        iou_layout = QHBoxLayout()
        iou_label = QLabel("IOU阈值:")
        self.iou_spinbox = QDoubleSpinBox()
        self.iou_spinbox.setRange(0.0, 1.0)
        self.iou_spinbox.setSingleStep(0.05)
        self.iou_spinbox.setValue(0.45)
        iou_layout.addWidget(iou_label)
        iou_layout.addWidget(self.iou_spinbox)
        params_layout.addLayout(iou_layout)
        
        params_group.setLayout(params_layout)
        left_layout.addWidget(params_group)
        
        # 保存选项
        save_group = QGroupBox("保存选项")
        save_layout = QVBoxLayout()
        
        self.save_checkbox = QCheckBox("保存检测结果")
        self.save_checkbox.setChecked(True)
        save_layout.addWidget(self.save_checkbox)
        
        self.save_btn = QPushButton("选择保存路径")
        save_layout.addWidget(self.save_btn)
        
        self.save_path_label = QLabel("保存路径: outputs/detections/")
        self.save_path_label.setStyleSheet("color: #666666; font-size: 10px;")
        save_layout.addWidget(self.save_path_label)
        
        save_group.setLayout(save_layout)
        left_layout.addWidget(save_group)
        
        # 日志显示
        log_group = QGroupBox("系统日志")
        log_layout = QVBoxLayout()
        
        self.log_text = QTextEdit()
        self.log_text.setReadOnly(True)
        self.log_text.setMaximumHeight(150)
        self.log_text.setStyleSheet("""
            QTextEdit {
                background-color: #2b2b2b;
                color: #ffffff;
                font-family: Consolas, Monaco, monospace;
                font-size: 10px;
            }
        """)
        log_layout.addWidget(self.log_text)
        
        log_group.setLayout(log_layout)
        left_layout.addWidget(log_group)
        
        left_layout.addStretch()
        
        return left_widget
        
    def create_right_panel(self) -> QWidget:
        """创建右侧面板"""
        right_widget = QWidget()
        right_layout = QVBoxLayout(right_widget)
        
        # 标签页
        self.tab_widget = QTabWidget()
        
        # 图像显示标签页
        image_tab = QWidget()
        image_layout = QVBoxLayout(image_tab)
        
        # 图像显示区域
        self.image_label = QLabel()
        self.image_label.setAlignment(Qt.AlignCenter)
        self.image_label.setMinimumSize(640, 480)
        self.image_label.setStyleSheet("""
            QLabel {
                background-color: #1e1e1e;
                border: 2px solid #333333;
                border-radius: 5px;
            }
        """)
        image_layout.addWidget(self.image_label)
        
        # 图像信息
        info_group = QGroupBox("图像信息")
        info_layout = QVBoxLayout()
        
        self.file_path_label = QLabel("文件路径: 未选择")
        self.image_size_label = QLabel("图像尺寸: -")
        self.detection_count_label = QLabel("检测数量: 0")
        self.processing_time_label = QLabel("处理时间: -")
        
        for label in [self.file_path_label, self.image_size_label, 
                     self.detection_count_label, self.processing_time_label]:
            label.setStyleSheet("color: #666666; font-size: 12px;")
            info_layout.addWidget(label)
        
        info_group.setLayout(info_layout)
        image_layout.addWidget(info_group)
        
        self.tab_widget.addTab(image_tab, "图像显示")
        
        # 视频播放标签页
        video_tab = QWidget()
        video_layout = QVBoxLayout(video_tab)
        
        self.video_label = QLabel("视频播放区域")
        self.video_label.setAlignment(Qt.AlignCenter)
        self.video_label.setMinimumSize(640, 480)
        self.video_label.setStyleSheet("""
            QLabel {
                background-color: #1e1e1e;
                border: 2px solid #333333;
                border-radius: 5px;
                color: #888888;
            }
        """)
        video_layout.addWidget(self.video_label)
        
        # 视频控制
        video_control_layout = QHBoxLayout()
        self.play_btn = QPushButton("播放")
        self.pause_btn = QPushButton("暂停")
        self.stop_video_btn = QPushButton("停止")
        
        video_control_layout.addWidget(self.play_btn)
        video_control_layout.addWidget(self.pause_btn)
        video_control_layout.addWidget(self.stop_video_btn)
        video_layout.addLayout(video_control_layout)
        
        self.tab_widget.addTab(video_tab, "视频播放")
        
        # 检索结果标签页
        result_tab = QWidget()
        result_layout = QVBoxLayout(result_tab)
        
        # 结果表格
        self.result_table = QTableWidget()
        self.result_table.setColumnCount(6)
        self.result_table.setHorizontalHeaderLabels(["ID", "类型", "置信度", "位置", "大小", "时间"])
        self.result_table.horizontalHeader().setSectionResizeMode(QHeaderView.Stretch)
        result_layout.addWidget(self.result_table)
        
        self.tab_widget.addTab(result_tab, "检测结果")
        
        # 历史记录标签页
        history_tab = QWidget()
        history_layout = QVBoxLayout(history_tab)
        
        self.history_list = QListWidget()
        history_layout.addWidget(self.history_list)
        
        self.tab_widget.addTab(history_tab, "历史记录")
        
        right_layout.addWidget(self.tab_widget)
        
        return right_widget
    
    def setup_connections(self):
        """设置信号槽连接"""
        # 文件操作
        self.select_image_btn.clicked.connect(self.select_image)
        self.select_video_btn.clicked.connect(self.select_video)
        self.select_camera_btn.clicked.connect(self.select_camera)
        self.select_folder_btn.clicked.connect(self.select_folder)
        
        # 检测控制
        self.start_btn.clicked.connect(self.start_detection)
        self.stop_btn.clicked.connect(self.stop_detection)
        
        # 参数设置
        self.conf_slider.valueChanged.connect(self.update_confidence)
        self.iou_spinbox.valueChanged.connect(self.update_iou)
        
        # 保存选项
        self.save_btn.clicked.connect(self.select_save_path)
        
        # 模型选择
        self.model_combo.currentTextChanged.connect(self.load_model)
        
    def load_default_model(self):
        """加载默认模型"""
        try:
            model_path = "models/yolo11n.pt"
            if Path(model_path).exists():
                self.detector = FireSmokeDetector(model_path)
                self.model_status_label.setText("模型状态: 已加载")
                self.model_status_label.setStyleSheet("color: #4CAF50;")
                self.log("模型加载成功")
            else:
                self.model_status_label.setText("模型状态: 未找到模型文件")
                self.model_status_label.setStyleSheet("color: #f44336;")
                self.log("警告: 未找到模型文件，请下载模型")
        except Exception as e:
            self.model_status_label.setText("模型状态: 加载失败")
            self.model_status_label.setStyleSheet("color: #f44336;")
            self.log(f"模型加载失败: {e}")
    
    def load_model(self, model_name: str):
        """加载模型"""
        model_map = {
            "YOLOv11n (默认)": "models/yolo11n.pt",
            "YOLOv11s": "models/yolo11s.pt",
            "YOLOv11m": "models/yolo11m.pt",
            "YOLOv11l": "models/yolo11l.pt",
            "YOLOv11x": "models/yolo11x.pt"
        }
        
        model_path = model_map.get(model_name)
        if model_path and Path(model_path).exists():
            try:
                self.detector = FireSmokeDetector(model_path)
                self.model_status_label.setText(f"模型状态: 已加载 ({model_name})")
                self.model_status_label.setStyleSheet("color: #4CAF50;")
                self.log(f"模型切换为: {model_name}")
            except Exception as e:
                self.log(f"模型加载失败: {e}")
    
    def select_image(self):
        """选择图片"""
        file_path, _ = QFileDialog.getOpenFileName(
            self, "选择图片", "",
            "图片文件 (*.jpg *.jpeg *.png *.bmp *.tiff)"
        )
        
        if file_path:
            self.current_file = file_path
            self.file_path_label.setText(f"文件路径: {Path(file_path).name}")
            self.display_image(file_path)
            
            # 自动切换到图像标签页
            self.tab_widget.setCurrentIndex(0)
            
            self.log(f"已选择图片: {Path(file_path).name}")
    
    def select_video(self):
        """选择视频"""
        file_path, _ = QFileDialog.getOpenFileName(
            self, "选择视频", "",
            "视频文件 (*.mp4 *.avi *.mov *.mkv *.flv)"
        )
        
        if file_path:
            self.current_file = file_path
            self.file_path_label.setText(f"文件路径: {Path(file_path).name}")
            
            # 显示视频第一帧
            cap = cv2.VideoCapture(file_path)
            ret, frame = cap.read()
            if ret:
                self.display_frame(frame)
            cap.release()
            
            # 自动切换到视频标签页
            self.tab_widget.setCurrentIndex(1)
            
            self.log(f"已选择视频: {Path(file_path).name}")
    
    def select_camera(self):
        """选择摄像头"""
        self.current_file = None
        self.file_path_label.setText("摄像头: 0")
        
        # 自动切换到视频标签页
        self.tab_widget.setCurrentIndex(1)
        
        self.log("已选择摄像头")
    
    def select_folder(self):
        """选择文件夹"""
        folder_path = QFileDialog.getExistingDirectory(self, "选择文件夹")
        
        if folder_path:
            self.log(f"已选择文件夹: {folder_path}")
            # 这里可以添加批量处理逻辑
    
    def start_detection(self):
        """开始检测"""
        if not self.detector:
            QMessageBox.warning(self, "警告", "请先加载模型")
            return
        
        if not hasattr(self, 'current_file') and not hasattr(self, 'camera_selected'):
            QMessageBox.warning(self, "警告", "请先选择检测源")
            return
        
        # 创建检测线程
        self.detection_thread = DetectionThread(self.detector)
        
        if hasattr(self, 'current_file'):
            file_path = self.current_file
            if file_path.lower().endswith(('.jpg', '.jpeg', '.png', '.bmp', '.tiff')):
                self.detection_thread.set_source('image', file_path)
                self.log(f"开始图片检测: {Path(file_path).name}")
            else:
                self.detection_thread.set_source('video', file_path)
                self.log(f"开始视频检测: {Path(file_path).name}")
        else:
            self.detection_thread.set_source('camera', camera_id=0)
            self.log("开始摄像头实时检测")
        
        # 连接信号
        self.detection_thread.detection_completed.connect(self.on_detection_completed)
        self.detection_thread.error_occurred.connect(self.on_detection_error)
        
        # 更新UI状态
        self.start_btn.setEnabled(False)
        self.stop_btn.setEnabled(True)
        self.progress_bar.setRange(0, 0)  # 无限进度条
        
        # 开始检测
        self.detection_thread.start()
    
    def stop_detection(self):
        """停止检测"""
        if self.detection_thread and self.detection_thread.isRunning():
            self.detection_thread.stop()
            self.detection_thread.quit()
            self.detection_thread.wait()
        
        # 更新UI状态
        self.start_btn.setEnabled(True)
        self.stop_btn.setEnabled(False)
        self.progress_bar.setRange(0, 1)
        self.progress_bar.setValue(0)
        
        self.log("检测已停止")
    
    @pyqtSlot(object)
    def on_detection_completed(self, result: DetectionResult):
        """检测完成槽函数"""
        self.current_result = result
        
        # 更新统计
        fire_count = sum(1 for d in result.detections if d.class_name == "fire")
        smoke_count = sum(1 for d in result.detections if d.class_name == "smoke")
        
        self.fire_count_label.setText(str(fire_count))
        self.smoke_count_label.setText(str(smoke_count))
        self.detection_count_label.setText(f"检测数量: {len(result.detections)}")
        self.processing_time_label.setText(f"处理时间: {result.processing_time:.3f}秒")
        
        # 更新图像显示
        self.display_detection_result(result)
        
        # 更新结果表格
        self.update_result_table(result)
        
        # 添加到历史记录
        self.add_to_history(result)
        
        # 保存结果
        if self.save_checkbox.isChecked():
            self.save_detection_result(result)
    
    @pyqtSlot(str)
    def on_detection_error(self, error_msg: str):
        """检测错误槽函数"""
        QMessageBox.critical(self, "错误", f"检测过程中发生错误:\n{error_msg}")
        self.log(f"检测错误: {error_msg}")
        self.stop_detection()
    
    def display_image(self, image_path: str):
        """显示图片"""
        try:
            # 使用OpenCV读取图片
            image = cv2.imread(image_path)
            if image is not None:
                # 调整大小以适应显示区域
                h, w = image.shape[:2]
                self.image_size_label.setText(f"图像尺寸: {w}x{h}")
                
                # 显示图片
                self.display_frame(image)
        except Exception as e:
            self.log(f"显示图片错误: {e}")
    
    def display_frame(self, frame: np.ndarray):
        """显示帧"""
        try:
            # 转换颜色空间 BGR -> RGB
            rgb_image = cv2.cvtColor(frame, cv2.COLOR_BGR2RGB)
            
            # 获取显示区域大小
            label_size = self.image_label.size()
            target_width = label_size.width() - 20
            target_height = label_size.height() - 20
            
            # 计算缩放比例
            h, w = rgb_image.shape[:2]
            scale = min(target_width / w, target_height / h, 1.0)
            new_w = int(w * scale)
            new_h = int(h * scale)
            
            # 缩放图片
            if scale < 1.0:
                rgb_image = cv2.resize(rgb_image, (new_w, new_h), interpolation=cv2.INTER_AREA)
            
            # 转换为QImage
            qimage = QImage(rgb_image.data, rgb_image.shape[1], rgb_image.shape[0], 
                           rgb_image.strides[0], QImage.Format_RGB888)
            
            # 创建QPixmap
            pixmap = QPixmap.fromImage(qimage)
            
            # 显示图片
            self.image_label.setPixmap(pixmap)
            
        except Exception as e:
            self.log(f"显示帧错误: {e}")
    
    def display_detection_result(self, result: DetectionResult):
        """显示检测结果"""
        try:
            # 复制图像
            image_with_boxes = result.image.copy()
            
            # 绘制检测框
            for det in result.detections:
                x1, y1, x2, y2 = det.bbox
                confidence = det.confidence
                class_name = det.class_name
                
                # 设置颜色
                if class_name == "fire":
                    color = (0, 0, 255)  # 红色
                else:  # smoke
                    color = (255, 0, 0)  # 蓝色
                
                # 绘制边界框
                cv2.rectangle(image_with_boxes, (x1, y1), (x2, y2), color, 2)
                
                # 绘制标签
                label = f"{class_name} {confidence:.2f}"
                (label_width, label_height), baseline = cv2.getTextSize(
                    label, cv2.FONT_HERSHEY_SIMPLEX, 0.5, 2
                )
                
                # 标签背景
                cv2.rectangle(image_with_boxes, 
                             (x1, y1 - label_height - baseline - 5),
                             (x1 + label_width, y1),
                             color, -1)
                
                # 标签文字
                cv2.putText(image_with_boxes, label, (x1, y1 - 5),
                           cv2.FONT_HERSHEY_SIMPLEX, 0.5, (255, 255, 255), 2)
            
            # 绘制统计信息
            stats_text = [
                f"处理时间: {result.processing_time:.3f}s",
                f"火焰数量: {sum(1 for d in result.detections if d.class_name == 'fire')}",
                f"烟雾数量: {sum(1 for d in result.detections if d.class_name == 'smoke')}",
                f"总检测数: {len(result.detections)}"
            ]
            
            for i, text in enumerate(stats_text):
                y_pos = 30 + i * 25
                cv2.putText(image_with_boxes, text, (10, y_pos),
                           cv2.FONT_HERSHEY_SIMPLEX, 0.6, (0, 255, 0), 2)
            
            # 显示图片
            self.display_frame(image_with_boxes)
            
        except Exception as e:
            self.log(f"显示检测结果错误: {e}")
    
    def update_result_table(self, result: DetectionResult):
        """更新结果表格"""
        self.result_table.setRowCount(len(result.detections))
        
        for i, det in enumerate(result.detections):
            # ID
            self.result_table.setItem(i, 0, QTableWidgetItem(str(i+1)))
            
            # 类型
            type_item = QTableWidgetItem(det.class_name)
            if det.class_name == "fire":
                type_item.setForeground(QColor(255, 107, 107))
            else:
                type_item.setForeground(QColor(78, 205, 196))
            self.result_table.setItem(i, 1, type_item)
            
            # 置信度
            conf_item = QTableWidgetItem(f"{det.confidence:.3f}")
            if det.confidence > 0.7:
                conf_item.setForeground(QColor(0, 255, 0))
            elif det.confidence > 0.4:
                conf_item.setForeground(QColor(255, 255, 0))
            else:
                conf_item.setForeground(QColor(255, 0, 0))
            self.result_table.setItem(i, 2, conf_item)
            
            # 位置
            pos_item = QTableWidgetItem(f"({det.center[0]}, {det.center[1]})")
            self.result_table.setItem(i, 3, pos_item)
            
            # 大小
            width = det.bbox[2] - det.bbox[0]
            height = det.bbox[3] - det.bbox[1]
            size_item = QTableWidgetItem(f"{width}x{height}")
            self.result_table.setItem(i, 4, size_item)
            
            # 时间
            time_str = datetime.fromtimestamp(det.timestamp).strftime("%H:%M:%S")
            self.result_table.setItem(i, 5, QTableWidgetItem(time_str))
    
    def add_to_history(self, result: DetectionResult):
        """添加到历史记录"""
        timestamp = datetime.now().strftime("%Y-%m-%d %H:%M:%S")
        fire_count = sum(1 for d in result.detections if d.class_name == "fire")
        smoke_count = sum(1 for d in result.detections if d.class_name == "smoke")
        
        history_text = f"{timestamp} - 火焰: {fire_count}, 烟雾: {smoke_count}, 耗时: {result.processing_time:.3f}s"
        self.history_list.addItem(history_text)
        
        # 保存到内存历史
        self.history.append({
            "timestamp": timestamp,
            "result": result,
            "fire_count": fire_count,
            "smoke_count": smoke_count
        })
        
        # 限制历史记录数量
        if self.history_list.count() > 100:
            self.history_list.takeItem(0)
            self.history.pop(0)
    
    def save_detection_result(self, result: DetectionResult):
        """保存检测结果"""
        try:
            # 创建保存目录
            save_dir = Path("outputs/detections")
            save_dir.mkdir(parents=True, exist_ok=True)
            
            # 生成文件名
            timestamp = datetime.now().strftime("%Y%m%d_%H%M%S")
            if hasattr(self, 'current_file'):
                filename = Path(self.current_file).stem
                save_path = save_dir / f"{filename}_{timestamp}.jpg"
            else:
                save_path = save_dir / f"detection_{timestamp}.jpg"
            
            # 保存图片
            image_with_boxes = result.image.copy()
            
            # 绘制检测框
            for det in result.detections:
                x1, y1, x2, y2 = det.bbox
                color = (0, 0, 255) if det.class_name == "fire" else (255, 0, 0)
                label = f"{det.class_name} {det.confidence:.2f}"
                
                cv2.rectangle(image_with_boxes, (x1, y1), (x2, y2), color, 2)
                
                (label_width, label_height), baseline = cv2.getTextSize(
                    label, cv2.FONT_HERSHEY_SIMPLEX, 0.5, 2
                )
                
                cv2.rectangle(image_with_boxes, 
                             (x1, y1 - label_height - baseline - 5),
                             (x1 + label_width, y1),
                             color, -1)
                
                cv2.putText(image_with_boxes, label, (x1, y1 - 5),
                           cv2.FONT_HERSHEY_SIMPLEX, 0.5, (255, 255, 255), 2)
            
            cv2.imwrite(str(save_path), image_with_boxes)
            
            # 保存检测信息
            info_path = save_path.with_suffix('.txt')
            with open(info_path, 'w', encoding='utf-8') as f:
                f.write(f"检测时间: {datetime.now().strftime('%Y-%m-%d %H:%M:%S')}\n")
                f.write(f"图片尺寸: {result.image.shape[1]}x{result.image.shape[0]}\n")
                f.write(f"处理时间: {result.processing_time:.3f}秒\n")
                f.write(f"火焰数量: {sum(1 for d in result.detections if d.class_name == 'fire')}\n")
                f.write(f"烟雾数量: {sum(1 for d in result.detections if d.class_name == 'smoke')}\n")
                f.write(f"总检测数: {len(result.detections)}\n\n")
                
                for i, det in enumerate(result.detections, 1):
                    f.write(f"目标 {i}:\n")
                    f.write(f"  类别: {det.class_name}\n")
                    f.write(f"  置信度: {det.confidence:.3f}\n")
                    f.write(f"  边界框: [{det.bbox[0]}, {det.bbox[1]}, {det.bbox[2]}, {det.bbox[3]}]\n")
                    f.write(f"  中心点: ({det.center[0]}, {det.center[1]})\n")
                    f.write(f"  时间戳: {det.timestamp}\n")
            
            self.log(f"检测结果已保存: {save_path.name}")
            
        except Exception as e:
            self.log(f"保存结果错误: {e}")
    
    def update_confidence(self, value: int):
        """更新置信度阈值"""
        conf_threshold = value / 100.0
        self.conf_value_label.setText(f"{conf_threshold:.2f}")
        if self.detector:
            self.detector.set_confidence_threshold(conf_threshold)
    
    def update_iou(self, value: float):
        """更新IOU阈值"""
        if self.detector:
            self.detector.set_iou_threshold(value)
    
    def select_save_path(self):
        """选择保存路径"""
        folder_path = QFileDialog.getExistingDirectory(self, "选择保存路径")
        if folder_path:
            self.save_path_label.setText(f"保存路径: {folder_path}")
            self.log(f"保存路径设置为: {folder_path}")
    
    def log(self, message: str):
        """记录日志"""
        timestamp = datetime.now().strftime("%H:%M:%S")
        log_message = f"[{timestamp}] {message}"
        self.log_text.append(log_message)
        
        # 滚动到底部
        scrollbar = self.log_text.verticalScrollBar()
        scrollbar.setValue(scrollbar.maximum())
    
    def closeEvent(self, event):
        """关闭事件"""
        if self.detection_thread and self.detection_thread.isRunning():
            self.detection_thread.stop()
            self.detection_thread.quit()
            self.detection_thread.wait()
        
        event.accept()

def main():
    """主函数"""
    app = QApplication(sys.argv)
    app.setStyle('Fusion')
    
    # 设置应用样式
    app.setStyleSheet("""
        QMainWindow {
            background-color: #2b2b2b;
        }
        QWidget {
            background-color: #2b2b2b;
            color: #ffffff;
        }
        QLabel {
            color: #ffffff;
        }
        QPushButton {
            background-color: #3c3f41;
            border: 1px solid #555555;
            padding: 5px;
            border-radius: 3px;
        }
        QPushButton:hover {
            background-color: #4c5052;
        }
        QPushButton:pressed {
            background-color: #2b2b2b;
        }
        QGroupBox {
            border: 2px solid #555555;
            border-radius: 5px;
            margin-top: 10px;
            font-weight: bold;
        }
        QGroupBox::title {
            subcontrol-origin: margin;
            left: 10px;
            padding: 0 5px 0 5px;
        }
        QComboBox, QSpinBox, QDoubleSpinBox {
            background-color: #3c3f41;
            border: 1px solid #555555;
            padding: 3px;
            border-radius: 3px;
        }
        QSlider::groove:horizontal {
            height: 6px;
            background: #555555;
            border-radius: 3px;
        }
        QSlider::handle:horizontal {
            background: #4CAF50;
            width: 18px;
            margin: -6px 0;
            border-radius: 9px;
        }
        QTabWidget::pane {
            border: 1px solid #555555;
            background-color: #2b2b2b;
        }
        QTabBar::tab {
            background-color: #3c3f41;
            padding: 8px 12px;
            margin-right: 2px;
        }
        QTabBar::tab:selected {
            background-color: #4CAF50;
        }
        QTableWidget {
            background-color: #2b2b2b;
            alternate-background-color: #3c3f41;
            gridline-color: #555555;
        }
        QHeaderView::section {
            background-color: #3c3f41;
            padding: 5px;
            border: 1px solid #555555;
        }
    """)
    
    window = MainWindow()
    window.show()
    
    sys.exit(app.exec_())

if __name__ == "__main__":
    main()
```

## 7. src/utils.py (工具函数)
```python
#!/usr/bin/env python3
# -*- coding: utf-8 -*-
"""
工具函数模块
"""

import os
import json
import yaml
import time
import logging
from datetime import datetime
from pathlib import Path
from typing import Dict, List, Any, Optional
from loguru import logger
import numpy as np
import cv2
import smtplib
from email.mime.text import MIMEText
from email.mime.multipart import MIMEMultipart
import threading

def setup_logging(log_dir: str = "logs", level: str = "INFO"):
    """
    设置日志系统
    
    Args:
        log_dir: 日志目录
        level: 日志级别
    """
    # 创建日志目录
    log_path = Path(log_dir)
    log_path.mkdir(parents=True, exist_ok=True)
    
    # 配置loguru
    logger.remove()  # 移除默认handler
    
    # 控制台输出
    logger.add(
        sys.stderr,
        format="<green>{time:YYYY-MM-DD HH:mm:ss}</green> | <level>{level: <8}</level> | <cyan>{name}</cyan>:<cyan>{function}</cyan>:<cyan>{line}</cyan> - <level>{message}</level>",
        level=level
    )
    
    # 文件输出
    logger.add(
        log_path / "system_{time:YYYY-MM-DD}.log",
        rotation="00:00",  # 每天轮转
        retention="30 days",  # 保留30天
        compression="zip",  # 压缩旧日志
        format="{time:YYYY-MM-DD HH:mm:ss} | {level: <8} | {name}:{function}:{line} - {message}",
        level=level
    )
    
    logger.info("日志系统初始化完成")

def load_config(config_path: str = "config/config.yaml") -> Dict[str, Any]:
    """
    加载配置文件
    
    Args:
        config_path: 配置文件路径
        
    Returns:
        配置字典
    """
    config_path = Path(config_path)
    
    if not config_path.exists():
        logger.warning(f"配置文件不存在: {config_path}，使用默认配置")
        return get_default_config()
    
    try:
        with open(config_path, 'r', encoding='utf-8') as f:
            config = yaml.safe_load(f)
        logger.info(f"配置文件加载成功: {config_path}")
        return config
    except Exception as e:
        logger.error(f"配置文件加载失败: {e}")
        return get_default_config()

def get_default_config() -> Dict[str, Any]:
    """获取默认配置"""
    return {
        "model": {
            "path": "models/yolo11n.pt",
            "imgsz": 640,
            "conf": 0.25,
            "iou": 0.45,
            "device": "cuda:0"
        },
        "ui": {
            "window_size": [1200, 800],
            "theme": "dark",
            "language": "zh"
        },
        "logging": {
            "level": "INFO",
            "file_path": "logs/system.log"
        },
        "alert": {
            "enabled": True,
            "min_confidence": 0.5,
            "cooldown_time": 10
        }
    }

def save_config(config: Dict[str, Any], config_path: str = "config/config.yaml"):
    """
    保存配置文件
    
    Args:
        config: 配置字典
        config_path: 配置文件路径
    """
    config_path = Path(config_path)
    config_path.parent.mkdir(parents=True, exist_ok=True)
    
    try:
        with open(config_path, 'w', encoding='utf-8') as f:
            yaml.dump(config, f, default_flow_style=False, allow_unicode=True)
        logger.info(f"配置文件保存成功: {config_path}")
    except Exception as e:
        logger.error(f"配置文件保存失败: {e}")

def draw_detections(image: np.ndarray, detections: List[Dict], 
                   confidence_threshold: float = 0.5) -> np.ndarray:
    """
    在图像上绘制检测结果
    
    Args:
        image: 输入图像
        detections: 检测结果列表
        confidence_threshold: 置信度阈值
        
    Returns:
        绘制了检测框的图像
    """
    if not detections:
        return image
    
    image_copy = image.copy()
    
    for det in detections:
        if det.get("confidence", 0) < confidence_threshold:
            continue
        
        bbox = det.get("bbox", [])
        if len(bbox) != 4:
            continue
        
        class_name = det.get("class_name", "unknown")
        confidence = det.get("confidence", 0)
        
        # 根据类别设置颜色
        if class_name.lower() == "fire":
            color = (0, 0, 255)  # 红色
        elif class_name.lower() == "smoke":
            color = (255, 0, 0)  # 蓝色
        else:
            color = (0, 255, 0)  # 绿色
        
        x1, y1, x2, y2 = map(int, bbox)
        
        # 绘制边界框
        cv2.rectangle(image_copy, (x1, y1), (x2, y2), color, 2)
        
        # 准备标签文本
        label = f"{class_name}: {confidence:.2f}"
        
        # 计算标签尺寸
        (label_width, label_height), baseline = cv2.getTextSize(
            label, cv2.FONT_HERSHEY_SIMPLEX, 0.5, 2
        )
        
        # 绘制标签背景
        cv2.rectangle(image_copy, 
                     (x1, y1 - label_height - baseline - 5),
                     (x1 + label_width, y1),
                     color, -1)
        
        # 绘制标签文本
        cv2.putText(image_copy, label, (x1, y1 - 5),
                   cv2.FONT_HERSHEY_SIMPLEX, 0.5, (255, 255, 255), 2)
    
    return image_copy

def calculate_iou(box1: List[int], box2: List[int]) -> float:
    """
    计算两个边界框的IoU
    
    Args:
        box1: [x1, y1, x2, y2]
        box2: [x1, y1, x2, y2]
        
    Returns:
        IoU值
    """
    # 计算交集区域
    x1 = max(box1[0], box2[0])
    y1 = max(box1[1], box2[1])
    x2 = min(box1[2], box2[2])
    y2 = min(box1[3], box2[3])
    
    # 计算交集面积
    inter_area = max(0, x2 - x1) * max(0, y2 - y1)
    
    # 计算并集面积
    box1_area = (box1[2] - box1[0]) * (box1[3] - box1[1])
    box2_area = (box2[2] - box2[0]) * (box2[3] - box2[1])
    union_area = box1_area + box2_area - inter_area
    
    # 计算IoU
    iou = inter_area / union_area if union_area > 0 else 0
    
    return iou

def non_max_suppression(detections: List[Dict], iou_threshold: float = 0.5) -> List[Dict]:
    """
    非极大值抑制
    
    Args:
        detections: 检测结果列表
        iou_threshold: IoU阈值
        
    Returns:
        抑制后的检测结果
    """
    if not detections:
        return []
    
    # 按置信度降序排序
    detections = sorted(detections, key=lambda x: x.get("confidence", 0), reverse=True)
    
    keep = []
    
    while detections:
        # 取置信度最高的检测
        best = detections.pop(0)
        keep.append(best)
        
        # 移除与best重叠度高的检测
        detections = [
            det for det in detections
            if calculate_iou(best.get("bbox", []), det.get("bbox", [])) < iou_threshold
        ]
    
    return keep

def format_detection_stats(detections: List[Dict]) -> Dict[str, Any]:
    """
    格式化检测统计信息
    
    Args:
        detections: 检测结果列表
        
    Returns:
        统计信息字典
    """
    stats = {
        "total": len(detections),
        "fire_count": 0,
        "smoke_count": 0,
        "avg_confidence": 0.0,
        "detection_details": []
    }
    
    if not detections:
        return stats
    
    total_confidence = 0.0
    
    for det in detections:
        class_name = det.get("class_name", "").lower()
        confidence = det.get("confidence", 0)
        
        if class_name == "fire":
            stats["fire_count"] += 1
        elif class_name == "smoke":
            stats["smoke_count"] += 1
        
        total_confidence += confidence
        
        # 记录详细信息
        stats["detection_details"].append({
            "class": class_name,
            "confidence": confidence,
            "bbox": det.get("bbox", []),
            "timestamp": det.get("timestamp", time.time())
        })
    
    stats["avg_confidence"] = total_confidence / len(detections)
    
    return stats

def save_detection_results(detections: List[Dict], image_path: str, 
                          output_dir: str = "outputs/results"):
    """
    保存检测结果
    
    Args:
        detections: 检测结果列表
        image_path: 原始图片路径
        output_dir: 输出目录
    """
    output_dir = Path(output_dir)
    output_dir.mkdir(parents=True, exist_ok=True)
    
    # 生成输出文件名
    timestamp = datetime.now().strftime("%Y%m%d_%H%M%S")
    image_name = Path(image_path).stem
    output_name = f"{image_name}_{timestamp}"
    
    # 保存JSON格式结果
    json_path = output_dir / f"{output_name}.json"
    results = {
        "image_path": image_path,
        "detection_time": datetime.now().isoformat(),
        "detections": detections,
        "stats": format_detection_stats(detections)
    }
    
    with open(json
