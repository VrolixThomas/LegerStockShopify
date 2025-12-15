# Metal Price Calculator - Setup Guide

## Overview
The custom size calculator allows customers to input their own dimensions and get instant dynamic pricing based on material type, form/shape, and size specifications.

---

## Features

✅ **Dynamic Pricing** - Real-time price calculation based on weight and material cost
✅ **Multiple Forms** - Support for flat sheets, tubes, round bars, L-shapes, T-shapes, and square bars
✅ **Material Selection** - Aluminium, Iron, Steel, Stainless Steel (Inox), and Copper
✅ **Smart Calculations** - Automatically calculates volume, weight, and pricing
✅ **Quantity Support** - Calculate prices for multiple pieces
✅ **Mobile Responsive** - Works perfectly on all devices
✅ **Custom Dimensions** - Customers can specify exact measurements they need

---

## Quick Start

### Option 1: Add to All Product Pages

To add the calculator to all product pages automatically:

1. Open `sections/product-information.liquid`
2. Find the line with `{% capture additional_blocks %}`
3. Add the calculator BEFORE that line:

```liquid
{%- render 'metal-price-calculator' -%}

{% capture additional_blocks %}
```

### Option 2: Add to Specific Products Only

To add the calculator only to specific product pages:

1. Go to **Shopify Admin → Online Store → Themes → Customize**
2. Navigate to a product page
3. Click **Add section** or **Add block**
4. Add a **Custom Liquid** block
5. Paste this code:

```liquid
{% render 'metal-price-calculator' %}
```

### Option 3: Create a Standalone Calculator Page

To create a dedicated calculator page:

1. Go to **Shopify Admin → Online Store → Pages → Add page**
2. Title: "Custom Size Calculator"
3. In the content editor, click **Show HTML**
4. Paste this code:

```liquid
<div class="page-width">
  {% render 'metal-price-calculator' %}
</div>
```

---

## Configuration

### Adjusting Prices

Prices are configured in the calculator snippet at `snippets/metal-price-calculator.liquid`.

**Find this section (around line 123):**

```javascript
const PRICING_CONFIG = {
  aluminium: { pricePerKg: 3.50, density: 2.7 },
  iron: { pricePerKg: 2.20, density: 7.87 },
  steel: { pricePerKg: 2.50, density: 7.85 },
  inox: { pricePerKg: 5.50, density: 8.00 },
  copper: { pricePerKg: 8.50, density: 8.96 }
};
```

**To change prices:**
- Modify the `pricePerKg` value (in Euros)
- Keep the `density` values as they are (g/cm³) - these are physical properties

**Example:** To change aluminium to €4.00 per kg:
```javascript
aluminium: { pricePerKg: 4.00, density: 2.7 }
```

### Minimum Order Settings

**Find this section (around line 131):**

```javascript
const MINIMUM_ORDER = { threshold: 10, surcharge: 5 };
```

This adds a €5 surcharge for orders under €10.

**To change:**
- `threshold`: Minimum order value in Euros
- `surcharge`: Additional charge for small orders in Euros

**Example:** €15 minimum with €8 surcharge:
```javascript
const MINIMUM_ORDER = { threshold: 15, surcharge: 8 };
```

**To disable minimum order surcharge:**
```javascript
const MINIMUM_ORDER = { threshold: 0, surcharge: 0 };
```

### Adding New Materials

To add a new material (e.g., Brass):

1. Open `snippets/metal-price-calculator.liquid`

2. **Add the option in the HTML** (around line 22):
```html
<option value="brass" data-price="6.50" data-density="8.5">Brass</option>
```

3. **Add the pricing config** (around line 123):
```javascript
const PRICING_CONFIG = {
  aluminium: { pricePerKg: 3.50, density: 2.7 },
  iron: { pricePerKg: 2.20, density: 7.87 },
  steel: { pricePerKg: 2.50, density: 7.85 },
  inox: { pricePerKg: 5.50, density: 8.00 },
  copper: { pricePerKg: 8.50, density: 8.96 },
  brass: { pricePerKg: 6.50, density: 8.5 }  // NEW
};
```

