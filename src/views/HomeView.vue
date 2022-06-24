<template>
  <div class="home">
    <div class="container">
      <div class="img-box">
        <img
          v-if="img"
          :src="img"
          id="img"
          alt="image"
          :style="'filter: ' + filterStyle"
        />
        <form @submit.prevent="">
          <label for="upload">Upload</label>
          <input type="file" id="upload" @change="uploadImage" />
        </form>
      </div>
      <div class="filters">
        <ul>
          <li>
            <label for="text">Text</label>
            <input
              type="text"
              id="text"
              v-model="text"
              placeholder="Enter your text"
            />
          </li>
          <li>
            <label for="text-color">Text color</label>
            <input
              type="color"
              id="text-color"
              v-model="textColor"
              placeholder="Enter your text color"
            />
          </li>
          <li>
            <label for="font-size">Font size ({{ fontSize }})</label>
            <input
              type="range"
              id="font-size"
              min="5"
              max="12"
              v-model="fontSize"
            />
          </li>
          <li>
            <label for="saturate">Saturate</label>
            <input
              type="range"
              id="saturate"
              min="0"
              max="200"
              value="100"
              @input="filterImage"
            />
          </li>
          <li>
            <label for="contrast">Contrast</label>
            <input
              type="range"
              id="contrast"
              min="0"
              max="200"
              value="100"
              @input="filterImage"
            />
          </li>
          <li>
            <label for="brightness">Brightness</label>
            <input
              type="range"
              id="brightness"
              min="0"
              max="200"
              value="100"
              @input="filterImage"
            />
          </li>
          <li>
            <label for="sepia">Sepia</label>
            <input
              type="range"
              id="sepia"
              min="0"
              max="200"
              value="0"
              @input="filterImage"
            />
          </li>
          <li>
            <label for="grayscale">Grayscale</label>
            <input
              type="range"
              id="grayscale"
              step="0.1"
              min="0"
              max="1"
              value="0"
              @input="filterImage"
            />
          </li>
          <li>
            <label for="blur">Blur</label>
            <input
              type="range"
              id="blur"
              step="0.1"
              min="0"
              max="10"
              value="0"
              @input="filterImage"
            />
          </li>
          <li>
            <label for="hueRotate">Hue Rotate</label>
            <input
              type="range"
              id="hueRotate"
              min="0"
              max="350"
              value="0"
              @input="filterImage"
            />
          </li>
          <li>
            <a download="image" :href="href" id="download">Download</a>
            <button @click="reset">Reset</button>
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "HomeView",
  data() {
    return {
      filterStyle: '',
      img: '',
      href: '',
      text: '',
      textColor: '',
      fontSize: 10,
    };
  },
  mounted() {
    let downloadFile = document.getElementById('download');
    downloadFile.addEventListener('click', this.downloadImage, false);
  },
  methods: {
    filterImage(e) {
      let style = `
      saturate(${document.getElementById('saturate').value}%)
      contrast(${document.getElementById('contrast').value}%)
      brightness(${document.getElementById('brightness').value}%)
      sepia(${document.getElementById('sepia').value}%)
      grayscale(${document.getElementById('grayscale').value})
      blur(${document.getElementById('blur').value}px)
      hue-rotate(${document.getElementById('hueRotate').value}deg)
      `;
      this.filterStyle = style;
    },
    uploadImage() {
      this.reset();
      let upload = document.getElementById('upload');
      let file = new FileReader();
      file.readAsDataURL(upload.files[0]);
      file.onload = () => {
        this.img = file.result;
      };
    },
    reset() {
      this.filterStyle = '';
      this.text = '';
      this.textColor = '#000';
      this.fontSize = 10;
      document.getElementById('saturate').value = 100;
      document.getElementById('contrast').value = 100;
      document.getElementById('brightness').value = 100;
      document.getElementById('sepia').value = 0;
      document.getElementById('grayscale').value = 0;
      document.getElementById('blur').value = 0;
      document.getElementById('hueRotate').value = 0;
    },
    downloadImage() {
      let canvas = document.createElement('canvas');
      let img = document.getElementById("img");
      canvas.width = img.width;
      canvas.height = img.height;
      let ctx = canvas.getContext('2d');
      ctx.filter = this.filterStyle;
      ctx.drawImage(img,0,0, canvas.width, canvas.height);
      ctx.lineWidth = 1;
      ctx.fillStyle = this.textColor;
      ctx.lineStyle = this.textColor;
      ctx.font = `${this.fontSize}px sans-serif`;
      ctx.fillText(this.text, 3, 12);
      let dt = canvas.toDataURL('image/jpeg');
      this.href = dt;
    }
  }
};
</script>

