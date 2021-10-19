<template>
    <div class="input-box">
        <div class="input-content" @keydown="keydown" @keyup="keyup" @paste="paste" @mousewheel="mousewheel"
             @input="inputEvent">
            <input max="9" min="0" maxlength="1" data-index="0" v-model.trim.number="input[0]" type="password"
                   ref="firstinput"/>
            <input max="9" min="0" maxlength="1" data-index="1" v-model.trim.number="input[1]" type="password"/>
            <input max="9" min="0" maxlength="1" data-index="2" v-model.trim.number="input[2]" type="password"/>
            <input max="9" min="0" maxlength="1" data-index="3" v-model.trim.number="input[3]" type="password"/>
            <input max="9" min="0" maxlength="1" data-index="4" v-model.trim.number="input[4]" type="password"/>
            <input max="9" min="0" maxlength="1" data-index="5" v-model.trim.number="input[5]" type="password"/>
        </div>
    </div>
</template>

<script>
export default {
    data() {
        return {
            // 存放粘贴进来的数字
            pasteResult: [],
            numberLength: 6, // 固定6个盒子
        };
    },
    props: {
        code: {
            type: String
        }
    },
    computed: {
        input() {
            // code 是父组件传进来的默认值，必须是6位长度的数组，这里就不再做容错判断处理
            // 最后空数组是默认值
            return this.code || this.pasteResult.length === this.numberLength ? this.pasteResult : ['', '', '', '', '', '']
        }
    },
    methods: {
        // 解决一个输入框输入多个字符
        inputEvent(e) {
            let index = e.target.dataset.index * 1;
            let el = e.target;
            this.$set(this.input, index, el.value.slice(0, 1))
        },
        keydown(e) {
            let index = e.target.dataset.index * 1;
            let el = e.target;
            if (e.key === 'Backspace') {
                if (this.input[index].length > 0) {
                    this.$set(this.input, index, '')
                } else {
                    if (el.previousElementSibling) {
                        el.previousElementSibling.focus()
                        this.$set(this.input, index - 1, '')
                    }
                }
            } else if (e.key === 'Delete') {
                if (this.input[index].length > 0) {
                    this.$set(this.input, index, '')
                } else {
                    if (el.nextElementSibling) {
                        this.$set(this.input, index = 1, '')
                    }
                }
                if (el.nextElementSibling) {
                    el.nextElementSibling.focus()
                }
            } else if (e.key === 'Home') {
                el.parentElement.children[0] && el.parentElement.children[0].focus()
            } else if (e.key === 'End') {
                el.parentElement.children[this.input.length - 1] && el.parentElement.children[this.input.length - 1].focus()
            } else if (e.key === 'ArrowLeft') {
                if (el.previousElementSibling) {
                    el.previousElementSibling.focus()
                }
            } else if (e.key === 'ArrowRight') {
                if (el.nextElementSibling) {
                    el.nextElementSibling.focus()
                }
            } else if (e.key === 'ArrowUp') {
                if (this.input[index] * 1 < 9) {
                    this.$set(this.input, index, (this.input[index] * 1 + 1).toString());
                }
            } else if (e.key === 'ArrowDown') {
                if (this.input[index] * 1 > 0) {
                    this.$set(this.input, index, (this.input[index] * 1 - 1).toString());
                }
            }
        },
        keyup(e) {
            let index = e.target.dataset.index * 1;
            let el = e.target;
            if (/Digit|Numpad/i.test(e.code)) {
                this.$set(this.input, index, e.code.replace(/Digit|Numpad/i, ''));
                el.nextElementSibling && el.nextElementSibling.focus();
                if (index === 5) {
                    if (this.input.join('').length === this.numberLength) {
                        document.activeElement.blur();
                        this.$emit('completed', this.input.join(""));
                    }
                }
            } else {
                if (this.input[index] === '') {
                    this.$set(this.input, index, '');
                }
            }
        },
        mousewheel(e) {
            let index = e.target.dataset.index;
            if (e.wheelDelta > 0) {
                if (this.input[index] * 1 < 9) {
                    this.$set(this.input, index, (this.input[index] * 1 + 1).toString());
                }
            } else if (e.wheelDelta < 0) {
                if (this.input[index] * 1 > 0) {
                    this.$set(this.input, index, (this.input[index] * 1 - 1).toString());
                }
            } else if (e.key === 'Enter') {
                if (this.input.join('').length === this.numberLength) {
                    document.activeElement.blur();
                    this.$emit('completed', this.input.join(""));
                }
            }
        },
        paste(e) {
            // 当进行粘贴时
            e.clipboardData.items[0].getAsString(str => {
                if (str.toString().length === this.numberLength) {
                    this.pasteResult = str.split('');
                    document.activeElement.blur();
                    this.$emit('completed', this.input.join(""));
                }
            })
        }
    },
    mounted() {
        // 等待dom渲染完成，在执行focus,否则无法获取到焦点
        this.$nextTick(() => {
            this.$refs.firstinput.focus()
        })
    },
}
</script>

<style lang="scss" scoped>
.input-box {
    .input-content {
        display: flex;
        justify-content: space-between;
        box-sizing: border-box;

        input {
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
    }
    input::-webkit-outer-spin-button,
    input::-webkit-inner-spin-button {
        appearance: none;
        margin: 0;
    }

}

</style>
