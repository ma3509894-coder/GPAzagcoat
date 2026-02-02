# GPA - Medical Education Platform
(formerly ZagCoat)

## Overview
GPA is a medical education application that transforms complex medical concepts into memorable stories and mnemonics. It supports multiple Arabic dialects (Egyptian, Sudanese, Jordanian, Syrian, Saudi) and English, making medical learning more accessible and engaging.

## Tech Stack
- **Frontend**: React + TypeScript + Vite
- **Backend**: Express.js + TypeScript
- **Styling**: Tailwind CSS with custom medical theme
- **UI Components**: Shadcn/ui
- **State Management**: TanStack Query v5
- **Routing**: Wouter
- **Animations**: Framer Motion
- **Authentication**: Custom session-based auth with express-session

## Key Features
1. **Medical Text Conversion**: Convert complex medical texts into stories, mnemonics, and visual descriptions
2. **AI Voice-Over**: Listen to generated content with language-specific voices (OpenAI gpt-audio)
3. **AI Image Generation**: Generate visual illustrations from picture descriptions (OpenAI gpt-image-1)
4. **AI Chat Assistant**: Chat with an AI for medical learning assistance
5. **Dynamic Quiz Generation**: 1 question per 1.5 lines of text (min 3, max 15 questions)
6. **Related Videos**: Find educational videos from Vimeo and Dailymotion
7. **User Messaging**: Real-time messaging between users with online status
8. **Leaderboard & Gamification**: Streak tracking, friend system, and rankings
9. **Subscription System**: Free tier with premium upgrade options
10. **Dark/Light Theme**: Full theme support with system preference detection
11. **PWA Support**: Installable as mobile app shortcut on iOS and Android
12. **Shared Conversions**: All users can see conversions from other users

## Project Structure
```
├── client/
│   ├── public/
│   │   ├── manifest.json     # PWA manifest
│   │   ├── sw.js             # Service worker
│   │   ├── icon-192.png      # App icon
│   │   └── icon-512.png      # App icon large
│   ├── src/
│   │   ├── components/       # React components
│   │   ├── hooks/            # Custom React hooks
│   │   ├── lib/              # Utilities (queryClient)
│   │   └── pages/            # Page components (Home, Auth, Chat, UserChat, Leaderboard)
├── server/
│   ├── routes.ts             # API endpoints
│   ├── storage.ts            # In-memory storage (MemStorage)
│   ├── index.ts              # Express server setup
│   └── replit_integrations/  # AI audio utilities
├── shared/
│   └── schema.ts             # Type definitions and validation schemas
```

## API Routes
- `/api/auth/signup` - Create account with email, username, password
- `/api/auth/login` - Login with username OR email
- `/api/auth/logout` - Logout user
- `/api/auth/user` - Get current user
- `/api/user/mark-welcome-seen` - Mark welcome screen as seen
- `/api/user/language` - Update user language preference
- `/api/conversions` - CRUD for user's medical text conversions
- `/api/conversions/shared` - Get all conversions from all users
- `/api/chat` - AI chat streaming endpoint
- `/api/tts` - Text-to-speech using OpenAI gpt-audio (POST, streaming)
- `/api/generate-image` - AI image generation using gpt-image-1 (POST)
- `/api/search-videos` - Search related videos from Vimeo/Dailymotion (GET)
- `/api/user-chat/*` - User-to-user messaging
- `/api/leaderboard` - Global rankings (all users)
- `/api/friends/leaderboard` - Friends-only leaderboard
- `/api/friends/*` - Friend system management

## Authentication
- Custom session-based authentication using express-session
- Signup requires: username, email, name, password, language
- Login supports: username OR email
- AI-generated nickname on signup and each login (GPT-4o-mini)
- Welcome screen shown only on first login (tracked via hasSeenWelcome)

## Design Tokens
- Primary color: `#0079F2` (Medical Blue)
- Font families: Inter, Noto Sans Arabic, Tajawal
- Border radius: 0.5rem
- Supports RTL for Arabic text
- Mobile-first responsive design with touch-friendly targets

## User Schema
```typescript
{
  id: string,
  username: string,
  email: string (optional),
  name: string,
  passwordHash: string,
  language: "Egyptian" | "Sudanese" | "Jordanian" | "Syrian" | "Saudi" | "English",
  nickname: string (AI-generated),
  hasSeenWelcome: boolean,
  streak: number,
  subscriptionPlan: "free" | "premium",
  // ... other fields
}
```

