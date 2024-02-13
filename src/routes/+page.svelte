<script>
    import {afterUpdate} from 'svelte';
    import {fade} from 'svelte/transition';
    import milk from '$lib/assets/milk.png';
    import mocha from '$lib/assets/mocha.png';
    import kiss from '$lib/assets/kiss.gif';
    import music from '$lib/assets/music.mp3';
    import '$lib/styles/animation.css';
    import '@fontsource/balsamiq-sans';
    import "@fontsource/great-vibes";

    let isInitialized = false;

    let stage = 1;

    function goToNextStage() {
        stage += 1;
    }

    let unusedPhrases = [
        "Why not?",
        "Misclicked?",
        "Are you sure?",
        "Really sure?",
        "Think again!",
        "Last chance!",
        "Surely not?",
        "You might regret this!",
        "Give it another thought!",
        "Are you certain?",
        "This could be a mistake!",
        "Have a heart!",
        "Don't be so cold!",
        "Change of heart?",
        "Wouldn't you reconsider?",
        "Is that your final answer?",
        "You're breaking my heart...",
        "Please?",
        "Pretty please?",
        "I won't give up.",
    ];

    let noCount = 0;
    $: unusedIndex = ((noCount - 1) % (unusedPhrases.length));

    function saidNo() {
        noCount += 1;
    }

    let audio = null;
    let audioButtonState = "Play";

    function playMusic() {
        audio = new Audio(music);
        audio.play();
        audioButtonState = "Playing";
    }

    function pauseMusic() {
        audio.pause();
        audioButtonState = "Play";
    }

    afterUpdate(() => {
        isInitialized = true;
    });
    
</script>

<div class="bg-container">
    <img class="left-pic" alt="Mocha" src={mocha}/>
    <img class="right-pic" alt="Milk" src={milk}/>
</div>

