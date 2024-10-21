<template>
  <div
    id="landing-page"
    @mousemove="updateCursor"
    @mouseleave="hideCursor"
    @mouseenter="showCursor"
  >
    <!-- Custom Cursor Elements -->
    <div
      v-show="isVisible"
      id="cursor"
      :style="{
        backgroundColor: cursorColor,
        width: cursorSize,
        height: cursorSize,
      }"
    ></div>
    <div
      v-show="isVisible"
      id="cursor-border"
      :style="{
        width: borderSize,
        height: borderSize,
        boxShadow: `0 0 0 1px ${cursorBorderColor}`,
      }"
    ></div>

    <!-- Hero Section -->
    <section id="hero" class="fade-in">
      <div class="hero-content">
        <img src="@/assets/brand.png" alt="Logo" class="hero-logo" />
        <h1>Welcome to Our Brand Book Studio</h1>
        <p>
          Your journey starts here. Join us and explore endless possibilities.
        </p>
        <div class="cta-buttons">
          <button @click="openLoginModal">Login</button>
          <button @click="exploreMore">Explore More</button>
        </div>
      </div>
    </section>
    <!-- Features Section -->
    <section id="features" class="fade-in">
      <h2>Features</h2>
      <div class="feature-cards">
        <div
          class="feature-card"
          v-for="(feature, index) in features"
          :key="feature.id"
          @mouseover="hoverCard(index)"
          @mouseleave="leaveCard(index)"
          :class="{ hovered: hoveredCard === index }"
        >
          <img :src="feature.image" alt="Feature Image" class="feature-image" />
          <h3>{{ feature.title }}</h3>
          <p>{{ feature.description }}</p>
        </div>
      </div>
    </section>

    <!-- Contact Section -->
    <section id="contact" class="fade-in">
      <h2>Contact Us</h2>
      <p>Have any questions? Feel free to reach out to us!</p>
      <button @click="scrollToContactForm">Get in Touch</button>
    </section>

    <!-- Login Modal -->
    <div v-if="showLogin" class="login-modal fade-in">
      <Login @authenticated="handleAuthenticated" />
    </div>
  </div>
</template>

<script>
import Login from "./components/Login.vue"; // Ensure this points to the correct Login component

export default {
  name: "LandingPage",
  components: {
    Login,
  },
  data() {
    return {
      showLogin: false,
      cursorColor: "rgba(10, 45, 248, 0.842)",
      cursorBorderColor: "rgb(231, 11, 11)",
      cursorSize: "20px",
      borderSize: "50px",
      isVisible: true,
      hoveredCard: null,
      features: [
        {
          id: 1,
          title: "Feature 1",
          description: "Description of feature 1.",
          image: require("@/assets/aap.jpg"), // Replace with your actual image paths
        },
        {
          id: 2,
          title: "Feature 2",
          description: "Description of feature 2.",
          image: require("@/assets/ford.jpg"), // Replace with your actual image paths
        },
        {
          id: 3,
          title: "Feature 3",
          description: "Description of feature 3.",
          image: require("@/assets/axis.png"), // Replace with your actual image paths
        },
        // Add more features as needed
      ],
    };
  },
  methods: {
    // Function to generate a random color
    getRandomColor() {
      const letters = "0123456789ABCDEF";
      let color = "#";
      for (let i = 0; i < 6; i++) {
        color += letters[Math.floor(Math.random() * 16)];
      }
      return color;
    },
    // Function to change cursor color randomly
    changeCursorColor() {
      this.cursorColor = this.getRandomColor(); // Set random cursor color
      this.cursorBorderColor = this.getRandomColor(); // Set random border color
    },
    openLoginModal() {
      this.showLogin = true; // Open login modal
      this.changeCursorColor(); // Change cursor color on button click
    },
    exploreMore() {
      window.scrollTo({
        top: document.getElementById("features").offsetTop,
        behavior: "smooth",
      });
      this.changeCursorColor(); // Change cursor color on button click
    },
    hoverCard(index) {
      this.hoveredCard = index; // Set the index of the hovered card
    },
    leaveCard() {
      this.hoveredCard = null; // Reset the hovered card index
    },
    scrollToContactForm() {
      window.scrollTo({
        top: document.getElementById("contact").offsetTop,
        behavior: "smooth",
      });
      this.changeCursorColor(); // Change cursor color on button click
    },
    handleAuthenticated(authenticated) {
      if (authenticated) {
        console.log("User authenticated!");
        this.showLogin = false;
      }
    },
    updateCursor(event) {
      const cursor = document.getElementById("cursor");
      const cursorBorder = document.getElementById("cursor-border");

      // Update cursor position
      cursor.style.top = `${event.clientY}px`;
      cursor.style.left = `${event.clientX}px`;
      cursorBorder.style.top = `${event.clientY}px`;
      cursorBorder.style.left = `${event.clientX}px`;
    },
    hideCursor() {
      this.isVisible = false; // Hide cursor on mouse leave
    },
    showCursor() {
      this.isVisible = true; // Show cursor on mouse enter
    },
  },
};
</script>

