<template>
    <view>
        {{text}}
    </view>
    <transition name="fade">
        <view class="money" v-if="showMoney">
            <button>会员9.9</button>
            <button>会员19.9</button>
            <button>会员29.9</button>
        </view>
    </transition>
</template>

<script>
import { TextText } from "@/taluo.js"

export default {
    components: {},
    data() {
        return {
            text: "",
            showMoney: false
        }
    },
    methods: {},
    created() {
        const randomIndex = Math.floor(Math.random() * TextText.length);
        const randomValue = TextText[randomIndex];
        this.text = randomValue;

        setTimeout(() => {
            this.showMoney = true;
        }, 1000);
    }
}
</script>

<style scoped>
/* 弹出框样式 */
.money {
    background-color: white;
    border: 1px solid #ccc;
    padding: 10px;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
    width: 80%; /* 宽度为可视区域的80% */
    margin: 0 auto; /* 居中弹出框 */
    border-radius: 5px; /* 圆角 */
}

/* 淡入淡出动画 */
.fade-enter-active, .fade-leave-active {
    transition: opacity 0.5s;
}
.fade-enter, .fade-leave-to /* .fade-leave-active in <2.1.8 */ {
    opacity: 0;
}
</style>
