<style lang="stylus" rel="stylesheet/stylus">
  @import 'common/stylus/index'
  .tab
    display:flex
    width:100%
    height:40px
    line-height:40px
    border-1px(rgba(7,17,27,0.1))
    .tab-item
      flex:1
      text-align:center
      a
        display:block
        font-size:14px
        color rgb(77,85,93)
        &.active
          font-size 14px
          color rgb(240,20,20)
</style>

<template>
<div >
  <v-header :seller="seller" :isTop="isTop"></v-header>
  <div class="tab">
    <div class="tab-item">
      <router-link to="/goods">商品</router-link>
    </div>
    <div class="tab-item">
      <router-link to="/ratings">评论</router-link>
    </div>
    <div class="tab-item">
      <router-link to="/seller">商家</router-link>
    </div>
  </div>
  <keep-alive>
    <router-view :seller="seller" :isTop="isTop" @getTop="isTop = false"></router-view>
  </keep-alive>
</div>

</template>

<script>
import header from 'components/header/header'
import axios from 'axios'

const ERR_OK = 0

export default {
  data() {
    return {
	  seller: {},
	  isTop: false,
    }
  },
  created() {
    axios.get('static/data.json').then((res) => {
      this.seller = res.data.seller
	});
	this.$nextTick(()=>{
		var startX, startY; 
		let self = this;
        document.addEventListener('touchstart',function (ev) { 
            startX = ev.touches[0].pageX; 
            startY = ev.touches[0].pageY;   
        }, false); 
        document.addEventListener('touchend',function (ev) { 
            var endX, endY; 
            endX = ev.changedTouches[0].pageX; 
            endY = ev.changedTouches[0].pageY; 
            var direction = self.GetSlideDirection(startX, startY, endX, endY); 
            switch(direction) { 
                case 0: 
                    // alert("没滑动"); 
                    break; 
				case 1: 
					console.log("向上")

                    // alert("向上"); 
                    break; 
                case 2: 
					// alert("向下"); 
					console.log("向下")
					self.isTop = true;
                    break; 
                case 3: 
                    // alert("向左"); 
                    break; 
                case 4: 
                    // alert("向右"); 
                    break; 
                default:            
            } 
        }, false);
	})
},
components: {
	'v-header': header
},
methods: {
    GetSlideAngle(dx, dy) { 
      return Math.atan2(dy, dx) * 180 / Math.PI; 
	} ,
	GetSlideDirection(startX, startY, endX, endY) { 
		var dy = endY - startY; 
		var dx = endX - startX; 
		var result = 0;   
		//如果滑动距离太短 
        if(Math.abs(dx) < 2 && Math.abs(dy) < 2) { 
            return result; 
        } 
        var angle = this.GetSlideAngle(dx, dy); 
        if(angle >= -45 && angle < 45) { 
            result = 4; 
        }else if (angle >= 45 && angle < 135) { 
            result = 1; 
        }else if (angle >= -135 && angle < -45) { 
            result = 2; 
        } 
        else if ((angle >= 135 && angle <= 180) || (angle >= -180 && angle < -135)) { 
            result = 3; 
        } 
        return result; 
    } 
}
}

</script>