## AI Integration
- **Model**: OpenAI gpt-4o-mini via Replit AI Integrations
- **TTS**: OpenAI tts-1-hd (high-quality TTS API) with MP3 output
- **Features**:
  - Story generation with dialect-specific prompts
  - Mnemonic creation (short sentence, acronym, rhyme, visual)
  - Picture description generation
  - Quiz generation with difficulty levels
  - Streaming AI chat for medical Q&A
  - AI nickname generation
- **Dialect Support**: Native expressions for each Arabic dialect
- **Humor Scale**: 1-10 levels from "very serious and formal" to "maximum humor"
- **Voice Mapping**:
  - Egyptian: shimmer
  - Sudanese: nova
  - Jordanian: echo
  - Syrian: onyx
  - Saudi: fable
  - English: alloy

## Recent Changes (January 2026)
- Initial implementation of GPA application
- **Jan 25, 2026 - Major Update**:
  - **Custom Auth**: Replaced Replit Auth with custom username/password auth
  - **Security**: Upgraded to bcrypt (12 salt rounds) for password hashing
  - **Email Support**: Users can signup with email and login with username OR email
  - **AI Nicknames**: New nickname generated on each login using GPT-4o-mini
  - **Welcome Screen**: Only shows on first login, persisted to user record
  - **Shared Conversions**: Added /api/conversions/shared for global content
  - **PWA Support**: Added manifest.json and service worker for mobile installation
  - **Mobile Responsive**: Touch-friendly form inputs (44px min-height), safe area support
  - **TTS Fix**: Improved text-to-speech with proper voice type handling
  - **UI Reorder**: Related videos now appear below AI images section
- **Jan 25, 2026 - Final Features**:
  - **MCQ Language Selection**: Users can choose quiz language independently of content language
  - **News/Announcements System**: Admin can create, edit, toggle, and delete news items
  - **News Auto-Display**: News popup shows to users after 24 hours if unviewed
  - **Payment Link Management**: Admin can manage payment links shown in subscription modal
  - **Admin Tabs**: Added News and Payment tabs to admin panel (8 tabs total)
- **Jan 25, 2026 - Premium Request System**:
  - **User-Admin Messaging**: Users can submit premium requests with custom messages
  - **Admin Response**: Admins can approve/reject requests with response messages
  - **Custom Duration**: Admins can set subscription duration (1-12 months) when approving
  - **Subscription Expiration**: Track and display subscription end dates
  - **Subscription Renewal**: Admins can extend existing premium subscriptions
  - **Request History**: Both users and admins can view past requests and responses
- **Jan 25, 2026 - Latest Updates**:
  - **Fixed OTP**: Master OTP set to fixed value "A32423242aaa" for development
  - **Email Update**: Admin notification email updated to ma2509894@gmail.com
  - **Stripe Removed**: Removed Stripe payment option, using admin-managed payment links only
  - **YouTube Integration**: Related videos now link to YouTube search results
  - **Faster Images**: Image generation optimized with 512x512 size and low quality for speed
  - **Customer Support**: Support tab added to Admin Panel (9 tabs total)

## Admin Panel
- **URL**: /admin
- **Master Email**: ma2509894@gmail.com
- **Master Phone**: +201033736098
- **Password**: Set via ADMIN_MASTER_PASSWORD environment variable

### Access Levels
- **Basic**: Email/password login - can manage users, premium requests, broadcasts
- **Master**: Requires OTP verification - can view stats, manage admin accounts

### Security Features
- Bcrypt password hashing (12 rounds)
- OTP rate limiting (5 attempts, 15-minute lockout)
- Master password from environment variable (not hardcoded)
- OTP only logged in development mode

### Admin Features
- View/delete users
- Approve/reject premium requests
- Send broadcast messages to users
- View analytics (requires OTP)
- Add/remove admin accounts (requires OTP)

## Notes
- **Email Notifications**: Gmail SMTP configured (requires GMAIL_USER and GMAIL_APP_PASSWORD secrets)
  - Sends notifications to ma3509894@gmail.com for: signups, logins, subscriptions, support requests
  - To enable: Add GMAIL_USER (your Gmail address) and GMAIL_APP_PASSWORD (Gmail App Password, not regular password)
  - Get App Password: Google Account → Security → 2-Step Verification → App Passwords
- Twilio integration needed for production SMS OTP (currently logs to console in dev)
- **Subscription**: 70 GPA = 70 LE per month, first month free for all new users
- **Customer Support**: Users can contact admin via Support button in header

## Recent Changes (January 2026 - Latest)
- Added Gmail SMTP email notifications for signup/login/subscription/support
- Added Customer Support system (users can message admin)
- Added Support tab in Admin Panel for managing support tickets (9 total tabs)
- New users get 1 month free premium on signup
- Max MCQ questions increased to 100
- Session persistence extended to 30 days
- Price updated to 70 GPA = 70 LE
