<script>
    function handleMouseMove(event, element) {
        const rect = element.getBoundingClientRect();
        const x = event.clientX - rect.left; // Position X du curseur sur la carte
        const y = event.clientY - rect.top;  // Position Y du curseur sur la carte

        // Calculer les rotations en fonction de la position du curseur
        const rotateX = ((y / rect.height) - 0.5) * -20; // Inclinaison verticale
        const rotateY = ((x / rect.width) - 0.5) * 30;  // Inclinaison horizontale

        // Appliquer les transformations 3D
        element.style.transform = `perspective(1000px) rotateX(${rotateX}deg) rotateY(${rotateY}deg)`;
    }

    async function resetCard(element) {
        await new Promise(resolve => setTimeout(resolve, 1000));
        element.style.transform = "perspective(1000px) rotateX(0deg) rotateY(0deg)";
    }
</script>

<style>
    .container {
        display: flex;
        justify-content: center;
        align-items: center;
        height: 100vh;
        overflow: hidden;
        background: linear-gradient(152deg, rgba(131,58,180,1) 0%, rgba(253,29,29,1) 50%, rgba(252,176,69,1) 100%);
        margin: 0;
    }

    .card {
        width: 300px;
        height: 200px;
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

<div class="container">
    <div class="card"on:mousemove={(event) => handleMouseMove(event, event.currentTarget)} on:mouseleave={(event) => resetCard(event.currentTarget)} role="presentation">
        <img src="//tokyocards.com/cdn/shop/files/image_b13157a3-a0cc-4323-9e28-af4ccec4de81.webp?v=1720805478&width=1946">
    </div>
    <div class="card"on:mousemove={(event) => handleMouseMove(event, event.currentTarget)} on:mouseleave={(event) => resetCard(event.currentTarget)} role="presentation">
        Projet
    </div>
    <div class="card"on:mousemove={(event) => handleMouseMove(event, event.currentTarget)} on:mouseleave={(event) => resetCard(event.currentTarget)} role="presentation">
        Projet
    </div>
    <div class="card"on:mousemove={(event) => handleMouseMove(event, event.currentTarget)} on:mouseleave={(event) => resetCard(event.currentTarget)} role="presentation">
        Projet
    </div>
</div>
