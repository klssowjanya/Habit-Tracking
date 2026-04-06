# 🏗️ NLP Features Architecture

## System Overview

```
┌─────────────────────────────────────────────────────────────────┐
│                         USER INTERFACE                          │
│                      (React Frontend)                           │
│                     http://localhost:3000                       │
└─────────────────────────────────────────────────────────────────┘
                              │
                              │ HTTP Requests
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│                      BACKEND API SERVER                         │
│                    (Node.js + Express)                          │
│                     http://localhost:5000                       │
│                                                                 │
│  Routes:                                                        │
│  • /api/nlp/sentiment      → Proxy to AI Service               │
│  • /api/nlp/keywords       → Proxy to AI Service               │
│  • /api/nlp/reminder       → Proxy to AI Service               │
│  • /api/nlp/intent         → Proxy to AI Service               │
│  • /api/nlp/parse-habit    → Proxy to AI Service               │
└─────────────────────────────────────────────────────────────────┘
                              │
                              │ HTTP Requests
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│                       AI SERVICE                                │
│                    (Python + FastAPI)                           │
│                     http://localhost:8000                       │
│                                                                 │
│  Endpoints:                                                     │
│  • POST /sentiment         → DistilBERT Model                  │
│  • POST /keywords          → Frequency Analysis                │
│  • POST /reminder          → Template Engine                   │
│  • POST /intent            → BART Model                        │
│  • POST /parse-habit       → Regex Parser                      │
└─────────────────────────────────────────────────────────────────┘
                              │
                              │ Model Inference
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│                    ML MODELS (HuggingFace)                      │
│                                                                 │
│  • distilbert-base-uncased-finetuned-sst-2-english (255MB)    │
│  • facebook/bart-large-mnli (1.6GB)                            │
└─────────────────────────────────────────────────────────────────┘
```

## Data Flow Examples

### 1. Sentiment Analysis Flow

```
User Input: "I'm feeling great about my progress!"
    │
    ▼
[SentimentAnalyzer.jsx]
    │
    ▼
API.post('/api/nlp/sentiment', { text: "..." })
    │
    ▼
[Backend: nlpRoutes.js]
    │
    ▼
axios.post('http://localhost:8000/sentiment', { text: "..." })
    │
    ▼
[AI Service: app.py]
    │
    ▼
sentiment_analyzer(text) → DistilBERT Model
    │
    ▼
Response: { sentiment: "POSITIVE", score: 0.9998 }
    │
    ▼
Display: ✅ POSITIVE (99.98%)
```

### 2. Natural Language Habit Creation Flow

```
User Input: "I want to exercise 3 times a week"
    │
    ▼
[NaturalLanguageHabitInput.jsx]
    │
    ▼
API.post('/api/nlp/parse-habit', { text: "..." })
    │
    ▼
[Backend: nlpRoutes.js]
    │
    ▼
axios.post('http://localhost:8000/parse-habit', { text: "..." })
    │
    ▼
[AI Service: app.py]
    │
    ▼
Regex Pattern Matching:
  - Extract frequency: "week" → "weekly"
  - Extract habit: "exercise"
    │
    ▼
Response: { title: "Exercise", frequency: "weekly" }
    │
    ▼
Display parsed habit → User confirms → Create habit
```

### 3. Smart Reminder Flow

```
Habit Data: { name: "Meditation", streak: 7, completionRate: 0.85 }
    │
    ▼
[SmartReminder.jsx]
    │
    ▼
API.post('/api/nlp/reminder', { habitName, streak, completionRate })
    │
    ▼
[Backend: nlpRoutes.js]
    │
    ▼
axios.post('http://localhost:8000/reminder', { ... })
    │
    ▼
[AI Service: app.py]
    │
    ▼
Template Selection Logic:
  - completionRate >= 0.8 → Celebration template
  - completionRate >= 0.5 → Encouragement template
  - completionRate < 0.5  → Fresh start template
    │
    ▼
Response: { message: "🔥 Amazing! Keep your 7-day streak alive!" }
    │
    ▼
Display: Gradient card with motivational message
```

## Component Integration Map

```
Dashboard.jsx
    │
    ├─→ AddHabitForm.jsx
    │       └─→ KeywordTagger.jsx (Auto-tags)
    │
    ├─→ HabitCard.jsx
    │       └─→ SmartReminder.jsx (Personalized messages)
    │
    └─→ ChatbotPanel.jsx
            ├─→ IntentClassifier.jsx (Route requests)
            └─→ SentimentAnalyzer.jsx (Detect mood)

NLPDemoPage.jsx (Showcase all features)
    ├─→ NaturalLanguageHabitInput.jsx
    ├─→ SentimentAnalyzer.jsx
    ├─→ IntentClassifier.jsx
    ├─→ KeywordTagger.jsx
    └─→ SmartReminder.jsx
```

## Technology Stack

