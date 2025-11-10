

## ❗ Why you can’t link `staging.metabolism.cloud` to the preview URL

* Each preview deployment (triggered by a push to `staging`) gets a **unique URL** based on the commit hash (like `bce4639e.metabolism.pages.dev`).
* Cloudflare does **not** allow setting a custom domain (like `staging.metabolism.cloud`) to **automatically track** the latest preview.
* Custom domains can **only be attached to production deployments**.

---

## ✅ Recommended Solution: Create a Second “Production” Project for Staging

The workaround is to treat `staging` as a separate **Cloudflare Pages project**:

### 🔧 Steps:

1. **Create a new Cloudflare Pages project** in your Cloudflare dashboard.
2. **Set the Git branch to `staging`** instead of `main`.
3. **Assign the custom domain `staging.metabolism.cloud`** to this project.
4. This deployment will act like a **stable staging environment**, always reflecting the latest code on the `staging` branch.

### ✅ Benefits:

* You get a consistent URL (`staging.metabolism.cloud`).
* Each push to the `staging` branch will auto-deploy to this URL.
* You retain your preview deployments for other branches if needed.

---

## ✅ Summary:

| Branch      | Project Type     | Custom Domain              | URL Behavior                               |
| ----------- | ---------------- | -------------------------- | ------------------------------------------ |
| `main`      | Production       | `metabolism.cloud`         | Always up-to-date with main branch         |
| `staging`   | Separate Project | `staging.metabolism.cloud` | Always up-to-date with staging branch      |
| PR branches | Preview          | None                       | Unique URL like `commit.project.pages.dev` |

