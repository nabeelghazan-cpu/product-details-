<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>Product Details Example</title>
  <style>
    /* Basic reset */
    * { box-sizing: border-box; margin: 0; padding: 0; }
    body { font-family: Inter, system-ui, -apple-system, "Segoe UI", Roboto, "Helvetica Neue", Arial; color: #222; background: #f6f7fb; line-height: 1.4; padding: 24px; }

    /* Header */
    header { display: flex; align-items: center; justify-content: space-between; gap: 16px; margin-bottom: 24px; }
    .brand { font-weight: 700; font-size: 1.25rem; color: #0b5cff; }
    .search { display: flex; align-items: center; gap: 8px; }
    .search input { width: 320px; max-width: 40vw; padding: 10px 12px; border-radius: 8px; border: 1px solid #d6d9e6; background: #fff; }
    .search input::placeholder { color: #9aa3c7; }
    .cart-indicator { background: #0b5cff; color: #fff; padding: 8px 12px; border-radius: 8px; font-weight: 600; }

    /* Layout */
    .container { display: flex; gap: 32px; align-items: flex-start; }
    .gallery { flex: 1 1 420px; background: #fff; padding: 20px; border-radius: 12px; box-shadow: 0 6px 18px rgba(12,18,40,0.06); }
    .details { flex: 1 1 520px; background: #fff; padding: 20px; border-radius: 12px; box-shadow: 0 6px 18px rgba(12,18,40,0.06); }

    /* Image */
    .product-image { width: 100%; height: 420px; display: grid; place-items: center; border-radius: 8px; overflow: hidden; background: linear-gradient(180deg,#eef2ff,#fff); }
    .product-image img { max-width: 100%; max-height: 100%; object-fit: contain; }

    /* Product meta */
    h1 { font-size: 1.5rem; margin-bottom: 8px; }
    .price { color: #0b5cff; font-weight: 700; font-size: 1.25rem; margin-bottom: 12px; }
    .desc { color: #4b5563; margin-bottom: 16px; }

    /* Controls */
    .controls { display: flex; gap: 12px; align-items: center; margin-bottom: 18px; }
    select { padding: 10px 12px; border-radius: 8px; border: 1px solid #d6d9e6; background: #fff; }
    .qty { display: flex; align-items: center; gap: 8px; }
    .qty button { padding: 8px 10px; border-radius: 8px; border: 1px solid #d6d9e6; background: #fff; cursor: pointer; }
    .add-to-cart { background: #0b5cff; color: #fff; padding: 12px 18px; border-radius: 10px; border: none; cursor: pointer; font-weight: 700; }
    .add-to-cart:active { transform: translateY(1px); }

    /* Reviews */
    .reviews { margin-top: 18px; }
    .review { border-top: 1px solid #eef2f6; padding: 12px 0; }
    .review:first-child { border-top: none; }
    .review .meta { font-weight: 600; color: #111827; margin-bottom: 6px; }
    .review .text { color: #374151; font-size: 0.95rem; }

    /* Responsive */
    @media (max-width: 900px) {
      .container { flex-direction: column; }
      .search input { width: 200px; }
      .product-image { height: 320px; }
    }
  </style>
</head>
<body>

  <header>
    <div class="brand">ShopLab</div>

    <div class="search" aria-hidden="true">
      <input type="text" placeholder="Search products" aria-label="Search products" />
    </div>

    <div class="cart-indicator" id="cartCount">Cart 0</div>
  </header>

  <main class="container" role="main">
    <section class="gallery" aria-labelledby="product-title">
      <div class="product-image" id="imageWrap">
        <img src="https://via.placeholder.com/600x600.png?text=Product+Image" alt="Product image" id="productImg" />
      </div>
    </section>

    <section class="details">
      <h1 id="product-title">Classic Canvas Sneakers</h1>
      <div class="price" id="price">$59.00</div>
      <div class="desc" id="description">
        Lightweight canvas sneakers with cushioned insole and durable rubber sole. Available in multiple colors and sizes.
      </div>

      <div class="controls" aria-label="Purchase controls">
        <label for="sizeSelect" style="font-weight:600;color:#111827;">Size</label>
        <select id="sizeSelect" aria-label="Select size">
          <option value="">Select size</option>
          <option value="6">6</option>
          <option value="7">7</option>
          <option value="8">8</option>
          <option value="9">9</option>
          <option value="10">10</option>
        </select>

        <div class="qty" aria-label="Quantity">
          <button id="decrease" aria-label="Decrease quantity">−</button>
          <div id="quantity" style="min-width:28px;text-align:center;">1</div>
          <button id="increase" aria-label="Increase quantity">+</button>
        </div>

        <button class="add-to-cart" id="addToCart">Add to Cart</button>
      </div>

      <div class="reviews" id="reviews">
        <h2 style="font-size:1.05rem;margin-bottom:8px;">Customer Reviews</h2>
        <div class="review">
          <div class="meta">Aisha — 5 stars</div>
          <div class="text">Very comfortable and true to size. Great value.</div>
        </div>
        <div class="review">
          <div class="meta">Omar — 4 stars</div>
          <div class="text">Good build quality. The color is slightly different than the photo.</div>
        </div>
      </div>
    </section>
  </main>

  <script>
    // Simple interactivity: quantity, add to cart, size validation, cart counter
    (function() {
      const qtyEl = document.getElementById('quantity');
      const inc = document.getElementById('increase');
      const dec = document.getElementById('decrease');
      const addBtn = document.getElementById('addToCart');
      const cartCount = document.getElementById('cartCount');
      const sizeSelect = document.getElementById('sizeSelect');

      let qty = 1;
      let cartItems = 0;

      function updateQty() { qtyEl.textContent = qty; }

      inc.addEventListener('click', () => { qty = Math.min(10, qty + 1); updateQty(); });
      dec.addEventListener('click', () => { qty = Math.max(1, qty - 1); updateQty(); });

      addBtn.addEventListener('click', () => {
        // Basic validation: require size selection
        if (sizeSelect && sizeSelect.value === '') {
          sizeSelect.focus();
          sizeSelect.style.outline = '2px solid #ffb4b4';
          setTimeout(() => sizeSelect.style.outline = '', 1200);
          return;
        }
        cartItems += qty;
        cartCount.textContent = 'Cart ' + cartItems;
        // Simple visual feedback
        addBtn.textContent = 'Added ✓';
        addBtn.disabled = true;
        setTimeout(() => { addBtn.textContent = 'Add to Cart'; addBtn.disabled = false; }, 900);
      });

      // Hover effect for image: swap placeholder color for demo
      const img = document.getElementById('productImg');
      img.addEventListener('mouseenter', () => img.style.transform = 'scale(1.02)');
      img.addEventListener('mouseleave', () => img.style.transform = 'scale(1)');

      // Accessibility: keyboard support for quantity
      inc.addEventListener('keydown', (e) => { if (e.key === 'Enter') inc.click(); });
      dec.addEventListener('keydown', (e) => { if (e.key === 'Enter') dec.click(); });
    })();
  </script>

</body>
</html>
 product-details-