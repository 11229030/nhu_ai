# YOLOv5 貓狗檢測完整流程 (Google Colab)

---

## 1️⃣ 掛載 Google Drive

```python
from google.colab import drive
drive.mount('/content/drive')
```

<img width="598" height="159" alt="a1" src="https://github.com/user-attachments/assets/0c8bfffe-74ae-49a4-9127-4e56e7b39786" />
點選連線至google雲端硬碟
<img width="778" height="273" alt="a1_1" src="https://github.com/user-attachments/assets/f7ba8cd0-5d48-4b1c-b185-70f3bf46c597" />

## 2️⃣ 下載 YOLOv5 原始碼

```python
%cd /content
!git clone https://github.com/ultralytics/yolov5.git
%cd yolov5
!pip install -r requirements.txt
```

<img width="1728" height="723" alt="a2" src="https://github.com/user-attachments/assets/7e96ef62-2e7c-4676-bce4-55990f35e9a2" />

## 3️⃣ 準備資料集
```bath
/content/drive/MyDrive/yolov5_dataset/
 ├── images/train/       # 訓練集圖片
 ├── images/val/         # 驗證集圖片
 ├── labels/train/       # 訓練集標註檔
 ├── labels/val/         # 驗證集標註檔
 └── data.yaml           # 資料集配置
```

<img width="427" height="508" alt="a3" src="https://github.com/user-attachments/assets/eb5d4462-71cf-4574-ba6f-afdaf4361763" />

### 關於圖片的標註檔
https://www.makesense.ai/
<img width="1592" height="911" alt="a3_1" src="https://github.com/user-attachments/assets/495ecb93-f3c9-47f5-993a-cc8658a9c9cd" />

詳細步驟:
1.創建標註並命名
<img width="686" height="672" alt="a3_2" src="https://github.com/user-attachments/assets/b923cfdd-6a5a-4c93-a758-4378e6899844" />
<img width="726" height="670" alt="a3_3" src="https://github.com/user-attachments/assets/88c656bd-8dcc-424e-b13b-033f355800bf" />

2.使用矩形框主體cat
<img width="1877" height="926" alt="a3_4" src="https://github.com/user-attachments/assets/708c2b98-af36-42ba-9a04-58ef5e16e787" />

3.輸出標註檔並選YOLO格式
<img width="513" height="368" alt="a3_5" src="https://github.com/user-attachments/assets/0be924c8-41a0-4654-8377-b3a97e034ea8" />
<img width="656" height="541" alt="a3_6" src="https://github.com/user-attachments/assets/ed77750d-ccda-4b30-9e25-77e3ffa60c5d" />

### 3️⃣-1️⃣ 建立 data.yaml

```python
import os

os.makedirs("/content/drive/MyDrive/yolov5_dataset", exist_ok=True# 建立資料夾（若不存在）
yaml_content = """train: /content/drive/MyDrive/yolov5_dataset/images/train
val: /content/drive/MyDrive/yolov5_dataset/images/val

nc: 2
names: ['cat', 'dog']
"""


with open('/content/drive/MyDrive/yolov5_dataset/data.yaml', 'w') as f:
    f.write(yaml_content)# 把 YAML 存成檔案

print("✅ data.yaml 已建立成功！")
```
<img width="908" height="157" alt="a3-1" src="https://github.com/user-attachments/assets/8b0a8584-f484-44b2-bc37-86930e158659" />

<img width="1456" height="487" alt="a3-1 1" src="https://github.com/user-attachments/assets/b749628a-c4ea-4e83-a96e-0867ef8a3d1b" />

### 3️⃣-2️⃣ 複製 train 跟 val（少量資料測試用）

<img width="1400" height="370" alt="a3-2" src="https://github.com/user-attachments/assets/98097f64-8cb9-45db-b22c-7058b0de25de" />

<img width="1118" height="565" alt="a3-2 1" src="https://github.com/user-attachments/assets/23d770e1-b162-46e4-a903-e6850e2cc616" />

<img width="1187" height="716" alt="a3-2 2" src="https://github.com/user-attachments/assets/78c64e29-9649-4da0-878e-517f939cad13" />

<img width="1154" height="427" alt="a3-2 3" src="https://github.com/user-attachments/assets/4b8cface-c2a3-448c-9a0a-f206e45c534e" />

## 4️⃣訓練 YOLOv5
```python
!python train.py --img 640 --batch 4 --epochs 20 --data /content/drive/MyDrive/yolov5_dataset/data.yaml --weights yolov5s.pt --cache
```
<img width="1833" height="722" alt="a4" src="https://github.com/user-attachments/assets/f923ebcc-ac29-4476-ad1a-4556989d91f0" />

<img width="1780" height="699" alt="a4_1" src="https://github.com/user-attachments/assets/4894f680-0219-4729-99f7-b1458a785565" />

## 5️⃣ 使用模型偵測圖片
```python
!python detect.py \
--weights runs/train/exp/weights/best.pt \
--img 640 \
--source /content/drive/MyDrive/yolov5_dataset/images/val
```
<img width="1844" height="641" alt="a5" src="https://github.com/user-attachments/assets/3e04c534-4f80-487e-871e-cf85cf5f5814" />

<img width="1211" height="671" alt="a5_1" src="https://github.com/user-attachments/assets/dd65abc0-c42b-4b98-93f3-92684c8b26fe" />


## 6️⃣ 查看訓練過程 (TensorBoard)
```python
%load_ext tensorboard
%tensorboard --logdir runs/train/exp3
```
<img width="558" height="81" alt="a6" src="https://github.com/user-attachments/assets/dd9b28f3-fbdb-4b03-bd74-ebbfad9cf9ad" />

<img width="1812" height="752" alt="a6_1" src="https://github.com/user-attachments/assets/6a668359-f359-4343-821c-90699d134f0d" />


<img width="1844" height="758" alt="a6_2" src="https://github.com/user-attachments/assets/7c47166b-bbd8-498f-bd2c-e1f2fc092945" />


## 7️⃣ 心得收穫

**熟悉了 YOLOv5 的完整訓練流程：資料準備、訓練、偵測、結果觀察。**

**了解資料量、標註檔與路徑對訓練的重要性。**

**雖然資料很少，但流程已掌握，未來換更多圖片就能做實用的物件偵測。**



