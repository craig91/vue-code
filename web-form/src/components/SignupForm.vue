<template>
    <form @submit.prevent="handleSubmit">
        <label>Email</label>
        <input type="email" v-model="email" required>

        <label>Password</label>
        <input type="password" v-model="password" required>
        <div v-if="passwordError" class="error">{{ passwordError }}</div>

        <label>Role:</label>
        <select v-model="role">
            <option value="developer">Web Developer</option>
            <option value="designer">Web Designer</option>
        </select>

        <label> Skills: </label>
        <input type="text" v-model="tempSkill" @keyup="addSkill">
        <div v-for="skill in skills" :key="skill" class="pill">
            <!-- vue needs to keep track of the elements in this list so use v-bind to a key property. Using v-for always needs a key property-->
            <span @click="deleteSkill(skill)">{{ skill }}</span>
        </div>

        <div class="terms">
            <input type="checkbox" v-model="terms" required>
            <label>Accept terms and conditions</label>
        </div>

        <div class="submit">
            <button>Create an Account</button>
        </div>

    </form>

    <p>Email: {{ email }}</p> <!-- What is typed in email and password input fields are displayed on the page here-->
    <p>Password: {{ password }}</p>
    <p>Role: {{ role }}</p> <!-- What is selected in the drop down for Role will displayed out here.-->
    <p>Terms accepted: {{ terms }}</p>
    <!-- true or false will display here if the terms check box is clicked/unclicked -->
    <!-- <p>Names: {{ names }}</p> -->
</template>

<script>

export default {
    data() {
        return {
            email: '', // when a user types data into the email input, this string gets updated with that value
            password: '', // the same as stated above applies to password. (v-model does this 2 way data binding)
            role: 'Designer', // whatever value the user selects in the dropped down, that selection will be updated in role: Whatever value I put here will be the default option. In this case it is the Designer value (see the role label above)
            tempSkill: '',
            terms: false, // by default, terms and conditions checkbox is unselected, when the box is clicked, the boolean will change to true.
            skills: [],
            passwordError: ''
            // names: []    
        }

    },
    methods: {
        addSkill(e) {
            if (e.key === ',' && this.tempSkill) { // if ',' is pressed and tempSkill has a value (meaning if the skill input has something written in it) then:
                console.log(e)
                if (!this.skills.includes(this.tempSkill)) { // this checks if what is being typed in tempSkill is already in the skills array the ! makes this to where duplicate skills can't be added because it already exists in the skills array
                    this.skills.push(this.tempSkill) // whatever is typed into the input field will be added to the skills array.
                }

                this.tempSkill = '' // after the data is pushed into the array, this statement clears the input field and puts tempSkill in its original state: an empty string waiting for data again
            }
        },
        deleteSkill(skill) {
            this.skills = this.skills.filter((item) => {
                return skill !== item
            })
            console.log("Deleted")
        },
        handleSubmit() {
            // validate password
            this.passwordError = this.password.length > 5 ?
                '' : 'Password must be at least 6 chars long'


            if (!this.passwordError) {
                console.log('email: ', this.email)
                console.log('password: ', this.password)
                console.log('role: ', this.role)
                console.log('skills: ', this.skills)
                console.log('terms accepted: ', this.terms)
            }
        }
    }
}
</script>

<style>
form {
    max-width: 420px;
    margin: 30px auto;
    background: white;
    text-align: left;
    padding: 40px;
    border-radius: 10px;
}

label {
    color: #aaa;
    display: inline-block;
    margin: 25px 0 15px;
    font-size: 0.6em;
    text-transform: uppercase;
    letter-spacing: 1px;
    font-weight: bold;
}

input,
select {
    display: block;
    padding: 10px 6px;
    width: 100%;
    box-sizing: border-box;
    border: none;
    border-bottom: 1px solid #ddd;
    color: #555;

}

input[type="checkbox"] {
    display: inline-block;
    width: 16px;
    margin: 0 10px 0 0;
    position: relative;
    top: 2px;
}

.pill {
    display: inline-block;
    margin: 20px 10px 0 0;
    padding: 6px 12px;
    background: #eee;
    border-radius: 20px;
    font-size: 12px;
    letter-spacing: 1px;
    font-weight: bold;
    color: #777;
    cursor: pointer;
}

button {
    background: #0b6dff;
    border: 0;
    padding: 10px 20px;
    margin-top: 20px;
    color: white;
    border-radius: 20px;
}

.submit {
    text-align: center;
}

.error {
    color: #ff0062;
    margin-top: 10px;
    font-size: 0.8em;
    font-weight: bold;
}
</style>