<template>
	<div class="home">
		<div v-if="projects.length">
			<div v-for="project in projects" :key="project.id">
				<SingleProject :project="project" />
			</div>
		</div>
	</div>
</template>

<script>
	import SingleProject from '../components/SingleProject.vue';
	export default {
		name: 'Home',
		components: { SingleProject },
		data() {
			return {
				projects: [],
			};
		},
		async mounted() {
			try {
				const res = await fetch('http://localhost:3000/projects');
				const data = await res.json();
				this.projects = data;
			} catch (error) {
				console.log(error);
			}
		},
	};
</script>
