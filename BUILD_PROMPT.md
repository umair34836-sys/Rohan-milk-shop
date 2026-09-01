# Rohan Milk Shop — BUILD_PROMPT

You are the implementation engineer for Rohan Milk Shop.

## Mandatory first actions
1. Read `PROJECT_ARCHITECTURE.md` completely.
2. Read the uploaded UI/UX Pro Max source package, especially:
   - `.claude/skills/ui-ux-pro-max/SKILL.md`
   - `.claude/skills/design/SKILL.md`
   - `.claude/skills/design-system/SKILL.md`
   - `.claude/skills/design-system/references/token-architecture.md`
   - `.claude/skills/design-system/references/component-specs.md`
   - `.claude/skills/brand/SKILL.md`
   - `.claude/skills/brand/references/logo-usage-rules.md`
3. Read `design-system/rohan-milk-shop/MASTER.md`.
4. Treat those sources as design/UX authority. Do not silently replace their terminology or rules with unrelated design conventions.
5. Use the supplied Rohan logo and hero/banner assets in `assets/`.

## Implementation requirements
Build the complete Rohan Milk Shop website described in `PROJECT_ARCHITECTURE.md`.

Use a lightweight static architecture unless the existing project proves another stack is required. Preserve GitHub Pages compatibility.

### Required pages
- index.html
- shop.html
- product.html
- cart.html
- checkout.html
- order-success.html
- auth.html
- account.html
- admin/index.html

### Required modules
Create reusable Firebase/application logic in `js/`.
Use Firebase Authentication and Firestore only.
Do NOT use Firebase Storage.
Do NOT use Cloud Functions.
Do NOT introduce a server backend.

### Firebase
Use the provided Firebase project configuration in `firebase/config.js`.
Do not invent another project.
Do not expose or create service-account credentials in frontend files.

### Authentication
Support:
- email/password signup
- email/password login
- guest checkout
- customer order history

Admin authorization must depend on Firestore `admins/{uid}` and Firestore rules. Never rely on a hidden frontend route as the security mechanism.

### Catalog
Products:
- Fresh Milk
- Dahi/Yogurt
- Lassi
- Butter
- Cheese
- Cream
- Eggs

Support:
- category
- price
- stock
- unit
- custom quantity
- minimum quantity
- quantity step
- optional image URL
- description

### Orders
Pickup only.
Delivery cost = 0.
Statuses:
- pending
- confirmed
- ready_for_pickup
- picked_up
- cancelled

### Admin
Implement:
- dashboard
- products CRUD
- stock
- orders and status updates
- customers
- coupons
- banners
- store settings
- payment method settings

Keep one admin role for now.

### Payments
Keep multiple payment methods configurable:
- Cash on Pickup
- Bank Transfer
- Easypaisa
- JazzCash

Do not pretend a gateway is verified if no gateway integration/account exists.

## UI/UX requirements
Follow the persisted Rohan Milk Shop design system:
- Vibrant & Block-based style
- Feature-Rich Showcase pattern
- Rubik + Nunito Sans
- green/natural brand palette with orange CTA
- strong visual hierarchy
- cards with depth
- minimum 44px touch targets
- visible focus
- keyboard-accessible controls
- reduced-motion support
- no emoji used as interface icons
- use consistent SVG icons
- no horizontal mobile overflow
- mobile-first responsive behavior
- transitions around 150–300ms where useful
- avoid layout-shifting hover effects

Do not make the UI text-heavy.

## Brand rules
Use the provided logo without:
- stretching
- rotating
- recoloring
- adding effects
- cropping
- rearranging

Maintain clear space around it and keep it legible.

## Product images
Because Firebase Storage is prohibited, use:
- supplied local brand assets where appropriate
- admin-entered external image URLs for product images
- graceful SVG/CSS fallback artwork when an image is absent

## Data and security
Implement Firestore security rules from the architecture.
Never claim client-side hiding is security.
Validate user inputs.
Do not trust client-calculated totals for privileged financial logic; keep the data model explicit and make the limitation clear because there is no Cloud Function/backend.

## Error handling
Every important async operation needs:
- loading feedback
- error feedback
- empty state
- disabled state during submission
- recovery/retry where useful

Checkout must never leave the user stuck on “Placing order…” after a successful Firestore write. On success:
1. save a local confirmation snapshot
2. clear cart
3. push purchase event
4. redirect to `order-success.html`

## Analytics
Preserve the existing dataLayer event names and payload intent:
- add_to_cart
- view_cart
- begin_checkout
- purchase
- sign_up
- login

Do not add external analytics accounts without configuration.

## SEO
Implement:
- semantic HTML
- titles
- meta descriptions
- canonical tags where appropriate
- Open Graph tags
- accessible alt text
- descriptive links
- indexable public catalog pages where static hosting permits

## Testing checklist
Before declaring complete, test:
1. Home loads.
2. Shop loads.
3. Search/filter/sort work.
4. Product detail loads.
5. Custom quantity works.
6. Cart add/update/remove works.
7. Guest checkout works.
8. Order is written to Firestore.
9. Success redirect works.
10. Customer signup/login works.
11. Customer order history works.
12. Unauthorized user cannot access admin data.
13. Admin can manage products.
14. Admin can update order status.
15. Admin can manage coupons/banners/settings.
16. Mobile widths 375/480/768 work.
17. Keyboard focus is visible.
18. No broken console errors remain.
19. No Firebase Storage/Cloud Functions were introduced.
20. GitHub Pages relative paths work.

If something genuinely critical is missing, identify only that specific blocker. Otherwise implement continuously without unnecessary questions.

## Final deliverables
Return a complete deployment-ready project with:
- source files
- assets
- Firebase rules
- Firebase indexes
- `PROJECT_ARCHITECTURE.md`
- `BUILD_PROMPT.md`
- `FIREBASE_SETUP.md`
- README
