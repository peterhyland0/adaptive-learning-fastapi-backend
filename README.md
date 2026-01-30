# AdaptiveLearningBackend

A comprehensive FastAPI-based backend service for an adaptive learning application that leverages AI to create personalized educational content based on individual learning styles.

## 🎯 Overview

AdaptiveLearningBackend powers a React Native adaptive learning application by providing intelligent content generation, learning style prediction, and multi-modal educational resource creation. The system uses machine learning to identify user learning preferences and generates customized study materials including flashcards, mind maps, podcasts, and quizzes.

## ✨ Features

### 🧠 Learning Style Prediction
- **AI-Powered Classification**: Uses a trained TensorFlow/Keras model to predict learning styles based on user responses
- **Support for Multiple Learning Styles**:
  - Visual learners
  - Auditory learners
  - Kinesthetic learners
- **Confidence Scoring**: Provides confidence levels for each prediction to create balanced learning experiences

### 📚 Multi-Modal Content Generation
- **PDF/Image/Audio Processing**: Extract text content from various file formats
- **Dynamic Module Creation**: Automatically generates structured learning modules from uploaded content
- **Personalized Sub-modules**: Creates tailored content based on user's learning style preferences:
  - **Flash Cards**: Interactive repetitive learning for kinesthetic learners
  - **Mind Maps**: Visual representation of concepts for visual learners
  - **Podcast Sessions**: Audio content with transcripts for auditory learners
  - **Multiple Choice Quizzes**: Assessment modules for knowledge verification

### 🔊 Audio Processing
- **Text-to-Speech (TTS)**: Generates natural-sounding podcast content using OpenAI's TTS API
- **Speech-to-Text (STT)**: Transcribes audio files and generates searchable transcripts
- **Multi-Voice Support**: Creates engaging podcast-style content with different voices

### 🔥 Firebase Integration
- **User Management**: Complete user authentication and profile management
- **Firestore Database**: Stores modules, submodules, and user progress
- **Cloud Storage**: Manages file uploads and generated content
- **Cloud Vision API**: Extracts text from images using Google Cloud Vision

### 🎓 Admin Features
- **Student Management**: Admin users can create and manage student accounts
- **Module Assignment**: Share modules with multiple students
- **Progress Tracking**: Monitor student completion and engagement

### 🤖 OpenAI Integration
- **GPT-4 Integration**: Generates high-quality educational content
- **Realtime API**: Provides interactive tutoring sessions
- **Structured Output**: Creates JSON-formatted content for flashcards, mind maps, and quizzes

## 🛠 Technology Stack

### Core Framework
- **FastAPI**: Modern, fast web framework for building APIs
- **Python 3.x**: Primary programming language
- **Pydantic**: Data validation using Python type annotations

### Machine Learning
- **TensorFlow/Keras**: Deep learning framework for learning style classification
- **NumPy**: Numerical computing library
- **Pre-trained Model**: Custom-trained H5 model for learning style prediction

### AI/ML Services
- **OpenAI API**: 
  - GPT-4o for content generation
  - Whisper for speech-to-text
  - TTS for text-to-speech
  - Realtime API for interactive tutoring

### Firebase Services
- **Firebase Admin SDK**: Backend integration with Firebase services
- **Firebase Authentication**: User authentication and management
- **Cloud Firestore**: NoSQL document database
- **Cloud Storage**: File storage and retrieval
- **Cloud Vision API**: Image text extraction

### Additional Libraries
- **httpx**: Async HTTP client for external API calls
- **PyPDF**: PDF text extraction

## 📁 Project Structure

```
AdaptiveLearningBackend/
├── app/
│   ├── api_routes/
│   │   ├── __init__.py
│   │   └── api_routes.py          # Main API endpoint definitions
│   ├── firebaseHandling/
│   │   ├── __init__.py
│   │   └── firebaseHandling.py    # Firebase operations and utilities
│   └── model_utils/
│       ├── __init__.py
│       ├── predict_learning_style.py  # ML prediction logic
│       ├── LearningStyleClassifier.h5 # Trained Keras model
│       ├── tokenizer.pickle           # Text tokenizer for model
│       └── labelEncoder.pickle        # Label encoder for predictions
└── README.md
```

