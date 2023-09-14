<script setup>
//import...
import { ref, reactive , computed} from 'vue';


//ตัวแปร
const travelList=ref([
  { name :'ท่างช้าง' , price: 200 , size:{s:145,m:150,l:200}, img:"https://www.awaygpub.com/wp-content/uploads/2021/03/%E0%B8%97%E0%B9%88%E0%B8%B2%E0%B8%8A%E0%B9%89%E0%B8%B2%E0%B8%87%E0%B8%84%E0%B8%B2%E0%B9%80%E0%B8%9F%E0%B9%88-%E0%B8%A3%E0%B9%89%E0%B8%B2%E0%B8%99%E0%B9%80%E0%B8%AB%E0%B8%A5%E0%B9%89%E0%B8%B2%E0%B8%8A%E0%B8%B7%E0%B9%88%E0%B8%AD%E0%B8%94%E0%B8%B1%E0%B8%87%E0%B9%83%E0%B8%88%E0%B8%81%E0%B8%A5%E0%B8%B2%E0%B8%87%E0%B9%80%E0%B8%A1%E0%B8%B7%E0%B8%AD%E0%B8%87%E0%B9%83%E0%B8%AB%E0%B8%A1%E0%B9%88-awaygpub-2.jpg", quantity: 0 },
  { name :'Overnight' , price: 120, size:{s:120,m:180,l:250}, img:"https://scontent.fbkk4-5.fna.fbcdn.net/v/t39.30808-6/317936665_121576337434685_8382304064389607974_n.jpg?_nc_cat=101&ccb=1-7&_nc_sid=49d041&_nc_eui2=AeGTk1tlQE1N_dMGVXWKIih7c-tk2J9p4fBz62TYn2nh8M0BlwpsE9_BHagAepANWUzeQRH0yX02-8BmLadbvQgw&_nc_ohc=gjffHK5gYGsAX8o5lp5&_nc_zt=23&_nc_ht=scontent.fbkk4-5.fna&oh=00_AfDkI8Bc0mM3bIqMhff-W3emg42RnTAMtQz-HhXhaAaPmA&oe=6507ECFD", quantity: 0 } ,
  { name :'สุนทรารมณ์', price: 100,  size:{s:100,m:190,l:300}, img:"https://scontent.fbkk3-2.fna.fbcdn.net/v/t39.30808-6/240458393_4180233748759457_367999437414233225_n.jpg?_nc_cat=104&ccb=1-7&_nc_sid=5614bc&_nc_eui2=AeEyShai6LA2m9oMaZstvjoio9OimscKz8Oj06KaxwrPw89CKxoTpQR7ZuUzVC6kcSNCQSKXKoyJO1ufBjgYXswK&_nc_ohc=3h1K5wFXip0AX-ONYDc&_nc_zt=23&_nc_ht=scontent.fbkk3-2.fna&oh=00_AfDiDHYje1tT5_pJ3orInaayW4QVjGzgg1Y8EuPCeDtxvw&oe=6509214B", quantity: 0 } ,
  { name :'The Good view', price: 150 ,  size:{s:150,m:200,l:500}, img:"https://www.northspace.life/wp-content/uploads/2022/12/The-Good-View-01-1536x830.jpeg", quantity: 0 },
  { name :'ตะวันแดง', price: 170 ,  size:{s:190,m:250,l:700}, img:"https://img-prod.api-onscene.com/cdn-cgi/image/format=auto,width=640/https://sls-prod.api-onscene.com/partner_files/trueidintrend/21246/CC697BDB-184E-48B4-9CD1-A85019FE3B35.jpeg", quantity: 0 },
  { name :'Bang   ', price: 100 ,  size:{s:110,m:130,l:650}, img:"https://www.northspace.life/wp-content/uploads/2022/12/%E0%B8%9A%E0%B8%B2%E0%B8%87%E0%B8%82%E0%B8%A7%E0%B8%B2%E0%B8%87-03-1024x683.jpeg", quantity: 0 }
  
]);

// เก็บข้อมูลการจองร้าน
const bookingList = reactive([]);



const totalAmount = computed(() => {
  return bookingList.reduce((total, item) => total + item.price, 0);
});


//function

function booking_control(name, quantity) {
  const itemindex = bookingList.findIndex(item => item.name === name);

  if (itemindex !== -1) {
    bookingList[itemindex].quantity += quantity;
    bookingList[itemindex].price = bookingList[itemindex].quantity * travelList.value.find(item => item.name === name).price;
  } else {
    const item = {
      name: name,
      quantity: quantity,
      price: quantity * travelList.value.find(item => item.name === name).price
    };
    bookingList.push(item);
  }
}

