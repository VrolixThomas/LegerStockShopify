# Custom Size Calculator - Quick Start 🚀

## Add Calculator to Your Store (3 Easy Steps)

### Method 1: Using Theme Editor (Recommended - No Code!)

1. **Open Shopify Admin**
   - Go to: **Online Store → Themes → Customize**

2. **Add the Calculator Section**
   - Navigate to any product page
   - Click **Add section**
   - Search for **"Custom Size Calculator"**
   - Click to add it
   - Drag it to position it where you want (usually below product info)

3. **Customize (Optional)**
   - Click on the calculator section
   - Edit the heading/subheading text
   - Adjust padding if needed
   - Click **Save**

**Done!** The calculator is now live on your product pages. ✅

---

### Method 2: Add Code to All Product Pages

If you want the calculator on ALL product pages automatically:

1. Open `sections/product-information.liquid` in the code editor

2. Find line 171 (look for `{% render 'product-information-content',`)

3. Add this line RIGHT BEFORE it:
   ```liquid
   {% render 'metal-price-calculator' %}
   ```

4. Save the file

**Done!** Calculator appears on all product pages. ✅

---

## Customize Pricing (Important!)

**Default prices are examples - update them to match your costs!**

1. Open `snippets/metal-price-calculator.liquid`

2. Find the pricing section (around line 123):
   ```javascript
   const PRICING_CONFIG = {
     aluminium: { pricePerKg: 3.50, density: 2.7 },
     iron: { pricePerKg: 2.20, density: 7.87 },
     steel: { pricePerKg: 2.50, density: 7.85 },
     inox: { pricePerKg: 5.50, density: 8.00 },
     copper: { pricePerKg: 8.50, density: 8.96 }
   };
   ```

3. Change the `pricePerKg` values to YOUR prices (in €)
   - Don't change the `density` values

4. Save the file

---

## What the Calculator Does

✅ Customers select material type (Aluminium, Steel, etc.)
✅ Choose form/shape (Flat sheet, Tube, L-shape, etc.)
✅ Enter custom dimensions in millimeters
✅ See instant price calculation
✅ Adjust quantity
✅ Get total price

**Calculation:** Weight × Material Price = Total Price

---

## Supported Forms

The calculator automatically handles these shapes:

- **Flat Sheet** - Length × Width × Thickness
- **Tube (Rectangular)** - With wall thickness
- **Round Bar** - Diameter × Length
- **L-Shape Angle** - Two legs with thickness
- **T-Shape** - Flange and web
- **Square Bar** - Solid square profile

Each form shows only the relevant dimension fields!

---

## Next Steps

For detailed configuration options, see:
- **CALCULATOR_SETUP_GUIDE.md** - Complete setup and customization guide

Common customizations:
- Change currency from € to $ or £
- Add new materials (e.g., Brass)
- Adjust minimum order settings
- Enable "Add to Cart" functionality
- Customize colors and styling

---

## Quick Test

To test if it's working:

1. View a product page with the calculator
2. Select "Aluminium" as material
3. Select "Flat Sheet" as form
4. Enter dimensions: 2000 × 1000 × 3
5. You should see a price calculation appear

If you see the price, it's working! 🎉

---

## Support

- Calculator file: `snippets/metal-price-calculator.liquid`
- Section file: `sections/custom-size-calculator.liquid`
- Full guide: `CALCULATOR_SETUP_GUIDE.md`

All calculations are based on:
1. Volume from dimensions
2. Weight from volume × density
3. Price from weight × material cost

**Material densities are standard physical properties - don't change them!**
**Only change the pricePerKg values to match your costs.**

---

**That's it! Your customers can now get instant quotes for custom sizes.** 🎉
