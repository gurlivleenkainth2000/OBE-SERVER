# OBE Platform — Server

> Status: Archived (2021-2022). Kept public for portfolio review. Not maintained.

The Node/Express/MongoDB backend for the OBE (Outcome-Based Education) Platform: the REST API and the attainment-computation engine behind the accreditation tool (NBA / Washington Accord).

This is one half of a two-repo project:

- Server (this repo): the API, data model, and attainment engine.
- Client: the Angular app, at [OBE-CLIENT](https://github.com/gurlivleenkainth2000/OBE-CLIENT).

It was my final-year project at Guru Nanak Dev Engineering College, built to meet a real accreditation reporting requirement for the college.

## What it does

- Authenticates faculty against the college's existing LDAP directory, so staff sign in with the accounts they already have.
- Stores courses, Course Outcomes, assessments, and CO to PO/PSO mappings.
- Accepts imported student marks and computes direct and indirect attainment, plus attainment gaps.
- Serves the data the client turns into accreditation PDF reports.
- Applies role-based filtering on what each user can see.

## Tech stack

- Node.js with Express 4
- MongoDB via Mongoose 6
- jsonwebtoken and bcrypt for auth
- ldapjs for institutional (LDAP) login
- cors, morgan, dotenv

## How it is put together

- Routes are split per resource (one router and one model per domain noun: courses, course outcomes, assessments, attainment, PO attainment, curriculum, and so on), mounted behind a single token-verification middleware.
- Documents are intentionally denormalised for read-cheap, write-fan-out access, rather than relying on joins at read time.
- One marks schema powers two collections (CIA and ESE), selected per request, which keeps the model simple.
- Attainment routes carry the core domain math: direct attainment, total CO attainment, and PO / PSO calculation.

## Running it locally

```bash
npm install
# create a .env file (see below)
npm start   # or: nodemon
```

Environment variables you will need:

- `MONGODB_PROD_URI` — MongoDB connection string
- a JWT secret
- LDAP connection settings for institutional login

> Note: as a graded college project this was never deployed to production, so secret management is basic. Set real secrets via `.env` before running anywhere that matters.

## Authors

Built by Gurlivleen Singh Kainth and Manjot Singh.

## License

Source-available for review only. See [LICENSE](./LICENSE). This is not open-source software: you may read and clone it to evaluate it, but reuse, redistribution, or submission as your own work is not permitted without written permission.
