<template>
	<div class="home">
		<div v-if="projects.length">
			<div v-for="project in projects" :key="project.id">
				<p>{{ project.title }}</p>
			</div>
		</div>
	</div>
</template>

<script>
	export default {
		name: 'Home',
		components: {},
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