### Customizing Available Forms

The calculator supports these forms by default:
- Flat Sheet
- Tube (Square/Rectangular)
- Round Bar
- L-Shape Angle
- T-Shape
- Square Bar

**To remove a form**, delete its option from the HTML (around line 34):
```html
<!-- DELETE THIS LINE if you don't sell tubes: -->
<option value="tube" data-calc-type="tube">Tube (Square/Rectangular)</option>
```

---

## Enabling "Add to Cart" Functionality

By default, the calculator shows an alert with order details. To enable actual cart functionality:

### Step 1: Create a "Custom Order" Product

1. Go to **Shopify Admin → Products → Add product**
2. **Title**: "Custom Metal Order"
3. **Price**: Set to €0.00 (price will be calculated dynamically)
4. **Description**: "Custom-sized metal product made to your specifications"
5. Make sure the product is **Published**
6. **Save** the product

### Step 2: Get the Variant ID

1. In the product page, click on the variant
2. Look at the URL - it will contain the variant ID
3. Example URL: `myshop.myshopify.com/admin/products/1234567890/variants/9876543210`
4. The variant ID is: `9876543210`

**OR** use this method:
1. Open your browser's Developer Console (F12)
2. Paste this code and press Enter:
```javascript
fetch('/admin/api/2024-01/products.json')
  .then(r => r.json())
  .then(data => console.log(data));
```

### Step 3: Update the Calculator Code

1. Open `snippets/metal-price-calculator.liquid`
2. Find the `addCalculatedToCart()` function (around line 358)
3. Find this commented section:
```javascript
/* Uncomment and configure this when you have a Custom Order product set up:

const CUSTOM_PRODUCT_VARIANT_ID = 'YOUR_VARIANT_ID_HERE';
```

4. **Uncomment** the entire block and replace `YOUR_VARIANT_ID_HERE` with your actual variant ID:
```javascript
const CUSTOM_PRODUCT_VARIANT_ID = '9876543210';

fetch('/cart/add.js', {
  method: 'POST',
  // ... rest of the code
```

5. **Comment out or delete** the alert code above it:
```javascript
/*
alert(`Custom Order Details:\n\n...`);
*/
```

6. **Save** the file

Now when customers click "Add Custom Size to Cart", it will add the custom order to their cart with all the specifications as line item properties!

---

## How Calculations Work

### Volume Calculations

The calculator automatically determines the correct formula based on form:

**Flat Sheet:**
```
Volume = Length × Width × Thickness
```

**Round Bar:**
```
Volume = π × (Diameter/2)² × Length
```

**Tube (Rectangular):**
```
Volume = (Outer Volume) - (Inner Volume)
Inner dimensions = Outer - (2 × Wall Thickness)
```

**L-Shape:**
```
Volume = Leg 1 + Leg 2 - Overlap
```

**T-Shape:**
```
Volume = Flange + Web - Overlap
```

**Square Bar:**
```
Volume = Size² × Length
```

### Weight Calculation

```
Weight (kg) = Volume (cm³) × Density (g/cm³) / 1000
```

### Price Calculation

```
Unit Price = Weight (kg) × Price per kg (€/kg)
Total Price = Unit Price × Quantity

If Total Price < Minimum Order Threshold:
  Total Price = Total Price + Surcharge
```

---

## Customization

### Changing Colors

The calculator uses a blue color scheme by default. To customize:

**Find the `.calc-result` style** (around line 446):
```css
.calc-result {
  background: linear-gradient(135deg, #f0f9ff 0%, #e0f2fe 100%);
  border: 2px solid #0ea5e9;
  /* ... */
}
```

