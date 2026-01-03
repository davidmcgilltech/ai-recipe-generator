# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

AI-powered recipe generator built with React, TypeScript, and AWS Amplify Gen 2. Users enter ingredients and receive AI-generated recipes via Amazon Bedrock (Claude 3 Sonnet).

## Commands

```bash
# Development
npm run dev          # Start Vite dev server with HMR
npm run build        # TypeScript compile + Vite production build
npm run preview      # Preview production build locally
npm run lint         # Run ESLint

# Amplify Backend
npx ampx sandbox     # Deploy backend to isolated dev sandbox
npx ampx sandbox delete  # Tear down sandbox resources
```

## Architecture

### Frontend (React + Vite)
- `src/main.tsx` - App entry point, wraps with Amplify Authenticator
- `src/App.tsx` - Main component with ingredient form and recipe display
- Amplify UI components handle auth flow (sign up, sign in, password reset)

### Backend (AWS Amplify Gen 2)
- `amplify/backend.ts` - Main backend definition, registers auth + data + Bedrock data source
- `amplify/auth/resource.ts` - Cognito authentication config (email-based login)
- `amplify/data/resource.ts` - GraphQL schema defining `askBedrock` query
- `amplify/data/bedrock.js` - Custom resolver that calls Bedrock API with ingredient prompt

### Data Flow
1. User submits ingredients via form
2. Frontend calls `askBedrock` GraphQL query via Amplify Data client
3. AppSync routes to custom resolver (`bedrock.js`)
4. Resolver sends prompt to Bedrock Claude 3 Sonnet model
5. Response parsed and returned to frontend

### Key Files After Full Implementation
- `amplify_outputs.json` - Auto-generated client config (created by `npx ampx sandbox`)
- Backend resources deployed to AWS via Amplify CI/CD on git push

## AWS Services Used
- **Amplify Hosting** - CI/CD and static hosting
- **Cognito** - User authentication
- **AppSync** - GraphQL API
- **Lambda** - Serverless compute (via Amplify functions)
- **Bedrock** - Claude 3 Sonnet foundation model (us-east-1)