<style lang="scss" scoped>
.home {
  .container {
    display: grid;
    grid-template-columns: 2fr 1fr;
    gap: 20px;
    .filters {
      ul {
        width: 100%;
        height: 100%;
        display: flex;
        flex-direction: column;
        justify-content: space-between;
        gap: 20px;
        li {
          width: 100%;
          height: 100%;
          display: flex;
          flex-direction: column;
          gap: 10px;
          &:first-child {
            input {
              border: none;
              background-color: #14162b;
              border-radius: 4px;
              font-size: 15px;
              font-weight: 500;
              padding: 0 20px 0 40px;
              box-shadow: 0 0 0 2px rgb(134 140 160 / 2%);
              // background-image: url("data:image/svg+xml;charset=UTF-8,%3csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 56.966 56.966' fill='%23717790c7'%3e%3cpath d='M55.146 51.887L41.588 37.786A22.926 22.926 0 0046.984 23c0-12.682-10.318-23-23-23s-23 10.318-23 23 10.318 23 23 23c4.761 0 9.298-1.436 13.177-4.162l13.661 14.208c.571.593 1.339.92 2.162.92.779 0 1.518-.297 2.079-.837a3.004 3.004 0 00.083-4.242zM23.984 6c9.374 0 17 7.626 17 17s-7.626 17-17 17-17-7.626-17-17 7.626-17 17-17z'/%3e%3c/svg%3e");
              background-size: 14px;
              background-repeat: no-repeat;
              background-position: 16px 48%;
              color: #f9fafb;
              padding: 10px;
              outline: none;
              &::placeholder {
                color: rgb(113 119 144 / 78%);
                font-size: 15px;
                font-weight: 500;
              }
            }
          }
          &:last-child {
            flex-direction: row;
            justify-content: center;
            gap: 20px;
            height: 35px;
            a,
            button {
              color: rgba(113, 119, 144, 78%);
              background-color: #14162b;
              border: none;
              padding: 8px 26px;
              border-radius: 20px;
              cursor: pointer;
              transition: 0.3s;
              white-space: nowrap;
              &:hover {
                background-color: #0e101f;
              }
            }
          }
        }
      }
    }
    .img-box {
      display: flex;
      flex-direction: column;
      gap: 20px;
      justify-content: center;
      align-items: center;
      .loading {
        width: 40px;
      }
      img {
        width: 90%;
      }
      form {
        label {
          background-color: #3a6df0;
          border: none;
          padding: 8px 26px;
          color: #fff;
          border-radius: 20px;
          margin-top: 16px;
          cursor: pointer;
          transition: 0.3s;
          white-space: nowrap;
          &:hover {
            background-color: #1e59f1;
          }
        }
        input {
          display: none;
        }
      }
    }
  }
}

.filters::-webkit-scrollbar,
.container::-webkit-scrollbar,
.img-box::-webkit-scrollbar {
  width: 5px;
}
.filters::-webkit-scrollbar-track,
.container::-webkit-scrollbar-track,
.img-box::-webkit-scrollbar-track {
  background: transparent;
  border-radius: 10px;
}
.filters::-webkit-scrollbar-thumb,
.container::-webkit-scrollbar-thumb,
.img-box::-webkit-scrollbar-thumb {
  background-color: rgba(1, 2, 3, 0.4);
  border-radius: 10px;
}

@media (min-width: 850px) {
  .home {
    .container {
      .filters,
      .img-box {
        overflow: auto;
        padding-right: 5px;
      }
    }
  }
}

@media (max-width: 850px) {
  .home {
    .container {
      grid-template-columns: 1fr;
      overflow: auto;
      .filters,
      .img-box {
        overflow: none;
        padding-right: 0;
      }
    }
  }
}
</style>
