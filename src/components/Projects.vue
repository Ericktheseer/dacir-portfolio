<script setup>
	
	import projects from '../data/projects.json';
	import ProjectCard from './ProjectCard.vue';

	import {computed} from 'vue';

	const chunkSize = 3

	const chunkedProjects = computed(() => {

		const chunks = [];

		for (let i = 0; i < projects.length; i+= chunkSize) {

			chunks.push(projects.slice(i, i + chunkSize))
			console.log(chunks);
		}

		return chunks;

	}) 

</script>

<template>
	<!-- Projects -->
	<section class="container-fluid py-4 px-4 bg-primary" id="projects">        	
        	<section class="row justify-content-center align-items-center gap-2 px-5 py-5">
        		<h2 class="name text-center py-3">• Projects •</h2>
	    <div
	    	v-for="(group, index) in chunkedProjects"
	    	:key="index"
	    	class="card-deck my-5 justify-content-center"
	    >
	    	<ProjectCard 
	    		v-for="project in group"
	    		:key="project.id"
	    		:project="project"
	    	 />
	    </div>	

		</section>        	        	       	
	</section>
	<!-- End of Projects -->

</template>

<style scoped>	

</style>