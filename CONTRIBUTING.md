# Contributing to Myanmar Fire Hotspots Dashboard

Thank you for your interest in contributing! This project aims to improve fire monitoring and awareness in Myanmar and Southeast Asia through open-source geospatial visualization.

## How to Contribute

### Reporting Issues

If you find a bug or have a feature request, please open an issue on GitHub with:

- A clear and descriptive title
- Steps to reproduce the problem (for bugs)
- Expected vs actual behavior
- Browser and OS information
- Screenshots if applicable

### Submitting Changes

1. **Fork** the repository
2. **Create a branch** for your feature or fix:
   ```bash
   git checkout -b feature/add-geojson-boundaries
   ```
3. **Make your changes** and test thoroughly in multiple browsers
4. **Commit** with a clear message:
   ```bash
   git commit -m "Add Myanmar state/region GeoJSON boundaries for accurate classification"
   ```
5. **Push** to your fork and open a **Pull Request**

### Development Guidelines

- The application is a single `index.html` file — keep it self-contained unless there is a strong reason to split
- External dependencies should be loaded via CDN (no build step required)
- Test on both desktop and mobile browsers
- Maintain the dark theme aesthetic and consistent UI language
- Add comments for complex logic, especially coordinate calculations and data processing
- Ensure new features degrade gracefully on slower connections

### Priority Contribution Areas

| Area | Description | Difficulty |
|------|-------------|------------|
| Live FIRMS API | Replace synthetic data with real-time NASA FIRMS feed | Medium |
| GeoJSON Boundaries | Add Myanmar state/region polygons for proper spatial classification | Medium |
| Time Animation | Animate fire progression across multiple days | Hard |
| Multi-Country | Extend to Thailand, Laos, Cambodia with ASEAN view | Medium |
| Accessibility | ARIA labels, keyboard navigation, screen reader support | Medium |
| Performance | WebGL rendering for 10,000+ points | Hard |
| Data Export | CSV/GeoJSON download of visible hotspots | Easy |
| Bookmark/Share | URL parameters for sharing specific map views | Easy |

### Code Style

- Use `const` and `let` (no `var`)
- Use template literals for string interpolation
- Follow the existing naming conventions (camelCase for variables, kebab-case for CSS classes)
- Keep functions focused and under 50 lines where possible

## Code of Conduct

Be respectful, constructive, and inclusive. This project serves a humanitarian purpose — contributions should reflect that spirit.

## Questions?

Open a discussion on GitHub or reach out through the repository's issue tracker.
