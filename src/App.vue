<template>
    <div id="app">
        <div class="header">
            <div
                class="cart-wrapper"
                @drop.prevent='onDrop($event)'
                @dragover.prevent
                @dragenter.prevent
            >
                <font-awesome-icon
                    icon="shopping-cart"
                    class="cart"
                    @click="show = !show"
                />
                <div class="qty" v-if="cart.length > 0"><p>{{ cart.length }}</p></div>

                <div class="cart-info-wrapper" v-show="show">
                    <p v-if="cart.length < 1" class="empty">Cart is empty.</p>
                    <div class="item" v-for="(item, key) in cart" :key="key">
                        <img :src="require(`@/assets/${item.img}.jpg`)" :alt="item.title">
                        <div class="item-info">
                            <p class="title">{{ item.title }}</p>
                            <p class="sub-title">{{ item.subtitle }}</p>
                        </div>
                        <p class="price">${{ item.price * item.qty }}</p>
                        <input type="number" min="1" v-model="item.qty">
                        <font-awesome-icon icon="trash-alt" class="trash" @click="cart.splice(key, 1)"/>
                    </div>
                </div>
            </div>
            <p class="total-amount">
                ${{ cart.reduce((a,b) => a + (b.price * b.qty), 0) }}
            </p>
        </div>
        <div class="items-wrapper">
            <div
                class="item-container"
                v-for="(item, key) in items" :key="key"
                @dragstart="startDrag($event, item)"
                draggable
            >
                <img
                    :src="require(`@/assets/${item.img}.jpg`)"
                    :alt="item.title"

                >
                <div
                    class="add-wrapper"
                    @click="addToCart(item)"
                >
                    <font-awesome-icon icon="plus" class="add"/>
                </div>
            </div>
        </div>
    </div>
</template>

<script>

export default {
	name: 'App',
	data () {
		return {
			show: false,
			totalItem: 0,

			items: [
				{
					code: '1',
					title: 'Shirt',
					subtitle: 'Shirt subtitle',
					price: 50,
					qty: 1,
					img: 'shirt'
				},

				{
					code: '2',
					title: 'Short',
					subtitle: 'Short subtitle',
					price: 150,
					qty: 1,
					img: 'short'
				},

				{
					code: '3',
					title: 'Pants',
					subtitle: 'Pants subtitle',
					price: 250,
					qty: 1,
					img: 'pants'
				}
			],

			cart: []
		}
	},

	methods: {
		addToCart (item) {
			const obj = { ...item, id: this.cart.length }
			this.cart.push(obj)
		},

		startDrag (evt, item) {
		    evt.dataTransfer.dropEffect = 'move'
		    evt.dataTransfer.effectAllowed = 'move'
		    evt.dataTransfer.setData('itemCode', item.code)
		},

		onDrop (evt) {
			const itemCode = evt.dataTransfer.getData('itemCode')
			const item = this.items.find(item => item.code === itemCode)
			this.addToCart(item)
		}
	}
}
</script>

<style lang="scss">
#app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    margin-top: 60px;
    width: 100%;

    .header {
        display: flex;
        justify-content: right;
        align-items: center;
        padding-right: 30px;

        .cart-wrapper {
            position: relative;
            margin-right: 15px;

            .cart {
                width: 40px;
                height: 40px;
                cursor: pointer;
            }

            .qty {
                height: 18px;
                width: 18px;
                position: absolute;
                top: 0;
                right: -12px;
                display: flex;
                justify-content: center;
                align-items: center;
                border: 1px solid black;
                border-radius: 50%;
                font-size: 8px;
                font-weight: 600;
                background-color: gray;
                text-align: center;
                margin: auto;
            }

            .cart-info-wrapper {
                border: 2px solid black;
                border-radius: 10px;
                position: absolute;
                right: -50px;
                padding: 20px;
                z-index: 999;
                background-color: #ffffff;

                .empty {
                    margin: 0px;
                }

                .item {
                    display: flex;
                    justify-content: center;
                    align-items: center;
                    margin-bottom: 15px;

                    img {
                        height: 60px;
                        width: 60px;
                        margin-right: 20px;
                    }

                    .item-info {
                        margin-right: 20px;
                        width: 150px;

                        p {
                            text-align: left;
                            margin: 0px;
                        }
                    }

                    .price {
                        margin-right: 20px;
                    }

                    input {
                        width: 38px;
                        margin-right: 20px;
                    }

                    .trash {
                        cursor: pointer;
                    }
                }

            }
        }
    }

    .items-wrapper {
        flex-wrap: wrap;
        display: flex;
        padding: 20px 0px;

        .item-container {
            border: 8px solid black;
            border-radius: 5px;
            position: relative;
            padding: 5px;
            margin-right: 20px;
            margin-bottom: 20px;

            img {
                height: 120px;
                width: 120px;
            }

            .add-wrapper {
                cursor: pointer;
                position: absolute;
                top: -20px;
                right: -20px;
                border: 3px solid black;
                border-radius: 50%;
                width: 40px;
                height: 40px;
                margin: auto;
                font-size: 20px;
                text-align: center;
                background-color: grey;
                display: flex;
                align-items: center;
                justify-content: center;
            }
        }
    }
}
</style>
