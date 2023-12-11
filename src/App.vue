<template>
  <prototype-alert-modal></prototype-alert-modal>
  <div id="app">
    <div class="box">
      <div class="box-header">
        <h4>Déviations sagittale</h4>
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
                <div class="badge" style="background-color: #a8a8a8 !important; color: #eeeeee; padding-right: 0.6em; padding-left: 0.6em; border-radius: 10rem;">AG</div><br>
                <span style="font-size: 80%; font-weight: 400; white-space: nowrap !important;">Antérieur gauche</span>
              </div>
            </div>
            <div class="dev-select-panel">
              <strong>Carpe</strong>
              <div class="dev-information">
                <input type="range" min="-14.14" max="14.14" step="4.7133" :style="{ background: defaultCarpe ? '#808080' : '' }" v-model="calculatedAngleCarpe" @input="handleCarpeSliderChange" :class="computeSliderClass('carpe')(angleCarpe)"><br>
                <div class="btn btn-link" @click="resetCarpe"><font-awesome-icon icon="eraser" /></div>
                <p class="libelle">{{ getLibelleText('carpe', angleCarpe) }}</p>
              </div>
            </div>
            <div class="dev-select-panel">
              <strong>Boulet</strong>
              <div class="dev-information">
                <input type="range" min="-36" max="36" step="12" :style="{ background: defaultBoulet ? '#808080' : '' }" v-model="angleBoulet" @input="handleBouletSliderChange" :class="computeSliderClass('boulet')(angleBoulet)">
                <div class="btn btn-link" @click="resetBoulet"><font-awesome-icon icon="eraser" /></div>
                <p class="libelle">{{ getLibelleText('boulet', angleBoulet) }}</p>
              </div>
            </div>
            <div class="dev-select-panel">
              <strong>Pied</strong>
              <div class="dev-information">
                <input type="range" min="-12" max="12" step="4" :style="{ background: defaultPied ? '#808080' : '' }" v-model="anglePieds" @input="handlePiedSliderChange" :class="computeSliderClass('pied')(anglePieds)">
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
  width: 400px;
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




p{margin: 10px}
h2{margin: 0px}
.slider{height: 120px; width: 200px; display: flex; flex-direction: column; justify-content: center}
.container { display: flex; }
.side { margin-right: 2rem; }
/* Style for the slider thumb */
.slider-thumb {
  -webkit-appearance: none;
  appearance: none;
  width: 25px;
  height: 25px;
  border-radius: 50%;
  background-color: grey;
  cursor: pointer;
}

