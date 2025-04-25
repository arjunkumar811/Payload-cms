
# Payload CMS + Next.js Starter

This is a full-stack starter template that integrates Payload CMS (backend) with a Next.js (frontend) application. Perfect for building custom content-driven web apps using modern tools like TypeScript, MongoDB, and React.

---

##  Project Structure

```
my-project/
├── cms/              ← Payload CMS (backend)
│   ├── payload.config.ts
│   ├── collections/
│   └── ...
├── frontend/         ← Next.js frontend
│   ├── pages/
│   ├── components/
│   └── ...
├── .env
└── README.md
```

---

##  Quick Start

### 1. Clone the Repo & Install Dependencies

```bash
git clone https://github.com/your-username/your-repo.git
cd my-project
```

Install CMS dependencies:

```bash
cd cms
npm install
```

Install Frontend dependencies:

```bash
cd ../frontend
npm install
```

---

### 2. Setup Environment

In the root of the project, create a `.env` file:

```
MONGODB_URI=your_mongodb_connection_string
PAYLOAD_SECRET=your_secure_random_string
```

You can get MongoDB free at https://cloud.mongodb.com.

---

### 3. Run Locally

Start CMS (backend):

```bash
cd cms
npm run dev
```

Start Next.js (frontend):

```bash
cd ../frontend
npm run dev
```

Access:
- Payload Admin Panel → http://localhost:3000/admin
- Frontend Site → http://localhost:3001 (or whichever port you set)

---

## Fetching Data from Payload in Next.js

Example to fetch blog posts from Payload:

```tsx
useEffect(() => {
  fetch('http://localhost:3000/api/posts')
    .then(res => res.json())
    .then(data => setPosts(data.docs));
}, []);
```

---

## Features

- ✅ Modern Payload CMS backend
- ✅ Custom collections (e.g., Posts)
- ✅ MongoDB database integration
- ✅ Next.js frontend rendering
- ✅ REST API or GraphQL support
- ✅ Fully typed with TypeScript

---

## 🛠 Tech Stack

- Payload CMS
- Next.js
- React
- MongoDB
- TypeScript
- Node.js





# CMS Integration Strategy – Payload CMS

> Powering a dynamic multi-tenant frontend using Payload CMS APIs.

---

## 1. Data & Collection Structure

To support a scalable, customizable frontend:

### Collections:

- **Users** – Admins or editors.
- **Tenants** – Organizational profiles (name, slug, theme).
- **Pages** – Dynamic page data (home, about, etc.).
- **Posts** – Blog or article content.
- **Media** – Uploaded assets.
- **Navigation** – Menu structures per tenant.

### 🔗 Relationships:
- Each `Page`, `Post`, and `Navigation` links to a `Tenant` via a `relationship` field.
- This ensures **multi-tenancy** support within one CMS instance.

---

## 2. Dynamic Content Fetching & Rendering

### Frontend Strategy:

1. **Identify Tenant**  
   From subdomain or route slug (e.g., `arjun.myapp.com` → `arjun`).

2. **Fetch Tenant Info**  
   ```ts
   GET /api/tenants?where[slug][equals]=arjun
   ```

3. **Fetch Page Content**  
   ```ts
   GET /api/pages?where[slug][equals]=home&where[tenant.slug][equals]=arjun
   ```

4. **Render Components Dynamically**  
   Use layout blocks (hero, CTA, etc.) to render content on the frontend.

---

##  Bonus: Sample Payload CMS Schemas

### `pages.ts`
```ts
const Pages = {
  slug: "pages",
  fields: [
    { name: "title", type: "text" },
    { name: "slug", type: "text", unique: true },
    {
      name: "tenant",
      type: "relationship",
      relationTo: "tenants"
    },
    {
      name: "layout",
      type: "blocks",
      blocks: [
        {
          slug: "hero",
          fields: [{ name: "heading", type: "text" }]
        },
        {
          slug: "cta",
          fields: [{ name: "buttonText", type: "text" }]
        }
      ]
    }
  ]
};
```

### `tenants.ts`
```ts
const Tenants = {
  slug: "tenants",
  fields: [
    { name: "name", type: "text" },
    { name: "slug", type: "text", unique: true },
    { name: "themeColor", type: "text" }
  ]
};
```

---

## Summary

Payload CMS makes it easy to:

- Structure multi-tenant content
- Fetch data dynamically using REST/GraphQL
- Build flexible frontends powered by reusable layout blocks

Use cases: multi-brand platforms, SaaS tools with client-facing portals, and more.
