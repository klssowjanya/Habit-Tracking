# 🚀 Quick Setup Guide - NLP Features

## Prerequisites
- Python 3.8+
- Node.js 14+
- ~2GB free disk space (for ML models)

## Setup Steps

### 1. Install AI Service Dependencies
```bash
cd ai-service
pip install -r requirements.txt
```

**Note**: First installation will take 5-10 minutes to download ML models.

### 2. Start AI Service
```bash
cd ai-service
uvicorn app:app --reload
```

You should see:
```
INFO:     Uvicorn running on http://127.0.0.1:8000
Groq client initialized successfully
```

**First Request**: Models will load on first API call (~10 seconds). Subsequent requests are instant.

### 3. Backend (Already Configured)
```bash
cd Backend
npm install  # if not already done
npm start
```

### 4. Frontend (Already Configured)
```bash
cd frontend
npm install  # if not already done
npm start
```

### 5. Test NLP Features
```bash
# In project root
node test-nlp.js
```

Expected output:
```
🧪 Testing NLP Features...

1️⃣ Testing Sentiment Analysis...
✅ Sentiment: { sentiment: 'POSITIVE', score: 0.9998 }

2️⃣ Testing Keyword Extraction...
✅ Keywords: { keywords: ['running', 'morning', 'improve', 'fitness', 'health'] }

3️⃣ Testing Smart Reminder Generation...
✅ Reminder: { message: '🔥 Amazing! Keep your 7-day streak alive with Morning Meditation!' }

4️⃣ Testing Intent Classification...
✅ Intent: { intent: 'create_habit', confidence: 0.9234 }

5️⃣ Testing Natural Language Habit Parsing...
✅ Parsed Habit: { title: 'Drink water', frequency: 'daily' }

🎉 All NLP features working correctly!
```

## Access Demo Page

Once all services are running:
1. Login to the app: `http://localhost:3000`
2. Navigate to: `http://localhost:3000/nlp-demo`
3. Try all 5 features interactively!

## Quick Feature Test

### Test Sentiment Analysis
```bash
curl -X POST http://localhost:8000/sentiment \
  -H "Content-Type: application/json" \
  -d '{"text": "I love this app!"}'
```

### Test Keyword Extraction
```bash
curl -X POST http://localhost:8000/keywords \
  -H "Content-Type: application/json" \
  -d '{"text": "I want to exercise daily for better health"}'
```

### Test Smart Reminder
```bash
curl -X POST http://localhost:8000/reminder \
  -H "Content-Type: application/json" \
  -d '{"habitName": "Reading", "streak": 5, "completionRate": 0.8}'
```

### Test Intent Classification
```bash
curl -X POST http://localhost:8000/intent \
  -H "Content-Type: application/json" \
  -d '{"text": "Show me my progress"}'
```

### Test Habit Parsing
```bash
curl -X POST http://localhost:8000/parse-habit \
  -H "Content-Type: application/json" \
  -d '{"text": "I want to meditate every morning"}'
```

## Troubleshooting

### Issue: "Module not found: transformers"
**Solution**:
```bash
cd ai-service
pip install transformers==4.36.0 torch==2.1.0
```

### Issue: "Connection refused on port 8000"
**Solution**: Make sure AI service is running:
```bash
cd ai-service
uvicorn app:app --reload
```

### Issue: "Models taking too long to load"
**Solution**: This is normal on first request. Models are cached after first load.

### Issue: "CORS error in browser"
**Solution**: Verify AI service CORS settings in `app.py`:
```python
allow_origins=["http://localhost:3000", "http://127.0.0.1:3000"]
```

## Integration Examples

### Add Sentiment to Chat
```jsx
import { getSentiment } from '../services/api';

const handleMessage = async (text) => {
  const { data } = await getSentiment(text);
  if (data.sentiment === 'NEGATIVE') {
    // Show supportive message
  }
};
```

### Add Keywords to Habit Form
```jsx
import KeywordTagger from '../components/KeywordTagger';

<KeywordTagger 
  text={habitDescription}
  onTagsExtracted={(tags) => setTags(tags)}
/>
```

### Add Smart Reminders to Dashboard
```jsx
import SmartReminder from '../components/SmartReminder';

{habits.map(habit => (
  <SmartReminder 
    habit={habit.title}
    streak={habit.streak}
    completionRate={habit.completionRate}
  />
))}
```

## Performance Tips

1. **Model Caching**: Models load once and stay in memory
2. **Batch Requests**: Process multiple texts together when possible
3. **Async Processing**: All endpoints are non-blocking
4. **Error Handling**: Graceful fallbacks if AI service is down

## Next Steps

1. ✅ Test all 5 features using demo page
2. ✅ Integrate into existing components
3. ✅ Show mentor the working features
4. 🚀 Deploy to production (optional)

## Demo Script for Mentor

1. **Start**: "I've implemented 5 NLP features using HuggingFace transformers"
2. **Show Natural Language Habit Creation**: Type "I want to exercise 3 times a week"
3. **Show Intent Classification**: Type different queries, show intelligent routing
4. **Show Smart Reminders**: Display personalized messages based on performance
5. **Show Sentiment Analysis**: Analyze positive/negative text
6. **Show Keyword Extraction**: Auto-tag habits from descriptions

**Key Points**:
- Production-ready ML models
- Fast response times (<200ms)
- Clean architecture
- Easy to extend

Good luck! 🎉
