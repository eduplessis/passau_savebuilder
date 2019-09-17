<template>
  <div id="savebuilder">
    <div class="shadow" v-if="open">
      <div class="lightbox">
        <button class="close" @click.prevent="open = false">fermer</button>
        <h1>Vos équipements sauvegardés</h1>
        <ul>
          <li v-for="(save, key) in storage" :key="key">
            <div class="thumb">
              <img :src="save.selection.base" alt />
              <div
                class="color"
                v-for="(color, keyColor) in save.selection.color"
                :key="key + '-' + keyColor"
              >
                <img v-if="color.imageUrl" :src="color.imageUrl" />
              </div>
            </div>
            <div>
              <div class="caption">Équipement</div>
              {{ save.selection.product }}
            </div>
            <div class="date">
              <div class="caption">Date de création</div>
              {{ savedDate(save.date) }}
            </div>
            <a class="btnsave" @click.prevent="loadEquipement(save.selection)">Voir l'équipement</a>
          </li>
        </ul>
      </div>
    </div>

    <button class="triggersave btnsave" @click.prevent="open = !open">Consultez vos équipements</button>
  </div>
</template>

<script>
export default {
  name: "savebuilder",
  data() {
    return {
      storage: false,
      open: false
    };
  },
  methods: {
    loadStorage() {
      if (localStorage.getItem("passau"))
        this.storage = JSON.parse(localStorage.getItem("passau"));
    },
    savedDate(date) {
      var local = document.querySelector("html");

      var Newdate = new Date(date);
      return Newdate.toLocaleDateString(local.lang, {
        weekday: "long",
        year: "numeric",
        month: "long",
        day: "numeric"
      });
    },
    loadEquipement(selection) {
      localStorage.setItem("loadpassau", JSON.stringify(selection));
      window.location.search =
        "?p=11&step=2&type=" +
        selection.type +
        "&family=" +
        selection.family +
        "";
    }
  },
  computed: {},
  mounted() {
    this.loadStorage();
  }
};
</script>

<style lang="scss">
html,
body {
  margin: 0;
  padding: 0;
}
.shadow {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  background: rgba(0, 0, 0, 0.2);
  width: 100%;
  height: 100%;
  position: fixed;
  top: 0;
  left: 0;
  z-index: 9998;
}
.lightbox {
  background: #fff;
  margin: 0 auto;
  padding: 30px;
  max-width: 90vw;
  width: 900px;
  text-align: center;
  position: fixed;
  z-index: 99999;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  ul {
    padding: 0;
    margin: 0;
    list-style: none;
  }
  li {
    padding: 5px 0;
    margin: 0;
    list-style: none;
    display: flex;
    justify-content: space-between;
    align-items: center;
    text-align: left;
    font-size: 12px;
    @media screen and (min-width: 768px) {
      font-size: 17px;
    }
  }
  .caption {
    font-weight: 700;
  }
  .date {
    display: none;
    @media screen and (min-width: 768px) {
      display: block;
    }
  }
}
.btnsave {
  background-color: #00adee;
  padding: 10px 15px;
  border-radius: 50px;
  width: auto;
  display: inline-block;
  font-size: 12px;
  font-weight: 300;
  font-style: normal;
  font-stretch: normal;
  line-height: normal;
  letter-spacing: normal;
  text-align: center;
  color: #ffffff;
  cursor: pointer;
  border: none;
  &:hover {
    background: #fff;
    border: 1px solid #00adee;
    color: #00adee;
  }
  @media screen and (min-width: 768px) {
    font-size: 17px;
    padding: 15px 42px;
    display: block;
  }
}
.thumb {
  width: 50px;
  height: 50px;
  background: hsl(0, 0, 85);
  border-radius: 100%;
  position: relative;

  @media screen and (min-width: 768px) {
    width: 75px;
    height: 75px;
  }

  img {
    width: 60%;
    height: 60%;
    object-fit: cover;
    transform: translate(-50%, -50%);
    left: 50%;
    top: 50%;

    position: absolute;
  }
}
.triggersave {
  position: fixed;
  bottom: 20px;
  left: 80px;
  z-index: 9999;
}
</style>
