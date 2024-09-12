<script setup lang="ts">
// API response
// {
//     "status": "fail",
//     "errors": "Invalid password"
// }

const data = ref(null);
const errorCode = ref(null);
const errorMessage = ref("");

const handleSubmit = async () => {
	const response = await useFetch(`http://localhost:5000/api/v1/users/login`, {
		method: "POST",
		body: JSON.stringify({
			email: "farismnrr1726148744047@gmail.com",
			password: "WrongPassword",
		}),
	});

	if (response.status.value === "success") {
		errorCode.value = response.status.value; // <== Get API data
	} else {
		errorCode.value = response.error.value.statusCode; // <== Get the error code
		errorMessage.value = response.error.value.data.errors; // <== Get the error message
	}
};
</script>

<template>
	<button @click="handleSubmit">Submit</button>
	<div>
		<span v-if="data">
			API Data:
			<pre>{{ data }}</pre>
		</span>
		<p v-if="errorCode">{{ errorCode }}</p>
		<p v-if="errorMessage">{{ errorMessage }}</p>
	</div>
</template>
