<template>
  <h1>Home</h1>
  <main class="container">
    <section class="mb-5">
      <form class="border rounded-3 p-4" @submit.prevent="crearCita">
        <div class="row mb-3">
          <div class="col-3">
            <label for="paciente" :class="[!currentAppointment.paciente ? 'text-danger' : '']">Paciente</label>
            <input type="text" class="form-control" id="paciente" v-model="currentAppointment.paciente">
          </div>
          <div class="col-2">
            <label for="fecha" :class="[!currentAppointment.fecha ? 'text-danger' : '']">Fecha</label>
            <input type="date" class="form-control" id="fecha" v-model="currentAppointment.fecha">
          </div>
          <div class="col-2">
            <label for="hora" :class="[!currentAppointment.hora ? 'text-danger' : '']">Hora</label>
            <input type="time" class="form-control" id="hora" v-model="currentAppointment.hora">
          </div>
          <div class="col-2">
            <label for="gravedad" :class="[!currentAppointment.gravedad ? 'text-danger' : '']">Gravedad</label>
            <select class="form-control" id="gravedad" v-model="currentAppointment.gravedad">
              <option disabled value="" selected>Selecciona una opción</option>
              <option :value="gravedad" v-for="(gravedad, index) in gravedades" :key="index">{{ gravedad }}</option>
            </select>
          </div>
          <div class="col-3">
            <label for="motivo" :class="[!currentAppointment.motivo ? 'text-danger' : '']">Motivo</label>
            <input type="text" class="form-control" id="motivo" v-model="currentAppointment.motivo">
          </div>
        </div>
        <div class="d-flex justify-content-center">
          <button type="submit" class="btn btn-primary" :class="{ disabled: buttonDisabled }">Agregar</button>
        </div>
      </form>
    </section>

    <section>
      <div v-if="citas.length">
        <div class="row">
          <div class="col-12 col-md-6 col-lg-3" v-for="(cita, index) in citas" :key="index">
            <CardCitaComp :cita="cita" :index="index" @delete="deleteCita" />
          </div>
        </div>
      </div>
      <div v-else>
        <p class="text-danger text-center">Aún no hay citas registradas.</p>
      </div>
    </section>
  </main>
</template>

<script>
import CardCitaComp from '@/components/CardCitaComp.vue'

export default {
  name: 'App',
  data() {
    return {
      currentAppointment: {
        paciente: '',
        fecha: '',
        hora: '',
        gravedad: '',
        motivo: '',
      },
      gravedades: ['Baja', 'Media', 'Alta'],
      citas: []
    }
  },
  computed: {
    buttonDisabled() {
      return !this.currentAppointment.paciente || !this.currentAppointment.fecha || !this.currentAppointment.hora || !this.currentAppointment.gravedad || !this.currentAppointment.motivo;
    }
  },
  methods: {
    crearCita() {
      let newCita = { ...this.currentAppointment };
      this.citas.push(newCita);
      // Vaciar formulario
      this.currentAppointment = {
        paciente: '',
        fecha: '',
        hora: '',
        gravedad: '',
        motivo: ''
      };
    },
    deleteCita(index) {
      this.citas.splice(index, 1)
    }
  },
  components: {
    CardCitaComp
  }

}
</script>

<style></style>
