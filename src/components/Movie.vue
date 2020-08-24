<template>
  <div class="container .bg-dark" id="home">
    <div class="row">
        <br>
        <div>
          <!-- horizontal -->
    <div class="row"><center>

<div class="flip"  v-for="(data) in movie_list" :key="data">
    <div class="front" :style='{ backgroundImage: "url(" + data.image + ")", }'>
       <h1 class="text-shadow"></h1>
    </div>
    <div class="back">
       <h4>{{data.name}}</h4>
       <p><small>{{data.detail}}</small></p>
       <div class="row"  style="position: absolute; bottom: 10px;"> 
       <div class="col-sm-3"> <b-button size="md" variant="outline-success" @click="Add_cart(data,'buy')"> Buy </b-button> </div> 
       <div class="col-sm-5"> <b-button size="md" variant="outline-primary" @click="Add_cart(data)"><span><div class="fas fa fa-shopping-cart fa-lg" style="margin-right:5px"></div> </span>+ </b-button> </div> 
       <div class="col-sm-4"> 
         <div style="margin-top:6px;text-align:left;">
              <h5>$ 20</h5>
            </div>
        </div> 
       </div>
    </div>
</div>
</center>
    </div>

  </div>
 <!-- <div class="card-deck" >
  <div class="card" v-for="(data) in movie_list" :key="data" style="margin-bottom:10px">
<div class="hvrbox">
	<img class="card-img-top hvrbox-layer_bottom" :src= data.image alt="Card image cap">
	<div class="hvrbox-layer_top">
		<div class="hvrbox-text">{{data.detail}}</div>
	</div>
</div>
    
    <div class="card-body">
      <h5 class="card-title">{{data.name}}</h5>
    </div>
    <div class="card-footer">
        <div class="row"> 
       <div class="col-sm-3"> <b-button size="md" variant="success" @click="Add_cart(data,'buy')"> Buy </b-button> </div> 
       <div class="col-sm-5"> <b-button size="md" variant="warning" @click="Add_cart(data)"><span><img class="img" src="@/assets/cart.png" style="width:1em; height:1em;margin-right:5px;margin-bottom:5px"> </span>+ </b-button> </div> 
       <div class="col-sm-4"> 
         <div style="margin-top:6px;text-align:right">
              <h5>$ 20</h5>
            </div>
        </div> 
       </div>
      </div>
    </div>
  </div> -->
  <br>
    


</div>

 <div class="overflow-auto" style="margin-left: auto; margin-right: auto;display: block;">
    <div  @click="get_page">
      <b-pagination
        v-model="currentPage"
        :total-rows="rows"
        :per-page="perPage"
        align="center"
        first-number
      ></b-pagination>
    </div>
  </div>

  <b-modal id="modal-1" ref="my-modal" size="lg" title="Shopping cart">
        <b><div class="row">
          <div class="col-sm-5">
            Product
          </div>
           <div class="col-sm-2">
            Quantity
          </div>
             <div class="col-sm-1">
            Price	
          </div>
           <div class="col-sm-2">
            Total
          </div>
        </div></b>
        <br>
        <div class="row" style="margin-bottom: 7px" v-for="(data) in cart" :key="data">
          <div class="col-sm-5">
            <div class="row">
              <div class="col-sm-4">
                <img class="img responsive" :src= data.image>
              </div>
              <div class="col-sm-8">
                {{data.name}} 
              </div>
            </div>
          </div>
           <div class="col-sm-2">
              <b-form-input type="number" size="sm" class="mr-sm-2" v-model="data.quantity" v-on:change="quantity_change(data)"></b-form-input>         
               </div>
             <div class="col-sm-1">
           ${{data.price}}
          </div>
           <div class="col-sm-2">
           ${{data.total_price}}
          </div>
          <div class="col-sm-2">
             <b-button size="sm" variant="danger" @click="remove_cart(data.name)"> Remove </b-button>
          </div>
        </div>  
        <hr>  
        <div class="row">
          <div class="col-sm-8">
          </div>
          <div class="col-sm-4">
              <div class="row">
              <div class="col-sm-6">
                Subtotal
              </div>
              <div class="col-sm-6">
                ${{Subtotal}}
              </div>
            </div>  
          </div>
        </div>

        <div class="row" v-if="have_discount === true">
          <div class="col-sm-8">
          </div>
          <div class="col-sm-4">
              <div class="row">
              <div class="col-sm-6" v-if="discount_10 == true">
                10% discount
              </div>
              <div class="col-sm-6" v-if="discount_20 == true">
                20% discount
              </div>
              <div class="col-sm-6">
                ${{discount}}
              </div>
            </div>  
          </div>

        </div>   
         
