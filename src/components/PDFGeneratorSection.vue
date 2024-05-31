<script setup>
import { onMounted } from 'vue'
// import pdfMake from 'pdfmake/build/pdfmake.js'
// import pdfFonts from 'pdfmake/build/vfs_fonts'
// Assign the fonts to the virtual file system
// pdfMake.vfs = pdfFonts.pdfMake.vfs

onMounted(() => {
  // Define the standard fonts
  pdfMake.fonts = {
    Courier: {
      normal: 'Courier',
      bold: 'Courier-Bold',
      italics: 'Courier-Oblique',
      bolditalics: 'Courier-BoldOblique'
    },
    Helvetica: {
      normal: 'Helvetica',
      bold: 'Helvetica-Bold',
      italics: 'Helvetica-Oblique',
      bolditalics: 'Helvetica-BoldOblique'
    },
    Times: {
      normal: 'Times-Roman',
      bold: 'Times-Bold',
      italics: 'Times-Italic',
      bolditalics: 'Times-BoldItalic'
    },
    Symbol: {
      normal: 'Symbol'
    },
    ZapfDingbats: {
      normal: 'ZapfDingbats'
    }
  }
})

const makeTag = (text = '') => {
  let canvas = {
    type: 'rect',
    x: -306.5,
    y: -263.5,
    w: 198.2,
    h: 19.2,
    r: 0,
    color: '#f9dbb7'
  }
  let column = {
    text,
    width: 180.2,
    color: '#000000',
    alignment: 'center',
    noWrap: true,
    maxHeight: 16,
    bold: true,
    style: 'header'
  }
  return {
    stack: [
      { canvas: [canvas], alignment: 'center' },
      {
        columns: [column],
        relativePosition: { y: -20.8, x: 188 },
        style: { fontSize: 7, bold: true },
        margin: [0, 0, 0, 13.2],
        characterSpacing: -5
      }
    ]
  }
}

const footerDivider = () => {
  let canvas = { type: 'line', x1: 30, y1: 0, x2: 582, y2: 0, lineWidth: 0.1 }
  return {
    canvas: [canvas],
    margin: [0, 0, 0, 2.5]
  }
}

