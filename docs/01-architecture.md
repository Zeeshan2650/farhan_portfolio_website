# Phase 1: Requirement Analysis and Website Architecture

Project: Farhan Ali, Environmental Specialist and Air Quality Monitoring and Analysis Expert
Status: Waiting for approval before Phase 2 (UI/UX design)
Prepared: 21 November 2026

---

## 1. CV review status

The CV PDF was not received in this session. No PDF was found in the repository or in the session upload folders. Everything below is built from the detailed information in the project brief, which states that it was taken from the updated CV.

Before any content is loaded into the database, I will compare it line by line against the PDF. Please upload it again (see section 10).

### Points that need confirming against the CV

| # | Item | Why it matters |
|---|------|----------------|
| 1 | Total experience. The three jobs run from June 2022 to today, which is about 4 years and 5 months. The CV says "almost 5 years". | The homepage statistic must match the CV exactly. Is there earlier work, an internship, or a training period that the CV counts? |
| 2 | Graduation year and dates for the B.Sc. | The education timeline needs a start and end year. |
| 3 | Which employer each project was delivered under. The named clients are all in Pakistan, which suggests ESPAK. | Each project page will say "Delivered while working with ...", so this must be correct. |
| 4 | Your role on the projects (for example field analyst, monitoring team member, team lead). | The brief asks not to imply you managed every project alone. Each project gets a "My role" field. |
| 5 | Co-authors of the IntechOpen book chapter and the chapter abstract. | A publication page must list authors accurately. I will not write an abstract myself; I will use the published one. |
| 6 | Month and certificate number for the ERA training (2026). | The EIA certificate has both (July 2026, RETC-10075). The ERA entry currently has only the year. |
| 7 | Any CV item not mentioned in the brief (memberships, software skills such as Excel, Openair or GIS, references, nationality, driving licence, Iqama status). | The brief says nothing may be omitted from the CV. |

---

## 2. Content inventory (initial database content)

This is what will be loaded as the starting content. All of it is editable from the admin dashboard afterwards.

| Area | Entries |
|------|---------|
| Profile | Name, designation, summary, location (Saudi Arabia), email farhan.ali@recec.sa, phone +966 50 562 1451, LinkedIn linkedin.com/in/engr-farhanali |
| Experience | 3 entries: REC Saudi Arabia (Oct 2025 to present, 10 responsibilities), IPC Pakistan (Nov 2024 to Sep 2025, 7), ESPAK Pakistan (Jun 2022 to Nov 2024, 8) |
| Expertise categories | 5: Ambient Air Quality Monitoring (9 items), Gaseous Pollutant Monitoring (7), Air Quality Data Analysis (10), Stack Emission Monitoring (6), Other Environmental Specializations (13) |
| Equipment | 3 categories, 12 entries. Technical specifications left empty until verified. |
| Project categories | 7: Cement, Automotive, Oil and Gas, Textile, Housing Societies, Healthcare, Hydropower |
| Projects | 16 client entries across the 7 categories |
| Education | 1 degree with 1 final year project |
| Certifications | 2 (EIA and ERA, RETC Saudi Arabia) |
| Publications | 1 book chapter, IntechOpen, 15 January 2024 |
| Awards | 2 |
| Languages | 3: English, Urdu, Punjabi |

Note on sectors: the summary lists eight sectors including petrochemical. Petrochemical experience comes from the IPC employment rather than a named project, so it will appear on the experience timeline and in the sector list, but not as a project card unless you want one.

### Rule for statistics

Homepage numbers will be counted live from the database (for example "16 client engagements", "7 industry sectors", "1 publication"). Nothing is typed in by hand except years of experience, which will use your CV wording. This keeps every number honest and keeps it correct when you add new content.

---

## 3. Sitemap

### Public website