function quantity_increased(item) {
  item.quantity++;
  item.price = item.quantity * travelList.value.find(travel => travel.name === item.name).price;
}

function quantity_decreased(item) {
  if (item.quantity > 0) {
    item.quantity--;
    item.price = item.quantity * travelList.value.find(travel => travel.name === item.name).price;
  }
}

function removeItem(index) {
  bookingList.splice(index, 1);
}

//pop up

const form_costumer = ref({
  name: "",
  phone: "",
  date: "",
  time: ""
});

const showPopup = ref(false);

function togglePopup() {
  showPopup.value = !showPopup.value;
}

const isFormComplete = ref(false);

function checkFormCompletion() {
  const { name, phone, date, time } = form_costumer;
  isFormComplete.value = name !== "" && phone !== "" && date !== "" && time !== "";
}


</script>

<template>

    <div class="container">
      <div class="row">
        <div class="col" v-for=" (i,index) in travelList" :key="index">
          <div class="card shadow">
            <div class="bd-placeholder-img card-img-top" width="100%" height="225"  >
              <title>Placeholder</title>
              <img :src="i.img" height="300" width="300">
              <rect width="100%" height="100%" fill="#55595c" />
            </div>
            <div class="card-body">
              <h5 class="card-title">{{ i.name }}</h5>
                <p>ราคาโต๊ะ ={{i.size.s}}</p>
                <h>**** 6 คนต่อ 1 โต๊ะ ****</h>
                <p class="card-text">จำนวนโต๊ะ <br>
                   <input type="number" class="value_out" v-model="i.quantity">
                </p>
              <div class="box_btn">
                <div class="btn-group">
                  <button type="button" class="btn btn-sm btn-outline-secondary">View</button>
                  <button type="button" class="btn btn-sm btn-outline-secondary" @click="booking_control(i.name,i.quantity)">จองโต๊ะ</button>
                </div>
              </div>
            </div>
          </div>
        </div>
    </div>
  </div>

  <div class="contrainer_costumer " v-if="bookingList.length>0">
  <div class="form_box"> <h2 style="text-align: center;">กรุณากรอกข้อมูล</h2></div>
  <form>
    <div class="form_box">
      <label for="name">ชื่อ-นามสกุล </label><br>
      <input type="text" id="name" class="form-control" v-model="form_costumer.name" required @input="checkFormCompletion">
    </div>
    <div class="form_box">
      <label for="phone">เบอร์โทร </label><br>
      <input type="tel" id="phone" class="form-control" v-model="form_costumer.phone" required @input="checkFormCompletion">
    </div>
    <div class="form_box">
      <label for="date">วันที่ </label><br>
      <input type="date" id="date" class="form-control" v-model="form_costumer.date" required @input="checkFormCompletion">
    </div>
    <div class="form_box">
      <label for="time">เวลา </label><br>
      <input type="time" id="time" class="form-control" v-model="form_costumer.time" required @input="checkFormCompletion">
    </div>
    <div class="form_box">
      <button type="button" class="btn btn-success" @click="togglePopup" :disabled="!isFormComplete">ยืนยันการจอง</button>
    </div>
  </form>
</div>

      <h1 style="text-align: center;" v-if="bookingList.length>0" class="head">รายการจองโต๊ะ</h1>
      <div class="List_cout">
        <table class="table table-hover" v-if="bookingList.length>0">
          <table class="table">
            <thead>
              <tr>
                <th scope="col">ลำดับ</th>
                <th scope="col">ชื่อร้าน</th>
                <th scope="col">จำนวนโต๊ะ</th>
                <th scope="col">ราคา</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="(i,index) in bookingList" :key="index" >
                <th scope="row">{{index+1}}</th>
                <td>{{ i.name }}</td>
                <td>
                    <div class="table_quantity">
                        <button type="button" class="add" @click="quantity_increased(i)" >+</button>
                        <span>{{ i.quantity }}</span>
                        <button type="button" class="cut"  @click="quantity_decreased(i)" :end="i.quantity === 0">-</button>
                    </div>
                </td>
                <td>{{ i.price }}</td>
                <td>
                    <button type="button" class="btn btn-danger" @click="removeItem(index)" >ลบ</button>
                </td>
              </tr>
            </tbody>
          </table>
        </table>
      </div>
      
    
