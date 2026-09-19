# Skill-Squad-frontend

The **React front-end shell for SkillSquad**, an online-learning / interview-preparation web app. It provides the routing skeleton of the public site (Home, About, Services, Courses, Contact), a shared navbar and footer, and an animated **login / sign-up box** with password validation and Google sign-in. It is an early UI prototype: the pages only show their titles and the account box is the only part that talks to an API.

Related repositories: [Skill-Squad-backend](https://github.com/SanaAkram/Skill-Squad-backend) (DRF registration/login API) and [Django-SkillSquad](https://github.com/SanaAkram/Django-SkillSquad) (the full server-rendered LMS).

## What is in it

- **Site pages** — `Home`, `About`, `Services`, `Courses`, `Contact` (react-router v5, `Switch`), plus `Navbar` and `Footer`.
- **Account box** (`src/Components/accountBox`) — login and sign-up panels that slide into each other (framer-motion). Forms use **Formik** and **Yup**; sign-up has a password-strength bar and a validation helper.
- **Google sign-in** via `react-google-login` (`Gsignin.js`); the returned profile is stored through a small Redux reducer.
- A `Dashboard` stub (just a heading; not routed yet).

## Architecture

```
Browser ── React 17 SPA (this repo)
   │   routes: /Home  /about  /service  /Courses  /contact    (default view: account box)
   │
   └── axios ──► POST http://127.0.0.1:8000/Kavtech/login/      {username, password}
                 POST http://127.0.0.1:8000/Kavtech/register/   {username, email, password}
```

```
src/
├─ index.js, App.js                Router, layout
├─ pages/                          Home · About · Service · Courses · Contact
└─ Components/
   ├─ Navbar/, Footer/, Dashboard/, marginer/, Account.js
   ├─ accountBox/                  index · loginForm · signup · Gsignin · accountContext
   │                               common · styles · validity.css · reducers/ (auth)
   └─ constants/actionTypes.js
```

The endpoints above are the ones the forms currently call: they follow the **Kavtech HRM** API contract ([KavtechHRM_backend](https://github.com/SanaAkram/KavtechHRM_backend)), because this project was started from the same UI. The dedicated [Skill-Squad-backend](https://github.com/SanaAkram/Skill-Squad-backend) exposes `POST /api/registration` and `POST /api/login` (with `confirm_password` / `email`), so to use that backend, change the two URLs and payloads in `loginForm.jsx` and `signup.jsx`.

## Stack

React 17 · React Router 5 · Redux + redux-thunk · Formik + Yup · styled-components · Material UI · Bootstrap 4 · framer-motion · axios · `react-google-login` · Create React App (`react-scripts` 4.0.2)

## Getting started

Prerequisites: Node.js 14 or 16 with npm (on Node 17+ set `NODE_OPTIONS=--openssl-legacy-provider` for react-scripts 4).

```bash
git clone https://github.com/SanaAkram/Skill-Squad-frontend.git
cd Skill-Squad-frontend
npm install
npm start           # http://localhost:3000
```

The pages render without a backend; login and sign-up need one running on `127.0.0.1:8000` (see above).

### Google sign-in

`Gsignin.js` contains a Google OAuth client id. Create your own in the Google Cloud console (Credentials → OAuth client ID → Web application), add `http://localhost:3000` as an authorised JavaScript origin and put it there.

## Scripts

| Command | What it does |
|---|---|
| `npm start` | Dev server |
| `npm run build` | Production build into `build/` |
| `npm run deploy` | Publishes `build/` to GitHub Pages with `gh-pages` (change `homepage` in `package.json` first — it is a local URL) |

`npm test` is a placeholder.

## Status / notes

- Login and sign-up currently only show an alert / message on success — no token is stored or used to guard pages.
- The Home, About, Services, Courses and Contact pages are placeholders — each renders only its title.
- `main.py` is a leftover PyCharm sample and is not part of the app.
