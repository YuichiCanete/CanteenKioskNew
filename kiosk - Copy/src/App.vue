<script setup>
  import { ref } from 'vue';
  let foods = []
  let users = []
  let orders = []

  let scene = ref('login')
  let currentUser = {}
  let currentPayType = 'Cash'
  let orderNum = 0

  function setScene(newScene){
    scene.value = newScene
  }

  function tryLogin(){
    const userID = document.getElementById('inputUser').value;
    const password = document.getElementById('inputPass').value;
    
    const user = users.find(u => u.User_ID === userID && u.Password === password);
    if (user) {
      if (user.UserType === 'student') {
        setScene('createOrder')
        console.log('Student logged in');
      } else if (user.UserType === 'personnel') {
        setScene('createOrder')
        console.log('Personnel logged in');
      } else if (user.UserType === 'cashier') {
        setScene('viewOrders')
        console.log('Cashier logged in');
      } else {
        console.log('Unknown user type');
      }
    } else {
      console.log('Invalid credentials');
    }
  }

  function addFood(Name,Price) {
    foods.push({
      Name:Name,
      Price:Price,
      Quantity: 0,
      Inventory: 10,
    })
  }

  function addUser(User_ID,Password,UserType){
    users.push({
      User_ID:User_ID,
      Password:Password,
      UserType:UserType
    })
  }

  function addOrder(){
    orderNum += 1
    const currentOrder = {
      OrderNum: orderNum,
      User: currentUser,
      Foods: [],
      PaymentType: currentPayType,
      Date: '20/03/2024',
    }
    foods.forEach(function(food){
      const f = food
      if (f.Quantity > 0){
        currentOrder.Foods.push({...f})
      }
    })
    orders.push(currentOrder)
    resetQuantity()
  }

  function resetQuantity() {
    foods.forEach(function(food){food.Quantity = 0})
  }

  function getTotal(ord){
    let total = 0
    ord.forEach(function(food){
      total += (food.Price * food.Quantity)
    })
    return total
  }

  addFood('Egg',10)
  addFood('Hotdog',15)
  addFood('Rice',10)
  addFood('Egg',10)
  addFood('Hotdog',15)
  addFood('Rice',10)
  addFood('Egg',10)
  addFood('Hotdog',15)
  addFood('Rice',10)
  addFood('Egg',10)
  addFood('Hotdog',15)
  addFood('Rice',10)

  addUser('220000000743','userpass123','student')
  addUser('220000000744','userpass123','personnel')
  addUser('220000000745','userpass123','cashier')

  foods[0].Quantity = 20
  foods[1].Quantity = 60
  foods[2].Quantity = 400
  foods[3].Quantity = 9

  currentUser = users[0].User_ID
  addOrder()

  foods[2].Quantity = 120
  foods[4].Quantity = 360
  currentUser = users[1].User_ID
  addOrder()

</script>

<script>
  import FoodCard from './components/FoodCard.vue';
  export default {
    components: {
      FoodCard
    }
  }
</script>

