<template>
    <div class="option">
        <h2 class="option__title">Crayon Size</h2>
        <button v-for="btn in btns" 
                :key="btn.name" 
                class="btn"
                :class="{ 'btn--active':btn.active }"
                @click="changePenSize(btn.val)">
                <span class="btn--span" 
                      :class="'btn--span--' + btn.name">
                </span>
        </button>
        
        <p class="erase" @click="erase">Erase Page</p>
        <p>Extras</p>
        <a href="#" download="painting.png" @click="print" ref="print" class="print"><p>Print</p></a>
    </div>
    
</template>

<script>
import bus from '../../assets/eventBus.js'
export default {
    data() {
        return {
            btns:[
                {
                    name: 'small',
                    val: 3,
                    active: true
                },{
                    name: 'medium',
                    val: 7,
                    active: false
                },{
                    name: 'large',
                    val: 11,
                    active: false
                }
            ]
        };  
    },
    mounted() {
        bus.$on('receivePrintImg', this.receivePrintImg);
    },
    methods: {
        erase() {
            bus.$emit('erase');
        },
        print() {
            bus.$emit('print');
        },
        changePenSize(val) {
            bus.$emit('changePenSize', val);
            for (let i =0; i<=this.btns.length; i++) {
                if (this.btns[i].val === val) {
                    this.btns[i].active = true;
                } else {
                    this.btns[i].active = false;
                }
            }
        },
        receivePrintImg(url) {
            this.$refs.print.href = url;
        }
    }
}
</script>

<style lang="scss">
    .option {
        width: .2rem;

        display: grid;
        grid-template-rows: repeat(7, 1fr);
        justify-items: center;
        align-items: center;
        text-align: center;
        margin-left: .1rem;
        
        &__title{
            font-size: .2rem;

            @media only screen and (max-height: 660px) {
                font-size: .15rem;
                font-size: .15rem;
            }
        }

        & p {
            font-size: .2rem;
            @media only screen and (max-height: 660px) {
                font-size: .15rem;
                font-size: .15rem;
            }
        }
    }
    .btn {
        display: flex;
        background-color: #f4f5eb;
        border:none;
        border-radius: 50%;
        cursor: pointer;
        width: 30px;
        height: 30px;
        
        align-items: center;
        justify-content: center;
        &--active {
            border: 3px solid black;
        }
        &--span {
            background-color: black;
            display: block;
            border-radius: 50%;
            
            &--small{
                width: 5px;
                height: 5px;
            }

            &--medium{
                width: 10px;
                height: 10px;
            }

            &--large{
                width: 15px;
                height: 15px;
            }
        }
        
    }
    .erase {
        cursor: pointer;
    }

    .print:link,
    .print:visited{
        color: black;
        text-decoration: none;
    }

    .btn:hover,
    .erase:hover,
    .print:hover {
        opacity: .5;
    }
</style>

