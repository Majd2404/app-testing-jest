# AppTestingJest

# run your Angular tests without a browser

# run test suite several times faster

1. Install the needed dependencies

        $ npm install jest jest-preset-angular @types/jest --save-dev

    or

        $ yarn add jest jest-preset-angular @types/jest --dev

2. Let's say we've generated an Angular application called app-testing-jest with the following command:  

        $ ng new app-testing-jest

3. Create the jest.config.js file at the root of your project

        const { pathsToModuleNameMapper } = require('ts-jest/utils');

        const { compilerOptions } = require('./tsconfig');

        module.exports = {

        preset: 'jest-preset-angular',

        roots: ['<rootDir>/src/'],

        testMatch: ['**/+(*.)+(spec).+(ts)'],

        setupFilesAfterEnv: ['<rootDir>/src/test.ts'],

        collectCoverage: true,

        coverageReporters: ['html'],

        coverageDirectory: 'coverage/my-app',

        moduleNameMapper: pathsToModuleNameMapper(compilerOptions.paths || {}, {

            prefix: '<rootDir>/'
        
        })
        };

4. Update the content of  the src/test.ts file
Replace the content of the src/test.ts with the following:

        import 'jest-preset-angular';

        Object.defineProperty(window, 'CSS', {value: null});

        Object.defineProperty(window, 'getComputedStyle', {

        value: () => {

            return {

            display: 'none',

            appearance: ['-webkit-appearance']

            };
        }

        });

        Object.defineProperty(document, 'doctype', {

        value: '<!DOCTYPE html>'

        });

        Object.defineProperty(document.body.style, 'transform', {

        value: () => {

            return {

            enumerable: true,

            configurable: true

            };
        }
        });

5. Update the content of the tsconfig.spec.json file

            {
                "extends": "./tsconfig.json",
                "compilerOptions": {
                    "outDir": "./out-tsc/spec",
                    "types": [
                    "jest", // 1
                    "node"
                    ],
                    "esModuleInterop": true, // 2
                    "emitDecoratorMetadata": true // 3
                },
                "files": [
                    "src/test.ts",
                    "src/polyfills.ts"
                ],
                "include": [
                    "src/**/*.spec.ts",
                    "src/**/*.d.ts"
                ]
            }

6. Remove Karma

    + Remove dependencies

            $ npm uninstall karma karma-chrome-launcher karma-coverage-istanbul-reporter karma-jasmine karma-jasmine-html-reporter

        or

            $ yarn remove karma karma-chrome-launcher karma-coverage-istanbul-reporter karma-jasmine karma-jasmine-html-reporter

    + Remove the Karma configuration file 

        rm karma.conf.js

    + Remove the test target inside the angular.json file

            "test": {
                "builder": "@angular-devkit/build-angular:karma",
                "options": {
                    "main": "src/test.ts",
                    "polyfills": "src/polyfills.ts",
                    "tsConfig": "tsconfig.spec.json",
                    "karmaConfig": "karma.conf.js",
                    "assets": [
                    "src/favicon.ico",
                    "src/assets"
                    ],
                    "styles": [
                    "src/styles.css"
                    ],
                    "scripts": []
                }
            }

7. Run the tests

        $ npx jest 