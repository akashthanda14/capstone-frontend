# Skill-based Micro-job Marketplace – University Edition

Project README for a React + Vite frontend (single-folder install) and suggested backend.

## Project Overview

This project is a skill-based micro-job marketplace designed primarily for university students, while also allowing external users to hire students for short, paid tasks. It connects students who want to monetize skills (design, coding, writing, tutoring, etc.) with peers and external clients needing quick, affordable services.

## Problem Statement

Many students have marketable skills but lack structured ways to find paid, short-term work. Others need low-cost, fast services but don't know who to hire. The platform fills this gap with a safe, verified campus-first marketplace.

## Objectives

- Provide a place for students to list services and find short gigs.
- Enable secure payments and payouts (virtual wallet or payment gateway).
- Support ratings, portfolios, and verified profiles.
- Make the product extensible for university integrations (career cell, alumni).

## Key Features

- User Profiles: skills, portfolio, verification status, ratings.
- Service Listings: create/edit service gigs with pricing and delivery time.
- Job Posting: post task requirements and receive proposals.
- Secure Payments: escrow-style payments; release on completion.
- Ratings & Reviews: feedback system to build trust.
- Search & Filters: by skill, price, delivery time, ratings.
- Chat/Notifications: in-app messaging and notifications.
- Admin Panel: verify users, resolve disputes, analytics.

## Suggested Technology Stack

- Frontend: React + Vite (this repo)
- Backend: Node.js (Express/Fastify) or Django/DRF
- Database: MongoDB or MySQL/Postgres
- Auth: JWT + optional SSO (university SSO)
- Payments: Razorpay / Paytm / Stripe (depending on region)
- Storage: AWS S3 or similar for portfolio assets

## Setup — create the React + Vite app in this folder (no new subfolder)

Run these commands in the project root (this folder). They will initialize a Vite + React app here and install dependencies.

```bash
# initialize a Vite + React project in the current directory
npm create vite@latest . -- --template react

# install dependencies
npm install

# start dev server
npm run dev
```

Notes:
- If your folder already contains files (like this README), Vite may prompt before overwriting; carefully confirm or back up files.
- To create a production build: `npm run build` and preview with `npm run preview`.

## Project Structure (recommended)

Example file/folder layout for the frontend React app:

- public/
- src/
	- api/               # API client wrappers (axios/fetch)
	- assets/            # images, icons
	- components/        # reusable UI components
		- Avatar/
			- Avatar.jsx
			- Avatar.css
		- Rating/
			- Rating.jsx
		- Modal/
			- Modal.jsx
	- features/          # feature-specific components and hooks
		- auth/
		- listings/
		- jobs/
		- chat/
	- pages/
		- Home.jsx
		- Profile.jsx
		- Listing.jsx
		- CreateListing.jsx
		- JobPost.jsx
		- Dashboard.jsx
	- routes/            # route definitions
	- styles/            # global styles and theme
	- utils/             # helpers
	- App.jsx
	- main.jsx
- package.json
- vite.config.js

## Components to implement (detailed)

Below is a prioritized list of React components and a short contract (props/state) for each. Start with the core pages and shared components.

- AppShell (layout)
	- Purpose: global header, footer, nav, and responsive layout.
	- Props: children

- Header / NavBar
	- Purpose: site navigation, search, auth quick links, wallet balance.
	- Props: user, onSearch(query)

- Auth components
	- LoginForm: email/sso/password flow, returns token on success.
		- Props: onSuccess(token)
	- SignupForm: student verification (university email), upload ID.
		- Props: onSuccess(user)

- ProfileCard / ProfilePage
	- Purpose: display user info, skills, portfolio, ratings.
	- Props: user, onMessage(userId), onHire(userId)

- ListingCard
	- Purpose: show a single service listing in search/result lists.
	- Props: listing (id, title, price, deliveryTime, seller, rating), onOpen(id)

- ListingForm (Create / Edit)
	- Purpose: create or edit a listing with title, description, price, delivery time, attachments, tags.
	- Props: listing?, onSubmit(listingData)

- JobPostForm
	- Purpose: create a job request for proposals.
	- Props: onSubmit(jobData)

- Wallet / Payments UI
	- Purpose: show balance, add funds, request payouts.
	- Props: balance, onAddFunds(amount), onRequestPayout(amount)

- Chat / Messaging
	- Purpose: real-time or near real-time messaging between buyer and seller.
	- Props: conversationId, currentUser

- Rating & Reviews
	- Purpose: submit and display ratings after job completion.
	- Props: reviewList, onSubmit(review)

- SearchBar + Filters
	- Purpose: search listings by skill, price, delivery time.
	- Props: onSearch, filters, onFilterChange

- Admin components
	- UserVerificationPanel, DisputeResolution
	- Props: adminUser

Edge cases to handle in components

- Empty states (no listings, no messages)
- Network errors and retries
- Large file uploads (show progress)
- Concurrent updates (race conditions on wallet)

## API contracts (example endpoints)

These are suggested REST endpoints; adapt to GraphQL if preferred.

- POST /api/auth/login -> { token }
- POST /api/auth/signup -> { user }
- GET /api/users/:id -> { user }
- GET /api/listings -> [{ listing }]
- POST /api/listings -> { listing }
- GET /api/listings/:id -> { listing }
- POST /api/jobs -> { job }
- POST /api/payments/create -> { paymentIntent }
- POST /api/payments/complete -> { success }
- GET /api/conversations/:id/messages -> [{ message }]

Auth: use bearer token in Authorization header. Protect routes that create/modify resources.

## Data shapes (examples)

Listing:
{
	id: string,
	title: string,
	description: string,
	price: number,
	currency: string,
	deliveryDays: number,
	tags: string[],
	sellerId: string,
	rating: number
}

User:
{
	id: string,
	name: string,
	email: string,
	university: string | null,
	verified: boolean,
	skills: string[],
	portfolio: [{ url, type, description }]
}

## Testing

- Frontend: use Jest + React Testing Library for components and basic integration tests.
- E2E: use Playwright or Cypress to cover critical flows (signup, create listing, purchase flow).

Example test cases:
- Create and view a listing (happy path).
- Purchase flow with mocked payment provider.
- Profile verification flow and restricted actions for unverified users.

## Deployment

- Frontend can be deployed to Vercel, Netlify, or static hosting from the Vite build output.
- Backend: deploy to Heroku, Render, DigitalOcean, or any container platform. Use HTTPS and environment-secure secrets for payment keys.

## Security & Privacy

- Verify university email for student accounts where required.
- Sanitize uploaded files and restrict file types.
- Use rate-limiting, input validation, and CSRF protection if using cookies.

## Next Steps & Roadmap

1. Scaffold frontend in this folder (run the Vite commands above). Back up existing files if prompted.
2. Implement auth flows and user profile pages.
3. Add listing creation and search.
4. Integrate a sandbox payment provider for testing.
5. Add messaging and reviews.

Optional extensions:
- Gamification and leaderboards.
- University career cell integration.
- Alumni and external client onboarding.

## Contributors

Add contributors and license information as needed.

---

If you want, I can also scaffold the initial React components and basic routes in this folder now (create `src/` with minimal components and `package.json`). Tell me if you want TypeScript or plain JavaScript.
