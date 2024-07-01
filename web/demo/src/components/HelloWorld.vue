<template>
  <div>
    <div class="button-container">
      <button @click.stop="play(0, 'anim')">play(无融合)</button>
      <button @click.stop="play(1, 'anim', 3)">play(有融合)</button>
      <button @click.stop="play(0, 'anim2')">play(无融合)</button>
      <button @click.stop="play(1, 'anim2', 4)">play(有融合)</button>
    </div>
    <div class="vap-container">
      <div ref="anim" class="anim-container"></div>
      <div ref="anim2" class="anim-container"></div>
    </div>
  </div>
</template>

<script>
import Vap from '../../../dist/vap.js';
import config from './demo.json';

export default {
  name: 'vap',
  data() {
    return {
      access: true,
      url: require('./demo.mp4'),
      vap: null,
      vap2: null,
    };
  },
  methods: {
    play(flag, container, type) {
      if (!this.access) {
        return;
      }
      this.vap = new Vap()
        .play(
          Object.assign(
            {},
            {
              container: this.$refs[container || 'anim'],
              // 素材视频链接
              src: this.url,
              // 素材配置json对象
              config: config,
              width: 900,
              height: 400,
              // 同素材生成工具中配置的保持一致
              fps: 20,
              // 是否循环
              loop: false,
              // 起始播放时间点
              beginPoint: 0,
              // 精准模式
              accurate: true,
              // 播放起始时间点(秒)
            },
            flag
              ? {
                  // 融合信息（图片/文字）,同素材生成工具生成的配置文件中的srcTag所对应，比如[imgUser] => imgUser
                  imgUser: 'https://cdn.fnmain.com/maintao/blog/2024/mowen-trans-14/3.jpeg',
                  imgAnchor: 'https://cdn.fnmain.com/maintao/blog/2024/mowen-trans-14/3.jpeg',
                  textUser: '升高',
                  textAnchor: '高高',
                  type,
                }
              : { type: 1 }
          )
        )
        .on('playing', () => {
          console.log('playing');
        })
        .on('ended', () => {
          this.vap = null;
          console.log('play ended');
        })
        .on('frame', (frame, timestamp) => {
          // frame: 当前帧(从0开始)  timestamp: (播放时间戳)
          if (frame === 50) {
            // do something
          }
          console.log(frame, '-------', timestamp);
        });
    },
    pause() {
      this.vap.pause();
    },
    playContinue() {
      this.vap.play();
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.anim-container {
  width: 900px;
  height: 600px;
  border: 1px solid #cccccc;
  margin: auto;
  margin-bottom: 20px;
}
button.disable {
  background: gray;
}
</style>
