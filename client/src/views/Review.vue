<script>
import NavbarVue from '../components/Navbar.vue';
import { mapActions } from 'pinia';
import { useUserStore } from '../stores/user';
import { useVehicleStore } from '../stores/vehicle';
import Swal from 'sweetalert2'

const Toast = Swal.mixin({
    toast: true,
    position: 'top-end',
    showConfirmButton: false,
    timer: 1500,
    timerProgressBar: true,
    didOpen: (toast) => {
        toast.addEventListener('mouseenter', Swal.stopTimer)
        toast.addEventListener('mouseleave', Swal.resumeTimer)
    }
})


export default {
    data() {
        return {
            rating: '',
            msg: ''
        }
    },

    created() {
        this.checkAccessToken()
    },
    components: {
        NavbarVue
    },

    methods: {
        ...mapActions(useUserStore, ['reviewHandler', 'checkAccessToken']),
        ...mapActions(useVehicleStore, 'fetchVehicleDetail'),
        submitReview() {
            const rating = this.rating
            const msg = this.msg
            this.reviewHandler(this.$route.params.id, { rating, msg })
            this.$router.push(`/vehicle/detail/${this.$route.params.id}`)
            Toast.fire({
                icon: 'success',
                title: 'Review posted. Thanks'
            })
            this.fetchVehicleDetail(this.$route.params.id)
        }
    }

}
</script>

<template>
    <NavbarVue />
    <div class="bg-black text-white py-20">
        <div class="container mx-auto flex flex-col md:flex-row my-6 md:my-24">
            <div class="flex flex-col w-full lg:w-1/3 p-8">
                <p class="ml-6 text-yellow-300 text-lg uppercase tracking-loose">REVIEW</p>
                <p class="text-3xl md:text-5xl my-4 leading-relaxed md:leading-snug">Leave us a feedback!</p>
                <p class="text-sm md:text-base leading-snug text-gray-50 text-opacity-100">
                    Please provide your valuable feedback and something something ...
                </p>
            </div>
            <div class="flex flex-col w-full lg:w-2/3 justify-center">
                <div class="container w-full px-4">
                    <div class="flex flex-wrap justify-center">
                        <div class="w-full lg:w-6/12 px-4">
                            <div
                                class="relative flex flex-col min-w-0 break-words w-full mb-6 shadow-lg rounded-lg bg-white">
                                <div class="flex-auto p-5 lg:p-10">
                                    <h4 class="text-2xl mb-4 text-black font-semibold">Have a suggestion?</h4>
                                    <form @submit.prevent="submitReview">
                                        <div class="relative w-full mb-3">
                                            <label class="block uppercase text-gray-700 text-xs font-bold mb-2"
                                                for="email">Rating</label>
                                                <input type="number"
                                                class="border-0 px-3 py-3 rounded text-sm shadow w-full
                    bg-gray-300 placeholder-black text-gray-800 outline-none focus:bg-gray-400" placeholder=" "
                                                style="transition: all 0.15s ease 0s;" v-model="rating" />
                                        </div>
                                        <div class="relative w-full mb-3">
                                            <label class="block uppercase text-gray-700 text-xs font-bold mb-2"
                                                for="message">Review</label><textarea maxlength="300" name="feedback"
                                                id="feedback" rows="4" cols="80"
                                                class="border-0 px-3 py-3 bg-gray-300 placeholder-black text-gray-800 rounded text-sm shadow focus:outline-none w-full"
                                                placeholder="" v-model="msg"></textarea>
                                        </div>
                                        <div class="text-center mt-6">
                                            <button id="feedbackBtn"
                                                class="bg-yellow-300 text-black text-center mx-auto active:bg-yellow-400 text-sm font-bold uppercase px-6 py-3 rounded shadow hover:shadow-lg outline-none focus:outline-none mr-1 mb-1"
                                                type="submit" style="transition: all 0.15s ease 0s;">Submit
                                            </button>
                                        </div>
                                    </form>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>