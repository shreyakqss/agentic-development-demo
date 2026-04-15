---
name: Database Schema Design
description: Design database schemas with proper normalization and constraints
agents: [backend-dev]
---

# Database Schema Design Skill

Design database schemas that are normalized, performant, and maintainable.

## Process

1. **Identify entities**: What are the core objects?
2. **Define relationships**: How do entities relate?
3. **Normalize**: Eliminate redundancy (aim for 3NF)
4. **Add constraints**: Enforce data integrity
5. **Plan indexes**: Optimize for query patterns
6. **Write migration**: Create reversible migration script

## Schema Design Principles

### Normalization Levels
- **1NF**: Atomic values, no repeating groups
- **2NF**: No partial dependencies on composite keys
- **3NF**: No transitive dependencies

### Common Patterns

**One-to-Many**
```sql
-- Parent table
CREATE TABLE authors (
  id SERIAL PRIMARY KEY,
  name VARCHAR(255) NOT NULL
);

-- Child table with foreign key
CREATE TABLE books (
  id SERIAL PRIMARY KEY,
  author_id INTEGER REFERENCES authors(id),
  title VARCHAR(255) NOT NULL
);
```

**Many-to-Many**
```sql
-- Junction table
CREATE TABLE book_categories (
  book_id INTEGER REFERENCES books(id),
  category_id INTEGER REFERENCES categories(id),
  PRIMARY KEY (book_id, category_id)
);
```

## Index Strategy

- Primary keys (automatic)
- Foreign keys used in JOINs
- Columns in WHERE clauses
- Columns in ORDER BY (if frequently sorted)

## Migration Checklist

- [ ] Migration is reversible (has down/rollback)
- [ ] No data loss in down migration
- [ ] Indexes added for query patterns
- [ ] Constraints enforce business rules
- [ ] Default values specified where appropriate
- [ ] NOT NULL on required fields
