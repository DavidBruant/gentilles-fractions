<script>
	const dénominateurs = [2, 3, 4, 5, 6, 8, 10, 12]

	const fractions = []

	const abs = Math.abs

	for(const dénominateur of dénominateurs){
		for(let numérateur = 1 ; numérateur < dénominateur ; numérateur++){
			const value = numérateur / dénominateur

			if(fractions.every(f => f.value !== value)){
				fractions.push({
					numérateur,
					dénominateur,
					value
				})
			}
		}
	}

	console.log('fractions', fractions)


	/**
	 * 
	 * @param {number} n 
	 */
	function trouverFractionLaPlusProche(n){

		const candidats = fractions

		let meilleurCandidat = {
			value: Infinity
		}

		for(const candidat of candidats){
			if(abs(candidat.value - n) < abs(meilleurCandidat.value - n)){
				meilleurCandidat = candidat
			}
		}

		return meilleurCandidat
	}

	let pourcentageStr = "37"
	$: pourcentage = parseFloat(pourcentageStr.replace(',', '.').trim())/100

	$: fractionLaPlusProche = trouverFractionLaPlusProche(pourcentage)
	
	$: console.log('best approx of', pourcentage, fractionLaPlusProche)
</script>

<h1>Gentilles fractions</h1>

<p>Parce que c'est chiant les pourcentages</p>
<p>et qu'on a rarement besoin de ce niveau de précision à part pour faire semblant d'être sérieux.se</p>

<label>
	<h3>Saisir un pourcentage</h3>
	<input bind:value={pourcentageStr} type="text"> %
</label>

<h3>Fraction la plus proche</h3>

<div class="fraction-visuelle">
	{#each Array(fractionLaPlusProche.dénominateur).fill(undefined) as _, index }
		<span class:active={index + 1 <= fractionLaPlusProche.numérateur}>☀️</span>
	{/each}
</div>

<span>{fractionLaPlusProche.numérateur}</span> / <span>{fractionLaPlusProche.dénominateur}</span>
<br>
(<span>{fractionLaPlusProche.value}</span> - erreur de ~{Math.round(abs(fractionLaPlusProche.value - pourcentage)*100)}%)



<style lang="scss">
	
	:global(main) {
		text-align: left;
		padding: 1em;
		max-width: 240px;
		margin: 0 auto;

		@media (min-width: 640px) {
			max-width: none;
		}
	}

	input{
		text-align: center;
		font-size: 1.2em;
		border: 1px solid #DDD;
		border-radius: 1em;
		padding: 0.3em;

		width: 4em;
	}

	.fraction-visuelle{
		span{
			display: inline-block;
			padding: 0.3em;
		}

		span:not(.active){
			filter: grayscale(100%);
		}
	}

	h2 {
		color: #ff3e00;
		text-transform: uppercase;
		font-size: 4em;
		font-weight: 100;
	}
	
</style>
