<template>
  <div id="burguer-table">
    <div>
      <div id="burguer-table-heading">
        <div class="order-id">#</div>
        <div>Cliente</div>
        <div>Pão</div>
        <div>Carne</div>
        <div>Opcionais</div>
        <div>Ações</div>
      </div>
    </div>

    <div id="burguer-table-rows">
      <div class="burguer-table-row" v-for="burguer in burguers" :key="burguer.id">
        <div class="order-number">{{burguer.id}}</div>
        <div>{{burguer.nome}}</div>
        <div>{{burguer.pao}}</div>
        <div>{{burguer.carne}}</div>

        <div>
          <ul>
            <li v-for="(opcional, index) in burguer.opcionais" :key="index">{{opcional}}</li>
          </ul>
        </div>

        <div>
          <select name="status" class="status">
            <option value="">Status</option>
          </select>
          <button class="delete-btn">Cancelar</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "Dashboard",

  data() {
    return {
      burguers: null,
      burguers_id: null,
      status: [],
    };
  },

  methods: {
    async getPedidos() {
      const req = await fetch("http://localhost:3000/burgers");

      const data = await req.json();

      this.burguers = data;

      console.log(this.burguers);

      //Resgatar status
    },
  },

  mounted() {
    this.getPedidos();
  },
};
</script>

<style scoped>
#burguer-table {
  max-width: 1200px;
  margin: 0 auto;
}

#burguer-table-heading,
#burguer-table-rows,
.burguer-table-row {
  display: flex;
  flex-wrap: wrap;
}

#burguer-table-heading {
  font-weight: bold;
  padding: 12px;
  border-bottom: 3px solid #333;
}

#burguer-table-heading div,
.burguer-table-row div {
  width: 18%;
}

.burguer-table-row {
  width: 100%;
  padding: 12px;
  border-bottom: 3px solid #ccc;
}

#burguer-table-heading .order-id,
.burguer-table-row .order-number {
  width: 5%;
}

select {
  padding: 12px 6px;
  margin-right: 12px;
  outline-color: #fcba03;
}

.delete-btn {
  background: #222;
  color: #fcba03;
  font-weight: bold;
  border: 2px solid #222;
  padding: 10px;
  font-size: 16px;
  margin: 0 auto;
  cursor: pointer;
  transition: 0.5s;
}

.delete-btn:hover {
  background: transparent;
  color: #222;
}
</style>

