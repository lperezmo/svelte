<!-- Grid.svelte -->

<script>
    export let width = 500;
    export let height = 500;
    export let step = 50;
    export let circles = [];
    export let maxhorizontallines = 0;
    export let maxverticallines = 0;
  </script>
  
  <style>
    .grid {
      position: absolute;
      left: 0;
      top: 0;
      width: 100%;
      height: 100%;
    }
    .grid-line {
      stroke: lightgray;
    }
    .line {
      stroke: darkmagenta;
      stroke-width: 3;
    }
    .annotation {
      font-size: 16px;
      fill: darkblue;
    }
  </style>
  
  <svg class="grid">
    {#each Array(Math.floor(height / step)) as _, y}
      <line
        class="grid-line"
        x1="0"
        y1="{y * step}"
        x2="{width}"
        y2="{y * step}"
      />
    {/each}
    {#each Array(Math.floor(width / step)) as _, x}
      <line
        class="grid-line"
        x1="{x * step}"
        y1="0"
        x2="{x * step}"
        y2="{height}"
      />
    {/each}
    {#each circles as circle1}
      {#each circles as circle2}
        {#if circle1.id !== circle2.id && Math.abs(circle1.x + circle1.radius / 2 - circle2.x - circle2.radius / 2) < step / 2 && maxhorizontallines < 1}
        <line
            class="line"
            x1="{circle1.x + circle1.radius / 2}"
            y1="{circle1.y + circle1.radius / 2}"
            x2="{circle2.x + circle2.radius / 2}"
            y2="{circle2.y + circle2.radius / 2}"
          />
          <text
            class="annotation"
            x="{(circle1.x + circle2.x + circle1.radius + circle2.radius) / 2}"
            y="{(circle1.y + circle2.y + circle1.radius + circle2.radius) / 2}"
          >
            {Math.floor(Math.abs(circle1.y - circle2.y))}
          </text>
        {/if}
        {#if circle1.id !== circle2.id && Math.abs(circle1.y + circle1.radius / 2 - circle2.y - circle2.radius / 2) < step / 2 && maxverticallines < 1}
            <!-- Max vertical lines +1 -->
            <script>
                maxverticallines += 1;
            </script>
            <line
            class="line"
            x1="{circle1.x + circle1.radius / 2}"
            y1="{circle1.y + circle1.radius / 2}"
            x2="{circle2.x + circle2.radius / 2}"
            y2="{circle2.y + circle2.radius / 2}"
          />
          <text
            class="annotation"
            x="{(circle1.x + circle2.x + circle1.radius + circle2.radius) / 2}"
            y="{(circle1.y + circle2.y + circle1.radius + circle2.radius) / 2}"
          >
            {Math.floor(Math.abs(circle1.x - circle2.x))}
          </text>
        {/if}
      {/each}
    {/each}
  </svg>
  
  

<!-- 
<script>
    export let width = 500;
    export let height = 500;
    export let step = 50;
    export let circles = [];
  </script>
  
  <style>
    .grid {
      position: absolute;
      width: 100%;
      height: 100%;
    }
    .grid-line {
      stroke: lightskyblue;
    }
    .line {
      stroke: red;
      stroke-width: 3;
    }
    .annotation {
      font-size: 16px;
      fill: red;
    }
  </style>
  
  <svg class="grid">
    {#each Array(Math.floor(height / step)) as _, y}
      <line
        class="grid-line"
        x1="0"
        y1="{y * step}"
        x2="{width}"
        y2="{y * step}"
      />
    {/each}
    {#each Array(Math.floor(width / step)) as _, x}
      <line
        class="grid-line"
        x1="{x * step}"
        y1="0"
        x2="{x * step}"
        y2="{height}"
      />
    {/each}
    {#each circles as circle1}
      {#each circles as circle2}
        {#if circle1.id !== circle2.id}
          <line
            class="line"
            x1="{circle1.x + circle1.radius / 2}"
            y1="{circle1.y + circle1.radius / 2}"
            x2="{circle2.x + circle2.radius / 2}"
            y2="{circle2.y + circle2.radius / 2}"
          />
          <text
            class="annotation"
            x="{(circle1.x + circle2.x + circle1.radius + circle2.radius) / 2}"
            y="{(circle1.y + circle2.y + circle1.radius + circle2.radius) / 2}"
          >
            {Math.floor(Math.sqrt(Math.pow(circle1.x - circle2.x, 2) + Math.pow(circle1.y - circle2.y, 2)))}
          </text>
        {/if}
      {/each}
    {/each}
  </svg>
   -->
<!-- <script>
    export let width = 500;
    export let height = 500;
    export let step = 50;
  </script>
  
  <style>
    .grid {
      position: relative;
      width: 100%;
      height: 100%;
    }
    .grid-line {
      stroke: lightgray;
    }
  </style>
  
  <svg class="grid">
    {#each Array(Math.floor(height / step)) as _, y}
      <line
        class="grid-line"
        x1="0"
        y1="{y * step}"
        x2="{width}"
        y2="{y * step}"
      />
    {/each}
    {#each Array(Math.floor(width / step)) as _, x}
      <line
        class="grid-line"
        x1="{x * step}"
        y1="0"
        x2="{x * step}"
        y2="{height}"
      />
    {/each}
  </svg> -->
  