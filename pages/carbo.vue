<template>
    <div id="container">
        <h1>カーボカウント計算機</h1>
        <router-link to="/carbo/setting">設定</router-link>
        <div>
            <div>
                <p>インスリンカーボ比</p>
                <p>1カーボで<span><input v-model.number="insulincarbo_ratio" type="number" step="0.1" min="0"></span>単位</p>
            </div>
            <p>主食:<input v-model.number="staple" type="number" step="0.1" min="0">カーボ</p>
            <p>おかず:<input v-model.number="dish" type="number" step="0.1" min="0">カーボ</p>
            <p>いも類:<input v-model.number="potato" type="number" step="0.1" min="0">カーボ</p>
            <p>デザート:<input v-model.number="desert" type="number" step="0.1" min="0">カーボ</p>
            <p>飲み物:<input v-model.number="drink" type="number" step="0.1" min="0">カーボ</p>
            <p>合計:<span class="">{{ meal }}</span></p>
            <p>詳細:<span>{{ meal }}</span> &times; <span>{{ insulincarbo_ratio }}</span> = {{ meal_total }}</p>
            <h3>食事に対するインスリン量: {{ meal_total }}単位</h3>
            <div>
                <p>インスリン効果値</p>
                <p>インスリン1単位で<span class=""><input v-model.number="insulin_effectiveness" type="number" min="0"></span>[mg/dl]低下する</p>
            </div>
            <p>目標血糖値:<span><input v-model.number="targeted_bg" type="number" min="0"></span>[mg/dl]</p>
            <p>血糖値:<span><input v-model.number="BloodGlucose" type="number" min="0"></span>[mg/dl]</p>
            <p><span>{{ BloodGlucose }}</span> - <span>{{ targeted_bg }}</span> = <span>{{ delta_bg }}</span></p>
            <p><span>{{ delta_bg }}</span> &divide; <span>{{ insulin_effectiveness }}</span> = <span>{{ correct_bg }}</span></p>
            <h3>補正インスリン量: <span>{{ correct_bg }}</span>単位</h3>

            <h2>合計:{{ total }}単位</h2>
        </div>
    </div>
</template>

<script>
export default {
    data: () => ({
        insulincarbo_ratio: '',
        staple: '',
        dish: '',
        potato: '',
        desert: '',
        drink: '',
        insulin_effectiveness: '',
        targeted_bg: '',
        BloodGlucose: '',
    }),
    computed: {
        meal: function() {
            return this.staple + this.dish + this.potato + this.desert + this.drink
        },
        meal_total: function () {
            return this.meal * this.insulincarbo_ratio
        },
        delta_bg: function() {
            return this.BloodGlucose - this.targeted_bg
        },
        correct_bg: function () {
            return this.delta_bg / this.insulin_effectiveness
        },
        total: function () {
            return this.correct_bg + this.meal_total
        },
    }
}
</script>
