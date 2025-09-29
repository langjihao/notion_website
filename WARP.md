# WARP.md

This file provides guidance to WARP (warp.dev) when working with code in this repository.

## Repository Overview

This is a Next.js Notion Starter Kit - a website builder that uses Notion as a CMS. It's built with Next.js 15, TypeScript, React 19, and the react-notion-x library. The site renders Notion pages as fast, SEO-friendly web pages.

## Key Architecture

### Core Data Flow
1. **Site Configuration**: `site.config.ts` defines the root Notion page ID and site metadata
2. **Page Resolution**: `lib/resolve-notion-page.ts` fetches Notion content and resolves page data
3. **Static Generation**: Next.js pages use `getStaticProps` to pre-render Notion content at build time
4. **Dynamic Routing**: `pages/[pageId].tsx` handles all Notion pages via dynamic routing

### Essential Files
- `site.config.ts` - Main configuration file defining Notion page ID, site name, domain, social links
- `lib/config.ts` - Processes site config and environment variables into usable constants
- `lib/notion.ts` - Core Notion API integration with preview images and tweet embedding
- `lib/site-config.ts` - TypeScript interfaces for configuration
- `lib/types.ts` - Core TypeScript type definitions
- `components/NotionPage.tsx` - Main component that renders Notion content

### Directory Structure
- `components/` - React components for rendering pages and UI elements
- `lib/` - Core business logic, API clients, and utilities  
- `pages/` - Next.js pages including dynamic routing and API endpoints
- `pages/api/` - API routes for search, social images, and page info
- `styles/` - CSS including Notion-specific styling overrides

## Development Commands

### Setup and Development
```bash
# Install dependencies (uses pnpm)
pnpm install

# Start development server
pnpm dev
# Opens http://localhost:3000

# Build for production
pnpm build

# Start production server
pnpm start
```

### Code Quality
```bash
# Run all tests (linting + formatting)
pnpm test

# Run ESLint
pnpm test:lint

# Check Prettier formatting
pnpm test:prettier

# Format code with Prettier
prettier '**/*.{js,jsx,ts,tsx}' --write
```

### Bundle Analysis
```bash
# Analyze bundle size
pnpm analyze

# Analyze server bundle
pnpm analyze:server  

# Analyze browser bundle
pnpm analyze:browser
```

### Deployment
```bash
# Deploy to Vercel
pnpm deploy
```

### Development with Local react-notion-x
```bash
# Link local react-notion-x packages for development
pnpm deps:link

# Unlink and restore npm packages
pnpm deps:unlink

# Upgrade Notion-related dependencies
pnpm deps:upgrade
```

## Configuration

### Essential Setup
1. Edit `site.config.ts`:
   - Set `rootNotionPageId` to your Notion page ID
   - Update `name`, `domain`, `author`
   - Configure social media handles

2. Environment variables (optional):
   - `REDIS_HOST` and `REDIS_PASSWORD` for preview image caching
   - `NEXT_PUBLIC_FATHOM_ID` for Fathom analytics
   - `NEXT_PUBLIC_POSTHOG_ID` for PostHog analytics

### Notion Page Setup
- Root Notion page must be public
- Page ID is the last part of the Notion URL (32-character string)
- Use collections on home page for organizing content
- Add `Slug` text property to database pages for custom URLs

## Testing and Quality

This project uses:
- **ESLint**: Code linting with `@fisch0920/eslint-config`
- **Prettier**: Code formatting with `@fisch0920/config/prettier`
- **TypeScript**: Strict type checking
- **No unit tests**: Project relies on linting and type checking

The test command runs linting and formatting checks but no traditional unit tests.

## Key Technical Details

### URL Generation
- **Development**: URLs include Notion ID suffix for debugging
- **Production**: Clean URLs without Notion IDs
- **Custom URLs**: Override with `Slug` property in Notion database
- **URL Overrides**: Use `pageUrlOverrides` in site config for custom paths

### Image Handling  
- Uses `next/image` for optimization
- Optional LQIP preview images (can be disabled for faster builds)
- Redis caching for preview images in production
- Supports remote images from Notion, Unsplash, Twitter

### Performance Features
- Static site generation with ISR (revalidate: 10 seconds)
- Automatic social media preview images
- Bundle splitting and optimization
- Dark mode support with CSS variables

### Navigation
- **Default**: Uses Notion's built-in navigation
- **Custom**: Define `navigationLinks` in config with specific page IDs

## Common Development Tasks

### Adding New Features
1. Components go in `components/`
2. Utilities and logic go in `lib/`
3. API routes go in `pages/api/`
4. Follow existing TypeScript patterns and imports

### Styling Changes
- Global styles in `styles/notion.css`
- Target specific Notion blocks with `.notion-block-{id}` classes
- Use CSS modules for component-specific styles

### Debugging Build Issues
```bash
# Clear Next.js cache
rm -rf .next

# Check for duplicate URL pathnames
# Error will show during build if pages have same slugified names
```

### Vercel Deployment Setup
- Disable Vercel Authentication in Project Settings > Deployment Protection
- Required for social media preview images to work
- Set environment variables in Vercel dashboard for Redis/analytics

## Package Management

This project uses **pnpm** as specified in `package.json` with `packageManager: "pnpm@10.11.1"`. Always use pnpm commands, not npm or yarn.

Key dependencies:
- `next` (15.5.3) - React framework  
- `notion-client`, `notion-types`, `notion-utils` - Notion API integration
- `react-notion-x` - React components for rendering Notion content
- `react` (19.1.1), `react-dom` (19.1.1) - React framework