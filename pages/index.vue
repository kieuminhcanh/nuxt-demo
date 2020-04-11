<template>
  <v-card>
    <v-card-title>Demo AI image</v-card-title>
    <img id="target-image" class="white--text align-end" height="200px" :src="imagePath" />
    <v-card-text>
      <v-list>
        <v-list-item v-for="(item, index) in predictions" :key="index">
          <v-list-item-action>{{(item.probability * 100).toFixed(2) + "%"}}</v-list-item-action>
          <v-list-item-content>
            <v-list-item-title>{{item.className}}</v-list-item-title>
          </v-list-item-content>
        </v-list-item>
      </v-list>
    </v-card-text>
    <v-card-actions>
      <v-spacer></v-spacer>
      <v-btn @click="changeImage()">Change Image</v-btn>
      <v-btn color="primary" @click="analytics">Analytics</v-btn>
    </v-card-actions>
  </v-card>
</template>

<script>
import * as mobilenet from '@tensorflow-models/mobilenet'
export default {
  data() {
    return {
      imageIndex: 1,
      model: null,
      predictions: null
    }
  },
  computed: {
    imagePath() {
      return `/images/${this.imageIndex}.jpg`
    }
  },
  methods: {
    changeImage() {
      this.imageIndex < 9 ? this.imageIndex++ : (this.imageIndex = 1)
    },
    async analytics() {
      const img = document.getElementById('target-image')

      this.predictions = await this.model.classify(img)
    }
  },
  async mounted() {
    this.model = await mobilenet.load()
  }
}
</script>