```
┌─────────────────────────────────────────────────────────────────┐
│                         FRONTEND LAYER                          │
├─────────────────────────────────────────────────────────────────┤
│ React 18                                                        │
│ Material-UI (MUI)                                               │
│ Axios (HTTP Client)                                             │
└─────────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────────┐
│                         BACKEND LAYER                           │
├─────────────────────────────────────────────────────────────────┤
│ Node.js + Express                                               │
│ Axios (Proxy to AI Service)                                     │
│ MongoDB + Mongoose (Data persistence)                           │
└─────────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────────┐
│                         AI SERVICE LAYER                        │
├─────────────────────────────────────────────────────────────────┤
│ Python 3.8+                                                     │
│ FastAPI (Web framework)                                         │
│ Transformers (HuggingFace)                                      │
│ PyTorch (ML backend)                                            │
└─────────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────────┐
│                         MODEL LAYER                             │
├─────────────────────────────────────────────────────────────────┤
│ DistilBERT (Sentiment Analysis)                                │
│ BART (Intent Classification)                                    │
│ Regex + Templates (Parsing & NLG)                              │
└─────────────────────────────────────────────────────────────────┘
```

## Feature Complexity Matrix

```
Feature                    | Complexity | Model Size | Response Time
---------------------------|------------|------------|---------------
Sentiment Analysis         | Medium     | 255MB      | ~100ms
Keyword Extraction         | Low        | 0MB        | ~10ms
Smart Reminders (NLG)      | Low        | 0MB        | ~5ms
Intent Classification      | High       | 1.6GB      | ~200ms
NL Habit Creation          | Low        | 0MB        | ~5ms
```

## Deployment Architecture (Production)

```
┌─────────────────────────────────────────────────────────────────┐
│                         LOAD BALANCER                           │
│                         (AWS ALB / Nginx)                       │
└─────────────────────────────────────────────────────────────────┘
                              │
                ┌─────────────┴─────────────┐
                │                           │
                ▼                           ▼
┌──────────────────────────┐  ┌──────────────────────────┐
│   Frontend (S3 + CF)     │  │   Backend (EC2/ECS)      │
│   Static React Build     │  │   Node.js API Server     │
└──────────────────────────┘  └──────────────────────────┘
                                            │
                                            ▼
                              ┌──────────────────────────┐
                              │   AI Service (EC2/ECS)   │
                              │   Python FastAPI         │
                              │   GPU Instance (optional)│
                              └──────────────────────────┘
                                            │
                                            ▼
                              ┌──────────────────────────┐
                              │   Model Cache (EFS)      │
                              │   Persistent Storage     │
                              └──────────────────────────┘
```

## Security Considerations

```
┌─────────────────────────────────────────────────────────────────┐
│                         SECURITY LAYERS                         │
├─────────────────────────────────────────────────────────────────┤
│ 1. CORS Protection (AI Service)                                │
│    - Only allow requests from frontend origin                  │
│                                                                 │
│ 2. Rate Limiting (Backend)                                     │
│    - Prevent abuse of NLP endpoints                            │
│                                                                 │
│ 3. Input Validation (All layers)                               │
│    - Sanitize user input before processing                     │
│                                                                 │
│ 4. Authentication (Backend)                                    │
│    - JWT tokens for API access                                 │
│                                                                 │
│ 5. Model Security (AI Service)                                 │
│    - Trusted models from HuggingFace                           │
└─────────────────────────────────────────────────────────────────┘
```

## Scalability Strategy

```
Current (Development):
  Frontend: 1 instance
  Backend: 1 instance
  AI Service: 1 instance
  
Production (Scaled):
  Frontend: CDN (CloudFront)
  Backend: Auto-scaling group (2-10 instances)
  AI Service: Auto-scaling group (2-5 instances)
  Load Balancer: Distribute traffic
  Model Cache: Shared EFS volume
```

## Monitoring & Observability

```
┌─────────────────────────────────────────────────────────────────┐
│                         MONITORING STACK                        │
├─────────────────────────────────────────────────────────────────┤
│ Metrics to Track:                                              │
│ • Response times per endpoint                                  │
│ • Model inference latency                                      │
│ • Error rates                                                  │
│ • Request volume                                               │
│ • Model accuracy (sentiment/intent)                            │
│                                                                 │
│ Tools:                                                          │
│ • CloudWatch (AWS)                                             │
│ • Prometheus + Grafana                                         │
│ • Sentry (Error tracking)                                      │
└─────────────────────────────────────────────────────────────────┘
```

## Cost Optimization

```
Development:
  • Local development: $0
  • Model downloads: Free (HuggingFace)
  
Production (Estimated Monthly):
  • EC2 instances (t3.medium x3): ~$75
  • Load Balancer: ~$20
  • S3 + CloudFront: ~$5
  • Total: ~$100/month
  
With GPU (for faster inference):
  • EC2 GPU instance (g4dn.xlarge): ~$300/month
```

This architecture provides a solid foundation for scaling the NLP features as the application grows!
