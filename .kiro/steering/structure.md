# Project Structure

```
src/
├── main.js              # App entry point, Vue/Bootstrap setup
├── App.vue              # Root component with global styles
├── assets/              # Static images (fallback defaults)
├── components/          # Reusable Vue components
│   ├── AboutMe.vue
│   ├── GreetingHeader.vue
│   ├── JmkAward.vue
│   └── NavBar.vue
├── contentManagement/
│   └── content.json     # CMS-managed content data
├── router/
│   └── index.js         # Vue Router config (single route)
└── views/
    └── HomeView.vue     # Main page (single-page site)

public/
├── admin/
│   ├── config.yml       # NetlifyCMS configuration
│   ├── imageUploads/    # CMS uploaded media
│   └── index.html       # CMS admin interface
└── index.html           # App HTML template
```

## Architecture Notes
- Single-page portfolio site with one view (`HomeView.vue`)
- Content loaded from JSON file managed by NetlifyCMS
- Components receive content via props from HomeView
- Images referenced in content.json use `/public/admin/imageUploads/` paths
- `imageSrc()` helper strips `/public` prefix for runtime paths

## Component Patterns
- Options API style (data, computed, methods)
- Props for data flow, events for child-to-parent communication
- Scoped styles per component
- BootstrapVue components (`b-*`) for UI elements
