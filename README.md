# Blood Cell Classification Web Application
![Bood Cell Types](BloodCellImage.jpg)

This repository contains an end-to-end deep learning web application that classifies blood cell images as either Eosinophil, Lymphocyte, Monocyte, or Neutrophil. The application is built with Python and Flask, using a pre-trained VGG16 model fine-tuned on a dataset of blood cell images. It is designed for easy deployment on AWS infrastructure with continuous integration and deployment (CI/CD) using GitHub Actions.

---

## Table of Contents
- [Project Overview](#project-overview)
- [Dataset](#dataset)
- [Directory Structure](#directory-structure)
- [Features](#features)
- [Technologies Used](#technologies-used)
- [Pipeline Stages](#pipeline-stages)
- [Setup and Installation](#setup-and-installation)
- [AWS Deployment](#deployment)
- [Usage](#usage)
- [Future Work](#future-work)

---

## Overview

Diagnosing blood-related diseases often requires identifying and analyzing patient blood samples. Automated techniques for detecting and classifying blood cell subtypes play a crucial role in advancing medical diagnostics. This project aims to classify blood cell images to support medical diagnosis. The web application allows users to upload an image of a blood cell, which is then classified as one of four types:
- Eosinophil
- Lymphocyte
- Monocyte
- Neutrophil

This application classifies blood cell images using a Convolutional Neural Network (CNN) model fine-tuned from the VGG16 model pre-trained on the ImageNet dataset. The model was modified to include a flatten layer and a softmax dense layer with 4 output units for multi-class classification. Blood cell images are served to the model through a web app built with Flask, and the application is fully containerized using Docker and hosted on an AWS EC2 instance.

## Dataset

The dataset used for training and evaluating the model was sourced from [Kaggle](https://www.kaggle.com/datasets/paultimothymooney/blood-cells/data). This dataset contains augmented images of blood cells (JPEG) for each of 4 different cell types grouped into 4 different folders (according to cell type). The compressed dataset was stored in AWS S3 and downloaded from there for model training and evaluation. 

## Directory Structure

```
.
├── .github/                  
│   └── workflows/
│       └── main.py
├── artifacts/
│   ├── data_ingestion/
│   ├── prepare_base_model/
│   ├── prepare_callbacks/
│   └── training/
├── config/
│   └── config.yaml
├── log/
│   └── running_logs.log        
├── research/
│   ├── 01_data_ingestion.ipynb
│   ├── 02_prepare_base_model.ipynb
│   ├── 03_prepare_callbacks.ipynb
│   ├── 04_training.ipynb
│   └── 05_model_evaluation.ipynb
├── src/                  
│   └── bloodcellClassifier/
│       ├── components/
│       ├── config/
│       ├── constants/
│       ├── entity/
│       ├── pipeline/
│       └── utils
├── templates/                        # HTML templates               
│   └── index.html
├── app.py                            # Main Flask application
├── Dockefile                         # Docker configuration file    
├── dvc.yaml                          # DVC pipeline stages
├── main.py            
├── params.yaml              
├── README.md                         # Project documentation
├── requirements.txt                  # Python dependencies
├── scores.json      
├── setup.py             
└── template.py             
```

## Features

- **Transfer Learning**: Leveraging VGG16 pretrained on ImageNet for efficient classification.
- **Automated Pipeline**: Data ingestion, model training, and evaluation managed with DVC.
- **Web Application**: Flask-based interface for uploading and classifying blood cell images.
- **CI/CD Deployment**: Docker, GitHub Actions, and AWS integration for continuous deployment.
- **AWS Hosted**: Application hosted on an EC2 instance with Docker image pulled from ECR.

## Technologies Used

- **Python**: Data processing, modeling, and backend application.
- **TensorFlow / Keras**: CNN model architecture, transfer learning, and training.
- **Flask**: Web application framework for serving the model.
- **DVC**: Pipeline management for reproducible machine learning.
- **Docker**: Containerization for consistent deployment across environments.
- **GitHub Actions**: CI/CD pipeline for automatic deployment on AWS.
- **AWS (ECR, EC2, IAM)**: Cloud infrastructure for hosting and managing the application.

## Pipeline Stages

The project follows a well-defined pipeline using **DVC** to organize data and model pipelines. Pipeline stages include:
1. **Data Ingestion**: Downloads the dataset from S3 to a local storage.
2. **Base Model Development**: Defines the base CNN model architecture (VGG16 with transfer learning).
3. **Model Training**: Trains the modified VGG16 model with the blood cell images.
4. **Model Evaluation**: Evaluates the model on the validation set to assess accuracy.

## Setup and Installation

### Prerequisites
- Python 3.10+
- Docker
- AWS Account with IAM permissions for EC2 and ECR access
- GitHub Account

### Local Setup
1. **Clone the repository**:
```
git clone https://github.com/OlumideOlumayegun/Blood-Cells-Classification-Using-CNN-DVC-Pipeline.git
cd Blood-Cells-Classification-Using-CNN-DVC-Pipeline
```
2. **Install dependencies**:
```
pip install -r requirements.txt
```
3. **Run the application locally**:
```
python app.py
```
Visit http://localhost:8080 in your browser to access the application.

### Docker Setup
1. **Build the Docker image**:
```
docker build -t blood-cell-classification .
```
2. **Run the Docker container**:
```
docker run -p 8080:8080 blood-cell-classification
```

## AWS Deployment

### Infrastructure Setup

The application is hosted on AWS infrastructure, with the following setup:
1. **IAM User**: Configured with specific EC2 and ECR access policies.
2. **ECR Repository**: Stores the Docker image of the application.
3. **EC2 Instance**: Serves as the self-hosted runner for the GitHub Actions CI/CD pipeline and runs the application in a Docker container.

### CI/CD Workflow

The **GitHub Actions** workflow automates the CI/CD process as follows:
1. **Build Docker Image**: Creates a Docker image of the source code.
2. **Push Image to ECR**: Uploads the Docker image to AWS ECR.
3. **Launch EC2 Instance**: If not already running, launches an EC2 instance.
4. **Deploy to EC2**: Pulls the image from ECR to the EC2 instance and runs the application container.

**GitHub Secrets** are used to securely store AWS credentials, and changes pushed to the main branch automatically trigger the deployment pipeline.

## Usage

1. **Upload Image**: Access the web application hosted on the EC2 instance and upload an image of a blood cell.
2. **View Prediction**: The application will classify the cell as an eosinophil, lymphocyte, monocyte, or neutrophil and display the result.



