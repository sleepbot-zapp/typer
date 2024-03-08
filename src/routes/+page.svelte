<h1 class="text-6xl font-extrabold text-center">
    Typer
</h1>
<br><br>
<div class="{isPlaying ? 'flex justify-around mr-[2rem] font-bold text-5xl text-red-400' : 'hidden'}">
    <h4>{wpm}</h4>
    <h4>{time}</h4>  
</div>
<br><br>
<div class="flex justify-center">
    <div class="flex justify-left w-[75rem] h-[20rem] border-orange-800">
        <div class="text-3xl font-bold leading-[4rem]">
            {#each data_nums as i}
                <span id="text{i}" class="{notPressed}">{data[i]}</span>
            {/each}
        </div>    
    </div>
</div>
  
<!-- <style lang="postcss"></style> -->

<script lang="ts">
// @ts-nocheck
    let n = 0;
    let isPlaying = false;
    let time = 59; 
    let wpm = 0;
    let worldsTyped = 0;
    let wordsCorrect = 0;
    let lettersNotCorrect = 0;
    let accuracy = 0;

    let data = "hello world and no way this is working as expected".split("");
    let data_nums = [...Array(data.length).keys()]
    const notPressed = "text-slate-600";
    // @ts-ignore
    const pressed = "text-cyan-100";
    // @ts-ignore
    const wrongPressed = "text-red-600"

    // @ts-ignore
    // @ts-ignore
    function start(e){
        if (!isPlaying){
            isPlaying = true;
            setInterval(() => {
                if (time > 0){
                    time--;
                    wpm = Math.floor(wordsCorrect/(1-(time/60))) - lettersNotCorrect;
                    lettersNotCorrect = 0;
                }
            }, 1000)
            worldsTyped++;
            if (e.key == data[n]){
                document.getElementById("text"+n).className = pressed;
            } else {
                document.getElementById("text"+n).className = wrongPressed;
                lettersNotCorrect++;
            }
            n++;
        } else if (isPlaying && time > 0){
            if (e.keyCode == 8){
                if (n !== 0){
                    n--;
                }
                document.getElementById("text"+n).className = notPressed;
            } else {
                if (e.key == data[n]){
                    document.getElementById("text"+n).className = pressed;
                    if (e.keyCode == 32){
                        worldsTyped++;
                        wordsCorrect++;
                    }
                } else {
                    document.getElementById("text"+n).className = wrongPressed;
                    lettersNotCorrect++;
                }
                if (n !== data.length){
                    n++;
                }
            }
        }
    }
</script>

<svelte:window on:keydown={start} />