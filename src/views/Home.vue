<template>
  <div id="home">
    <main>
      <header>
        <div class="container">
          <img
            class="img-working"
            src="@/assets/images/illustration-working.svg"
            alt="Illustration"
          />
        </div>
        <div class="container">
          <h1>More than just shorter links</h1>
          <p>
            Build your brand’s recognition and get detailed insights on how your
            links are performing.
          </p>
          <button class="btn-get-started">Get Started</button>
        </div>
      </header>
      <div class="container">
        <div class="main-card">
          <div class="main-card-content">
            <input
              id="form-input"
              type="text"
              aria-label="Link Input"
              placeholder="Shorten a link here..."
            />
            <p>Please add a link</p>
          </div>
          <button class="btn-shorten-it" @click="shortenUrl">
            Shorten It!
          </button>
        </div>
        <div class="card" v-for="(form, index) in forms" :key="form.newUrl">
          <p>
            {{ form.url }}
          </p>
          <hr />
          <div class="card-content">
            <p>{{ form.newUrl }}</p>
            <div class="btn-container">
              <button class="btn-copy" @click="copyUrl(form.newUrl, index)">
                Copy
              </button>
            </div>
          </div>
        </div>
        <div class="main-section">
          <h2>Advanced Statistics</h2>
          <p>
            Track how your links are performing across the web with our advanced
            statistics dashboard.
          </p>
          <div class="cards">
            <div class="card-section" v-for="card in cards" :key="card.title">
              <div class="card-description">
                <h3>{{ card.title }}</h3>
                <p>
                  {{ card.desc }}
                </p>
              </div>
              <div class="icon-section">
                <img
                  :src="require('@/assets/images/' + card.icon)"
                  :alt="card.title"
                />
              </div>
            </div>
          </div>
        </div>
      </div>
      <div class="container">
        <h2>Boost your links today</h2>
        <button class="btn-get-started">Get Started</button>
      </div>
    </main>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "Home",
  data() {
    return {
      forms: [],
      cards: [
        {
          title: "Brand Recognition",
          desc:
            "Boost your brand recognition with each click. Generic links don’t mean a thing. Branded links help instil confidence in your content.",
          icon: "icon-brand-recognition.svg",
        },
        {
          title: "Detailed Records",
          desc:
            "Gain insights into who is clicking your links. Knowing when and where people engage with your content helps inform better decisions.",
          icon: "icon-detailed-records.svg",
        },
        {
          title: "Fully Customizable",
          desc:
            "Improve brand awareness and content discoverability through customizable links, supercharging audience engagement.",
          icon: "icon-fully-customizable.svg",
        },
      ],
    };
  },
  methods: {
    shortenUrl() {
      var form = document.getElementById("form-input");
      var error = document
        .getElementsByClassName("main-card")[0]
        .getElementsByTagName("p")[0];
      if (form.value.trim() === "") {
        error.style.display = "block";
        error.innerHTML = "Please add a link";
        form.style.border = "2px solid var(--red)";
      } else {
        error.style.display = "none";
        form.style.border = "none";
        axios
          .post("https://rel.ink/api/links/", {
            url: form.value,
          })
          .then(
            (res) => {
              var url = form.value;
              var newUrl = "https://rel.ink/" + res.data.hashid;
              var duplicate = this.duplicateUrls(newUrl);
              if (!duplicate || this.forms.length === 0) {
                this.forms.push({ url, newUrl });
                localStorage.setItem('data', JSON.stringify(this.forms));
                form.value = "";
                error.style.display = "none";
                form.style.border = "none";
              } else {
                error.style.display = "block";
                error.innerHTML = "You already shorten this link";
                form.style.border = "2px solid var(--red)";
              }
            },
            (err) => {
              error.style.display = "block";
              error.innerHTML = "Please enter a valid URL";
              form.style.border = "2px solid var(--red)";
              console.log(err);
            }
          );
      }
    },
    duplicateUrls(newUrl) {
      const isDuplicate = this.forms.some((url) => {
        return url.newUrl === newUrl;
      });
      return isDuplicate;
    },
    copyUrl(value, index) {
      var button = document.getElementsByClassName("btn-copy")[index];
      button.style = "background-color: var(--very-dark-blue)";
      button.textContent = "Copied!";

      var dummy = document.createElement("textarea");
      document.body.appendChild(dummy);
      dummy.value = value;
      dummy.select();
      document.execCommand("copy");
      document.body.removeChild(dummy);

      setTimeout(() => {
        button.style = "background-color: var(--cyan)";
        button.textContent = "Copy";
      }, 3000);
    },
  },
  mounted() {
    var array = JSON.parse(localStorage.getItem('data'));
    if (array != null) {
      for (let index = 0; index < array.length; index++) {
      var url = array[index].url;
      var newUrl = array[index].newUrl;
      this.forms.push({ url, newUrl });
    }
    console.log(this.forms);
    }
  }
};
</script>

