# Rebound: Interactive Fitness Guide

## Overview

Rebound is an AI-powered Medical Diagnosis & Rehabilitation Support system designed to help injured athletes recover and restart their journeys. It integrates real-time injury detection, personalized treatment recommendations, and price optimization to ensure fair and effective care built with React, TypeScript, and Tailwind CSS. It helps users with fitness recommendations, injury detection, and recovery predictions.

## Features
- **AI-Powered Fitness Chatbot with Groq + Pathways Technology**: Get personalized fitness guidance.
- **Injury Detection**: Identify potential injuries based on symptoms.
- **Recovery Prediction**: Estimate recovery time and suggest rehabilitation exercises.
- **Modern UI**: Built with Tailwind CSS for a sleek and responsive design.

## Tech Stack
- **Frontend**: React, TypeScript, Vite
- **Styling**: Tailwind CSS
- **Linting**: ESLint
- **Build Tool**: Vite

## Installation
To set up the project locally, follow these steps:

### Prerequisites
Ensure you have the following installed:
- Node.js (>= 16.x)
- npm or yarn

### Steps
1. Clone the repository:
   ```bash
   git clone https://github.com/Edward876/Rebound.git
   cd Rebound
   ```
2. Install dependencies:
   ```bash
   npm install
   ```
   or
   ```bash
   yarn install
   ```
3. Start the development server:
   ```bash
   npm run dev
   ```
   or
   ```bash
   yarn dev
   ```
4. Open the application in your browser at `http://localhost:5173/`.

## Project Structure
```
Rebound-main/
├── src/
│   ├── components/
│   │   ├── FitnessChat.tsx
│   │   ├── Header.tsx
│   │   ├── Hero.tsx
│   │   ├── InjuryDetection.tsx
│   │   ├── RecoveryPrediction.tsx
│   ├── App.tsx
│   ├── main.tsx
│   ├── index.css
│   ├── vite-env.d.ts
├── public/
├── .gitignore
├── package.json
├── package-lock.json
├── tailwind.config.js
├── tsconfig.json
├── vite.config.ts
└── README.md
```
-----
## Models
### Injury Detection 
#### Knee OsteoOrthoritis

1. Dataset & Preprocessing

- The dataset consists of labeled knee X-ray images categorized into different severity levels:

  - Healthy

  - Doubtful

  - Minimal

  - Moderate

  - Severe

- Classes like Minimal and Doubtful were removed to focus on clearer classification.

- The dataset was preprocessed by normalizing pixel values and augmenting images to balance class distribution.

2. Deep Learning Model

- Base Model: EfficientNetB5, pretrained on ImageNet, used for feature extraction.

- Custom Layers:

  - Batch Normalization for stabilization.

  - Fully Connected Layers with dropout to prevent overfitting.

  - Softmax Output Layer for multi-class classification.

  - Optimizer: Adamax, with a learning rate of 0.001.

  - Loss Function: Categorical Cross-Entropy.

3. Training & Validation

- Data Augmentation (rotation, flipping, and zooming) was applied to increase dataset diversity.

- The model was trained for 40 epochs, achieving high accuracy.

- The validation dataset ensured proper generalization before testing.

4. Evaluation & Performance

- The model was tested on an unseen dataset, achieving a test accuracy of 95.84%.

- Performance was evaluated using a confusion matrix and classification report.

5. Model Deployment & Future Scope

