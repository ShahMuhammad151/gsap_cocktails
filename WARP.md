# WARP.md

This file provides guidance to WARP (warp.dev) when working with code in this repository.

## Project Overview
This is a React + Vite project showcasing a "Mojito" themed landing page with GSAP (GreenSock Animation Platform) animations. The project uses React 19, Tailwind CSS 4, and GSAP 3 with ScrollTrigger for scroll-based animations.

## Development Commands

### Start Development Server
```powershell
npm run dev
```
Starts Vite dev server with HMR (Hot Module Replacement) on http://localhost:5173

### Build for Production
```powershell
npm run build
```
Creates optimized production build in `dist/` directory

### Preview Production Build
```powershell
npm run preview
```
Preview the production build locally before deploying

### Linting
```powershell
npm run lint
```
Runs ESLint on all JavaScript/JSX files. Note: This project uses the new flat config format (eslint.config.js)

## Architecture

### Key Technologies
- **React 19**: Latest React with StrictMode enabled
- **Vite**: Build tool and dev server
- **GSAP 3**: Animation library with ScrollTrigger and SplitText plugins
- **@gsap/react**: Official React hooks for GSAP (useGSAP)
- **Tailwind CSS 4**: Utility-first CSS with custom @theme configuration
- **react-responsive**: Responsive component utilities

### Project Structure
- `src/main.jsx`: Application entry point
- `src/App.jsx`: Root component that registers GSAP plugins (ScrollTrigger, SplitText)
- `src/components/`: React components (navbar.jsx, hero.jsx)
- `src/index.css`: Global styles with Tailwind imports and custom utilities
- `constants/index.js`: Shared constants (e.g., navLinks array)
- `public/`: Static assets organized in subdirectories:
  - `fonts/`: Custom fonts (Modern Negra)
  - `images/`: Image assets (leaves, logo, masks, noise textures)
  - `videos/`: Video files
  - `readme/`: Documentation assets

### GSAP Integration Pattern
GSAP plugins are registered once in `App.jsx`:
```javascript
gsap.registerPlugin(ScrollTrigger, SplitText);
```

Components use the `useGSAP` hook from `@gsap/react` for animation setup. Example from navbar.jsx:
```javascript
useGSAP(() => {
  const navTween = gsap.timeline({
    scrollTrigger: {
      trigger: "nav",
      start: "bottom top",
    },
  });
  // Animation code...
});
```

### Styling Architecture
- Tailwind CSS 4 is integrated via Vite plugin (`@tailwindcss/vite`)
- Custom theme variables defined in `@theme` block in index.css:
  - Colors: `--color-yellow`, `--color-white-100`
  - Fonts: `--font-sans`, `--font-modern-negra`, `--font-serif`
- Custom utilities defined with `@utility` directive:
  - `flex-center`: Centers content with flexbox
  - `col-center`: Centers column content
  - `abs-center`: Absolute positioning centered
  - `text-gradient`: Text gradient effect
  - `radial-gradient`: Radial background gradient
  - `masked-img`: Image masking utility
- Component-specific styles in `@layer components` block
- Nested CSS is supported via postcss-nesting

### ESLint Configuration
- Uses new flat config format (eslint.config.js)
- Ignores `dist/` directory
- Custom rule: Allows unused variables that start with uppercase or underscore
- Configured for React hooks and React Refresh (Vite HMR)

## Important Conventions

### File Naming
- React components use lowercase filenames (hero.jsx, navbar.jsx)
- Constants file uses lowercase (constants/index.js)

### Import Paths
- Components import from relative paths: `"./components/navbar"`
- Constants imported from `"../../constants"` (relative to component location)
- Static assets referenced from public directory root: `"/images/hero-left-leaf.png"`

### Component Export Pattern
All components use default exports:
```javascript
const ComponentName = () => { /* ... */ };
export default ComponentName;
```

### GSAP Animation Cleanup
When using `useGSAP`, the hook automatically handles cleanup of GSAP animations and ScrollTriggers on unmount. No manual cleanup needed.

## Working with Animations
- All GSAP animations should use the `useGSAP` hook from `@gsap/react`
- ScrollTrigger animations should be wrapped in timelines for better control
- Plugins (ScrollTrigger, SplitText) must be registered before use - this is done in App.jsx

## Asset Management
- Fonts: Place in `public/fonts/` and reference via `@font-face` in index.css
- Images: Organize in `public/images/` by purpose
- Videos: Place in `public/videos/`
- All public assets are referenced with absolute paths starting with `/`
