# Amigo Academy

**Learn. Explore. Practice. Grow. Succeed.**

A free, curriculum-aligned digital learning platform built with South Sudanese learners in mind — from Early Childhood Development through Primary, Secondary, University and Professional Skills.

> Amigo Academy is not a government-approved or officially accredited platform. Content is labelled with a source and verification status throughout, and nothing is presented as an official examination paper or curriculum document unless independently verified.

This is the **Phase 1 frontend** — HTML, CSS and vanilla JavaScript, backed by structured JSON demo data. No backend, database, or paid service is required to run it.

---

## What's in this version

Rebuilt from the original single-file "Amigo Learning" landing page (kept for reference at `legacy/amigo-learning-original.html`) into a proper multi-page structure, on the same navy / gold / paper visual identity.

| Area | Status |
|---|---|
| Homepage (hero, academies, curriculum explorer, exam centre, tutor teaser, teacher/parent teaser, FAQ) | ✅ Built |
| Courses listing + course detail page (modules, lessons, mark-complete progress) | ✅ Built, functional |
| Quiz Centre (filters, 10-question rounds, practice/exam mode, scoring, review, attempt history) | ✅ Built, functional |
| Video Centre (filters, modal player, official YouTube embeds) | ✅ Built, functional — **sample video entries need verification before publishing, see below** |
| Past Papers Centre (filters by level/subject/year, status labelling) | ✅ Built, functional |
| University Hub (universities/faculties, course tabs) | ✅ Built, functional |
| AI Tutor (demo interface, mode switching, small local topic library) | ✅ Built — **demo only, no live AI connected** |
| Learner Dashboard (stats, continue learning, recent results, simple recommendations) | ✅ Built, uses `localStorage` |
| Resource Library (unified search across courses/videos/papers/resources) | ✅ Built, also powers global search |
| Teacher Centre / Parent Centre / About | ✅ Built as content pages — most sub-sections show "Content coming soon" |
| ECD/Primary/Secondary quiz content | ✅ Sample questions only — small starter set, expand via `data/questions.json` |
| PWA (manifest, service worker) | ⚠️ `manifest.json` included; no service worker/offline caching yet (Phase 8) |
| User accounts / auth | ❌ Not in this phase (Phase 3) |
| Real backend / database | ❌ Not in this phase (Phase 4) |

**Important on the video data:** `data/videos.json` entries carry `"status": "Example entry — verify link before publishing"`. The titles/channels are representative examples of real educational channels, but the specific YouTube IDs have not been individually verified against live videos in this session. Check each one before treating it as real content — swap in verified IDs or remove entries as needed.

---

## Project structure

```
amigo-academy/
  index.html                 Homepage
  manifest.json               PWA manifest
  .gitignore
  css/
    main.css                  Design tokens, layout, nav/footer, buttons, cards
    components.css             Quiz engine, tabs, tutor chat, dashboard, course book
  js/
    app.js                    Shared header/footer/nav injection (all pages)
    data.js                   fetch() wrapper for /data JSON, with caching
    dashboard.js               localStorage progress store (quiz attempts, course %, streak)
    courses.js / course-detail.js / quizzes.js / videos.js /
    past-papers.js / university.js / tutor.js / dashboard-page.js / resources.js
                               One file per page's behaviour
  data/
    academies.json  curriculum.json  courses.json  questions.json
    videos.json  past-papers.json  universities.json  resources.json
  pages/
    courses.html  course.html  quizzes.html  videos.html  past-papers.html
    university.html  tutor.html  dashboard.html  resources.html
    teachers.html  parents.html  about.html
  assets/
    images/  icons/  documents/
```

## Run it locally

The site loads JSON with `fetch()`, which browsers block for files opened directly from disk (`file://`). Run a tiny local server instead — no install needed if you have Python:

```bash
cd amigo-academy
python3 -m http.server 8000
```

Then open **http://localhost:8000** in your browser.

No Python? Any of these work the same way:
- VS Code: install the "Live Server" extension, right-click `index.html` → *Open with Live Server*
- Node: `npx serve .`

## How to add content

All content-adding is done by editing JSON files in `data/` — no HTML/JS changes needed for new entries.

- **Add a course** → append an object to `data/courses.json` (see existing entries for the shape: `id`, `courseCode`, `title`, `academy`, `level`, `modules[]`, etc.)
- **Add quiz questions** → append to `data/questions.json` (`subject`, `level`, `type`, `options[]`, `correctIndex`, `explanation`)
- **Add a video** → append to `data/videos.json` with a real, verified `youtubeId`, and set `"status"` to reflect its actual verification state
- **Add a past paper** → append to `data/past-papers.json`, and use an honest `status` (`Teacher Created`, `Verified`, `Official Source`, etc.) — never label something "Official Past Paper" unless the source is verified
- **Add a university/faculty** → append to `data/universities.json`
- **Add a general resource** → append to `data/resources.json`

## Testing done this session

- All 8 JSON files validated as well-formed JSON
- All 12 JS files passed `node --check` (syntax valid)
- Every page and asset served with a `200` over a local HTTP server (routes, data, CSS, JS)
- Cross-checked every `getElementById()` call in each page's JS against that page's HTML — no missing element IDs
- Manual review of script load order on every page (`app.js` → `data.js` → `dashboard.js` → page script)
- Responsive breakpoints included at 980px / 860px / 640px (nav collapses to a slide-in drawer under 980px)

**Not yet tested:** a real browser session (this sandbox has no headless-browser access), so please click through the main journey once locally — home → academy → course → quiz → dashboard — before you rely on it.

## Push to GitHub

```bash
cd amigo-academy
git init
git add .
git commit -m "Amigo Academy: Phase 1 frontend"
git branch -M main
git remote add origin <your-repo-url>
git push -u origin main
```

## Roadmap (matches the phases in the original brief)

1. **Frontend** — this build
2. **Content engine** — this JSON structure is already shaped for it; grow the datasets
3. **User accounts** — real authentication, replacing the localStorage dashboard
4. **Database** — Django + PostgreSQL, matching the entities implied by the JSON shapes (User, Course, Module, Lesson, Quiz, Question, QuizAttempt, Enrollment, etc.)
5. **Learning management** — enrollment, real progress tracking, certificates, achievements
6. **Admin & teacher management** — the Create → Draft → Submit → Review → Approve → Publish workflow
7. **Real AI Tutor** — swap `getDemoResponse()` in `js/tutor.js` for a call to your own backend endpoint (never call an AI provider directly from frontend code — that exposes API keys)
8. **PWA & offline learning** — add a service worker and caching strategy on top of the existing `manifest.json`
9. **Production** — security review, performance pass, monitoring, deployment
