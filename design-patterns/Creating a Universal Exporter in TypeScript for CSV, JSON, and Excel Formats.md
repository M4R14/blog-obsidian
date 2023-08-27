## Introduction
Exporting data to different formats can be a common task in software development. This can involve exporting data from various sources like databases, spreadsheets, and APIs to various file formats like CSV, JSON, Excel, and others. In this document, we'll show you how to create a Universal Exporter in **TypeScript** that can export data to CSV, JSON, and Excel formats.

## Requirements
To follow this guide, you'll need the following:

- Node.js and npm installed on your system
- A text editor or integrated development environment (IDE) for editing **TypeScript** code

## Creating the Exporter Classes
We'll start by creating classes that implement the actual exporting logic for each supported format. We'll create three classes: **CsvExporter**, **JsonExporter**, and **ExcelExporter**. Each class will extend an abstract Exporter class that defines a single export method as an abstract method that must be implemented by its **subclasses**.

Here's an example implementation for the **CsvExporter** class:
```ts
import { ExportTarget } from './interfaces';

abstract class Exporter {
  abstract async export(data: any[], target: ExportTarget): Promise<void>;
}

export class CsvExporter extends Exporter {
  async export(data: any[], target: ExportTarget): Promise<void> {
    // implementation goes here
  }
}
```
The **CsvExporter** class extends the Exporter class and implements the export method to export data to CSV format.

You can implement the **JsonExporter** and **ExcelExporter** classes in a similar manner.

## Creating the Universal Exporter Class
Next, we'll create the **UniversalExporter** class that will act as the main entry point for exporting data. This class will take a source object that provides the data to be exported, a target object that defines where the data should be written to, and an options object that specifies the desired output format.

Here's an example implementation for the **UniversalExporter** class:
```ts
import { ExportOptions, ExportSource, ExportTarget } from './interfaces';
import { CsvExporter } from './csv-exporter';
import { JsonExporter } from './json-exporter';
import { ExcelExporter } from './excel-exporter';

export class UniversalExporter {
  private exporters: { [key: string]: Exporter } = {
    csv: new CsvExporter(),
    json: new JsonExporter(),
    excel: new ExcelExporter(),
  };

  constructor(private readonly format: string) {}

  async export(source: ExportSource, target: ExportTarget, options: ExportOptions): Promise<void> {
    const data = source.getData();

    const exporter = this.exporters[this.format];
    if (!exporter) {
      throw new Error(`Unsupported format: ${this.format}`);
    }

    await exporter.export(data, target);
  }
}
```
The **UniversalExporter** class initializes an object that contains instances of all the supported exporters, keyed by their format name. The constructor takes the desired output format as a parameter and uses it to select the appropriate exporter from the exporters object. The export method then calls the selected exporter's export method with the provided data and target objects.

class diagram:
![[class-diagram.png]]

## Using the Universal Exporter
To use the **UniversalExporter** class, you'll need to create an instance of the class with the desired output format, a source object that provides the data to be exported, and a target object that defines where the data should be written to.

Here's an example usage of the **UniversalExporter** class:
```ts
import { UniversalExporter } from './universal-exporter';
import { CsvExportTarget, JsonExportTarget, ExcelExportTarget } from './interfaces';

const data = [
  { name: 'John', age: 32, city: 'New York' },
  { name: 'Jane', age: 27, city: 'San Francisco' },
  { name: 'Bob', age: 45, city: 'Boston' },
];

const csvTarget: CsvExportTarget = {
  filePath: '/path/to/file.csv',
};

const jsonTarget: JsonExportTarget = {
  filePath: '/path/to/file.json',
};

const excelTarget: ExcelExportTarget = {
  filePath: '/path/to/file.xlsx',
  sheetName: 'Sheet1',
};

const csvExporter = new UniversalExporter('csv');
await csvExporter.export({ getData: () => data }, csvTarget);

const jsonExporter = new UniversalExporter('json');
await jsonExporter.export({ getData: () => data }, jsonTarget);

const excelExporter = new UniversalExporter('excel');
await excelExporter.export({ getData: () => data }, excelTarget);
```
This code creates an array of data objects, and then creates instances of the **CsvExportTarget**, **JsonExportTarget**, and **ExcelExportTarget** classes, which define the output file path and other options for each exporter.

It then creates instances of the **UniversalExporter** class with the desired output format, and calls the export method with the source object and target object for each exporter.

## Conclusion
In this document, we showed you how to use **TypeScript** to create a Universal Exporter that can export data to CSV, JSON, and Excel formats. We started by creating classes for each supported format, and then created a **UniversalExporter** class that acts as the main entry point for exporting data. Finally, we showed you how to use the **UniversalExporter** class to export data to each supported format.

This Universal Exporter can be extended to support other formats like XML or PDF by simply adding a new exporter class and modifying the **UniversalExporter** class to include it.