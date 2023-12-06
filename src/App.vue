<template>
  <div id="app">
    <h1>Sagittale PREVIEW</h1>
    <div class="container">
      <object type="image/svg+xml" :data="currentSvg" ref="svgElement"></object>
      <div class="side">
        <div class="search-box">
          <select v-model="selectedSvg" @change="updateSvg">
            <option disabled value="">Pathologie</option>
            <option value="hyperlaxityLevel3">Hyperlaxité de niveau 3</option>
            <!-- Add other options here -->
          </select>
        </div>
        <h2>Carpe</h2>
        <input type="range" min="-14.14" max="14.14" step="0.1" v-model="angleCarpe" @input="handleCarpeSliderChange">
        <h2>Boulet</h2>
        <input type="range" min="-36" max="36" step="0.1" v-model="angleBoulet" @input="handleBouletSliderChange">
        <h2>Pied</h2>
        <input type="range" min="-12" max="12" step="0.1" v-model="anglePieds" @input="handleSliderChange('ant-C', anglePieds)">

      </div>
    </div>
  </div>
</template>

<style scoped>
.container { display: flex; }
.side { margin-left: 2rem; }
input[type=range] {
  width: 100%;
  margin: 10px 0;
}
.search-box {
  margin-bottom: 1rem;
}
</style>

<script>
export default {
  data() {
    return {
      selectedSvg: '',
      svgOptions: {
        hyperlaxityLevel3: '/hyperlax3-ant.svg',
        // Add other SVG paths as needed
      },
      currentSvg: '/sagittale-ant.svg',
      rotationZ: 0,
      rotationA: 0,
      activeJarret: null,
      activeCarpe: null,
      activeBoulet: null,
      activePieds: null,
      angleJarret: 0,
      angleCarpe: 0,
      angleBoulet: 0,
      anglePieds: 0,
      totalAngle: 0,
    };
  },
  methods: {
    updateSvg() {
      const newSvgPath = this.svgOptions[this.selectedSvg];
      console.log(`Selected SVG: ${this.selectedSvg}, Path: ${newSvgPath}`);
      if (newSvgPath) {
        this.currentSvg = newSvgPath;
      } else {
        console.error('SVG path not found for the selected option');
      }
    },
    handleSliderChange(elementId, angle) {
      const strokeColor = this.determineStrokeColor(angle);
      const strokeWidth = '2pt'; // Adjust as needed
      this.rotateElement(elementId, angle, strokeColor, strokeWidth);
    },
    handleCarpeSliderChange() {
      // Adjust the shoulder (Jarret) based on Carpe angle
      // Jarret reaches its max angle of 5.8 when Carpe is at -14.14
      this.angleJarret = 5.8 * (this.angleCarpe / -14.14);

      // Ensuring Jarret does not exceed its maximum range
      if (this.angleJarret > 5.8) {
        this.angleJarret = 5.8;
      }
      if (this.angleJarret < -5.8) {
        this.angleJarret = -5.8;
      }

      this.handleSliderChange('ant-Z', this.angleJarret);
      this.handleSliderChange('ant-A', this.angleCarpe);
    },

    handleBouletSliderChange() {
      // Adjust the Carpe based on Boulet angle
      // Carpe reaches its max angle of -14.14 when Boulet is at -36
      this.angleCarpe = -14.14 * (this.angleBoulet / -36);

      // Ensuring Carpe does not exceed its maximum range
      if (this.angleCarpe > 14.14) {
        this.angleCarpe = 14.14;
      }
      if (this.angleCarpe < -14.14) {
        this.angleCarpe = -14.14;
      }

      this.handleSliderChange('ant-A', this.angleCarpe); // Adjust Carpe only
      this.handleSliderChange('ant-B', this.angleBoulet); // Continue to adjust Boulet
    },
    determineStrokeColor(angle) {
      if (angle >= 5 || angle <= -5) {
        return '#d03431'; // Red
      } else if (angle > -5 && angle < 5) {
        return '#ea9c2b'; // Orange
      } else {
        return '#5cb85c'; // Green
      }
    },
    rotateElement(elementId, angle, strokeColor, strokeWidth) {
      const svgDocument = this.$refs.svgElement.contentDocument;
      const element = svgDocument.getElementById(elementId);
      const pivot = svgDocument.getElementById(elementId + '-join');
      const devElement = svgDocument.getElementById(elementId + '-dev'); // Get the -dev element

      if (element && pivot) {
        // Calculate the pivot point
        const cx = pivot.cx.baseVal.value;
        const cy = pivot.cy.baseVal.value;

        // Apply rotation and style to the main element
        element.setAttribute('transform', `rotate(${angle}, ${cx}, ${cy})`);
        pivot.style.stroke = strokeColor;
        pivot.style.strokeWidth = strokeWidth;

        // Apply the same styles to the -dev element if it exists
        if (devElement) {
          devElement.style.stroke = strokeColor;
          devElement.style.strokeWidth = strokeWidth;
          // Apply rotation to the dev element (uncomment if needed)
          // devElement.setAttribute('transform', `rotate(${angle}, ${cx}, ${cy})`);
        }

        // Update the angle values for each part based on elementId
        if (elementId.includes('ant-Z')) {
          this.angleJarret = parseFloat(angle);
        } else if (elementId.includes('ant-A')) {
          this.angleCarpe = parseFloat(angle);
        } else if (elementId.includes('ant-B')) {
          this.angleBoulet = parseFloat(angle);
        } else if (elementId.includes('ant-C')) {
          this.anglePieds = parseFloat(angle);
        }

        // Recalculate total angle
        this.totalAngle = this.angleJarret + this.angleCarpe + this.angleBoulet + this.anglePieds;

        // Apply rotation to ant-mask-foot if needed (existing logic)
        this.applyRotationToMaskFoot();
      } else {
        console.error(`Element or pivot not found for ${elementId}`);
      }
    },
    applyRotationToMaskFoot() {
      const svgDocument = this.$refs.svgElement.contentDocument;
      const maskFoot = svgDocument.getElementById('ant-mask-foot');
      const pivot = svgDocument.getElementById('ant-C-join');

      if (maskFoot && pivot) {
        const inverseAngle = -this.totalAngle;
        const cx = pivot.cx.baseVal.value;
        const cy = pivot.cy.baseVal.value;

        maskFoot.setAttribute('transform', `rotate(${inverseAngle}, ${cx}, ${cy})`);
      }
    },
    checkRotations() {
      const svgDocument = this.$refs.svgElement.contentDocument;
      const antNormal = svgDocument.getElementById('ant-normal');
      const antExt2 = svgDocument.getElementById('ant-ext-2');

      if (this.rotationZ === 5.8 && this.rotationA === -14.14) {
        if (antNormal) antNormal.style.display = 'none';
        if (antExt2) {
          antExt2.style.display = 'block';
          antExt2.style.fill = '#c6c5c4';
          antExt2.style.stroke = '#c3c1c1';
          antExt2.style.stroke = '2pt';
        }
      } else {
        if (antNormal) antNormal.style.display = 'block';
        if (antExt2) antExt2.style.display = 'none';
      }
    }
  }
}
</script>


