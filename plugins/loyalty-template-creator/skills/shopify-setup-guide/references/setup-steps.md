# CDN Template Setup Steps

Detailed instructions for adding a custom CDN loyalty page template to a Shopify store.

## Step 1: Add the section to the theme

The `.liquid` template file must be placed in the theme's `sections/` folder.

### Option A: Via Shopify Admin (no code editor needed)

1. Go to **Shopify Admin → Online Store → Themes**
2. Click **"..." → Edit code** on the active theme
3. In the left sidebar, find the **Sections** folder
4. Click **"Add a new section"**
5. Name it to match the template (e.g., `jeri-loyalty-mystore`)
6. Replace the default content with the full `.liquid` template code
7. Click **Save**

### Option B: Via Shopify CLI (for developers)

If you have the theme checked out locally:

```bash
# Copy the template into the theme's sections folder
cp theme-extensions/loyalty-page/templates/mystore.liquid sections/jeri-loyalty-mystore.liquid

# Push to Shopify
shopify theme push --only sections/jeri-loyalty-mystore.liquid
```

### Naming convention

Use a `jeri-loyalty-` prefix for the section filename to avoid conflicts:

| Template file | Section filename |
|--------------|-----------------|
| `mystore.liquid` | `jeri-loyalty-mystore.liquid` |
| `brand-name.liquid` | `jeri-loyalty-brand-name.liquid` |

## Step 2: Create a page template

Create a JSON page template that references the section. This tells Shopify to render the loyalty section when a page uses this template.

### Via Shopify Admin

1. In the theme code editor, find the **Templates** folder
2. Click **"Add a new template"**
3. Select **Type: page**, name it `loyalty`
4. Replace the content with:

```json
{
  "sections": {
    "loyalty": {
      "type": "jeri-loyalty-mystore"
    }
  },
  "order": ["loyalty"]
}
```

Replace `jeri-loyalty-mystore` with your actual section filename (without `.liquid`).

### Via Shopify CLI

```bash
# Create the template file
echo '{
  "sections": {
    "loyalty": {
      "type": "jeri-loyalty-mystore"
    }
  },
  "order": ["loyalty"]
}' > templates/page.loyalty.json

shopify theme push --only templates/page.loyalty.json
```

## Step 3: Create a Shopify page

1. Go to **Shopify Admin → Online Store → Pages**
2. Click **"Add page"**
3. Set the **Title** (e.g., "Loyalty Program", "Rewards")
4. In the **Theme template** dropdown (right sidebar), select **"loyalty"**
5. Click **Save**

The page will now render using your custom loyalty section.

## Step 4: Add to navigation

1. Go to **Shopify Admin → Online Store → Navigation**
2. Select the menu to edit (Main menu, Footer, etc.)
3. Click **"Add menu item"**
4. Set the **Name** (e.g., "Rewards", "Loyalty")
5. For the **Link**, search for and select your loyalty page
6. Click **Save menu**

## Step 5: Configure via theme editor

1. Go to **Shopify Admin → Online Store → Themes → Customize**
2. Navigate to **Pages → your loyalty page**
3. Click on the loyalty section in the left sidebar
4. Adjust settings:
   - **Hero title and subtitle** — match the brand messaging
   - **CTA configuration** — set guest redirect URL and logged-in behavior
   - **Section visibility** — toggle earnings, tiers, how-it-works
   - **Colors** — adjust to match the store's brand
   - **Custom CSS** — add any final overrides

## Step 6: Verify

Open the page URL in an incognito window to verify:

1. **As a guest** — check the page loads, CTA shows "Join now" or equivalent
2. **As a logged-in customer** — check CTA shows "See rewards" and widget opens
3. **On mobile** — check responsive layout
4. **Browser console** — no JS errors, API calls return data

### Quick verification URL

```
https://{store-name}.myshopify.com/pages/{page-handle}
```

## Alternative: Theme Editor Setup (Add Section)

If the template includes a `"presets"` array in its `{% schema %}`, the user can also add it via the theme editor without creating a JSON template:

1. Go to **Customize** → any page
2. Click **"Add section"**
3. Search for the template name (from the schema `"name"` field)
4. The section appears and can be configured directly

This method is simpler but gives less control over page layout (the section is added alongside other sections rather than being the only content on the page).

## Troubleshooting

### Template not appearing in theme editor

- Verify the section file is in the `sections/` folder (not `snippets/` or `templates/`)
- Check the `{% schema %}` includes a valid `"presets"` array
- The schema `"name"` must be 25 characters or fewer

### JS not loading / blank page

- Check the `<script src>` URL is correct for the environment:
  - Staging: `https://s.cdn.jericommerce.com/shopify/loyalty-page.js`
  - Production: `https://cdn.jericommerce.com/shopify/loyalty-page.js`
- Ensure the `defer` attribute is on the script tag
- Check browser console for CORS or 404 errors

### Data not loading (earnings/tiers empty)

- Verify `data-shop="{{ shop.permanent_domain }}"` is on the root element
- Check that the JeriCommerce program is active for this shop
- Try `data-shop="your-store.myshopify.com"` hardcoded to test

### Widget CTA not working

- The JeriCommerce storefront widget must be installed and active
- The `#jeri=loyalty/rewards` route only works when the widget JS is loaded
- For stores without the widget, use `cta_action: "account"` instead