- The trained model (best_knee_effnetb5.keras) can be integrated into medical diagnostic applications.
- Deployed in HuggingFace with Gradio Interface [Knee Model Link](https://huggingface.co/spaces/Shinichi876/knee_osteoarthritis/tree/main)
#### Spine Injury 

1. Data Preprocessing 
- **Corrupted Image Removal**: A function scans and removes any unreadable or damaged images.  
- **Data Augmentation**:
  - Rotation (±30°)
  - Width & Height Shift
  - Shear Transformation
  - Zoom (Up to 30%)
  - Brightness Adjustments
  - Horizontal & Vertical Flip  
- **Dataset Split**: 80% Training, 20% Validation  

2. Model Architecture  
- **Base Model**: ResNet50 (Pretrained on ImageNet)  
- **Fine-Tuning**: Last 30 layers unfrozen for better feature learning.  
- **Custom Layers**:
  - **Global Average Pooling** for dimensionality reduction.  
  - **Fully Connected Layers** (512 → 256 → 128).  
  - **Batch Normalization** for stability.  
  - **Dropout (40%)** for regularization.  
  - **Softmax Output Layer** (for multi-class classification).  

3. Training & Optimization  
- **Optimizer**: Adam (Learning rate = 1e-4).  
- **Loss Function**: Categorical Cross-Entropy.  
- **Callbacks**:
  - **Early Stopping** (patience=7)
  - **Model Checkpoint** (saves best-performing model)
  - **Reduce Learning Rate on Plateau**  
- **Epochs**: 30  

4. Evaluation & Results  
- The trained model was tested on **20 random images** from the validation set.
- **Overall Accuracy**: **86.0%**
- **Class-wise Performance (F1-Score)**:
  - C-Spine Dislocation: **95%**
  - C-Spine Fracture: **95%**
  - C-Spine Normal: **91%**
  - Chest Flail: **78%**
  - Chest Hemothorax: **82%**
  - Chest Normal: **87%**
  - Chest Pneumothorax: **82%**
  - Pelvis Fracture: **83%**
  - Pelvis Normal: **75%**

#### Deployment  
- The model is saved as **"best_spine_model.keras"** and can be used for real-time predictions.  
- Predictions are visualized with **Matplotlib**, displaying the classified injury type with confidence scores.  
- Deployed in HuggingFace With Gradio interface [Spine Model](https://huggingface.co/spaces/Shinichi876/spineinjury)

### FITNESS CHATBOT USING PATHWAYS AND GROQ

## Key Technologies  

### **1. Pathway (PW.io) for Efficient Data Processing**  
**Pathway** is used for handling real-time data processing. It enables seamless integration with structured data formats like CSV and ensures efficient **schema-based data loading**.  
- **Schema Definition**: Pathway enforces a structured schema (`FitnessSchema`) for processing fitness-related Q&A pairs.  
- **CSV Parsing & Processing**: The chatbot loads `Fitness_Guide_Dataset.csv` in **static mode**, converting it into a **Pathway table** and then into a Pandas DataFrame for further processing.  

> 🚀 Pathway is excellent for streaming data pipelines and real-time processing, but here, it is used in a static mode to load structured fitness Q&A data.  

### **2. Groq API for AI-Powered Responses**  
**Groq** is an advanced AI-powered API optimized for **low-latency, high-performance large language models (LLMs)**. In this chatbot, Groq is responsible for **enhancing responses** retrieved from the dataset by:  
- **Understanding Context**: The model generates responses in a conversational manner while ensuring domain-specific accuracy.  
- **Fine-Tuned Creativity**: A **temperature setting of 0.7** ensures that responses are **varied and engaging**.  
- **Limiting Scope**: The chatbot restricts discussions to fitness and healthcare, preventing it from responding to **irrelevant queries**.  

The **Groq AI API call** works as follows:  
1. The **retrieved answer** from the dataset is **fed into Groq's API**.  
2. A **structured system prompt** ensures the AI-generated response remains **on-topic and engaging**.  
3. The model **mixtral-8x7b-32768** is used for generating responses with a token limit of **2000**.  

> 🏋️ **Groq is ideal for performance-driven AI applications**, ensuring **fast and high-quality** responses.  

## Workflow of the Chatbot  
1. **User Inputs Query**: The chatbot takes a **fitness or nutrition-related question** as input.  
2. **Pathway Retrieves Best-Matching Answer**:  
   - The chatbot uses **TF-IDF Vectorization** and **Cosine Similarity** to find the **closest matching question** from the dataset.  
3. **Groq API Enhances the Response**:  
   - The best-matching answer is fed into **Groq's LLM**, which **refines, enhances, and personalizes** the response.  
4. **User Receives a Comprehensive Answer**:  
   - The chatbot returns the **Groq-enhanced** response in a conversational style.  

## **Deployment with Gradio**  
To make the chatbot **user-friendly**, **Gradio** provides an **interactive interface**, allowing users to ask fitness-related questions.  
- **Input**: Textbox for user queries.  
- **Output**: AI-generated response.  
- **Title & Description**: Clearly states the chatbot's purpose and functionalities.  
- **Web Interface**: The chatbot runs **locally or on the web** with `interface.launch()`.  

## **Advantages of Using Pathway & Groq**  
✅ **Fast Data Retrieval**: Pathway allows structured retrieval, ensuring **efficient Q&A matching**.  
✅ **High-Performance AI Responses**: Groq’s optimized LLM **enhances responses** dynamically.  
✅ **Scalability**: The system can **scale** by expanding datasets or integrating **real-time updates**.  
✅ **Domain-Specific Intelligence**: Ensures **context-aware** and **relevant** fitness-related answers.  

**Check the model here - [FITNESS CHATBOT](https://huggingface.co/spaces/atharvbindal/fitness_chatbot/tree/main)**
-------
- Spine Injury Model - https://huggingface.co/spaces/Shinichi876/spineinjury
- knee Injury Model - https://huggingface.co/spaces/Shinichi876/knee_osteoarthritis/tree/main
- RAG chatbot with PathWay package + Groq API - https://huggingface.co/spaces/atharvbindal/fitness_chatbot/tree/main
- Recovery prediction - https://huggingface.co/spaces/Shinichi876/rec_new/tree/main

------- 
## Contributing
Contributions are welcome! To contribute:
1. Fork the repository.
2. Create a new branch (`feature-xyz`).
3. Commit your changes.
4. Push to your fork and submit a pull request.
## Contributors 

1. Pragyan Pant 
2. Atharv Bindal  
3. Sloke Bhakat 
