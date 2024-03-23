<template>
    <div class="shopping-app">
      <header>
        <h1>Welcome to Our Store</h1>
        
        <button class="logout-btn" @click="handleLogout">Logout</button>
      </header>
  
      <div class="item-list">
        <h2>Items</h2>
        <div v-for="item in products" :key="item.id" class="item-container">
          <div class="item-info">
            <img :src="item.image" :alt="item.name" class="item-image" />
            <p class="item-name">{{ item.name }} - ₱{{ item.price }}</p>
            <button class="add-to-cart-btn" @click="addToCart(item)">Add to Cart</button>
          </div>
        </div>
      </div>
  
      <div class="cart">
        <h2>Cart</h2>
        <div v-if="cart.length === 0" class="empty-cart">Your cart is empty</div>
        <div v-else>
          <div v-for="(item, index) in cart" :key="index" class="cart-item">
            <img :src="item.product.image" :alt="item.product.name" class="cart-item-image" />
            <p class="cart-item-info">{{ item.product.name }} - ₱{{ item.product.price }} x {{ item.quantity }}</p>
            <div class="quantity-controls">
              <button @click="decreaseQuantity(index)" class="quantity-btn">-</button>
              <button @click="increaseQuantity(index)" class="quantity-btn">+</button>
              <button @click="removeFromCart(index)" class="remove-btn">Remove</button>
            </div>
          </div>
          <p class="total">Total: ₱{{ totalPrice }}</p>
        </div>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        products: [
          { id: 1, name: 'Stylish Bag', price: 5, image: require('@/assets/bagss.jpg') },
          { id: 2, name: 'Fashionable Shoes', price: 10, image: require('@/assets/shoes.jpg') },
          { id: 3, name: 'Elegant Handbag', price: 25, image: require('@/assets/bags.jpg')}
        ],
        cart: []
      };
    },
    computed: {
      totalPrice() {
        return this.cart.reduce((total, item) => {
          return total + (item.product.price * item.quantity);
        }, 0);
      }
    },
    methods: {
      addToCart(item) {
        const foundIndex = this.cart.findIndex(cartItem => cartItem.product.id === item.id);
        if (foundIndex !== -1) {
          this.cart[foundIndex].quantity++;
        } else {
          this.cart.push({ product: item, quantity: 1 });
        }
      },
      removeFromCart(index) {
        this.cart.splice(index, 1);
      },
      increaseQuantity(index) {
        this.cart[index].quantity++;
      },
      decreaseQuantity(index) {
        if (this.cart[index].quantity > 1) {
          this.cart[index].quantity--;
        }
      },
      handleLogout() {
        localStorage.removeItem('token')
        this.$router.push('/')
      }
    }
  };
  </script>
  
  <style scoped>
  .shopping-app {
    font-family: Arial, sans-serif;
    max-width: 800px;
    margin: 0 auto;
    padding: 20px;
    background-color: #f5f5f5;
    border-radius: 10px;
    box-shadow: 0px 0px 10px rgba(0, 0, 0, 0.1);
  }
  
  header {
    display: flex;
    justify-content: space-between;
    align-items: center;
  }
  
  .logout-btn {
    background-color: #3498db;
    color: white;
    border: none;
    padding: 8px 16px;
    border-radius: 5px;
    cursor: pointer;
    transition: background-color 0.3s;
  }
  
  .logout-btn:hover {
    background-color: #2980b9;
  }
  
  .item-list {
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
    margin-top: 30px;
  }
  
  .item-container {
    margin: 10px;
    padding: 10px;
    background-color: #fff;
    border-radius: 10px;
    box-shadow: 0px 2px 5px rgba(0, 0, 0, 0.1);
  }
  
  .item-info {
    text-align: center;
  }
  
  .item-image {
    width: 100px;
    height: 100px;
    border-radius: 5px;
  }
  
  .item-name {
    margin-top: 10px;
    font-weight: bold;
  }
  
  .add-to-cart-btn {
    background-color: #3498db;
    color: white;
    border: none;
    padding: 8px 16px;
    border-radius: 5px;
    cursor: pointer;
    transition: background-color 0.3s;
  }
  
  .add-to-cart-btn:hover {
    background-color: #2980b9;
  }
  
  .cart {
    margin-top: 50px;
  }
  
  .empty-cart {
    text-align: center;
  }
  
  .cart-item {
    display: flex;
    align-items: center;
    margin-bottom: 10px;
  }
  
  .cart-item-image {
    width: 50px;
    height: 50px;
    border-radius: 5px;
  }
  
  .cart-item-info {
    margin-left: 10px;
  }
  
  .quantity-controls {
    margin-left: auto;
  }
  
  .quantity-btn {
    background-color: #3498db;
    color: white;
    border: none;
    padding: 5px 10px;
    border-radius: 5px;
    cursor: pointer;
    transition: background-color 0.3s;
  }
  
  .quantity-btn:hover {
    background-color: #2980b9;
  }
  
  .remove-btn {
    background-color: #e74c3c;
    color: white;
    border: none;
    padding: 5px 10px;
    border-radius: 5px;
    cursor: pointer;
    transition: background-color 0.3s;
  }
  
  .remove-btn:hover {
    background-color: #c0392b;
  }
  
  .total {
    text-align: right;
    margin-top: 20px;
    font-weight: bold;
  }
  h1{
    font-family: 'Trebuchet MS', 'Lucida Sans Unicode', 'Lucida Grande', 'Lucida Sans', Arial, sans-serif;
  }
  </style>
  