<div v-if="showPopup" class="overlay">
  <div class="popup">
    <h2 style="text-align: center; color:green;">ยืนยันการจองสำเร็จ</h2>
    <h3>รายละเอียด</h3>
    <div class="form">
      <strong>ชื่อ-นามสกุล:</strong> {{ form_costumer.name }}
      <strong>เบอร์โทร:</strong> {{ form_costumer.phone }}
      <strong>วันที่:</strong> {{ form_costumer.date }}
      <strong>เวลา:</strong> {{ form_costumer.time }}
    </div>

    <table class="box_table">
      <thead>
        <tr>
          <th>ลำดับ</th>
          <th>ชื่อร้าน</th>
          <th>จำนวนโต๊ะ</th>
          <th>ราคา</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(item, index) in bookingList" :key="index">
          <td>{{ index + 1 }}</td>
          <td>{{ item.name }}</td>
          <td>{{ item.quantity }}</td>
          <td>{{ item.price }}</td>
        </tr>
      </tbody>
    </table>

    <div class="total_all">
    <strong>ยอดรวมทั้งหมด:</strong> {{ totalAmount }} บาท
    </div>

    <button class="btn btn-danger" @click="showPopup = false">ปิด</button>
  </div>
</div>



</template>
      


<style>
body {
  font-family: Arial, sans-serif;
}
.container {
  display: flex;
  flex-wrap: wrap;
  justify-content: space-between;
  margin: 20px auto;
  max-width: 1200px;
}
.card {
  box-shadow: 0px 4px 6px rgba(0, 0, 0, 0.1);
}
.card-title {
  font-size: 1.2rem;
  margin: 10px 0;
}

.card-text {
  font-size: 0.9rem;
}

.value_out {
  background-color: floralwhite;
  color:black;
  padding: 5px;
  border: 1px solid #ccc;
  border-radius: 5px;
  width: 40px; /* ปรับขนาดความกว้างของ input */
  text-align: center; /* จัดข้อความตรงกลาง */
  font-size: 1rem;
}

.btn-group button {
  cursor: pointer;
}

.btn-group button {
  cursor: pointer;
}

.head {
  text-align: center;
  margin-top: 20px;
  font-size: 1.5rem;
}

.List_cout {
  margin-top: 20px;
}

.table_quantity {
  display: flex;
  align-items: center;
  justify-content: space-between; /* เพิ่มระยะห่างระหว่างองค์ประกอบ */
}

.add,
.cut {
  width: 30px;
  height: 30px;
  display: flex;
  justify-content: center;
  align-items: center;
  background-color: floralwhite;
  color:navy;
  border: none;
  cursor: pointer;
  font-size: 1rem;
  border-radius: 30%;
  margin-right: 20%; /* เพิ่มระยะห่างขวา */
  margin-left: 20%;  /* เพิ่มระยะห่างซ้าย */
}

.add:hover,
.cut:hover {
  background-color: grey;

}

.contrainer_costumer {
  max-width: 400px;
  margin: 20px auto;
  padding: 20px;
  border: 1px solid #ccc;
  border-radius: 5px;
  box-shadow: 0px 4px 6px rgba(0, 0, 0, 0.1);
  background-color: #f9f9f9;
}

.form_box {
  margin-bottom: 15px;
}

.form-control {
  width: 100%;
  padding: 10px;
  border: 1px solid #ccc;
  border-radius: 5px;
}

.btn-success {
  background-color: #28a745;
  color: #fff;
  border: none;
  padding: 10px 20px;
  cursor: pointer;
  font-size: 1rem;
  border-radius: 5px;
}

.btn-success:hover {
  background-color: #1d8c3b;
}

.overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 100;
}

.popup {
  max-width: 750px;
  background-color: #fff;
  padding: 20px;
  border-radius: 5px;
  box-shadow: 0px 4px 6px rgba(0, 0, 0, 0.1);
}

.popup h2 {
  text-align: center;
  color: green;
  margin-bottom: 10px;
}

.popup h3 {
  margin-top: 10px;
}

.popup .form strong {
  display: block;
  margin-top: 10px;
}

.box_table {
  width: 100%;
  margin-top: 20px;
}

.popup button {
  background-color: #dc3545;
  color: #fff;
  border: none;
  padding: 10px 20px;
  cursor: pointer;
  font-size: 1rem;
  border-radius: 5px;
  margin-top: 10px;
}

.popup button:hover {
  background-color: #c82333;
}


.contrainer_costumer {
  width: 380px; 
  height: 100%; 
  margin: 10px auto;
  padding: 20px;
  border: 1px solid #ccc;
  border-radius: 5px;
  box-shadow: 0px 4px 6px rgba(0, 0, 0, 0.1);
  background-color: #f9f9f9;
  float: right; /* เพิ่ม float: right; เพื่อจัดองค์ประกอบทางขวา */
}

.table {
  width: 100%; /* ตารางที่มีความกว้างเต็มหน้าจอ */
  max-width: 800px; /* ตั้งค่าความกว้างสูงสุดของตาราง */
}




</style>