# Rohan Milk Shop — PROJECT_ARCHITECTURE

## 1. Project Overview
Rohan Milk Shop is a Pakistan-first dairy ecommerce website for ordering Fresh Milk, Dahi/Yogurt, Lassi, Butter, Cheese, Cream and Eggs online for **shop pickup**. Delivery is currently disabled.

## 2. Goals
- Present the Rohan Dairy brand professionally.
- Make dairy products easy to discover on mobile.
- Support custom product quantities.
- Allow guest checkout and Firebase customer accounts.
- Give customers order history when signed in.
- Give one authenticated admin complete catalog/order/store management.
- Keep the project static-hosting friendly and GitHub Pages compatible.

## 3. User Roles
### Guest
Browse products, add to cart, checkout, select payment method, receive an order ID.

### Customer
Everything a guest can do, plus Firebase email/password authentication and order history.

### Admin
Authenticated Firebase user whose UID exists in `admins/{uid}`. Can manage products, stock, orders, customers, coupons, banners, store settings and payment methods.

## 4. Core Features
- Responsive storefront
- Product categories
- Product search and sorting
- Product detail pages
- Custom quantity/step controls
- Persistent local cart
- Guest checkout
- Customer signup/login
- Customer order history
- Pickup-only ordering
- Multiple configurable payment methods
- Admin dashboard
- Product CRUD
- Stock management
- Order status management
- Customer list
- Coupon CRUD
- Banner CRUD
- Store/contact/hours settings
- Firestore-backed catalog and orders
- Analytics-ready dataLayer events

## 5. Pages
| Page | URL | Purpose |
|---|---|---|
| Home | `/index.html` | Brand hero, categories, products, pickup CTA |
| Shop | `/shop.html` | Search, filters, sorting, catalog |
| Product | `/product.html?id=...` | Product detail and custom quantity |
| Cart | `/cart.html` | Review/edit cart |
| Checkout | `/checkout.html` | Guest/customer order creation |
| Success | `/order-success.html` | Order confirmation |
| Auth | `/auth.html` | Email/password signup/login |
| Account | `/account.html` | Customer profile and order history |
| Admin | `/admin/` | Protected management dashboard |

## 6. User Flow
Home → Shop → Product → Add to Cart → Cart → Checkout → Payment Selection → Place Pickup Order → Success.

Customer flow:
Auth → Account → Order History.

Admin flow:
Admin Login → Dashboard → Products / Orders / Customers / Coupons / Banners / Settings.

## 7. Order Lifecycle
`pending → confirmed → ready_for_pickup → picked_up`
with `cancelled` available at any point where appropriate.

## 8. Store Fulfillment
Delivery is disabled. `deliveryCost` is always `0` in the current implementation. Store location, hours and contact details are temporary settings and can be changed from the admin panel.

## 9. Payment Architecture
The store settings document contains a `payments` array. Current UI supports:
- `cash_pickup`
- `bank_transfer`
- `easypaisa`
- `jazzcash`

The architecture is intentionally extensible. Actual transfer account instructions can be added to settings later.

## 10. Firebase Architecture
Services:
- Firebase Authentication
- Cloud Firestore

Explicitly not used:
- Firebase Storage
- Firebase Cloud Functions

Firebase config lives in `firebase/config.js`.

## 11. Firestore Collections
### `users/{uid}`
- `email`: string
- `name`: string
- `createdAt`: string/timestamp

### `admins/{uid}`
Admin authorization marker. The document can contain `role: "admin"`.

### `products/{productId}`
- `name`: string
- `category`: `Milk | Dairy | Eggs`
- `description`: string
- `price`: number
- `stock`: number
- `unit`: string
- `minQuantity`: number
- `step`: number
- `image`: string URL, optional
- `createdAt`: timestamp
- `updatedAt`: timestamp

### `orders/{orderId}`
- `userId`: string or null
- `customerName`: string
- `phone`: string
- `pickupNote`: string
- `notes`: string
- `items`: array of `{productId,name,price,quantity,unit}`
- `subtotal`: number
- `total`: number
- `deliveryCost`: number
- `paymentMethod`: string
- `status`: string
- `createdAt`: timestamp
- `updatedAt`: timestamp

