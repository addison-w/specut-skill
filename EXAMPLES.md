# Specut Examples

## Example 1: E-commerce checkout feature

### Clarify phase output

- **Feature**: Checkout flow for an e-commerce app — cart review, payment, order confirmation
- **Spike mode**: full
- **Core user paths**: Review cart → Submit payment → Receive order confirmation
- **Tech stack** (auto-detected): TypeScript, Express, PostgreSQL, Jest

### Spike

In worktree: Wire up Express routes, service stubs, DB migration placeholders. All endpoints return fake data. App boots and routes are reachable.

### Output: specut-checkout.md

```markdown
# Checkout Feature

## SCAFFOLD

### Intent

Enable customers to review their cart, pay, and receive order confirmation.

### Core user paths

**Path 1: Review cart**
- Layers: DB → CartService → API → UI
- Interface direction: CartService exposes cart retrieval by user ID
- Key stubs: CartService.getCart(userId) → returns fake cart with 2 items

**Path 2: Submit payment**
- Layers: DB → PaymentService → OrderService → API → UI
- Interface direction: PaymentService.processPayment(cart) → returns fake success, OrderService.createOrder(userId, paymentResult) → returns fake order
- Key stubs: PaymentService.processPayment(), OrderService.createOrder()

**Path 3: Order confirmation**
- Layers: DB → OrderService → API → UI
- Interface direction: OrderService.getOrder(orderId) → returns fake order with status confirmed
- Key stubs: OrderService.getOrder()

### Acceptance criteria

- [ ] All endpoints compile and return fake data
- [ ] Cart, payment, and order flows each have a reachable API endpoint
- [ ] No real business logic — stubs and fake data only
- [ ] Interface direction is documented but not locked — fill cards may adjust

### Notes

- Spike validated: Express routing works with the project's middleware chain
- Payment gateway SDK requires async initialization — stub includes init placeholder
- CartService and OrderService both read from `orders` table — fill cards extending this table may conflict

---

## FILL 1: Add email+password login method

### Intent

Enable customers to authenticate before checkout.

### User path

Prerequisite for Path 2 (Submit payment)

### Acceptance criteria

- [ ] POST /auth/login accepts email + password, returns JWT
- [ ] Invalid credentials return 401 with error message
- [ ] JWT middleware protects checkout endpoints
- [ ] Tests pass independently

### Blocked by

- SCAFFOLD Checkout Feature (hard dependency)

### ⚠️ Potential conflicts

- FILL 2: both touch auth middleware
- FILL 4: both add to users table schema

---

## FILL 2: Add Stripe payment processing

### Intent

Process real payments through Stripe.

### User path

Path 2: Submit payment

### Acceptance criteria

- [ ] PaymentService.processPayment() calls Stripe API
- [ ] Success creates a payment record in DB
- [ ] Failure returns appropriate error codes (card declined, network error)
- [ ] Webhook endpoint receives Stripe events
- [ ] Tests pass independently using Stripe test mode

### Blocked by

- SCAFFOLD Checkout Feature (hard dependency)

### ⚠️ Potential conflicts

- FILL 1: both touch auth middleware

---

## FILL 3: Add cart retrieval API

### Intent

Customers can view their cart with item details and totals.

### User path

Path 1: Review cart

### Acceptance criteria

- [ ] GET /cart/:userId returns cart with items, quantities, prices, and total
- [ ] Empty cart returns 200 with empty items array
- [ ] Cart data read from database (not fake data)
- [ ] Tests pass independently

### Blocked by

- SCAFFOLD Checkout Feature (hard dependency)

### ⚠️ Potential conflicts

- None identified

---

## FILL 4: Add order creation and confirmation

### Intent

After payment, create an order and let customers view it.

### User path

Path 2: Submit payment → Path 3: Order confirmation

### Acceptance criteria

- [ ] OrderService.createOrder() persists order with payment reference
- [ ] GET /orders/:orderId returns full order with status and items
- [ ] Order status transitions: pending → confirmed
- [ ] Tests pass independently

### Blocked by

- SCAFFOLD Checkout Feature (hard dependency)

### ⚠️ Potential conflicts

- FILL 1: both add to users table schema
```

---

## Example 2: Light spike mode

For a smaller feature like "add dark mode toggle", light spike might:
- Verify the CSS variable system exists and can be extended
- Confirm the toggle component library supports theme switching

No runnable skeleton, just validation of key assumptions.

## Example 3: No spike mode

For a feature where the tech lead already has clear architecture (e.g., "add another CRUD endpoint following the existing pattern"), skip spike entirely. Cut cards based on reasoning about the existing codebase patterns.