/* Dynamic background colors based on class */
.slider-thumb.green { background: #5CB85C; }
.slider-thumb.light-red { background: #ED9D2B; }
.slider-thumb.red { background: #D23430; }
.slider-thumb.dark-red { background: #D23430; }

/* Styles for the slider track */
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
      svgOptions: {
        hyperlaxityLevel3: '/hyperlax3-ant.svg',
        boultureLevel4:'/boulture4-ant.svg'
        // Add other SVG paths as needed
      },
      colorThresholds: {
        carpe: {
          lightRedNegative: -4.7134, redNegative: -9.4267, darkRedNegative: -14.14,
          lightRed: 4.7132, red: 9.4265, darkRed: 14.1398
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
      currentSvg: '/sagittale-ant.svg',
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
      calculatedAngleCarpe: 0,
      defaultCarpe: true,
      defaultBoulet: true,
      defaultPied: true,
      userInteractedCarpe: false,
      userInteractedBoulet: false,
      userInteractedPied: false,
    };
  },
  methods: {
    validateSliders() {
      // Vérifier et mettre à jour le slider Carpe si son état par défaut est true
      if (this.defaultCarpe) {
        this.calculatedAngleCarpe = 0;
        this.angleCarpe = 0;
        this.handleCarpeSliderChange();
        this.defaultCarpe = false;
      }

      // Vérifier et mettre à jour le slider Boulet si son état par défaut est true
      if (this.defaultBoulet) {
        this.angleBoulet = 0;
        this.handleBouletSliderChange();
        this.defaultBoulet = false;
      }

      // Vérifier et mettre à jour le slider Pied si son état par défaut est true
      if (this.defaultPied) {
        this.anglePieds = 0;
        this.handlePiedSliderChange();
        this.defaultPied = false;
      }

      // Mettre à jour les éléments SVG en vert pour ceux modifiés
      this.updateSvgElementsForValidation();
    },

    updateSvgElementsForValidation() {
      const svgDocument = this.$refs.svgElement.contentDocument;
      if (svgDocument) {
        // Mettre à jour chaque partie du SVG en vert si l'état par défaut était true
        if (!this.defaultCarpe) {
          this.updateSvgElementColor(svgDocument, 'ant-A', '#5CB85C');
        }
        if (!this.defaultBoulet) {
          this.updateSvgElementColor(svgDocument, 'ant-B', '#5CB85C');
        }
        if (!this.defaultPied) {
          this.updateSvgElementColor(svgDocument, 'ant-C', '#5CB85C');
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
    resetSliders() {
      // Réinitialiser les valeurs des sliders
      this.calculatedAngleCarpe = 0;
      this.angleCarpe = 0;
      this.angleBoulet = 0;
      this.anglePieds = 0;

      // Appeler les méthodes de gestion des sliders pour mettre à jour les SVG
      this.handlePiedSliderChange();
      this.handleCarpeSliderChange();
      this.handleBouletSliderChange();

      // Réinitialiser les états par défaut
      this.defaultPied = true;
      this.defaultCarpe = true;
      this.defaultBoulet = true;

      this.userInteractedCarpe = false;
      this.userInteractedBoulet = false;
      this.userInteractedPied = false;

      const svgDocument = this.$refs.svgElement.contentDocument;
      if (svgDocument) {
        // Modifier les éléments -dev, -dev-top et -join
        ['ant-A', 'ant-B', 'ant-C'].forEach(elementId => {
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
      this.adjustSvgElements('ant-A');
    },

    resetBoulet() {
      this.angleBoulet = 0;
      this.handleBouletSliderChange();
      this.defaultBoulet = true;
      this.userInteractedBoulet = false;
      this.adjustSvgElements('ant-B');

      // Vérifie si defaultCarpe est true et ajuste les éléments SVG si nécessaire
      if (this.defaultCarpe) {
        this.adjustSvgElements('ant-A');
      }
    },

    resetPied() {
      this.anglePieds = 0;
      this.handlePiedSliderChange();
      this.defaultPied = true;
      this.userInteractedPied = false;
      this.adjustSvgElements('ant-C');
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
        return ''; // Retourne une chaîne vide si l'état par défaut est true
      }

      value = parseFloat(value);
      // Définir les textes de libellé en fonction des seuils
      if (sliderName === 'carpe') {
        value = this.calculatedAngleCarpe;
        if (value <= -14.14) return 'Flexion Sévère';
        if (value <= -9.4267) return 'Flexion Modérée';
        if (value <= -4.7134) return 'Flexion Discrète';
        if (value >= 14.1398) return 'Extension Sévère';
        if (value >= 9.4265) return 'Extension Modérée';
        if (value >= 4.7132) return 'Extension Discret';
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
      // Répétez pour les autres sliders
      return 'Normal';
    },
    computeSliderClass(sliderName) {
      return (value) => {
        value = parseFloat(value);
        if (sliderName === 'carpe') {
          value = this.calculatedAngleCarpe;
        }
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
      const sliderName = elementId.includes('ant-A') ? 'carpe' :
          elementId.includes('ant-B') ? 'boulet' :
              'pied'; // Exemple de détermination du sliderName

      if ((sliderName === 'carpe' && this.userInteractedCarpe) ||
          (sliderName === 'boulet' && this.userInteractedBoulet) ||
          (sliderName === 'pied' && this.userInteractedPied)) {
        if (sliderName === 'carpe') this.defaultCarpe = false;
        if (sliderName === 'boulet') this.defaultBoulet = false;
        if (sliderName === 'pied') this.defaultPied = false;
      }

      const strokeColor = this.determineStrokeColor(sliderName, angle);
      const strokeWidth = '2pt'; // Adjust as needed
      this.rotateElement(elementId, angle, strokeColor, strokeWidth);
    },
    handleCarpeSliderChange() {
      this.userInteractedCarpe = true;
      this.angleCarpe = this.calculatedAngleCarpe;
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
      this.userInteractedBoulet = true;
      this.angleCarpe = 14.14 * (this.angleBoulet / -36);

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

    handlePiedSliderChange() {
      this.userInteractedPied = true;
      this.defaultPied = false;
      this.handleSliderChange('ant-C', this.anglePieds);
    },

    determineStrokeColor(sliderName, angle) {
      angle = parseFloat(angle);
      if (sliderName === 'carpe') {
        angle = this.calculatedAngleCarpe;
        // Utilisez des seuils spécifiques pour Carpe
        if (angle <= -14.14 || angle >= 14.1398) return '#d03431'; // Rouge pour Flexion/Extension Sévère
        if (angle <= -9.4267 || angle >= 9.4265) return '#d03431'; // Orange pour Flexion/Extension Modérée
        if (angle <= -4.7134 || angle >= 4.7132) return '#ea9c2b'; // Vert pour Flexion/Extension Discrète
      } else if (sliderName === 'boulet') {
        // Utilisez des seuils spécifiques pour Boulet
        if (angle <= -36 || angle >= 36) return '#d03431'; // Rouge pour Flexion/Extension Sévère
        if (angle <= -24 || angle >= 24) return '#d03431'; // Orange pour Flexion/Extension Modérée
        if (angle <= -12 || angle >= 12) return '#ea9c2b'; // Vert pour Flexion/Extension Discrète
      } else if (sliderName === 'pied') {
        // Utilisez des seuils spécifiques pour Pied
        if (angle <= -12 || angle >= 12) return '#d03431'; // Rouge pour Flexion/Extension Sévère
        if (angle <= -8 || angle >= 8) return '#d03431'; // Orange pour Flexion/Extension Modérée
        if (angle <= -4 || angle >= 4) return '#ea9c2b'; // Vert pour Flexion/Extension Discrète
      }

      // Fallback au cas où
      return '#5cb85c'; // Vert par défaut
    },

    rotateElement(elementId, angle, strokeColor, strokeWidth) {
      const svgDocument = this.$refs.svgElement.contentDocument;
      const element = svgDocument.getElementById(elementId);
      const pivot = svgDocument.getElementById(elementId + '-join');
      const devElement = svgDocument.getElementById(elementId + '-dev'); // Get the -dev element
      const devTopElement = svgDocument.getElementById(elementId + '-dev-top');

      if (element && pivot) {
        // Calculate the pivot point
        const cx = pivot.cx.baseVal.value;
        const cy = pivot.cy.baseVal.value;

        // Apply rotation and style to the main element
        element.setAttribute('transform', `rotate(${angle}, ${cx}, ${cy})`);
        if (pivot.id !== "ant-Z-join") {
          pivot.style.stroke = strokeColor;
          pivot.style.strokeWidth = strokeWidth;
        }

        // Apply the same styles to the -dev element if it exists
        if (devElement) {
          devElement.style.stroke = strokeColor;
          devElement.style.strokeWidth = strokeWidth;
          if (devTopElement) {
            devTopElement.style.stroke = strokeColor;
            devTopElement.style.strokeWidth = strokeWidth;
          }
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