<style scoped>
#landing-page {
  font-family: Arial, sans-serif;
}

/* Hide default cursor */
* {
  cursor: none; /* Hides the default cursor */
}

/* Custom Cursor Styles */
#cursor {
  position: fixed;
  top: 0;
  left: 0;
  border-radius: 50%;
  pointer-events: none;
  z-index: 999;
  transition: width 0.15s ease-out, height 0.15s ease-out,
    background-color 0.15s ease-out;
  transform: translate(-50%, -50%); /* Center the cursor on the mouse pointer */
}

/* Cursor border */
#cursor-border {
  position: fixed;
  border-radius: 50%;
  pointer-events: none;
  transition: top 0.15s ease-out, left 0.15s ease-out, width 0.15s ease-out,
    height 0.15s ease-out, box-shadow 0.15s ease-out;
  z-index: 998;
  transform: translate(-50%, -50%); /* Center the border on the mouse pointer */
}
/* Hero Section */
#hero {
  background: linear-gradient(
      135deg,
      rgba(164, 25, 182, 0.548),
      rgba(0, 204, 255, 0.8)
    ),
    url("assets/hero-bg.png") no-repeat center center/cover;
  height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  color: white;
  text-align: center;
}
.hero-logo {
  max-width: 150px; /* Maximum width */
  height: auto; /* Keep aspect ratio */
  margin-bottom: 20px; /* Space below the logo */
  display: block; /* Ensure it behaves as a block element */
  margin-left: auto; /* Center the logo */
  margin-right: auto; /* Center the logo */
}
.hero-content {
  background: rgba(0, 0, 0, 0.5);
  padding: 40px;
  border-radius: 10px;
  box-shadow: 0 4px 15px rgba(0, 0, 0, 0.5); /* Shadow effect */
}
h1 {
  font-size: 3rem;
  margin-bottom: 20px;
}
p {
  font-size: 1.25rem;
  margin-bottom: 30px;
}
.cta-buttons button {
  background-color: #007bff;
  color: white;
  border: none;
  padding: 15px 30px;
  margin: 10px;
  font-size: 1rem;
  border-radius: 5px;
  cursor: none;
  transition: background-color 0.3s; /* Transition effect */
}
.cta-buttons button:hover {
  background-color: #0056b3;
}
/* Features Section */
#features {
  padding: 50px 0;
  background-color: #f8f9fa; /* Light grey background for the section */
  text-align: center;
  background: linear-gradient(
    135deg,
    rgba(164, 25, 182, 0.548),
    rgba(0, 204, 255, 0.8)
  );
}

#features h2 {
  font-size: 2.5rem;
  margin-bottom: 40px;
  color: #007bff; /* Feature title color */
}

/* Feature Cards */
.feature-card {
  padding: 20px;
  margin: 20px auto;
  width: 80%; /* or a fixed width like 300px */
  max-width: 400px; /* Ensure it doesn’t get too wide */
  background-color: #6c757d; /* Attractive grey background */
  color: white; /* Text color */
  border-radius: 10px;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.3); /* Shadow effect */
  transition: transform 0.3s, background-color 0.3s; /* Smooth transition for hover effect */
}

/* Hover Effect */
.feature-card:hover {
  transform: scale(1.05); /* Slightly scale up on hover */
  background-color: #007bff; /* Change background color on hover */
}

/* Feature Headings */
.feature-card h3 {
  font-size: 1.5rem;
  margin-bottom: 10px;
}

/* Feature Descriptions */
.feature-card p {
  font-size: 1rem;
}

.feature-image {
  width: 100%; /* Ensure the image fills the card */
  border-radius: 10px; /* Round the image corners */
}
/* Contact Section */
#contact {
  padding: 50px 0;
  text-align: center;
  background-color: #6c757d;
  color: white;
}
#contact h2 {
  font-size: 2.5rem;
  margin-bottom: 20px;
}
#contact p {
  font-size: 1.25rem;
  margin-bottom: 30px;
}
#contact button {
  background-color: #007bff;
  color: white;
  border: none;
  padding: 15px 30px;
  font-size: 1rem;
  border-radius: 5px;
  cursor: pointer;
  transition: background-color 0.3s; /* Transition effect */
}
#contact button:hover {
  background-color: #0056b3;
}
</style>
