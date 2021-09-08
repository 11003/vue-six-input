<template>
    <div class="code-container">
        <div class="filed-wrap" v-for="index in inputNumber" :key="index">
            <input maxlength="1" autocorrect="off" autocomplete="off" autocapitalize="off" spellcheck="false"
                   :type="inputType" class="verify-text-input" :id="'verifyCode__'+index"
                   oncopy="return false"
                   onpaste="return false"
                   @keyup="handleKeyUp($event,index)" />
        </div>
    </div>
</template>

<script>
export default {
    name: "VueSixInput",
    props: {
        inputNumber: {
            default: 6,
            type: Number
        },
        inputType: {
            default: 'text',
            type: String
        }
    },
    data () {
        return {
            inputReg: /^[0-9]*$/,
            inputList: [],
            arrayInput: [],
            code: []
        }
    },
    mounted() {
        for (let i = 0, j = this.inputNumber; i < j; i++) {
            this.code.push('')
        }
        this.inputList = document.getElementsByClassName('verify-text-input')
        this.arrayInput = Array.from(this.inputList)
        this.$nextTick(() => {
            this.inputList[0].focus()
        })
    },
    methods: {
        handleKeyUp(e, next) {
            this.$forceUpdate();
            let keyCode = e.keyCode || e.which;
            if (keyCode === 8) {
                // backspace删除
                this.code[next - 1] = ''
                let pre = next - 2
                if (pre >= 0) {
                    this.inputList[pre].focus()
                }
            } else {
                let value = e.target.value;
                if(!value) return; // 解决输入过快的bug
                // 如果不是数字就清除该值
                if (!this.inputReg.test(value)) {
                    e.target.value = '';
                } else {
                    this.code[next - 1] = value
                    if (next < this.inputNumber) {
                        this.inputList[next].focus()
                    }
                }
            }
            this.$emit("completed", this.code.join(""));
        }
    }
}
</script>

<style scoped>
.code-container {
    display: flex;
    justify-content: space-between;
    box-sizing: border-box;
}
.code-container .filed-wrap {
    width: 50px;
    height: 50px;
}
.code-container .filed-wrap:nth-child(n+1) {
    margin-left: 6px;
}
.code-container .filed-wrap:first-child {
    margin-left: 0;
}
.code-container .filed-wrap .verify-text-input {
    display: inline-block;
    font-family: SF Pro Text, SF Pro Icons, Helvetica Neue, Helvetica, Arial, sans-serif;
    width: 50px;
    height: 50px;
    font-size: 24px;
    color: #494949;
    padding: 0;
    vertical-align: top;
    text-align: center;
    margin-bottom: 5px;
    border-radius: 5px;
    background-color: #f7f7f7;
    background-clip: padding-box;
    font-synthesis: none;
    -moz-font-feature-settings: "kern";
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    direction: ltr;
}
</style>
