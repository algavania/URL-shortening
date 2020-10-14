<template>
  <nav>
    <div class="main-nav">
      <img src="@/assets/images/logo.svg" alt="Logo" />
      <i class="fa fa-bars" @click="showNav"></i>
    </div>
    <div id="nav-card">
      <div class="flex-container">
        <div class="nav-section">
          <ul>
            <li v-for="nav in navs" :key="nav.text">
              <a :href="nav.to">{{ nav.text }}</a>
            </li>
          </ul>
        </div>
        <hr />
        <div class="nav-login">
          <ul>
            <li
              v-for="log in logs"
              :key="log.text"
              :class="log.text === 'Sign Up' ? 'button' : ''"
            >
              <a :href="log.to">{{ log.text }}</a>
            </li>
          </ul>
        </div>
      </div>
    </div>
  </nav>
</template>

<script>
export default {
  name: "Navbar",
  data() {
    return {
      navs: [
        { text: "Features", to: "#" },
        { text: "Pricing", to: "#" },
        { text: "Resources", to: "#" },
      ],
      logs: [
        { text: "Login", to: "#" },
        { text: "Sign Up", to: "#" },
      ],
    };
  },
  methods: {
    showNav() {
      var card = document.getElementById("nav-card");
      if (card.style.display != "block") {
        card.style.display = "block";
      } else card.style.display = "none";
    },
  },
};
</script>

<style>
.main-nav {
  margin: 1.5rem 1rem;
  display: flex;
  justify-content: space-between;
  position: relative;
}

i {
  margin-top: 0.4rem;
  cursor: pointer;
}

#nav-card {
  color: white;
  background-color: var(--dark-violet);
  border-radius: 10px;
  margin: auto;
  left: 1rem;
  right: 1rem;
  position: absolute;
  padding: 1rem;
  display: none;
  z-index: 99;
}

ul {
  text-align: center;
}

ul li {
  list-style: none;
  margin: 1.5rem auto;
}

ul li a {
  text-decoration: none;
  font-size: 18px;
  color: white;
}

hr {
  background-color: white;
}

.button {
  background-color: var(--cyan);
  padding: 0.8rem;
  border-radius: 30px;
  cursor: pointer;
}

.button:hover {
  background-color: var(--cyan-hover);
}

@media only screen and (min-width: 900px) {
  nav {
    display: flex;
    align-content: center;
    width: 80%;
    margin: auto;
  }

  .main-nav {
    display: inline-block;
  }

  .main-nav img {
    vertical-align: middle;
  }

  .main-nav i {
    display: none;
  }

  .flex-container {
    display: flex;
    justify-content: space-between;
  }

  #nav-card {
    display: block;
    width: auto;
    position: static;
    margin: 0;
    flex-grow: 1;
    padding: 0;
    background-color: transparent;
  }

  .nav-section ul li {
    display: inline-block;
    margin-left: 2.5rem;
  }

  ul li a {
    color: var(--grayish-violet);
    font-weight: 700;
  }

  ul li a:hover {
    color: var(--dark-violet);
  }

  .button a {
    color: white;
  }

  .button a:hover {
    color: white;
  }

  .button {
    padding: 0.5rem 1.5rem;
  }

  hr {
    display: none;
  }

  .nav-section ul li:hover {
    color: var(--dark-violet);
  }

  .nav-login ul li {
    display: inline-block;
    margin-top: 0.7rem;
    margin-left: 2.5rem;
  }
}
</style>