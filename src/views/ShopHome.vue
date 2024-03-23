<template>
    <div class="container">
      <!-- Item Listings -->
      <div class="content">
        <span v-if="isLoggedIn">Shopping is FUN!!</span>
        <div>
          <span> </span>
          <button @click="handleClick" class="login-btn">{{ isLoggedIn ? 'Logout' : 'Login' }}</button>

          <h1>Items</h1>
          <div class="item-list" v-if="products.length > 0">
            <div v-for="item in products" :key="item.id" class="item-container">
              <div class="item-info">
                <img :src="item.image" alt="Item Image" style="width: 100px; height: 100px;" />
                <p>{{ item.name }} - ₱{{ item.price }}</p>
              </div>
              <div class="add-to-cart-btn">
                <button @click="addToCart(item)">Add to Cart</button>
              </div>
            </div>
          </div>
          <div v-else>
            No items available.
          </div>
        </div>
        <div v-if="isLoggedIn">
          <h3>Cart</h3>
          <p>Total Price: ₱{{ totalPrice }}</p>
          <ul>
            <li v-for="(cartItem, index) in cart" :key="index">
              {{ cartItem.product.name }} - ₱{{ cartItem.product.price }} (Quantity: {{ cartItem.quantity }})
            </li>
          </ul>
        </div>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        products: [
          { id: 1, name: 'Item ', price: 5, image: require('@/assets/bagss.jpg') },
          { id: 2, name: 'Item', price: 10, image: require('@/assets/shoes.jpg') },
          { id: 3, name: 'Item', price: 25, image: require('@/assets/bags.jpg')}
        ],
        cart: [],
        isLoggedIn: false // Assuming initially the user is not logged in
      };
    },
    computed: {
      totalPrice() {
        return this.cart.reduce((total, item) => {
          return total + item.product.price * item.quantity;
        }, 0);
      }
    },
    methods: {
      addToCart(item) {
        if (this.isLoggedIn) {
          const foundIndex = this.cart.findIndex((cartItem) => cartItem.product.id === item.id);
          if (foundIndex !== -1) {
            this.cart[foundIndex].quantity++;
          } else {
            this.cart.push({ product: item, quantity: 1 });
          }
        } else {
          const confirmed = window.confirm('Please log in to add items to your cart. Would you like to log in now?');
          if (confirmed) {
            localStorage.setItem('token', '12345');
            this.isLoggedIn = true;
            this.$router.push({ name: 'shop' });
            setTimeout(() => {
              window.confirm('You are now logged in. You can add items to your cart.');
            }, 100);
          }
        }
      },
      handleClick() {
        if (this.isLoggedIn) {
          localStorage.removeItem('token');
          this.isLoggedIn = false;
          this.cart = []; // Clear the cart on logout
        } else {
          localStorage.setItem('token', '12345');
          this.isLoggedIn = true;
          this.$router.push({ name: 'shop' });
          setTimeout(() => {
            window.confirm('You are now logged in. You can add items to your cart.');
          }, 100);
        }
      }
    }
  };
  </script>
  
  <style scoped>
  .container {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
  background-color: lightblue;
}

.content {
  text-align: center;
}

.item-list {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
}

.item-container {
  margin: 10px;
  display: flex;
  flex-direction: column;
  align-items: center;
  width: 200px;
  background-color: #ffdab9; /* Peach color */
  border-radius: 15px; /* Rounded corners */
  padding: 15px;
  box-shadow: 0px 4px 6px rgba(0, 0, 0, 0.1); /* Soft shadow */
}

.item-info {
  text-align: center;
}

.add-to-cart-btn {
  margin-top: 10px;
}

/* Additional styles for cart section */
.cart-section {
  background-color: #ffdab9; /* Peach color */
  border-radius: 15px; /* Rounded corners */
  padding: 20px;
  margin-top: 20px;
  box-shadow: 0px 4px 6px rgba(0, 0, 0, 0.1); /* Soft shadow */
}

.cart-total {
  font-weight: bold;
  margin-top: 10px;
}
.login-btn {
  background-color: #3498db; /* Blue color */
  color: #fff; /* White color */
  border: none;
  padding: 10px 20px;
  font-size: 16px;
  border-radius: 5px;
  cursor: pointer;
  transition: background-color 0.3s, color 0.3s;
}

.login-btn:hover {
  background-color: #2980b9; /* Darker shade of blue */
}

h1{
    font-family: 'Gill Sans', 'Gill Sans MT', Calibri, 'Trebuchet MS', sans-serif;
}
  </style>