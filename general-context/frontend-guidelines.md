# Frontend Development Guidelines

Best practices for frontend development in this project.

## Component Architecture

### Component Types

| Type | Purpose | State | Examples |
|------|---------|-------|----------|
| Page | Route entry point | Yes | HomePage, SettingsPage |
| Container | Data fetching, logic | Yes | UserListContainer |
| Presentational | Pure UI rendering | Props only | Button, Card |
| Layout | Page structure | No | Header, Sidebar |

### Component Structure

```typescript
// components/Button/Button.tsx
interface ButtonProps {
  children: React.ReactNode;
  variant?: 'primary' | 'secondary';
  disabled?: boolean;
  onClick?: () => void;
}

export function Button({ 
  children, 
  variant = 'primary',
  disabled = false,
  onClick 
}: ButtonProps) {
  return (
    <button 
      className={styles[variant]}
      disabled={disabled}
      onClick={onClick}
    >
      {children}
    </button>
  );
}
```

### File Organization

```
components/
  Button/
    Button.tsx       # Component
    Button.test.tsx  # Tests
    Button.module.css # Styles
    index.ts         # Export
```

## State Management

### State Location Rules

1. **Local state**: UI state, form inputs (useState)
2. **Lifted state**: Shared between siblings (parent component)
3. **Context**: Theme, auth, app-wide settings
4. **Server state**: API data (React Query, SWR)

### State Patterns

```typescript
// Local UI state
const [isOpen, setIsOpen] = useState(false);

// Server state with React Query
const { data, isLoading, error } = useQuery({
  queryKey: ['users'],
  queryFn: fetchUsers
});

// Always handle all states
if (isLoading) return <Spinner />;
if (error) return <ErrorMessage error={error} />;
if (!data?.length) return <EmptyState />;
return <UserList users={data} />;
```

## Styling

### Design Tokens

Use CSS custom properties for consistency:

```css
:root {
  /* Colors */
  --color-primary: #3b82f6;
  --color-error: #ef4444;
  
  /* Spacing */
  --space-sm: 0.5rem;
  --space-md: 1rem;
  --space-lg: 1.5rem;
  
  /* Typography */
  --font-size-sm: 0.875rem;
  --font-size-base: 1rem;
}
```

### Responsive Design

Mobile-first approach:

```css
/* Base: mobile */
.container {
  padding: var(--space-sm);
}

/* Tablet: 640px+ */
@media (min-width: 40rem) {
  .container {
    padding: var(--space-md);
  }
}

/* Desktop: 1024px+ */
@media (min-width: 64rem) {
  .container {
    padding: var(--space-lg);
  }
}
```

## Accessibility

### Required Practices

1. **Semantic HTML**: Use correct elements (`button`, `nav`, `main`)
2. **Keyboard Navigation**: All interactive elements reachable via Tab
3. **Focus Management**: Visible focus indicators
4. **ARIA Labels**: When semantic HTML isn't enough
5. **Color Contrast**: Minimum 4.5:1 for text

### Accessibility Patterns

```tsx
// Good: Semantic HTML
<button onClick={handleSubmit}>Submit</button>

// Bad: Div as button (missing keyboard support)
<div onClick={handleSubmit}>Submit</div>

// Icon button needs aria-label
<button aria-label="Close dialog" onClick={onClose}>
  <CloseIcon />
</button>

// Form inputs need labels
<label htmlFor="email">Email</label>
<input id="email" type="email" />
```

### Accessibility Checklist

- [ ] Can navigate with keyboard only
- [ ] Focus order is logical
- [ ] Focus indicators visible
- [ ] Images have alt text
- [ ] Form inputs have labels
- [ ] Color is not only indicator
- [ ] Works at 200% zoom

## Forms

### Form Handling

```typescript
// Controlled form with validation
function LoginForm() {
  const [email, setEmail] = useState('');
  const [error, setError] = useState('');

  const handleSubmit = (e: FormEvent) => {
    e.preventDefault();
    
    if (!email.includes('@')) {
      setError('Valid email required');
      return;
    }
    
    // Submit logic
  };

  return (
    <form onSubmit={handleSubmit}>
      <label htmlFor="email">Email</label>
      <input
        id="email"
        type="email"
        value={email}
        onChange={(e) => setEmail(e.target.value)}
        aria-describedby={error ? 'email-error' : undefined}
      />
      {error && <span id="email-error" role="alert">{error}</span>}
      <button type="submit">Login</button>
    </form>
  );
}
```

## Error Handling

### Error Boundaries

```typescript
// Wrap major sections in error boundaries
<ErrorBoundary fallback={<ErrorFallback />}>
  <UserDashboard />
</ErrorBoundary>
```

### API Error Display

```typescript
function ErrorMessage({ error }: { error: Error }) {
  return (
    <div role="alert" className={styles.error}>
      <strong>Something went wrong</strong>
      <p>{error.message}</p>
      <button onClick={() => window.location.reload()}>
        Try again
      </button>
    </div>
  );
}
```

## Testing

### Component Tests

```typescript
import { render, screen } from '@testing-library/react';
import userEvent from '@testing-library/user-event';

describe('Button', () => {
  it('calls onClick when clicked', async () => {
    const onClick = vi.fn();
    render(<Button onClick={onClick}>Click me</Button>);
    
    await userEvent.click(screen.getByRole('button'));
    
    expect(onClick).toHaveBeenCalledTimes(1);
  });

  it('is disabled when disabled prop is true', () => {
    render(<Button disabled>Click me</Button>);
    
    expect(screen.getByRole('button')).toBeDisabled();
  });
});
```

### What to Test

- User interactions (clicks, typing)
- Conditional rendering
- Accessibility (roles, labels)
- Error states
- Loading states
