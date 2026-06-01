<script setup>

	import {defineProps} from 'vue';

	defineProps({
		
		project:Object
	})
	
</script>

<template>

	<div class="col">
	  <div class="card h-100">
	    <img :src="project.image" class="card-img-top img-fluid" alt="...">
	    <div class="card-body">
	      <h5 class="secondname card-title text-center">{{project.title}}</h5>
	      <p class="pname card-text text-center">{{project.description}}</p>
	    </div>
	  </div>
	</div>
	
</template>

<style scoped>
	
</style>