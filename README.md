# 🏡 ProZone — Real Estate Mobile App

ProZone is a real-estate discovery mobile application built with React Native and Expo. It connects property seekers, property owners, and agents through property discovery, listing management, inquiries, and inspection requests.

The application uses Supabase for authentication, database storage, secure role management, and property image uploads.

## ✨ Features

- 🔐 User registration, sign-in, and password recovery
- 👤 Seeker, Owner, and Agent accounts
- 🏠 Property discovery and filtering
- ❤️ Saved properties
- 📝 Property listing creation and management
- 📸 Property image uploads
- 💬 Property inquiries
- 📅 Inspection requests and status tracking
- 🛡️ Admin dashboard and role approval system
- 🔎 Agent and Owner verification workflow
- 🔒 Database security using Row Level Security (RLS)

## 🛠️ Tech Stack

| Technology | Purpose |
|---|---|
| React Native | Mobile application development |
| Expo | Development and build platform |
| Expo Router | Navigation and routing |
| TypeScript | Type-safe development |
| NativeWind | Styling |
| Supabase Auth | Authentication |
| Supabase PostgreSQL | Database |
| Supabase Storage | Property image storage |
| Git & GitHub | Version control |

## 👥 User Roles

### Seeker
- Discover and filter properties
- Save properties
- Contact property owners or agents
- Request property inspections
- Track inquiries and inspection requests

### Owner
- Request Owner account verification
- Create and manage property listings after approval
- Upload property photos
- Respond to inquiries and inspection requests

### Agent
- Request Agent account verification
- Create and manage property listings after approval
- Upload property photos
- Respond to inquiries and inspection requests

### Administrator
- Review Agent and Owner verification requests
- Approve, reject, or request additional information
- Review submitted verification documents
- Manage role approval decisions

## 🔐 Security

ProZone uses Supabase Row Level Security (RLS) and database policies to protect user data and restrict access.

- New accounts are assigned the Seeker role.
- Owner and Agent registration creates a pending role request.
- Listing permissions require the appropriate approved and verified role.
- Admin permissions are checked against the database.
- Users cannot authorize themselves by changing client-side role information.
- Verification documents are stored in a private storage bucket.

## 🚀 Getting Started

### 1. Clone the repository

```bash
git clone YOUR_REPOSITORY_URL
cd YOUR_PROJECT_FOLDER
```

### 2. Install dependencies

```bash
npm install
```

### 3. Configure Supabase

Create a Supabase project.

In your Supabase dashboard, open **Connect** and obtain your project URL and publishable key (or legacy anon key).

Create `.env.local` from `.env.example` and configure:

```env
EXPO_PUBLIC_SUPABASE_URL=https://your-project-id.supabase.co
EXPO_PUBLIC_SUPABASE_PUBLISHABLE_KEY=your-publishable-key
```

On Windows PowerShell:

```powershell
Copy-Item .env.example .env.local
```

### 4. Apply database migrations

Run the SQL migrations in timestamp order using the Supabase SQL Editor:

1. `20261106000000_initial_schema.sql`
2. `20261106000100_secure_profile_roles.sql`
3. `20261106000200_admin_approval_system.sql`
4. `20261107000100_secure_role_verification.sql`

If the earlier migrations are already applied, apply only the verification migration.

### 5. Configure password recovery

Add the following redirect URL under:

**Supabase → Authentication → URL Configuration → Redirect URLs**

```text
zone://reset-password
```

For web builds, also add your deployed website's `/reset-password` route.

### 6. Start the application

```bash
npm run start
```

Never expose database passwords or service-role keys in the mobile application. Only the publishable/anon key should be used by the client.

## 👨‍💻 Administrator Setup

ProZone does not include public administrator registration.

After creating an account and applying the migrations, the first administrator must be promoted through the Supabase SQL Editor by the project owner.

Replace `YOUR_ADMIN_EMAIL` with the email address of your own account:

```sql
update public.profiles
set role = 'admin'
where id = (
  select id
  from auth.users
  where lower(email) = lower('YOUR_ADMIN_EMAIL')
)
and role = 'seeker';
```

Verify that the update affects one row. Sign out and back in to reload the database role.

Administrator access must only be granted to accounts you control.

## 🧪 Validation

Before deployment, run:

```bash
npm run lint
npx tsc --noEmit
npx expo export --platform web
```

## ⚠️ Project Status

This project requires a configured Supabase project, database migrations, environment variables, and authentication settings before it can connect and operate.

Email delivery, notifications, legal pages, support operations, and production monitoring require additional setup.

## 📌 Developer

**Obiechina — Mobile App Developer**

GitHub: https://github.com/Obiechina-Mobile

Built with React Native, Expo, TypeScript, NativeWind, and Supabase.
