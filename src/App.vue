<template>
	<div id="app">
		<main-component />
	</div>
</template>

<script>
import MainComponent from "./components/MainComponent";

export default {
	name: "App",
	components: {
		MainComponent,
	},
};
</script>

<style>
@font-face {
	font-family: "Inter";
	src: url("./assets/fonts/Inter-Regular.ttf") format("truetype");
}
@font-face {
	font-family: "Inter";
	src: url("./assets/fonts/Inter-Thin.ttf") format("truetype");
	font-weight: 100;
}

@font-face {
	font-family: "Inter";
	src: url("./assets/fonts/Inter-Light.ttf") format("truetype");
	font-weight: 300;
}
@font-face {
	font-family: "Inter";
	src: url("./assets/fonts/Inter-Medium.ttf") format("truetype");
	font-weight: 500;
}
#app {
	width: 100%;
	height: 100%;
	margin: 0;
	padding: 0;
	font-family: "Inter", Arial, sans-serif;
	background-image: url("./assets/img/background.png");
}
body {
	width: 100%;
	height: 100%;
	margin: 0;
	padding: 0;
}
html {
	width: 100%;
	height: 100%;
	margin: 0;
	padding: 0;
	font-size: 62.5%;
}
</style>
