# Blood Cell Classification Web Application

This repository contains an end-to-end deep learning web application that classifies blood cell images as either Eosinophil, Lymphocyte, Monocyte, or Neutrophil. The application is built with Python and Flask, using a pre-trained convolution neural network (CNN) based on VGG16 model fine-tuned on a dataset of blood cell images. It is designed for easy deployment on AWS infrastructure with continuous integration and deployment (CI/CD) using GitHub Actions.

---

## Table of Contents
- [Project Overview](#project-overview)
- [Dataset](#dataset)
- [Features](#features)
- [Technologies Used](#technologies-used)
- [Setup and Installation](#setup-and-installation)
- [Directory Structure](#directory-structure)
- [Model Architecture](#model-architecture)
- [Pipeline Stages](#pipeline-stages)
- [ASW Deployment](#deployment)
- [Usage](#usage)
- [Development Setup](#development-setup)
- [Future Work](#future-work)

---

## Overview

This project aims to classify blood cell images to support medical diagnosis. The web application allows users to upload an image of a blood cell, which is then classified as one of four types:
- Eosinophil
- Lymphocyte
- Monocyte
- Neutrophil

The model was trained on blood cell image data stored in an **AWS S3 bucket** and developed using **Python**, **Keras**, and **TensorFlow**.

## Dataset

The dataset for model training and evaluation was sourced from an AWS S3 bucket. It contains labeled images of blood cells categorized into the four classes. 

## Model Architecture

The model is based on **VGG16** pretrained on the **ImageNet** dataset. The final fully connected layers of VGG16 were replaced with:
- A **Flatten layer**
- A **Dense SoftMax layer** with 4 units, corresponding to the 4 blood cell classes.

This modified model architecture leverages transfer learning for improved accuracy with a relatively small dataset. The network was fine-tuned using the ingested blood cell images, and evaluation was performed on a validation dataset to measure model accuracy.

## Pipeline Stages

The project follows a well-defined pipeline using **DVC** to organize data and model pipelines. Pipeline stages include:
1. **Data Ingestion**: Downloads the dataset from S3 to a local storage.
2. **Base Model Development**: Defines the base CNN model architecture (VGG16 with transfer learning).
3. **Model Training**: Trains the modified VGG16 model with the blood cell images.
4. **Model Evaluation**: Evaluates the model on the validation set to assess accuracy.

## Deployment

### Infrastructure

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

## Development Setup

To set up the project for development or testing, follow these steps:

### Prerequisites
- **Docker**: Ensure Docker is installed on your local machine.
- **AWS CLI**: Set up AWS CLI with access credentials for EC2 and ECR.
- **Python**: Install Python 3.8 or later with dependencies listed in `requirements.txt`.

### Steps
1. **Clone Repository**:
   ```bash
   git clone https://github.com/username/repo-name.git
   cd repo-name

