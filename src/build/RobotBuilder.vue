<template>
	<div class="content">
		<h2 class="robot-name">
			{{ selectedRobot.head.title }} {{ selectedRobot.leftArm.title }} {{ selectedRobot.torso.title }}
			{{ selectedRobot.rightArm.title }} {{ selectedRobot.base.title }}
		</h2>
		<button class="add-to-cart" @click="addToCart()">Add to cart</button>
		<div class="top-row">
			<div class="top part" :class="{ borderRed: selectedRobot.head.onSale }">
				<div class="sale" v-if="selectedRobot.head.onSale">Sale!</div>
				<img :src="selectedRobot.head.src" title="head" />
				<button @click="selectHead('prev')" class="prev-selector">&#9668;</button>
				<button @click="selectHead('next')" class="next-selector">&#9658;</button>
			</div>
		</div>
		<div class="middle-row">
			<div class="left part">
				<img :src="selectedRobot.leftArm.src" title="left arm" />
				<button @click="selectLeftArm('prev')" class="prev-selector">&#9650;</button>
				<button @click="selectLeftArm('next')" class="next-selector">&#9660;</button>
			</div>
			<div class="center part">
				<img :src="selectedRobot.torso.src" title="left arm" />
				<button @click="selectTorso('prev')" class="prev-selector">&#9668;</button>
				<button @click="selectTorso('next')" class="next-selector">&#9658;</button>
			</div>
			<div class="right part">
				<img :src="selectedRobot.rightArm.src" title="left arm" />
				<button @click="selectRightArm('prev')" class="prev-selector">&#9650;</button>
				<button @click="selectRightArm('next')" class="next-selector">&#9660;</button>
			</div>
		</div>
		<div class="bottom-row">
			<div class="bottom part">
				<img :src="selectedRobot.base.src" title="left arm" />
				<button @click="selectBase('prev')" class="prev-selector">&#9668;</button>
				<button @click="selectBase('next')" class="next-selector">&#9658;</button>
			</div>
		</div>
		<div>
			<h3>Cart</h3>
			<table>
				<thead>
					<tr>
						<th>Robot</th>
						<th class="cost">Cost</th>
					</tr>
				</thead>
				<tbody>
					<tr v-for="(robot, index) in cart" :key="index">
						<td>
							<div>
								{{ robot.head.title }}
							</div>
							<div>
								{{ robot.leftArm.title }}
							</div>
							<div>
								{{ robot.torso.title }}
							</div>
							<div>
								{{ robot.rightArm.title }}
							</div>
							<div>
								{{ robot.base.title }}
							</div>
							<div>Total:</div>
						</td>

						<td class="cost">
							<div :class="{ costSale: robot.head.onSale }">{{ robot.head.cost.toFixed(2) }}$</div>
							<div>{{ robot.leftArm.cost.toFixed(2) }}$</div>
							<div>{{ robot.torso.cost.toFixed(2) }}$</div>
							<div>{{ robot.rightArm.cost.toFixed(2) }}$</div>
							<div>{{ robot.base.cost.toFixed(2) }}$</div>

							<div class="total">{{ robot.cost.toFixed(2) }}$</div>
						</td>
					</tr>
				</tbody>
			</table>
		</div>
	</div>
</template>

<script>
import availableParts from '../data/parts'
import createdHookMixin from './created-hook-mixin'

function getPreviousValidIndex(index, length) {
	const deprecatedIndex = index - 1
	return deprecatedIndex < 0 ? length - 1 : deprecatedIndex
}

function getNextValidIndex(index, length) {
	const deprecatedIndex = index + 1
	return deprecatedIndex >= length ? 0 : deprecatedIndex
}

