# Kitchen Craft PK — website

Smart Kitchen, Better Life.

Single-page kitchen store website: 75 products (naam, tafseel, rate — koi tasveer nahi),
category filter, search, hover par zoom + mini details, order slip aur seedha WhatsApp order.
Logo file ke andar hi embedded hai, alag image file ki zarurat nahi.

WhatsApp number pehle se set hai: **0301 9088013** (`923019088013`).

## Files

| File | Kaam |
|---|---|
| `index.html` | Poori website — HTML, CSS, JS, logo sab isi mein. Koi build step nahi. |
| `supabase-schema.sql` | `products` + `orders` tables aur RLS policies |
| `supabase-seed.sql` | 75 products database mein daalne ke liye |

## 1. GitHub par daalein

```bash
git init
git add .
git commit -m "Kitchen Craft PK website"
git branch -M main
git remote add origin https://github.com/USERNAME/REPO.git
git push -u origin main
```

Phir GitHub par: **Settings → Pages → Source: Deploy from a branch → main / (root) → Save**.
Do minute mein site `https://USERNAME.github.io/REPO/` par live ho jayegi.

## 2. Supabase — step by step

Supabase lagane se do faide hain: products website ki file chhoo'e baghair badle ja sakte hain,
aur har order database mein bhi save ho jata hai (WhatsApp ke ilawa).

1. **Account** — [supabase.com](https://supabase.com) par "Start your project", GitHub se sign in.
2. **New project** — naam `kitchen-craft-pk`, database password likh kar kisi jagah save kar lein,
   region **Southeast Asia (Singapore)** chunein (Pakistan ke liye sab se tez). "Create new project".
   Do teen minute lagte hain.
3. **Tables banayein** — left menu mein **SQL Editor → New query**. `supabase-schema.sql` ki poori
   file copy kar ke paste karein aur **Run** dabayein. "Success" aana chahiye.
4. **Products daalein** — phir **New query**, ab `supabase-seed.sql` paste kar ke **Run**.
   **Table Editor → products** mein 75 rows nazar aayengi.
5. **Keys lein** — **Project Settings (gear) → API**. Do cheezein copy karein:
   - `Project URL` — jaise `https://abcdefgh.supabase.co`
   - `anon` `public` key — lambi `eyJ...` wali string
   (`service_role` key **kabhi** website mein na daalein.)
6. **Website mein daalein** — `index.html` kholein, `<script>` ke shuru mein:

```js
const SUPABASE = {
  url: "https://abcdefgh.supabase.co",
  anonKey: "eyJhbGciOi..."
};
```

7. **Push karein** — `git add . && git commit -m "supabase" && git push`. Site refresh karein.
   Ab products database se load ho rahe hain.
8. **Orders dekhein** — koi test order bhejein, phir **Table Editor → orders** kholein.
   Naam, phone, pata, items aur total wahan aa jayega. Status column `new` se `confirmed` /
   `delivered` khud badal sakte hain.

Anon key public hoti hai — yeh normal hai. Asli hifazat RLS policies karti hain:
products sirf parhe ja sakte hain, orders sirf **daale** ja sakte hain (koi unhe parh nahi sakta),
aur products website se badle nahi ja sakte.

### Naya product Supabase se

**Table Editor → products → Insert row**: `sku` (unique, jaise `CW-13`), `name`, `sub`,
`cat` (in mein se aik: `cookware`, `tools`, `cutlery`, `storage`, `serving`, `bake`, `appliance`, `clean`),
`price`, aur `specs` jo aisi list hai: `["Steel body", "26cm size"]`.
Item chhupana ho to `active` ko `false` kar dein.

## Supabase ke baghair naya product

`index.html` mein `LOCAL_PRODUCTS` array mein aik line barha dein:

```js
{ "sku": "CW-13", "name": "Naya Item", "sub": "Chhoti si tafseel", "cat": "cookware", "price": 1500, "specs": ["Steel body", "26cm"] },
```

## Badalne wali cheezein

- Footer mein dukaan ka pata aur timing (`Shop 12, Main Bazaar, Lahore`) — apna asli pata daal dein.
- Rates — maine market ke qareeb rate daale hain, apni asli list se check kar lein.
- `STORE` block mein naam, WhatsApp number aur display number.

## Baad mein kar sakte hain

- Product tasveerein (Supabase Storage + `image_url` column)
- Admin page jahan se rate aur stock badla ja sake
- Custom domain (jaise `kitchencraft.pk`) GitHub Pages par
