# AI Module for Astra Application

## Features
- **Face Registration**: Extract facial embeddings and store them in Qdrant.
- **Face Verification**: Check if a face already exists in the database.
- **OCR Extraction**: Extract text from documents using OCR.
- **Image Processing**: Handle base64 image decoding, noise and blur detection.

## Technologies Used
- **FastAPI**: Web framework for API development.
- **OpenCV**: Image processing and face detection.
- **MediaPipe**: Facial landmark detection.
- **TensorFlow/ResNet50**: Face embedding generation.
- **Qdrant**: Vector database for storing and searching embeddings.
- **Google Cloud Vision API**: OCR extraction.

## Installation
### Prerequisites
- Python 3.12.11
- Qdrant account and API key
- Google Cloud Vision API credentials

### Setup
1. **Clone the repository**
   ```bash
   git clone <repo_url>
   cd <project_directory>
   ```

2. **Install dependencies**
   
If the system has a GPU (NVIDIA). then run the following command:
   ```bash
   pip install -r requirements_GPU.txt
   ```

Otherwise, run the following command:
   ```bash
   pip install -r requirements_CPU.txt
   ```


3. **Set up environment variables**
   ```bash
   export QDRANT_HOST="your_qdrant_host"
   export QDRANT_PORT="your_qdrant_port"
   export QDRANT_API_KEY="your_qdrant_api_key"
   export GOOGLE_APPLICATION_CREDENTIALS="path_to_your_google_credentials.json"
   export ID_ANALYZER_API_KEY="IDAnalyzer API Key"
   ```

## Running the Application
```bash
uvicorn main:app --host 0.0.0.0 --port 8000 --reload
```

## API Endpoints
### Face Registration
- **Endpoint**: `POST /register-face/`
- **Description**: Registers a new face and stores the embedding.
- **Request Body**:
  ```json
  {
    "UserId": 9,
    "Base64ScannedImage": "base64 encoded image"
  }
  ```
- **Response**:
  ```json
  {
    "Status": "Success | Failed | Unknown",
    "PointId": "guid - Special uuid4",
    "UserId": 9,
    "Message": "Message"
  }
  ```

### Document OCR Extraction
- **Endpoint**: `POST /extract-info/`
- **Description**: Extracts text and document details from an image.
- **Request Body**:
  ```json
  {
    "UserId": "string UUID",
    "PointId": "string - point id (A guid generated by uuid4)", 
    "DocumentType": "Passport | CNIC | Unknown",
    "Base64ScannedImage": "Base64 encoeded image"
  }
  ```
- **Response**:
  ```json
  {
    "Status": "Failed | Success | Unknown",
    "PointId": "string - point id (A guid generated by uuid4)",
    "UserId": "string UUID",
    "Message": "Message",
    "Data": {
        "DocumentType": "Passport | CNIC | Unknown",
        "Name": "Person Name",
        "Nationality": "Person Natiionality",
        "DOB": "Person's dob in %Y-%m-%d",
        "Gender": "Male | Female | Unknown",
        "ExpiryDate": "document expiry in %Y-%m-%d",
        "IsExpired": true
    }
  }
  ```

## Exception Handling:

If there is any issues with the server, the server will respind with 500 and return the following:
```json
{
  "Error": "ErrorClassName",
  "StatusCode": 500,
  "Message": "Error Message",
  "Details": "Error Stack"
}
```

## Notes
- Ensure that Qdrant is properly configured before running the application.
- The Google Cloud Vision API requires authentication via a service account key.
- The libraries are also provided incase requirement.txt is not updated.

## License
MIT License

