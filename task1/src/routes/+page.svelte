<script lang="ts">
	import { onMount } from 'svelte';

	let sevenLetterString = '';
	let formData = {
		firstname: '',
		lastname: '',
		gender: '',
		age: 18,
		date: '',
		userInput: ''
	};
	let submissionStatus = false;
	let errorMessage = '';

	const fetchCaptcha = async () => {
		const API_URL = 'https://baconipsum.com/api/?type=meat-and-filler';

		try {
			const response = await fetch(API_URL);
			const captchaData = await response.json();

			// Extract a 7-letter string from the first entry in captchaData
			sevenLetterString = captchaData[0].replace(/[^a-zA-Z0-9]/g, '').substring(0, 7);
			console.log(sevenLetterString, 'sevenLetterString');
		} catch (error) 
		{
			console.error(error);
			// Handle error appropriately
		}
	};

	onMount(fetchCaptcha);
	{
		console.log("ON MOUNT CALLED")
	}

	const submitForm = () => {
		// Check if user input matches the fetched captcha
		if (formData.userInput === sevenLetterString) {
			console.log('Captcha is correct. Proceed with form submission.');
			// Additional form submission logic here
			submissionStatus = true;
		} else {
			console.log('Captcha is incorrect. Please try again.');
			errorMessage = 'Mismatch! Enter the correct captcha.';
		}
		console.log(formData);
	};

	const reloadCaptcha = () => {
		fetchCaptcha();
		formData.userInput = ''; // Clear user input when reloading Captcha
		errorMessage = ''; // Clear error message on reload
	};
</script>

{#if submissionStatus === false}
	<div class="min-h-screen flex items-center justify-center">
		<form
			name="myForm"
			on:submit|preventDefault={submitForm}
			class="bg-white p-6 rounded-lg shadow-md w-full md:w-1/2 lg:w-1/3"
		>
			<div class="mb-4">
				<label for="firstname" class="block text-sm font-medium text-gray-600">First Name:</label>
				<input
					type="text"
					id="firstname"
					class="border-2 border-gray-300 p-2 w-full"
					bind:value={formData.firstname}
					required
				/>
			</div>
			<div class="mb-4">
				<label for="lastname" class="block text-sm font-medium text-gray-600">Last Name:</label>
				<input
					type="text"
					id="lastname"
					class="border-2 border-gray-300 p-2 w-full"
					bind:value={formData.lastname}
					required
				/>
			</div>
			<div class="mb-4">
				<label class="block text-sm font-medium text-gray-600">Gender:</label>
				<div class="flex">
					<div class="mr-4">
						<input
							type="radio"
							id="male"
							name="gender"
							bind:group={formData.gender}
							value="Male"
							required
						/>
						<label for="male" class="ml-1">Male</label>
					</div>
					<div class="mr-4">
						<input
							type="radio"
							id="female"
							name="gender"
							bind:group={formData.gender}
							value="Female"
							required
						/>
						<label for="female" class="ml-1">Female</label>
					</div>
					<div>
						<input
							type="radio"
							id="others"
							name="gender"
							bind:group={formData.gender}
							value="Others"
							required
						/>
						<label for="others" class="ml-1">Others</label>
					</div>
				</div>
			</div>
			<div class="mb-4">
				<label for="age" class="block text-sm font-medium text-gray-600">Age:</label>
				<select class="border-2 border-gray-300 p-2 w-full" bind:value={formData.age} required>
					{#each Array.from({ length: 83 }, (_, i) => i + 18) as ageOption}
						<option value={ageOption}>{ageOption}</option>
					{/each}
				</select>
			</div>
			<div class="mb-4">
				<label for="date" class="block text-sm font-medium text-gray-600">Date:</label>
				<input
					type="date"
					class="border-2 border-gray-300 p-2 w-full"
					bind:value={formData.date}
					required
				/>
			</div>
			<div class="mb-4 flex items-center space-x-10">
				<div class="flex-shrink-0">
					<p class="text-gray-600 mb-2">Captcha:</p>
				</div>
				<div class="flex-grow">
					<p class="font-bold rounded text-lg text-center mr-2 mb-2 line-through bg-gray-600">{sevenLetterString}</p>
				</div>
				<div>
					<button
						type="button"
						class="p-2 border border-gray-400 rounded-md"
						on:click={reloadCaptcha}
					>
						Reload
					</button>
				</div>
			</div>
			

			{#if errorMessage}
				<p class="text-red-500 text-sm mb-4">{errorMessage}</p>
			{/if}

			<div class="mb-4">
				<label for="captcha-form" class="block text-sm font-medium text-gray-600"
					>Enter Captcha:</label
				>
				<input
					type="text"
					id="captcha-form"
					class="border-2 border-gray-300 p-2 w-full"
					bind:value={formData.userInput}
					placeholder="Enter captcha"
					required
				/>
			</div>
			<button type="submit" class="bg-blue-500 text-white py-2 px-4 rounded hover:bg-blue-600">
				Submit
			</button>
		</form>
	</div>
{:else}
	<p class="text-center text-green-500 text-xl">Successfully Submitted!</p>
{/if}
