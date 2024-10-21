<template>
  <div class="login-page">
    <!-- Background wrapper -->
    <div class="login-modal">
      <h1 v-if="!showLogin">Sign In</h1>
      <h1 v-else>Login</h1>
      <img src="@/assets/brand.png" alt="Logo" class="login-modal-logo" />

      <!-- Sign In Form -->
      <div v-if="!showLogin">
        <input type="text" v-model="input.username" placeholder="Username" />

        <input
          :type="passwordVisible ? 'text' : 'password'"
          v-model="input.password"
          placeholder="Password"
        />

        <input
          :type="passwordVisible ? 'text' : 'password'"
          v-model="input.confirmPassword"
          placeholder="Confirm Password"
        />

        <button @click="goToLogin">Next</button>

        <!-- Validation message -->
        <p v-if="passwordError" class="error-message">{{ passwordError }}</p>
      </div>

      <!-- Login Form -->
      <div v-else>
        <input
          type="text"
          v-model="loginInput.username"
          placeholder="Username"
        />

        <input
          :type="passwordVisible ? 'text' : 'password'"
          v-model="loginInput.password"
          placeholder="Password"
        />

        <button @click="login">Login</button>
        <button @click="reset">Reset</button>

        <!-- Login message -->
        <p v-if="loginMessage" class="login-message">{{ loginMessage }}</p>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "Log-in",
  data() {
    return {
      input: {
        username: "",
        password: "",
        confirmPassword: "", // Added confirm password field
      },
      loginInput: {
        // New data object for login input
        username: "",
        password: "",
      },
      passwordVisible: false,
      loginMessage: "", // Variable to hold the login message
      passwordError: "", // Variable for password match error message
      showLogin: false, // Control whether to show the login form
      savedCredentials: null, // Variable to store signed-in credentials
    };
  },
  methods: {
    goToLogin() {
      this.passwordError = ""; // Reset the error message

      // Check if passwords match and validate password strength
      if (this.input.password !== this.input.confirmPassword) {
        this.passwordError = "Passwords do not match.";
        return;
      }

      if (!this.validatePassword(this.input.password)) {
        this.passwordError =
          "Password must include uppercase, lowercase, number, and special character.";
        return;
      }

      // If all validations pass, save the credentials and switch to login form
      this.savedCredentials = {
        username: this.input.username,
        password: this.input.password,
      };
      this.showLogin = true; // Switch to Login form
      this.resetInputs(); // Clear inputs for the next step
    },
    validatePassword(password) {
      const hasUpperCase = /[A-Z]/.test(password);
      const hasLowerCase = /[a-z]/.test(password);
      const hasNumbers = /\d/.test(password);
      const hasSpecial = /[!@#$%^&*(),.?":{}|<>]/.test(password);
      const minLength = password.length >= 8; // Minimum length requirement

      return (
        hasUpperCase && hasLowerCase && hasNumbers && hasSpecial && minLength
      );
    },
    login() {
      // Check if login credentials match the saved credentials
      if (
        this.loginInput.username === this.savedCredentials.username &&
        this.loginInput.password === this.savedCredentials.password
      ) {
        this.loginMessage = "Login successful! Data saved!";
      } else {
        this.loginMessage = "Please check your details.";
      }
    },
    reset() {
      this.input.username = "";
      this.input.password = "";
      this.input.confirmPassword = ""; // Reset confirm password
      this.loginInput.username = ""; // Reset login username
      this.loginInput.password = ""; // Reset login password
      this.showLogin = false; // Reset to Sign In form after reset
      this.passwordError = ""; // Clear any password error message
      this.loginMessage = ""; // Clear any login message
    },
    resetInputs() {
      this.input.username = "";
      this.input.password = "";
      this.input.confirmPassword = "";
    },
    togglePasswordVisibility() {
      this.passwordVisible = !this.passwordVisible;
    },
  },
};
</script>

<style>
/* Page background */
.login-page {
  height: 100vh;
  background: linear-gradient(
    45deg,
    #002366,
    #ff4500
  ); /* Gradient background */
  display: flex;
  align-items: center;
  justify-content: center;
}

/* Modal Styles */
.login-modal {
  width: 400px;
  background: linear-gradient(135deg, #ff0066, #00cfff); /* Existing gradient */
  padding: 40px;
  margin-top: 100px; /* Adjusted margin */
  margin: auto;
  border-radius: 10px;
  box-shadow: 0 10px 30px rgba(0, 0, 0, 0.15); /* Larger shadow for depth */
  color: white;
  text-align: center;
}

/* Logo */
.login-modal-logo {
  width: 100px; /* Adjusted size */
  height: auto;
  margin-bottom: 20px;
}

.error-message {
  color: red; /* Change color as needed */
  font-size: 14px; /* Adjust font size */
  margin-top: 10px; /* Space between input and message */
}

.login-message {
  margin-top: 20px;
  color: yellow;
  font-weight: bold;
  font-size: 18px; /* Slightly larger font */
}

/* Input fields */
input {
  width: 100%;
  padding: 10px;
  margin-bottom: 15px;
  border: none;
  border-radius: 5px;
  font-size: 16px;
  box-shadow: 0 5px 15px rgba(0, 0, 0, 0.1); /* Shadow effect */
  background-color: rgba(255, 255, 255, 0.2); /* Transparent background */
  color: white;
  transition: 0.3s ease;
}

/* Input hover & focus */
input:hover,
input:focus {
  background-color: rgba(255, 255, 255, 0.4); /* Slightly lighter on focus */
  box-shadow: 0 8px 20px rgba(0, 0, 0, 0.2); /* Stronger shadow on hover */
  outline: none; /* Remove default outline */
}

/* Buttons */
button {
  width: 100%;
  padding: 15px;
  margin-bottom: 15px;
  border: none;
  border-radius: 10px;
  font-size: 16px;
  background: linear-gradient(135deg, #ff0066, #ff4500); /* Gradient button */
  color: white;
  cursor: none;
  transition: 0.3s ease;
  box-shadow: 0 5px 15px rgba(0, 0, 0, 0.1); /* Button shadow */
}

/* Button hover effect */
button:hover {
  background: linear-gradient(
    135deg,
    #ff4500,
    #ff0066
  ); /* Reverse gradient on hover */
  box-shadow: 0 8px 20px rgba(0, 0, 0, 0.2); /* Stronger shadow on hover */
}

/* Message Styling */
.login-message {
  margin-top: 20px;
  color: yellow;
  font-weight: bold;
  font-size: 18px; /* Slightly larger font */
}
</style>