# ORAGENAI Dynamic Frontend 🚀

A modern React TypeScript web template with **automatic site discovery** that dynamically detects new subdirectories and displays them as interactive cards. Perfect for showcasing multiple projects, portfolios, or applications in a unified interface.

![Built with React](https://img.shields.io/badge/React-18+-61DAFB?style=flat&logo=react)
![TypeScript](https://img.shields.io/badge/TypeScript-5+-3178C6?style=flat&logo=typescript)
![Vite](https://img.shields.io/badge/Vite-6+-646CFF?style=flat&logo=vite)
![Tailwind CSS](https://img.shields.io/badge/Tailwind-4+-06B6D4?style=flat&logo=tailwindcss)

## ✨ Key Features

- **🔍 Automatic Site Discovery** - Automatically detects subdirectories with `index.html` files
- **🎨 Modern Dark Theme** - Beautiful gradient UI with custom accent colors
- **📱 Responsive Design** - Works perfectly on desktop, tablet, and mobile
- **⚡ Lightning Fast** - Built with Vite for optimal performance
- **🏷️ Smart Metadata Extraction** - Automatically extracts titles, descriptions, and tags from HTML
- **🔗 Direct Navigation** - Click cards to open sites in new tabs
- **♿ Accessible** - Full keyboard navigation and screen reader support
- **🛠️ TypeScript Ready** - Full type safety and IntelliSense support

## 🚀 Quick Start

### Prerequisites
- Node.js 18+ 
- npm or yarn

### Installation

```bash
# Clone the repository
git clone <repository-url>
cd oragenai-website

# Install dependencies
npm install

# Start development server
npm run dev
```

The site will be available at `http://localhost:3000` (or the next available port).

## 📁 Adding New Sites

The system automatically discovers any subdirectory in `public/sites/` that contains an `index.html` file.

### Step 1: Create Your Site Directory

```bash
mkdir public/sites/my-awesome-project
```

### Step 2: Add Your Site Files

Create `public/sites/my-awesome-project/index.html`:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>My Awesome Project</title>
    
    <!-- These meta tags enhance auto-discovery -->
    <meta name="description" content="An amazing web application that does incredible things">
    <meta name="keywords" content="React, JavaScript, Web App, Portfolio">
    <meta name="category" content="development">
    
    <!-- Open Graph tags for better card display -->
    <meta property="og:title" content="My Awesome Project">
    <meta property="og:description" content="An amazing web application">
    <meta property="og:image" content="./thumbnail.jpg">
</head>
<body>
    <h1>Welcome to My Awesome Project!</h1>
    <!-- Your site content here -->
</body>
</html>
```

### Step 3: Add a Thumbnail (Optional)

Add `public/sites/my-awesome-project/thumbnail.jpg` for a custom preview image.

### Step 4: Rebuild the Manifest

```bash
npm run build
```

Your new site will automatically appear as a card on the main page! 🎉

## 🎨 Customizing Site Metadata

The system extracts metadata from your HTML files to create rich card displays:

### Supported Meta Tags

| Tag | Purpose | Example |
|-----|---------|---------|
| `<title>` | Card title | `<title>My Project</title>` |
| `meta[name="description"]` | Card description | `<meta name="description" content="...">` |
| `meta[name="keywords"]` | Tags/badges | `<meta name="keywords" content="React,TypeScript">` |
| `meta[name="category"]` | Category icon | `<meta name="category" content="development">` |
| `meta[property="og:title"]` | Override title | `<meta property="og:title" content="...">` |
| `meta[property="og:description"]` | Override description | `<meta property="og:description" content="...">` |
| `meta[property="og:image"]` | Custom thumbnail | `<meta property="og:image" content="./thumb.jpg">` |

### Category Icons

The system automatically displays icons based on the category:

- `development` or `code` → Code icon
- `design` → Desktop/design icon  
- Default → Globe icon

## 🔧 Development

### Project Structure

```
oragenai-website/
├── public/
│   ├── sites/                     # Auto-discovered site directories
│   │   ├── portfolio-showcase/
│   │   ├── sample-react-app/
│   │   └── vector-3-tut/
│   └── sites-manifest.json        # Generated manifest file
├── src/
│   ├── components/
│   │   ├── SiteCard.tsx           # Individual site card component
│   │   ├── SiteGrid.tsx           # Grid layout component
│   │   ├── ErrorMessage.tsx       # Error handling component
│   │   └── LoadingSpinner.tsx     # Loading state component
│   ├── hooks/
│   │   └── useSiteDiscovery.ts    # Site discovery hook
│   ├── types/
│   │   └── index.ts               # TypeScript interfaces
│   └── utils/
│       └── icons.ts               # FontAwesome icon setup
├── scripts/
│   └── generate-manifest.js       # Manifest generation script
└── dist/                          # Built application
```

### Available Scripts

```bash
# Development
npm run dev          # Start development server
npm run build        # Build for production
npm run preview      # Preview production build

# Testing
npm run test         # Run unit tests
npm run test:ui      # Run tests with UI
npm run e2e          # Run end-to-end tests

# Code Quality
npm run lint         # Lint code
npm run lint:fix     # Fix linting issues
npm run type-check   # Type checking
```

## 🏗️ Architecture

### Auto-Discovery System

The core innovation is the **manifest-based discovery system**:

1. **Build-time Generation**: `scripts/generate-manifest.js` scans `public/sites/` during build
2. **Metadata Extraction**: Parses HTML files to extract titles, descriptions, tags
3. **Manifest Creation**: Generates `sites-manifest.json` with all site data
4. **Runtime Loading**: React app fetches manifest and displays cards

### Key Components

#### `useSiteDiscovery` Hook

```typescript
const { sites, loading, error, refetch } = useSiteDiscovery()
```

Fetches and manages the sites manifest with error handling and retry logic.

#### `SiteCard` Component

```typescript
<SiteCard 
  site={siteMetadata} 
  onClick={(site) => handleSiteClick(site)}
  className="custom-styles"
/>
```

Displays individual site information with thumbnail, title, description, and tags.

#### `SiteGrid` Component

```typescript
<SiteGrid 
  sites={sites}
  loading={loading}
  error={error}
  onRetry={refetch}
/>
```

Handles grid layout, loading states, and error handling.

### TypeScript Interfaces

```typescript
interface SiteMetadata {
  path: string           // Directory name
  title: string          // Site title
  description: string    // Site description
  thumbnail?: string     // Thumbnail URL
  lastModified: string   // ISO timestamp
  tags?: string[]        // Array of tags
  category?: string      // Site category
}

interface SitesManifest {
  sites: SiteMetadata[]
  lastUpdated: string
  totalSites: number
}
```

## 🎨 Customization

### Theme Customization

The dark theme uses custom Tailwind colors defined in `tailwind.config.js`:

```javascript
theme: {
  extend: {
    colors: {
      'dark-bg': '#0f172a',        // Main background
      'dark-surface': '#1e293b',   // Card/surface background
      'dark-border': '#334155',    // Border color
      'accent-blue': '#3b82f6',    // Primary accent
      'accent-purple': '#8b5cf6',  // Secondary accent
      'accent-green': '#10b981'    // Success accent
    }
  }
}
```

### Adding Custom Components

1. Create component in `src/components/`
2. Export from component index file
3. Import in parent components
4. Add TypeScript interfaces in `src/types/`

### Extending the Manifest Generator

Modify `scripts/generate-manifest.js` to extract additional metadata:

```javascript
const extractCustomField = (doc) => {
  return doc.querySelector('meta[name="custom-field"]')?.getAttribute('content') || null
}

// Add to metadata extraction
const metadata = {
  // ... existing fields
  customField: extractCustomField(doc)
}
```

## 🚀 Deployment

### Build for Production

```bash
npm run build
```

This creates:
1. Optimized React bundle in `dist/`
2. Generated `sites-manifest.json`
3. All static assets

### Deployment Platforms

#### Netlify

```toml
# netlify.toml
[build]
  command = "npm run build"
  publish = "dist"

[[headers]]
  for = "/sites-manifest.json"
  [headers.values]
    Cache-Control = "public, max-age=3600"
```

#### Vercel

```json
{
  "buildCommand": "npm run build",
  "outputDirectory": "dist",
  "installCommand": "npm install"
}
```

#### GitHub Pages

```yaml
# .github/workflows/deploy.yml
name: Deploy to GitHub Pages
on:
  push:
    branches: [main]
jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Setup Node
        uses: actions/setup-node@v4
        with:
          node-version: '18'
      - run: npm ci
      - run: npm run build
      - name: Deploy
        uses: peaceiris/actions-gh-pages@v3
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./dist
```

## 🔧 Troubleshooting

### Common Issues

#### Sites Not Appearing

1. **Check directory structure**: Ensure `public/sites/your-site/index.html` exists
2. **Rebuild manifest**: Run `npm run build` to regenerate manifest
3. **Check console**: Look for errors in browser developer tools

#### Broken Links

1. **Verify file paths**: Ensure `index.html` exists in site directory
2. **Check Vite config**: Static file serving may need configuration
3. **Test manually**: Try accessing `/sites/your-site/index.html` directly

#### Development Server Issues

```bash
# Clear node modules and reinstall
rm -rf node_modules package-lock.json
npm install

# Clear Vite cache
rm -rf .vite
npm run dev
```

#### Build Errors

```bash
# Type checking
npm run type-check

# Linting
npm run lint:fix

# Test build locally
npm run build && npm run preview
```

### Performance Optimization

#### Large Number of Sites

For 50+ sites, consider:

1. **Pagination**: Implement virtual scrolling or pagination
2. **Lazy loading**: Load thumbnails on scroll
3. **Search/filtering**: Add search functionality
4. **Caching**: Implement manifest caching

#### Bundle Size

Monitor bundle size with:

```bash
npm run build:analyze
```

## 📝 Contributing

1. Fork the repository
2. Create a feature branch: `git checkout -b feature/amazing-feature`
3. Commit changes: `git commit -m 'Add amazing feature'`
4. Push to branch: `git push origin feature/amazing-feature`
5. Open a Pull Request

### Development Guidelines

- Follow TypeScript strict mode
- Write tests for new components
- Update documentation for new features
- Use semantic commit messages
- Ensure accessibility compliance

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🙏 Acknowledgments

- Built with [Vite](https://vitejs.dev/) for lightning-fast development
- Styled with [Tailwind CSS](https://tailwindcss.com/) for rapid UI development
- Icons by [FontAwesome](https://fontawesome.com/)
- Powered by [React](https://reactjs.org/) and [TypeScript](https://www.typescriptlang.org/)

---

**Ready to showcase your projects dynamically?** Just drop them in `public/sites/` and watch them appear automatically! ✨
