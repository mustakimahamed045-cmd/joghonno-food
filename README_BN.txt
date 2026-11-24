# JOGHONNO FOOD — Cloudflare Worker + D1 Preview Package

এই প্যাকেজে গ্রাহক ওয়েবসাইট, Cloudflare Worker API, অর্ডার ট্র্যাকিং এবং একটি সরল Admin Order Management প্যানেল একসঙ্গে রাখা হয়েছে।

## গুরুত্বপূর্ণ
- এটি `joghonno-food-platform` Worker-এর জন্য; বর্তমান লাইভ `round-flower-3c4a` Worker বা `joghonnofood.com` custom domain পরিবর্তন করবে না।
- প্রথমে Preview/নতুন Worker URL-এ পরীক্ষা করুন। সব পরীক্ষা সফল না হওয়া পর্যন্ত custom domain যুক্ত করবেন না।
- Admin প্যানেল আপাতত অর্ডার দেখা, সর্বশেষ অর্ডার আগে দেখা, order status এবং payment status আপডেট করতে পারে। পুরোনো Admin-এর Food/Category/Settings CRUD ফিচার এই API-তে এখনো যুক্ত করা হয়নি।
- মূল ZIP-এর favicon এবং apple-touch-icon ফাইল রাখা হয়েছে।

## GitHub / Cloudflare Workers Builds
1. ZIP Extract করুন।
2. ফাইল ও `public` ফোল্ডারসহ পুরো প্রজেক্টটি GitHub repository-তে আপলোড করুন (root-এ `wrangler.jsonc` ও `worker.js` থাকতে হবে)।
3. Cloudflare Dashboard → Workers & Pages → `joghonno-food-platform` → Settings/Builds-এ repository connect করুন, যদি আগে থেকে connected না থাকে।
4. Build command: `npx wrangler deploy`
5. Deploy-এর আগে নিশ্চিত করুন Worker-এর D1 binding `DB` এই database-এ যুক্ত আছে:
   `32dafbbf-3d93-4600-9ef8-2379b370c764`
   এই binding `wrangler.jsonc`-এও দেওয়া আছে।
6. Worker secrets হিসেবে `ADMIN_USERNAME`, `ADMIN_PASSWORD`, `TELEGRAM_CHAT_ID` থাকতে হবে। `TELEGRAM_BOT_TOKEN` না থাকলে Telegram notification পাঠানো হবে না। Token Dashboard-এর Secrets-এ যোগ করবেন; GitHub-এ বা source code-এ লিখবেন না।
7. Deploy হলে আগে পরীক্ষা করুন:
   - `/api/health`
   - `/` (গ্রাহক ওয়েবসাইট)
   - `/admin` (অ্যাডমিন)
   - একটি test order, তারপর tracking এবং admin status update।

## API routes
- `GET /api/health`
- `GET /api/store`
- `POST /api/order`
- `GET /api/track?orderCode=...&mobile=...`
- `POST /api/admin/login`
- `GET /api/admin/orders`
- `POST /api/admin/update`

## নিরাপত্তা
Admin password শুধুমাত্র Cloudflare Worker Secret হিসেবে রাখুন। Public GitHub repository-তে secret/token রাখবেন না।