export default {
	name: 'RobotBuilder',
	data() {
		return {
			availableParts,
			selectHeadIndex: 0,
			selectLeftArmIndex: 0,
			selectTorsoIndex: 0,
			selectRightArmIndex: 0,
			selectBaseIndex: 0,
			cart: []
		}
	},
	mixins: [createdHookMixin],
	computed: {
		selectedRobot() {
			return {
				head: availableParts.heads[this.selectHeadIndex],
				leftArm: availableParts.arms[this.selectLeftArmIndex],
				torso: availableParts.torsos[this.selectTorsoIndex],
				rightArm: availableParts.arms[this.selectRightArmIndex],
				base: availableParts.bases[this.selectBaseIndex]
			}
		}
	},
	methods: {
		addToCart() {
			const robot = this.selectedRobot
			const cost = robot.head.cost + robot.leftArm.cost + robot.torso.cost + robot.rightArm.cost + robot.base.cost
			this.cart.push({
				...robot,
				cost
			})
		},
		selectHead(position) {
			console.log('selectHead()')
			if (position === 'prev') {
				this.selectHeadIndex = getPreviousValidIndex(this.selectHeadIndex, this.availableParts.heads.length)
			}
			if (position === 'next') {
				this.selectHeadIndex = getNextValidIndex(this.selectHeadIndex, this.availableParts.heads.length)
			}
		},
		selectLeftArm(position) {
			console.log('selectLeftArm()')
			if (position === 'prev') {
				this.selectLeftArmIndex = getPreviousValidIndex(this.selectLeftArmIndex, this.availableParts.arms.length)
			}
			if (position === 'next') {
				this.selectLeftArmIndex = getNextValidIndex(this.selectLeftArmIndex, this.availableParts.arms.length)
			}
		},
		selectTorso(position) {
			console.log('selectLeftArm()')
			if (position === 'prev') {
				this.selectTorsoIndex = getPreviousValidIndex(this.selectTorsoIndex, this.availableParts.torsos.length)
			}
			if (position === 'next') {
				this.selectTorsoIndex = getNextValidIndex(this.selectTorsoIndex, this.availableParts.torsos.length)
			}
		},
		selectRightArm(position) {
			if (position === 'prev') {
				this.selectRightArmIndex = getPreviousValidIndex(this.selectRightArmIndex, this.availableParts.arms.length)
			}
			if (position === 'next') {
				this.selectRightArmIndex = getNextValidIndex(this.selectRightArmIndex, this.availableParts.arms.length)
			}
		},
		selectBase(position) {
			if (position === 'prev') {
				this.selectBaseIndex = getPreviousValidIndex(this.selectBaseIndex, this.availableParts.bases.length)
			}
			if (position === 'next') {
				this.selectBaseIndex = getNextValidIndex(this.selectBaseIndex, this.availableParts.bases.length)
			}
		}
	}
}
</script>

<style scoped lang="scss">
.part {
	position: relative;
	width: 165px;
	height: 165px;
	border: 3px solid #aaa;
}
.part img {
	width: 165px;
}
.top-row {
	display: flex;
	justify-content: space-around;
}

.top {
	display: flex;
	align-items: flex-end;
}
.middle-row {
	display: flex;
	justify-content: center;
}
.bottom-row {
	display: flex;
	justify-content: space-around;
	border-top: none;
}
.head {
	border-bottom: none;
}
.left {
	border-right: none;
}
.right {
	border-left: none;
}
.left img {
	transform: rotate(-90deg);
}
.right img {
	transform: rotate(90deg);
}
.bottom {
	border-top: none;
}
.prev-selector {
	position: absolute;
	z-index: 1;
	top: -3px;
	left: -28px;
	width: 25px;
	height: 171px;
}
.next-selector {
	position: absolute;
	z-index: 1;
	top: -3px;
	right: -28px;
	width: 25px;
	height: 171px;
}
.center .prev-selector,
.center .next-selector {
	opacity: 0.8;
}
.left .prev-selector {
	top: -28px;
	left: -3px;
	width: 144px;
	height: 25px;
}
.left .next-selector {
	top: auto;
	bottom: -28px;
	left: -3px;
	width: 144px;
	height: 25px;
}
.right .prev-selector {
	top: -28px;
	left: 24px;
	width: 144px;
	height: 25px;
}
.right .next-selector {
	top: auto;
	bottom: -28px;
	left: 24px;
	width: 144px;
	height: 25px;
}
.right .next-selector {
	right: -3px;
}

.content {
	position: relative;
}

.robot-name {
	position: absolute;
	top: -25px;
	left: 50%;
	transform: translateX(-50%);
	text-align: center;
	width: 100%;
	font-size: 16px;
	margin: 0;
}

.sale {
	position: absolute;
	top: 2px;
	color: red;
}

.borderRed {
	border: 3px solid red;
}

.add-to-cart {
	position: absolute;
	right: 20px;
	width: 220px;
	padding: 3px;
	font-size: 16px;
}

td,
th {
	text-align: left;
	padding: 5px;
	padding-right: 20px;
}

.cost {
	text-align: right;
	.total {
		font-weight: bold;
	}
	.costSale {
		color: red;
	}
}
</style>
