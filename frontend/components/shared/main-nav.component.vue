<template>
	<div class="main-nav">
		<main-nav-button v-bind:parent="this"></main-nav-button>
		<div
			v-bind:class="'nav-overlay ' + (navOverlayActive ? 'active' : 'inactive')"
		></div>
		<div class="nav-button-group-center-wrapper">
			<div
				v-bind:class="
					'nav-button-group ' + (navOverlayActive ? 'active' : 'inactive')
				"
			>
				<span v-bind:class="'nav-button'" v-on:click="navigateToLogin()">
					<img class="nav-button-icon" src="assets/icons/user.svg" />{{
						isLoggedIn ? user.name.trim() : 'Log In'
					}}
				</span>
				<span v-on:click="getNewVid()" class="nav-button">
					<img
						class="nav-button-icon"
						src="assets/icons/video-camera.svg"
					/>Discover Radness
				</span>
				<span class="nav-button disabled">
					<img class="nav-button-icon" src="assets/icons/video-player.svg" />All
					Videos
				</span>
				<span class="nav-button disabled">
					<img class="nav-button-icon" src="assets/icons/team.svg" />About
					Perpetual Shred
				</span>
			</div>
		</div>
		<div class="beta">BETA</div>
	</div>
</template>

<script>
	module.exports = {
		props: ['player'],
		data() {
			return {
				navOverlayActive: false,
				isLoggedIn: PS._authenticationService.isLoggedIn(),
				user: PS._authenticationService.getUserDetails()
			};
		},
		methods: {
			toggleMainNavOverlay() {
				this.navOverlayActive = !this.navOverlayActive;
			},
			getNewVid() {
				window.location.reload();
			},
			navigateToLogin() {
				PS._store.set('time', this.player.controller.getCurrentTime());
				this.$router.push(this.isLoggedIn ? '/account' : '/login');
			}
		},
		components: {
			'main-nav-button': httpVueLoader(
				'components/shared/main-nav-button.component.vue'
			)
		}
	};
</script>

<style scoped>
	.main-nav {
		position: fixed;
		top: 0;
	}

	.nav-overlay {
		position: fixed;
		top: 0;
		width: 100vw;
		height: 100vh;
		background: black;
		z-index: -10;
	}
	.nav-overlay.active {
		opacity: 0.8;
		transition: 0.2s;
	}
	.nav-overlay.inactive {
		opacity: 0;
		visibility: hidden;
		transition: 0.2s;
	}

	.nav-button-group-center-wrapper {
		height: 100%;
	}
	.nav-button-group {
		color: white;
		font-family: serif;
		font-size: 2rem;
		display: flex;
		flex-direction: column;
		position: fixed;
		top: 50%;
		left: 50%;
		transform: translate(-50%, -50%);
		white-space: nowrap;
	}
	.nav-button-group.active {
		opacity: 1;
		transition: visibility 0s, opacity 0.2s linear;
		cursor: pointer;
	}
	.nav-button-group.inactive {
		opacity: 0;
		visibility: hidden;
		transition: visibility 0s, opacity 0.2s linear;
	}

	.nav-button {
		cursor: pointer;
	}
	.nav-button-icon {
		height: 1.8rem;
		margin-right: 10px;
		transform: translateY(5px);
	}

	.disabled {
		opacity: 0.5;
		text-decoration: line-through;
	}
	.beta {
		font-size: 2rem;
		position: fixed;
		right: 1rem;
		bottom: 1rem;
		font-family: sans-serif;
		color: white;
		font-weight: bold;
	}
</style>