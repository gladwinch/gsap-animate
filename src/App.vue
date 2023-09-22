<template>
	<div class="container">
		<h1>Vue 3 + Vite + GSAP</h1>
		<button @click="animate" 
			class="bg-blue-500 p-2 text-white text-bold rounded-md mt-6"
		>Animate</button>
		<div class="word mt-6">
			<div 
				style="position: relative;"
				v-for="(item, i) in morpheme" 
				:key="item.morpheme"
				:class="[
					i === 0 ? 'capitalize morph' : ''
				]"
			>
				{{ item.morpheme }}

				<div 
					class="line-box" 
					:ref="`line-${i}`" 
					:class="[i % 2 !== 0 ? 'flip': '']"
				> 
					<div class="line-vertical liner"></div>
					<div class="line-horizonal liner"></div>
					<div :class="[i % 2 !== 0 ? 'anti-flip': '']" v-if="showBox">
						<Card :data="item" :index="i" />
					</div>
				</div>
			</div>
		</div>
	</div>
</template>

<script>
import gsap from 'gsap'
import Card from './components/Card.vue'

export default {
	name: 'App',
	components: {
		Card
	},	
	data() {
		return {
			morpheme: [
				{
					"morpheme": "vic",
					"type": "root",
					"meaning": "conquer",
					"_id": "6507caa5ca67865ab4db453b"
				},
				{
					"morpheme": "tory",
					"type": "suffix",
					"meaning": "state or condition of",
					"_id": "6507caa5ca67865ab4db453c"
				}
			],
			showBox: false
		}
	},
	methods: {
		animate() {
			const tl = gsap.timeline()

			tl.to('.word', {
				duration: 1,
				height: 300,
				ease: 'power2.inOut'
			})

			tl.to(".morph", {
				marginRight: 30
			})

			tl.to(".line-vertical", {
				height: '100%'
			})

			tl.to(".line-horizonal", {
				width: '60%',
				onComplete: () => {
					this.showBox = true
				}
			})
		},
		// showBox() {
		// 	this.showBox = true
		// }
	},
	mounted() {
		// set styles
		for(let i = 0; i < this.morpheme.length; i++) {
			const line = this.$refs[`line-${i}`][0]
			if(i % 2 == 0) {
				line.style.bottom = '-30%'
			} else {
				line.style.top = '-30%'
			}

			line.style.right = '-0%'

			console.log('setting', line)
		}
	}
}
</script>

<style>
	.container {
		display: flex;
		align-items: center;
		justify-content: center;
		flex-direction: column;
		/* height: 20vh; */
	}

	.word {
		font-size: 4rem;
		font-weight: 500;
		display: flex;
		justify-content: center;
		align-items: center;
		padding: 2rem 4rem;
		border-radius: 10px;
		background-color: rgb(221, 221, 221);
	}

	.line-box {
		position: absolute;
		/* background-color: red; */
		height: 50px;
		width: 50px;
	}

	.flip {
		transform: scaleX(-1) rotate(180deg)
	}

	.liner {
		background-color: black;
	}

	.line-vertical {
		height: 0;
		width: 1px;
	}

	.line-horizonal {
		height: 1px;
		width: 0;
		display: flex;
		align-items: right;
	}

	.info-box {
		font-size: 14px !important;
		font-weight: 400;
		display: hidden !important;
		background-color: white;
		padding: .8rem;
		position: absolute;
		border-radius: 10px;
		top: 50px
	}

	.anti-flip {
		transform: scaleX(-1) rotate(180deg)
	}
</style>
