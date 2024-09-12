<script setup lang="ts">
// API response
// {
//     "status": "fail",
//     "errors": "Invalid password"
// }

const config = useRuntimeConfig();
const data = ref(null);
const errorCode = ref(null);
const errorMessage = ref("");

const handleSubmit = async () => {
    try {
        const response = await $fetch(`http://localhost:5000/api/v1/users/login`, {
            method: "POST",
            body: JSON.stringify({
                email: "farismnrr1726148744047@gmail.com",
                password: "WrongPassword",
            }),
        });

        data.value = response; // <== Get API data
    } catch (err: any) {
        const responseMessage = err.response;
        errorMessage.value = JSON.stringify(responseMessage._data.errors, null, 2); // <== Get the error message

        const responseCode = err.response.status;
        errorCode.value = responseCode; // <== Get the error code
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
