# PDF Report Generator (with puppeteer)

## Description

pdf-test is a clean formatted integration of the google puppeteer addon for generate pdf report

## Prerequisite and install

1. Install in your project puppeteer with NPM: `npm i puppeteer`.
2. [optionally] Install in your project uuid: `npm install uuid` or change the name generation otherwise.
3. Recover and import ServicePuppeter file located in "/src/service/ServicePuppeteer" of this repository to your project.

## Generate an PDF

For generating pdf with PuppeteerService, first you need to import the class named servicePuppeteer where he is located.

```js
const PDFGeneratorService = require('./src/service/PDFGeneratorService');
```

### Use

```js
const pdf = new PDFGeneratorService(name, header, footer, parsedData, templatePath);
```

### Methods

#### pdf.setName(name);
  * `name` [string] The name of company for example (for name of pdf generated by the service) 
#### pdf.setHeader(html);
  * `html` [string] A html template for print header  
#### pdf.setFooter(html);
  * `html` [string] A html template for print footer
```js
// Example
pdf.setFooter(`
 <div style="color: lightgray; font-size: 10px; padding-top: 5px; text-align: center; width: 100%;">
   <span>LNA SANTE</span> - <span class="pageNumber"></span>/<span class='totalPages'></span>
 </div>
`);
```
#### pdf.setTemplatePath(path);
  * `path` [string] A path to set location of template
```js
// Example
pdf.setTemplatePath('views/template/report.ejs');
```
#### pdf.setParsedData(JSON parsedData);
  * `parsedData` [JSON] A json of data that will be displayed on the template
```js
// Example
const rawData = await ReadFile('src/service/response.json');

const parsedData = {
  data: JSON.parse(rawData)
};

pdf.setParsedData(parsedData);
```
#### pdf.generatePdf(uploadPath);
  * `uploadPath` [string] A upload path for generate pdf
  * Generate pdf with all the information set
```js
// Example
const generatedPdf = await pdf.generatePdf('/src/upload/report');
```

### Use in Rest Controller example

```js
// Example
app.get('/generatePDF', async (req, res) => {
  // Import and call the PDFGeneratorService
  const PDFGeneratorService = require('./src/service/PDFGeneratorService.js');
  let pdf = new PDFGeneratorService();

  // Setup the provided data
  const rawData = await ReadFile('src/service/response.json');
  const parsedData = {
    data: JSON.parse(rawData)
  };

  // Define the customs properties of the pdf
  pdf.setName('LNA_SANTE');
  pdf.setTemplatePath('views/template/report.ejs');
  pdf.setParsedData(parsedData);
  pdf.setFooter(`
    <div style="color: lightgray; font-size: 10px; padding-top: 5px; text-align: center; width: 100%;">
      <span>LNA SANTE</span> - <span class="pageNumber"></span>/<span class='totalPages'></span>
    </div>
  `);

  // And finally, call the pdf generation function
  const generatedPdf = await pdf.generatePdf('/src/upload');

  res.download(generatedPdf.path, 'report.pdf')
})
```
