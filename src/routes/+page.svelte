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

    .parallax {
        position: relative;
        height: 60vh;
        overflow: hidden;
        perspective: 1000px;
    }

    .layer {
        position: absolute;
        width: 100%;
        height: 100%;
        will-change: transform;
    }

    .layer:nth-child(1) {
        background: linear-gradient(121deg, rgba(4,0,18,1) 0%, rgba(0,17,85,1) 50%, rgba(36,0,57,1) 100%); ;
        z-index: 1;
        top: 0;
    }

    .layer:nth-child(2) {
        background: #7f7fff;
        z-index: 2;
        top: 20vh;
    }

    .layer:nth-child(3) {
        background: #7fff7f;
        z-index: 3;
        top: 40vh;
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

    .projets {
        display: flex;
        justify-content: center;
        align-items: center;
        height: 100vh;
        overflow: hidden;
        background: linear-gradient(152deg, rgba(32,1,54,1) 0%, rgba(0,30,147,1) 50%, rgba(38,0,255,1) 100%);
        margin: 0;
    }

    .card {
        width: 300px;
        height: 400px;
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
</style>

<div class="parallax">
    <div class="layer" data-speed="0.999999">
        <div class="dots">
            {#each dots as dot (dot.x)}
                <div
                        class="dot"
                        style="top: {dot.y}%; left: {dot.x}%; width: {dot.size}px; height: {dot.size}px;"
                ></div>
            {/each}
        </div>
    </div>
    <div class="layer" data-speed="0.666666"></div>
    <div class="layer" data-speed="0.333333"></div>
</div>

<div class="projets">
    <div class="card" on:mousemove={(event) => handleMouseMove(event, event.currentTarget)} on:mouseleave={(event) => resetCard(event.currentTarget)} role="presentation">
        <img src="//tokyocards.com/cdn/shop/files/image_b13157a3-a0cc-4323-9e28-af4ccec4de81.webp?v=1720805478&width=1946">
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