| Page | Address | Main content |
|------|---------|--------------|
| Home | `/` | Hero, statistics, air quality focus, featured projects, equipment, timeline, publication, certifications, latest posts, contact call to action |
| About Me | `/about` | Biography, summary, languages, awards summary, photo |
| Professional Experience | `/experience` | Interactive employment timeline |
| Technical Expertise | `/expertise` | All five specialization groups, air quality shown first and largest |
| Expertise detail | `/expertise/[slug]` | Description, photos, technical details, documents |
| Air Quality Monitoring | `/air-quality-monitoring` | Dedicated flagship page: AQMS, pollutants, data analysis workflow, standards (NCEC, NEQS) |
| Environmental Services | `/services` | EIA, IEE, baseline studies, audits, EMP, noise, water, wastewater and more |
| Projects Portfolio | `/projects` | Filterable grid by sector, with search |
| Project detail | `/projects/[slug]` | Overview, scope, parameters, equipment, methodology, photos, documents, my role |
| Monitoring Instruments | `/instruments` | Equipment grouped by category |
| Instrument detail | `/instruments/[slug]` | Photo, application, parameters, principle, documents |
| Education | `/education` | Degree and final year project |
| Certifications | `/certifications` | Certificate gallery with image preview and PDF download |
| Research and Publications | `/publications` and `/publications/[slug]` | Publication list and detail |
| Awards and Achievements | `/awards` | Awards list |
| Blog and Insights | `/blog`, `/blog/[slug]`, `/blog/category/[slug]`, `/blog/tag/[slug]` | Articles with categories, tags and search |
| Gallery | `/gallery` and `/gallery/[album]` | Albums with lightbox preview |
| Contact | `/contact` | Contact form, details, LinkedIn |
| Download CV | `/cv` | CV preview and download of the latest uploaded file |
| Search | `/search` | Search across projects, posts, expertise and equipment |
| System | `/sitemap.xml`, `/robots.txt`, custom 404 page, Open Graph images | |

### Admin dashboard (login required)

| Module | Address |
|--------|---------|
| Login | `/admin/login` |
| Overview | `/admin` |
| Profile | `/admin/profile` |
| Experience | `/admin/experience` |
| Projects and categories | `/admin/projects`, `/admin/projects/new`, `/admin/projects/[id]`, `/admin/project-categories` |
| Expertise and categories | `/admin/expertise` |
| Equipment | `/admin/equipment` |
| Education | `/admin/education` |
| Certifications | `/admin/certifications` |
| Publications | `/admin/publications` |
| Awards | `/admin/awards` |
| Languages | `/admin/languages` |
| Blog posts, categories, tags | `/admin/blog` |
| Gallery and albums | `/admin/gallery` |
| Contact messages | `/admin/messages` |
| Media library | `/admin/media` |
| Settings (general, navigation, SEO, homepage sections, theme) | `/admin/settings` |
| Admin users and roles | `/admin/users` |

Awards, languages and the media library were not in the module list of the brief, but they are needed so that every section of the site can be edited.

---

## 4. Database architecture

Database: PostgreSQL on Supabase. Every table has `id` (UUID), `created_at` and `updated_at` (updated automatically by a trigger). Content tables also have `status`, `sort_order` and `published_at`.

### Shared types

| Type | Values |
|------|--------|
| `content_status` | draft, published, unpublished, archived |
| `admin_role` | owner, editor |
| `message_status` | unread, read, archived |

### Tables