### Module Descriptions

#### `api_routes/`
Contains all FastAPI route definitions and API endpoint handlers including:
- Learning style prediction endpoints
- File upload and processing
- User management endpoints
- Module and submodule creation
- OpenAI Realtime API session management

#### `firebaseHandling/`
Manages all Firebase-related operations:
- User creation and deletion
- Module and submodule CRUD operations
- Student-teacher relationship management
- Cloud Storage file operations
- Image text extraction via Cloud Vision

#### `model_utils/`
Contains machine learning components:
- Learning style prediction model
- Text preprocessing and tokenization
- Model inference logic

## 🚀 API Endpoints

### User Management

#### Create User
```http
POST /signup-users
Content-Type: application/json

{
  "email": "user@example.com",
  "password": "securepassword",
  "admin": false,
  "adminUid": "optional-admin-uid"
}
```

#### Delete User
```http
DELETE /user/{user_uid}
```

#### Get Admin Students
```http
GET /admin/{admin_uid}/students
```

#### Add Users to Module
```http
POST /addUsersToModule
Content-Type: application/json

{
  "moduleId": "module-id",
  "userIds": ["user-id-1", "user-id-2"],
  "adminUid": "admin-user-id"
}
```

### Learning Style Prediction

#### Predict Learning Style
```http
POST /predict-learning-style
Content-Type: application/json

{
  "answers": [
    {"answer": "I prefer visual diagrams"},
    {"answer": "I learn by doing"},
    ...
  ]
}
```
*Note: Requires exactly 16 answers*

**Response:**
```json
{
  "Visual": 45.2,
  "Kinesthetic": 32.8,
  "Auditory": 22.0
}
```

### Content Generation

#### Upload File and Generate Module
```http
POST /upload-file
Content-Type: multipart/form-data

useruid: string
submodulepreference: ["Visual", "Kinesthetic", "Auditory"]
file: File (PDF, Image, or Audio)
```

**Supported File Types:**
- PDF documents
- Images (JPEG, PNG)
- Audio files (WAV, MP3, MP4, MPEG)

**Response:**
```json
{
  "ok": true
}
```

### OpenAI Realtime Session

#### Create Realtime Session
```http
POST /session
Content-Type: application/json

{
  "content": "Your study material content here"
}
```

Returns an ephemeral OpenAI Realtime API session token for interactive tutoring.

## 📋 Prerequisites

- Python 3.8 or higher
- Firebase project with the following enabled:
  - Firebase Authentication
  - Cloud Firestore
  - Cloud Storage
  - Cloud Vision API
- OpenAI API account and API key
- Required Python packages (see Installation)

## 🔧 Installation

1. **Clone the repository**
```bash
git clone https://github.com/peterhyland0/AdaptiveLearningBackend.git
cd AdaptiveLearningBackend
```

2. **Install dependencies**
```bash
pip install fastapi uvicorn
pip install firebase-admin
pip install tensorflow keras numpy
pip install openai httpx
pip install pydantic
pip install python-multipart
# Add other dependencies as needed
```

3. **Set up Firebase credentials**
   - Download your Firebase Admin SDK service account key
   - Place it at `app/firebaseHandling/adaptive-learning-app-example.json`
   - Update the storage bucket name in `firebaseHandling.py`

4. **Configure OpenAI API**
   - Set your OpenAI API key in the configuration
   - Update organization ID in `api_routes.py` if needed

5. **Prepare ML Models**
   - Ensure the following files exist in `app/model_utils/`:
     - `LearningStyleClassifier.h5`
     - `tokenizer.pickle`
     - `labelEncoder.pickle`