const generatePdf = () => {
  // const pdfMakeModule = await import('@/assets/fonts/pdfmake.js')

  // const pdfMake = pdfMakeModule.default || pdfMakeModule

  // try {
  //   // Try to define the fonts property
  //   pdfMake.fonts = {
  //     Courier: {
  //       normal: 'Courier',
  //       bold: 'Courier-Bold',
  //       italics: 'Courier-Oblique',
  //       bolditalics: 'Courier-BoldOblique'
  //     },
  //     Helvetica: {
  //       normal: 'Helvetica',
  //       bold: 'Helvetica-Bold',
  //       italics: 'Helvetica-Oblique',
  //       bolditalics: 'Helvetica-BoldOblique'
  //     },
  //     Times: {
  //       normal: 'Times-Roman',
  //       bold: 'Times-Bold',
  //       italics: 'Times-Italic',
  //       bolditalics: 'Times-BoldItalic'
  //     },
  //     Symbol: {
  //       normal: 'Symbol'
  //     },
  //     ZapfDingbats: {
  //       normal: 'ZapfDingbats'
  //     }
  //   }
  // } catch (error) {
  //   // Handle error if the property cannot be redefined
  //   console.error('Error defining fonts:', error)
  // }

  const documentDefinition = {
    userPassword: '',
    ownerPassword: '123',
    permissions: {
      printing: 'highResolution',
      modifying: false,
      copying: false,
      annotating: false,
      fillingForms: false,
      contentAccessibility: false,
      documentAssembly: false
    },
    version: '1.4',
    info: {
      title: ' ',
      author: ' ',
      subject: ' ',
      keywords: ' ',
      creator: ' ',
      producer: 'KWSP',
      creationDate: new Date(),
      modDate: new Date()
    },
    header: {
      image: 'logo',
      width: 127,
      margin: [27.5, 27, 0, 0]
    },
    footer: function (currentPage) {
      return [
        footerDivider(),
        {
          columns: [
            {
              width: '85.25%',
              text:
                currentPage === 1
                  ? 'PEJABAT KWSP PUSAT PROSESAN P.O BOX 00220 JALAN SULTAN 46720 SELANGOR DARUL EHSAN.'
                  : 'Cetakan myEPF',
              noWrap: true,
              margin: [32, 1],
              fontSize: 8,
              lineHeight: 1.4
            },
            {
              width: '14.75%',
              text: 'Cetakan myEPF',
              margin: [0, 1, 0, 1],
              fontSize: 8,
              lineHeight: 1.4
            }
          ]
        },
        {
          columns: [
            {
              width: '85.5%',
              columns: [
                { text: currentPage === 1 ? 'Tel : 03-89226000 / ' : '', width: '15.4%' },
                {
                  text: currentPage === 1 ? 'www.kwsp.gov.my' : '',
                  width: '10%',
                  decoration: currentPage === 1 ? 'underline' : '',
                  lineHeight: 1.6
                }
              ],
              noWrap: true,
              margin: [32, 0],
              fontSize: 8,
              lineHeight: 1.4
            },
            {
              width: '14.5%',
              text: 'Muka Surat ' + currentPage,
              margin: [9, 1],
              fontSize: 8,
              lineHeight: 1.4
            }
          ]
        }
      ]
    },
    content: [
      { text: 'SULIT DAN PERSENDIRIAN', bold: true, margin: [0, 0, 0, 17] },
      {
        stack: [
          { text: 'LIM HANG SENG', characterSpacing: 0 },
          '2 JALAN NUSARIA 4/18',
          'TAMAN NUSANTARA',
          'JOHOR',
          '81550 PULAU SATU',
          'JOHOR DARUL TAKZIM'
        ],
        lineHeight: 1.6
      },
      {
        text: 'PENYATA AHLI TAHUN 2023',
        style: 'header'
      },
      {
        layout: 'noBorders',
        margin: [2, 0],
        table: {
          widths: [110.2, 111, 129, 71, 50],
          heights: [9, 9, 9],
          body: [
            ['No Ahli KWSP', ': 15884467', '', 'Tarikh', ': 14/05/2024'],
            ['No Kad Pengenalan', ': 780315075059', '', '', ''],
            ['No Majikan', ': 017700146', '', '', '']
          ]
        }
      },
      makeTag('JUMLAH SIMPANAN: RM168,273.72'),
      { text: 'RINGKASAN AKAUN', style: 'subheader' },
      {
        margin: [2, 0],
        table: {
          widths: [130, 70, 70, 70, 70, 70],
          heights: [10, 10, 10],
          body: [
            [
              'Jenis Akaun',
              { text: 'Baki Pembuka\n(RM)', style: 'tableHeader' },
              { text: 'Masuk\n(RM)', style: 'tableHeader' },
              { text: 'Keluar/Pindahan\n(RM)', style: 'tableHeader' },
              { text: 'Dividen Tahunan**\n(RM)', style: 'tableHeader' },
              { text: 'Jumlah\n(RM)', style: 'tableHeader' }
            ],
            [
              'Akaun 1',
              { text: '13,1241.61', style: 'tableCellWithAmount' },
              { text: '9,1251.00', style: 'tableCellWithAmount' },
              { text: '0.00', style: 'tableCellWithAmount' },
              { text: '5,121.54', style: 'tableCellWithAmount' },
              { text: '113,142.15', style: 'tableCellWithAmount', bold: true }
            ],
            [
              'Akaun 2*',
              { text: '14,134.47', style: 'tableCellWithAmount' },
              { text: '8,5124.00', style: 'tableCellWithAmount' },
              { text: '0.00', style: 'tableCellWithAmount' },
              { text: '1,123.10', style: 'tableCellWithAmount' },
              { text: '23,123.57', style: 'tableCellWithAmount', bold: true }
            ],
            [{ text: 'JUMLAH (RM)', colSpan: 5, alignment: 'right' }, '', '', '', '', '412,132.72']
          ]
        },
        layout: {
          fillColor: function (rowIndex, node) {
            return rowIndex === 0 || rowIndex === node.table.body.length - 1 ? '#a3cff2' : null
          },
          paddingTop: function (rowIndex, node) {
            return rowIndex === 0 ? 7 : 0
          },
          paddingBottom: function (rowIndex, node) {
            return rowIndex === 0 ? 7 : 0
          }
        }
      },
      //------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------//
      { text: 'Column/row spans', pageBreak: 'before', style: 'subheader' },
      //------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------//
      { text: 'Column/row spans', pageBreak: 'before', style: 'subheader' }
    ],
    styles: {
      header: {
        fontSize: 10,
        bold: true,
        characterSpacing: 0.2,
        alignment: 'center',
        margin: [0, 7.5, 0, 9]
      },
      subheader: {
        bold: true,
        decoration: 'underline',
        lineHeight: 1.6,
        margin: [0, 0, 0, 17]
      },
      tableHeader: {
        bold: true,
        alignment: 'center'
      },
      tableCellWithAmount: {
        alignment: 'right'
      }
    },
    defaultStyle: {
      font: 'Helvetica',
      fontSize: 8,
      alignment: 'left'
    },
    images: {
      logo: 'data:@/../src/assets/img/logo.jpg;base64,/9j/4AAQSkZJRgABAQEAYABgAAD/4QKERXhpZgAATU0AKgAAAAgABAE7AAIAAAAGAAABSodpAAQAAAABAAABUJydAAEAAAAMAAACcOocAAcAAAEMAAAAPgAAAAAc6gAAAAEAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAQWRtaW4AAAHqHAAHAAABDAAAAWIAAAAAHOoAAAABAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAQQBkAG0AaQBuAAAA/+EDXmh0dHA6Ly9ucy5hZG9iZS5jb20veGFwLzEuMC8APD94cGFja2V0IGJlZ2luPSfvu78nIGlkPSdXNU0wTXBDZWhpSHpyZVN6TlRjemtjOWQnPz4NCjx4OnhtcG1ldGEgeG1sbnM6eD0iYWRvYmU6bnM6bWV0YS8iPjxyZGY6UkRGIHhtbG5zOnJkZj0iaHR0cDovL3d3dy53My5vcmcvMTk5OS8wMi8yMi1yZGYtc3ludGF4LW5zIyI+PHJkZjpEZXNjcmlwdGlvbiByZGY6YWJvdXQ9InV1aWQ6ZmFmNWJkZDUtYmEzZC0xMWRhLWFkMzEtZDMzZDc1MTgyZjFiIiB4bWxuczpkYz0iaHR0cDovL3B1cmwub3JnL2RjL2VsZW1lbnRzLzEuMS8iLz48cmRmOkRlc2NyaXB0aW9uIHJkZjphYm91dD0idXVpZDpmYWY1YmRkNS1iYTNkLTExZGEtYWQzMS1kMzNkNzUxODJmMWIiIHhtbG5zOmRjPSJodHRwOi8vcHVybC5vcmcvZGMvZWxlbWVudHMvMS4xLyI+PGRjOmNyZWF0b3I+PHJkZjpTZXEgeG1sbnM6cmRmPSJodHRwOi8vd3d3LnczLm9yZy8xOTk5LzAyLzIyLXJkZi1zeW50YXgtbnMjIj48cmRmOmxpPkFkbWluPC9yZGY6bGk+PC9yZGY6U2VxPg0KCQkJPC9kYzpjcmVhdG9yPjwvcmRmOkRlc2NyaXB0aW9uPjwvcmRmOlJERj48L3g6eG1wbWV0YT4NCiAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAKICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgIAogICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICA8P3hwYWNrZXQgZW5kPSd3Jz8+/9sAQwAHBQUGBQQHBgUGCAcHCAoRCwoJCQoVDxAMERgVGhkYFRgXGx4nIRsdJR0XGCIuIiUoKSssKxogLzMvKjInKisq/9sAQwEHCAgKCQoUCwsUKhwYHCoqKioqKioqKioqKioqKioqKioqKioqKioqKioqKioqKioqKioqKioqKioqKioqKioq/8AAEQgAZgDCAwEiAAIRAQMRAf/EAB8AAAEFAQEBAQEBAAAAAAAAAAABAgMEBQYHCAkKC//EALUQAAIBAwMCBAMFBQQEAAABfQECAwAEEQUSITFBBhNRYQcicRQygZGhCCNCscEVUtHwJDNicoIJChYXGBkaJSYnKCkqNDU2Nzg5OkNE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'
    },
    pageSize: 'Letter',
    pageMargins: [30, 108, 30, 33.5]
  }

  pdfMake.createPdf(documentDefinition).getBlob((blob) => {
    // Check the size of the generated PDF blob
    if (blob.size > 23000) {
      // Handle the case where the PDF size exceeds the limit
      console.warn('PDF size exceeds 23 kB')
      console.warn('PDF size: ' + blob.size / 1000 + ' kB')
    } else {
      // Save the document if size is within the limit
      pdfMake.createPdf(documentDefinition).download('generated_pdf.pdf')
    }
  })
}

