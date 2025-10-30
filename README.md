

````markdown
# 🏀 Basketball Analysis - Google Colab Version  

> **A full basketball game analysis system powered by YOLOv8 and OpenCV**  
> Based on: [abdullahtarek/basketball_analysis](https://github.com/abdullahtarek/basketball_analysis)

---

## ✨ Overview  

This project provides a **complete Google Colab implementation** of a basketball analysis system using **YOLO models**.  
It performs automatic player tracking, ball tracking, team color classification, and ball possession analysis — producing an annotated video showing all key metrics.  

---

## 🎯 Features  

| Feature | Description |
|----------|-------------|
| 👥 **Player Detection & Tracking** | Detect and track multiple players across frames using YOLOv8 |
| 🏀 **Ball Tracking** | Track basketball position and motion throughout the video |
| 👕 **Team Assignment** | Automatically classify players into teams using K-Means clustering on jersey colors |
| 🎯 **Court Detection (Basic)** | Identify court boundaries for perspective analysis (can be extended) |
| 🤝 **Pass Detection (Planned)** | Detect passes and interceptions between players |
| 📊 **Ball Possession** | Determine which player has the ball and track team possession time |
| 🎥 **Visual Overlays** | Annotated video output showing players, teams, ball, and possession stats |

---

## 🧠 Tech Stack  

- **Python 3.10+**
- **Google Colab (with GPU support)**
- **YOLOv8 (Ultralytics)**
- **OpenCV**
- **Scikit-Learn**
- **Pandas**
- **Supervision**
- **Transformers**

---


### **4️⃣–12️⃣ Utility Functions, Classes & Main Pipeline**

Copy all the remaining cells from the project:

* Video utilities
* Bounding box helpers
* `PlayerTracker` class
* `BallTracker` class
* `TeamAssigner` class
* `BallPossessionAssigner` class
* Drawing utilities
* Main pipeline
* Annotation and saving functions

---

## 🚀 How to Run the Project

1. Open **Google Colab** → [https://colab.research.google.com](https://colab.research.google.com)
2. Enable GPU:

   * Go to **Runtime → Change runtime type → Hardware accelerator → GPU**
3. Copy and paste all 12 code sections (cells) in order
4. Upload your basketball video when prompted
5. Run all cells sequentially
6. Wait for processing (10–20 minutes for a 30-second clip)
7. Watch the **annotated output video** directly inside Colab

---

## 📊 Output Features

✅ Player bounding boxes with unique IDs
✅ Automatic team color detection
✅ Ball tracking and interpolation
✅ Possession visualization (triangles and overlays)
✅ Team possession percentage on video
✅ Reusable `.pkl` detection files for faster reruns

---

## ⚡ Performance Tips

| Setting                  | Description                                                      |
| ------------------------ | ---------------------------------------------------------------- |
| **GPU**                  | Enable GPU runtime for faster processing                         |
| **Short videos**         | Use 10–30 second clips for testing                               |
| **Confidence threshold** | Adjust `conf=0.15` in `BallTracker` for sensitivity              |
| **Possession distance**  | Change `max_player_ball_distance=70` in `BallPossessionAssigner` |
| **Frame rate**           | Modify `fps=24` in `save_video()` if needed                      |
| **Stub files**           | Set `read_from_stub=True` to skip re-detection                   |

---

## 📦 Output Example

The final output is:

* `output_basketball_analysis.mp4` – Annotated game video
* `player_detections.pkl` – Saved player detection data
* `ball_detections.pkl` – Saved ball detection data

---

## 🧩 Project Structure

```
basketball_analysis_colab/
│
├── 🧾 README.md
├── 🏀 output_basketball_analysis.mp4
├── 📦 player_detections.pkl
├── 📦 ball_detections.pkl
└── 📓 basketball_analysis_colab.ipynb
```

---

## 🧠 Future Enhancements

* 🔹 Add **pass detection** and interception logic
* 🔹 Implement **court keypoint detection** for perspective correction
* 🔹 Calculate **player speed, distance, and heatmaps**
* 🔹 Export detailed stats as **CSV or JSON**

---


---

⭐ **If you find this project helpful, please give it a star on GitHub!** ⭐

```

---

Would you like me to make this **README include example screenshots / sample output video preview (Markdown embeds)** for GitHub display? I can add a section like “📸 Sample Output” with image/video placeholders for better presentation.
```
