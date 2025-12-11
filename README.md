# Emotion Recognition Project  
利用 DeepFace 與 FER 模型，分別對 **台灣人臉情緒資料集**、**vlog 影片** 與 **自選影片** 進行情緒辨識。

---

# 📌 Contents  
1. **Part 1 — Taiwanese Dataset Emotion Classification（Taiwanese_face.ipynb）**  
2. **Part 3 — My Chosen Video Emotion Recognition（My_chosen_videos.ipynb）**  
3. **Part 2 — Vlog Emotion Recognition（vlog.ipynb）**  

並附上模型輸出的 CSV：  
- `test_emotions.csv`  
- `vlog_emotions.csv`

---

# #️⃣ Part 1 — Taiwanese Dataset Emotion Classification  
使用 DeepFace 的情緒模型（7-class FER）對 **Taiwanese dataset** 進行推論，並與 ground truth 做比較。

---

## 📁 Dataset Description  
- 資料來自 Taiwanese Facial Expression Dataset  
- Excel：`Image_info.xls`  
- 每張圖片具備觀察者投票結果（counterMax、entropyVal）  
- 本作業以 `maxIntCategory` 作為 ground truth  
- Label mapping：

| Label | Emotion |
|-------|---------|
| 1 | Happy |
| 2 | Sad |
| 3 | Angry |
| 4 | Disgust |
| 5 | Fear |
| 6 | Surprise |

---

## ✔️ Confusion Matrix  

![Taiwanese Confusion Matrix](/result/taiwanese_confmat.png)

📌 **Model Accuracy = 0.52428**

---

## 🧠 **Result Discussion（討論與反思）**

### 1️⃣ 整體準確率不高（約 52%）  
DeepFace 原始模型是在 **西方臉孔** 上訓練，直接用在 **台灣人臉（東亞）** 的辨識效果會受到 domain gap 影響。  
→ 臉部外觀特徵不同、文化差異造成表情強度不同，都會降低辨識效果。

### 2️⃣ 模型偏向預測 “Happy”  
Confusion Matrix 可以看到：  
- 許多非 “Happy” 的圖片（如 Sad、Angry、Disgust）也常被預測為 “Happy”  
- 表示模型可能把 **微笑或眼型特徵當成快樂訊號**

### 3️⃣ Angry / Disgust / Fear 幾乎辨識不到  
這些情緒通常表現更 subtle（不明顯）  
→ 同時亞洲臉孔的皺眉、嘴角動作較細微，模型更難捕捉

### 4️⃣ Surprise 辨識最佳  
因為 surprise 的表情高度一致（張大眼睛、張嘴）  
→ 影像中特徵容易與訓練資料相符

### ⭐ 總結  
DeepFace 對 east-asian face 的 domain generalization 限制明顯，需要：  
- 使用台灣人 / 亞洲人臉重新訓練  
- 或使用 FER2013+RAF-DB 等更廣泛 dataset 訓練的模型（如 SOTA CNN / ViT）

---

# #️⃣ Part 2 — Emotion Recognition on My Chosen Video  
（My_chosen_videos.ipynb）

影片：`test.mp4`

---

## 🎞️ Video Information  


![Test Video Info](/result/test_video_info.png)

---

## 📊 Emotion Distribution  


![Test Emotion Distribution](/result/test_emotion_distribution.png)

---

## 📈 Emotion Timeline (1 FPS)  


![Test Emotion Timeline](/result/test_emotion_timeline.png)

---

## 🧠 **Result Discussion（討論與反思）**

### 1️⃣ 中性（Neutral）與悲傷（Sad）占多數  
這可能受影片本身情緒內容影響，例如：  
- 光線不足  
- 臉部表情不明顯  
- 主角語氣沉穩  

使模型更容易預測為 neutral 或 sad。

### 2️⃣ 模型可能受到拍攝角度影響  
若主角未面向鏡頭（側臉、低頭、遠距離）  
→ 人臉偵測失敗或表情特徵不足

### 3️⃣ Timeline 可清楚呈現情緒變化  
例如：  
- 中間段落出現較多 happy  
- 結尾逐漸回到 neutral  

這可用於分析影片敘事節奏、情緒曲線。

---

# #️⃣ Part 3 — Emotion Recognition on vlog.mp4  
（vlog.ipynb）

影片：`vlog.mp4`

---

## 🎞️ Video Information  


![Vlog Video Info](/result/vlog_video_info.png)

---

## 📊 Emotion Distribution  


![Vlog Emotion Distribution](/result/vlog_emotion_distribution.png)

---

## 📈 Emotion Timeline (1 FPS)  


![Vlog Emotion Timeline](/result/vlog_emotion_timeline.png)

---

## 🧠 **Result Discussion（討論與反思）**

### 1️⃣ “Sad” 明顯占最大宗  
這與影片內容（運動選手訪問、激動落淚情緒）一致  
→ 模型辨識與語境相符，具合理性

### 2️⃣ Neutral / Happy 也出現少量  
說話時臉部肌肉自然放鬆 → Neutral  
提到努力與感謝時表現微笑 → Happy

### 3️⃣ 模型可能放大 “Sad” 特徵  
落淚、眼眶濕潤、眉型變化使模型更容易判定為 Sad  
→ 但仍需注意模型可能 over-sensitive

### 4️⃣ Timeline 呈現情緒動態變化  
前段：沉重（Sad）  
中段：中性與少量 happy 交替  
後段：情緒起伏大、sad 再度增加  

這可以用來分析人物訪談的敘事與情緒高點。

---

# 📌 Final Summary  

| Part | File | Task | Output | Insight |
|------|------|------|--------|---------|
| Part 1 | Taiwanese_face.ipynb | 圖像分類 | Confusion matrix | 模型偏向 Happy，domain gap 顯著 |
| Part 3 | My_chosen_videos.ipynb | 自選影片辨識 | Distribution + Timeline | Neutral/Sad 佔多，角度光線影響大 |
| Part 2 | vlog.ipynb | 影片情緒辨識 | Distribution + Timeline | Sad 佔多，與情境符合 |

---

