<!DOCTYPE html>
<html lang="="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>TITANIUM</title>
  <link rel="stylesheet" href="https://fonts.googleapis.com/css2?family=Inter:wght@400;700&display=swap">
  <style>
    * { box-sizing: border-box; }
    body {
      font-family: 'Inter', sans-serif;
      margin: 0; padding: 0;
      background-color: #f5f5f5;
      color: #333;
      transition: background 0.3s, color 0.3s;
    }
    body.dark {
      background-color: #121212;
      color: #eee;
    }
    header {
      background: #2d2d2d;
      color: white;
      display: flex; justify-content: space-between;
      align-items: center;
      padding: 15px 25px;
      position: sticky; top: 0; z-index: 1000;
    }
    header .logo {
      font-size: 1.5em;
      font-weight: bold;
    }
    nav a {
      color: white; text-decoration: none;
      margin: 0 10px;
    }
    .dark-toggle {
      background: none;
      border: none;
      color: white;
      font-size: 1.2em;
      cursor: pointer;
    }
    .hero {
      text-align: center;
      padding: 60px 20px;
      background: linear-gradient(to right, #667eea, #764ba2);
      color: white;
    }
    .search-bar {
      padding: 20px;
      text-align: center;
    }
    .search-bar input {
      padding: 10px;
      width: 80%;
      max-width: 400px;
      border: 1px solid #ccc;
      border-radius: 5px;
    }
    .container {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(240px, 1fr));
      gap: 20px;
      padding: 20px;
    }
    .product-card {
      background: white;
      border-radius: 10px;
      padding: 15px;
      text-align: center;
      box-shadow: 0 2px 8px rgba(0,0,0,0.1);
      transition: transform 0.2s;
    }
    .dark .product-card {
      background: #1e1e1e;
    }
    .product-card:hover {
      transform: translateY(-5px);
    }
    .product-card img {
      width: 100%;
      border-radius: 8px;
    }
    .product-card button {
      background: #667eea;
      color: white;
      border: none;
      padding: 10px 15px;
      border-radius: 5px;
      margin-top: 10px;
      cursor: pointer;
    }
    .cart-section {
      padding: 20px;
      background: #fff;
      margin: 20px;
      border-radius: 10px;
    }
    .dark .cart-section {
      background: #1e1e1e;
    }
    .cart-item {
      display: flex; justify-content: space-between;
      align-items: center; padding: 10px 0;
      border-bottom: 1px solid #eee;
    }
    .checkout-form input, .checkout-form textarea {
      width: 100%; padding: 10px; margin: 10px 0;
      border-radius: 5px; border: 1px solid #ccc;
    }
    .checkout-form button {
      background: #764ba2;
      color: white;
      padding: 10px 20px;
      border: none;
      border-radius: 5px;
      cursor: pointer;
    }
    footer {
      text-align: center;
      padding: 20px;
      background: #2d2d2d;
      color: white;
    }
  </style>
</head>
<body>

<header>
  <div class="logo">TITANIUM</div>
  <nav>
    <a href="#products">Products</a>
    <a href="#cart">Cart (<span id="cart-count">0</span>)</a>
    <button class="dark-toggle" onclick="toggleDark()">🌙</button>
  </nav>
</header>

<section class="hero">
  <h1>Welcome to TITANIUM</h1>
  <p>Your favorite tech gear in one beautiful page</p>
</section>

<div class="search-bar">
  <input type="text" id="search" placeholder="Search products...">
</div>

<section id="products" class="container">
  <!-- Products will be inserted here -->
</section>

<section id="cart" class="cart-section">
  <h2>Your Cart 🛒</h2>
  <div id="cart-items"></div>
  <p><strong>Total:</strong> $<span id="cart-total">0.00</span></p>
  <button onclick="clearCart()">Clear Cart</button>

  <h3>Checkout</h3>
  <form class="checkout-form" onsubmit="event.preventDefault(); alert('Order submitted!'); clearCart();">
    <input type="text" placeholder="Your Name" required>
    <input type="email" placeholder="Email Address" required>
    <textarea rows="3" placeholder="Shipping Address" required></textarea>
    <button type="submit">Place Order</button>
  </form>
</section>

<footer>
  <p>&copy; 2025 OnePage Shop Pro | Made with ❤️</p>
</footer>

<script>
  const products = [
    {
      id: 1,
      name: "Wireless Headphones",
      price: 59.99,
      image: "https://cdn-icons-png.flaticon.com/512/1048/1048940.png"
    },
    {
      id: 2,
      name: "Smart Watch",
      price: 129.99,
      image: "https://cdn-icons-png.flaticon.com/512/1008/1008946.png"
    },
    {
      id: 3,
      name: "Laptop Stand",
      price: 35.00,
      image: "https://cdn-icons-png.flaticon.com/512/1041/1041916.png"
    },
    {
      id: 4,
      name: "Gaming Mouse",
      price: 49.95,
      image: "https://cdn-icons-png.flaticon.com/512/1048/1048914.png"
    }
  ];

  let cart = [];

  function displayProducts() {
    const container = document.getElementById('products');
    container.innerHTML = '';
    const search = document.getElementById('search').value.toLowerCase();
    products.filter(p => p.name.toLowerCase().includes(search)).forEach(product => {
      const div = document.createElement('div');
      div.className = 'product-card';
      div.innerHTML = `
        <img src="${product.image}" alt="${product.name}" />
        <h3>${product.name}</h3>
        <p>$${product.price.toFixed(2)}</p>
        <button onclick="addToCart(${product.id})">Add to Cart</button>
      `;
      container.appendChild(div);
    });
  }

  function addToCart(id) {
    const item = products.find(p => p.id === id);
    const existing = cart.find(i => i.id === id);
    if (existing) {
      existing.quantity += 1;
    } else {
      cart.push({ ...item, quantity: 1 });
    }
    updateCart();
  }

  function updateCart() {
    const itemsContainer = document.getElementById('cart-items');
    itemsContainer.innerHTML = '';
    let total = 0;
    cart.forEach(item => {
      total += item.price * item.quantity;
      const row = document.createElement('div');
      row.className = 'cart-item';
      row.innerHTML = `
        <div>
          ${item.name} <br>
          $${item.price.toFixed(2)} x ${item.quantity}
        </div>
        <div>
          <button onclick="removeFromCart(${item.id})">❌</button>
        </div>
      `;
      itemsContainer.appendChild(row);
    });
    document.getElementById('cart-total').textContent = total.toFixed(2);
    document.getElementById('cart-count').textContent = cart.reduce((sum, i) => sum + i.quantity, 0);
  }

  function removeFromCart(id) {
    cart = cart.filter(item => item.id !== id);
    updateCart();
  }

  function clearCart() {
    cart = [];
    updateCart();
  }

  function toggleDark() {
    document.body.classList.toggle('dark');
  }

  document.getElementById('search').addEventListener('input', displayProducts);

  document.addEventListener('DOMContentLoaded', () => {
    displayProducts();
    updateCart();
  });
</script>

</body>
</html>
