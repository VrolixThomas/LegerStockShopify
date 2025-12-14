# Metal Sheet Shop - Product Setup Guide

## Overview
Your shop is now configured to sell metal sheets with filtering by material type and form, with quick add-to-cart functionality for different dimensions.

## 🎯 Product Structure

Each product represents **one combination** of material + form. For example:
- "Aluminium Tube"
- "Iron L-Shape"
- "Inox Flat Sheet"

The different **dimensions** are handled as **product variants**.

---

## 📦 How to Add Products

### Step 1: Create a Product

1. Go to **Shopify Admin → Products → Add product**

2. **Product Title**: Use format `{Material} {Form}`
   - Examples:
     - "Aluminium Tube"
     - "Iron L-Shape"
     - "Inox T-Shape"
     - "Steel Flat Sheet"

3. **Description**: Add product details, material specs, etc.

4. **Product Image**: Upload an image of the metal product (optional but recommended)

---

### Step 2: Add Tags for Filtering

**IMPORTANT**: Tags are how the filtering system works!

Add TWO tags to each product:

1. **Material tag**: `material:{type}`
   - `material:aluminium`
   - `material:iron`
   - `material:inox`
   - `material:steel`
   - `material:copper`

2. **Form tag**: `form:{shape}`
   - `form:tube`
   - `form:l-shape`
   - `form:t-shape`
   - `form:flat`
   - `form:round`
   - `form:square`

**Example**: For "Aluminium Tube", add tags:
- `material:aluminium`
- `form:tube`

---

### Step 3: Create Variants for Dimensions

Each variant represents a specific size/dimension.

1. Scroll to **Variants** section
2. Click **Add variant**
3. For each dimension, add:
   - **Option name**: "Dimension" or "Size"
   - **Option value**: The specific measurements

**Example for Aluminium Tube:**
```
Variant 1: 20mm x 20mm x 2m - €15.00
Variant 2: 25mm x 25mm x 2m - €18.00
Variant 3: 30mm x 30mm x 2m - €22.00
Variant 4: 40mm x 40mm x 3m - €35.00
```

**How to add variants:**
1. Click "Add variant"
2. Option name: "Dimension"
3. Option values (add one at a time):
   - `20mm x 20mm x 2m`
   - `25mm x 25mm x 2m`
   - `30mm x 30mm x 2m`
   - etc.

4. Set the **price** for each variant
5. Set **inventory** (stock quantity) if needed

---

## 🏷️ Complete Tag Reference

### Materials
Use these exact tag names (lowercase):
- `material:aluminium`
- `material:iron`
- `material:inox`
- `material:steel`
- `material:copper`

### Forms
Use these exact tag names (lowercase):
- `form:tube`
- `form:l-shape`
- `form:t-shape`
- `form:flat`
- `form:round`
- `form:square`

**To add custom materials/forms:**
1. Add the tag to your product (e.g., `material:brass`)
2. Update `sections/product-list.liquid` to add it to the filter dropdown

---

## 📋 Example Product Setup

### Product: "Aluminium L-Shape"

**Details:**
- Title: `Aluminium L-Shape Angle Bar`
- Description: `High-quality aluminium L-shaped angle bars, perfect for structural applications`
- Tags: `material:aluminium`, `form:l-shape`

**Variants (Dimensions):**
| Variant | Dimensions | Price |
|---------|------------|-------|
| 1 | 20mm x 20mm x 3mm x 2m | €12.50 |
| 2 | 25mm x 25mm x 3mm x 2m | €15.00 |
| 3 | 30mm x 30mm x 4mm x 2m | €18.50 |
| 4 | 40mm x 40mm x 5mm x 3m | €28.00 |
| 5 | 50mm x 50mm x 5mm x 3m | €35.00 |

---

## 🎨 Customizing Filters

To add/modify filter options, edit `sections/product-list.liquid`:

**Add a new material** (around line 10):
```html
<option value="brass">Brass</option>
```

**Add a new form** (around line 22):
```html
<option value="hexagon">Hexagon Bar</option>
```

---

## ✅ Quick Checklist for Each Product

- [ ] Product title follows format: `{Material} {Form}`
- [ ] Added tag: `material:{type}`
- [ ] Added tag: `form:{shape}`
- [ ] Created variants for each dimension
- [ ] Set prices for all variants
- [ ] Added product image (optional)
- [ ] Set inventory levels (if tracking stock)

---

## 🛒 How Customers Will Shop

1. **Browse products** on homepage with filter dropdowns
2. **Filter by material** (e.g., "Aluminium")
3. **Filter by form** (e.g., "Tube")
4. **Select dimension** from dropdown on product card
5. **Enter quantity**
6. **Click "Add to Cart"**
7. Product added instantly without page reload!

---

## 🔧 Advanced: Bulk Product Import

If you have many products, consider using:
1. Shopify CSV import
2. Shopify API
3. Apps like "Matrixify" for bulk variant creation

**CSV Example:**
```csv
Handle,Title,Tags,Option1 Name,Option1 Value,Variant Price
alu-tube,Aluminium Tube,material:aluminium form:tube,Dimension,20mm x 2m,15.00
alu-tube,Aluminium Tube,material:aluminium form:tube,Dimension,25mm x 2m,18.00
alu-tube,Aluminium Tube,material:aluminium form:tube,Dimension,30mm x 2m,22.00
```

---

## 📞 Support

Need help? The theme is now fully customized for metal products with:
- ✅ Material + Form filtering
- ✅ Dimension selection on product cards
- ✅ Quick add to cart with quantity
- ✅ Mobile responsive
- ✅ No page reloads

Happy selling! 🎉
