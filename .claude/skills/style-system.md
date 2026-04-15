---
name: Style System
description: Implement consistent styling with design tokens and responsive layouts
agents: [frontend-dev]
---

# Style System Skill

Create consistent, maintainable styling with design tokens and responsive design.

## Process

1. **Define tokens**: Colors, spacing, typography
2. **Create base styles**: Reset, typography, layout
3. **Build component styles**: Consistent with tokens
4. **Add responsive rules**: Mobile-first approach
5. **Document**: Style guide with examples

## Design Tokens

### Color Palette
```css
:root {
  /* Primary */
  --color-primary-50: #eff6ff;
  --color-primary-500: #3b82f6;
  --color-primary-900: #1e3a8a;
  
  /* Neutral */
  --color-gray-50: #f9fafb;
  --color-gray-500: #6b7280;
  --color-gray-900: #111827;
  
  /* Semantic */
  --color-success: #10b981;
  --color-warning: #f59e0b;
  --color-error: #ef4444;
}
```

### Spacing Scale
```css
:root {
  --space-1: 0.25rem;  /* 4px */
  --space-2: 0.5rem;   /* 8px */
  --space-3: 0.75rem;  /* 12px */
  --space-4: 1rem;     /* 16px */
  --space-6: 1.5rem;   /* 24px */
  --space-8: 2rem;     /* 32px */
}
```

### Typography
```css
:root {
  --font-sans: system-ui, -apple-system, sans-serif;
  --font-mono: ui-monospace, monospace;
  
  --text-xs: 0.75rem;
  --text-sm: 0.875rem;
  --text-base: 1rem;
  --text-lg: 1.125rem;
  --text-xl: 1.25rem;
}
```

## Responsive Breakpoints

```css
/* Mobile first */
.container { padding: var(--space-4); }

/* Tablet (640px+) */
@media (min-width: 40rem) {
  .container { padding: var(--space-6); }
}

/* Desktop (1024px+) */
@media (min-width: 64rem) {
  .container { padding: var(--space-8); }
}
```

## Style Checklist

- [ ] Uses design tokens (no magic numbers)
- [ ] Mobile-first responsive rules
- [ ] Consistent spacing rhythm
- [ ] Typography hierarchy clear
- [ ] Dark mode support (if required)
- [ ] Print styles (if applicable)
