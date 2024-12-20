<script>
    import {onMount} from "svelte";

    function handleMouseMove(event, element) {
        const rect = element.getBoundingClientRect();
        const x = event.clientX - rect.left;
        const y = event.clientY - rect.top;

        const rotateX = ((y / rect.height) - 0.5) * -20;
        const rotateY = ((x / rect.width) - 0.5) * 30;

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

    let activeSkill = "langues";
    let buttonRefs = [];
    let selectorStyle = "";

    function setActiveSkill(skill, index) {
        activeSkill = skill;
        const button = buttonRefs[index];
        updateSelector(index);
    }

    function updateSelector(index) {
        const button = buttonRefs[index];
        if (button) {
            const { offsetTop } = button;
            selectorStyle = `top: ${offsetTop}px;`;
        }
    }

    onMount(() => {
        updateSelector(0);
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
        background: rgb(100,33,235);
        background: linear-gradient(135deg, rgba(100,33,235,1) 5%, rgba(29,49,195,1) 16%, rgba(0,12,112,1) 45%, rgba(100,33,235,1) 90%);
        font-family: 'Open Sans', sans-serif;
        overflow-x: hidden;
    }

    .parallax {
        background: radial-gradient(circle at 70% 100%, rgba(29,49,195,1) 13%, rgba(0,12,112,1) 74%);
        position: relative;
        height: 70vh;
        overflow: hidden;
        perspective: 1000px;
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
        opacity: 0.9;
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

    .delimiter{
        display: block;
        width: 100vw;
        background: white;
        height: 5px;
    }

    .photoEtNom{
        display: flex;
        margin-top: -15vw;
        flex-direction: column;
        align-items: center;
        font-size: 50px;
        color: white;
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

    .photoEtNom p:nth-child(2) {
        font-weight: bold;
    }

    .photoEtNom p:nth-child(3) {
        margin-top: -5vh;
        font-size: 25px;
        text-align: center;
    }

    .aPropos {
        position: relative;
        background: #43437c;
        left: -40px;
        padding: 50px;
        border-radius: 15px;
        width: 60%;
        text-align: justify;
        margin: 10px;
        transition: 1s;
    }

    .aPropos:hover {
        background: #e8e8e8;
        left: -20px;
    }

    .wrapperFormExp{
        position: relative;
        display: flex;
        flex-direction: row;
        background: #43437c;
        right: -40px;
        padding: 50px;
        border-radius: 15px;
        width: 50%;
        margin: 10px 10px 10px auto;
        transition: 1s;
    }

    .wrapperFormExp:hover {
        background: #e8e8e8;
        right: -20px;
    }

    .experiences{
        margin-left: auto;
    }

    .skills {
        margin-top: 50px;
        display: flex;
        flex-direction: row;
    }

    .skillButtonContainer {
        margin-left: auto;
        display: flex;
        flex-direction: column;
        width: 300px;
        position: relative; /* Nécessaire pour positionner le sélecteur */
    }

    .skillButton {
        position: relative;
        font-weight: bold;
        font-size: 1.5rem;
        height: 100px;
        margin: 30px;
        cursor: pointer;
        border-radius: 15px;
        border: none;
        transition: 0.5s;
        background-color: #e8e8e8;
    }

    .skillButton:hover {
        box-shadow: 0 0 0 8px #8d8d8d;
    }

    .skillButton.active:hover {
        box-shadow: none;
    }

    .selector {
        position: absolute;
        right: -40px;
        width: 100%;
        height: 100px;
        background: #e8e8e8;
        border-radius: 15px;
        transition: all 0.3s ease;
        z-index: -1;
    }

    .textSkillContainer {
        right: -15px;
        width: 70%;
        position: relative;
        background: #e8e8e8;
        border-radius: 15px;
        padding: 20px;
    }

    .langues {
        display: flex;
        justify-content: space-evenly;
    }

    .langues div{
        display: flex;
        flex-direction: column;
        align-items: center;
    }

    .langues img {
        margin: 5px;
        width: 250px;
    }

    #brit{
        width: 332px;
    }

    .competences{
        width: 99%;
        height: 100%;
        display: inline-grid;
        grid-template-columns: repeat(5, 1fr);
        grid-template-rows: repeat(2, 1fr);
        grid-column-gap: 20px;
        grid-row-gap: 20px;
    }

    .competences div{
        background: white;
        display: flex;
        flex-direction: column;
        width: 100%;
        border-radius: 15px;
        text-align: center;
        font-weight: bold;
        font-size: 20px;
        border: 2px solid Black;
        transition: 0.3s;
    }

    .competences div:hover {
        background: #e8e8e8;
    }

    .competences img{
        margin: 5px;
        max-height: 240px;
    }

    .softSkills{
        width: 99%;
        height: 100%;
        display: inline-grid;
        grid-template-columns: repeat(3, 1fr);
        grid-template-rows: repeat(2, 1fr);
        grid-column-gap: 20px;
        grid-row-gap: 20px;
    }

    .softSkills div{
        background: white;
        display: flex;
        flex-direction: column;
        width: 100%;
        border-radius: 15px;
        text-align: center;
        align-items: center;
        font-weight: bold;
        font-size: 20px;
        border: 2px solid Black;
        transition: 0.3s;
    }

    .softSkills img{
        margin: 5px;
        max-height: 240px;
        max-width: 240px;
    }

    .projets {
        display: flex;
        justify-content: center;
        align-items: center;
        overflow: hidden;
        margin: 0;
    }

    .card {
        display: flex;
        flex-direction: column;
        width: 300px;
        height: 200px;
        background: white;
        border-radius: 15px;
        transition: transform 0.1s ease-out;
        justify-content: center;
        align-items: center;
        font-size: 1.5rem;
        font-weight: bold;
        text-align: center;
        color: #333;
        margin: 10px;
        padding: 10px;
    }

    .card a {
        display: flex;
        align-items: center;
        cursor: pointer;
        margin-top: auto;
        background: rgba(29,49,195,1);
        height: 50px;
        width: 100px;
        color: white;
        border-radius: 5px;
    }

    .card a > p{
        margin-left: 26px;
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
    <div class="dots" data-speed="0.5">
        {#each dots as dot (dot.x)}
            <div class="dot" style="top: {dot.y}%; left: {dot.x}%; width: {dot.size}px; height: {dot.size}px;"></div>
        {/each}
    </div>
</div>


<span class="delimiter"></span>


<div class="photoEtNom">
    <img src="/images/Portfolio_photo.jpg" alt="Camille Okubo">
    <p>Camille Okubo</p>
    <p>Étudiant en BUT Informatique parcours réalisation d'applications</p>
</div>


<div class="aPropos">
    <h1>À propos</h1>
    <h3>Passionné d'informatique et des médias numériques depuis mon enfance, je suis actuellement en 2ème année de BUT Informatique à l'Université de Lille.</h3>
    Durant mon cursus universitaire et grâce à la spécialité Numérique et Sciences Informatique que j'ai eu l'occasion de suivre au lycée pour un Baccalauréat général, j'ai pu acquérir des compétences techniques diverses, notamment Python, Java, Bash, le développement web avec du HTML et CSS ainsi la manipulation de bases de données avec SQL et postgresql.<br>
    J'ai aussi eu l'occasion de participer à de multiples projets dans des équipes différentes dans le cadre de ces formations, ce qui m'a permi d'acquérir un certain esprit d'équipe et une adaptabilité pour de nouvelles équipes avec de personnes avec lesquelles je n'aurais pas travaillé avant.
</div>


<div class="wrapperFormExp">
    <div class="formations">
        <h3>Formations</h3>
        <span>2021 - 2023</span><br>
        Baccalauréat général spécialités NSI et LLCER anglais avec mention bien<br>
        <i>Lycée privé Lasalle Lille</i><br><br>
        <span>2023 - aujourd'hui</span><br>
        BUT Informatique parcours réalisation d'applications<br>
        <i>Université de Lille campus</i>
    </div>
    <div class="experiences">
        <h3>Experiences</h3>
        <span>nov. 2023 - mars 2024</span><br>
        Agent d'entretien<br>
        <i>Isor exploitation, site d'HR Path, Villeneuve-d'Ascq</i><br><br>
        <span>avr. 2025 - juin 2025</span><br>
        Échange étudiant<br>
        <i>Japon</i>
    </div>
</div>


<div class="skills">
    <div class="skillButtonContainer">
        {#each ["langues", "competences", "softSkills"] as skill, i}
            <button
                    bind:this={buttonRefs[i]}
                    class="skillButton {activeSkill === skill ? 'active' : ''}"
                    on:click={() => setActiveSkill(skill, i)}
            >
                {skill === "langues" ? "Langues" : ""}
                {skill === "competences" ? "Compétences techniques" : ""}
                {skill === "softSkills" ? "Soft Skills" : ""}
            </button>
        {/each}
        <div class="selector" style={selectorStyle}></div>
    </div>

    <div class="textSkillContainer">
        {#if activeSkill === "langues"}
            <div class="langues">
                <div>
                    <h2>Français</h2>
                    langue maternelle
                    <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/c/c3/Flag_of_France.svg/langfr-1280px-Flag_of_France.svg.png" alt="Drapeau Français">
                    <h2>Anglais</h2>
                    niveau C2
                    <img id="brit" src="https://upload.wikimedia.org/wikipedia/commons/thumb/a/a5/Flag_of_the_United_Kingdom_%281-2%29.svg/1920px-Flag_of_the_United_Kingdom_%281-2%29.svg.png" alt="Drapeau du Royaume-Unis">
                </div>
                <div>
                    <h2>Japonais</h2>
                    parlé, niveau B1
                    <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/9/9e/Flag_of_Japan.svg/langfr-1280px-Flag_of_Japan.svg.png" alt="Drapeau japonais">
                    <h2>Espagnol</h2>
                    niveau B1
                    <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/9/9a/Flag_of_Spain.svg/langfr-1280px-Flag_of_Spain.svg.png" alt="Drapeau espagnol">
                </div>
            </div>
        {/if}
        {#if activeSkill === "competences"}
            <div class="competences">
                <div>Java<img src="/images/JavaLogo.png" alt="Java Logo"></div>
                <div>Python<img src="/images/PythonLogo.png" alt="Python Logo"></div>
                <div>HTML<img src="/images/HTMLLogo.png" alt="HTML Logo"></div>
                <div>CSS<img src="/images/CSSLogo.png" alt="CSS Logo"></div>
                <div>JavaScript<img src="/images/JavaScriptLogo.png" alt="JavaScript Logo"></div>
                <div>Svelte<img src="/images/SvelteLogo.svg" alt="Svelte Logo"></div>
                <div>Git<img src="/images/GitLogo.png" alt="Git Logo"></div>
                <div>Bash<img src="/images/BashLogo.png" alt="Bash Logo"></div>
                <div>SQL<img src="/images/SQLLogo.png" alt="SQL Logo"></div>
                <div>PostgreSQL<img src="/images/PostgresqlLogo.png" alt="PostgreSQL Logo"></div>
            </div>
        {/if}
        {#if activeSkill === "softSkills"}
            <div class="softSkills">
                <div>Travail en équipe<img src="/images/people.png" alt="Logo personnes"></div>
                <div>Communication<img src="/images/speak.png" alt="Logo communication"></div>
                <div>Autonomie<img src="/images/brain.png" alt="Logo cerveau"></div>
                <div>Recherche<img src="/images/learn.png" alt="Logo recherche"></div>
                <div>Résolution de problèmes<img src="/images/idea.png" alt="Logo idée"></div>
            </div>
        {/if}
    </div>
</div>


<h1 style="color: white; position: relative; text-align: center;">Projets</h1>

<div class="projets">
    <div class="card" on:mousemove={(event) => handleMouseMove(event, event.currentTarget)} on:mouseleave={(event) => resetCard(event.currentTarget)} role="presentation">
        Application de classification et visualisation de données
        <a href="https://github.com/TheAereus/Classification"><p>Voir</p></a>
    </div>
    <div class="card" on:mousemove={(event) => handleMouseMove(event, event.currentTarget)} on:mouseleave={(event) => resetCard(event.currentTarget)} role="presentation">
        Nuit de l'info 2024
        <a href="https://hdfc-beige.vercel.app/"><p>Voir</p></a>
    </div>
    <div class="card" on:mousemove={(event) => handleMouseMove(event, event.currentTarget)} on:mouseleave={(event) => resetCard(event.currentTarget)} role="presentation">
        Plateforme de covoiturage
        <a href="https://c3c5323b-3d37-42ca-b226-5550baf25c49-00-2rbq1dat5x819.riker.replit.dev/index.html"><p>Voir</p></a>
    </div>
</div>

<div class="contact">
    <form>
        <input type="text" value="email">
        <input type="text" value="nom">
        <input type="text" value="Rédigez votre message">
    </form>
</div>