<hr>
       <b> <h4><div class="row">
          <div class="col-sm-8">
          </div>
          <div class="col-sm-4">
              <div class="row">
              <div class="col-sm-6">
               Total
              </div>
              <div class="col-sm-6">
                ${{total}}
              </div>
            </div>  
          </div>
        </div>  </h4> </b>
         <template v-slot:modal-footer>
        <div class="w-100">
          <div class="row">
          <div class="col-sm-8">
            <b-button size="md" variant="outline-primary" @click="close_modal"> 
              <i class="fas fa fa-arrow-left"></i>
              Continue shopping </b-button>
          </div>
          <div class="col-sm-4">
              <div class="row">
              <div class="col-sm-6">
              </div>
              <div class="col-sm-6">
                 <b-button size="md" variant="success" @click="Check_out" > Checkout </b-button>
              </div>
            </div>  
          </div>
        </div> 

        </div>
      </template>
  </b-modal>

<div>
  <b-modal id="modal-1" ref="my-modal2" title="Payment" hide-footer>
   <h5> <div class="d-block text-center">
      Bank : xxx-xxx-xxxx
    </div>
    <div class="d-block text-center">
      Amount : ${{this.amount}}
    </div>
    <div class="d-block text-center">
     Time : {{ countDown }} S
    </div></h5>
  </b-modal>
</div>

  </div>
</template>  

<script>

export default {
    data(){
      return{ 
        rows: 100,
        perPage: 1,
        currentPage: 1,
        movie_list : [],
        movie_page : [],
        search_cookie : "",
        cart : [],
        Subtotal : "",
        discount : "",
        have_discount: true,
        countDown : 60
        }
    },
    methods:{  
        Get_Start : function(page){
          this.axios.get("https://api.themoviedb.org/3/discover/movie?api_key=7a884526e4e67695d33cd2c16acbfccc&language=en-US&sort_by=popularity.desc&include_adult=false&include_video=false&page="+page).then((response) => {
         this.list(response.data)
          this.rows = response.data.total_pages
        })
        },
        list : function(data){
            this.movie_list = []
             var movie = {}
            for(var i = 0 ; i<data.results.length ; i++){
                 movie = {name : data.results[i].original_title , detail : data.results[i].overview , image : "https://image.tmdb.org/t/p/original" + data.results[i].poster_path}  
                this.movie_list.push(movie)
         }
        },
        search_movie : function(data,page){ 
          console.log(data)
          if(data == ""){
            this.search_cookie = ""
            this.Get_Start()
             this.currentPage = 1
          }
          else{    
            var search_page = ""        
            if(page == undefined){
              search_page = 1
            }
            else{
              search_page = page
            }
            console.log(page)
           var search_api = "https://api.themoviedb.org/3/search/movie?api_key=7a884526e4e67695d33cd2c16acbfccc&language=en-US&query=" + data + "&page=" + search_page + "&include_adult=false"
           console.log(search_api)
          this.axios.get(search_api).then((response) => {
          this.rows = response.data.total_pages
          this.list(response.data)
          this.search_cookie = data
        })
          }
        },
        get_page : function(){
          if(this.search_cookie != ""){
          this.search_movie(this.search_cookie,this.currentPage)
          }
          else{
          this.Get_Start(this.currentPage)
          }
          window.scrollTo(0,0);
        },
        Buy_movie : function(){
          this.$refs['my-modal'].show()
        },
        Add_cart : function(data,buy){ 
          var duplicate = false
          for(var i  = 0 ; i < this.cart.length ; i++){
            if(data.name == this.cart[i].name){
              this.cart[i].quantity = parseInt(this.cart[i].quantity) + 1
              this.cart[i].total_price = this.cart[i].total_price + this.cart[i].price
              console.log(  this.cart[i].total_price)
              duplicate = true
              this.$cookies.set("cookies_cart",JSON.stringify(this.cart))
              this.updata_cart_total()
            }
          }
          if(duplicate == false){
          var item = {name : data.name, image : data.image, price : 20, quantity : 1, total_price : 20}
          this.cart.push(item)
          this.$cookies.set("cookies_cart",JSON.stringify(this.cart))
          this.updata_cart_total()
          }
          if(buy === 'buy'){
            this.$refs['my-modal'].show()
          }
        },
        quantity_change : function(data){
          if(data.quantity == 0){
            this.remove_cart(data.name)
          }
          var get_cookies = JSON.parse(this.$cookies.get("cookies_cart"))
          for(var i  = 0 ; i < this.cart.length ; i++){
            if(data.name == this.cart[i].name){
              if(data.quantity > get_cookies[i].quantity){
                var total_quantity = data.quantity-get_cookies[i].quantity
                total_quantity = total_quantity * data.price
                this.cart[i].total_price = this.cart[i].total_price + total_quantity
                this.cart[i].quantity = data.quantity
              }
               else{
               total_quantity = get_cookies[i].quantity-data.quantity
                total_quantity = total_quantity * data.price
                this.cart[i].total_price = this.cart[i].total_price - total_quantity
                this.cart[i].quantity = data.quantity
              }
            }
            this.$cookies.set("cookies_cart",JSON.stringify(this.cart))
            this.updata_cart_total()
          }
        },
        updata_cart_total : function(){
          var Subtotal = 0
          var cart_item = 0
          var discount = 0
           for(var i  = 0 ; i < this.cart.length ; i++){
              Subtotal = Subtotal + this.cart[i].total_price
              cart_item = cart_item + parseInt(this.cart[i].quantity)
           }
          this.Subtotal = Subtotal
           this.update_header(cart_item)
          if(cart_item < 3){
            this.have_discount = false
          }
          if(cart_item >= 3 && cart_item <5){
            discount = (Subtotal/100)*10
            this.have_discount = true
            this.discount_10 = true
            this.discount_20 = false
          }
          if(cart_item >= 5){
            discount = (Subtotal/100)*20
            this.have_discount = true
            this.discount_10 = false
            this.discount_20 = true
          }
          this.discount = discount
          this.total = Subtotal - discount  
          
          
        },
        update_header : function(cart_item){
              this.$root.$emit('update_cart',cart_item)
        },
        Check_out : function(){
          var money = this.total
          this.amount = money
          if(this.cart.length != 0){
          this.$cookies.remove("cookies_cart")
          this.cart = []
          this.updata_cart_total()
          this.$refs['my-modal'].hide()
          this.$refs['my-modal2'].show()
          this.countDown = 60
          this.countDownTimer()
          }
        },
        countDownTimer : function() {
          //this.countDown = 10
                if(this.countDown >= 0) {
                    setTimeout(() => {
                        this.countDown -= 1
                        this.countDownTimer()
                    }, 1000)
                }
                else{
                  this.$refs['my-modal2'].hide()
                }
        },
        check_cookies : function(){
          if(JSON.parse(this.$cookies.get("cookies_cart")) != undefined){
            this.cart = JSON.parse(this.$cookies.get("cookies_cart"))
            this.updata_cart_total()
          }
        },
        remove_cart : function(data){
           for(var i  = 0 ; i < this.cart.length ; i++){
             if(data == this.cart[i].name){
                this.cart.splice(i,1)
             }
           }
           this.$cookies.set("cookies_cart",JSON.stringify(this.cart))
           this.updata_cart_total()
           
        },
        close_modal : function(){
          this.$refs['my-modal'].hide()
          this.$refs['my-modal2'].hide()
        }
    },
     beforeMount(){
    this.Get_Start("1")
    this.check_cookies()
    this.update_header()
 },
  mounted(){
        this.$root.$on('Movie_comp', (item) => {
            this.search_movie(item)
        }) 
        this.$root.$on('home', () => {
            this.search_movie("")
        }) 
        this.$root.$on('Cart', () => {
            this.Buy_movie()
        }) 
  },
}
</script>
<style scoped >

