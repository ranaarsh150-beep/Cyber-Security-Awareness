# CyberShield Hub 🛡️

## Project Overview
CyberShield Hub is a comprehensive **Cyber Security Awareness Platform** designed to educate users about modern digital threats. The application features a dynamic, single-page scrolling interface to browse articles, partake in quizzes, and explore detailed breakdowns of common cyber attacks and prevention methodologies. 

It also includes a fully protected, JWT-authenticated **Admin Dashboard** allowing authorized users to manage content across the platform seamlessly.

---

## Tech Stack
- **Frontend Engine**: [Next.js 16.2.1](https://nextjs.org/) (App Router) & [React 19](https://react.dev/)
- **Styling**: [Tailwind CSS v4](https://tailwindcss.com/)
- **Programming Language**: [TypeScript](https://www.typescriptlang.org/)
- **Database**: [MongoDB](https://www.mongodb.com/) via [Mongoose 9.3](https://mongoosejs.com/)
- **Authentication**: Custom JWT using [jose](https://github.com/panva/jose) & Password Hashing using [bcryptjs](https://www.npmjs.com/package/bcryptjs)
- **Scripting**: tsx for database seeding

---

## Architecture
The application leverages the robust **Next.js App Router** architecture handling both Front-End Views and Back-End RESTful API Routes within the same monolith. 

- **Server-Side Rendering (SSR)**: Fetches and pre-renders data directly from MongoDB within server components (e.g., `src/app/(site)/page.tsx`), providing optimal SEO and fast load times.
- **Client Components**: utilized strategically strictly for interactivity—like modals, forms, and dynamic UI state toggles (e.g., `src/components/home-sections.tsx`).
- **Middleware/Guards**: Admin endpoints are protected via an API guard (`src/lib/api-guard.ts`) that verifies HTTP-only JWT cookies to authorize CRUD operations.

---

## Folder Structure

\`\`\`
cyber-shield-hub-main/
├── scripts/
│   └── seed.ts                  # Database seeding script for mock data & initial admin user setup
├── src/
│   ├── app/
│   │   ├── (site)/              # Public-facing views (Homepage, Articles, Quizzes)
│   │   ├── admin/               # Admin Dashboard views (Protected Route)
│   │   └── api/                 # Next.js Serverless API Endpoints
│   ├── components/              # Reusable React components (UI, Modals, Forms)
│   ├── lib/                     # Utilities (DB connection, Auth/JWT handlers, Queries)
│   └── models/                  # Mongoose Schema Definitions
├── package.json                 # Project dependencies & scripts
└── tailwind.config.ts           # Tailwind CSS Configuration
\`\`\`

---

## Key Features
- **Scrolling Single-Page Layout**: Visitors can effortlessly scroll through the Home section, Types of Cyber Attacks, Prevention Methods, Conclusion, and external References using smooth-scrolling anchor links.
- **Interactive Modals**: Click "View Details" on any Attack or Prevention card to unveil a popup modal with comprehensive descriptions, images, and embedded educational YouTube videos.
- **Article & Quiz Engine**: Users can search via keywords and categories to read extensive articles or test their knowledge answering dynamically loaded quizzes.
- **Admin Dashboard**:
  - Secure login flow.
  - Complete CMS to Add, Edit, or Delete Articles, Quizzes, Cyber Attacks, Prevention Methods, and References.
  - WYSIWYG/Textarea forms for immediate database updates.

---

## API Endpoints

All data operations are primarily structured as standard RESTful endpoints under `/api`. 
*(Note: Most POST/PUT/DELETE endpoints require valid JWT Admin Auth).*

### Public / General
- \`GET /api/articles\` - Fetch paginated/searchable articles.
- \`GET /api/quizzes\` - Fetch quizzes available to users.

### Attacks & Preventions (CRUD)
- \`GET /api/attacks\` - List all cyber attacks.
- \`POST /api/attacks\` - (Admin) Create a new cyber attack record.
- \`PUT /api/attacks/:id\` - (Admin) Update an existing cyber attack.
- \`DELETE /api/attacks/:id\` - (Admin) Delete an attack record.
- \`GET /api/prevention\` - List all prevention methods.
- \`POST /api/prevention\` - (Admin) Create a new prevention method.
- \`PUT /api/prevention/:id\` - (Admin) Update an existing prevention method.
- \`DELETE /api/prevention/:id\` - (Admin) Delete a prevention method.

### Platform Content (CRUD)
- \`POST /api/conclusion\` - (Admin) Upsert the global conclusion statement.
- \`GET /api/references\` - List all reference links.
- \`POST /api/references\` - (Admin) Add a new reference link.
- \`PUT /api/references/:id\` - (Admin) Edit reference link.
- \`DELETE /api/references/:id\` - (Admin) Remove a reference link.

---

## Database Schema (MongoDB Collections)

1. **Admin**: \`{ email: String, passwordHash: String }\`
2. **Article**: \`{ title: String, slug: String, shortDescription: String, content: String, category: String, keywords: [String] }\`
3. **Quiz**: \`{ title: String, description: String, questions: [{ question: String, options: [String], correctIndex: Number }] }\`
4. **Attack**: \`{ title: String, shortDescription: String, description: String, image: String, youtubeUrl: String }\`
5. **Prevention**: \`{ title: String, shortDescription: String, description: String, youtubeUrl: String }\`
6. **Conclusion**: \`{ content: String }\` *(Singleton structure intended)*
7. **Reference**: \`{ title: String, url: String }\`

---

## Setup Instructions

**Prerequisites**: Node.js v20+ and an active MongoDB instance (Local or Atlas).

1. **Clone & Install Dependencies**
   \`\`\`bash
   npm install
   \`\`\`

2. **Environment Configuration**
   Create a \`.env\` file in the root directory:
   \`\`\`env
   MONGODB_URI="mongodb://127.0.0.1:27017/cybershield"
   SEED_ADMIN_EMAIL="admin@cybershield.local"
   SEED_ADMIN_PASSWORD="ChangeMe123!"
   JWT_SECRET="YourSuperSecretRandomStringHere"
   \`\`\`

3. **Seed Database**
   Populates MongoDB with required sample attacks, prevention methods, sample articles, quizzes, and initializes the Admin user.
   \`\`\`bash
   npm run seed
   \`\`\`

4. **Enhance Content & Link Images** (Optional)
   To enrich the seeded data with detailed descriptions, YouTube links, and automatically associate them with images in `public/uploads`:
   ```bash
   npm run reseed-content
   npm run link-images
   ```

5. **Run Development Server**
   \`\`\`bash
   npm run dev
   \`\`\`
   Navigate to [http://localhost:3001](http://localhost:3001) in your browser.

---

## Known Issues & Improvements

- **Image Upload Handling**: Currently, the platform relies on external physical URLs for images. Integrating AWS S3, Cloudinary, or local Next.js API storage handling would improve media independence.
- **Pagination on Homepage Views**: As the database of attacks and preventions scales up, fetching everything into a single-page view might degrade page load. Future enhancements should include infinite scrolling or "Load More" pagination.
- **Error Boundaries**: Client-side validation is basic. Wrapping components in dedicated Next.js Error Boundaries would improve resilience over network drops.
- **Rich Text Formatting**: While the platform supports textareas stringing out line breaks, implementing an actual Markdown or WYSIWYG editor (like TipTap or Quill) in the admin panel would augment layout designs significantly.
