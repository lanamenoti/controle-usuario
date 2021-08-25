<template>
  <div id="app">
    <h1>Controle de usuários</h1>
    <div>
      <h4>Listagem</h4>
      <ul>
        <li v-for="usuario in usuarios" :key="usuario._id">
          {{ usuario.firstName }}
        </li>
      </ul>
    </div>
    <div>
      <hr />
      <h4>Cadastro</h4>
      <div>
        <label>Primeiro nome</label>
        <input type="text" v-model="firstName" />
      </div>
      <div>
        <label>Sobrenome</label>
        <input type="text" v-model="lastName" />
      </div>
      <div>
        <label>Idade</label>
        <input type="text" v-model="age" />
      </div>
      <div>
        <label>username</label>
        <input type="text" v-model="username" />
      </div>
      <div>
        <label>password</label>
        <input type="password" v-model="password" />
      </div>
      <button @click="addUsuario()">Enviar</button>
      {{ message }}
    </div>
  </div>
</template>

<script>
export default {
  name: "App",
  data: function() {
    return {
      usuarios: [],
      firstName: "",
      lastName: "",
      age: "",
      username: "",
      password: "",
      message: "",
    }
  },
  methods: {
    getUsuarios: async function() {
      const result = await fetch("http://localhost:3000/")
        .then((res) => res.json())
        .catch((erro) => {
          return {
            erro: true,
            message: erro,
          }
        })

      if (!result.erro) {
        this.usuarios = result
      }
    },
    addUsuario: async function() {
      const novoUsuario = {
        firstName: this.firstName,
        lastName: this.lastName,
        age: this.age,
        username: this.username,
        password: this.password,
      }

      if (novoUsuario.firstName === "") {
        this.message = "Primeiro nome é obrigatório"
        return
      }
      if (novoUsuario.lastName === "") {
        this.message = "Sobrenome nome é obrigatório"
        return
      }
      if (novoUsuario.age === "") {
        this.message = "Idade nome é obrigatória"
        return
      }
      if (novoUsuario.username === "") {
        this.message = "Username nome é obrigatório"
        return
      }
      if (novoUsuario.password === "") {
        this.message = "Senha nome é obrigatório"
        return
      }

      const result = await fetch("http://localhost:3000", {
        headers: {
          Accept: "application/json",
          "Content-Type": "application/json",
        },
        method: "POST",
        body: JSON.stringify(novoUsuario),
      })
        .then((res) => res.json())
        .catch((erro) => {
          return {
            erro: true,
            message: erro,
          }
        })

      if (!result.erro) {
        this.message = "Usuário cadastrado com sucesso!"
        this.getUsuarios()
      }

      console.log(result)
    },
  },
  created: function() {
    this.getUsuarios()
  },
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  padding: 30px;
}
label {
  display: block;
}
input {
  height: 30px;
  width: 300px;
  margin-bottom: 30px;
}
</style>
