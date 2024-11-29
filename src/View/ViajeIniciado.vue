<template>
  <div class="apartado">
    <!-- Botón de terminar viaje -->
    <button class="finish-button" @click="finishTrip">Terminar Viaje</button>

    <!-- Contenedor principal -->
    <div class="main-content">
      <div class="circle-bg"></div>
      <div class="loading-container">
        <h2>Estás en viaje</h2>
      </div>
    </div>

    <!-- Ventana emergente de calificación -->
    <transition name="popup">
      <div v-if="showFeedback" class="popup feedback">
        <h2>Califica el Viaje</h2>
        <div class="stars">
          <input type="radio" id="star5" name="rating" value="5" v-model="rating" />
          <label for="star5">&#9733;</label>
          <input type="radio" id="star4" name="rating" value="4" v-model="rating" />
          <label for="star4">&#9733;</label>
          <input type="radio" id="star3" name="rating" value="3" v-model="rating" />
          <label for="star3">&#9733;</label>
          <input type="radio" id="star2" name="rating" value="2" v-model="rating" />
          <label for="star2">&#9733;</label>
          <input type="radio" id="star1" name="rating" value="1" v-model="rating" />
          <label for="star1">&#9733;</label>
        </div>

        <textarea v-model="comment" rows="4" cols="50" placeholder="Deja un comentario"></textarea>
        <br />
        <button class="primary-button" @click="startReport">Reportar Viaje</button>
        <router-link to="/communityTSJZ/Raite" >
        <button class="primary-button" @click="submitFeedback">Enviar</button>
        </router-link>
      </div>
    </transition>

    <!-- Ventana emergente de reporte -->
    <transition name="popup">
      <div v-if="showReportForm" class="popup report-form">
        <h2>Describe el motivo del reporte</h2>
        <textarea v-model="reportReason" rows="4" cols="50" placeholder="Motivo del reporte"></textarea>
        <br />
        
          <button class="primary-button" @click="submitReport">Enviar Reporte</button>
       
        
      </div>
    </transition>

    <!-- Apartado de faltas
    <transition name="popup">
      <div v-if="showFeedback || showReportForm" class="popup report-summary">
        <h2>Perfil</h2>
        <p>Faltas acumuladas: <span class="strike-count">{{ strikeCount }}</span></p>
        <p v-if="strikeCount >= 3" class="warning">No puedes crear más viajes.</p>
      </div>
    </transition> -->

    <!-- Elemento del carrito -->
    <div id="carrito"></div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      showFeedback: false,
      showReportForm: false,
      rating: null,
      comment: "",
      reportReason: "",
      strikeCount: 0,
    };
  },
  methods: {
    finishTrip() {
      this.showFeedback = true;
    },
    startReport() {
      this.showReportForm = true;
      this.showFeedback = false;
    },
    submitReport() {
      if (this.reportReason.trim() === "") {
        alert("Por favor, describe el motivo del reporte.");
        return;
      }
      this.strikeCount++;
      alert(`Gracias por tu reporte: "${this.reportReason}".`);
      this.reportReason = "";
      this.showReportForm = false;
    },
    submitFeedback() {
      alert(`Gracias por tu retroalimentación: ${this.rating} estrellas, comentario: "${this.comment}".`);
      this.showFeedback = false;
    },
  },
};
</script>

<style>
/* Ajustes globales */
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

.apartado {
  position: relative;
  height: 100vh;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  background: linear-gradient(135deg, #201f2e, #201f2e);
  overflow: hidden;
}

/* Botón de terminar viaje */
.finish-button {
  position: absolute;
  top: 10px;
  right: 10px;
  background-color: #37109F;
  color: white;
  border: none;
  padding: 10px 15px;
  border-radius: 5px;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.finish-button:hover {
  background-color: #0056b3;
}

/* Contenido principal centrado */
.main-content {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  text-align: center;
}

.circle-bg {
  width: 300px;
  height: 300px;
  border: 10px solid rgba(255, 255, 255, 0.3);
  border-top: 10px solid #fff;
  border-radius: 50%;
  animation: spin 2s linear infinite;
  margin-bottom: 20px;
}

h2 {
  font-size: 24px;
  color: #fff;
}

/* Popup de calificación y reportes */
.popup {
  position: absolute;
  background-color: #fff;
  padding: 20px;
  border-radius: 10px;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.2);
  text-align: center;
  
}

.feedback {
  top: 20%;
}

.report-form {
  top: 40%;
}

.report-summary {
  top: 70%;
}

/* Animaciones */
@keyframes spin {
  0% {
    transform: rotate(0deg);
  }
  100% {
    transform: rotate(360deg);
  }
}

@keyframes fadeIn {
  from {
    opacity: 0;
    transform: scale(0.8);
  }
  to {
    opacity: 1;
    transform: scale(1);
  }
}

/* Botones */
.primary-button {
  background-color: #28a745;
  color: white;
  border: none;
  padding: 10px 20px;
  border-radius: 5px;
  cursor: pointer;
  margin: 10px 5px;
  transition: background-color 0.3s ease, transform 0.2s ease;
}

.primary-button:hover {
  background-color: #218838;
  transform: scale(1.05);
}

/* Estrellas */
.stars input {
  display: none;
}
.stars label {
  font-size: 2em;
  color: gray;
  cursor: pointer;
  transition: color 0.3s ease;
}
.stars input:checked ~ label,
.stars label:hover,
.stars label:hover ~ label {
  color: gold;
}

/* Contador de faltas */
.strike-count {
  font-size: 1.5em;
  font-weight: bold;
  color: #dc3545;
}

.warning {
  color: #dc3545;
  font-weight: bold;
}

/* Transición de ventanas emergentes */
.popup-enter-active,
.popup-leave-active {
  transition: all 0.5s ease;
}

.popup-enter-from,
.popup-leave-to {
  opacity: 0;
  transform: scale(0.8);
}

#carrito {
    position: fixed;
    bottom: 0;
    left: 0;
    width: 100px;
    height: 100px;
    background-image: url('https://img.icons8.com/?size=100&id=oizVkflf4uIT&format=png&color=000000');
    background-size: contain;
    background-repeat: no-repeat;
    animation: moveCarrito 4s linear infinite alternate;
}
@keyframes moveCarrito {
    0% {
        left: 0;
        transform: scaleX(1);
    }
    100% {
        left: calc(100% - 100px);
        transform: scaleX(-1);
    }
}


</style>

