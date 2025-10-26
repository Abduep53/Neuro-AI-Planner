# Neuro AI Study Planner - Architecture Documentation

## System Overview

Neuro AI Study Planner is built as a modern, cloud-native web application using Firebase for backend services and OpenAI GPT-4 for AI-powered plan generation.

## High-Level Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                        CLIENT LAYER                          │
│  ┌──────────┐  ┌──────────┐  ┌──────────┐  ┌──────────┐   │
│  │ HTML UI  │  │   CSS    │  │    JS    │  │   i18n   │   │
│  └──────────┘  └──────────┘  └──────────┘  └──────────┘   │
└────────────────────────┬────────────────────────────────────┘
                         │
┌────────────────────────┼────────────────────────────────────┐
│                    FIREBASE LAYER                           │
│  ┌──────────┐  ┌──────────┐  ┌──────────┐  ┌──────────┐   │
│  │  Auth    │  │ Firestore│  │ Storage  │  │ Hosting  │   │
│  └──────────┘  └──────────┘  └──────────┘  └──────────┘   │
└────────────────────────┼────────────────────────────────────┘
                         │
┌────────────────────────┼────────────────────────────────────┐
│                       AI LAYER                               │
│                   ┌──────────────┐                          │
│                   │  OpenAI GPT-4│                          │
│                   └──────────────┘                          │
└─────────────────────────────────────────────────────────────┘
```

## Technical Stack

### Frontend
- **HTML5**: Semantic markup
- **CSS3**: Modern styling with animations
- **JavaScript (ES6+)**: Vanilla JS, no frameworks
- **AOS**: Animate On Scroll library
- **Firebase SDK**: Client-side Firebase integration

### Backend & Services
- **Firebase Authentication**: User management
- **Cloud Firestore**: NoSQL database
- **Firebase Storage**: File storage
- **Firebase Hosting**: CDN deployment
- **OpenAI API**: GPT-4 integration

## System Components

### 1. Authentication System

**Technology**: Firebase Authentication

**Features**:
- Email/password authentication
- Social login providers (Google, GitHub)
- Password reset functionality
- Session management
- Secure token handling

**Implementation**:
```javascript
// scripts/auth.js
- User registration
- Login/logout
- Auth state persistence
- Protected routes
```

### 2. Database Architecture

**Technology**: Cloud Firestore

**Collections Structure**:
```
users/
  └── {userId}/
      ├── profile/
      ├── plans/
      ├── progress/
      └── preferences/

plans/
  └── {planId}/
      ├── metadata/
      ├── days/
      └── resources/

analytics/
  └── {userId}/
      ├── sessions/
      └── events/
```

**Security Rules**:
```javascript
// firestore.rules
- User can only access their own data
- Plans are private by default
- Read/write permissions enforced
```

### 3. AI Integration

**Technology**: OpenAI GPT-4

**Flow**:
```
User Input → Validation → API Call → Response Processing → Plan Generation
```

**System Prompt Structure**:
- Context about the application
- Requirements for plan format
- Resource URL requirements
- Educational quality standards
- JSON structure specification

**Implementation**:
```javascript
// scripts/planGenerator.js
- OpenAI API integration
- Prompt engineering
- Response parsing
- Error handling
- Rate limiting
```

### 4. Plan Generation System

**Components**:
- **plan.js**: Wizard UI and flow control
- **planGenerator.js**: AI API integration
- **planRenderer.js**: Display and interaction

**Process Flow**:
```
Step 1: Subject Selection
  ↓
Step 2: Goal Configuration
  ↓
Step 3: AI Generation
  ↓
Step 4: Plan Display
```

### 5. UI Component System

**Modular Components**:
- Header (navigation, auth)
- Footer (links, info)
- Auth Modals (signup, login)
- Plan Wizard (3-step flow)
- Plan Renderer (interactive display)
- Dashboard (progress tracking)

**Styling System**:
- Main CSS (global styles)
- Component CSS (isolated styles)
- Responsive breakpoints
- Animation keyframes

## Data Flow

### Plan Generation Flow

```
┌──────────┐     ┌──────────┐     ┌──────────┐     ┌──────────┐
│   User   │────▶│  Wizard  │────▶│    AI    │────▶│  Display │
│  Input   │     │   Form   │     │   GPT-4  │     │   Plan   │
└──────────┘     └──────────┘     └──────────┘     └──────────┘
     │                │                 │                │
     │                │                 │                │
     ▼                ▼                 ▼                ▼
