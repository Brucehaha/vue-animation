<template>
    <div class="container">
        <div class="row">
            <div class="col-xs-12 col-sm-8 col-sm-offset-2 col-md-6 col-md-offset-3">
                <h1>Animations</h1>
                <hr>
                <select v-model="alertAnimation" class="form-control">
                    <option value="fade">Fade</option>
                    <option value="slide">Slide</option>
                </select>
                <br>
                <button class="btn btn-primary" @click="show = !show">Show Altert</button>
                <br><br>
                <transition :name="alertAnimation">
                    <div class="alter alert-info" v-if="show">This is some Info</div>
                </transition>
                <br> <br>
                <transition name="slide" type="animation">
                    <div class="alter alert-info" v-if="show">This is some Info</div>

                </transition>
                <br> <br>
                <!--appear in transition make it work when load -->
                <transition
                        enter-active-class="animated bounce"
                        leave-active-class="animated shake"
                        >
                    <div class="alter alert-info" v-if="show">This is some Info</div>
                </transition>

                <br><br>
                <transition :name="alertAnimation" mode="out-in">
                    <div class="alter alert-info" v-if="show" key="info">This is some Info</div>
                    <div class="alter alert-warning" v-else="show" key="warning">This is some Info</div>

                </transition>
                <hr>
                <br>
                <!--Javascript animation-->
                <button class="btn btn-primary" @click="load = !load">Load / Remove Element</button>
                <br>             <br>
                <transition
                            @before-enter="beforeEnter"
                            @enter="enter"
                            @after-enter="afterEnter"
                            @enter-cancelled="enterCancelled"
                            @before-leave="beforeLeave"
                            @leave="leave"
                            @after-leave="afterLeave"
                            @leave-cancelled="leaveCancelled"
                            :css="false">
                    <div style="width: 300px; height:100px; background-color:lightgreen" v-if="load"></div>
                </transition>
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        data() {
            return {
                show:false,
                alertAnimation: 'fade',
                load:true,
                elWidth: 100,

            }
        },
        methods: {
            beforeEnter(el) {
                console.log('beforeEnter: '+el );
                this.elWidth=100;
                el.style.width = this.elWidth + "px" ;


            },
            enter(el, done) {
                console.log('Enter: '+el);
                let round = 1;
                const interval = setInterval(() => {
                    el.style.width = (this.elWidth+round*10)+'px';
                    round++;
                    if (round>20) {
                        clearInterval(interval);
                        done();
                    }
                }, 20);
            },
            afterEnter(el) {
                console.log('AfterEnter: '+ el);
            },
            enterCancelled(el) {
                console.log("EnterCancelled: "+el);
            },
            beforeLeave(el) {
                console.log("beforeLeave")
                this.elWidth=300;
                el.style.width = this.elWidth + "px" ;
            },
            leave(el, done) {
                console.log('leave: '+el);
                let round = 1;
                const interval = setInterval(() => {
                    el.style.width = (this.elWidth-round*10)+'px';
                    round++;
                    if (round>20) {
                        clearInterval(interval);
                        done();
                    }
                }, 20);
                },
            afterLeave(el, done) {
                console.log('AfterLeave: '+ el);

            },
            leaveCancelled() {
                console.log("LeaveCancelled: "+el);
            }

        }
    }
</script>
<style>
    .fade-enter {
        opacity: 0;
    }
    .fade-enter-active {
        transition: opacity 1s;
    }
    .fade-leave-active{
        transition: opacity 1s;
        opacity: 0;
    }
    .fade-leave {
        /*opacity: 1;*/
    }


    .slide-enter {
        opacity: 0;
    }
    .slide-enter-active {
        animation: slide-in 1s ease-out forwards;
        transition: opacity .5s;

    }
    .slide-leave {
        opacity: 0.5;
        /*transform: translateY(20px);*/
    }
    .slide-leave-active {
        animation: slide-out 1s ease-out forwards;

        transition: opacity 3s;
        opacity: 0;

    }
    @keyframes slide-in {
        from {
            transform: translateY(20px);
        }
        to {
            transform: translateY(0);
        }
    }
    @keyframes slide-out {
        from {
            transform: translateY(0);
        }
        to {
            transform: translateY(20px);
        }
    }
</style>
