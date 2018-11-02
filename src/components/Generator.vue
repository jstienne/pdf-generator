<template>
  <div>
    <p>Le fichier doit être au format csv ou txt sans entête ayant comme séparateur des points virgules comme dans l'exemple suivant :</p>
    <pre class="pre">
      codePlanteur1;nom1;ville1
      codePlanteur2;nom2;ville2
      codePlanteur3;nom3;ville3
    </pre>
    <p><input type="file" v-on:change="changeCurrentFile"></p>
    <p><button class="btn btn-primary" v-on:click="exportPdf">Exporter</button></p>
  </div>
</template>

<script>
import * as pdfmake from 'pdfmake/build/pdfmake'
import * as pdfFonts from 'pdfmake/build/vfs_fonts'

export default {
  data: function () {
    return {
      file: '',
      persons: []
    }
  },
  methods: {
    changeCurrentFile: function (e) {
      const context = this
      this.file = e.target.files[0]
      const reader = new FileReader()
      reader.onload = function (event) {
        const lines = event.target.result.split('\n')
        lines.pop()
        context.persons = lines.map(line => {
          const person = line.split(';')
          return { code: person[0], name: person[1], city: person[2] }
        })
      }
      reader.readAsText(this.file)
    },
    exportPdf: function () {
      pdfmake.vfs = pdfFonts.pdfMake.vfs
      var docDefinition = {
        content: [
          ...this.persons.sort((a, b) => a.city > b.city).map((person, index) => { return { text: (index + 1) + '. ' + person.city + ' - ' + person.code + ' - ' + person.name } }),
          { text: ' ', pageBreak: 'after' },
          ...this.persons.sort((a, b) => a.city > b.city).map((person, index) => {
            return [
              { text: 'Commune : ' + person.city, style: 'header' },
              { text: 'Code planteur : ' + person.code, style: 'header' },
              { text: 'Nom : ' + person.name, style: 'header' },
              { text: 'Livraison', style: 'title' },
              {
                table: {
                  headerRows: 1,
                  heights: 20,
                  widths: [100, 100, 100, 100],
                  body: [
                    [{text: 'Date', style: 'tableHeader'}, {text: 'Variété', style: 'tableHeader'}, {text: 'N° de lot', style: 'tableHeader'}, {text: 'Quantité', style: 'tableHeader'}],
                    [' ', '', '', ''],
                    [' ', '', '', ''],
                    [' ', '', '', ''],
                    [' ', '', '', ''],
                    [' ', '', '', ''],
                    [' ', '', '', ''],
                    [' ', '', '', ''],
                    [' ', '', '', ''],
                    [' ', '', '', ''],
                    [' ', '', '', '']
                  ]
                },
                layout: 'lightHorizontalLines'
              },
              { text: 'Reprise', style: 'title' },
              {
                table: {
                  headerRows: 1,
                  heights: 20,
                  widths: [100, 100, 100, 100],
                  body: [
                    [{text: 'Date', style: 'tableHeader'}, {text: 'Variété', style: 'tableHeader'}, {text: 'N° de lot', style: 'tableHeader'}, {text: 'Quantité', style: 'tableHeader'}],
                    [' ', '', '', ''],
                    [' ', '', '', ''],
                    [' ', '', '', ''],
                    [' ', '', '', ''],
                    [' ', '', '', ''],
                    [' ', '', '', ''],
                    [' ', '', '', ''],
                    [' ', '', '', ''],
                    [' ', '', '', ''],
                    [' ', '', '', '']
                  ]
                },
                layout: 'lightHorizontalLines'
              },
              { text: index + 1, style: 'footer', pageBreak: 'after' }
            ]
          })
        ],
        styles: {
          header: { fontSize: 20 },
          title: { bold: true, fontSize: 30, alignment: 'center' },
          tableHeader: { bold: true, fontSize: 15 },
          footer: { fontSize: 10, alignment: 'right' }
        }
      }
      pdfmake.createPdf(docDefinition).download()
    }
  }
}
</script>

<style>
.pre {
  text-align: left;
  margin-left: 40%;
}
</style>
