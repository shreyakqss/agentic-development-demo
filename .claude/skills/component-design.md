---
name: Component Design
description: Design and implement reusable UI components with proper props and accessibility
agents: [frontend-dev]
---

# Component Design Skill

Design reusable, accessible UI components following best practices.

## Process

1. **Define purpose**: What does this component do?
2. **Design API**: What props does it accept?
3. **Plan variants**: What variations are needed?
4. **Consider accessibility**: How will all users interact?
5. **Implement**: Build with tests
6. **Document**: Usage examples and prop descriptions

## Component API Design

### Props Guidelines
- Required props first, optional with defaults
- Use descriptive names (not abbreviations)
- Prefer primitive types when possible
- Document each prop's purpose

### Example Component Structure
```typescript
interface ButtonProps {
  /** Button text content */
  children: React.ReactNode;
  /** Visual style variant */
  variant?: 'primary' | 'secondary' | 'danger';
  /** Size of the button */
  size?: 'sm' | 'md' | 'lg';
  /** Disabled state */
  disabled?: boolean;
  /** Click handler */
  onClick?: () => void;
}

const Button: React.FC<ButtonProps> = ({
  children,
  variant = 'primary',
  size = 'md',
  disabled = false,
  onClick,
}) => {
  // Implementation
};
```

## Accessibility Checklist

- [ ] Uses semantic HTML (`button`, `nav`, `main`, etc.)
- [ ] Has accessible name (visible text or aria-label)
- [ ] Keyboard navigable (Tab, Enter, Escape)
- [ ] Focus states visible
- [ ] Color contrast meets WCAG AA (4.5:1 for text)
- [ ] Works with screen readers

## Component Checklist

- [ ] Single responsibility
- [ ] Props are typed and documented
- [ ] Default props for optional values
- [ ] Handles loading/error/empty states
- [ ] Responsive across breakpoints
- [ ] Has unit tests
