
#  Basketball AI: Detect, Track, and Identify Players

This project is a complete computer vision pipeline to analyze basketball game footage. It uses a series of advanced AI models to automatically detect players, track their movement, segment them from the background, cluster them into teams, and read their jersey numbers to identify them by name.

This implementation is based on the [Roboflow Basketball AI Notebook](https://colab.research.google.com/github/roboflow/notebooks/blob/main/notebooks/basketball-ai-how-to-detect-track-and-identify-basketball-players.ipynb).

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/YOUR_USERNAME/YOUR_REPOSITORY/blob/main/basketball-ai-how-to-detect-track-and-identify-basketball-players.ipynb)
*(Note: You'll need to update the Colab badge link to point to your own repository after you upload it.)*

## 🎥 Final Result

![Demo GIF of basketball player tracking and identification]([Insert-Your-Demo.gif])
*(**Important:** Replace this line with a GIF of your final output video!)*

---

## ✨ Core Features

* **Player Detection:** Identifies all players on the court in a given frame.
* **High-Fidelity Tracking:** Tracks each detected player's movement across the entire video, even in complex scenes.
* **Unsupervised Team Clustering:** Automatically groups players into two teams based on their jersey colors without needing pre-labeled team data.
* **Jersey Number Recognition:** Uses a specialized Vision Language Model (VLM) to read the numbers off player jerseys.
* **Player Identification:** Matches the validated jersey number and team to a provided roster to identify each player by name.

---

## 🛠️ Tech Stack & AI Pipeline

This project combines several state-of-the-art models:

1.  **Player/Number Detection:** A fine-tuned **RF-DETR** model from Roboflow is used to detect `player` and `number` classes.
2.  **Player Tracking:** **SAM2.1 (Segment Anything Model 2.1)** is used for high-accuracy, real-time segmentation and tracking.
3.  **Team Clustering:** An unsupervised approach using:
    * **SigLIP:** To generate powerful image embeddings for player crops.
    * **KMeans:** To cluster those embeddings into two distinct teams.
4.  **Number Recognition:** A fine-tuned **SmolVLM2**, a specialized VLM, acts as an OCR model to read the jersey numbers from cropped images.
5.  **Core Libraries:** `supervision` for annotations and video processing, `roboflow-inference`, `torch`, and `google-colab`.

---

## 🚀 How to Run

This project is designed to be run in a Google Colab notebook.

### 1. Prerequisites

You will need API keys from two services:

* **Roboflow API Key:** To download the detection and recognition models.
    * Get it here: [app.roboflow.com/settings/api](https://app.roboflow.com/settings/api)
* **Hugging Face Token:** To download the SigLIP model for team clustering.
    * Get it here: [huggingface.co/settings/tokens](https://huggingface.co/settings/tokens)

### 2. Colab Setup

1.  **Open the Notebook:** Click the "Open in Colab" badge at the top of this README.
2.  **Set GPU Runtime:** In Colab, go to **Runtime** -> **Change runtime type** and select **T4 GPU** or **L4 GPU**.
3.  **Configure Secrets:**
    * In the Colab sidebar, click the **Secrets (🔑)** tab.
    * Create a new secret named `HF_TOKEN` and paste your Hugging Face Token as the value.
    * Create another secret named `ROBOFLOW_API_KEY` and paste your Roboflow API Key.
4.  **Run All Cells:** Run the cells sequentially from top to bottom.

---

## ⚙️ How It Works: The Pipeline Explained

The project runs in several distinct stages:

### Step 1: Team Clustering (Unsupervised)

Before processing the video, the system first builds a team classifier.
1.  It samples player crops from all available source videos.
2.  It uses **SigLIP** to generate vector embeddings for each player crop.
3.  **KMeans** clustering is applied to these embeddings to separate them into two groups (Team 0 and Team 1) based on visual similarity (i.e., jersey color).
4.  You manually map these cluster IDs (0 and 1) to the correct team names (e.g., "Boston Celtics" and "New York Knicks").

### Step 2: Initialization (First Frame)

The main pipeline is initialized on the *first frame* of the target video.
1.  **RF-DETR** detects all players.
2.  The team classifier from Step 1 assigns each player to a team.
3.  These initial player bounding boxes are fed as prompts to the **SAM2.1** tracker to begin tracking each player.

### Step 3: Track, Detect, and Recognize (Video Loop)

For every subsequent frame in the video:
1.  **SAM2.1** tracks the players, generating a precise segmentation mask for each one.
2.  For efficiency, number recognition runs every 5 frames:
    * **RF-DETR** detects all jersey numbers visible in the frame.
    * **SmolVLM2** reads the text (the number) from each detected number box.
    * A **Mask IoS (Intersection over Smaller)** metric is used to match the detected numbers to their corresponding player masks.
3.  A `PropertyValidator` class collects these number readings. A player's number is only considered "validated" after it has been read *identically* for 3 consecutive detections, filtering out errors.

### Step 4: Final Annotation

The final video is generated by drawing the validated data onto each frame.
* The **SAM2.1** mask is used to color-code the player based on their team.
* A label is added showing the validated jersey number and the corresponding player's name, pulled from the `TEAM_ROSTERS` dictionary.


* This project is based on the **Basketball AI** notebook created by [Roboflow](https://roboflow.com/).
* Models and code for [SAM2.1](https://github.com/Gy920/segment-anything-2-real-time) are used for tracking.
