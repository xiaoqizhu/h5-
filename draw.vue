<template>
    <div>
		<div class="box">
			<div class="groups animation-ease" v-for="k in 3" @webkitTransitionEnd="endGame(k)">
				<div class="group-item" v-for="i in (round+1)" >
					<li class="prize-item" v-for="item in prizes">
                        <img :src="item.src">
                    </li>
				</div>
			</div>
		</div>
		<div  @click="startClick">{{disClick?'抽奖中...':'点击开始'}}</div>
	</div>
</template>
<script>
import $ from 'jquery'
import { parse } from 'path'
export default {
	data(){
		return {
			round:6,//转几回合后停下来
			prizes:[{
                src:require('../assets/2333.jpg')},
                {
                src:require('../assets/2333333.jpg')},
                {
                src:require('../assets/logo.jpg')},
            ],
			disClick:false,//防止多次点击
			itemHeight:0,//每个奖品的高度
            prize_id:'',//中奖id
		}
	},
	mounted(){
		this.itemHeight = $('.prize-item').outerHeight()
		$('.groups').css('height',this.itemHeight * this.prizes.length*(this.round+1))
	},
	methods:{
		startClick(){//开始抽奖
			if(this.disClick){
				return
            }
			//获取中奖的id
            // let index = parseInt(Math.random()*this.prizes.length);
			// this.prizd_id = this.prizes[index];
			this.runGame(2)
		},
		runGame(index){//启动抽奖
			this.disClick = true;
			this.resetGame();
			var itemHeight = this.itemHeight;
			var groupsHeight = this.round*$('.group-item').height();
			$('.groups').each(function(e){
				var pos = index*itemHeight + groupsHeight
				setTimeout(()=>{
					$(this).addClass('animation-ease').css('transform','translate3d(0, -'+pos+'px, 0)')
				},e*300)
			})
		},
        endGame(k){//抽奖结束后的回调
            if(k==3){
                alert('恭喜您中了'+ this.prizd_id)
                this.disClick = false;
            }
		},
		resetGame(){//重置状态
			$('.groups').removeClass('animation-ease').css('transform','');
		}
	}
}
</script>
<style lang="scss" scoped>
.box{
	width:300px;
	height:100px;
	overflow: hidden;
	background: #fff;
	.animation-ease{
		transition-property:transform;
		transition-duration: 3s;
		transition-timing-function: ease;
	}
	.groups{
		float: left;
		width:100px;
		text-align: center;
		.prize-item{
            list-style: none;
			width:100px;
			height:100px;
			font-size:50px;
		}
	}
    .prize-item > img{
        width: 100%;
        height: 100%;
    }
}
</style>