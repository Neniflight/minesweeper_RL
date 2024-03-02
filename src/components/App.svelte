<script>
  // Write your JS here, or import other files
  import Scroller from "@sveltejs/svelte-scroller";
  let count, index, offset, progress;
  let width, height;

  let hoverIndex = -1; // Index of the hovered image, -1 means no hover
  const images = [
    { src: "ex1.jpg", alt: "Minesweeper 1", caption: "Not enough information. You have to look at a different place." },
    { src: "ex2.png", alt: "Minesweeper 2", caption: "It can only be the top right because the “2” tiles have too many options to pick from." },
    { src: "ex3.jpg", alt: "Minesweeper 3", caption: "“1” tiles indicates the bottom left and top right flags are bombs. Since there's only one. The bottom right bomb is the only option left." }
  ];

  const hoveredImages = [
    { src: "ex1.jpg", alt: "Minesweeper 1 (Hovered)", caption: "Caption 1 (Hovered)" },
    { src: "ex2flag.png", alt: "Minesweeper 2 (Hovered)", caption: "Caption 2 (Hovered)" },
    { src: "ex3flag.jpg", alt: "Minesweeper 3 (Hovered)", caption: "Caption 3 (Hovered)" }
  ];

</script>

<main>
  <Scroller
  top={0.0}
  bottom={1}
  threshold={0.5}
  bind:count
  bind:index
  bind:offset
  bind:progress
>
  <div class="background" slot="background" bind:clientWidth={width} bind:clientHeight={height}>

    <div class="progress-bars">
      <p>current section: <strong>{index + 1}/{count}</strong></p>
      <progress value={count ? (index + 1) / count : 0} />

      <p>total progress</p>
      <progress value={progress || 0} />
    </div>
  </div>

  <div class="foreground" slot="foreground">
    <section>
      <p class = "section-text"> Minesweeper Reinforcement Learning Explained</p>
      <div class="image-row">
        <img src="bomb.jpg" alt="Image 1" />  
        <img src="title.jpg" alt="Image 2" />
        <img src="flag.jpg" alt="Image 3" />
      </div>
    </section>
    <section>
      <p class = "section-text">What is Minesweeper?</p>
      <img src="fullboardex.jpg" alt="Image 3" />
      <div class="caption-box">
        <p>It is a game where you 
          find a certain number 
          of bombs on a board 
          based on clues on the 
          numbers on the board</p>
      </div>
    </section>
    <section>
      <div class="image-row">
        <img src="bomb.jpg" alt="Image 1" />
        <img src="abc.png" alt="Image 2" />
        <img src="flag.jpg" alt="Image 3" />
      </div>
      <div class="image-row">
        <div class="caption-box">
          <p>This is the bomb you are finding</p>
        </div>      
        <img src="abc.png" alt="Image 2" />
        <img src="Empty.png" alt="Image 2" />
        <img src="abc.png" alt="Image 2" />

        <div class="caption-box">
          <p>These flags indicate which tiles 
            you think are bombs</p>
        </div>
      </div>
      <div class="image-caption">
        <img src="cleanex.jpg" alt="Image 6" class="small-image" />
        <div class="caption-box">
          <p>Numbers indicate how many bombs are around that tile.
            In this example, there are 2 bombs around the middle tile and the rest are safe so 2 is shown.
          </p>
        </div>
      </div>
    </section>
    <section>
      <p class = "section-text">Examples!</p>
      <div class="caption-box">
        <p> Where are the bombs? Try to solve them yourself then hover over for the answerg
        </p>
      </div>
     
      <div class="image-row2">
        {#each images as image, index}
          <div 
            class="image-container" 
            on:mouseenter={() => hoverIndex = index} 
            on:mouseleave={() => hoverIndex = -1}
          >
            {#if hoverIndex === index}

              <img src={hoveredImages[index].src} alt={hoveredImages[index].alt} class="hovered-image" />
              <div class="caption-box2">
                <p>{image.caption}</p>
              </div>
            {:else}
              <img src={image.src} alt={image.alt} class="normal-image" />
              
            {/if}
          </div>
        {/each}
      </div>

    </section>
  </div>
</Scroller>
</main>

<style>
  /* Write your CSS here */
  .background {
    width: 100%;
    height: 100vh;
    position: relative;
    outline: green solid 3px;
  }

  .foreground {
    width: 50%;
    margin: 0 auto;
    height: auto;
    position: relative;
    outline: red solid 3px;
  }

  .progress-bars {
    position: absolute;
    background: rgba(170, 51, 120, 0.2) /*  40% opaque */;
    visibility: visible;
  }

  section {
    height: 80vh;
    background-color: rgba(0, 0, 0, 0.2); /* 20% opaque */
    /* color: white; */
    outline: magenta solid 3px;
    text-align: center;
    max-width: 900px; /* adjust at will */
    color: black;
    padding: 1em;
    margin: 0 0 2em 0;
    display: flex;
    flex-direction: column;
    align-items: center; /* Center horizontally */
    text-align: center; /* Center text horizontally */
  }

  .image-row {
  display: flex;
  justify-content: space-around;
  align-items: center; /* Align items vertically in the center */
}

.image-row img {
  height: auto;
}

.image-row img:nth-child(1) {
  max-width: 15%; /* Width of the first image */
}

.image-row img:nth-child(2) {
  max-width: 70%; /* Width of the second image */
}

.image-row img:nth-child(3) {
  max-width: 15%; /* Width of the third image */
}

.section-text{
  font-size: 24px; /* Adjust font size as needed */
  font-weight: bold;
}
img {
    max-width: 100%;
    height: auto;
  }

  .image-caption {
    text-align: center;
    margin-top: 10px;
  }

  .small-image {
  max-width: 200px; /* Adjust maximum width as needed */
  height: auto;
  }


  .caption-box {
  background-color: white;
  padding: 10px;
  max-width: 300px; /* Adjust maximum width as needed */
  word-wrap: break-word;
  display: flex;
  align-items: center; /* Center vertically */
  justify-content: center; /* Center horizontally */
  font-family:  'Comic Sans MS', 'Chalkboard SE', 'Comic Neue', sans-serif;; /* Change font as needed */
  }


  .image-row2 {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-top: 20px; /* Add some space between rows */

  }

  .image-container {
    position: relative;
    margin: 0 10px; /* Add some space between images */

  }

  .normal-image,
  .hovered-image {
    width: 200px; /* Adjust image width as needed */
    height: auto;
    transition: transform 0.5s ease;
  }

  .hovered-image {
    transform: scale(1.1); /* Scale up the image on hover */
  }

  .caption-box2 {
    position: absolute;
    bottom: -100px;
    left: 0;
    right: 0;
    text-align: center;
    background-color: white;
    padding: 5px;
    border: 1px solid black;
    opacity: 0; /* Initially hidden */
    transition: opacity 0.3s ease;
  }

  .image-container:hover .caption-box2 {
    opacity: 1; /* Show caption box on hover */
  }
</style>
