<template>
  <!--  <prototype-alert-modal></prototype-alert-modal>-->
  <div id="app">
    <div class="box">
      <div class="box-header">
        <h4>Déviations sagittales</h4>
      </div>
      <div class="box-body">
        <div class="top-button">
          <a @click.prevent="validateSliders"><font-awesome-icon icon="fa-solid fa-square-check" /></a>
          <a @click.prevent="resetSliders"><font-awesome-icon icon="eraser" /></a>
        </div>
        <hr>
        <div class="row" style="border-bottom: 1px solid #eee;">
          <div class="col-4">
            <div>
              <div style="color: #6c757d !important; text-align: center !important; margin-bottom: 20px;">
                <div class="badge" style="background-color: #a8a8a8 !important; color: #eeeeee; padding-right: 0.6em; padding-left: 0.6em; border-radius: 10rem;">PG</div><br>
                <span style="font-size: 80%; font-weight: 400; white-space: nowrap !important;">Postérieur gauche</span>
              </div>
            </div>
            <div class="dev-select-panel">
              <strong>Tarse</strong>
              <div class="dev-information">
                <input type="range" min="-3" max="3" step="1" :style="{ background: defaultCarpe ? '#808080' : '' }" v-model="sliderValueA" @input="handleCarpeSliderChange" @click="handleSliderClick('carpe')" :class="computeSliderClass('carpe')(angleA)"><br>
                <div class="btn btn-link" @click="resetCarpe"><font-awesome-icon icon="eraser" /></div>
                <p class="libelle">{{ getLibelleText('carpe', angleCarpe) }}</p>
              </div>
            </div>
            <div class="dev-select-panel">
              <strong>Boulet</strong>
              <div class="dev-information">
                <input type="range" min="-3" max="3" step="1" :style="{ transform: 'rotate(180deg)', background: defaultBoulet ? '#808080' : '' }" v-model="sliderValueB" @input="handleBouletSliderChange" @click="handleSliderClick('boulet')" :class="computeSliderClass('boulet')(angleB)"><br>
                <div class="btn btn-link" @click="resetBoulet"><font-awesome-icon icon="eraser" /></div>
                <p class="libelle">{{ getLibelleText('boulet', angleBoulet) }}</p>
              </div>
            </div>
            <div class="dev-select-panel">
              <strong>Pied</strong>
              <div class="dev-information">
                <input type="range" min="-3" max="3" step="1" :style="{ transform: 'rotate(180deg)', background: defaultPied ? '#808080' : '' }" v-model="sliderValueC" @input="handlePiedSliderChange" @click="handleSliderClick('pied')" :class="computeSliderClass('pied')(angleC)"><br>
                <div class="btn btn-link" @click="resetPied"><font-awesome-icon icon="eraser" /></div>
                <p class="libelle">{{ getLibelleText('pied', anglePieds) }}</p>
              </div>
            </div>
          </div>
          <div class="col-2" style="border: none; padding-right: 0px; padding-left: 0px; padding-bottom: 5px;">
            <object type="image/svg+xml" :data="currentSvg" ref="svgElement"></object>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.box{
  position: relative;
  display: flex;
  flex-direction: column;
  min-width: 0;
  word-wrap: break-word;
  background-color: #fff;
  background-clip: border-box;
  border: 1px solid rgba(0,0,0,0.125);
  border-radius: 0.25rem;
}

