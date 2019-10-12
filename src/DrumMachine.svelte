<script>
  import DrumField from './DrumField.svelte'
  import {Howl, Howler} from 'howler';

  const sounds = {
    kick: new Howl({
      src: "/audio/kick.mp3",
      sprite: {
        cropped: [50, 300]
      }
    }),
    clap: new Howl({
      src: "/audio/clap.mp3",
      sprite: {
        cropped: [50, 300]
      },
      volume: 0.8
    }),
    liv: new Howl({
      src: "/audio/liv.mp3",
      sprite: {
        cropped: [250, 300]
      }
    }),
    bongo: new Howl({
      src: "/audio/bongo.mp3",
      sprite: {
        cropped: [250, 300]
      }
    })
  };

  export let height;
  export let width;
  export let bpm;
  export let useBothDirections = true

  $: console.log("h,w,bpm:", height, width, bpm)
  $: console.log("useBothDirections:", useBothDirections)

  $: beatgrid = new Array(height).fill(false).map(() => new Array(width).fill(null).map(() => {
    let o = {};
    for(let sound in sounds){
      o[sound] = false;
    }
    return o;
  }))
  $: highlightgrid = new Array(height).fill(null).map(() => new Array(width).fill(null))
  let currentStep = 0
  let looper

  $: {clearInterval(looper);
      looper = setInterval(() => {
        let way1 = getQuotientAndRemainderOfStep(currentStep,width)
        let way2 = getQuotientAndRemainderOfStep(currentStep,height)
        for(let sound in sounds) {
          if(beatgrid[way1.q][way1.r][sound] || (useBothDirections && beatgrid[way2.r][way2.q][sound])) {
            sounds[sound].play("cropped")
          }
        }
        // Reset all highlights and set the new ones
        highlightgrid.forEach((r) => r.forEach((val, index) => { if(val) { r[index] = false; } }))
        highlightgrid[way1.q][way1.r] = true;
        if(useBothDirections) highlightgrid[way2.r][way2.q] = true;
        currentStep = (currentStep + 1) % (height * width);
      }, 30000/bpm)
    }

  function toggleField(i,j,event){
    let key = event.detail.sound
    beatgrid[i][j][key] = !beatgrid[i][j][key];
  }
  function gridToString(mat){
    return mat.map((r) => r.map((v) => v ? "X" : "O")).join("\n")
  }
  function getQuotientAndRemainderOfStep(step,ax){
    let quotient = Math.floor(step/ax);
    let remainder = step % ax
    return {q :quotient, r: remainder}
 }
</script>

<style>
  .drum-machine {
    position: relative;
    display: grid;
    grid-template-rows: auto;
    grid-gap: 10px;
  }
</style>

<div class="drum-machine" style={"grid-template-columns: " + ("1fr ".repeat(width))}>
  {#each beatgrid as row,i}
    {#each row as value,j}
      <DrumField
        state={value}
        sounds={Object.keys(sounds)}
        height={height}
        highlight={highlightgrid[i][j]}
        on:click={(event) => toggleField(i,j,event)}
        />
    {/each}
  {/each}
</div>