| # | Table | Key columns | Relationships |
|---|-------|-------------|---------------|
| 1 | `admin_users` | user_id, role, display_name | user_id links to Supabase `auth.users` |
| 2 | `profile` | full_name, designation, short_intro, biography (rich text), summary, photo_id, cv_file_id, email, phone, location, linkedin_url, experience_label | Single row. photo_id and cv_file_id link to `media` |
| 3 | `profile_stats` | label, value, source (manual or automatic count), sort_order | Homepage statistics |
| 4 | `experiences` | organization, organization_short, position, location, country, start_date, end_date, is_current, summary, responsibilities (ordered list), logo_id, status | |
| 5 | `education` | degree, institution, location, start_year, end_year, description, status | |
| 6 | `education_projects` | education_id, title, description, image_id | Belongs to `education` |
| 7 | `project_categories` | name, slug, description, icon, sort_order | |
| 8 | `projects` | title, slug, category_id, experience_id, client_name, hide_client_name, client_alias, location, overview, scope_of_work, parameters (list), methodology, my_role, standards (list), project_date_text, is_featured, status, sort_order | category_id links to `project_categories`; experience_id links to the employer |
| 9 | `project_media` | project_id, media_id, kind (image or document), caption, sort_order | Multiple photos and documents per project |
| 10 | `project_equipment` | project_id, equipment_id | Many to many: equipment used on a project |
| 11 | `expertise_categories` | name, slug, summary, is_primary, sort_order | is_primary marks Air Quality for top placement |
| 12 | `expertise_items` | category_id, title, slug, description (rich text), technical_details, status | |
| 13 | `expertise_media` | expertise_item_id, media_id, kind, caption | Photos and supporting documents |
| 14 | `equipment_categories` | name, slug, sort_order | |
| 15 | `equipment` | name, slug, category_id, manufacturer, model, description, application, parameters (list), measurement_principle, principle_verified, status | Specification fields stay empty until verified |
| 16 | `equipment_media` | equipment_id, media_id, kind, caption | Photos and datasheets |
| 17 | `certifications` | title, issuer, issuer_location, issue_date_text, certificate_number, image_id, document_id, status | |
| 18 | `publications` | title, slug, authors, publication_type, book_title, publisher, published_on, external_url, doi, summary, cover_id, document_id, research_category, status | |
| 19 | `awards` | title, awarding_body, event, date_text, description, image_id, status | |
| 20 | `languages` | name, proficiency, details, sort_order | |
| 21 | `blog_categories` | name, slug, description | |
| 22 | `blog_tags` | name, slug | |
| 23 | `blog_posts` | title, slug, excerpt, content_json, content_html, featured_image_id, category_id, seo_title, seo_description, status, published_at, reading_minutes, author_id | |
| 24 | `blog_post_tags` | post_id, tag_id | Many to many |
| 25 | `gallery_albums` | title, slug, description, cover_id, status | |
| 26 | `gallery_items` | album_id, media_id, title, description, taken_on, sort_order | |
| 27 | `contact_messages` | name, email, subject, message, status, ip_hash, user_agent | Visitors can only insert, never read |
| 28 | `site_settings` | site_name, logo_id, favicon_id, contact email, phone, address, social_links, footer_text, default_seo, theme_default | Single row |
| 29 | `navigation_items` | label, href, location (header or footer), parent_id, sort_order, is_visible | |
| 30 | `seo_metadata` | route_path, title, description, og_image_id, no_index | One row per page that needs custom SEO |
| 31 | `homepage_sections` | key, title, subtitle, is_visible, sort_order | Turn homepage blocks on or off and reorder them |
| 32 | `media` | bucket, path, file_name, mime_type, size_bytes, width, height, alt_text, uploaded_by | Every uploaded file is recorded here |
| 33 | `activity_log` | user_id, action, entity_type, entity_id, summary | "Recent website activity" on the dashboard |

All 22 tables from the brief are covered. The extra tables (join tables, media, statistics, homepage sections, activity log) are what make photos, documents and reordering work properly.

### Indexes and constraints

1. Unique slugs on projects, expertise items, equipment, publications, blog posts, categories, tags and albums.
2. Indexes on every foreign key, on `status`, and on `sort_order`.
3. Full text search index on projects, blog posts, expertise and equipment for the site search.
4. Check constraints: end date after start date, valid email format on contact messages, message length limits.
5. Deleting a project removes its photo links, but files in `media` are only removed when no other record uses them.

### Security rules in the database (Row Level Security)