// import PDFDocument from 'pdfkit'
// import { PDFDocument as PDFLibDocument, StandardFonts } from 'pdf-lib'
// import { saveAs } from 'file-saver'

// const download = (data, filename, type) => {
//   const blob = new Blob([data], { type })
//   saveAs(blob, filename)
// }

// const setDocumentMetadata = async () => {
//   try {
//     // Create a new PDFDocument with PDF version 1.4 (Acrobat 5.0 and later)
//     const pdfDoc = await PDFDocument.create()

//     // Embed the Helvetica font
//     const helveticaFont = await pdfDoc.embedFont(StandardFonts.Helvetica)

//     // Add a page and draw some text on it
//     const page = pdfDoc.addPage([500, 600])
//     page.setFont(helveticaFont)
//     page.drawText('The Life of an Egg', { x: 60, y: 500, size: 50 })
//     page.drawText('An Epic Tale of Woe', { x: 125, y: 460, size: 25 })

//     // Set all available metadata fields on the PDFDocument. Note that these fields
//     // are visible in the "Document Properties" section of most PDF readers.
//     // pdfDoc.setTitle('🥚 The Life of an Egg 🍳')
//     // pdfDoc.setAuthor('Humpty Dumpty')
//     // pdfDoc.setSubject('📘 An Epic Tale of Woe 📖')
//     // pdfDoc.setKeywords(['eggs', 'wall', 'fall', 'king', 'horses', 'men'])
//     pdfDoc.setProducer('KWSP')
//     // pdfDoc.setCreator('pdf-lib (https://github.com/Hopding/pdf-lib)')
//     pdfDoc.setCreationDate(new Date('2018-06-24T01:58:37.228Z'))
//     pdfDoc.setModificationDate(new Date('2019-12-21T07:00:11.000Z'))