.card-deck {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
    grid-gap: .5rem;
    
}

.card {
  box-shadow: 0 3px 6px rgba(0,0,0,0.16), 0 3px 6px rgba(0,0,0,0.23);
}

.hvrbox * {
	box-sizing: border-box;
}
.hvrbox {
	position: relative;
	display: inline-block;
	overflow: hidden;
	max-width: 100%;
	height: auto;
}
.hvrbox img {
	max-width: 100%;
}
.hvrbox .hvrbox-layer_bottom {
	display: block;
}
.hvrbox .hvrbox-layer_top {
	opacity: 0;
	position: absolute;
	top: 0;
	left: 0;
	right: 0;
	bottom: 0;
	width: 100%;
	height: 100%;
	background: rgba(0, 0, 0, 0.6);
	color: #fff;
	padding: 15px;
	-moz-transition: all 0.4s ease-in-out 0s;
	-webkit-transition: all 0.4s ease-in-out 0s;
	-ms-transition: all 0.4s ease-in-out 0s;
	transition: all 0.4s ease-in-out 0s;
}
.hvrbox:hover .hvrbox-layer_top,
.hvrbox.active .hvrbox-layer_top {
	opacity: 1;
}
.hvrbox .hvrbox-text {
	text-align: center;
	font-size: 14px;
	display: inline-block;
	position: absolute;
	top: 50%;
	left: 50%;
  right: -40%;
	-moz-transform: translate(-50%, -50%);
	-webkit-transform: translate(-50%, -50%);
	-ms-transform: translate(-50%, -50%);
	transform: translate(-50%, -50%);
}
.hvrbox .hvrbox-text_mobile {
	font-size: 12px;
	border-top: 1px solid rgb(179, 179, 179); /* for old browsers */
	border-top: 1px solid rgba(179, 179, 179, 0.7);
	margin-top: 5px;
	padding-top: 2px;
	display: none;
}
.hvrbox.active .hvrbox-text_mobile {
	display: block;
}

.responsive {
  width: 100%;
  max-width: 400px;
  height: auto;
}


</style>