1. Visitors (not logged in) can read only rows where `status = 'published'`, plus the profile, settings, navigation, languages and visible homepage sections.
2. Visitors can insert a contact message, but cannot read, edit or delete any message.
3. Only logged in users listed in `admin_users` can create, edit or delete anything. This is checked by a database function `is_admin()`, so even someone who signs up through Supabase directly gets no access.
4. Only the `owner` role can manage other admin users and site settings.
5. Public sign up will be switched off in Supabase.

### File storage (Supabase Storage)

| Bucket | Access | Contents |
|--------|--------|----------|
| `public-media` | Anyone can view, only admins can upload | Photos, logos, certificate images, publication covers, gallery |
| `documents` | Anyone can view, only admins can upload | CV PDF, certificate PDFs, public technical documents |
| `private-documents` | Admins only, shared through short lived links | Confidential project reports you do not want public |

Uploads are checked on the server for file type and size (images: JPG, PNG, WebP, AVIF up to 8 MB; documents: PDF up to 20 MB).

### First administrator account

No password will ever be written into the code. You will create your login in the Supabase dashboard, then run one short SQL command (I will give it to you) that marks your account as owner.

---

## 5. Folder structure

```
farhan_portfolio_website/
  docs/                       Guides written for you (this file, setup, deployment)
  supabase/
    migrations/               SQL files that build the database, run in order
    seed.sql                  Your initial CV content
  public/                     Static files such as default icons
  src/
    app/
      (site)/                 All public pages
        page.tsx              Home
        about/  experience/  expertise/  air-quality-monitoring/
        services/  projects/  instruments/  education/
        certifications/  publications/  awards/  blog/
        gallery/  contact/  cv/  search/
      admin/
        login/                Login page
        (dashboard)/          Every protected admin module
      api/                    Server endpoints (CV download, uploads)
      sitemap.ts  robots.ts  not-found.tsx  layout.tsx
    components/
      ui/                     shadcn/ui base components
      site/                   Header, footer, theme switch, cards, timeline
      sections/               Homepage and page sections
      admin/                  Tables, editors, upload widgets, forms
    lib/
      supabase/               Secure connection helpers (browser and server)
      data/                   Functions that read content for public pages
      actions/                Server actions that save admin changes
      validation/             Zod rules shared by forms and server
      seo.ts  utils.ts
    proxy.ts                  Protects /admin routes before the page loads
  .env.example                List of settings you fill in (no secrets inside)
  README.md
```

---

## 6. Technology stack in simple words

| Tool | Version today | What it does for your site |
|------|---------------|----------------------------|
| Next.js (App Router) | 16.4 | The framework that builds both the public website and the admin pages. Pages are prepared on the server so Google sees full content and pages load fast. |
| React | 19.3 | The library used to build the screen components. |
| TypeScript | latest | Catches mistakes in the code before they reach visitors. |
| Tailwind CSS | 4.3 | Styling system that keeps spacing, colours and layout consistent. |
| shadcn/ui | 4.23 | High quality buttons, dialogs, tables and form fields that we own and can restyle. |
| Motion (formerly Framer Motion) | 14.14 | Smooth, restrained animations. |
| Supabase | supabase-js 2.119 | Your database, login system and file storage in one service. Free plan is enough to start. |
| React Hook Form and Zod | 7.92 and 4.6 | Forms with clear error messages, and the same checks repeated on the server. |
| Tiptap | 3.35 | The rich text editor for blog posts and long descriptions. |
| Lucide | 1.77 | Clean line icons. |
| Vercel | hosting | Publishes the website and rebuilds it every time code is pushed to GitHub. |

How updates reach visitors: when you save something in the dashboard, the server tells Next.js to refresh only the affected pages. Visitors see the change within seconds, while pages remain fast because they are cached.

Contact form protection: server side validation, a hidden honeypot field that catches bots, and a limit of a few messages per visitor per hour. Optional email notification for new messages (see decision B in section 10).

---

## 7. Visual design direction

The goal is the feel of an international environmental consultancy rather than a student template: calm, precise, data focused, with plenty of white space.

### Colour use

