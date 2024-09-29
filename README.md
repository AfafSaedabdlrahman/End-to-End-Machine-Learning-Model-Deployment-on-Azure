
# **Deploying a Machine Learning Model on Azure**

## **Project Overview**
This project showcases the deployment of a machine learning model for predicting diabetes using Azure Machine Learning services. It includes the end-to-end process of training, registering, and deploying a model, followed by creating a RESTful API to make predictions. The deployment is done using Azure's scalable and secure cloud resources.

## **Directory Structure**
```
.
├── 1. Diabetes model to be deployed.ipynb  # Jupyter notebook for training the model
├── 2. deploy ML model on Azure.ipynb       # Notebook to deploy the model on Azure
├── score.py                                # Scoring script for model inference
├── requirements.txt                        # Dependencies for the project
└── README.md                               # Project documentation
```

## **Prerequisites**
Before starting, ensure you have the following:

- An **Azure Subscription** with access to create and manage resources.
- **Python 3.8 or higher** installed on your local environment.
- **Azure Machine Learning SDK** version 1.54.0 or higher.
- Necessary Python packages listed in `requirements.txt`.
- **Jupyter Notebook** environment for running `.ipynb` files.

## **Environment Setup**

1. **Clone the Repository:**
   Download the project files to your local machine:

   ```bash
   git clone https://github.com/<username>/azure-ml-deployment.git
   cd azure-ml-deployment
   ```

2. **Create a Virtual Environment:**
   Set up a virtual environment to manage dependencies:

   ```bash
   python -m venv azure_ml_env
   source azure_ml_env/bin/activate  # On Windows use: azure_ml_env\Scripts\activate
   ```

3. **Install Required Packages:**
   Use `requirements.txt` to install all necessary dependencies:

   ```bash
   pip install -r requirements.txt
   ```

## **Project Files Description**
1. **`1. Diabetes model to be deployed.ipynb`**:
   - This notebook handles data preprocessing, model training, and evaluation.
   - Outputs the trained model to be registered and deployed in Azure.

2. **`2. deploy ML model on Azure.ipynb`**:
   - Creates an Azure Machine Learning workspace, registers the trained model, and deploys it as a web service.
   - The notebook contains detailed instructions and code for setting up Azure resources.

3. **`score.py`**:
   - A scoring script used during deployment to handle HTTP POST requests.
   - Loads the model, processes incoming data, and returns predictions.

4. **`requirements.txt`**:
   - Lists all necessary libraries and their versions for reproducibility.
   - Includes `azureml-sdk`, `numpy`, `pandas`, and `scikit-learn`.

## **Deployment Steps**
Follow these steps to deploy your model on Azure:

1. **Model Training:**
   - Run the `1. Diabetes model to be deployed.ipynb` notebook.
   - This notebook will train and save the model for deployment.

2. **Azure Deployment:**
   - Run the `2. deploy ML model on Azure.ipynb` notebook.
   - Configure your Azure workspace, register the trained model, and deploy it as a web service.

3. **Scoring Script:**
   - Ensure `score.py` is available in your deployment environment.
   - This script will handle incoming requests and serve model predictions.

## **Testing the Deployed Model**

### **API Endpoint:**
After successful deployment, you will receive an endpoint URL in the following format:

```
https://<your-service-name>.azurewebsites.net/score
```

Replace `<your-service-name>` with your actual service name.

### **Request Format:**
Send a POST request with the following format:

- **URL:** `https://<your-service-name>.azurewebsites.net/score`
- **Method:** `POST`
- **Headers:**
  - `Content-Type: application/json`
  - `Authorization: Bearer <access-token>`  # Replace with your Azure access token if applicable

- **Body Example:**

  ```json
  {
    "data": [
      {
        "Pregnancies": 6,
        "Glucose": 148,
        "BloodPressure": 72,
        "SkinThickness": 35,
        "Insulin": 0,
        "BMI": 33.6,
        "DiabetesPedigreeFunction": 0.627,
        "Age": 50
      }
    ]
  }
  ```

### **Response Format:**
The response will be a JSON object containing the model's predictions:

```json
{
  "predictions": [1]
}
```

## **Example Python Script for Testing**
```python
import requests
import json

# Replace these with your actual endpoint URL and access token
endpoint_url = "https://<your-service-name>.azurewebsites.net/score"
access_token = "<your-access-token>"

# Sample input data
input_data = {
    "data": [
        {
            "Pregnancies": 6,
            "Glucose": 148,
            "BloodPressure": 72,
            "SkinThickness": 35,
            "Insulin": 0,
            "BMI": 33.6,
            "DiabetesPedigreeFunction": 0.627,
            "Age": 50
        }
    ]
}

# Set up the headers
headers = {
    "Content-Type": "application/json",
    "Authorization": f"Bearer {access_token}"
}

# Make the request
response = requests.post(endpoint_url, headers=headers, json=input_data)

# Print the response
print(response.json())
```

## **Troubleshooting**
1. **Model Deployment Fails:**
   - Check that your Azure workspace is correctly configured.
   - Ensure that your Python environment has all necessary packages.

2. **API Endpoint Returns 401 Unauthorized:**
   - Make sure your access token is valid.
   - Re-check authentication settings in Azure.

3. **HTTP 500 Internal Server Error:**
   - Inspect the `score.py` file for any errors in the `run()` method.
   - Ensure that the model is correctly loaded in the scoring script.

