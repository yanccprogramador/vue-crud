<template>
  <div class="container">
  <div class="row">
    <div class="col-md-3"></div>
    <div class="col-md-6" >
    <form v-on:submit.prevent="create">
    <div class="form-group">
      <label for="nome">Nome</label>
      <input type="text" id="nome" v-model="product.nome">
    </div>
    <div class="form-group">
      <label for="quantidade">Quantidade</label>
      <input type="text" id="quantidade" v-model="product.quantidade" >

    </div>
    <button class="btn btn-primary" >Salvar</button>
    </form>
    <table class="table">
      <thead>
          <td>Quantidade</td>
          <td>Nome</td>
      </thead>
      <tr v-for="product in txt">
                    <td>{{ product.quantidade }}</td>
                    <td>{{ product.nome }}</td>
                    <td><button class="btn btn-danger" v-on:click="del(product.id)">Excluir</button></td>

      </tr>
    </table>
  </div>
  </div>
</div>
</template>

<script>
export default {
  name: 'hello',
  data () {
    return {
      msg: 'Welcome to Your Vue.js App',
      txt:[],
      product:{}
    }
  },
  created:function(){
      this.fetchProductData();
  },
  methods: {
            fetchProductData: function()
            {
                this.$http.get('http://localhost:3000/api/products').then((response) => {
                    this.txt = response.body;

                }, (response) => {
                });
            },
          del: function(id)
          {
              this.$http.delete('http://localhost:3000/api/products/'+id).then((response) => {
                      this.fetchProductData();

              }, (response) => {
              });
          },
        create: function()
        {
            this.$http.post('http://localhost:3000/api/products/', this.product, {
                    headers : {
                        'Content-Type' : 'application/json'
                    }
                }).then((response) => {
                    this.fetchProductData();

            }, (response) => {
            });
        }
      }

}
</script>
