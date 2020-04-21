<template>
  <div class="contaier mx-auto min-h-screen" :class="product.color">
    <div
      class="grid grid-cols-1 sm:grid-cols-1 md:grid-cols-2 lg:grid-cols-2 xl:grid-cols-2"
    >
      <div
        class="flex items-center justify-center min-h-full sm:min-h-full md:min-h-screen lg:min-h-screen xl:min-h-screen p-12"
      >
        <img :src="product.image" alt="" />
      </div>
      <div
        class="sm:min-h-full md:min-h-screen lg:min-h-screen xl:min-h-screen p-12 bg-white"
      >
        <div>
          <p class="text-4xl font-bold mb-2 text-gray-800">
            {{ product.name }}
          </p>
          <p class="text-gray-600 mb-4 font-bold">By {{ product.brand }}</p>
          <p v-html="product.description" class="text-gray-600"></p>
        </div>
        <div class="my-8">
          <img
            v-for="n in product.stars"
            :key="n"
            class="mr-1 inline"
            src="star.png"
            alt=""
          />
          <span>{{ product.stars }} ({{ product.ratings }} ratings)</span>
        </div>
        <div class="my-8">
          <p class="text-4xl font-bold">
            {{ product.currency }} {{ product.price }}
          </p>
        </div>

        <div class="my-8 border-b-4 border-black"></div>

        <div class="my-8">
          <p>Sizes</p>
          <div class="flex flex-wrap">
            <div
              class="h-16 w-16 mr-2 rounded-lg border-2 border-black border-b-4 font-bold flex items-center justify-center text-gray-800"
            >
              <span class="text-4xl">XS</span>
            </div>
            <div
              class="h-16 w-16 mr-2 rounded-lg border-2 border-black border-b-4 font-bold flex items-center justify-center text-gray-800"
            >
              <span class="text-4xl">S</span>
            </div>
            <div
              class="h-16 w-16 mr-2 rounded-lg border-2 border-black bg-yellow-500 font-bold flex items-center justify-center text-gray-800"
            >
              <span class="text-4xl">M</span>
            </div>
            <div
              class="h-16 w-16 mr-2 rounded-lg border-2 border-black border-b-4 font-bold flex items-center justify-center text-gray-800"
            >
              <span class="text-4xl">L</span>
            </div>
          </div>
        </div>

        <div class="my-8 flex flex-wrap justify-between">
          <div>
            <p>Colors</p>
            <div class="flex flex-wrap">
              <div
                class="h-8 w-8 mr-2 rounded-full border-2 border-black bg-red-500  font-bold flex items-center justify-center text-gray-800"
              ></div>
              <div
                class="h-8 w-8 mr-2 rounded-full border-2 border-black border-b-4 bg-green-500 font-bold flex items-center justify-center text-gray-800"
              ></div>
              <div
                class="h-8 w-8 mr-2 rounded-full border-2 border-black border-b-4 bg-yellow-500 font-bold flex items-center justify-center text-gray-800"
              ></div>
            </div>
          </div>

          <div>
            <p>Quantity</p>
            <div
              class="rounded-lg border-2 border-black flex w-32 text-2xl font-bold"
            >
              <div
                class="flex-1 flex items-center justify-center bg-yellow-500 rounded-l-lg"
              >
                -
              </div>
              <div class="flex-1 flex items-center justify-center">2</div>
              <div
                class="flex-1 flex items-center justify-center bg-yellow-500 rounded-r-lg"
              >
                +
              </div>
            </div>
          </div>
        </div>

        <div class="my-8">
          <button
            @click="order"
            class="bg-black text-white font-bold w-full text-2xl text-center p-2 rounded-lg border-4 border-black"
          >
            Pay ₹ {{ product.price }}
          </button>
        </div>
        <p v-if="payment_id" class="text-xl text-center font-bold">
          Your Order was succesful, here is your payment reference #{{
            payment_id
          }}
        </p>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      payment_id: null
    };
  },
  methods: {
    order() {
      const self = this;
      const rzp_options = {
        key: process.env.RZP_KEY,
        amount: this.product.price * 100,
        name: "The Bollywood Store",
        description: this.product.title,
        handler: function(response) {
          self.$toast.success(
            `Payment Succesful`,
            {
              position: "bottom-center",
              theme: "outline",
              duration: 5000
            }
          );
          self.payment_id = response.razorpay_payment_id;
        },
        modal: {
          ondismiss: function() {
            self.$toast.error(`Payment Failed`, {
              position: "bottom-center",
              theme: "outline",
              duration: 5000
            });
          }
        },
        prefill: {
          email: 'test@email.com',
          contact: +914455667788
        },
        notes: {
          name: "Customer Name",
          item: self.product.title,
        },
        theme: {
          color: "#667eea"
        }
      };
      const rzp1 = new Razorpay(rzp_options);
      rzp1.open();
    }
  },
  computed: {
    product() {
      return this.$store.state.products.find(
        item => item.slug == this.$route.params.slug
      );
    }
  }
};
</script>

<style></style>
