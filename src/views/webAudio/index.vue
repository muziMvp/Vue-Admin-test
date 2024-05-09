<template>
  <div class="audioContainer">
    <el-row>
      <el-card class="box-card" style="text-align:left">
        <audio ref="audio" style="border:1px solid green">
          <source :src="audioUrl" type="audio/mpeg">
        </audio>
        <!-- <audio :src="audioUrl" ref="audio" style="width:100px;height:35px;border:1px solid green;" @loadedmetadata="onLoadedMetadata" @play="onPlay" @pause="onPause"></audio> -->
        <el-button @click="onLoadedMetadata">加载</el-button>
        <el-button @click="onPlay">播放</el-button>
        <el-button @click="onPause">暂停</el-button>
      </el-card>
    </el-row>
    <el-row>
      <el-card class="box-card" style="text-align:left">
        <div ref="visualization" style="border:1px solid orange"></div>
      </el-card>
    </el-row>
    
  </div>
</template>

<script>
import * as d3 from 'd3'

export default {
  name: 'WebAudio',
  data() {
    return {
      audioUrl: require('./wave2.mp3'),
      audioContext: null,
      audioSource: null,
      audioAnalyser: null,
      audioBuffer: null,
      audioData: null,
      audioPlaying: false,
    };
  },
  mounted() {
    this.audioContext = new AudioContext();
    this.audioSource = this.audioContext.createBufferSource();
    this.audioAnalyser = this.audioContext.createAnalyser();
    this.audioSource.connect(this.audioAnalyser);
    this.audioAnalyser.connect(this.audioContext.destination);
  },
  methods: {
    onLoadedMetadata() {
      const audioElement = this.$refs.audio;
      console.log(this.$refs.audio, 'audio')
      const audioDuration = audioElement.duration;
      const audioUrl = audioElement.currentSrc;
      console.log(audioUrl,'audioUrl')
      console.log(this.audioUrl,'audioUrlrrrrr')
      const audioRequest = new XMLHttpRequest();
      audioRequest.open('GET', audioUrl, true);
      audioRequest.responseType = 'arraybuffer';
      audioRequest.onload = () => {
        this.audioContext.decodeAudioData(audioRequest.response, (buffer) => {
          this.audioBuffer = buffer;
          this.audioData = new Uint8Array(this.audioAnalyser.frequencyBinCount);
          this.audioSource.buffer = this.audioBuffer;
          this.audioSource.start(0);
        });
      };
      audioRequest.send();
    },
    onPlay() {
      this.audioPlaying = true;
    },
    onPause() {
      this.audioPlaying = false;
    },
  },
  updated() {
    if (this.audioPlaying) {
      this.audioAnalyser.getByteFrequencyData(this.audioData);
      const visualizationElement = this.$refs.visualization;
      // 清空之前的可视化效果
      visualizationElement.innerHTML = '';
      // 使用D3.js绘制频谱图
      const svg = d3.select(visualizationElement).append('svg')
        .attr('width', '100%')
        .attr('height', '100%');
      const width = visualizationElement.clientWidth;
      const height = visualizationElement.clientHeight;
      const xScale = d3.scaleLinear().domain([0, this.audioData.length - 1]).range([0, width]);
      const yScale = d3.scaleLinear().domain([0, 255]).range([height, 0]);
      const line = d3.line().x((d, i) => xScale(i)).y(d => yScale(d));
      svg.append('path').datum(this.audioData).attr('d', line).attr('stroke', 'black').attr('stroke-width', '2').attr('fill', 'none');
    }
  },
};
</script>
<style scoped>
  .audioContainer {
    border: 1px solid red;
  }
  .el-row {
  margin-bottom: 20px;
  text-align: center;

  /* &:last-child {
    margin-bottom: 0;
  } */
}
.el-row:last-child {
  margin-bottom: 0;
}
</style>
