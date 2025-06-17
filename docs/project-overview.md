# 🏗️ MedhirWeb Project Structure

## 📁 Directory Structure

```plaintext
.
├── src/                      # Source code directory
│   ├── components/           # Reusable UI components
│   │   ├── request-tables/   # Request management components
│   │   │   ├── SuperadminHeaders.js
│   │   │   ├── RequestDetails.jsx
│   │   │   └── RequestTable.jsx
│   │   ├── ui/              # Base UI components
│   │   │   ├── Button.jsx
│   │   │   ├── Input.jsx
│   │   │   └── Modal.jsx
│   │   └── withAuth.js      # Authentication HOC
│   │
│   ├── pages/               # Next.js pages
│   │   ├── api/            # API routes
│   │   ├── employee/       # Employee portal pages
│   │   │   ├── dashboard.js
│   │   │   └── requests.js
│   │   ├── hradmin/       # HR Admin portal pages
│   │   │   ├── dashboard.js
│   │   │   ├── employees.js
│   │   │   └── requests.js
│   │   ├── manager/       # Manager portal pages
│   │   │   ├── dashboard.js
│   │   │   └── team.js
│   │   └── superadmin/    # Superadmin portal pages
│   │       ├── dashboard.js
│   │       └── companies.js
│   │
│   ├── redux/              # State management
│   │   ├── slices/        # Redux slices
│   │   │   ├── authSlice.js
│   │   │   ├── leadSlice.js
│   │   │   └── requestSlice.js
│   │   └── store.js       # Redux store configuration
│   │
│   ├── hooks/             # Custom React hooks
│   │   ├── useAuth.js
│   │   └── useRequests.js
│   │
│   ├── lib/               # Utility libraries
│   │   ├── axios.js
│   │   └── validation.js
│   │
│   ├── models/            # Data models
│   │   ├── User.js
│   │   └── Request.js
│   │
│   ├── styles/            # Global styles
│   │   ├── globals.css
│   │   └── variables.css
│   │
│   └── utils/             # Helper functions
│       ├── api.js
│       └── helpers.js
│
├── public/                # Static assets
│   ├── images/
│   └── icons/
│
├── tailwind.config.mjs    # Tailwind CSS configuration
└── package.json          # Project dependencies
```

## 📦 Key Directories Explained

### 1. Components (`src/components/`)
The components directory contains all reusable UI components organized by feature and type:

#### Request Tables (`request-tables/`)
- **SuperadminHeaders.js**: Table headers for superadmin view
- **RequestDetails.jsx**: Detailed request information component
- **RequestTable.jsx**: Main request listing table

#### UI Components (`ui/`)
- **Button.jsx**: Reusable button component with variants
- **Input.jsx**: Form input component with validation
- **Modal.jsx**: Modal dialog component

### 2. Pages (`src/pages/`)
Next.js pages organized by user role:

#### Employee Portal (`employee/`)
- Dashboard with personal metrics
- Request management interface
- Team collaboration tools

#### HR Admin Portal (`hradmin/`)
- Employee management
- Request approval workflow
- HR metrics dashboard

#### Manager Portal (`manager/`)
- Team performance tracking
- Request management
- Resource allocation

#### Superadmin Portal (`superadmin/`)
- Company management
- System configuration
- Global analytics

### 3. State Management (`src/redux/`)
Redux implementation for global state management:

#### Slices
- **authSlice.js**: Authentication state
- **leadSlice.js**: Lead management state
- **requestSlice.js**: Request management state

### 4. Custom Hooks (`src/hooks/`)
Reusable React hooks for common functionality:

- **useAuth.js**: Authentication and authorization
- **useRequests.js**: Request management operations

### 5. Utility Libraries (`src/lib/`)
Core utility functions and configurations:

- **axios.js**: API client configuration
- **validation.js**: Form validation utilities

### 6. Models (`src/models/`)
Data models and type definitions:

- **User.js**: User data structure
- **Request.js**: Request data structure

### 7. Styles (`src/styles/`)
Global styling and theme configuration:

- **globals.css**: Global CSS rules
- **variables.css**: CSS custom properties

## 🛠️ Technology Stack

### Frontend
- **Framework**: Next.js
- **Styling**: Tailwind CSS
- **State Management**: Redux Toolkit
- **API Client**: Axios

### Development Tools
- **Package Manager**: npm
- **Build Tool**: Next.js
- **Code Quality**: ESLint, Prettier
- **Version Control**: Git

## 🔧 Configuration Files

### Tailwind Configuration (`tailwind.config.mjs`)
```javascript
module.exports = {
  content: [
    './src/pages/**/*.{js,ts,jsx,tsx}',
    './src/components/**/*.{js,ts,jsx,tsx}',
  ],
  theme: {
    extend: {
      colors: {
        primary: '#1a73e8',
        secondary: '#34a853',
      },
    },
  },
  plugins: [],
};
```

### Environment Configuration
```plaintext
.env.local
├── NEXT_PUBLIC_API_URL
├── NEXT_PUBLIC_AUTH_DOMAIN
└── NEXT_PUBLIC_CLIENT_ID
```

## 📝 Development Guidelines

### Component Structure
- Use functional components with hooks
- Implement proper prop types
- Follow atomic design principles
- Maintain consistent file naming

### State Management
- Use Redux for global state
- Implement proper loading states
- Handle errors consistently
- Use selectors for data access

### Code Organization
- Keep components small and focused
- Use proper file structure
- Implement proper error boundaries
- Follow consistent naming conventions

## 🚀 Getting Started

1. **Clone the repository**
   ```bash
   git clone https://github.com/medhir/medhirweb.git
   ```

2. **Install dependencies**
   ```bash
   npm install
   ```

3. **Set up environment variables**
   ```bash
   cp .env.example .env.local
   ```

4. **Start development server**
   ```bash
   npm run dev
   ```

## 📚 Additional Resources

- [Next.js Documentation](https://nextjs.org/docs)
- [Tailwind CSS Documentation](https://tailwindcss.com/docs)
- [Redux Toolkit Documentation](https://redux-toolkit.js.org/)
- [Project Wiki](https://github.com/medhir/medhirweb/wiki)
