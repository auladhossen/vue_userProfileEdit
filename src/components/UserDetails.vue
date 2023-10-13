<script setup>
import { ref, computed, watch } from "vue";
import { userdetails } from "../data/dummyuser.js";
import Swal from "sweetalert2";

const initUser = userdetails[0];

const getDefaultDate = () => {
  const defaultDate = new Date();
  defaultDate.setFullYear(defaultDate.getFullYear() - 30);
  return defaultDate.toISOString().split("T")[0];
};

const user = {
  name: ref(initUser.name),
  birthdate: ref(getDefaultDate()),
  email: ref(initUser.email),
  description: ref(initUser.description),
  profileImage: ref(initUser.image),
  age: ref(0),
};

const birthYear = computed(() => {
  const birthdate = new Date(user.birthdate.value);
  return birthdate.getFullYear();
});

const votingStatus = computed(() => {
  return user.age.value >= 18
    ? "You are eligible for voting"
    : "You are not eligible for voting";
});

const updateAge = () => {
  const birthdate = new Date(user.birthdate.value);
  const today = new Date();
  const age = today.getFullYear() - birthdate.getFullYear();
  user.age.value = age;
};
const submit = () => {
  if (user.age.value >= 18) {
    Swal.fire({
      icon: "success",
      title: "Submitted Successfully!",
      text: "You are eligible for voting",
      confirmButtonText: "OK",
    });
  } else {
    Swal.fire({
      icon: "error",
      title: "Error!",
      text: "You are not eligible for voting",
      confirmButtonText: "OK",
    });
  }
};

watch(user.birthdate, (newAge, oldAge) => {
  updateAge();
  if (newAge !== oldAge) {
    user.age.value = newAge;
  }
});

updateAge();
</script>

<template>
  <div class="userform">
    <div class="form">
      <h2>User Details</h2>
      <form>
        <label for="name">Name:</label>
        <input type="text" id="name" v-model="user.name.value" />
        <br />
        <label for="birthdate">Birthdate:</label>
        <input
          type="date"
          @input="updateAge"
          id="birthdate"
          v-model="user.birthdate.value"
        />
        <br />
        <label for="email">Email:</label>
        <input type="email" id="email" v-model="user.email.value" />
        <br />
        <label for="description">Description:</label>
        <textarea id="description" v-model="user.description.value"></textarea>
        <br />
        <label for="profile-image">Profile Image URL:</label>
        <input
          type="text"
          id="profile-image"
          v-model="user.profileImage.value"
        />
      </form>
      <button style="margin-top: 10px" @click="submit">Submit</button>
    </div>
    <div class="years">
      <div>
        <img
          height="200"
          width="200"
          style="border-radius: 50%"
          :src="user.profileImage.value"
          alt="user image"
        />
        <p>{{ user.name.value }}</p>
        <p>Birth Year: {{ birthYear }}</p>
        <p>Age: {{ user.age.value }}</p>
        <p>{{ votingStatus }}</p>
      </div>
    </div>
    <br />
  </div>
</template>

<style scoped>
body {
  font-family: Arial, sans-serif;
  background-color: #f5f5f5;
  margin: 0;
  padding: 0;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  height: 100vh;
}
.userform {
  display: flex;
  gap: 3rem;
  align-items: center;
}

form {
  background-color: #fff;
  padding: 20px;
  border-radius: 10px;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.2);
  width: 250px;
}

label {
  display: block;
  font-weight: bold;
  margin-bottom: 5px;
}

input[type="text"],
input[type="date"],
input[type="email"],
textarea {
  width: 90%;
  padding: 6px;
  margin-bottom: 15px;
  border: 1px solid #ccc;
  border-radius: 5px;
}

p {
  font-weight: bold;
  margin-top: 10px;
}

#profile-image {
  width: 90%;
  margin-bottom: 10px;
}

p.voting-status {
  color: #2e8b57;
}

button {
  background-color: #007bff;
  color: #fff;
  border: none;
  padding: 10px 15px;
  border-radius: 5px;
  cursor: pointer;
}

button:hover {
  background-color: #0056b3;
}
</style>