//     // Serialize the PDFDocument to bytes (a Uint8Array)
//     const pdfBytes = await pdfDoc.save({
//       useObjectStreams: false,
//       updateMetadata: false,
//       pdfVersion: '1.4'
//     })

//     // Trigger the browser to download the PDF document
//     download(pdfBytes, 'pdf-lib_creation_example.pdf', 'application/pdf')
//   } catch (error) {
//     console.error('An error occurred while creating the PDF:', error)
//   }
// }

// const createAndModifyPdf = async () => {
//   try {
//     // Create a new PDF with pdfkit
//     const pdfKitDoc = new PDFDocument({
//       size: 'A4',
//       layout: 'portrait',
//       version: '1.4' // Set the desired PDF version here
//     })

//     // Add content to the PDF using pdfkit
//     pdfKitDoc.text('Hello', 100, 100) // Add "Hello" text at position (100, 100)

//     // Save the PDF created with pdfkit
//     const pdfKitBytes = await new Promise((resolve, reject) => {
//       const chunks = []
//       pdfKitDoc.on('data', (chunk) => chunks.push(chunk))
//       pdfKitDoc.on('end', () => {
//         const pdfBuffer = new Blob(chunks)
//         const reader = new FileReader()
//         reader.onload = () => {
//           resolve(new Uint8Array(reader.result))
//         }
//         reader.readAsArrayBuffer(pdfBuffer)
//       })
//       pdfKitDoc.end()
//     })

