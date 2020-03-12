<template>
    <div class="product_section">
        <div class="product_wrapper" v-for="(product, index) in products" v-bind:key="index">
            <div class="image_container">
                <a href="#">
                    <img src='../assets/images/df126-52f2-11e5-b9a9-00259036a192_220x220_1.jpg'
                         v-bind:alt='"@/assets/images/"+product.productId+"_220x220_1.jpg"'>
                </a>
            </div>
            <div class="object_info">
                <div class="id">
                    Код: {{product.code}}
                </div>
                <div class="description">
                    <a href="#">
                        {{ product.title }}
                    </a>
                </div>
                <div class="tags">
                    <p>Могут понадобиться: </p>
                    <template v-for="(tag, index) in strSplit(product.assocProducts)">
                        <a v-bind:key="index" href="#">{{ tag }}</a>{{index === strSplit(product.assocProducts).length - 1 ? '.' : ','}}
                    </template>
                </div>
            </div>
            <div class="cart_and_status">
                <div class="product_status">
                    <span>Наличие</span>
                </div>
                <div class="pay_w_card">
                    <span class="w_card">
                        По карте<br>клуба:
                    </span>
                    <span class="gold_price" :id="'gp'+index">
                        {{ isPriceAlt ? round(product.priceGold) : round(product.priceGoldAlt) }} Р
                    </span>
                </div>
                <div class="pay_w_cash" >
                    <span :id="'rp'+index">
                        {{isPriceAlt ? round(product.priceRetail) : round(product.priceRetailAlt) }} Р
                    </span>
                </div>
                <div class="pay_w_points">
                    Можно купить за 231,75 балла
                </div>
                <div class="measure_units">
                    <p @click="priceToggle($event, index)" class="unit meter" :class="{active: !isPriceAlt}">
                        За м.кв.
                    </p>
                    <p @click="priceToggle($event, index)" class="unit" :class="{active: isPriceAlt}">
                        За упаковку
                    </p>
                </div>
                <div class="info_on_units">
                    <div class="info_ico"></div>
                    <div>
                        <p>Продается упаковками:</p>
                        <p>1 упак. = 2.47 м. кв.</p>
                    </div>
                </div>
                <div class="cart">
                    <div class="amount_input">
                        <input :id="'inp'+product.productId" value="1" type="text" name="amnt">
                        <div class="arrows">
                            <div @click="inputPlus(product.productId, 1)" class="arrow up"></div>
                            <div @click="inputPlus(product.productId, 0)" class="arrow down"></div>
                        </div>
                    </div>
                    <div :data-product-id="product.productId" class="cart_btn"></div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        name: 'HelloWorld',
        props: {
            products: Array
        },
        data() {
            return {
                isPriceAlt: false
            }
        },
        methods: {
            strSplit: function (str) {
                let arr = str.split(";");
                return arr.filter(function (el) {
                    return el.match(/\s/);
                });
            },
            round: function (num) {
                return Math.round((parseFloat(num) + Number.EPSILON) * 100) / 100;
            },
            priceToggle: function (event, id) {
                if(!event.target.classList.contains("active")) {
                    let alt = event.target.classList.contains("meter") ? 1 : 0;
                    let siblings = event.target.parentElement.children;
                    for (let i = 0; i < siblings.length; i++) {
                        siblings[i].classList.remove("active");
                    }
                    event.target.classList.add("active");
                    let goldPrice = document.getElementById("gp" + id);
                    goldPrice.innerText = alt ? this.round(this.products[id]['priceGoldAlt']) +' Р'
                        : this.round(this.products[id]['priceGold']) +' Р';
                    let retailPrice = document.getElementById("rp" + id);
                    retailPrice.innerText = alt ? this.round(this.products[id]['priceRetailAlt']+' Р') +' Р'
                        : this.round(this.products[id]['priceRetail']) +' Р';
                }
            },
            inputPlus: function (id, bool) {
                let input = document.getElementById('inp'+id);
                if (input.value > 1) {
                    bool ? input.value++ : input.value--;
                } else if(input.value == 1) {
                    bool ? input.value++ : null;
                }
            }
        }
    }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="scss" scoped>
    .product_section {
        margin: 30px auto 14px;
        box-sizing: border-box;
        width: 1250px;

        .product_wrapper {
            box-sizing: border-box;
            margin: 10px 5px 0;
            padding: 10px 10px 18px;
            width: 1000px;
            display: flex;
            flex-wrap: nowrap;
            border: 1px solid #ccc;

            .image_container {
                box-sizing: border-box;
                text-align: center;
                width: 220px;
                height: 220px;

                img {
                    max-width: 160px;
                    max-height: 160px;
                }
            }

            .object_info {
                box-sizing: border-box;
                padding: 10px;
                width: 505px;
                .id {
                    font-size: 12px;
                    color: #666;
                }
                .description {
                    margin: 20px 0 0 0;
                    a {
                        text-decoration: none;
                        color: #000;
                        font-weight: 700;
                        transition: color .15s ease-in-out,background-color .15s ease-in-out,border-color .15s ease-in-out;
                        &:hover {
                            text-decoration: underline;
                        }
                    }
                }
                .tags {
                    margin: 10px 0 0 0;
                    p {
                        display: inline-block;
                        font-size: 13px;
                        line-height: 16px;
                        color: #545454;
                        font-weight: 700;
                        margin: 0;
                    }
                    a {
                        text-decoration: none;
                        display: inline;
                        font-size: 13px;
                        line-height: 16px;
                        color: #666;
                        &:hover {
                            text-decoration: underline;
                        }
                    }
                }
            }

            .cart_and_status {
                box-sizing: border-box;
                .product_status {
                    text-align: right;
                    span {
                        cursor: pointer;
                        color: #093;
                        border-bottom: 1px dotted #093;
                        text-decoration: none;
                        margin-right: auto;
                        &:hover {
                            border-bottom: none;
                        }
                    }
                }
                .pay_w_card {
                    display: flex;
                    justify-content: flex-end;
                    span {
                        margin-right: 15px;
                    }
                    .w_card {
                        font-weight: 400;
                        font-size: 14px;
                        display: inline-block;
                        text-align: right;
                        line-height: 17px;
                        margin-right: 16px;
                    }
                    .gold_price {
                        font-size: 25px;
                        font-weight: 700;
                        color: #000;
                        margin: 10px 0 0 0;
                    }
                }
                .pay_w_cash {
                    text-align: right;
                    span {
                        color: #a7a7a7;
                        font-size: 25px;
                    }
                }
                .pay_w_points {
                    font-size: 12px;
                    line-height: 17px;
                    color: #999;
                    text-align: right;
                }
                .measure_units {
                    height: 34px;
                    display: flex;
                    justify-content: flex-end;
                    .unit {
                        margin: 5px 0 5px 20px;
                        padding: 7px 8px 4px;
                        font-size: 13px;
                        line-height: 13px;
                        color: #707070;
                        border-bottom: 1px dotted #707070;
                        cursor: pointer;
                        &.active {
                            background: #000;
                            cursor: default;
                            color: #fff;
                            border-bottom: 1px transparent;
                        }
                        &:hover {
                            border-bottom: none;
                        }
                    }
                }
                .info_on_units {
                    font-size: 13px;
                    display: flex;
                    border: 1px solid #bbb;
                    width: 222px;
                    margin: 5px 0 12px;
                    div {
                        width: 90%;
                        p {
                            margin: 0;
                        }
                    }
                    .info_ico {
                        width: 10%;
                        margin: 10px;
                        margin-right: 20px;
                        /*width: 16px;*/
                        height: 16px;
                        background-image: url("../assets/images/icons/product/unit--i.png");
                        background-size: 16px;
                        background-repeat: no-repeat;
                    }
                }
                .cart {
                    display: flex;
                    .amount_input {
                        margin-right: 10px;
                        display: flex;
                        height: 36px;
                        border: 1px solid #ccc;
                        cursor: pointer;
                        overflow: hidden;
                        width: 60px;
                        input {
                            text-align: center;
                            box-sizing: border-box;
                            display: block;
                            width: 35px;
                            height: 36px;
                            overflow: hidden;
                            padding: 12px 0 10px;
                            border: none;
                            color: #333;
                            font-size: 13px;
                            line-height: 1.2;
                            margin: 0;
                        }
                        .arrows {
                            display: inline-block;
                            width: 25px;
                            height: 35px;
                            .arrow {
                                height: 18px;
                                background-image: url("../assets/images/icons/product/count--i.png");
                                background-size: 50px;
                                background-repeat: no-repeat;
                                &:hover {
                                    background-color: #999999;
                                }
                            }
                            .up {
                                background-position: -25px -1px;
                                border-bottom: 1px solid #ccc;
                                border-left: 1px solid #ccc;
                            }
                            .down {
                                background-position: -25px -18px;
                                border-left: 1px solid #ccc;
                            }
                        }
                    }
                    .cart_btn {
                        box-sizing: border-box;
                        height: 38px;
                        background: #f90;
                        flex: 1 1 80%;
                        padding: 0 0 0 10px;
                        cursor: pointer;
                        background-image: url("../assets/images/shopping_cart.png");
                        background-size: 150px;
                        background-repeat: no-repeat;
                        &:hover {
                            background: #222;
                            background-image: url("../assets/images/shopping_cart_dark.png");
                            background-size: 150px;
                            background-repeat: no-repeat;
                        }
                    }
                }
            }
        }
    }
</style>
