<script>
	import { goto } from '$app/navigation';
	import { page } from '$app/stores';

	const urlParams = $page.url;
	let people_list = ['aldo', 'giovanni', 'giacomo'];
	let choosen_person = [];

	if(urlParams.searchParams.has('people')){
		people_list = urlParams.searchParams.get('people').split(" ")
	}

	if(urlParams.searchParams.has('choosen')){
		choosen_person = urlParams.searchParams.get('choosen').split(" ")
	}

	let new_person = '';
	let number_of_people = 1;

	const addToList = () => {
		if (new_person != '' && new_person != undefined && new_person[new_person.length - 1] == ' ') {
			people_list = people_list.concat([new_person]);
			new_person = '';
			$page.url.searchParams.set('people', people_list.join("%20"));
		}
	};

	const addPersonToDrive = () => {
		if (number_of_people < people_list.length) {
			number_of_people = number_of_people + 1;
			return;
		}
		number_of_people = 1;
	};

	const findInList = (list, elem) => {
		for (let i = 0; i < list.length; i++) {
			if (list[i] == elem) {
				return true;
			}
		}
		return false;
	};

	const chooseThePerson = () => {
		choosen_person = Array(number_of_people);
		for (let i = 0; i < number_of_people; i++) {
			var random_number = Math.floor(Math.random() * people_list.length);
			while (findInList(choosen_person, people_list[random_number])) {
				random_number = Math.floor(Math.random() * people_list.length);
			}
			choosen_person[i] = people_list[random_number];
		}
		$page.url.searchParams.set('choosen', choosen_person);
	};

	const shareURL = () => {
		const sharable = "http://" + $page.url.host + "?people=" + people_list.join("%20") + "&choosen=" + choosen_person.join("%20")
		goto(sharable); 
		navigator.clipboard.writeText(sharable);
	}
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
		<div>
			Click here to clean the list<button on:click={() => {people_list = [];}}>Clean</button>
		</div>
		<div>
			How many you want to pick? <button on:click={addPersonToDrive}>{number_of_people}</button>
		</div>
		<div>Ready to pick?<button on:click={chooseThePerson}>Pick!</button></div>
		<div><button on:click={shareURL}>Share</button></div>
	</div>
	<div class="peopleview">
		{#if choosen_person.length > 0}
			{#each choosen_person as cs}
				{cs + ' '}
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
		margin-left: 20px;
	}

	button:hover {
		cursor: pointer;
		color: grey;
	}

	.showroom {
		font-size: 30px;
	}

	.peopleview {
		margin: 10px;
		text-align: center;
		font-size: 50px;
	}

	input {
		background-color: transparent;
		border: solid black 1px;
		padding: 5px;
	}

	@media only screen and (max-width: 800px) {
		input {
			width: 80vw;
		}
	}
</style>
