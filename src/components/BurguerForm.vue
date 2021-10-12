<template>
  <div>
    <Message :msg="msg" v-show="msg"/>

    <form id="burguer-form" @submit="createBurger">
      <div class="input-container">
        <label for="nome"
          ><i class="fas fa-pencil-alt"></i> Nome do cliente</label
        >
        <input
          type="text"
          name="nome"
          id="nome"
          v-model="nome"
          autocomplete="off"
          placeholder="Digite o seu nome"
        />
      </div>

      <div class="input-container">
        <label for="pao"
          ><i class="fas fa-bread-slice"></i> Escolha o pão</label
        >
        <select name="pao" id="pao" v-model="pao">
          <option value="">Selecione o seu pão</option>
          <option v-for="pao in paes" :key="pao.id" :value="pao.tipo">
            {{ pao.tipo }}
          </option>
        </select>
      </div>

      <div class="input-container">
        <label for="carne"
          ><i class="fas fa-drumstick-bite"></i> Escolha a carne</label
        >
        <select name="carne" id="carne" v-model="carne">
          <option value="">Selecione a carne do seu Burguer</option>
          <option v-for="carne in carnes" :key="carne.id" :value="carne.tipo">
            {{ carne.tipo }}
          </option>
        </select>
      </div>

      <div id="opcionais-container" class="input-container">
        <label for="opcionais" id="opcionais-title"
          ><i class="fas fa-utensils"></i> Escolha os opcionais</label
        >
        <div
          class="checkbox-container"
          v-for="opcional in opcionaisdata"
          :key="opcional.id"
        >
          <input
            type="checkbox"
            name="opcionais"
            v-model="opcionais"
            :value="opcional.tipo"
          />
          <span>{{ opcional.tipo }}</span>
        </div>
      </div>

      <div class="input-container">
        <input type="submit" class="submit-btn" value="Criar meu Burguer" />
      </div>
    </form>
  </div>
</template>

<script>
import Message from "./Messege.vue";

export default {
  name: "BurguerForm",
  data() {
    return {
      paes: null,
      carnes: null,
      opcionaisdata: null,
      nome: null,
      pao: null,
      carne: null,
      opcionais: [],
      msg: null,
    };
  },

  components: {
    Message,
  },

  methods: {
    async getIngredientes() {
      const req = await fetch("http://localhost:3000/ingredientes");
      const data = await req.json();

      this.paes = data.paes;
      this.carnes = data.carnes;
      this.opcionaisdata = data.opcionais;
    },

    async createBurger(e) {
      e.preventDefault();

      const data = {
        nome: this.nome,
        pao: this.pao,
        carne: this.carne,
        opcionais: Array.from(this.opcionais),
        status: "Solicitado",
      };

      const dataJson = JSON.stringify(data);

      const req = await fetch("http://localhost:3000/burgers", {
        method: "POST",
        headers: { "Content-Type": "application/json" },
        body: dataJson,
      });

      const res = await req.json();

      //Colocar uma msg de sistema
        this.msg = `Pedido N° ${res.id} realizado com sucesso`

      //Limpar msg
      setTimeout(() => this.msg = "", 3000)

      // Limpar os campos do formulário
        this.nome = "",
        this.pao = "",
        this.carne = "",
        this.opcionais = "";
    },
  },

  mounted() {
    this.getIngredientes();
  },
};
</script>

<style scoped>
#burguer-form {
  max-width: 400px;
  margin: 0 auto;
}

.input-container {
  display: flex;
  flex-direction: column;
  margin-bottom: 20px;
}

label {
  font-weight: bold;
  margin-bottom: 15px;
  color: #222;
  padding: 5px 10px;
  border-left: 4px solid #fcba03;
}

input,
select {
  padding: 5px 10px;
  width: 400px;
  outline-color: #fcba03;
}

#opcionais-container {
  flex-direction: row;
  flex-wrap: wrap;
}

#opcionais-title {
  width: 100%;
}

.checkbox-container {
  display: flex;
  align-items: flex-start;
  width: 50%;
  margin-bottom: 20px;
}

.checkbox-container span,
.checkbox-container input {
  width: auto;
}

.checkbox-container span {
  margin-left: 6px;
  font-weight: bold;
}

.submit-btn {
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

.submit-btn:hover {
  background: transparent;
  color: #222;
}
</style>
