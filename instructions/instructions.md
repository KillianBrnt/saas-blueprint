# Project Instructions

This document serves as the living rulebook for the SaaS Blueprint project. All contributors must adhere to these guidelines.

## 1. Coding Standards

### 1.1 Comments
- **Placement**: Comments must always be placed **above** the function or logic they describe.
- **Prohibition**: Do not place comments inside the function body unless absolutely necessary for complex inline logic.

**Correct:**
```typescript
/**
 * Calculates the total price including tax.
 * @param price - Raw price
 * @param taxRate - Tax percentage
 */
function calculateTotal(price: number, taxRate: number): number {
  return price * (1 + taxRate);
}
```

**Incorrect:**
```typescript
function calculateTotal(price: number, taxRate: number): number {
  // Calculates the total price (Avoid this!)
  return price * (1 + taxRate);
}
```
