<script setup>
import { onMounted, ref } from 'vue';
import HelloWorld from './components/HelloWorld.vue'
import TheWelcome from './components/TheWelcome.vue'


let studentsData = ref([])



onMounted(function(){
  // get api will be called here-----
  fetch('http://127.0.0.1:8000/api/students')
  .then(response => response.json())
  .then(payload => {
    studentsData.value = payload.students_information
  })
  


})
const image=ref('')
const candidate_name=ref('')
const roll=ref('')
const address=ref('')
const handlesave=async()=>{
  const formData=new FormData() 
  formData.append('candidate_name',candidate_name.value)
  formData.append('roll',roll.value)
  formData.append('address',address.value)
  await fetch('http://127.0.0.1:8000/api/students',{
    method:'post',
    body:formData


  })
  fetch('http://127.0.0.1:8000/api/students')
  .then(response => response.json())
  .then(payload => {
    studentsData.value = payload.students_information
  })
  
  

}

const handledelete=async(id)=>{
  await fetch('http://127.0.0.1:8000/api/students/'+id,{
    method:'delete'

  })
  fetch('http://127.0.0.1:8000/api/students')
  .then(response => response.json())
  .then(payload => {
    studentsData.value = payload.students_information
  })

  }
const selectedcandidateid =ref('')
const handleedit=(val)=>{
console.log(val)
selectedcandidateid.value =val.id
candidate_name.value=val.candidate_name
roll.value=val.roll
address.value=val.address

}
const handleUpdate=async()=>{
  const formData=new FormData() 
  formData.append('candidate_name',candidate_name.value)
  formData.append('roll',roll.value)
  formData.append('address',address.value)
  await fetch('http://127.0.0.1:8000/api/students/'+selectedcandidateid.value,{
    method:'post',
    body:formData

  })
  fetch('http://127.0.0.1:8000/api/students')
  .then(response => response.json())
  .then(payload => {
    studentsData.value = payload.students_information
  })

}

const handleimagechange=(e)=>{
  console.log(e.target.files[0])

}
 
</script>

<template>
  <table>
    <tr>
      <th>ID</th>
      <th>Image</th>
      <th>Candidate Name</th>
      <th>Roll(ID)</th>
      <th>Address</th>
      <th>Action Status</th>
      <th>Action Status</th>
    </tr>
    <tr v-for="(item,index) in studentsData" i= index>
      <td>{{index+1}}</td>
      <td>
      <img :src="'http://127.0.0.1:8000/public/uploads/images/'+item.image" width="100"/>
      </td>
      <td>{{item.candidate_name}}</td>
      <td>{{item.roll}}</td>
      <td>{{item.address}}</td>
     <td><button @click="handleedit(item)">EDIT</button></td>
     <td><button @click="handledelete(item.id)">DELETE</button></td>
    </tr>

</table><br/>
<input v-model="candidate_name" placeholder="candidate_name"/><br/>
<input v-model="roll" placeholder="roll"/><br/>
<input v-model="address" placeholder="address"/><br/>
<input type="file" @change="handleimagechange" placeholder="image"/><br/>
<button @click="handlesave">SAVE</button><br/>
<button @click="handleUpdate">UPDATE</button>
</template>

<style scoped>
table {
  font-family: arial, sans-serif;
  border-collapse: collapse;
  width: 100%;
}

td, th {
  border: 1px solid #dddddd;
  text-align: left;
  padding: 8px;
}

tr:nth-child(even) {
  background-color: #dddddd;
}
header {
  line-height: 1.5;
}

.logo {
  display: block;
  margin: 0 auto 2rem;
}

@media (min-width: 1024px) {
  header {
    display: flex;
    place-items: center;
    padding-right: calc(var(--section-gap) / 2);
  }

  .logo {
    margin: 0 2rem 0 0;
  }

  header .wrapper {
    display: flex;
    place-items: flex-start;
    flex-wrap: wrap;
  }
}
</style>