### `coupons/{couponId}`
- `code`: string
- `type`: `percent | fixed`
- `value`: number
- `active`: boolean
- `createdAt`: timestamp

### `banners/{bannerId}`
- `title`: string
- `subtitle`: string
- `image`: string URL
- `active`: boolean
- `createdAt`: timestamp

### `settings/store`
- `storeName`
- `location`
- `hours`
- `phone`
- `whatsapp`
- `payments[]`

## 12. Security Model
Firestore rules must be the authority for authorization. Hiding admin UI is not security.

- Public users can read products/categories/banner/store settings as configured.
- Only admins can create/update/delete products, coupons, banners and store settings.
- Customers can read/write only their own user document.
- Signed-in customers can read only their own orders.
- Admins can read/update all orders and customer records.
- Guest order creation is allowed only for a validated order shape; guest users cannot read arbitrary orders afterward.

## 13. UI/UX Source Basis
The implementation uses the uploaded UI/UX Pro Max source package as the design-system basis. Relevant source areas include:
- `ui-ux-pro-max/SKILL.md`
- `design/SKILL.md`
- `design-system/SKILL.md`
- `design-system/references/token-architecture.md`
- `design-system/references/component-specs.md`
- `brand/SKILL.md`
- `brand/references/logo-usage-rules.md`

The generated project design system is persisted at:
`design-system/rohan-milk-shop/MASTER.md`

## 14. Design System
The generated UI/UX system selected:
- Pattern: Feature-Rich Showcase
- Style: Vibrant & Block-based
- Primary: `#059669`
- Secondary: `#10B981`
- Accent/CTA: `#EA580C`
- Background: `#ECFDF5`
- Foreground: `#064E3B`
- Typography: Rubik headings + Nunito Sans body
- 4px/8px/16px/24px/32px/48px/64px spacing scale
- 200ms-ish transitions
- Visible focus states
- Minimum 44px interactive controls
- Reduced-motion support
- No emoji icons; SVG line icons are used

## 15. Responsive Design
Intentional mobile-first layouts at:
- 375px
- 480px
- 768px
- 1024px
- 1440px

No horizontal scrolling should be required.

## 16. SEO
- Unique titles/descriptions for public pages
- Semantic headings
- Canonical metadata on primary pages
- Open Graph metadata
- Product URLs use query IDs in this static implementation
- Static hosting compatibility is prioritized

For a production launch, generate a sitemap/robots strategy matching the final domain.

## 17. Analytics
The app pushes events to `window.dataLayer`:
- `add_to_cart`
- `view_cart`
- `begin_checkout`
- `purchase`
- `sign_up`
- `login`

GTM/GA4/Google Ads/Meta Pixel can consume this dataLayer later without changing core checkout logic.

## 18. Performance
- User hero image is loaded from local assets.
- Product images are lazy-loaded where applicable.
- No unnecessary framework/build dependency is required.
- CSS is centralized.
- Firebase modules are loaded from the official CDN.
- Reduced-motion is respected.

## 19. Error/Empty States
Implemented or planned states include:
- Empty cart
- Empty product catalog
- Product not found
- Firebase catalog failure
- Checkout failure
- Unauthorized admin
- No customer orders
- Loading states
- No search results

## 20. Hosting
The site is static-file based and compatible with GitHub Pages. Firebase Authentication and Firestore remain external services.

## 21. Known Limitations
- Product images currently use admin-entered URLs because Firebase Storage is intentionally not used.
- Guest orders are not exposed through a public tracking page; customers should keep the order confirmation or sign in for account-based history.
- Payment gateway verification is not implemented because no external payment gateway account/API was specified.
- Temporary shop details remain placeholders until updated in Admin → Settings.

## 22. Acceptance Criteria
- All public pages load without framework build tooling.
- Cart persists between pages.
- Product quantity can be customized.
- Checkout creates a Firestore order.
- Successful order redirects to `order-success.html`.
- Signed-in customers can see their own orders.
- Admin access depends on `admins/{uid}`.
- Admin can CRUD products, coupons and banners.
- Admin can update order statuses.
- Admin can update store/payment settings.
- No Firebase Storage or Cloud Functions are used.
- Mobile navigation and controls remain usable at small widths.
