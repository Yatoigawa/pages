<script setup lang="ts">
import { onMounted, ref, reactive } from 'vue'
import loadImage from 'blueimp-load-image'

/**
 * コンポーネントプロパティ
 */
const props = defineProps({
  width: {
    type: String,
    required: false,
    default: '20rem'
  },
  height: {
    type: String,
    required: false,
    default: '15rem'
  },
  img: {
    type: String,
    required: true
  }
})

const metadata = reactive({
  maker: '',
  model: '',
  focalLength: '',
  focalLengthIn35mmFilm: '',
  iso: '',
  shutterSpeed: '',
  all: ''
})

const meta = ref(null)

onMounted(() => {
  getExif()
})

/**
 * ファイル名
 */
const fileTitle = () => {
  return props.img.substring(props.img.lastIndexOf('/') + 1)
}

const getExif = async () => {
  const file = await fetch(props.img).then((r) => r.blob())
  console.log(file)
  loadImage.parseMetaData(file, function (data: any) {
    console.log('All Data:', data)
    console.log('Exif data: ', data.exif)
    if (data.exif) {
      // meta.value = data.exif.getText('Make')
      meta.value = data.exif.getAll()
      metadata.maker = data.exif.getText('Make')
      metadata.model = data.exif.getText('Model')
      metadata.focalLength = data.exif.getText('FocalLength')
      metadata.focalLengthIn35mmFilm = data.exif.getText('FocalLengthIn35mmFilm')
      metadata.iso = data.exif.get('Exif').getText('PhotographicSensitivity')
      metadata.shutterSpeed = data.exif.get('Exif').getText('ShutterSpeedvalue')
      metadata.all = data.exif.get('Exif').getAll()
    }
  })
}
// const getExifString = () => {
//   return metadata.maker + ' / ' + metadata.model + ' / ISO ' + metadata.iso
// }
</script>

<template>
  <div class="frame">
    <el-container class="container">
      <el-header class="header">
        <div class="title">{{ fileTitle() }}</div>
      </el-header>
      <el-main class="main">
        <el-image
          style="{ width: 200px; height: 200px; }"
          :src="img"
          :zoom-rate="1.2"
          :max-scale="7"
          :min-scale="0.2"
          :preview-src-list="[img]"
          fit="cover"
        />
      </el-main>
      <!-- <el-footer class="footer">
        <div>{{ getExifString() }}</div>
      </el-footer> -->
    </el-container>
  </div>
</template>

<style scoped>
.frame {
  width: v-bind(width);
  /* height: v-bind(height); */
}
.container {
  border: 1px solid var(--el-border-color);
}
.header {
  height: 1rem;
}
.title {
  text-align: center;
}
.main {
  height: 100%;
}
.footer {
  height: 2rem;
}
</style>
