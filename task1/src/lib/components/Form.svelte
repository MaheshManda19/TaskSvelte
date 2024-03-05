<script lang="ts">
	import { onMount } from 'svelte';
    import Captcha from './Captcha.svelte';

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
			<Captcha/>
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