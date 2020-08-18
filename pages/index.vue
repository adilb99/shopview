<template>
  
  <div class="container">
    
    <Sidebar>
     <ul class="sidebar-panel-nav" style="list-style-type:none;">
       <li> Filter items: </li>
       <li> 
         <select>
           <option value="" selected> by Category... </option>
           <option> SmartPhone </option>
           <option> SmartWatch </option>
           <option> Accessories </option>
          </select> 
        </li>

       <li>
         <select>
           <option value="" selected> by Price... </option>
           <option> Expensive first </option>
           <option> Cheap first </option>
          </select> 
         </li>
       
       <li>
         <select>
           <option value="" selected> by Popularity... </option>
           <option> Popular first </option>
           <option> Unpopular first </option>
          </select> 
       </li>
     
     </ul>
   </Sidebar>
    
    <div class="main">

      <h1> Welcome to iBuy! </h1>

    </div>
    
    <nuxt />
      
  </div>


</template>
<script>
import Sidebar from '../components/Sidebar.vue';
export default {
  components: {
    Sidebar
  }
}
</script>

<style scoped>

  option[value=""][selected] {
    color: gray;

  }

  .sidebar-panel-nav li {
    padding-bottom: 8px;
  }

  .sidebar-panel-nav select {
    
  }

  .main {
    margin-left: 200px;
    padding-left: 15px;
    background-color: #f0f0f0;
  }

</style>