**Change to green:**
```css
.calc-result {
  background: linear-gradient(135deg, #f0fdf4 0%, #dcfce7 100%);
  border: 2px solid #22c55e;
  /* ... */
}
```

**Change button color** (around line 493):
```css
.calc-add-to-cart-btn {
  background: #0ea5e9; /* Change this color */
  /* ... */
}
```

### Changing Currency

By default, prices show in Euros (€). To change to another currency:

1. Find all instances of `€` in the calculator code
2. Replace with your currency symbol (e.g., `$`, `£`, `kr`)

**Or use currency codes:**
```javascript
document.getElementById('calc-total-price').textContent = `USD ${totalPrice.toFixed(2)}`;
```

---

## Pricing Examples

### Example 1: Flat Aluminium Sheet
- **Material:** Aluminium (€3.50/kg, density 2.7 g/cm³)
- **Dimensions:** 2000mm × 1000mm × 3mm
- **Quantity:** 1

**Calculation:**
```
Volume = (2000/10) × (1000/10) × (3/10) = 6,000 cm³
Weight = 6,000 × 2.7 / 1000 = 16.2 kg
Price = 16.2 × €3.50 = €56.70
```

### Example 2: Steel Round Bar
- **Material:** Steel (€2.50/kg, density 7.85 g/cm³)
- **Dimensions:** 3000mm length × 25mm diameter
- **Quantity:** 5

**Calculation:**
```
Volume = π × (25/2/10)² × (3000/10) = 1,472.62 cm³
Weight = 1,472.62 × 7.85 / 1000 = 11.56 kg
Unit Price = 11.56 × €2.50 = €28.90
Total Price = €28.90 × 5 = €144.50
```

---

## Troubleshooting

### Calculator doesn't show up
- Check that you've added `{% render 'metal-price-calculator' %}` to your template
- Verify the file exists at `snippets/metal-price-calculator.liquid`
- Clear your browser cache

### Prices seem incorrect
- Verify your pricing configuration matches your actual costs
- Check that density values are correct (standard material property)
- Ensure dimensions are being entered in millimeters

### "Add to Cart" doesn't work
- Make sure you've created a "Custom Order" product
- Verify you've uncommented the cart functionality code
- Check that the variant ID is correct
- Look for errors in browser console (F12)

### Dimensions fields don't appear
- Make sure you've selected both Material and Form
- Check browser console for JavaScript errors
- Try refreshing the page

---

## Best Practices

1. **Set Realistic Prices** - Include material cost, cutting fees, handling, and margin
2. **Test Thoroughly** - Try different materials and sizes to ensure pricing makes sense
3. **Custom Order Workflow** - Set up a process to handle custom orders (e.g., email notification, manual review)
4. **Lead Times** - Inform customers that custom sizes may take longer to prepare
5. **Minimum Sizes** - Consider adding minimum dimension requirements for manufacturing feasibility
6. **Maximum Sizes** - Add maximum dimensions based on your equipment capabilities

---

## Advanced: Adding Custom Validation

To add minimum/maximum dimension limits:

```javascript
function calculatePrice() {
  // ... existing code ...

  // Add validation
  if (dimensions.length < 100) {
    showError('Minimum length is 100mm');
    return;
  }

  if (dimensions.length > 6000) {
    showError('Maximum length is 6000mm');
    return;
  }

  // Continue with calculation...
}
```

---

## Support

For questions or customization help:
- Review the code comments in `snippets/metal-price-calculator.liquid`
- All calculation logic is clearly documented
- Pricing configuration is centralized at the top of the JavaScript section

---

## Summary

You now have a fully functional dynamic pricing calculator that:
- Calculates accurate prices based on material, form, and dimensions
- Supports all common metal forms and materials
- Can be easily customized for your business
- Provides a great customer experience for custom orders

The calculator helps customers get instant quotes for custom sizes, reducing the need for manual quote requests and improving sales conversion! 🎉