.box-header{
  padding: 0.75rem 1.25rem;
  margin-bottom: 0;
  border-bottom: 1px solid rgba(0,0,0,0.125);
  background: linear-gradient(to right, #aceaff, #d8f5fd);
}

.box-header h4{
  font-size: 0.9rem;
  display: inline-block;
  border-bottom: 3px solid #00b0f6;
  padding-bottom: 1px;
}

.box-body{
  flex: 1 1 auto;
  min-height: 1px;
  padding: 1.25rem;
}

.top-button{
  text-align: center;
}

.top-button a{
  cursor: pointer;
  font-weight: 400;
  color: #009fdb;
  text-decoration: none;
  padding: 6px;
  font-size: 1.3rem;
}

.row{
  display: flex;
  flex-wrap: wrap;
  margin-right: -15px;
  margin-left: -15px;
}

.col-4{
  flex: 0 0 33%;
  max-width: 33%;
  position: relative;
  width: 100%;
  padding-right: 15px;
  padding-left: 15px;
}

.col-2{
  flex: 0 0 60%;
  max-width: 60%;
  position: relative;
  width: 100%;
}

object{
  overflow: hidden;
  vertical-align: middle;
  width: 350px;
}

hr{
  margin-top: 1rem;
  margin-bottom: 1rem;
  border: 0;
  border-top: 1px solid rgba(0,0,0,0.1);
  box-sizing: content-box;
  height: 0;
  overflow: visible;
}

.badge{
  display: inline-block;
  padding: 0.25em 0.4em;
  font-size: 75%;
  font-weight: 700;
  line-height: 1;
  text-align: center;
  white-space: nowrap;
  vertical-align: baseline;
  border-radius: 0.25rem;
  transition: color 0.15s ease-in-out, background-color 0.15s ease-in-out, border-color 0.15s ease-in-out, box-shadow 0.15s ease-in-out;
}

strong{
  font-weight: bolder;
}

.btn{
  display: inline-block;
  font-weight: 400;
  color: #212529;
  text-align: center;
  vertical-align: middle;
  -webkit-user-select: none;
  -moz-user-select: none;
  user-select: none;
  background-color: transparent;
  border: 1px solid transparent;
  padding: 0.375rem 0.75rem;
  font-size: 0.9rem;
  line-height: 1.5;
  border-radius: 0.25rem;
  transition: color 0.15s ease-in-out, background-color 0.15s ease-in-out, border-color 0.15s ease-in-out, box-shadow 0.15s ease-in-out;
}

.btn-link{
  font-weight: 400;
  color: #009fdb;
  text-decoration: none;
  cursor: pointer;
}

.libelle{
  height: 34px;
  font-size: 80%;
  font-weight: 400;
  color: #6c757d !important;
}

div{
  display: block;
}

@media only screen and (min-device-width : 320px) and (max-device-width : 705px) {
  .col-4{
    padding-right: 0;
  }
  object{
    width: 300px;
  }
}

p{margin: 10px}
h2{margin: 0px}
.slider{height: 120px; width: 200px; display: flex; flex-direction: column; justify-content: center}
.container { display: flex; }
.side { margin-right: 2rem; }

.slider-thumb {
  -webkit-appearance: none;
  appearance: none;
  width: 25px;
  height: 25px;
  border-radius: 50%;
  background-color: grey;
  cursor: pointer;
}

.slider-thumb.green { background: #5CB85C; }
.slider-thumb.light-red { background: #ED9D2B; }
.slider-thumb.red { background: #D23430; }
.slider-thumb.dark-red { background: #D23430; }

input[type=range] {
  width: 100%;
  margin: 10px 0;
  -webkit-appearance: none;
  appearance: none;
  height: 10px;
  border-radius: 5px;
  background: #ddd;
  outline: none;
  opacity: 0.7;
  -webkit-transition: .2s;
  transition: opacity .2s;
}

input[type=range]:hover {
  opacity: 1;
}

</style>

<script>
import PrototypeAlertModal from './components/PrototypeAlertModal.vue';
export default {
  components: {
    PrototypeAlertModal
  },
  data() {
    return {
      selectedSvg: '',
      SIDE_DEVIATION: {
        A_DEGS: [-14.1, -9.4, -4.7, 0, 4.7, 9.4, 14.1],
        B_DEGS: [-36, -24, -12, 0, 12, 24, 36],
        C_DEGS: [-21, -9, -6, 0, 4, 8, 12],
      },
      sliderValueA: 0,
      sliderValueB: 0,
      sliderValueC: 0,
      colorThresholds: {
        carpe: {
          lightRedNegative: -4.7, redNegative: -9.4, darkRedNegative: -14.1,
          lightRed: 4.7, red: 9.4, darkRed: 14.1
        },
        boulet: {
          lightRedNegative: -12, redNegative: -24, darkRedNegative: -36,
          lightRed: 12, red: 24, darkRed: 36
        },
        pied: {
          lightRedNegative: -4, redNegative: -8, darkRedNegative: -12,
          lightRed: 4, red: 8, darkRed: 12
        },
      },
      currentSvg: '/dev-post-left.svg',
      rotationZ: 0,
      rotationA: 0,
      activeJarret: null,
      activeCarpe: null,
      activeBoulet: null,
      activePieds: null,
      angleJarret: 0,
      angleCarpe: 0,
      compensatedAngleCarpe: 0,
      angleBoulet: 0,
      anglePieds: 0,
      totalAngle: 0,
      totalAngleWithoutFoot: 0,
      calculatedAngleCarpe: 0,
      defaultCarpe: true,
      defaultBoulet: true,
      defaultPied: true,
      userInteractedCarpe: false,
      userInteractedBoulet: false,
      userInteractedPied: false,
      maskFootMoved: false,
    };
  },
  computed: {
    angleA() {
      const index = this.mapSliderValueToIndex(this.sliderValueA);
      const value = this.SIDE_DEVIATION.A_DEGS[index];
      return value;
    },
    angleB() {
      const index = this.mapSliderValueToIndex(this.sliderValueB);
      const value = this.SIDE_DEVIATION.B_DEGS[index];
      return value;
    },
    angleC() {
      const index = this.mapSliderValueToIndex(this.sliderValueC);
      const value = this.SIDE_DEVIATION.C_DEGS[index];
      return value;
    },
    // ...
  },
  methods: {
    adjustMaskFoot() {
      const svgDocument = this.$refs.svgElement.contentDocument;
      const maskFoot = svgDocument.getElementById('post-mask-foot');

      if (maskFoot) {
        const currentY = parseFloat(maskFoot.getAttribute('y') || 0);

        if (this.angleB >= 24 && !this.maskFootMoved) {
          maskFoot.setAttribute('y', currentY + 8);
          this.maskFootMoved = true;
        } else if (this.angleB < 24 && this.maskFootMoved) {
          maskFoot.setAttribute('y', currentY - 8);
          this.maskFootMoved = false;
        }
      }
    },
    mapSliderValueToIndex(sliderValue) {
      console.log('Received sliderValue:', sliderValue); // Log pour vérifier la valeur reçue
      const index = +sliderValue + 3; // Force la conversion en nombre
      console.log('Computed index:', index); // Log pour vérifier l'index calculé

      if (index >= 0 && index < this.SIDE_DEVIATION.B_DEGS.length) {
        return index;
      } else {
        console.error('Index out of bounds:', index);
        return 0; // Retournez une valeur sûre
      }
    },
    validateSliders() {
      if (this.defaultCarpe) {
        this.calculatedAngleCarpe = 0;
        this.angleCarpe = 0;
        this.handleCarpeSliderChange();
        this.defaultCarpe = false;
      }

      if (this.defaultBoulet) {
        this.angleBoulet = 0;
        this.handleBouletSliderChange();
        this.defaultBoulet = false;
      }

      if (this.defaultPied) {
        this.anglePieds = 0;
        this.handlePiedSliderChange();
        this.defaultPied = false;
      }

      this.updateSvgElementsForValidation();
    },
    updateSvgElementsForValidation() {
      const svgDocument = this.$refs.svgElement.contentDocument;
      if (svgDocument) {
        if (!this.defaultCarpe) {
          this.updateSvgElementColor(svgDocument, 'post-B', '#5CB85C');
        }
        if (!this.defaultBoulet) {
          this.updateSvgElementColor(svgDocument, 'post-C', '#5CB85C');
        }
        if (!this.defaultPied) {
          this.updateSvgElementColor(svgDocument, 'post-D', '#5CB85C');
        }
      }
    },
    updateSvgElementColor(svgDocument, elementId, color) {
      const devElement = svgDocument.getElementById(elementId + '-dev');
      const devTopElement = svgDocument.getElementById(elementId + '-dev-top');
      const joinElement = svgDocument.getElementById(elementId + '-join');

      if (devElement) {
        devElement.style.stroke = color;
        devElement.style.strokeWidth = '2pt';
      }
      if (devTopElement) {
        devTopElement.style.stroke = color;
        devTopElement.style.strokeWidth = '2pt';
      }
      if (joinElement) {
        joinElement.style.stroke = color;
      }
    },
    updateSvgElementStyle(elementId, color, strokeWidth) {
      const svgDocument = this.$refs.svgElement.contentDocument;
      if (svgDocument) {
        const devElement = svgDocument.getElementById(elementId + '-dev');
        const devTopElement = svgDocument.getElementById(elementId + '-dev-top');
        const joinElement = svgDocument.getElementById(elementId + '-join');

        if (devElement) {
          devElement.style.stroke = color;
          devElement.style.strokeWidth = strokeWidth;
        }
        if (devTopElement) {
          devTopElement.style.stroke = color;
          devTopElement.style.strokeWidth = strokeWidth;
        }
        if (joinElement) {
          joinElement.style.stroke = color;
          joinElement.style.strokeWidth = strokeWidth;
        }
      }
    },
    resetSliders() {
      this.angleCarpe = 0;
      this.angleBoulet = 0;
      this.anglePieds = 0;

      this.angleA = 0;
      this.angleB = 0;
      this.angleC = 0;

      this.sliderValueA = 0;
      this.sliderValueB = 0;
      this.sliderValueC = 0;

      this.handlePiedSliderChange();
      this.handleCarpeSliderChange();
      this.handleBouletSliderChange();

      this.defaultPied = true;
      this.defaultCarpe = true;
      this.defaultBoulet = true;

      this.userInteractedCarpe = false;
      this.userInteractedBoulet = false;
      this.userInteractedPied = false;

      const svgDocument = this.$refs.svgElement.contentDocument;
      if (svgDocument) {
        ['post-A', 'post-B', 'post-C', 'post-D'].forEach(elementId => {
          const devElement = svgDocument.getElementById(elementId + '-dev');
          const devTopElement = svgDocument.getElementById(elementId + '-dev-top');
          const joinElement = svgDocument.getElementById(elementId + '-join');

          if (devElement) {
            devElement.style.strokeWidth = '0';
          }
          if (devTopElement) {
            devTopElement.style.strokeWidth = '0';
          }
          if (joinElement) {
            joinElement.style.stroke = '#b8b7b7';
          }
        });
      }
    },
    resetCarpe() {
      this.calculatedAngleCarpe = 0;
      this.angleCarpe = 0;
      this.handleCarpeSliderChange();
      this.defaultCarpe = true;
      this.userInteractedCarpe = false;
      this.adjustSvgElements('post-B');
    },
    resetBoulet() {
      this.angleBoulet = 0;
      this.handleBouletSliderChange();
      this.defaultBoulet = true;
      this.userInteractedBoulet = false;
      this.adjustSvgElements('post-C');

      if (this.defaultCarpe) {
        this.adjustSvgElements('post-B');
      }
    },
    resetPied() {
      this.anglePieds = 0;
      this.handlePiedSliderChange();
      this.defaultPied = true;
      this.userInteractedPied = false;
      this.adjustSvgElements('post-D');
    },
    adjustSvgElements(elementId) {
      const svgDocument = this.$refs.svgElement.contentDocument;
      if (svgDocument) {
        const devElement = svgDocument.getElementById(elementId + '-dev');
        const devTopElement = svgDocument.getElementById(elementId + '-dev-top');
        const joinElement = svgDocument.getElementById(elementId + '-join');

        if (devElement) {
          devElement.style.strokeWidth = '0';
        }
        if (devTopElement) {
          devTopElement.style.strokeWidth = '0';
        }
        if (joinElement) {
          joinElement.style.stroke = '#b8b7b7';
        }
      }
    },
    getLibelleText(sliderName, value) {
      if ((sliderName === 'carpe' && this.defaultCarpe) ||
          (sliderName === 'boulet' && this.defaultBoulet) ||
          (sliderName === 'pied' && this.defaultPied)) {
        return '';
      }

      value = parseFloat(value);
      if (sliderName === 'carpe') {
        if (value <= -14.1) return 'Extension Sévère';
        if (value <= -9.4) return 'Extension Modérée';
        if (value <= -4.7) return 'Extension Discrète';
        if (value >= 14.1) return 'Flexion Sévère';
        if (value >= 9.4) return 'Flexion Modérée';
        if (value >= 4.7) return 'Flexion Discret';
      }
      if (sliderName === 'boulet') {
        if (value <= -36) return 'Flexion Sévère';
        if (value <= -24) return 'Flexion Modérée';
        if (value <= -12) return 'Flexion Discrète';
        if (value >= 36) return 'Extension Sévère';
        if (value >= 24) return 'Extension Modérée';
        if (value >= 12) return 'Extension Discrète';
      }
      if (sliderName === 'pied') {
        if (value <= -12) return 'Flexion Sévère';
        if (value <= -8) return 'Flexion Modérée';
        if (value <= -4) return 'Flexion Discrète';
        if (value >= 12) return 'Extension Sévère';
        if (value >= 8) return 'Extension Modérée';
        if (value >= 4) return 'Extension Discrète';
      }
      return 'Normal';
    },
    computeSliderClass(sliderName) {
      return (value) => {
        value = parseFloat(value);
        const thresholds = this.colorThresholds[sliderName];

        if (value <= thresholds.darkRedNegative) return 'slider-thumb dark-red';
        if (value <= thresholds.redNegative) return 'slider-thumb red';
        if (value <= thresholds.lightRedNegative) return 'slider-thumb light-red';
        if (value >= thresholds.darkRed) return 'slider-thumb dark-red';
        if (value >= thresholds.red) return 'slider-thumb red';
        if (value >= thresholds.lightRed) return 'slider-thumb light-red';
        return 'slider-thumb green';
      };
    },
    handleSliderChange(elementId, angle) {
      const sliderName = elementId.includes('post-B') ? 'carpe' :
          elementId.includes('post-C') ? 'boulet' :
              'pied';

      if ((sliderName === 'carpe' && this.userInteractedCarpe) ||
          (sliderName === 'boulet' && this.userInteractedBoulet) ||
          (sliderName === 'pied' && this.userInteractedPied)) {
        if (sliderName === 'carpe') this.defaultCarpe = false;
        if (sliderName === 'boulet') this.defaultBoulet = false;
        if (sliderName === 'pied') this.defaultPied = false;
      }

      const strokeColor = this.determineStrokeColor(sliderName, angle);
      const strokeWidth = '2pt';
      this.rotateElement(elementId, angle, strokeColor, strokeWidth);
    },
    handleCarpeSliderChange() {
      this.userInteractedCarpe = true;
      this.defaultCarpe = false;

      // Utilisez la valeur calculée pour angleCarpe.
      this.angleCarpe = this.angleA;

      console.log('angleA:', this.angleA); // Vérifiez la valeur de angleA
      console.log('angleCarpe:', this.angleCarpe); // Vérifiez la valeur de angleCarpe

      // Calculez angleJarret en fonction de angleCarpe.
      this.angleJarret = 5.8 * (this.angleCarpe / -14.14);

      // Assurez-vous que angleJarret reste dans les bornes [-5.8, 5.8].
      this.angleJarret = Math.max(Math.min(this.angleJarret, 5.8), -5.8);

      console.log('angleJarret:', this.angleJarret); // Vérifiez la valeur de angleJarret

      // Vérifiez si les valeurs sont des nombres valides avant de les utiliser
      if (!isNaN(this.angleJarret) && !isNaN(this.angleCarpe)) {
        // Appliquez les changements aux éléments SVG.
        this.handleSliderChange('post-A', this.angleJarret);
        this.handleSliderChange('post-B', this.angleCarpe);
      } else {
        console.error('Invalid angleJarret or angleCarpe');
      }
    },
    handleBouletSliderChange() {
      this.userInteractedBoulet = true;
      this.defaultBoulet = false;
      this.handleSliderChange('post-C', this.angleB);
    },
    handlePiedSliderChange() {
      this.userInteractedPied = true;
      this.defaultPied = false;
      this.handleSliderChange('post-D', this.angleC);
    },
    handleSliderClick(sliderName) {
      if (sliderName === 'carpe' && this.defaultCarpe) {
        this.userInteractedCarpe = true;
        this.defaultCarpe = false;
        this.updateSvgElementStyle('post-B', '#5CB85C', '2pt');
      } else if (sliderName === 'boulet' && this.defaultBoulet) {
        this.userInteractedBoulet = true;
        this.defaultBoulet = false;
        this.updateSvgElementStyle('post-C', '#5CB85C', '2pt');
      } else if (sliderName === 'pied' && this.defaultPied) {
        this.userInteractedPied = true;
        this.defaultPied = false;
        this.updateSvgElementStyle('post-D', '#5CB85C', '2pt');
      }
    },
    determineStrokeColor(sliderName, angle) {
      angle = parseFloat(angle);
      if (sliderName === 'carpe') {
        if (angle <= -14.1 || angle >= 14.1) return '#d03431';
        if (angle <= -9.4 || angle >= 9.4) return '#d03431';
        if (angle <= -4.7 || angle >= 4.7) return '#ea9c2b';
      } else if (sliderName === 'boulet') {
        if (angle <= -36 || angle >= 36) return '#d03431';
        if (angle <= -24 || angle >= 24) return '#d03431';
        if (angle <= -12 || angle >= 12) return '#ea9c2b';
      } else if (sliderName === 'pied') {
        if (angle <= -12 || angle >= 12) return '#d03431';
        if (angle <= -8 || angle >= 8) return '#d03431';
        if (angle <= -4 || angle >= 4) return '#ea9c2b';
      }
      return '#5cb85c'; // Vert par défaut
    },
    rotateElement(elementId, angle, strokeColor, strokeWidth) {
      const svgDocument = this.$refs.svgElement.contentDocument;
      const element = svgDocument.getElementById(elementId);
      const pivot = svgDocument.getElementById(elementId + '-join');
      const devElement = svgDocument.getElementById(elementId + '-dev');
      const devTopElement = svgDocument.getElementById(elementId + '-dev-top');

      if (element && pivot) {
        const cx = pivot.cx.baseVal.value;
        const cy = pivot.cy.baseVal.value;

        element.setAttribute('transform', `rotate(${angle}, ${cx}, ${cy})`);
        if (pivot.id !== "post-A-join") {
          pivot.style.stroke = strokeColor;
          pivot.style.strokeWidth = strokeWidth;
        }

        if (elementId.includes('post-C')) {
          this.angleBoulet = parseFloat(angle);
          this.angleB = this.angleBoulet; // Assurez-vous que this.angleB est mis à jour avec la nouvelle valeur
          this.adjustMaskFoot(); // Ajustez la position du maskFoot si nécessaire
        }

        if (devElement) {
          devElement.style.stroke = strokeColor;
          devElement.style.strokeWidth = strokeWidth;
          if (devTopElement) {
            devTopElement.style.stroke = strokeColor;
            devTopElement.style.strokeWidth = strokeWidth;
          }
        }

        if (elementId.includes('post-A')) {
          this.angleJarret = parseFloat(angle);
        } else if (elementId.includes('post-B')) {
          this.angleCarpe = parseFloat(angle);
        } else if (elementId.includes('post-C')) {
          this.angleBoulet = parseFloat(angle);
        } else if (elementId.includes('post-D')) {
          this.anglePieds = parseFloat(angle);
        }

        this.totalAngleWithoutFoot = this.angleJarret + this.angleCarpe + this.angleBoulet;
        this.totalAngle = this.angleJarret + this.angleCarpe + this.angleBoulet + this.anglePieds;

        this.applyRotationToMaskFoot();
        this.applyRotationToFoot();
      } else {
        console.error(`Element or pivot not found for ${elementId}`);
      }
    },
    applyRotationToFoot() {
      const svgDocument = this.$refs.svgElement.contentDocument;
      const foot = svgDocument.getElementById('post-normal');
      const footDev = svgDocument.getElementById('post-D-dev');
      const footJoin = svgDocument.getElementById('post-E-join');
      const pivot = svgDocument.getElementById('post-D-join');

      if (foot && pivot) {
        const inverseAngle = -this.totalAngleWithoutFoot;
        const cx = pivot.cx.baseVal.value;
        const cy = pivot.cy.baseVal.value;

        foot.setAttribute('transform', `rotate(${inverseAngle}, ${cx}, ${cy})`);
        footDev.setAttribute('transform', `rotate(${inverseAngle}, ${cx}, ${cy})`);
        footJoin.setAttribute('transform', `rotate(${inverseAngle}, ${cx}, ${cy})`);
      }
    },
    applyRotationToMaskFoot() {
      const svgDocument = this.$refs.svgElement.contentDocument;
      const maskFoot = svgDocument.getElementById('post-mask-foot');
      const pivot = svgDocument.getElementById('post-E-join');

      if (maskFoot && pivot) {
        const inverseAngle = -this.totalAngle;
        const cx = pivot.cx.baseVal.value;
        const cy = pivot.cy.baseVal.value;


        maskFoot.setAttribute('transform', `rotate(${inverseAngle}, ${cx}, ${cy})`);
      }
    },
  }
}
</script>
