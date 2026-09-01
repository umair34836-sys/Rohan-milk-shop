# Rohan Milk Shop — FIREBASE_SETUP

## 1. Firebase Project
Project ID:
`rohan-milk`

The web configuration is already placed in:
`firebase/config.js`

The Firebase API key in a web app config is not a service-account secret. Firestore Security Rules are still mandatory.

## 2. Enable Authentication
Firebase Console → Authentication → Sign-in method:
- Enable Email/Password.

## 3. Create Firestore Database
Firebase Console → Firestore Database → Create database.

Use the production security rules supplied in:
`firestore.rules`

## 4. Create the first admin
1. Create an account through `/auth.html`.
2. In Firebase Console → Authentication → Users, copy that user's UID.
3. In Firestore create:
`admins/{UID}`
4. Add:
`role: "admin"`

Admin access is controlled by Firestore rules and the presence of that document.

## 5. Collections
The application uses:
- `users`
- `admins`
- `products`
- `orders`
- `coupons`
- `banners`
- `settings`

Create `settings/store` when ready, or let the Admin Settings page create/update it.

## 6. Initial Catalog
After logging into `/admin/`, use:
Dashboard → Seed sample catalog

This creates example dairy products. Replace their prices/stock/details with the real shop catalog.

## 7. Product Images
Firebase Storage is intentionally not used. Product records may contain an `image` URL. If no image exists, the UI renders a clean SVG/CSS fallback.

## 8. Payment Methods
Admin → Settings supports:
- Cash on Pickup
- Bank Transfer
- Easypaisa
- JazzCash

Gateway verification/API integration is not included because no payment gateway account/API was provided.

## 9. Temporary Store Details
Admin → Settings can replace:
- Pakistan — temporary location
- 8:00 AM – 10:00 PM — temporary
- +92 300 0000000 — temporary

## 10. Firestore Index
Customer order history queries by `userId` + `createdAt` may require a composite index depending on Firestore's current index state. If Firebase returns an index-creation link, create the suggested index.

## 11. Hosting
GitHub Pages can host the static frontend.
Firebase Authentication and Firestore remain external.

For GitHub Pages:
- keep relative asset paths
- deploy the project root
- do not convert Firebase config into server-only code
- no server runtime is required
