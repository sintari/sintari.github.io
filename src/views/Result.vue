<template>
  <v-content>
    <v-container grid-list-md align-center>
      <v-layout wrap>
        <v-flex xs12 sm6 class="pa-4">
          <div class="result-container">
            <img src="@/assets/loops.svg" alt="loops" class="top-loops">
            <div class="header-section pa-5">
              <p class="display-1 modal-title">
                Wskazanie:
                <span class="font-italic">{{$route.params.title}}</span>
              </p>
            </div>
            <div class="body-section pa-5 pt-6">
              <div v-for="(value) in values">
                <div class="result-item-label">
                  {{value.label}}:
                </div>
                <div class="result-item-value">
                  {{value.value}}
                </div>
              </div>
              <v-btn round outline color="blue darken-1" class="white--text submit-button"
                @click="goBack">
                Edytuj
              </v-btn>
            </div>
            <img class="result-container-shadow" src="@/assets/box-shadow.png" alt="shadow">
          </div>
        </v-flex>
        <v-flex xs12 sm6 class="pa-4">
          <div class="possiblities-container text-sm-center">
            <div class="section">
              <p class="section-title">Możliwości terapeutyczne</p>
              <v-btn round outline color="blue darken-1" class="white--text"
                href="static/profilizer-raport.pdf"
                download="profilizer-raport">
                Ściągnij raport
              </v-btn>
            </div>
            <div class="section">
              <img src="@/assets/nfz.svg" alt="nfz">
              <p>Obecnie finansowane ze środków publicznych</p>
              <v-btn outline color="#666" href="https://www.gov.pl/web/zdrowie/programy-lekowe">
                CHEM
              </v-btn>
            </div>
            <div class="section">
              <img src="@/assets/ncc.svg" alt="nccn">
              <p>Rekomendowane przez wytyczne kliniczne</p>
              <v-flex xs12>
                <v-btn outline color="#666" href="https://www.ema.europa.eu/documents/product-information/alecensa-epar-product-information_pl.pdf" target="_blank">
                  Alektynib
                </v-btn>
                <v-btn outline color="#666" href="https://www.ema.europa.eu/documents/product-information/xalkori-epar-product-information_pl.pdf" target="_blank">
                  Kryzotynib
                </v-btn>
                <v-btn outline color="#666" href="https://www.ema.europa.eu/documents/product-information/zykadia-epar-product-information_pl.pdf" target="_blank">
                  Cerytynib
                </v-btn>
              </v-flex>
            </div>
            <div class="section">
              <img src="@/assets/leki.svg" alt="leki">
              <p>Lista badań klinicznych</p>
              <v-flex xs12 class="treatment-list">
                <v-tooltip top>
                  <v-btn outline color="#666" target="_blank"
                    href="https://clinicaltrials.gov/ct2/show/NCT03456076"
                    slot="activator">
                    A Study Comparing Adjuvant..
                  </v-btn>
                  <span>
                    Study Comparing Adjuvant Alectinib Versus Adjuvant Platinum-Based Chemotherapy in Patients With ALK Positive Non-Small Cell Lung Cancer
                  </span>
                </v-tooltip>
                <v-tooltip top>
                  <v-btn outline color="#666" target="_blank"
                    href="https://clinicaltrials.gov/ct2/show/NCT03052608"
                    slot="activator">
                    A Study Of Lorlatinib..
                  </v-btn>
                  <span>
                    Study Of Lorlatinib Versus Crizotinib In First Line Treatment Of Patients With ALK-Positive NSCLC
                  </span>
                </v-tooltip>
                <v-tooltip top>
                  <v-btn outline color="#666" target="_blank"
                    href="https://clinicaltrials.gov/ct2/show/NCT02299505"
                    slot="activator">
                    Pharmacokinetic and Safety..
                  </v-btn>
                  <span>
                    Pharmacokinetic and Safety Study of Lower Doses of Ceritinib Taken With a Low-fat Meal Versus 750 mg of Ceritinib in the Fasted State in Adult Patients With (ALK-positive) Metastatic Non-small Cell Lung Cancer (NSCLC)
                  </span>
                </v-tooltip>
                <v-tooltip top>
                  <v-btn outline color="#666" target="_blank"
                    href="https://clinicaltrials.gov/ct2/show/NCT02767804"
                    slot="activator">
                    eXalt3: Study Comparing..
                  </v-btn>
                  <span>
                    eXalt3: Study Comparing X-396 (Ensartinib) to Crizotinib in ALK Positive Non-Small Cell Lung Cancer (NSCLC) Patients
                  </span>
                </v-tooltip>
              </v-flex>
            </div>
          </div>
        </v-flex>
      </v-layout>
    </v-container>
  </v-content>
</template>

<script>
  import jsPDF from 'jspdf'
  import jsPDFCustom from 'jspdf-customfonts'
  import font from '../assets/robotoBase64'
  export default {
    name: 'Result',

    props: {
      dialog: {
        default: true,
        required: false
      },
      data: {
        values: [],
        default: ''
      }
    },

    created() {
      this.values = this.$route.params.values;
    },

    methods: {
      closeModal() {
        this.$emit('update:dialog', false)
      },
      goBack() {
        this.$router.replace({
          name: "select-values",
          params: { select: this.$route.params.selectedValues }
        })
      },
      generatePDF() {
        const doc = new jsPDF();
        let pdfName = 'Raport-profilizer';
        doc.addFileToVFS("Roboto.ttf", font.fontFace)
        
        doc.addFont("Roboto.ttf", "Roboto", 'regular');
        doc.setFont("Roboto", 'regular');
        doc.setFontSize(28)
        doc.text(20, 15, 'Profilizer - Raport')
        this.values.forEach(function(item, index) {
          doc.setFontSize(22)
          doc.text(20, (index + 1) * 30, item.label)
          doc.setFontSize(16)
          doc.text(20, ((index + 1) * 30) + 10, item.value)
        })
        doc.save(pdfName + '.pdf');
      }
    }
  }
</script>

<style lang="scss">
@import "./../assets/variables.scss";
.result-item-label {
  color: #7696AE;
  font-size: 12px;
  margin-bottom: 4px;
  font-weight: bold;
}

.header-section {
  background: #fff;
  padding-top: 58px !important;
}

.possiblities-container {
  background: #fff;
  box-shadow: 0px 12px 24px rgba(0, 0, 0, 0.1);
  border-radius: 4px;
  .section {
    padding: 23px;
    border-bottom: 1px solid #DEDEDE;
    p {
      color: #666666;
      font-size: 16px;
      &.section-title {
        font-size: 24px;
      }
    }
  }
}

.body-section {
  background-color: #E0F2FF;
}

.result-item-value {
  font-size: 18px;
  margin-bottom: 10px;
}

.result-container {
  position: relative;
  box-shadow: 0px 24px 36px rgba(0, 0, 0, 0.2);
  border-radius: 4px;
  .result-container-shadow {
    position: absolute;
    bottom: -48px;
    left: 50%;
    transform: translateX(-50%);
    width: calc(100% + 51px);
  }
}

.top-loops {
  position: absolute;
  top: -15px;
  left: 50%;
  width: 80%;
  transform: translateX(-50%);
}

.modal-title {
  font-weight: normal;
  color: #666666;
}
.modal-title span {
  color: $secondary-blue-title;
  font-weight: bold;
}
</style>