<style scoped>
body {
  height: 100%;
  width: 100%;
  font-size: 18px;
}

.container {
  margin: auto 1rem;
  text-align: center;
}

.img-working {
  width: 100%;
}

.container h1 {
  color: var(--very-dark-blue);
  margin-top: 1rem;
  line-height: 2.6rem;
}

.container p {
  margin-top: 0.5rem;
  color: var(--grayish-violet);
}

header .container:nth-child(2) {
  margin-left: 1rem;
  margin-right: 1rem;
}

button {
  outline: none;
  border: none;
  padding: 0.8rem;
  color: white;
  background: var(--cyan);
  font-size: 1rem;
  cursor: pointer;
  border-radius: 50px;
  width: 50%;
}

button:hover,
.btn-copy:hover {
  background: var(--cyan-hover);
}

.btn-get-started {
  margin-top: 1.5rem;
}

.container:nth-child(2) {
  background-image: linear-gradient(270deg, #f0f1f6, #f0f1f6);
  background-repeat: no-repeat;
  background-position: 0 80px;
  margin: 5rem auto;
  margin-bottom: 0;
  position: relative;
  padding-bottom: 7rem;
}

.main-card {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-content: center;
  margin: auto 1rem;
  background-color: var(--dark-violet);
  background-image: url(../assets/images/bg-shorten-mobile.svg);
  background-repeat: no-repeat;
  background-position: top right;
  border-radius: 10px;
  padding: 1.5rem;
}

.main-card-content p {
  text-align: start;
  font-style: italic;
  color: var(--red);
  display: none;
}

.main-card-content input {
  border-radius: 5px;
  padding: 0.8rem 1rem;
  width: 100%;
  font-size: 18px;
  border: none;
  outline: none;
}

.btn-shorten-it {
  margin-top: 1rem;
  border-radius: 5px;
  width: 100%;
}

.card {
  margin: 1rem;
  margin-top: 1.5rem;
  border-radius: 5px;
  background-color: white;
}

.card p {
  text-align: start;
  margin: 0 1rem;
  padding: 0.5rem;
  color: var(--cyan);
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
  max-width: 100%;
  font-size: 18px;
}

.card-content p {
  color: var(--cyan) !important;
}

header .container:nth-child(2) {
  background-image: none;
}

.card p:nth-child(1) {
  color: var(--dark-violet);
}

.card button {
  width: 100%;
  border-radius: 5px;
  font-size: 18px;
  padding: 0.5rem;
}

.btn-container {
  padding: 0 1.5rem;
  padding-bottom: 1rem;
}

.main-section {
  margin-top: 5rem;
}

.card-section {
  background-color: white;
  border-radius: 10px;
  margin: 1rem;
  margin-top: 7rem;
  position: relative;
  padding: 1rem;
}

.icon-section {
  background-color: var(--dark-violet);
  width: fit-content;
  height: 80px;
  display: flex;
  width: 80px;
  border-radius: 50%;
  align-items: center;
  justify-content: center;
  align-content: center;
  position: absolute;
  top: -2.5rem;
  margin: auto;
  left: 0;
  right: 0;
}

.icon-section img {
  height: 40px;
  width: 40px;
}

.card-description {
  margin-top: 2.5rem;
  margin-bottom: 1rem;
}

.card-section::after {
  content: "";
  z-index: 0;
  position: absolute;
  background-color: var(--cyan);
  width: 10px;
  height: 100%;
  margin: 1rem auto;
  left: 0;
  right: 0;
}

.card-section:last-child::after {
  display: none;
}

.container:nth-child(3) {
  margin: 0;
  background-color: var(--dark-violet);
  color: white;
  padding: 5rem 1rem;
  background-image: url(../assets/images/bg-boost-mobile.svg);
  background-repeat: no-repeat;
  background-position: top right;
}

.btn-get-started:last-child {
  margin-top: 1rem;
  padding: 0.7rem;
  font-size: 18px;
}

@media only screen and (min-width: 900px) {
  header {
    margin-bottom: 5rem;
    display: flex;
    flex-direction: row-reverse;
    justify-content: center;
    align-content: center;
    align-items: center;
    margin: auto;
    width: 80%;
  }

  header .container {
    text-align: start;
  }

  header .container:first-child {
    width: 50%;
  }

  header .container:last-child {
    width: 50%;
  }

  header .btn-get-started:last-child {
    width: auto;
    padding: 0.8rem 2rem;
    margin-top: 2rem;
  }

  header .container h1 {
    font-weight: 700;
    text-align: start;
    line-height: 5.5rem;
    font-size: 70px;
  }

  header .container p {
    font-size: 20px;
    margin-right: 5rem;
  }

  .main-card {
    width: 80%;
    margin: auto;
    display: flex;
    flex-direction: row;
    padding: 3rem;
    position: relative;
    background-image: url(../assets/images/bg-shorten-desktop.svg);
    background-size: cover;
  }

  .main-card button {
    width: 20%;
    height: 100%;
    margin: 0;
    margin-left: 1rem;
  }

  button {
    font-weight: 700;
    font-size: 18px;
    letter-spacing: 1px;
  }

  .main-section h2 {
    font-size: 40px;
  }

  .main-section p {
    font-size: 18px;
  }

  .card-section {
    width: 30%;
    margin-left: auto;
    margin-right: auto;
    text-align: start;
    padding: 2rem;
  }

  .card-section h3 {
    font-size: 25px;
  }

  .cards {
    display: flex;
    width: 80%;
    margin: auto;
    align-items: flex-start;
  }

  .card-section::after {
    height: 10px;
    width: 100%;
    top: 0;
    left: 100%;
    bottom: 0;
    margin-top: auto;
    margin-bottom: auto;
  }

  .card-section:nth-child(2) {
    margin-top: 10rem;
  }

  .card-section:last-child {
    margin-top: 15rem;
  }

  .icon-section {
    margin: 0;
    left: 2rem;
  }

  .container:nth-child(3) {
    background-image: url(../assets/images/bg-boost-desktop.svg);
    background-size: cover;
    padding: 3rem 1rem;
  }

  .container:nth-child(3) h2 {
    font-size: 40px;
  }

  .btn-get-started:last-child {
    width: 200px;
  }

  .card {
    width: 80%;
    margin-left: auto;
    margin-right: auto;
    justify-content: space-between;
    align-items: center;
    align-content: center;
    display: flex;
    padding: 1rem;
  }

  .card-content {
    display: flex;
    justify-content: space-between;
    align-items: center;
    align-content: center;
  }

  .btn-container {
    padding: 0;
  }

  .card button {
    width: 120px;
    margin-right: 2rem;
  }

  .main-card-content {
    width: 80%;
    height: 100%;
    display: flex;
    flex-direction: column;
  }
}

@media only screen and (max-width: 300px) {
  button {
    font-size: 15px!important;
  }
}
</style>