| Role | Colour | Where |
|------|--------|-------|
| Primary teal | #0F7C82 | Buttons, links, active states, key figures |
| Deep teal | #145B65 | Header on scroll, footer, dark sections |
| Forest green | #1E6B50 | Secondary accents, sector tags, success states |
| Background | #FFFFFF | Main page background |
| Secondary background | #F5F8F7 | Alternating sections, cards |
| Text | #202B33 | Body text |
| Muted text | #66757D | Captions, dates, labels |

Dark mode uses a deep blue green background (close to #0E1A1D) with the same teal lifted slightly for contrast. All text pairs will be checked against the WCAG AA contrast standard.

### Typography

1. Headings: Manrope, a modern geometric sans serif with an engineering feel.
2. Body text: Inter, highly readable at small sizes.
3. Data labels and units (PM2.5, µg/m³, SO2): JetBrains Mono, which gives technical sections an instrument panel character.

### Signature elements

1. Hero: large professional photo on one side, headline and buttons on the other, with a slim "parameter strip" listing the pollutants you monitor (PM10, PM2.5, SO2, NO2, CO, O3, H2S, VOCs) in monospace. No invented readings or values.
2. Air quality given clear priority: the first and largest expertise block, its own flagship page, and a short visual of the data workflow (collection, validation, QA/QC, averaging, exceedance check, reporting).
3. Timeline: vertical line with employer cards that expand to show responsibilities.
4. Project cards: sector tag, location, parameter chips; filter bar by sector.
5. Subtle contour line pattern (like a wind or terrain map) used sparingly as a section background. No cartoon illustrations, no heavy gradients.
6. Photography: your real field photos (stations, sampling trains, site work). Where a photo is missing, a clean neutral placeholder is shown instead of stock images pretending to be your work.

Phase 2 will turn this into visual mockups of the homepage, about, projects, expertise, timeline, blog, contact and admin dashboard for your approval.

---

## 8. Development roadmap

| Phase | Work | You will be able to |
|-------|------|---------------------|
| 1 | This document | Approve the structure |
| 2 | Visual design mockups | See and approve the look |
| 3 | Public pages built with reusable components | Click through every page locally |
| 4 | Supabase database, migrations, security rules, storage | See your content stored in the database |
| 5 | Admin login and protected routes | Log in securely |
| 6 | All admin modules with create, edit, delete, reorder, publish | Manage every section |
| 7 | Public pages connected to the database | See dashboard changes appear on the site |
| 8 | Uploads for photos, CV, certificates, documents | Upload files from the dashboard |
| 9 | Testing (pages, mobile, login, permissions, uploads, contact form, SEO) | Read a test report with real results |
| 10 | Deployment to Vercel with Supabase and your domain | Use the live site |

At the end of each phase I will commit the work to GitHub on branch `claude/nice-carson-syk9ey` and wait for your approval.

---

## 9. Honest limitations

1. I cannot create Supabase, Vercel or domain accounts for you or log into them. When we reach those steps I will give exact click by click instructions, and you share only the values the site needs.
2. Equipment specifications and measurement principles will stay empty unless you confirm them or provide the manufacturer datasheet. This follows your instruction not to invent specifications.
3. Photographs must come from you. I will not use stock images that look like your own project work.
4. I will not write monitoring results, client feedback or project statistics. Project pages will contain only the scope and parameters stated in your CV until you add more.

---

## 10. What I need from you

1. Upload the CV PDF again in this chat so I can verify every item in section 1.
2. Answers to the seven points in section 1.
3. Decision A: Should client names be shown by default, with the option to hide them per project? (Default plan: shown.)
4. Decision B: Do you want an email notification when someone sends a contact message? This needs a free Resend account. Without it, messages are still stored and shown in the dashboard.
5. Decision C: Do you already own a domain (for example farhanali.com)? If not, it can be added later.
6. Your professional photograph and any field photos you are happy to show publicly (these can come later, during Phase 8).
7. Approval to move to Phase 2.
