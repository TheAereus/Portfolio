<script>
    import {onMount} from "svelte";

    function handleMouseMove(event, element) {
        // Pos curseur
        const rect = element.getBoundingClientRect();
        const x = event.clientX - rect.left;
        const y = event.clientY - rect.top;

        // Calcul rotations
        const rotateX = ((y / rect.height) - 0.5) * -20;
        const rotateY = ((x / rect.width) - 0.5) * 30;

        // Transformations
        element.style.transform = `perspective(1000px) rotateX(${rotateX}deg) rotateY(${rotateY}deg)`;
    }

    async function resetCard(element) {
        await new Promise(resolve => setTimeout(resolve, 1000));
        element.style.transform = "perspective(1000px) rotateX(0deg) rotateY(0deg)";
    }

    let layers;

    function handleScroll() {
        const scrollPosition = window.scrollY;
        layers.forEach((layer, index) => {
            const speed = layer.dataset.speed;
            layer.style.transform = `translateY(${scrollPosition * speed}px)`;
        });
    }

    let dots = [];

    function generateDots() {
        dots = [];
        const numberOfDots = 200;
        for (let i = 0; i < numberOfDots; i++) {
            const x = Math.random() * 100;
            const y = Math.random() * 100;
            const size = Math.random() * 8;
            dots.push({ x, y, size });
        }
    }

    onMount(() => {
        layers = document.querySelectorAll("[data-speed]");
        window.addEventListener("scroll", handleScroll);
        generateDots();
        return () => {
            window.removeEventListener("scroll", handleScroll);
        };
    });
</script>

<style>

    :global(body) {
        background: linear-gradient(152deg, rgba(32,1,54,1) 0%, rgba(0,30,147,1) 50%, rgba(38,0,255,1) 100%);
        font-family: 'Open Sans', sans-serif;
    }

    .parallax {
        background: radial-gradient(circle at 70% 100%, rgba(29,49,195,1) 13%, rgba(0,12,112,1) 74%);
        position: relative;
        height: 50vh;
        overflow: hidden;
        perspective: 1000px;
    }

    .layer {
        position: absolute;
        width: 100%;
        height: 100%;
        will-change: transform;
    }

    .layer:nth-child(2) {
        background: #7f7fff;
        z-index: 2;
        top: 30vh;
    }

    .dots {
        position: absolute;
        top: 0;
        left: 0;
        width: 100vw;
        height: 100%;
        pointer-events: none;
        z-index: 0;
    }

    .dot {
        position: absolute;
        background-color: white;
        border-radius: 50%;
        opacity: 0.6;
        animation: move-dots 10s infinite linear;
        transition: transform 0.3s ease;
    }

    @keyframes move-dots {
        0% {
            transform: scale(1);
        }
        50% {
            transform: scale(1.2);
        }
        100% {
            transform: scale(1);
        }
    }

    .photoEtNom{
        display: flex;
        margin-top: -15vh;
        flex-direction: column;
        align-items: center;
        font-size: 50px;
    }

    .photoEtNom img {
        position: relative;
        width: 20vw;
        height: 20vw;
        border-radius: 50%;
    }

    .photoEtNom p {
        position: relative;
    }

    .photoEtNom p:nth-child(3) {
        margin-top: -5vh;
        font-size: 25px;
        text-align: center;
    }

    .projets {
        display: flex;
        justify-content: center;
        align-items: center;
        overflow: hidden;
        margin: 0;
    }

    .card {
        min-width: 300px;
        min-height: 400px;
        background: white;
        border-radius: 15px;
        transition: transform 0.1s ease-out;
        display: flex;
        justify-content: center;
        align-items: center;
        font-size: 1.5rem;
        font-weight: bold;
        text-align: center;
        color: #333;
        margin: 10px;
    }

    .titre {
        position: relative;
        text-align: center;
    }

    @media (max-width: 1000px) {
        .photoEtNom img {
            position: relative;
            top: -15vh;
            width: 50vw;
            height: 50vw;
            border-radius: 50%;
        }
    }
</style>

<div class="parallax">
    <div class="dots">
        {#each dots as dot (dot.x)}
            <div class="dot" style="top: {dot.y}%; left: {dot.x}%; width: {dot.size}px; height: {dot.size}px;"></div>
        {/each}
    </div>
    <div class="layer" data-speed="0.3"></div>
</div>

<div class="photoEtNom">
    <img src="/images/Portfolio_photo.jpg" alt="Camille Okubo">
    <p>Camille Okubo</p>
    <p>Étudiant en BUT Informatique parcours réalisation d'applications</p>
</div>

<h1 class="titre">Projets</h1>

<div class="projets">
    <div class="card" on:mousemove={(event) => handleMouseMove(event, event.currentTarget)} on:mouseleave={(event) => resetCard(event.currentTarget)} role="presentation">
        Projet
    </div>
    <div class="card" on:mousemove={(event) => handleMouseMove(event, event.currentTarget)} on:mouseleave={(event) => resetCard(event.currentTarget)} role="presentation">
        Projet
    </div>
    <div class="card" on:mousemove={(event) => handleMouseMove(event, event.currentTarget)} on:mouseleave={(event) => resetCard(event.currentTarget)} role="presentation">
        Projet
    </div>
    <div class="card" on:mousemove={(event) => handleMouseMove(event, event.currentTarget)} on:mouseleave={(event) => resetCard(event.currentTarget)} role="presentation">
        Projet
    </div>
</div>
