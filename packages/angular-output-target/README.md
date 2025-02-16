# @stencil/angular-output-target

Stencil can generate Angular component wrappers for your web components. This allows your Stencil components to be used within an Angular application. The benefits of using Stencil's component wrappers over the standard web components include:

- Angular component wrappers will be detached from change detection, preventing unnecessary repaints of your web component.
- Web component events will be converted to RxJS observables to align with Angular's @Output() and will not emit across component boundaries.
- Optionally, form control web components can be used as control value accessors with Angular's reactive forms or [ngModel].

For a detailed guide on how to add the angular output target to a project, visit: https://stenciljs.com/docs/angular.

## Installation

```bash
npm install @stencil/angular-output-target
```

## Usage

In your `stencil.config.ts` add the following configuration to the `outputTargets` section:

```ts
import { Config } from '@stencil/core';
import { angularOutputTarget } from '@stencil/angular-output-target';

export const config: Config = {
  namespace: 'demo',
  outputTargets: [
    angularOutputTarget({
      componentCorePackage: 'component-library',
      directivesProxyFile: '../component-library-angular/src/directives/proxies.ts',
      directivesArrayFile: '../component-library-angular/src/directives/index.ts',
    }),
    {
      type: 'dist',
      esmLoaderPath: '../loader',
    },
  ],
};
```

## Config Options

| Property                      | Description                                                                                                                                                                                                                                                                                                                      |
| ----------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `componentCorePackage`        | The NPM package name of your Stencil component library. This package is used as a dependency for your Angular wrappers.                                                                                                                                                                                                          |
| `directivesProxyFile`         | The output file of all the component wrappers generated by the output target. This file path should point to a location within your Angular library/project.                                                                                                                                                                     |
| `directivesArrayFile`         | The output file of a constant of all the generated component wrapper classes. Used for easily declaring and exporting the generated components from an `NgModule`. This file path should point to a location within your Angular library/project.                                                                                |
| `valueAccessorConfigs`        | The configuration object for how individual web components behave with Angular control value accessors.                                                                                                                                                                                                                          |
| `excludeComponents`           | An array of tag names to exclude from generating component wrappers for. This is helpful when have a custom framework implementation of a specific component or need to extend the base component wrapper behavior.                                                                                                              |
| `includeImportCustomElements` | If `true`, the output target will import the custom element instance and register it with the Custom Elements Registry when the component is imported inside of a user's app. This can only be used with the [Custom Elements Bundle](https://stenciljs.com/docs/custom-elements) and will not work with lazy loaded components. |
| `customElementsDir`           | This is the directory where the custom elements are imported from when using the [Custom Elements Bundle](https://stenciljs.com/docs/custom-elements). Defaults to the `components` directory. Only applies when `includeImportCustomElements` is `true`.                                                                        |
