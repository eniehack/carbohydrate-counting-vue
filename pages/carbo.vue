<template>
    <div class="container">
        <header>
            <h1>カーボカウント計算機</h1>
            <router-link to="/carbo/setting">設定</router-link>
        </header>
        <div class="item item1 form">
            <div class="boader">
                <p>インスリンカーボ比</p>
                <p>1カーボで<span class="purple"><input v-model.number="insulincarbo_ratio" type="number" step="0.1" min="0"></span>単位</p>
            </div>
            <p>主食:<input v-model.number="staple" type="number" step="0.1" min="0">カーボ</p>
            <p>おかず:<input v-model.number="dish" type="number" step="0.1" min="0">カーボ</p>
            <p>いも類:<input v-model.number="potato" type="number" step="0.1" min="0">カーボ</p>
            <p>デザート:<input v-model.number="desert" type="number" step="0.1" min="0">カーボ</p>
            <p>飲み物:<input v-model.number="drink" type="number" step="0.1" min="0">カーボ</p>
        </div>
            <div class="item item3">
                <p>合計:<span class="blue">{{ meal }}</span>カーボ</p>
                <p>詳細:<span class="blue">{{ meal }}</span> &times; <span class="purple">{{ insulincarbo_ratio }}</span> = {{ meal_total }}</p>
                <h3>食事に対するインスリン量: {{ meal_total }}単位</h3>
            </div>
        <div class="item item2 form">
            <div class="boader">
                <p>インスリン効果値</p>
                <p>インスリン1単位で<span class="yellow"><input v-model.number="insulin_effectiveness" type="number" min="0"></span>[mg/dl]低下する</p>
            </div>
                <p>目標血糖値:<span><input v-model.number="targeted_bg" type="number" min="0"></span>[mg/dl]</p>
                <p>血糖値:<span class="blue-line"><input v-model.number="BloodGlucose" type="number" min="0"></span>[mg/dl]</p>
        </div>
        <div class="item item4">
            <p><span class="blue-line">{{ BloodGlucose }}</span> - <span>{{ targeted_bg }}</span> = <span>{{ delta_bg }}</span></p>
            <p><span>{{ delta_bg }}</span> &divide; <span class="yellow">{{ insulin_effectiveness }}</span> = <span>{{ correct_bg }}</span></p>
            <h3>補正インスリン量: <span>{{ correct_bg }}</span>単位</h3>
        </div>
        <div class="item result">
            <h2>合計:{{ total }}単位</h2>
        </div>
        <footer>
        </footer>
    </div>
</template>

<style lang="scss">

    $header: 115px;

    .blue {
        background-color: aqua;
    }
    .purple {
        background-color: blueviolet;
    }
    .yellow {
        background-color: rgb(255, 255, 71);
    }
    
    input, textarea {
        /* すべてのテキストフィールドのフォント設定を一致させる
            デフォルトで、textarea は等幅フォントが設定されている */
        font: 1em sans-serif;

        /* すべてのテキストフィールドを同じサイズにする */
        width: 55px;
        box-sizing: border-box;

        /* テキストフィールドのボーダーの外見を同一にする */
        border: 1px solid #999;
    }

    .form {
        /* フォームをページの中央に置く */
        margin: 0 auto;
        width: 400px;
        }
        form, .boader {
        /* フォームの範囲がわかるようにする */
        padding: 1em;
        border: 1px solid #CCC;
        border-radius: 1em;
    }

    .container {
        display: grid;
        grid-template-columns: repeat(2,450px);
        grid-template-rows: $header 375px auto 50px $header;
        grid-gap: 1rem;
        grid-template-areas: 
            "header header"
            "item1 item2"
            "item3 item4"
            "result result"
            "footer footer";
    }

    header {
        grid-area: header;
    }

    .item.item1.form {
	    grid-area: item1;
    }

    .item.item2.form {
        grid-area: item2;
    }

    .item.item3 {
        grid-area: item3;
    }

    .item.item4 {
        grid-area: item4;
    }
    .item.result {
        grid-area: result;
    }

</style>

<script>
export default {
    data: () => ({
        insulincarbo_ratio: 0,
        staple: 0,
        dish: 0,
        potato: 0,
        desert: 0,
        drink: 0,
        insulin_effectiveness: 0,
        targeted_bg: 0,
        BloodGlucose: 0,
    }),
    computed: {
        meal: function() {
            return this.staple + this.dish + this.potato + this.desert + this.drink
        },
        insulinfrommeal: function () {
            return this.meal * this.insulincarbo_ratio
        },
        delta_bg: function() {
            return this.BloodGlucose - this.targeted_bg
        },
        insulinfrombg: function () {
            return this.delta_bg / this.insulin_effectiveness
        },
        total: function () {
            return this.insulinfrombg + this.insulinfrommeal
        },
    }
}
</script>
