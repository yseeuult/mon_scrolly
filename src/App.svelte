<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">

</head>
<body>
 
<script>

  import { onMount, onDestroy } from 'svelte';

import LoremIpsum from './LoremIpsum.svelte'

	import Introduction from './Introduction.svelte'

  let activeStepIndex = 0;
  let observer;
  let scrollyRef;
  let flourishID = "2342413"; // L'ID de la story Flourish

// le texte des boites
	
  let stepsData = [
    { "text": "En Belgique, les produits emballés mis sur le marché <mark style='background-color: #B118C8; color:white; padding: 2px; border-radius: 5px;'><strong>sur la courbe mauve</strong></mark>,généraient 188,7 kg de déchets d'emballages par habitant en 2021. Au total, ça fait un peu moins de 2 millions de tonnes de produits." },
    { "text": "La Belgique a un taux de recyclage des déchets <mark style='background-color:#32CBFF80; color:white; padding 2px; border-radius: 5px;'> 80,4% des emballages depuis 2021 </mark>, soit un peu moins de 1,6 millions de tonnes." },
    { "text": "Depuis 2001, le taux de recyclage belge a augmenté de 9%, allant de 71,8% (un peu moins de 1,2 millions de tonnes) à 80,4% en 2021." },
		{ "text": "A partir de 2014, le taux de recyclage en Belgique passe le cap  <mark style='background-color: #B118C8; color:white; padding: 2px; border-radius: 5px;'><strong> des 80%</strong></mark> </mark>"  },
{"text":"Que ce soit aujourd'hui ou il y a 20 ans, le plastique demeure la catégorie de déchets la moins recyclée. Toutefois, son taux de recyclage a considérablement augmenté, passant de 28% en 2001 à 49% en 2021." }
  ];

	// Le "moteur" du scrollytelling qui utilise l'Intersection Observer API (en gros, le code observe ce qu'il y a à l'écran)
	// ça on ne touche pas sinon tout se casse !

  onMount(() => {
    observer = new IntersectionObserver((entries) => {
      entries.forEach(entry => {
        const { target, isIntersecting } = entry;
        const index = Array.from(scrollyRef.querySelectorAll('.step')).indexOf(target);
        if (isIntersecting) {
          activeStepIndex = index;
        }
      });
    }, { threshold: 0.6 });

    const stepElements = scrollyRef.querySelectorAll('.step');
    stepElements.forEach(el => observer.observe(el));
  });

  onDestroy(() => {
    observer.disconnect();
  });
</script>

<h1>Pacte Vert: les chiffres du recyclage des déchets en Belgique </h1>
<figure><img src="https://cor.europa.eu/en/events/PublishingImages/4565%20GREEN%20DEAL%20new%20branding%20-%20web%20banner%20900x600.png" alt="le pacte vert de l'Europe" width=500 height=350>
<figcaption> <i> Le Pacte Vert est un des enjeux forts de la présidence belge de l'UE. (c) CdR </i></figcaption></figure><Introduction/> 


<!-- si tu affiches la step_0, alors montre la slide_0 -->
<!-- si tu affiches la step_1, alors montre la slide_1 -->
<!-- si tu affiches la step_2, alors montre la slide_2 -->

<section bind:this={scrollyRef} class="section-container">

	  <div class="foreground">
    {#each stepsData as { text }, index}
      <div class="step" data-step={index}>
        <div class="step-content">
          <p>{@html text}</p> <!-- @html important pour que le css du script soit pris en compte-->
        </div>
      </div>
    {/each}
  </div>
	
  <div class="sticky-background">
    <!-- On affiche la slide Flourish en fonction de l'index -->
		<iframe src={`https://flo.uri.sh/story/${flourishID}/embed#slide-${activeStepIndex}`} title="Contenu interactif ou visuel" class="flourish-embed" frameborder="0" scrolling="no" style="width:100%;height:100vh;"></iframe>
  </div>


</section>


<style>

	/* Ici les valeurs pour l'ensemble de la page > 
	peut nécessiter des modifs de couleurs dans Flourish 
	pour s'assurer que le graphe soit tjs bien visible (titre de graphique noir sur
	fond de page noir,ça ne se voit pas bien...*/

	:global(body) {
    background-color: white; 
		color: black;
		font-family: 'Times New Roman', sans-serif; 
  }
	
	*{
		box-sizing: border-box;
	}
	.section-container {
    margin-top: 1em;
    text-align: center;
    display: flex;
    flex-direction: row; 
		
  }

  .sticky-background {
    position: sticky;
    top: 0;
    height: 100vh;
    flex: 0 1 60%;
    overflow: hidden;
    z-index: 1;
  }

  .foreground {
    display: flex;
    flex-direction: column;
    align-items: center;
    flex: 0 1 40%;
		margin-bottom: 100vh;
  }

  .step {
    min-height: 100vh;
    display: flex;
    justify-content: center;
    align-items: center;
    padding: 20px;
    position: relative;
    z-index: 2;
    width: 100%;
  }

  .step-content {
    background-color: rgba(245, 245, 245, 0.8);
		box-shadow: 1px 1px 10px rgba(0, 0, 0, 0.2);
    color: black;
    border-radius: 20px;
		border: 3px solid black;
    padding: 0.5rem 1rem;
    display: flex;
    flex-direction: column;
    justify-content: center;
    z-index: 10;
    font-size: 1rem;
    text-align: left;
    width: 100%; 
    max-width: 500px; 
    margin: auto;
  }

	/* Pour adapter la vue en mobile: steps centrées par dessus le graphique */

  @media screen and (max-width: 768px) {
    .section-container {
      flex-direction: column-reverse;
    }
    .sticky-background, .foreground {
      width: 100%; 
    }
    .foreground {
      margin-top: -80vh; 
    }
  }
</style>
</body>
</html>
