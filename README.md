# Habit Tracker System

A full-stack habit tracking application with AI-powered productivity assistance.

## Architecture

**Frontend**: React.js with Material-UI  
**Backend**: Node.js/Express with MongoDB  
**AI Service**: FastAPI with Groq LLM integration  
**Database**: MongoDB with Mongoose ODM

## Features

- **Habit Management**: Create, track, and manage daily habits
- **Analytics**: Visual progress tracking with charts (Recharts)
- **Streak Tracking**: Monitor habit consistency and streaks
- **Reminders**: Automated habit reminders
- **AI Chat**: Productivity coaching with contextual advice
- **User Authentication**: Secure user accounts and data

## Project Structure

```
Habit-tracker-system/
├── Frontend/                 # React frontend (Material-UI)
├── Backend/                  # Node.js API server
│   ├── controllers/         # Route handlers
│   ├── models/             # MongoDB schemas
│   ├── routes/             # API endpoints
│   └── services/           # Business logic
├── ai-service/             # Python FastAPI AI service
└── *.postman_*            # API testing collections
```

## Tech Stack

**Frontend**: React 18, Material-UI, Axios, Recharts  
**Backend**: Express.js, MongoDB, Mongoose, JWT  
**AI**: FastAPI, Groq API, Python-dotenv  
**Tools**: Postman collections for API testing

## Quick Start

1. **Backend**: `cd Backend && npm install && npm start`
2. **AI Service**: `cd ai-service && pip install -r requirements.txt && uvicorn app:app --reload`
3. **Frontend**: `cd Frontend/habit-tracker-frontend && npm install && npm start`

## Environment Variables

- `GROQ_API_KEY`: Required for AI chat functionality
- MongoDB connection string in Backend/.env