//     // Load the PDF into pdf-lib
//     const pdfLibDoc = await PDFLibDocument.load(pdfKitBytes)

//     // Modify the "Producer" attribute
//     pdfLibDoc.setProducer('Your Custom Producer Name')

//     // Save the modified PDF with pdf-lib
//     const pdfBytes = await pdfLibDoc.save()

//     // Trigger download or use the PDF bytes as needed
//     download(pdfBytes, 'modified_pdf.pdf', 'application/pdf')
//   } catch (error) {
//     console.error('An error occurred while creating the PDF:', error)
//   }
// }

// const generatePdf = () => {
//   // Create a new PDFDocument instance
//   const doc = new PDFDocument()

//   // Add content to the PDF
//   doc.text('Hello, this is a PDF generated using pdfkit!', 50, 50)

//   // Set response headers to trigger download
//   const fileName = 'generated_pdf.pdf'
//   downloadPdf(doc, fileName)
// }

// // Method to trigger download
// const downloadPdf = (doc, fileName) => {
//   // Stream the PDF content to the client
//   doc.pipe(createWriteStream(fileName))

//   // End the document
//   doc.end()
// }

// // Method to create a writable stream
// const createWriteStream = (fileName) => {
//   const link = document.createElement('a')
//   link.href = '#'
//   link.download = fileName
//   document.body.appendChild(link)
//   return {
//     write: (chunk) => {
//       const blob = new Blob([chunk], { type: 'application/pdf' })
//       const url = window.URL.createObjectURL(blob)
//       link.href = url
//       link.click()
//       window.URL.revokeObjectURL(url)
//     }
//   }
// }

// pdfMake.fonts = {
//   Helvetica: {
//     normal: 'Helvetica',
//     bold: 'Helvetica-Bold',
//     italics: 'Helvetica-Oblique',
//     bolditalics: 'Helvetica-BoldOblique'
//   }
// }
</script>

<template>
  <div class="divider">
    <div class="title">Click to generate a sample pdf.</div>
    <button @click="generatePdf" class="createPDFBtn">Create PDF</button>
  </div>
</template>

<style>
.divider {
  border-left: 1px solid #c4c4c4;
  min-height: 100%;
  padding-left: 100px;
  text-align: center;
  margin-top: 10px;
  padding-top: 10px;
}

.title {
  font-size: 18px;
  font-weight: 600;
  text-align: center;
  margin-bottom: 10px;
}

.createPDFBtn {
  height: 60px;
  width: 250px;
  border-radius: 100px;
  border: none;
  background-color: rgb(73, 199, 73);
  color: #ffffff;
  font-weight: 700;
  font-size: 20px;
  cursor: pointer;
}
.createPDFBtn:hover {
  background-color: rgb(58, 160, 58);
}

@media only screen and (min-width: 768px) {
  .divider {
    border-top: 1px solid #c4c4c4;
    border-left: none;
    padding-left: 0;
  }
}
</style>
