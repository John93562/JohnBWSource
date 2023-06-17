<template>
  <div id="container">
    <Transition mode="out-in">
      <div v-if="view == 'normal-view'" id="normal-view">
        <h3>Tu Es Invité à la <span>Semaine Anniversaire de Ioanni</span></h3>
        <button id="cta-button" @click="view = 'events-view'">
          <h5>Voir la liste des évènements</h5>
        </button>
      </div>
      <div
        v-else-if="view == 'events-view'"
        id="events-view"
        :class="canRegister ? 'registering' : ''"
      >
        <div id="events-view-top-bar">
          <img src="/images/flower.svg" width="70" style="rotate: 180deg" />
          <img src="/images/top-title.svg" width="400" />
          <img src="/images/flower.svg" width="70" />
        </div>
        <div id="events-view-events" v-if="canRegister">
          <div
            class="events-view-event event-clickable"
            @click="beers = !beers"
            :class="beers ? 'is-chosen' : 'is-not-chosen'"
          >
            <h4 class="events-view-event-title">MERCREDI <span>19/04/2023 ( 20:30 )</span></h4>
            <h4 class="events-view-event-content">Bières au MyBeers</h4>
          </div>
          <div
            class="events-view-event event-clickable"
            @click="hiking = !hiking"
            :class="hiking ? 'is-chosen' : 'is-not-chosen'"
          >
            <h4 class="events-view-event-title">DIMANCHE <span>23/04/2023 ( 9:00 )</span></h4>
            <h4 class="events-view-event-content">Randonnée et Pic-nique au Pic Saint-Loup</h4>
            <h4 class="events-view-event-subcontent">
              ( Je ramènerai de la nourriture grecque pour tout le monde )
            </h4>
          </div>
          <div
            class="events-view-event event-clickable"
            @click="laser = !laser"
            :class="laser ? 'is-chosen' : 'is-not-chosen'"
          >
            <h4 class="events-view-event-title">
              MARDI <span>25/04/2023 ( 19:00 ou 20:00 ou 21:00)</span>
            </h4>
            <h4 class="events-view-event-content">Soirée Laser Game</h4>
            <h4 class="events-view-event-subcontent">( On va se tuer )</h4>
          </div>
        </div>
        <div id="events-view-events" v-else>
          <div class="events-view-event">
            <h4 class="events-view-event-title">MERCREDI <span>19/04/2023 ( 20:30 )</span></h4>
            <h4 class="events-view-event-content">Bières au MyBeers</h4>
          </div>
          <div class="events-view-event">
            <h4 class="events-view-event-title">DIMANCHE <span>23/04/2023 ( 9:00 )</span></h4>
            <h4 class="events-view-event-content">Randonnée et Pic-nique au Pic Saint-Loup</h4>
            <h4 class="events-view-event-subcontent">
              ( Je ramènerai de la nourriture grecque pour tout le monde )
            </h4>
          </div>
          <div class="events-view-event">
            <h4 class="events-view-event-title">
              MARDI <span>25/04/2023 ( 19:00 ou 20:00 ou 21:00)</span>
            </h4>
            <h4 class="events-view-event-content">Soirée Laser Game</h4>
            <h4 class="events-view-event-subcontent">( On va se tuer )</h4>
          </div>
        </div>
        <button v-if="canRegister == false" id="events-view-cta-button" @click="canRegister = true">
          <h4>Enregister</h4>
        </button>
        <div v-else id="events-view-cta-button-with-name">
          <div id="events-view-cta-text-description">
            <h4>Veuillez choisir les événements auxquels vous souhaitez assister</h4>
          </div>
          <div id="events-view-cta-text-title">
            <h4>Votre nom</h4>
            <input type="text" v-model="name" />
          </div>
          <transition>
            <h4 id="fail-text" v-if="isMessageVisible">{{ message }}</h4>
          </transition>

          <button @click="RegisterUser" id="events-view-cta-button-with-name-button">
            <h4>Send To Ioannis</h4>
          </button>
        </div>
      </div>
      <div v-else-if="view == 'thank-you-view'" id="thank-you-view">
        <h3>Merci de votre réponse</h3>
        <h4>Vous pouvez m'envoyer un message si vous souhaitez modifier votre réponse.</h4>
      </div>
    </Transition>
  </div>
</template>

<script>
import emailjs from 'emailjs-com'

export default {
  name: 'JBApp',
  data() {
    return {
      view: 'normal-view',

      isMessageVisible: false,
      message: '',
      isApplying: false,
      name: '',
      canRegister: false,
      beers: false,
      hiking: false,
      laser: false
    }
  },
  methods: {
    RegisterUser() {
      try {
        if (!this.name) {
          this.message = 'Veuillez saisir un nom'
          this.isMessageVisible = true
          return
        }
        if (this.beers == false && this.hiking == false && this.laser == false) {
          this.message =
            'Veuillez sélectionner au moins un événement auquel vous souhaitez participer'
          this.isMessageVisible = true
          return
        }
        this.isMessageVisible = false
        emailjs.send(
          'service_qurqjck',
          'template_92qwl4y',
          {
            from_name: this.name,
            from_beers: this.beers,
            from_hiking: this.hiking,
            from_laser: this.laser
          },
          'Lp8X1sT82xU2YFogq'
        )
        this.view = 'thank-you-view'
      } catch (error) {
        console.log({ error })
      }
    }
  }
}
</script>
