<template>
<div>
    <h1> MY CART HERE </h1>
    <p> {{ response }} </p>
 </div>
    
</template>
<script>
export default {
    async asyncData({params, $http}) {
    const response = await $http.$get('http://192.168.99.100:1338/api/product');
    return { response }
  }

}
</script>