<div class="flex-container">
    {#if isInitialized && stage === 1}
        <div class="text-container" in:fade={{delay: 500, duration: 1000}} out:fade={{duration:1000}}>
            <h1>Hey</h1>
        </div>
    {:else if stage === 2}
        <div class="text-container" in:fade={{delay: 500, duration: 1000}} out:fade={{duration:1000}}>
            <p>It's almost</p>
            <div class="row-container"><h1 class="special-text">Valentine's</h1><p class="p-pd-left-special">day</p></div>

        </div>
    {:else if stage === 3}
        <div class="text-container" in:fade={{delay: 500, duration: 1000}} out:fade={{duration:1000}}>
            <h1>And I'd just</h1>
            <h1>like to ask...</h1>
        </div>
    {:else if stage === 4}
        <div class="text-container" in:fade={{delay: 500, duration: 1000}} out:fade={{duration:1000}}>
            <h2>Will you be my</h2>
            <div class="row-container">
                <h1 class="special-text">Valentine</h1>
                <h1 class="h1-pd-left-special">?</h1>
            </div>
            <div class="row-container">
                <button class="yes-button" on:click={goToNextStage}>Yes</button>
                <button class="no-button" on:click={saidNo}>{noCount === 0 ? 'No' : unusedPhrases[unusedIndex]}</button>
            </div>
        </div>
    {:else if stage === 5}
        <div class="text-container" in:fade={{delay: 500, duration: 1000}} out:fade={{duration:1000}}>
            <img class="last-pic" alt="Kisses" src={kiss}/>
            <h1>Yay!</h1>
            <h1>I love you!!!</h1>
            <div class="emoji heart1">❤️</div>
        </div>
    {/if}

    {#if isInitialized && stage <= 3} 
        <button class="styless-button" on:click={goToNextStage} in:fade={{delay: 1000, duration: 1000}} out:fade={{duration:1000}}>Press here to continue...</button>
    {/if}
    {#if stage === 5 && noCount !== 0} 
        <p class="special-p" in:fade={{delay: 1000, duration: 1000}} out:fade={{duration:1000}}>You said no {noCount} times though...</p>
    {/if}

    {#if stage === 5} 
        <button class={"audio-button " + audioButtonState.toLowerCase()} on:click={audioButtonState === "Playing" ? pauseMusic : playMusic}>{audioButtonState}: Can't Help Falling in Love...</button>
    {/if}
</div>

<style>
    :global(body) {
        margin: 0;
        padding: 0;
        box-sizing: border-box;
        background: #e19aa8; 
        -webkit-animation: bg-change 5s linear infinite alternate both;
        animation: bg-change 5s linear infinite alternate both;
        overflow: hidden;
    }

    .flex-container {
        height: 100dvh;
        width: 100dvw;
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: center;
    }

    img {
        -webkit-filter: drop-shadow(6px 6px 0 white)
                drop-shadow(-6px 6px 0 white)
                drop-shadow(6px -6px 0 white)
                drop-shadow(-6px -6px 0 white);

        filter: drop-shadow(6px 6px 0 white)
                drop-shadow(-6px 6px 0 white)
                drop-shadow(6px -6px 0 white)
                drop-shadow(-6px -6px 0 white);
        
        z-index: 100;
    }

    .right-pic {
        position: absolute;
        bottom: 0;
        right: 0;
        object-fit: contain;
        width: clamp(200px, 20vw, 300px)
    }

    .left-pic {
        position: absolute;
        bottom: 0;
        left: 0;
        object-fit: contain;
        width: clamp(200px, 20vw, 300px)
    }

    .last-pic {
        object-fit: contain;
        width: clamp(200px, 20vw, 300px);
        padding-right: 50px;
        padding-bottom: 20px;
    }

    .emoji {
        padding-top: 15%;
        font-size: clamp(4rem, 10vw ,6rem);
        padding-bottom: 40%;
	    -webkit-animation: wobble-hor-bottom 5s infinite both;
	    animation: wobble-hor-bottom 5s infinite both;
    }

    .text-container {
        position: absolute;
        display: flex;
        flex-direction: column;
        align-items: center;
        
        color: white;
        font-family: 'Balsamiq Sans', system-ui;
        white-space: normal;
        line-height: 0;
        text-align: center;
    }

    .row-container {
        display: flex;
        flex-direction: row;
        justify-content: center;
        align-items: center;
        flex-wrap: wrap;
    }

    h1 {
        font-size: clamp(3rem, 10vw, 6rem);
    }

    .special-text {
        color: #9a3848;
        font-family: 'Great Vibes', cursive;
        font-size: clamp(4rem, 10vw, 7rem);
    }

    h2{
        font-size: clamp(2rem, 8vw, 4rem);
    }

    p {
        display: block;
        color: white;
        font-family: 'Balsamiq Sans', system-ui;
        font-weight: 200;
        font-size: clamp(1.5rem, 8vw, 2rem);
        text-align: center;
    }

    .styless-button {
        position: absolute;
        bottom: 20vh;
        background: none;
        border: none;
        cursor: pointer;
        color: white;
        font-family: 'Balsamiq Sans', system-ui;
        font-size: clamp(1.5rem, 4vw, 2rem);
    }

    .special-p {
        position: absolute;
        bottom: 17vh;
        background: none;
        border: none;
        color: white;
        font-family: 'Balsamiq Sans', system-ui;
        font-size: clamp(1rem, 3vw, 1.5rem);
    }

    .special-p-top {
        position: absolute;
        top: 0.5vh;
        background: none;
        border: none;
        color: white;
        font-family: 'Balsamiq Sans', system-ui;
        font-size: clamp(1rem, 3vw, 1.5rem);
    }

    .h1-pd-left-special {
        padding-left: clamp(10px, 1vw, 15px);
    }

    .p-pd-left-special {
        padding-left: clamp(20px, 1vw, 25px);
    }

    .yes-button {
        width: 200px;
        height: 100px;
        background-color: #6b8e23;
        color: white;
        padding: 8px;
        margin: 8px;
        cursor: pointer;
        border-radius: 16px;
        border: 6px solid white;
        font-family: 'Balsamiq Sans', system-ui;
        font-size: 1.5rem;
        transition: background-color 0.3s ease;
    }

    .yes-button:hover,
    .audio-button.playing:hover {
        background-color: #556b2f;
    }

    .no-button {
        width: 200px;
        height: 100px;
        background-color: #c35b6a;
        color: white;
        padding: 8px;
        cursor: pointer;
        margin: 8px;
        border-radius: 16px;
        border: 6px solid white;
        font-family: 'Balsamiq Sans', system-ui;
        font-size: 1.5rem;
        transition: background-color 0.3s ease;
    }

    .no-button:hover,
    .audio-button.play:hover{
        background-color: #a44754;
    }

    .audio-button {
        position: absolute;
        top: 1vh;
        width: 300px;
        height: 50px;
        color: white;
        padding: 8px;
        margin: 8px;
        cursor: pointer;
        border-radius: 16px;
        border: 6px solid white;
        font-family: 'Balsamiq Sans', system-ui;
        font-size: 1rem;
        transition: background-color 0.3s ease;
    }

    .audio-button.play {
        background-color: #c35b6a;
    }

    .audio-button.playing {
        background-color: #6b8e23;
    }
</style>