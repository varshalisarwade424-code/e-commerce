# e-commerce
This is my first project
// app.js
const express = require('express');
const bodyParser = require('body-parser');
const app = express();
app.use(bodyParser.json());

// Mock database
let products = [
  { id: 1, name: 'Laptop', price: 800 },
  { id: 2, name: 'Phone', price: 500 },
  { id: 3, name: 'Headphones', price: 100 }
];

let cart = [];

// Get all products
app.get('/products', (req, res) => {
  res.json(products);
});

// Add product to cart
app.post('/cart', (req, res) => {
  const { productId, quantity } = req.body;
  const product = products.find(p => p.id === productId);
  if (product) {
    cart.push({ ...product, quantity });
    res.json({ message: 'Added to cart', cart });
  } else {
    res.status(404).json({ message: 'Product not found' });
  }
});

// View cart
app.get('/cart', (req, res) => {
  res.json(cart);
});

// Checkout
app.post('/checkout', (req, res) => {
  const total = cart.reduce((sum, item) => sum + item.price * item.quantity, 0);
  cart = []; // clear cart
  res.json({ message: 'Order placed successfully', total });
});

app.listen(3000, () => console.log('E-commerce API running on port 3000'));

