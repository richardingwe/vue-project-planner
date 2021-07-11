<template>
	<div class="home">
		<FilterNav @filterChange="current = $event" :current="current" />
		<div v-if="projects.length">
			<div v-for="project in filteredProjects" :key="project.id">
				<SingleProject
					:project="project"
					@complete="handleComplete"
					@delete="handleDelete"
				/>
			</div>
		</div>
	</div>
</template>

<script>
	import SingleProject from '../components/SingleProject.vue';
	import FilterNav from '../components/FilterNav.vue';

	export default {
		name: 'Home',
		components: { SingleProject, FilterNav },
		data() {
			return {
				projects: [],
				current: 'all',
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
		methods: {
			handleDelete(id) {
				this.projects = this.projects.filter((project) => project.id !== id);
			},
			handleComplete(id) {
				let p = this.projects.find((project) => project.id === id);
				p.complete = !p.complete;
			},
		},
		computed: {
			filteredProjects() {
				if (this.current === 'completed') {
					return this.projects.filter((project) => project.complete);
				}
				if (this.current === 'ongoing') {
					return this.projects.filter((project) => !project.complete);
				}
				return this.projects;
			},
		},
	};
</script>
