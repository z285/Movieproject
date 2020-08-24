<template>

   <div>
     
  <b-navbar toggleable="lg" type="dark" variant="secondary">
    <b-navbar-brand tag="h1" class="mb-0" @click="Get_home">
        <img src="@/assets/movie.png"  style="width:1.5em; height:1.5em;margin-right:10px" class="d-inline-block align-top" alt="Moive">
       <b> Movie Database</b></b-navbar-brand>

    <b-navbar-toggle target="nav-collapse"></b-navbar-toggle>

    <b-collapse id="nav-collapse" is-nav>
      <b-navbar-nav>
      </b-navbar-nav>

      <!-- Right aligned nav items -->
      <b-navbar-nav class="ml-auto">
        <b-nav-form>
          <b-form-input size="sm" class="mr-sm-2" placeholder="Search" v-model="movie_Search"></b-form-input>
          <b-button size="sm" class="my-2 my-sm-0" variant="outline-light" ref="modal" @click="Get_Start">Search</b-button>
             <div class="header navigation" @click="Get_Cart">

    <ul class="navbar-nav flex-row justify-content-end flex-wrap align-items-center mr-lg-4 mr-xl-0">
      <li class="nav-item px-3 text-uppercase mb-0 position-relative d-lg-flex">
        <a href="#" class="cart position-relative d-inline-flex" aria-label="View your shopping cart">
          <i class="fas fa fa-shopping-cart fa-lg" style="font-size:36px;	margin-top:3px;"></i>
          <span class="cart-basket d-flex align-items-center justify-content-center" v-if="item == true">
            {{cart_quantity}}
          </span>
        </a>
      </li>
    </ul>
 
</div>
        </b-nav-form>        
      </b-navbar-nav>
    </b-collapse>
  </b-navbar>
</div>


</template>
           

<script>
export default {
    data(){
      return{
        movie_Search : "",
        cart_quantity : 0,
        item : false
        }
    },
    methods:{  //response.data.results.length
        Get_Start : function(){
         this.$root.$emit('Movie_comp',this.movie_Search) //like this
        },
        Get_home : function(){
         this.$root.$emit('home',this.movie_Search) //like this 
        },
         Get_Cart : function(){
         this.$root.$emit('Cart',this.movie_Search) //like this
        },
        update_cart : function(data){
          this.cart_quantity = data
          this.item_cart()
        },
        getcart_item : function(){
          var quantity = 0
          var data = JSON.parse(this.$cookies.get("cookies_cart"))
          console.log(data)
          for(var i  = 0 ; i < data.length ; i++){
            quantity = quantity + parseInt(data[i].quantity)
          }
          this.cart_quantity = quantity
          this.item_cart()
        },
        item_cart : function(){
          if(this.cart_quantity == 0){
            this.item = false
          }
          else{
            this.item = true
          }
        },
    },
     mounted(){
        this.$root.$on('update_cart', (data) => {
            console.log(data)
            this.update_cart(data)
        }) 
        }
        ,
     beforeMount(){
       this.getcart_item()
 },
}
</script>
<style scoped>
.logo {
  max-height: 70px;
}

li {
  list-style: none;
}

.fa,
.fas {
  font-family: 'FontAwesome';
}

ul li a {
  font-size: 1.1rem;
  color: #000000;
}

ul li a.cart:hover {
  text-decoration: none;
  color: #FFFFFF;
}

ul li a.cart .cart-basket {
  font-size: .6rem;
  position: absolute;
  top: -6px;
  right: -5px;
  width: 15px;
  height: 15px;
  color: #fff;
  background-color: #418deb;
  border-radius: 50%;
}

</style>
