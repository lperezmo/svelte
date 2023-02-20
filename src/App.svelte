<!-- App.svelte -->

<script>
	// Import onMount to add event listeners
	import { onMount } from 'svelte';
	import Grid from './Grid.svelte';
	
	// Array of circles & variable to show if circle is selected
	let circles = [];
	let selectedCircle = null;
	
	function addCircle(event) {
	const newCircle = {
		id: Date.now(),
		x: event.clientX,
		y: event.clientY,
		radius: 50,
		height: 50,
		zIndex: circles.length
	};
	circles = [...circles, newCircle] 
	// .sort((a, b) => b.zIndex - a.zIndex);
	}

	// // Add circle when clicking anywhere on main surface
	// function addCircle(event) {
	// const newCircle = {
	// 	id: Date.now(),
	// 	x: event.clientX,
	// 	y: event.clientY,
	// 	radius: 50,
	// 	height: 50
	// };
	// circles = [...circles, newCircle];
	// }

	// Select circle when clicked
	function selectCircle(circle) {
		if (selectedCircle === circle) {
      // if the same circle is clicked twice, deselect it
      selectedCircle = null;
		} else {
		selectedCircle = circle;
		}
	}
	
	// Change height of circle with input
	function editCircle(event) {
	  selectedCircle.height = parseInt(event.target.value, 10);
	  circles = [...circles];
	}

	// Change radius of circle with mouse wheel
	function editCircleRadius(event) {
		const amount = event.deltaY < 0 ? 5 : -5;
		selectedCircle.radius += amount;
		selectedCircle.radius = Math.max(20, selectedCircle.radius); // don't let the radius go below 20
		circles = [...circles];
		const circleElement = document.getElementById(selectedCircle.id);
		circleElement.style.width = '${selectedCircle.radius}px';
		circleElement.style.height = '${selectedCircle.radius}px';
	}

	// Drag circles
	function handleDrag(event) {
	  if (selectedCircle) {
		selectedCircle.x = event.clientX;
		selectedCircle.y = event.clientY;
		circles = [...circles];
	  }
	}
  
	// Log circles to console (for debugging)
	function logCircles() {
	  console.log(circles);
	}

	// Deselect circle when escape key is pressed
	function deselectCircle(event) {
    if (event.key === 'Escape') {
      selectedCircle = null;
    	}
	}
	
	// Delete circle when delete key is pressed
	function handleDelete(event) {
    if (event.key === 'Delete') {
		// Find index of selected circle
		const index = circles.indexOf(selectedCircle);
		// If circle is selected, delete it
		if (index !== -1) {
			// delete circles without using splice
			circles = circles.filter((circle) => circle !== selectedCircle);

			// Deselect circle
			selectedCircle = null;
			}
		}
	}

	// Event listeners
	onMount(() => {
    window.addEventListener('mousemove', handleDrag);
    window.addEventListener('keydown', deselectCircle);
	window.addEventListener('keydown', handleDelete);
    window.addEventListener('wheel', editCircleRadius);
  });


  </script>
  
  <!-- Pretty styling for our Hess circles -->
  <style>
	main {
	  height: 50vh;
	  width: 100vw;
	  display: flex;
	  flex-direction: column;
	}
	.circle {
	  position: absolute;
	  border-radius: 50%;
	  background-color: lightblue;
	  width: 100px;
	  height: 100px;
	  display: flex;
	  justify-content: center;
	  align-items: center;
	  font-size: 20px;
	}
	.circle-info {
		position: absolute;
		align-items: left;
		top: -100px;
		left: -5px;
	}
	.circle-height {
		position: absolute;
		align-items: left;
		top: -50px;
		left: -5px;
		width: 100px;
	}
	/* .h1 {
		display: flex;
		flex-direction: column;
		text-align: center;
		font-size: 30px;
	} */
  </style>
  
  <header>
	<h1>Walkways/Stairs Configurator for Tank Batteries</h1>
  </header>

  <!-- Make whole surface clickable -->
  <main on:click={addCircle}>
	<!-- Add grid -->
	<Grid width={window.innerWidth} height={window.innerHeight} step={50} circles={circles} />
	<!-- <Grid width={window.innerWidth} height={window.innerHeight} /> -->
	<!-- Add circles -->
	{#each circles as circle}
	  <div
		class="circle"
		style="top: {circle.y}px; left: {circle.x}px; height: {circle.radius}px; width: {circle.radius}px; background-color: {selectedCircle === circle ? 'lightgreen' : 'lightblue'};"
		on:click|stopPropagation={() => selectCircle(circle)}
	  >
	  <div>
		<!-- If radius is >65 show the radius inside circle -->
		{#if circle.radius > 65}
			<p> r = {circle.radius} ft</p>
		{/if}
		</div>
	  	<!-- Show height and input to change height at the top -->
		<div>
			<div class="circle-info">
				<p>Height</p>
			</div>
		  <input
			class="circle-height"
			type="number"
			bind:value={circle.height}
			on:input={editCircle}
        />
		</div>
	  </div>
	{/each}
  </main>
  
  <!-- Current location of all circles, height, and radius -->
  <footer>
	<p>Current location and height of all circles:</p>
	<ul>
	  {#each circles as circle}
      <li>({circle.x}, {circle.y}) - Height: {circle.height}px - Radius: {circle.radius}px</li>	  {/each}
	</ul>
  </footer>
  


<!-- OLD CODE -->

<!-- function deleteCircle() {
	const index = circles.indexOf(selectedCircle);
	if (index !== -1) {
	  circles.splice(index, 1);
	  selectedCircle = null;
	}
  } -->