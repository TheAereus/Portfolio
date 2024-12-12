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

    onMount(() => {
        layers = document.querySelectorAll("[data-speed]");
        window.addEventListener("scroll", handleScroll);

        return () => {
            window.removeEventListener("scroll", handleScroll);
        };
    });
</script>

<style>
    .parallax {
        position: relative;
        height: 70vh;
        overflow: hidden;
        perspective: 1px;
    }

    .layer {
        position: absolute;
        width: 100%;
        height: 100%;
        will-change: transform;
        bottom: 0;
    }

    .container {
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
    <defs>
        <path id="gentle-wave" d="M-160 44c30 0 58-18 88-18s 58 18 88 18 58-18 88-18 58 18 88 18 v44h-352z" />
    </defs>
    <div class="layer" data-speed="0.999999"><use xlink:href="#gentle-wave" x="48" y="-1" /></div>
    <div class="layer" data-speed="0.666666"><use xlink:href="#gentle-wave" x="48" y="2" /></div>
    <div class="layer" data-speed="0.333333"><use xlink:href="#gentle-wave" x="48" y="7" /></div>
</div>

<div class="container">
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
