<script>
	let people_list = [];
	let new_person = '';
	let number_of_people = 1;
	let choosen_person = [];

	const addToList = () => {
		if (new_person != '' && new_person != undefined && new_person[new_person.length - 1] == ' ') {
			people_list = people_list.concat([new_person]);
			new_person = '';
		}
	};

	const addPersonToDrive = () => {
		if (number_of_people < people_list.length) {
			number_of_people = number_of_people + 1;
			return;
		}
		number_of_people = 1;
	};

	const chooseThePerson = () => {
		choosen_person = Array(number_of_people)
		for (let i = 0; i < number_of_people; i++) {
			const random_number = Math.floor(Math.random() * people_list.length);

			if (
				(people_list[random_number].toLowerCase() == 'michele' ||
					people_list[random_number].toLowerCase() == 'mike') &&
				people_list[random_number].toLowerCase() != 'miche'
			) {
				choosen_person[i] = people_list[Math.floor(Math.random() * people_list.length)];
			} else {
				choosen_person[i] = people_list[random_number];
			}
		}
	};
</script>

<svelte:head>
	<title>Home</title>
	<meta name="description" content="Svelte demo app" />
	<link rel="preconnect" href="https://fonts.googleapis.com" />
	<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin="true" />
	<link href="https://fonts.googleapis.com/css2?family=Borel&display=swap" rel="stylesheet" />
</svelte:head>

<section>
	<h1>Who Drives Tonight?</h1>
	<div class="showroom">
		{#if people_list.length > 0}
			{#each people_list as person}
				{person + ' '}
			{/each}
		{/if}
		<input bind:value={new_person} type="text" placeholder="name" on:keyup={addToList} />
	</div>

	<div>
		Pick <button on:click={addPersonToDrive}>{number_of_people}</button> to drive
		<button on:click={chooseThePerson}>Pick!</button>
	</div>
	<div class="peopleview">
		{#if choosen_person.length > 0}
			{#each choosen_person as cs}
				{cs}
			{/each}
		{/if}
	</div>
</section>

<style>
	h1 {
		font-family: 'Borel', cursive;
		line-height: 30px;
	}

	button {
		border: 1px solid black;
		padding: 10px;
		background-color: transparent;
		margin-top: 20px;
		border-radius: 20px;
	}

	button:hover {
		cursor: pointer;
		color: grey;
	}

	.showroom {
		font-size: 30px;
	}

	.peopleview{
		margin: 10px;
		text-align: center;
		font-size: 50px;
	}

	@media only screen and (max-width: 800px) {
		input {
			width: 80vw;
		}
	}
</style>
