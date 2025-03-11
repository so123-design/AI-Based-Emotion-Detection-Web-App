# AI-Based Emotion Detection Web App  

#### 📌 Project Overview  
This project is an AI-powered web application that analyzes customer feedback and detects the emotions expressed in the text. Using natural language processing (NLP) techniques, the system determines the emotional sentiment behind a given statement, classifying it into categories such as **joy, anger, sadness, fear, and disgust**.  

The application is built using **Flask** for the backend, integrates **IBM Watson’s NLP API** for emotion detection, and includes **unit testing** to ensure accuracy.  

---

## 🚀 Features  
✅ Analyze customer feedback and detect emotions  
✅ Categorize emotions into **joy, sadness, anger, disgust, and fear**  
✅ Web-based interface for easy interaction  
✅ API-powered backend for accurate sentiment analysis  
✅ Unit testing for reliability  

---

## 🏗️ Technologies Used  
- **Flask** – Backend framework  
- **IBM Watson NLP API** – Emotion detection  
- **Requests** – API integration  
- **UnitTest** – Testing framework  

---

## 🔧 Project Structure  
```
EmotionDetection-WebApp/
│── EmotionDetection/
│   ├── __init__.py
│   ├── emotion_detection.py  # Emotion detection logic
│── templates/
│   ├── index.html  # Web UI
│── test/
│   ├── test_emotion_detection.py  # Unit tests
│── server.py  # Flask backend
│── requirements.txt  # Dependencies
│── README.md  # Documentation
```

---

## 📜 Code Implementation  

### 1️⃣ Backend - `server.py`  
This file sets up the **Flask** web server and defines the routes for handling user input and returning emotion detection results.

```python
from flask import Flask, render_template, request
from EmotionDetection.emotion_detection import emotion_detector

app = Flask("Emotion Detector")

@app.route("/emotionDetector")
def emotion_detector_function():
    '''Processes user input and returns detected emotions.'''
    text_to_analyze = request.args.get('textToAnalyze')
    response = emotion_detector(text_to_analyze)

    if response['dominant_emotion'] is None:
        response_text = "Invalid Input! Please try again."
    else:
        response_text = f"For the given statement, the system response is 'anger': \
                    {response['anger']}, 'disgust': {response['disgust']}, \
                    'fear': {response['fear']}, 'joy': {response['joy']}, \
                    'sadness': {response['sadness']}. The dominant emotion is \
                    {response['dominant_emotion']}."

    return response_text

@app.route("/")
def render_index_page():
    ''' Renders the HTML interface for user input '''
    return render_template('index.html')

if __name__ == "__main__":
    app.run(host="0.0.0.0", port=5000)
```

---

### 2️⃣ Emotion Detection Logic - `emotion_detection.py`  
This file sends the user's input text to **IBM Watson NLP API** and retrieves the emotion analysis.

```python
import requests
import json

def emotion_detector(text_to_analyse):
    url = 'https://sn-watson-emotion.labs.skills.network/v1/watson.runtime.nlp.v1/NlpService/EmotionPredict'
    headers = {"grpc-metadata-mm-model-id": "emotion_aggregated-workflow_lang_en_stock"}
    myobj = {"raw_document": {"text": text_to_analyse}}
    
    response = requests.post(url, json=myobj, headers=headers)
    
    if response.status_code == 400:
        return {
            'anger': None, 'disgust': None, 'fear': None,
            'joy': None, 'sadness': None, 'dominant_emotion': None
        }

    res = json.loads(response.text)
    formatted_response = res['emotionPredictions'][0]['emotion']
    formatted_response['dominant_emotion'] = max(formatted_response, key=lambda x: formatted_response[x])

    return formatted_response
```

---

### 3️⃣ Unit Testing - `test_emotion_detection.py`  
To ensure our model is correctly identifying emotions, we wrote **unit tests** using Python’s `unittest` module.

```python
import unittest
from EmotionDetection.emotion_detection import emotion_detector
 
class TestEmotionDetector(unittest.TestCase):
    def test_emotion_detector(self):
        self.assertEqual(emotion_detector("I am glad this happened")['dominant_emotion'], 'joy')
        self.assertEqual(emotion_detector("I am really mad about this")['dominant_emotion'], 'anger')
        self.assertEqual(emotion_detector("I feel disgusted just hearing about this")['dominant_emotion'], 'disgust')
        self.assertEqual(emotion_detector("I am so sad about this")['dominant_emotion'], 'sadness')
        self.assertEqual(emotion_detector("I am really afraid that this will happen")['dominant_emotion'], 'fear')

unittest.main()
```

---

## ✅ How to Run the Project  
1️⃣ **Install Dependencies**  
```bash
pip install -r requirements.txt
```

2️⃣ **Run the Flask Server**  
```bash
python server.py
```

3️⃣ **Open in Browser**  
Navigate to `http://127.0.0.1:5000/` and enter your text to analyze emotions.

---

## 📌 Results & Analysis  
The system successfully detects emotions in text input and categorizes them. Here’s an example output:

**Input:**  
📝 *"I am extremely happy about this!"*  

**Output:**  
🎭 Joy: `0.85`, Sadness: `0.02`, Anger: `0.01`, Fear: `0.03`, Disgust: `0.02`  
➡️ **Dominant Emotion: Joy**

---

## 🎯 Conclusion  
This project showcases **AI-driven emotion detection** using **IBM Watson NLP** integrated into a **Flask web app**. With unit testing and an interactive interface, it provides a great example of NLP-based sentiment analysis for businesses to evaluate customer feedback.  