## 🏃 Running the Application

### Development Mode
```bash
uvicorn app.main:app --reload --host 0.0.0.0 --port 8000
```

### Production Mode
```bash
uvicorn app.main:app --host 0.0.0.0 --port 8000 --workers 4
```

The API will be available at `http://localhost:8000`

API documentation will be available at:
- Swagger UI: `http://localhost:8000/docs`
- ReDoc: `http://localhost:8000/redoc`

## 🔒 Environment Variables

Create a `.env` file or set the following environment variables:

```bash
OPENAI_API_KEY=your_openai_api_key
FIREBASE_CREDENTIALS_PATH=path/to/firebase-credentials.json
FIREBASE_STORAGE_BUCKET=your-storage-bucket-name
```

## 📊 Learning Style Classification

The system uses a trained neural network to classify user responses into three learning styles:

1. **Visual**: Prefers diagrams, charts, and visual representations
2. **Auditory**: Learns best through listening and discussion
3. **Kinesthetic**: Prefers hands-on activities and practice

The model:
- Accepts 16 text-based answers from a questionnaire
- Processes text using a trained tokenizer
- Uses LSTM/GRU architecture for sequence classification
- Returns confidence scores for each learning style
- Calculates percentage distribution across all styles

## 🎨 Content Generation Workflow

1. **File Upload**: User uploads PDF, image, or audio file
2. **Text Extraction**: System extracts text content from the file
3. **Module Generation**: OpenAI GPT-4 creates structured module content
4. **Submodule Creation**: Based on learning style preferences:
   - **Kinesthetic**: Generates interactive flashcards
   - **Visual**: Creates hierarchical mind maps
   - **Auditory**: Produces podcast-style audio with transcript
5. **Quiz Generation**: Creates assessment questions for all users
6. **Storage**: All content saved to Firebase with progress tracking

## 💰 Cost Tracking

The system tracks OpenAI API usage and costs:
- Input/Output token counts for each content type
- TTS character count and costs
- STT duration and costs
- Detailed cost breakdown logged for each module creation

## 🧪 Testing

To test the API endpoints:

```bash
# Test learning style prediction
curl -X POST http://localhost:8000/predict-learning-style \
  -H "Content-Type: application/json" \
  -d '{"answers": [{"answer": "test"}, ...]}'

# Test file upload
curl -X POST http://localhost:8000/upload-file \
  -F "useruid=test-user-id" \
  -F "submodulepreference=Visual,Kinesthetic" \
  -F "file=@/path/to/file.pdf"
```

## 🤝 Contributing

Contributions are welcome! Please follow these guidelines:

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit your changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

## 📝 Code Style

- Follow PEP 8 guidelines for Python code
- Use type hints for function parameters and return values
- Write docstrings for functions and classes
- Keep functions focused and single-purpose

## 🔮 Future Enhancements

- [ ] Add more learning style categories (Reading/Writing learners)
- [ ] Implement caching for frequently accessed content
- [ ] Add rate limiting for API endpoints
- [ ] Create WebSocket support for real-time progress updates
- [ ] Implement advanced analytics and reporting
- [ ] Add support for more file formats
- [ ] Multi-language support for content generation
- [ ] Collaborative learning features

## 📄 License

This project is part of a React Native adaptive learning application. Please contact the repository owner for licensing information.

## 👥 Authors

- **Peter Hyland** - *Initial work* - [peterhyland0](https://github.com/peterhyland0)

## 🙏 Acknowledgments

- OpenAI for GPT-4 and Whisper APIs
- Firebase for backend infrastructure
- TensorFlow team for the ML framework
- FastAPI community for the excellent web framework

## 📞 Support

For issues, questions, or contributions, please:
- Open an issue on GitHub
- Contact the repository maintainer
- Check existing documentation and API specs

---

**Note**: This is a backend service designed to work with a React Native mobile application. Ensure the mobile app is configured with the correct API endpoint URLs.
