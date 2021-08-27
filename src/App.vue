<template>
  <div id="app">
    <h1>Controle de usuários</h1>
    <div>
      <h4>Listagem</h4>
      <ul>
        <li v-for="usuario in usuarios" :key="usuario._id">
          {{ usuario.firstName }} {{ usuario.lastName }}
          <button @click="deletarUsuario(usuario._id)">Excluir</button>
          <button @click="getUsuarioById(usuario._id)">Alterar</button>
        </li>
      </ul>
    </div>
    <div>
      <hr />
      <h4 v-if="action === 'add'">Cadastro</h4>
      <h4 v-else>Alterar</h4>
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
      <button v-if="action === 'add'" @click="addUsuario()">Enviar</button>
      <button v-if="action === 'update'" @click="atualizarUsuario()">
        Editar
      </button>
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
      action: "add",
      idUsuario: null,
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
        await this.getUsuarios()
      }
    },
    deletarUsuario: async function(idUsuario) {
      const result = await fetch(`http://localhost:3000/${idUsuario}`, {
        method: "DELETE",
      })
        .then((res) => res.json())
        .catch((erro) => {
          return {
            erro: true,
            message: erro,
          }
        })

      if (!result.erro) {
        this.message = "Usuário deletado com sucesso!"
        await this.getUsuarios()
      }
    },
    getUsuarioById: function(idUsuario) {
      const [usuario] = this.usuarios.filter(
        (usuario) => usuario._id === idUsuario
      )

      this.firstName = usuario.firstName
      this.lastName = usuario.lastName
      this.age = usuario.age
      this.username = usuario.username
      this.password = usuario.password
      this.idUsuario = usuario._id

      this.action = "update"
    },
    atualizarUsuario: async function() {
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

      const result = await fetch(`http://localhost:3000/${this.idUsuario}`, {
        headers: {
          Accept: "application/json",
          "Content-Type": "application/json",
        },
        method: "PUT",
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
        await this.getUsuarios()
        this.message = "Usuario alterado com sucesso"
      }
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
li {
  height: 50px;
  width: 400px;
  border-bottom: 1px solid #c1c1c1;
  padding: 10px;
  box-sizing: border-box;
}

li button {
  margin-left: 10px;
  float: right;
}
</style>
