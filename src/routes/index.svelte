<script context="module">
	import { browser } from '$app/env';
	import * as THREE from 'three';

	if (browser) {
		// Canvas
		const canvas = document.querySelector('canvas.webgl');

		// Loading
		const textureLoader = new THREE.TextureLoader();

		const normalTexture = textureLoader.load('/NormalMap.png');

		// Scene
		const scene = new THREE.Scene();

		// Objects
		const geometry = new THREE.SphereBufferGeometry(0.5, 64, 64);

		// Materials
		const material = new THREE.MeshStandardMaterial();
		material.roughness = 0.2;
		material.metalness = 0.7;
		material.normalMap = normalTexture;
		material.color = new THREE.Color(0x292929);

		// Mesh
		const sphere = new THREE.Mesh(geometry, material);
		scene.add(sphere);

		// Lights

		const pointLight = new THREE.PointLight(0xffffff, 0.1);
		pointLight.position.set(2, 3, 4);
		scene.add(pointLight);

		const pointLight2 = new THREE.PointLight(0xff0000, 2);
		pointLight2.position.set(-1.86, 1, -1.65);
		pointLight2.intensity = 5;
		scene.add(pointLight2);

		const pointLight3 = new THREE.PointLight(0xe1ff, 2);
		pointLight3.position.set(2.13, -3, -1.98);
		pointLight3.intensity = 6.8;
		scene.add(pointLight3);

		/**
		 * Sizes
		 */
		const sizes = {
			width: window.innerWidth,
			height: window.innerHeight
		};

		window.addEventListener('resize', () => {
			// Update sizes
			sizes.width = window.innerWidth;
			sizes.height = window.innerHeight;

			// Update camera
			camera.aspect = sizes.width / sizes.height;
			camera.updateProjectionMatrix();

			// Update renderer
			renderer.setSize(sizes.width, sizes.height);
			renderer.setPixelRatio(Math.min(window.devicePixelRatio, 2));
		});

		/**
		 * Camera
		 */
		// Base camera
		const camera = new THREE.PerspectiveCamera(75, sizes.width / sizes.height, 0.1, 100);
		camera.position.x = 0;
		camera.position.y = 0;
		camera.position.z = 2;
		scene.add(camera);

		/**
		 * Renderer
		 */
		const renderer = new THREE.WebGLRenderer({
			canvas: canvas,
			alpha: true
		});
		renderer.setSize(sizes.width, sizes.height);
		renderer.setPixelRatio(Math.min(window.devicePixelRatio, 2));

		/**
		 * Animate
		 */
		document.addEventListener('mousemove', (event) => {
			mouseX = event.clientX - windowX;
			mouseY = event.clientY - windowY;
		});

		let mouseX = 0;
		let mouseY = 0;

		let targetX = 0;
		let targetY = 0;

		const windowX = window.innerWidth / 2;
		const windowY = window.innerHeight / 2;

		window.addEventListener('scroll', (event) => {
			sphere.position.y = window.scrollY * .001;
		})

		const clock = new THREE.Clock();

		const tick = () => {
			targetX = mouseX * 0.001;
			targetY = mouseY * 0.001;

			const elapsedTime = clock.getElapsedTime();

			// Update objects
			sphere.rotation.y = 0.5 * elapsedTime;

			sphere.rotation.y += 0.5 * (targetX - sphere.rotation.y);
			sphere.rotation.x += 0.05 * (targetY - sphere.rotation.x);
			sphere.position.z += -0.05 * (targetY - sphere.rotation.x);

			// Render
			renderer.render(scene, camera);

			// Call tick again on the next frame
			window.requestAnimationFrame(tick);
		};

		tick();
	}
</script>

<svelte:head>
	<title>Home</title>
</svelte:head>

<div>
	<div class="container">
		<h1>Nikolaj Jensen</h1>
	</div>
	<canvas class="webgl" />
	<section></section>
</div>
