<script lang="ts" setup>
let navbar = [
	{ name: 'Users', path: '/search' },
	{ name: 'Beer', path: '/beer' },
	{ name: 'Deposit', path: '/deposit' },
]
</script>

<template>
    <v-app class="bg-grey-darken-4">
		<nav class="navbar">
			<NuxtLink to="/" class="home-item navbar-item">Home</NuxtLink>
			<div>
				<NuxtLink to="/search" class="page-item navbar-item">Users</NuxtLink>
				<NuxtLink to="/beer" class="page-item navbar-item">Beer</NuxtLink>
				<NuxtLink to="/deposit" class="page-item navbar-item">Deposit</NuxtLink>
			</div>
		</nav>
        <v-main>
            <v-container style="height: 100vh">
                <v-layout full-height="">
                    <v-row align="center" justify="center">
                        <v-container>
                            <NuxtPage />
                        </v-container>
                    </v-row>
                </v-layout>
            </v-container>
            <v-footer class="bg-teal-darken-4 text-center d-flex flex-column">
                <span>FOOTER</span>
            </v-footer>
        </v-main>
    </v-app>
</template>

<style>
.navbar {
	background-color: #333;
	display: flex;
	padding-top: 1rem;
	padding-bottom: 1rem;
	padding-left: 1rem;
	padding-right: 1rem;
	justify-content: space-between;
	overflow: hidden;
}

.home-item {
	color: white;
}

.page-item {
	color: white;
	margin-left: 1rem;
}
</style>