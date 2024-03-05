<!-- Captcha.svelte -->
<script lang="ts">
    import { onMount } from 'svelte';
  
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
  </script>
  
  <!-- Captcha display section -->
  <div class="mb-4 flex items-center space-x-10">
    <!-- Captcha label -->
    <div class="flex-shrink-0">
      <p class="text-gray-600 mb-2">Captcha:</p>
    </div>
    <!-- Display the 7-letter string -->
    <div class="flex-grow">
      <p class="font-bold rounded text-lg text-center mr-2 mb-2 line-through bg-gray-600">{sevenLetterString}</p>
    </div>
    <!-- Reload button -->
    <div>
      <button type="button" class="p-2 border border-gray-400 rounded-md" on:click={reloadCaptcha}>
        Reload
      </button>
    </div>
  </div>
  