┌──────────┐     ┌──────────┐     ┌──────────┐     ┌──────────┐
│ Validate │     │ Process │     │ Generate│     │ Track    │
│  Input   │     │ Config  │     │   Plan  │     │ Progress │
└──────────┘     └──────────┘     └──────────┘     └──────────┘
```

### User Journey

1. **Landing**: User arrives at index.html
2. **Auth**: Login or continue as guest
3. **Wizard**: Select subject and configure goals
4. **Generation**: AI creates personalized plan
5. **Display**: Interactive plan with resources
6. **Track**: Monitor progress and complete tasks

## File Structure

```
neuro-ai-planner/
├── index.html                    # Landing page
├── plan.html                     # Plan wizard
├── dashboard.html                 # User dashboard
├── profile.html                   # User profile
├── pricing.html                   # Subscription
│
├── scripts/
│   ├── firebase-config.js       # Firebase setup
│   ├── auth.js                  # Authentication
│   ├── auth-modals.js           # Auth UI
│   ├── header.js                # Header logic
│   ├── footer.js                # Footer logic
│   ├── main.js                  # Landing page
│   ├── plan.js                  # Plan wizard
│   ├── planGenerator.js         # AI integration
│   ├── planRenderer.js          # Plan display
│   ├── planManager.js           # Plan management
│   ├── dashboard.js             # Dashboard logic
│   ├── data.js                  # Data utilities
│   └── ...                      # Additional modules
│
├── styles/
│   ├── main.css                 # Global styles
│   ├── plan.css                 # Plan styles
│   ├── auth.css                 # Auth styles
│   ├── dashboard.css            # Dashboard styles
│   └── ...                      # Component styles
│
├── components/
│   ├── header.html              # Site header
│   ├── footer.html              # Site footer
│   └── auth-modals.html         # Auth modals
│
├── i18n/
│   ├── en.json                  # English
│   ├── ru.json                  # Russian
│   ├── uz.json                  # Uzbek
│   └── i18n.js                  # i18n system
│
├── chatbot/
│   ├── chatbot.js               # Chatbot logic
│   ├── chatbot.css              # Chatbot styles
│   └── config.js                # Chatbot config
│
└── docs/
    ├── architecture.md          # This file
    ├── ai-tools.md              # AI documentation
    └── ...                      # Additional docs
```

## Security Architecture

### Authentication Security
- **Firebase Auth**: Industry-standard OAuth
- **Token Management**: Secure token storage
- **Session Handling**: Automatic refresh
- **Password Security**: Firebase password policies

### Data Security
- **Firestore Rules**: Role-based access control
- **Encryption**: In-transit and at-rest
- **Privacy**: User data isolation
- **GDPR**: Compliance with regulations

### API Security
- **Rate Limiting**: Prevent abuse
- **Input Validation**: Sanitize all inputs
- **Output Encoding**: XSS prevention
- **HTTPS Only**: Encrypted connections

## Performance Optimization

### Frontend Optimization
- **Minification**: Compressed JS/CSS
- **Lazy Loading**: On-demand resource loading
- **Caching**: Browser caching strategies
- **CDN**: Global content delivery

### Backend Optimization
- **Firebase SDK**: Optimized client SDK
- **Connection Pooling**: Efficient API usage
- **Caching**: Plan caching for repeat requests
- **Indexing**: Optimized Firestore queries

### AI Optimization
- **Prompt Engineering**: Efficient prompts
- **Response Caching**: Cache similar plans
- **Streaming**: Real-time response streaming
- **Fallbacks**: Graceful degradation

## Scalability

### Horizontal Scaling
- **Firebase**: Auto-scaling infrastructure
- **Global CDN**: Multi-region deployment
- **Load Balancing**: Distributed traffic
- **Caching Layers**: Multiple cache tiers

### Vertical Scaling
- **Resource Optimization**: Efficient code
- **Database Optimization**: Indexed queries
- **AI Optimization**: Cost-effective API usage
- **Monitoring**: Performance tracking

## Monitoring & Analytics

### Application Monitoring
- **Firebase Analytics**: User behavior
- **Error Tracking**: Real-time error monitoring
- **Performance Monitoring**: Latency tracking
- **Custom Events**: Business metrics

### Infrastructure Monitoring
- **Uptime**: Service availability
- **Latency**: Response time tracking
- **Errors**: Error rate monitoring
- **Resources**: Usage tracking

## Development Workflow

### Local Development
```bash
# Setup
git clone [repo]
cd neuro-ai-planner

# Configure
# Add Firebase credentials
# Add OpenAI API key

# Run
# Open index.html in browser
# Use live server
```

### Deployment
```bash
# Build
npm run build

# Deploy
firebase deploy

# Verify
# Check production site
```

### Testing
- **Manual Testing**: User journey testing
- **Cross-browser**: Multiple browsers
- **Responsive**: Multiple screen sizes
- **Performance**: Lighthouse audits

## Future Enhancements

### Short-term
- Mobile applications (iOS/Android)
- Enhanced AI model fine-tuning
- Advanced analytics dashboard
- Social sharing features

### Long-term
- Collaborative planning
- Real-time collaboration
- AI model training on user data
- Enterprise API access

---

**Related Documentation**:
- [AI Tools Guide](ai-tools.md)
- [Product Brief](product-brief.md)
- [Model Card](model-card.md)
- [Roadmap](roadmap.md)

