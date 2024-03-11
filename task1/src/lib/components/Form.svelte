<script lang="ts">
	import RadioField from './RadioField.svelte';
	import InputField from './InputField.svelte';
	import { onMount } from 'svelte';
	import SelectField from './SelectField.svelte';
	import { goto } from '$app/navigation';

	let formData = {
		firstname: '',
		lastname: '',
		gender: '',
		age: '',
		date: '',
		userInput: ''
	};
	let submissionStatus = false;
	let errorMessage = '';

	const ageOptions = {
		option1: '18-28',
		option2: '28-38',
		option3: '38-48',
		option4: '48-58'
	};
	const genderOptions = { male: 'Male', female: 'Female', others: 'Others' };
	// Declaring a variable to store the generated 7-letter string
	let sevenLetterString = '';
	// Function to fetch and set the captcha on component mount
	const fetchCaptcha = async () => {
		// API URL for fetching captcha data
		const API_URL = 'https://baconipsum.com/api/?type=meat-and-filler';

		try {
			// Fetch data from the API
			const response = await fetch(API_URL);
			const captchaData = await response.json();
			// Extract a 7-letter string from the first entry in captchaData
			sevenLetterString = captchaData[0].replace(/[^a-zA-Z0-9]/g, '').substring(0, 7);
			console.log(sevenLetterString, 'sevenLetterString');
		} catch (error) {
			// Handle errors
			console.error(error);
		}
	};

	// Call fetchCaptcha on component mount
	onMount(fetchCaptcha);

	// Function to reload the captcha
	const reloadCaptcha = () => {
		// Call fetchCaptcha to reload the captcha
		fetchCaptcha();
	};

	const submitForm = () => {
		const isFormValid = Object.values(formData).every((value) => value !== '');

		if (isFormValid) {
			if (formData.userInput === sevenLetterString) {
				console.log('Captcha is correct. Proceed with form submission.');
				submissionStatus = true;
				errorMessage = '';
				console.log(formData);
				if (submissionStatus) {
					goto('/');
				}
			} else {
				console.log('Captcha is incorrect. Please try again.');
				submissionStatus = false;
				errorMessage = 'Mismatch! Enter the correct captcha.';
			}
		} else {
			console.log('Please fill in all required fields.');
			submissionStatus = false;
		}
	};
</script>

{#if submissionStatus === false}
	<div class="min-h-screen flex items-center justify-center">
		<form
			name="myForm"
			on:submit|preventDefault={submitForm}
			class="bg-white p-6 rounded-lg shadow-md w-full md:w-1/2 lg:w-1/3"
		>
			<InputField
				label="First Name"
				id="firstname"
				type="text"
				bind:value={formData.firstname}
				required
			></InputField>
			<InputField
				label="Last Name"
				id="lastname"
				type="text"
				bind:value={formData.lastname}
			></InputField>
			<SelectField
				label="Age"
				id="age"
				type="select"
				options={ageOptions}
				bind:value={formData.age}
				required
			></SelectField>
			<RadioField
				label="Gender"
				id="gender"
				type="radio"
				options={genderOptions}
				bind:value={formData.gender}
				required
			></RadioField>
			<InputField label="Date" id="date" type="date" bind:value={formData.date} required
			></InputField>
			<div class="mb-4 flex items-center space-x-10">
				<div class="flex-shrink-0">
					<p class="text-gray-600 mb-2">Captcha:</p>
				</div>
				<div class="flex-grow">
					<p class="font-bold rounded text-lg text-center mr-2 mb-2 line-through bg-gray-600">
						{sevenLetterString}
					</p>
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
			<p class="text-red-500">{errorMessage}</p>
			<InputField
				type="text"
				label="Enter Captcha"
				id="captcha-form"
				bind:value={formData.userInput}
				required
			/>
			<button type="submit" class="bg-blue-500 text-white py-2 px-4 rounded hover:bg-blue-600">
				Submit
			</button>
		</form>
	</div>
{:else}
	<p class="text-center text-green-500 text-xl">Successfully Submitted!</p>
{/if}