<template>

  <div class="container-fluid">

    <!-- Header -->
    <div class="row bg-pink p-2 pt-3 mb-3">
      <h1><img src="./assets/Canteen Kiosk.png" style="width: 5rem; border-radius: 100%;"> Canteen Kiosk </h1>
    </div>

    <div class="row justify-content-center " v-if="scene=='login'">
      
      <div class="col-5 col-md-3 card bg-pink text-center">
        <div class="p-3">
          <h2 class="text-white">Login</h2>
          <input type="text" placeholder="User ID" class="form-control mb-2" id="inputUser">
          <input type="password" placeholder="Password" class="form-control mb-2" id="inputPass">
          <input type="button" value="Login" class="btn btn-light" v-on:click="tryLogin()"> <!-- Temporary -->
        </div>
      </div>
    </div>

    <div class="row" v-else-if="scene=='createOrder'">
      
      <div class="col-md-8 col-12">
        <h2 class="text-black">Create Order</h2>
        <div class="row" style="height: 35rem; overflow-y: scroll;">
          <div class="col-6 col-md-4" v-for="food in foods">
            <FoodCard :Name=food.Name :Price=food.Price></FoodCard>
          </div>
        </div>
      </div>

      <div class="col-md-4 col-12">
        <h2 class="text-black">My Order</h2>
        <div class="card bg-pink p-1" style="height: 20rem; overflow-y: auto;">

          <div v-for="food in foods" class="m-1 d-inline-flex">
            <img src="https://images.squarespace-cdn.com/content/v1/57879a6cbebafb879f256735/1579721909133-R2KSZ8VGDGBI90DYATBK/header4.jpg" style="width: 20%;" class="m-2">
            <div>
              <p class="mb-0">{{ food.Name }} x{{ food.Quantity }}</p>
              <p>P{{ food.Price }}</p>
            </div>
          </div>

        </div>

        <h2 class="text-black mt-2">Payment</h2>
        <div class="card bg-pink p-1 text-center" style="height: 10rem;">
          <p>Total: 0</p>
          <div>
            <button class="btn btn-light m-1" v-on:click="setScene('orderSuccess')">Cash</button>
            <button class="btn btn-light m-1" v-on:click="setScene('orderSuccess')">Tally</button>
          </div>
        </div>

      </div>

    </div>

    <div class="row justify-content-center" v-else-if="scene=='orderSuccess'">
      <div class="text-center">
        <h2 class="text-black">Order Successful</h2>
      </div>

      <div class="col-5 col-md-3 card bg-pink text-center">
        <div class="p-3">
          <h2 class="text-white">Your Order Number is</h2>
          <h1> ## </h1>
        </div>
      </div>

      <div class="text-center">
        <button class="btn bg-pink text-white m-2" v-on:click="setScene('login')">Create Order</button>
        <button class="btn bg-pink text-white m-2" v-on:click="setScene('login')">Log Out</button>
      </div>
    </div>

    <div class="row justify-content-center" v-else-if="scene=='viewOrders'">
      <table class="bg-pink col-sm-12 col-md-11">
        <thead>
          <tr>
            <th scope="col">Order Number</th>
            <th scope="col">User ID</th>
            <th scope="col">Order</th>
            <th scope="col">Total</th>
            <th scope="col">Payment Type</th>
            <th scope="col">Date</th>
            <th scope="col">Actions</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="order in orders" class="text-align-top">
            <td>{{ order.OrderNum }}</td>
            <td scope="row">{{ order.User }}</td>
            <td><p class='m-1' v-for="food in order.Foods">{{ food.Name }} x {{ food.Quantity }}</p></td>
            <td>{{ getTotal(order.Foods) }}</td>
            <td>{{ order.PaymentType }}</td>
            <td>{{ order.Date }}</td>
            <td>
              <button class="btn btn-light m-2">Confirm</button>
              <button class="btn btn-light">Cancel</button>
            </td>
          </tr>
        </tbody>
      </table>

      <div class="row justify-content-center mt-2">
        <div class="col-12">
          <button class="btn bg-pink text-light m-2" v-on:click="setScene('editMenu')">Edit Menu</button>
          <button class="btn bg-pink text-light m-2">Print</button>
        </div>
      </div>

    </div>

    <div class="row justify-content-center" v-else-if="scene=='editMenu'">
      <table class="bg-pink col-sm-12 col-md-11">
        <thead>
          <tr>
            <th scope="col">Image</th>
            <th scope="col">Name</th>
            <th scope="col">Price</th>
            <th scope="col">Inventory</th>
            <th scope="col">Actions</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="food in foods" class="text-align-top">
            <td></td>
            <td>{{ food.Name }}</td>
            <td>{{ food.Price }}</td>
            <td>{{ food.Inventory }}</td>
            <td>
              <button class="btn btn-light m-2">Edit</button>
              <button class="btn btn-light">Remove</button>
            </td>
          </tr>
        </tbody>
      </table>
    </div>

    

    


  </div>

</template>

